---
title: Azure Site Recovery ile Azure VM olağanüstü durum kurtarma için destek matrisi
description: Azure Site Recovery olan ikincil bir bölgeye Azure VM olağanüstü durum kurtarma desteğini özetler.
ms.topic: article
ms.date: 01/10/2020
ms.author: raynew
ms.openlocfilehash: 73160a6bf416722021d76da21a32a1cd1ee04386
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82111734"
---
# <a name="support-matrix-for-azure-vm-disaster-recovery-between-azure-regions"></a>Azure bölgeleri arasında Azure VM olağanüstü durum kurtarma için destek matrisi

Bu makalede, Azure VM 'lerinin bir Azure bölgesinden diğerine olağanüstü durum kurtarmaya yönelik destek ve Önkoşullar [Azure Site Recovery](site-recovery-overview.md) hizmeti kullanılarak özetlenmektedir.


## <a name="deployment-method-support"></a>Dağıtım yöntemi desteği

**Dağıtım** |  **Destek**
--- | ---
**Azure portal** | Destekleniyor.
**PowerShell** | Destekleniyor. [Daha fazlasını öğrenin](azure-to-azure-powershell.md)
**REST API** | Destekleniyor.
**CLI** | Şu anda desteklenmiyor


## <a name="resource-support"></a>Kaynak desteği

**Kaynak eylemi** | **Bilgileri**
--- | ---
**Kaynakları kaynak grupları arasında taşıma** | Desteklenmiyor
**İşlem/depolama/ağ kaynaklarını kaynak grupları arasında taşıma** | Desteklenmiyor.<br/><br/> VM çoğaltıldıktan sonra bir VM 'yi veya depolama/ağ gibi ilişkili bileşenleri taşırsanız VM için çoğaltmayı devre dışı bırakıp yeniden etkinleştirmeniz gerekir.
**Olağanüstü durum kurtarma için Azure VM 'lerini bir abonelikten diğerine çoğaltma** | Aynı Azure Active Directory kiracısı içinde desteklenir.
**Desteklenen coğrafi kümeler içindeki bölgeler arasında VM 'Leri geçirme (ve abonelikler arasında)** | Aynı Azure Active Directory kiracısı içinde desteklenir.
**Aynı bölgedeki VM 'Leri geçirme** | Desteklenmiyor.

## <a name="region-support"></a>Bölge desteği

Aynı coğrafi küme içindeki iki bölge arasında VM 'Leri çoğaltıp kurtarabilirsiniz. Coğrafi kümeler, veri gecikmesi ve egemenlik göz önünde tutulması için tanımlanır.


**Coğrafi küme** | **Azure bölgeleri**
-- | --
Amerika | Kanada Doğu, Kanada Orta, Orta Güney ABD, Orta Batı ABD, Doğu ABD, Doğu ABD 2, Batı ABD, Batı ABD 2, Orta ABD, Orta Kuzey ABD
Avrupa | UK Batı, UK Güney, Kuzey Avrupa, Batı Avrupa, Güney Afrika Batı, Güney Afrika Kuzey, Norveç Doğu, Norveç Batı
Asya | Güney Hindistan, Orta Hindistan, Batı Hindistan, Güneydoğu Asya, Doğu Asya, Japonya Doğu, Japonya Batı, Kore Orta, Kore Güney
Avustralya    | Avustralya Doğu, Avustralya Güneydoğu, Avustralya Orta, Avustralya Orta 2
Azure Kamu    | ABD devleti Virginia, US GOV IWA, US GOV Arizona, US GOV Texas, US DOD Doğu, US DOD orta
Almanya    | Almanya Orta, Almanya Kuzeydoğu
Çin | Çin Doğu, Çin Kuzey, Çin North2, Çin Doğu2
Ülke içi olağanüstü durum kurtarma için ayrılan kısıtlı bölgeler |Almanya Orta Batı için ayrılan Almanya Kuzey, Fransa Güney için ayrılan Fransa orta BAE Orta müşterileri için sınırlı İsviçre Kuzey İsviçre Batı

>[!NOTE]
>
> - **Brezilya Güney**için, şu bölgelere çoğaltabilir ve yük devredebilmeniz gerekir: Orta Güney ABD, Orta Batı ABD, Doğu ABD, Doğu ABD 2, Batı ABD, Batı ABD 2 ve Orta Kuzey ABD.
> - Brezilya Güney yalnızca, Site Recovery kullanılarak sanal makinelerin çoğaltılacağı kaynak bölge olarak kullanılabilir. Hedef bölge işlevi görebilir. Bu, coğrafi uzaklıkları nedeniyle gecikme sorunlarından kaynaklanır. Bir hedefe kaynak bölge olarak Brezilya Güney yük devretmek, hedef bölgeden Brezilya Güney için yeniden çalışma desteklendiğini unutmayın.
> - Uygun erişiminiz olan bölgeler içinde çalışabilirsiniz.
> - Bir kasa oluşturmak istediğiniz bölge görünmüyorsa, aboneliğinizin bu bölgedeki kaynakları oluşturmak için erişime sahip olduğundan emin olun.
> - Çoğaltmayı etkinleştirdiğinizde coğrafi bir küme içinde bölge göremiyorsanız, aboneliğinizin bu bölgede VM oluşturma izinlerine sahip olduğundan emin olun.



