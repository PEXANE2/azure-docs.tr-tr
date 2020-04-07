---
title: Azure Site Kurtarma ile Azure VM olağanüstü durum kurtarma için destek matrisi
description: Azure Site Kurtarma ile ikincil bir bölgeye Azure VM'ler olağanüstü durum kurtarma desteğini özetler.
ms.topic: article
ms.date: 01/10/2020
ms.author: raynew
ms.openlocfilehash: f61f32ddc0a1cc6575907bc72522228b77552947
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673800"
---
# <a name="support-matrix-for-azure-vm-disaster-recovery-between-azure-regions"></a>Azure bölgeleri arasında Azure VM olağanüstü durum kurtarma için destek matrisi

Bu makalede, [Azure Site Kurtarma](site-recovery-overview.md) hizmetini kullanarak Azure VM'lerinin bir Azure bölgesinden diğerine olağanüstü kurtarma desteği ve ön koşulları özetlenmiştir.


## <a name="deployment-method-support"></a>Dağıtım yöntemi desteği

**Dağıtım** |  **Destek**
--- | ---
**Azure portal** | Destekleniyor.
**PowerShell** | Destekleniyor. [Daha fazlasını öğrenin](azure-to-azure-powershell.md)
**REST API** | Destekleniyor.
**CLI** | Şu anda desteklenmiyor


## <a name="resource-support"></a>Kaynak desteği

**Kaynak eylemi** | **Şey**
--- | ---
**Kaynak grupları arasında kasaları taşıma** | Desteklenmiyor
**Bilgi işlem/depolama/ağ kaynaklarını kaynak grupları arasında taşıma** | Desteklenmiyor.<br/><br/> VM çoğaltıldıktan sonra bir VM veya depolama/ağ gibi ilişkili bileşenleri taşırsanız, VM için çoğaltmayı devre dışı bırakmanız ve ardından çoğaltmayı yeniden etkinleştirmeniz gerekir.
**Olağanüstü durum kurtarma için Azure VM'lerini bir abonelikten diğerine çoğaltma** | Aynı Azure Etkin Dizin kiracısı içinde desteklenir.
**VM'leri desteklenen coğrafi kümeler içindeki bölgeler e göre (abonelikler içinde ve bunlar arasında) geçirme** | Aynı Azure Etkin Dizin kiracısı içinde desteklenir.
**VM'leri aynı bölgeye geçirme** | Desteklenmiyor.

## <a name="region-support"></a>Bölge desteği

Aynı coğrafi küme içindeki herhangi iki bölge arasında VM'leri çoğaltabilir ve kurtarabilirsiniz. Coğrafi kümeler veri gecikmesi ve egemenliği göz önünde bulundurarak tanımlanır.


**Coğrafi küme** | **Azure bölgeleri**
-- | --
Amerika | Kanada Doğu, Kanada Merkez, Güney Orta ABD, Batı Orta ABD, Doğu ABD, Doğu ABD 2, Batı ABD, Batı ABD 2, Orta ABD, Kuzey Orta ABD
Avrupa | İngiltere Batı, İngiltere Güney, Kuzey Avrupa, Batı Avrupa, Güney Afrika Batı, Güney Afrika Kuzey, Norveç Doğu, Norveç Batı
Asya | Güney Hindistan, Orta Hindistan, Batı Hindistan, Güneydoğu Asya, Doğu Asya, Japonya Doğu, Japonya Batı, Kore Orta, Kore Güney
Avustralya    | Avustralya Doğu, Avustralya Güneydoğu, Avustralya Merkez, Avustralya Merkez 2
Azure Kamu    | ABD GOV Virginia, ABD GOV Iowa, ABD GOV Arizona, ABD GOV Texas, ABD DOD Doğu, ABD DOD Merkez
Almanya    | Almanya Merkez, Almanya Kuzeydoğu
Çin | Çin Doğu, Çin Kuzey, Çin North2, Çin Doğu2
Ülke içi felaket kurtarma için ayrılmış kısıtlı bölgeler |Almanya Kuzey Almanya West Central, İsviçre Batı İsviçre Kuzey, Fransa Güney için ayrılmış Fransa Merkez, BAE Merkez BAE Kuzey müşterileri için sınırlı saklıdır

>[!NOTE]
>
> - **Brezilya Güney**için, çoğaltmak ve bu bölgeler üzerinde başarısız: Güney Orta ABD, Batı Orta ABD, Doğu ABD, Doğu ABD 2, Batı ABD, Batı ABD 2 ve Kuzey Orta ABD.
> - Brezilya Güney sadece VM'ler Site Kurtarma kullanarak çoğaltmak hangi bir kaynak bölge olarak kullanılabilir. Hedef bölge olarak hareket edemez. Bunun nedeni coğrafi mesafeler nedeniyle gecikme sorunlarıdır. Bir hedefe kaynak bölge olarak Brezilya Güney üzerinde başarısız olursa, hedef bölgeden Brezilya Güney failback desteklenir unutmayın.
> - Uygun erişime sahip olduğunuz bölgelerde çalışabilirsiniz.
> - Kasa oluşturmak istediğiniz bölge görünmüyorsa, aboneliğinizin o bölgedeki kaynakları oluşturmak için erişimi olduğundan emin olun.
> - Çoğaltmayı etkinleştirdiğinizde coğrafi küme içindeki bir bölgeyi göremiyorsanız, aboneliğinizin o bölgede VM oluşturma izinlerine sahip olduğundan emin olun.



