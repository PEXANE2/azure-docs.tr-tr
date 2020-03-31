---
title: URI & yanıtlama URL kısıtlamaları yönlendirme - Microsoft kimlik platformu | Azure
description: URL'leri yanıtla/yeniden yönlendirme Rİ'ler & sınırlamaları
author: SureshJa
ms.author: sureshja
manager: CelesteDG
ms.date: 06/29/2019
ms.topic: conceptual
ms.subservice: develop
ms.custom: aaddev
ms.service: active-directory
ms.reviewer: lenalepa, manrath
ms.openlocfilehash: 8fdc64632be8b5fcb3dca8de2ee833fef25719fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77656747"
---
# <a name="redirect-urireply-url-restrictions-and-limitations"></a>Yeniden yönlendirme URI’si/yanıt URL’si kısıtlamaları ve sınırlamaları

Yeniden yönlendirme URI veya yanıt URL'si, uygulama başarıyla yetkiverildikten ve bir yetkilendirme kodu veya erişim jetonu verildikten sonra yetkilendirme sunucusunun kullanıcıya göndereceği yerdir. Kod veya belirteç, yeniden yönlendirme URI'sinde veya yanıt belirtecinde bulunur, bu nedenle doğru konumu uygulama kayıt sürecinin bir parçası olarak kaydetmeniz önemlidir.

 Url'leri yanıtlamak için aşağıdaki kısıtlamalar geçerlidir:

    * Yanıt URL'si şema `https`ile başlamalıdır.
    * Yanıt URL'si büyük/küçük harf duyarlıdır. Bu durumda çalışan uygulamanızın URL yolu durumda eşleşmesi gerekir. Örneğin, uygulamanız yolunun `.../abc/response-oidc`bir parçası olarak içeriyorsa, yanıt URL'sinde belirtmeyin. `.../ABC/response-oidc` Web tarayıcısı yolları büyük/küçük harf duyarlı `.../abc/response-oidc` olarak ele aldığı için, büyük/küçük harf eşleşmesi `.../ABC/response-oidc` url'ye yönlendirilirse ilişkili tanımlama bilgileri hariç tutulabilir.
    
## <a name="maximum-number-of-redirect-uris"></a>Maksimum yönlendirme URI sayısı

Aşağıdaki tablo, uygulamanızı kaydederken ekleyebileceğiniz en fazla yeniden yönlendirme ÜR'leri sayısını gösterir.

| Oturum da oturum alabilmekte olan hesaplar | Maksimum yönlendirme URI sayısı | Açıklama |
|--------------------------|---------------------------------|-------------|
| Herhangi bir kuruluşun Azure Etkin Dizini 'ndeki (Azure AD) kiracısında microsoft iş veya okul hesapları | 256 | `signInAudience`uygulama bildirimindeki alan *AzureADMyOrg veya AzureADMultipleOrgs* olarak ayarlanır *AzureADMultipleOrgs* |
| Kişisel Microsoft hesapları ve iş ve okul hesapları | 100 | `signInAudience`uygulama bildirimindeki alan *AzureADandPersonalMicrosoftAccount* olarak ayarlanır |

## <a name="maximum-uri-length"></a>Maksimum URI uzunluğu

Bir uygulama kaydına eklediğiniz her yeniden yönlendirme URI için en fazla 256 karakter kullanabilirsiniz.

## <a name="supported-schemes"></a>Desteklenen şemalar
Azure AD uygulama modeli bugün, microsoft iş lerinde veya okul hesaplarında oturum açan uygulamalar için hem HTTP hem de HTTPS şemalarını tüm kuruluşun Azure Etkin Dizini (Azure AD) kiracısında destekler. Bu `signInAudience` alan uygulama bildiriminde ya *AzureADMyOrg* veya *AzureADMultipleOrgs*olarak ayarlanır. Kişisel Microsoft hesaplarında oturum açan uygulamalar için iş ve `signInAudience` okul hesapları *(AzureAD andPersonalMicrosoftAccount*olarak ayarlanır) yalnızca HTTPS düzenine izin verilir.

