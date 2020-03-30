---
title: Günlük Analizi Niyokası'nı Kullanma
description: Log Analytics Alert REST API, Log Analytics'in bir parçası olan Log Analytics'te uyarılar oluşturmanıza ve yönetmenize olanak tanır.  Bu makalede, API ayrıntıları ve farklı işlemleri gerçekleştirmek için çeşitli örnekler sağlar.
ms.subservice: logs
ms.topic: conceptual
ms.date: 07/29/2018
ms.openlocfilehash: a85dad2ba638505233e5df769e55fa5bd7b8dafd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77665009"
---
# <a name="create-and-manage-alert-rules-in-log-analytics-with-rest-api"></a>REST API ile Log Analytics'te uyarı kuralları oluşturma ve yönetme 

Log Analytics Alert REST API, Log Analytics'te uyarılar oluşturmanıza ve yönetmenize olanak tanır.  Bu makalede, API ayrıntıları ve farklı işlemleri gerçekleştirmek için çeşitli örnekler sağlar.

> [!IMPORTANT]
> [Daha önce duyurulduğu](https://azure.microsoft.com/updates/switch-api-preference-log-alerts/)gibi, 1 Haziran *2019'dan* sonra oluşturulan günlük analitik çalışma alanı(lar) **yalnızca** Azure scheduledQueryRules [REST API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/), [Azure Resource Mananger Template](../../azure-monitor/platform/alerts-log.md#managing-log-alerts-using-azure-resource-template) ve [PowerShell cmdlet](../../azure-monitor/platform/alerts-log.md#managing-log-alerts-using-powershell)kullanarak uyarı kurallarını yönetebilecektir. Müşteriler, azure monitor scheduledQueryRules'dan varsayılan olarak yararlanmak ve yerel PowerShell cmdlets'i kullanma becerisi, kurallarda artan geri dönüş süresi, ayrı kaynak grubunda veya abonelikte kurallar oluşturulması ve çok daha fazlası gibi birçok [yeni avantaj](../../azure-monitor/platform/alerts-log-api-switch.md#benefits-of-switching-to-new-azure-api) elde etmek için eski çalışma alanları için tercih ettikleri uyarı kuralı yönetimi araçlarını kolayca [değiştirebilirler.](../../azure-monitor/platform/alerts-log-api-switch.md#process-of-switching-from-legacy-log-alerts-api)

Log Analytics Search REST API yeniden kullanılabilir ve Azure Kaynak Yöneticisi REST API üzerinden erişilebilir. Bu belgede, Azure Kaynak Yöneticisi API'sini başlatmayı kolaylaştıran açık kaynak kodlu komut satırı aracı [ARMClient'ı](https://github.com/projectkudu/ARMClient)kullanarak API'ye PowerShell komut satırından erişilen örnekler bulacaksınız. ARMClient ve PowerShell'in kullanımı, Log Analytics Search API'sine erişmek için birçok seçenekten biridir. Bu araçlarla, Yeniden Kullanılabilen Azure Kaynak Yöneticisi API'sini kullanarak Analytics'i günlüğe kaydedebilir ve bunların içinde arama komutları gerçekleştirebilirsiniz. API, arama sonuçlarını programlı olarak birçok farklı şekilde kullanmanıza olanak tanıyan Arama sonuçlarını JSON biçiminde çıktına göre elde edecektir.

## <a name="prerequisites"></a>Ön koşullar
Şu anda, uyarılar yalnızca Log Analytics'te kaydedilmiş bir aramayla oluşturulabilir.  Daha fazla bilgi için [Log Search REST API'sine](../../azure-monitor/log-query/log-query-overview.md) başvurabilirsiniz.

## <a name="schedules"></a>Zamanlamalar
Kaydedilen bir aramanın bir veya daha fazla zamanlaması olabilir. Zamanlama, aramanın ne sıklıkta çalıştırıldığını ve ölçütlerin tanımlandığı zaman aralığını tanımlar.
Zamanlamaların özellikleri aşağıdaki tabloda dır.

| Özellik | Açıklama |
|:--- |:--- |
| Interval |Aramanın ne sıklıkta yürütüldürün. Dakikalar içinde ölçülür. |
| QueryTimeSpan |Ölçütlerin değerlendirildiği zaman aralığı. Interval'a eşit veya daha büyük olmalıdır. Dakikalar içinde ölçülür. |
| Sürüm |ÖBM sürümü kullanılıyor.  Şu anda, bu her zaman 1 olarak ayarlanmalıdır. |