## <a name="cache-storage"></a>Önbellek depolama

Bu tablo, çoğaltma sırasında Site Kurtarma tarafından kullanılan önbellek depolama hesabı için destek özetler.

**Ayar** | **Destek** | **Şey**
--- | --- | ---
Genel amaçlı V2 depolama hesapları (Sıcak ve Cool katmanı) | Destekleniyor | V2'nin işlem maliyetleri V1 depolama hesaplarından önemli ölçüde yüksek olduğundan GPv2 kullanımı önerilmez.
Premium depolama | Desteklenmiyor | Maliyetleri optimize etmeye yardımcı olmak için önbellek depolamaiçin standart depolama hesapları kullanılır.
Sanal ağlar için Azure Depolama güvenlik duvarları  | Destekleniyor | Güvenlik duvarı etkin önbellek depolama hesabı veya hedef depolama hesabı kullanıyorsanız, ['Güvenilir Microsoft hizmetlerine izin verin'](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions)emin olun.<br></br>Ayrıca, kaynak Vnet'in en az bir alt ağına erişime izin verdiğinden emin olun.


## <a name="replicated-machine-operating-systems"></a>Çoğaltılmış makine işletim sistemleri

Site Kurtarma, bu bölümde listelenen işletim sistemlerini çalıştıran Azure VM'lerinin çoğaltılamasını destekler. Zaten çoğaltma işlemi yapılan bir makine daha sonra farklı bir ana çekirdeğin üzerine yükseltilirse (veya indirgenmişse), çoğaltmayı devre dışı bırakmanız ve yükseltmeden sonra çoğaltmayı yeniden etkinleştirmeniz gerektiğini lütfen unutmayın.

### <a name="windows"></a>Windows


**İşletim sistemi** | **Şey**
--- | ---
Windows Server 2019 | Server Core, Desktop Experience ile Server için desteklenir.
Windows Server 2016  | Desteklenen Server Core, Masaüstü Deneyimi ile Sunucu.
Windows Server 2012 R2 | Destekleniyor.
Windows Server 2012 | Destekleniyor.
Windows Server 2008 SP1/SP2 ile R2 | Destekleniyor.<br/><br/> Azure VM'ler için Mobilite hizmet uzantısı sürüm [9.30'dan](https://support.microsoft.com/en-us/help/4531426/update-rollup-42-for-azure-site-recovery) Itibaren, Windows Server 2008 R2 SP1/SP2 çalıştıran makinelere Windows [servis yığını güncelleştirmesi (SSU)](https://support.microsoft.com/help/4490628) ve [SHA-2 güncelleştirmesi](https://support.microsoft.com/help/4474419) yüklemeniz gerekir.  SHA-1 Eylül 2019'dan itibaren desteklenmez ve SHA-2 kod imzalama etkin değilse aracı uzantısı beklendiği gibi yüklenmez/yükseltmeyapmaz. [SHA-2 yükseltmesi ve gereksinimleri](https://aka.ms/SHA-2KB)hakkında daha fazla bilgi edinin.
Windows 10 (x64) | Destekleniyor.
Windows 8.1 (x64) | Destekleniyor.
Windows 8 (x64) | Destekleniyor.
Windows 7 (x64) SP1 ile ileriye doğru | Azure VM'ler için Mobilite hizmet uzantısı sürüm [9.30'dan,](https://support.microsoft.com/en-us/help/4531426/update-rollup-42-for-azure-site-recovery) Windows 7'yi SP1 ile çalıştıran makinelere Windows [servis yığını güncelleştirmesi (SSU)](https://support.microsoft.com/help/4490628) ve [SHA-2 güncelleştirmesi](https://support.microsoft.com/help/4474419) yüklemeniz gerekir.  SHA-1 Eylül 2019'dan itibaren desteklenmez ve SHA-2 kod imzalama etkin değilse aracı uzantısı beklendiği gibi yüklenmez/yükseltmez... [SHA-2 yükseltmesi ve gereksinimleri](https://aka.ms/SHA-2KB)hakkında daha fazla bilgi edinin.



#### <a name="linux"></a>Linux

**İşletim sistemi** | **Şey**
--- | ---
Red Hat Enterprise Linux | 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6,[7.7](https://support.microsoft.com/help/4528026/update-rollup-41-for-azure-site-recovery), [8.0](https://support.microsoft.com/en-us/help/4531426/update-rollup-42-for-azure-site-recovery), 8.1
CentOS | 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6, 7.7, 8.0, 8.1
Ubuntu 14.04 LTS Sunucu | [Desteklenen çekirdek sürümleri](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
Ubuntu 16.04 LTS Sunucu | [Desteklenen çekirdek sürümü](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)<br/><br/> Parola tabanlı kimlik doğrulama ve oturum açma ve oturum açma ve bulut VM'lerini yapılandırmak için bulut girişi paketi kullanan Ubuntu sunucuları, başarısız olduğunda (cloudinit yapılandırmasına bağlı olarak) parola tabanlı oturum açma devre dışı bırakılmış olabilir. Parola tabanlı oturum açma, Destek > Sorun Giderme > Ayarları menüsünden (Azure portalındaki VM üzerinden başarısız olanın) parolasını sıfırlayarak sanal makinede yeniden etkinleştirilebilir.
Ubuntu 18.04 LTS Sunucu | [Desteklenen çekirdek sürümü](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
Debian 7 | [Desteklenen çekirdek sürümleri](#supported-debian-kernel-versions-for-azure-virtual-machines)
Debian 8 | [Desteklenen çekirdek sürümleri](#supported-debian-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 12 | SP1, SP2, SP3, SP4. [(Desteklenen çekirdek sürümleri)](#supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines)
SUSE Linux Kurumsal Sunucu 15 | 15 ve 15 SP1. [(Desteklenen çekirdek sürümleri)](#supported-suse-linux-enterprise-server-15-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 11 | SP3<br/><br/> Çoğaltma makinelerinin SP3'ten SP4'e yükseltimi desteklenmez. Çoğaltılan bir makine yükseltildiyse, çoğaltmayı devre dışı bırakıp yükseltmeden sonra çoğaltmayı yeniden etkinleştirmeniz gerekir.
SUSE Linux Enterprise Server 11 | SP4
Oracle Linux | 6.4, 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6, [7.7](https://support.microsoft.com/en-us/help/4531426/update-rollup-42-for-azure-site-recovery) <br/><br/> Red Hat uyumlu çekirdek veya Kırılmaz Enterprise Çekirdek Yayın 3, 4 & 5 (UEK3, UEK4, UEK5)


#### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Azure sanal makineleri için desteklenen Ubuntu çekirdek sürümleri

**Yayınla** | **Mobilite hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
14.04 LTS | 9.32| 3.13.0-24-jenerik için 3.13.0-170-jenerik,<br/>3.16.0-25-jenerik için 3.16.0-77-jenerik,<br/>3.19.0-18-jenerik için 3.19.0-80-jenerik,<br/>4.2.0-18-jenerik için 4.2.0-42-jenerik,<br/>4.4.0-21-jenerik için 4.4.0-148-jenerik,<br/>4.15.0-1023-azure için 4.15.0-1045-azure |
14.04 LTS | 9.31 | 3.13.0-24-jenerik için 3.13.0-170-jenerik,<br/>3.16.0-25-jenerik için 3.16.0-77-jenerik,<br/>3.19.0-18-jenerik için 3.19.0-80-jenerik,<br/>4.2.0-18-jenerik için 4.2.0-42-jenerik,<br/>4.4.0-21-jenerik için 4.4.0-148-jenerik,<br/>4.15.0-1023-azure için 4.15.0-1045-azure |
14.04 LTS | 9.30 | 3.13.0-24-jenerik için 3.13.0-170-jenerik,<br/>3.16.0-25-jenerik için 3.16.0-77-jenerik,<br/>3.19.0-18-jenerik için 3.19.0-80-jenerik,<br/>4.2.0-18-jenerik için 4.2.0-42-jenerik,<br/>4.4.0-21-jenerik için 4.4.0-148-jenerik,<br/>4.15.0-1023-azure için 4.15.0-1045-azure |
14.04 LTS | 9.29 | 3.13.0-24-jenerik için 3.13.0-170-jenerik,<br/>3.16.0-25-jenerik için 3.16.0-77-jenerik,<br/>3.19.0-18-jenerik için 3.19.0-80-jenerik,<br/>4.2.0-18-jenerik için 4.2.0-42-jenerik,<br/>4.4.0-21-jenerik için 4.4.0-148-jenerik,<br/>4.15.0-1023-azure için 4.15.0-1045-azure |
|||
16.04 LTS | 9.32 | 4.4.0-21-jenerik için 4.4.0-171-jenerik,<br/>4.8.0-34-jenerik için 4.8.0-58-jenerik,<br/>4.10.0-14-jenerik için 4.10.0-42-jenerik,<br/>4.11.0-13-jenerik için 4.11.0-14-jenerik,<br/>4.13.0-16-jenerik için 4.13.0-45-jenerik,<br/>4.15.0-13-jenerik için 4.15.0-74-jenerik<br/>4.11.0-1009-azure için 4.11.0-1016-azure,<br/>4.13.0-1005-azure için 4.13.0-1018-azure <br/>4.15.0-1012-azure için 4.15.0-1066-azure|
16.04 LTS | 9.31 | 4.4.0-21-jenerik için 4.4.0-170-jenerik,<br/>4.8.0-34-jenerik için 4.8.0-58-jenerik,<br/>4.10.0-14-jenerik için 4.10.0-42-jenerik,<br/>4.11.0-13-jenerik için 4.11.0-14-jenerik,<br/>4.13.0-16-jenerik için 4.13.0-45-jenerik,<br/>4.15.0-13-jenerik için 4.15.0-72-jenerik<br/>4.11.0-1009-azure için 4.11.0-1016-azure,<br/>4.13.0-1005-azure için 4.13.0-1018-azure <br/>4.15.0-1012-azure için 4.15.0-1063-azure|
16.04 LTS | [9.30](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery) | 4.4.0-21-jenerik için 4.4.0-166-jenerik,<br/>4.8.0-34-jenerik için 4.8.0-58-jenerik,<br/>4.10.0-14-jenerik için 4.10.0-42-jenerik,<br/>4.11.0-13-jenerik için 4.11.0-14-jenerik,<br/>4.13.0-16-jenerik için 4.13.0-45-jenerik,<br/>4.15.0-13-jenerik için 4.15.0-66-jenerik<br/>4.11.0-1009-azure için 4.11.0-1016-azure,<br/>4.13.0-1005-azure için 4.13.0-1018-azure <br/>4.15.0-1012-azure için 4.15.0-1061-azure|
16.04 LTS | 9.29 | 4.4.0-21-jenerik için 4.4.0-164-jenerik,<br/>4.8.0-34-jenerik için 4.8.0-58-jenerik,<br/>4.10.0-14-jenerik için 4.10.0-42-jenerik,<br/>4.11.0-13-jenerik için 4.11.0-14-jenerik,<br/>4.13.0-16-jenerik için 4.13.0-45-jenerik,<br/>4.15.0-13-jenerik için 4.15.0-64-jenerik<br/>4.11.0-1009-azure için 4.11.0-1016-azure,<br/>4.13.0-1005-azure için 4.13.0-1018-azure <br/>4.15.0-1012-azure için 4.15.0-1059-azure|
|||
18.04 LTS | 9.32| 4.15.0-20-jenerik için 4.15.0-74-jenerik </br> 4.18.0-13-jenerik için 4.18.0-25-jenerik </br> 5.0.0-15-jenerik için 5.0.0-37-jenerik </br> 5.3.0-19-jenerik için 5.3.0-24-jenerik </br> 4.15.0-1009-azure için 4.15.0-1037-azure </br> 4.18.0-1006-azure için 4.18.0-1025-azure </br> 5.0.0-1012-azure için 5.0.0-1028-azure </br> 5.3.0-1007-azure için 5.3.0-1009-azure|
18.04 LTS | 9.31| 4.15.0-20-jenerik için 4.15.0-72-jenerik </br> 4.18.0-13-jenerik için 4.18.0-25-jenerik </br> 5.0.0-15-jenerik için 5.0.0-37-jenerik </br> 5.3.0-19-jenerik için 5.3.0-24-jenerik </br> 4.15.0-1009-azure için 4.15.0-1037-azure </br> 4.18.0-1006-azure için 4.18.0-1025-azure </br> 5.0.0-1012-azure için 5.0.0-1025-azure </br> 5.3.0-1007-azure|
18.04 LTS | [9.30](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery) | 4.15.0-20-jenerik için 4.15.0-66-jenerik </br> 4.18.0-13-jenerik için 4.18.0-25-jenerik </br> 5.0.0-15-genel 5.0.0-32-jenerik </br> 4.15.0-1009-azure için 4.15.0-1037-azure </br> 4.18.0-1006-azure için 4.18.0-1025-azure </br> 5.0.0-1012-azure için 5.0.0-1023-azure|
18.04 LTS | [9.29](https://support.microsoft.com/help/4528026/update-rollup-41-for-azure-site-recovery) | 4.15.0-20-jenerik için 4.15.0-64-jenerik </br> 4.18.0-13-jenerik için 4.18.0-25-jenerik </br> 5.0.0-15-genel için 5.0.0-29-jenerik </br> 4.15.0-1009-azure için 4.15.0-1037-azure </br> 4.18.0-1006-azure için 4.18.0-1025-azure </br> 5.0.0-1012-azure için 5.0.0-1020-azure|


#### <a name="supported-debian-kernel-versions-for-azure-virtual-machines"></a>Azure sanal makineleri için desteklenen Debian çekirdek sürümleri

**Yayınla** | **Mobilite hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
Debian 7 | 9.28,9.29,9.30,9.31 | 3.2.0-4-amd64 için 3.2.0-6-amd64, 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | 9.29,9.30,9.31 | 3.16.0-4-amd64 ila 3.16.0-10-amd64, 4.9.0-0.bpo.4-amd64 ila 4.9.0-0.bpo.11-amd64 |
Debian 8 | 9.28 | 3.16.0-4-amd64 ila 3.16.0-10-amd64, 4.9.0-0.bpo.4-amd64 ila 4.9.0-0.bpo.9-amd64 |

#### <a name="supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines"></a>Azure sanal makineleri için desteklenen SUSE Linux Enterprise Server 12 çekirdek sürümleri

**Yayınla** | **Mobilite hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
SUSE Linux Enterprise Server 12 (SP1,SP2,SP3,SP4) | 9.32 | Tüm [stok SUSE 12 SP1,SP2,SP3, SP4 çekirdekleri](https://wiki.microfocus.com/index.php/SUSE/SLES/Kernel_versions#SUSE_Linux_Enterprise_Server_12) desteklenir.</br></br> 4.4.138-4.7-azure için 4.4.180-4.31-azure,</br>4.12.14-6.3-azure için 4.12.14-6.34-azure  |
SUSE Linux Enterprise Server 12 (SP1,SP2,SP3,SP4) | 9.31 | Tüm [stok SUSE 12 SP1,SP2,SP3, SP4 çekirdekleri](https://wiki.microfocus.com/index.php/SUSE/SLES/Kernel_versions#SUSE_Linux_Enterprise_Server_12) desteklenir.</br></br> 4.4.138-4.7-azure için 4.4.180-4.31-azure,</br>4.12.14-6.3-azure için 4.12.14-6.29-azure  |
SUSE Linux Enterprise Server 12 (SP1,SP2,SP3,SP4) | 9.30 | Tüm [stok SUSE 12 SP1,SP2,SP3, SP4 çekirdekleri](https://wiki.microfocus.com/index.php/SUSE/SLES/Kernel_versions#SUSE_Linux_Enterprise_Server_12) desteklenir.</br></br> 4.4.138-4.7-azure için 4.4.180-4.31-azure,</br>4.12.14-6.3-azure için 4.12.14-6.29-azure  |
SUSE Linux Enterprise Server 12 (SP1,SP2,SP3,SP4) | 9.29 | Tüm [stok SUSE 12 SP1,SP2,SP3, SP4 çekirdekleri](https://wiki.microfocus.com/index.php/SUSE/SLES/Kernel_versions#SUSE_Linux_Enterprise_Server_12) desteklenir.</br></br> 4.4.138-4.7-azure için 4.4.180-4.31-azure,</br>4.12.14-6.3-azure için 4.12.14-6.23-azure  |

#### <a name="supported-suse-linux-enterprise-server-15-kernel-versions-for-azure-virtual-machines"></a>Azure sanal makineleri için desteklenen SUSE Linux Enterprise Server 15 çekirdek sürümleri

**Yayınla** | **Mobilite hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
SUSE Linux Enterprise Server 15 ve 15 SP1 | 9.32 | Tüm [stok SUSE 15 ve 15 çekirdekleri](https://wiki.microfocus.com/index.php/SUSE/SLES/Kernel_versions#SUSE_Linux_Enterprise_Server_15) desteklenir.</br></br> 4.12.14-5.5-azure için 4.12.14-8.22-azure |

## <a name="replicated-machines---linux-file-systemguest-storage"></a>Çoğaltılmış makineler - Linux dosya sistemi/konuk depolama

* Dosya sistemleri: ext3, ext4, XFS, BTRFS
* Birim yöneticisi: LVM2
* Multipath yazılımı: Device Mapper


## <a name="replicated-machines---compute-settings"></a>Çoğaltılmış makineler - bilgi işlem ayarları

**Ayar** | **Destek** | **Şey**
--- | --- | ---
Boyut | En az 2 CPU çekirdeği ve 1 GB RAM ile herhangi bir Azure VM boyutu | [Azure sanal makine boyutlarını](../virtual-machines/windows/sizes.md)doğrulayın.
Kullanılabilirlik kümeleri | Destekleniyor | Varsayılan seçeneklerle bir Azure VM için çoğaltmayı etkinleştiriseniz, kaynak bölge ayarlarına bağlı olarak otomatik olarak bir kullanılabilirlik kümesi oluşturulur. Bu ayarları değiştirebilirsiniz.
Kullanılabilirlik alanları | Destekleniyor |
Hibrit Kullanım Avantajı (HUB) | Destekleniyor | Kaynak VM'de HUB lisansı etkinleştirilmişse, VM üzerinde bir test başarısız veya başarısız da HUB lisansını kullanır.
Sanal makine ölçek kümeleri | Desteklenmiyor |
Azure galeri resimleri - Microsoft yayınlandı | Destekleniyor | VM desteklenen bir işletim sistemi üzerinde çalışıyorsa desteklenir.
Azure Galerisi resimleri - Üçüncü taraf yayınlandı | Destekleniyor | VM desteklenen bir işletim sistemi üzerinde çalışıyorsa desteklenir.
Özel görüntüler - Üçüncü taraf yayınlandı | Destekleniyor | VM desteklenen bir işletim sistemi üzerinde çalışıyorsa desteklenir.
Site Kurtarma kullanarak geçirilen VM'ler | Destekleniyor | Bir VMware VM veya fiziksel makine Site Kurtarma'yı kullanarak Azure'a geçirildiyse, makinede çalışan Mobilite hizmetinin eski sürümünü kaldırmanız ve başka bir Azure bölgesine çoğaltmadan önce makineyi yeniden başlatmanız gerekir.
RBAC ilkeleri | Desteklenmiyor | VM'lerde rol tabanlı Erişim denetimi (RBAC) ilkeleri hedef bölgedeki başarısız VM'ye çoğaltılmaz.
Uzantıları | Desteklenmiyor | Uzantılar hedef bölgede başarısız VM çoğaltılamaz. Bu failover sonra el ile yüklenmesi gerekir.

## <a name="replicated-machines---disk-actions"></a>Çoğaltılan makineler - disk eylemleri

**Eylem** | **Şey**
-- | ---
Çoğaltılan VM'deki diski yeniden boyutlandırma | Başarısız olmadan önce kaynak VM'de desteklenir. Çoğaltmayı devre dışı kaldırmaya/yeniden etkinleştirmeye gerek yok.<br/><br/> Başarısız olduktan sonra kaynak VM'yi değiştirirseniz, değişiklikler yakalanmaz.<br/><br/> Başarısız olduktan sonra Azure VM'deki disk boyutunu değiştirirseniz, değişiklikler Site Kurtarma tarafından yakalanmaz ve geri dönüş orijinal VM boyutuna gelir.
Yinelenen VM'ye disk ekleme | Destekleniyor

## <a name="replicated-machines---storage"></a>Çoğaltılmış makineler - depolama

Bu tablo, Azure VM İşletim Sistemi diski, veri diski ve geçici disk desteği özetlenmiştir.

- Herhangi bir performans sorunu önlemek için [Linux](../virtual-machines/linux/disk-scalability-targets.md) ve [Windows](../virtual-machines/windows/disk-scalability-targets.md) VM'ler için VM disk sınırlarını ve hedeflerini gözlemlemek önemlidir.
- Varsayılan ayarlarla dağıtıyorsanız, Site Kurtarma kaynak ayarlarına göre otomatik olarak diskler ve depolama hesapları oluşturur.
- Özelleştirirseniz, yönergelere uyduğunuzdan emin olun.

**Bileşen** | **Destek** | **Şey**
--- | --- | ---
İşletim sistemi disk maksimum boyutu | 2048 GB | VM diskler hakkında [daha fazla bilgi edinin.](../virtual-machines/windows/managed-disks-overview.md)
Geçici disk | Desteklenmiyor | Geçici disk her zaman çoğaltma dışında tutulur.<br/><br/> Geçici diskte kalıcı veri depolamayın. [Daha fazla bilgi edinin](../virtual-machines/windows/managed-disks-overview.md).
Veri diski maksimum boyutu | Yönetilen diskler için 8192 GB<br></br>Yönetilmeyen diskler için 4095 GB|
Veri diski minimum boyutu | Yönetilmeyen diskler için kısıtlama yok. Yönetilen diskler için 2 GB |
Veri diski maksimum sayı | Belirli bir Azure VM boyutu desteğine uygun olarak 64'e kadar | VM boyutları hakkında [daha fazla bilgi edinin.](../virtual-machines/windows/sizes.md)
Veri diski değişim hızı | Premium depolama için disk başına maksimum 10 MBps. Standart depolama için disk başına maksimum 2 MBps. | Diskteki ortalama veri değişim hızı sürekli olarak maksimumdan yüksekse, çoğaltma yetişmez.<br/><br/>  Ancak, maksimum ara sıra aşılırsa, çoğaltma yetişebilir, ancak biraz gecikmiş kurtarma noktaları görebilirsiniz.
Veri diski - standart depolama hesabı | Destekleniyor |
Veri diski - premium depolama hesabı | Destekleniyor | Bir VM'de premium ve standart depolama hesaplarına yayılmış diskler varsa, hedef bölgede aynı depolama yapılandırmasına sahip olmak için her disk için farklı bir hedef depolama hesabı seçebilirsiniz.
Yönetilen disk - standart | Azure Site Kurtarma'nın desteklendiği Azure bölgelerinde desteklenir. |
Yönetilen disk - premium | Azure Site Kurtarma'nın desteklendiği Azure bölgelerinde desteklenir. |
Standart SSD | Destekleniyor |
Yedeklilik | LRS ve GRS desteklenir.<br/><br/> ZRS desteklenmiyor.
Serin ve sıcak depolama | Desteklenmiyor | VM diskler serin ve sıcak depolama da desteklenmez
Depolama Alanları | Destekleniyor |
Şifreleme istirahatte (SSE) | Destekleniyor | SSE, depolama hesaplarında varsayılan ayardır.
Istirahatte şifreleme (CMK) | Destekleniyor | Hem Yazılım hem de HSM tuşları yönetilen diskler için desteklenir
Windows OS için Azure Disk Şifreleme (ADE) | Yönetilen disklerle VM'ler için desteklenir. | Yönetilmeyen diskleri kullanan VM'ler desteklenmez. <br/><br/> HSM korumalı anahtarlar desteklenmez. |
Linux işletim sistemi için Azure Disk Şifreleme (ADE) | Yönetilen disklerle VM'ler için desteklenir. | Yönetilmeyen diskleri kullanan VM'ler desteklenmez. <br/><br/> HSM korumalı anahtarlar desteklenmez. |
Sıcak ekle    | Destekleniyor | Yinelenen azure VM'ye eklediğiniz bir veri diski için çoğaltmayı etkinleştirme, yönetilen diskleri kullanan VM'ler için desteklenir.
Sıcak kaldırma diski    | Desteklenmiyor | VM'deki veri diskini kaldırırsanız, çoğaltmayı devre dışı bırakıp VM için çoğaltmayı yeniden etkinleştirmeniz gerekir.
Diski hariç tutma | Destek. Yapılandırmak için [PowerShell'i](azure-to-azure-exclude-disks.md) kullanmanız gerekir. |    Geçici diskler varsayılan olarak dışlanır.
Doğrudan Depolama Alanları  | Çarpışma tutarlı kurtarma noktaları için desteklenir. Uygulama tutarlı kurtarma noktaları desteklenmez. |
Dosya Sunucusu'nun Ölçeklendirmesi  | Çarpışma tutarlı kurtarma noktaları için desteklenir. Uygulama tutarlı kurtarma noktaları desteklenmez. |
DRBD | DRBD kurulumunun parçası olan diskler desteklenmez. |
LRS | Destekleniyor |
GRS | Destekleniyor |
RA-GRS | Destekleniyor |
ZRS | Desteklenmiyor |
Serin ve Sıcak Depolama | Desteklenmiyor | Sanal makine diskleri serin ve sıcak depolama da desteklenmez
Sanal ağlar için Azure Depolama güvenlik duvarları  | Destekleniyor | Sanal ağ erişimini depolama hesaplarına kısıtlamak istiyorsanız, [güvenilen Microsoft hizmetlerine İzin ver'i](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions)etkinleştirin.
Genel amaçlı V2 depolama hesapları (Hem Sıcak hem de Cool katmanı) | Destekleniyor | İşlem maliyetleri Genel amaçlı V1 depolama hesaplarına göre önemli ölçüde artar
Nesil 2 (UEFI önyükleme) | Destekleniyor

>[!IMPORTANT]
> Performans sorunlarını önlemek için, [Linux](../virtual-machines/linux/disk-scalability-targets.md) veya [Windows](../virtual-machines/windows/disk-scalability-targets.md) VM'ler için VM disk ölçeklenebilirliği ve performans hedeflerini izlediğinden emin olun. Varsayılan ayarları kullanırsanız, Site Kurtarma kaynak yapılandırmasına bağlı olarak gerekli diskleri ve depolama hesaplarını oluşturur. Kendi ayarlarınızı özelleştirip seçerseniz, kaynak VM'leriniz için disk ölçeklenebilirliği ve performans hedeflerini izleyin.

## <a name="limits-and-data-change-rates"></a>Limitler ve veri değiştirme oranları

Aşağıdaki tablo, Site Kurtarma sınırlarını özetleyin.

- Bu sınırlar bizim testlere dayanmaktadır, ancak belli ki tüm olası uygulama G/Ç kombinasyonlarını kapsamamaktadır.
- Gerçek sonuçlar, uygulama G/Ç karışımına bağlı olarak değişebilir.
- Disk veri karmaşası başına ve sanal makine veri karmaşası başına göz önünde bulundurulması gereken iki sınır vardır.

**Depolama hedefi** | **Ortalama kaynak disk I/O** |**Ortalama kaynak disk veri değişim sıklığı** | **Günde toplam kaynak disk veri karmaşası**
---|---|---|---
Standart depolama | 8 KB    | 2 MB/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 8 KB    | 2 MB/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 16 KB | 4 MB/sn |    Disk başına 336 GB
Premium P10 veya P15 disk | 32 KB veya daha büyük | 8 MB/sn | Disk başına 672 GB
Premium P20 veya P30 veya P40 veya P50 disk | 8 KB    | 5 MB/sn | Disk başına 421 GB
Premium P20 veya P30 veya P40 veya P50 disk | 16 KB veya daha büyük |20 MB/sN | Disk başına 1684 GB

## <a name="replicated-machines---networking"></a>Çoğaltılmış makineler - ağ
**Ayar** | **Destek** | **Şey**
--- | --- | ---
NIC | Belirli bir Azure VM boyutu için desteklenen maksimum sayı | NİC'ler, başarısızlık sırasında VM oluşturulduğunda oluşturulur.<br/><br/> Başarısız VM'deki NIC sayısı, çoğaltma etkinleştirildiğinde kaynak VM'deki NIC sayısına bağlıdır. Çoğaltmayı etkinleştirdikten sonra bir NIC ekler veya kaldırırsanız, başarısız olduktan sonra çoğaltılan VM'deki NIC sayısını etkilemez. Ayrıca, başarısız olduktan sonra NIC'lerin sırasının orijinal siparişle aynı olduğu garanti edilmez.
İnternet Yük Dengeleyici | Destekleniyor | Önceden yapılandırılmış yük bakiyesini bir kurtarma planında Azure Otomasyon komut dosyası kullanarak ilişkilendirin.
Dahili Yük dengeleyici | Destekleniyor | Önceden yapılandırılmış yük bakiyesini bir kurtarma planında Azure Otomasyon komut dosyası kullanarak ilişkilendirin.
Genel IP adresi | Destekleniyor | Varolan bir genel IP adresini NIC ile ilişkilendirin. Veya, bir kurtarma planında Azure Automation komut dosyası kullanarak herkese açık bir IP adresi oluşturun ve NIC ile ilişkilendirin.
Nic üzerinde NSG | Destekleniyor | Kurtarma planında Azure Otomasyon komut dosyası kullanarak NSG'yi NIC ile ilişkilendirin.
NSG alt ağ üzerinde | Destekleniyor | Kurtarma planında Bir Azure Otomasyon komut dosyası kullanarak NSG'yi alt ağla ilişkilendirin.
Ayrılmış (statik) IP adresi | Destekleniyor | Kaynak VM'deki NIC statik bir IP adresine sahipse ve hedef alt ağ da aynı IP adresine sahipse, VM üzerinden başarısız olana atanır.<br/><br/> Hedef alt ağda aynı IP adresi yoksa, alt ağdaki kullanılabilir IP adreslerinden biri VM için ayrılmıştır.<br/><br/> **Ayrıca Çoğaltılmış öğeler** > **Ayarlar** > **Bilgi İşlem ve Ağ** > **Ağ arabirimlerinde**sabit bir IP adresi ve alt net belirtebilirsiniz.
Dinamik IP adresi | Destekleniyor | Kaynaktaki NIC'de dinamik IP adresi varsa, VM üzerinden başarısız olan NIC de varsayılan olarak dinamiktir.<br/><br/> Gerekirse bunu sabit bir IP adresiolarak değiştirebilirsiniz.
Birden çok IP adresi | Desteklenmiyor | Birden çok IP adresi olan bir NIC'e sahip bir VM üzerinde başarısız olduğunuzda, yalnızca kaynak bölgedeki NIC'nin birincil IP adresi tutulur. Birden çok IP adresi atamak için, [kurtarma planına](recovery-plan-overview.md) VM'ler ekleyebilir ve plana ek IP adresleri atamak için bir komut dosyası ekleyebilirsiniz veya değişikliği el ile veya başarısız olduktan sonra komut dosyasıyla yapabilirsiniz.
Traffic Manager     | Destekleniyor | Trafik Yöneticisi'ni, trafiğin kaynak bölgedeki bitiş noktasına düzenli olarak ve başarısız olması durumunda hedef bölgedeki bitiş noktasına yönlendirilen şekilde önceden yapılandırabilirsiniz.
Azure DNS | Destekleniyor |
Özel DNS    | Destekleniyor |
Kimlik doğrulaması olmayan proxy | Destekleniyor | [Daha fazlasını öğrenin](site-recovery-azure-to-azure-networking-guidance.md)
Kimlik doğrulaması Proxy | Desteklenmiyor | VM giden bağlantı için kimlik doğrulaması verilen bir proxy kullanıyorsa, Azure Site Kurtarma kullanılarak çoğaltılamaz.
Şirket içi VPN siteden siteye bağlantı<br/><br/>(ExpressRoute ile veya ExpressRoute olmadan)| Destekleniyor | ÜD'ler ve NSG'ler Site Kurtarma trafiği şirket içinde yönlendirilmeyecek şekilde yapılandırıldığından emin olun. [Daha fazlasını öğrenin](site-recovery-azure-to-azure-networking-guidance.md)
VNET - VNET bağlantısı    | Destekleniyor | [Daha fazlasını öğrenin](site-recovery-azure-to-azure-networking-guidance.md)
Sanal Ağ Hizmet Uç Noktaları | Destekleniyor | Sanal ağ erişimini depolama hesaplarına bağlıyorsanız, güvenilen Microsoft hizmetlerinin depolama hesabına erişmesine izin verildiğinden emin olun.
Hızlandırılmış ağ iletişimi | Destekleniyor | Kaynak VM'de hızlandırılmış ağ etkinleştirilmelidir. [Daha fazla bilgi edinin](azure-vm-disaster-recovery-with-accelerated-networking.md).



## <a name="next-steps"></a>Sonraki adımlar
- Azure VM'lerini çoğaltmak için [ağ kılavuzunu](site-recovery-azure-to-azure-networking-guidance.md) okuyun.
- [Azure VM'leri çoğaltarak](site-recovery-azure-to-azure.md)olağanüstü durum kurtarma dağıtma.
