---
title: Yeniden yönlendirme URI 'SI (yanıt URL 'SI) kısıtlamaları | Mavisi
titleSuffix: Microsoft identity platform
description: Microsoft Identity platformu tarafından zorlanan yeniden yönlendirme URI 'SI (yanıt URL 'SI) biçimindeki kısıtlamaların ve kısıtlamaların açıklaması.
author: SureshJa
ms.author: sureshja
manager: CelesteDG
ms.date: 08/07/2020
ms.topic: conceptual
ms.subservice: develop
ms.custom: aaddev
ms.service: active-directory
ms.reviewer: lenalepa, manrath
ms.openlocfilehash: 6a8cc588ff7325242e7e010e9869eaa9a24f6fc2
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88033345"
---
# <a name="redirect-uri-reply-url-restrictions-and-limitations"></a>Yeniden yönlendirme URI 'SI (yanıt URL 'SI) kısıtlamaları ve sınırlamaları

Yeniden yönlendirme URI 'SI veya yanıt URL 'SI, uygulama başarıyla yetkilendirildiğinde ve bir yetkilendirme kodu ya da erişim belirteci verildiğinde, yetkilendirme sunucusunun kullanıcıyı gönderdiği konumdur. Yetkilendirme sunucusu kodu veya belirteci yeniden yönlendirme URI 'sine gönderir; bu nedenle, uygulama kayıt sürecinin bir parçası olarak doğru konumu kaydetmeniz önemlidir.

 Yeniden yönlendirme URI 'Leri için aşağıdaki kısıtlamalar geçerlidir:

* Yeniden yönlendirme URI 'SI şemayla başlamalıdır `https` .

* Yeniden yönlendirme URI 'SI, büyük/küçük harfe duyarlıdır. Büyük/küçük harf durumu, çalışan uygulamanızın URL yolu ile aynı olmalıdır. Örneğin, uygulamanız yolunun bir parçası olarak içeriyorsa `.../abc/response-oidc` , `.../ABC/response-oidc` yeniden yönlendirme URI 'si içinde belirtmeyin. Web tarayıcısı yollara büyük/küçük harfe duyarlı olarak davrandığı için, bununla ilişkili tanımlama bilgileri, `.../abc/response-oidc` büyük/küçük harfe eşleşmeyen URL 'ye yönlendiriliyorsa dışlanamaz `.../ABC/response-oidc` .

## <a name="maximum-number-of-redirect-uris"></a>Maksimum yeniden yönlendirme URI sayısı

Bu tablo, Microsoft Identity platformunda bir uygulama kaydına ekleyebileceğiniz en fazla yeniden yönlendirme URI sayısını gösterir.

| Oturum açan hesaplar | Maksimum yeniden yönlendirme URI sayısı | Açıklama |
|--------------------------|---------------------------------|-------------|
| Herhangi bir kuruluşun Azure Active Directory (Azure AD) kiracısındaki Microsoft iş veya okul hesapları | 256 | `signInAudience`uygulama bildirimindeki alan *Azureadmyorg* ya da *Azureadmultipleorgs* olarak ayarlandı |
| Kişisel Microsoft hesapları ve iş ve okul hesapları | 100 | `signInAudience`uygulama bildirimindeki alan *Azureadandpersonmicrosoftaccount* olarak ayarlandı |

## <a name="maximum-uri-length"></a>En fazla URI uzunluğu

Bir uygulama kaydına eklediğiniz her yeniden yönlendirme URI 'SI için en fazla 256 karakter kullanabilirsiniz.

## <a name="supported-schemes"></a>Desteklenen düzenler

