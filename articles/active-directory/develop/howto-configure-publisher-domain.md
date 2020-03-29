---
title: Bir uygulamanın yayımcı etki alanını yapılandırma | Azure
titleSuffix: Microsoft identity platform
description: Kullanıcılara bilgilerinin nereye gönderildiğini bildirmek için bir uygulamanın yayımcı etki alanını nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/05/2019
ms.author: ryanwi
ms.reviewer: lenalepa, sureshja, zachowd
ms.custom: aaddev
ms.openlocfilehash: 68040c8ee22454c300296493b6c840eabbca98aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76697141"
---
# <a name="how-to-configure-an-applications-publisher-domain"></a>Nasıl yapilir: Bir uygulamanın yayımcı etki alanını yapılandırma

Bir uygulamanın yayımcı etki alanı, kullanıcılara bilgilerinin nereye gönderildiğini bildirmek için [uygulamanın onay isteminde](application-consent-experience.md) kullanıcılara görüntülenir. Yayımcı etki alanı olmayan 21 Mayıs 2019'dan sonra kaydedilmiş çok kiracılı uygulamalar doğrulanmamış olarak **gösterilmektedir.** Çok kiracılı uygulamalar, tek bir kuruluş dizininin dışındaki hesapları destekleyen uygulamalardır; örneğin, tüm Azure AD hesaplarını destekleyin veya tüm Azure AD hesaplarını ve kişisel Microsoft hesaplarını destekleyin.

## <a name="new-applications"></a>Yeni uygulamalar

Yeni bir uygulama kaydettiğinizde, uygulamanızın yayıncı alan adı varsayılan bir değerolarak ayarlanabilir. Değer, uygulamanın nerede kaydedildiğine, özellikle de uygulamanın kiracıya kaydedilip kaydedilmediğine ve kiracının doğrulanmış etki alanları olup olmadığına bağlıdır.

Kiracı tarafından doğrulanmış etki alanları varsa, uygulamanın yayımcı etki alanı varsayılan olarak kiracının birincil doğrulanmış etki alanı için. Kiracı doğrulanmış etki alanı yoksa (ki uygulama kiracıya kaydedilmediyse durum böyledir), uygulamanın yayımcı etki alanı geçersiz kılınacak şekilde ayarlanır.

Aşağıdaki tabloda yayımcı etki alanı değerinin varsayılan davranışı özetlenir.  

| Kiracı tarafından doğrulanmış etki alanları | Yayımcı etki alanının varsayılan değeri |
|-------------------------|----------------------------|
| null | null |
| *.onmicrosoft.com | *.onmicrosoft.com |
| - *.onmicrosoft.com<br/>- domain1.com<br/>- domain2.com (birincil) | domain2.com |

Çok kiracılı bir uygulamanın yayımcı etki alanı ayarlanmamışsa veya .onmicrosoft.com ile biten bir etki alanına ayarlanmışsa, uygulamanın onay istemi yayımcı etki alanının yerine **doğrulanmamış** olarak gösterilecektir.

## <a name="grandfathered-applications"></a>Dedesi uygulamaları

Uygulamanız 21 Mayıs 2019'dan önce kaydedilmişse, bir yayıncı etki alanı ayarlamadıysanız uygulamanızın onay istemi **doğrulanmamış** olarak gösterilmez. Kullanıcıların bu bilgileri uygulamanızın onay isteminde görebilmeleri için yayıncı etki alanı değerini ayarlamanızı öneririz.

## <a name="configure-publisher-domain-using-the-azure-portal"></a>Azure portalını kullanarak yayıncı etki alanını yapılandırma

Uygulamanızın yayımcı etki alanını ayarlamak için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.

1. Hesabınız birden fazla Azure AD kiracısında mevcutsa:
   1. Sayfanın sağ üst köşesindeki menüden profilinizi seçin ve **ardından dizini değiştirin.**
   1. Oturumunuzu, uygulamanızı oluşturmak istediğiniz Azure AD kiracısına değiştirin.

