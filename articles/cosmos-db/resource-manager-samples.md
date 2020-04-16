---
title: Azure Cosmos DB için Azure Resource Manager şablonları
description: Azure Cosmos DB oluşturmak ve yapılandırmak için Azure Kaynak Yöneticisi şablonlarını kullanın.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mjbrown
ms.openlocfilehash: 1c4f1a1920d98052231eaa3a7d5c0454441c88ee
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81390872"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure Resource Manager şablonları

Aşağıdaki tablolar, Azure Cosmos DB için Azure Kaynak Yöneticisi şablonlarına bağlantılar içerir:

## <a name="sql-core-api"></a>SQL (Core) API'si

|**Şablon**|**Açıklama**|
|---|---|
|[Azure Cosmos hesabı, veritabanı, kapsayıcı oluşturma](manage-sql-with-resource-manager.md#create-resource) | Bu şablon, paylaşılan veritabanı iş veritabanı iş tinemi ve özel iş girdisi olan bir kapsayıcı ile iki kapsayıcı ile iki bölgede bir SQL (Core) API hesabı oluşturur. İş verme değeri, şablonu güncelleştirilmiş iş sahibi özellik değeriyle yeniden göndererek güncelleştirilebilir. |
|[Depolanmış yordam, tetikleme ve UDF içeren bir Azure Cosmos hesabı, veritabanı ve kapsayıcı oluşturun](manage-sql-with-resource-manager.md#create-sproc) | Bu şablon, bir kapsayıcı için depolanmış yordam, tetikleyici ve UDF ile iki bölgede bir SQL (Core) API hesabı oluşturur. |
|[Varolan bir Azure Cosmos hesabı için özel bir bitiş noktası oluşturma](how-to-configure-private-endpoints.md#create-a-private-endpoint-by-using-a-resource-manager-template) |  Bu şablon, varolan bir sanal ağdaki varolan Bir Azure Cosmos SQL API hesabı için özel bir bitiş noktası oluşturur. |

## <a name="mongodb-api"></a>MongoDB API’si

|**Şablon**|**Açıklama**|
|---| ---|
|[Azure Cosmos hesabı, veritabanı, koleksiyon oluşturma](manage-mongodb-with-resource-manager.md#create-resource) | Bu şablon, mongodb için Azure Cosmos DB API'sini kullanarak, çok büyük etkinleştirilmiş iki bölgede bir hesap oluşturur. Azure Cosmos hesabında veritabanı düzeyinde iş verisini paylaşan iki kapsayıcı bulunur. |

## <a name="cassandra-api"></a>Cassandra API’si

|**Şablon**|**Açıklama**|
|---| ---|
|[Azure Cosmos hesabı, keyspace, tablo oluşturma](manage-cassandra-with-resource-manager.md#create-resource) | Bu şablon, çok büyük etkin iki bölgede bir Cassandra API hesabı oluşturur. Azure Cosmos hesabında anahtar alanı düzeyinde iş paylaşımı yapan iki tablo olacaktır. |

## <a name="gremlin-api"></a>Gremlin API

|**Şablon**|**Açıklama**|
|---| ---|
|[Azure Cosmos hesabı, veritabanı, grafik oluşturma](manage-gremlin-with-resource-manager.md#create-resource) | Bu şablon, çok büyük etkin iki bölgede bir Gremlin API hesabı oluşturur. Azure Cosmos hesabı, veritabanı düzeyinde iş verisini paylaşan iki grafiğe sahip olacaktır. |

## <a name="table-api"></a>Tablo API’si

|**Şablon**|**Açıklama**|
|---| ---|
|[Azure Cosmos hesabı oluşturma, tablo](manage-table-with-resource-manager.md#create-resource) | Bu şablon, çok büyük etkin iki bölgede bir Tablo API hesabı oluşturur. Azure Cosmos hesabının tek bir tablosu olacaktır. |

> [!TIP]
> Table API kullanırken paylaşılan iş artışını etkinleştirmek için Azure Portalı'nda hesap düzeyinde iş paylaşımını etkinleştirin.

Başvuru belgeleri [için Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions) sayfası için Azure Kaynak Yöneticisi başvurusuna bakın.