Azure Active Directory (Azure AD) uygulama modeli, herhangi bir kuruluşun Azure AD kiracısında iş veya okul hesaplarında oturum açmış uygulamalar için hem HTTP hem de HTTPS düzenlerini desteklemektedir. Bu hesap türleri, `AzureADMyOrg` `AzureADMultipleOrgs` uygulama bildiriminin alanındaki ve değerleri tarafından belirtilir `signInAudience` . Kişisel Microsoft hesapları (MSA) *ve* iş ve okul hesaplarında (yani, `signInAudience` olarak ayarlanır) oturum açmak için `AzureADandPersonalMicrosoftAccount` yalnızca https şemasına izin verilir.

İş veya okul hesaplarında oturum açmaya yönelik uygulama kayıtlarına bir HTTP düzeniyle yeniden yönlendirme URI 'Leri eklemek için, Azure portal [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) içindeki uygulama bildirimi düzenleyicisini kullanmanız gerekir. Ancak, bildirim düzenleyicisini kullanarak HTTP tabanlı yeniden yönlendirme URI 'SI ayarlamak mümkün olsa da, yeniden yönlendirme URI 'larınız için HTTPS şemasını kullanmanız *önemle* önerilir.

## <a name="localhost-exceptions"></a>Localhost özel durumları

