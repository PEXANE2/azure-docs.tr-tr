---
title: Azure IoT Hub modüllerini anlayın | Microsoft Dokümanlar
description: Geliştirici kılavuzu - IoT Hub ile aygıtlarınız arasındaki durum ve yapılandırma verilerini senkronize etmek için modül ikizlerini kullanın
author: chrissie926
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/01/2020
ms.author: menchi
ms.openlocfilehash: 5ef6c4de288a764abbe434c5d84fc99e154f7492
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78303605"
---
# <a name="understand-and-use-module-twins-in-iot-hub"></a>IoT Hub'da modül ikizleri anlama ve kullanma

Bu makalede, önce [IoT Hub'da Anla ve aygıt ikizlerini kullandığınızı](iot-hub-devguide-device-twins.md) varsayar. IoT Hub'da, her aygıt kimliği altında en fazla 20 modül kimliği oluşturabilirsiniz. Her modül kimliği örtülü olarak bir modül ikizi oluşturur. Aygıt ikizleri gibi, modül ikizleri de meta veriler, yapılandırmalar ve koşullar dahil olmak üzere modül durumu bilgilerini depolayan JSON belgeleridir. Azure IoT Hub, IoT Hub'a bağlandığınız her modül için bir modül ikizi tutar. 

Aygıt tarafında, IoT Hub aygıt SDK'ları, her birinin IoT Hub'a bağımsız bir bağlantı açtığı modüller oluşturmanıza olanak tanır. Bu işlev, cihazınızdaki farklı bileşenler için ayrı ad alanları kullanmanıza olanak tanır. Örneğin, üç farklı sensöre sahip bir otomat var. Her sensör şirketinizdeki farklı departmanlar tarafından kontrol edilir. Her sensör için bir modül oluşturabilirsiniz. Bu şekilde, her departman yalnızca iş gönderebilir veya yöntemleri denetledikleri sensöre yönlendirebilir, çakışmaları ve kullanıcı hatalarını önler.

 Modül kimliği ve modül ikizi, cihaz kimliği ve aygıt ikizi ile aynı yetenekleri sağlar, ancak daha ince bir tanecikliolarak. Bu ince parçalılık, işletim sistemi tabanlı aygıtlar veya birden çok bileşeni yöneten firmware aygıtları gibi yetenekli aygıtların bu bileşenlerin her biri için yapılandırmayı ve koşulları yalıtmalarını sağlar. Modülkimliği ve modül ikizleri modüler yazılım bileşenlerine sahip IoT cihazlarıyla çalışırken endişelerin yönetimden ayrılmasını sağlar. Modül ikiz ilerletme düzeyindeki tüm cihaz ikiz işlevlerini modül ikiz genel kullanılabilirliği ile desteklemeyi hedefliyoruz. 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu makalede açıklanır:

* Modül ikiz yapısı: *etiketleri*, *istenen* ve *bildirilen özellikleri*.
* Modüllerin ve arka uçların modül ikizleri üzerinde gerçekleştirebileceği işlemler.

Bildirilen özellikleri, aygıttan buluta iletileri veya dosya yüklemeyi kullanma konusunda rehberlik etmek için [Aygıttan buluta iletişim kılavuzuna](iot-hub-devguide-d2c-guidance.md) bakın.

İstenilen özellikleri, doğrudan yöntemleri veya buluttan aygıta iletileri kullanma konusunda rehberlik etmek için [buluttan aygıta iletişim kılavuzuna](iot-hub-devguide-c2d-guidance.md) bakın.

## <a name="module-twins"></a>Modül ikizleri

Modül ikizleri modülle ilgili bilgileri saklar:

* Aygıttaki ve IoT Hub'daki modüller modül koşullarını ve yapılandırmayı senkronize etmek için kullanabilir.

* Çözüm arka uç sorgulamak ve uzun süren işlemleri hedeflemek için kullanabilirsiniz.

Bir modül ikizinin yaşam döngüsü ilgili [modül kimliğine](iot-hub-devguide-identity-registry.md)bağlıdır. Modülikizleri, IoT Hub'da bir modül kimliği oluşturulduğunda veya silindiğinde örtülü olarak oluşturulur ve silinir.

Modül ikizi, şunları içeren bir JSON belgesidir:

* **Etiketler**. JSON belgesinin bir bölümü, çözümün arka ucunun okuyabileceği ve yazabileceği bir bölümdür. Etiketler aygıttaki modüller tarafından görülemez. Etiketler, amacı sorgulamak için ayarlanır.

* **İstenilen özellikler**. Modül yapılandırmasını veya koşullarını eşitlemek için bildirilen özelliklerle birlikte kullanılır. Çözüm arka uç istenilen özellikleri ayarlayabilir ve modül uygulaması bunları okuyabilir. Modül uygulaması da istenilen özelliklerdeki değişikliklerin bildirimlerini alabilir.

* **Bildirilen özellikler.** Modül yapılandırmasını veya koşullarını senkronize etmek için istenilen özelliklerle birlikte kullanılır. Modül uygulaması bildirilen özellikleri ayarlayabilir ve çözüm arka uç bunları okuyabilir ve sorgulayabilir.

* **Modül kimlik özellikleri**. Modül ikiz JSON belgenin [kökü, kimlik kayıt defterinde](iot-hub-devguide-identity-registry.md)depolanan ilgili modül kimliğinden salt okunur özelliklerini içerir.

![Cihaz ikizinin mimari temsili](./media/iot-hub-devguide-device-twins/module-twin.jpg)

Aşağıdaki örnekte bir modül ikiz JSON belge gösterir:

```json
{
    "deviceId": "devA",
    "moduleId": "moduleA",
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

Kök nesnede modül kimlik özellikleri ve kapsayıcı `tags` nesneleri `reported` ve `desired` her ikisi ve özellikleri vardır. `properties` Kapsayıcı, Modül çift meta`$metadata` [verileri](iot-hub-devguide-module-twins.md#module-twin-metadata) ve `$version` [İyimser eşzamanlılık](iot-hub-devguide-device-twins.md#optimistic-concurrency) bölümlerinde açıklanan bazı salt okunur öğeleri (, `$etag`ve ) içerir.

### <a name="reported-property-example"></a>Bildirilen özellik örneği

Önceki örnekte, modül ikizi `batteryLevel` modül uygulaması tarafından bildirilen bir özellik içerir. Bu özellik, bildirilen son pil düzeyine göre modülleri sorgulamayı ve çalıştırmayı mümkün kılar. Diğer örnekler modül uygulaması raporlama modülü yetenekleri veya bağlantı seçenekleri içerir.

> [!NOTE]
> Bildirilen özellikler, çözümün arka ucunun bir özelliğin bilinen son değeriyle ilgilendiği senaryoları basitleştirir. Çözüm arka uç zaman serisi gibi zaman damgalı olayların dizileri şeklinde modül telemetri işlemek gerekiyorsa [aygıttan buluta iletileri](iot-hub-devguide-messages-d2c.md) kullanın.

### <a name="desired-property-example"></a>İstenilen özellik örneği

Önceki örnekte, `telemetryConfig` istenen ve bildirilen modül ikizi, bu modül için telemetri yapılandırmasını senkronize etmek için çözüm arka uç ve modül uygulaması tarafından kullanılır. Örnek:

1. Çözüm arka uç istenilen yapılandırma değeri ile istenen özelliği ayarlar. Belgenin istenilen özellik kümesine sahip kısmı aşağıda veda edebilirsiniz:

    ```json
    ...
    "desired": {
        "telemetryConfig": {
            "sendFrequency": "5m"
        },
        ...
    },
    ...
    ```

2. Modül uygulaması, bağlı ysa veya ilk yeniden bağlantıda değişiklikten hemen haberdar edilir. Modül uygulaması daha sonra güncelleştirilmiş yapılandırmayı (veya `status` özelliği kullanarak bir hata koşulu) bildirir. Bildirilen özelliklerin bir kısmı aşağıdadır:

    ```json
    "reported": {
        "telemetryConfig": {
            "sendFrequency": "5m",
            "status": "success"
        }
        ...
    }
    ```

3. Çözüm arka uç modül ikizler [sorgulayarak,](iot-hub-devguide-query-language.md) birçok modül ler arasında yapılandırma işleminin sonuçlarını izleyebilirsiniz.

> [!NOTE]
> Önceki parçacıklar, bir modül yapılandırmasını ve durumunu kodlamanın bir yolunun okunabilirliği için optimize edilmiş örneklerdir. IoT Hub, modül ikizlerinde istenen ve bildirilen özellikler için belirli bir şema dayatmaz.
> 
> 

## <a name="back-end-operations"></a>Arka uç işlemleri
Çözelti arka uç, https ile açığa çıkan aşağıdaki atomik işlemleri kullanarak modül ikizi üzerinde çalışır:

* **Modül ikizi kimlikle alın.** Bu işlem, etiketler ve istenen ve bildirilen sistem özellikleri de dahil olmak üzere modül ikiz belgeyi döndürür.

* **Kısmen modül ikiz güncelleme**. Bu işlem, çözüm arka ucunun etiketleri veya istenen özellikleri modül ikizinde kısmen güncelleştirmesini sağlar. Kısmi güncelleştirme, herhangi bir özellik ekleyen veya güncelleştiren bir JSON belgesi olarak ifade edilir. Ayarlanan `null` özellikler kaldırılır. Aşağıdaki örnek, `{"newProperty": "newValue"}`değeri ile yeni bir istenen özellik oluşturur, `existingProperty` ile `"otherNewValue"`varolan değeri üzerine yazar ve `otherOldProperty`kaldırır. Varolan istenen özelliklerde veya etiketlerde başka değişiklik yapılmaz:

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

* **İkiz bildirimler alın.** Bu işlem, ikiz değiştirildiğinde çözümün arka ucunun bildirilmesini sağlar. Bunu yapmak için, IoT çözümünüzün bir rota oluşturması ve Veri Kaynağını *twinChangeEvents'e*eşit olarak ayarlaması gerekir. Varsayılan olarak, hiçbir ikiz bildirimleri gönderilir, yani, bu tür yollar önceden var. Değişiklik hızı çok yüksekse veya dahili hatalar gibi diğer nedenlerden dolayı, IoT Hub tüm değişiklikleri içeren yalnızca bir bildirim gönderebilir. Bu nedenle, uygulamanızın tüm ara durumların güvenilir denetimi ne olursa ve günlüğe kaydedilmesi gerekiyorsa, aygıttan buluta iletileri kullanmanız gerekir. İkiz bildirim iletisi özellikleri ve gövdesi içerir.

  - Özellikler

    | Adı | Değer |
    | --- | --- |
    $content tipi | uygulama/json |
    $iothub-enqueuedtime |  Bildirimin gönderildiği saat |
    $iothub-mesaj-kaynak | ikizChangeEvents |
    $content kodlama | utf-8 |
    deviceId | Cihazın kimliği |
    moduleId | Modülün kimliği |
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

Önceki tüm işlemler [İyimser eşzamanlılığı](iot-hub-devguide-device-twins.md#optimistic-concurrency) destekler ve [IoT Hub'a Denetim Erişimi](iot-hub-devguide-security.md) makalesinde tanımlandığı şekilde **ServiceConnect** izni gerektirir.

Bu işlemlere ek olarak, çözüm arka uç SQL benzeri [IoT Hub sorgu dilini](iot-hub-devguide-query-language.md)kullanarak modül ikiz sorgulayabilirsiniz.

## <a name="module-operations"></a>Modül işlemleri

Modül uygulaması aşağıdaki atomik işlemleri kullanarak modül ikizüzerinde çalışır:

* **Modül ikiz alın.** Bu işlem, şu anda bağlı olan modül için modül ikiz belgesini (etiketler ve istenilen ve bildirilen sistem özellikleri dahil) döndürür.

* **Bildirilen özellikleri kısmen güncelleştirin.** Bu işlem, bağlı olan modülün bildirilen özelliklerinin kısmi olarak güncelleştirilmesini sağlar. Bu işlem, çözüm arka uç istenen özelliklerin kısmi bir güncelleştirme için kullandığı aynı JSON güncelleştirme biçimini kullanır.

* **İstenilen özellikleri gözlemleyin.** Şu anda bağlı olan modül, gerçekleştiğinde istenen özelliklerdeki güncelleştirmelerden haberdar edilmeyi seçebilir. Modül, çözüm arka uç tarafından yürütülen aynı güncelleştirme biçimini (kısmi veya tam değiştirme) alır.

Önceki tüm işlemler, [IoT Hub'a Denetim Erişimi](iot-hub-devguide-security.md) makalesinde tanımlandığı şekilde **ModuleConnect** izni gerektirir.

[Azure IoT aygıt SDK'ları,](iot-hub-devguide-sdks.md) önceki işlemleri birçok dil ve platformdan kullanmayı kolaylaştırır.

## <a name="tags-and-properties-format"></a>Etiketler ve özellikler biçimi

Etiketler, istenen özellikler ve bildirilen özellikleri aşağıdaki kısıtlamalar ile JSON nesneleri şunlardır:

* **Anahtarlar**: JSON nesnelerindeki tüm tuşlar büyük/küçük harf duyarlı 64 bayt UTF-8 UNICODE dizeleridir. İzin verilen karakterler UNICODE denetim karakterlerini (C0 ve `.`C1 segmentleri) ve , SP ve `$`.

* **Değerler**: JSON nesnelerindeki tüm değerler aşağıdaki JSON türlerinden olabilir: boolean, sayı, string, object. Dizilere izin verilmez.

    * Tamsayılar minimum değeri -4503599627370496 ve maksimum değeri 4503599627370495 olabilir.

    * Dize değerleri UTF-8 kodlanır ve maksimum uzunluğu 512 bayt olabilir.

* **Derinlik**: Etiketlerdeki, istenilen ve bildirilen tüm JSON nesneleri maksimum 5 derinliğe sahip olabilir. Örneğin, aşağıdaki nesne geçerlidir:

    ```json
    {
        ...
        "tags": {
            "one": {
                "two": {
                    "three": {
                        "four": {
                            "five": {
                                "property": "value"
                            }
                        }
                    }
                }
            }
        },
        ...
    }
    ```

## <a name="module-twin-size"></a>Modül ikiz boyutu

IoT Hub değeri `tags`üzerinde 8 KB boyut sınırı uygular ve 32 KB boyutu `properties/desired` `properties/reported`sınırı her değeri ve . Bu toplamlar `$etag`, , `$version`ve `$metadata/$lastUpdated`.

İkiz boyutu aşağıdaki gibi hesaplanır:

* JSON belgesindeki her özellik için, IoT Hub toplu olarak hesaplar ve özelliğin anahtarı ve değerinin uzunluğunu ekler.

* Özellik anahtarları UTF8 kodlanmış dizeleri olarak kabul edilir.

* Basit özellik değerleri UTF8 kodlanmış dizeleri, sayısal değerler (8 Bayt) veya Boolean değerleri (4 Bayt) olarak kabul edilir.

* UTF8 kodlanmış dizelerin boyutu, UNICODE denetim karakterleri (C0 ve C1 segmentleri) hariç tüm karakterler sayılarak hesaplanır.

* Karmaşık özellik değerleri (iç içe nesneler) içerdikleri özellik anahtarlarının ve özellik değerlerinin toplam boyutuna göre hesaplanır.

IoT Hub, bu belgelerin boyutunu sınırın üzerinde artıracak tüm işlemleri bir hatayla reddeder.

## <a name="module-twin-metadata"></a>Modül ikiz meta verileri

IoT Hub, modül ikizi istenen ve bildirilen özelliklerdeki her JSON nesnesi için son güncelleştirmenin zaman damgasını korur. Zaman damgaları UTC'dedir ve [ISO8601](https://en.wikipedia.org/wiki/ISO_8601) biçiminde `YYYY-MM-DDTHH:MM:SS.mmmZ`kodlanır.
Örnek:

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

Modül ikiz istenilen ve bildirilen özellikleri ETags `$version` yok, ancak artımlı olması garanti bir değere sahip. Benzer şekilde, sürüm güncelleştirmelerin tutarlılığını zorlamak için güncelleştirme tarafı tarafından kullanılabilir. Örneğin, bildirilen bir özellik için bir modül uygulaması veya istenen bir özellik için çözüm arka uç.

Sürümler, bir gözlem aracısının (istenen özellikleri gözlemleyerek modül uygulaması gibi) bir retrieve işlemi nin sonucu yla bir güncelleştirme bildirimi arasındaki yarışları uzlaştırması gerektiğinde de yararlıdır. Bölüm [Aygıt yeniden bağlantı akışı](iot-hub-devguide-device-twins.md#device-reconnection-flow) daha fazla bilgi sağlar. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede açıklanan bazı kavramları denemek için aşağıdaki IoT Hub öğreticilerine bakın:

* [.NET arka uç ve .NET cihazını kullanarak IoT Hub modülü kimliği ve modül ikizi ile başlayın](iot-hub-csharp-csharp-module-twin-getstarted.md)
