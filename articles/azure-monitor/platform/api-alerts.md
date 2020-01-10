---
title: Log Analytics uyarı REST API kullanma
description: Log Analytics uyarı REST API, Log Analytics bir parçası olan Log Analytics uyarı oluşturmanıza ve yönetmenize olanak sağlar.  Bu makalede, farklı işlemler gerçekleştirmek için API ve birkaç örnek ayrıntılarını sağlar.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/29/2018
ms.openlocfilehash: 7112f86ca123c66c5969236617f35fcb8d698030
ms.sourcegitcommit: a100e3d8b0697768e15cbec11242e3f4b0e156d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75680673"
---
# <a name="create-and-manage-alert-rules-in-log-analytics-with-rest-api"></a>Oluşturma ve REST API ile Log analytics'teki uyarı kurallarını yönet
Log Analytics uyarı REST API oluşturma ve Log analytics'teki uyarılar yönetmenize olanak sağlar.  Bu makalede, farklı işlemler gerçekleştirmek için API ve birkaç örnek ayrıntılarını sağlar.

> [!IMPORTANT]
> [Daha önce duyurulduğu](https://azure.microsoft.com/updates/switch-api-preference-log-alerts/)gibi, *1 Haziran 2019* tarihinden sonra oluşturulan Log Analytics çalışma alanı (ler), **yalnızca** Azure Scheduledqueryrules [REST API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/), [Azure Resource mananger şablonu](../../azure-monitor/platform/alerts-log.md#managing-log-alerts-using-azure-resource-template) ve [PowerShell cmdlet 'ini](../../azure-monitor/platform/alerts-log.md#managing-log-alerts-using-powershell)kullanarak uyarı kurallarını yönetebilecektir. Müşteriler, daha eski çalışma alanları için [tercih edilen uyarı kuralı yönetimi kullanımını](../../azure-monitor/platform/alerts-log-api-switch.md#process-of-switching-from-legacy-log-alerts-api) kolayca değiştirebilir ve yerel PowerShell cmdlet 'lerini kullanma yeteneği, kurallarda daha fazla geçiş süresi dönemi ve ayrı kaynak grubunda veya abonelikte kural oluşturma gibi birçok [yeni avantaj](../../azure-monitor/platform/alerts-log-api-switch.md#benefits-of-switching-to-new-azure-api) elde edebilir.

Log Analytics arama REST API, RESTful olduğu ve Azure Resource Manager REST API aracılığıyla erişilebilir. Bu belgede, API'yi kullanarak bir PowerShell komut satırı burada erişilen örnekler bulabilirsiniz [ARMClient](https://github.com/projectkudu/ARMClient), Azure Resource Manager API'si çağırma basitleştiren bir açık kaynak komut satırı aracı. PowerShell ile ARMClient ve Log Analytics arama API'sine erişmek için birçok seçenekten birini kullanılır. Bu araçlarla, Log Analytics çalışma alanları çağrı yapmak ve bunların içindeki arama komutları gerçekleştirmek için RESTful Azure Resource Manager API'si kullanabilir. API, birçok farklı şekilde, program aracılığıyla arama sonuçlarını kullanmanıza olanak sağlayan, arama sonuçları JSON biçiminde çıkarır.

## <a name="prerequisites"></a>Ön koşullar
Şu anda ile kayıtlı bir aramayı Log analytics'te uyarıları yalnızca oluşturulabilir.  Başvurabilirsiniz [Log Search REST API'sine](../../azure-monitor/log-query/log-query-overview.md) daha fazla bilgi için.

## <a name="schedules"></a>Zamanlamalar
Kayıtlı bir aramayı bir veya daha fazla zamanlama olabilir. Ne sıklıkta arama çalıştırma ve zaman aralığı üzerinde ölçütler tanımlanmıştır, zamanlamayı tanımlar.
Zamanlamaları aşağıdaki tabloda özelliklere sahiptir.

| Özellik | Açıklama |
|:--- |:--- |
| Interval |Arama ne kadar sıklıkla çalışır. Birkaç dakika içinde ölçülür. |
| QueryTimeSpan |Zaman aralığı üzerinde ölçüt değerlendirme. Aralık değerinden büyük veya eşit olmalıdır. Birkaç dakika içinde ölçülür. |
| Sürüm |API sürümü kullanılıyor.  Şu anda, bu her zaman 1 olarak ayarlanması gerekir. |

Örneğin, 15 dakikalık bir aralığı ve bir zaman aralığı 30 dakika ile olay sorgusu göz önünde bulundurun. Bu durumda, sorgu 15 dakikada bir çalışır ve doğru üzerinden çözmek ölçütleri devam bir uyarı tetiklenecek 30 dakikalık aralık.

