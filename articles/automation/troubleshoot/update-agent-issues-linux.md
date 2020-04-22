---
title: Azure Otomasyon Güncelleme Yönetimi'nde Linux güncelleştirme aracısı sorunlarını giderme
description: Update Management çözümlerini kullanarak Linux Windows update aracısıyla ilgili sorunları nasıl gidereceğinizi ve gidereceğinizi öğrenin.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 12/03/2019
ms.topic: conceptual
ms.service: automation
ms.subservice: update-management
manager: carmonm
ms.openlocfilehash: bba1c7e89a9c3bb1c9aa1567e36dd71a40f14636
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81679067"
---
# <a name="troubleshoot-linux-update-agent-issues"></a>Sorun giderme Linux güncelleme aracısı sorunları

Güncelleştirme Yönetimi'nde makinenizin hazır (sağlıklı) olarak görünmemesi birçok nedeni olabilir. Güncelleştirme Yönetimi'nde, altta yatan sorunu belirlemek için bir Karma Runbook Worker aracısının durumunu denetleyebilirsiniz. Bu makalede, Azure portalından Azure makineleri için sorun giderici nin nasıl çalıştırılacak olduğu ve [çevrimdışı senaryoda](#troubleshoot-offline)Azure olmayan makinelerüzerinde nasıl çalıştırılacakaçık. 

Aşağıdaki liste, bir makinenin içinde olabileceği üç hazır durumtur:

* Hazır - Hybrid Runbook Worker dağıtılır ve en son 1 saatten daha az bir süre önce görülmüştür.
* Bağlantısız - Hybrid Runbook Worker dağıtıldı ve en son 1 saat önce görüldü.
* Yapılandırılmamış - Karma Runbook Worker bulunamadı veya onboarding tamamlanmadı.

> [!NOTE]
> Azure portalının gösterdiği yle bir makinenin geçerli durumu arasında küçük bir gecikme olabilir.

## <a name="start-the-troubleshooter"></a>Sorun gidericiyi başlatın

Azure makineleri için, portaldaki **Temsilciye Hazırlık Durumunu Güncelleştir** sütunundaki **Sorun Giderme** bağlantısını tıklattığınızda Sorun Giderme Durumu Temsilcisi sayfası açılır. Azure olmayan makineler için bağlantı sizi bu makaleye getirir. Azure olmayan bir makinenin sorun gidermesini sağlamak için çevrimdışı yönergeleri görün.

![vm liste sayfası](../media/update-agent-issues-linux/vm-list.png)

> [!NOTE]
> Denetimler VM'nin çalışmasını gerektirir. VM çalışmıyorsa, size Bir **Başlat VM** düğmesi sunulur.

Sorun Giderme Sorunu Giderme Aracısı sayfasında, sorun gidericiyi başlatmak için **Denetimleri Çalıştır'ı**tıklatın. Sorun giderici, bağımlılıkları doğrulamak için makinede bir komut dosyası çalıştırmak için [Çalıştır komutunu](../../virtual-machines/linux/run-command.md) kullanır. Sorun giderici tamamlandığında, denetimlerin sonucunu döndürür.

![Sorun Giderme sayfası](../media/update-agent-issues-linux/troubleshoot-page.png)

Tamamlandığında, sonuçlar pencerede döndürülür. Denetim bölümleri, her çekin ne aradığı hakkında bilgi sağlar.

![Aracı denetimleri sayfasını güncelleştir](../media/update-agent-issues-linux/update-agent-checks.png)

## <a name="prerequisite-checks"></a>Önkoşul denetimleri

### <a name="operating-system"></a>İşletim sistemi

İşletim sistemi denetimi, Karma Runbook Worker'ın aşağıdaki işletim sistemlerinden birini çalıştırıp çalıştırmayı onaylar:

|İşletim sistemi  |Notlar  |
|---------|---------|
|CentOS 6 (x86/x64) ve 7 (x64)      | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır. Sınıflandırma tabanlı yama, CentOS'un kutusundan çıkarıolmayan güvenlik verilerini döndürmek için 'yum' gerektirir.         |
|Red Hat Enterprise 6 (x86/x64) ve 7 (x64)     | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.        |
|SUSE Linux Enterprise Server 11 (x86/x64) ve 12 (x64)     | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.        |
|Ubuntu 14,04 LTS, 16,04 LTS ve 18,04 LTS (x86/x64)      |Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.         |

## <a name="monitoring-agent-service-health-checks"></a>Ajan hizmeti sağlık kontrollerinin izlenmesi

### <a name="log-analytics-agent"></a>Log Analytics aracısı

Bu denetim, Linux için Log Analytics aracısının yüklü olmasını sağlar. Nasıl yüklenir yönergeleri [için, Linux için aracıyı yükleyin](../../azure-monitor/learn/quick-collect-linux-computer.md#install-the-agent-for-linux
)bakın.

### <a name="log-analytics-agent-status"></a>Günlük Analytics aracısı durumu

Bu denetim, Linux için Log Analytics aracısının çalışmasını sağlar. Aracı çalışmıyorsa, yeniden başlatmayı denemek için aşağıdaki komutu çalıştırabilirsiniz. Aracıyı sorun giderme hakkında daha fazla bilgi için [Linux Hybrid Runbook çalışanı sorun giderme](hybrid-runbook-worker.md#linux)

```bash
sudo /opt/microsoft/omsagent/bin/service_control restart
```

### <a name="multihoming"></a>Multihom

Bu denetim, aracının birden çok çalışma alanlarına bildirimde olup olmadığını belirler. Çoklu homing Güncelleştirme Yönetimi tarafından desteklenmez.

### <a name="hybrid-runbook-worker"></a>Karma Runbook Çalışanı

Bu denetim, Linux için Log Analytics aracısının Hibrit Runbook Worker paketine sahip olup olmadığını doğrular. Bu paket, Güncelleştirme Yönetimi'nin çalışması için gereklidir.

### <a name="hybrid-runbook-worker-status"></a>Karma Runbook İşçi durumu

Bu denetim, Karma Runbook Worker'ın makinede çalışmasını sağlar. Karma Runbook Worker doğru çalışıyorsa aşağıdaki işlemler bulunmalıdır. Daha fazla bilgi edinmek [için Linux için Log Analytics Agent sorun giderme bölümüne](hybrid-runbook-worker.md#oms-agent-not-running)bakın.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

## <a name="connectivity-checks"></a>Bağlantı denetimleri

### <a name="general-internet-connectivity"></a>Genel internet bağlantısı

Bu denetim, makinenin interneterişimi olmasını sağlar.

### <a name="registration-endpoint"></a>Kayıt bitiş noktası

Bu denetim, Karma Runbook Worker'ın Azure Otomasyonu ile Log Analytics çalışma alanı yla düzgün bir şekilde iletişim kurabileceğini belirler.

Proxy ve güvenlik duvarı yapılandırmaları, Karma Runbook Worker aracısının kayıt bitiş noktasıyla iletişim kurmasına izin vermelidir. Açılacak adres ler ve bağlantı noktaları listesi [için, Karma Çalışanlar için Ağ planlamasına](../automation-hybrid-runbook-worker.md#network-planning) bakın

### <a name="operations-endpoint"></a>İşlemler bitiş noktası

Bu denetim, aracının İş Çalıştırma Zamanı Veri Hizmeti ile düzgün bir şekilde iletişim kurayıp iletilemeyebileceğini belirler.

Proxy ve güvenlik duvarı yapılandırmaları, Karma Runbook Worker aracısının İş Çalıştırma Zamanı Veri Hizmeti ile iletişim kurmasına izin vermelidir. Açılacak adres ler ve bağlantı noktaları listesi [için, Karma Çalışanlar için Ağ planlamasına](../automation-hybrid-runbook-worker.md#network-planning) bakın

### <a name="log-analytics-endpoint-1"></a>Günlük Analytics bitiş noktası 1

Bu denetim, makinenizin Log Analytics aracısının ihtiyaç duyduğu uç noktalara erişebiliyor olduğunu doğrular.

### <a name="log-analytics-endpoint-2"></a>Günlük Analytics bitiş noktası 2

Bu denetim, makinenizin Log Analytics aracısının ihtiyaç duyduğu uç noktalara erişebiliyor olduğunu doğrular.

### <a name="log-analytics-endpoint-3"></a>Günlük Analytics bitiş noktası 3

Bu denetim, makinenizin Log Analytics aracısının ihtiyaç duyduğu uç noktalara erişebiliyor olduğunu doğrular.

## <a name="troubleshoot-offline"></a><a name="troubleshoot-offline"></a>Sorun giderme çevrimdışı

Komut dosyasını yerel olarak çalıştırarak, sorunlu gidericiyi Karma Runbook Çalışanı'nda çevrimdışı olarak kullanabilirsiniz. Python komut dosyası, [update_mgmt_health_check.py](https://gallery.technet.microsoft.com/scriptcenter/Troubleshooting-utility-3bcbefe6) Script Center bulunabilir. Bu komut dosyasının çıktısının bir örneği aşağıdaki örnekte gösterilmiştir:

```output
Debug: Machine Information:   Static hostname: LinuxVM2
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 00000000000000000000000000000000
           Boot ID: 00000000000000000000000000000000
    Virtualization: microsoft
  Operating System: Ubuntu 16.04.5 LTS
            Kernel: Linux 4.15.0-1025-azure
      Architecture: x86-64


Passed: Operating system version is supported

Passed: Microsoft Monitoring agent is installed

Debug: omsadmin.conf file contents:
        WORKSPACE_ID=00000000-0000-0000-0000-000000000000
        AGENT_GUID=00000000-0000-0000-0000-000000000000
        LOG_FACILITY=local0
        CERTIFICATE_UPDATE_ENDPOINT=https://00000000-0000-0000-0000-000000000000.oms.opinsights.azure.com/ConfigurationService.Svc/RenewCertificate
        URL_TLD=opinsights.azure.com
        DSC_ENDPOINT=https://scus-agentservice-prod-1.azure-automation.net/Accou            nts/00000000-0000-0000-0000-000000000000/Nodes\(AgentId='00000000-0000-0000-0000-000000000000'\)
        OMS_ENDPOINT=https://00000000-0000-0000-0000-000000000000.ods.opinsights            .azure.com/OperationalData.svc/PostJsonDataItems
        AZURE_RESOURCE_ID=/subscriptions/00000000-0000-0000-0000-000000000000/re            sourcegroups/myresourcegroup/providers/microsoft.compute/virtualmachines/linuxvm            2
        OMSCLOUD_ID=0000-0000-0000-0000-0000-0000-00
        UUID=00000000-0000-0000-0000-000000000000


Passed: Microsoft Monitoring agent is running

Passed: Machine registered with log analytics workspace:['00000000-0000-0000-0000-000000000000']

Passed: Hybrid worker package is present

Passed: Hybrid worker is running

Passed: Machine is connected to internet

Passed: TCP test for {scus-agentservice-prod-1.azure-automation.net} (port 443)             succeeded

Passed: TCP test for {eus2-jobruntimedata-prod-su1.azure-automation.net} (port 4            43) succeeded

Passed: TCP test for {00000000-0000-0000-0000-000000000000.ods.opinsights.azure.            com} (port 443) succeeded

Passed: TCP test for {00000000-0000-0000-0000-000000000000.oms.opinsights.azure.            com} (port 443) succeeded

Passed: TCP test for {ods.systemcenteradvisor.com} (port 443) succeeded

```

## <a name="next-steps"></a>Sonraki adımlar

Karma Runbook Çalışanlarınızla ilgili ek sorunları gidermek için [Sorun Giderme - Karma Runbook İşçileri](hybrid-runbook-worker.md)konusuna bakın.