## <a name="cache-storage"></a>Önbellek depolama

Bu tablo, çoğaltma sırasında Site Recovery tarafından kullanılan önbellek depolama hesabı desteğini özetler.

**Ayar** | **Destek** | **Bilgileri**
--- | --- | ---
Genel amaçlı v2 depolama hesapları (sık erişimli ve Seyrek Erişimli Katman) | Destekleniyor | V2 için işlem maliyetleri, v1 depolama hesaplarından önemli ölçüde yüksek olduğundan, GPv2 kullanımı önerilmez.
Premium depolama | Desteklenmiyor | Standart depolama hesapları, maliyetleri iyileştirmenize yardımcı olmak için önbellek depolaması için kullanılır.
Sanal ağlar için Azure Storage güvenlik duvarları  | Destekleniyor | Güvenlik Duvarı etkin önbellek depolama hesabı veya hedef depolama hesabı kullanıyorsanız, [' güvenilen Microsoft hizmetlerine Izin ver ' '](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions)i doğrulayın.<br></br>Ayrıca, kaynak VNET 'in en az bir alt ağına erişime izin verdiğinizden emin olun.


## <a name="replicated-machine-operating-systems"></a>Çoğaltılan makine işletim sistemleri

Site Recovery, bu bölümde listelenen işletim sistemlerini çalıştıran Azure VM 'lerinin çoğaltılmasını destekler. Zaten çoğaltılan bir makine daha sonra farklı bir ana çekirdeğe yükseltildiyse (veya indirgenirse), çoğaltmayı devre dışı bırakmanız ve yükseltmeden sonra çoğaltmayı yeniden etkinleştirmeniz gerektiğini unutmayın.

### <a name="windows"></a>Windows


**İşletim sistemi** | **Bilgileri**
--- | ---
Windows Server 2019 | Sunucu çekirdeği için desteklenir, masaüstü deneyimi olan sunucu.
Windows Server 2016  | Desteklenen sunucu çekirdeği, masaüstü deneyimi ile sunucu.
Windows Server 2012 R2 | Destekleniyor.
Windows Server 2012 | Destekleniyor.
SP1/SP2 ile Windows Server 2008 R2 | Destekleniyor.<br/><br/> Azure VM 'Leri için Mobility hizmeti uzantısının [9,30](https://support.microsoft.com/en-us/help/4531426/update-rollup-42-for-azure-site-recovery) sürümünden windows Server 2008 R2 SP1/SP2 çalıştıran makinelere bir Windows [bakım yığını güncelleştirmesi (SSU)](https://support.microsoft.com/help/4490628) ve [SHA-2 güncelleştirmesi](https://support.microsoft.com/help/4474419) yüklemeniz gerekir.  SHA-1 Eylül 2019 ' den desteklenmez ve SHA-2 kod imzalama etkinleştirilmemişse, aracı uzantısı beklendiği gibi yüklenmez/yükseltilmez. [SHA-2 yükseltme ve gereksinimleri](https://aka.ms/SHA-2KB)hakkında daha fazla bilgi edinin.
Windows 10 (x64) | Destekleniyor.
Windows 8.1 (x64) | Destekleniyor.
Windows 8 (x64) | Destekleniyor.
SP1 ile Windows 7 (x64) sürümleri | Azure VM 'Leri için Mobility hizmeti uzantısının [9,30](https://support.microsoft.com/en-us/help/4531426/update-rollup-42-for-azure-site-recovery) sürümünden WINDOWS 7 SP1 çalıştıran makinelerde bir Windows [bakım yığını güncelleştirmesi (SSU)](https://support.microsoft.com/help/4490628) ve [SHA-2 güncelleştirmesi](https://support.microsoft.com/help/4474419) yüklemeniz gerekir.  SHA-1 Eylül 2019 ' den desteklenmez ve SHA-2 kod imzalama etkinleştirilmemişse, aracı uzantısı beklendiği gibi yüklenmez/yükseltilmez. [SHA-2 yükseltme ve gereksinimleri](https://aka.ms/SHA-2KB)hakkında daha fazla bilgi edinin.



#### <a name="linux"></a>Linux

**İşletim sistemi** | **Bilgileri**
--- | ---
Red Hat Enterprise Linux | 6,7, 6,8, 6,9, 6,10, 7,0, 7,1, 7,2, 7,3, 7,4, 7,5, 7,6,[7,7](https://support.microsoft.com/help/4528026/update-rollup-41-for-azure-site-recovery), [8,0](https://support.microsoft.com/en-us/help/4531426/update-rollup-42-for-azure-site-recovery), 8,1
CentOS | 6,5, 6,6, 6,7, 6,8, 6,9, 6,10, 7,0, 7,1, 7,2, 7,3, 7,4, 7,5, 7,6, 7,7, 8,0, 8,1
Ubuntu 14,04 LTS sunucusu | [Desteklenen çekirdek sürümleri](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
Ubuntu 16,04 LTS sunucusu | [Desteklenen çekirdek sürümü](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)<br/><br/> Parola tabanlı kimlik doğrulaması ve oturum açma kullanan Ubuntu sunucuları ve bulut VM 'Leri yapılandırmak için Cloud-init paketi, yük devretmede (cloudinit yapılandırmasına bağlı olarak) parola tabanlı oturum açma devre dışı kalabilir. Parola tabanlı oturum açma, Azure portal > sorun giderme > ayarları menüsündeki (yük devredilen VM 'nin) parolasını sıfırlayarak sanal makinede yeniden etkinleştirilebilir.
Ubuntu 18,04 LTS sunucusu | [Desteklenen çekirdek sürümü](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
Deyi 7 | [Desteklenen çekirdek sürümleri](#supported-debian-kernel-versions-for-azure-virtual-machines)
Debian 8 | [Desteklenen çekirdek sürümleri](#supported-debian-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 12 | SP1, SP2, SP3, SP4. [(Desteklenen çekirdek sürümleri)](#supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 15 | 15 ve 15 SP1. [(Desteklenen çekirdek sürümleri)](#supported-suse-linux-enterprise-server-15-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 11 | SP3<br/><br/> Makine, SP3 ile SP4'E çoğaltma için yükseltme desteklenmez. Çoğaltılan bir makine yükseltildiyse, çoğaltmayı devre dışı bırakmanız ve yükseltmeden sonra çoğaltmayı yeniden etkinleştirmeniz gerekir.
SUSE Linux Enterprise Server 11 | MADAN
Oracle Linux | 6,4, 6,5, 6,6, 6,7, 6,8, 6,9, 6,10, 7,0, 7,1, 7,2, 7,3, 7,4, 7,5, 7,6, [7,7](https://support.microsoft.com/en-us/help/4531426/update-rollup-42-for-azure-site-recovery) <br/><br/> Red Hat uyumlu çekirdek veya ayırıcı kurumsal çekirdek sürümü 3, 4 & 5 (UEK3, UEK4, UEK5) çalıştırılıyor


#### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Azure sanal makineleri için desteklenen Ubuntu çekirdek sürümleri

**Yayınla** | **Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
14,04 LTS | 9,32| 3.13.0-24-genel-3.13.0-170-Generic,<br/>3.16.0-25-Genel-3.16.0-77-Generic,<br/>3.19.0-18-Generic ila 3.19.0-80-Generic,<br/>4.2.0-18-Generic ila 4.2.0-42-Generic,<br/>4.4.0-21-Generic-4.4.0-148-Generic,<br/>4.15.0-1023-Azure to 4.15.0-1045-Azure |
14,04 LTS | 9,31 | 3.13.0-24-genel-3.13.0-170-Generic,<br/>3.16.0-25-Genel-3.16.0-77-Generic,<br/>3.19.0-18-Generic ila 3.19.0-80-Generic,<br/>4.2.0-18-Generic ila 4.2.0-42-Generic,<br/>4.4.0-21-Generic-4.4.0-148-Generic,<br/>4.15.0-1023-Azure to 4.15.0-1045-Azure |
14,04 LTS | 9,30 | 3.13.0-24-genel-3.13.0-170-Generic,<br/>3.16.0-25-Genel-3.16.0-77-Generic,<br/>3.19.0-18-Generic ila 3.19.0-80-Generic,<br/>4.2.0-18-Generic ila 4.2.0-42-Generic,<br/>4.4.0-21-Generic-4.4.0-148-Generic,<br/>4.15.0-1023-Azure to 4.15.0-1045-Azure |
14,04 LTS | 9,29 | 3.13.0-24-genel-3.13.0-170-Generic,<br/>3.16.0-25-Genel-3.16.0-77-Generic,<br/>3.19.0-18-Generic ila 3.19.0-80-Generic,<br/>4.2.0-18-Generic ila 4.2.0-42-Generic,<br/>4.4.0-21-Generic-4.4.0-148-Generic,<br/>4.15.0-1023-Azure to 4.15.0-1045-Azure |
|||
16,04 LTS | 9,32 | 4.4.0-21-Generic-4.4.0-171-Generic,<br/>4.8.0-34-Generic ile 4.8.0-58-Generic,<br/>4.10.0-14-generic-4.10.0-42-Generic,<br/>4.11.0-13-Genel-4.11.0-14-generic,<br/>4.13.0-16-Generic to 4.13.0-45-Generic,<br/>4.15.0-13-Genel-4.15.0-74-Generic<br/>4.11.0-1009-Azure-4.11.0-1016-Azure,<br/>4.13.0-1005-Azure-4.13.0-1018-Azure <br/>4.15.0-1012-Azure-4.15.0-1066-Azure|
16,04 LTS | 9,31 | 4.4.0-21-Generic-4.4.0-170-Generic,<br/>4.8.0-34-Generic ile 4.8.0-58-Generic,<br/>4.10.0-14-generic-4.10.0-42-Generic,<br/>4.11.0-13-Genel-4.11.0-14-generic,<br/>4.13.0-16-Generic to 4.13.0-45-Generic,<br/>4.15.0-13-Genel-4.15.0-72-Generic<br/>4.11.0-1009-Azure-4.11.0-1016-Azure,<br/>4.13.0-1005-Azure-4.13.0-1018-Azure <br/>4.15.0-1012-Azure-4.15.0-1063-Azure|
16,04 LTS | [9,30](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery) | 4.4.0-21-Generic ile 4.4.0-166-Generic,<br/>4.8.0-34-Generic ile 4.8.0-58-Generic,<br/>4.10.0-14-generic-4.10.0-42-Generic,<br/>4.11.0-13-Genel-4.11.0-14-generic,<br/>4.13.0-16-Generic to 4.13.0-45-Generic,<br/>4.15.0-13-Genel-4.15.0-66-Generic<br/>4.11.0-1009-Azure-4.11.0-1016-Azure,<br/>4.13.0-1005-Azure-4.13.0-1018-Azure <br/>4.15.0-1012-Azure-4.15.0-1061-Azure|
16,04 LTS | 9,29 | 4.4.0-21-Generic-4.4.0-164-Generic,<br/>4.8.0-34-Generic ile 4.8.0-58-Generic,<br/>4.10.0-14-generic-4.10.0-42-Generic,<br/>4.11.0-13-Genel-4.11.0-14-generic,<br/>4.13.0-16-Generic to 4.13.0-45-Generic,<br/>4.15.0-13-Generic-4.15.0-64-Generic<br/>4.11.0-1009-Azure-4.11.0-1016-Azure,<br/>4.13.0-1005-Azure-4.13.0-1018-Azure <br/>4.15.0-1012-Azure-4.15.0-1059-Azure|
|||
18,04 LTS | 9,32| 4.15.0-20-Genel-4.15.0-74-Generic </br> 4.18.0-13-Genel-4.18.0-25-genel </br> 5.0.0-15-genel-5.0.0-37-Generic </br> 5.3.0-19-Generic-5.3.0-24-genel </br> 4.15.0-1009-Azure-4.15.0-1037-Azure </br> 4.18.0-1006-Azure to 4.18.0-1025-Azure </br> 5.0.0-1012-Azure-5.0.0-1028-Azure </br> 5.3.0-1007-Azure-5.3.0-1009-Azure|
18,04 LTS | 9,31| 4.15.0-20-Genel-4.15.0-72-Generic </br> 4.18.0-13-Genel-4.18.0-25-genel </br> 5.0.0-15-genel-5.0.0-37-Generic </br> 5.3.0-19-Generic-5.3.0-24-genel </br> 4.15.0-1009-Azure-4.15.0-1037-Azure </br> 4.18.0-1006-Azure to 4.18.0-1025-Azure </br> 5.0.0-1012-Azure-5.0.0-1025-Azure </br> 5.3.0-1007-Azure|
18,04 LTS | [9,30](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery) | 4.15.0-20-Genel-4.15.0-66-Generic </br> 4.18.0-13-Genel-4.18.0-25-genel </br> 5.0.0-15-genel-5.0.0-32-Generic </br> 4.15.0-1009-Azure-4.15.0-1037-Azure </br> 4.18.0-1006-Azure to 4.18.0-1025-Azure </br> 5.0.0-1012-Azure-5.0.0-1023-Azure|
18,04 LTS | [9,29](https://support.microsoft.com/help/4528026/update-rollup-41-for-azure-site-recovery) | 4.15.0-20-Generic ila 4.15.0-64-Generic </br> 4.18.0-13-Genel-4.18.0-25-genel </br> 5.0.0-15-genel-5.0.0-29-Generic </br> 4.15.0-1009-Azure-4.15.0-1037-Azure </br> 4.18.0-1006-Azure to 4.18.0-1025-Azure </br> 5.0.0-1012-Azure-5.0.0-1020-Azure|


#### <a name="supported-debian-kernel-versions-for-azure-virtual-machines"></a>Azure sanal makineleri için desteklenen de, çekirdek sürümleri

**Yayınla** | **Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
Deyi 7 | 9.28,9.29,9.30,9.31 | 3.2.0-4-AMD64-3.2.0-6-AMD64, 3.16.0 -0. BPO. 4-AMD64 |
|||
Debian 8 | 9.29,9.30,9.31 | 3.16.0-4-AMD64-3.16.0-10-AMD64, 4.9.0 -0. BPO. 4-AMD64 ila 4.9.0 -0. BPO. 11-AMD64 |
Debian 8 | 9,28 | 3.16.0-4-AMD64-3.16.0-10-AMD64, 4.9.0 -0. BPO. 4-AMD64 ila 4.9.0 -0. BPO. 9-AMD64 |

#### <a name="supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines"></a>Azure sanal makineleri için desteklenen SUSE Linux Enterprise Server 12 çekirdek sürümü

**Yayınla** | **Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4) | 9,32 | Tüm hisse senedi SUSE 12 SP1, SP2, SP3, SP4 çekirdekleri desteklenir.</br></br> 4.4.138-4.7-Azure to 4.4.180-4.31-Azure,</br>4.12.14-6.3-Azure to 4.12.14-6.34-Azure  |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4) | 9,31 | Tüm hisse senedi SUSE 12 SP1, SP2, SP3, SP4 çekirdekleri desteklenir.</br></br> 4.4.138-4.7-Azure to 4.4.180-4.31-Azure,</br>4.12.14-6.3-Azure to 4.12.14-6.29-Azure  |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4) | 9,30 | Tüm hisse senedi SUSE 12 SP1, SP2, SP3, SP4 çekirdekleri desteklenir.</br></br> 4.4.138-4.7-Azure to 4.4.180-4.31-Azure,</br>4.12.14-6.3-Azure to 4.12.14-6.29-Azure  |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4) | 9,29 | Tüm hisse senedi SUSE 12 SP1, SP2, SP3, SP4 çekirdekleri desteklenir.</br></br> 4.4.138-4.7-Azure to 4.4.180-4.31-Azure,</br>4.12.14-6.3-Azure to 4.12.14-6.23-Azure  |

#### <a name="supported-suse-linux-enterprise-server-15-kernel-versions-for-azure-virtual-machines"></a>Azure sanal makineleri için desteklenen SUSE Linux Enterprise Server 15 çekirdek sürümü

**Yayınla** | **Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
SUSE Linux Enterprise Server 15 ve 15 SP1 | 9,32 | Tüm hisse senedi SUSE 15 ve 15 çekirdekler desteklenir.</br></br> 4.12.14-5,5-Azure to 4.12.14-8.22-Azure |

## <a name="replicated-machines---linux-file-systemguest-storage"></a>Çoğaltılan makineler-Linux dosya sistemi/Konuk depolama

* Dosya sistemleri: ext3, ext4, XFS, BTRFS
* Birim Yöneticisi: LVM2
* Çok yollu yazılım: cihaz Eşleyici


## <a name="replicated-machines---compute-settings"></a>Çoğaltılan makineler-işlem ayarları

**Ayar** | **Destek** | **Bilgileri**
--- | --- | ---
Boyut | En az 2 CPU çekirdeğine ve 1 GB RAM 'e sahip Azure VM boyutu | [Azure sanal makine boyutlarını](../virtual-machines/windows/sizes.md)doğrulayın.
Kullanılabilirlik kümeleri | Destekleniyor | Varsayılan seçeneklerle bir Azure VM için çoğaltmayı etkinleştirirseniz, kaynak bölge ayarlarına bağlı olarak bir kullanılabilirlik kümesi otomatik olarak oluşturulur. Bu ayarları değiştirebilirsiniz.
Kullanılabilirlik alanları | Destekleniyor |
Karma kullanım teklifi (HUB) | Destekleniyor | Kaynak VM 'nin etkinleştirilmiş bir HUB lisansı varsa, yük devretme testi veya yük devretme VM 'si de HUB lisansı kullanır.
Sanal makine ölçek kümeleri | Desteklenmiyor |
Azure galeri görüntüleri-Microsoft tarafından yayımlandı | Destekleniyor | VM desteklenen bir işletim sisteminde çalışıyorsa desteklenir.
Azure galeri görüntüleri-üçüncü taraf yayımlandı | Destekleniyor | VM desteklenen bir işletim sisteminde çalışıyorsa desteklenir.
Özel görüntüler-üçüncü taraf yayımlandı | Destekleniyor | VM desteklenen bir işletim sisteminde çalışıyorsa desteklenir.
Site Recovery kullanılarak geçirilen VM 'Ler | Destekleniyor | Site Recovery kullanılarak Azure 'a bir VMware VM veya fiziksel makine geçirilirse, makinede çalışan Mobility hizmetinin eski sürümünü kaldırmanız ve makineyi başka bir Azure bölgesine çoğaltmadan önce yeniden başlatmanız gerekir.
RBAC ilkeleri | Desteklenmiyor | VM 'lerde rol tabanlı erişim denetimi (RBAC) ilkeleri hedef bölgedeki yük devretme VM 'sine çoğaltılmaz.
Uzantıları | Desteklenmiyor | Uzantılar, hedef bölgedeki yük devretme VM 'sine çoğaltılmaz. Yük devretmenin ardından el ile yüklenmesi gerekir.

## <a name="replicated-machines---disk-actions"></a>Çoğaltılan makineler-disk eylemleri

**Eylem** | **Bilgileri**
-- | ---
Çoğaltılan VM 'de diski yeniden boyutlandır | Yük devretmeden önce kaynak VM 'de desteklenir. Çoğaltmayı devre dışı bırakıp yeniden etkinleştirmeye gerek yoktur.<br/><br/> Yük devretmeden sonra kaynak VM 'yi değiştirirseniz, değişiklikler yakalanmaz.<br/><br/> Yük devretmeden sonra Azure VM 'de disk boyutunu değiştirirseniz, değişiklikler Site Recovery tarafından yakalanmaz ve yeniden çalışma özgün VM boyutuna göre yapılır.
Çoğaltılan bir VM 'ye disk ekleme | Destekleniyor

## <a name="replicated-machines---storage"></a>Çoğaltılan makineler-depolama

Bu tabloda, Azure VM işletim sistemi diski, veri diski ve geçici disk için destek özetlenmektedir.

- Tüm performans sorunlarını önlemek için, [Linux](../virtual-machines/linux/disk-scalability-targets.md) ve [WINDOWS](../virtual-machines/windows/disk-scalability-targets.md) VM 'lerinin VM disk sınırlarını ve hedeflerini gözlemlemek önemlidir.
- Varsayılan ayarlarla dağıtırsanız, Site Recovery kaynak ayarlarına bağlı olarak diskleri ve depolama hesaplarını otomatik olarak oluşturur.
- ' Yi özelleştirirseniz, yönergeleri izlediğinizden emin olun.

**Bileşen** | **Destek** | **Bilgileri**
--- | --- | ---
İşletim sistemi diski en büyük boyutu | 2048 GB | VM diskleri hakkında [daha fazla bilgi edinin](../virtual-machines/windows/managed-disks-overview.md) .
Geçici disk | Desteklenmiyor | Geçici disk her zaman çoğaltmadan çıkarılır.<br/><br/> Kalıcı verileri geçici diskte depolamamayın. [Daha fazla bilgi edinin](../virtual-machines/windows/managed-disks-overview.md).
Veri diski en büyük boyutu | yönetilen diskler için 8192 GB<br></br>yönetilmeyen diskler için 4095 GB|
Veri diski en küçük boyutu | Yönetilmeyen diskler için kısıtlama yoktur. yönetilen diskler için 2 GB |
Veri diski en fazla sayı | Belirli bir Azure VM boyutu için desteğe uygun olarak 64 'e kadar | VM boyutları hakkında [daha fazla bilgi edinin](../virtual-machines/windows/sizes.md) .
Veri diski değişim oranı | Premium Depolama için disk başına en fazla 10 MBps. Standart depolama için disk başına en fazla 2 MBps. | Disk üzerindeki ortalama veri değişim oranı, en yüksek değerden sürekli yüksek ise çoğaltma işlemi gerçekleştirilmez.<br/><br/>  Ancak, en yüksek değer, tek tek aşılırsa, çoğaltma yakalayabilir, ancak biraz gecikmeli kurtarma noktası görebilirsiniz.
Veri diski-standart depolama hesabı | Destekleniyor |
Veri diski-Premium depolama hesabı | Destekleniyor | Bir VM 'nin Premium ve standart depolama hesapları arasında yayıldığı diskler varsa, hedef bölgede aynı depolama yapılandırmasına sahip olduğunuzdan emin olmak için her bir disk için farklı bir hedef depolama hesabı seçebilirsiniz.
Yönetilen disk-standart | Azure Site Recovery desteklendiği Azure bölgelerinde desteklenir. |
Yönetilen disk-Premium | Azure Site Recovery desteklendiği Azure bölgelerinde desteklenir. |
Standart SSD | Destekleniyor |
Yedeklilik | LRS ve GRS desteklenir.<br/><br/> ZRS desteklenmez.
Seyrek erişimli ve sık erişimli depolama | Desteklenmiyor | Seyrek erişimli ve sık erişimli depolamada VM diskleri desteklenmez
Depolama Alanları | Destekleniyor |
Bekleyen şifreleme (SSE) | Destekleniyor | SSE, depolama hesaplarında varsayılan ayardır.
Bekleyen şifreleme (CMK) | Destekleniyor | Yönetilen diskler için hem yazılım hem de HSM anahtarları desteklenir
Windows işletim sistemi için Azure disk şifrelemesi (ADE) | Yönetilen disklere sahip VM 'Ler için desteklenir. | Yönetilmeyen diskleri kullanan VM 'Ler desteklenmez. <br/><br/> HSM korumalı anahtarlar desteklenmez. |
Linux işletim sistemi için Azure disk şifrelemesi (ADE) | Yönetilen disklere sahip VM 'Ler için desteklenir. | Yönetilmeyen diskleri kullanan VM 'Ler desteklenmez. <br/><br/> HSM korumalı anahtarlar desteklenmez. |
Sık erişimli ekleme    | Destekleniyor | Çoğaltılan bir Azure VM 'ye eklediğiniz bir veri diski için çoğaltmanın etkinleştirilmesi, yönetilen diskleri kullanan VM 'Ler için desteklenir.
Diski sık erişimli kaldır    | Desteklenmiyor | SANAL makinede veri diski kaldırırsanız, çoğaltmayı devre dışı bırakmanız ve VM için çoğaltmayı yeniden etkinleştirmeniz gerekir.
Diski hariç tutma | Support. Yapılandırmak için [PowerShell](azure-to-azure-exclude-disks.md) kullanmanız gerekir. |    Geçici diskler varsayılan olarak dışlanır.
Doğrudan Depolama Alanları  | Kilitlenme ile tutarlı kurtarma noktaları için desteklenir. Uygulamayla tutarlı kurtarma noktaları desteklenmez. |
Genişleme dosya sunucusu  | Kilitlenme ile tutarlı kurtarma noktaları için desteklenir. Uygulamayla tutarlı kurtarma noktaları desteklenmez. |
DRBD | Bir DRBD kurulumunun parçası olan diskler desteklenmez. |
LRS | Destekleniyor |
GRS | Destekleniyor |
RA-GRS | Destekleniyor |
ZRS | Desteklenmiyor |
Seyrek erişimli ve sık erişimli depolama | Desteklenmiyor | Seyrek erişimli ve sık erişimli depolamada sanal makine diskleri desteklenmez
Sanal ağlar için Azure Storage güvenlik duvarları  | Destekleniyor | Depolama hesaplarına sanal ağ erişimini kısıtladıysanız, [Güvenilen Microsoft hizmetlerine Izin vermeyi](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions)etkinleştirin.
Genel amaçlı v2 depolama hesapları (sık erişimli ve Seyrek Erişimli Katman) | Destekleniyor | İşlem maliyetleri, genel amaçlı v1 depolama hesaplarıyla karşılaştırıldığında önemli ölçüde artar
2. nesil (UEFı önyüklemesi) | Destekleniyor

>[!IMPORTANT]
> Performans sorunlarından kaçınmak için, [Linux](../virtual-machines/linux/disk-scalability-targets.md) veya [Windows](../virtual-machines/windows/disk-scalability-targets.md) VM 'leri için VM disk ölçeklenebilirliğini ve performans hedeflerini izlediğinizden emin olun. Varsayılan ayarları kullanıyorsanız, Site Recovery, kaynak yapılandırmasına göre gerekli diskleri ve depolama hesaplarını oluşturur. Kendi ayarlarınızı özelleştirip seçerseniz, kaynak sanal makinelerinize yönelik disk ölçeklenebilirlik ve performans hedeflerini izleyin.

## <a name="limits-and-data-change-rates"></a>Sınırlar ve veri değişim oranları

Aşağıdaki tablo Site Recovery sınırlarını özetler.

- Bu sınırlar, testlerimize dayalıdır, ancak olası tüm uygulama g/ç birleşimlerini kapsamamaktadır.
- Gerçek sonuçlar, uygulama g/ç karışımına göre farklılık gösterebilir.
- Disk başına veri dalgalanması ve sanal makine veri dalgalanması için göz önünde bulundurulması gereken iki sınır vardır.

**Depolama hedefi** | **Ortalama kaynak disk g/ç** |**Ortalama kaynak disk veri değişim sıklığı** | **Gün başına toplam kaynak disk veri değişim sıklığı**
---|---|---|---
Standart depolama | 8 KB    | 2 MB/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 8 KB    | 2 MB/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 16 KB | 4 MB/sn |    Disk başına 336 GB
Premium P10 veya P15 disk | 32 KB veya daha büyük | 8 MB/sn | Disk başına 672 GB
Premium P20 veya P30 veya P40 veya P50 disk | 8 KB    | 5 MB/sn | Disk başına 421 GB
Premium P20 veya P30 veya P40 veya P50 disk | 16 KB veya daha büyük |20 MB/sn | disk başına 1684 GB

## <a name="replicated-machines---networking"></a>Çoğaltılan makineler-ağ
**Ayar** | **Destek** | **Bilgileri**
--- | --- | ---
NIC | Belirli bir Azure VM boyutu için desteklenen en yüksek sayı | NIC 'ler, yük devretme sırasında VM oluşturulduğunda oluşturulur.<br/><br/> Yük devretme sanal makinesinde bulunan NIC 'lerin sayısı, çoğaltma etkinleştirildiğinde kaynak VM üzerindeki NIC 'lerin sayısına bağlıdır. Çoğaltmayı etkinleştirdikten sonra bir NIC ekler veya kaldırırsanız, yük devretmeden sonra çoğaltılan VM 'deki NIC 'lerin sayısını etkilemez. Ayrıca, yük devretme sonrasında NIC sırasının orijinal siparişle aynı olması garanti edilmez.
İnternet Yük Dengeleyici | Destekleniyor | Önceden yapılandırılmış yük dengeleyiciyi bir kurtarma planında bir Azure Otomasyonu betiği kullanarak ilişkilendirin.
İç yük dengeleyici | Destekleniyor | Önceden yapılandırılmış yük dengeleyiciyi bir kurtarma planında bir Azure Otomasyonu betiği kullanarak ilişkilendirin.
Genel IP adresi | Destekleniyor | Mevcut bir genel IP adresini NIC ile ilişkilendirin. Ya da bir genel IP adresi oluşturun ve bir kurtarma planında Azure Otomasyonu betiği kullanarak NIC ile ilişkilendirin.
NIC üzerinde NSG | Destekleniyor | Bir kurtarma planında Azure Otomasyonu betiği kullanarak NSG 'yi NIC ile ilişkilendirin.
Alt ağda NSG | Destekleniyor | Bir kurtarma planında Azure Otomasyonu betiği kullanarak NSG 'yi alt ağ ile ilişkilendirin.
Ayrılmış (statik) IP adresi | Destekleniyor | Kaynak VM üzerindeki NIC 'nin statik bir IP adresi varsa ve hedef alt ağda aynı IP adresi varsa, yük devredilen VM 'ye atanır.<br/><br/> Hedef alt ağa aynı IP adresi yoksa, alt ağdaki kullanılabilir IP adreslerinden biri VM için ayrılmıştır.<br/><br/> Ayrıca, **çoğaltılan öğeler** > **ayarları** > **işlem ve ağ** > **ağı arabirimleri**' nde sabit bir IP adresi ve alt ağ belirtebilirsiniz.
Dinamik IP adresi | Destekleniyor | Kaynaktaki NIC dinamik IP adreslemesini içeriyorsa, yük devredilen VM üzerindeki NIC de varsayılan olarak dinamik olur.<br/><br/> Gerekirse bunu sabit bir IP adresi ile değiştirebilirsiniz.
Birden çok IP adresi | Desteklenmiyor | Birden çok IP adresli NIC 'ye sahip bir VM 'nin yükünü devretmek için yalnızca kaynak bölgedeki NIC 'in birincil IP adresi tutulur. Birden çok IP adresi atamak için, bir [kurtarma planına](recovery-plan-overview.md) VM ekleyebilir ve plana ek IP adresleri atamak için bir komut dosyası ekleyebilir ya da değişikliği yük devretmeden sonra el ile veya bir komut dosyasıyla yapabilirsiniz.
Traffic Manager     | Destekleniyor | Traffic Manager, trafiğin kaynak bölgedeki uç noktaya ve yük devretme durumunda hedef bölgedeki uç noktaya yönlendirilmesi için önceden yapılandırabilirsiniz.
Azure DNS | Destekleniyor |
Özel DNS    | Destekleniyor |
Kimliği doğrulanmamış proxy | Destekleniyor | [Daha fazlasını öğrenin](site-recovery-azure-to-azure-networking-guidance.md)
Kimliği doğrulanmış ara sunucu | Desteklenmiyor | VM, giden bağlantı için kimliği doğrulanmış bir proxy kullanıyorsa, Azure Site Recovery kullanılarak çoğaltılamaz.
Şirket içinde VPN siteden siteye bağlantısı<br/><br/>(ExpressRoute ile veya olmayan)| Destekleniyor | UDRs ve NSG 'Lerin Site Recovery trafiğinin şirket içine yönlendirilmediğinden emin olmak için yapılandırıldığından emin olun. [Daha fazlasını öğrenin](site-recovery-azure-to-azure-networking-guidance.md)
VNET 'ten VNET 'e bağlantı    | Destekleniyor | [Daha fazlasını öğrenin](site-recovery-azure-to-azure-networking-guidance.md)
Sanal Ağ Hizmet Uç Noktaları | Destekleniyor | Depolama hesaplarına sanal ağ erişimini kısıtladığınız takdirde, güvenilen Microsoft hizmetlerinin depolama hesabına erişmesine izin verildiğinden emin olun.
Hızlandırılmış ağ iletişimi | Destekleniyor | Kaynak VM 'de hızlandırılmış ağ etkin olmalıdır. [Daha fazla bilgi edinin](azure-vm-disaster-recovery-with-accelerated-networking.md).



## <a name="next-steps"></a>Sonraki adımlar
- Azure VM 'lerini çoğaltmak için [ağ kılavuzunu](site-recovery-azure-to-azure-networking-guidance.md) okuyun.
- [Azure VM 'lerini çoğaltarak](site-recovery-azure-to-azure.md)olağanüstü durum kurtarma dağıtın.
