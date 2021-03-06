---
title: Choisir un magasin de données de recherche
description: ''
author: zoinerTejada
ms.date: 02/12/2018
ms.openlocfilehash: b5943cd1410777b974a8cefcd77c7c2f1f2bfe67
ms.sourcegitcommit: e7e0e0282fa93f0063da3b57128ade395a9c1ef9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52902321"
---
# <a name="choosing-a-search-data-store-in-azure"></a>Choisir un magasin de données de recherche dans Azure

Cet article compare les choix technologiques pour les magasins de données de recherche dans Azure. Un magasin de données de recherche est utilisé pour créer et stocker des index spécialisés afin d’effectuer des recherches sur du texte de forme libre. Le texte qui est indexé peut résider dans un magasin de données distinct, comme le stockage blob. Une application soumet une requête au magasin de données de recherche, et le résultat est une liste de documents correspondants. Pour plus d’informations sur ce scénario, consultez [Traitement de texte de forme libre pour la recherche](../scenarios/search.md). 

## <a name="what-are-your-options-when-choosing-a-search-data-store"></a>Quelles sont vos options lors du choix d’un magasin de données de recherche ?
Dans Azure, tous les magasins de données suivants respectent les exigences principales pour la recherche sur des données de texte de forme libre en fournissant un index de recherche :
- [Azure Search](/azure/search/search-what-is-azure-search)
- [Elasticsearch](https://azuremarketplace.microsoft.com/marketplace/apps/elastic.elasticsearch?tab=Overview)
- [HDInsight avec Solr](/azure/hdinsight/hdinsight-hadoop-solr-install-linux)
- [Azure SQL Database avec recherche en texte intégral](/sql/relational-databases/search/full-text-search)


## <a name="key-selection-criteria"></a>Critères de sélection principaux

Pour les scénarios de recherche, commencez par choisir le magasin de données de recherche approprié pour vos besoins en répondant à ces questions :

- Voulez-vous un service managé plutôt que de gérer vos propres serveurs ?

- Pouvez-vous spécifier votre schéma d’index au moment de la conception ? Dans le cas contraire, choisissez une option qui prend en charge les schémas pouvant être mis à jour.

- Avez-vous besoin d’un index uniquement pour la recherche en texte intégral ou avez-vous également besoin de l’agrégation rapide de données numériques et autres outils analytiques ? Si vous avez besoin de fonctionnalités au-delà de la recherche en texte intégral, envisagez les options qui prennent en charge une analytique supplémentaire.

- Avez-vous besoin d’un index de recherche pour l’analytique des journaux, avec prise en charge de la collecte de journaux, de l’agrégation et des visualisations sur les données indexées ? Dans ce cas, envisagez d’utiliser Elasticsearch, qui fait partie d’une pile d’analytique des journaux.

- Avez-vous besoin d’indexer les données dans des formats de documents courants tels que PDF, Word, PowerPoint et Excel ? Si la réponse est Oui, choisissez une option qui fournit des indexeurs de documents.

- Votre base de données a-t-elle des besoins de sécurité spécifiques ? Si la réponse est Oui, consultez les fonctionnalités de sécurité ci-dessous.

## <a name="capability-matrix"></a>Matrice des fonctionnalités

Les tableaux suivants résument les principales différences entre les fonctionnalités.

### <a name="general-capabilities"></a>Fonctionnalités générales

| | Recherche Azure | Elasticsearch | HDInsight avec Solr | Base de données SQL | 
| --- | --- | --- | --- | --- | 
| Est un service géré | Oui | Non  | OUI | Oui |  
| API REST | Oui | OUI | Oui | Non  |
| Programmabilité | .NET | Java | Java | T-SQL | 
| Indexeurs de documents pour les types de fichiers courants (PDF, DOCX, TXT, etc.) | Oui | Non  | Oui | Non  |

### <a name="manageability-capabilities"></a>Fonctionnalités de facilité de gestion

| | Recherche Azure | Elasticsearch | HDInsight avec Solr | Base de données SQL | 
| --- | --- | --- | --- | --- |
| Schéma pouvant être mis à jour | Non  | OUI | OUI | Oui |
| Prend en charge la montée en puissance  | Oui | OUI | Oui | Non  |

### <a name="analytic-workload-capabilities"></a>Fonctionnalités de la charge de travail analytique

| | Recherche Azure | Elasticsearch | HDInsight avec Solr | Base de données SQL | 
| --- | --- | --- | --- | --- | 
| Prend en charge l’analytique au-delà de la recherche en texte intégral | Non  | OUI | OUI | Oui |
| Fait partie d’une pile d’analytique des journaux | Non  | Oui (ELK) |  Non  | Non  |
| Prend en charge la recherche sémantique | Oui (rechercher uniquement les documents similaires) | Oui | OUI | Oui | 

### <a name="security-capabilities"></a>Fonctionnalités de sécurité

| | Recherche Azure | Elasticsearch | HDInsight avec Solr | Base de données SQL | 
| --- | --- | --- | --- | --- | 
| Sécurité au niveau des lignes | Partielle (requête d’application requise pour filtrer par ID de groupe) | Partielle (requête d’application requise pour filtrer par ID de groupe) | Oui | Oui | 
| Chiffrement transparent des données | Non  | Non  | Non  | Oui |  
| Restreindre l’accès à des adresses IP spécifiques | Non  | OUI | OUI | Oui |   
| Restreindre l’accès pour autoriser l’accès au réseau virtuel uniquement | Non  | OUI | OUI | Oui |  
| Authentification Active Directory (authentification intégrée) | Non  | Non  | Non  | Oui | 

## <a name="see-also"></a>Voir aussi

[Traitement de texte de forme libre pour la recherche](../scenarios/search.md)
