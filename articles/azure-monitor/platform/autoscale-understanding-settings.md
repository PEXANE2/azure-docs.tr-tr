---
title: Azure Monitör'de otomatik ölçek ayarlarını anlama
description: Otomatik ölçek ayarlarının ve bunların nasıl çalıştığının ayrıntılı bir dökümü. Sanal Makineler, Bulut Hizmetleri, Web Uygulamaları için geçerlidir
ms.topic: conceptual
ms.date: 12/18/2017
ms.subservice: autoscale
ms.openlocfilehash: 9a2b94208de7ce490a0e7acfbb71175b4a7c846e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75364314"
---
# <a name="understand-autoscale-settings"></a>Otomatik Ölçeklendirme ayarlarını anlama
Otomatik ölçeklendirme ayarları, uygulamanızın dalgalanan yükünü işlemek için çalışan doğru miktarda kaynağa sahip olduğunuzu garanti eder. Otomatik Ölçek ayarlarını, yük veya performansı gösteren ölçümlere göre tetiklenecek veya zamanlanan bir tarih ve saatte tetiklenecek şekilde yapılandırabilirsiniz. Bu makalede, bir Autoscale ayarı anatomisi ayrıntılı bir göz alır. Makale şema ve bir ayar özellikleri ile başlar ve sonra yapılandırılabilir farklı profil türleri arasında yürür. Son olarak makalede, Azure'daki Otomatik Ölçeklendirme özelliğinin herhangi bir anda hangi profili çalıştırılacak nasıl değerlendirdiği anlatılmaktadır.

## <a name="autoscale-setting-schema"></a>Otomatik ölçek ayar şeması
Otomatik Ölçek ayar şemasını göstermek için aşağıdaki Otomatik Ölçek ayarı kullanılır. Bu Otomatik Ölçeklendirme ayarı olduğunu unutmayın:
- Bir profil. 
- Bu profilde iki metrik kural var: biri ölçeklendirme küçülüyor, diğeri ölçeklendirme için.
  - Sanal makine ölçeği kümesinin ortalama yüzde CPU ölçümü son 10 dakika için yüzde 85'ten büyük olduğunda ölçeklendirme kuralı tetiklenir.
  - Sanal makine ölçeği kümesinin ortalaması son dakika için yüzde 60'tan az olduğunda ölçekleme kuralı tetiklenir.