1. Yapılandırmak istediğiniz uygulamayı bulmak ve seçmek için [Azure Active Directory > App kayıtlarına](https://go.microsoft.com/fwlink/?linkid=2083908) gidin.

   Uygulamayı seçtikten sonra uygulamanın **Genel Bakış** sayfasını görürsünüz.

1. Uygulamanın Genel **Bakış** sayfasından **Markalandırma** bölümünü seçin.

1. Publisher **etki alanı** alanını bulun ve aşağıdaki seçeneklerden birini seçin:

   - Etki alanını daha önce yapılandırmadıysanız **etki alanını yapılandır'ı** seçin.
   - Etki alanı zaten yapılandırılmışsa **etki alanını güncelleştir'i** seçin.

Uygulamanız kiracıda kayıtlıysa, seçilecek iki sekme görürsünüz: **Doğrulanmış bir etki alanı seçin** ve yeni bir **etki alanını doğrulayın.**

Uygulamanız kiracıda kayıtlı değilse, yalnızca uygulamanız için yeni bir etki alanını doğrulama seçeneğini görürsünüz.

### <a name="to-verify-a-new-domain-for-your-app"></a>Uygulamanız için yeni bir etki alanını doğrulamak için

1. Adlandırılmış `microsoft-identity-association.json` bir dosya oluşturun ve aşağıdaki JSON kod parçacığıyapıştırın.

   ```json
   {
      "associatedApplications": [
        {
           "applicationId": "{YOUR-APP-ID-HERE}"
        }
      ]
    }
   ```

1. Yer tutucu *{YOUR-APP-ID-HERE}* ile uygulamanıza karşılık gelen uygulama (istemci) kimliği değiştirin.

1. Dosyayı şu `https://{YOUR-DOMAIN-HERE}.com/.well-known/microsoft-identity-association.json`anda barındırın: . Doğrulanmış etki alanıyla eşleşecek şekilde yer tutucu *{YOUR-DOMAIN-HERE}* değiştirin.

1. Etki **alanını doğrula ve kaydet** düğmesini tıklatın.

### <a name="to-select-a-verified-domain"></a>Doğrulanmış bir etki alanı seçmek için

- Kiracınız etki alanlarını doğruladıysa, **doğrulanmış etki alanı** açılır düşüşünü seç'ten etki alanlarından birini seçin.

>[!Note]
> Döndürülmesi beklenen 'İçerik Türü' üstbilgisi. `application/json` Eğer başka bir şey kullanırsanız aşağıda belirtildiği gibi bir hata alabilirsiniz`application/json; charset=utf-8` 
> 
>``` "Verification of publisher domain failed. Error getting JSON file from https:///.well-known/microsoft-identity-association. The server returned an unexpected content type header value. " ```
>

## <a name="implications-on-the-app-consent-prompt"></a>Uygulama onayı istemi üzerindeki etkileri

Yayıncı etki alanının yapılandırılması, kullanıcıların uygulama onay isteminde gördükleri üzerinde bir etkiye sahiptir. Onay isteminin bileşenlerini tam olarak anlamak için [bkz.](application-consent-experience.md)

Aşağıdaki tabloda 21 Mayıs 2019'dan önce oluşturulan uygulamalar için davranış açıklanmaktadır.

![21 Mayıs 2019'dan önce oluşturulan uygulamalar için onay istemi](./media/howto-configure-publisher-domain/old-app-behavior-table.png)

21 Mayıs 2019'dan sonra oluşturulan yeni uygulamalar için davranış yayımcı etki alanına ve uygulama türüne bağlıdır. Aşağıdaki tabloda, farklı yapılandırma kombinasyonlarıyla görmeyi beklemeniz gereken değişiklikler açıklanmaktadır.

![21 Mayıs 2019'dan sonra oluşturulan uygulamalar için onay istemi](./media/howto-configure-publisher-domain/new-app-behavior-table.png)

## <a name="implications-on-redirect-uris"></a>Yönlendirme URI'leri üzerindeki etkileri

Herhangi bir iş veya okul hesabı veya kişisel Microsoft hesapları[(çok kiracı)](single-and-multi-tenant-apps.md)ile kullanıcılar oturum açan uygulamalar, yeniden yönlendirme IU'ları belirtirken birkaç kısıtlamaya tabidir.

### <a name="single-root-domain-restriction"></a>Tek kök etki alanı kısıtlaması

Çok kiracılı uygulamaların yayıncı etki alanı değeri geçersiz kılındığında, uygulamaların yeniden yönlendirme IU'ları için tek bir kök etki alanını paylaşması kısıtlanır. Örneğin, kök etki alanı contoso.com fabrikam.com eşleşmediği için aşağıdaki değerler birleşimine izin verilmez.

```
"https://contoso.com",
"https://fabrikam.com",
```

### <a name="subdomain-restrictions"></a>Alt etki alanı kısıtlamaları

Alt etki adlarına izin verilir, ancak kök etki alanını açıkça kaydetmeniz gerekir. Örneğin, aşağıdaki URI'ler tek bir kök etki alanını paylaşırken, birleşime izin verilmez.

```
"https://app1.contoso.com",
"https://app2.contoso.com",
```

Ancak, geliştirici açıkça kök etki alanı eklerse, birleşim izin verilir.

```
"https://contoso.com",
"https://app1.contoso.com",
"https://app2.contoso.com",
```

### <a name="exceptions"></a>Özel durumlar

Aşağıdaki durumlar tek kök etki alanı kısıtlamasına tabi değildir:

- Tek kiracı uygulamaları veya tek bir dizindeki hesapları hedefleyen uygulamalar
- Localhost'un yeniden yönlendirme URI'leri olarak kullanılması
- Özel şemalar (HTTP veya HTTPS olmayan) ile Url'leri yönlendirme

## <a name="configure-publisher-domain-programmatically"></a>Yayımcı etki alanını programlı olarak yapılandırma

Şu anda, yayıncı etki alanını programlı olarak yapılandırmak için REST API veya PowerShell desteği yoktur.
