---
title: Desteklenen hesap türlerine göre doğrulama farklılıkları-Microsoft Identity platform | Mavisi
description: Uygulamanızı Microsoft Identity platformu ile kaydederken desteklenen farklı hesap türleri için çeşitli özelliklerin doğrulama farklılıkları hakkında bilgi edinin.
author: SureshJa
ms.author: sureshja
manager: CelesteDG
ms.date: 10/12/2019
ms.topic: conceptual
ms.subservice: develop
ms.custom: aaddev
ms.service: active-directory
ms.reviewer: lenalepa, manrath
ms.openlocfilehash: 2a1507b008903085886f9392f3f4e5461997b6e2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80128868"
---
# <a name="validation-differences-by-supported-account-types-signinaudience"></a>Desteklenen hesap türlerine (Signınaudience) göre doğrulama farklılıkları

Bir uygulamayı geliştiriciler için Microsoft Identity platformu ile kaydederken, uygulamanızın desteklediği hesap türlerini seçmeniz istenir. Uygulama nesnesi ve bildiriminde, bu özellik olur `signInAudience` .

Seçenekler şunları içerir:

- *Azureadmyorg*: yalnızca uygulamanın kaydedildiği kuruluş dizinindeki hesaplar (tek kiracılı)
- *Azureadmultipleorgs*: herhangi bir kuruluş dizinindeki (çok kiracılı) hesaplar
- *Azureadandpersonmicrosoftaccount*: herhangi bir kurumsal dizin (çok kiracılı) ve kişisel Microsoft hesabı (örneğin, Skype, Xbox ve Outlook.com) hesapları

Kayıtlı uygulamalar için, desteklenen hesap türleri için değeri bir uygulamanın **kimlik doğrulama** bölümünde bulabilirsiniz. `signInAudience` **Bildirimin**özelliği altında de bulabilirsiniz.

Bu özellik için seçtiğiniz değer diğer uygulama nesnesi özelliklerinde etkileri vardır. Sonuç olarak, bu özelliği değiştirirseniz, önce diğer özellikleri değiştirmeniz gerekebilir.

Desteklenen farklı hesap türleri için çeşitli özelliklerin doğrulama farklılıkları için aşağıdaki tabloya bakın.

| Özellik | `AzureADMyOrg` | `AzureADMultipleOrgs` | `AzureADandPersonalMicrosoftAccount` ve `PersonalMicrosoftAccount` |
|--------------|---------------|----------------|----------------|
| Uygulama KIMLIĞI URI 'SI ( `identifierURIs` )  | Kiracıda benzersiz olmalıdır <br><br> urn://şemaları destekleniyor <br><br> Joker karakterler desteklenmiyor <br><br> Sorgu dizeleri ve parçalar destekleniyor <br><br> En fazla 255 karakter uzunluğunda <br><br> Limit * sayısı, ıdentifieruri sayısıyla  | Genel olarak benzersiz olması gerekir <br><br> urn://şemaları destekleniyor <br><br> Joker karakterler desteklenmiyor <br><br> Sorgu dizeleri ve parçalar destekleniyor <br><br> En fazla 255 karakter uzunluğunda <br><br> Limit * sayısı, ıdentifieruri sayısıyla | Genel olarak benzersiz olması gerekir <br><br> urn://şemaları desteklenmez <br><br> Joker karakterler, parçalar ve sorgu dizeleri desteklenmez <br><br> En fazla 120 karakter uzunluğunda <br><br> Maksimum 50 ıdentifieruri |
| Sertifikalar ( `keyCredentials` ) | Simetrik imzalama anahtarı | Simetrik imzalama anahtarı | Şifreleme ve asimetrik imzalama anahtarı | 
| İstemci gizli dizileri ( `passwordCredentials` ) | Sınır yok * | Sınır yok * | LiveSDK etkinse: en fazla 2 istemci parolası | 
| Yeniden yönlendirme URI 'Leri ( `replyURLs` ) | Daha fazla bilgi için bkz. [yeniden YÖNLENDIRME URI/yanıt URL kısıtlamaları ve sınırlamaları](reply-url.md) . | | | 
| API izinleri ( `requiredResourceAccess` ) | Sınır yok * | Sınır yok * | Kaynak başına izin verilen en fazla 30 izin (örn. Microsoft Graph) | 
| Bu API () tarafından tanımlanan kapsamlar `oauth2Permissions` | 120 karakterlik en büyük kapsam adı uzunluğu <br><br> Tanımlı kapsam sayısında sınır yok * | 120 karakterlik en büyük kapsam adı uzunluğu <br><br> Tanımlı kapsam sayısında sınır yok * |  40 karakterlik en büyük kapsam adı uzunluğu <br><br> Tanımlanan en fazla 100 kapsam | 
| Yetkilendirilmiş istemci uygulamaları ( `preautorizedApplications` ) | Sınır yok * | Sınır yok * | Toplam 500 üst sınırı <br><br> En fazla 100 istemci uygulaması tanımlandı <br><br> İstemci başına tanımlanan en fazla 30 kapsam | 
| appRoles | Destekleniyor <br> Sınır yok * | Destekleniyor <br> Sınır yok * | Desteklenmiyor | 
| Oturum kapatma URL 'SI | http://localhostizin verilir <br><br> En fazla 255 karakter uzunluğunda | http://localhostizin verilir <br><br> En fazla 255 karakter uzunluğunda | <br><br> https://localhostizin veriliyor, http://localhost MSA için başarısız oluyor <br><br> En fazla 255 karakter uzunluğunda <br><br> HTTP şemasına izin verilmiyor <br><br> Joker karakterler desteklenmiyor | 

* Uygulama nesnesindeki tüm koleksiyon özellikleri üzerinde 1000 öğe hakkında genel bir limit vardır

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama kaydı](app-objects-and-service-principals.md) hakkında bilgi edinin
- [Uygulama bildirimi](reference-app-manifest.md) hakkında bilgi edinin
