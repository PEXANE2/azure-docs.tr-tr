---
title: Windows Sanal Masaüstü tanılama günlüğü analitiği - Azure
description: Windows Sanal Masaüstü tanılama özelliği ile günlük analitiği nasıl kullanılır.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 355acb081afef8c78cdf971c7a82acdb91ab5593
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127945"
---
# <a name="use-log-analytics-for-the-diagnostics-feature"></a>Tanılama özelliği için Günlük Analizi'ni kullanın

Windows Sanal Masaüstü, yöneticinin sorunları tek bir arabirim üzerinden tanımlamasına olanak tanıyan bir tanılama özelliği sunar. Bu özellik, windows sanal masaüstü rolü atanan biri hizmeti kullandığında tanılama bilgilerini günlüğe kaydeder. Her günlük, etkinlikte hangi Windows Sanal Masaüstü rolünün yer aldığı, oturum sırasında görünen hata iletileri, kiracı bilgileri ve kullanıcı bilgileri hakkında bilgiler içerir. Tanılama özelliği, hem kullanıcı hem de yönetim eylemleri için etkinlik günlükleri oluşturur. Her etkinlik günlüğü üç ana kategoriye girer: 

- Abonelik tenebe: Kullanıcı, Microsoft Remote Desktop uygulamaları aracılığıyla özet akışına bağlanmaya çalıştığında.
- Bağlantı etkinlikleri: Bir kullanıcı Microsoft Remote Desktop uygulamaları aracılığıyla bir masaüstüne veya RemoteApp'a bağlanmaya çalıştığında.
- Yönetim etkinlikleri: Yönetici, ana bilgisayar havuzları oluşturma, kullanıcıları uygulama gruplarına atama ve rol atamaları oluşturma gibi sistemde yönetim işlemleri gerçekleştirdiğinde.

Tanılama rol hizmetiwindows sanal masaüstünün bir parçası olduğundan, Windows Sanal Masaüstü'ne ulaşmayan bağlantılar tanılama sonuçlarında görünmez. Windows Sanal Masaüstü bağlantı sorunları, kullanıcı ağ bağlantısı sorunları yaşıyorsa meydana gelebilir.

## <a name="why-you-should-use-log-analytics"></a>Log Analytics'i neden kullanmalısınız?

Azure istemcisinde tek kullanıcılı sorun giderme nin ötesine geçen tanılama verilerini analiz etmek için Log Analytics'i kullanmanızı öneririz. VM performans sayaçlarını Log Analytics'e çekebildiğiniz için dağıtımınız için bilgi toplamak için tek bir aracınız vardır.

## <a name="before-you-get-started"></a>Başlamadan önce

Tanılama özelliğiyle Log Analytics'i kullanabilmeniz için [önce bir çalışma alanı oluşturmanız](../azure-monitor/learn/quick-collect-windows-computer.md#create-a-workspace)gerekir.

