---
title: Azure Sanal Makineleri Kullanarak Gelişmiş Otomatik Ölçeklendirme
description: E-posta gönderen ve ölçek eylemleriyle webhook URL'lerini arayan birden çok kural ve profiliçeren Kaynak Yöneticisi ve VM Ölçek Kümeleri kullanır.
ms.topic: conceptual
ms.date: 02/22/2016
ms.subservice: autoscale
ms.openlocfilehash: e22806ff94ce2eb830bb6918bfc7f80e5ad3ba0a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75364229"
---
# <a name="advanced-autoscale-configuration-using-resource-manager-templates-for-vm-scale-sets"></a>VM Ölçek Kümeleri için Kaynak Yöneticisi şablonlarını kullanarak gelişmiş otomatik ölçek yapılandırması
Performans metrik eşiklerine, yinelenen bir zamanlamaya veya belirli bir tarihe göre Sanal Makine Ölçeği Kümeleri'nde ölçeklendirilebilir ve ölçeklendirebilirsiniz. Ayrıca, ölçek eylemleri için e-posta ve webhook bildirimlerini yapılandırabilirsiniz. Bu gözden geçirme, VM Ölçeği Kümesi'nde Kaynak Yöneticisi şablonu kullanarak tüm bu nesneleri yapılandırmaya bir örnek gösterir.

