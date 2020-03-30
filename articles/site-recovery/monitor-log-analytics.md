---
title: Azure Monitör Günlükleri ile Azure Site Kurtarma'yı izleme
description: Azure Monitör Günlükleri (Log Analytics) ile Azure Site Kurtarma'yı nasıl izleyeceğinizi öğrenin
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/15/2019
ms.author: raynew
ms.openlocfilehash: f20d0d38a7fbd831d3e97a69373bac04b9b330aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74133426"
---
# <a name="monitor-site-recovery-with-azure-monitor-logs"></a>Azure İzleyici Günlükleriyle Site Recovery’yi izleme

Bu makalede, [Azure Monitör Günlükleri](../azure-monitor/platform/data-platform-logs.md)ve [Günlük Analitiği](../azure-monitor/log-query/log-query-overview.md)kullanılarak Azure [Site Kurtarma](site-recovery-overview.md)tarafından çoğaltılan makinelerin nasıl izlendiği açıklanmaktadır.

Azure Monitor Günlükleri, diğer izleme verilerinin yanı sıra etkinlik ve tanılama günlüklerini toplayan bir günlük veri platformu sağlar. Azure Monitör Günlükleri'nde, günlük sorgularını yazmak ve test etmek ve günlük verilerini etkileşimli olarak analiz etmek için Log Analytics'i kullanırsınız. Günlük sonuçlarını görselleştirebilir ve sorgulayabilir ve izlenen verilere dayalı eylemleri yapmak için uyarıları yapılandırabilirsiniz.

Site Kurtarma için, aşağıdakileri yapmanıza yardımcı olmak için Azure Monitör Günlükleri'ni izleyebilirsiniz:

