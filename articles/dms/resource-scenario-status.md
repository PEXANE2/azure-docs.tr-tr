---
title: Veritabanı geçiş senaryosunun durumu
titleSuffix: Azure Database Migration Service
description: Azure Veritabanı Geçiş Hizmeti tarafından desteklenen geçiş senaryolarının durumu hakkında bilgi edinin.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 09/05/2019
ms.openlocfilehash: 9652b78674d6a6b905eb049564d1b17cdc7c17b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78254924"
---
# <a name="status-of-migration-scenarios-supported-by-azure-database-migration-service"></a>Azure Veritabanı Geçiş Hizmeti tarafından desteklenen geçiş senaryolarının durumu

Azure Veritabanı Geçiş Hizmeti, hem çevrimdışı (tek seferlik) hem de çevrimiçi (sürekli eşitleme) geçişler için farklı geçiş senaryolarını (kaynak/hedef çiftleri) desteklemek üzere tasarlanmıştır. Azure Veritabanı Geçiş Hizmeti tarafından sağlanan senaryo kapsamı zaman içinde genişletiliyor. Düzenli olarak yeni senaryolar ekleniyor. Bu makalede, şu anda Azure Veritabanı Geçiş Hizmeti tarafından desteklenen geçiş senaryoları ve her senaryo için durum (özel önizleme, genel önizleme veya genel kullanılabilirlik) tanımlanmaktadır.

## <a name="offline-versus-online-migrations"></a>Çevrimiçi geçişlere karşı çevrimdışı

Azure Veritabanı Geçiş Hizmeti ile çevrimdışı veya çevrimiçi geçiş yapabilirsiniz. *Çevrimdışı* geçişlerde, uygulama kapalı kalma süresi geçişin başladığı anda başlar. Geçiş tamamlandığında yeni ortama kesinti yapmak için gereken süreyle kapalı kalma süresini sınırlamak için *çevrimiçi* geçiş kullanın. Kapalı kalma süresinin kabul edilebilir olup olmadığını belirlemek için çevrimdışı geçişin sınanması önerilir; değilse, çevrimiçi geçiş yapın.

## <a name="migration-scenario-status"></a>Geçiş senaryosu durumu

Azure Veritabanı Geçiş Hizmeti tarafından desteklenen geçiş senaryolarının durumu zamana göre değişir. Genellikle, senaryolar ilk **özel önizleme**yayımlanır. Özel önizlemeye katılmak, müşterilerin [DMS Preview sitesi](https://aka.ms/dms-preview)üzerinden aday göstermelerini gerektirir. Özel önizlemeden sonra, senaryo durumu **genel önizlemeye**dönüşür. Azure Veritabanı Geçiş Hizmeti kullanıcıları, genel önizlemedeki geçiş senaryolarını doğrudan kullanıcı arabiriminden deneyebilir. Kayıt gerekmez.  Ancak, genel önizlemedeki geçiş senaryoları tüm bölgelerde kullanılamayabilir ve son sürümden önce ek değişikliklere uğrayabilir. Genel önizlemeden sonra, senaryo durumu **genel kullanılabilirlik**durumuna dönüşür. Genel kullanılabilirlik (GA) son sürüm durumudur ve işlevsellik tamamlandı ve tüm kullanıcılar tarafından erişilebilir.

## <a name="migration-scenario-support"></a>Geçiş senaryosu desteği

Aşağıdaki tablolar, Azure Veritabanı Geçiş Hizmeti kullanırken hangi geçiş senaryolarının destekleniyi gösterir.

> [!NOTE]
> Aşağıda desteklenen bir senaryo kullanıcı arabiriminde görünmüyorsa, ek bilgi için lütfen [Azure Veritabanı Geçişleri Sor](mailto:AskAzureDatabaseMigrations@service.microsoft.com) takma adıyla iletişime geçin.

> [!IMPORTANT]
> Azure Veritabanı Geçiş Hizmeti tarafından desteklenen tüm senaryoları Özel Önizleme'de görüntülemek için [DMS Önizleme sitesine](https://aka.ms/dms-preview)bakın.

### <a name="offline-one-time-migration-support"></a>Çevrimdışı (tek seferlik) geçiş desteği

Aşağıdaki tabloda, çevrimdışı geçişler için Azure Veritabanı Geçiş Hizmeti desteği gösterilmektedir.

| Hedef  | Kaynak | Destek | Durum |
| ------------- | ------------- |:-------------:|:-------------:|
| **Azure SQL DB** | SQL Server | ✔ | GA |
|   | RDS SQL |  |  |
|   | Oracle |  |  |
| **Azure SQL DB MI** | SQL Server | ✔ | GA |
|   | RDS SQL |  |  |
|   | Oracle |  |   |
| **Azure SQL VM** | SQL Server | ✔ | GA |
|   | Oracle |   |   |
| **Azure Cosmos DB** | MongoDB | ✔ | GA |
| **MySQL için Azure DB** | MySQL |   |   |
|   | RDS MySQL |   |   |
| **PostgreSQL için Azure DB** | PostgreSQL |  |
|  | RDS PostgreSQL |   |   |

### <a name="online-continuous-sync-migration-support"></a>Çevrimiçi (sürekli eşitleme) geçiş desteği

Aşağıdaki tabloda, çevrimiçi geçişler için Azure Veritabanı Geçiş Hizmeti desteği gösterilmektedir.

| Hedef  | Kaynak | Destek | Durum |
| ------------- | ------------- |:-------------:|:-------------:|
| **Azure SQL DB** | SQL Server | ✔ | GA |
|   | RDS SQL | ✔ | GA |
|   | Oracle |  |  |
| **Azure SQL DB MI** | SQL Server | ✔ | GA |
|   | RDS SQL | ✔ | GA |
|   | Oracle | ✔ | Özel önizleme |
| **Azure SQL VM** | SQL Server |   |   |
|   | Oracle  |  |  |
| **Azure Cosmos DB** | MongoDB | ✔ | GA |
| **MySQL için Azure DB** | MySQL | ✔ | GA |
|   | RDS MySQL | ✔ | GA |
| **PostgreSQL için Azure DB** | PostgreSQL | ✔ | GA |
|   | RDS PostgreSQL | ✔ | GA |
|   | Oracle | ✔ | Genel önizleme |

## <a name="next-steps"></a>Sonraki adımlar

Azure Veritabanı Geçiş Hizmeti'ne ve bölgesel kullanılabilirlik bölümüne genel bir bakış için, Azure [Veritabanı Geçiş Hizmeti nedir makalesine](dms-overview.md)bakın.
