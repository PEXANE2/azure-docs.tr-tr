---
title: Gelişmiş Tehdit Koruması'nı yapılandırma
titleSuffix: Azure SQL Managed Instance
description: Gelişmiş tehdit koruması, Azure SQL yönetilen örneği 'nde veritabanına ilişkin olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto
ms.date: 08/05/2019
ms.openlocfilehash: a60156762a4d8dfb6b11ae70e608fb26b07e5764
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84045763"
---
# <a name="configure-advanced-threat-protection-in-azure-sql-managed-instance"></a>Azure SQL yönetilen örneği 'nde Gelişmiş tehdit koruması yapılandırma
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

[Azure SQL yönetilen örneği](sql-managed-instance-paas-overview.md) Için [Gelişmiş tehdit koruması](../database/threat-detection-overview.md) , veritabanları için olağandışı ve olası zararlı girişimleri gösteren anormal etkinlikleri algılar. Gelişmiş tehdit koruması, **OLASı SQL ekleme**, **olağan dışı konum veya veri merkezinden erişim**, **bilinmeyen bir sorumlu veya zararlı uygulamadan ERIŞIM**ve **deneme yanılma SQL kimlik bilgilerini** belirleyebilir. [Gelişmiş tehdit koruması uyarıları](../database/threat-detection-overview.md#alerts)hakkında daha fazla ayrıntı için bkz..

Algılanan tehditler hakkında [e-posta bildirimleri](../database/threat-detection-overview.md#explore-detection-of-a-suspicious-event) veya [Azure Portal](../database/threat-detection-overview.md#explore-alerts-in-azure-portal) ile ilgili bildirimler alabilirsiniz

Gelişmiş [tehdit koruması](../database/threat-detection-overview.md) , gelişmiş SQL güvenlik özelliklerine yönelik Birleşik bir paket olan [Gelişmiş veri güvenliği](../database/advanced-data-security.md) sunumunun bir parçasıdır. Gelişmiş Tehdit Koruması'na merkezi SQL ADS portalı yoluyla erişilebilir ve yönetilebilir.

##  <a name="azure-portal"></a>Azure portal

1. [Azure Portal](https://portal.azure.com)oturum açın. 
2. Korumak istediğiniz SQL yönetilen örneğinin yapılandırma sayfasına gidin. **Ayarlar** sayfasında **Gelişmiş veri güvenliği**' ni seçin.
3. Gelişmiş veri güvenliği yapılandırma sayfasında
   - Gelişmiş **veri** güvenliğini açın.
   - Anormal veritabanı etkinliklerinin algılanması üzerine güvenlik uyarıları almak için **e-posta listesini** yapılandırın.
   - Anormal tehdit denetim kayıtlarının kaydedildiği **Azure Storage hesabını** seçin.
   - Yapılandırmak istediğiniz **Gelişmiş tehdit koruması türlerini** seçin. [Gelişmiş tehdit koruması uyarıları](../database/threat-detection-overview.md)hakkında daha fazla bilgi edinin.
4. Yeni veya güncelleştirilmiş gelişmiş veri güvenliği ilkesini kaydetmek için **Kaydet** ' e tıklayın.

   ![Gelişmiş Tehdit Koruması](./media/threat-detection-configure/threat-detection.png)


## <a name="next-steps"></a>Sonraki adımlar

- [Gelişmiş tehdit koruması](../database/threat-detection-overview.md)hakkında daha fazla bilgi edinin.
- Yönetilen örnekler hakkında bilgi edinin, bkz. [Azure SQL yönetilen örneği nedir](sql-managed-instance-paas-overview.md).
- [Azure SQL veritabanı Için Gelişmiş tehdit koruması](../database/threat-detection-configure.md)hakkında daha fazla bilgi edinin.
- [SQL yönetilen örnek denetimi](https://go.microsoft.com/fwlink/?linkid=869430)hakkında daha fazla bilgi edinin.
- [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)hakkında daha fazla bilgi edinin.