Örneğin, 15 dakikalık aralıklı ve 30 dakikalık bir Zaman Aralığı olan bir olay sorgusu düşünün. Bu durumda, sorgu her 15 dakikada bir çalıştırılır ve ölçütler 30 dakikalık bir süre boyunca doğru çözmeye devam ederse bir uyarı tetiklenir.

### <a name="retrieving-schedules"></a>Zamanlamaları alma
Kaydedilmiş bir aramanın tüm zamanlamalarını almak için Get yöntemini kullanın.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Kaydedilmiş bir arama için belirli bir zamanlama almak için zamanlama kimliğiyle Get yöntemini kullanın.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Aşağıda bir zamanlama için örnek bir yanıt ver.

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

### <a name="creating-a-schedule"></a>Zamanlama oluşturma
Yeni bir zamanlama oluşturmak için benzersiz bir zamanlama kimliğiyle Put yöntemini kullanın.  İki zamanlama, kaydedilmiş farklı aramalarla ilişkilendirilse ler bile aynı kimliğe sahip olamaz.  Log Analytics konsolunda bir zamanlama oluşturduğunuzda, zamanlama kimliği için bir GUID oluşturulur.

> [!NOTE]
> Günlük Analizi API'si ile oluşturulan tüm kaydedilen aramaların, zamanlamaların ve eylemlerin adı küçük olmalıdır.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Enabled':'true' } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Zamanlama düzenleme
Bu zamanlamayı değiştirmek için aynı kaydedilmiş arama için varolan bir zamanlama kimliğiyle Put yöntemini kullanın; aşağıdaki örnekte zamanlama devre dışı bırakılır. İsteğin gövdesi zamanlamanın *eetiketini* içermelidir.

      $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Enabled':'false' } }"
      armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Zamanlamaları silme
Zamanlamayı silmek için zamanlama kimliğiyle Sil yöntemini kullanın.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Eylemler
Zamanlamada birden çok eylem olabilir. Bir eylem, posta gönderme veya runbook başlatma gibi gerçekleştirmek için bir veya daha fazla işlem tanımlayabilir veya bir aramanın sonuçlarının bazı ölçütlerle ne zaman eşleşebileceğini belirleyen bir eşik tanımlayabilir.  Bazı eylemler, eşik karşılandığında işlemlerin gerçekleştirileceği şekilde her ikisini de tanımlar.

Tüm eylemlerin özellikleri aşağıdaki tabloda vardır.  Farklı uyarı türleri, aşağıda açıklanan farklı ek özelliklere sahiptir.

| Özellik | Açıklama |
|:--- |:--- |
| `Type` |Eylem türü.  Şu anda olası değerler Alert ve Webhook'tır. |
| `Name` |Uyarının adını görüntüleyin. |
| `Version` |ÖBM sürümü kullanılıyor.  Şu anda, bu her zaman 1 olarak ayarlanmalıdır. |

### <a name="retrieving-actions"></a>Eylemleri alma

Bir zamanlama için tüm eylemleri almak için Get yöntemini kullanın.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Bir zamanlama için belirli bir eylem almak için eylem kimliği ile Get yöntemini kullanın.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Eylemler oluşturma veya düzenleme
Yeni bir eylem oluşturmak için zamanlamaya özgü bir eylem kimliğiyle Put yöntemini kullanın.  Log Analytics konsolunda bir eylem oluşturduğunuzda, bir GUID eylem kimliği içindir.

> [!NOTE]
> Günlük Analizi API'si ile oluşturulan tüm kaydedilen aramaların, zamanlamaların ve eylemlerin adı küçük olmalıdır.

Bu zamanlamayı değiştirmek için aynı kaydedilmiş arama için varolan bir eylem kimliğiyle Put yöntemini kullanın.  İsteğin gövdesi zamanlamanın eetiketini içermelidir.

Yeni bir eylem oluşturmak için istek biçimi eylem türüne göre değişir, bu nedenle bu örnekler aşağıdaki bölümlerde sağlanır.

### <a name="deleting-actions"></a>Eylemleri silme

