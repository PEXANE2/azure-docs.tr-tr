---
title: Azure SignalR Hizmetinde esneklik ve olağanüstü durum kurtarma
description: Esneklik ve olağanüstü durum kurtarma sağlamak için birden fazla SignalR hizmeti örneğinin nasıl ayarlanacaklarına ilişkin genel bir bakış
author: chenkennt
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: kenchen
ms.openlocfilehash: cf0f345b0fbf9fea2512f72c1996c9a1597cc0cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73747655"
---
# <a name="resiliency-and-disaster-recovery"></a>Dayanıklılık ve olağanüstü durum kurtarma

Esneklik ve olağanüstü durum kurtarma online sistemler için ortak bir ihtiyaçtır. Azure SignalR Hizmeti zaten %99,9 kullanılabilirliği garanti eder, ancak yine de bölgesel bir hizmettir.
Hizmet örneğiniz her zaman bir bölgede çalışır ve bölge genelinde kesinti olduğunda başka bir bölgeye gitmez.

Bunun yerine, hizmetImiz SDK birden çok SignalR servis örneğini desteklemek ve bazıları kullanılamadığında otomatik olarak diğer örneklere geçmek için bir işlev sağlar.
Bu özellik sayesinde, bir felaket gerçekleştiğinde kurtarabilirsiniz, ancak doğru sistem topolojisini kendiniz kurmanız gerekir. Bunu nasıl yapacağınızı bu belgede öğreneceksiniz.

## <a name="high-available-architecture-for-signalr-service"></a>SignalR hizmeti için yüksek kullanılabilir mimari

SignalR hizmeti için bölgeler arası esneklik sağlamak için, farklı bölgelerde birden çok hizmet örneği ayarlamanız gerekir. Bu nedenle, bir bölge çöktüğinde, diğerleri yedek olarak kullanılabilir.
Birden çok hizmet örneğini uygulama sunucusuna bağlarken, birincil ve ikincil olmak üzere iki rol vardır.
Birincil çevrimiçi trafik alıyor ve ikincil birincil için tam işlevsel ama yedekleme örneği bir örnektir.
SDK uygulamamızda, müzakere yalnızca birincil uç noktaları döndürecek, böylece normal durumda istemciler yalnızca birincil uç noktalara bağlanır.
Ancak birincil örnek düştüğünde, istemci hala bağlantı kurabilmesi için anlaşma ikincil uç noktaları döndürecektir.
Birincil örnek ve uygulama sunucusu normal sunucu bağlantıları üzerinden bağlanır, ancak ikincil örnek ve uygulama sunucusu zayıf bağlantı adı verilen özel bir bağlantı türü aracılığıyla bağlanır.
Zayıf bir bağlantının temel farkı, ikincil örnek başka bir bölgede bulunduğundan istemci bağlantı yönlendirmesini kabul etmemesidir. İstemciyi başka bir bölgeye yönlendirmeen iyi bir seçim değildir (gecikme yitirmesi artar).

Bir hizmet örneği, birden çok uygulama sunucusuna bağlanırken farklı rollere sahip olabilir.
Bölgeler arası senaryo için tipik bir kurulum, iki (veya daha fazla) SignalR hizmet örneği ve uygulama sunucusuna sahip olmaktır.
Her çift uygulama sunucusu ve SignalR hizmeti içinde aynı bölgede bulunan ve SignalR hizmeti birincil rol olarak uygulama sunucusuna bağlanır.
Her çift arasında uygulama sunucusu ve SignalR hizmeti de bağlanır, ancak SignalR başka bir bölgedeki sunucuya bağlanırken ikincil olur.

Bu topoloji ile, tüm uygulama sunucuları ve SignalR hizmet örnekleri birbirine bağlı olduğundan, bir sunucudan gelen ileti tüm istemcilere iletilebilir.
Ancak bir istemci bağlı olduğunda, en iyi ağ gecikmesi elde etmek için her zaman aynı bölgedeki uygulama sunucusuna yönlendirilir.

Aşağıda bu tür topoloji gösteren bir diyagram:

![topology](media/signalr-concept-disaster-recovery/topology.png)

