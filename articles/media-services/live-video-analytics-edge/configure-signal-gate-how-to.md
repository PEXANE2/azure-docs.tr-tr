---
title: Olay tabanlı video kaydı için bir sinyal kapısı Yapılandırma-Azure
description: Bu makalede, bir medya grafiğinde sinyal kapısını yapılandırma hakkında rehberlik sunulmaktadır.
ms.topic: how-to
ms.date: 11/3/2020
ms.openlocfilehash: 4204a43915f9c79cae7dfe82b37e741fee89fb4a
ms.sourcegitcommit: 0d171fe7fc0893dcc5f6202e73038a91be58da03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93380239"
---
# <a name="configure-a-signal-gate-for-event-based-video-recording"></a>Olay tabanlı video kaydı için sinyal kapısı yapılandırma

Bir ortam grafiğinde, [sinyal kapısı işlemci düğümü](media-graph-concept.md#signal-gate-processor) , bir olay tarafından tetiklendiğinde bir düğümden diğerine medya iletmesine olanak tanır. Tetiklendiğinde, ağ geçidi açılır ve belirtilen süre boyunca medya akışına izin verir. Ağ geçidini tetiklemeye yönelik olay yokluğunda, ağ geçidi kapanır ve medya akışı durur. Sinyal kapısı işlemcisi, olay tabanlı video kaydı için geçerlidir.
Bu makalede, sinyal kapısı işlemcisinin nasıl yapılandırılacağı hakkında ayrıntılı bilgi edineceksiniz.

## <a name="suggested-pre-reading"></a>Önerilen önceden okuma
-   [Medya grafiği](media-graph-concept.md)
-   [Olay tabanlı video kaydı](event-based-video-recording-concept.md)


## <a name="problem"></a>Sorun
Kullanıcı, ağ geçidi bir olay tarafından tetiklendikten önce veya sonra belirli bir zamanı kaydetmeye başlamak isteyebilir. Kullanıcı, sistem içinde kabul edilebilir gecikmeyi bilir, böylece Kullanıcı sinyal kapısı işlemcisinin gecikmesini belirtmek istemektedir. Kullanıcı, kaydetme süresinin kaç tane yeni olay alındıklarından bağımsız olarak, en kısa ve en uzun süreyi belirtmek ister.
 
### <a name="use-case-scenario"></a>Kullanım örneği senaryosu
Binanın ön kapısının açıldığı her seferinde video kaydetmek istediğinizi varsayalım. Yukarıdaki kapıdan önce **X** saniye kaydedilmesini istiyorsunuz. Kapı bir daha açılmadıysa, kaydın en az **Y** saniye olmasını istersiniz. Kapı sürekli olarak açılırsa, kaydın en fazla **Z** saniyelik olmasını istersiniz. Kapı sensörizin **K** saniye gecikme süresine sahip olduğunu ve olayların yoksayılacağı ("geç alındı"), olayların gelmesi için en az **K** saniyeye izin vermek istediğinizi bilirsiniz.


## <a name="solution"></a>Çözüm

**_Sinyal kapısı Işlemci parametrelerini değiştirme_* _

Sinyal kapısı işlemcisi 4 parametre ile yapılandırılır:
- _ *etkinleştirme değerlendirme penceresi**
- **etkinleştirme sinyali boşluğu**
- **Minimum etkinleştirme penceresi**
- **en yüksek etkinleştirme penceresi**. 

Sinyal kapısı işlemcisi tetiklendiğinde, en düşük etkinleştirme süresi boyunca açık kalır. Etkinleştirme olayı, en erken olayın zaman damgasında başlar ve etkinleştirme sinyali denkleştirilmesi gerekir. Sinyal kapısı işlemcisi yeniden tetikleniyorsa, zamanlayıcı sıfırlanır ve ağ geçidi en az etkinleştirme süresi boyunca açık kalır. Sinyal kapısı işlemcisi, en yüksek etkinleştirme süresinden daha uzun bir süre açık kalmayacaktır. Bir olay **(olay 1)** , medya zaman damgalarına dayalı olarak, sistem lags ve **olay 1** ' **den sonra sinyal** kapısı işlemcisine alınırsa, başka bir olaydan **(olay** 1) daha önce gerçekleşen bir olaydır. Olay **1** , **olay 2** ve **etkinleştirme değerlendirme penceresinin** varışı arasında gelmezse, **olay 1** yok sayılamaz ve sinyal kapısı işlemcisi üzerinden geçirilmeyecektir. Bağıntı kimlikleri her olay için ayarlanır. Bu kimlikler ilk olaydan ayarlanır ve aşağıdaki her olay için sıralıdır.

> [!IMPORTANT]
> Medya süresi, medyada bir olay oluştuğunda oluşan medya zaman damgasını temel alır. Sinyal kapıya ulaşan olay sırası, medya zamanında ulaşan olay sırasını yansıtmayabilir.


### <a name="parameters-based-on-when-events-arrive-in-physical-time-to-the-signal-gate"></a>Parametreler: (olayların sinyal kapıya fiziksel zamanlı olarak ulaştığını temel alır)

* **minimumactivationtime (en az bir kayıt süresi)** = maksimum **Umactivationtime** tarafından kesilmediği takdirde, sinyal kapısı işlemcisinin yeni olayları almak üzere tetiklendikten sonra açık kalacağı en az saniye sayısı
* **Maximumactivationtime (bir kaydın en uzun olası süresi)** = ilk olaydan, alınan olayların ne olursa olsun, sinyal kapısı işlemcisinin yeni olayları almak için tetiklendikten sonra açık kalacağı en fazla saniye sayısı
* **Activationsignalkayması** = sinyal kapısı işlemcisinin etkinleştirilmesi ile video kaydının başladığı zaman arasındaki saniye sayısı, kaydın tetikleme olayından önce başlatılması için genellikle bu değer negatif olur
* **activationEvaluationWindow** = ilk tetikleme olayından başlayarak, medya zamanında ilk olaydan önce gerçekleşen bir olayın, yok sayılmadan ve "geç varış" olarak kabul edilmeden önce sinyal kapısı işlemcisine ulaşması gereken saniye sayısı

> [!NOTE]
> Geç varış, etkinleştirme değerlendirme penceresi geçtiğinde, ancak bu olay medya zamanında ilk olaydan önce ulaştığında gelen olaydır.

### <a name="limits-of-parameters"></a>Parametrelerin limitleri

* **activationEvaluationWindow: 0 saniye ila 10 saniye**

* **Activationsignalkayması:-1 dakika-1 dakika**

* **minimumActivationTime: 1 saniye-1 saat**

* **maximumActivationTime: 1 saniye-1 saat**


Kullanım örneğine göre parametreler aşağıdaki şekilde ayarlanır:

* **activationEvaluationWindow = K sn**
* **Activationsignalkayması =-X sn**
* **minimumActivationWindow = Y sn**
* **maximumActivationWindow = Z sn**


Aşağıda, sinyal kapısı Işlemci düğümü bölümünün bir medya grafiği topolojisinde aşağıdaki parametre değerleri için nasıl benzediklerine bir örnek verilmiştir:
* **activationEvaluationWindow = 1 saniye**
* **Activationsignalkayması =-5 saniye**
* **minimumActivationTime = 20 saniye**
* **maximumActivationTime = 40 saniye**

> [!IMPORTANT]
> [Iso 8601 süre biçimi](https://en.wikipedia.org/wiki/ISO_8601#Durations
) her bir parametre değeri için beklenmektedir. 
**Ex: PT1S = 1 saniye**


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


Bu sinyal kapısı işlemci yapılandırmasının farklı kayıt senaryolarında nasıl davranacağını ele alalım.


**1 kaynaktan 1 olay ( *normal etkinleştirme* )**

Bir sinyal kapısı işlemcisi bir olay alma, bir olay, ağ geçidiyle gelen olaydan önce "etkinleştirme sinyali kayması" (5) saniye başlayan bir kayıtla sonuçlanır. "En az etkinleştirme süresi" (20) saniye, kaydın geri kalanı ise, ağ geçidini yeniden almak için en düşük etkinleştirme süresi tamamlanmadan önce başka hiçbir olay gelmedikçe.

Örnek Diyagram:
> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/configure-signal-gate-how-to/normal-activation.svg" alt-text="Normal etkinleştirme":::

* Kayıt süresi =-fark + minimumActivationTime = [E1 + kayması, E1 + minimumActivationTime]


**1 kaynaktan 2 olay ( *retriggered etkinleştirme* )**

İki olay alan sinyal kapısı işlemcisi, ağ geçidini ilk olaydan önce "etkinleştirme sinyali kayması" (beş) saniye sonra Başlatan bir kayıtla sonuçlanır. İkinci olay ilk olaydan sonra beş saniye gelir, bu, ilk olaydan "minimum etkinleştirme süresi" (20) saniyeden önce, bu nedenle kapı açık kalmak için retriggered. Kaydın geri kalanı "en az etkinleştirme süresi" (20) saniye uzunluğundadır, bu yana 2. etkinliğin en düşük etkinleştirme süresi, ağ geçidini yeniden almak için tamamlanmadan önce başka hiçbir olay gelmedikçe.

Örnek Diyagram:
> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/configure-signal-gate-how-to/retriggering-activation.svg" alt-text="Retriggered etkinleştirmesi":::

* Kayıt süresi =-fark + (1. etkinliğin varışı) + en düşük Umactivationtime


**1 kaynaktaki N olay ( *en fazla etkinleştirme* )**

N olay alan bir sinyal geçidi işlemcisi, ağ geçidini ilk olaydan önce "etkinleştirme sinyali kayması" (5) saniye sonra Başlatan bir kayda neden olur. Her olay, önceki olaydan "en az etkinleştirme süresi" (20) saniye tamamlanmadan önce ulaştığında, ağ geçidi sürekli olarak retriggered ve ilk olaydan sonra "en uzun etkinleştirme süresi" (40) saniye sonra açık kalır ve bu da hiçbir yeni olayı kabul etmez.

Örnek Diyagram:
> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/configure-signal-gate-how-to/maximum-activation.svg" alt-text="En fazla etkinleştirme":::
 
* Kayıt süresi =-fark + maximumActivationTime

> [!IMPORTANT]
> Diyagramlar, her olayın fiziksel ve medya zamanında aynı örneğe ulaştığı varsayılmıştır. (Geç varış yok)

## <a name="next-steps"></a>Sonraki adımlar

### <a name="try-it-out"></a>Deneyin

[Olay tabanlı video kaydı öğreticisi](event-based-video-recording-tutorial.md)

Olay tabanlı video kaydı öğreticisini kullanarak, [topology.jsüzerinde](https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/evr-hubMessage-assets/topology.json)düzenleyin, signalgateProcessor düğümünün parametrelerini değiştirin, ardından öğreticinin geri kalanını izleyin. Parametrelerin etkisini analiz etmek için video kayıtlarını gözden geçirin.



