---
title: Azure Site Kurtarma işlem sunucusunu sorun giderme
description: Bu makalede, Azure Site Kurtarma işlemi sunucusuyla ilgili sorunları nasıl giderikarşılayacakaçık
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: troubleshooting
ms.date: 09/09/2019
ms.author: raynew
ms.openlocfilehash: 812cd0293f9627b7438e9870d8985e71dae1d147
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79256867"
---
# <a name="troubleshoot-the-process-server"></a>İşlem sunucusunu sorun giderme

[Site Kurtarma](site-recovery-overview.md) işlemi sunucusu, şirket içi VMware VM'leri ve fiziksel sunucular için Azure'da olağanüstü durum kurtarma ayarladığında kullanılır. Bu makalede, çoğaltma ve bağlantı sorunları da dahil olmak üzere işlem sunucusu ile sorunları gidermek için nasıl açıklanmaktadır.

İşlem sunucusu hakkında [daha fazla bilgi edinin.](vmware-physical-azure-config-process-server-overview.md)

## <a name="before-you-start"></a>Başlamadan önce

Sorun gidermeye başlamadan önce:

1. [İşlem sunucularını](vmware-physical-azure-monitor-process-server.md)nasıl izleyeceğinizi anladığınızdan emin olun.
2. Aşağıdaki en iyi uygulamaları gözden geçirin.
3. Kapasite yle [ilgili hususları](site-recovery-plan-capacity-vmware.md#capacity-considerations)takip ettiğinizden ve [yapılandırma sunucusu](site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-configuration-server-and-inbuilt-process-server) veya bağımsız işlem [sunucuları](site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-process-server)için boyutlandırma kılavuzu kullandığınızdan emin olun.

## <a name="best-practices-for-process-server-deployment"></a>İşlem sunucusu dağıtımı için en iyi uygulamalar

İşlem sunucularının optimum performansı için, bir dizi genel en iyi uygulamayı özetledik.

**En iyi uygulama** | **Şey**
--- |---
**Kullanım** | Yapılandırma sunucusu/bağımsız işlem sunucusunun yalnızca amaç lanan amaç için kullanıldığından emin olun. Makinede başka bir şey çalıştırma.
**IP adresi** | İşlem sunucusunun statik bir IPv4 adresi olduğundan ve NAT yapılandırılmamışolduğundan emin olun.
**Bellek/CPU kullanımını denetleme** |CPU ve bellek kullanımını %70'in altında tutun.
**Boş alan sağlayın** | Boş alan, işlem sunucusundaki önbellek disk alanı anlamına gelir. Çoğaltma verileri Azure'a yüklenmeden önce önbellekte depolanır.<br/><br/> Boş alanı %25'in üzerinde tutun. %20'nin altına düşerse, işlem sunucusuyla ilişkili çoğaltılmış makineler için çoğaltma azaltılır.

## <a name="check-process-server-health"></a>İşlem sunucusu durumunu denetleyin

Sorun gidermedeki ilk adım, işlem sunucusunun durumunu ve durumunu denetlemektir. Bunu yapmak için tüm uyarıları gözden geçirin, gerekli hizmetlerin çalıştığını kontrol edin ve işlem sunucusundan bir sinyal olduğunu doğrulayın. Bu adımlar aşağıdaki grafikte özetlenir ve ardından adımları gerçekleştirmenize yardımcı olacak yordamlar izlenir.

![Sorun giderme işlemi sunucu sağlığı](./media/vmware-physical-azure-troubleshoot-process-server/troubleshoot-process-server-health.png)

## <a name="step-1-troubleshoot-process-server-health-alerts"></a>Adım 1: Sorun giderme işlemi sunucusu sistem durumu uyarıları

İşlem sunucusu bir dizi sistem durumu uyarısı oluşturur. Bu uyarılar ve önerilen eylemler aşağıdaki tabloda özetlenmiştir.

**Uyarı türü** | **Hata** | **Sorun giderme**
--- | --- | --- 
![Sağlam][green] | None  | İşlem sunucusu bağlı ve sağlıklı.
![Uyarı][yellow] | Belirtilen hizmetler çalışmıyor. | 1. Hizmetlerin çalıştığını kontrol edin.<br/> 2. Hizmetler beklendiği gibi çalışıyorsa, [bağlantı ve çoğaltma sorunlarını gidermek](#check-connectivity-and-replication)için aşağıdaki yönergeleri izleyin.
![Uyarı][yellow]  | CPU kullanımı son 15 dakika için %80 >. | 1. Yeni makineler eklemeyin.<br/>2. İşlem sunucusunu kullanan VM sayısının [tanımlanan sınırlara](site-recovery-plan-capacity-vmware.md#capacity-considerations)hizalanıp hizalanmadığını kontrol edin ve ek bir [işlem sunucusu](vmware-azure-set-up-process-server-scale.md)ayarlamayı düşünün.<br/>3. Bağlantı ve [çoğaltma sorunlarını gidermek](#check-connectivity-and-replication)için aşağıdaki yönergeleri izleyin.
![Kritik][red] |  CPU kullanımı son 15 dakika için %95 >. | 1. Yeni makineler eklemeyin.<br/>2. İşlem sunucusunu kullanan VM sayısının [tanımlanan sınırlara](site-recovery-plan-capacity-vmware.md#capacity-considerations)hizalanıp hizalanmadığını kontrol edin ve ek bir [işlem sunucusu](vmware-azure-set-up-process-server-scale.md)ayarlamayı düşünün.<br/>3. Bağlantı ve [çoğaltma sorunlarını gidermek](#check-connectivity-and-replication)için aşağıdaki yönergeleri izleyin.<br/> 4. Sorun devam ederse, VMware/fiziksel sunucu çoğaltma için [Dağıtım Planlayıcısını](https://aka.ms/asr-v2a-deployment-planner) çalıştırın.
![Uyarı][yellow] | Bellek kullanımı son 15 dakika için %80 >. |  1. Yeni makineler eklemeyin.<br/>2. İşlem sunucusunu kullanan VM sayısının [tanımlanan sınırlara](site-recovery-plan-capacity-vmware.md#capacity-considerations)hizalanıp hizalanmadığını kontrol edin ve ek bir [işlem sunucusu](vmware-azure-set-up-process-server-scale.md)ayarlamayı düşünün.<br/>3. Uyarıile ilgili talimatları uygulayın.<br/> 4. Sorun devam ederse, [bağlantı ve çoğaltma sorunlarını gidermek](#check-connectivity-and-replication)için aşağıdaki yönergeleri izleyin.
![Kritik][red] | Bellek kullanımı son 15 dakika için %95 >. | 1. Yeni makineler eklemeyin ve ek bir [işlem sunucusu](vmware-azure-set-up-process-server-scale.md)kurmayı düşünün.<br/> 2. Uyarı ile ilgili talimatları uygulayın.<br/> 3. 4. Sorun devam ederse, [bağlantı ve çoğaltma sorunlarını gidermek](#check-connectivity-and-replication)için aşağıdaki yönergeleri izleyin.<br/> 4. Sorun devam ederse, VMware/fiziksel sunucu çoğaltma sorunları için [Dağıtım Planlayıcısını](https://aka.ms/asr-v2a-deployment-planner) çalıştırın.
![Uyarı][yellow] | Önbellek klasörü boş alan son 15 dakika için% 30 <. | 1. Yeni makineler eklemeyin ve ek bir [işlem sunucusu](vmware-azure-set-up-process-server-scale.md)kurmayı düşünün.<br/>2. İşlem sunucusunu kullanan VM sayısının [yönergelere](site-recovery-plan-capacity-vmware.md#capacity-considerations)uygun olup olmadığını kontrol edin.<br/> 3. Bağlantı ve [çoğaltma sorunlarını gidermek](#check-connectivity-and-replication)için aşağıdaki yönergeleri izleyin.
![Kritik][red] |  Boş alan son 15 dakika için %25 < | 1. Bu sorun için uyarı ile ilişkili yönergeleri izleyin.<br/> 2. 3. [Bağlantı ve çoğaltma sorunlarını gidermek](#check-connectivity-and-replication)için aşağıdaki yönergeleri izleyin.<br/> 3. Sorun devam ederse, VMware/fiziksel sunucu çoğaltma için [Dağıtım Planlayıcısını](https://aka.ms/asr-v2a-deployment-planner) çalıştırın.
![Kritik][red] | İşlem sunucusundan 15 dakika veya daha fazla sinyal yok. Tmansvs hizmeti yapılandırma sunucusuyla iletişim kurmuyor. | 1) İşlem sunucusunun çalışır durumda olup olmadığını kontrol edin.<br/> 2. Tmassvc'nin işlem sunucusunda çalıştığını kontrol edin.<br/> 3. Bağlantı ve [çoğaltma sorunlarını gidermek](#check-connectivity-and-replication)için aşağıdaki yönergeleri izleyin.


![Tablo tuşu](./media/vmware-physical-azure-troubleshoot-process-server/table-key.png)


## <a name="step-2-check-process-server-services"></a>Adım 2: İşlem sunucusu hizmetlerini denetleyin

İşlem sunucusunda çalıştırılması gereken hizmetler aşağıdaki tabloda özetlenmiştir. İşlem sunucusunun nasıl dağıtıldıklarına bağlı olarak hizmetlerde küçük farklılıklar vardır. 

Microsoft Azure Kurtarma Hizmetleri Aracısı (obengine) dışındaki tüm hizmetlerde, StartType'ın **Otomatik** veya **Otomatik (Gecikmeli Başlangıç)** olarak ayarlı olup olmadığını denetleyin.
 
**Dağıtım** | **Hizmetleri çalıştırma**
--- | ---
**Yapılandırma sunucusundaki işlem sunucusu** | ProcessServer; ProcessServerMonitor; cxprocessserver; InMage PushInstall; Günlük Yükleme Hizmeti (LogUpload); InMage Scout Uygulama Hizmeti; Microsoft Azure Kurtarma Hizmetleri Aracısı (obengine); InMage Scout VX Agent-Sentinel/Outpost (svagents); tmansvc; World Wide Web Yayıncılık Hizmeti (W3SVC); MySQL; Microsoft Azure Site Kurtarma Hizmeti (dra)
**Bağımsız sunucu olarak çalışan sunucuişlemini** | ProcessServer; ProcessServerMonitor; cxprocessserver; InMage PushInstall; Günlük Yükleme Hizmeti (LogUpload); InMage Scout Uygulama Hizmeti; Microsoft Azure Kurtarma Hizmetleri Aracısı (obengine); InMage Scout VX Agent-Sentinel/Outpost (svagents); tmansvc.
**Hata geri ödeme için Azure'da dağıtılan işlem sunucusu** | ProcessServer; ProcessServerMonitor; cxprocessserver; InMage PushInstall; Günlük Yükleme Hizmeti (LogUpload)


## <a name="step-3-check-the-process-server-heartbeat"></a>Adım 3: İşlem sunucusu sinyalatışını denetleyin

İşlem sunucusundan sinyal (hata kodu 806) sinyal yoksa, aşağıdakileri yapın:

1. İşlem sunucusu VM'nin çalışır durumda olduğunu doğrulayın.
2. Hatalar için bu günlükleri denetleyin.

    C:\ProgramData\ASR\home\svsystems\eventmanager *.log C\ProgramData\ASR\home\svsystems\monitor_protection*.log

## <a name="check-connectivity-and-replication"></a>Bağlantı ve çoğaltmayı denetleyin

 İlk ve devam eden çoğaltma hataları genellikle kaynak makineler ve işlem sunucusu arasındaki veya işlem sunucusu ile Azure arasındaki bağlantı sorunlarından kaynaklanır. Bu adımlar aşağıdaki grafikte özetlenir ve ardından adımları gerçekleştirmenize yardımcı olacak yordamlar izlenir.

![Sorun giderme bağlantısı ve çoğaltma](./media/vmware-physical-azure-troubleshoot-process-server/troubleshoot-connectivity-replication.png)


## <a name="step-4-verify-time-sync-on-source-machine"></a>Adım 4: Kaynak makinede zaman eşitleme doğrulayın

Çoğaltılan makinenin sistem tarih/saatinin eşit olduğundan emin olun. [Daha fazla bilgi edinin](https://docs.microsoft.com/windows-server/networking/windows-time-service/accurate-time)

## <a name="step-5-check-anti-virus-software-on-source-machine"></a>Adım 5: Kaynak makinede anti-virüs yazılımı kontrol edin

Çoğaltılan makinede hiçbir anti-virüs yazılımı Site Kurtarma engelliyor olup olmadığını kontrol edin. Site Kurtarma'yı virüsten koruma programlarından hariç tutmanız gerekiyorsa, [bu makaleyi](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program)inceleyin.

## <a name="step-6-check-connectivity-from-source-machine"></a>Adım 6: Kaynak makineden bağlantı kontrol edin


1. Gerekirse [Telnet istemcisini](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx) kaynak makineye töyorum. Ping kullanma.
2. Kaynak makineden, Telnet ile HTTPS bağlantı noktasındaki işlem sunucusunu pingleyin. Varsayılan olarak 9443 çoğaltma trafiği için HTTPS bağlantı noktasıdır.

    `telnet <process server IP address> <port>`

3. Bağlantının başarılı olup olmadığını doğrulayın.


**Bağlantı** | **Şey** | **Eylem**
--- | --- | ---
**Başarılı** | Telnet boş bir ekran gösterir ve işlem sunucusuna ulaşılabilir. | Başka bir işlem gerekli değildir.
**Başarısız** | Bağlanamam | Gelen bağlantı noktası 9443'ün işlem sunucusunda izin aldığından emin olun. Örneğin, çevre ağınız veya ekranlı bir alt ağınız varsa. Bağlantıyı tekrar kontrol edin.
**Kısmen başarılı** | Bağlanabilirsiniz, ancak kaynak makine işlem sunucusuna ulaşılamadığını bildirir. | Sonraki sorun giderme yordamı ile devam edin.

## <a name="step-7-troubleshoot-an-unreachable-process-server"></a>Adım 7: Erişilemenebilen bir işlem sunucusunun sorun giderme

İşlem sunucusuna kaynak makineden erişilemiyorsa, hata 78186 görüntülenir. Bu sorun giderilemezse, hem uygulama tutarlılığı hem de kilitlenme tutarlıkurtarma noktalarının beklendiği gibi oluşturulmamalarına neden olur.

Kaynak makinenin işlem sunucusunun IP adresine erişip ulaşamayacağını kontrol ederek sorun giderme ve uçtan uca bağlantıyı denetlemek için kaynak makinedeki cxpsclient aracını çalıştırın.


### <a name="check-the-ip-connection-on-the-process-server"></a>İşlem sunucusundaki IP bağlantısını denetleme

Telnet başarılı ysa ancak kaynak makine işlem sunucusuna ulaşılamayacağını bildiriyorsa, işlem sunucusunun IP adresine ulaşıp ulaşamayacağınızı denetleyin.

1. Bir web tarayıcısında, IP adresine>:<PS_Data_Port>/> PS_IP <.
2. Bu denetim bir HTTPS sertifika hatası gösteriyorsa, bu normaldir. Hatayı yoksanız, 400 - Kötü İstek görmeniz gerekir. Bu, sunucunun tarayıcı isteğine hizmet edilemeyebileceği ve standart HTTPS bağlantısının iyi olduğu anlamına gelir.
3. Bu denetim işe yaramazsa, tarayıcı hata iletisini not edin. Örneğin, bir 407 hatası proxy kimlik doğrulamaile ilgili bir sorunu gösterir.

### <a name="check-the-connection-with-cxpsclient"></a>cxpsclient ile bağlantıyı kontrol edin

Ayrıca, uçlardan uca bağlantıyı denetlemek için cxpsclient aracını çalıştırabilirsiniz.

1. Aracı aşağıdaki gibi çalıştırın:

    ```
    <install folder>\cxpsclient.exe -i <PS_IP> -l <PS_Data_Port> -y <timeout_in_secs:recommended 300>
    ```

2. İşlem sunucusunda, bu klasörlerde oluşturulan günlükleri denetleyin:

    C:\ProgramData\ASR\home\svsystems\transport\log\cxps.err C:\ProgramData\ASR\home\svsystems\transport\log\cxps.xfer



### <a name="check-source-vm-logs-for-upload-failures-error-78028"></a>Yükleme hataları için kaynak VM günlüklerini denetleyin (hata 78028)

Kaynak makinelerden işlem hizmetine engellenen veri yüklemeleriyle ilgili sorun, hem kilitlenme tutarlısı hem de uygulama tutarlıkurtarma noktalarının oluşturulmamasına neden olabilir. 

1. Ağ yükleme hatalarını gidermek için bu günlükte hataları alabilirsiniz:

    C:\Program Dosyaları (x86)\Microsoft Azure Site Kurtarma\aracı\svagents*.log 

2. Bu makaledeki yordamların geri kalanını kullanmak, veri yükleme sorunlarını çözmenize yardımcı olabilir.



## <a name="step-8-check-whether-the-process-server-is-pushing-data"></a>Adım 8: İşlem sunucusunun verileri itip itmediğini denetleme

İşlem sunucusunun verileri Azure'a etkin bir şekilde taşıyıp itmediğini denetleyin.

  1. İşlem sunucusunda Görev Yöneticisi'ni açın (Ctrl+Shift+Esc tuşuna basın).
  2. Kaynak İzleyicisi'ni **aç>** **Performans** sekmesini seçin.
  3. **Kaynak İzle** sayfasında **Ağ** sekmesini seçin. **Ağ Etkinliği ile İşlemler**altında, cbengine.exe'nin büyük miktarda veri gönderip göndermediğini kontrol edin.

       ![Ağ etkinliği olan işlemler kapsamındaki birimler](./media/vmware-physical-azure-troubleshoot-process-server/cbengine.png)

  cbengine.exe büyük miktarda veri göndermiyorsa, aşağıdaki bölümlerdeki adımları tamamlayın.

## <a name="step-9-check-the-process-server-connection-to-azure-blob-storage"></a>Adım 9: Azure blob depolama işleminin sunucusu bağlantısını kontrol edin

1. Kaynak İzle'de **cbengine.exe'yi**seçin.
2. **TCP Connections**altında, işlem sunucusundan Azure depolamasına bağlantı olup olmadığını denetleyin.

  ![cbengine.exe ve Azure Blob depolama URL'si arasındaki bağlantı](./media/vmware-physical-azure-troubleshoot-process-server/rmonitor.png)

### <a name="check-services"></a>Hizmetleri kontrol edin

İşlem sunucusundan Azure blob depolama URL'sine bağlantı yoksa, hizmetlerin çalıştığını kontrol edin.

1. Denetim Masası'nda **Hizmetler'i**seçin.
2. Aşağıdaki hizmetlerin çalıştığını doğrulayın:

    - cxprocessserver
    - InMage Scout VX Ajan - Sentinel / Karakol
    - Microsoft Azure Kurtarma Hizmetleri Aracısı
    - Microsoft Azure Site Kurtarma Hizmeti
    - tmansvc

3. Çalışmayan herhangi bir hizmeti başlatın veya yeniden başlatın.
4. İşlem sunucusunun bağlı ve ulaşılabilir olduğunu doğrulayın. 

## <a name="step-10-check-the-process-server-connection-to-azure-public-ip-address"></a>Adım 10: Azure genel IP adresine işlem sunucusu bağlantısını kontrol edin

1. İşlem sunucusunda, **%programfiles%\Microsoft Azure Recovery Services Agent\Temp'ta**en son CBEngineCurr.errlog dosyasını açın.
2. Dosyada, **443**için arama veya dize **bağlantı girişimi başarısız oldu.**

  ![Temp klasöründe hata günlükleri](./media/vmware-physical-azure-troubleshoot-process-server/logdetails1.png)

3. Sorunlar görürseniz, CBEngineCurr.currLog dosyasında 443 no'lu bağlantı noktasını kullanarak Azure genel IP adresinizi konumlandı:

  `telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443`

5. İşlem sunucusundaki komut satırında, Azure genel IP adresinizi pinglemek için Telnet'i kullanın.
6. Bağlanamıyorsanız, bir sonraki yordamı uygulayın.

## <a name="step-11-check-process-server-firewall-settings"></a>Adım 11: İşlem sunucusu güvenlik duvarı ayarlarını denetleyin. 

İşlem sunucusundaki IP adresi tabanlı güvenlik duvarının erişimi engelleyip engellemediğini denetleyin.

1. IP adresi tabanlı güvenlik duvarı kuralları için:

    a) Microsoft Azure [veri merkezi IP aralıklarının](https://www.microsoft.com/download/details.aspx?id=41653)tam listesini indirin.

    b) Güvenlik duvarının Azure'a (ve varsayılan HTTPS bağlantı noktası, 443) iletişime izin verdiğinden emin olmak için IP adresi aralıklarını güvenlik duvarı yapılandırmanıza ekleyin.

    c) Aboneliğinizin Azure bölgesi ve Azure Batı ABD bölgesi (erişim denetimi ve kimlik yönetimi için kullanılır) için IP adres aralıklarına izin verin.

2. URL tabanlı güvenlik duvarları için, aşağıdaki tabloda listelenen URL'leri güvenlik duvarı yapılandırmasına ekleyin.

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  


## <a name="step-12-verify-process-server-proxy-settings"></a>Adım 12: İşlem sunucusu proxy ayarlarını doğrulayın 

1. Proxy sunucusu kullanıyorsanız, proxy sunucu adının DNS sunucusu tarafından çözüldüğünden emin olun. Kayıt defteri anahtarıHKEY_LOCAL_MACHINE yapılandırma sunucusunu ayarlarken sağladığınız değeri **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Kurtarma\Proxy Ayarları'nı**denetleyin.
2. Aynı ayarların veri göndermek için Azure Site Kurtarma aracısı tarafından kullanıldığından emin olun.

    a) **Microsoft Azure Yedekleme'yi**arayın.

    b) **Microsoft Azure Yedekleme'yi**açın ve **Eylem** > **Değişikliği Özellikleri'ni**seçin.

    c) **Proxy Yapılandırma** sekmesinde, proxy adresi kayıt defteri ayarlarında gösterilen proxy adresiyle aynı olmalıdır. Değilse, aynı adrese değiştirin.

## <a name="step-13-check-bandwidth"></a>Adım 13: Bant genişliğini kontrol edin

İşlem sunucusu ile Azure arasındaki bant genişliğini artırın ve sorunun hala oluşup oluşmadığını denetleyin.


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla yardıma ihtiyacınız varsa, sorunuz Azure [Site Kurtarma forumunda](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)yayınlayın. 

[green]: ./media/vmware-physical-azure-troubleshoot-process-server/green.png
[yellow]: ./media/vmware-physical-azure-troubleshoot-process-server/yellow.png
[red]: ./media/vmware-physical-azure-troubleshoot-process-server/red.png
