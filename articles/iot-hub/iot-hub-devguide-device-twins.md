---
title: Azure IoT Hub aygıt ikizlerini anlama | Microsoft Dokümanlar
description: Geliştirici kılavuzu - IoT Hub ile aygıtlarınız arasındaki durum ve yapılandırma verilerini senkronize etmek için aygıt ikizlerini kullanın
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/01/2020
ms.custom: mqtt
ms.openlocfilehash: 3bec3d19ed68b7eb8bb50baa8f6c11135ef778cc
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81731460"
---
# <a name="understand-and-use-device-twins-in-iot-hub"></a>IoT Hub'da aygıt ikizlerini anlama ve kullanma

*Aygıt ikizleri,* meta veriler, yapılandırmalar ve koşullar da dahil olmak üzere aygıt durumu bilgilerini depolayan JSON belgeleridir. Azure IoT Hub, IoT Hub'a bağladığınız her cihaz için bir cihaz çifti tutar. 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu makalede açıklanır:

* Cihazın yapısı ikiz: *etiketleri*, *istenen* ve *bildirilen özellikleri*.
* Uygulamaları ve arka uçları aygıtta gerçekleştirebileceğiniz işlemler aygıt ikizlerinde gerçekleştirilebilir.

Aygıt ikizleri kullanarak şunları kullanın:

* Aygıta özgü meta verileri bulutta depolayın. Örneğin, bir otomatın dağıtım konumu.

* Cihazınızdan kullanılabilir özellikler ve koşullar gibi geçerli durum bilgilerini bildirin. Örneğin, bir aygıt Hücresel veya Wi-Fi üzerinden IoT hub'ınıza bağlanır.

* Aygıt uygulaması ve arka uç uygulaması arasında uzun süredir devam eden iş akışlarının durumunu senkronize edin. Örneğin, çözüm arka uç yüklemek için yeni firmware sürümünü belirtir ve cihaz uygulaması güncelleştirme işleminin çeşitli aşamalarını raporlar.

* Aygıt meta verilerinizi, yapılandırmanızı veya durumunuzu sorgulayın.

Bildirilen özellikleri, aygıttan buluta iletileri veya dosya yüklemeyi kullanma konusunda rehberlik etmek için [Aygıttan buluta iletişim kılavuzuna](iot-hub-devguide-d2c-guidance.md) bakın.

İstenilen özellikleri, doğrudan yöntemleri veya buluttan aygıta iletileri kullanma konusunda rehberlik etmek için [buluttan aygıta iletişim kılavuzuna](iot-hub-devguide-c2d-guidance.md) bakın.

## <a name="device-twins"></a>Cihaz ikizleri

Aygıt ikizleri aygıtla ilgili bilgileri saklar:

* Aygıt ve arka uçlar aygıt koşullarını ve yapılandırmayı eşitlemek için kullanılabilir.

* Çözüm arka uç sorgulamak ve uzun süren işlemleri hedeflemek için kullanabilirsiniz.

Aygıt ikizinin yaşam döngüsü ilgili [aygıt kimliğine](iot-hub-devguide-identity-registry.md)bağlıdır. Aygıt ikizleri, IoT Hub'da bir aygıt kimliği oluşturulduğunda veya silindiğinde örtülü olarak oluşturulur ve silinir.

Aygıt ikizi, şunları içeren bir JSON belgesidir:

* **Etiketler**. JSON belgesinin bir bölümü, çözümün arka ucunun okuyabileceği ve yazabileceği bir bölümdür. Etiketler aygıt uygulamaları tarafından görülemez.

* **İstenilen özellikler**. Aygıt yapılandırmasını veya koşullarını eşitlemek için bildirilen özelliklerle birlikte kullanılır. Çözüm arka uç istenilen özellikleri ayarlayabilir ve cihaz uygulaması bunları okuyabilir. Cihaz uygulaması da istenilen özelliklerdeki değişikliklerle ilgili bildirimler alabilir.

* **Bildirilen özellikler.** Aygıt yapılandırmasını veya koşullarını eşitlemek için istenilen özelliklerle birlikte kullanılır. Aygıt uygulaması bildirilen özellikleri ayarlayabilir ve çözüm arka uç bunları okuyabilir ve sorgulayabilir.

