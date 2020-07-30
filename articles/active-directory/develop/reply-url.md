---
title: Yeniden yönlendirme URI & yanıt URL 'SI kısıtlamaları-Microsoft Identity platform | Mavisi
description: Yanıt URL 'Leri/yeniden yönlendirme URL kısıtlamaları & sınırlamaları
author: SureshJa
ms.author: sureshja
manager: CelesteDG
ms.date: 07/17/2020
ms.topic: conceptual
ms.subservice: develop
ms.custom: aaddev
ms.service: active-directory
ms.reviewer: lenalepa, manrath
ms.openlocfilehash: 4fdeb0018e27a2557161b2ec1c4794d975403523
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87311628"
---
# <a name="redirect-urireply-url-restrictions-and-limitations"></a>Yeniden yönlendirme URI’si/yanıt URL’si kısıtlamaları ve sınırlamaları

Bir yeniden yönlendirme URI 'si veya yanıt URL 'SI, uygulama başarıyla yetkilendirildiğinde ve yetkilendirme kodu ya da erişim belirteci verildiğinde, yetkilendirme sunucusunun kullanıcıyı kullanıcıya göndereceği konumdur. Kod veya belirteç yeniden yönlendirme URI 'SI veya Yanıt belirtecinde bulunur, bu nedenle doğru konumu uygulama kayıt işleminin bir parçası olarak kaydetmeniz önemlidir.

 Yanıt URL 'Leri için aşağıdaki kısıtlamalar geçerlidir:

* Yanıt URL 'SI, şemayla başlamalıdır `https` .

* Yanıt URL 'SI, büyük/küçük harfe duyarlıdır. Büyük/küçük harf durumu, çalışan uygulamanızın URL yolu ile aynı olmalıdır. Örneğin, uygulamanız yolunun bir parçası olarak içeriyorsa `.../abc/response-oidc` , `.../ABC/response-oidc` yanıt URL 'sinde belirtmeyin. Web tarayıcısı yollara büyük/küçük harfe duyarlı olarak davrandığı için, bununla ilişkili tanımlama bilgileri, `.../abc/response-oidc` büyük/küçük harfe eşleşmeyen URL 'ye yönlendiriliyorsa dışlanamaz `.../ABC/response-oidc` .
    
## <a name="maximum-number-of-redirect-uris"></a>Maksimum yeniden yönlendirme URI sayısı

Aşağıdaki tabloda, uygulamanızı kaydettiğinizde ekleyebileceğiniz en fazla yeniden yönlendirme URI sayısı gösterilmektedir.

| Oturum açan hesaplar | Maksimum yeniden yönlendirme URI sayısı | Description |
|--------------------------|---------------------------------|-------------|
| Herhangi bir kuruluşun Azure Active Directory (Azure AD) kiracısındaki Microsoft iş veya okul hesapları | 256 | `signInAudience`uygulama bildirimindeki alan *Azureadmyorg* ya da *Azureadmultipleorgs* olarak ayarlandı |
| Kişisel Microsoft hesapları ve iş ve okul hesapları | 100 | `signInAudience`uygulama bildirimindeki alan *Azureadandpersonmicrosoftaccount* olarak ayarlandı |

## <a name="maximum-uri-length"></a>En fazla URI uzunluğu

Bir uygulama kaydına eklediğiniz her bir yeniden yönlendirme URI 'SI için en fazla 256 karakter kullanabilirsiniz.

## <a name="supported-schemes"></a>Desteklenen düzenler

Azure AD uygulama modeli, Microsoft iş veya okul hesaplarında herhangi bir kuruluşun Azure Active Directory (Azure AD) kiracısında oturum açmasını sağlayan uygulamalar için hem HTTP hem de HTTPS düzenlerini destekler. `signInAudience`Uygulama bildirimindeki alan, *Azureadmyorg* veya *Azureadmultipleorgs*olarak ayarlanır. Kişisel Microsoft hesaplarında ve iş ve okul hesaplarında oturum açmak için ( `signInAudience` *Azureadandpersonmicrosoftaccount*olarak AYARLANıR) yalnızca https düzenine izin verilir.

