---
title: Eski Log Analytics uyarılarından API'yi yeni Azure Uyarıları API'sine dönüştürme
description: Eski kaydedilmiş Arama tabanlı Log Analytics Alert API'ye genel bakış ve uyarı kurallarını yeni ScheduledQueryRules API'ye geçiş yapma süreci ve ortak müşteri endişelerini gideren ayrıntılar.
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 05/30/2019
ms.subservice: alerts
ms.openlocfilehash: c1ca85ba9c79f828b5ddcf0d640cfe7f8b6e097c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249444"
---
# <a name="switch-api-preference-for-log-alerts"></a>Günlük Uyarıları için API tercihini değiştirme

> [!NOTE]
> Kullanıcılar için geçerli olduğu belirtilen içerik Yalnızca Azure genel bulutu için geçerlidir ve Azure Kamu veya Azure Çin bulutu için **geçerli değildir.**  

> [!NOTE]
> Bir kullanıcı tercihini yeni [tarifeliQueryRules API'sine](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) değiştirmeyi seçtiğinde eski eski [Log Analytics Alert API'sını](api-alerts.md)kullanmaya geri dönmek mümkün değildir.

Yakın zamana kadar, Microsoft Operations Management Suite portalında uyarı kurallarını yönetmiştiniz. Yeni uyarı deneyimi, Log Analytics de dahil olmak üzere Microsoft Azure'daki çeşitli hizmetlerle entegre edildi ve [uyarı kurallarınızı OMS portalından Azure'a genişletmemizi](alerts-extend.md)istedik. Ancak müşteriler için en az kesintiyi sağlamak için, süreç tüketimi için programatik arabirimi değiştirmedi - SavedSearch'e dayalı [Log Analytics Alert API.](api-alerts.md)