> [!NOTE]
> Bu izlenme, VM Ölçek Kümeleri için adımları açıklarken, aynı bilgiler [Bulut Hizmetleri,](https://azure.microsoft.com/services/cloud-services/) [Uygulama Hizmeti - Web Uygulamaları](https://azure.microsoft.com/services/app-service/web/)ve [API Yönetimi hizmetlerini](https://docs.microsoft.com/azure/api-management/api-management-key-concepts) otomatik leştirmek için geçerlidir CPU gibi basit bir performans ölçümüne dayalı vm ölçeği kümesinde basit bir ölçek açma/çıkış ayarı için [Linux](../../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-cli.md) ve [Windows](../../virtual-machine-scale-sets/tutorial-autoscale-powershell.md) belgelerine bakın
>
>

## <a name="walkthrough"></a>Kılavuz
Bu izbarada, bir ölçek kümesi için otomatik ölçek ayarını yapılandırmak ve güncelleştirmek için [Azure Kaynak Gezgini'ni](https://resources.azure.com/) kullanırız. Azure Kaynak Gezgini, Kaynak Yöneticisi şablonları aracılığıyla Azure kaynaklarını yönetmenin kolay bir yoludur. Azure Kaynak Gezgini aracında yeniyseniz, [bu girişi](https://azure.microsoft.com/blog/azure-resource-explorer-a-new-tool-to-discover-the-azure-api/)okuyun.

1. Temel otomatik ölçek ayarı içeren yeni bir ölçek kümesi dağıtın. Bu makalede, temel otomatik ölçek şablonu içeren bir Windows ölçeği kümesi bulunan Azure QuickStart Galerisi'ndeki nikullanır. Linux ölçeği setleri aynı şekilde çalışır.
2. Ölçek kümesi oluşturulduktan sonra, Azure Kaynak Gezgini'nden ölçek kümesi kaynağına gidin. Microsoft.Insights düğümü altında aşağıdakileri görürsünüz.

    ![Azure Gezgini](media/autoscale-virtual-machine-scale-sets/azure_explorer_navigate.png)

    Şablon yürütme **adı 'autoscalewad'** ile varsayılan bir otomatik ölçek ayarı oluşturdu. Sağ tarafta, bu otomatik ölçek ayarının tam tanımını görüntüleyebilirsiniz. Bu durumda, varsayılan otomatik ölçek ayarı CPU% tabanlı ölçeklendirme ve ölçek-in kuralı ile birlikte gelir.  

3. Artık zamanlamaya veya belirli gereksinimlere göre daha fazla profil ve kural ekleyebilirsiniz. Üç profilli otomatik ölçeklendirme ayarı oluşturuyoruz. Otomatik ölçekteki profilleri ve kuralları anlamak için Otomatik Ölçekle En İyi Uygulamaları gözden [geçirin.](autoscale-best-practices.md)  

    | Profiller & Kuralları | Açıklama |
    |--- | --- |
    | **Profil** |**Performans/metrik tabanlı** |
    | Kural |Servis Veri Günü Sıra İletisayısı > x |
    | Kural |Servis Veri Servisi Sıra İletisayısı < y |
    | Kural |CPU% > n |
    | Kural |CPU% < p |
    | **Profil** |**Hafta içi sabah saatleri (kural yok)** |
    | **Profil** |**Ürün Lansman günü (kural yok)** |

4. İşte bu geçiş için kullandığımız varsayımsal bir ölçekleme senaryosu.

   * **Yük tabanlı** - Ölçek setimde barındırılan uygulamamdaki yüke göre veya ölçeklendirmek istiyorum.*
   * **İleti Sırası boyutu** - Uygulamama gelen iletiler için Bir Hizmet Veri Servisi Kuyruğu kullanırım. Sıranın ileti sayısını ve CPU'yu kullanıyorum ve ileti sayısı veya CPU eşleğine ulaştığında bir ölçek eylemini tetiklemek için varsayılan profili yapılandırıyorum.\*
   * **Hafta nın ve günün saati** - 'Weekday Morning Hours' adlı haftalık yinelenen 'günün saati' tabanlı profil istiyorum. Geçmiş verilere dayanarak, bu süre içinde uygulamamın yükünü işlemek için belirli sayıda VM örneğine sahip olmak daha iyidir.\*
   * **Özel Tarihler** - Bir 'Ürün Lansman Günü' profili ekledim. Uygulamamın yük nedeniyle pazarlama duyuruları işlemek için hazır ve biz uygulamaya yeni bir ürün koymak böylece belirli tarihler için önceden plan.\*
   * *Son iki profil, içlerinde başka performans ölçümü tabanlı kurallar da olabilir. Bu durumda, ben bir ve bunun yerine varsayılan performans metrik tabanlı kurallara güvenmek için karar verdi. Kurallar yinelenen ve tarih tabanlı profiller için isteğe bağlıdır.*

     Otomatik ölçeklendirme motorunun profilleri ve kuralları önceliklendirmesi de [otomatikleştirme en iyi uygulamalar](autoscale-best-practices.md) makalesinde yakalanır.
     Otomatik ölçeklendirme için ortak ölçümlerin listesi [için, Otomatik Ölçek için Ortak ölçümlere](autoscale-common-metrics.md) bakın

5. Kaynak Gezgini'nde **Okuma/Yazma** modunda olduğundan emin olun

    ![Otomatik ölçeklendirme, varsayılan otomatik ölçeklendirme ayarı](media/autoscale-virtual-machine-scale-sets/autoscalewad.png)

6. Düzenle’ye tıklayın. Otomatik ölçek ayarındaki 'profiller' öğesini aşağıdaki yapılandırmayla **değiştirin:**

    ![Profil](media/autoscale-virtual-machine-scale-sets/profiles.png)

    ```
    {
            "name": "Perf_Based_Scale",
            "capacity": {
              "minimum": "2",
              "maximum": "12",
              "default": "2"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "MessageCount",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 10
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
                  "metricName": "MessageCount",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 3
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "Percentage CPU",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/<this_vmss_name>",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT30M",
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
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/<this_vmss_name>",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT30M",
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
          },
          {
            "name": "Weekday_Morning_Hours_Scale",
            "capacity": {
              "minimum": "4",
              "maximum": "12",
              "default": "4"
            },
            "rules": [],
            "recurrence": {
              "frequency": "Week",
              "schedule": {
                "timeZone": "Pacific Standard Time",
                "days": [
                  "Monday",
                  "Tuesday",
                  "Wednesday",
                  "Thursday",
                  "Friday"
                ],
                "hours": [
                  6
                ],
                "minutes": [
                  0
                ]
              }
            }
          },
          {
            "name": "Product_Launch_Day",
            "capacity": {
              "minimum": "6",
              "maximum": "20",
              "default": "6"
            },
            "rules": [],
            "fixedDate": {
              "timeZone": "Pacific Standard Time",
              "start": "2016-06-20T00:06:00Z",
              "end": "2016-06-21T23:59:00Z"
            }
          }
    ```
    Desteklenen alanlar ve değerleri için, [Autoscale REST API belgelerine](https://msdn.microsoft.com/library/azure/dn931928.aspx)bakın. Artık otomatik ölçek ayarınız daha önce açıklanan üç profili içerir.

7. Son olarak, Otomatik **Ölçekbildirim** bölümüne bakın. Otomatik ölçeklendirme bildirimleri, bir ölçeklendirme veya eylem başarılı bir şekilde tetiklendiğinde üç şey yapmanıza olanak sağlar.
   - Aboneliğinizin yönetici ve yardımcı yöneticilerine bildirin
   - Bir dizi kullanıcıya e-posta gönder
   - Bir webhook aramasını tetikle. Bu webhook, ateşlendiğinde, otomatik ölçeklendirme durumu ve ölçek kümesi kaynağı hakkında meta veriler gönderir. Otomatik ölçekwebhook yükü hakkında daha fazla bilgi için, [Webhook & Otomatik Ölçek için E-posta Bildirimleri Yapılandırma bölümüne](autoscale-webhook-email.md)bakın.

   Değeri null olan **bildirim** öğenizin yerine Otomatik Ölçek lendirme ayarına aşağıdakileri ekleyin

   ```
   "notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": true,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]

   ```

   Otomatik ölçek ayarını güncelleştirmek için Kaynak Gezgini'nde **Koy** düğmesine basın.

Birden çok ölçek profili ve ölçek bildirimleri içerecek şekilde VM Ölçeği kümesinde otomatik ölçek ayarını güncelleştirmişsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
Otomatik ölçekleme hakkında daha fazla bilgi edinmek için bu bağlantıları kullanın.

[Sanal Makine Ölçek Setleri ile TroubleShoot Otomatik Ölçeklendirme](../../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)

[Otomatik Ölçeklendirme için Ortak Ölçümler](autoscale-common-metrics.md)

[Azure Otomatik Ölçeklendirme için En İyi Uygulamalar](autoscale-best-practices.md)

[PowerShell'i kullanarak Otomatik Ölçeklendirmeyi Yönet](../../azure-monitor/platform/powershell-quickstart-samples.md#create-and-manage-autoscale-settings)

[CLI kullanarak Otomatik Ölçeklendirmeyi Yönetme](cli-samples.md#autoscale)

[Webhook & E-posta Bildirimlerini Otomatik Ölçeklendirme için yapılandırın](autoscale-webhook-email.md)

[Microsoft.Insights/autoscalesettings](/azure/templates/microsoft.insights/autoscalesettings) şablon başvurusu
