---
title: "Bilinen sorunlar: MongoDB'den Azure CosmosDB'ye geçiş"
titleSuffix: Azure Database Migration Service
description: Azure Veritabanı Geçiş Hizmeti'ni kullanarak MongoDB'den Azure Cosmos DB'ye geçişlerle ilgili bilinen sorunlar ve geçiş sınırlamaları hakkında bilgi edinin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78256023"
---
# <a name="known-issuesmigration-limitations-with-migrations-from-mongodb-to-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB'den Azure Cosmos DB'nin MongoDB api'sine yapılan geçişlerle ilgili bilinen sorunlar/geçiş sınırlamaları

MongoDB'den Cosmos DB'nin MongoDB api'sine yapılan göçlerle ilgili bilinen sorunlar ve sınırlamalar aşağıdaki bölümlerde açıklanmıştır.

## <a name="migration-fails-as-a-result-of-using-the-incorrect-ssl-cert"></a>Geçiş, yanlış SSL Cert'in kullanılması sonucunda başarısız olur

* **Belirti**: Bir kullanıcı MongoDB kaynak sunucusuna bağlanamıyorsa bu sorun belirgindir. Tüm güvenlik duvarı bağlantı noktaları açık olmasına rağmen, kullanıcı hala bağlanamıyor.

| Nedeni         | Çözüm |
| ------------- | ------------- |
| Azure Veritabanı Geçiş Hizmeti'nde kendi imzalı bir sertifika kullanmak, yanlış SSL Cert nedeniyle geçişin başarısız lığa yol açabilir. Hata iletisi "Uzak sertifika doğrulama prosedürüne göre geçersizdir" içerebilir. | CA'dan orijinal bir sertifika kullanın.  Kendi imzalı sertifikalar genellikle yalnızca iç testlerde kullanılır. Bir CA yetkilisinden orijinal bir sertifika yüklediğinizde, Azure Veritabanı Geçiş Hizmeti'nde SSL'yi sorunsuz bir şekilde kullanabilirsiniz (Cosmos DB bağlantıları Mongo API üzerinden SSL kullanır).<br><br> |

## <a name="unable-to-get-the-list-of-databases-to-map-in-dms"></a>DMS'de eşlenecek veritabanlarının listesini alamıyor

* **Belirti**: **Kaynak** bıçak seç'te **Azure Depolama modundan Veriler** kullanılırken Veritabanı **ayarı** bıçağında DB listesi alınamıyor.

| Nedeni         | Çözüm |
| ------------- | ------------- |
| Depolama hesabı bağlantı dizesi SAS bilgisi eksikve bu nedenle doğrulaması yapılamaz. | Depolama Gezgini'ndeki blob kapsayıcısında SAS'ı oluşturun ve kaynak ayrıntı bağlantı dizesi olarak kapsayıcı SAS bilgisine sahip URL'yi kullanın.<br><br> |

## <a name="using-an-unsupported-version-of-the-database"></a>Veritabanının desteklenmeyen sürümünü kullanma

* **Belirti**: Geçiş başarısız olur.

| Nedeni         | Çözüm |
| ------------- | ------------- |
| MongoDB'nin desteklenmeyen bir sürümünden Azure Cosmos DB'ye geçiş yapmaya çalışırsınız. | MongoDB'nin yeni sürümleri yayımlandıkça, Azure Veritabanı Geçiş Hizmeti ile uyumluluğu sağlamak için test edilirler ve hizmet en son sürümü(ler) kabul etmek üzere düzenli aralıklarla güncellenmektedir. Hemen geçiş gerekirse, geçici çözüm olarak veritabanlarını/koleksiyonları Azure Depolama'ya ve kaynağın ortaya çıkan döküme yönlendirebileceğiniz. Depolama Gezgini'ndeki blob kapsayıcısında SAS'ı oluşturun ve ardından kaynak ayrıntı bağlantı dizesi olarak kapsayıcı SAS bilgisine sahip URL'yi kullanın.<br><br> |

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici Geçir MongoDB'yi Azure Cosmos DB'nin MongoDB için Çevrimiçi DMS'deki API'sine geçirin.](tutorial-mongodb-cosmos-db-online.md)
* Öğretici [Yi Geçir MongoDB'yi DMS kullanarak MongoDB'nin MongoDB'un çevrimdışı için Azure Cosmos DB'sAP'sine geçirin.](tutorial-mongodb-cosmos-db.md)