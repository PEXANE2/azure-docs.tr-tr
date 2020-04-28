---
title: Azure Cosmos DB için Azure Resource Manager şablonları
description: Azure Cosmos DB oluşturmak ve yapılandırmak için Azure Resource Manager şablonları kullanın.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/27/2020
ms.author: mjbrown
ms.openlocfilehash: 14c3438c5d6e7bed110f9e0dbfcd04655c3e9121
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82193267"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure Resource Manager şablonları

Aşağıdaki tablolar Azure Cosmos DB için Azure Resource Manager şablonlarının bağlantılarını içerir:

## <a name="sql-core-api"></a>SQL (Core) API'si

|**Şablon**|**Açıklama**|
|---|---|
|[Azure Cosmos hesabı, veritabanı, kapsayıcı oluşturma](manage-sql-with-resource-manager.md#create-resource) | Bu şablon, paylaşılan veritabanı işleme ve adanmış aktarım hızı olan bir kapsayıcı olmak üzere iki kapsayıcı ile iki bölgede bir SQL (Core) API hesabı oluşturur. Verimlilik, güncelleştirilmiş işleme özelliği değeri olan şablonu yeniden göndererek güncelleştirilemeyebilir. |
|[Saklı yordam, tetikleyici ve UDF ile bir Azure Cosmos hesabı, veritabanı ve kapsayıcısı oluşturma](manage-sql-with-resource-manager.md#create-sproc) | Bu şablon, bir kapsayıcı için bir saklı yordam, tetikleyici ve UDF ile iki bölgede bir SQL (Core) API hesabı oluşturur. |
|[Mevcut bir Azure Cosmos hesabı için özel bir uç nokta oluşturma](how-to-configure-private-endpoints.md#create-a-private-endpoint-by-using-a-resource-manager-template) |  Bu şablon, mevcut bir sanal ağdaki mevcut bir Azure Cosmos SQL API hesabı için özel bir uç nokta oluşturur. |
|[Ücretsiz katman Azure Cosmos hesabı oluşturma](manage-sql-with-resource-manager.md#free-tier) |  Bu şablon, ücretsiz katmanda bir Azure Cosmos DB hesabı oluşturur. |

## <a name="mongodb-api"></a>MongoDB API’si

|**Şablon**|**Açıklama**|
|---| ---|
|[Azure Cosmos hesabı, veritabanı, koleksiyon oluşturma](manage-mongodb-with-resource-manager.md#create-resource) | Bu şablon, çoklu yönetici özellikli iki bölgede MongoDB için Azure Cosmos DB API kullanarak bir hesap oluşturur. Azure Cosmos hesabının, veritabanı düzeyinde üretilen işi paylaşan iki kapsayıcısı olacaktır. |

## <a name="cassandra-api"></a>Cassandra API’si

|**Şablon**|**Açıklama**|
|---| ---|
|[Azure Cosmos hesabı, keyspace, tablo oluşturma](manage-cassandra-with-resource-manager.md#create-resource) | Bu şablon, çoklu yönetici özellikli iki bölgede bir Cassandra API hesabı oluşturur. Azure Cosmos hesabının, anahtar alanı düzeyinde üretilen işi paylaşan iki tablosu olacaktır. |

## <a name="gremlin-api"></a>Gremlin API

|**Şablon**|**Açıklama**|
|---| ---|
|[Azure Cosmos hesabı, veritabanı, grafik oluşturma](manage-gremlin-with-resource-manager.md#create-resource) | Bu şablon, çoklu yönetici özellikli iki bölgede Gremlin API hesabı oluşturur. Azure Cosmos hesabında, veritabanı düzeyinde üretilen işi paylaşan iki grafik olacaktır. |

## <a name="table-api"></a>Tablo API’si

|**Şablon**|**Açıklama**|
|---| ---|
|[Azure Cosmos hesabı, tablosu oluşturma](manage-table-with-resource-manager.md#create-resource) | Bu şablon, çoklu yönetici özellikli iki bölgede bir Tablo API'si hesabı oluşturur. Azure Cosmos hesabının tek bir tablosu olacaktır. |

> [!TIP]
> Tablo API'si kullanırken paylaşılan aktarım hızını etkinleştirmek için, Azure portalında hesap düzeyinde aktarım hızını etkinleştirin.

Başvuru belgelerinin Azure Cosmos DB sayfasına yönelik [Azure Resource Manager başvuru](/azure/templates/microsoft.documentdb/allversions) bölümüne bakın.
