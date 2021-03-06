---
title: Intégrer un environnement Active Directory local à Azure
titleSuffix: Azure Reference Architectures
description: Comparatif des différentes architectures de référence pour intégrer un environnement Active Directory local à Azure.
ms.date: 07/02/2018
ms.custom: seodec18
ms.openlocfilehash: 905dedda6de1a107f55b2f7651441780a685aea7
ms.sourcegitcommit: 88a68c7e9b6b772172b7faa4b9fd9c061a9f7e9d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53119861"
---
# <a name="choose-a-solution-for-integrating-on-premises-active-directory-with-azure"></a>Choisir une solution pour intégrer l’environnement Active Directory local à Azure

Cet article compare les options disponibles pour intégrer votre environnement Active Directory (AD) local avec un réseau Azure. Pour chaque option, une architecture de référence plus détaillée est disponible.

De nombreuses organisations utilisent les services Active Directory Domain Services (AD DS) pour authentifier les identités associées aux utilisateurs, aux ordinateurs, aux applications ou aux autres ressources incluses dans un périmètre de sécurité donné. Les services de répertoire et d’identité sont généralement hébergés localement, mais si votre application est hébergée à la fois localement et dans Azure, il peut y avoir un certain temps de latence lors de l’envoi de requêtes d’authentification à partir d’Azure vers l’environnement local. L’implémentation des services de répertoire et d’identité dans Azure peut réduire cette latence.

Azure offre deux solutions pour implémenter les services de répertoire et d’identité dans Azure :

- Utilisez [Azure AD][azure-active-directory] pour créer un domaine Active Directory dans le cloud et le connecter à votre domaine Active Directory local. [Azure AD Connect][azure-ad-connect] intègre vos répertoires locaux à Azure AD.

- Étendez votre infrastructure Active Directory locale dans Azure en déployant une machine virtuelle dans Azure qui exécute les services AD DS en tant que contrôleur de domaine. Cette architecture est couramment utilisée quand le réseau local et le réseau virtuel Azure (VNet) sont connectés par une connexion VPN ou ExpressRoute. Il existe plusieurs variantes de cette architecture :

  - Créez un domaine dans Azure et associez-le à votre forêt AD locale.
  - Créez une forêt distincte dans Azure qui est approuvée par des domaines de votre forêt locale.
  - Répliquez un déploiement Active Directory Federation Services (AD FS) dans Azure.

Les sections suivantes décrivent chacune de ces options plus en détails.

## <a name="integrate-your-on-premises-domains-with-azure-ad"></a>Intégrer vos domaines locaux à Azure AD

Utilisez Azure Active Directory (Azure AD) pour créer un domaine dans Azure et l’associer à un domaine AD local.

Le répertoire Azure AD n’est pas une extension d’un répertoire local. Il s’agit en réalité d’une copie qui contient les mêmes objets et identités. Les modifications apportées à ces éléments locaux sont copiées vers Azure AD, mais les modifications apportées dans Azure AD ne sont pas répliquées sur le domaine local.

Vous pouvez également utiliser Azure AD sans utiliser un répertoire local. Dans ce cas, Azure AD agit comme la source principale de toutes les informations d’identité, au lieu d’obtenir des données répliquées d’un répertoire local.

**Avantages**

- Vous n’avez pas besoin de gérer une infrastructure AD dans le cloud. Azure AD est entièrement géré et mis à jour par Microsoft.
- Azure AD fournit les mêmes informations d’identité que celles disponibles localement.
- L’authentification peut s’effectuer dans Azure, ce qui évite aux utilisateurs et applications externes d’avoir à contacter le domaine local.

**Défis**

- Les services d’identité sont réservés aux utilisateurs et aux groupes. Aucune authentification de comptes de service et d’ordinateur n’est possible.
- Vous devez configurer la connectivité à votre domaine local pour assurer la synchronisation du répertoire Azure AD. 
- Les applications devront peut-être être réécrites pour activer l’authentification via Azure AD.

**Architecture de référence**

- [Intégrer des domaines Active Directory locaux avec Azure Active Directory][aad]

