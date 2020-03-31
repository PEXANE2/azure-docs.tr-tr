---
title: Azure Güvenlik Merkezlerinde uç nokta koruma önerileri
description: Uç nokta koruma çözümlerinin nasıl keşfedildiği ve sağlıklı olarak nasıl tanımlanabildiğini.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 2730a2f5-20bc-4027-a1c2-db9ed0539532
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/29/2019
ms.author: memildin
ms.openlocfilehash: dcf7df501665ea3885d00b9f7668a95cbbf02428
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78208551"
---
# <a name="endpoint-protection-assessment-and-recommendations-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde uç nokta koruma değerlendirmesi ve önerileri

Azure Güvenlik Merkezi, Endpoint koruma çözümlerinin [desteklenen](security-center-services.md#endpoint-supported) sürümlerinin sistem durumu değerlendirmelerini sağlar. Bu makalede, Güvenlik Merkezi'nin aşağıdaki iki öneriyi oluşturmasına neden olan senaryolar açıklanmaktadır:

* **Sanal makinenize uç nokta koruma çözümleri yükleyin**
* **Makinelerinizdeki uç nokta koruması sağlık sorunlarını çözme**

## <a name="windows-defender"></a>Windows Defender

* Güvenlik Merkezi, [Get-MpComputerStatus](https://docs.microsoft.com/powershell/module/defender/get-mpcomputerstatus?view=win10-ps) çalıştığında ve sonuç **AMServiceEnabled** olduğunda **"Sanal makineye uç nokta koruma çözümleri yükleyin"** önerir: False

* Güvenlik Merkezi, [Get-MpComputerStatus](https://docs.microsoft.com/powershell/module/defender/get-mpcomputerstatus?view=win10-ps) çalıştığında ve aşağıdakilerden herhangi biri oluştuğunda **"Makinelerinizdeki uç nokta koruma sorunlarının giderilmesi"** önerir:

  * Aşağıdaki özelliklerden herhangi biri yanlış:

    **AMServiceEnabled**

    **Casus YazılımönlemeEtkin**

    **RealTimeProtectionEnabled**

    **Davranışİzleme Etkin**

    **IoavProtectionEtkin**

    **OnAccessProtectionEnabled**

  * Aşağıdaki özelliklerden biri veya her ikisi 7 veya daha fazla ise.

    **Casus yazılımlar önlemeSignatureAge**

    **AntivirusSignatureAge**

## <a name="microsoft-system-center-endpoint-protection"></a>Microsoft System Center uç nokta koruması

* Güvenlik Merkezi, **SCEPMpModule 'yi ("$env:ProgramFiles\Microsoft Security Client\MpProvider\MpProvider.psd1")** ve **AMServiceEnabled = ile** **Get-MProtComputerStatus** sonuçlarını çalıştırırken **"Sanal makineye uç nokta koruma çözümleri yüklemenizi"** önerir

* Güvenlik Merkezi, **Get-MprotComputerStatus** çalıştığında ve aşağıdakilerden herhangi biri oluştuğunda **"Makinelerinizdeki uç nokta koruma sorunlarının giderilmesi"** önerir:

    * Aşağıdaki özelliklerden en az biri yanlıştır:

            **AMServiceEnabled**

            **AntispywareEnabled**
    
            **RealTimeProtectionEnabled**
    
            **BehaviorMonitorEnabled**
    
            **IoavProtectionEnabled**
    
            **OnAccessProtectionEnabled**
          
    * Aşağıdaki İmza Güncelleştirmelerinden biri veya her ikisi daha büyük veya 7'ye eşitse. 

            **AntispywareSignatureAge**
    
            **AntivirusSignatureAge**

## <a name="trend-micro"></a>Trend Mikro

* Güvenlik Merkezi, aşağıdaki denetimlerden herhangi biri karşılanmadığında **"Sanal makineye uç nokta koruma çözümleri yüklemenizi"** önerir:
    * **HKLM:\SOFTWARE\TrendMicro\Derin Güvenlik Aracısı** var
    * **HKLM:\SOFTWARE\TrendMicro\Derin Güvenlik Aracısı\InstallationFolder** var
    * **dsa_query.cmd** dosyası Yükleme Klasöründe bulunur
    * **Component.AM.mode** ile **dsa_query.cmd** sonuçları nın çalıştırılışı: açık - Trend Micro Deep Security Agent algılandı

## <a name="symantec-endpoint-protection"></a>Symantec uç nokta koruması
Güvenlik Merkezi, aşağıdaki denetimlerden herhangi biri karşılanmadığında **"Sanal makineye uç nokta koruma çözümleri yüklemenizi"** önerir:

* **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\PRODUCTNAME = "Symantec Endpoint Protection"**

* **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\ASRunningStatus = 1**

Veya

* **HKLM:\Yazılım\Wow6432Düğüm\Symantec\Symantec Uç Nokta Koruması\CurrentVersion\PRODUCTNAME = "Symantec Endpoint Protection"**

* **HKLM:\Yazılım\Wow6432Düğüm\Symantec\Symantec Uç Nokta Koruması\CurrentVersion\public-opstate\ASRunningStatus = 1**

Güvenlik Merkezi, aşağıdaki denetimlerden herhangi biri karşılanmadığında **"Makinelerinizdeki uç nokta koruma sorunlarının giderilmesi"** önerir:

* Kontrol Symantec Sürüm >= 12: Kayıt defteri konumu: **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion" -Değer "PRODUCTVERSION"**

* Gerçek Zamanlı Koruma durumunu kontrol edin: **HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection\AV\Storages\Filesystem\RealTimeScan\OnOff == 1**

* İmza Güncelleme durumunu denetleyin: **HKLM\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\LatestVirusDefsDate <= 7 gün**

* Tam Tcan durumunu kontrol edin: **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\LastSuccessfulScanDateTimeTime <= 7 gün**

* Symantec 12 için imza sürümüne giden imza sürüm numarasını bulma: **Kayıt Yolları+ "CurrentVersion\SharedDefs" -Value "SRTSP"** 

* Symantec 14 için imza sürümüne giden yol: **Kayıt Yolları+ "CurrentVersion\SharedDefs\SDSDefs" -Değer "SRTSP"**

Kayıt Yolları:

* **"HKLM:\Software\Symantec\Symantec Endpoint Protection" + $Path;**
* **"HKLM:\Software\Wow6432Node\Symantec\Symantec Uç Nokta Koruması" + $Path**

## <a name="mcafee-endpoint-protection-for-windows"></a>Windows için McAfee uç noktası koruması

Güvenlik Merkezi, aşağıdaki denetimlerden herhangi biri karşılanmadığında **"Sanal makineye uç nokta koruma çözümleri yüklemenizi"** önerir:

* **HKLM:\SOFTWARE\McAfee\Endpoint\AV\ProductVersion** var

* **HKLM:\SOFTWARE\McAfee\AVSolution\MCSHIELDGLOBAL\GLOBAL\enableoas = 1**

Güvenlik Merkezi, aşağıdaki denetimlerden herhangi biri karşılanmadığında **"Makinelerinizdeki uç nokta koruma sorunlarının giderilmesi"** önerir:

* McAfee Sürüm: **HKLM:\SOFTWARE\McAfee\Endpoint\AV\ProductVersion >= 10**

* İmza Sürümünü Bul: **HKLM:\Software\McAfee\AVSolution\DS\DS\DS -Değer "dwContentMajorVersion"**

* İmza tarihi bul: **HKLM:\Software\McAfee\AVSolution\DS\DS\DS\DS -Değer "szContentCreationDate" >= 7 gün**

* Tkayı bul tarih: **HKLM:\Software\McAfee\Endpoint\AV\ODS -Değer "LastFullScanOdsRunTime" >= 7 gün**

## <a name="mcafee-endpoint-security-for-linux-threat-prevention"></a>Linux Tehdit Önleme için McAfee Endpoint Güvenlik 

Güvenlik Merkezi, aşağıdaki denetimlerden herhangi biri karşılanmadığında **"Sanal makineye uç nokta koruma çözümleri yüklemenizi"** önerir:

- Dosya **/opt/isec/ens/threatprevention/bin/isecav** çıkışları 

- **"/opt/isec/ens/threatprevention/bin/isecav --version"** çıktısı: **McAfee adı = McAfee Endpoint Security for Linux Threat Prevention ve McAfee version >= 10**

Güvenlik Merkezi, aşağıdaki denetimlerden herhangi biri karşılanmadığında **"Makinelerinizdeki uç nokta koruma sorunlarının giderilmesi"** önerir:

- **"/opt/isec/ens/threatprevention/bin/isecav --listtask"** **hızlı tarama, Tam tarama** ve her iki tarama <= 7 gün döndürür

- **"/opt/isec/ens/threatprevention/bin/isecav --listtask"** **DAT ve motor Güncelleme süresini** döndürür ve her ikisi de <= 7 gün

- **"/opt/isec/ens/threatprevention/bin/isecav --getoasconfig --summary"** **Access Scan** durumu hakkında döndürür

## <a name="sophos-antivirus-for-linux"></a>Linux için Sophos Antivirus 

Güvenlik Merkezi, aşağıdaki denetimlerden herhangi biri karşılanmadığında **"Sanal makineye uç nokta koruma çözümleri yüklemenizi"** önerir:

- Dosya **/opt/sophos-av/bin/savdstatus** çıkar veya özelleştirilmiş konum için arama **"readlink $(hangi savscan)"**

- **"/opt/sophos-av/bin/savdstatus --version"** Sophos adını döndürür = **Sophos Anti-Virüs ve Sophos sürüm >= 9**

Güvenlik Merkezi, aşağıdaki denetimlerden herhangi biri karşılanmadığında **"Makinelerinizdeki uç nokta koruma sorunlarının giderilmesi"** önerir:

- **"/opt/sophos-av/bin/savlog --maxage=7 | grep -i "Planlanmış tcan . \* tamamlandı" | kuyruk -1"**, bir değer döndürür

- **"/opt/sophos-av/bin/savlog --maxage=7 | grep "scan bitmiş"** | kuyruk -1", bir değer döndürür

- **"/opt/sophos-av/bin/savdstatus --lastupdate"** lastUpdate'i döndürür, bu da <= 7 gün olmalıdır 

- **"/opt/sophos-av/bin/savdstatus -v"** **eşittir "Erişimde tarama çalışıyor"** 

- **"/opt/sophos-av/bin/savconfig liveprotection olsun"** returns etkin

## <a name="troubleshoot-and-support"></a>Sorun giderme ve destek

### <a name="troubleshoot"></a>Sorun giderme

Microsoft Malware uzantılı günlükleri şu adresten edinilebilir: **%Systemdrive%\WindowsAzure\Logs\Plugins\Microsoft.Azure.Security.IaaSAntimalware(Veya PaaSAntimalware)\1.5.5.x(sürüm#)\CommandExecution.log**

### <a name="support"></a>Destek

Daha fazla yardım için [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarına başvurun. Veya bir Azure destek olayı dosyala. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve destek al'ı seçin. Azure Desteği'ni kullanma hakkında daha fazla bilgi için [Microsoft Azure destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.