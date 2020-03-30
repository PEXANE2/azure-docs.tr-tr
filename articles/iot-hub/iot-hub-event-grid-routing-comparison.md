---
title: Olay Izgarakarşılaştırın, IoT Hub için yönlendirme | Microsoft Dokümanlar
description: IoT Hub kendi ileti yönlendirme hizmetini sunar, ancak etkinlik yayımlama için Event Grid ile tümleşir. İki özelliği karşılaştırın.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: kgremban
ms.openlocfilehash: 877467b65d346c871dd93f4b3f96b2c1664fa4b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73906797"
---
# <a name="compare-message-routing-and-event-grid-for-iot-hub"></a>IoT Hub için ileti yönlendirme ve Olay Izgara'yı karşılaştırın

Azure IoT Hub, bağlı aygıtlarınızdan veri akışı sağlama ve bu verileri iş uygulamalarınıza entegre etme olanağı sağlar. IoT Hub, IoT etkinliklerini diğer Azure hizmetlerine veya iş uygulamalarına entegre etmek için iki yöntem sunar. Bu makalede, senaryonuz için en iyi seçeneği seçebilmeniz için bu özelliği sağlayan iki özellik anlatılmaktadır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

**[IoT Hub ileti yönlendirme](iot-hub-devguide-messages-d2c.md)**: Bu IoT Hub özelliği, kullanıcıların aygıttan buluta iletileri Azure Depolama kapsayıcıları, Olay Hub'ları, Hizmet Veri Yolu kuyrukları ve Servis Veri Yolu konuları gibi hizmet bitiş noktalarına yönlendirmelerine olanak tanır. Yönlendirme, verileri uç noktalara yönlendirmeden önce filtrelemek için bir sorgu lama yeteneği de sağlar. Aygıt telemetri verilerine ek olarak, eylemleri tetiklemek için kullanılabilecek [telemetri dışı olaylar](iot-hub-devguide-messages-d2c.md#non-telemetry-events) da gönderebilirsiniz. 