> [!NOTE]
> Yeni [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyimi, geliştiricilerin UI'ye HTTP şemasıyla ÜRİ'ler eklemesine izin vermez. İş veya okul hesaplarında oturum açan uygulamalar için HTTP URI'lerin eklenmesi yalnızca uygulama bildirimi düzenleyicisi aracılığıyla desteklenir. İleriye dönük olarak, yeni uygulamalar URI yönlendirmede HTTP şemalarını kullanamaz. Ancak, Yeniden Yönlendirme URL'lerinde HTTP şemaları içeren eski uygulamalar çalışmaya devam eder. Geliştiriciler, yeniden yönlendirme ÜR'lerinde HTTPS düzenlerini kullanmalıdır.

## <a name="restrictions-using-a-wildcard-in-uris"></a>URI'lerde joker karakter kullanma kısıtlamaları

Wildcard URI'ler, örneğin, `https://*.contoso.com`uygundur, ancak kaçınılmalıdır. Yeniden yönlendirme URI joker kullanarak güvenlik etkileri vardır. OAuth 2.0 belirtimine[(RFC 6749'un 3.1.2. bölümü)](https://tools.ietf.org/html/rfc6749#section-3.1.2)göre, yeniden yönlendirme bitiş noktası URI mutlak bir URI olmalıdır. 

Azure AD uygulaması modeli, kişisel Microsoft hesaplarında ve iş veya okul hesaplarında oturum açacak şekilde yapılandırılan uygulamalar için joker karakter URL'lerini desteklemez. Ancak, bir kuruluşun Azure AD kiracısında iş veya okul hesaplarında oturum açacak şekilde yapılandırılan uygulamalar için joker karakter URL'lerine izin verilir. 
 
> [!NOTE]
> Yeni [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyimi, geliştiricilerin Web-AI'ye joker karakter URL'leri eklemesine izin vermez. İş veya okul hesaplarında oturum açan uygulamalar için wilcard URI eklenmesi yalnızca uygulama bildirimi düzenleyicisi aracılığıyla desteklenir. İleriye dönük olarak, yeni uygulamalar yeniden yönlendirme URI joker kullanmak mümkün olmayacaktır. Ancak, Yeniden Yönlendirme URL'lerinde joker karakterler içeren eski uygulamalar çalışmaya devam eder.

Senaryonuz izin verilen maksimum sınırdan daha fazla yönlendirme URI gerektiriyorsa, joker karakter yeniden yönlendirme URI eklemek yerine aşağıdaki yaklaşımı göz önünde bulundurun.

### <a name="use-a-state-parameter"></a>Durum parametresi kullanma

Bir dizi alt etki alanınız varsa ve senaryonuz başarılı kimlik doğrulamaüzerine kullanıcıları başladıkları aynı sayfaya yönlendirmenizi gerektiriyorsa, durum parametresi kullanmak yararlı olabilir. 

Bu yaklaşımda:

1. Yetkilendirme bitiş noktasından aldığınız güvenlik belirteçlerini işlemek için uygulama başına "paylaşılan" bir yeniden yönlendirme URI oluşturun.
1. Uygulamanız, uygulamanın özel parametrelerini (kullanıcının geldiği alt etki alanı URL'si veya marka bilgileri gibi herhangi bir şey gibi) devlet parametresinde gönderebilir. Durum parametresi kullanırken, [6749 sayılı RFC'nin 10.12 bölümünde](https://tools.ietf.org/html/rfc6749#section-10.12)belirtildiği gibi CSRF korumasına karşı koruma sağlar). 
1. Uygulamaya özgü parametreler, uygulamanın kullanıcı için doğru deneyimi işlemesi, yani uygun uygulama durumunu oluşturması için gereken tüm bilgileri içerir. Azure AD yetkilendirme bitiş noktası HTML'yi durum parametresi'nden sıyırıyor, bu nedenle bu parametrede HTML içeriğini geçirmediğinizden emin olun.
1. Azure AD, "paylaşılan" yeniden yönlendirme URI'ye bir yanıt gönderdiğinde, durum parametresini uygulamaya geri gönderir.
1. Uygulama daha sonra kullanıcıyı daha fazla gönderecek leri URL'yi belirlemek için durum parametresindeki değeri kullanabilir. CSRF koruması için doğruladığınızdan emin olun.

> [!NOTE]
> Bu yaklaşım, gizliliği ihlal edilen bir istemcinin durum parametresinde gönderilen ek parametreleri değiştirmesine ve böylece kullanıcıyı RFC 6819'da açıklanan [açık yeniden yönlendirme tehdidi](https://tools.ietf.org/html/rfc6819#section-4.2.4) olan farklı bir URL'ye yönlendirmesine olanak tanır. Bu nedenle, istemci durumu şifreleyerek veya uri yeniden yönlendirmesinde etki alanı adını belirteç karşı doğrulamak gibi başka yollarla doğrulayarak bu parametreleri korumak gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama bildirimi](reference-app-manifest.md) hakkında bilgi edinin
