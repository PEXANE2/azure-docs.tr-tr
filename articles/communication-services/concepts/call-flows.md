---
title: Azure Iletişim hizmetlerinde çağrı akışları
titleSuffix: An Azure Communication Services concept document
description: Azure Iletişim hizmetlerindeki çağrı akışları hakkında bilgi edinin.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 7172e3319e60603d46dc2af87f3818a5c3664285
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948264"
---
# <a name="call-flows"></a>Çağrı akışları

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

Aşağıdaki bölümde, Azure Iletişim hizmetlerindeki çağrı akışlarına ilişkin bir genel bakış sunulmaktadır. Sinyal ve medya akışları kullanıcılarınızın yapmakta olduğu çağrı türlerine bağlıdır. Çağrı türlerine örnek olarak, VoIP ve PSTN bağlantılı katılımcılar bileşimini içeren tek-bir VoIP, bire bir PSTN ve grup çağrısı vardır. [Çağrı türlerini](./voice-video-calling/about-call-types.md)gözden geçirin.

## <a name="about-signaling-and-media-protocols"></a>Sinyal ve medya protokolleri hakkında

Eşler arası veya grup çağrısı yaptığınızda, arka planda iki protokol kullanılır-HTTP (REST) for sinyal ve medya için SRTP. 

İstemci kitaplıkları, istemci kitaplıkları ve Iletişim Hizmetleri sinyal denetleyicileri arasında sinyal verme, HTTP REST (TLS) ile işlenir. Gerçek zamanlı medya trafiği (RTP) için, Kullanıcı veri birimi Protokolü (UDP) tercih edilir. UDP 'nin kullanılması güvenlik duvarınız tarafından engellenmediyse, istemci kitaplığı, medya için Iletim Denetim Protokolü (TCP) kullanır. 

Sinyal ve medya protokollerini çeşitli senaryolarda gözden geçirelim. 

## <a name="call-flow-cases"></a>Çağrı akışı durumları

### <a name="case-1-voip-where-a-direct-connection-between-two-devices-is-possible"></a>Durum 1: iki cihaz arasında doğrudan bir bağlantının mümkün olduğu VoIP

