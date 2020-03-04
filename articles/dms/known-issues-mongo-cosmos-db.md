---
title: "Bilinen sorunlar: MongoDB 'den Azure CosmosDB 'ye geçiş"
titleSuffix: Azure Database Migration Service
description: Azure veritabanı geçiş hizmeti kullanılarak MongoDB ile Azure Cosmos DB geçişlerle ilgili bilinen sorunlar ve geçiş sınırlamaları hakkında bilgi edinin.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 02/27/2020
ms.openlocfilehash: 194da036260a78b27748dfc7f755212ab4f30b1e
ms.sourcegitcommit: d4a4f22f41ec4b3003a22826f0530df29cf01073
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78256023"
---
# <a name="known-issuesmigration-limitations-with-migrations-from-mongodb-to-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB 'den Azure Cosmos DB MongoDB için API 'sine yapılan geçişlerle ilgili bilinen sorunlar/geçiş sınırlamaları

MongoDB 'den Cosmos DB 'ın MongoDB için API 'sine yapılan geçişlerle ilişkili bilinen sorunlar ve sınırlamalar aşağıdaki bölümlerde açıklanmıştır.

## <a name="migration-fails-as-a-result-of-using-the-incorrect-ssl-cert"></a>Yanlış SSL sertifikası kullanmanın bir sonucu olarak geçiş başarısız olur

* **Belirti**: bir Kullanıcı MongoDB kaynak sunucusuna bağlanamadığınızda bu sorun görünür. Tüm güvenlik duvarı bağlantı noktalarının açık olmasına rağmen Kullanıcı hala bağlanamaz.

| Nedeni         | Çözüm |
| ------------- | ------------- |
| Azure veritabanı geçiş hizmeti 'nde otomatik olarak imzalanan bir sertifika kullanmak, yanlış SSL sertifikası nedeniyle geçiş başarısız olabilir. Hata iletisi "uzak sertifika, doğrulama yordamına göre geçersiz." iletisini içerebilir. | CA 'dan gerçek bir sertifika kullanın.  Otomatik olarak imzalanan sertifikalar genellikle yalnızca iç testlerde kullanılır. Bir CA yetkilisinden orijinal bir sertifika yüklediğinizde, Azure veritabanı geçiş hizmeti 'nde SSL 'yi (Cosmos DB bağlantısı Mongo API üzerinden SSL kullanmak) kullanabilirsiniz.<br><br> |

## <a name="unable-to-get-the-list-of-databases-to-map-in-dms"></a>DMS 'de eşlenecek veritabanlarının listesi alınamıyor

* **Belirti**: **Kaynak Seç** dikey penceresinde **Azure Storage modundayken veri** kullanılırken **veritabanı ayarı** dikey penceresinde DB listesi alınamıyor.

| Nedeni         | Çözüm |
| ------------- | ------------- |
| Depolama hesabı bağlantı dizesinde SAS bilgileri eksik ve bu nedenle kimlik doğrulaması yapılamıyor. | Depolama Gezgini içindeki blob kapsayıcısında SAS oluşturun ve kaynak ayrıntısı bağlantı dizesi olarak kapsayıcı SAS bilgileri ile URL 'YI kullanın.<br><br> |

## <a name="using-an-unsupported-version-of-the-database"></a>Desteklenmeyen bir veritabanının sürümünü kullanma

* **Belirti**: geçiş başarısız olur.

| Nedeni         | Çözüm |
| ------------- | ------------- |
| Azure Cosmos DB, MongoDB 'nin desteklenmeyen bir sürümünden geçirmeye çalıştığınızda. | MongoDB 'nin yeni sürümleri kullanıma sunulduktan sonra, Azure veritabanı geçiş hizmeti ile uyumluluğu sağlamak için test edilir ve hizmet en son sürümleri kabul etmek için düzenli olarak güncelleştirilir. Geçiş yapmak için hemen bir sorun oluşursa, geçici bir çözüm olarak veritabanlarını/koleksiyonları Azure depolama 'ya aktarabilir ve kaynağı elde edilen döküm noktasına geçirebilirsiniz. Depolama Gezgini içindeki blob kapsayıcısında SAS oluşturun ve kaynak ayrıntısı bağlantı dizesi olarak kapsayıcı SAS bilgileri ile URL 'YI kullanın.<br><br> |

## <a name="next-steps"></a>Sonraki adımlar

* [MongoDB 'YI DMS kullanarak MongoDB Online için Azure Cosmos DB API 'Sine geçirme](tutorial-mongodb-cosmos-db-online.md)öğreticisini görüntüleyin.
* [MongoDB 'in, DMS kullanarak MongoDB için Azure Cosmos DB API 'Sine geçiş](tutorial-mongodb-cosmos-db.md)öğreticisini görüntüleyin.