* **Aygıt kimlik özellikleri**. Aygıt ikizJSON belgesinin [kökü, kimlik kayıt defterinde](iot-hub-devguide-identity-registry.md)depolanan ilgili aygıt kimliğinden salt okunur özelliklerini içerir. Özellikleri `connectionStateUpdatedTime` `generationId` ve dahil olmayacaktır.

![Aygıt ikiz özelliklerinin ekran görüntüsü](./media/iot-hub-devguide-device-twins/twin.png)

Aşağıdaki örnekte bir aygıt ikiz JSON belge gösterir:

```json
{
    "deviceId": "devA",
    "etag": "AAAAAAAAAAc=", 
    "status": "enabled",
    "statusReason": "provisioned",
    "statusUpdateTime": "0001-01-01T00:00:00",
    "connectionState": "connected",
    "lastActivityTime": "2015-02-30T16:24:48.789Z",
    "cloudToDeviceMessageCount": 0, 
    "authenticationType": "sas",
    "x509Thumbprint": {     
        "primaryThumbprint": null, 
        "secondaryThumbprint": null 
    }, 
    "version": 2, 
    "tags": {
        "$etag": "123",
        "deploymentLocation": {
            "building": "43",
            "floor": "1"
        }
    },
    "properties": {
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            "$metadata" : {...},
            "$version": 1
        },
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            },
            "batteryLevel": 55,
            "$metadata" : {...},
            "$version": 4
        }
    }
}
```

