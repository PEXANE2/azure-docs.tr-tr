---
title: Azure AD deposu verileri ne kadar süreyle bildiriyor? | Microsoft Belgeleri
description: Azure 'un çeşitli raporlama verisi türlerini ne kadar depoladığını öğrenin.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80239554"
---
# <a name="how-long-does-azure-ad-store-reporting-data"></a>Azure AD deposu verileri ne kadar süreyle bildiriyor?


Bu makalede, Azure Active Directory farklı etkinlik raporları için veri bekletme ilkeleri hakkında bilgi edineceksiniz. 

### <a name="when-does-azure-ad-start-collecting-data"></a>Azure AD ne zaman veri toplamaya başlar?

| Azure AD sürümü | Koleksiyon başlangıcı |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Abonelik için kaydolduğunuzda |
| Azure AD Ücretsiz| [Azure Active Directory dikey penceresini](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) ilk açışınızda veya [Raporlama API 'lerini](https://aka.ms/aadreports) kullandığınızda  |

---

### <a name="when-is-the-activity-data-available-in-the-azure-portal"></a>Azure portal etkinlik verileri ne zaman kullanılabilir?

- **Hemen** -Azure Portal raporlarla zaten çalışıyorsanız.
- **2 saat içinde** -Azure Portal raporlamayı açmadıysanız.

---

### <a name="how-soon-can-i-see-activities-data-after-getting-a-premium-license"></a>Premium lisans aldıktan sonra etkinlik verilerini ne kadar yakında görebilirim?

Ücretsiz lisansınız olan etkinlik verileriniz zaten varsa, yükseltme sırasında hemen görebilirsiniz. Hiç veriniz yoksa, Premium lisansa yükselttikten sonra verilerin raporlarda gösterilmesi bir veya iki gün sürer.

---

### <a name="can-i-see-last-months-data-after-getting-an-azure-ad-premium-license"></a>Bir Azure AD Premium lisansı aldıktan sonra geçen aya ait verileri görebilir miyim?

Yakın zamanda bir premium sürüme (deneme sürümü dahil) geçiş yaptıysanız, başlangıçta verileri en fazla 7 güne kadar görebilirsiniz. Veriler birikme sırasında son 30 güne ait verileri görebilirsiniz.

---

### <a name="when-does-azure-ad-start-collecting-security-signal-data"></a>Azure AD, güvenlik sinyali verilerini toplamaya ne zaman başlar?  

Güvenlik sinyalleri için, **kimlik koruma merkezini**kullanmayı tercih ettiğinizde koleksiyon işlemi başlar. 

---

### <a name="how-long-does-azure-ad-store-the-data"></a>Azure AD verileri ne kadar süreyle depolar?

**Etkinlik raporları**    

| Rapor                 | Azure AD Ücretsiz | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| Denetim günlükleri             | 7 gün        | 30 gün             | 30 gün             |
| Oturum açma işlemleri               | 7 gün        | 30 gün             | 30 gün             |
| Azure MFA kullanımı        | 30 gün       | 30 gün             | 30 gün             |

Azure Izleyici 'yi kullanarak bir Azure depolama hesabına yönlendirerek, yukarıda özetlenen varsayılan saklama süresinden daha uzun bir süre için denetim ve oturum açma etkinlik verilerini koruyabilirsiniz. Daha fazla bilgi için bkz. Azure [ad günlüklerini bir Azure depolama hesabına arşivleme](quickstart-azure-monitor-route-logs-to-storage-account.md).

**Güvenlik sinyalleri**

| Rapor         | Azure AD Ücretsiz | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--                 | :--                 |
| Risk altındaki kullanıcılar  | 7 gün        | 30 gün             | 90 gün             |
| Riskli oturum açma işlemleri | 7 gün        | 30 gün             | 90 gün             |

---
