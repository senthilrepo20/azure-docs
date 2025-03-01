---
title: "Known issues: Migrate from MongoDB to Azure Cosmos DB"
titleSuffix: Azure Database Migration Service
description: Learn about known issues and migration limitations with migrations from MongoDB to Azure Cosmos DB using the Azure Database Migration Service.
services: database-migration
author: croblesm
ms.author: roblescarlos
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom:
- "seo-lt-2019"
- kr2b-contr-experiment
ms.topic: troubleshooting
ms.date: 05/18/2022
---

# Known issues with migrations from MongoDB to Azure Cosmos DB's API

The following sections describe known issues and limitations associated with migrations from MongoDB to Cosmos DB's API for MongoDB.

## Migration fails as a result of using the incorrect TLS/SSL Cert

This issue is apparent when a user can't connect to the MongoDB source server. Despite having all firewall ports open, the user still can't connect.

| Cause         | Resolution |
| ------------- | ------------- |
| Using a self-signed certificate in Azure Database Migration Service might lead to the migration failing because of the incorrect TLS/SSL certificate. The error message might include "The remote certificate is invalid according to the validation procedure." | Use a genuine certificate from CA. Connections to Cosmos DB use TLS over Mongo API. Self-signed certs are generally only used in internal tests. When you install a genuine cert from a CA authority, you can then use TLS in Azure Database Migration Service without issue. |

## Unable to get the list of databases to map in DMS

Unable to get database list in the **Database setting** area when using **Data from Azure Storage** mode in the **Select source** area.

| Cause         | Resolution |
| ------------- | ------------- |
| The storage account connection string is missing the shared access signature (SAS) information and can't be authenticated. | Create the SAS on the blob container in Storage Explorer and use the URL with container SAS info as the source detail connection string. |

## Using an unsupported version of the database

The migration fails.

| Cause         | Resolution |
| ------------- | ------------- |
| You attempt to migrate to Azure Cosmos DB from an unsupported version of MongoDB. | As new versions of MongoDB are released, they're tested to ensure compatibility with Azure Database Migration Service. The service is being updated periodically to accept the latest versions. If there's an immediate need to migrate, as a workaround you can export the databases or collections to Azure Storage and then point the source to the resulting dump. Create the SAS on the blob container in Storage Explorer, and then use the URL with container SAS information as the source detail connection string. |

## Next steps

* View the tutorial [Migrate MongoDB to Azure Cosmos DB's API for MongoDB online using DMS](tutorial-mongodb-cosmos-db-online.md).
* View the tutorial [Migrate MongoDB to Azure Cosmos DB's API for MongoDB offline using DMS](tutorial-mongodb-cosmos-db.md).
