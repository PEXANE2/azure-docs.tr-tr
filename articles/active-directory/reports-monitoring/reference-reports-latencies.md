---
title: Gecikme leri bildiren Azure Etkin Dizin | Microsoft Dokümanlar
description: Azure portalınızda olayları bildirmenin ne kadar sürdüğünü öğrenin
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 05/13/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6d031546bb4f1f05e9ea2abb5b74fe911b0b507f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74007695"
---
# <a name="azure-active-directory-reporting-latencies"></a>Azure Etkin Dizin raporlama gecikmeleri

Gecikme süresi, Azure Etkin Dizini'nin (Azure AD) verileri bildirmesinin [Azure portalında](https://portal.azure.com)gösterilmesi için gereken süredir. Bu makalede, farklı rapor türleri için beklenen gecikme durumu listelenistır. 

## <a name="activity-reports"></a>Etkinlik raporları

İki tür etkinlik raporu vardır:

- [Oturum açma -](concept-sign-ins.md) Yönetilen uygulamaların kullanımı ve kullanıcı oturum açma etkinlikleri hakkında bilgi sağlar
- [Denetim günlükleri](concept-audit-logs.md) - Kullanıcılar ve gruplar, yönetilen uygulamalar ve dizin etkinlikleri hakkında sistem etkinlik bilgileri sağlar

Aşağıdaki tabloda etkinlik raporlarının gecikme bilgileri listelenistir. 

> [!NOTE]
> **Gecikme süresi (95. yüzdelik dilimi),** günlüklerin %95'inin bildirilecek zamanı, **Gecikme Süresinin (99.yüzde)** ise günlüklerin %99'unun bildirileceği zamanı ifade eder. 
>

| Rapor | Gecikme sonu (95. yüzdelik) |Gecikme sonu (99. yüzdelik)|
| :-- | --- | --- |
| Denetim günlükleri | 2 dk.  | 5 dk.  |
| Oturum açma işlemleri | 2 dk.  | 5 dk. |

### <a name="how-soon-can-i-see-activities-data-after-getting-a-premium-license"></a>Premium lisans aldıktan sonra etkinlik verilerini ne kadar sürede görebiliyorum?

Zaten ücretsiz lisans ile etkinlik verileri varsa, o zaman yükseltme hemen görebilirsiniz. Herhangi bir veriniz yoksa, premium lisansa yükselttikten sonra verilerin raporlarda gösterilmesi bir veya iki gün sürer.

## <a name="security-reports"></a>Güvenlik raporları

İki tür güvenlik raporu vardır:

- [Riskli oturum açma işlemleri](concept-risky-sign-ins.md) - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir. 
- [Riskli oldukları belirlenen kullanıcılar](concept-user-at-risk.md) - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. 

Aşağıdaki tabloda güvenlik raporlarıiçin gecikme bilgileri listelenistır.

| Rapor | Minimum | Ortalama | Maksimum |
| :-- | --- | --- | --- |
| Risk altındaki kullanıcılar          | 5 dakika   | 15 dakika  | 2 saat  |
| Riskli oturum açma işlemleri         | 5 dakika   | 15 dakika  | 2 saat  |

## <a name="risk-detections"></a>Risk algılamaları

Azure AD, kullanıcı hesaplarınızla ilgili şüpheli eylemleri algılamak için uyarlanabilir makine öğrenme algoritmaları ve buluşsal algoritmalar kullanır. Algılanan her şüpheli **eylem, risk algılama**adı verilen bir kayıtta depolanır.

Aşağıdaki tabloda risk algılamaları için gecikme bilgileri listelenistır.

| Rapor | Minimum | Ortalama | Maksimum |
| :-- | --- | --- | --- |
| Anonim IP adreslerinden oturum açma işlemleri |5 dakika |15 Dakika |2 saat |
| Alışılmadık konumlardan oturum açma işlemleri |5 dakika |15 Dakika |2 saat |
| Sızan kimlik bilgilerine sahip kullanıcılar |2 saat |4 saat |8 saat |
| Alışılmadık konumlara imkansız seyahat |5 dakika |1 saat |8 saat  |
| Bulaşma olan cihazlardan oturum açma işlemleri |2 saat |4 saat |8 saat  |
| Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri |2 saat |4 saat |8 saat  |


## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD raporlarına genel bakış](overview-reports.md)
* [Azure AD raporlarına programlı erişim](concept-reporting-api.md)
* [Azure Active Directory risk algılamaları](concept-risk-events.md)
