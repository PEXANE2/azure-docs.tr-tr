---
title: Azure Güvenlik Merkezi 'nde Endpoint Protection çözümlerini bulma ve sistem durumu değerlendirmesi | Microsoft Docs
description: Endpoint Protection çözümlerinin nasıl keşfedildiği ve sağlıklı olduğu nasıl tanımlandığı.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
ms.assetid: 2730a2f5-20bc-4027-a1c2-db9ed0539532
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2019
ms.author: v-mohabe
ms.openlocfilehash: 4d3fc90a722b9f4043e891a14b542e6b90c94c55
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68881029"
---
# <a name="endpoint-protection-assessment-and-recommendations-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde Endpoint Protection değerlendirmesi ve önerileri

Azure Güvenlik Merkezi 'ndeki Endpoint Protection değerlendirmesi ve önerileri, Endpoint Protection çözümlerinin [desteklenen](https://docs.microsoft.com/azure/security-center/security-center-os-coverage#supported-platforms-for-windows-computers-and-vms) sürümlerinin sistem durumu değerlendirmesini algılar ve sağlar. Bu konuda, Azure Güvenlik Merkezi tarafından Endpoint Protection çözümleri için aşağıdaki iki öneriyi üreten senaryolar açıklanmaktadır.

* **Uç nokta koruma çözümlerini sanal makinenize yükler**
* **Makinelerinizdeki Endpoint Protection sistem durumu sorunlarını çözün**

## <a name="windows-defender"></a>Windows Defender

