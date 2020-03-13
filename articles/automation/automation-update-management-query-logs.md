---
title: Azure Güncelleştirme Yönetimi günlüklerini sorgulama
description: Bu makalede, Log Analytics çalışma alanınızdaki Güncelleştirme Yönetimi günlüklerinin nasıl sorgulanacağını açıklar.
services: automation
ms.subservice: update-management
ms.date: 03/11/2020
ms.topic: conceptual
ms.openlocfilehash: f31168d47f31d8e740c95cb3d9e449f473cc78dc
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79216838"
---
# <a name="query-update-records-for-update-management-in-azure-monitor-logs"></a>Azure Izleyici günlüklerinde Güncelleştirme Yönetimi için sorgu güncelleştirme kayıtları

Güncelleştirme Yönetimi çözümünde sunulan ayrıntılara ek olarak, Log Analytics çalışma alanınızda depolanan günlüklere göre arama yapabilirsiniz. Çözüm sayfasında, sol taraftaki bölmede **Günlükler**' i seçin. **Günlük araması** sayfası açılır.

Ayrıca, bkz: [Log Analytics arama API 'si belgeleri](https://dev.loganalytics.io/)' ni ziyaret ederek sorguları özelleştirmeyi veya farklı istemcilerden daha fazlasını kullanmayı da öğrenebilirsiniz.

## <a name="update-records"></a>Güncelleştirme kayıtları

Windows ve Linux VM 'Ler için Güncelleştirme Yönetimi tarafından toplanan kayıtlar ve günlük araması sonuçlarında görünen veri türleri. Aşağıdaki bölümlerde bu kayıtlar açıklanır.

### <a name="required-updates"></a>Gerekli güncelleştirmeler

Bir makine için gereken güncelleştirmeleri temsil eden `RequiredUpdate` türünde bir kayıt oluşturulur. Bu kayıtlar aşağıdaki tabloda özelliklere sahiptir:

| Özellik | Açıklama | 
|----------|-------------|
| Bilgisayar | Raporlama makinesinin tam etki alanı adı. |
| KBID | Windows Update Bilgi Bankası makalesi KIMLIĞI. |
| ManagementGroupName | Operations Manager yönetim grubunun veya Log Analytics çalışma alanının adı. | 
| Ürün | Güncelleştirmenin geçerli olduğu ürünler. | 
| PublishDate | Güncelleştirmenin Windows Update indirilmeye ve yüklenmeye hazırlanma tarihi. |
| Sunucu | | 
| Sourcehealthserviceıd | Log Analytics Windows Agent KIMLIĞINI temsil eden benzersiz tanımlayıcı. |
| SourceSystem | *OperationsManager* | 
| TenantId | Azure Active Directory kurumları örneğinizi temsil eden benzersiz tanımlayıcı. | 
| TimeGenerated | Kaydın oluşturulduğu tarih ve saat. | 
| Tür | *Güncelleştirme* | 
| UpdateClassification | Uygulanabilecek güncelleştirmelerin türünü gösterir. Windows için:<br> *Kritik güncelleştirmeler*<br> *Güvenlik güncelleştirmeleri*<br> *Güncelleştirme paketleri*<br> *Özellik paketleri*<br> *Hizmet paketleri*<br> *Tanım güncelleştirmeleri*<br> *Araçlar*<br> *Güncelleştirmeler*. Linux için:<br> *Kritik güncelleştirmeler ve güvenlik güncelleştirmeleri*<br> *Diğer* |
| Updateönem derecesi | Güvenlik Açığı için önem derecesi. Değerler şunlardır:<br> *Başlatma*<br> *Önemli*<br> *Düzey*<br> *Zayıf* |
| UpdateTitle | Güncelleştirme başlığı.|

### <a name="update"></a>Güncelleştir

Bir makine için kullanılabilir güncelleştirmeleri ve bunların yükleme durumlarını temsil eden `Update` türünde bir kayıt oluşturulur. Bu kayıtlar aşağıdaki tabloda özelliklere sahiptir:

| Özellik | Açıklama | 
|----------|-------------|
| ApprovalSource | Yalnızca Windows işletim sistemi için geçerlidir. Değer *Microsoft Update*. |
| Onaylandı | *True* veya *false* |
| Sınıflandırma | *Güncelleştirmeler* |
| Bilgisayar | Raporlama makinesinin tam etki alanı adı. |
| Bilgisayar ortamı | *Azure* veya *Azure dışı*. |
| MSRCBulletinID | Güvenlik Bülteni KIMLIK numarası | 
| MSRCSeverity | Güvenlik Açığı için önem derecesi. Değerler şunlardır:<br> *Başlatma*<br> *Önemli*<br> *Düzey*<br> *Zayıf* |  
| KBID | Windows Update Bilgi Bankası makalesi KIMLIĞI. |
| ManagementGroupName | Operations Manager yönetim grubunun veya Log Analytics çalışma alanının adı. |
| UpdateID | Yazılım güncelleştirmesinin benzersiz tanıtıcısı. |
| RevisionNumber | Bir güncelleştirmenin belirli bir düzeltmesine ait düzeltme numarası. |
| İsteğe bağlı | *True* veya *false* | 
| RebootBehavior | Bir güncelleştirmeyi yükledikten/kaldırdıktan sonra yeniden başlatma davranışı. |
| _ResourceId | Kaydın ilişkilendirildiği kaynak için benzersiz tanımlayıcı. |
| Tür | *Güncelleştirme* |
| VMUUID | Sanal makine için benzersiz tanımlayıcı. |
| MG | Yönetim grubu veya Log Analytics çalışma alanı için benzersiz tanımlayıcı. | 
| TenantId | Azure Active Directory kurumları örneğinizi temsil eden benzersiz tanımlayıcı. | 
| SourceSystem | *OperationsManager* | 
| TimeGenerated | Kaydın oluşturulduğu tarih ve saat. | 
| SourceComputerId | Kaynak bilgisayarı temsil eden benzersiz tanımlayıcı. | 
| Başlık | Güncelleştirme başlığı. |
| PublishedDate (UTC) | Güncelleştirmenin Windows Update indirilmeye ve yüklenmeye hazırlanma tarihi.  |
| UpdateState | Güncelleştirmenin geçerli durumu. | 
| Ürün | Güncelleştirmenin geçerli olduğu ürünler. |
| kaynak grubundaki | Azure aboneliğinin benzersiz tanımlayıcısı. | 
| adlı yönetilen örnek, | Kaynağın üyesi olduğu kaynak grubunun adı. | 
| ResourceProvider | Kaynak sağlayıcısını belirtir. | 
| Kaynak | Kaynağın adı. | 
| ResourceType | Kaynak türünün adı. | 

### <a name="update-agent"></a>Aracıyı Güncelleştir

Makinedeki güncelleştirme aracısının ayrıntılarını sağlayan `UpdateAgent` türünde bir kayıt oluşturulur. Bu kayıtlar aşağıdaki tabloda özelliklere sahiptir:

| Özellik | Açıklama | 
|----------|-------------|
| AgeofOldestMissingRequiredUpdate | | 
| AutomaticUpdateEnabled | | 
| Bilgisayar | Raporlama makinesinin tam etki alanı adı. |
| Daysıncelastupdatebucket | | 
| ManagementGroupName | Operations Manager yönetim grubunun veya Log Analytics çalışma alanının adı. |
| İşletim sistemi sürümü | İşletim sisteminin sürümü. |
| Sunucu | |
| Sourcehealthserviceıd | Log Analytics Windows Agent KIMLIĞINI temsil eden benzersiz tanımlayıcı. |
| SourceSystem | *OperationsManager* | 
| TenantId | Azure Active Directory kurumları örneğinizi temsil eden benzersiz tanımlayıcı. |
| TimeGenerated | Kaydın oluşturulduğu tarih ve saat. |
| Tür | *Güncelleştirme* | 
| WindowsUpdateAgentVersion | Windows Update aracısının sürümü. |
| WSUSServer | Windows Update aracısında sorun gidermeye yardımcı olmak için bir sorun varsa hataları gösterir. |

### <a name="update-deployment-status"></a>Güncelleştirme dağıtım durumu 

Makine tarafından zamanlanmış bir dağıtımın güncelleştirme dağıtım durumunu sağlayan `UpdateRunProgress` türünde bir kayıt oluşturulur. Bu kayıtlar aşağıdaki tabloda özelliklere sahiptir:

| Özellik | Açıklama | 
|----------|-------------|
| Bilgisayar | Raporlama makinesinin tam etki alanı adı. |
| Bilgisayar ortamı | *Azure* veya *Azure dışı*. | 
| CorrelationId | Güncelleştirme için çalışan runbook işinin benzersiz tanıtıcısı. |
| EndTime | Eşitleme işleminin sona erdiği zaman. | 
| ErrorResult | Bir güncelleştirme yüklenemediğinde Windows Update hata kodu oluşturulur. | 
| Yüklemedurumu | İstemci bilgisayarda bir güncelleştirmenin olası yükleme durumları, *devam ediyor*, *kısmen başarısız oldu*. *Succeeded* |
| KBID | Windows Update Bilgi Bankası makalesi KIMLIĞI. | 
| ManagementGroupName | Operations Manager yönetim grubunun veya Log Analytics çalışma alanının adı. |
| OSType | İşletim sisteminin, *Windows* veya *Linux*'un türünü belirtir. | 
| Ürün | Güncelleştirmenin geçerli olduğu ürünler. |
| Kaynak | Kaynağın adı. | 
| ResourceId | Kaydın ilişkilendirildiği kaynak için benzersiz tanımlayıcı. |
| ResourceProvider | Kaynak sağlayıcısını belirtir. | 
| ResourceType | Kaynak türünün adı. | 
| SourceComputerId | Kaynak bilgisayarı temsil eden benzersiz tanımlayıcı. | 
| SourceSystem | *OperationsManager* |
| StartTime | Güncelleştirme yüklenmek üzere zamanlandığında geçen zaman. |
| kaynak grubundaki | Azure aboneliğinin benzersiz tanımlayıcısı. | 
| SucceededOnRetry | İlk denemede güncelleştirme yürütmesinin başarısız olduğunu ve geçerli işlemin yeniden deneme girişimi olduğunu gösterir. |
| TimeGenerated | Kaydın oluşturulduğu tarih ve saat. |
| Başlık | Güncelleştirme başlığı. |
| Tür | *UpdateRunProgress* |
| UpdateID | Yazılım güncelleştirmesinin benzersiz tanıtıcısı. |
| VMUUID | Sanal makine için benzersiz tanımlayıcı. |
| _ResourceId | Kaydın ilişkilendirildiği kaynak için benzersiz tanımlayıcı. |

### <a name="update-summary"></a>Güncelleştirme Özeti 

Makineye göre güncelleştirme Özeti sağlayan `UpdateSummary` türünde bir kayıt oluşturulur. Bu kayıtlar aşağıdaki tabloda özelliklere sahiptir:

| Özellik | Açıklama | 
|----------|-------------|
| Bilgisayar | Raporlama makinesinin tam etki alanı adı. |
| Bilgisayar ortamı | *Azure* veya *Azure dışı*. | 
| CriticalUpdatesMissing | Geçerli olan eksik kritik güncelleştirme sayısı. | 
| ManagementGroupName | Operations Manager yönetim grubunun veya Log Analytics çalışma alanının adı. |
| NETRuntimeVersion | Windows bilgisayarda yüklü .NET Framework sürümü. |
| OldestMissingSecurityUpdateBucket | Değerler şunlardır:<br> *Son*<br> *30 gün önce*<br> *60 gün önce*<br> *Yaş* | 
| OldestMissingSecurityUpdateInDays | En eski güncelleştirme yüklü olmayan şekilde algılanan toplam gün sayısı. |
| OsVersion | İşletim sisteminin sürümü. |
| OtherUpdatesMissing | Algılanan güncelleştirme sayısı eksik. |
| Kaynak |  Kaynağın adı. | 
| adlı yönetilen örnek, | Kaynağın üyesi olduğu kaynak grubunun adı. |
| ResourceId | Kaydın ilişkilendirildiği kaynak için benzersiz tanımlayıcı. |
| ResourceProvider | Kaynak sağlayıcısını belirtir. |
| ResourceType | Kaynak türünün adı. |
| RestartPending | *True* veya *false*. |
| SecurityUpdatesMissing | Geçerli olan eksik güvenlik güncelleştirmelerinin sayısı.| 
| SourceComputerId | Sanal makine için benzersiz tanımlayıcı. |
| SourceSystem | *OpsManager* | 
| kaynak grubundaki | Azure aboneliğinin benzersiz tanımlayıcısı. |
| TimeGenerated | Kaydın oluşturulduğu tarih ve saat. |
| TotalUpdatesMissing | Geçerli olan eksik güncelleştirmelerin toplam sayısı. | 
| Tür | *UpdateSummary* |
| VMUUID | Sanal makine için benzersiz tanımlayıcı. |
| WindowsUpdateAgentVersion | Windows Update aracısının sürümü. |
| WindowsUpdateSetting | Windows Update aracısının durumunu gösterir. Olası değerler şunlardır:<br> *Zamanlanan yükleme*<br> *Yüklemeden önce bildir*<br> Sağlıksız WUA aracısından hata döndürüldü. | 
| WSUSServer | Windows Update aracısında sorun gidermeye yardımcı olmak için bir sorun varsa hataları gösterir. |
| _ResourceId | Kaydın ilişkilendirildiği kaynak için benzersiz tanımlayıcı. |

## <a name="sample-queries"></a>Örnek sorgular

Aşağıdaki bölümlerde, Güncelleştirme Yönetimi için toplanan güncelleştirme kayıtlarına yönelik örnek günlük sorguları sağlanmaktadır.

### <a name="confirm-that-non-azure-machines-are-onboarded"></a>Azure olmayan makinelerin eklendi olduğunu onaylayın

Doğrudan bağlı makinelerin Azure Izleyici günlükleriyle iletişim kurduğunu onaylamak için aşağıdaki günlük aramalarıyla birini çalıştırın.

#### <a name="linux"></a>Linux

```loganalytics
Heartbeat
| where OSType == "Linux" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

#### <a name="windows"></a>Windows

```loganalytics
Heartbeat
| where OSType == "Windows" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

Bir Windows bilgisayarında, Azure Izleyici günlükleriyle aracı bağlantısını doğrulamak için aşağıdaki bilgileri gözden geçirebilirsiniz:

1. Denetim Masası 'nda **Microsoft Monitoring Agent**açın. **Azure Log Analytics** sekmesinde, aracı şu iletiyi görüntüler: **Microsoft Monitoring Agent Log Analytics başarıyla bağlandı**.
2. Windows olay günlüğü 'Nü açın. **Uygulama ve hizmetler Logs\Operations Manager** ' a gidin ve kaynak **hizmeti bağlayıcısından**olay KIMLIĞI 3000 ve olay kimliği 5002 ' ni arayın. Bu olaylar, bilgisayarın Log Analytics çalışma alanına kaydoldığını ve yapılandırma aldığını gösterir.

Aracı Azure Izleyici günlükleriyle iletişim kuramıyorsa ve aracı bir güvenlik duvarı veya ara sunucu üzerinden internet ile iletişim kurmak üzere yapılandırılmışsa, güvenlik duvarının veya ara sunucunun düzgün yapılandırıldığını onaylayın. Güvenlik duvarının veya proxy sunucusunun düzgün bir şekilde yapılandırıldığını nasıl doğrulayacağınızı öğrenmek için bkz. [Windows Aracısı Için ağ yapılandırması](../azure-monitor/platform/agent-windows.md) veya [Linux Aracısı için ağ yapılandırması](../log-analytics/log-analytics-agent-linux.md).

> [!NOTE]
> Linux sistemleriniz bir ara sunucu veya Log Analytics ağ geçidiyle iletişim kuracak şekilde yapılandırıldıysa ve bu çözümü eklediğinizde, aşağıdaki komutları kullanarak dosya üzerinde omıuser grubuna Okuma izni vermek için *proxy. conf* izinlerini güncelleştirin:
>
> `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`

Yeni eklenen Linux aracıları, bir değerlendirme gerçekleştirildikten sonra **güncelleştirilmiş** durumunu gösterir. Bu işlem 6 saat kadar sürebilir.

Operations Manager yönetim grubunun Azure Izleyici günlükleriyle iletişim kurduğunu onaylamak için bkz. [Azure izleyici günlükleriyle Operations Manager tümleştirmeyi doğrulama](../azure-monitor/platform/om-agents.md#validate-operations-manager-integration-with-azure-monitor).

### <a name="single-azure-vm-assessment-queries-windows"></a>Tek Azure VM değerlendirme sorguları (Windows)

VMUUID değerini, sorguladığınız sanal makinenin VM GUID 'SI ile değiştirin. Azure Izleyici günlüklerinde aşağıdaki sorguyu çalıştırarak kullanılması gereken VMUUID 'yi bulabilirsiniz: `Update | where Computer == "<machine name>" | summarize by Computer, VMUUID`

#### <a name="missing-updates-summary"></a>Eksik güncelleştirmeler Özeti

```loganalytics
Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and VMUUID=~"b08d5afa-1471-4b52-bd95-a44fea6e4ca8"
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize by UpdateID, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security")
```

#### <a name="missing-updates-list"></a>Eksik güncelleştirmeler listesi

```loganalytics
Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and VMUUID=~"8bf1ccc6-b6d3-4a0b-a643-23f346dfdf82"
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, KBID, PublishedDate, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| project-away UpdateState, Approved, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), displayName=any(Title), publishedDate=min(PublishedDate), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(UpdateID, "_", KBID), classification=Classification, InformationId=strcat("KB", KBID), InformationUrl=iff(isnotempty(KBID), strcat("https://support.microsoft.com/kb/", KBID), ""), osType=2
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl
```

### <a name="single-azure-vm-assessment-queries-linux"></a>Tek Azure VM değerlendirmesi sorguları (Linux)

Bazı Linux 'lar için, Azure Resource Manager ve Azure Izleyici günlüklerinde depolanan VMUUID değeri ile bir [bitiçlik](https://en.wikipedia.org/wiki/Endianness) uyumsuzluğu vardır. Aşağıdaki sorgu, her iki durumda da bir eşleşme olup olmadığını denetler. Sonuçları doğru bir şekilde döndürmek için VMUUID değerlerini, GUID 'nin büyük endian ve little-endian biçimiyle değiştirin. Azure Izleyici günlüklerinde aşağıdaki sorguyu çalıştırarak kullanılması gereken VMUUID 'yi bulabilirsiniz: `Update | where Computer == "<machine name>"
| summarize by Computer, VMUUID`

#### <a name="missing-updates-summary"></a>Eksik güncelleştirmeler Özeti

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and (VMUUID=~"625686a0-6d08-4810-aae9-a089e68d4911" or VMUUID=~"a0865662-086d-1048-aae9-a089e68d4911")
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| summarize by Product, ProductArch, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security")
```

#### <a name="missing-updates-list"></a>Eksik güncelleştirmeler listesi

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and (VMUUID=~"625686a0-6d08-4810-aae9-a089e68d4911" or VMUUID=~"a0865662-086d-1048-aae9-a089e68d4911")
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, BulletinUrl, BulletinID) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| project-away UpdateState, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(Product, "_", ProductArch), displayName=Product, productArch=ProductArch, classification=Classification, InformationId=BulletinID, InformationUrl=tostring(split(BulletinUrl, ";", 0)[0]), osType=1
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl

```

### <a name="multi-vm-assessment-queries"></a>Çoklu VM değerlendirmesi sorguları

#### <a name="computers-summary"></a>Bilgisayar Özeti

```loganalytics
Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(14h) and OSType!="Linux"
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Approved, Optional, Classification) by SourceComputerId, UpdateID
    | distinct SourceComputerId, Classification, UpdateState, Approved, Optional
    | summarize WorstMissingUpdateSeverity=max(iff(UpdateState=~"Needed" and (Optional==false or Classification has "Critical" or Classification has "Security") and Approved!=false, iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1)), 0)) by SourceComputerId
)
on SourceComputerId
| extend WorstMissingUpdateSeverity=coalesce(WorstMissingUpdateSeverity, -1)
| summarize computersBySeverity=count() by WorstMissingUpdateSeverity
| union (Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(5h) and OSType=="Linux"
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by SourceComputerId, Product, ProductArch
    | distinct SourceComputerId, Classification, UpdateState
    | summarize WorstMissingUpdateSeverity=max(iff(UpdateState=~"Needed", iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1)), 0)) by SourceComputerId
)
on SourceComputerId
| extend WorstMissingUpdateSeverity=coalesce(WorstMissingUpdateSeverity, -1)
| summarize computersBySeverity=count() by WorstMissingUpdateSeverity)
| summarize assessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity>-1), notAssessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==-1), computersNeedCriticalUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==4), computersNeedSecurityUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==2), computersNeedOtherUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==1), upToDateComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==0)
| summarize assessedComputersCount=sum(assessedComputersCount), computersNeedCriticalUpdatesCount=sum(computersNeedCriticalUpdatesCount),  computersNeedSecurityUpdatesCount=sum(computersNeedSecurityUpdatesCount), computersNeedOtherUpdatesCount=sum(computersNeedOtherUpdatesCount), upToDateComputersCount=sum(upToDateComputersCount), notAssessedComputersCount=sum(notAssessedComputersCount)
| extend allComputersCount=assessedComputersCount+notAssessedComputersCount