- **Site Kurtarma sağlık ve durumunu izleyin.** Örneğin, çoğaltma durumunu, test başarısız durumunu, Site Kurtarma olaylarını, korumalı makineler için kurtarma noktası hedeflerini (RDO' ları) ve disk/veri değiştirme oranlarını izleyebilirsiniz.
- **Site Kurtarma için uyarılar ayarlayın.** Örneğin, makine durumu, test başarısız durumu veya Site Kurtarma iş durumu için uyarıları yapılandırabilirsiniz.

Site Kurtarma ile Azure Monitör Günlükleri'nin kullanılması **Azure'dan Azure'a** çoğaltma için ve **VMware VM/fiziksel sunucudan Azure çoğaltmasına** desteklenir.

> [!NOTE]
> VMware ve fiziksel makineler için karmaşalı veri günlükleri ve yükleme hızı günlüklerini almak için İşlem Sunucusu'na bir Microsoft izleme aracısı yüklemeniz gerekir. Bu aracı, çoğaltma makinelerinin günlüklerini çalışma alanına gönderir. Bu özellik sadece 9.30 mobilite aracısı sürümü için kullanılabilir.

## <a name="before-you-start"></a>Başlamadan önce

İşte gerekenler:

- Kurtarma Hizmetleri kasasında korunan en az bir makine.
- Site Kurtarma günlüklerini depolamak için bir Günlük Analizi çalışma alanı. Çalışma alanı ayarlama [hakkında bilgi edinin.](../azure-monitor/learn/quick-create-workspace.md)
- Log Analytics'te günlük sorgularının nasıl yazılabildiğini, çalıştırılabildiğini ve analiz edilebildiğini temel bir şekilde anlayın. [Daha fazla bilgi edinin](../azure-monitor/log-query/get-started-portal.md).

Başlamadan önce sık sorulan izleme sorularını gözden [geçirmenizi](monitoring-common-questions.md) öneririz.

## <a name="configure-site-recovery-to-send-logs"></a>Günlükleri göndermek için Site Kurtarma'yı yapılandırma

1. Kasada, **Tanılama ayarlarını** > tıklatın**Tanılama ayarı ekleyin.**

    ![Tanısal günlüğe kaydetmeyi seçin](./media/monitoring-log-analytics/add-diagnostic.png)

2. **Tanılama ayarlarında,** bir ad belirtin ve Günlük **Analitiğine Gönder**kutusunu işaretleyin.
3. Azure Monitör Günlükleri aboneliğini ve Günlük Analizi çalışma alanını seçin.
4. Geçişte **Azure Tanılama'yı** seçin.
5. Günlük listesinden **AzureSiteRecovery**önekiyle tüm günlükleri seçin. Ardından **Tamam**'a tıklayın.

    ![Çalışma alanını seçme](./media/monitoring-log-analytics/select-workspace.png)

Site Kurtarma günlükleri, seçili çalışma alanında bir tabloya **(Azure Diagnostics)** beslemeye başlar.

## <a name="configure-microsoft-monitoring-agent-on-the-process-server-to-send-churn-and-upload-rate-logs"></a>İşlem Sunucusu'ndaki Microsoft izleme aracısını karmaşa ve yükleme hızı günlükleri göndermek için yapılandırın

VMware/fiziksel makineleriniz için veri karmaşası oranı bilgilerini ve kaynak veri yükleme hızı bilgilerini şirket içinde yakalayabilirsiniz. Bunu etkinleştirmek için İşlem Sunucusu'na bir Microsoft izleme aracısının yüklenmesi gerekir.

1. Log Analytics çalışma alanına gidin ve **Gelişmiş Ayarlar'a**tıklayın.
2. Bağlı **Kaynaklar** sayfasına tıklayın ve windows **sunucularını**seçin.
3. Windows Agent'ı (64 bit) İşlem Sunucusu'na indirin. 
4. [Çalışma alanı kimliğini ve anahtarı edinme](../azure-monitor/platform/agent-windows.md#obtain-workspace-id-and-key)
5. [TLS 1.2 kullanmak üzere aracıyı yapılandır](../azure-monitor/platform/agent-windows.md#configure-agent-to-use-tls-12)
6. Elde edilen çalışma alanı kimliğini ve anahtarı sağlayarak [aracı yüklemeyi tamamlayın.](../azure-monitor/platform/agent-windows.md#install-the-agent-using-setup-wizard)
7. Yükleme tamamlandıktan sonra, Log Analytics çalışma alanına gidin ve **Gelişmiş Ayarlar'a**tıklayın. **Veri** sayfasına gidin ve **Windows Performans Sayaçları'na**daha fazla tıklayın. 
8. Örnek aralığı 300 saniye olan aşağıdaki iki sayıcıyı eklemek için **'+'** düğmesine tıklayın:

        ASRAnalytics(*)\SourceVmChurnRate 
        ASRAnalytics(*)\SourceVmThrpRate 

Karmaşa ve yükleme hızı verileri çalışma alanına beslemeye başlar.


## <a name="query-the-logs---examples"></a>Günlükleri sorgula - örnekler

[Kusto sorgu dili](../azure-monitor/log-query/get-started-queries.md)ile yazılmış günlük sorgularını kullanarak günlüklerden veri alırsınız. Bu bölümde, Site Kurtarma izleme için kullanabileceğiniz ortak sorguların birkaç örneği yer almaktadır.

> [!NOTE]
> Bazı örnekler **a2A**ayarlanmış **replicationProviderName_s** kullanın. Bu, Site Kurtarma'yı kullanarak ikincil bir Azure bölgesine çoğaltılan Azure VM'lerini alır. Bu örneklerde, Site Kurtarma kullanarak Azure'a çoğaltılan şirket içi VMware VM'leri veya fiziksel sunucuları almak istiyorsanız, **A2A'yı** **InMageAzureV2**ile değiştirebilirsiniz.


### <a name="query-replication-health"></a>Sorgu çoğaltma durumu

Bu sorgu, tüm korumalı Azure VM'lerinin geçerli çoğaltma durumu için bir pasta grafiği çizer ve üç durum la ayrılır: Normal, Uyarı veya Kritik.

```
AzureDiagnostics  
| where replicationProviderName_s == "A2A"   
| where isnotempty(name_s) and isnotnull(name_s)  
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s  
| project name_s , replicationHealth_s  
| summarize count() by replicationHealth_s  
| render piechart   
```
### <a name="query-mobility-service-version"></a>Mobilite hizmet sürümünü sorgula

Bu sorgu, Çalıştıkları Mobilite aracısının sürümüne göre ayrılmış, Site Kurtarma ile çoğaltılan Azure VM'leri için bir pasta grafiği çizer.

```
AzureDiagnostics  
| where replicationProviderName_s == "A2A"   
| where isnotempty(name_s) and isnotnull(name_s)  
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s  
| project name_s , agentVersion_s  
| summarize count() by agentVersion_s  
| render piechart 
```

### <a name="query-rpo-time"></a>RPO süresini sorgula

Bu sorgu, Kurtarma noktası hedefine (RPO) göre ayrılmış, Site Kurtarma ile çoğaltılan Azure VM'lerinin çubuk grafiğini çizer: 15 dakikadan az, 15-30 dakika arasında, 30 dakikadan fazla.

```
AzureDiagnostics 
| where replicationProviderName_s == "A2A"   
| where isnotempty(name_s) and isnotnull(name_s)  
| extend RPO = case(rpoInSeconds_d <= 900, "<15Min",   
rpoInSeconds_d <= 1800, "15-30Min", ">30Min")  
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s  
| project name_s , RPO  
| summarize Count = count() by RPO  
| render barchart 
```

![Sorgu RPO](./media/monitoring-log-analytics/example1.png)

### <a name="query-site-recovery-jobs"></a>Site Kurtarma işlerini sorgula

Bu sorgu, son 72 saat içinde tetiklenen tüm Site Kurtarma işlerini (tüm olağanüstü durum kurtarma senaryoları için) ve bunların tamamlanma durumunu alır.

```
AzureDiagnostics  
| where Category == "AzureSiteRecoveryJobs"  
| where TimeGenerated >= ago(72h)   
| project JobName = OperationName , VaultName = Resource , TargetName = affectedResourceName_s, State = ResultType  
```

### <a name="query-site-recovery-events"></a>Site Kurtarma olaylarını sorgula

Bu sorgu, son 72 saat içinde yükseltilen tüm Site Kurtarma olaylarını (tüm olağanüstü durum kurtarma senaryoları için) önemderecesiyle birlikte alır. 

```
AzureDiagnostics   
| where Category == "AzureSiteRecoveryEvents"   
| where TimeGenerated >= ago(72h)   
| project AffectedObject=affectedResourceName_s , VaultName = Resource, Description_s = healthErrors_s , Severity = Level  
```

### <a name="query-test-failover-state-pie-chart"></a>Sorgu testi başarısız durumu (pasta grafiği)

Bu sorgu, Site Kurtarma ile çoğaltılan Azure VM'lerinin test başarısızlığı durumu için bir pasta grafiği çizer.

```
AzureDiagnostics  
| where replicationProviderName_s == "A2A"   
| where isnotempty(name_s) and isnotnull(name_s)  
| where isnotempty(failoverHealth_s) and isnotnull(failoverHealth_s)  
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s  
| project name_s , Resource, failoverHealth_s  
| summarize count() by failoverHealth_s  
| render piechart 
```

### <a name="query-test-failover-state-table"></a>Sorgu testi başarısız durumu (tablo)

Bu sorgu, Site Kurtarma ile çoğaltılan Azure VM'lerinin test başarısızlığı durumu için bir tablo çizer.

```
AzureDiagnostics   
| where replicationProviderName_s == "A2A"   
| where isnotempty(name_s) and isnotnull(name_s)   
| where isnotempty(failoverHealth_s) and isnotnull(failoverHealth_s)   
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s   
| project VirtualMachine = name_s , VaultName = Resource , TestFailoverStatus = failoverHealth_s 
```

### <a name="query-machine-rpo"></a>Sorgu makinesi RPO

Bu sorgu, belirli bir Azure VM'nin (ContosoVM123) son 72 saat için RPO'sunun izini süren bir eğilim grafiği çizer.

```
AzureDiagnostics   
| where replicationProviderName_s == "A2A"   
| where TimeGenerated > ago(72h)  
| where isnotempty(name_s) and isnotnull(name_s)   
| where name_s == "ContosoVM123"  
| project TimeGenerated, name_s , RPO_in_seconds = rpoInSeconds_d   
| render timechart 
```
![Sorgu makinesi RPO](./media/monitoring-log-analytics/example2.png)

### <a name="query-data-change-rate-churn-and-upload-rate-for-an-azure-vm"></a>Azure VM için veri değiştirme oranını (karmaşa) ve yükleme oranını sorgula

Bu sorgu, veri değişim oranını (Saniyede Bayt yaz) ve veri yükleme oranını temsil eden belirli bir Azure VM (ContosoVM123) için bir eğilim grafiği çizer. 

```
AzureDiagnostics   
| where Category in ("AzureSiteRecoveryProtectedDiskDataChurn", "AzureSiteRecoveryReplicationDataUploadRate")   
| extend CategoryS = case(Category contains "Churn", "DataChurn",   
Category contains "Upload", "UploadRate", "none")  
| extend InstanceWithType=strcat(CategoryS, "_", InstanceName_s)   
| where TimeGenerated > ago(24h)   
| where InstanceName_s startswith "ContosoVM123"   
| project TimeGenerated , InstanceWithType , Churn_MBps = todouble(Value_s)/1048576   
| render timechart  
```
![Sorgu veri değişikliği](./media/monitoring-log-analytics/example3.png)

### <a name="query-data-change-rate-churn-and-upload-rate-for-a-vmware-or-physical-machine"></a>VMware veya fiziksel makine için veri değiştirme oranını (karmaşa) ve yükleme oranını sorgula

> [!Note]
> Bu günlükleri almak için Process Server'daki izleme aracısını ayarladığınızdan emin olun. [İzleme aracısını yapılandırmak için gereken adımlara](#configure-microsoft-monitoring-agent-on-the-process-server-to-send-churn-and-upload-rate-logs)bakın.

Bu sorgu, veri değişim oranını (Saniyede Bayt yaz) ve veri yükleme hızını temsil eden, çoğaltılmış bir **öğewin-9r7sfh9qlru'nun**belirli bir **disk disk0'ü** için bir eğilim grafiği çizer. Yinelenen öğenin **Diskler** bıçağında disk adını kurtarma hizmetleri kasasında bulabilirsiniz. Sorguda kullanılacak örnek adı, bu örnekte olduğu gibi makinenin DNS adı ve ardından gelen disk adıdır.

```
Perf
| where ObjectName == "ASRAnalytics"
| where InstanceName contains "win-9r7sfh9qlru_disk0"
| where TimeGenerated >= ago(4h) 
| project TimeGenerated ,CounterName, Churn_MBps = todouble(CounterValue)/5242880 
| render timechart
```
Process Server bu verileri her 5 dakikada bir Log Analytics çalışma alanına iter. Bu veri noktaları, 5 dakika için hesaplanan ortalamayı temsil ediyor.

### <a name="query-disaster-recovery-summary-azure-to-azure"></a>Olağanüstü durum kurtarma özetini sorgula (Azure'dan Azure'a)

Bu sorgu, ikincil bir Azure bölgesine çoğaltılan Azure VM'leri için bir özet tablosu çizer.  VM adı, çoğaltma ve koruma durumu, RPO, test başarısızlığı durumu, Mobilite aracısı sürümü, etkin çoğaltma hataları ve kaynak konumu gösterir.

```
AzureDiagnostics 
| where replicationProviderName_s == "A2A"   
| where isnotempty(name_s) and isnotnull(name_s)   
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s   
| project VirtualMachine = name_s , Vault = Resource , ReplicationHealth = replicationHealth_s, Status = protectionState_s, RPO_in_seconds = rpoInSeconds_d, TestFailoverStatus = failoverHealth_s, AgentVersion = agentVersion_s, ReplicationError = replicationHealthErrors_s, SourceLocation = primaryFabricName_s 
```

### <a name="query-disaster-recovery-summary-vmwarephysical-servers"></a>Olağanüstü durum kurtarma özetini sorgula (VMware/fiziksel sunucular)

Bu sorgu, Azure'da çoğaltılan VMware VM'leri ve fiziksel sunucuları için bir özet tablosu çizer.  Makine adı, çoğaltma ve koruma durumu, RPO, test hatası durumu, Mobilite aracısı sürümü, etkin çoğaltma hataları ve ilgili işlem sunucusunu gösterir.

```
AzureDiagnostics  
| where replicationProviderName_s == "InMageAzureV2"   
| where isnotempty(name_s) and isnotnull(name_s)   
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s   
| project VirtualMachine = name_s , Vault = Resource , ReplicationHealth = replicationHealth_s, Status = protectionState_s, RPO_in_seconds = rpoInSeconds_d, TestFailoverStatus = failoverHealth_s, AgentVersion = agentVersion_s, ReplicationError = replicationHealthErrors_s, ProcessServer = processServerName_g  
```

## <a name="set-up-alerts---examples"></a>Uyarıları ayarlama - örnekler

Azure Monitor verilerine dayalı Olarak Site Kurtarma uyarıları ayarlayabilirsiniz. Günlük uyarıları ayarlama hakkında [daha fazla bilgi edinin.](../azure-monitor/platform/alerts-log.md#managing-log-alerts-from-the-azure-portal) 

> [!NOTE]
> Bazı örnekler **a2A**ayarlanmış **replicationProviderName_s** kullanın. Bu, ikincil bir Azure bölgesine çoğaltılan Azure VM'leri için uyarılar ayarlar. Bu örneklerde, şirket içi VMware VM'ler veya Azure'da çoğaltılan fiziksel sunucular için uyarılar ayarlamak istiyorsanız **A2A'yı** **InMageAzureV2** ile değiştirebilirsiniz.

### <a name="multiple-machines-in-a-critical-state"></a>Kritik durumda birden çok makine

Çoğaltılan 20'den fazla Azure VM'si Kritik duruma giriyorsa bir uyarı ayarlayın.

```
AzureDiagnostics   
| where replicationProviderName_s == "A2A"   
| where replicationHealth_s == "Critical"  
| where isnotempty(name_s) and isnotnull(name_s)   
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s   
| summarize count() 
```
Uyarı için **Eşik değerini** 20 olarak ayarlayın.

### <a name="single-machine-in-a-critical-state"></a>Kritik durumda tek makine

Belirli bir yinelenen Azure VM kritik duruma geçerse bir uyarı ayarlayın.

```
AzureDiagnostics   
| where replicationProviderName_s == "A2A"   
| where replicationHealth_s == "Critical"  
| where name_s == "ContosoVM123"  
| where isnotempty(name_s) and isnotnull(name_s)   
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s   
| summarize count()  
```
Uyarı için **Eşik değerini** 1 olarak ayarlayın.

### <a name="multiple-machines-exceed-rpo"></a>Birden fazla makine RPO'u aşıyor

20'den fazla Azure VM'si için RPO 30 dakikayı aşarsa bir uyarı ayarlayın.
```
AzureDiagnostics   
| where replicationProviderName_s == "A2A"   
| where isnotempty(name_s) and isnotnull(name_s)   
| where rpoInSeconds_d > 1800  
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s   
| project name_s , rpoInSeconds_d   
| summarize count()  
```
Uyarı için **Eşik değerini** 20 olarak ayarlayın.

### <a name="single-machine-exceeds-rpo"></a>Tek makine RPO aşıyor

Tek bir Azure VM için RPO 30 dakikayı aşarsa bir uyarı ayarlayın.

```
AzureDiagnostics   
| where replicationProviderName_s == "A2A"   
| where isnotempty(name_s) and isnotnull(name_s)   
| where name_s == "ContosoVM123"  
| where rpoInSeconds_d > 1800  
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s   
| project name_s , rpoInSeconds_d   
| summarize count()  
```
Uyarı için **Eşik değerini** 1 olarak ayarlayın.

### <a name="test-failover-for-multiple-machines-exceeds-90-days"></a>Birden fazla makine için test başarısız90 günü aşıyor

Son başarılı test başarısızlığı 90 günden fazla ysa, 20 VM'den fazla bir uyarı ayarlayın. 

```
AzureDiagnostics  
| where replicationProviderName_s == "A2A"   
| where Category == "AzureSiteRecoveryReplicatedItems"  
| where isnotempty(name_s) and isnotnull(name_s)   
| where lastSuccessfulTestFailoverTime_t <= ago(90d)   
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s   
| summarize count()  
```
Uyarı için **Eşik değerini** 20 olarak ayarlayın.

### <a name="test-failover-for-single-machine-exceeds-90-days"></a>Tek makine için test başarısız90 günü aşıyor

Belirli bir VM için son başarılı test başarısızlığı 90 günden fazla olduysa bir uyarı ayarlayın.
```
AzureDiagnostics  
| where replicationProviderName_s == "A2A"   
| where Category == "AzureSiteRecoveryReplicatedItems"  
| where isnotempty(name_s) and isnotnull(name_s)   
| where lastSuccessfulTestFailoverTime_t <= ago(90d)   
| where name_s == "ContosoVM123"  
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by name_s   
| summarize count()  
```
Uyarı için **Eşik değerini** 1 olarak ayarlayın.

### <a name="site-recovery-job-fails"></a>Site Kurtarma işi başarısız olur

Bir Site Kurtarma işi (bu durumda Yeniden Koruma işi) son gün boyunca herhangi bir Site Kurtarma senaryosunda başarısız olursa bir uyarı ayarlayın. 
```
AzureDiagnostics   
| where Category == "AzureSiteRecoveryJobs"   
| where OperationName == "Reprotect"  
| where ResultType == "Failed"  
| summarize count()  
```

Uyarı için, hataları son gün deki hataları denetlemek için **Eşik değerini** 1'e, **Dönem'i** 1 dakikaya ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar

Dahili Site Kurtarma izleme [hakkında bilgi edinin.](site-recovery-monitor-and-troubleshoot.md)
