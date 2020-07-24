---
author: baanders
description: Azure dijital TWINS kurulumu 'nda olası ek gereksinimler için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 7/22/2020
ms.author: baanders
ms.openlocfilehash: b490b4304dd74d8266b24d0ea5af58726b14d747
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87125826"
---
Kuruluşunuzun, bir uygulama kaydını başarıyla ayarlaması için (ve sonuç olarak, kullanılabilir bir Azure dijital TWINS örneği ayarlamayı tamamlaması) abonelik sahiplerinden ek eylemler gerektirmesi olasıdır. Gerekli adımlar kuruluşunuzun belirli ayarlarına bağlı olarak değişiklik gösterebilir.

Bir sahibin gerçekleştirmesi gerekebilecek bazı yaygın etkinlikler aşağıda verilmiştir. Bu ve diğer işlemler Azure portal [*Azure AD uygulaması kayıtları*](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) sayfasından gerçekleştirilebilir.
* Uygulama kaydı için yönetici onayı verin. Kuruluşunuzun Azure AD 'de aboneliğinizdeki tüm uygulama kayıtları için *yönetici onayı gerekir* . Bu durumda, uygulamanın geçerli olması için uygulama kaydının *API izinleri* sayfasında şirketiniz için bu düğmeyi seçmeniz gerekir:

    :::image type="content" source="../articles/digital-twins/media/how-to-set-up-instance/grant-admin-consent.png" alt-text="API izinleri altında ' yönetici onayı verme ' düğmesinin Portal görünümü":::
  - Onay başarıyla verildiyse, Azure Digital TWINS girişi için bir *durum* değeri belirtilmelidir _ **(şirketiniz)** _
   
    :::image type="content" source="../articles/digital-twins/media/how-to-set-up-instance/granted-admin-consent.png" alt-text="Şirket için API izinleri kapsamında verilen yönetici izninin Portal görünümü":::
  - Sahip, bu işlemi oluşturulan her uygulama kaydı için yinelemeyi veya bir kez yapmayı tercih edebilir ve abonelikteki tüm Azure dijital TWINS örnekleri için tek bir paylaşılan uygulama kaydı oluşturabilir. İkinci senaryoda, sahip, uygulama kaydını kullanması gereken geliştiriciler ile uygulama kaydı için *ISTEMCI kimliğini* ve *Kiracı kimliğini* paylaşmalıdır. (Microsoft 'un kendi kiracısında bu şekilde yapılır).
* Ortak istemci erişimini etkinleştir
* Web ve masaüstü erişimi için belirli yanıt URL 'Lerini ayarlayın
* Örtük OAuth2 kimlik doğrulama akışları için izin ver

Uygulama kaydı ve farklı seçenekleri hakkında daha fazla bilgi için bkz. [*Microsoft Identity platformu ile uygulama kaydetme*](https://docs.microsoft.com/graph/auth-register-app-v2).

Artık kullanıma hazır bir Azure dijital TWINS örneğine sahipsiniz, bunu yönetmek için izinler atandı ve bir istemci uygulamasının bu uygulamaya erişmesi için izinleri ayarla.