**Olay Izgaraile IoT Hub tümleştirmesi**: Azure Event Grid, yayımlama-abone etme modelini kullanan tam olarak yönetilen bir olay yönlendirme hizmetidir. IoT Hub ve Olay Ağı, [IoT Hub olaylarını](iot-hub-event-grid.md)azure ve Azure olmayan hizmetlere neredeyse gerçek zamanlı olarak entegre etmek için birlikte çalışır. IoT Hub hem [aygıt olayları](iot-hub-event-grid.md#event-types) hem de telemetri olaylarını yayımlar.

## <a name="differences"></a>Farklılıklar

Hem ileti yönlendirme hem de Olay Izgara uyarı yapılandırmasını etkinleştirirken, ikisi arasında bazı önemli farklar vardır. Ayrıntılar için aşağıdaki tabloya bakın:

| Özellik | IoT Hub ileti yönlendirme | Olay Izgara ile IoT Hub tümleştirmesi |
| ------- | --------------- | ---------- |
| **Cihaz iletileri ve olayları** | Evet, ileti yönlendirme selemetri verileri, rapor aygıtı ikiz değişiklikleri, aygıt yaşam döngüsü olayları ve dijital ikiz değiştirme olayları [(IoT Tak ve Genel Önizleme'nin](../iot-pnp/overview-iot-plug-and-play.md)bir parçası) için kullanılabilir. | Evet, Olay Izgarası telemetri verileri için kullanılabilir, ancak aygıtlar oluşturulduğunda, silindiğinde, bağlandığında ve IoT Hub'ından bağlantısı kesildiğinde de rapor lanabilir |
| **Sıralama** | Evet, olayların sıralanması korunur.  | Hayır, olayların sırası garanti değildir. | 
| **Filtreleme** | İleti uygulama özellikleri, ileti sistemi özellikleri, ileti gövdesi, aygıt ikiz etiketleri ve aygıt ikiz özellikleri üzerinde zengin filtreleme. Filtreleme, dijital ikiz değiştirme olaylarına uygulanmaz. Örneğin, [bkz.](iot-hub-devguide-routing-query-syntax.md) | Her olaydaki olay türüne, konu türüne ve özniteliklerine göre filtreleme. Örneğin, [Olay Izgara Abonelikleri'ndeki filtreleme olaylarını anlayın.](../event-grid/event-filtering.md) Telemetri olaylarına abone olduğunuzda, Olay İzi'ni yayımlamadan önce, IoT Hub'ınızdaki ileti özelliklerine, ileti gövdesine ve aygıt ikizlerine filtre uygulamak için verilere ek filtreler uygulayabilirsiniz. [Olayları nasıl filtreleyeceklerini](../iot-hub/iot-hub-event-grid.md#filter-events)görün. |
| **Uç Noktalar** | <ul><li>Event Hubs</li> <li>Azure Blob Depolama</li> <li>Service Bus kuyruğu</li> <li>Servis Veri Günü konuları</li></ul><br>Ücretli IoT Hub SKUs 'ları (S1, S2 ve S3) 10 özel uç noktaile sınırlıdır. IoT Hub başına 100 rota oluşturulabilir. | <ul><li>Azure İşlevleri</li> <li>Azure Otomasyonu</li> <li>Event Hubs</li> <li>Logic Apps</li> <li>Depolama Blobu</li> <li>Özel Konu Başlıkları</li> <li>Kuyruk Depolama</li> <li>Microsoft Flow</li> <li>WebHooks üzerinden üçüncü taraf hizmetleri</li></ul><br>IoT Hub başına 500 uç nokta desteklenir. En güncel uç noktaları listesi için Olay [Izgara olay işleyicilerine](../event-grid/overview.md#event-handlers)bakın. |
| **Maliyet** | İleti yönlendirme için ayrı bir ücret alınmaz. Sadece IoT Hub'a telemetri girişi ücretlendirilir. Örneğin, üç farklı uç noktaya yönlendirilmiş bir iletiniz varsa, yalnızca bir ileti için faturalandırılırsınız. | IoT Hub'dan ücret alınmaz. Event Grid, ayda ilk 100.000 operasyonu ücretsiz olarak ve daha sonra milyon işlem başına $0,60 sunar. |

## <a name="similarities"></a>Benzerlik

IoT Hub ileti yönlendirme ve Olay Grid de benzerlikler var, bazıları aşağıdaki tabloda ayrıntılı olan:

| Özellik | IoT Hub ileti yönlendirme | Olay Izgara ile IoT Hub tümleştirmesi |
| ------- | --------------- | ---------- |
| **Maksimum ileti boyutu** | 256 KB, cihazdan buluta | 256 KB, cihazdan buluta |
| **Güvenilirlik** | Yüksek: Her iletiyi her rota için en az bir kez uç noktasına iletin. Bir saat içinde teslim edilmeyen tüm iletilerin süresi doluyor. | Yüksek: Her iletiyi her abonelik için en az bir kez webhook'a iletin. 24 saat içinde teslim edilmeyen tüm olayların süresi doluyor. | 
| **Ölçeklenebilir -lik** | Yüksek: Milyarlarca ileti gönderen aynı anda bağlı milyonlarca cihazı desteklemek için optimize edin. | Yüksek: Bölge başına saniyede 10.000.000 olay yönlendirme yeteneğine sahiptir. |
| **Gecikme süresi** | Düşük: Neredeyse gerçek zamanlı. | Düşük: Neredeyse gerçek zamanlı. |
| **Birden çok uç noktaya gönderme** | Evet, birden çok uç noktaya tek bir ileti gönderin. | Evet, birden çok uç noktaya tek bir ileti gönderin.  
| **Güvenlik** | Iot Hub cihaz başına kimlik ve geri alınabilir erişim denetimi sağlar. Daha fazla bilgi için [IoT Hub erişim denetimine](iot-hub-devguide-security.md)bakın. | Olay Izgara üç noktada doğrulama sağlar: olay abonelikleri, olay yayımlama ve webhook olay teslim. Daha fazla bilgi için [Olay Izgara güvenliği ve kimlik doğrulaması'na](../event-grid/security-authentication.md)bakın. |

## <a name="how-to-choose"></a>Nasıl seçilir?

IoT Hub ileti yönlendirme ve Olay Grid ile IoT Hub tümleştirme benzer sonuçlar elde etmek için farklı eylemler gerçekleştirin. Her ikisi de IoT Hub çözümünden bilgi alır ve diğer hizmetlerin tepki verebilmeleri için aktarabilir. Peki hangisini kullanacağınıza nasıl karar verebilirsiniz? Kararınızı yönlendirmek için aşağıdaki soruları göz önünde bulundurun: 

* **Uç noktalara ne tür veriler gönderiyorsunuz?**

   Diğer hizmetlere telemetri verileri göndermeniz gerektiğinde IoT Hub ileti yönlendirmesini kullanın. İleti yönlendirme, ileti uygulaması nın ve sistem özelliklerinin, ileti gövdesinin, aygıt ikiz etiketlerinin ve aygıt ikiz özelliklerinin sorgulanmasını da sağlar.

   Olay Grid ile IoT Hub tümleştirmesi, IoT Hub hizmetinde meydana gelen olaylarla çalışır. Bu IoT Hub olayları telemetri verilerini, oluşturulan aygıtı, silinini, bağlanmasını ve bağlantısını kesmeyi içerir. Telemetri olaylarına abone olduğunuzda, Olay İzi'ni yayımlamadan önce, IoT Hub'ınızdaki ileti özelliklerine, ileti gövdesine ve aygıt ikizlerine filtre uygulamak için verilere ek filtreler uygulayabilirsiniz. [Olayları nasıl filtreleyeceklerini](../iot-hub/iot-hub-event-grid.md#filter-events)görün.

* **Bu bilgileri almak için hangi uç noktalara ihtiyaç vardır?**

   IoT Hub ileti yönlendirmesi sınırlı sayıda benzersiz uç nokta ve uç nokta türünü destekler, ancak verileri ve olayları ek uç noktalara yönlendirmek için bağlayıcılar oluşturabilirsiniz. Desteklenen uç noktaların tam listesi için önceki bölümdeki tabloya bakın. 

   Event Grid ile IoT Hub tümleştirmesi, IoT Hub'ı başına 500 uç noktayı ve daha çeşitli uç nokta türlerini destekler. Azure İşlevleri, Mantıksal Uygulamalar, Depolama ve Hizmet Veri Hizmeti kuyruklarıyla yerel olarak bütünleşir ve ayrıca Azure hizmet ekosisteminin dışına ve üçüncü taraf iş uygulamalarına veri göndermeyi genişletmek için webhook'larla birlikte çalışır.

* **Verilerinizin sırayla gelmesi önemli mi?**

   IoT Hub ileti yönlendirmesi, iletilerin aynı şekilde ulaşması için iletilerin gönderilme sırasını korur.

   Olay Izgarası, uç noktalarının olayları oluştukları sırada alacağını garanti etmez. İletilerin mutlak sırasının önemli olduğu ve/veya tüketicinin iletiler için güvenilir benzersiz bir tanımlayıcıya ihtiyaç duyduğu durumlarda ileti yönlendirmesini kullanmanızı öneririz. 

## <a name="next-steps"></a>Sonraki adımlar

* [IoT Hub İleti Yönlendirme](iot-hub-devguide-messages-d2c.md) ve [IoT Hub uç noktaları](iot-hub-devguide-endpoints.md)hakkında daha fazla bilgi edinin.
* [Azure Event Grid](../event-grid/overview.md) hakkında daha fazla bilgi edinin.
* İleti Yolları'nı nasıl oluşturacağımı öğrenmek için, rotalar öğreticisini [kullanarak İşlem IoT Hub'ı aygıttan buluta iletilere](../iot-hub/tutorial-routing.md) bakın.
* [Logic Apps'ı kullanarak Azure IoT Hub olayları hakkında e-posta bildirimleri göndererek](../event-grid/publish-iot-hub-events-to-logic-apps.md)Olay Ağı tümleştirmesini deneyin.
