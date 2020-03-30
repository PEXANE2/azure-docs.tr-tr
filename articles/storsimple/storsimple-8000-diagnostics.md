---
title: StorSimple 8000 cihazının sorun giderimi için Tanılama Aracı | Microsoft Dokümanlar
description: StorSimple aygıt modlarını açıklar ve aygıt modunu değiştirmek için StorSimple için Windows PowerShell'in nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: 48bd909eefbaea15cf6ca2427e106ad9bc0ffbb4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80298754"
---
# <a name="use-the-storsimple-diagnostics-tool-to-troubleshoot-8000-series-device-issues"></a>8000 serisi cihaz sorunlarını gidermek için StorSimple Diagnostics Aracı'nı kullanın

## <a name="overview"></a>Genel Bakış

StorSimple Diagnostics aracı, StorSimple aygıtının sistem, performans, ağ ve donanım bileşen iması ile ilgili sorunları tanılar. Tanılama aracı çeşitli senaryolarda kullanılabilir. Bu senaryolar arasında iş yükü planlaması, StorSimple aygıtının dağıtılması, ağ ortamının değerlendirilmesi ve bir operasyonel aygıtın performansının belirlenmesi yer almaktadır. Bu makalede, tanılama aracına genel bir bakış sağlar ve aracın bir StorSimple aygıtıyla nasıl kullanılabileceğini açıklar.

Tanılama aracı öncelikle StorSimple 8000 serisi şirket içi cihazlar (8100 ve 8600) için tasarlanmıştır.

## <a name="run-diagnostics-tool"></a>Tanılama aracını çalıştırın

Bu araç, StorSimple cihazınızın Windows PowerShell arabirimi üzerinden çalıştırılabilir. Cihazınızın yerel arabirimine erişmenin iki yolu vardır:

* [Cihaz seri konsoluna bağlanmak için PuTTY'yi kullanın.](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console)
* [StorSimple için Windows PowerShell üzerinden araca uzaktan erişin.](storsimple-8000-remote-connect.md)

Bu makalede, putty üzerinden cihaz seri konsoluna bağlı olduğunuzu varsayıyoruz.

#### <a name="to-run-the-diagnostics-tool"></a>Tanılama aracını çalıştırmak için

Aygıtın Windows PowerShell arabirimine bağlandıktan sonra cmdlet'i çalıştırmak için aşağıdaki adımları gerçekleştirin.
1. Cihaz seri konsoluna bağlanmak için [PuTTY'yi kullan](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console)adımlarını izleyerek aygıt seri konsoluna oturum açın.

2. Aşağıdaki komutu yazın:

    `Invoke-HcsDiagnostics`

    Kapsam parametresi belirtilmemişse, cmdlet tüm tanılama testlerini yürütür. Bu testler sistem, donanım bileşeni sistem, ağ ve performans içerir. 
    
    Yalnızca belirli bir testi çalıştırmak için kapsam parametresini belirtin. Örneğin, yalnızca ağ testini çalıştırmak için,

    `Invoke-HcsDiagnostics -Scope Network`

3. PuTTY penceresinden çıktıyı seçin ve daha fazla analiz için bir metin dosyasına kopyalayın.

## <a name="scenarios-to-use-the-diagnostics-tool"></a>Tanılama aracını kullanmak için senaryolar

Sistemin ağ, performans, sistem ve donanım sistem durumunu gidermek için tanılama aracını kullanın. Bazı olası senaryolar şunlardır:

* **Aygıt çevrimdışı** - StorSimple 8000 serisi cihazınız çevrimdışı. Ancak, Windows PowerShell arabiriminden, her iki denetleyicide de çalışır durumda gibi görünüyor.
    * Daha sonra ağ durumunu belirlemek için bu aracı kullanabilirsiniz.
         
         > [!NOTE]
         > Bu aracı, kayıt (veya kurulum sihirbazı aracılığıyla yapılandırma) önce bir aygıttaki performansı ve ağ ayarlarını değerlendirmek için kullanmayın. Kurulum sihirbazı ve kayıt sırasında cihaza geçerli bir IP atanır. Bu cmdlet'i, kayıtlı olmayan bir aygıtta, donanım durumu ve sistem için çalıştırabilirsiniz. Kapsam parametresini kullanın, örneğin:
         >
         > `Invoke-HcsDiagnostics -Scope Hardware`
         >
         > `Invoke-HcsDiagnostics -Scope System`

* **Kalıcı aygıt sorunları** - Devam ediyor gibi görünen aygıt sorunları yaşıyorsunuz. Örneğin, kayıt başarısız oluyor. Ayrıca, aygıt bir süre başarıyla kaydedildikten ve çalışır durumda olduktan sonra aygıt sorunları yla da karşılaşabilirsiniz.
    * Bu durumda, Microsoft Destek ile bir hizmet isteği günlüğe kaydetmeden önce ön sorun giderme için bu aracı kullanın. Bu aracı çalıştırmanızı ve bu aracın çıktısını yakalamanızı öneririz. Daha sonra sorun giderme hızlandırmak için Destek için bu çıktı sağlayabilir.
    * Herhangi bir donanım bileşeni veya küme hatası varsa, bir Destek isteği oturum açmanız gerekir.

* **Düşük cihaz performansı** - StorSimple cihazınız yavaştır.
    * Bu durumda, bu cmdlet'i performansa ayarlı kapsam parametresi ile çalıştırın. Çıktıyı analiz et. Bulut okuma-yazma gecikmelerini alırsın. Bildirilen geç kalmasürelerini en yüksek ulaşılabilir hedef olarak kullanın, dahili veri işleme için bazı ek yükleri hesaba katın ve ardından sistemdeki iş yüklerini dağıtın. Daha fazla bilgi için aygıt [performansını gidermek için ağ testini kullanın'](#network-test)a gidin.


## <a name="diagnostics-test-and-sample-outputs"></a>Tanılama testi ve numune çıktıları

### <a name="hardware-test"></a>Donanım testi

Bu sınama donanım bileşenlerinin durumunu, USM firmware ve disk firmware sisteminizde çalışan belirler.

* Bildirilen donanım bileşenleri, testte başarısız olan veya sistemde bulunmayan bileşenlerdir.
* USM firmware ve disk firmware sürümleri, Sisteminizdeki Denetleyici 0, Denetleyici 1 ve paylaşılan bileşenler için raporlanır. Donanım bileşenlerinin tam listesi için şu na gidin:

    * [Birincil kasadaki bileşenler](storsimple-8000-monitor-hardware-status.md#component-list-for-primary-enclosure-of-storsimple-device)
    * [EBOD kasasındaki bileşenler](storsimple-8000-monitor-hardware-status.md#component-list-for-ebod-enclosure-of-storsimple-device)

> [!NOTE]
> Donanım testi bileşenleri ni rapor ederse, [Microsoft Desteği ile bir hizmet isteğinde oturum açın.](storsimple-8000-contact-microsoft-support.md)

#### <a name="sample-output-of-hardware-test-run-on-an-8100-device"></a>8100 aygıtında çalıştırılatan donanım testinin örnek çıktısı

Burada bir StorSimple 8100 cihazından bir örnek çıktı. 8100 model cihazda EBOD muhafazası mevcut değildir. Bu nedenle, EBOD denetleyici bileşenleri rapor edilmez.

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

Bu test, sistem bilgilerini, kullanılabilir güncelleştirmeleri, küme bilgilerini ve cihazınızın servis bilgilerini raporlar.

* Sistem bilgileri modeli, aygıt seri numarasını, saat dilimini, denetleyici durumunu ve sistemde çalışan ayrıntılı yazılım sürümünü içerir. Çıktı olarak bildirilen çeşitli sistem parametrelerini anlamak [için, yorumlama sistemi bilgilerine](#appendix-interpreting-system-information)gidin.

* Güncelleştirme kullanılabilirliği, normal ve bakım modlarının kullanılabilir olup olmadığını ve ilişkili paket adlarını bildirir. Varsa `RegularUpdates` `MaintenanceModeUpdates` ve `false`varsa, bu güncelleştirmelerin kullanılmadığını gösterir. Cihazınız güncel.
* Küme bilgileri, tüm HCS küme gruplarının çeşitli mantıksal bileşenleri ve ilgili durumları hakkındaki bilgileri içerir. Raporun bu bölümünde çevrimdışı küme grubu görürseniz, [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)
* Servis bilgileri, cihazınızda çalışan tüm HCS ve CiS hizmetlerinin adlarını ve durumlarını içerir. Bu bilgiler, aygıt sorununu gidermede Microsoft Desteği için yararlıdır.

#### <a name="sample-output-of-system-test-run-on-an-8100-device"></a>8100 aygıtında çalışan sistem testinin örnek çıktısı

Burada bir 8100 cihaz üzerinde çalışan sistem testi bir örnek çıktı.

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

Bu test, ağ arabirimlerinin, bağlantı noktalarının, DNS ve NTP sunucu bağlantısının, TLS/SSL sertifikasının, depolama hesabı kimlik bilgilerinin, Update sunucularına bağlantının ve StorSimple aygıtınızdaki web proxy bağlantısının durumunu doğrular.

#### <a name="sample-output-of-network-test-when-only-data0-is-enabled"></a>Yalnızca DATA0 etkinleştirildiğinde ağ testinin örnek çıktısı

İşte 8100 cihazının örnek çıktısı. Çıktıda şunları görebilirsiniz:
* Yalnızca DATA 0 ve DATA 1 ağ arabirimleri etkinleştirilmiş ve yapılandırılır.
* DATA 2 - 5 portalda etkin değildir.
* DNS sunucu yapılandırması geçerlidir ve aygıt DNS sunucusu üzerinden bağlanabilir.
* NTP sunucu bağlantısı da iyidir.
* 80 ve 443 bağlantı noktaları açık. Ancak, bağlantı noktası 9354 engellendi. [Sistem ağı gereksinimlerine](storsimple-system-requirements.md)bağlı olarak, bu bağlantı noktasını servis veri aracı iletişimi için açmanız gerekir.
* TLS/SSL sertifikası geçerlidir.
* Cihaz depolama hesabına bağlanabilir: _myss8000storageacct_.
* Güncelleştirme sunucularına bağlantı geçerlidir.
* Web proxy bu aygıtta yapılandırılmamıştır.

#### <a name="sample-output-of-network-test-when-data0-and-data1-are-enabled"></a>DATA0 ve DATA1 etkinleştirildiğinde ağ testinin örnek çıktısı

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

Bu test, bulut performansını cihazınızın bulut okuma yazma gecikmeleri aracılığıyla bildirir. Bu araç, StorSimple ile elde edebileceğiniz bulut performansının temelini oluşturmak için kullanılabilir. Araç, bağlantınız için alabileceğiniz maksimum performansı (okuma yazma gecikmeleri için en iyi durum senaryosu) bildirir.

Araç en yüksek ulaşılabilir performansı raporettikçe, iş yüklerini dağıtırken bildirilen okuma yazma gecikmelerini hedef olarak kullanabiliriz.

Test, aygıttaki farklı ses düzeyi türleri ile ilişkili blob boyutlarını simüle eder. Yerel olarak sabitlenmiş birimlerin düzenli katmanlı ve yedekleri 64 KB blob boyutu kullanır. Arşiv seçeneği işaretli katmanlı hacimler 512 KB blob veri boyutu kullanın. Aygıtınız katmanlı ve yerel olarak sabitlenmiş birimleri yapılandırmışsa, yalnızca 64 KB blob veri boyutuna karşılık gelen test çalıştırılır.

Bu aracı kullanmak için aşağıdaki adımları gerçekleştirin:

1.  İlk olarak, arşivlenmiş seçeneği işaretli katmanlı hacimler ve katmanlı birimlerin bir karışımını oluşturun. Bu eylem, aracın testleri hem 64 KB hem de 512 KB blob boyutları için çalıştırmasını sağlar.

2. Birimleri oluşturduktan ve yapılandırıldıktan sonra cmdlet'i çalıştırın. Şunu yazın:

    `Invoke-HcsDiagnostics -Scope Performance`

3. Araç tarafından bildirilen okuma-yazma gecikmelerini not edin. Bu sınama sonuçları bildirmeden önce çalışması birkaç dakika sürebilir.

4. Bağlantı gecikmelerinin tümü beklenen aralığın altındaysa, araç tarafından bildirilen gecikmeler iş yüklerini dağıtırken ulaşılabilecek maksimum hedef olarak kullanılabilir. Dahili veri işleme için bazı ek yükü faktör.

    Tanılama aracı tarafından bildirilen okuma-yazma gecikmeleri yüksekse:

    1. Azure depolama hesabının gecikmelerini anlamak için depolama analizini blob hizmetleri için yapılandırın ve çıktıyı analiz edin. Ayrıntılı talimatlar için [Depolama Analizini etkinleştirmeye ve yapılandırmaya](../storage/common/storage-enable-and-view-metrics.md)gidin. Bu gecikmeler de yüksekse ve StorSimple Diagnostics aracından aldığınız numaralarla karşılaştırılabilirse, bir hizmet isteğini Azure depolama alanıyla günlüğe kaydetmeniz gerekir.

    2. Depolama hesabı gecikmeleri düşükse, ağınızdaki gecikme sorunlarını araştırmak için ağ yöneticinize başvurun.

#### <a name="sample-output-of-performance-test-run-on-an-8100-device"></a>8100 aygıtında çalışan performans testinin örnek çıktısı

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

## <a name="appendix-interpreting-system-information"></a>Ek: sistem bilgilerinin yorumlanması

Burada, sistem bilgi haritasındaki çeşitli Windows PowerShell parametrelerinin ne için olduğunu açıklayan bir tablo verilmiştir. 

| PowerShell Parametresi    | Açıklama  |
|-------------------------|------------------|
| Örnek Kimliği             | Her denetleyicinin benzersiz bir tanımlayıcısı veya onunla ilişkili bir GUID'i vardır.|
| Adı                    | Aygıtın, aygıt dağıtımı sırasında Azure portalı aracılığıyla yapılandırıldığı gibi, aygıtın dostu adı. Varsayılan dostu ad, aygıt seri numarasıdır. |
| Model                   | StorSimple 8000 serisi cihazınızın modeli. Model 8100 veya 8600 olabilir.|
| SerialNumber            | Cihaz seri numarası fabrikada atanır ve 15 karakter uzunluğundadır. Örneğin, 8600-SHX0991003G44HT gösterir:<br> 8600 – Cihaz modelidir.<br>SHX – Üretim tesisidir.<br> 0991003 - Belirli bir üründür. <br> G44HT- son 5 basamak benzersiz seri numaraları oluşturmak için artmaktadır. Bu sıralı bir küme olmayabilir.|
| TimeZone                | Aygıt dağıtımı sırasında Azure portalında yapılandırılan aygıt saat dilimi.|
| Akım Denetleyicisi       | StorSimple cihazınızın Windows PowerShell arabirimi aracılığıyla bağlı olduğunuz denetleyici.|
| ActiveController        | Cihazınızda etkin olan ve tüm ağ ve disk işlemlerini denetleyen denetleyici. Bu Denetleyici 0 veya Denetleyici 1 olabilir.  |
| Denetleyici0Durum       | Aygıtınızdaki Denetleyici 0'ın durumu. Denetleyici durumu normal, kurtarma modunda veya erişilemez olabilir.|
| Denetleyici1Durum       | Aygıtınızdaki Denetleyici 1'in durumu.  Denetleyici durumu normal, kurtarma modunda veya erişilemez olabilir.|
| SystemMode              | StorSimple cihazınızın genel durumu. Aygıt durumu normal, bakım gerektiren veya kullanımdan kaldırılmış olabilir (Azure portalında devre dışı bırakılmaya karşılık gelir).|
| FriendlySoftwareVersion | Aygıt yazılımı sürümüne karşılık gelen dostu dize. Güncelleme 4 çalıştıran bir sistem için, dostu yazılım sürümü StorSimple 8000 Serisi Güncelleme 4.0 olacaktır.|
| HcsSoftwareVersion      | Cihazınızda çalışan HCS yazılım sürümü. Örneğin, StorSimple 8000 Serisi Güncelleme 4.0'a karşılık gelen HCS yazılım sürümü 6.3.9600.17820'dir. |
| ApiVersiyon              | HCS aygıtının Windows PowerShell API'sinin yazılım sürümü.|
| VhdVersion              | Aygıtın sevk edildiği fabrika görüntüsünün yazılım sürümü. Cihazınızı fabrika varsayılanlarına sıfırlarsanız, bu yazılım sürümünü çalıştırır.|
| OSVersion               | Windows Server işletim sisteminin aygıtta çalışan yazılım sürümü. StorSimple aygıtı, 6.3.9600'e karşılık gelen Windows Server 2012 R2'ye dayanır.|
| CisAgentVersion         | StorSimple cihazınızda çalışan Cis aracınızın sürümü. Bu aracı, Azure'da çalışan StorSimple Manager hizmetiyle iletişim kurmaya yardımcı olur.|
| MdsAgentVersion         | StorSimple cihazınızda çalışan Mds aracısına karşılık gelen sürüm. Bu aracı, verileri İzleme ve Tanılama Hizmeti'ne (MDS) taşır.|
| Lsisas2Sürüm          | StorSimple cihazınızdaki LSI sürücülerine karşılık gelen sürüm.|
| Kapasite                | Baytlar halindeki cihazın toplam kapasitesi.|
| Uzaktan Yönetim Modu    | Aygıtın Windows PowerShell arabirimi üzerinden uzaktan yönetilip yönetilemeyeceğini gösterir. |
| FipsMode                | Aygıtınızda ABD Federal Bilgi İşlem Standardı (FIPS) modunun etkin olup olmadığını gösterir. FIPS 140 standardı, hassas verilerin korunması için ABD Federal hükümet bilgisayar sistemleri tarafından kullanılmak üzere onaylanan şifreleme algoritmalarını tanımlar. Güncelleştirme 4 veya daha sonra çalıştıran aygıtlar için FIPS modu varsayılan olarak etkinleştirilir. |

## <a name="next-steps"></a>Sonraki adımlar

* [Invoke-HcsDiagnostics cmdlet sözdizimini](https://technet.microsoft.com/library/mt795371.aspx)öğrenin.

* StorSimple cihazınızdaki dağıtım sorunlarını nasıl [giderecek](storsimple-troubleshoot-deployment.md) hakkında daha fazla bilgi edinin.
