---
title: Azure Etkin Dizin'de kurumsal durum dolaşımı nedir?
description: Kurumsal Durum Dolaşımı, kullanıcılara Windows aygıtlarında birleşik bir deneyim sağlar
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: overview
ms.date: 02/12/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: na
ms.collection: M365-identity-device-management
ms.openlocfilehash: c22baf0a08718883f0c0c9844cc395f607b5b20d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77194288"
---
# <a name="what-is-enterprise-state-roaming"></a>Enterprise State Roaming nedir?

Windows 10 ile [Azure Active Directory (Azure AD)](../fundamentals/active-directory-whatis.md) kullanıcıları, kullanıcı ayarlarını ve uygulama ayarları verilerini bulutla güvenli bir şekilde eşitleme olanağı elde eder. Kurumsal Durum Dolaşımı, kullanıcılara Windows aygıtlarında birleşik bir deneyim sağlar ve yeni bir aygıtı yapılandırmak için gereken süreyi azaltır. Kurumsal Durum Dolaşımı, ilk olarak Windows 8'de tanıtılan standart [tüketici ayarları eşitlemesine](https://go.microsoft.com/fwlink/?linkid=2015135) benzer şekilde çalışır. Ayrıca, Enterprise State Roaming şunları sunar:

* **Kurumsal ve tüketici verilerinin ayrılması** – Kuruluşlar verilerini kontrol altındadır ve kurumsal bulut hesabındaki kurumsal verilerin veya tüketici verilerinin kurumsal bulut hesabında karıştırılması yoktur.
* **Gelişmiş güvenlik** – Veriler, Azure Hakları Yönetimi (Azure RMS) kullanılarak kullanıcının Windows 10 aygıtından çıkmadan önce otomatik olarak şifrelenir ve veriler bulutta sabit kalarak şifrelenir. Ayarlar adları ve Windows uygulama adları gibi ad alanları dışında tüm içerik bulutta sabit teşifre edilir.  
* **Daha iyi yönetim ve izleme** – Azure AD portalı tümleştirmesi aracılığıyla kuruluşunuzdaki ayarları kimin ve hangi cihazlarda eşitlediği konusunda denetim ve görünürlük sağlar. 

Kurumsal Durum Dolaşımı birden çok Azure bölgesinde kullanılabilir. Azure Etkin Dizini altında Azure [Hizmetlerine göre Bölgeler](https://azure.microsoft.com/regions/#services) sayfasında kullanılabilir bölgelerin güncelleştirilmiş listesini bulabilirsiniz.

| Makale | Açıklama |
| --- | --- |
| [Azure Active Directory'de Kurumsal Durumda Dolaşımı etkinleştirme](enterprise-state-roaming-enable.md) |Kurumsal Durum Dolaşımı, Premium Azure Etkin Dizin (Azure AD) aboneliği olan tüm kuruluşların kullanımına açıktır. Azure AD aboneliğini nasıl alacağınız hakkında daha fazla bilgi için [Azure AD ürün](https://azure.microsoft.com/services/active-directory) sayfasına bakın. |
| [Ayarlar ve veri dolaşımı hakkında SSS](enterprise-state-roaming-faqs.md) |Bu makalede, BT yöneticilerinin ayarlar ve uygulama verileri eşitleme yle ilgili bazı soruları yanıtlar. |
| [Ayarlar eşitleme için grup ilkesi ve MDM ayarları](enterprise-state-roaming-group-policy-settings.md) |Windows 10, ayarları eşitlemek için Grup İlkesi ve mobil aygıt yönetimi (MDM) ilkesi ayarlarını sağlar. |
| [Windows 10 dolaşım ayarları başvurusu](enterprise-state-roaming-windows-settings-reference.md) |Windows 10'da gezinilecek ve/veya yedeklenecek ayarların listesi. |
| [Sorun giderme](enterprise-state-roaming-troubleshooting.md) |Bu makalede, sorun giderme için bazı temel adımlar geçer ve bilinen sorunların listesini içerir. |

## <a name="next-steps"></a>Sonraki adımlar

Kurumsal durum dolaşımını etkinleştirme hakkında bilgi için [bkz.](enterprise-state-roaming-enable.md)
