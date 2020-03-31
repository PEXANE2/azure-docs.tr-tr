---
title: Windows için Log Analytics aracısıyla sorun giderme
description: Azure Monitor'da Windows için Log Analytics aracısıyla en sık karşılaşılan sorunların belirtilerini, nedenlerini ve çözümünü açıklayın.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/21/2019
ms.openlocfilehash: 4112555347ce1d718375fbab3f166c6f2f5deeaa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80333515"
---
# <a name="how-to-troubleshoot-issues-with-the-log-analytics-agent-for-windows"></a>Windows için Log Analytics aracısıyla ilgili sorunları giderme 

Bu makalede, Azure Monitor'da Windows için Log Analytics aracısıyla karşılaşabileceğiniz sorun giderme hataları sağlar ve bunları çözmek için olası çözümler önerir.

Bu adımlardan hiçbiri sizin için çalışmıyorsa, aşağıdaki destek kanalları da kullanılabilir:

* Premier destek avantajları olan müşteriler [Premier](https://premier.microsoft.com/)ile bir destek isteği açabilir.
* Azure destek anlaşmaları olan [müşteriler, Azure portalında](https://manage.windowsazure.com/?getsupport=true)bir destek isteği açabilir.
* Gönderilen fikirleri ve hataları [https://aka.ms/opinsightsfeedback](https://aka.ms/opinsightsfeedback) incelemek veya yeni sini dosyalamak için Günlük Analitiği Geri Bildirimi sayfasını ziyaret edin. 

## <a name="important-troubleshooting-sources"></a>Önemli sorun giderme kaynakları

 Aracı, Windows için Log Analytics aracısıyla ilgili sorun giderme sorunlarına yardımcı olmak için, olayları özellikle *Uygulama ve Hizmetler\Operations Manager*altında Windows Olay Günlüğü'nde günlüğe kaydeder.  

## <a name="connectivity-issues"></a>Bağlantı sorunları

Aracı bir proxy sunucusu veya güvenlik duvarı üzerinden iletişim kuruyorsa, kaynak bilgisayar dan ve Azure Monitor hizmetinden iletişimi engelleyen kısıtlamalar olabilir. İletişimin engellenmesi durumunda, yanlış yapılandırma nedeniyle, aracıyı yüklemeye çalışırken veya aracıyı ek bir çalışma alanına rapor edecek şekilde yapılandırmaya çalışırken çalışma alanıyla kayıt başarısız olabilir. Aracı iletişimi başarılı bir kayıttan sonra başarısız olabilir. Bu bölümde, Windows aracısıyla ilgili bu tür bir sorunu gidermek için yöntemler açıklanmaktadır.

Güvenlik duvarının veya proxy'nin aşağıdaki tabloda açıklanan aşağıdaki bağlantı noktalarına ve URL'lere izin verecek şekilde yapılandırıldığından çift kontrol edin. Ayrıca, aracı ile Azure Monitor arasında güvenli bir TLS kanalının engellenmesinden, HTTP denetiminin web trafiği için etkinleştirilemediğini de onaylayın.  

|Aracı Kaynağı|Bağlantı Noktaları |Yön |HTTPS denetlemesini atlama|
|------|---------|--------|--------|   
|*.ods.opinsights.azure.com |Bağlantı Noktası 443 |Giden|Evet |  
|*.oms.opinsights.azure.com |Bağlantı Noktası 443 |Giden|Evet |  
|*.blob.core.windows.net |Bağlantı Noktası 443 |Giden|Evet |  

Azure Kamu için gerekli güvenlik duvarı bilgileri için Bkz. [Azure Kamu yönetimi.](../../azure-government/documentation-government-services-monitoringandmanagement.md#azure-monitor-logs) Ortamınızdaki runbook'ları veya yönetim çözümlerini kullanmak için Otomasyon hizmetine bağlanmak ve ona kaydolmak için Azure Otomasyon Karma Runbook Worker'ı kullanmayı planlıyorsanız, bu hizmet noktası numarasına ve [Karma Runbook Çalışanı için ağınızı Yapılandırma'da](../../automation/automation-hybrid-runbook-worker.md#network-planning)açıklanan URL'lere erişimi olmalıdır. 

Aracının Azure Monitor ile başarılı bir şekilde iletişim kurarak iletişim kurip kurmadığınızı doğrulamanın birkaç yolu vardır.

- Çalışma alanında [Azure Log Analytics Aracısı Sağlık değerlendirmesini](../insights/solution-agenthealth.md) etkinleştirin. Aracı Durumu panosundan, **aracının** listeli olup olmadığını hızlı bir şekilde görmek için yanıt vermeyen aracısayısı sütununa bakın.  

- Aracının rapor vermek üzere yapılandırıldığı çalışma alanına bir sinyal gönderdiğini doğrulamak için aşağıdaki sorguyu çalıştırın. Makinenin gerçek adı ile değiştirin. `<ComputerName>`

    ```
    Heartbeat 
    | where Computer like "<ComputerName>"
    | summarize arg_max(TimeGenerated, * ) by Computer 
    ```

    Bilgisayar hizmetle başarılı bir şekilde iletişim kuruyorsa, sorgu bir sonucu döndürmelidir. Sorgu bir sonuç döndürmediyse, önce aracının doğru çalışma alanına rapor vermek üzere yapılandırılmadığını doğrulayın. Doğru şekilde yapılandırılırsa, 3 adıma geçin ve aracının Azure Monitor ile iletişim kurmasını engelleyen hangi sorunu kaydedip kaydedilemeyebileceğini belirlemek için Windows Event Log'da arama yapın.

- Bir bağlantı sorununu tanımlamak için başka bir yöntem **TestCloudConnectivity** aracını çalıştırarak. Araç varsayılan olarak *%SystemRoot%\Program Files\Microsoft Monitoring Agent\Agent*klasöründe aracı ile yüklenir. Yükseltilmiş bir komut isteminden klasöre gidin ve aracı çalıştırın. Araç sonuçları döndürür ve testin başarısız olduğu yeri vurgular (örneğin, engellenen belirli bir bağlantı noktası/URL ile ilgiliyse). 

    ![TestCloudConnection araç yürütme sonuçları](./media/agent-windows-troubleshoot/output-testcloudconnection-tool-01.png)

- Aşağıdaki tablodan yazılı olaylar olup olmadığını doğrulamak için *Operasyon Yöneticisi* olay günlüğünü **Olay kaynaklarına** - *Sağlık Hizmeti Modülleri,* *HealthService*ve *Hizmet Bağlayıcısı* ve **Olay Düzeyi** *Uyarısı* ve *Hatası ile* filtre uygulayın. Varsa, olası her olay için dahil edilen çözüm adımlarını gözden geçirin.

    |Olay Kimliği |Kaynak |Açıklama |Çözüm |
    |---------|-------|------------|-----------|
    |2133 & 2129 |Sistem Sağlığı Hizmeti |Aracıdan hizmete bağlantı başarısız oldu |Bu hata, aracı doğrudan veya bir güvenlik duvarı/proxy sunucusu aracılığıyla Azure Monitor hizmetine iletişim kuramıyorsa oluşabilir. Aracı proxy ayarlarını veya ağ güvenlik duvarının/proxy'sinin bilgisayardan hizmete TCP trafiğine izin verdiğini doğrulayın.|
    |2138 |Sağlık Hizmeti Modülleri |Proxy kimlik doğrulaması gerektirir |Aracı proxy ayarlarını yapılandırın ve proxy sunucusuyla kimlik doğrulaması yapmak için gereken kullanıcı adını/parolayı belirtin. |
    |2129 |Sağlık Hizmeti Modülleri |Başarısız bağlantı/Başarısız TLS anlaşması |Ağ bağdaştırıcınızı TCP/IP ayarlarınızı ve aracı proxy ayarlarını kontrol edin.|
    |2127 |Sağlık Hizmeti Modülleri |Hata gönderme verisi alınan hata kodu |Eğer sadece gün içinde periyodik olarak gerçekleşirse, sadece göz ardı edilebilen rastgele bir anomali olabilir. Ne sıklıkta olduğunu anlamak için izleyin. Gün boyunca sık sık oluyorsa, önce ağ yapılandırmanızı ve proxy ayarlarınızı kontrol edin. Açıklama HTTP hata kodu 404 içeriyorsa ve aracı hizmete ilk kez veri göndermeye çalışırsa, iç 404 hata koduyla birlikte 500 hata içerir. 404 bulunamadı anlamına gelir, bu da yeni çalışma alanının depolama alanının hala sağlandığını gösterir. Bir sonraki yeniden denemede, veriler beklendiği gibi çalışma alanına başarıyla yazacaktır. BIR HTTP hatası 403 bir izin veya kimlik bilgileri sorunu gösterebilir. Sorunu gidermeye yardımcı olmak için 403 hatasıyla birlikte daha fazla bilgi vardır.|
    |4000 |Servis Konektörü |DNS ad çözümlemesi başarısız oldu |Makine, hizmete veri gönderirken kullanılan Internet adresini çözemedi. Bu, makinenizdeki DNS çözümleyici ayarları, yanlış proxy ayarları veya sağlayıcınızla ilgili geçici bir DNS sorunu olabilir. Düzenli aralıklarla gerçekleşirse, ağla ilgili geçici bir sorundan kaynaklanabilir.|
    |4001 |Servis Konektörü |Hizmete bağlantı başarısız oldu. |Bu hata, aracı doğrudan veya bir güvenlik duvarı/proxy sunucusu aracılığıyla Azure Monitor hizmetine iletişim kuramıyorsa oluşabilir. Aracı proxy ayarlarını veya ağ güvenlik duvarının/proxy'sinin bilgisayardan hizmete TCP trafiğine izin verdiğini doğrulayın.|
    |4002 |Servis Konektörü |Hizmet, bir sorguya yanıt olarak HTTP durum kodu 403 döndü. Hizmetin durumu için servis yöneticisine danışın. Sorgu daha sonra yeniden denenecektir. |Bu hata aracının ilk kayıt aşamasında yazılır ve aşağıdakilere benzer bir URL görürsünüz: *https://\<workspaceID>.oms.opinsights.azure.com/AgentService.svc/AgentTopologyRequest*. Hata kodu 403 yasak anlamına gelir ve yanlış yazılmış bir Çalışma Alanı kimliği veya anahtarı ndan kaynaklanabilir veya bilgisayardaki veri ve zaman yanlıştır. Geçerli zamana +/- 15 dakika ise, onboarding başarısız olur. Bunu düzeltmek için Windows bilgisayarınızın tarihini ve/veya saat dilimini güncelleştirin.|

## <a name="data-collection-issues"></a>Veri toplama sorunları

Aracı yüklendikten ve yapılandırılan çalışma alanına veya çalışma alanlarına rapor verdikten sonra, bilgisayarın etkinleştirilmesine ve hedefalındığına bağlı olarak yapılandırma, toplama veya yönlendirme performansı, günlükler veya diğer verileri hizmete almayı durdurabilir. Bu belirlemek için gereklidir:

- Belirli bir veri türü mü yoksa çalışma alanında bulunmayan tüm veriler mi?
- Veri türü bir çözüm tarafından mı belirtiliyor yoksa çalışma alanı veri toplama yapılandırmasının bir parçası olarak mı belirtiliyor?
- Kaç bilgisayar etkilenir? Çalışma alanına bildiren tek bir bilgisayar mı yoksa birden çok bilgisayar mı?
- Çalışıyor muydu ve günün belirli bir saatinde durdu mu, yoksa hiç toplanmadı mı? 
- Kullandığınız günlük arama sorgusu sözdizimi olarak doğru mu? 
- Aracı yapılandırmasını hiç Azure Monitor'dan aldı mı?

Sorun gidermedeki ilk adım, bilgisayarın bir sinyal olayı gönderip göndermeyini belirlemektir.

```
Heartbeat 
    | where Computer like "<ComputerName>"
    | summarize arg_max(TimeGenerated, * ) by Computer
```

Sorgu sonuçları döndürürse, belirli bir veri türünün toplanıp hizmete iletilmediğini belirlemeniz gerekir. Bunun nedeni, aracının hizmetten güncelleştirilmiş yapılandırma almaması veya aracının normal çalışmasını engelleyen başka bir belirti olabilir. Daha fazla sorun gidermek için aşağıdaki adımları gerçekleştirin.

1. Bilgisayarda yükseltilmiş bir komut istemi açın ve yazarak `net stop healthservice && net start healthservice`aracı hizmetini yeniden başlatın.
2. Operasyon *Yöneticisi* olay günlüğünü açın ve **Olay kaynağı** *HealthService'ten* *7023, 7024, 7025, 7028* ve *1210* **olay ihdası** arayın.  Bu olaylar aracının Azure Monitor'dan yapılandırmayı başarıyla aldığını ve bilgisayarı etkin bir şekilde izlediğini gösterir. Olay Kimliği 1210'un olay açıklaması, aracının izleme kapsamına giren tüm çözümleri ve Öngörüleri son satırda da belirtir.  

    ![Olay Kimliği 1210 açıklaması](./media/agent-windows-troubleshoot/event-id-1210-healthservice-01.png)

3. Birkaç dakika sonra sorgu sonuçlarında veya görselleştirmede beklenen verileri göremiyorsanız, bir çözümden veya Öngörü'den, *Operations Manager* olay günlüğünden verileri görüntüleyip görüntülemediğinize bağlı olarak, Olay kaynakları Sağlık *Hizmeti* ve Sağlık Hizmeti *Modüllerini* arayın ve aşağıdaki tablodan yazılı olaylar olup olmadığını doğrulamak için **Olay** **Düzeyi** *Uyarı* ve *Hatasına* göre filtre uygulayın.

    |Olay Kimliği |Kaynak |Açıklama |Çözüm |
    |---------|-------|------------|
    |8000 |HealthService |Bu olay, toplanan performans, olay veya diğer veri türüyle ilgili bir iş akışının çalışma alanına aktarılmak üzere hizmete iletilip ilemeyeceğini belirtir. | Kaynak HealthService'ten Olay Kimliği 2136 bu olayla birlikte yazılır ve aracının muhtemelen proxy ve kimlik doğrulama ayarlarının yanlış yapılandırılması, ağ kesintisi veya ağ güvenlik duvarı/proxy'sinin bilgisayardan hizmete TCP trafiğine izin vermemektedir nedeniyle hizmetle iletişim kuramayacağını gösterebilir.| 
    |10102 ve 10103 |Sağlık Hizmeti Modülleri |İş akışı veri kaynağını çözemedi. |Bu, belirtilen performans sayacı veya örneği bilgisayarda yoksa veya çalışma alanı veri ayarlarında yanlış tanımlanmışsa oluşabilir. Bu kullanıcı tarafından belirtilen [bir performans sayacıysa,](data-sources-performance-counters.md#configuring-performance-counters)belirtilen bilgilerin doğru biçimi izleyip hedef bilgisayarlarda var olduğunu doğrulayın. |
    |26002 |Sağlık Hizmeti Modülleri |İş akışı veri kaynağını çözemedi. |Belirtilen Windows olay günlüğü bilgisayarda yoksa bu oluşabilir. Bilgisayarın bu olay günlüğünün kaydedilmesi beklenmiyorsa, bu hata güvenli bir şekilde yoksayılabilir, aksi takdirde bu kullanıcı tarafından belirtilen [olay günlüğüyse,](data-sources-windows-events.md#configuring-windows-event-logs)belirtilen bilgilerin doğru luğunu doğrulayın. |