Ama şimdi log analytics için kullanıcıları gerçek bir Azure programlı alternatif, [Azure Monitor - ScheduledQueryRules API ,](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules)aynı zamanda Azure fatura yansıtıcı - günlük uyarıları için uyarı [duyurdu.](alerts-unified-log.md#pricing-and-billing-of-log-alerts) API'yi kullanarak günlük uyarılarınızı nasıl yöneteceğimiz hakkında daha fazla bilgi edinmek için [Azure Kaynak Şablonu'nu kullanarak günlük uyarılarını yönetme](alerts-log.md#managing-log-alerts-using-azure-resource-template) ve [PowerShell'i kullanarak günlük uyarılarını yönetme](alerts-log.md#managing-log-alerts-using-powershell)'ye bakın.

## <a name="benefits-of-switching-to-new-azure-api"></a>Yeni Azure API'sine geçmenin avantajları

[Eski Log Analytics Alert API](api-alerts.md)üzerinde [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) kullanarak uyarıları oluşturma ve yönetme çeşitli avantajları vardır; biz aşağıda önemli olanları listeledik:

- Uyarı kurallarında [çalışma alanı günlüğü aramasını çapraz laşabilme](../log-query/cross-workspace-query.md) ve Log Analytics çalışma alanları ve hatta Application Insights uygulamaları gibi harici kaynakları kaplayabilme
- Sorguda Gruplandırmak için birden çok alan kullanıldığında, [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) kullanıcısını kullanarak Azure portalında hangi alanın toplandığını belirtebilir
- [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) kullanılarak oluşturulan günlük uyarıları, 48 saate kadar tanımlanmış bir süreye sahip olabilir ve verileri öncekinden daha uzun süre için alabilir
- [Eski Log Analytics Alert API'sinde](api-alerts.md) olduğu gibi üç kaynak düzeyi oluşturmaya gerek kalmadan tek bir kaynak olarak tek çekimde uyarı kuralları oluşturun
- Azure'daki sorgu tabanlı günlük uyarılarının tüm varyantları için tek programlı arabirim - Yeni [scheduledQueryRules API'](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) si Log Analytics ve Application Insights kurallarını yönetmek için kullanılabilir
- [Powershell cmdlets](alerts-log.md#managing-log-alerts-using-powershell) kullanarak günlük uyarılarınızı yönetme
- Tüm yeni günlük uyarısı işlevselliği ve gelecekteki geliştirme yalnızca yeni [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) üzerinden kullanılabilir olacak

## <a name="process-of-switching-from-legacy-log-alerts-api"></a>Eski Günlük Uyarıları API'sinden geçiş süreci

Kullanıcılar eski Log [Analytics Alert API](api-alerts.md) veya yeni [scheduledQueryRules API'yi](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules)kullanmakta serbesttir. Her iki API tarafından oluşturulan uyarı kuralları, *yalnızca aynı API tarafından yönetilebilir* - hem de Azure portalı. Varsayılan olarak, Azure Monitor, Log Analytics'in mevcut çalışma alanları için Azure portalından yeni bir uyarı kuralı oluşturmak için [eski Log Analytics Alert API'sini](api-alerts.md) kullanmaya devam eder. [1 Haziran 2019 tarihinde veya sonrasında oluşturulan yeni Log çalışma alanı duyurulduğu](https://azure.microsoft.com/updates/switch-api-preference-log-alerts/) gibi, Azure portalı da dahil olmak üzere varsayılan olarak yeni [programlıQueryRules API'yi](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) otomatik olarak kullanır.

PlanlananQueryRules API tercih anahtarının etkileri aşağıda derlenmiştir:

- Programlı arayüzler üzerinden günlük uyarıları yönetmek için yapılan tüm etkileşimler şimdi [yerine scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) kullanılarak yapılmalıdır. Daha fazla bilgi için, Azure [Kaynak Şablonu üzerinden örnek kullanım](alerts-log.md#managing-log-alerts-using-azure-resource-template) ve [PowerShell üzerinden örnek kullanım](alerts-log.md#managing-log-alerts-using-powershell)
- Azure portalında oluşturulan herhangi bir yeni günlük uyarı kuralı yalnızca [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) kullanılarak oluşturulur ve kullanıcıların Azure portalı üzerinden [yeni API'nin ek işlevlerini](#benefits-of-switching-to-new-azure-api) kullanmasına izin verir
- Günlük uyarı kurallarının önem derecesi şu şekilde değişir: *Kritik, Uyarı & Bilgilendirme*, *Önem derecesi 0, 1 & 2' ye*. Önem derecesi 3 ve 4 ile uyarı kuralları oluşturma/güncelleştirme seçeneğiyle birlikte.

[Eski Log Analytics Alert API'den](api-alerts.md) uyarı kurallarıtaşıma işlemi, uyarı tanımınızı, sorgunuzu veya yapılandırmanızı hiçbir şekilde değiştirmeyi içermez. Uyarı kurallarınız ve izlemeniz etkilenmez ve uyarılar geçiş sırasında veya sonrasında durmaz veya durdurulmaz. Tek değişiklikler şunlardır:

- Yeni bir API ile API tercihinde ve kurallarınıza erişimde değişiklik.
- Bu yapıdaki `<WorkspaceName>|<savedSearchId>|<scheduleId>|<ActionId>`uyarı kuralı adı yerine [eski Log Analytics Alert API'sında](api-alerts.md) kullanılan ilikleri içeren değiştirilmiş bir uyarı kuralı kaynağı URI. Uyarı kuralının görüntü adı değişmeden kalır.

Yeni [tarifeliQueryRules'a](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) gönüllü olarak geçmek ve eski [Log Analytics Alert API'sinden](api-alerts.md)kullanımı engellemek isteyen tüm müşteriler; bunu, belirli Log Analytics çalışma alanıyla ilişkili tüm uyarı kurallarını değiştirmek için aşağıdaki API'de BIR PUT çağrısı gerçekleştirerek yapabilirsiniz.

```
PUT /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Aşağıdaki JSON içeren istek gövdesi ile.

```json
{
    "scheduledQueryRulesEnabled" : true
}
```

API'ye, Azure Kaynak Yöneticisi API'sini başlatmayı kolaylaştıran açık kaynak kodlu komut satırı aracı [ARMClient](https://github.com/projectkudu/ARMClient)kullanılarak powershell komut satırından da erişilebilir. Aşağıda gösterildiği gibi, belirli Log Analytics çalışma alanıyla ilişkili tüm uyarı kurallarını değiştirmek için ARMclient aracını kullanarak örnek PUT aramasında.

```powershell
$switchJSON = '{"scheduledQueryRulesEnabled": "true"}'
armclient PUT /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $switchJSON
```

Log Analytics çalışma alanındaki tüm uyarı kurallarının yeni [scheduledQueryRules'ı](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) kullanmak için geçişi başarılı olursa, aşağıdaki yanıt sağlanacaktır.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : true
}
```

Kullanıcılar ayrıca Log Analytics çalışma alanınızın geçerli durumunu kontrol edebilir ve yalnızca [scheduledQueryRules'ı](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) kullanmak üzere değiştirilip değiştirilemediğini görebilir. Denetlemek için, kullanıcılar aşağıdaki API'de GET çağrısı gerçekleştirebilir.

```
GET /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

[ARMClient](https://github.com/projectkudu/ARMClient) aracını kullanarak PowerShell komut satırını kullanarak yukarıdakileri uygulamak için aşağıdaki örneğe bakın.

```powershell
armclient GET /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Belirtilen Log Analytics çalışma alanı yalnızca [scheduledQueryRules'ı](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) kullanmak üzere değiştirildiyse; sonra yanıt JSON aşağıda listelenen olacaktır.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : true
}
```
Aksi takdirde, belirtilen Log Analytic çalışma alanı henüz [yalnızca scheduledQueryRules'ı](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) kullanmak üzere değiştirilmemişse; sonra yanıt JSON aşağıda listelenen olacaktır.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : false
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Monitörü - Günlük Uyarıları](alerts-unified-log.md)hakkında bilgi edinin.
- [Azure Uyarıları'nda günlük uyarılarının](alerts-log.md)nasıl oluşturulabildiğini öğrenin.
- Azure Uyarıları [deneyimi](../../azure-monitor/platform/alerts-overview.md)hakkında daha fazla bilgi edinin.
