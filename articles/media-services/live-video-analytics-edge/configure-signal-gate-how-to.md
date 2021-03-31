---
title: Olay tabanlı video kaydı için bir sinyal kapısı Yapılandırma-Azure
description: Bu makalede, bir medya grafiğinde sinyal kapısını yapılandırma hakkında rehberlik sunulmaktadır.
ms.topic: how-to
ms.date: 11/3/2020
ms.openlocfilehash: afcec7c03f1353f08b58311278f5a533e0c911bc
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94410802"
---
# <a name="configure-a-signal-gate-for-event-based-video-recording"></a>Olay tabanlı video kaydı için sinyal kapısı yapılandırma

Bir medya grafiğinde, [sinyal kapısı işlemci düğümü](media-graph-concept.md#signal-gate-processor) , bir olay tarafından tetiklendiğinde bir düğümden diğerine medya iletmesine olanak tanır. Tetiklendiğinde, ağ geçidi açılır ve belirtilen süre boyunca medya akışına izin verir. Ağ geçidini tetiklemeye yönelik olaylar yokluğunda, ağ geçidi kapanır ve medya akışı durur. Olay tabanlı video kaydı için sinyal kapısı işlemcisini kullanabilirsiniz.

Bu makalede, sinyal kapısı işlemcisini yapılandırmayı öğreneceksiniz.

## <a name="suggested-prereading"></a>Önerilen prereading
-   [Medya grafiği](media-graph-concept.md)
-   [Olay tabanlı video kaydı](event-based-video-recording-concept.md)


## <a name="problem"></a>Sorun
Bir Kullanıcı, bir olay tarafından tetiklendikten önce veya sonra belirli bir zamanda kayıt başlatmak isteyebilir. Kullanıcı, sistem içinde kabul edilebilir gecikme süresini bilir. Bu nedenle, sinyal kapısı işlemcisinin gecikmesini belirtmek ister. Ayrıca, kaç yeni olay alındıklarından bağımsız olarak, kayıtlarının en düşük ve en uzun süresini belirtmek ister.
 
### <a name="use-case-scenario"></a>Kullanım örneği senaryosu
Binanın ön kapısının açıldığı her seferinde video kaydetmek istediğinizi varsayalım. Kaydın şunları yapmak istiyorsunuz: 

- Kapı açılmadan önce *X* saniye ekleyin. 
- Kapı yeniden açılmazsa en az *Y* saniye. 
- Kapı sürekli açılırsa en fazla *Z* saniye. 
 
Kapı sensörlerinizin *K* saniye gecikme süresi olduğunu bilirsiniz. Olayların, geç olarak yok sayılacağı olasılığını azaltmak için olayların gelmesi için en az *K* saniyeye izin vermek istersiniz.


## <a name="solution"></a>Çözüm

Sorunu gidermek için, sinyal kapısı işlemci parametrelerinizi değiştirin.

Bir sinyal kapısı işlemcisini yapılandırmak için şu dört parametreyi kullanın:
- Etkinleştirme değerlendirme penceresi
- Etkinleştirme sinyali boşluğu
- Minimum etkinleştirme penceresi
- En yüksek etkinleştirme penceresi

Sinyal kapısı işlemcisi tetiklendiğinde, en düşük etkinleştirme süresi boyunca açık kalır. Etkinleştirme olayı, en erken olayın zaman damgasında başlar ve etkinleştirme sinyali denkleştirilmesi gerekir. 

Sinyal kapısı işlemcisi açıkken tekrar tetikleniyorsa, süreölçer sıfırlanır ve ağ geçidi en az etkinleştirme süresi boyunca açık kalır. Sinyal kapısı işlemcisi, en yüksek etkinleştirme süresinden daha uzun bir süre açık kalır. 

Bir olay (olay 1), medya zaman damgalarına dayalı olarak, sistem lags ve olay 1 ' den sonra sinyal kapısı işlemcisine ulaştığında yok sayıardı edilebilir. Olay 1, olay 2 ve etkinleştirme değerlendirme penceresinin varışı arasında gelmezse, olay 1 yok sayıdır. Sinyal kapısı işlemcisi aracılığıyla geçirilmiyor. 

Bağıntı kimlikleri her olay için ayarlanır. Bu kimlikler ilk olaydan ayarlanır. Bunlar, aşağıdaki her olay için sıralıdır.

> [!IMPORTANT]
> Medya süresi, medyada bir olay oluştuğunda oluşan medya zaman damgasına dayalıdır. Sinyal Kapısı ' ne gelen olayların sırası, medya zamanına ulaşan olayların sırasını yansıtmayabilir.


### <a name="parameters-based-on-the-physical-time-that-events-arrive-at-the-signal-gate"></a>Olayların sinyal kapısını aldığı fiziksel saate göre parametreler

* **minimumactivationtime (en kısa bir kayıt süresi)**: en az umactivationtime tarafından kesintiye uğramadığı sürece, sinyal kapısı işlemcisinin yeni olayları almak için tetiklendikten sonra açık kalacağı en az saniye sayısı.
* **Maximumactivationtime (bir kaydın en uzun olası süresi)**: ilk olaydan, alınan olayların ne olursa olsun, sinyal kapısı işlemcisinin yeni olayları almak için tetiklendikten sonra açık kaldığı en fazla saniye sayısı.
* **Activationsignalkayması**: sinyal kapısı işlemcisinin etkinleştirilmesi ile video kaydının başlangıcı arasındaki saniye sayısı. Genellikle, bu değer, tetikleme olayından önce kaydı başlattığı için negatiftir.
* **activationEvaluationWindow**: ilk tetikleme olayından başlayarak, medya zamanında ilk olaydan önce gerçekleşen bir olayın, yok sayılmadan ve geç olarak kabul edilmeden önce sinyal kapısı işlemcisine gelmesi gereken saniye sayısı.

> [!NOTE]
> *Geç varış* , etkinleştirme değerlendirme penceresi geçtikten sonra, ancak medya zamanında ilk olaydan önce gelen olaydır.

### <a name="limits-of-parameters"></a>Parametrelerin limitleri

* **activationEvaluationWindow**: 0 saniye ila 10 saniye
* **Activationsignalkayması**:-1 dakika-1 dakika
* **Minimumactivationtime**: 1 saniye-1 saat
* **Maximumactivationtime**: 1 saniye-1 saat


Kullanım durumunda, parametreleri aşağıdaki gibi ayarlarsınız:

* **activationEvaluationWindow**: *K* saniye
* **Activationsignalkayması**: *-X* saniye
* **Minimumactivationwindow**: *Y* saniye
* **Maximumactivationwindow**: *Z* saniye


**Sinyal kapısı işlemci** düğümü bölümünün şu parametre değerleri için bir medya grafik topolojisinde nasıl görüneceğine ilişkin bir örnek aşağıda verilmiştir:
* **activationEvaluationWindow**: 1 saniye
* **Activationsignalkayması**:-5 saniye
* en **az Umactivationtime**: 20 saniye
* **Maximumactivationtime**: 40 saniye

> [!IMPORTANT]
> [Iso 8601 süre biçimi](https://en.wikipedia.org/wiki/ISO_8601#Durations
) her bir parametre değeri için beklenmektedir. Örneğin, PT1S = 1 saniye.


```
"processors":              
[
          {
            "@type": "#Microsoft.Media.MediaGraphSignalGateProcessor",
            "name": "signalGateProcessor",
            "inputs": [
              {
                "nodeName": "iotMessageSource"
              },
              {
                "nodeName": "rtspSource"
              }
            ],
            "activationEvaluationWindow": "PT1S",
            "activationSignalOffset": "-PT5S",
            "minimumActivationTime": "PT20S",
            "maximumActivationTime": "PT40S"
          }
]
```


Bu sinyal kapısı işlemci yapılandırmasının farklı kayıt senaryolarında nasıl davranacağını göz önünde bulundurun.

### <a name="recording-scenarios"></a>Kayıt senaryoları

**Bir kaynaktan bir olay (*normal etkinleştirme*)**

Bir olay, bir olay, ağ geçidi 'ne ulaşmadan önce 5 saniye (etkinleştirme sinyali = 5 saniye) başlayan bir kayıtta sonuçları alan bir sinyal geçidi işlemcisi. Ağ geçidini almak için en düşük etkinleştirme zamanının sonundan önce başka hiçbir olay gelmediğinden, kaydın geri kalanı 20 saniyedir (minimum etkinleştirme süresi = 20 saniye).

Örnek Diyagram:
> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/configure-signal-gate-how-to/normal-activation.svg" alt-text="Bir kaynaktan bir olayın normal etkinleştirilmesini gösteren diyagram.":::

* Kayıt süresi =-fark + minimumActivationTime = [E1 + kayması, E1 + minimumActivationTime]


**Bir kaynaktan iki olay (*retriggered etkinleştirme*)**

İki olay alan bir sinyal geçidi işlemcisi, olay kapıya ulaşmadan önce 5 saniye (etkinleştirme sinyali kayması = 5 saniye) başlayan bir kayıtla sonuçlanır. Ayrıca, olay 2 olay 1 ' den 5 saniye sonra gider. Olay 2 olay 1 ' in en az etkinleştirme süresi (20 saniye) sonundan önce alındığından, ağ geçidi retriggered olur. Kaydın geri kalanı 20 saniye (en az etkinleştirme süresi = 20 saniye) olduğundan, ağ geçidini yeniden almak için olay 2 ' den en düşük etkinleştirme zamanının sonundan önce hiçbir olay gelmediği için.

Örnek Diyagram:
> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/configure-signal-gate-how-to/retriggering-activation.svg" alt-text="Bir kaynaktaki iki olayın retriggered etkinleştirmesini gösteren diyagram.":::

* Kayıt süresi =-fark + (olay 2 ' nin varışı-olay 1 ' in varışı) + en düşük Umactivationtime


**Bir kaynaktan *N* olay (*en fazla etkinleştirme*)**

*N* olayları alan bir sinyal geçidi işlemcisi, ağ geçidini ilk olay ulaşmadan önce 5 saniye (etkinleştirme sinyali kayması = 5 saniye) başlatan bir kayıtla sonuçlanır. Her olay, önceki olaydan 20 saniyelik en düşük etkinleştirme zamanının sonundan önce ulaştığında, geçit sürekli olarak retriggered. İlk olaydan sonra 40 saniyelik en fazla etkinleştirme zamanına kadar açık kalır. Ardından, ağ geçidi kapanır ve artık yeni olayları kabul etmez.

Örnek Diyagram:
> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/configure-signal-gate-how-to/maximum-activation.svg" alt-text="Bir kaynaktan en fazla N olay etkinleştirmeyi gösteren diyagram.":::
 
* Kayıt süresi =-fark + maximumActivationTime

> [!IMPORTANT]
> Yukarıdaki diyagramlarda, her olayın fiziksel saat ve medya zamanında aynı anında ulaştığı varsayılmıştır. Diğer bir deyişle, hiç geç yok olduğunu varsaymaktadır.

## <a name="next-steps"></a>Sonraki adımlar

[Olay tabanlı video kaydı öğreticisini](event-based-video-recording-tutorial.md)deneyin. [Üzerindetopology.js](https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/evr-hubMessage-assets/topology.json)düzenleyerek başlayın. SignalgateProcessor düğümünün parametrelerini değiştirin ve ardından öğreticinin geri kalanını izleyin. Parametrelerin etkisini analiz etmek için video kayıtlarını gözden geçirin.



