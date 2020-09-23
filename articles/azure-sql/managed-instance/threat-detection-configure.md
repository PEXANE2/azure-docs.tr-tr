---
title: Gelişmiş Tehdit Koruması'nı yapılandırma
titleSuffix: Azure SQL Managed Instance
description: Gelişmiş tehdit koruması, Azure SQL yönetilen örneği 'nde veritabanına ilişkin olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: security
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto
ms.date: 09/21/2020
ms.openlocfilehash: d81550f5c6906e409675002fde670db3734de883
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90885060"
---
# <a name="configure-advanced-threat-protection-in-azure-sql-managed-instance"></a>Azure SQL yönetilen örneği 'nde Gelişmiş tehdit koruması yapılandırma
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

[Azure SQL yönetilen örneği](sql-managed-instance-paas-overview.md) Için [Gelişmiş tehdit koruması](../database/threat-detection-overview.md) , veritabanları için olağandışı ve olası zararlı girişimleri gösteren anormal etkinlikleri algılar. Gelişmiş tehdit koruması, **OLASı SQL ekleme**, **olağan dışı konum veya veri merkezinden erişim**, **bilinmeyen bir sorumlu veya zararlı uygulamadan ERIŞIM**ve **deneme yanılma SQL kimlik bilgilerini** belirleyebilir. [Gelişmiş tehdit koruması uyarıları](../database/threat-detection-overview.md#alerts)hakkında daha fazla ayrıntı için bkz..

Algılanan tehditler hakkında [e-posta bildirimleri](../database/threat-detection-overview.md#explore-detection-of-a-suspicious-event) veya [Azure Portal](../database/threat-detection-overview.md#explore-alerts-in-the-azure-portal) ile ilgili bildirimler alabilirsiniz

[Gelişmiş tehdit koruması](../database/threat-detection-overview.md) , gelişmiş SQL güvenlik özelliklerine yönelik Birleşik bir paket olan [SQL teklifi için Azure Defender](../database/azure-defender-for-sql.md)  'ın bir parçasıdır. Gelişmiş tehdit korumasına, SQL Portal için merkezi Azure Defender aracılığıyla erişilebilir ve yönetebilirsiniz.

##  <a name="azure-portal"></a>Azure portal

1. [Azure Portal](https://portal.azure.com)oturum açın. 
2. Korumak istediğiniz SQL yönetilen örneği örneğinin yapılandırma sayfasına gidin. **Güvenlik**altında **Güvenlik Merkezi**' ni seçin.
3. SQL için Azure Defender yapılandırma sayfasında
   - SQL **için Azure Defender 'ı açın** .
   - Anormal veritabanı etkinliklerinin algılanması üzerine güvenlik uyarıları almak için **e-posta listesini** yapılandırın.
   - Anormal tehdit denetim kayıtlarının kaydedildiği **Azure Storage hesabını** seçin.
   - Yapılandırmak istediğiniz **Gelişmiş tehdit koruması türlerini** seçin. [Gelişmiş tehdit koruması uyarıları](../database/threat-detection-overview.md)hakkında daha fazla bilgi edinin.
4. Yeni veya güncelleştirilmiş SQL için Azure Defender ilkesini kaydetmek için **Kaydet** ' e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Gelişmiş tehdit koruması](../database/threat-detection-overview.md)hakkında daha fazla bilgi edinin.
- Yönetilen örnekler hakkında bilgi edinin, bkz. [Azure SQL yönetilen örneği nedir](sql-managed-instance-paas-overview.md).
- [Azure SQL veritabanı Için Gelişmiş tehdit koruması](../database/threat-detection-configure.md)hakkında daha fazla bilgi edinin.
- [SQL yönetilen örnek denetimi](https://go.microsoft.com/fwlink/?linkid=869430)hakkında daha fazla bilgi edinin.
- [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)hakkında daha fazla bilgi edinin.
