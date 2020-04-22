---
title: Azure IoT Hub kimlik kayıt defterini anlama | Microsoft Dokümanlar
description: Geliştirici kılavuzu - IoT Hub kimlik kayıt defterinin açıklaması ve aygıtlarınızı yönetmek için nasıl kullanılacağı. Aygıt kimliklerinin toplu olarak içe aktarılaması ve dışa aktarımı hakkında bilgi içerir.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/29/2018
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 2ef259bf76815fdf8672b696d2260fe6a143b798
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81730167"
---
# <a name="understand-the-identity-registry-in-your-iot-hub"></a>IoT hub'ınızdaki kimlik kayıt defterini anlama

Her IoT hub'ı, IoT hub'ına bağlanmasına izin verilen aygıtlar ve modüller hakkında bilgi depolayan bir kimlik kayıt defterine sahiptir. Bir aygıt veya modülün bir IoT hub'ına bağlanabilmesi için, ioT hub'ın kimlik kayıt defterinde bu aygıt veya modül için bir giriş olması gerekir. Bir aygıt veya modül, kimlik kayıt defterinde depolanan kimlik bilgilerine dayalı olarak IoT hub'ı ile de kimlik doğrulamalıdır.

Kimlik kayıt defterinde depolanan aygıt veya modül kimliği büyük/küçük harf duyarlıdır.

Yüksek düzeyde, kimlik kayıt cihazı veya modül kimlik kaynaklarının REST özellikli bir koleksiyondur. Kimlik kayıt defterine bir giriş eklediğinizde, IoT Hub, uçuş içi buluttan cihaza iletiler içeren kuyruk gibi aygıt başına bir dizi kaynak oluşturur.

Aşağıdakileri yapmanız gerektiğinde kimlik defterini kullanın:

* IoT hub'ınıza bağlanan aygıtları veya modülleri sağlama.
* Hub'ınızın aygıtına veya modüle bakan uç noktalarına cihaz başına/modül başına erişimi kontrol edin.

> [!NOTE]
> * Kimlik kayıt defteri uygulamaya özgü meta veriler içermez.
> * Modül kimliği ve modül ikizi genel önizlemede. Aşağıdaki özellik, genel kullanılabilir olduğunda modül kimliği nde desteklenecektir.
>

## <a name="identity-registry-operations"></a>Kimlik kayıt defteri işlemleri

IoT Hub kimlik kayıt defteri aşağıdaki işlemleri ortaya çıkarır:

* Aygıt veya modül kimliği oluşturma
* Aygıt veya modül kimliğini güncelleştirme
* Cihaz veya modül kimliğini kimlikle alma
* Aygıt veya modül kimliğini silme
* 1000'e kadar kimlik listesi
* Aygıt kimliklerini Azure blob depolamasına dışa aktarma
* Azure blob depolamadan aygıt kimlikleri alma

