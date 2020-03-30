---
title: Azure IoT Hub ileti yönlendirmeyi anlama | Microsoft Dokümanlar
description: Geliştirici kılavuzu - aygıttan buluta iletigöndermek için ileti yönlendirmenin nasıl kullanılacağı. Hem telemetri hem de telemetri dışı verileri gönderme hakkında bilgi içerir.
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: asrastog
ms.openlocfilehash: d10744f2536cdf89115cdccd0bea6f1e5155774c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79370466"
---
# <a name="use-iot-hub-message-routing-to-send-device-to-cloud-messages-to-different-endpoints"></a>Aygıttan buluta iletileri farklı uç noktalara göndermek için IoT Hub ileti yönlendirmesini kullanma

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

İleti yönlendirme, aygıtlarınızdan bulut hizmetlerine otomatik, ölçeklenebilir ve güvenilir bir şekilde ileti göndermenize olanak tanır. İleti yönlendirmesi için kullanılabilir: 

* **Aygıt telemetri iletilerinin yanı sıra olaylar,** aygıt yaşam döngüsü olayları ve aygıt ikiz değiştirme olaylarını yerleşik bitiş noktası ve özel uç noktalara gönderme. Uç [noktaları yönlendirme](#routing-endpoints)hakkında bilgi edinin.

* Zengin sorgular uygulayarak **verileri çeşitli uç noktalara yönlendirmeden önce filtreleme.** İleti yönlendirme, ileti özellikleri ve ileti gövdesinin yanı sıra aygıt ikiz etiketleri ve aygıt ikiz özelliklerini sorgulamanızı sağlar. [İleti yönlendirmede sorguları](iot-hub-devguide-routing-query-syntax.md)kullanma hakkında daha fazla bilgi edinin.

IoT Hub'ın ileti yönlendirmenin çalışması için bu hizmet uç noktalarına yazma erişimi ne zaman olması gerekir. Uç noktalarınızı Azure portalı üzerinden yapılandırırsanız, sizin için gerekli izinler eklenir. Hizmetlerinizi beklenen iş buzunu destekleyecek şekilde yapılandırdığınızdan emin olun. Örneğin, Olay Hub'larını özel bir bitiş noktası olarak kullanıyorsanız, ioT Hub ileti yönlendirmesi yoluyla göndermeyi planladığınız olayların girişini işletebilmek için söz konusu olay hub'ı için **iş akışı birimlerini** yapılandırmanız gerekir. Benzer şekilde, bir Hizmet Veri Servisi Kuyruğu'nu bitiş noktası olarak kullanırken, sıranın tüketiciler tarafından çıkışa gelene kadar tüm verileri tutabileceğinden emin olmak için **en büyük boyutu** yapılandırmanız gerekir. IoT çözümünüzü ilk yapılandırdığınızda, ek uç noktalarınızı izlemeniz ve gerçek yük için gerekli ayarlamaları yapmanız gerekebilir.

IoT Hub, protokoller arasında birlikte çalışabilirlik için tüm aygıttan buluta iletiler için ortak bir [biçim](iot-hub-devguide-messages-construct.md) tanımlar. İleti aynı bitiş noktasına işaret eden birden çok rotayla eşleşirse, IoT Hub bu bitiş noktasına yalnızca bir kez ileti sunar. Bu nedenle, Servis Veri Servisi kuyruğunuzda veya konunuzda çoğaltma yapılandırmanız gerekmez. Bölümlenmiş kuyruklarda, bölüm afinite ileti siparişi garanti eder. İleti yönlendirmeyi nasıl [yapılandıracağımıöğrenmek](tutorial-routing.md)için bu öğreticiyi kullanın.

## <a name="routing-endpoints"></a>Yönlendirme uç noktaları

IoT hub'ı, Olay Hub'larıyla uyumlu varsayılan dahili uç nokta **(iletiler/olaylar)** vardır. Aboneliğinizdeki diğer hizmetleri IoT Hub'ına bağlayarak iletileri yönlendirmek için [özel uç noktalar](iot-hub-devguide-endpoints.md#custom-endpoints) oluşturabilirsiniz. 

Her ileti, yönlendirme sorguları eşleştiği tüm uç noktalara yönlendirilir. Başka bir deyişle, bir ileti birden çok uç noktaya yönlendirilebilir.

IoT Hub şu anda özel uç noktaları olarak aşağıdaki hizmetleri destekler:

### <a name="built-in-endpoint"></a>Yerleşik bitiş noktası

Yerleşik uç noktadan **(iletiler/olaylar)** aygıttan buluta iletiler almak için standart [Olay Hub'ları tümleştirmesini ve SDK'ları](iot-hub-devguide-messages-read-builtin.md) kullanabilirsiniz. Bir Rota oluşturulduktan sonra, bu uç noktaya bir Rota oluşturulmadığı sürece, veriler yerleşik uç noktaya akmayı durdurur.

### <a name="azure-storage"></a>Azure Storage

IoT Hub'ın iletileri [Azure Blob Depolama](../storage/blobs/storage-blobs-introduction.md) ve [Azure Veri Gölü Depolama Gen2](../storage/blobs/data-lake-storage-introduction.md) (ADLS Gen2) hesaplarına yönlendirebileceği iki depolama hizmeti vardır. Azure Veri Gölü Depolama hesapları, blob depolamasının üstüne kurulmuş [hiyerarşik ad alanı](../storage/blobs/data-lake-storage-namespace.md)etkin depolama hesaplarıdır. Bu kullanım lekeleri her ikisi de kendi depolama için.

IoT Hub, [Apache Avro](https://avro.apache.org/) formatında ve JSON formatında Azure Depolama'ya veri yazmayı destekler. Varsayılan avrodur. Kodlama biçimi yalnızca blob depolama bitiş noktası yapılandırıldığında ayarlanabilir. Biçim varolan bir bitiş noktası için düzenlenemez. JSON kodlama kullanırken, ileti [sistemi özelliklerinde](iot-hub-devguide-routing-query-syntax.md#system-properties)contentType'ı **uygulama/json** ve contentEncoding'i **UTF-8** olarak ayarlamanız gerekir. Bu değerlerin her ikisi de büyük/küçük harf duyarsızdır. İçerik kodlaması ayarlanmazsa, IoT Hub iletileri temel 64 kodlanmış biçimde yazar. IoT Hub Oluştur veya REST API'yi, özellikle [RoutingStorageContainerProperties,](https://docs.microsoft.com/rest/api/iothub/iothubresource/createorupdate#routingstoragecontainerproperties)Azure portalı, [Azure CLI](https://docs.microsoft.com/cli/azure/iot/hub/routing-endpoint?view=azure-cli-latest)veya [Azure PowerShell'i](https://docs.microsoft.com/powershell/module/az.iothub/add-aziothubroutingendpoint?view=azps-1.3.0)kullanarak kodlama biçimini seçebilirsiniz. Aşağıdaki diyagram, Azure portalında kodlama biçiminin nasıl seçilebildiğini gösterir.

![Blob depolama uç nokta kodlama](./media/iot-hub-devguide-messages-d2c/blobencoding.png)

IoT Hub, toplu iş belirli bir boyuta ulaştığında veya belirli bir süre geçtiğinde iletileri toplu olarak ve depolama alanına veri yazar. IoT Hub varsayılan olarak aşağıdaki dosya adlandırma kuralına göre olur: 

```
{iothub}/{partition}/{YYYY}/{MM}/{DD}/{HH}/{mm}
```

Herhangi bir dosya adlandırma kuralı nı kullanabilirsiniz, ancak listelenen tüm belirteçleri kullanmanız gerekir. IoT Hub, yazacak veri yoksa boş bir blob'a yazar.

Tüm blob'ların veya dosyaların bölümleme varsayımı yapmadan okunduklarından emin olmak için lekeleri veya dosyaları listelemenizi ve sonra bunların üzerine tekrarlamanızı öneririz. Bölüm aralığı, Microsoft tarafından [başlatılan bir failover](iot-hub-ha-dr.md#microsoft-initiated-failover) veya IoT Hub [el ile başarısız](iot-hub-ha-dr.md#manual-failover)lık sırasında değişebilir. Dosya listesi için blobs listesini veya [ADLS Gen2 API](https://docs.microsoft.com/rest/api/storageservices/datalakestoragegen2/path/list) listesini listelemek için [Liste Blobs API'yi](https://docs.microsoft.com/rest/api/storageservices/list-blobs) kullanabilirsiniz. Lütfen aşağıdaki örneği kılavuz olarak görebilirsiniz.

```csharp
public void ListBlobsInContainer(string containerName, string iothub)
{
    var storageAccount = CloudStorageAccount.Parse(this.blobConnectionString);
    var cloudBlobContainer = storageAccount.CreateCloudBlobClient().GetContainerReference(containerName);
    if (cloudBlobContainer.Exists())
    {
        var results = cloudBlobContainer.ListBlobs(prefix: $"{iothub}/");
        foreach (IListBlobItem item in results)
        {
            Console.WriteLine(item.Uri);
        }
    }
}
```

> [!NOTE]
> Depolama hesabınızda IoT Hub'ın bağlantısını kısıtlayan güvenlik duvarı yapılandırmaları varsa, [Microsoft'un güvendiği birinci taraf özel durumu](./virtual-network-support.md#egress-connectivity-to-storage-account-endpoints-for-routing) kullanmayı düşünün (yönetilen hizmet kimliğine sahip IoT hub'ları için belirli bölgelerde kullanılabilir).

Azure Veri Gölü Gen2 uyumlu bir depolama hesabı oluşturmak için, yeni bir V2 depolama hesabı oluşturun ve Aşağıdaki resimde gösterildiği gibi **Gelişmiş** sekmesinde *Hiyerarşik ad alanı* alanında *etkin* leştirilmiş bir seçim seçin:

![Azure Date Lake Gen2 depolama alanını seçin](./media/iot-hub-devguide-messages-d2c/selectadls2storage.png)


### <a name="service-bus-queues-and-service-bus-topics"></a>Servis Veri Günü Kuyrukları ve Servis Otobüsü Konuları

Servis Veri Hizmetleri kuyrukları ve IoT Hub uç noktaları olarak kullanılan konular **Oturumlar** veya **Yinelenen Algılama** etkin olmamalıdır. Bu seçeneklerden biri etkinleştirilirse, bitiş noktası Azure portalında **erişilemez** olarak görünür.

> [!NOTE]
> Hizmet veri yolu kaynağınızda IoT Hub'ın bağlantısını kısıtlayan güvenlik duvarı yapılandırmaları varsa, [Microsoft'a güvenilen birinci taraf özel durumu](./virtual-network-support.md#egress-connectivity-to-service-bus-endpoints-for-routing) (yönetilen hizmet kimliğine sahip IoT hub'ları için belirli bölgelerde kullanılabilir) kullanmayı düşünün.


### <a name="event-hubs"></a>Event Hubs

Yerleşik Olay Hub'ları uyumlu bitiş noktasının yanı sıra, verileri olay hub'larının özel uç noktalarına da yönlendirebilirsiniz. 

> [!NOTE]
> Olay hub'ları kaynağınızda IoT Hub'ın bağlantısını kısıtlayan güvenlik duvarı yapılandırmaları varsa, [Microsoft'a güvenilen birinci taraf özel durumu](./virtual-network-support.md#egress-connectivity-to-event-hubs-endpoints-for-routing) kullanmayı düşünün (yönetilen hizmet kimliğine sahip IoT hub'ları için belirli bölgelerde kullanılabilir).


## <a name="reading-data-that-has-been-routed"></a>Yönlendirilmiş verileri okuma

Bu [öğreticiyi](tutorial-routing.md)izleyerek bir rotayı yapılandırabilirsiniz.

İletiyi bitiş noktasından okumayı öğrenmek için aşağıdaki öğreticileri kullanın.

* [Dahili uç noktadan](quickstart-send-telemetry-node.md) okuma

* [Blob depolamadan](../storage/blobs/storage-blob-event-quickstart.md) okuma

* Etkinlik [Hub'larından](../event-hubs/event-hubs-dotnet-standard-getstarted-send.md) Okuma

* Servis [Veri Otobüsü Kuyruklarından](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md) Okuma

* Servis [Veri Günü Konularından](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions) Okuyun


## <a name="fallback-route"></a>Geri dönüş rotası

Geri dönüş rotası, varolan yolların hiçbirindeki sorgu koşullarını karşılamayan tüm iletileri [Olay Hub'larıyla](/azure/event-hubs/)uyumlu yerleşik Olay Hub'larına **(iletiler/olaylar)** gönderir. İleti yönlendirmesi açıksa, geri dönüş rotası özelliğini etkinleştirebilirsiniz. Bir rota oluşturulduktan sonra, bu uç noktaya bir rota oluşturulmadığı sürece, veri yerleşik uç noktaya akmayı durdurur. Yerleşik bitiş noktasına giden yol yoksa ve geri dönüş rotası etkinse, yalnızca rotalardaki sorgu koşullarıyla eşleşmeyan iletiler yerleşik bitiş noktasına gönderilir. Ayrıca, varolan tüm rotalar silinirse, tüm verileri dahili uç noktadan almak için geri dönüş rotasının etkinleştirilmesi gerekir.

Azure portal->İleti Yönlendirme bıçağındaki geri dönüş rotasını etkinleştirebilir/devre dışı kullanabilirsiniz. [FallbackRouteProperties](/rest/api/iothub/iothubresource/createorupdate#fallbackrouteproperties) için Azure Kaynak Yöneticisi'ni geri dönüş rotası için özel bir bitiş noktası kullanmak için de kullanabilirsiniz.

## <a name="non-telemetry-events"></a>Telemetri dışı olaylar

Cihaz telemetrisine ek olarak, ileti yönlendirme, aygıta ikiz değişiklik olayları, aygıt yaşam döngüsü etkinlikleri ve dijital ikiz değiştirme olayları (genel önizlemede) gönderilmesini de sağlar. Örneğin, **aygıt ikiz değiştirme olayları**için veri kaynağı kümesi ile bir rota oluşturulursa, IoT Hub aygıt ikizindeki değişikliği içeren bitiş noktasına iletigönderir. Benzer şekilde, **aygıt yaşam döngüsü olaylarına**veri kaynağı kümesi yle bir rota oluşturulursa, IoT Hub aygıtın silinip silinmediğini veya oluşturulmadığını belirten bir ileti gönderir. Son olarak, [IoT Tak ve Oyna genel önizlemesinin](../iot-pnp/overview-iot-plug-and-play.md)bir parçası olarak, bir geliştirici **dijital ikiz değiştirme olaylarına** veri kaynağı seti ile rotalar oluşturabilir ve IoT Hub dijital bir ikiz [özelliği](../iot-pnp/iot-plug-and-play-glossary.md) ayarlandığında veya değiştirildiğinde, [dijital ikiz](../iot-pnp/iot-plug-and-play-glossary.md) değiştirildiğinde veya altta yatan aygıt ikizi için bir değişiklik olayı olduğunda ileti gönderir.

[IoT Hub,](iot-hub-event-grid.md) gerçek zamanlı tümleştirmeleri ve bu olaylara dayalı iş akışlarının otomasyonunu desteklemek için aygıt olaylarını yayımlamak için Azure Event Grid ile de entegre dir. Senaryonuz için en uygun olanı öğrenmek için [ileti yönlendirme sağlama ve Olay Izgarası arasındaki](iot-hub-event-grid-routing-comparison.md) temel farklara bakın.

## <a name="testing-routes"></a>Test rotaları

Yeni bir rota oluşturduğunuzda veya varolan bir rotayı düzenleyicirken, rota sorgusunu örnek bir iletiyle sınamalısınız. Tek tek yolları sınayabilir veya tüm yolları aynı anda test edebilirsiniz ve test sırasında hiçbir ileti uç noktalara yönlendirilmemektedir. Azure portalı, Azure Kaynak Yöneticisi, Azure PowerShell ve Azure CLI sınama için kullanılabilir. Sonuçlar, örnek iletinin sorguyla eşleşip eşleşmediğini, iletinin sorguyla eşleşip eşleşmediğini veya örnek ileti veya sorgu sözdizimi yanlış olduğundan testin çalıştırılamadığını belirlemeye yardımcı olur. Daha fazla bilgi için Test [Rotası](/rest/api/iothub/iothubresource/testroute) ve [Tüm Yolları Test](/rest/api/iothub/iothubresource/testallroutes)Etme'ye bakın.

## <a name="ordering-guarantees-with-at-least-once-delivery"></a>En az bir kez teslimat ile sipariş garantileri

IoT Hub ileti yönlendirme, sipariş edilen ve en az bir kez iletilerin uç noktalara teslim ini garanti eder. Bu, yinelenen iletiler olabileceği ve bir dizi iletinin özgün ileti siparişini onurlandırarak yeniden iletilebildiği anlamına gelir. Örneğin, özgün ileti sırası [1,2,3,4] ise, [1,2,1,2,3,1,2,3,4] gibi bir ileti dizisi alabilirsiniz. Sipariş garantisi, eğer mesaj alırsanız [1], bunu her zaman [2,3,4] takip edeceğidir.

İleti yinelemelerini işlemek için, genellikle bir aygıt veya modül olan iletinin başlangıç noktasındaki uygulama özelliklerinde benzersiz bir tanımlayıcıdamgalamanızı öneririz. İletileri tüketen hizmet, bu tanımlayıcıyı kullanarak yinelenen iletileri işleyebilir.

## <a name="latency"></a>Gecikme süresi

Yerleşik uç noktaları kullanarak aygıttan buluta telemetri iletilerini yönlendirdiğinizde, ilk rotaoluşturulduktan sonra uçtan uca gecikme de hafif bir artış olur.

Çoğu durumda, gecikme ortalama artış az 500 ms. Yönlendirme'yi kullanarak gecikmeyi **izleyebilirsiniz: iletiler/olaylar için ileti gecikmesi** veya **d2c.endpoints.latency.builtIn.events** IoT Hub ölçümü. İlkinden sonra herhangi bir rota oluşturma veya silme, uçtan uca gecikmeyi etkilemez.

## <a name="monitoring-and-troubleshooting"></a>İzleme ve sorun giderme

IoT Hub, hub'ınızın ve gönderilen iletilerin durumu hakkında genel bir bakış sağlamak için yönlendirme ve uç noktalarla ilgili çeşitli ölçümler sağlar. Sorunların temel nedenini belirlemek için birden çok ölçümdeki bilgileri birleştirebilirsiniz. Örneğin, metrik **Yönlendirme'yi kullanın: telemetri iletileri bırakılan** veya **d2c.telemetry.egress.herhangi** bir rotadaki sorguları eşleştirmedikleri ve geri dönüş rotası devre dışı bırakıldığında bırakılan ileti lerin sayısını belirlemek için bırakılan iletiler devre dışı bırakıldı. [IoT Hub ölçümleri, IoT Hub'ınız](iot-hub-metrics.md) için varsayılan olarak etkinleştirilen tüm ölçümleri listeler.

Endpoints [sağlık durumunu](iot-hub-devguide-endpoints.md#custom-endpoints) almak için REST API [Get Endpoint Health'i](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) kullanabilirsiniz. Uç nokta durumu ölü veya sağlıksız olduğunda hataları tanımlamak ve hata ayıklamak için yönlendirme iletisi gecikmesi ile ilgili [IoT Hub ölçümlerini](iot-hub-metrics.md) kullanmanızı öneririz. Örneğin, uç nokta türü Olay Hub'ları için **d2c.endpoints.latency.eventHubs'ı**izleyebilirsiniz. IoT Hub sonunda tutarlı bir sağlık durumu oluşturduğunda sağlıksız bir bitiş noktasının durumu sağlıklı olarak güncelleştirilir.

Azure Monitor [tanılama ayarlarındaki](../iot-hub/iot-hub-monitor-resource-health.md) **yolları** tanılama günlemesini kullanarak, örneğin bir bitiş noktası ölüolduğunda, ioT Hub tarafından algılanan yönlendirme sorgusunun ve uç nokta durumunun değerlendirilmesi sırasında oluşan hataları izleyebilirsiniz. Bu tanılama günlükleri, özel işleme için Azure Monitor günlüklerine, Etkinlik Hub'larına veya Azure Depolama'ya gönderilebilir.

## <a name="next-steps"></a>Sonraki adımlar

* İleti Yolları'nı nasıl oluşturabilirsiniz öğrenmek [için, yolları kullanarak İşlem IoT Hub aygıttan buluta iletilere](tutorial-routing.md)bakın.

* [Aygıttan buluta iletigönderme](quickstart-send-telemetry-node.md)

* Aygıttan buluta ileti göndermek için kullanabileceğiniz SDK'lar hakkında bilgi için [Azure IoT SDK'larına](iot-hub-devguide-sdks.md)bakın.
