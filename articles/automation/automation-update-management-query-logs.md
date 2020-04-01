---
title: Azure Güncelleştirme Yönetimi günlüklerini sorgula
description: Bu makalede, Log Analytics çalışma alanınızda Güncelleştirme Yönetimi günlüklerinin nasıl sorgulanır.
services: automation
ms.subservice: update-management
ms.date: 03/31/2020
ms.topic: conceptual
ms.openlocfilehash: 863016bbeda9b4aec3bf2b4e12830bd30098150f
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437836"
---
# <a name="query-update-records-for-update-management-in-azure-monitor-logs"></a>Azure Monitör Günlüklerinde Güncelleştirme Yönetimi kayıtlarını sorgula

Güncelleme Yönetimi çözümünde sağlanan ayrıntılara ek olarak, Log Analytics çalışma alanınızda depolanan günlüklere karşı arama yapabilirsiniz. Çözüm sayfasından, sol bölmede **Günlükleri**seçin. **Günlük Arama** sayfası açılır.

Ayrıca sorguları nasıl özelleştirebileceğinizi veya farklı istemcilerden nasıl kullanacağınızı ve daha fazlasını ziyaret ederek öğrenebilirsiniz: [Log Analytics arama API belgeleri.](https://dev.loganalytics.io/)

## <a name="update-records"></a>Güncelleştirme kayıtları

Windows ve Linux VM'leri için Güncelleştirme Yönetimi tarafından toplanan kayıtlar ve günlük arama sonuçlarında görünen veri türleri. Aşağıdaki bölümlerde bu kayıtlar açıklayınız.

### <a name="required-updates"></a>Zorunlu güncelleştirmeler

Bir makine tarafından `RequiredUpdate` gerekli güncelleştirmeleri temsil eden bir türe sahip bir kayıt oluşturulur. Bu kayıtların özellikleri aşağıdaki tabloda dır:

| Özellik | Açıklama | 
|----------|-------------|
| Bilgisayar | Raporlama makinesinin tam nitelikli alan adı. |
| KBID | Windows güncelleştirmesi için bilgi temel makale kimliği. |
| ManagementGroupName | Operasyon Yöneticisi yönetim grubunun veya Log Analytics çalışma alanının adı. | 
| Ürün | Güncelleştirmenin geçerli olduğu ürünler. | 
| PublishDate | Güncelleştirmenin Windows Update'ten indirilip yüklenmeye hazır olduğu tarih. |
| Sunucu | | 
| KaynakHealthServiceId | Log Analytics Windows aracı kimliği'ni temsil eden benzersiz tanımlayıcı. |
| SourceSystem | *OperationsManager* | 
| TenantId | Kuruluşlarınizin Azure Etkin Dizini örneğini temsil eden benzersiz tanımlayıcı. | 
| TimeGenerated | Kaydın oluşturulduğu tarih ve saat. | 
| Tür | *Güncelleştirme* | 
| Güncelleme Sınıflandırması | Uygulanabilecek güncelleştirmelerin türünü gösterir. Windows için:<br> *Kritik güncellemeler*<br> *Güvenlik güncelleştirmeleri*<br> *Güncelleştirme paketleri*<br> *Özellik paketleri*<br> *Hizmet paketleri*<br> *Tanım güncelleştirmeleri*<br> *Araçlar*<br> *Güncelleştirmeler*. Linux için:<br> *Kritik güncelleştirmeler ve güvenlik güncelleştirmeleri*<br> *Diğer* |
| UpdateSeverity | Güvenlik açığı için önem derecesi. Değerler şunlardır:<br> *Kritik*<br> *Önemli*<br> *Orta*<br> *Düşük* |
| Güncelleme Başlığı | Güncelleştirmenin başlığı.|

### <a name="update"></a>Güncelleştirme

Kullanılabilir güncelleştirmeleri ve `Update` bir makine için kurulum durumlarını temsil eden bir türe sahip bir kayıt oluşturulur. Bu kayıtların özellikleri aşağıdaki tabloda dır:

| Özellik | Açıklama | 
|----------|-------------|
| OnayKaynak | Yalnızca Windows işletim sistemi için geçerlidir. Değer *Microsoft*Update'tir. |
| Onaylandı | *Doğru* veya *Yanlış* |
| Sınıflandırma | *Güncelleştirmeler* |
| Bilgisayar | Raporlama makinesinin tam nitelikli alan adı. |
| BilgisayarOrtamı | *Azure* veya *Azure'suz.* |
| MSRCBulletinID | Güvenlik bülteni kimlik numarası | 
| MSRCSeverity | Güvenlik açığı için önem derecesi. Değerler şunlardır:<br> *Kritik*<br> *Önemli*<br> *Orta*<br> *Düşük* |  
| KBID | Windows güncelleştirmesi için bilgi temel makale kimliği. |
| ManagementGroupName | Operasyon Yöneticisi yönetim grubunun veya Log Analytics çalışma alanının adı. |
| UpdateID | Yazılım güncelleştirmesinin benzersiz tanımlayıcısı. |
| RevisionNumber | Bir güncelleştirmenin belirli bir revizyonunun düzeltme numarası. |
| İsteğe bağlı | *Doğru* veya *Yanlış* | 
| RebootBehavior | Güncelleştirmeyi yükledikten/kaldırmadan sonra yeniden başlatma davranışı. |
| _ResourceId | Kaydın ilişkili olduğu kaynak için benzersiz tanımlayıcı. |
| Tür | *Güncelleştirme* |
| VMUUID | Sanal makine için benzersiz tanımlayıcı. |
| MG | Yönetim grubu veya Log Analytics çalışma alanı için benzersiz tanımlayıcı. | 
| TenantId | Kuruluşlarınizin Azure Etkin Dizini örneğini temsil eden benzersiz tanımlayıcı. | 
| SourceSystem | *OperationsManager* | 
| TimeGenerated | Kaydın oluşturulduğu tarih ve saat. | 
| SourceComputerId | Kaynak bilgisayarı temsil eden benzersiz tanımlayıcı. | 
| Başlık | Güncelleştirmenin başlığı. |
| Yayın Tarihi (UTC) | Güncelleştirmenin Windows Update'ten indirilip yüklenmeye hazır olduğu tarih.  |
| UpdateState | Güncelleştirmenin geçerli durumu. | 
| Ürün | Güncelleştirmenin geçerli olduğu ürünler. |
| SubscriptionId | Azure aboneliğinin benzersiz tanımlayıcısı. | 
| ResourceGroup | Kaynağın üyesi olduğu kaynak grubunun adı. | 
| ResourceProvider | Kaynak sağlayıcısını belirtir. | 
| Kaynak | Kaynağın adı. | 
| ResourceType | Kaynak türünün adı. | 

### <a name="update-agent"></a>Temsilciyi Güncelle

Makinedeki güncelleştirme aracısının ayrıntılarını sağlayan bir türe `UpdateAgent` sahip bir kayıt oluşturulur. Bu kayıtların özellikleri aşağıdaki tabloda dır:

| Özellik | Açıklama | 
|----------|-------------|
| AgeofOldestMissingGerekliUpdate | | 
| Otomatik Güncelleştirme Etkin | | 
| Bilgisayar | Raporlama makinesinin tam nitelikli alan adı. |
| DaySinceLastUpdateBucket | | 
| ManagementGroupName | Operasyon Yöneticisi yönetim grubunun veya Log Analytics çalışma alanının adı. |
| OSVersion | İşletim sisteminin sürümü. |
| Sunucu | |
| KaynakHealthServiceId | Log Analytics Windows aracı kimliği'ni temsil eden benzersiz tanımlayıcı. |
| SourceSystem | *OperationsManager* | 
| TenantId | Kuruluşlarınizin Azure Etkin Dizini örneğini temsil eden benzersiz tanımlayıcı. |
| TimeGenerated | Kaydın oluşturulduğu tarih ve saat. |
| Tür | *Güncelleştirme* | 
| WindowsUpdateAgentVersion | Windows Update aracısının sürümü. |
| WSUSServer | Windows Update aracısının sorun giderme de yardımcı olmak için bir sorun varsa hataları gösterir. |

### <a name="update-deployment-status"></a>Dağıtım Durumunu Güncelleştir 

Makine tarafından zamanlanan `UpdateRunProgress` bir dağıtımın güncelleştirme dağıtım durumunu sağlayan bir türe sahip bir kayıt oluşturulur. Bu kayıtların özellikleri aşağıdaki tabloda dır:

| Özellik | Açıklama | 
|----------|-------------|
| Bilgisayar | Raporlama makinesinin tam nitelikli alan adı. |
| BilgisayarOrtamı | *Azure* veya *Azure'suz.* | 
| CorrelationId | Güncelleştirme için runbook iş çalıştırın benzersiz tanımlayıcısı. |
| EndTime | Eşitleme işleminin sona erdirilmesi zamanı. | 
| Hata Sonucu | Güncelleştirme yüklenmezse oluşturulan Windows Update hata kodu. | 
| Kurulum Durumu | İstemci bilgisayarda bir güncelleştirmenin olası yükleme durumları, *Devam eden*, *Başarılı*, Kısmen *başarısız oldu.* |
| KBID | Windows güncelleştirmesi için bilgi temel makale kimliği. | 
| ManagementGroupName | Operasyon Yöneticisi yönetim grubunun veya Log Analytics çalışma alanının adı. |
| OSType | İşletim sisteminin, *Windows'un* veya *Linux'un*türünü belirtir. | 
| Ürün | Güncelleştirmenin geçerli olduğu ürünler. |
| Kaynak | Kaynağın adı. | 
| ResourceId | Kaydın ilişkili olduğu kaynak için benzersiz tanımlayıcı. |
| ResourceProvider | Kaynak sağlayıcısını belirtir. | 
| ResourceType | Kaynak türünün adı. | 
| SourceComputerId | Kaynak bilgisayarı temsil eden benzersiz tanımlayıcı. | 
| SourceSystem | *OperationsManager* |
| StartTime | Güncelleştirmenin yüklenmesi zamanlandığında ki zaman. |
| SubscriptionId | Azure aboneliğinin benzersiz tanımlayıcısı. | 
| BaşarılıOnRetry | Güncelleştirme yürütmesinin ilk denemede başarısız olduğunu ve geçerli işlemin yeniden deneme girişimi olduğunu gösterir. |
| TimeGenerated | Kaydın oluşturulduğu tarih ve saat. |
| Başlık | Güncelleştirmenin başlığı. |
| Tür | *UpdateRunProgress* |
| UpdateId | Yazılım güncelleştirmesinin benzersiz tanımlayıcısı. |
| VMUUID | Sanal makine için benzersiz tanımlayıcı. |
| _ResourceId | Kaydın ilişkili olduğu kaynak için benzersiz tanımlayıcı. |

### <a name="update-summary"></a>Özeti Güncelle 

Makine tarafından güncelleştirme `UpdateSummary` özeti sağlayan bir türe sahip bir kayıt oluşturulur. Bu kayıtların özellikleri aşağıdaki tabloda dır:

| Özellik | Açıklama | 
|----------|-------------|
| Bilgisayar | Raporlama makinesinin tam nitelikli alan adı. |
| BilgisayarOrtamı | *Azure* veya *Azure'suz.* | 
| CriticalUpdatesMissing | Geçerli olan kritik güncelleştirmelerin sayısı eksiktir. | 
| ManagementGroupName | Operasyon Yöneticisi yönetim grubunun veya Log Analytics çalışma alanının adı. |
| NETRuntimeVersion | Windows bilgisayarında yüklü .NET Framework sürümü. |
| OldestMissingSecurityUpdateBucket | Değerler şunlardır:<br> Değeri 30 günden azise *son*<br> *30 gün önce*<br> *60 gün önce*<br> *90 gün önce*<br> *120 gün önce*<br> *150 gün önce*<br> *180 gün önce*<br> Değer 180 günden büyük olduğunda *daha eski* | 
| OldestMissingSecurityUpdateInDays | Yüklenmemiş olarak algılanan en eski güncelleştirme için toplam gün sayısı. |
| OsVersion | İşletim sisteminin sürümü. |
| OtherUpdatesMissing | Algılanan güncelleştirme sayısı eksik. |
| Kaynak |  Kaynağın adı. | 
| ResourceGroup | Kaynağın üyesi olduğu kaynak grubunun adı. |
| ResourceId | Kaydın ilişkili olduğu kaynak için benzersiz tanımlayıcı. |
| ResourceProvider | Kaynak sağlayıcısını belirtir. |
| ResourceType | Kaynak türünün adı. |
| Yeniden Başlatma | *Doğru* veya *Yanlış*. |
| SecurityUpdatesMissing | Eksik güvenlik güncelleştirmeleri sayısı uygulanabilir.| 
| SourceComputerId | Sanal makine için benzersiz tanımlayıcı. |
| SourceSystem | *OpsManager* | 
| SubscriptionId | Azure aboneliğinin benzersiz tanımlayıcısı. |
| TimeGenerated | Kaydın oluşturulduğu tarih ve saat. |
| TotalUpdatesMissing | Geçerli olan toplam eksik güncelleştirme sayısı. | 
| Tür | *UpdateSummary* |
| VMUUID | Sanal makine için benzersiz tanımlayıcı. |
| WindowsUpdateAgentVersion | Windows Update aracısının sürümü. |
| WindowsUpdateSetting | Windows Update aracısının durumunu gösterir. Olası değerler şunlardır:<br> *Zamanlanmış yükleme*<br> *Kurulumdan önce bildirin*<br> Hata sağlıksız WUA aracısından döndü. | 
| WSUSServer | Windows Update aracısının sorun giderme de yardımcı olmak için bir sorun varsa hataları gösterir. |
| _ResourceId | Kaydın ilişkili olduğu kaynak için benzersiz tanımlayıcı. |

## <a name="sample-queries"></a>Örnek sorgular

Aşağıdaki bölümler, Güncelleştirme Yönetimi için toplanan güncelleştirme kayıtları için örnek günlük sorguları sağlar.

### <a name="confirm-that-non-azure-machines-are-onboarded"></a>Azure olmayan makinelerin yerleşik olduğunu doğrulayın

Doğrudan bağlı makinelerin Azure Monitor günlükleriyle iletişim kurduğunu doğrulamak için aşağıdaki günlük aramalarından birini çalıştırın.

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

Windows bilgisayarında, Azure Monitor günlükleriyle aracı bağlantısını doğrulamak için aşağıdaki bilgileri gözden geçirebilirsiniz:

1. Denetim Masası'nda **Microsoft Monitoring Agent'ı**açın. Azure **Günlük Analizi** sekmesinde, aracı aşağıdaki iletiyi görüntüler: **Microsoft İzleme Aracısı Log Analytics'e başarıyla bağlanmıştır.**
2. Windows Olay Günlüğü'nün açılışını yapın. Uygulama **ve Hizmet Günlükleri\Operasyon Yöneticisi'ne** gidin ve kaynak **Servis Bağlayıcısından**Olay Kimliği 3000 ve Olay Kimliği 5002'yi arayın. Bu olaylar, bilgisayarın Log Analytics çalışma alanına kaydolduğunu ve yapılandırmayı aldığını gösterir.

Aracı Azure Monitor günlükleriyle iletişim kuramıyorsa ve aracı bir güvenlik duvarı veya proxy sunucusu aracılığıyla Internet ile iletişim kurmak üzere yapılandırılırsa, güvenlik duvarının veya proxy sunucusunun düzgün şekilde yapılandırıldığına onaylayın. Güvenlik duvarının veya proxy sunucusunun doğru şekilde nasıl yapılandırıldığı hakkında bilgi edinmek [için, Windows aracısı için Ağ yapılandırmasına](../azure-monitor/platform/agent-windows.md) veya [Linux aracısı için Ağ yapılandırmasına](../log-analytics/log-analytics-agent-linux.md)bakın.

> [!NOTE]
> Linux sistemleriniz bir proxy veya Log Analytics Ağ Geçidi ile iletişim kurmak üzere yapılandırıldıysa ve bu çözüme biniyorsanız, aşağıdaki komutları kullanarak dosyadaki okuma iznini omiuser grubuna vermek için *proxy.conf* izinlerini güncelleyin:
>
> `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`

Yeni eklenen Linux aracıları, bir değerlendirme yapıldıktan sonra **Güncelleştirilmiş** bir durum gösterir. Bu işlem 6 saat kadar sürebilir.

Bir Operasyon Yöneticisi yönetim grubunun Azure Monitor günlükleriyle iletişim kurduğunu doğrulamak için [Bkz.](../azure-monitor/platform/om-agents.md#validate-operations-manager-integration-with-azure-monitor)

### <a name="single-azure-vm-assessment-queries-windows"></a>Tek Azure VM Değerlendirme sorguları (Windows)

VMUUID değerini, sorguladığınız sanal makinenin VM GUID değeriyle değiştirin. Azure Monitor günlüklerinde aşağıdaki sorguyu çalıştırarak kullanılması gereken VMUUID'yi bulabilirsiniz:`Update | where Computer == "<machine name>" | summarize by Computer, VMUUID`

#### <a name="missing-updates-summary"></a>Eksik güncelleştirmeler özeti

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

### <a name="single-azure-vm-assessment-queries-linux"></a>Tek Azure VM değerlendirme sorguları (Linux)

Bazı Linux dağıtımları için, Azure Kaynak Yöneticisi'nden gelen VMUUID değeri yle ve Azure Monitor günlüklerinde depolananlarla bir [son durum uyuşmazlığı](https://en.wikipedia.org/wiki/Endianness) vardır. Aşağıdaki sorgu, her iki uçta da bir eşleşmeyi denetler. Sonuçları düzgün bir şekilde döndürmek için VMUUID değerlerini GUID'in büyük enson ve az enson formatıyla değiştirin. Azure Monitor günlüklerinde aşağıdaki sorguyu çalıştırarak kullanılması gereken VMUUID'yi bulabilirsiniz:`Update | where Computer == "<machine name>"
| summarize by Computer, VMUUID`

#### <a name="missing-updates-summary"></a>Eksik güncelleştirmeler özeti

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

### <a name="multi-vm-assessment-queries"></a>Çoklu VM değerlendirme sorguları

#### <a name="computers-summary"></a>Bilgisayar özeti

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

#### <a name="missing-updates-summary"></a>Eksik güncelleştirmeler özeti

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

* Ayrıntılı güncelleştirme verilerini görüntülemek için [Azure Monitor günlüklerinde](../log-analytics/log-analytics-log-searches.md) günlük aramalarını kullanın.
* Güncelleştirme dağıtım durumu için [uyarılar oluşturun.](automation-tutorial-update-management.md#configure-alerts)