### <a name="retrieving-schedules"></a>Zamanlamalar alınıyor
Kayıtlı arama için tüm zamanlamalar almak için Get yöntemini kullanın.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Get yöntemi, belirli bir zamanlama için kayıtlı bir aramayı almak için bir zamanlama kimliği ile kullanın.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Bir zamanlama için bir örnek yanıt aşağıdadır.

```json
{
    "value": [{
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/sampleRG/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
            "Interval": 15,
            "QueryTimeSpan": 15,
            "Enabled": true,
        }
    }]
}
```

### <a name="creating-a-schedule"></a>Bir zamanlama oluşturma
Put yöntemi yeni bir zamanlama oluşturmak için bir benzersiz zamanlama kimliği ile kullanın.  Farklı kaydedilmiş aramalarla ilişkilendirilseler de iki zamanlama aynı KIMLIĞE sahip olamaz.  Log Analytics konsolda bir zamanlama oluşturmak, zamanlama kimliği için bir GUID oluşturulur

> [!NOTE]
> Tüm kayıtlı aramalar, çizelgeler ve günlük analizi API'si ile oluşturulan eylem adını, küçük olmalıdır.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Enabled':'true' } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Bir zamanlama düzenleme
Bu zamanlamayı değiştirmek için aynı kayıtlı arama için mevcut bir zamanlama KIMLIĞIYLE put metodunu kullanın; Aşağıdaki örnekte zamanlamanın devre dışı bırakılması. İsteğin gövdesi, zamanlamanın *ETag* öğesini içermelidir.

      $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Enabled':'false' } }"
      armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Zamanlama siliniyor
Delete yöntemi bir zamanlama kimliği ile bir zamanlamayı silmek için kullanın.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Eylemler
Bir zamanlama birden fazla eylem olabilir. Posta gönderme veya bir runbook başlatma gibi gerçekleştirmek için bir veya daha fazla işlem bir eylem tanımlayabilir veya ne zaman bir aramanın sonuçları bazı ölçütlerle eşleşen belirleyen bir eşiği tanımlayabilir.  Eşiğine ulaşıldığında işlemleri gerçekleştirilir böylece bazı eylemler her ikisi de tanımlayın.

Tüm eylemler aşağıdaki tabloda özelliklere sahiptir.  Farklı uyarı türleri aşağıda açıklanan farklı ek özellikler vardır.

| Özellik | Açıklama |
|:--- |:--- |
| `Type` |Eylem türü.  Şu anda uyarı ve Web kancası olası değerler şunlardır. |
| `Name` |Uyarı görünen adı. |
| `Version` |API sürümü kullanılıyor.  Şu anda, bu her zaman 1 olarak ayarlanması gerekir. |

### <a name="retrieving-actions"></a>Eylemleri alınıyor

Bir zamanlama için tüm eylemleri almak için Get yöntemini kullanın.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Get yöntemi, bir zamanlama için belirli bir eylemi almak için eylem kimliği ile kullanın.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Oluşturma veya düzenleme eylemleri
Yeni bir eylem oluşturmak için zamanlama için benzersiz olan bir eylem kimliği ile Put yöntemini kullanın.  Log Analytics konsolunda bir eylem oluşturduğunuzda, eylem kimliği için bir GUID değeridir.

> [!NOTE]
> Tüm kayıtlı aramalar, çizelgeler ve günlük analizi API'si ile oluşturulan eylem adını, küçük olmalıdır.

Bu zamanlamayı değiştirmek için var olan bir eylem kimliği aynı kayıtlı arama için Put yöntemini kullanın.  İstek gövdesi, zamanlama etag'i içermesi gerekir.

Aşağıdaki bölümlerde bu örnekler sağlanır, yeni bir eylem oluşturmak için istek biçimi eylem türüne göre değişir.

### <a name="deleting-actions"></a>Eylemler siliniyor

Delete yöntemi eylem kimliği ile bir eylemi silmek için kullanın.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Uyarı eylemleri
Bir ve yalnızca bir uyarı eylemi bir zamanlamaya sahip olmalıdır.  Uyarı eylemleri bir veya daha fazla aşağıdaki tabloda bölümler var.  Her daha aşağıda ayrıntılı olarak açıklanmıştır.

