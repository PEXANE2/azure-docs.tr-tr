---
title: StorSimple 8000 cihazında sorun gidermek için Tanılama Aracı | Microsoft Docs
description: StorSimple cihaz modlarını açıklar ve cihaz modunu değiştirmek için StorSimple için Windows PowerShell kullanmayı açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: d3e9aff1a38bbabc4f878a4d2e2fb96dafe59c92
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85504466"
---
# <a name="use-the-storsimple-diagnostics-tool-to-troubleshoot-8000-series-device-issues"></a>8000 serisi cihaz sorunlarını gidermek için StorSimple Tanılama aracını kullanın

## <a name="overview"></a>Genel Bakış

StorSimple Tanılama Aracı, StorSimple cihazı için sistem, performans, ağ ve donanım bileşeni sistem durumu ile ilgili sorunları tanılar. Tanılama aracı çeşitli senaryolarda kullanılabilir. Bu senaryolar iş yükü planlama, StorSimple cihazı dağıtma, ağ ortamını değerlendirme ve işlemsel bir cihazın performansını belirleme içerir. Bu makale, tanılama aracına genel bir bakış sağlar ve aracın bir StorSimple aygıtıyla nasıl kullanılabileceğini açıklar.

Tanılama Aracı öncelikle StorSimple 8000 serisi şirket içi cihazlara (8100 ve 8600) yöneliktir.

## <a name="run-diagnostics-tool"></a>Tanılama aracını Çalıştır

Bu araç, StorSimple cihazınızın Windows PowerShell arabirimi aracılığıyla çalıştırılabilir. Cihazınızın yerel arabirimine erişmenin iki yolu vardır:

* [Cihaz seri konsoluna bağlanmak Için PuTTY kullanın](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
* [StorSimple için Windows PowerShell aracılığıyla araca uzaktan erişin](storsimple-8000-remote-connect.md).

Bu makalede, PuTTY aracılığıyla cihaz seri konsoluna bağladığınızı varsayalım.

#### <a name="to-run-the-diagnostics-tool"></a>Tanılama aracını çalıştırmak için

Cihazın Windows PowerShell arabirimine bağlandıktan sonra, cmdlet 'ini çalıştırmak için aşağıdaki adımları gerçekleştirin.
1. [Cihaz seri konsoluna bağlanmak Için PuTTY kullanma](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console)bölümündeki adımları izleyerek cihaz seri konsolunda oturum açın.

2. Aşağıdaki komutu yazın:

    `Invoke-HcsDiagnostics`

    Kapsam parametresi belirtilmemişse, cmdlet tüm tanılama testlerini yürütür. Bu sınamalar sistem, donanım bileşeni sistem durumu, ağ ve performansı içerir. 
    
    Yalnızca belirli bir testi çalıştırmak için kapsam parametresini belirtin. Örneğin, yalnızca ağ testini çalıştırmak için şunu yazın

    `Invoke-HcsDiagnostics -Scope Network`

3. Daha fazla analiz için, PuTTY penceresindeki çıktıyı seçip bir metin dosyasına kopyalayın.

## <a name="scenarios-to-use-the-diagnostics-tool"></a>Tanılama aracını kullanma senaryoları

Sistemin ağ, performans, sistem ve donanım durumunu gidermek için Tanılama aracını kullanın. Olası bazı senaryolar aşağıda verilmiştir:

* **Cihaz çevrimdışı** -storsimple 8000 serisi cihazınız çevrimdışı. Ancak, Windows PowerShell arabiriminden, her iki denetleyicinin de çalışıyor olduğu anlaşılıyor.
    * Bu aracı daha sonra ağ durumunu tespit etmek için kullanabilirsiniz.
         
         > [!NOTE]
         > Bu aracı, kayıt işleminden önce (veya Kurulum Sihirbazı aracılığıyla yapılandırmadan) bir cihazdaki performans ve ağ ayarlarını değerlendirmek için kullanmayın. Kurulum Sihirbazı ve kayıt sırasında cihaza geçerli bir IP atanır. Bu cmdlet 'i, donanım sağlığı ve sistem için kayıtlı olmayan bir cihazda çalıştırabilirsiniz. Kapsam parametresini kullanın, örneğin:
         >
         > `Invoke-HcsDiagnostics -Scope Hardware`
         >
         > `Invoke-HcsDiagnostics -Scope System`

* **Kalıcı cihaz sorunları** -kalıcı olarak görünen cihaz sorunları yaşıyor. Örneğin, kayıt başarısız olur. Ayrıca cihaz başarıyla kaydedildikten ve bir süre boyunca çalışmaya başladıktan sonra cihaz sorunlarıyla karşılaşmanız da gerekebilir.
    * Bu durumda, Microsoft Desteği bir hizmet isteğini günlüğe kaydetmeden önce, ön sorun giderme için bu aracı kullanın. Bu aracı çalıştırmanızı ve bu aracın çıkışını yakalamanızı öneririz. Daha sonra bu çıktıyı sorun gidermeyi hızlandırmak için desteklemek üzere sağlayabilirsiniz.
    * Herhangi bir donanım bileşeni veya küme başarısızlığı varsa, Destek isteği oturum açmanız gerekir.

* **Düşük cihaz performansı** -StorSimple cihazınız yavaş.
    * Bu durumda, bu cmdlet 'i scope parametresi Performance olarak ayarlanmış şekilde çalıştırın. Çıktıyı çözümleyin. Buluta okuma yazma gecikmeleri alırsınız. Bildirilen gecikme sürelerini maksimum ulaşılabilir target olarak kullanın, iç veri işleme için bazı ek yüklerle faktörü yapın ve sonra iş yüklerini sistemde dağıtın. Daha fazla bilgi için, [cihaz performansının sorunlarını gidermek için ağ testini kullanma](#network-test)bölümüne gidin.


## <a name="diagnostics-test-and-sample-outputs"></a>Tanılama testi ve örnek çıktıları

### <a name="hardware-test"></a>Donanım sınaması

Bu test, sisteminizde çalışan donanım bileşenlerinin, USı belleniminin ve disk belleniminin durumunu belirler.

* Bildirilen donanım bileşenleri, test başarısız olan veya sistemde bulunmayan bileşenlerdir.
* USı bellenimi ve disk üretici yazılımı sürümleri, sistem içindeki denetleyici 0, denetleyici 1 ve paylaşılan bileşenler için raporlanır. Donanım bileşenlerinin tamamı listesi için şuraya gidin:

    * [Birincil muhafaza içindeki bileşenler](storsimple-8000-monitor-hardware-status.md#component-list-for-primary-enclosure-of-storsimple-device)
    * [EBOD muhafazadaki bileşenler](storsimple-8000-monitor-hardware-status.md#component-list-for-ebod-enclosure-of-storsimple-device)

> [!NOTE]
> Donanım testi başarısız bileşenleri bildirirse, [Microsoft desteği bir hizmet isteğinde oturum açın](storsimple-8000-contact-microsoft-support.md).

#### <a name="sample-output-of-hardware-test-run-on-an-8100-device"></a>8100 cihazında donanım testi çalıştırmasının örnek çıktısı

StorSimple 8100 cihazından alınan örnek bir çıktı aşağıda verilmiştir. 8100 model cihazında, EBOD Kasası yok. Bu nedenle, EBOD denetleyici bileşenleri bildirilmemiştir.

```
Controller0>Invoke-HcsDiagnostics -Scope Hardware
Running hardware diagnostics ...
--------------------------------------------------
Hardware components failed or not present
----------------------

           Type           State      Controller           Index     EnclosureId
           ----           -----      ----------           -----     -----------
...rVipResource      NotPresent            None               1            None
...rVipResource      NotPresent            None               2            None
...rVipResource      NotPresent            None               3            None
...rVipResource      NotPresent            None               4            None
...rVipResource      NotPresent            None               5            None
...rVipResource      NotPresent            None               6            None
...rVipResource      NotPresent            None               7            None
...rVipResource      NotPresent            None               8            None
...rVipResource      NotPresent            None               9            None
...rVipResource      NotPresent            None              10            None
...rVipResource      NotPresent            None              11            None

Firmware information
----------------------
TalladegaController : ActiveBIOS:0.45.0010
                      BackupBIOS:0.45.0006
                      MainCPLD:17.0.000b
                      ActiveBMCRoot:2.0.001F
                      BackupBMCRoot:2.0.001F
                      BMCBoot:2.0.0002
                      LsiFirmware:20.00.04.00
                      LsiBios:07.37.00.00
                      Battery1Firmware:06.2C
                      Battery2Firmware:06.2C
                      DomFirmware:X231600
                      CanisterFirmware:3.5.0.56
                      CanisterBootloader:5.03
                      CanisterConfigCRC:0x9134777A
                      CanisterVPDStructure:0x06
                      CanisterGEMCPLD:0x19
                      CanisterVPDCRC:0x142F7DC2
                      MidplaneVPDStructure:0x0C
                      MidplaneVPDCRC:0xA6BD4F64
                      MidplaneCPLD:0x10
                      PCM1Firmware:1.00|1.05
                      PCM1VPDStructure:0x05
                      PCM1VPDCRC:0x41BEF99C
                      PCM2Firmware:1.00|1.05
                      PCM2VPDStructure:0x05
                      PCM2VPDCRC:0x41BEF99C

EbodController      :
DisksFirmware       : SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08

TalladegaController : ActiveBIOS:0.45.0010
                      BackupBIOS:0.45.0006
                      MainCPLD:17.0.000b
                      ActiveBMCRoot:2.0.001F
                      BackupBMCRoot:2.0.001F
                      BMCBoot:2.0.0002
                      LsiFirmware:20.00.04.00
                      LsiBios:07.37.00.00
                      Battery1Firmware:06.2C
                      Battery2Firmware:06.2C
                      DomFirmware:X231600
                      CanisterFirmware:3.5.0.56
                      CanisterBootloader:5.03
                      CanisterConfigCRC:0x9134777A
                      CanisterVPDStructure:0x06
                      CanisterGEMCPLD:0x19
                      CanisterVPDCRC:0x142F7DC2
                      MidplaneVPDStructure:0x0C
                      MidplaneVPDCRC:0xA6BD4F64
                      MidplaneCPLD:0x10
                      PCM1Firmware:1.00|1.05
                      PCM1VPDStructure:0x05
                      PCM1VPDCRC:0x41BEF99C
                      PCM2Firmware:1.00|1.05
                      PCM2VPDStructure:0x05
                      PCM2VPDCRC:0x41BEF99C

EbodController      :
DisksFirmware       : SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08

--------------------------------------------------
```

### <a name="system-test"></a>Sistem testi

Bu test, sistem bilgilerini, kullanılabilir güncelleştirmeleri, küme bilgilerini ve cihazınızın hizmet bilgilerini raporlar.

* Sistem bilgileri, modeli, cihaz seri numarasını, saat dilimini, denetleyici durumunu ve sistemde çalışan ayrıntılı yazılım sürümünü içerir. Çıktı olarak bildirilen çeşitli sistem parametrelerini anlamak için [sistem bilgilerini yorumlama](#appendix-interpreting-system-information)bölümüne gidin.

* Güncelleştirme Kullanılabilirliği, düzenli ve bakım modlarının kullanılabilir olup olmadığını ve ilişkili paket adlarını raporlar. `RegularUpdates`Ve ise `MaintenanceModeUpdates` `false` , güncelleştirmelerin kullanılabilir olmadığını gösterir. Cihazınız güncel.
* Küme bilgileri, tüm HCS küme gruplarının ve ilgili durumlarının çeşitli mantıksal bileşenleriyle ilgili bilgileri içerir. Raporun bu bölümünde bir çevrimdışı küme grubu görürseniz, [Microsoft desteği başvurun](storsimple-8000-contact-microsoft-support.md).
* Hizmet bilgileri, cihazınızda çalışan tüm HCS ve CIS hizmetlerinin adlarını ve durumlarını içerir. Bu bilgiler, cihaz sorununu gidermeye yönelik Microsoft Desteği için yararlıdır.

#### <a name="sample-output-of-system-test-run-on-an-8100-device"></a>8100 cihazında sistem testi çalıştırmasının örnek çıktısı

Bir 8100 cihazında sistem testi çalıştırmasının örnek bir çıktısı aşağıda verilmiştir.

```
Controller0>Invoke-HcsDiagnostics -Scope System
Running system diagnostics ...
--------------------------------------------------

System information
----------------------
Controller0:

InstanceId              : 7382407f-a56b-4622-8f3f-756fe04cfd38
Name                    : 8100-SHX0991003G467K
Model                   : 8100
SerialNumber            : SHX0991003G467K
TimeZone                : (UTC-08:00) Pacific Time (US & Canada)
CurrentController       : Controller0
ActiveController        : Controller0
Controller0Status       : Normal
Controller1Status       : Normal
SystemMode              : Normal
FriendlySoftwareVersion : StorSimple 8000 Series Update 4.0
HcsSoftwareVersion      : 6.3.9600.17820
ApiVersion              : 9.0.0.0
VhdVersion              : 6.3.9600.17759
OSVersion               : 6.3.9600.0
CisAgentVersion         : 1.0.9441.0
MdsAgentVersion         : 35.2.2.0
Lsisas2Version          : 2.0.78.0
Capacity                : 219902325555200
RemoteManagementMode    : Disabled
FipsMode                : Enabled

Controller1:
InstanceId              : 7382407f-a56b-4622-8f3f-756fe04cfd38
Name                    : 8100-SHX0991003G467K
Model                   : 8100
SerialNumber            : SHX0991003G467K
TimeZone                :
CurrentController       : Controller0
ActiveController        : Controller0
Controller0Status       : Normal
Controller1Status       : Normal
SystemMode              : Normal
FriendlySoftwareVersion : StorSimple 8000 Series Update 4.0
HcsSoftwareVersion      : 6.3.9600.17820
ApiVersion              : 9.0.0.0
VhdVersion              : 6.3.9600.17759
OSVersion               : 6.3.9600.0
CisAgentVersion         : 1.0.9441.0
MdsAgentVersion         : 35.2.2.0
Lsisas2Version          : 2.0.78.0
Capacity                : 219902325555200
RemoteManagementMode    : HttpsAndHttpEnabled
FipsMode                : Enabled

Update availability
----------------------
RegularUpdates              : False
MaintenanceModeUpdates      : False
RegularUpdatesTitle         : {}
MaintenanceModeUpdatesTitle : {}

Cluster information
----------------------
Name                          State OwnerGroup
----                          ----- ----------
ApplicationHostRLUA           Online HCS Cluster Group
Data0v4                       Online HCS Cluster Group
HCS Vnic Resource             Online HCS Cluster Group
hcs_cloud_connectivity_...    Online HCS Cluster Group
hcs_controller_replacement    Online HCS Cluster Group
hcs_datapath_service          Online HCS Cluster Group
hcs_management_service        Online HCS Cluster Group
hcs_nvram_service             Online HCS Cluster Group
hcs_passive_datapath          Online HCS Passive Cluster Group
hcs_platform_service          Online HCS Cluster Group
hcs_saas_agent_service        Online HCS Cluster Group
HddDataClusterDisk            Online HCS Cluster Group
HddMgmtClusterDisk            Online HCS Cluster Group
HddReplClusterDisk            Online HCS Cluster Group
iSCSI Target Server           Online HCS Cluster Group
NvramClusterDisk              Online HCS Cluster Group
SSAdminRLUA                   Online HCS Cluster Group
SsdDataClusterDisk            Online HCS Cluster Group
SsdNvramClusterDisk           Online HCS Cluster Group

Service information
----------------------
Name                                          Status DisplayName
----                                          ------ -----------
CiSAgentSvc                                   Stopped CiS Service Agent
hcs_cloud_connectivity_...                    Running hcs_cloud_connectivity...
hcs_controller_replacement                    Running HCS Controller Replace...
hcs_datapath_service                          Running HCS Datapath Service
hcs_management_service                        Running HCS Management Service
hcs_minishell                                 Running hcs_minishell
HCS_NVRAM_Service                             Running HCS NVRAM Service
hcs_passive_datapath                          Stopped HCS Passive Datapath S...
hcs_platform_service                          Running HCS Platform Monitor S...
hcs_saas_agent_service                        Running hcs_saas_agent_service
hcs_startup                                   Stopped hcs_startup
--------------------------------------------------
```

### <a name="network-test"></a>Ağ testi

Bu test, ağ arabirimlerinin, bağlantı noktalarının, DNS ve NTP sunucu bağlantısının, TLS/SSL sertifikasının, depolama hesabı kimlik bilgilerinin, Güncelleştirme sunucularına bağlantısının ve StorSimple cihazınızda Web proxy bağlantısının durumunu doğrular.

#### <a name="sample-output-of-network-test-when-only-data0-is-enabled"></a>Yalnızca DATA0 etkin olduğunda ağ testinin örnek çıktısı

8100 cihazının örnek bir çıkışı aşağıda verilmiştir. Çıktıda şu şekilde görebilirsiniz:
* Yalnızca VERI 0 ve VERI 1 ağ arabirimleri etkinleştirilir ve yapılandırılır.
* Portalda VERI 2-5 etkin değildir.
* DNS sunucusu yapılandırması geçerli ve cihaz DNS sunucusu aracılığıyla bağlanabilir.
* NTP sunucusu bağlantısı da iyi bir şekilde yapılır.
* 80 ve 443 bağlantı noktaları açıktır. Ancak bağlantı noktası 9354 engellenir. [Sistem ağ gereksinimlerine](storsimple-system-requirements.md)bağlı olarak, bu bağlantı noktasını Service Bus iletişimi için açmanız gerekir.
* TLS/SSL sertifikası geçerli.
* Cihaz, depolama hesabına bağlanabilir: _myss8000storageacct_.
* Sunucuları güncelleştirme bağlantısı geçerlidir.
* Bu cihazda Web proxy yapılandırılmadı.

#### <a name="sample-output-of-network-test-when-data0-and-data1-are-enabled"></a>DATA0 ve VERI1 etkinken ağ testinin örnek çıktısı

```
Controller0>Invoke-HcsDiagnostics -Scope Network
Running network diagnostics ....
--------------------------------------------------
Validating networks .....
Name                Entity              Result              Details
----                ------              ------              -------
Network interface   Data0               Valid
Network interface   Data1               Valid
Network interface   Data2               Not enabled
Network interface   Data3               Not enabled
Network interface   Data4               Not enabled
Network interface   Data5               Not enabled
DNS                 10.222.118.154      Valid
NTP                 time.windows.com    Valid
Port                80                  Open
Port                443                 Open
Port                9354                Blocked
SSL certificate     https://myss8000... Valid
Storage account ... myss8000storageacct Valid
URL                 http://download.... Valid
URL                 http://download.... Valid
Web proxy                               Not enabled         Web proxy is not...
--------------------------------------------------
```

### <a name="performance-test"></a>Performans testi

Bu test, cihazınız için bulut okuma yazma gecikmeleri aracılığıyla bulut performansını raporlar. Bu araç, StorSimple ile elde edilebilecek bulut performansının temelini oluşturmak için kullanılabilir. Araç, bağlantınız için alabileceğiniz en yüksek performansı (okuma/yazma gecikmeleri için en iyi durum senaryosu) rapor edebilir.

Araç, en yüksek ulaşılabilir performansını raporladığında, iş yüklerini dağıttığınızda bildirilen okuma yazma gecikme sürelerini hedefler olarak kullanabiliriz.

Test, cihazdaki farklı birim türleriyle ilişkili blob boyutlarının benzetimini yapar. Yerel olarak sabitlenmiş birimlerin normal katmanlı ve yedeklemeleri 64 KB 'lik bir BLOB boyutu kullanır. Arşiv seçeneği işaretli katmanlı birimler kullanım 512 KB blob veri boyutu kullanır. Cihazınızda katmanlı ve yerel olarak sabitlenmiş birimler varsa, yalnızca 64 KB blob veri boyutuna karşılık gelen sınama çalıştırılır.

Bu aracı kullanmak için aşağıdaki adımları gerçekleştirin:

1.  İlk olarak, arşivlenmiş ve arşivlenen seçeneği işaretlenmiş katmanlı birimlerin bir karışımını oluşturun. Bu eylem, aracın hem 64 KB hem de 512 KB BLOB boyutu için testleri çalıştırmasını sağlar.

2. Birimleri oluşturup yapılandırdıktan sonra cmdlet 'ini çalıştırın. Şunu yazın:

    `Invoke-HcsDiagnostics -Scope Performance`

3. Araç tarafından bildirilen okuma yazma gecikmelerinin bir kısmını unutmayın. Bu testin, sonuçları raporladığında önce çalışması birkaç dakika sürebilir.

4. Bağlantı gecikmeleri beklenen aralığın altındaysa, araç tarafından bildirilen gecikme süreleri, iş yükleri dağıtıldığında maksimum ulaşılabilir hedef olarak kullanılabilir. İç veri işleme için bazı ek yükün faktörü.

    Tanılama aracı tarafından bildirilen okuma-yazma gecikme süreleri yüksekse:

    1. Blob Hizmetleri için Depolama Analizi yapılandırın ve Azure Storage hesabının gecikme sürelerini anlamak için çıktıyı çözümleyin. Ayrıntılı yönergeler için [depolama Analizi etkinleştirme ve yapılandırma](../storage/common/storage-enable-and-view-metrics.md)' ya gidin. Bu gecikme süreleri de yüksek ve StorSimple tanılama aracından aldığınız sayılarla karşılaştırmalarda, Azure depolama ile bir hizmet isteğini günlüğe yazmanız gerekir.

    2. Depolama hesabı gecikmeleri düşükse, ağınızdaki gecikme sorunlarını araştırmak için ağ yöneticinize başvurun.

#### <a name="sample-output-of-performance-test-run-on-an-8100-device"></a>8100 cihazında performans testi çalıştırmasının örnek çıktısı

```
Controller0>Invoke-HcsDiagnostics -Scope Performance
Running performance diagnostics...
--------------------------------------------------
Cloud performance: writing blobs
Cloud write latency: 194 ms using credential 'myss8000storageacct', blob size '64KB'
Cloud performance: reading blobs..
Cloud read latency: 544 ms using credential 'myss8000storageacct', blob size '64KB'
Cloud performance: writing blobs.
Cloud write latency: 369 ms using credential 'myss8000storageacct', blob size '512KB'
Cloud performance: reading blobs...
Cloud read latency: 4924 ms using credential 'myss8000storageacct', blob size '512KB'
--------------------------------------------------
Controller0>
```

## <a name="appendix-interpreting-system-information"></a>Ek: sistem bilgilerini yorumlama

Aşağıda, sistem bilgileri 'ndeki çeşitli Windows PowerShell parametrelerinin ne şekilde eşlendiğini açıklayan bir tablo verilmiştir. 

| PowerShell parametresi    | Açıklama  |
|-------------------------|------------------|
| Örnek Kimliği             | Her denetleyicinin benzersiz bir tanımlayıcısı veya onunla ilişkili bir GUID 'SI vardır.|
| Adı                    | Cihaz dağıtımı sırasında Azure portal tarafından yapılandırılan cihazın kolay adı. Varsayılan kolay ad, cihaz seri numarasıdır. |
| Model                   | StorSimple 8000 serisi cihazınızın modeli. Model 8100 veya 8600 olabilir.|
| SerialNumber            | Cihaz seri numarası fabrikada atanır ve 15 karakter uzunluğundadır. Örneğin, 8600-SHX0991003G44HT şunu gösterir:<br> 8600 – cihaz modelidir.<br>SHX – üretim sitesidir.<br> 0991003-belirli bir üründür. <br> G44HT-son 5 basamak, benzersiz seri numaraları oluşturmak için artırılır. Bu sıralı bir küme olmayabilir.|
| TimeZone                | Cihaz dağıtımı sırasında Azure portal yapılandırılan cihaz saat dilimi.|
| CurrentController       | StorSimple cihazınızın Windows PowerShell arabirimi aracılığıyla bağlandığınız denetleyici.|
| ActiveController        | Cihazınızda etkin olan ve tüm ağ ve disk işlemlerini denetleyen denetleyici. Bu, Controller 0 veya Controller 1 olabilir.  |
| Controller0Status       | Cihazınızdaki denetleyicinin 0 durumu. Denetleyici durumu normal, kurtarma modunda veya ulaşılamaz olabilir.|
| Controller1Status       | Cihazınızdaki denetleyici 1 durumu.  Denetleyici durumu normal, kurtarma modunda veya ulaşılamaz olabilir.|
| SystemMode              | StorSimple cihazınızın genel durumu. Cihaz durumu normal, bakım veya kullanımdan kaldırılmış olabilir (Azure portal devre dışı bırakılmış olarak karşılık gelir).|
| Daha önce yazılım sürümü | Cihaz yazılımı sürümüne karşılık gelen kolay dize. Güncelleştirme 4 çalıştıran bir sistem için, kolay yazılım sürümü StorSimple 8000 serisi güncelleştirme 4,0 olacaktır.|
| HcsSoftwareVersion      | Cihazınızda çalışan HCS yazılım sürümü. Örneğin, StorSimple 8000 serisi güncelleştirme 4,0 ' ye karşılık gelen HCS yazılım sürümü 6.3.9600.17820 ' dir. |
| ApiVersion              | HCS cihazının Windows PowerShell API 'sinin yazılım sürümü.|
| VhdVersion              | Cihazın birlikte gönderildiği fabrika görüntüsünün yazılım sürümü. Cihazınızı fabrika ayarlarına sıfırlarsanız, bu yazılım sürümünü çalıştırır.|
| OSVersion               | Cihazda çalışan Windows Server işletim sisteminin yazılım sürümü. StorSimple cihazı, 6.3.9600 'e karşılık gelen Windows Server 2012 R2 'yi temel alan bir işlemdir.|
| Cıagentversion         | StorSimple cihazınızda çalışan CIS aracınızın sürümü. Bu aracı Azure 'da çalışan StorSimple Yöneticisi hizmetiyle iletişim kurmaya yardımcı olur.|
| MdsAgentVersion         | StorSimple cihazınızda çalışan MDS aracısına karşılık gelen sürüm. Bu aracı verileri Izleme ve tanılama hizmetine (MDS) taşısın.|
| Lsisas2Version          | StorSimple cihazınızdaki LSI sürücülerine karşılık gelen sürüm.|
| Kapasite                | Cihazın bayt cinsinden toplam kapasitesi.|
| RemoteManagementMode    | Cihazın Windows PowerShell arabirimi aracılığıyla uzaktan yönetilip yönetilmeyeceğini gösterir. |
| FipsMode                | Birleşik Devletler Federal bilgi Işleme standardı (FIPS) modunun cihazınızda etkinleştirilip etkinleştirilmediğini belirtir. FIPS 140 standardı, önemli verilerin korunması için ABD Federal Kamu bilgisayar sistemleri tarafından kullanılmak üzere onaylanan şifreleme algoritmalarını tanımlar. Güncelleştirme 4 veya üstünü çalıştıran cihazlarda FIPS modu varsayılan olarak etkinleştirilmiştir. |

## <a name="next-steps"></a>Sonraki adımlar

* [Invoke-HcsDiagnostics cmdlet 'inin sözdizimini](https://technet.microsoft.com/library/mt795371.aspx)öğrenin.

* StorSimple cihazınızda [dağıtım sorunlarını giderme](storsimple-troubleshoot-deployment.md) hakkında daha fazla bilgi edinin.