## <a name="configure-app-servers-with-multiple-signalr-service-instances"></a>Birden çok SignalR hizmeti örneği olan uygulama sunucularını yapılandırma

Her bölgede SignalR hizmeti ve uygulama sunucuları oluşturulduktan sonra, uygulama sunucularınızı tüm SignalR hizmet örneklerine bağlanacak şekilde yapılandırabilirsiniz.

Bunu yapmanın iki yolu vardır:

### <a name="through-config"></a>Config ile

SignalR servis bağlantı dizesini çevre değişkenleri/uygulama ayarları/web.cofig aracılığıyla nasıl ayarladığınızı zaten bilmeniz `Azure:SignalR:ConnectionString`gerekir.
Birden çok uç noktanız varsa, bunları her biri aşağıdaki biçimde birden çok config girişine ayarlayabilirsiniz:

```
Azure:SignalR:ConnectionString:<name>:<role>
```

Burada `<name>` bitiş noktasının adı `<role>` ve rolü (birincil veya ikincil).
Ad isteğe bağlıdır, ancak yönlendirme davranışını birden çok uç nokta arasında daha da özelleştirmek istiyorsanız yararlı olacaktır.

### <a name="through-code"></a>Kod aracılığıyla

Bağlantı dizelerini başka bir yerde depolamayı tercih ederseniz, bunları kodunuzda okuyabilir `AddAzureSignalR()` ve (ASP.NET Core'da) veya `MapAzureSignalR()` (ASP.NET) ararken parametre olarak kullanabilirsiniz.

İşte örnek kodu:

ASP.NET Core:

```cs
services.AddSignalR()
        .AddAzureSignalR(options => options.Endpoints = new ServiceEndpoint[]
        {
            new ServiceEndpoint("<connection_string1>", EndpointType.Primary, "region1"),
            new ServiceEndpoint("<connection_string2>", EndpointType.Secondary, "region2"),
        });
```

ASP.NET:

```cs
app.MapAzureSignalR(GetType().FullName, hub,  options => options.Endpoints = new ServiceEndpoint[]
    {
        new ServiceEndpoint("<connection_string1>", EndpointType.Primary, "region1"),
        new ServiceEndpoint("<connection_string2>", EndpointType.Secondary, "region2"),
    };
```

Birden çok birincil veya ikincil örneği yapılandırabilirsiniz. Birden çok birincil ve/veya ikincil örnek varsa, anlaşma aşağıdaki sırada bir bitiş noktası döndürecektir:

1. Çevrimiçi olarak en az bir birincil örnek varsa, rasgele bir birincil çevrimiçi örneği döndürün.
2. Tüm birincil örnekler aşağıdaysa, rasgele bir ikincil çevrimiçi örneği döndürün.

## <a name="failover-sequence-and-best-practice"></a>Failover dizisi ve en iyi uygulama

Şimdi doğru sistem topoloji kurulum var. Bir SignalR servis örneği düştüğünde, çevrimiçi trafik diğer örneklere yönlendirilir.
Birincil örnek düştüğünde (ve bir süre sonra iyileştiğinde) aşağıda ne olur:

1. Birincil hizmet örneği kapalı, bu örnekteki tüm sunucu bağlantıları bırakılır.
2. Bu örne bağlı tüm sunucular çevrimdışı olarak işaretleyecek ve bu bitiş noktasını döndürmeyi durduracak ve ikincil bitiş noktasını döndürmeye başlayacak.
3. Bu örnekteki tüm istemci bağlantıları da kapatılacak, istemciler yeniden bağlanır. Uygulama sunucuları artık ikincil bitiş noktasını döndürdüklerine göre, istemciler ikincil örneklere bağlanır.
4. Şimdi ikincil örnek tüm online trafik alır. İkincil tüm uygulama sunucularına bağlı olduğu için sunucudan istemcilere tüm iletiler yine de teslim edilebilir. Ancak istemciden sunucuya iletiler yalnızca aynı bölgedeki uygulama sunucusuna yönlendirilir.
5. Birincil örnek kurtarıldıktan ve çevrimiçi olarak geri döndükten sonra, uygulama sunucusu bağlantılarını yeniden kurar ve çevrimiçi olarak işaretler. Yeni istemciler birincil e yeniden bağlı böylece Negotiate şimdi birincil bitiş noktasını yeniden döndürecek. Ancak varolan istemciler bırakılacak ve bağlantılarını kesene kadar ikincil istemcilere yönlendirilmeye devam edeceklerdir.

Aşağıdaki diyagramlar SignalR hizmetinde failover'ın nasıl yapıldığını göstermektedir:

Şekil.1 ![Failover'dan Önce Başarısız olmadan önce](media/signalr-concept-disaster-recovery/before-failover.png)

Şekil.2 ![Failover'dan sonra başarısız olduktan sonra](media/signalr-concept-disaster-recovery/after-failover.png)

Şekil.3 Primer sonrası ![kısa süre primer kurtarmalardan kısa bir süre sonra](media/signalr-concept-disaster-recovery/after-recover.png)

Normal durumda sadece birincil uygulama sunucusu ve SignalR hizmeti (mavi) online trafik var görebilirsiniz.
Başarısız olduktan sonra, ikincil uygulama sunucusu ve SignalR hizmeti de etkin hale gelir.
Birincil SignalR hizmeti yeniden çevrimiçi olduktan sonra, yeni istemciler birincil SignalR'a bağlanır. Ancak varolan istemciler yine de ikincil bağlantıya bağlanır, bu nedenle her iki örnekde de trafik vardır.
Varolan tüm istemciler bağlantıyı kestikten sonra, sisteminiz normale döner (Şekil 1).

Çapraz bölge yüksek kullanılabilir mimarisi uygulamak için iki ana desen vardır:

1. Bunlardan ilki, tüm çevrimiçi trafiği ele alan bir çift uygulama sunucusu ve SignalR servis örneğine sahip olmak ve yedek olarak başka bir çifte sahip olmaktır (Fig.1'de gösterildiği gibi aktif/pasif olarak adlandırılır). 
2. Diğeri ise, her biri çevrimiçi trafiğin bir parçası olan ve diğer çiftler için yedek olarak hizmet veren (Fig.3'e benzer aktif/etkin olarak adlandırılır) iki (veya daha fazla) uygulama sunucusu ve SignalR hizmet örneği çifti olmasıdır.

SignalR hizmeti her iki deseni de destekleyebilir, temel fark uygulama sunucularını nasıl uyguladığınızdır.
Uygulama sunucuları etkin/pasifse, SignalR hizmeti de etkin/pasif olacaktır (birincil uygulama sunucusu yalnızca birincil SignalR hizmet örneğini döndürdükçe).
Uygulama sunucuları etkin/etkinse, SignalR hizmeti de etkin/etkin olacaktır (tüm uygulama sunucuları kendi birincil SignalR örneklerini döndüreceği için hepsi trafik alabilir).

Hangi desenleri kullanmayı seçerseniz seçin, her SignalR servis örneğini birincil olarak bir uygulama sunucusuna bağlamanız gerekir.

Ayrıca SignalR bağlantısının doğası gereği (uzun bir bağlantıdır), müşteriler bir felaket olduğunda ve arıza olduğunda bağlantı düşüşleri yaşarlar.
Son müşterilerinize saydam hale getirmek için bu tür servis taleplerini istemci tarafında ele almanız gerekir. Örneğin, bağlantı kapatıldıktan sonra yeniden bağlanmayı yapın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, SignalR hizmeti için esneklik elde etmek için uygulamanızı yapılandırmayı öğrendiniz. SignalR hizmetinde sunucu/istemci bağlantısı ve bağlantı yönlendirmesi hakkında daha fazla bilgi almak için SignalR hizmeti dahili bilgileri için [bu makaleyi](signalr-concept-internals.md) okuyabilirsiniz.

Çok sayıda bağlantıyı işlemek için birden çok örneği birlikte kullanan parçalama gibi ölçeklendirme [senaryoları için, birden çok örneği nasıl ölçeklendirileceği](signalr-howto-scale-multi-instances.md)okundu.