Tüm bu işlemler [RFC7232'de](https://tools.ietf.org/html/rfc7232)belirtildiği gibi iyimser eşzamanlılık kullanabilir.

> [!IMPORTANT]
> Bir IoT hub'ın kimlik kayıt defterindeki tüm kimlikleri almanın tek yolu [Dışa Aktarma](iot-hub-devguide-identity-registry.md#import-and-export-device-identities) işlevini kullanmaktır.

Bir IoT Hub kimlik kayıt defteri:

* Herhangi bir uygulama meta veri içermez.
* Anahtar olarak **deviceId** veya **moduleId** kullanılarak sözlük gibi erişilebilir.
* Anlamlı sorguları desteklemez.

Bir IoT çözüm genellikle uygulamaya özgü meta verileri içeren ayrı bir çözüme özgü deposu vardır. Örneğin, akıllı bina çözümündeki çözüme özel mağaza, sıcaklık sensörünün devreye saldığı odayı kaydeder.

> [!IMPORTANT]
> Kimlik kaydını yalnızca aygıt yönetimi ve sağlama işlemleri için kullanın. Çalışma zamanındaki yüksek iş elde işlemleri, kimlik kayıt defterindeki işlemleri gerçekleştirmeye bağlı olmamalıdır. Örneğin, komut göndermeden önce aygıtın bağlantı durumunu denetlemek desteklenen bir desen değildir. Kimlik kayıt defteri ve [aygıt sinyal idelesi](iot-hub-devguide-identity-registry.md#device-heartbeat) için [azaltma oranlarını](iot-hub-devguide-quotas-throttling.md) kontrol ettiğinizden emin olun.

## <a name="disable-devices"></a>Cihazları devre dışı bırakma

Kimlik kayıt defterindeki bir kimliğin **durum** özelliğini güncelleştirerek aygıtları devre dışı kullanabilirsiniz. Genellikle, bu özelliği iki senaryoda kullanırsınız:

* Bir provizyon düzenleme işlemi sırasında. Daha fazla bilgi için Bkz. [Aygıt Sağlama.](iot-hub-devguide-identity-registry.md#device-provisioning)

* Herhangi bir nedenle, bir aygıtın ele geçirildiğini veya yetkisiz hale geldiğini düşünüyorsanız.

Bu özellik modüller için kullanılamaz.

## <a name="import-and-export-device-identities"></a>Aygıt kimliklerini alma ve dışa aktarma

Aygıt kimliklerini bir IoT hub'ın kimlik kayıt defterinden toplu olarak dışa aktarmak için [IoT Hub kaynak sağlayıcısı bitiş noktasında](iot-hub-devguide-endpoints.md) eşzamanlı işlemler kullanın. Dışa aktarma, kimlik kayıt defterinden okunan aygıt kimlik verilerini kaydetmek için müşteri tarafından sağlanan blob kapsayıcısını kullanan uzun soluklu işlerdir.

Aygıt kimliklerini toplu olarak Bir IoT hub'ın kimlik kayıt defterine almak için [IoT Hub kaynak sağlayıcısı bitiş noktasında](iot-hub-devguide-endpoints.md) eşzamanlı işlemler kullanın. İçeri aktarma, aygıt kimlik verilerini kimlik defterine yazmak için müşteri tarafından sağlanan blob kapsayıcısındaki verileri kullanan uzun soluklu işlerdir.

Alma ve dışa aktarma API'leri hakkında daha fazla bilgi için [IoT Hub kaynak sağlayıcısı REST API'leri'ne](/rest/api/iothub/iothubresource)bakın. Alma ve dışa aktarma işlerini çalıştırma hakkında daha fazla bilgi edinmek [için, IoT Hub aygıt kimliklerinin toplu yönetimine](iot-hub-bulk-identity-mgmt.md)bakın.

Aygıt kimlikleri, [REST API](/rest/api/iothub/service/jobclient/createimportexportjob) veya IoT Hub [Service SDK'lardan](/azure/iot-hub/iot-hub-devguide-sdks#azure-iot-hub-service-sdks)biri aracılığıyla Hizmet API'si aracılığıyla ioT Hub'dan da dışa aktarılabilir ve içe aktarılabilir.

## <a name="device-provisioning"></a>Cihaz sağlama

Belirli bir IoT çözümünün depoladığınız aygıt verileri, bu çözümün özel gereksinimlerine bağlıdır. Ancak, en az olarak, bir çözüm aygıt kimlikleri ve kimlik doğrulama anahtarları depolamak gerekir. Azure IoT Hub, kimlikler, kimlik doğrulama anahtarları ve durum kodları gibi her aygıt için değerleri depolayabilen bir kimlik kayıt defteri içerir. Bir çözüm, ek aygıt verilerini depolamak için tablo depolama, blob depolama veya Cosmos DB gibi diğer Azure hizmetlerini kullanabilir.

*Aygıt sağlama,* çözümünüzüzdeki mağazalara ilk aygıt verilerini ekleme işlemidir. Hub'ınıza bağlanmak için yeni bir aygıtın etkinleştirmek için IoT Hub kimlik kayıt defterine bir aygıt kimliği ve anahtarları eklemeniz gerekir. Sağlama işleminin bir parçası olarak, diğer çözüm mağazalarında aygıta özgü verileri başlatmanız gerekebilir. İnsan müdahalesine gerek kalmadan bir veya daha fazla IoT hub'ına sıfır dokunuş, tam zamanında sağlama sağlamak için Azure IoT Hub Aygıt Sağlama Hizmetini de kullanabilirsiniz. Daha fazla bilgi edinmek [için, hizmet sağlama belgelerine](https://azure.microsoft.com/documentation/services/iot-dps)bakın.

## <a name="device-heartbeat"></a>Cihaz kalp atışı

IoT Hub kimlik kayıt defteri **connectionState**adlı bir alan içerir. Yalnızca geliştirme ve hata ayıklama sırasında **connectionState** alanını kullanın. IoT çözümleri çalışma zamanında alanı sorgulamamalıdır. Örneğin, buluttan cihaza ileti veya SMS göndermeden önce aygıtın bağlı olup olmadığını kontrol etmek için **connectionState** alanını sorgulamayın. Uyarıları almak ve aygıt bağlantı durumunu izlemek için Olay Izgara'sındaki [ **aygıt bağlantısı kesilen** olaya](iot-hub-event-grid.md#event-types) abone olmanızı öneririz. IoT Hub'ından Aygıt Bağlantısı ve Aygıt Bağlantısı Kesilen olayları IoT çözümünüze nasıl entegre acağınızı öğrenmek için bu [öğreticiyi](iot-hub-how-to-order-connection-state-events.md) kullanın.

IoT çözümünüzün bir aygıtın bağlı olup olmadığını bilmesi gerekiyorsa, *sinyal deseni*uygulayabilirsiniz.
Sinyal deseninde, aygıt her sabit zamanda en az bir kez (örneğin, saatte en az bir kez) aygıttan buluta iletigönderir. Bu nedenle, bir aygıtın gönderilecek verisi olmasa bile, yine de boş bir aygıttan buluta ileti gönderir (genellikle aygıtı sinyal olarak tanımlayan bir özellik ile). Hizmet tarafında, çözüm her aygıt için alınan son sinyalile bir harita tutar. Çözüm aygıttan beklenen süre içinde bir sinyal iletisi almazsa, aygıtla ilgili bir sorun olduğunu varsayar.

Daha karmaşık bir uygulama, bağlanmaya veya iletişim kurmaya çalışan ancak başarısız olan aygıtları tanımlamak için [Azure Monitor](../azure-monitor/index.yml) ve Azure [Kaynak Durumu'ndan](../service-health/resource-health-overview.md) gelen bilgileri içerebilir, [tanılama kılavuzuyla Monitor'u](iot-hub-monitor-resource-health.md) kontrol edin. Sinyal deseni uyguladığınız zaman, [IoT Hub Kotaları ve Throttles](iot-hub-devguide-quotas-throttling.md)kontrol ettiğinizden emin olun.

> [!NOTE]
> Bir IoT çözümü yalnızca buluttan aygıta ileti gönderip göndermemeyi belirlemek için bağlantı durumunu kullanıyorsa ve iletiler büyük aygıt kümelerine yayınlaşmıyorsa, daha basit *kısa son kullanma süresi* modelini kullanmayı düşünün. Bu desen, daha verimli olurken, sinyal deseni kullanarak bir aygıt bağlantısı durumu kayıt defterini korumakla aynı sonucu elde eder. İleti bildirimleri isterseniz, IoT Hub hangi aygıtların ileti alabildiği ve hangilerinin alınamayacağı konusunda sizi bilgilendirebilir.

## <a name="device-and-module-lifecycle-notifications"></a>Cihaz ve modül yaşam döngüsü bildirimleri

IoT Hub, yaşam döngüsü bildirimleri göndererek bir kimlik oluşturulduğunda veya silindiğinde IoT çözümünüzü bildirebilir. Bunu yapmak için, IoT çözümünüzün bir rota oluşturması ve Veri Kaynağını *DeviceLifecycleEvents veya ModuleLifecycleEvents'e* eşit ayarlaması gerekir. *ModuleLifecycleEvents* Varsayılan olarak, hiçbir yaşam döngüsü bildirimleri gönderilir, yani, bu tür yollar önceden var. Bildirim iletisi özellikleri ve gövdeiçerir.

Özellikler: İleti sistemi özellikleri `$` sembolle önceden belirlenmiştir.

Cihaz için bildirim iletisi:

| Adı | Değer |
| --- | --- |
|$content tipi | uygulama/json |
|$iothub-enqueuedtime |  Bildirimin gönderildiği saat |
|$iothub-mesaj-kaynak | deviceLifecycleEvents |
|$content kodlama | utf-8 |
|Optype | **oluşturmaDeviceIdentity** veya **deleteDeviceIdentity** |
|hubName | IoT Hub adı |
|deviceId | Cihazın kimliği |
|operationTimestamp | ISO8601 çalışma zaman damgası |
|iothub-mesaj-şema | cihazYaşam döngüsüBildirimi |

Gövde: Bu bölüm JSON biçimindedir ve oluşturulan aygıt kimliğinin ikizini temsil eder. Örneğin,

```json
{
    "deviceId":"11576-ailn-test-0-67333793211",
    "etag":"AAAAAAAAAAE=",
    "properties": {
        "desired": {
            "$metadata": {
                "$lastUpdated": "2016-02-30T16:24:48.789Z"
            },
            "$version": 1
        },
        "reported": {
            "$metadata": {
                "$lastUpdated": "2016-02-30T16:24:48.789Z"
            },
            "$version": 1
        }
    }
}
```
Modül için bildirim iletisi:

| Adı | Değer |
| --- | --- |
$content tipi | uygulama/json |
$iothub-enqueuedtime |  Bildirimin gönderildiği saat |
$iothub-mesaj-kaynak | modülYaşam DöngüsüEtkinlikler |
$content kodlama | utf-8 |
Optype | **oluşturmaModuleIdentity** veya **silmeModuleIdentity** |
hubName | IoT Hub adı |
moduleId | Modülün kimliği |
operationTimestamp | ISO8601 çalışma zaman damgası |
iothub-mesaj-şema | modülYaşam DöngüsüBildirimi |

Gövde: Bu bölüm JSON formatındadır ve oluşturulan modül kimliğinin ikizini temsil eder. Örneğin,

```json
{
    "deviceId":"11576-ailn-test-0-67333793211",
    "moduleId":"tempSensor",
    "etag":"AAAAAAAAAAE=",
    "properties": {
        "desired": {
            "$metadata": {
                "$lastUpdated": "2016-02-30T16:24:48.789Z"
            },
            "$version": 1
        },
        "reported": {
            "$metadata": {
                "$lastUpdated": "2016-02-30T16:24:48.789Z"
            },
            "$version": 1
        }
    }
}
```

## <a name="device-identity-properties"></a>Aygıt kimlik özellikleri

Aygıt kimlikleri aşağıdaki özelliklere sahip JSON belgeleri olarak temsil edilir:

| Özellik | Seçenekler | Açıklama |
| --- | --- | --- |
| deviceId |gerekli, salt okunur güncelleştirmeleri |ASCII 7-bit alfanümerik karakterlerin yanı sıra belirli özel karakterlerin `- . + % _ # * ? ! ( ) , = @ $ '`büyük/küçük harf duyarlı dizesi (128 karaktere kadar) |
| generationId |gerekli, salt okunur |128 karaktere kadar ioT hub'ı tarafından oluşturulmuş, büyük/küçük harf duyarlı bir dize. Bu değer, silinip yeniden oluşturulduklarında aynı **deviceId'e**sahip aygıtları ayırt etmek için kullanılır. |
| Etag |gerekli, salt okunur |[RFC7232'ye](https://tools.ietf.org/html/rfc7232)göre aygıt kimliği için zayıf bir ETag'ı temsil eden bir dize. |
| Auth |isteğe bağlı |Kimlik doğrulama bilgileri ve güvenlik materyalleri içeren bileşik bir nesne. |
| auth.symkey |isteğe bağlı |Base64 biçiminde depolanan birincil ve ikincil anahtar içeren bileşik nesne. |
| durum |gerekli |Erişim göstergesi. **Etkin** veya **Devre Dışı Tutulabilir.** **Etkinse,** aygıtın bağlanmasına izin verilir. **Devre dışı bırakılırsa,** bu aygıt aygıta bakan herhangi bir uç noktaya erişemez. |
| statusReason |isteğe bağlı |Aygıt kimlik durumunun nedenini depolayan 128 karakter uzunluğundabir dize. Tüm UTF-8 karakterlerine izin verilir. |
| statusUpdateTime |salt okunur |Son durum güncelleştirmesinin tarih ve saatini gösteren geçici bir gösterge. |
| bağlantıDevlet |salt okunur |Bağlantı durumunu gösteren bir alan: **Bağlı** veya **Bağlantısız.** Bu alan, aygıt bağlantı durumunun IoT Hub görünümünü temsil eder. **Önemli**: Bu alan sadece geliştirme/hata ayıklama amacıyla kullanılmalıdır. Bağlantı durumu yalnızca MQTT veya AMQP kullanan aygıtlar için güncelleştirilir. Ayrıca, protokol düzeyinde ping (MQTT ping veya AMQP ping) dayanmaktadır ve sadece 5 dakika maksimum gecikme olabilir. Bu nedenlerden dolayı, bağlı olarak bildirilen ancak bağlantısı kesilen aygıtlar gibi yanlış pozitif sonuçlar olabilir. |
| bağlantıStateUpdatedTime |salt okunur |Bağlantı durumunun güncelleştirilme tarihini ve son kez güncelleştirilmesini gösteren geçici bir gösterge. |
| lastActivityTime |salt okunur |Aygıtın bağlı olduğu, aldığı veya gönderdiği tarihi ve son zamanı gösteren geçici bir gösterge. |

> [!NOTE]
> Bağlantı durumu yalnızca bağlantı durumunun IoT Hub görünümünü temsil edebilir. Bu durumdaki güncelleştirmeler, ağ koşullarına ve yapılandırmalara bağlı olarak gecikebilir.

> [!NOTE]
> Şu anda cihaz SDK'lar `+` `#` **deviceId**ve karakterleri kullanarak destek yok.

## <a name="module-identity-properties"></a>Modül kimlik özellikleri

Modül kimlikleri aşağıdaki özelliklere sahip JSON belgeleri olarak temsil edilir:

| Özellik | Seçenekler | Açıklama |
| --- | --- | --- |
| deviceId |gerekli, salt okunur güncelleştirmeleri |ASCII 7-bit alfanümerik karakterlerin yanı sıra belirli özel karakterlerin `- . + % _ # * ? ! ( ) , = @ $ '`büyük/küçük harf duyarlı dizesi (128 karaktere kadar) |
| moduleId |gerekli, salt okunur güncelleştirmeleri |ASCII 7-bit alfanümerik karakterlerin yanı sıra belirli özel karakterlerin `- . + % _ # * ? ! ( ) , = @ $ '`büyük/küçük harf duyarlı dizesi (128 karaktere kadar) |
| generationId |gerekli, salt okunur |128 karaktere kadar ioT hub'ı tarafından oluşturulmuş, büyük/küçük harf duyarlı bir dize. Bu değer, silinip yeniden oluşturulduklarında aynı **deviceId'e**sahip aygıtları ayırt etmek için kullanılır. |
| Etag |gerekli, salt okunur |[RFC7232'ye](https://tools.ietf.org/html/rfc7232)göre aygıt kimliği için zayıf bir ETag'ı temsil eden bir dize. |
| Auth |isteğe bağlı |Kimlik doğrulama bilgileri ve güvenlik materyalleri içeren bileşik bir nesne. |
| auth.symkey |isteğe bağlı |Base64 biçiminde depolanan birincil ve ikincil anahtar içeren bileşik nesne. |
| durum |gerekli |Erişim göstergesi. **Etkin** veya **Devre Dışı Tutulabilir.** **Etkinse,** aygıtın bağlanmasına izin verilir. **Devre dışı bırakılırsa,** bu aygıt aygıta bakan herhangi bir uç noktaya erişemez. |
| statusReason |isteğe bağlı |Aygıt kimlik durumunun nedenini depolayan 128 karakter uzunluğundabir dize. Tüm UTF-8 karakterlerine izin verilir. |
| statusUpdateTime |salt okunur |Son durum güncelleştirmesinin tarih ve saatini gösteren geçici bir gösterge. |
| bağlantıDevlet |salt okunur |Bağlantı durumunu gösteren bir alan: **Bağlı** veya **Bağlantısız.** Bu alan, aygıt bağlantı durumunun IoT Hub görünümünü temsil eder. **Önemli**: Bu alan sadece geliştirme/hata ayıklama amacıyla kullanılmalıdır. Bağlantı durumu yalnızca MQTT veya AMQP kullanan aygıtlar için güncelleştirilir. Ayrıca, protokol düzeyinde ping (MQTT ping veya AMQP ping) dayanmaktadır ve sadece 5 dakika maksimum gecikme olabilir. Bu nedenlerden dolayı, bağlı olarak bildirilen ancak bağlantısı kesilen aygıtlar gibi yanlış pozitif sonuçlar olabilir. |
| bağlantıStateUpdatedTime |salt okunur |Bağlantı durumunun güncelleştirilme tarihini ve son kez güncelleştirilmesini gösteren geçici bir gösterge. |
| lastActivityTime |salt okunur |Aygıtın bağlı olduğu, aldığı veya gönderdiği tarihi ve son zamanı gösteren geçici bir gösterge. |

> [!NOTE]
> Şu anda cihaz SDK'lar `+` deviceId ve `#` **moduleId** **moduleId**karakterleri kullanarak destek yok.

## <a name="additional-reference-material"></a>Ek referans materyali

IoT Hub geliştirici kılavuzundaki diğer başvuru konuları şunlardır:

* [IoT Hub uç noktaları,](iot-hub-devguide-endpoints.md) her IoT hub'ın çalışma zamanı ve yönetim işlemleri için ortaya çıkardığı çeşitli uç noktaları açıklar.

* [Azaltma ve kotalar,](iot-hub-devguide-quotas-throttling.md) IoT Hub hizmetiiçin geçerli olan kotaları ve azaltma davranışlarını açıklar.

* [Azure IoT aygıt ve hizmet SDK'ları,](iot-hub-devguide-sdks.md) IoT Hub ile etkileşimde bulunan hem aygıt hem de hizmet uygulamaları geliştirdiğinizde kullanabileceğiniz çeşitli dil SDK'larını listeler.

* [IoT Hub sorgu dili, IoT](iot-hub-devguide-query-language.md) Hub'dan aygıtınız ve işleriniz hakkında bilgi almak için kullanabileceğiniz sorgu dilini açıklar.

* [IoT Hub MQTT desteği,](iot-hub-mqtt-support.md) MQTT protokolü için IoT Hub desteği hakkında daha fazla bilgi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Artık IoT Hub kimlik kayıt defterini kullanmayı öğrendiğiniz için aşağıdaki IoT Hub geliştirici kılavuzu konuları ilginizi çekebilir:

* [IoT Hub’a erişimi denetleme](iot-hub-devguide-security.md)

* [Durumu ve yapılandırmaları eşitlemek için aygıt ikizlerini kullanma](iot-hub-devguide-device-twins.md)

* [Aygıtta doğrudan bir yöntem çağırma](iot-hub-devguide-direct-methods.md)

* [Birden fazla cihazda işleri zamanlama](iot-hub-devguide-jobs.md)

Bu makalede açıklanan bazı kavramları denemek için aşağıdaki IoT Hub öğreticisine bakın:

* [Azure IoT Hub ile başlayın](quickstart-send-telemetry-dotnet.md)

Sıfır dokunmayı, tam zamanında sağlamayı etkinleştirmek için IoT Hub Aygıt Sağlama Hizmetini kullanmak için bkz: 

* [Azure IoT Hub Cihazı Sağlama Hizmeti](https://azure.microsoft.com/documentation/services/iot-dps)