Bire bir VoIP veya video çağrılarında trafik, en doğrudan yolu tercih eder. "Doğrudan yol", iki istemci kitaplıklarının birbirlerine doğrudan ulaşabulabileceği anlamına gelir; doğrudan bağlantı kurar. Bu, genellikle iki istemci kitaplığı aynı alt ağda olduğunda (örneğin 192.168.1.0/24 ' te) veya her biri birbirini görebileceğiniz alt ağlarda bulunan cihazların (10.10.0.0/16 ve 192.168.1.0/24 alt ağındaki istemci kitaplıkları birbirlerine ulaşabildiğinde) iki istemci kitaplığı olduğunda mümkündür.

:::image type="content" source="./media/call-flows/about-voice-case-1.png" alt-text="Kullanıcılar ve Iletişim Hizmetleri arasında doğrudan bir VOıP çağrısını gösteren diyagram.":::

### <a name="case-2-voip-where-a-direct-connection-between-devices-is-not-possible-but-where-connection-between-nat-devices-is-possible"></a>Durum 2: cihazlar arasında doğrudan bir bağlantının mümkün olmadığı, ancak NAT cihazları arasındaki bağlantının mümkün olmadığı VoIP

İki cihaz birbirlerine ulaşamayan ağlarda bulunuyorsa (örneğin, Gamze bir kafeterden çalışarak ve can ev ofisinizden çalışıyorsa), ancak NAT cihazları arasındaki bağlantı, istemci tarafı istemci kitaplıkları NAT cihazları aracılığıyla bağlantı kurar. 

Çiğdem için kafeterde NAT ve Bob için ev ofisindeki NAT olacaktır. Çiğdem 'in cihazı, NAT 'nin dış adresini gönderecek ve Bob 'un aynı şekilde yapacağız. İstemci kitaplıkları, Azure Iletişim hizmetlerinin ücretsiz olarak sağladığı bir STUN (NAT için oturum çapraz araçları) hizmetinden dış adresler öğrenirler. Çiğdem ve Bob arasındaki el sıkışmasını işleyen mantık, istemci kitaplıkları tarafından sağlanmış olan Azure Iletişim Hizmetleri içine katıştırılır. (Ek yapılandırma gerekmez)

:::image type="content" source="./media/call-flows/about-voice-case-2.png" alt-text="Bir STUN bağlantısını kullanan bir VOıP çağrısını gösteren diyagram.":::

### <a name="case-3-voip-where-neither-a-direct-nor-nat-connection-is-possible"></a>Durum 3: doğrudan veya NAT bağlantısı mümkün olmayan VoIP

İstemci cihazlarının biri veya her ikisi simetrik bir NAT 'nin arkasında ise, iki istemci kitaplığı arasında medyanın geçişine yönelik ayrı bir bulut hizmeti gereklidir. Bu hizmet, SıRASıYLA (NAT etrafında geçişler kullanılarak çapraz geçiş) olarak adlandırılır ve Iletişim Hizmetleri tarafından da sağlanır. İstemci kitaplığı 'Nı çağıran Iletişim Hizmetleri, algılanan ağ koşullarına bağlı olarak Hizmetleri otomatik olarak kullanır. Microsoft 'un aç hizmetinin kullanımı ayrı olarak ücretlendirilir.

:::image type="content" source="./media/call-flows/about-voice-case-3.png" alt-text="Bir Aç bağlantısını kullanan bir VOıP çağrısını gösteren diyagram.":::
 
### <a name="case-4-group-calls-with-pstn"></a>Durum 4: PSTN ile grup çağrıları

PSTN çağrıları için hem sinyal hem de medya, Azure Iletişim Hizmetleri telefon kaynağını kullanır. Bu kaynak diğer taşıyıcılar ile bağlantılı.

PSTN medya trafiği, medya Işlemcisi adlı bir bileşen üzerinden akar.

:::image type="content" source="./media/call-flows/about-voice-pstn.png" alt-text="Iletişim hizmetleriyle bir PSTN Grup çağrısını gösteren diyagram.":::

> [!NOTE]
> Medya işleme hakkında bilgi sahibi olmak için, medya işlemcimiz, [RFC 3261 SIP: oturum başlatma Protokolü](https://tools.ietf.org/html/rfc3261)'nde tanımlandığı şekilde yeniden kullanıcı aracısına geri yüklenir, yani Microsoft ve taşıyıcı ağlar arasındaki çağrıları işlerken codec bileşenleri çevirebilir. Azure Iletişim Hizmetleri sinyal denetleyicisi, Microsoft 'un aynı RFC 'ye göre bir SIP Proxy uygulamasıdır.

Grup çağrılarında, medya ve sinyal her zaman Azure Iletişim Hizmetleri arka ucu aracılığıyla akar. Tüm katılımcılara ait ses ve/veya video, medya Işlemcisi bileşeninde karıştırılır. Bir grup çağrısının tüm üyeleri, ses ve/veya video akışlarını karışık medya akışları döndüren medya işlemcisine gönderir.

Grup çağrıları için varsayılan gerçek zamanlı protokol (RTP) Kullanıcı Datagram Protokolü (UDP) ' dir.

> [!NOTE]
> Medya Işlemcisi bir MultiPoint denetim birimi (MCU) veya seçmeli Iletme birimi (SFU) işlevi görebilir

:::image type="content" source="./media/call-flows/about-voice-group-calls.png" alt-text="Iletişim Hizmetleri içindeki UDP medya işlem akışını gösteren diyagram.":::

İstemci kitaplığı, güvenlik duvarı kısıtlamaları nedeniyle medya için UDP kullanamıyorum, Iletim Denetim Protokolü 'Nü (TCP) kullanmak için bir girişimde bulunuldu. Medya Işlemcisi bileşeninin UDP gerektirdiğini unutmayın. Bu durumda, Iletişim Hizmetleri çevirme hizmeti, TCP ile UDP 'ye çevirmek için Grup çağrısına eklenecektir. ETKINLEŞTIRME özellikleri el ile devre dışı bırakılmadığı takdirde, bu durumda ücretleri ETKINLEŞTIRIN.

:::image type="content" source="./media/call-flows/about-voice-group-calls-2.png" alt-text="Iletişim Hizmetleri içindeki TCP medya işlem akışını gösteren diyagram.":::

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Çağırma ile çalışmaya başlayın](../quickstarts/voice-video-calling/getting-started-with-calling.md)

Aşağıdaki belgeler sizin için ilginç olabilir:

- [Çağrı türleri](../concepts/voice-video-calling/about-call-types.md) hakkında daha fazla bilgi edinin
- [İstemci-sunucu mimarisi](./client-and-server-architecture.md) hakkında bilgi edinin