| Section | Açıklama | Kullanım |
|:--- |:--- |:--- |
| Eşik |Eylem çalıştırıldığında ölçütleri.| Her uyarı için önce veya sonra Azure'a genişletilmiş olan gereklidir. |
| Önem Derecesi |Uyarı tetiklendiğinde sınıflandırmak için kullanılan etiketi belirtin.| Her uyarı için önce veya sonra Azure'a genişletilmiş olan gereklidir. |
| Gizle |Uyarı bildirimleri durdurmak için seçenek. | Her uyarı için isteğe bağlı önce veya sonra Azure'a genişletildi. |
| Eylem Grupları |Azure burada gerekli Eylemler belirtilmiştir, e-postalar, SMSs, sesli aramalar, Web kancaları, Otomasyon runbook'ları, ITSM bağlayıcılar, vb. gibi - ActionGroup kimlikleri.| Uyarılar Azure'a genişletilmiş olan sonra gerekli|
| Eylemleri Özelleştirin|ActionGroup select eylemler için standart çıktı değiştirme| Her uyarı için isteğe bağlı kullanılabilir sonra uyarılar Azure'a genişletilir. |

### <a name="thresholds"></a>Eşikler
Bir uyarı eylemi, yalnızca tek bir eşik değeri olması gerekir.  Kayıtlı arama sonuçlarını bu arama ile ilişkili bir eylem Eşikte eşleştiğinde, ardından bu eylemi diğer tüm işlemler çalıştırılır.  Böylece eşikleri içermeyen diğer tür Eylemler ile kullanılabilmesi için bir eylem yalnızca bir eşiği de içerebilir.

Eşikleri aşağıdaki tabloda özelliklere sahiptir.

| Özellik | Açıklama |
|:--- |:--- |
| `Operator` |Eşik karşılaştırması için işleci. <br> gt büyük = <br> lt = kısa |
| `Value` |Eşik değeri. |

Örneğin, bir olay sorgusu ile 15 dakika, 30 dakikalık bir zaman aralığı ve bir eşik 10'dan büyük bir aralıkta göz önünde bulundurun. Bu durumda, sorgu 15 dakikada bir çalışır ve 30 dakikalık bir aralığı oluşturulan 10 olayları döndürdüyse bir uyarı tetiklenmesi.

Örnek yanıt yalnızca bir eşik ile bir eylem için aşağıda verilmiştir.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

Put yöntemi yeni bir eşik eylem için bir zamanlama oluşturmak için bir benzersiz bir eylem kimliği ile kullanın.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Put yöntemi, bir zamanlama için bir eşik eylem değiştirmek için var olan bir eylem kimliği ile kullanın.  İstek gövdesi, eylemin etag içermesi gerekir.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="severity"></a>Önem Derecesi
Log Analytics, daha kolay yönetim ve Önceliklendirme izin vermek için kategoriler halinde uyarılarınızı sınıflandırmak sağlar. Tanımlanan uyarı önem derecesi: bilgilendirme, uyarı ve kritik. Bunlar Azure Uyarıları ' normalleştirilmiş önem derecesi ölçeğini eşlenir:

|Log Analytics önem düzeyi  |Azure Uyarıları önem düzeyi  |
|---------|---------|
|`critical` |Önem Derecesi 0|
|`warning` |Önem Derecesi 1|
|`informational` | Önem Derecesi 2|

Örnek yanıt yalnızca eşiğini ve önem derecesine sahip bir eylem için aşağıda verilmiştir. 

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Severity": "critical",
        "Version": 1    }

Önem derecesine sahip yeni bir eylem için bir zamanlama oluşturmak için benzersiz işlem Kimliğine sahip Put yöntemini kullanın.  

    $thresholdWithSevJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1','Severity': 'critical', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdWithSevJson

Put yöntemi, bir zamanlama için bir önem derecesi eylem değiştirmek için var olan bir eylem kimliği ile kullanın.  İstek gövdesi, eylemin etag içermesi gerekir.

    $thresholdWithSevJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1','Severity': 'critical', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdWithSevJson

#### <a name="suppress"></a>Gizle
Log Analytics uyarı eşiği karşılanmadığından veya aşıldığından her zaman ateşlenir sorgu tabanlı. Sorguda kapsanan mantıksal bağlı olarak, bu uyarı bir dizi aralıkları için tetiklendi neden olabilir ve bu nedenle bildirimleri de gönderilen sürekli olarak. Bu senaryoyu engellemek için bir kullanıcı bir görünürlüğe süre bildirim uyarı kuralı ikinci kez tetiklenir önce beklenecek Log Analytics söyleyen gösterme seçeneği ayarlayabilirsiniz. Dolayısıyla bastır 30 dakika boyunca; ayarlayın ardından uyarı ilk defa at ve yapılandırılmış bildirimler gönderin. Ancak daha sonra 30 uyarı kuralı için bildirimi yeniden kullanılmadan önce dakika bekleyin. Ara dönemde uyarı kuralı çalışmaya devam edecek - yalnızca bildirim için uyarı kuralı bu dönemde harekete kaç kez bakılmaksızın belirtilen zaman Log Analytics tarafından bastırılır.

