---
title: Azure Site Kurtarma'da VMware/fiziksel olağanüstü durum kurtarma için destek matrisi
description: Azure Site Kurtarma'yı kullanarak VMware VM'lerinin ve fiziksel sunucunun Azure'a olağanüstü durum kurtarma desteğini özetler.
ms.topic: conceptual
ms.date: 2/24/2020
ms.openlocfilehash: b4cf19f4f74ba24951efb806a9f2e3d88fcad7bc
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478418"
---
# <a name="support-matrix-for-disaster-recovery--of-vmware-vms-and-physical-servers-to-azure"></a>VMware VM'lerinin ve fiziksel sunucuların Azure'a olağanüstü durum kurtarması için destek matrisi

Bu makalede, VMware VM'lerinin ve fiziksel sunucuların [Azure Site Kurtarma'yı](site-recovery-overview.md)kullanarak Azure'a olağanüstü durum kurtarması için desteklenen bileşenler ve ayarlar özetlenmiştir.

- VMware VM/fiziksel sunucu olağanüstü durum kurtarma mimarisi hakkında [daha fazla bilgi edinin.](vmware-azure-architecture.md)
- Felaket kurtarma denemek için [bizim öğreticiler](tutorial-prepare-azure.md) izleyin.

## <a name="deployment-scenarios"></a>Dağıtım senaryoları

**Senaryo** | **Şey**
--- | ---
VMware VM'lerin olağanüstü kurtarma | Şirket içi VMware VM'lerin Azure'a çoğaltılması. Bu senaryoyu Azure portalında veya [PowerShell'i](vmware-azure-disaster-recovery-powershell.md)kullanarak dağıtabilirsiniz.
Fiziksel sunucuların olağanüstü durum kurtarma | Şirket içi Windows/Linux fiziksel sunucularının Azure'a çoğaltılması. Bu senaryoyu Azure portalında dağıtabilirsiniz.

## <a name="on-premises-virtualization-servers"></a>Şirket içi sanallaştırma sunucuları

**Sunucu** | **Gereksinimler** | **Şey**
--- | --- | ---
vCenter Server | Sürüm 6.7, 6.5, 6.0 veya 5.5 | Olağanüstü durum kurtarma dağıtımınızda bir vCenter sunucusu kullanmanızı öneririz.
vSphere ev sahipliği | Sürüm 6.7, 6.5, 6.0 veya 5.5 | vSphere ana bilgisayarlarının ve vCenter sunucularının işlem sunucusuyla aynı ağda bulunmasını öneririz. Varsayılan olarak işlem sunucusu yapılandırma sunucusunda çalışır. [Daha fazla bilgi edinin](vmware-physical-azure-config-process-server-overview.md).


## <a name="site-recovery-configuration-server"></a>Site Kurtarma yapılandırma sunucusu

Yapılandırma sunucusu, yapılandırma sunucusu, işlem sunucusu ve ana hedef sunucu dahil olmak üzere Site Kurtarma bileşenlerini çalıştıran şirket içi bir makinedir.

- VMware VM'ler için bir VMware VM oluşturmak için bir OVF şablonu indirerek yapılandırma sunucusunu ayarlarsınız.
- Fiziksel sunucular için yapılandırma sunucusu makinesini el ile ayarlarsınız.

**Bileşen** | **Gereksinimler**
--- |---
CPU çekirdekleri | 8
RAM | 16 GB
Disk sayısı | 3 disk<br/><br/> Diskler işletim sistemi diski, işlem sunucusu önbellek diski ve failback için bekletme sürücüsü içerir.
Disk boş alanı | İşlem sunucusu önbelleği için 600 GB alan.
Disk boş alanı | Bekletme sürücüsü için 600 GB alan.
İşletim sistemi  | Masaüstü deneyimiyle Windows Server 2012 R2 veya Windows Server 2016 <br/><br> Bu cihazın dahili Master Target'ını geri tepme için kullanmayı planlıyorsanız, işletim sistemi sürümünün çoğaltılan öğelerle aynı veya daha yüksek olduğundan emin olun.|
İşletim sistemi yerel ayarı | İngilizce (en-us)
[PowerCLI](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) | Yapılandırma sunucusu sürümü [9.14](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery) veya sonrası için gerekli değildir.
Windows Server rolleri | Active Directory Domain Services'ı etkinleştirme; İnternet Bilgi Hizmetleri (IIS) veya Hyper-V.
Grup ilkeleri| - Komut istemine erişimi engelleyin. <br/> - Kayıt defteri düzenleme araçlarına erişimi engelleyin. <br/> - Dosya ekleri için güven mantığı. <br/> - Komut Dosyası Yürütme'yi açın. <br/> - [Daha fazla bilgi](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)|
IIS | Şunları yaptığınızdan emin olun:<br/><br/> - Önceden var olan bir varsayılan web siteniz yok <br/> - [Anonim kimlik doğrulamayı etkinleştirme](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731244(v=ws.10)) <br/> - [FastCGI](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753077(v=ws.10)) ayarını etkinleştirin  <br/> - 443 sayılı bağlantı noktasında önceden web sitesi/uygulama dinleme yok<br/>
NIC türü | VMXNET3 (VMware VM olarak dağıtıldığında)
IP adresi türü | Statik
Bağlantı Noktaları | Kontrol kanalı orkestrasyonu için kullanılan 443<br/>Veri taşıma için 9443

## <a name="replicated-machines"></a>Çoğaltılan makineler

Site Kurtarma, desteklenen bir makinede çalışan tüm iş yükünün çoğaltılması işlemini destekler.

