---
title: Azure AD raporlama verilerini ne kadar süreyle depolar? | Microsoft Belgeleri
description: Azure'un çeşitli raporlama verilerini ne kadar süreyle depoladığını öğrenin.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: ''
ms.topic: reference
ms.tgt_pltfrm: ''
ms.workload: identity
ms.subservice: report-monitor
ms.date: 03/24/2020
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 54636600c208f8f5df9fa2e25460c63dd9f46e85
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80239554"
---
# <a name="how-long-does-azure-ad-store-reporting-data"></a>Azure AD raporlama verilerini ne kadar süreyle depolar?


Bu makalede, Azure Etkin Dizini'ndeki farklı etkinlik raporlarının veri saklama ilkeleri hakkında bilgi edinebilirsiniz. 

### <a name="when-does-azure-ad-start-collecting-data"></a>Azure AD ne zaman veri toplamaya başlar?

| Azure AD Sürümü | Toplama Başlat |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Bir abonelik için kaydolduğunuzda |
| Azure AD Ücretsiz| [Azure Active Directory bıçağını](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) ilk kez açtığınızda veya [raporlama API'lerini](https://aka.ms/aadreports) kullandığınızda  |

---

### <a name="when-is-the-activity-data-available-in-the-azure-portal"></a>Etkinlik verileri Azure portalında ne zaman kullanılabilir?

- **Hemen** - Azure portalındaki raporlarla zaten çalışıyorsanız.
- **2 saat içinde** - Azure portalında raporlamayı açamadıysanız.

---

### <a name="how-soon-can-i-see-activities-data-after-getting-a-premium-license"></a>Premium lisans aldıktan sonra etkinlik verilerini ne kadar sürede görebiliyorum?

Zaten ücretsiz lisans ile etkinlik verileri varsa, o zaman yükseltme hemen görebilirsiniz. Herhangi bir veriniz yoksa, premium lisansa yükselttikten sonra verilerin raporlarda gösterilmesi bir veya iki gün sürer.

---

### <a name="can-i-see-last-months-data-after-getting-an-azure-ad-premium-license"></a>Azure AD premium lisansı aldıktan sonra geçen ayın verilerini görebilir miyim?

Yakın zamanda premium bir sürüme (deneme sürümü de dahil) geçtiyseniz, başlangıçta 7 güne kadar veriyi görebilirsiniz. Veriler biriktiğinde, son 30 güne ait verileri görebilirsiniz.

---

### <a name="when-does-azure-ad-start-collecting-security-signal-data"></a>Azure AD güvenlik sinyali verilerini toplamaya ne zaman başlar?  

Güvenlik sinyalleri için, **kimlik koruma merkezini**kullanmayı tercih ettiğinizde toplama işlemi başlar. 

---

### <a name="how-long-does-azure-ad-store-the-data"></a>Azure AD verileri ne kadar süreyle saklar?

**Etkinlik raporları**    

| Rapor                 | Azure AD Ücretsiz | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| Denetim günlükleri             | 7 gün        | 30 gün             | 30 gün             |
| Oturum açma işlemleri               | 7 gün        | 30 gün             | 30 gün             |
| Azure MFA kullanımı        | 30 gün       | 30 gün             | 30 gün             |

Denetim ve oturum açma etkinliği verilerini, Azure Monitor'u kullanarak bir Azure depolama hesabına yönlendirme yaparak yukarıda özetlenen varsayılan bekletme döneminden daha uzun süre saklayabilirsiniz. Daha fazla bilgi için [bkz.](quickstart-azure-monitor-route-logs-to-storage-account.md)

**Güvenlik sinyalleri**

| Rapor         | Azure AD Ücretsiz | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--                 | :--                 |
| Risk altındaki kullanıcılar  | 7 gün        | 30 gün             | 90 gün             |
| Riskli oturum açma işlemleri | 7 gün        | 30 gün             | 90 gün             |

---
