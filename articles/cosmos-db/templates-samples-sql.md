---
title: Azure Cosmos DB Core için Azure Resource Manager şablonları (SQL API)
description: Azure Cosmos DB oluşturmak ve yapılandırmak için Azure Resource Manager şablonları kullanın.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/14/2020
ms.author: mjbrown
ms.openlocfilehash: 3247f35fec8615b9c5e277b7453c340caaf56ffd
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92285003"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure Resource Manager şablonları

Bu makalede, çekirdek (SQL) API hesapları için yalnızca Azure Resource Manager şablon örnekleri gösterilmektedir. [Cassandra](templates-samples-cassandra.md), [Gremlin](templates-samples-gremlin.md), [MongoDB](templates-samples-mongodb.md)ve [tablo](templates-samples-table.md) API 'leri için şablon örnekleri de bulabilirsiniz.

## <a name="core-sql-api"></a>Çekirdek (SQL) API'si

|**Şablon**|**Açıklama**|
|---|---|
|[Bir Azure Cosmos hesabı, veritabanı, otomatik ölçeklendirme işleme ile kapsayıcı oluşturma](manage-with-templates.md#create-autoscale) | Bu şablon iki bölgede bir çekirdek (SQL) API hesabı, bir veritabanı ve otomatik ölçeklendirme işleme içeren bir kapsayıcı oluşturur. |
|[Analitik depo ile Azure Cosmos hesabı, veritabanı, kapsayıcı oluşturma](manage-with-templates.md#create-analytical-store) | Bu şablon, analitik TTL etkin ve el ile veya otomatik ölçeklendirme üretimi kullanma seçeneği ile yapılandırılmış bir kapsayıcı içeren bir bölgede çekirdek (SQL) API hesabı oluşturur. |
|[Standart (el ile) aktarım hızı ile bir Azure Cosmos hesabı, veritabanı, kapsayıcı oluşturma](manage-with-templates.md#create-manual) | Bu şablon, standart işleme sahip bir veritabanı ve kapsayıcı olmak üzere iki bölgede bir çekirdek (SQL) API hesabı oluşturur. |
|[Saklı yordam, tetikleyici ve UDF ile bir Azure Cosmos hesabı, veritabanı ve kapsayıcısı oluşturma](manage-with-templates.md#create-sproc) | Bu şablon bir kapsayıcı için saklı yordam, tetikleyici ve UDF olan iki bölgede bir çekirdek (SQL) API hesabı oluşturur. |
|[Mevcut bir Azure Cosmos hesabı için özel bir uç nokta oluşturma](how-to-configure-private-endpoints.md#create-a-private-endpoint-by-using-a-resource-manager-template) |  Bu şablon var olan bir sanal ağda mevcut bir Azure Cosmos Core (SQL) API hesabı için özel bir uç nokta oluşturur. |
|[Ücretsiz katman Azure Cosmos hesabı oluşturma](manage-with-templates.md#free-tier) |  Bu şablon, ücretsiz katmanda Azure Cosmos DB Çekirdek (SQL) API hesabı oluşturur. |

Başvuru belgelerinin Azure Cosmos DB sayfasına yönelik [Azure Resource Manager başvuru](/azure/templates/microsoft.documentdb/allversions) bölümüne bakın.