> [!NOTE]
> Bir ayarın birden çok profili olabilir. Daha fazla bilgi edinmek için [profiller](#autoscale-profiles) bölümüne bakın. Bir profilde birden çok ölçekleme kuralı ve ölçeklendirme kuralları tanımlanabilir. Nasıl değerlendirildiklerini görmek için [değerlendirme](#autoscale-evaluation) bölümüne bakın.

```JSON
{
  "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/setting1",
  "name": "setting1",
  "type": "Microsoft.Insights/autoscaleSettings",
  "location": "East US",
  "properties": {
    "enabled": true,
    "targetResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/vmss1",
    "profiles": [
      {
        "name": "mainProfile",
        "capacity": {
          "minimum": "1",
          "maximum": "4",
          "default": "1"
        },
        "rules": [
          {
            "metricTrigger": {
              "metricName": "Percentage CPU",
              "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/vmss1",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT10M",
              "timeAggregation": "Average",
              "operator": "GreaterThan",
              "threshold": 85
            },
            "scaleAction": {
              "direction": "Increase",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          },
    {
            "metricTrigger": {
              "metricName": "Percentage CPU",
              "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/vmss1",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT10M",
              "timeAggregation": "Average",
              "operator": "LessThan",
              "threshold": 60
            },
            "scaleAction": {
              "direction": "Decrease",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          }
        ]
      }
    ]
  }
}
```

| Section | Öğe adı | Açıklama |
| --- | --- | --- |
| Ayar | Kimlik | Otomatik Ölçeklendirme ayarının kaynak kılığı Otomatik ölçeklendirme ayarları bir Azure Kaynak Yöneticisi kaynağıdır. |
| Ayar | ad | Otomatik Ölçeklendirme ayar adı. |
| Ayar | location | Otomatik Ölçeklendirme ayarının konumu. Bu konum, ölçeklendirilen kaynağın konumundan farklı olabilir. |
| properties | hedefResourceUri | Ölçeklendirilen kaynağın kaynak kimliği. Kaynak başına yalnızca bir Otomatik Ölçeklendirme ayarı nız olabilir. |
| properties | Profil | Otomatik Ölçeklendirme ayarı bir veya daha fazla profilden oluşur. Otomatik Ölçeklendirme motoru her çalıştığında, bir profil çalıştırıyor. |
| profil | ad | Profilin adı. Profili tanımlamanıza yardımcı olacak herhangi bir ad seçebilirsiniz. |
| profil | Kapasite.maksimum | İzin verilen maksimum kapasite. Otomatik Ölçeklendirme, bu profili yürükarırken, kaynağınızı bu sayının üzerinde ölçeklendirmez. |
| profil | Kapasite.minimum | İzin verilen minimum kapasite. Otomatik Ölçeklendirme, bu profili yürütererken, kaynağınızı bu sayının altına ölçeklendirmez. |
| profil | Kapasite.varsayılan | Kaynak ölçümü (bu durumda CPU "vmss1") ve geçerli kapasite varsayılanın altındaysa, Otomatik Ölçek varsayılana ölçeklenir. Bu, kaynağın kullanılabilirliğini sağlamak içindir. Geçerli kapasite zaten varsayılan kapasiteden daha yüksekse, Otomatik Ölçek ölçeklendirmez. |
| profil | rules | Otomatik ölçek, profildeki kuralları kullanarak maksimum ve minimum kapasiteler arasında otomatik olarak ölçeklendirilir. Bir profilde birden çok kuralınız olabilir. Genellikle iki kural vardır: biri ne zaman ölçeklendirileni belirlemek, diğeri ise ne zaman ölçeklendireceklerini belirlemek için. |
| Kural | metricTrigger | Kuralın metrik koşulunu tanımlar. |
| metricTrigger | metricName | Metnin adı. |
| metricTrigger |  metricResourceUri | Metriği yayıtan kaynağın kaynak kimliği. Çoğu durumda, ölçeklendirilmekte olan kaynakla aynıdır. Bazı durumlarda, farklı olabilir. Örneğin, depolama kuyruğundaki ileti sayısını temel alan sanal makine ölçeği kümesini ölçeklendirebilirsiniz. |
| metricTrigger | timeGrain | Metrik örnekleme süresi. Örneğin, **TimeGrain = "PT1M",** ölçümlerin istatistik öğesinde belirtilen toplama yöntemi kullanılarak her 1 dakikada bir toplanması gerektiği anlamına gelir. |
| metricTrigger | Istatistik | Zaman Tahıl dönemi içinde toplama yöntemi. Örneğin, **istatistik = "Ortalama"** ve **timeGrain = "PT1M"** ölçümleri ortalama alarak, her 1 dakikada bir toplanmalıdır anlamına gelir. Bu özellik, ölçümün nasıl örneklenebildiğini belirler. |
| metricTrigger | zaman Penceresi | Ölçümleri geriye bakmak için zaman miktarı. Örneğin, **timeWindow = "PT10M"** Otomatik ölçek her çalıştığında, son 10 dakika için ölçümleri sorgular anlamına gelir. Zaman penceresi ölçümlerinizin normalleştirilmesini sağlar ve geçici ani artışlara tepki vermekten kaçınır. |
| metricTrigger | timeAggregation | Örneklenmiş ölçümleri toplamak için kullanılan toplama yöntemi. Örneğin, **TimeAggregation = "Ortalama"** ortalama alarak örneklenmiş ölçümleri toplamalıdır. Önceki durumda, on 1 dakikalık örnekleri alın ve ortalamasını alın. |
| Kural | ölçekEylem | Kuralın metricTrigger tetiklendiğinde yapılacak eylem. |
| ölçekEylem | yön | Ölçeklendirmek için "Artırın", ölçeklendirecek "Azaltın".|
| ölçekEylem | value | Kaynağın kapasitesini ne kadar artıracağınız veya azaltacağınız. |
| ölçekEylem | cooldown | Yeniden ölçekleme den önce ölçeklendirme işleminden sonra bekleme süresi. Örneğin, **dolum / "PT10M" varsa,** Otomatik Ölçek başka bir 10 dakika için yeniden ölçeklendirmegirişiminde bulunmaz. Bekleme, ölçümlerin örneklerin eklenmesi veya kaldırılmasından sonra stabilize olmasını sağlamaktır. |

## <a name="autoscale-profiles"></a>Otomatik ölçeklendirme profilleri

Üç tür Otomatik Ölçek profili vardır:

- **Normal profil:** En yaygın profil. Kaynağınızı haftanın gününe veya belirli bir güne göre ölçeklendirmeniz gerekmiyorsa, normal bir profil kullanabilirsiniz. Bu profil daha sonra ne zaman ölçeklendirilecek ve ne zaman ölçeklendirileceklerini belirleyen metrik kurallarla yapılandırılabilir. Yalnızca bir normal profil tanımlı olmalıdır.

    Bu makalede daha önce kullanılan örnek profil, normal bir profil örneğidir. Kaynağınız için statik örnek sayısına ölçeklendirecek bir profil ayarlamanın da mümkün olduğunu unutmayın.

- **Sabit tarih profili:** Bu profil özel durumlar içindir. Örneğin, 26 Aralık 2017'de (PST) önemli bir etkinliğiniz olduğunu varsayalım. Kaynağınızın minimum ve maksimum kapasitelerinin o gün farklı olmasını, ancak yine de aynı ölçümleri ölçeklendirmesini istiyorsunuz. Bu durumda, ayarınızın profil listesine sabit bir tarih profili eklemeniz gerekir. Profil, yalnızca etkinliğin gününde çalışacak şekilde yapılandırılır. Başka bir gün için, Otomatik Ölçek normal profili kullanır.

    ``` JSON
    "profiles": [{
    "name": " regularProfile",
    "capacity": {
    ...
    },
    "rules": [{
    ...
    },
    {
    ...
    }]
    },
    {
    "name": "eventProfile",
    "capacity": {
    ...
    },
    "rules": [{
    ...
    }, {
    ...
    }],
    "fixedDate": {
        "timeZone": "Pacific Standard Time",
               "start": "2017-12-26T00:00:00",
               "end": "2017-12-26T23:59:00"
    }}
    ]
    ```
    
- **Yineleme profili:** Bu profil türü, bu profilin her zaman haftanın belirli bir gününde kullanıldığından emin olabilirsiniz. Yineleme profillerinin yalnızca başlangıç zamanı vardır. Bir sonraki yineleme profili veya sabit tarih profili başlatılacak şekilde ayarlanana kadar çalıştırılırlar. Aynı ayarda tanımlanmış normal bir profil olsa bile, yalnızca bir yineleme profiliiçeren otomatik ölçeklendirme ayarı bu profili çalıştırZ. Aşağıdaki iki örnek, bu profilin nasıl kullanıldığını göstermektedir:

    **Örnek 1: Hafta içi ve hafta sonları**
    
    Diyelim ki hafta sonları maksimum kapasitenizin 4 olmasını istiyorsunuz. Hafta içi, daha fazla yük beklediğiniz için maksimum kapasitenizin 10 olmasını istersiniz. Bu durumda, ayarınızda biri hafta sonları, diğeri hafta içi çalışacak iki yineleme profili bulunur.
    Ayar şuna benzer:

    ``` JSON
    "profiles": [
    {
    "name": "weekdayProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }],
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Monday"
            ],
            "hours": [
                0
            ],
            "minutes": [
                0
            ]
        }
    }}
    },
    {
    "name": "weekendProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }]
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Saturday"
            ],
            "hours": [
                0
            ],
            "minutes": [
                0
            ]
        }
    }
    }]
    ```

    Önceki ayar, her yineleme profilinin bir zamanlamaya sahip olduğunu gösterir. Bu zamanlama, profilin ne zaman çalışmaya başlayacağını belirler. Profil, başka bir profil çalıştırma zamanı geldiğinde durur.

    Örneğin, önceki ayarda "weekdayProfile" Pazartesi günü saat 12:00'de başlayacak şekilde ayarlanmıştır. Bu da bu profilin Pazartesi günü saat 12:00'de çalışmaya başladığı anlamına gelir. "WeekendProfile"ın çalışmaya başlaması planlanan cumartesi günü saat 12:00'ye kadar devam ediyor.

    **Örnek 2: Çalışma saatleri**
    
    İş saatleri içinde bir metrik eşiğe (09:00-17:00) ve diğer tüm zamanlar için farklı bir eşik istediğinizi varsayalım. Ayar şuna benzerdi:
    
    ``` JSON
    "profiles": [
    {
    "name": "businessHoursProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }],
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Monday", “Tuesday”, “Wednesday”, “Thursday”, “Friday”
            ],
            "hours": [
                9
            ],
            "minutes": [
                0
            ]
        }
    }
    },
    {
    "name": "nonBusinessHoursProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }]
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Monday", “Tuesday”, “Wednesday”, “Thursday”, “Friday”
            ],
            "hours": [
                17
            ],
            "minutes": [
                0
            ]
        }
    }
    }]
    ```
    
    Önceki ayar, "businessHoursProfile"ın Pazartesi günü 09:00'da çalışmaya başladığını ve 17:00'ye kadar devam ettiğini gösterir. İşte o zaman "nonBusinessHoursProfile" çalışmaya başlar. "NonBusinessHoursProfile" Salı günü 09:00'a kadar çalışır ve ardından "businessHoursProfile" tekrar devreye girer. Bu, Cuma günü saat 17:00'ye kadar tekrarlar. Bu noktada, "nonBusinessHoursProfile" Pazartesi günü saat 9:00'a kadar çalışır.
    
> [!Note]
> Azure portalındaki Otomatik Ölçeklendirme kullanıcı arabirimi yineleme profilleri için bitiş saatlerini zorlar ve yinelenen profiller arasında Otomatik Ölçeklendirme ayarının varsayılan profilini çalıştırmaya başlar.
    
## <a name="autoscale-evaluation"></a>Otomatik ölçeklendirme değerlendirmesi
Otomatik Ölçeklendirme ayarlarının birden çok profili olabileceği ve her profilin birden çok metrik kuralı olabileceği göz önüne alındığında, Otomatik Ölçek ayarının nasıl değerlendirildiğini anlamak önemlidir. Otomatik Ölçeklendirme işi her çalıştığında, geçerli profili seçerek başlar. Ardından Otomatik Ölçek, en küçük ve en büyük değerleri ve profildeki tüm metrik kuralları değerlendirir ve ölçek eyleminin gerekli olup olmadığına karar verir.

### <a name="which-profile-will-autoscale-pick"></a>Autoscale hangi profili seçecek?

Otomatik ölçek profili seçmek için aşağıdaki sırayı kullanır:
1. İlk olarak, şimdi çalışacak şekilde yapılandırılan herhangi bir sabit tarih profilini arar. Varsa, Autoscale çalıştırın. Çalışması gereken birden çok sabit tarih profili varsa, Otomatik Ölçek ilkini seçer.
2. Sabit tarih profilleri yoksa, Otomatik Ölçek yineleme profillerine bakar. Yineleme profili bulunursa, çalıştırılır.
3. Sabit bir tarih veya yineleme profili yoksa, Otomatik Ölçek normal profili çalıştırZ.

### <a name="how-does-autoscale-evaluate-multiple-rules"></a>Otomatik Ölçek birden çok kuralı nasıl değerlendirir?

Otomatik ölçek hangi profilin çalıştırılacağını belirledikten sonra, profildeki tüm ölçeklendirme kurallarını değerlendirir (bunlar **yön = "Artış"** kuralıdır).

Bir veya daha fazla ölçeklendirme kuralı tetiklenirse, Otomatik Ölçek, bu kuralların her birinin **ölçek eylemi** tarafından belirlenen yeni kapasiteyi hesaplar. Daha sonra hizmet kullanılabilirliğini sağlamak için bu kapasitelerin maksimum ölçekler.

Örneğin, geçerli kapasitesi 10 olan sanal bir makine ölçeği kümesi olduğunu varsayalım. İki ölçeklendirme kuralı vardır: biri kapasiteyi yüzde 10 artıran, diğeri kapasiteyi 3 sayı artıran. İlk kural 11 yeni bir kapasite ile sonuçlanır ve ikinci kural 13 kapasite ile sonuçlanır. Hizmet kullanılabilirliğini sağlamak için, Otomatik Ölçek maksimum kapasiteyle sonuçlanan eylemi seçer, böylece ikinci kural seçilir.

Ölçeklendirme kuralları tetiklenmezse, Otomatik Ölçek tüm ölçekleme kurallarını değerlendirir **(yön = "Azaltın"** kuralı). Otomatik ölçeklendirme yalnızca tüm ölçekleme kuralları tetiklenirse ölçeklendirme eylemine neden olur.

Otomatik ölçek, bu kuralların her birinin **ölçek Eylem** tarafından belirlenen yeni kapasiteyi hesaplar. Daha sonra, hizmet kullanılabilirliğini sağlamak için bu kapasitelerin en fazla sınanması yla sonuçlanan ölçek eylemini seçer.

Örneğin, geçerli kapasitesi 10 olan sanal bir makine ölçeği kümesi olduğunu varsayalım. İki ölçek kuralı vardır: biri kapasiteyi yüzde 50 azaltan, diğeri kapasiteyi 3 sayı azaltan. İlk kural 5 yeni bir kapasite ile sonuçlanır ve ikinci kural 7 kapasite ile sonuçlanır. Hizmet kullanılabilirliğini sağlamak için, Otomatik Ölçek maksimum kapasiteyle sonuçlanan eylemi seçer, böylece ikinci kural seçilir.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdakilere atıfta bulunarak Otomatik Ölçeklendirme hakkında daha fazla bilgi edinin:

* [Otomatik ölçek'e genel bakış](../../azure-monitor/platform/autoscale-overview.md)
* [Azure Monitörotomatik ölçeklendirme ortak ölçümleri](../../azure-monitor/platform/autoscale-common-metrics.md)
* [En iyi Azure İzleyici otomatik ölçeklendirme yöntemleri](../../azure-monitor/platform/autoscale-best-practices.md)
* [E-posta ve webhook uyarı bildirimleri göndermek için otomatik ölçeklendirme eylemlerini kullanma](../../azure-monitor/platform/autoscale-webhook-email.md)
* [Otomatik ölçekLENDIRME REST API](https://msdn.microsoft.com/library/dn931953.aspx)