> [!NOTE]
> Yeni [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyimi, geliştiricilerin kullanıcı arabiriminde http düzenine sahip URI 'leri eklemesine izin vermez. İş veya okul hesaplarında oturum açma uygulamaları için HTTP URI 'Leri eklemek yalnızca uygulama bildirimi Düzenleyicisi aracılığıyla desteklenir. İleri giderek, yeni uygulamalar yeniden yönlendirme URI 'sinde HTTP şemaları kullanamaz. Ancak, yeniden yönlendirme URI 'Lerinde HTTP şemaları içeren eski uygulamalar çalışmaya devam edecektir. Geliştiricilerin yeniden yönlendirme URI 'Lerinde HTTPS şemaları kullanması gerekir.

## <a name="localhost-exceptions"></a>Localhost özel durumları

Her [RFC 8252 bölümü 8,3](https://tools.ietf.org/html/rfc8252#section-8.3) ve [7,3](https://tools.ietf.org/html/rfc8252#section-7.3), "geridöngü" veya "localhost" yeniden yönlendirme URI 'leri iki özel dikkat ile gelir:

1. `http`Yeniden yönlendirme cihazdan hiçbir şekilde ayrıldığından URI şemaları kabul edilebilir.  Bu `http://127.0.0.1/myApp` , kabul edilebilir olduğu anlamına gelir `https://127.0.0.1/myApp` . 
1. Yerel uygulamalar için genellikle gerekli olan kısa ömürlü bağlantı noktası aralıkları nedeniyle, bir yeniden yönlendirme URI 'sini eşleştirmek amacıyla bağlantı noktası bileşeni (ör. `:5001` veya `:443` ) yok sayılır.  Sonuç olarak, `http://127.0.0.1:5000/MyApp` ve `http://127.0.0.1:1234/MyApp` her ikisi de ile eşleşir `http://127.0.0.1/MyApp``http://127.0.0.1:8080/MyApp`

Geliştirme açısından, bu birkaç şey anlamına gelir:

1. Yalnızca bağlantı noktasının farklı olduğu birden çok yanıt URI 'sini kaydetme.  Oturum açma sunucusu rastgele bir değer seçer ve bu yanıt URI 'siyle ilişkili davranışı kullanır (örneğin, bir, `web` `native` ve `spa` -tür yönlendirmesi olup olmadığı).
1. Geliştirme sırasında farklı akışları test etmek için localhost 'a birden çok yeniden yönlendirme URI 'SI kaydetmeniz gerekiyorsa, bunları URI 'nin *yol* bileşenini kullanarak birbirinden ayırın.  `http://127.0.0.1/MyWebApp`eşleşmiyor `http://127.0.0.1/MyNativeApp` .  
1. RFC gözetimi başına, `localhost` yeniden yönlendirme URI 'si içinde kullanmamalısınız.  Bunun yerine, gerçek geri döngü IP adresini kullanın `127.0.0.1` . Bu, uygulamanızın yanlış yapılandırılmış güvenlik duvarları veya yeniden adlandırılmış ağ arabirimleri tarafından bozulmasına engel olur.

>[!NOTE]
> Şu anda IPv6 geri döngüsü ( `[::1]` ) Şu anda desteklenmiyor.  Bu, sonraki bir tarihte eklenir.

## <a name="restrictions-using-a-wildcard-in-uris"></a>URI 'Ler içinde bir joker karakter kullanan kısıtlamalar

Gibi joker karakter URI 'Leri `https://*.contoso.com` kullanışlıdır, ancak kaçınılmalıdır. Yeniden yönlendirme URI 'sinde joker karakter kullanılması güvenlik etkilerine sahiptir. OAuth 2,0 belirtimine göre ([RFC 6749 ' nin Bölüm 3.1.2](https://tools.ietf.org/html/rfc6749#section-3.1.2)), bir yeniden yönlendirme uç noktası URI 'si mutlak bir URI olmalıdır.

Azure AD uygulama modeli, kişisel Microsoft hesaplarında ve iş veya okul hesaplarında oturum açmak üzere yapılandırılan uygulamalar için joker karakter URI 'Lerini desteklemez. Ancak, günümüzde bir kuruluşun Azure AD kiracısında iş veya okul hesaplarında oturum açmak üzere yapılandırılan uygulamalar için joker karakter URI 'Lerinde izin verilir.

> [!NOTE]
> Yeni [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyimi, geliştiricilerin kullanıcı arabirimine joker URI 'ler eklemesine izin vermez. İş veya okul hesaplarında oturum açmak için yalnızca uygulama bildirimi Düzenleyicisi aracılığıyla desteklenen uygulamalar için solcard URI ekleme desteklenir. İleri giderek yeni uygulamalar, yeniden yönlendirme URI 'sinde joker karakterler kullanamaz. Ancak, yeniden yönlendirme URI 'Lerinde joker karakterler içeren eski uygulamalar çalışmaya devam edecektir.

Senaryonuz, izin verilen en yüksek sınırdan daha fazla yeniden yönlendirme URI 'si gerektiriyorsa, joker karakter yeniden yönlendirme URI 'si eklemek yerine aşağıdaki yaklaşımı göz önünde bulundurun.

### <a name="use-a-state-parameter"></a>Bir durum parametresi kullanın

Bir dizi alt etki alanı varsa ve senaryonuz, başarıyla başlatıldığı sayfada kullanıcıları yeniden yönlendirmenizi gerektiriyorsa, bir durum parametresinin kullanılması faydalı olabilir.

Bu yaklaşımda:

1. Yetkilendirme uç noktasından aldığınız güvenlik belirteçlerini işlemek için uygulama başına bir "paylaşılan" yeniden yönlendirme URI 'SI oluşturun.
1. Uygulamanız, durum parametresindeki uygulamaya özgü parametreleri (kullanıcının kaynaklandığı veya marka bilgileri gibi) uygulamaya özgü parametreler gönderebilir. Bir durum parametresi kullanırken, [10,12 ' ın RFC 6749 bölümünde](https://tools.ietf.org/html/rfc6749#section-10.12)belirtildiği gibi CSRF korumasına karşı koruma yapın. 
1. Uygulamaya özgü parametreler, uygulamanın kullanıcı için doğru deneyimi işlemesi için gereken tüm bilgileri ve diğer bir deyişle, uygun uygulama durumunu içerir. Azure AD yetkilendirme uç noktası, durum parametresinden HTML 'yi şeritler, bu nedenle bu parametreye HTML içeriği geçirdiğinizden emin olun.
1. Azure AD, "paylaşılan" yeniden yönlendirme URI 'sine bir yanıt gönderdiğinde, durum parametresini uygulamaya geri gönderir.
1. Daha sonra uygulama, kullanıcının daha fazla gönderileceği URL 'YI belirleyebilmek için durum parametresindeki değeri kullanabilir. CSRF koruması için doğrulama yaptığınızdan emin olun.

> [!NOTE]
> Bu yaklaşım, güvenliği aşılmış bir istemcinin durum parametresinde gönderilen ek parametreleri değiştirmesine olanak tanır. böylece Kullanıcı, RFC 6819 ' de açıklanan [Açık yeniden yönlendirici tehdidi](https://tools.ietf.org/html/rfc6819#section-4.2.4) olan farklı bir URL 'ye yeniden yönlendirme sağlar. Bu nedenle, istemci, durumu şifreleyerek veya yeniden yönlendirme URI 'sindeki etki alanı adını belirtece karşı doğrulamak gibi bazı yollarla bu parametreleri korumalıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama bildirimi](reference-app-manifest.md) hakkında bilgi edinin