Log Analytics uyarı kuralı kullanarak belirtilen özelliği Gizle *azaltma* değeri ve gizleme dönem kullanarak *Dakika Cinsiden Süre* değeri.

Yalnızca eşik, önem derecesi ve gizleme özelliği olan bir eylem için örnek yanıt aşağıda verilmiştir

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Throttling": {
          "DurationInMinutes": 30
        },
        "Severity": "critical",
        "Version": 1    }

Önem derecesine sahip yeni bir eylem için bir zamanlama oluşturmak için benzersiz işlem Kimliğine sahip Put yöntemini kullanın.  

    $AlertSuppressJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1','Severity': 'critical', 'Type':'Alert', 'Throttling': { 'DurationInMinutes': 30 },'Threshold': { 'Operator': 'gt', 'Value': 10 } } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myalert?api-version=2015-03-20 $AlertSuppressJson

Put yöntemi, bir zamanlama için bir önem derecesi eylem değiştirmek için var olan bir eylem kimliği ile kullanın.  İstek gövdesi, eylemin etag içermesi gerekir.

    $AlertSuppressJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1','Severity': 'critical', 'Type':'Alert', 'Throttling': { 'DurationInMinutes': 30 },'Threshold': { 'Operator': 'gt', 'Value': 10 } } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myalert?api-version=2015-03-20 $AlertSuppressJson

#### <a name="action-groups"></a>Eylem Grupları
Azure'daki tüm uyarılar eylemlerini işleyen varsayılan bir mekanizma olarak eylem grubu kullanın. Eylem grubu ile bir kez eylemleri belirtin ve birden çok uyarı - eylem grubuna Azure genelinde ilişkilendirin. Tekrar tekrar aynı eylemleri tekrar tekrar bildirme gerek kalmadan. Eylem grupları, birden fazla eylem - e-posta, SMS, sesli arama, ITSM bağlantısı, Otomasyon Runbook'u, Web kancası URI ve benzeri destekler. 

