---
title: Azure Cosmos DB için Azure Resource Manager Şablonlar
description: Azure Cosmos DB oluşturmak ve yapılandırmak için Azure Resource Manager şablonları kullanın.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: mjbrown
ms.openlocfilehash: ecdfe89e899e0d416784ae32a0d66b335c09e2b6
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73582818"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure Resource Manager Şablonlar

Aşağıdaki tablolar Azure Cosmos DB için Azure Resource Manager şablonlarının bağlantılarını içerir:

## <a name="sql-core-api"></a>SQL (Core) API

|**Şablon**|**Açıklama**|
|---|---|
|[Azure Cosmos hesabı, veritabanı, kapsayıcı oluşturma](manage-sql-with-resource-manager.md#create-resource) | Bu şablon, paylaşılan veritabanı işleme ve adanmış aktarım hızı olan bir kapsayıcı olmak üzere iki kapsayıcı ile iki bölgede bir SQL (Core) API hesabı oluşturur. Verimlilik, güncelleştirilmiş işleme özelliği değeri olan şablonu yeniden göndererek güncelleştirilemeyebilir. |
|[Saklı yordam, tetikleyici ve UDF ile bir Azure Cosmos hesabı, veritabanı ve kapsayıcısı oluşturma](manage-sql-with-resource-manager.md#create-sproc) | Bu şablon, bir kapsayıcı için bir saklı yordam, tetikleyici ve UDF ile iki bölgede bir SQL (Core) API hesabı oluşturur. |

## <a name="mongodb-api"></a>MongoDB API’si

|**Şablon**|**Açıklama**|
|---| ---|
|[Azure Cosmos hesabı, veritabanı, koleksiyon oluşturma](manage-mongodb-with-resource-manager.md#create-resource) | Bu şablon, çoklu yönetici özellikli iki bölgede MongoDB için Azure Cosmos DB API kullanarak bir hesap oluşturur. Azure Cosmos hesabının, veritabanı düzeyinde üretilen işi paylaşan iki kapsayıcısı olacaktır. |
|[Bir veritabanı için güncelleştirme üretilen iş (RU/sn)](manage-mongodb-with-resource-manager.md#database-ru-update) | Bu şablon bir MongoDB API hesabındaki bir veritabanının aktarım hızını güncelleştirir. |
|[Bir koleksiyon için güncelleştirme üretilen iş (RU/sn)](manage-mongodb-with-resource-manager.md#collection-ru-update) | Bu şablon bir MongoDB API hesabındaki bir kapsayıcının aktarım hızını güncelleştirir. |

## <a name="cassandra-api"></a>Cassandra API’si

|**Şablon**|**Açıklama**|
|---| ---|
|[Azure Cosmos hesabı, keyspace, tablo oluşturma](manage-cassandra-with-resource-manager.md#create-resource) | Bu şablon, çoklu yönetici özellikli iki bölgede bir Cassandra API hesabı oluşturur. Azure Cosmos hesabının, anahtar alanı düzeyinde üretilen işi paylaşan iki tablosu olacaktır. |
|[Anahtar alanı için üretilen iş (RU/s) güncelleştirme](manage-cassandra-with-resource-manager.md#keyspace-ru-update) | Bu şablon Cassandra API hesabındaki bir anahtar alanı için üretilen işi güncelleştirir. |
|[Bir tablo için üretilen iş (RU/s) güncelleştirme](manage-cassandra-with-resource-manager.md#table-ru-update) | Bu şablon Cassandra API hesabındaki bir tablo için üretilen işi güncelleştirir. |

## <a name="gremlin-api"></a>Gremlin API

|**Şablon**|**Açıklama**|
|---| ---|
|[Azure Cosmos hesabı, veritabanı, grafik oluşturma](manage-gremlin-with-resource-manager.md#create-resource) | Bu şablon, çoklu yönetici özellikli iki bölgede Gremlin API hesabı oluşturur. Azure Cosmos hesabında, veritabanı düzeyinde üretilen işi paylaşan iki grafik olacaktır. |
|[Bir veritabanı için güncelleştirme üretilen iş (RU/sn)](manage-gremlin-with-resource-manager.md#database-ru-update) | Bu şablon Gremlin API hesabındaki bir veritabanının aktarım hızını güncelleştirir. |
|[Grafik için üretilen iş (RU/s) güncelleştirme](manage-gremlin-with-resource-manager.md#graph-ru-update) | Bu şablon Gremlin API hesabındaki bir grafiğin aktarım hızını güncelleştirir. |

## <a name="table-api"></a>Tablo API’si

|**Şablon**|**Açıklama**|
|---| ---|
|[Azure Cosmos hesabı, tablosu oluşturma](manage-table-with-resource-manager.md#create-resource) | Bu şablon, çoklu yönetici özellikli iki bölgede bir Tablo API'si hesabı oluşturur. Azure Cosmos hesabının tek bir tablosu olacaktır. |
|[Bir tablo için üretilen iş (RU/s) güncelleştirme](manage-table-with-resource-manager.md#table-ru-update) | Bu şablon Tablo API'si hesabındaki bir tablo için üretilen işi güncelleştirir. |

> [!TIP]
> Tablo API'si kullanırken paylaşılan aktarım hızını etkinleştirmek için, Azure portalında hesap düzeyinde aktarım hızını etkinleştirin.

Başvuru belgeleri için [Azure Cosmos DB sayfasına yönelik ARM başvurusuna](/azure/templates/microsoft.documentdb/allversions) bakın.
