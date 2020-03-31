---
title: Desteklenen hesap türlerine göre doğrulama farklılıkları - Microsoft kimlik platformu | Azure
description: Uygulamanızı Microsoft kimlik platformuna kaydederken, desteklenen farklı hesap türleri için çeşitli özelliklerin doğrulama farklılıkları hakkında bilgi edinin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80128868"
---
# <a name="validation-differences-by-supported-account-types-signinaudience"></a>Desteklenen hesap türlerine göre doğrulama farklılıkları (signInAudience)

Geliştiriciler için Microsoft kimlik platformuna bir uygulama kaydederken, uygulamanızın hangi hesap türlerini desteklediğini seçmeniz istenir. Uygulama nesnesi ve bildiriminde, bu özellik `signInAudience`.

Seçenekler şunlardır:

- *AzureADMyOrg*: Yalnızca uygulamanın kayıtlı olduğu kuruluş dizinindeki hesaplar (tek kiracı)
- *AzureADMultipleOrgs*: Herhangi bir kuruluş dizinindeki hesaplar (çok kiracılı)
- *AzureADandPersonalMicrosoftAccount*: Herhangi bir kuruluş dizinindeki (çok kiracılı) ve kişisel Microsoft hesaplarındaki hesaplar (örneğin, Skype, Xbox ve Outlook.com)

Kayıtlı uygulamalar için, desteklenen hesap türlerinin değerini bir uygulamanın **Kimlik Doğrulama** bölümünde bulabilirsiniz. Ayrıca `signInAudience` **Manifest**özelliği altında bulabilirsiniz.

Bu özellik için seçtiğiniz değerin diğer uygulama nesnesi özellikleri üzerinde etkileri vardır. Sonuç olarak, bu özelliği değiştirirseniz, önce diğer özellikleri değiştirmeniz gerekebilir.

Desteklenen farklı hesap türleri için çeşitli özelliklerin doğrulama farkları için aşağıdaki tabloya bakın.

| Özellik | `AzureADMyOrg` | `AzureADMultipleOrgs` | `AzureADandPersonalMicrosoftAccount` ve `PersonalMicrosoftAccount` |
|--------------|---------------|----------------|----------------|
| Uygulama Kimliği`identifierURIs`URI ( )  | Kiracı benzersiz olmalı <br><br> urn:// şemaları desteklenir <br><br> Joker karakterler desteklenmiyor <br><br> Sorgu dizeleri ve parçaları desteklenir <br><br> Maksimum 255 karakter uzunluğu <br><br> Tanımlayıcı sayısında sınır yoktur*  | Genel olarak benzersiz olması gerekir <br><br> urn:// şemaları desteklenir <br><br> Joker karakterler desteklenmiyor <br><br> Sorgu dizeleri ve parçaları desteklenir <br><br> Maksimum 255 karakter uzunluğu <br><br> Tanımlayıcı sayısında sınır yoktur* | Genel olarak benzersiz olması gerekir <br><br> urn:// düzenleri desteklenmiyor <br><br> Joker karakterler, parçalar ve sorgu dizeleri desteklenmez <br><br> Maksimum 120 karakter uzunluğunda <br><br> Maksimum 50 identifierURIs |
| Sertifikalar`keyCredentials`( ) | Simetrik imzalama anahtarı | Simetrik imzalama anahtarı | Şifreleme ve asimetrik imzalama anahtarı | 
| Müşteri sırları`passwordCredentials`( ) | Sınır yok* | Sınır yok* | liveSDK etkinse: En fazla 2 istemci sırrı | 
| Rİ'leri`replyURLs`Yönlendirme ( ) | Daha fazla bilgi için [URI/reply URL kısıtlamalarını ve sınırlamalarını yeniden yönlendirme](reply-url.md) ye bakın. | | | 
| API izinleri`requiredResourceAccess`( ) | Sınır yok* | Sınır yok* | İzin verilen kaynak başına en fazla 30 izin (örneğin Microsoft Graph) | 
| Bu API tarafından tanımlanan`oauth2Permissions`kapsamlar ( ) | 120 karakterlik maksimum kapsam adı uzunluğu <br><br> Tanımlanan kapsam sayısında sınır yok* | 120 karakterlik maksimum kapsam adı uzunluğu <br><br> Tanımlanan kapsam sayısında sınır yok* |  40 karakterlik maksimum kapsam adı uzunluğu <br><br> Tanımlanan en fazla 100 kapsam | 
| Yetkili müşteri başvuruları`preautorizedApplications`( ) | Sınır yok* | Sınır yok* | Toplam maksimum 500 <br><br> Tanımlanan en fazla 100 istemci uygulaması <br><br> İstemci başına tanımlanan en fazla 30 kapsam | 
| appRoles | Destekleniyor <br> Sınır yok* | Destekleniyor <br> Sınır yok* | Desteklenmiyor | 
| Giriş URL'si | http://localhostizin verilir <br><br> Maksimum 255 karakter uzunluğu | http://localhostizin verilir <br><br> Maksimum 255 karakter uzunluğu | <br><br> https://localhostizin verilir, http://localhost MSA için başarısız <br><br> Maksimum 255 karakter uzunluğu <br><br> HTTP düzenine izin verilmez <br><br> Joker karakterler desteklenmiyor | 

*Uygulama nesnesindeki tüm koleksiyon özelliklerinde yaklaşık 1000 öğelik küresel bir sınır vardır

## <a name="next-steps"></a>Sonraki adımlar

- Uygulama [kaydı](app-objects-and-service-principals.md) hakkında bilgi edinin
- [Uygulama bildirimi](reference-app-manifest.md) hakkında bilgi edinin