```

#### <a name="missing-updates-summary"></a>Eksik güncelleştirmeler Özeti

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| summarize by Product, ProductArch, Classification
| union (Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize by UpdateID, Classification )
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security")
```

#### <a name="computers-list"></a>Bilgisayar listesi

```loganalytics
Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions, Computer, ResourceId, ComputerEnvironment, VMUUID) by SourceComputerId
| where Solutions has "updates"
| extend vmuuId=VMUUID, azureResourceId=ResourceId, osType=1, environment=iff(ComputerEnvironment=~"Azure", 1, 2), scopedToUpdatesSolution=true, lastUpdateAgentSeenTime=""
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
    | where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
    | summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
    | where Solutions has "updates"
    | distinct SourceComputerId))
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Product, Computer, ComputerEnvironment) by SourceComputerId, Product, ProductArch
    | summarize Computer=any(Computer), ComputerEnvironment=any(ComputerEnvironment), missingCriticalUpdatesCount=countif(Classification has "Critical" and UpdateState=~"Needed"), missingSecurityUpdatesCount=countif(Classification has "Security" and UpdateState=~"Needed"), missingOtherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security" and UpdateState=~"Needed"), lastAssessedTime=max(TimeGenerated), lastUpdateAgentSeenTime="" by SourceComputerId
    | extend compliance=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0, 2, 1)
    | extend ComplianceOrder=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0 or missingOtherUpdatesCount > 0, 1, 3)
)
on SourceComputerId
| project id=SourceComputerId, displayName=Computer, sourceComputerId=SourceComputerId, scopedToUpdatesSolution=true, missingCriticalUpdatesCount=coalesce(missingCriticalUpdatesCount, -1), missingSecurityUpdatesCount=coalesce(missingSecurityUpdatesCount, -1), missingOtherUpdatesCount=coalesce(missingOtherUpdatesCount, -1), compliance=coalesce(compliance, 4), lastAssessedTime, lastUpdateAgentSeenTime, osType=1, environment=iff(ComputerEnvironment=~"Azure", 1, 2), ComplianceOrder=coalesce(ComplianceOrder, 2)
| union(Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions, Computer, ResourceId, ComputerEnvironment, VMUUID) by SourceComputerId
| where Solutions has "updates"
| extend vmuuId=VMUUID, azureResourceId=ResourceId, osType=2, environment=iff(ComputerEnvironment=~"Azure", 1, 2), scopedToUpdatesSolution=true, lastUpdateAgentSeenTime=""
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(14h) and OSType!="Linux" and SourceComputerId in ((Heartbeat
    | where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
    | summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
    | where Solutions has "updates"
    | distinct SourceComputerId))
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, Optional, Approved, Computer, ComputerEnvironment) by Computer, SourceComputerId, UpdateID
    | summarize Computer=any(Computer), ComputerEnvironment=any(ComputerEnvironment), missingCriticalUpdatesCount=countif(Classification has "Critical" and UpdateState=~"Needed" and Approved!=false), missingSecurityUpdatesCount=countif(Classification has "Security" and UpdateState=~"Needed" and Approved!=false), missingOtherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security" and UpdateState=~"Needed" and Optional==false and Approved!=false), lastAssessedTime=max(TimeGenerated), lastUpdateAgentSeenTime="" by SourceComputerId
    | extend compliance=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0, 2, 1)
    | extend ComplianceOrder=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0 or missingOtherUpdatesCount > 0, 1, 3)
)
on SourceComputerId
| project id=SourceComputerId, displayName=Computer, sourceComputerId=SourceComputerId, scopedToUpdatesSolution=true, missingCriticalUpdatesCount=coalesce(missingCriticalUpdatesCount, -1), missingSecurityUpdatesCount=coalesce(missingSecurityUpdatesCount, -1), missingOtherUpdatesCount=coalesce(missingOtherUpdatesCount, -1), compliance=coalesce(compliance, 4), lastAssessedTime, lastUpdateAgentSeenTime, osType=2, environment=iff(ComputerEnvironment=~"Azure", 1, 2), ComplianceOrder=coalesce(ComplianceOrder, 2) )
| order by ComplianceOrder asc, missingCriticalUpdatesCount desc, missingSecurityUpdatesCount desc, missingOtherUpdatesCount desc, displayName asc
| project-away ComplianceOrder
```

#### <a name="missing-updates-list"></a>Eksik güncelleştirmeler listesi

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, BulletinUrl, BulletinID) by SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| project-away UpdateState, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(Product, "_", ProductArch), displayName=Product, productArch=ProductArch, classification=Classification, InformationId=BulletinID, InformationUrl=tostring(split(BulletinUrl, ";", 0)[0]), osType=1
| union(Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, KBID, PublishedDate, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| project-away UpdateState, Approved, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), displayName=any(Title), publishedDate=min(PublishedDate), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(UpdateID, "_", KBID), classification=Classification, InformationId=strcat("KB", KBID), InformationUrl=iff(isnotempty(KBID), strcat("https://support.microsoft.com/kb/", KBID), ""), osType=2)
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl
```

## <a name="next-steps"></a>Sonraki adımlar

* Ayrıntılı güncelleştirme verilerini görüntülemek için [Azure izleyici günlüklerinde](../log-analytics/log-analytics-log-searches.md) günlük aramalarını kullanın.
* Güncelleştirme dağıtım durumu için [uyarı oluştur](automation-tutorial-update-management.md#configure-alerts) .