Bir eylemi silmek için eylem kimliğiyle Sil yöntemini kullanın.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Uyarı Eylemleri
Zamanlama'da bir ve yalnızca bir Uyarı eylemi olmalıdır.  Uyarı eylemlerinde aşağıdaki tablodaki bölümlerden biri veya birkaçı bulunmaktadır.  Her biri aşağıda daha ayrıntılı olarak açıklanmıştır.

| Section | Açıklama | Kullanım |
|:--- |:--- |:--- |
| Eşik |Eylemin ne zaman çalıştırılırken ölçütleri.| Azure'a genişletilmeden önce veya sonra her uyarı için gereklidir. |
| Severity |Tetiklendiğinde uyarıyı sınıflandırmak için kullanılan etiket.| Azure'a genişletilmeden önce veya sonra her uyarı için gereklidir. |
| Önle |Bildirimleri durdurma seçeneği. | Azure'a genişletilmeden önce veya sonra her uyarı için isteğe bağlıdır. |
| Eylem Grupları |E-Postalar, Kobİ'ler, Sesli Aramalar, Webhook'lar, Otomasyon Runbook'ları, ITSM Bağlayıcıları vb. gibi gerekli eylemlerin belirtildiği Azure ActionGroup'un ideleri| Uyarılar Azure'a genişletildikten sonra gerekli|
| Eylemleri Özelleştir|ActionGroup'tan belirli eylemler için standart çıktıyı değiştirme| Her uyarı için isteğe bağlı, uyarılar Azure'a genişletildikten sonra kullanılabilir. |

### <a name="thresholds"></a>Eşik
Bir Uyarı eyleminde bir ve yalnızca bir eşik olmalıdır.  Kaydedilen bir aramanın sonuçları, bu aramayla ilişkili bir eylemdeki eşikle eşleştiğinde, bu eylemdeki diğer işlemler çalıştırılır.  Bir eylem, eşik içermeyen diğer türlerin eylemleri ile kullanılabilen yalnızca bir eşik de içerebilir.

Eşikler aşağıdaki tabloda özelliklere sahiptir.

| Özellik | Açıklama |
|:--- |:--- |
| `Operator` |Eşik karşılaştırması için işleç. <br> gt = Büyük <br> lt = Daha Az |
| `Value` |Eşik değeri. |

Örneğin, 15 dakikalık aralıklı bir olay sorgusu, 30 dakikalık bir Zaman Aralığı ve 10'dan büyük bir Eşik düşünün. Bu durumda, sorgu her 15 dakikada bir çalıştırılır ve 30 dakikalık bir süre içinde oluşturulan 10 olayı döndürürse bir uyarı tetiklenir.

Aşağıda, yalnızca eşiği olan bir eylem için örnek bir yanıt vereme işlenir.  

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

Zamanlama için yeni bir eşik eylemi oluşturmak için benzersiz bir eylem kimliğiyle Put yöntemini kullanın.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Bir zamanlama için eşik eylemini değiştirmek için varolan bir eylem kimliğiyle Put yöntemini kullanın.  İsteğin gövdesi eylemin eetiketini içermelidir.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="severity"></a>Severity
Log Analytics, daha kolay yönetim ve triyaj sağlamak için uyarılarınızı kategorilere ayırmanızı sağlar. Tanımlanan Uyarı şiddeti: bilgilendirme, uyarı ve kritik. Bunlar, Azure Uyarıları'nın normalleştirilmiş önem derecesine aşağıdaki şekilde eşlenir:

|Günlük Analitik Önem Düzeyi  |Azure Önem Düzeyi Uyarılar  |
|---------|---------|
|`critical` |Sev 0|
|`warning` |Sev 1|
|`informational` | Önem Derecesi 2|

Aşağıda, yalnızca bir eşik ve önem derecesi olan bir eylem için örnek bir yanıt vereme verilir. 

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

Önem derecesi olan bir zamanlama için yeni bir eylem oluşturmak için benzersiz bir eylem kimliğiyle Put yöntemini kullanın.  

    $thresholdWithSevJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1','Severity': 'critical', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdWithSevJson

Zamanlama için önem verme eylemini değiştirmek için varolan bir eylem kimliğiyle Put yöntemini kullanın.  İsteğin gövdesi eylemin eetiketini içermelidir.

    $thresholdWithSevJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1','Severity': 'critical', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdWithSevJson

#### <a name="suppress"></a>Önle
Günlük Analizi tabanlı sorgu uyarıları, eşik her karşılaştığında veya aşıldığında ateşlenir. Sorguda ima edilen mantığa bağlı olarak, bu uyarının bir dizi aralık için işten atılmasına ve dolayısıyla bildirimlerin de sürekli olarak gönderilmesine neden olabilir. Bu tür bir senaryoyu önlemek için, bir kullanıcı, Bildirim kuralı için ikinci kez çalıştırılmadan önce Log Analytics'e öngörülen süreyi beklemesini bildiren Suppress seçeneğini ayarlayabilir. Yani bastırma 30 dakika için ayarlanırsa; sonra uyarı ilk kez ateş ve yapılandırılmış bildirimleri gönderir. Ancak daha sonra 30 dakika bekleyin, uyarı kuralı için bildirim tekrar kullanılmadan önce. Ara dönemde, uyarı kuralı çalışmaya devam eder - bu dönemde uyarı kuralı kaç kez çalıştırılacağına bakılmaksızın yalnızca bildirim Log Analytics tarafından belirtilen süre boyunca bastırılır.

Log Analytics uyarı kuralının bastırma özelliği, *Azaltma* değeri ve *DurationInMinutes* değerini kullanarak bastırma süresi kullanılarak belirtilir.

Aşağıda yalnızca bir eşik, önem derecesi ve bastırma özelliği olan bir eylem için örnek bir yanıt

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

Önem derecesi olan bir zamanlama için yeni bir eylem oluşturmak için benzersiz bir eylem kimliğiyle Put yöntemini kullanın.  

    $AlertSuppressJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1','Severity': 'critical', 'Type':'Alert', 'Throttling': { 'DurationInMinutes': 30 },'Threshold': { 'Operator': 'gt', 'Value': 10 } } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myalert?api-version=2015-03-20 $AlertSuppressJson

Zamanlama için önem verme eylemini değiştirmek için varolan bir eylem kimliğiyle Put yöntemini kullanın.  İsteğin gövdesi eylemin eetiketini içermelidir.

    $AlertSuppressJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1','Severity': 'critical', 'Type':'Alert', 'Throttling': { 'DurationInMinutes': 30 },'Threshold': { 'Operator': 'gt', 'Value': 10 } } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myalert?api-version=2015-03-20 $AlertSuppressJson

#### <a name="action-groups"></a>Eylem Grupları
Azure'daki tüm uyarılar, eylemleri işlemek için varsayılan mekanizma olarak Eylem Grubu'nu kullanın. Action Group ile eylemlerinizi bir kez belirtebilir ve ardından eylem grubunu Azure genelinde birden çok uyarıyla ilişkilendirebilirsiniz. Gerek kalmadan, tekrar tekrar aynı eylemleri ilan etmek. Eylem Grupları, e-posta, SMS, Sesli Arama, ITSM Bağlantısı, Otomasyon Runbook, Webhook URI ve daha fazlası dahil olmak üzere birden çok eylemi destekler. 