Uyarıları Azure 'a genişletmiş olan kullanıcılar için bir zamanlama, bir uyarı oluşturabilmek için artık bir zamanlayıcı ile birlikte Işlem grubu ayrıntılarının geçirilmesini sağlamalıdır. E-posta ayrıntıları, Web kancası URL'leri, Runbook Otomasyon ayrıntıları ve diğer eylemler olması gereken bir eylem grubu ilk önce bir uyarı; oluşturma tarafta tanımlanan bir izin oluşturabilir [Azure İzleyici'eylem grubundan](../../azure-monitor/platform/action-groups.md) portalı veya [eylem grubu API](https://docs.microsoft.com/rest/api/monitor/actiongroups).

Bir uyarı eylem grubu ilişkisini eklemek için uyarı tanımında eylem grubu benzersiz Azure Resource Manager Kimliğini belirtin. Bir örnek resimde, aşağıda verilmiştir:

     "etag": "W/\"datetime'2017-12-13T10%3A52%3A21.1697364Z'\"",
      "properties": {
        "Type": "Alert",
        "Name": "test-alert",
        "Description": "I need to put a description here",
        "Threshold": {
          "Operator": "gt",
          "Value": 12
        },
        "AzNsNotification": {
          "GroupIds": [
            "/subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup"
          ]
        },
        "Severity": "critical",
        "Version": 1
      },

Put yöntemini, zaten mevcut olan eylem grubu için bir zamanlama ilişkilendirmek için bir benzersiz bir eylem kimliği ile kullanın.  Kullanım örneği bir gösterimi aşağıda verilmiştir.

    $AzNsJson = "{'properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup']} } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

Put yöntemi ile var olan bir eylem kimliği için bir zamanlama ilişkili bir eylem grubu değiştirmek için kullanın.  İstek gövdesi, eylemin etag içermesi gerekir.

    $AzNsJson = "{'etag': 'datetime'2017-12-13T10%3A52%3A21.1697364Z'\"', 'properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': { 'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup'] } } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

#### <a name="customize-actions"></a>Eylemleri Özelleştirin
Varsayılan eylemler tarafından standart şablon ve bildirimler için biçim izleyin. Ancak Eylem grupları tarafından kontrol edilir olsa bile, kullanıcı bazı eylemler özelleştirebilirsiniz. Şu anda, e-posta konusu ve Web kancası yükü özelleştirme mümkündür.

##### <a name="customize-e-mail-subject-for-action-group"></a>Eylem grubu için e-posta konusu özelleştirme
Varsayılan olarak, uyarılar için e-posta konusu olduğu: uyarı bildirimi `<AlertName>` için `<WorkspaceName>`. Ancak, belirli sözcükleri ya da kolayca filtre kuralları, gelen kutunuzdaki görevlendirmek olanak tanımak için etiketleri - böylece bu, özelleştirilebilir. Özelleştirme e-posta üst bilgisi ayrıntıları aşağıdaki örnekte olduğu gibi ActionGroup ayrıntılarını göndermeniz gerekir.

     "etag": "W/\"datetime'2017-12-13T10%3A52%3A21.1697364Z'\"",
      "properties": {
        "Type": "Alert",
        "Name": "test-alert",
        "Description": "I need to put a description here",
        "Threshold": {
          "Operator": "gt",
          "Value": 12
        },
        "AzNsNotification": {
          "GroupIds": [
            "/subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup"
          ],
          "CustomEmailSubject": "Azure Alert fired"
        },
        "Severity": "critical",
        "Version": 1
      },

Put yöntemini bir benzersiz bir eylem kimliği ile zaten mevcut olan eylem grubu için bir zamanlama özelleştirme ilişkilendirmek için kullanın.  Kullanım örneği bir gösterimi aşağıda verilmiştir.

    $AzNsJson = "{'properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup'], 'CustomEmailSubject': 'Azure Alert fired'} } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

Put yöntemi ile var olan bir eylem kimliği için bir zamanlama ilişkili bir eylem grubu değiştirmek için kullanın.  İstek gövdesi, eylemin etag içermesi gerekir.

    $AzNsJson = "{'etag': 'datetime'2017-12-13T10%3A52%3A21.1697364Z'\"', 'properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup']}, 'CustomEmailSubject': 'Azure Alert fired' } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

##### <a name="customize-webhook-payload-for-action-group"></a>Eylem grubu için Web kancası yükü özelleştirme
Varsayılan olarak, log analytics için eylem grubu aracılığıyla gönderilen Web kancası sabit yapısı vardır. Ancak, Web kancası uç noktası gereksinimlerini karşılamak için desteklenen belirli değişkenlerini kullanarak bir JSON yükü özelleştirebilirsiniz. Daha fazla bilgi için [günlük uyarı kuralları için Web kancası eylemi](../../azure-monitor/platform/alerts-log-webhook.md). 

ActionGroup ayrıntılarını birlikte göndermek ve Özelleştir Web kancası ayrıntıları gereken tüm Web kancası eylem grubu içinde; belirtilen URI uygulanabilir Aşağıdaki örnekte olduğu gibi.

     "etag": "W/\"datetime'2017-12-13T10%3A52%3A21.1697364Z'\"",
      "properties": {
        "Type": "Alert",
        "Name": "test-alert",
        "Description": "I need to put a description here",
        "Threshold": {
          "Operator": "gt",
          "Value": 12
        },
        "AzNsNotification": {
          "GroupIds": [
            "/subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup"
          ],
          "CustomWebhookPayload": "{\"field1\":\"value1\",\"field2\":\"value2\"}",
          "CustomEmailSubject": "Azure Alert fired"
        },
        "Severity": "critical",
        "Version": 1
      },

Put yöntemini bir benzersiz bir eylem kimliği ile zaten mevcut olan eylem grubu için bir zamanlama özelleştirme ilişkilendirmek için kullanın.  Kullanım örneği bir gösterimi aşağıda verilmiştir.

    $AzNsJson = "{'properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup'], 'CustomEmailSubject': 'Azure Alert fired','CustomWebhookPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}'} } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

Put yöntemi ile var olan bir eylem kimliği için bir zamanlama ilişkili bir eylem grubu değiştirmek için kullanın.  İstek gövdesi, eylemin etag içermesi gerekir.

    $AzNsJson = "{'etag': 'datetime'2017-12-13T10%3A52%3A21.1697364Z'\"', 'properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup']}, 'CustomEmailSubject': 'Azure Alert fired','CustomWebhookPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}' } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson


## <a name="next-steps"></a>Sonraki adımlar

* Kullanım [günlük aramaları yapmak için REST API](../../azure-monitor/log-query/log-query-overview.md) Log analytics'te.
* [Azure izleyici 'de günlük uyarıları](../../azure-monitor/platform/alerts-unified-log.md) hakkında bilgi edinin
* [Azure izleyici 'de günlük uyarı kuralları oluşturma, düzenleme veya yönetme](../../azure-monitor/platform/alerts-log.md)

