---
title: Azure Active Directory Kurumsal durum dolaşımı nedir?
description: Enterprise State Roaming, kullanıcılara Windows cihazlarında birleştirilmiş bir deneyim sağlar
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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "77194288"
---
# <a name="what-is-enterprise-state-roaming"></a>Enterprise State Roaming nedir?

Windows 10 ' da, [Azure Active Directory (Azure AD)](../fundamentals/active-directory-whatis.md) kullanıcıları Kullanıcı ayarlarını ve uygulama ayarları verilerini buluta güvenli bir şekilde eşitlemenize olanak tanır. Enterprise State Roaming, kullanıcılara Windows cihazlarında Birleşik bir deneyim sağlar ve yeni bir cihaz yapılandırmak için gereken süreyi azaltır. Enterprise State Roaming, ilk olarak Windows 8 ' de sunulan standart [tüketici ayarları eşitlemesine](https://go.microsoft.com/fwlink/?linkid=2015135) benzer şekilde çalışır. Ayrıca, Enterprise State Roaming teklifler:

* Kurumsal **ve tüketici verilerinin ayrımı** – kuruluşlar kendi verilerinin denetimidir ve bir kurumsal bulut hesabındaki bir tüketici bulutu hesabındaki veya tüketici verilerinde kurumsal verilerin bir karıştırılmasıyla kalmaz.
* **Gelişmiş güvenlik** – veriler, kullanıcının Windows 10 cihazından Azure Rights Management (Azure RMS) kullanılarak ayrılmadan önce otomatik olarak şifrelenir ve veriler buluttaki geri kalanında şifreli olarak kalır. Tüm içerik, ad alanları dışında, ayarlar adı ve Windows uygulama adları gibi buluttaki geri kalanında şifreli olarak kalır.  
* **Daha iyi yönetim ve izleme** – kuruluşunuzda ayarları ve Azure AD Portal tümleştirmesi aracılığıyla hangi cihazları eşitlediği hakkında denetim ve görünürlük sağlar. 

Enterprise State Roaming birden çok Azure bölgesinde kullanılabilir. Kullanılabilir bölgelerin güncelleştirilmiş listesini, Azure Active Directory altındaki [bölgelere göre Azure hizmetleri](https://azure.microsoft.com/regions/#services) sayfasında bulabilirsiniz.

| Makale | Açıklama |
| --- | --- |
| [Azure Active Directory'de Kurumsal Durumda Dolaşımı etkinleştirme](enterprise-state-roaming-enable.md) |Enterprise State Roaming, Premium Azure Active Directory (Azure AD) aboneliği olan her kuruluş için kullanılabilir. Azure AD aboneliği alma hakkında daha fazla bilgi için bkz. [Azure AD ürün](https://azure.microsoft.com/services/active-directory) sayfası. |
| [Ayarlar ve veri dolaşımı hakkında SSS](enterprise-state-roaming-faqs.md) |Bu makalede, yöneticilerin ayarlar ve uygulama verilerinin eşitlenmesi hakkında sahip olabileceği bazı sorular yanıtlanmaktadır. |
| [Ayarlar eşitleme için Grup İlkesi ve MDM ayarları](enterprise-state-roaming-group-policy-settings.md) |Windows 10, ayar eşitlemesini sınırlamak için grup ilkesi ve mobil cihaz yönetimi (MDM) ilke ayarları sağlar. |
| [Windows 10 dolaşım ayarları başvurusu](enterprise-state-roaming-windows-settings-reference.md) |Windows 10 ' da dolaşılabilir ve/veya yedeklenecek ayarların bir listesi. |
| [Sorun giderme](enterprise-state-roaming-troubleshooting.md) |Bu makale, sorun giderme için bazı temel adımlardan geçer ve bilinen sorunların bir listesini içerir. |

## <a name="next-steps"></a>Sonraki adımlar

Kurumsal durum dolaşımını etkinleştirme hakkında daha fazla bilgi için bkz. [kuruluş durumu dolaşımını etkinleştirme](enterprise-state-roaming-enable.md).
