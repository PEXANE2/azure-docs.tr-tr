---
title: Azure Güvenlik Duvarı günlüklerine ve ölçümlerine genel bakış
description: Güvenlik duvarı günlüklerini kullanarak Azure Güvenlik Duvarı'nı izleyebilirsiniz. Ayrıca etkinlik günlüklerini kullanarak Azure Güvenlik Duvarı kaynaklarıyla ilgili işlemleri denetleyebilirsiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 01/22/2020
ms.author: victorh
ms.openlocfilehash: 89c6700d5df3bcef1332121c3cf7d8f720fe054c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76315040"
---
# <a name="azure-firewall-logs-and-metrics"></a>Azure Güvenlik Duvarı günlükleri ve ölçümleri

Güvenlik duvarı günlüklerini kullanarak Azure Güvenlik Duvarı'nı izleyebilirsiniz. Ayrıca etkinlik günlüklerini kullanarak Azure Güvenlik Duvarı kaynaklarıyla ilgili işlemleri denetleyebilirsiniz.

Bu günlüklerden bazılarına portaldan erişebilirsiniz. Günlükler [Azure Monitor günlüklerine,](../azure-monitor/insights/azure-networking-analytics.md)Depolama ve Etkinlik Hub'larına gönderilebilir ve Azure Monitor günlüklerinde veya Excel ve Power BI gibi farklı araçlarla analiz edilebilir.

Ölçümler hafiftir ve neredeyse gerçek zamanlı senaryoları destekleyerek uyarı ve hızlı sorun algılama için kullanışlı hale getirir.

## <a name="diagnostic-logs"></a>Tanılama günlükleri

 Azure Güvenlik Duvarı'nda aşağıdaki tanılama günlükleri mevcuttur:

* **Uygulama kuralı günlüğü**

   Uygulama kural günlüğü bir depolama hesabına kaydedilir, Etkinlik hub'larına aktarılır ve/veya Azure Monitor günlüklerine yalnızca her Azure Güvenlik Duvarı için etkinleştirdiyseniz gönderilir. Yapılandırdığınız uygulama kurallarınızla eşleşen yeni bağlantılar kabul edilen/reddedilen bağlantı için bir günlük oluşturur. Veriler aşağıdaki örnekte gösterildiği gibi JSON biçiminde günlüğe kaydedilir:

   ```
   Category: application rule logs.
   Time: log timestamp.
   Properties: currently contains the full message.
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
   {
    "category": "AzureFirewallApplicationRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallApplicationRuleLog",
    "properties": {
        "msg": "HTTPS request from 10.1.0.5:55640 to mydestination.com:443. Action: Allow. Rule Collection: collection1000. Rule: rule1002"
    }
   }
   ```

* **Ağ kuralı günlüğü**

   Ağ kural günlüğü bir depolama hesabına kaydedilir, Etkinlik hub'larına aktarılır ve/veya Azure Monitor günlüklerine yalnızca her Azure Güvenlik Duvarı için etkinleştirdiyseniz gönderilir. Yapılandırdığınız ağ kurallarınızla eşleşen yeni bağlantılar kabul edilen/reddedilen bağlantı için bir günlük oluşturur. Veriler aşağıdaki örnekte gösterildiği gibi JSON biçiminde günlüğe kaydedilir:

   ```
   Category: network rule logs.
   Time: log timestamp.
   Properties: currently contains the full message.
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
  {
    "category": "AzureFirewallNetworkRule",
    "time": "2018-06-14T23:44:11.0590400Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallNetworkRuleLog",
    "properties": {
        "msg": "TCP request from 111.35.136.173:12518 to 13.78.143.217:2323. Action: Deny"
    }
   }

   ```

Günlüklerinizi depolamak için kullanabileceğiniz üç seçenek vardır:

* **Depolama hesabı**: Depolama hesaplarının en iyi kullanım amacı, günlüklerin uzun süre depolanması ve ihtiyaç duyulduğunda gözden geçirilmesi durumlarıdır.
* **Event Hubs**: Event Hubs, kaynaklarınızla ilgili uyarılar almak için diğer güvenlik bilgisi ve olay yönetimi (SEIM) araçlarıyla tümleştirmek için idealdir.
* **Azure Monitor günlükleri**: Azure Monitor günlükleri en iyi uygulamanızın genel gerçek zamanlı izlenmesi veya eğilimlere bakilmesi için kullanılır.

## <a name="activity-logs"></a>Etkinlik günlükleri

   Etkinlik günlüğü girişleri varsayılan olarak toplanır ve bunları Azure portalda görüntüleyebilirsiniz.

   Azure aboneliğinize gönderilen tüm işlemleri görüntülemek için [Azure etkinlik günlüklerini](../azure-resource-manager/management/view-activity-logs.md) (eski adıyla operasyonel günlükler ve denetim günlükleri olarak bilinir) kullanabilirsiniz.

## <a name="metrics"></a>Ölçümler

Azure Monitor'daki ölçümler, belirli bir anda bir sistemin bazı yönünü açıklayan sayısal değerlerdir. Ölçümler her dakika toplanır ve sık sık örneklenebileceğinden uyarı için yararlıdır. Bir uyarı nispeten basit bir mantıkla hızlı bir şekilde ateşlenebilir.

Azure Güvenlik Duvarı için aşağıdaki ölçümler kullanılabilir:

- **Uygulama kuralları sayısına isabet** - Bir uygulama kuralının kaç kez vurulduğu.

    Birim: sayısı

- **Ağ kuralları sayısına isabet -** Bir ağ kuralının kaç kez vurulduğu.

    Birim: sayısı

- **İşlenen veriler** - Güvenlik duvarında geçen veri miktarı.

    Birim: bayt

- **Güvenlik duvarı durumu durumu** - SNAT bağlantı noktası kullanılabilirliğine bağlı olarak güvenlik duvarının durumunu gösterir.

    Birim: yüzde

   Bu ölçüt iki boyutu vardır:
  - Durum: Olası değerler *Sağlıklı,* *Bozulmuş*, *Sağlıksız.*
  - Sebep: Güvenlik duvarının ilgili durumunun nedenini gösterir. 

     SNAT bağlantı noktaları %95 > kullanılırsa, tükenmiş olarak kabul edilir ler ve durum =**Bozulmuş** ve nedeni=**SNAT bağlantı noktası**ile sağlık %50'dir. Güvenlik duvarı trafiği işlemeye devam eder ve varolan bağlantılar etkilenmez. Ancak, yeni bağlantılar aralıklı olarak kurulamaz.

     SNAT bağlantı noktaları %95 < kullanılırsa, güvenlik duvarı sağlıklı kabul edilir ve sağlık %100 olarak gösterilir.

     SNAT bağlantı noktası kullanımı bildirilmemişse, sağlık %0 olarak gösterilir. 

- **SNAT bağlantı noktası kullanımı** - Güvenlik duvarı tarafından kullanılan SNAT bağlantı noktalarının yüzdesi.

    Birim: yüzde

   Güvenlik duvarınıza daha fazla genel IP adresi eklediğinizde, SNAT bağlantı noktaları kullanımını azaltarak daha fazla SNAT bağlantı noktası kullanılabilir. Ayrıca, güvenlik duvarı farklı nedenlerle (örneğin, CPU veya iş çıktısı) ek SNAT bağlantı noktaları da kullanılabilir hale geldiğinde. Böylece etkili bir şekilde, yalnızca hizmet ölçeklendirildiği için, herhangi bir genel IP adresi eklemeden SNAT bağlantı noktalarının belirli bir yüzdesi küçülebilir. Güvenlik duvarınızda bulunan bağlantı noktalarını artırmak için kullanılabilen genel IP adreslerinin sayısını doğrudan denetleyebilirsiniz. Ancak, güvenlik duvarı ölçekleme doğrudan kontrol edemez. Şu anda, SNAT bağlantı noktaları yalnızca ilk beş genel IP adresi için eklenir.   


## <a name="next-steps"></a>Sonraki adımlar

- Azure Güvenlik Duvarı günlüklerini ve ölçümlerini nasıl izleyeceğinizi öğrenmek için [Bkz.](tutorial-diagnostics.md)

- Azure Monitor'da ölçümler hakkında daha fazla bilgi edinmek için [Azure Monitor'daki Ölçümler'e](../azure-monitor/platform/data-platform-metrics.md)bakın.