* **"Endpoint Protection çözümlerini sanal makineye yükler"** önerisi, [Get-mpcomputerstatus](https://docs.microsoft.com/powershell/module/defender/get-mpcomputerstatus?view=win10-ps) çalıştırıldığında ve Result **amserviceenabled olduğunda üretilir: Yanlýþ**

* **"Makinelerinizdeki Endpoint Protection sistem durumu sorunlarını çözme"** önerisi, [Get-mpcomputerstatus](https://docs.microsoft.com/powershell/module/defender/get-mpcomputerstatus?view=win10-ps) çalıştırıldığında ve aşağıdakilerden biri veya her ikisi gerçekleştiğinde oluşturulur:

  * Aşağıdaki özelliklerden en az biri false şeklindedir:

     **AMServiceEnabled**

     **AntispywareEnabled**

     **RealTimeProtectionEnabled**

     **BehaviorMonitorEnabled**

     **Ioavprotectionenabled**

     **OnAccessProtectionEnabled**

  * Aşağıdaki özelliklerden biri veya her ikisi 7 ' den büyükse veya eşitse.

     **AntispywareSignatureAge**

     **AntivirusSignatureAge**

## <a name="microsoft-system-center-endpoint-protection"></a>Microsoft System Center Endpoint Protection

* **Scepmpmodule ("$env:P rogramFiles\Microsoft Security Client\mpprovider\mpprovider.exe")** içeri aktarılırken ve çalışırken **"Endpoint Protection çözümlerini sanal makineye yükler" önerisi üretilir** **Amserviceenabled = false** Ile Get-MProtComputerStatus sonuçları

* **"Makinelerinizdeki Endpoint Protection sistem durumu sorunlarını çözme"** önerisi, **Get-MprotComputerStatus** çalıştırıldığında ve aşağıdakilerden biri veya her ikisi gerçekleştiğinde oluşturulur:

    * Aşağıdaki özelliklerden en az biri false şeklindedir:

       **AMServiceEnabled**
    
       **AntispywareEnabled**
    
       **RealTimeProtectionEnabled**
    
       **BehaviorMonitorEnabled**
    
       **Ioavprotectionenabled**
    
       **OnAccessProtectionEnabled**
          
    * Aşağıdaki Imza güncelleştirmelerinden biri veya her ikisi 7 ' ye eşit veya daha büyükse. 

       **AntispywareSignatureAge**
    
       **AntivirusSignatureAge**

## <a name="trend-micro"></a>Trend Micro

* Aşağıdaki denetimlerden bir veya daha fazlası karşılanmazsa, **"Endpoint Protection çözümlerini sanal makineye yükler"** önerisi oluşur:
    * **HKLM: \ Software\trendmikro \ derin güvenlik Aracısı** var
    * **HKLM: \ Software\trendmikro \ derin güvenlik alanı \ ınstalyüklemeklasörü** var
    * **Dsq_query. cmd** dosyası yükleme klasöründe bulunur
    * **Dsa_query. cmd** Ile sonuçları **bileşen. har. Mode: on-Trend mikro derin güvenlik Aracısı algılandı**

## <a name="symantec-endpoint-protection"></a>Symantec uç nokta koruması
Aşağıdaki denetimlerden herhangi biri karşılanmazsa **"Endpoint Protection çözümlerini sanal makineye yükler"** önerisi oluşur:

* **HKLM: \ Software\Symantec\Symantec Endpoint Protection\CurrentVersion\PRODUCTNAME = "Symantec Endpoint Protection"**

* **HKLM: \ Software\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\ASRunningStatus = 1**

Or

* **HKLM: \ Software\Wow6432Node\Symantec\Symantec Endpoint Protection\CurrentVersion\PRODUCTNAME = "Symantec Endpoint Protection"**

* **HKLM: \ Software\Wow6432Node\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\ASRunningStatus = 1**

Aşağıdaki denetimlerden herhangi biri karşılanmazsa **"makinelerinizdeki Endpoint Protection sistem durumu sorunlarını çözün"** önerisi oluşur:  

* Symantec sürümünü denetle > = 12:  Kayıt defteri konumu: **HKLM: \ Software\Symantec\Symantec Endpoint Protection\CurrentVersion "-değer" PRODUCTVERSION "**

* Gerçek zamanlı koruma durumunu denetleyin: **HKLM: \ Software\Wow6432Node\Symantec\Symantec Endpoint Protection\AV\Storages\Filesystem\RealTimeScan\OnOff = = 1**

* Imza güncelleştirme durumunu denetle: **HKLM\Software\Symantec\Symantec uç noktası Protection\CurrentVersion\public-opstate\LatestVirusDefsDate < = 7 gün**

* Tam tarama durumunu denetleyin: **HKLM: \ Software\Symantec\Symantec Endpoint Protection\currentversion\public-opstate\lastbaşarılı fulscandatetime < = 7 gün**

* Symantec 12 için imza sürümüne ait imza sürüm numarası yolunu bul: **Kayıt defteri yolları + "CurrentVersion\SharedDefs"-değer "SRTSP"** 

* Symantec 14 için imza sürümü yolu: **Kayıt defteri yolları + "CurrentVersion\SharedDefs\SDSDefs"-değer "SRTSP"**

Kayıt defteri yolları:

* **"HKLM: \ Software\Symantec\Symantec Endpoint Protection" + $Path;**
* **"HKLM: \ Software\Wow6432Node\Symantec\Symantec Endpoint Protection" + $Path**

## <a name="mcafee-endpoint-protection-for-windows"></a>Windows için McAfee Endpoint Protection

Aşağıdaki denetimler karşılanmazsa **"Endpoint Protection çözümlerini sanal makineye yükler"** önerisi üretilir:

* **HKLM: \ SOFTWARE\McAfee\Endpoint\AV\ProductVersion** var

* **HKLM: \ SOFTWARE\McAfee\AVSolution\MCSHIELDGLOBAL\GLOBAL\enableoas = 1**

Aşağıdaki denetimler karşılanmazsa **"makinelerinizdeki Endpoint Protection sistem durumu sorunlarını çözün"** önerisi oluşur:

* McAfee sürümü: **HKLM: \ SOFTWARE\McAfee\Endpoint\AV\ProductVersion > = 10**

* Imza sürümünü bul: **HKLM: \ Software\McAfee\AVSolution\DS\DS-value "dwContentMajorVersion"**

* Imza bulma tarihi: **HKLM: \ Software\McAfee\AVSolution\DS\DS-value "szContentCreationDate" > = 7 gün**

* Tarama tarihini bul: **HKLM: \ Software\McAfee\Endpoint\AV\ODS-value "LastFullScanOdsRunTime" > = 7 gün**

## <a name="mcafee-endpoint-security-for-linux-threat-prevention"></a>Linux tehdit önleme için McAfee Endpoint Security 

Aşağıdaki denetimlerden biri veya her ikisi karşılanmazsa, **sanal makine önerisine Endpoint Protection çözümlerini yükler** önerisi oluşur:  

- **/Opt/iSEC/ens/threatprevention/bin/isecav** dosyası çıkıyor 

- **"/opt/iSEC/ens/threatprevention/bin/isecav--Version"** çıkışı: **McAfee Name = Linux tehdit önleme ve McAfee sürümü için McAfee Endpoint Security > = 10**

Aşağıdaki denetimlerden bir veya daha fazlası karşılanmazsa, **makinelerinizdeki Endpoint Protection sistem durumu sorunlarını çözün** .

- **"/opt/iSEC/ens/threatprevention/bin/isecav--listtask"** , taramanın her Ikisini de **hızlı tarama, tam tarama** ve her ikisi de döndürür < = 7 gün

- **"/opt/iSEC/ens/threatprevention/bin/isecav--listtask"** , **dat ve Engine güncelleştirme zamanını** ve her ikisini de döndürür < = 7 gün

- **"/opt/iSEC/ens/threatprevention/bin/isecav--getoasconfig--Summary"** , **erişim taraması** durumunda döndürür

## <a name="sophos-antivirus-for-linux"></a>Linux için Sophos Antivirus 

Aşağıdaki denetimlerden biri veya her ikisi karşılanmazsa, **sanal makine önerisine Endpoint Protection çözümlerini yükler** önerisi oluşur:

- **/Opt/Sophos-av/bin/savdstatus** dosyası çıkıyor veya özelleştirilmiş **"readlink $ (savscan)"** konumunu arar "

- **"/opt/Sophos-av/bin/savdstatus--Version"** , Sophos Name = **Sophos Anti-Virus ve Sophos Version > = 9** döndürür

Aşağıdaki denetimlerden bir veya daha fazlası karşılanmazsa, **makinelerinizdeki Endpoint Protection sistem durumu sorunlarını çözün** .

- **"/opt/Sophos-av/bin/savlog--maxAge = 7 | GREP-i "zamanlanmış tarama. "\* | tail-1" tamamlandı**, bir değer döndürüyor   

- **"/opt/Sophos-av/bin/savlog--maxAge = 7 | grep "tarama tamamlandı"** | Tail-1 ", bir değer döndürür   

- **"/opt/Sophos-av/bin/savdstatus--LastUpdate"** , < = 7 gün olması gereken LastUpdate döndürür 

- " **/opt/Sophos-av/bin/savdstatus-v"** , **"erişim taraması çalışıyor"** olarak eşit 

- **"/opt/Sophos-av/bin/savconfig LiveProtection al"** işlevi etkin  

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Microsoft kötü amaçlı yazılımdan koruma uzantısı günlükleri şurada bulunabilir:  
**%Systemdrive%\WindowsAzure\Logs\Plugins\Microsoft.Azure.Security.IaaSAntimalware (veya PaaSAntimalware) \1.5.5.x (sürüm #) \CommandExecution.log**

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Ya da bir Azure destek olayı da oluşturabilirsiniz. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Destek Al'ı seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).