> [!Note]
> Aşağıdaki tabloda BIOS önyüklemeli makineler için destek listelenir. UEFI tabanlı makinelerde destek için lütfen [Depolama](#storage) bölümüne bakın.

**Bileşen** | **Şey**
--- | ---
Makine ayarları | Azure'da çoğalan makineler [Azure gereksinimlerini](#azure-vm-requirements)karşılamalıdır.
Makine iş yükü | Site Kurtarma, desteklenen bir makinede çalışan tüm iş yükünün çoğaltılması işlemini destekler. [Daha fazla bilgi edinin](site-recovery-workload.md).
Windows Server 2019 | [Update rollup 34](https://support.microsoft.com/help/4490016) 'den (Mobilite hizmetinin 9.22 sürümü) itibaren desteklenir.
Windows Server 2016 64 bit | Server Core, Desktop Experience ile Server için desteklenir.
Windows Server 2012 R2 / Windows Server 2012 | Destekleniyor.
Windows Server 2008 R2 SP1 ile itibaren. | Destekleniyor.<br/><br/> Mobilite servis aracısının [9.30](https://support.microsoft.com/en-us/help/4531426/update-rollup-42-for-azure-site-recovery) sürümünden, Windows 2008 R2 çalıştıran makinelerde SP1 veya daha yeni olan servis [yığını güncelleştirmesi (SSU)](https://support.microsoft.com/help/4490628) ve [SHA-2 güncelleştirmesinin](https://support.microsoft.com/help/4474419) yüklü olarak yüklenmesi gerekir. SHA-1 Eylül 2019'dan itibaren desteklenmez ve SHA-2 kod imzalama etkin değilse aracı uzantısı beklendiği gibi yüklenmez/yükseltmeyapmaz. [SHA-2 yükseltmesi ve gereksinimleri](https://aka.ms/SHA-2KB)hakkında daha fazla bilgi edinin.
Windows Server 2008 SP2 veya daha sonra (64-bit/32-bit) ile |  Yalnızca geçiş için desteklenir. [Daha fazla bilgi edinin](migrate-tutorial-windows-server-2008.md).<br/><br/> Mobilite servis aracısının [9.30](https://support.microsoft.com/en-us/help/4531426/update-rollup-42-for-azure-site-recovery) sürümünden, Windows 2008 SP2 makinelerinde hizmet [yığını güncelleştirmesi (SSU)](https://support.microsoft.com/help/4493730) ve [SHA-2 güncelleştirmesinin](https://support.microsoft.com/help/4474419) yüklü olarak yüklenmesi gerekir. ISHA-1 Eylül 2019'dan itibaren desteklenmez ve SHA-2 kod imzalama etkin değilse aracı uzantısı beklendiği gibi yüklenmez/yükseltmez. [SHA-2 yükseltmesi ve gereksinimleri](https://support.microsoft.com/en-us/help/4472027/2019-sha-2-code-signing-support-requirement-for-windows-and-wsus)hakkında daha fazla bilgi edinin.
10, Windows 8.1, Windows 8 | Destekleniyor.
Windows 7 SP1 64-bit ile | [Update rollup 36'dan](https://support.microsoft.com/help/4503156) (Mobilite hizmetinin 9.22 sürümü) itibaren desteklenir. </br></br> Mobility servis aracısının [9.30'undan](https://support.microsoft.com/en-us/help/4531426/update-rollup-42-for-azure-site-recovery) itibaren, Windows 7 SP1 makinelerinde hizmet [yığını güncelleştirmesi (SSU)](https://support.microsoft.com/help/4490628) ve [SHA-2 güncelleştirmesinin](https://support.microsoft.com/help/4474419) yüklü olarak yüklenmesi gerekir.  SHA-1 Eylül 2019'dan itibaren desteklenmez ve SHA-2 kod imzalama etkin değilse aracı uzantısı beklendiği gibi yüklenmez/yükseltmeyapmaz. [SHA-2 yükseltmesi ve gereksinimleri](https://support.microsoft.com/en-us/help/4472027/2019-sha-2-code-signing-support-requirement-for-windows-and-wsus)hakkında daha fazla bilgi edinin.
Linux | Yalnızca 64 bit sistem desteklenir. 32 bit sistem desteklenmiyor.<br/><br/>Her Linux sunucusunda [Linux Entegrasyon Hizmetleri (LIS) bileşenleri](https://www.microsoft.com/download/details.aspx?id=55106) yüklü olmalıdır. Test başarısız olduktan/başarısız olduktan sonra sunucuyu Azure'da önyükleme yapmak gerekir. LIS bileşenleri eksikse, makinelerin Azure'da önyükleme yapmasını etkinleştirmeden önce [bileşenleri](https://www.microsoft.com/download/details.aspx?id=55106) yüklediğinizden emin olun. <br/><br/> Site Kurtarma, Azure'da Linux sunucularını çalıştırmak için başarısız olur. Ancak Linux satıcıları desteği yalnızca yaşam sonuna ulaşmamış dağıtım sürümleriyle sınırlandırabilir.<br/><br/> Linux dağıtımlarında, yalnızca dağıtım küçük sürüm sürümü/güncelleştirmesinin bir parçası olan stok çekirdekleri desteklenir.<br/><br/> Korumalı makinelerin büyük Linux dağıtım sürümlerinde yükseltilmesi desteklenmez. Çoğaltmayı yükseltmek için çoğaltmayı devre dışı, işletim sistemini yükseltin ve ardından çoğaltmayı yeniden etkinleştirin.<br/><br/> Azure'da Linux ve açık kaynak teknolojisi desteği hakkında [daha fazla bilgi edinin.](https://support.microsoft.com/help/2941892/support-for-linux-and-open-source-technology-in-azure)
Linux Red Hat Enterprise | 5.2 ile 5.11 arasında</b><br/> 6.1 ile 6.10 arasında</b> </br> 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6, [7.7,](https://support.microsoft.com/help/4528026/update-rollup-41-for-azure-site-recovery) [8.0](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery), 8.1 <br/> Red Hat Enterprise Linux 5.2-5.11 çalıştıran sunucular & 6.1-6.10 [Linux Entegrasyon Hizmetleri (LIS) bileşenleri](https://www.microsoft.com/download/details.aspx?id=55106) önceden yüklenmiş değil. Makinelerin Azure'da önyükleme yapmasını etkinleştirmeden önce [bileşenleri](https://www.microsoft.com/download/details.aspx?id=55106) yüklediğinizden emin olun.
Linux: CentOS | 5.2 ile 5.11 arasında</b><br/> 6.1 ile 6.10 arasında</b><br/> 7.0 ile 7.6 arasında<br/> <br/> 8.0 ile 8.1 arasında<br/><br/> CentOS 5.2-5.11 & 6.1-6.10 çalıştıran sunucularda [Linux Tümleştirme Hizmetleri (LIS) bileşenleri](https://www.microsoft.com/download/details.aspx?id=55106) önceden yüklenmemiştir. Makinelerin Azure'da önyükleme yapmasını etkinleştirmeden önce [bileşenleri](https://www.microsoft.com/download/details.aspx?id=55106) yüklediğinizden emin olun.
Ubuntu | Ubuntu 14.04 LTS sunucusu [(çekirdek sürümlerini gözden geçirin)](#ubuntu-kernel-versions)<br/><br/>Ubuntu 16.04 LTS sunucusu [(çekirdek sürümlerini gözden geçirin)](#ubuntu-kernel-versions) </br> Ubuntu 18.04 LTS sunucusu [(çekirdek sürümlerini gözden geçirin)](#ubuntu-kernel-versions)
Debian | Debian 7/Debian 8 [(inceleme desteklenen çekirdek sürümleri)](#debian-kernel-versions)
SUSE Linux | SUSE Linux Enterprise Server 12 SP1, SP2, SP3, SP4 [(çekirdek sürümlerini gözden geçirin)](#suse-linux-enterprise-server-12-supported-kernel-versions) <br/> SUSE Linux Enterprise Server 15, 15 SP1 [(çekirdek sürümlerini gözden geçirin)](#suse-linux-enterprise-server-15-supported-kernel-versions)<br/> SUSE Linux Enterprise Server 11 SP3, SUSE Linux Enterprise Server 11 SP4<br/> Çoğaltılan makinelerin SUSE Linux Enterprise Server 11 SP3'ten SP4'e yükseltilmesi desteklenmez. Yükseltmek için çoğaltmayı devre dışı bırakıp yükseltmeden sonra yeniden etkinleştirin.
Oracle Linux | 6.4, 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6, [7.7](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery)<br/><br/> Red Hat uyumlu çekirdek veya Kırılmaz Enterprise Çekirdek Yayın 3, 4 & 5 (UEK3, UEK4, UEK5)

> [!Note]
> Azure Site Kurtarma, Windows sürümlerinin her biri için yalnızca [Uzun Vadeli Servis Kanalı (LTSC)](/windows-server/get-started-19/servicing-channels-19#long-term-servicing-channel-ltsc) yapılarını destekler.  [Altı Aylık Kanal](/windows-server/get-started-19/servicing-channels-19#semi-annual-channel) sürümleri şu anda desteklenmiş.

### <a name="ubuntu-kernel-versions"></a>Ubuntu çekirdek sürümleri

**Desteklenen sürüm** | **Mobilite hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
14.04 LTS | [9.32][9.32 UR] | 3.13.0-24-jenerik için 3.13.0-170-jenerik,<br/>3.16.0-25-jenerik için 3.16.0-77-jenerik,<br/>3.19.0-18-jenerik için 3.19.0-80-jenerik,<br/>4.2.0-18-jenerik için 4.2.0-42-jenerik,<br/>4.4.0-21-jenerik için 4.4.0-148-jenerik,<br/>4.15.0-1023-azure için 4.15.0-1045-azure |
14.04 LTS | [9.31][9.31 UR] | 3.13.0-24-jenerik için 3.13.0-170-jenerik,<br/>3.16.0-25-jenerik için 3.16.0-77-jenerik,<br/>3.19.0-18-jenerik için 3.19.0-80-jenerik,<br/>4.2.0-18-jenerik için 4.2.0-42-jenerik,<br/>4.4.0-21-jenerik için 4.4.0-148-jenerik,<br/>4.15.0-1023-azure için 4.15.0-1045-azure |
14.04 LTS | [9.30][9.30 UR] | 3.13.0-24-jenerik için 3.13.0-170-jenerik,<br/>3.16.0-25-jenerik için 3.16.0-77-jenerik,<br/>3.19.0-18-jenerik için 3.19.0-80-jenerik,<br/>4.2.0-18-jenerik için 4.2.0-42-jenerik,<br/>4.4.0-21-jenerik için 4.4.0-148-jenerik,<br/>4.15.0-1023-azure için 4.15.0-1045-azure |
14.04 LTS | [9.29][9.29 UR]| 3.13.0-24-jenerik için 3.13.0-170-jenerik,<br/>3.16.0-25-jenerik için 3.16.0-77-jenerik,<br/>3.19.0-18-jenerik için 3.19.0-80-jenerik,<br/>4.2.0-18-jenerik için 4.2.0-42-jenerik,<br/>4.4.0-21-jenerik için 4.4.0-148-jenerik,<br/>4.15.0-1023-azure için 4.15.0-1045-azure |
|||
16.04 LTS | [9.32][9.32 UR] | 4.4.0-21-jenerik için 4.4.0-171-jenerik,<br/>4.8.0-34-jenerik için 4.8.0-58-jenerik,<br/>4.10.0-14-jenerik için 4.10.0-42-jenerik,<br/>4.11.0-13-jenerik için 4.11.0-14-jenerik,<br/>4.13.0-16-jenerik için 4.13.0-45-jenerik,<br/>4.15.0-13-jenerik için 4.15.0-74-jenerik<br/>4.11.0-1009-azure için 4.11.0-1016-azure,<br/>4.13.0-1005-azure için 4.13.0-1018-azure <br/>4.15.0-1012-azure için 4.15.0-1066-azure|
16.04 LTS | [9.31][9.31 UR] | 4.4.0-21-jenerik için 4.4.0-170-jenerik,<br/>4.8.0-34-jenerik için 4.8.0-58-jenerik,<br/>4.10.0-14-jenerik için 4.10.0-42-jenerik,<br/>4.11.0-13-jenerik için 4.11.0-14-jenerik,<br/>4.13.0-16-jenerik için 4.13.0-45-jenerik,<br/>4.15.0-13-jenerik için 4.15.0-72-jenerik<br/>4.11.0-1009-azure için 4.11.0-1016-azure,<br/>4.13.0-1005-azure için 4.13.0-1018-azure <br/>4.15.0-1012-azure için 4.15.0-1063-azure|
16.04 LTS | [9.30][9.30 UR] | 4.4.0-21-jenerik için 4.4.0-166-jenerik,<br/>4.8.0-34-jenerik için 4.8.0-58-jenerik,<br/>4.10.0-14-jenerik için 4.10.0-42-jenerik,<br/>4.11.0-13-jenerik için 4.11.0-14-jenerik,<br/>4.13.0-16-jenerik için 4.13.0-45-jenerik,<br/>4.15.0-13-jenerik için 4.15.0-66-jenerik<br/>4.11.0-1009-azure için 4.11.0-1016-azure,<br/>4.13.0-1005-azure için 4.13.0-1018-azure <br/>4.15.0-1012-azure için 4.15.0-1061-azure|
16.04 LTS | [9.29][9.29 UR] | 4.4.0-21-jenerik için 4.4.0-164-jenerik,<br/>4.8.0-34-jenerik için 4.8.0-58-jenerik,<br/>4.10.0-14-jenerik için 4.10.0-42-jenerik,<br/>4.11.0-13-jenerik için 4.11.0-14-jenerik,<br/>4.13.0-16-jenerik için 4.13.0-45-jenerik,<br/>4.15.0-13-jenerik için 4.15.0-64-jenerik<br/>4.11.0-1009-azure için 4.11.0-1016-azure,<br/>4.13.0-1005-azure için 4.13.0-1018-azure <br/>4.15.0-1012-azure için 4.15.0-1059-azure|
|||
18.04 LTS | [9.32][9.32 UR]| 4.15.0-20-jenerik için 4.15.0-74-jenerik </br> 4.18.0-13-jenerik için 4.18.0-25-jenerik </br> 5.0.0-15-jenerik için 5.0.0-37-jenerik </br> 5.3.0-19-jenerik için 5.3.0-24-jenerik </br> 4.15.0-1009-azure için 4.15.0-1037-azure </br> 4.18.0-1006-azure için 4.18.0-1025-azure </br> 5.0.0-1012-azure için 5.0.0-1028-azure </br> 5.3.0-1007-azure için 5.3.0-1009-azure|
18.04 LTS | [9.31][9.31 UR]| 4.15.0-20-jenerik için 4.15.0-72-jenerik </br> 4.18.0-13-jenerik için 4.18.0-25-jenerik </br> 5.0.0-15-jenerik için 5.0.0-37-jenerik </br> 5.3.0-19-jenerik için 5.3.0-24-jenerik </br> 4.15.0-1009-azure için 4.15.0-1037-azure </br> 4.18.0-1006-azure için 4.18.0-1025-azure </br> 5.0.0-1012-azure için 5.0.0-1025-azure </br> 5.3.0-1007-azure|
18.04 LTS | [9.30][9.30 UR] | 4.15.0-20-jenerik için 4.15.0-66-jenerik </br> 4.18.0-13-jenerik için 4.18.0-25-jenerik </br> 5.0.0-15-genel 5.0.0-32-jenerik </br> 4.15.0-1009-azure için 4.15.0-1037-azure </br> 4.18.0-1006-azure için 4.18.0-1025-azure </br> 5.0.0-1012-azure için 5.0.0-1023-azure|
18.04 LTS | [9.29][9.29 UR] | 4.15.0-20-jenerik için 4.15.0-62-jenerik </br> 4.18.0-13-jenerik için 4.18.0-25-jenerik </br> 5.0.0-15-genel için 5.0.0-27-jenerik </br> 4.15.0-1009-azure için 4.15.0-1037-azure </br> 4.18.0-1006-azure için 4.18.0-1025-azure </br> 5.0.0-1012-azure için 5.0.0-1018-azure|

### <a name="debian-kernel-versions"></a>Debian çekirdek sürümleri


**Desteklenen sürüm** | **Mobilite hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
Debian 7 | [9.29][9.29 UR], [9.30][9.30 UR], [9.31][9.31 UR], [9.32][9.32 UR]| 3.2.0-4-amd64 için 3.2.0-6-amd64, 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | [9.30][9.30 UR], [9.31][9.31 UR], [9.32][9.32 UR] | 3.16.0-4-amd64 ila 3.16.0-10-amd64, 4.9.0-0.bpo.4-amd64 ila 4.9.0-0.bpo.11-amd64 |
Debian 8 | [9.29][9.29 UR] | 3.16.0-4-amd64 ila 3.16.0-10-amd64, 4.9.0-0.bpo.4-amd64 ila 4.9.0-0.bpo.9-amd64 |

### <a name="suse-linux-enterprise-server-12-supported-kernel-versions"></a>SUSE Linux Enterprise Server 12 desteklenen çekirdek sürümleri

**Yayınla** | **Mobilite hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
SUSE Linux Enterprise Server 12 (SP1,SP2,SP3,SP4) | [9.32][9.32 UR] | Tüm [stok SUSE 12 SP1,SP2,SP3, SP4 çekirdekleri](https://wiki.microfocus.com/index.php/SUSE/SLES/Kernel_versions#SUSE_Linux_Enterprise_Server_12) desteklenir.</br></br> 4.4.138-4.7-azure için 4.4.180-4.31-azure,</br>4.12.14-6.3-azure için 4.12.14-6.34-azure  |
SUSE Linux Enterprise Server 12 (SP1,SP2,SP3,SP4) | [9.31][9.31 UR] | Tüm [stok SUSE 12 SP1,SP2,SP3, SP4 çekirdekleri](https://wiki.microfocus.com/index.php/SUSE/SLES/Kernel_versions#SUSE_Linux_Enterprise_Server_12) desteklenir.</br></br> 4.4.138-4.7-azure için 4.4.180-4.31-azure,</br>4.12.14-6.3-azure için 4.12.14-6.29-azure  |
SUSE Linux Enterprise Server 12 (SP1,SP2,SP3,SP4) | [9.30][9.30 UR] | Tüm [stok SUSE 12 SP1,SP2,SP3, SP4 çekirdekleri](https://wiki.microfocus.com/index.php/SUSE/SLES/Kernel_versions#SUSE_Linux_Enterprise_Server_12) desteklenir.</br></br> 4.4.138-4.7-azure için 4.4.180-4.31-azure,</br>4.12.14-6.3-azure için 4.12.14-6.26-azure  |
SUSE Linux Enterprise Server 12 (SP1,SP2,SP3,SP4) | [9.29][9.29 UR] | Tüm [stok SUSE 12 SP1,SP2,SP3, SP4 çekirdekleri](https://wiki.microfocus.com/index.php/SUSE/SLES/Kernel_versions#SUSE_Linux_Enterprise_Server_12) desteklenir.</br></br> 4.4.138-4.7-azure için 4.4.180-4.31-azure,</br>4.12.14-6.3-azure için 4.12.14-6.23-azure  |

### <a name="suse-linux-enterprise-server-15-supported-kernel-versions"></a>SUSE Linux Enterprise Server 15 desteklenen çekirdek sürümleri

**Yayınla** | **Mobilite hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
SUSE Linux Enterprise Server 15 ve 15 SP1 | 9.32 | Tüm [stok SUSE 15 ve 15 çekirdekleri](https://wiki.microfocus.com/index.php/SUSE/SLES/Kernel_versions#SUSE_Linux_Enterprise_Server_15) desteklenir.</br></br> 4.12.14-5.5-azure için 4.12.14-8.22-azure |

## <a name="linux-file-systemsguest-storage"></a>Linux dosya sistemleri/konuk depolama

**Bileşen** | **Desteklenen**
--- | ---
Dosya sistemleri | ext3, ext4, XFS, BTRFS (bu tabloya göre geçerli koşullar)
Birim yöneticisi | - LVM desteklenir.<br/> - /boot ON LVM, [Update Rollup 31'den](https://support.microsoft.com/help/4478871/) (Mobilite hizmetinin 9.20 sürümü) itibaren desteklenir. Önceki Mobilite hizmet sürümlerinde desteklenmez.<br/> - Birden çok işletim sistemi diski desteklenmez.
Paravirtualized depolama cihazları | Parasanallaştırılmış sürücüler tarafından dışarı aktarılan cihazlar desteklenmez.
Çok sıralı blok IO aygıtları | Desteklenmiyor.
HP CCISS depolama denetleyicisi olan fiziksel sunucular | Desteklenmiyor.
Aygıt/Montaj noktası adlandırma kuralı | Aygıt adı veya montaj noktası adı benzersiz olmalıdır.<br/> İki aygıtın/montaj noktasının büyük/küçük harf duyarlı adlar olmadığından emin olun. Örneğin, *aygıt1* ve *Device1* ile aynı VM aygıtlarını adlandırma desteklenmez.
Dizinler | Mobilite hizmetinin bir sürümünü sürüm 9.20'den [(Güncelleştirme Rollup 31'de](https://support.microsoft.com/help/4478871/)yayımlandı) daha önce çalıştırıyorsanız, bu kısıtlamalar geçerlidir:<br/><br/> - Bu dizinler (ayrı bölümler/dosya sistemleri olarak ayarlanmışsa) kaynak sunucuda aynı işletim sistemi diskinde olmalıdır: /(root), /boot, /usr, /usr/local, /var, /etc.</br> - /boot dizini bir disk bölümü üzerinde olmalı ve bir LVM hacmi olmamalıdır.<br/><br/> Sürüm 9.20'den itibaren bu kısıtlamalar geçerli değildir.
Önyükleme dizini | - Önyükleme diskleri GPT bölüm biçiminde olmamalıdır. Bu bir Azure mimarisi sınırlamasıdır. GPT diskleri veri diski olarak desteklenir.<br/><br/> VM'deki birden çok önyükleme diski desteklenmiyor<br/><br/> - Birden fazla diskteki LVM biriminde /boot desteklenmez.<br/> - Önyükleme diski olmayan bir makine çoğaltılamaz.
Boş alan gereksinimleri| /kök bölmede 2 GB <br/><br/> Yükleme klasöründe 250 MB
XFSv5 | Meta data checksum gibi XFS dosya sistemlerindeki XFSv5 özellikleri desteklenir (Mobilite hizmeti sürümü 9.10'dan itibaren).<br/> Bölüm için XFS süper bloğunu denetlemek için xfs_info yardımcı programını kullanın. 1 `ftype` olarak ayarlanmışsa, XFSv5 özellikleri kullanılır.
BTRFS | BTRFS, Update [Rollup 34'ten](https://support.microsoft.com/help/4490016) (Mobilite hizmetinin 9.22 sürümü) itibaren desteklenir. BTRFS şu larsa desteklenmez:<br/><br/> - BTRFS dosya sistemi alt hacmi, koruma sağladıktan sonra değiştirilir.</br> - BTRFS dosya sistemi birden fazla diske yayılır.</br> - BTRFS dosya sistemi RAID'i destekler.

## <a name="vmdisk-management"></a>VM/Disk yönetimi

**Eylem** | **Şey**
--- | ---
Çoğaltılan VM'deki diski yeniden boyutlandırma | Başarısız olmadan önce kaynak VM'de, doğrudan VM özelliklerinde desteklenir. Çoğaltmayı devre dışı kaldırmaya/yeniden etkinleştirmeye gerek yok.<br/><br/> Başarısız olduktan sonra kaynak VM'yi değiştirirseniz, değişiklikler yakalamaz.<br/><br/> Başarısız olduktan sonra Azure VM'deki disk boyutunu değiştirirseniz, başarısız olduğunuzda, Site Kurtarma güncelleştirmelerle birlikte yeni bir VM oluşturur.
Çoğaltılan VM'ye disk ekleme | Desteklenmiyor.<br/> VM için çoğaltmayı devre dışı bırakıp diski ekleyin ve çoğaltmayı yeniden etkinleştirin.

## <a name="network"></a>Ağ

**Bileşen** | **Desteklenen**
--- | ---
Ana ağ NIC Takım | VMware VM'ler için desteklenir. <br/><br/>Fiziksel makine çoğaltma için desteklenmez.
Ana ağ VLAN | Evet.
Ana ağ IPv4 | Evet.
Ana ağ IPv6 | Hayır.
Konuk/sunucu ağı NIC Ekip | Hayır.
Konuk/sunucu ağı IPv4 | Evet.
Konuk/sunucu ağı IPv6 | Hayır.
Konuk/sunucu ağı statik IP (Windows) | Evet.
Konuk/sunucu ağı statik IP (Linux) | Evet. <br/><br/>VM'ler, başarısız geri dönüşte DHCP kullanacak şekilde yapılandırılmıştır.
Konuk/sunucu ağı birden çok NIC | Evet.


## <a name="azure-vm-network-after-failover"></a>Azure VM ağı (başarısız olduktan sonra)

**Bileşen** | **Desteklenen**
--- | ---
Azure ExpressRoute | Evet
ILB | Evet
Elb | Evet
Azure Traffic Manager | Evet
Çoklu Nİş | Evet
Ayrılmış IP adresi | Evet
IPv4 | Evet
Kaynak IP adresini koruyun | Evet
Azure sanal ağ hizmeti bitiş noktaları<br/> | Evet
Hızlandırılmış ağ iletişimi | Hayır

## <a name="storage"></a>Depolama
**Bileşen** | **Desteklenen**
--- | ---
Dinamik disk | İşletim sistemi diski temel bir disk olmalıdır. <br/><br/>Veri diskleri dinamik diskler olabilir
Docker disk yapılandırması | Hayır
Ana Bilgisayar NFS | VMware için evet<br/><br/> Fiziksel sunucular için hayır
Ana bilgisayar SAN (iSCSI/FC) | Evet
Ev Sahibi vSAN | VMware için evet<br/><br/> Fiziksel sunucular için N/A
Ana bilgisayar çok (MPIO) | Evet, CLARiiON için Microsoft DSM, EMC PowerPath 5.7 SP4, EMC PowerPath DSM ile test edilmiştir
Ana Bilgisayar Sanal Hacimleri (VVols) | VMware için evet<br/><br/> Fiziksel sunucular için N/A
Konuk/sunucu VMDK | Evet
Konuk/sunucu paylaşılan küme diski | Hayır
Konuk/sunucu şifreli disk | Hayır
Konuk/sunucu NFS | Hayır
Konuk/sunucu iSCSI | Geçiş İçin - Evet<br/>Olağanüstü Durum Kurtarma için - Hayır, iSCSI VM'ye bağlı bir disk olarak geri
Konuk/sunucu Kobİ 3.0 | Hayır
Konuk/sunucu RDM | Evet<br/><br/> Fiziksel sunucular için N/A
Konuk/sunucu diski > 1 TB | Evet, disk 1024 MB'dan büyük olmalıdır<br/><br/>Yönetilen disklere çoğaltırken 8.192 GB'a kadar (9,26 sürümü<br></br> Depolama hesaplarına çoğalırken 4.095 GB'a kadar
4K mantıksal ve 4k fiziksel sektör boyutuna sahip konuk/sunucu diski | Hayır
4K mantıksal ve 512 bayt lık fiziksel sektör boyutuna sahip konuk/sunucu diski | Hayır
Çizgili diskli konuk/sunucu hacmi >4 TB <br/><br/>Mantıksal hacim yönetimi (LVM)| Evet
Konuk/sunucu - Depolama Alanları | Hayır
Konuk/sunucu sıcak ekle/kaldır diski | Hayır
Konuk/sunucu - disk hariç | Evet
Konuk/sunucu çok (MPIO) | Hayır
Konuk/sunucu GPT bölümleri | [Güncelleme Rollup 37'den](https://support.microsoft.com/help/4508614/) (Mobilite hizmetinin 9.25 sürümü) itibaren beş bölüm desteklenir. Daha önce dört desteklendi.
ReFS | Esnek Dosya Sistemi Mobilite hizmet sürümü 9.23 veya daha yüksek ile desteklenir
Konuk/sunucu EFI/UEFI önyükleme | - Windows Server 2012 veya sonrası için desteklenen, SLES 12 SP4 ve RHEL 8.0 mobilite aracısı sürümü 9.30'dan itibaren<br/> - Güvenli UEFI önyükleme türü desteklenmez.

## <a name="replication-channels"></a>Çoğaltma kanalları

|**Çoğaltma türü**   |**Desteklenen**  |
|---------|---------|
|Boşaltılmış Veri Aktarımları (ODX)    |       Hayır  |
|Çevrimdışı Tohumlama        |   Hayır      |
| Azure Data Box | Hayır

## <a name="azure-storage"></a>Azure depolama alanı

**Bileşen** | **Desteklenen**
--- | ---
Yerel olarak yedekli depolama | Evet
Coğrafi olarak yedekli depolama | Evet
Okuma erişimli coğrafi olarak yedekli depolama | Evet
Serin depolama | Hayır
Sıcak depolama| Hayır
Blok blobları | Hayır
Şifreleme-at-rest (SSE)| Evet
Şifreleme-at-rest (CMK)| Evet (Powershell Az 3.3.0 modülü üzerinden itibaren)
Premium depolama | Evet
İthalat/ihracat hizmeti | Hayır
VNets için Azure Depolama güvenlik duvarları | Evet.<br/> Hedef depolama/önbellek depolama hesabında (çoğaltma verilerini depolamak için kullanılır) yapılandırılır.
Genel amaçlı v2 depolama hesapları (sıcak ve serin katmanlar) | Evet (İşlem maliyetleri V1 ile karşılaştırıldığında V2 için önemli ölçüde daha yüksektir)

## <a name="azure-compute"></a>Azure işlem

**Özellik** | **Desteklenen**
--- | ---
Kullanılabilirlik kümeleri | Evet
Kullanılabilirlik alanları | Hayır
Hub | Evet
Yönetilen diskler | Evet

## <a name="azure-vm-requirements"></a>Azure VM gereksinimleri

Azure'da çoğaltılan şirket içi VM'ler, bu tabloda özetlenen Azure VM gereksinimlerini karşılamalıdır. Site Kurtarma çoğaltma için önkoşullar denetimi çalıştırdığında, bazı gereksinimler karşılanmazsa denetim başarısız olur.

**Bileşen** | **Gereksinimler** | **Şey**
--- | --- | ---
Konuk işletim sistemi | Çoğaltılan makineler için [desteklenen işletim sistemlerini](#replicated-machines) doğrulayın. | Desteklenmezse denetim başarısız olur.
Konuk işletim sistemi mimarisi | 64 bit. | Desteklenmezse denetim başarısız olur.
İşletim sistemi disk boyutu | 2.048 GB'a kadar. | Desteklenmezse denetim başarısız olur.
İşletim sistemi disk sayısı | 1 | Desteklenmezse denetim başarısız olur.
Veri diski sayısı | 64 ya da daha az. | Desteklenmezse denetim başarısız olur.
Veri diskboyutu | Yönetilen diske çoğalırken 8.192 GB'a kadar (9,26 sürüm itibaren)<br></br>Depolama hesabına çoğaltma yaparken 4.095 GB'a kadar| Desteklenmezse denetim başarısız olur.
Ağ bağdaştırıcıları | Birden çok bağdaştırıcı desteklenir. |
Paylaşılan VHD | Desteklenmiyor. | Desteklenmezse denetim başarısız olur.
FC diski | Desteklenmiyor. | Desteklenmezse denetim başarısız olur.
BitLocker | Desteklenmiyor. | Bir makineiçin çoğaltmayı etkinleştirmeden önce BitLocker devre dışı edilmelidir. |
VM adı | 1 ile 63 arasında karakter.<br/><br/> Harfler, sayılar ve kısa çizgilerden oluşabilir.<br/><br/> Makine adı bir harf veya sayı ile başlayıp bitmelidir. |  Site Kurtarma'daki makine özelliklerindeki değeri güncelleştirin.

## <a name="resource-group-limits"></a>Kaynak grubu sınırları

Tek bir kaynak grubu altında korunabilen sanal makinelerin sayısını anlamak için [abonelik sınırları ve kotalarla](/azure/azure-resource-manager/management/azure-subscription-service-limits#resource-group-limits)ilgili makaleye bakın.

## <a name="churn-limits"></a>Çalkalama sınırları

Aşağıdaki tablo, Azure Site Recovery sınırlarını sağlar.
- Bu sınırlar testlerimize dayanmaktadır, ancak olası tüm uygulama G/Ç kombinasyonlarını kapsamaz.
- Gerçek sonuçlar, uygulamanızın G/Ç karışımına göre değişebilir.
- En iyi sonuçlar için, [Dağıtım Planlayıcısı aracını](site-recovery-deployment-planner.md)çalıştırmanızı ve uygulamanız için gerçek performans resmini elde etmek için test hatalarını kullanarak kapsamlı uygulama testi yapmanızı şiddetle öneririz.

**Çoğaltma hedefi** | **Ortalama kaynak disk G/Ç boyutu** |**Ortalama kaynak disk veri değişim sıklığı** | **Günde toplam kaynak disk veri karmaşası**
---|---|---|---
Standart depolama | 8 KB    | 2 MB/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 8 KB    | 2 MB/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 16 KB | 4 MB/sn |    Disk başına 336 GB
Premium P10 veya P15 disk | 32 KB veya daha büyük | 8 MB/sn | Disk başına 672 GB
Premium P20 veya P30 veya P40 veya P50 disk | 8 KB    | 5 MB/sn | Disk başına 421 GB
Premium P20 veya P30 veya P40 veya P50 disk | 16 KB veya daha büyük |20 MB/sN | Disk başına 1684 GB


**Kaynak veri değişim sıklığı** | **Üst Sınır**
---|---
VM üzerindeki tüm disklerde en yüksek veri değişim sıklığı | 54 MB/sn
İşlem Sunucusu tarafından desteklenen günlük en fazla veri değişim sıklığı | 2 TB

- Bunlar yüzde 30 G/Ç çakışmasını varsayan ortalama sayılardır.
- Site Recovery; çakışma oranı, büyük yazma boyutları ve gerçek iş yükü G/Ç davranışına göre daha yüksek aktarım hızını işleyebilir.
- Bu sayılar, yaklaşık beş dakikalık tipik bir biriktirme listesi varsayar. Diğer bir deyişle, veriler karşıya yüklendikten sonra işlenir ve beş dakika içinde kurtarma noktası oluşturulur.

## <a name="vault-tasks"></a>Kasa görevleri

**Eylem** | **Desteklenen**
--- | ---
Kaynak grupları arasında kasa taşıma | Hayır
Vault'u abonelikler içinde ve genelinde taşıma | Hayır
Depolama, ağ, Azure VM'lerini kaynak gruplarına taşıma | Hayır
Depolama, ağ, Azure VM'leri abonelikler içinde ve arasında taşıyın. | Hayır


## <a name="obtain-latest-components"></a>En son bileşenleri edinin

**Adı** | **Açıklama** | **Şey**
--- | --- | ---
Yapılandırma Sunucusu | Şirket içinde yüklenmiş.<br/> Şirket içi VMware sunucuları veya fiziksel makineler ile Azure arasındaki iletişimi koordine eder. | - Yapılandırma sunucusu [hakkında bilgi edinin.](vmware-physical-azure-config-process-server-overview.md)<br/> - En son sürüme yükseltme [hakkında bilgi edinin.](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server)<br/> - Yapılandırma sunucusu ayarlama [hakkında bilgi edinin.](vmware-azure-deploy-configuration-server.md)
İşlem sunucusu | Varsayılan olarak yapılandırma sunucusuna yüklenir.<br/> Çoğaltma verilerini alır, önbelleğe alma, sıkıştırma ve şifreleme ile en iyi duruma getirin ve Azure'a gönderir.<br/> Dağıtımınız büyüdükçe, daha büyük birim çoğaltma trafiğini işlemek için ek işlem sunucuları ekleyebilirsiniz. | - İşlem sunucusu [hakkında bilgi edinin.](vmware-physical-azure-config-process-server-overview.md)<br/> - En son sürüme yükseltme [hakkında bilgi edinin.](vmware-azure-manage-process-server.md#upgrade-a-process-server)<br/> - Ölçeklendirme işlem sunucuları ayarlama [hakkında bilgi edinin.](vmware-physical-large-deployment.md#set-up-a-process-server)
Ulaşım Hizmeti | Çoğaltmak istediğiniz VMware VM veya fiziksel sunucularda yüklenir.<br/> Şirket içi VMware sunucuları/fiziksel sunucular ve Azure arasındaki çoğaltmayı koordine eder.| - Mobilite hizmeti [hakkında bilgi edinin.](vmware-physical-mobility-service-overview.md)<br/> - En son sürüme yükseltme [hakkında bilgi edinin.](vmware-physical-manage-mobility-service.md#update-mobility-service-from-azure-portal)<br/>



## <a name="next-steps"></a>Sonraki adımlar
Azure'u VMware VM'lerin olağanüstü kurtarma sına nasıl hazırlayacağınızı [öğrenin.](tutorial-prepare-azure.md)

[9.32 UR]: https://support.microsoft.com/en-in/help/4538187/update-rollup-44-for-azure-site-recovery
[9.31 UR]: https://support.microsoft.com/en-in/help/4537047/update-rollup-43-for-azure-site-recovery
[9.30 UR]: https://support.microsoft.com/en-in/help/4531426/update-rollup-42-for-azure-site-recovery
[9.29 UR]: https://support.microsoft.com/en-in/help/4528026/update-rollup-41-for-azure-site-recovery
[9.28 UR]: https://support.microsoft.com/en-in/help/4521530/update-rollup-40-for-azure-site-recovery
[9.27 UR]: https://support.microsoft.com/en-in/help/4517283/update-rollup-39-for-azure-site-recovery
[9.26 UR]: https://support.microsoft.com/en-in/help/4513507/update-rollup-38-for-azure-site-recovery
[9.25 UR]: https://support.microsoft.com/en-in/help/4508614/update-rollup-37-for-azure-site-recovery
[9.24 UR]: https://support.microsoft.com/en-in/help/4503156
[9.23 UR]: https://support.microsoft.com/en-in/help/4494485/update-rollup-35-for-azure-site-recovery
[9.22 UR]: https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery
[9.21 UR]: https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery
[9.20 UR]: https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery
[9.19 UR]: https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30
[9.18 UR]: https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery
