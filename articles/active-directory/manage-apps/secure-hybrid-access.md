---
title: Azure AD güvenli karma erişim | Microsoft Docs
description: Bu makalede, Azure AD ile eski şirket içi, genel bulutunuzun veya özel bulut uygulamalarınızın tümleştirilmesine yönelik iş ortağı çözümleri açıklanmaktadır. Uygulama teslim denetleyicilerini veya ağlarını Azure AD 'ye bağlayarak eski uygulamalarınızın güvenliğini sağlayın.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: overview
ms.workload: identity
ms.date: 12/18/2019
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0e97b95e290ef74ffd98a3396ffe4705270132b2
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75433757"
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