Her [RFC 8252 bölümü 8,3](https://tools.ietf.org/html/rfc8252#section-8.3) ve [7,3](https://tools.ietf.org/html/rfc8252#section-7.3), "geridöngü" veya "localhost" yeniden yönlendirme URI 'leri iki özel dikkat ile gelir:

1. `http`Yeniden yönlendirme cihazdan hiçbir şekilde ayrılmadığından URI şemaları kabul edilebilir. Bu nedenle, her ikisi de kabul edilebilir:
    - `http://127.0.0.1/myApp`
    - `https://127.0.0.1/myApp`
1. Yerel uygulamalar için genellikle gerekli olan kısa ömürlü bağlantı noktası aralıkları nedeniyle, bir yeniden yönlendirme URI 'sini eşleştirmek amacıyla bağlantı noktası bileşeni (örneğin, `:5001` veya `:443` ) yok sayılır. Sonuç olarak, bunların tümü eşdeğer olarak kabul edilir:
    - `http://127.0.0.1/MyApp`
    - `http://127.0.0.1:1234/MyApp`
    - `http://127.0.0.1:5000/MyApp`
    - `http://127.0.0.1:8080/MyApp`

Geliştirme açısından, bu birkaç şey anlamına gelir:

1. Yalnızca bağlantı noktasının farklı olduğu birden çok yeniden yönlendirme URI 'sini kaydetme. Oturum açma sunucusu bir rastgele seçer ve bu yeniden yönlendirme URI 'siyle ilişkili davranışı kullanır (örneğin,-, `web` `native` -veya `spa` -Type yeniden yönlendirmesi).
1. Geliştirme sırasında farklı akışları test etmek için localhost 'a birden çok yeniden yönlendirme URI 'SI kaydetmeniz gerekiyorsa, bunları URI 'nin *yol* bileşenini kullanarak birbirinden ayırın. Örneğin, `http://127.0.0.1/MyWebApp` eşleşmez `http://127.0.0.1/MyNativeApp` .
1. RFC gözetimi başına, `localhost` yeniden yönlendirme URI 'si içinde kullanmamalısınız. Bunun yerine, gerçek geri döngü IP adresini kullanın `127.0.0.1` . Bu, uygulamanızın yanlış yapılandırılmış güvenlik duvarları veya yeniden adlandırılmış ağ arabirimleri tarafından bozulmasına engel olur.

    IPv6 geri döngü adresi ( `[::1]` ) Şu anda desteklenmiyor.

## <a name="restrictions-on-wildcards-in-redirect-uris"></a>Yeniden yönlendirme URI 'Lerinde Joker karakterlere yönelik kısıtlamalar

Gibi joker karakter URI 'Leri `https://*.contoso.com` kullanışlı görünebilir, ancak güvenlik nedeniyle kaçınılması gerekir. OAuth 2,0 belirtimine göre ([RFC 6749 ' nin Bölüm 3.1.2](https://tools.ietf.org/html/rfc6749#section-3.1.2)), bir yeniden yönlendirme uç noktası URI 'si mutlak bir URI olmalıdır.

Joker karakterler şu anda kişisel Microsoft hesaplarında ve iş veya okul hesaplarında oturum açmak üzere yapılandırılmış uygulama kayıtlarında desteklenmez. Ancak, bir kuruluşun Azure AD kiracısında yalnızca iş veya okul hesaplarında oturum açmak üzere yapılandırılan uygulamalar için joker karakter URI 'Lerinde izin verilir.

İş veya okul hesaplarında oturum açmaya yönelik uygulama kayıtlarına joker karakterler içeren yeniden yönlendirme URI 'Leri eklemek için, Azure portal [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) içindeki uygulama bildirimi düzenleyicisini kullanmanız gerekir. Bildirim düzenleyicisini kullanarak bir joker karakterle yeniden yönlendirme URI 'SI ayarlamak mümkün olsa da, 3.1.2 for the [RFC 6749 bölümüne](https://tools.ietf.org/html/rfc6749#section-3.1.2) bağlı tutmanız ve yalnızca mutlak URI 'ler kullanmanız *önemle* önerilir.

Senaryonuz izin verilen üst sınırdan daha fazla yeniden yönlendirme URI 'si gerektiriyorsa, joker karakter yeniden yönlendirme URI 'si eklemek yerine [aşağıdaki yaklaşımı](#use-a-state-parameter) göz önünde bulundurun.

### <a name="use-a-state-parameter"></a>Bir durum parametresi kullanın

Birkaç alt etki alanı varsa ve senaryonuz, başarılı bir kimlik doğrulamasından sonra, kullanıcıları başlatıldığı sayfaya yönlendirdikten sonra bir durum parametresinin kullanılması faydalı olabilir.

Bu yaklaşımda:

1. Yetkilendirme uç noktasından aldığınız güvenlik belirteçlerini işlemek için uygulama başına bir "paylaşılan" yeniden yönlendirme URI 'SI oluşturun.
1. Uygulamanız, durum parametresindeki uygulamaya özgü parametreleri (kullanıcının kaynaklandığı veya marka bilgileri gibi bir şey) gönderebilir. Bir durum parametresi kullanırken, [10,12 ' ın RFC 6749 bölümünde](https://tools.ietf.org/html/rfc6749#section-10.12)belirtildiği gibi CSRF korumasına karşı koruma yapın.
1. Uygulamaya özgü parametreler, uygulamanın kullanıcı için doğru deneyimi işlemesi için gereken tüm bilgileri ve diğer bir deyişle, uygun uygulama durumunu içerir. Azure AD yetkilendirme uç noktası, durum parametresinden HTML 'yi şeritler, bu nedenle bu parametreye HTML içeriği geçirdiğinizden emin olun.
1. Azure AD, "paylaşılan" yeniden yönlendirme URI 'sine bir yanıt gönderdiğinde, durum parametresini uygulamaya geri gönderir.
1. Daha sonra uygulama, kullanıcının daha fazla gönderileceği URL 'YI belirleyebilmek için durum parametresindeki değeri kullanabilir. CSRF koruması için doğrulama yaptığınızdan emin olun.

> [!WARNING]
> Bu yaklaşım, güvenliği aşılmış bir istemcinin durum parametresinde gönderilen ek parametreleri değiştirmesine olanak tanır. böylece Kullanıcı, RFC 6819 ' de açıklanan [Açık yeniden yönlendirici tehdidi](https://tools.ietf.org/html/rfc6819#section-4.2.4) olan farklı bir URL 'ye yeniden yönlendirme sağlar. Bu nedenle, istemci, durumu şifreleyerek veya yeniden yönlendirme URI 'sindeki etki alanı adını belirtece karşı doğrulamak gibi bazı yollarla bu parametreleri korumalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Uygulama kaydı [uygulama bildirimi](reference-app-manifest.md)hakkında bilgi edinin.