Çalışma alanınızı oluşturduktan sonra, aşağıdaki bilgileri almak [için Windows bilgisayarlarını Azure Monitor'a bağlayın](../azure-monitor/platform/agent-windows.md#obtain-workspace-id-and-key) yönergeleri izleyin: 

- Çalışma alanı kimliği
- Çalışma alanınızın birincil anahtarı

Bu bilgilere daha sonra kurulum işleminde ihtiyacınız olacak.

## <a name="push-diagnostics-data-to-your-workspace"></a>Tanılama verilerini çalışma alanınıza itin 

Windows Sanal Masaüstü kiracınızdaki tanılama verilerini çalışma alanınız için Günlük Analizi'ne itebilirsiniz. Çalışma alanınızı kiracınıza bağlayarak kiracınızı ilk oluşturduğunuzda bu özelliği hemen ayarlayabilir veya daha sonra varolan bir kiracıyla ayarlayabilirsiniz.

Yeni kiracınızı ayarlarken kiracınızı Log Analytics çalışma alanınıza bağlamak için, TenantCreator kullanıcı hesabınızla Windows Sanal Masaüstünde oturum açmak için aşağıdaki cmdlet'i çalıştırın: 

```powershell
Add-RdsAccount -DeploymentUrl https://rdbroker.wvd.microsoft.com 
```

Yeni bir kiracı yerine varolan bir kiracıyı bağlayacaksanız, bunun yerine şu cmdlet'i çalıştırın: 

```powershell
Set-RdsTenant -Name <TenantName> -AzureSubscriptionId <SubscriptionID> -LogAnalyticsWorkspaceId <String> -LogAnalyticsPrimaryKey <String> 
```

Log Analytics'e bağlamak istediğiniz her kiracı için bu cmdletleri çalıştırmanız gerekir. 

>[!NOTE]
>Kiracı oluştururken Log Analytics çalışma alanını bağlamak istemiyorsanız, bunun `New-RdsTenant` yerine cmdlet'i çalıştırın. 

## <a name="cadence-for-sending-diagnostic-events"></a>Tanılama olaylarını göndermek için cadence

Tanılama olayları tamamlandığında Log Analytics'e gönderilir.  

## <a name="example-queries"></a>Örnek sorgular

Aşağıdaki örnek sorgular, tanılama özelliğinin sisteminizdeki en sık kullanılan etkinlikler için nasıl bir rapor oluşturduğunu gösterir:

Bu ilk örnek, desteklenen uzak masaüstü istemcileri olan kullanıcılar tarafından başlatılan bağlantı etkinliklerini gösterir:

```powershell
WVDActivityV1_CL 

| where Type_s == "Connection" 

| join kind=leftouter ( 

    WVDErrorV1_CL 

    | summarize Errors = makelist(pack('Time', Time_t, 'Code', ErrorCode_s , 'CodeSymbolic', ErrorCodeSymbolic_s, 'Message', ErrorMessage_s, 'ReportedBy', ReportedBy_s , 'Internal', ErrorInternal_s )) by ActivityId_g 

    ) on $left.Id_g  == $right.ActivityId_g   

| join  kind=leftouter (  

    WVDCheckpointV1_CL 

    | summarize Checkpoints = makelist(pack('Time', Time_t, 'ReportedBy', ReportedBy_s, 'Name', Name_s, 'Parameters', Parameters_s) ) by ActivityId_g 

    ) on $left.Id_g  == $right.ActivityId_g  

|project-away ActivityId_g, ActivityId_g1 
```

Bu sonraki örnek sorgu, yöneticilerin kiracılar üzerindeki yönetim etkinliklerini gösterir:

```powershell
WVDActivityV1_CL 

| where Type_s == "Management" 

| join kind=leftouter ( 

    WVDErrorV1_CL 

    | summarize Errors = makelist(pack('Time', Time_t, 'Code', ErrorCode_s , 'CodeSymbolic', ErrorCodeSymbolic_s, 'Message', ErrorMessage_s, 'ReportedBy', ReportedBy_s , 'Internal', ErrorInternal_s )) by ActivityId_g 

    ) on $left.Id_g  == $right.ActivityId_g   

| join  kind=leftouter (  

    WVDCheckpointV1_CL 

    | summarize Checkpoints = makelist(pack('Time', Time_t, 'ReportedBy', ReportedBy_s, 'Name', Name_s, 'Parameters', Parameters_s) ) by ActivityId_g 

    ) on $left.Id_g  == $right.ActivityId_g  

|project-away ActivityId_g, ActivityId_g1 
```
 
## <a name="stop-sending-data-to-log-analytics"></a>Günlük Analitiği'ne veri göndermeyi durdurun 

Varolan bir kiracıdan Log Analytics'e veri göndermeyi durdurmak için aşağıdaki cmdlet'i çalıştırın ve boş dizeleri ayarlayın:

```powershell
Set-RdsTenant -Name <TenantName> -AzureSubscriptionId <SubscriptionID> -LogAnalyticsWorkspaceId <String> -LogAnalyticsPrimaryKey <String> 
```

Veri göndermeyi durdurmak istediğiniz her kiracı için bu cmdlet çalıştırmak gerekir. 

## <a name="next-steps"></a>Sonraki adımlar 

Tanılama özelliğinin sizin için tanımlayabileceği yaygın hata senaryolarını gözden geçirmek için [bkz.](diagnostics-role-service.md#common-error-scenarios)
