---
title: Azure Güvenlik Merkezi 'nde veri Hizmetleri için tehdit algılama | Microsoft Docs
description: Bu makalede, Azure Güvenlik Merkezi 'nde bulunan veri Hizmetleri uyarıları sunulmaktadır.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: da960861-0b6c-4d80-932d-898cdebb4f83
ms.service: security-center
ms.topic: conceptual
ms.date: 01/05/2020
ms.author: memildin
ms.openlocfilehash: 6d6e48c84f558840b3266193c4cbb9c3576f1a58
ms.sourcegitcommit: ff9688050000593146b509a5da18fbf64e24fbeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75665743"
---
# <a name="threat-detection-for-data-services-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde veri Hizmetleri için tehdit algılama

 Azure Güvenlik Merkezi, veri depolama hizmetlerinin günlüklerini analiz eder ve veri kaynaklarınız için bir tehdit algıladığında uyarıları tetikler. Bu makalede, güvenlik merkezi 'nin aşağıdaki hizmetler için oluşturduğu uyarılar listelenmektedir:

* [Azure SQL veritabanı ve Azure SQL veri ambarı](#data-sql)
* [Azure depolama alanı](#azure-storage)
* [Azure Cosmos DB](#cosmos-db)

## SQL veritabanı ve SQL veri ambarı<a name="data-sql"></a>

Azure SQL veritabanı için Gelişmiş tehdit koruması, veritabanları için olağan dışı ve olası zararlı girişimleri gösteren anormal etkinlikleri algılar.

Şüpheli veritabanı etkinlikleri, olası güvenlik açıkları veya SQL ekleme saldırıları ve anormal veritabanı erişimi ve sorgu desenleri olduğunda uyarılar görürsünüz.

Azure SQL veritabanı ve SQL için Gelişmiş tehdit koruması, Azure SQL veritabanları, Azure SQL veritabanı yönetilen örnekleri, Azure SQL veri ambarı veritabanları ve Azure sanal makinelerinde SQL Server 'lar kapsayan gelişmiş SQL güvenlik özelliklerine yönelik [Gelişmiş veri güvenliği (ADS)](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security) Birleşik paketinin bir parçasıdır.

Daha fazla bilgi için bkz.

* [Azure SQL veritabanı için Gelişmiş tehdit korumasını etkinleştirme](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview)
* [Azure sanal makinelerinde SQL Server 'lar için Gelişmiş tehdit korumasını etkinleştirme](security-center-iaas-advanced-data.md)
* [SQL veritabanı ve SQL veri ambarı için tehdit koruması uyarılarının listesi](alerts-reference.md#alerts-sql-db-and-warehouse)

## Azure depolama<a name="azure-storage"></a>

Depolama için Gelişmiş tehdit koruması (Şu anda yalnızca BLOB depolama için kullanılabilir), depolama hesaplarına erişme veya açıktan yararlanma amacıyla olağan dışı ve potansiyel olarak zararlı girişimleri algılar. Bu koruma katmanı, güvenlik uzmanı olmanıza gerek kalmadan tehditleri sağlamanıza olanak tanır ve güvenlik izleme sistemlerinizi yönetmenize yardımcı olur.

>[!NOTE]
>Depolama için Gelişmiş tehdit koruması Şu anda Azure Kamu ve bağımsız bulut bölgelerinde kullanılamaz.

Daha fazla bilgi için bkz.

* [Azure depolama için Gelişmiş tehdit koruması 'nı etkinleştirme](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection)
* [Azure depolama için tehdit koruması uyarılarının listesi](alerts-reference.md#alerts-azurestorage)

## Azure Cosmos DB<a name="cosmos-db"></a>

Azure Cosmos DB uyarıları, Azure Cosmos DB hesaplarına erişmek veya bu hesaplara yararlanmaya yönelik olağan dışı ve potansiyel olarak zararlı denemelere göre oluşturulur.

Daha fazla bilgi için bkz.

* [Azure Cosmos DB için Gelişmiş tehdit koruması (Önizleme)](../cosmos-db/cosmos-db-advanced-threat-protection.md)
* [Azure Cosmos DB için tehdit koruması uyarıları listesi (Önizleme)](alerts-reference.md#alerts-azurecosmos)
