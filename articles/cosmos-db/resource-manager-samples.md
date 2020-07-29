---
title: Azure Cosmos DB için Azure Resource Manager şablonları
description: Azure Cosmos DB oluşturmak ve yapılandırmak için Azure Resource Manager şablonları kullanın.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/08/2020
ms.author: mjbrown
ms.openlocfilehash: a98aee61c3a330344b5b7fa0a5f7f53a65e5ecdb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83586159"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure Resource Manager şablonları

Aşağıdaki tablolar Azure Cosmos DB için Azure Resource Manager şablonlarının bağlantılarını içerir:

## <a name="core-sql-api"></a>Çekirdek (SQL) API'si

|**Şablon**|**Açıklama**|
|---|---|
|[Bir Azure Cosmos hesabı, veritabanı, otomatik ölçeklendirme işleme ile kapsayıcı oluşturma](manage-sql-with-resource-manager.md#create-autoscale) | Bu şablon iki bölgede bir çekirdek (SQL) API hesabı, bir veritabanı ve otomatik ölçeklendirme işleme içeren bir kapsayıcı oluşturur. |
|[Analitik depo ile Azure Cosmos hesabı, veritabanı, kapsayıcı oluşturma](manage-sql-with-resource-manager.md#create-analytical-store) | Bu şablon, analitik TTL etkin ve el ile veya otomatik ölçeklendirme üretimi kullanma seçeneği ile yapılandırılmış bir kapsayıcı içeren bir bölgede çekirdek (SQL) API hesabı oluşturur. |
|[Standart (el ile) aktarım hızı ile bir Azure Cosmos hesabı, veritabanı, kapsayıcı oluşturma](manage-sql-with-resource-manager.md#create-manual) | Bu şablon, standart işleme sahip bir veritabanı ve kapsayıcı olmak üzere iki bölgede bir çekirdek (SQL) API hesabı oluşturur. |
|[Saklı yordam, tetikleyici ve UDF ile bir Azure Cosmos hesabı, veritabanı ve kapsayıcısı oluşturma](manage-sql-with-resource-manager.md#create-sproc) | Bu şablon bir kapsayıcı için saklı yordam, tetikleyici ve UDF olan iki bölgede bir çekirdek (SQL) API hesabı oluşturur. |
|[Mevcut bir Azure Cosmos hesabı için özel bir uç nokta oluşturma](how-to-configure-private-endpoints.md#create-a-private-endpoint-by-using-a-resource-manager-template) |  Bu şablon var olan bir sanal ağda mevcut bir Azure Cosmos Core (SQL) API hesabı için özel bir uç nokta oluşturur. |
|[Ücretsiz katman Azure Cosmos hesabı oluşturma](manage-sql-with-resource-manager.md#free-tier) |  Bu şablon, ücretsiz katmanda Azure Cosmos DB Çekirdek (SQL) API hesabı oluşturur. |

## <a name="mongodb-api"></a>MongoDB API’si

|**Şablon**|**Açıklama**|
|---| ---|
|[Otomatik ölçeklendirme işleme ile bir Azure Cosmos hesabı, veritabanı, koleksiyon oluşturma](manage-mongodb-with-resource-manager.md#create-autoscale) | Bu şablon, iki bölgede MongoDB için Azure Cosmos DB API kullanarak, veritabanı düzeyinde otomatik ölçeklendirme verimini paylaşan iki kapsayıcı içeren bir hesap oluşturur. |
|[Standart (el ile) aktarım hızı ile bir Azure Cosmos hesabı, veritabanı, koleksiyon oluşturma](manage-mongodb-with-resource-manager.md#create-manual) | Bu şablon, iki bölgede MongoDB için Azure Cosmos DB API kullanarak, veritabanı düzeyinde standart aktarım hızını paylaşan iki kapsayıcı içeren bir hesap oluşturur. |

## <a name="cassandra-api"></a>Cassandra API’si

|**Şablon**|**Açıklama**|
|---| ---|
|[Bir Azure Cosmos hesabı, keyspace, otomatik ölçeklendirme işleme içeren tablo oluşturma](manage-cassandra-with-resource-manager.md#create-autoscale) | Bu şablon, anahtar uzayı ve tablo ile otomatik ölçeklendirme üretilen iş içeren iki bölgede bir Cassandra API hesabı oluşturur. |
|[Standart (el ile) aktarım hızı içeren bir Azure Cosmos hesabı, keyspace ve tablo oluşturma](manage-cassandra-with-resource-manager.md#create-manual) | Bu şablon, iki bölgede, el ile üretilen iş olan bir anahtar alanı ve tablo olan bir Cassandra API hesabı oluşturur. |

## <a name="gremlin-api"></a>Gremlin API

|**Şablon**|**Açıklama**|
|---| ---|
|[Otomatik ölçeklendirme işleme ile bir Azure Cosmos hesabı, veritabanı ve grafik oluşturma](manage-gremlin-with-resource-manager.md#create-autoscale) | Bu şablon, iki bölgede bir Gremlin API hesabı oluşturur ve otomatik ölçeklendirme işleme içeren bir veritabanı ve grafiktir. |
|[Standart (el ile) aktarım hızı ile bir Azure Cosmos hesabı, veritabanı ve grafik oluşturma](manage-gremlin-with-resource-manager.md#create-manual) | Bu şablon, iki bölgede bir Gremlin API hesabı oluşturur ve standart verimlilik içeren bir veritabanı ve grafikle. |

## <a name="table-api"></a>Tablo API’si

|**Şablon**|**Açıklama**|
|---| ---|
|[Bir Azure Cosmos hesabı, otomatik ölçeklendirme üretilen iş tablosu oluşturma](manage-table-with-resource-manager.md#create-autoscale) | Bu şablon iki bölgede bir Tablo API'si hesabı ve otomatik ölçeklendirme işleme içeren tek bir tablo oluşturur. |
|[Standart (el ile) aktarım hızı içeren bir Azure Cosmos hesabı oluşturun](manage-table-with-resource-manager.md#create-manual) | Bu şablon, iki bölgede bir Tablo API'si hesabı ve standart işleme içeren tek bir tablo oluşturur. |

Başvuru belgelerinin Azure Cosmos DB sayfasına yönelik [Azure Resource Manager başvuru](/azure/templates/microsoft.documentdb/allversions) bölümüne bakın.