## <a name="ad-ds-in-azure-joined-to-an-on-premises-forest"></a>Services AD DS dans Azure associés à une forêt locale

Déployer des serveurs AD Domain Services (AD DS) dans Azure. Créez un domaine dans Azure et associez-le à votre forêt AD locale. 

Envisagez cette option si vous avez besoin d’utiliser des fonctionnalités AD DS qui ne sont pas actuellement disponibles dans Azure AD. 

**Avantages**

- Cette méthode fournit les mêmes informations d’identité que celles disponibles localement.
- Vous pouvez authentifier les comptes d’utilisateur, de service et d’ordinateur localement et dans Azure.
- Vous n’avez pas besoin de gérer une autre forêt Active Directory. Le domaine dans Azure peut appartenir à la forêt locale.
- Vous pouvez appliquer la stratégie de groupe définie par les objets de stratégie de groupe locale au domaine dans Azure.

**Défis**

- Vous devez déployer et gérer vos propres domaine et serveurs AD DS dans le cloud.
- Il peut y avoir une certaine latence au niveau de la synchronisation entre les serveurs de domaine dans le cloud et les serveurs exécutés localement.

**Architecture de référence**

- [Étendre Active Directory Domain Services (AD DS) à Azure][ad-ds]

## <a name="ad-ds-in-azure-with-a-separate-forest"></a>Services AD DS dans Azure avec une forêt distincte

Déployez des serveurs AD Domain Services (AD DS) dans Azure, mais créez une [forêt][ad-forest-defn] Active Directory différente de la forêt locale. Cette forêt est approuvée par les domaines de votre forêt locale.

Parmi les utilisations courantes de cette architecture figurent la conservation d’une séparation de sécurité pour les objets et les identités contenus dans le cloud et la migration de domaines individuels depuis l’environnement local vers le cloud.

**Avantages**

- Vous pouvez implémenter des identités locales, ainsi que des identités Azure distinctes.
- Vous n’avez pas besoin de répliquer les données de la forêt Active Directory locale vers Azure.

**Défis**

- Dans Azure, l’authentification des identités locales nécessite des tronçons réseaux supplémentaires vers les serveurs Active Directory locaux.
- Vous devez déployer vos propres forêt et serveurs AD DS dans le cloud et établir les relations d’approbation appropriées entre les forêts.

**Architecture de référence**

- [Créer une forêt de ressources Active Directory Domain Services (AD DS) dans Azure][ad-ds-forest]

## <a name="extend-ad-fs-to-azure"></a>Étendre AD FS à Azure

Répliquez un déploiement Active Directory Federation Services (AD FS) dans Azure afin de permettre l’authentification et l’autorisation fédérées des composants en cours d’exécution dans Azure. 

Utilisations courantes de cette architecture :

- Authentifier et autoriser les utilisateurs des organisations partenaires.
- Permettre aux utilisateurs de s’authentifier à partir de navigateurs web s’exécutant en dehors du pare-feu de l’organisation.
- Permettre aux utilisateurs de se connecter à partir d’appareils externes autorisés tels que des appareils mobiles. 

**Avantages**

- Vous pouvez utiliser des applications prenant en charge les revendications.
- Vous pouvez approuver des partenaires externes pour l’authentification.
- Cette méthode est compatible avec un large éventail de protocoles d’authentification.

**Défis**

- Vous devez déployer vos propres serveurs proxy d’application web AD FS, AD DS et AD FS dans Azure.
- Cette architecture peut être difficile à configurer.

**Architecture de référence**

- [Étendre les services de fédération Active Directory (AD FS) dans Azure][adfs]

<!-- links -->

[aad]: ./azure-ad.md
[ad-ds]: ./adds-extend-domain.md
[ad-ds-forest]: ./adds-forest.md
[ad-forest-defn]: /windows/desktop/AD/forests
[adfs]: ./adfs.md

[azure-active-directory]: /azure/active-directory-domain-services/active-directory-ds-overview
[azure-ad-connect]: /azure/active-directory/hybrid/whatis-hybrid-identity
