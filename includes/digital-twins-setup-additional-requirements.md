---
author: baanders
description: Azure dijital TWINS kurulumu 'nda olası ek gereksinimler için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 7/22/2020
ms.author: baanders
ms.openlocfilehash: 230304677b78f00b2d1288c846f8bf704cd8a497
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87407575"
---
Kuruluşunuzun, bir uygulama kaydını başarıyla ayarlaması için (ve böylece kullanılabilir bir Azure dijital TWINS örneği ayarlamayı tamamlaması için) abonelik sahiplerinden ek eylemler gerektirmesi olasıdır. Gerekli adımlar kuruluşunuzun belirli ayarlarına bağlı olarak değişiklik gösterebilir.

Bir sahibin gerçekleştirmesi gerekebilecek bazı yaygın etkinlikler aşağıda verilmiştir. Bu ve diğer işlemler Azure portal [*Azure AD uygulaması kayıtları*](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) sayfasından gerçekleştirilebilir.
* Uygulama kaydı için yönetici onayı verin. Kuruluşunuzun Azure AD 'de aboneliğinizdeki tüm uygulama kayıtları için *yönetici onayı gerekir* . Bu durumda, uygulamanın geçerli olması için uygulama kaydının *API izinleri* sayfasında şirketiniz için bu düğmeyi seçmeniz gerekir:

    :::image type="content" source="../articles/digital-twins/media/how-to-set-up-instance/portal/grant-admin-consent.png" alt-text="API izinleri altında ' yönetici onayı verme ' düğmesinin Portal görünümü":::
  - Onay başarıyla verildiyse, Azure Digital TWINS girişi için giriş için verilen bir *durum* değeri gösterilir _ **(şirketiniz)** _
   
    :::image type="content" source="../articles/digital-twins/media/how-to-set-up-instance/portal/granted-admin-consent-done.png" alt-text="Şirket için API izinleri kapsamında verilen yönetici izninin Portal görünümü":::
* Ortak istemci erişimini etkinleştir
* Web ve masaüstü erişimi için belirli yanıt URL 'Lerini ayarlayın
* Örtük OAuth2 kimlik doğrulama akışları için izin ver

Uygulama kaydı ve farklı kurulum seçenekleri hakkında daha fazla bilgi için bkz. [*Microsoft Identity platformu ile uygulama kaydetme*](https://docs.microsoft.com/graph/auth-register-app-v2).

Artık kullanıma hazır bir Azure dijital TWINS örneğine sahipsiniz, bunu yönetmek için izinler atandı ve bir istemci uygulamasının bu uygulamaya erişmesi için izinleri ayarla.