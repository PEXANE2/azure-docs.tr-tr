---
title: Azure AD güvenli karma erişim | Microsoft Docs
description: Bu makalede, Azure AD ile eski şirket içi, genel bulutunuzun veya özel bulut uygulamalarınızın tümleştirilmesine yönelik iş ortağı çözümleri açıklanmaktadır. Uygulama teslim denetleyicilerini veya ağlarını Azure AD 'ye bağlayarak eski uygulamalarınızın güvenliğini sağlayın.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: how-to
ms.workload: identity
ms.date: 12/18/2019
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: 295891afbb0136e0b05bcd49f4045e0e8bcff6e5
ms.sourcegitcommit: bc943dc048d9ab98caf4706b022eb5c6421ec459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2020
ms.locfileid: "84763049"
---
# <a name="secure-hybrid-access-secure-legacy-apps-with-app-delivery-controllers-and-networks"></a>Güvenli karma erişim: uygulama teslim denetleyicileri ve ağlarla güvenli eski uygulamalar sağlayın

Artık şirket içi ve bulut eski kimlik doğrulama uygulamalarınızı, mevcut uygulama teslim denetleyicinize veya ağınıza Azure AD 'ye bağlayarak koruyabilirsiniz. Bu şekilde, Azure AD koşullu erişim ve Azure AD Kimlik Koruması gibi Azure AD özelliklerine sahip tüm uygulamalarda, boşluğu köprülemek ve güvenlik duruşunuzu güçlendirebilirsiniz.

Mevcut ağ ve teslim denetleyicinizi kullanarak, İş süreçleriniz için hala kritik olan eski uygulamaları kolayca koruyabilir ancak Azure AD ile önce koruyamazsınız. Bu uygulamaları korumaya başlamak için ihtiyacınız olan her şeye zaten sahipsiniz.

![Güvenli karma erişimi gösteren resim](media/secure-hybrid-access/secure-hybrid-access.png)

Aşağıdaki satıcılar, Azure AD ile tümleştirme için önceden oluşturulmuş çözümler ve ayrıntılı kılavuz sunar.

* [Akamai kurumsal uygulama erişimi (EAA)](../saas-apps/akamai-tutorial.md)
* [Citrix uygulama teslim denetleyicisi (ADC)](../saas-apps/citrix-netscaler-tutorial.md)
* [F5 Big-IP APM](https://aka.ms/f5-hybridaccessguide)
* [Zscaler Private Access (ZPA)](https://aka.ms/zscaler-hybridaccessguide)