Kök nesnede aygıt kimlik özellikleri ve kapsayıcı `tags` nesneleri `reported` ve `desired` her ikisi ve özellikleri vardır. Kapsayıcı, `properties` Aygıt ikiz meta`$metadata` [verileri](iot-hub-devguide-device-twins.md#device-twin-metadata) ve `$version` [İyimser eşzamanlılık](iot-hub-devguide-device-twins.md#optimistic-concurrency) bölümlerinde açıklanan bazı salt okunur öğeleri (, `$etag`ve ) içerir.

### <a name="reported-property-example"></a>Bildirilen özellik örneği

Önceki örnekte, aygıt ikizi `batteryLevel` aygıt uygulaması tarafından bildirilen bir özellik içerir. Bu özellik, bildirilen son pil düzeyine göre aygıtları sorgulamayı ve çalıştırmayı mümkün kılar. Diğer örnekler arasında cihaz uygulaması raporlama aygıtı yetenekleri veya bağlantı seçenekleri yer almaktadır.

> [!NOTE]
> Bildirilen özellikler, çözümün arka ucunun bir özelliğin bilinen son değeriyle ilgilendiği senaryoları basitleştirir. Çözüm arka uç zaman serisi gibi zaman damgalı olayların dizileri şeklinde aygıt telemetri işlemek gerekiyorsa [aygıttan buluta iletileri](iot-hub-devguide-messages-d2c.md) kullanın.

### <a name="desired-property-example"></a>İstenilen özellik örneği

Önceki örnekte, `telemetryConfig` istenen ve bildirilen aygıt ikizi, çözüm arka uç ve bu aygıt için telemetri yapılandırmasını senkronize etmek için aygıt uygulaması tarafından kullanılır. Örneğin:

1. Çözüm arka uç istenilen yapılandırma değeri ile istenen özelliği ayarlar. Belgenin istenilen özellik kümesine sahip kısmı aşağıda veda edebilirsiniz:

   ```json
   "desired": {
       "telemetryConfig": {
           "sendFrequency": "5m"
       },
       ...
   },
   ```

2. Cihaz uygulaması, bağlıysa veya ilk yeniden bağlanırsa değişiklikten hemen haberdar edilir. Aygıt uygulaması daha sonra güncelleştirilmiş yapılandırmayı (veya `status` özelliği kullanarak bir hata koşulu) bildirir. Bildirilen özelliklerin bir kısmı aşağıdadır:

   ```json
   "reported": {
       "telemetryConfig": {
           "sendFrequency": "5m",
           "status": "success"
       }
       ...
   }
   ```

3. Çözüm arka uç aygıt ikiz [sorgulayarak](iot-hub-devguide-query-language.md) birçok cihaz arasında yapılandırma işleminin sonuçlarını izleyebilirsiniz.

> [!NOTE]
> Önceki parçacıklar, bir aygıt yapılandırmasını ve durumunu kodlamanın bir yolunun okunabilirliği için en iyi duruma getirilmiş örneklerdir. IoT Hub, aygıt ikizlerinde istenen ve bildirilen özellikler için belirli bir şema dayatmaz.
> 

Firmware güncelleştirmeleri gibi uzun süren işlemleri eşitlemek için ikizleri kullanabilirsiniz. Aygıtlar arasında uzun süren bir işlemi eşitlemek ve izlemek için özelliklerin nasıl kullanılacağı hakkında daha fazla bilgi için, [aygıtları yapılandırmak için istenen özellikleri kullan'a](tutorial-device-twins.md)bakın.

## <a name="back-end-operations"></a>Arka uç işlemleri

Çözüm arka uç, https ile açığa çıkan aşağıdaki atomik işlemleri kullanarak aygıt ikizi üzerinde çalışır:

* **Aygıt ikizini kimlikle alın.** Bu işlem, etiketler ve istenen ve bildirilen sistem özellikleri de dahil olmak üzere aygıt ikiz belgesini döndürür.

* **Kısmen aygıt ikiz güncelleme**. Bu işlem, çözüm arka ucunun aygıt ikizindeki etiketleri veya istenen özellikleri kısmen güncelleştirmesini sağlar. Kısmi güncelleştirme, herhangi bir özellik ekleyen veya güncelleştiren bir JSON belgesi olarak ifade edilir. Ayarlanan `null` özellikler kaldırılır. Aşağıdaki örnek, `{"newProperty": "newValue"}`değeri ile yeni bir istenen özellik oluşturur, `existingProperty` ile `"otherNewValue"`varolan değeri üzerine yazar ve `otherOldProperty`kaldırır. Varolan istenen özelliklerde veya etiketlerde başka değişiklik yapılmaz:

   ```json
   {
       "properties": {
           "desired": {
               "newProperty": {
                   "nestedProperty": "newValue"
               },
               "existingProperty": "otherNewValue",
               "otherOldProperty": null
           }
       }
   }
   ```

* **İstenilen özellikleri değiştirin.** Bu işlem, çözüm arka ucunun tüm varolan tüm istenen özellikleri tamamen `properties/desired`üzerine yazmasını ve yeni bir JSON belgesini yerine atmasını sağlar.

* **Etiketleri değiştirin.** Bu işlem, çözümün arka ucunun tüm varolan etiketlerin tamamen üzerine `tags`yazılmasını ve yeni bir JSON belgesinin yerine geçmesini sağlar.

* **İkiz bildirimler alın.** Bu işlem, ikiz değiştirildiğinde çözümün arka ucunun bildirilmesini sağlar. Bunu yapmak için, IoT çözümünüzün bir rota oluşturması ve Veri Kaynağını *twinChangeEvents'e*eşit olarak ayarlaması gerekir. Varsayılan olarak, bu tür rotalar önceden var olmaz, bu nedenle ikiz bildirimler gönderilmez. Değişiklik hızı çok yüksekse veya dahili hatalar gibi diğer nedenlerden dolayı, IoT Hub tüm değişiklikleri içeren yalnızca bir bildirim gönderebilir. Bu nedenle, uygulamanızın tüm ara durumların güvenilir denetimi ne olursa ve günlüğe kaydedilmesi gerekiyorsa, aygıttan buluta iletileri kullanmanız gerekir. İkiz bildirim iletisi özellikleri ve gövdesi içerir.

  - Özellikler

    | Adı | Değer |
    | --- | --- |
    $content tipi | uygulama/json |
    $iothub-enqueuedtime |  Bildirimin gönderildiği saat |
    $iothub-mesaj-kaynak | ikizChangeEvents |
    $content kodlama | utf-8 |
    deviceId | Cihazın kimliği |
    hubName | IoT Hub adı |
    operationTimestamp | [ISO8601](https://en.wikipedia.org/wiki/ISO_8601) çalışma zaman damgası |
    iothub-mesaj-şema | ikizDeğişiklik Bildirimi |
    Optype | "replaceTwin" veya "updateTwin" |

    İleti sistemi özellikleri `$` sembolle önceden belirlenmiştir.

  - Gövde
        
    Bu bölümde JSON formatındaki tüm ikiz değişiklikleri içerir. Tüm ikiz bölümleri içerebilir farkı ile, bir yama olarak aynı biçimi kullanır: etiketleri, properties.reported, properties.desired, ve "$metadata" öğeleri içerir. Örneğin,

    ```json
    {
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

Önceki tüm işlemler [İyimser eşzamanlılığı](iot-hub-devguide-device-twins.md#optimistic-concurrency) destekler ve [IoT Hub'a Denetimi'nde](iot-hub-devguide-security.md)tanımlandığı gibi **ServiceConnect** izni gerektirir.

Bu işlemlere ek olarak, çözüm arka uç şunları yapabilir:

* SQL benzeri [IoT Hub sorgu dilini](iot-hub-devguide-query-language.md)kullanarak aygıtı sorgulayın.

* İşleri kullanarak büyük aygıt ikizkümelerinde işlemleri [gerçekleştirin.](iot-hub-devguide-jobs.md)

## <a name="device-operations"></a>Cihaz işlemleri

Cihaz uygulaması aşağıdaki atomik işlemleri kullanarak aygıt ikizi üzerinde çalışır:

* **Aygıt ikizi alın.** Bu işlem, aygıta bağlı aygıt için ikiz belgeyi (istenen ve bildirilen sistem özellikleri dahil) döndürür. (Etiketler aygıt uygulamaları tarafından görülemez.)

* **Bildirilen özellikleri kısmen güncelleştirin.** Bu işlem, şu anda bağlı olan aygıtın bildirilen özelliklerinin kısmi olarak güncelleştirilmesini sağlar. Bu işlem, çözüm arka uç istenen özelliklerin kısmi bir güncelleştirme için kullandığı aynı JSON güncelleştirme biçimini kullanır.

* **İstenilen özellikleri gözlemleyin.** Şu anda bağlı olan aygıt, gerçekleştiğinde istenen özelliklerdeki güncelleştirmelerden haberdar olmayı seçebilir. Aygıt, çözüm arka uç tarafından yürütülen aynı güncelleştirme biçimini (kısmi veya tam değiştirme) alır.

Önceki tüm işlemler, [IoT Hub'a Denetim Erişimi'nde](iot-hub-devguide-security.md)tanımlandığı gibi **DeviceConnect** izni gerektirir.

[Azure IoT aygıt SDK'ları,](iot-hub-devguide-sdks.md) önceki işlemleri birçok dil ve platformdan kullanmayı kolaylaştırır. İstenilen özellikler eşitleme için IoT Hub ilkel ayrıntıları hakkında daha fazla bilgi için [aygıt yeniden bağlantı akışına](iot-hub-devguide-device-twins.md#device-reconnection-flow)bakın.

## <a name="tags-and-properties-format"></a>Etiketler ve özellikler biçimi

Etiketler, istenen özellikler ve bildirilen özellikleri aşağıdaki kısıtlamalar ile JSON nesneleri şunlardır:

* **Tuşlar**: JSON nesnelerindeki tüm tuşlar UTF-8 kodlu, büyük/küçük harf duyarlı ve 1 KB'ye kadar uzunluktadır. İzin verilen karakterler UNICODE denetim karakterlerini (C0 ve `.` `$`C1 segmentleri) ve , ve SP'yi hariç tutar.

* **Değerler**: JSON nesnelerindeki tüm değerler aşağıdaki JSON türlerinden olabilir: boolean, sayı, string, object. Dizilere izin verilmez.

    * Tamsayılar minimum değeri -4503599627370496 ve maksimum değeri 4503599627370495 olabilir.

    * Dize değerleri UTF-8 kodlanır ve maksimum 4 KB uzunluğa sahip olabilir.

* **Derinlik**: Etiketlerdeki JSON nesnelerinin maksimum derinliği, istenen özellikler ve bildirilen özellikler 10'dur. Örneğin, aşağıdaki nesne geçerlidir:

   ```json
   {
       ...
       "tags": {
           "one": {
               "two": {
                   "three": {
                       "four": {
                           "five": {
                               "six": {
                                   "seven": {
                                       "eight": {
                                           "nine": {
                                               "ten": {
                                                   "property": "value"
                                               }
                                           }
                                       }
                                   }
                               }
                           }
                       }
                   }
               }
           }
       },
       ...
   }
   ```

## <a name="device-twin-size"></a>Cihaz ikiz boyutu

IoT Hub değeri `tags`üzerinde 8 KB boyut sınırı uygular ve 32 KB boyutu `properties/desired` `properties/reported`sınırı her değeri ve . Bu toplamlar `$etag`, , `$version`ve `$metadata/$lastUpdated`.

İkiz boyutu aşağıdaki gibi hesaplanır:

* JSON belgesindeki her özellik için, IoT Hub toplu olarak hesaplar ve özelliğin anahtarı ve değerinin uzunluğunu ekler.

* Özellik anahtarları UTF8 kodlanmış dizeleri olarak kabul edilir.

* Basit özellik değerleri UTF8 kodlanmış dizeleri, sayısal değerler (8 Bayt) veya Boolean değerleri (4 Bayt) olarak kabul edilir.

* UTF8 kodlanmış dizelerin boyutu, UNICODE denetim karakterleri (C0 ve C1 segmentleri) hariç tüm karakterler sayılarak hesaplanır.

* Karmaşık özellik değerleri (iç içe nesneler) içerdikleri özellik anahtarlarının ve özellik değerlerinin toplam boyutuna göre hesaplanır.

IoT `tags`Hub, sınırın `properties/desired`boyutunu veya `properties/reported` belgelerin boyutunu artıracak tüm işlemleri bir hatayla reddeder.

## <a name="device-twin-metadata"></a>Aygıt ikiz meta verileri

IoT Hub, aygıt ikizi istenen ve bildirilen özelliklerdeki her JSON nesnesi için son güncelleştirmenin zaman damgasını korur. Zaman damgaları UTC'dedir ve [ISO8601](https://en.wikipedia.org/wiki/ISO_8601) biçiminde `YYYY-MM-DDTHH:MM:SS.mmmZ`kodlanır.

Örneğin:

```json
{
    ...
    "properties": {
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            "$metadata": {
                "telemetryConfig": {
                    "sendFrequency": {
                        "$lastUpdated": "2016-03-30T16:24:48.789Z"
                    },
                    "$lastUpdated": "2016-03-30T16:24:48.789Z"
                },
                "$lastUpdated": "2016-03-30T16:24:48.789Z"
            },
            "$version": 23
        },
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            },
            "batteryLevel": "55%",
            "$metadata": {
                "telemetryConfig": {
                    "sendFrequency": "5m",
                    "status": {
                        "$lastUpdated": "2016-03-31T16:35:48.789Z"
                    },
                    "$lastUpdated": "2016-03-31T16:35:48.789Z"
                },
                "batteryLevel": {
                    "$lastUpdated": "2016-04-01T16:35:48.789Z"
                },
                "$lastUpdated": "2016-04-01T16:24:48.789Z"
            },
            "$version": 123
        }
    }
    ...
}
```

Bu bilgiler, nesne anahtarlarını kaldıran güncelleştirmeleri korumak için her düzeyde (yalnızca JSON yapısının yapraklarında) tutulur.

## <a name="optimistic-concurrency"></a>İyimser eşzamanlılık

Etiketler, istenilen ve bildirilen özellikleri tüm destek iyimser eşzamanlılık.
Etiketler, [RFC7232](https://tools.ietf.org/html/rfc7232)başına bir ETag var , bu etiketin JSON temsil temsil eder. Tutarlılık sağlamak için çözüm arka ucundan koşullu güncelleştirme işlemlerinde ETags kullanabilirsiniz.

Cihaz ikiz istenilen ve bildirilen özellikleri ETags `$version` yok, ancak artımlı olması garanti bir değere sahip. Benzer şekilde, sürüm güncelleştirmelerin tutarlılığını zorlamak için güncelleştirme tarafı tarafından kullanılabilir. Örneğin, bildirilen bir özellik için bir aygıt uygulaması veya istenen bir özellik için çözüm arka uç.

Sürümler, bir gözlem aracısının (istenen özellikleri gözlemleyerek cihaz uygulaması gibi) bir geri alma işlemi nin sonucu yla güncelleştirme bildirimi arasındaki yarışları uzlaştırması gerektiğinde de yararlıdır. [Aygıt yeniden bağlantı akışı bölümü](iot-hub-devguide-device-twins.md#device-reconnection-flow) daha fazla bilgi sağlar.

## <a name="device-reconnection-flow"></a>Cihaz yeniden bağlantı akışı

IoT Hub bağlantısı kesilen aygıtlar için istenen özellikleri güncelleştirme bildirimlerini korumaz. Bağlanan bir aygıtın, güncelleştirme bildirimleri için abone olmaya ek olarak istenen özellikler belgesinin tamamını alması gerekir. Güncelleştirme bildirimleri ile tam alma arasındaki yarışların olasılığı göz önüne alındığında, aşağıdaki akış sağlanmalıdır:

1. Aygıt uygulaması bir IoT hub'ına bağlanır.
2. İstenilen özellikler için cihaz uygulaması abone olun bildirimleri günceller.
3. Aygıt uygulaması istenilen özellikler için belgenin tamamını alır.

Cihaz uygulaması, tüm bildirimleri, alınan belgenin tamamından daha az veya eşit olan `$version` tüm bildirimleri yoksayabilir. IoT Hub sürümleri her zaman artış garanti çünkü bu yaklaşım mümkündür.

> [!NOTE]
> Bu mantık zaten Azure [IoT aygıt SDKs](iot-hub-devguide-sdks.md)uygulanmaktadır. Bu açıklama, yalnızca aygıt uygulaması Azure IoT aygıtı SDK'larından hiçbirini kullanamıyorsa ve MQTT arabirimini doğrudan programlamak zorundaysa yararlıdır.
> 

## <a name="additional-reference-material"></a>Ek referans materyali

IoT Hub geliştirici kılavuzundaki diğer başvuru konuları şunlardır:

* [IoT Hub uç noktaları](iot-hub-devguide-endpoints.md) makalesi, her IoT hub'ın çalışma zamanı ve yönetim işlemleri için ortaya çıkardığı çeşitli uç noktaları açıklar.

* [Azaltma ve kotalar](iot-hub-devguide-quotas-throttling.md) makalesi, IoT Hub hizmetine uygulanan kotaları ve hizmeti kullandığınızda bekleyeceğiniz azaltma davranışını açıklar.

* [Azure IoT aygıt ve hizmet SDK'ları](iot-hub-devguide-sdks.md) makalesi, Hem IoT Hub ile etkileşimde bulunan aygıt hem de hizmet uygulamaları geliştirdiğinizde kullanabileceğiniz çeşitli dil SDK'larını listeler.

* [Aygıt ikizleri, işleri ve ileti yönlendirme makalesi için IoT Hub sorgu dili,](iot-hub-devguide-query-language.md) IoT Hub'dan aygıtınız ve işleriniz hakkında bilgi almak için kullanabileceğiniz IoT Hub sorgu dilini açıklar.

* [IoT Hub MQTT destek](iot-hub-mqtt-support.md) makalesi, MQTT protokolü için IoT Hub desteği hakkında daha fazla bilgi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi cihaz ikizler hakkında öğrendim, aşağıdaki IoT Hub geliştirici kılavuzu konuları ilginizi çekebilir:

* [IoT Hub'da modül ikizleri anlama ve kullanma](iot-hub-devguide-module-twins.md)
* [Aygıtta doğrudan bir yöntem çağırma](iot-hub-devguide-direct-methods.md)
* [Birden fazla cihazda işleri zamanlama](iot-hub-devguide-jobs.md)

Bu makalede açıklanan bazı kavramları denemek için aşağıdaki IoT Hub öğreticilerine bakın:

* [Aygıt ikiznasıl kullanılır](iot-hub-node-node-twin-getstarted.md)
* [Aygıt ikiz özellikleri nasıl kullanılır?](tutorial-device-twins.md)
* [VS Code için Azure IoT Araçları ile cihaz yönetimi](iot-hub-device-management-iot-toolkit.md)