Uyarılarını Azure'a genişleten kullanıcılar için bir zamanlama, bir uyarı oluşturabilmek için artık Eylem Grubu ayrıntılarının eşikle birlikte geçirilmesi gerekir. E-posta ayrıntıları, Webhook URL'leri, Runbook Otomasyonu ayrıntıları ve diğer Eylemler, bir uyarı oluşturmadan önce bir Eylem Grubu tarafında tanımlanması gerekir; portalda [Azure Monitor'dan Eylem Grubu](../../azure-monitor/platform/action-groups.md) oluşturabilir veya [Eylem Grubu API'sini](https://docs.microsoft.com/rest/api/monitor/actiongroups)kullanabilir.

Eylem grubunun bir uyarıya ilişkilendirme sini eklemek için, uyarı tanımında eylem grubunun benzersiz Azure Kaynak Yöneticisi Kimliğini belirtin. Örnek bir örnek resim aşağıda verilmiştir:

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

Varolan Eylem Grubu'nu bir zamanlama yla ilişkilendirmek için benzersiz bir eylem kimliğiyle Put yöntemini kullanın.  Aşağıda, bir örnek kullanım örneği verem.

    $AzNsJson = "{'properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup']} } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

Zamanlama yla ilişkili bir Eylem Grubunu değiştirmek için varolan bir eylem kimliğiyle Put yöntemini kullanın.  İsteğin gövdesi eylemin eetiketini içermelidir.

    $AzNsJson = "{'etag': 'datetime'2017-12-13T10%3A52%3A21.1697364Z'\"', 'properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': { 'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup'] } } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

#### <a name="customize-actions"></a>Eylemleri Özelleştir
Varsayılan eylemlerle, bildirimler için standart şablonu ve biçimi izleyin. Ancak kullanıcı, Eylem Grupları tarafından denetlenebilse ler bile bazı eylemleri özelleştirebilir. Şu anda, e-posta konusu ve Webhook Yükü için özelleştirme mümkündür.

##### <a name="customize-e-mail-subject-for-action-group"></a>Eylem Grubu için E-Posta Konusunu Özelleştir
Varsayılan olarak, uyarılar için e-posta `<AlertName>` konusu: Uyarı Bildirimi için. `<WorkspaceName>` Ancak bu, gelen kutunuzda filtre kurallarını kolayca kullanmanıza olanak sağlamak için belirli sözcükleri veya etiketleri kullanabileceğiniz şekilde özelleştirilebilir. Özelleştir e-posta üstbilgisi ayrıntılarının aşağıdaki örnekte olduğu gibi ActionGroup ayrıntılarıyla birlikte gönderilmesi gerekir.

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

Zaten var olan Eylem Grubu'nu zamanlama için özelleştirmeyle ilişkilendirmek için benzersiz bir eylem kimliğiyle Put yöntemini kullanın.  Aşağıda, bir örnek kullanım örneği verem.

    $AzNsJson = "{'properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup'], 'CustomEmailSubject': 'Azure Alert fired'} } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

Zamanlama yla ilişkili bir Eylem Grubunu değiştirmek için varolan bir eylem kimliğiyle Put yöntemini kullanın.  İsteğin gövdesi eylemin eetiketini içermelidir.

    $AzNsJson = "{'etag': 'datetime'2017-12-13T10%3A52%3A21.1697364Z'\"', 'properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup']}, 'CustomEmailSubject': 'Azure Alert fired' } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

##### <a name="customize-webhook-payload-for-action-group"></a>Eylem Grubu için Webhook Yükünü Özelleştir
Varsayılan olarak, günlük analitiği için Action Group aracılığıyla gönderilen webhook sabit bir yapıya sahiptir. Ama bir webhook bitiş noktası gereksinimlerini karşılamak için, desteklenen belirli değişkenler kullanarak JSON yükü özelleştirebilirsiniz. Daha fazla bilgi için günlük [uyarı kuralları için Webhook eylemine](../../azure-monitor/platform/alerts-log-webhook.md)bakın. 

Webhook'u özelleştir ayrıntılarının ActionGroup ayrıntılarıyla birlikte gönderilmesi gerekir ve eylem grubu içinde belirtilen tüm Webhook URI'ye uygulanır; aşağıdaki örnekte olduğu gibi.

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

Zaten var olan Eylem Grubu'nu zamanlama için özelleştirmeyle ilişkilendirmek için benzersiz bir eylem kimliğiyle Put yöntemini kullanın.  Aşağıda, bir örnek kullanım örneği verem.

    $AzNsJson = "{'properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup'], 'CustomEmailSubject': 'Azure Alert fired','CustomWebhookPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}'} } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

Zamanlama yla ilişkili bir Eylem Grubunu değiştirmek için varolan bir eylem kimliğiyle Put yöntemini kullanın.  İsteğin gövdesi eylemin eetiketini içermelidir.

    $AzNsJson = "{'etag': 'datetime'2017-12-13T10%3A52%3A21.1697364Z'\"', 'properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup']}, 'CustomEmailSubject': 'Azure Alert fired','CustomWebhookPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}' } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson


## <a name="next-steps"></a>Sonraki adımlar

* Log Analytics'te [günlük aramaları yapmak için REST API'sini](../../azure-monitor/log-query/log-query-overview.md) kullanın.
* Azure [monitöründe günlük uyarıları](../../azure-monitor/platform/alerts-unified-log.md) hakkında bilgi edinin
* Azure [monitöründe günlük uyarı kurallarını oluşturma, yönetme veya yönetme](../../azure-monitor/platform/alerts-log.md)

