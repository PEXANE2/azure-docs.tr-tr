---
title: Azure Geçişi gereci
description: Sunucu değerlendirmesi ve geçişte kullanılan Azure geçişi gerecine genel bakış sağlar.
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 1b1e35c3b7a9d98e57ec4261f6f913c370bbb365
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77597550"
---
# <a name="azure-migrate-appliance"></a>Azure Geçişi gereci

Bu makalede, Azure geçişi gereci açıklanmaktadır. Microsoft Azure 'e geçiş için uygulamaları, altyapıyı ve iş yüklerini bulup değerlendirmek üzere [Azure geçişi: Sunucu değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracı 'nı kullandığınızda gereci dağıtırsınız. Bu gereç Ayrıca, Azure geçişi kullanarak VMware VM 'lerini Azure 'a geçirdiğinizde de kullanılır: [aracısız geçiş](server-migrate-overview.md)Ile [Sunucu değerlendirmesi](migrate-services-overview.md#azure-migrate-server-migration-tool) .

## <a name="appliance-overview"></a>Gereç genel bakışı

Azure geçişi gereci aşağıdaki senaryolarda kullanılır.

**Senaryo** | **Araç** | **Kullanıldığı yer** 
--- | --- | ---
VMware VM | Azure geçişi: Sunucu değerlendirmesi<br/><br/> Azure geçişi: sunucu geçişi | VMware VM 'lerini bulma<br/><br/> Makine uygulamalarını ve bağımlılıklarını bulma<br/><br/> Değerlendirmeler için makine meta verilerini ve performans meta verilerini toplayın.<br/><br/> VMware VM 'lerini aracısız geçişle çoğaltın.
Hyper-V VM | Azure geçişi: Sunucu değerlendirmesi | Hyper-V VM 'lerini bulma<br/><br/> Değerlendirmeler için makine meta verilerini ve performans meta verilerini toplayın.
Fiziksel makine |  Azure geçişi: Sunucu değerlendirmesi |  Fiziksel sunucuları bulma<br/><br/> Değerlendirmeler için makine meta verilerini ve performans meta verilerini toplayın.

## <a name="appliance---vmware"></a>Gereç-VMware 

**Gereksinim** | **VMware** 
--- | ---
**İndirme biçimi** | . OVA 
**Bağlantıyı indirin** | https://aka.ms/migrate/appliance/vmware 
**İndirme boyutu** | 11,2 GB
**Lisan** | İndirilen gereç şablonu, 180 gün için geçerli olan bir Windows Server 2016 değerlendirme lisansıyla birlikte gelir. Değerlendirme süresi sona ermeden yakın ise, yeni bir gereç indirmeniz ve dağıtmanız ya da gereç sanal makinesinin işletim sistemi lisansını etkinleştirmenizi öneririz.
**Dağıtım** | Gereci bir VMware VM 'si olarak dağıtırsınız. VCenter Server, 32 GB RAM, 8 vCPU, 80 GB disk depolama ve harici bir sanal anahtar içeren bir VM ayırmak için yeterli kaynağa ihtiyacınız vardır.<br/> Gereç doğrudan veya bir ara sunucu üzerinden internet erişimi gerektirir.<br/> Gereç tek bir vCenter Server bağlanabilir.
**Donanım** | VCenter 'daki kaynaklar, 32 GB RAM 8 vCPU ile, 80 GB disk depolaması ve harici bir sanal anahtarla bir VM ayırır. 
**Karma değeri** | [Buraya](https://docs.microsoft.com/azure/migrate/tutorial-assess-vmware#verify-security) bakın
**vCenter sunucusu/ana bilgisayar** | Gereç VM 'si, 5,5 veya sonraki bir sürümü çalıştıran bir ESXi konağına dağıtılmalıdır.<br/><br/> 5,5, 6,0, 6,5 veya 6,7 vCenter Server çalışıyor.
**Azure geçişi projesi** | Bir gereç, tek bir projeyle ilişkilendirilebilir. <br/> Herhangi bir sayıda gereç, tek bir projeyle ilişkilendirilebilir.<br/> 
**Keşfini** | Bir gereç, vCenter Server en fazla 10.000 VMware VM 'yi bulabilir.<br/> Bir gereç, tek bir vCenter Server bağlanabilir.
**Gereç bileşenleri** | Yönetim uygulaması: dağıtım sırasında Kullanıcı girişi için gereç 'de Web uygulaması.<br/> Keşif Aracısı: makine yapılandırma verilerini toplar.<br/> Değerlendirme Aracısı: performans verilerini toplayın.<br/> DRA: VM çoğaltmasını düzenleyin ve makineler/Azure arasındaki iletişimi koordine edin.<br/> Ağ geçidi: çoğaltılan verileri Azure 'a gönderir.<br/> Otomatik güncelleştirme hizmeti: güncelleştirme bileşenleri (24 saatte bir çalışır).
**VDDK (aracısız geçiş)** | Azure geçişi sunucu geçişi ile aracısız bir geçiş çalıştırıyorsanız, VMware vSphere VDDK gereci sanal makinesinde yüklü olmalıdır.


## <a name="appliance---hyper-v"></a>Gereç-Hyper-V

**Gereksinim** | **Hyper-V** 
--- | ---
**İndirme biçimi** | Daraltılmış klasör (VHD Ile)
**Bağlantıyı indirin** | https://aka.ms/migrate/appliance/hyperv 
**İndirme boyutu** | 10 GB
**Lisan** | İndirilen gereç şablonu, 180 gün için geçerli olan bir Windows Server 2016 değerlendirme lisansıyla birlikte gelir. Değerlendirme süresi sona ermeden yakın ise, yeni bir gereç indirmeniz ve dağıtmanız ya da gereç sanal makinesinin işletim sistemi lisansını etkinleştirmenizi öneririz.
**Gereç dağıtımı**   |  Gereci bir Hyper-V VM 'si olarak dağıtırsınız.<br/> Azure geçişi tarafından belirtilen gereç sanal makinesi, Hyper-V VM sürüm 5,0 ' dir.<br/> Hyper-V konağı Windows Server 2012 R2 veya üstünü çalıştırmalıdır.<br/> Ana bilgisayar, 16 GB RAM, 8 vCPU, 80 GB depolama alanı etrafında ve gereç VM 'si için bir dış anahtar ayırmak üzere yeterli alana sahip olmalıdır.<br/> Gereç statik veya dinamik bir IP adresi ve internet erişimi gerektirir.
**Donanım** | Hyper-V konağındaki kaynaklar, 16 GB RAM, 8 vCPU, 80 GB depolama alanı etrafında ve gereç VM 'si için bir dış anahtar ayırır.
**Karma değeri** | [Buraya](https://docs.microsoft.com/azure/migrate/tutorial-assess-hyper-v#verify-security) bakın
**Hyper-V konağı** | Windows Server 2012 R2 veya üstünü çalıştırın.
**Azure geçişi projesi** | Bir gereç, tek bir projeyle ilişkilendirilebilir. <br/> Herhangi bir sayıda gereç, tek bir projeyle ilişkilendirilebilir.<br/> 
**Keşfini** | Bir gereç, 5000 adede kadar Hyper-V VM 'Leri bulabilir.<br/> Bir gereç, 300 adede kadar Hyper-V konaklarına bağlanabilir.
**Gereç bileşenleri** | Yönetim uygulaması: dağıtım sırasında Kullanıcı girişi için gereç 'de Web uygulaması.<br/> Keşif Aracısı: makine yapılandırma verilerini toplar.<br/> Değerlendirme Aracısı: performans verilerini toplayın.<br/>  Otomatik güncelleştirme hizmeti: güncelleştirme bileşenleri (24 saatte bir çalışır).


## <a name="appliance---physical"></a>Gereç-fiziksel

**Gereksinim** | **Z** 
--- | ---
**İndirme biçimi** | Daraltılmış klasör (PowerShell tabanlı yükleyici betiği ile)
**Bağlantıyı indirin** | [Bağlantıyı indirin](https://go.microsoft.com/fwlink/?linkid=2105112)
**İndirme boyutu** | 59,7 MB
**Donanım** | Ayrılmış fiziksel makine veya bir sanal makine kullanın. Gereci çalıştıran makinenin 16 GB RAM, 8 vCPU, 80 GB depolama alanı ve harici bir anahtar olması gerekir.<br/> Gereç statik veya dinamik bir IP adresi ve internet erişimi gerektirir.
**Karma değeri** | [Buraya](https://docs.microsoft.com/azure/migrate/tutorial-assess-physical#verify-security) bakın
**İşletim Sistemi** | Gereç makinesi Windows Server 2016 ' i çalıştırıyor olmalıdır. 
**Gereç dağıtımı**   |  Gereç yükleyicisi betiği portaldan (daraltılmış bir klasörde) indirilir. <br/> Klasörü sıkıştırmasını açın ve PowerShell betiğini (AzureMigrateInstaller. ps1) çalıştırın.
**Keşfini** | Bir gereç, en fazla 250 fiziksel sunucu bulabilir.
**Gereç bileşenleri** | Yönetim uygulaması: dağıtım sırasında Kullanıcı girişi için gereç 'de Web uygulaması.<br/> Keşif Aracısı: makine yapılandırma verilerini toplar.<br/> Değerlendirme Aracısı: performans verilerini toplayın.<br/>  Otomatik güncelleştirme hizmeti: güncelleştirme bileşenleri (24 saatte bir çalışır).


## <a name="url-access"></a>URL erişimi

Azure geçişi gereci internet bağlantısı gerektirir.

- Gereci dağıttığınızda Azure geçişi, aşağıdaki tabloda özetlenen URL 'lere bir bağlantı denetimi yapar.
- İnternet 'e bağlanmak için URL tabanlı bir ara sunucu kullanıyorsanız, bu URL 'lere erişim izni verin ve proxy 'nin URL 'Leri ararken alınan CNAME kayıtlarını çözümlediği emin olun.

**URL** | **Ayrıntılar**  
--- | --- |
*.portal.azure.com  | Azure portal gidin.
*.windows.net <br/> *.msftauth.net <br/> *.msauth.net <br/> *. microsoft.com <br/> *. live.com | Azure aboneliğinizde oturum açın.
*.microsoftonline.com <br/> *.microsoftonline-p.com | Gereç için Azure geçişi ile iletişim kurmak üzere Active Directory uygulamalar oluşturun.
management.azure.com | Azure geçişi hizmeti ile iletişim kurmak için gereç için Active Directory uygulamalar oluşturun.
dc.services.visualstudio.com | İç izleme için kullanılan uygulama günlüklerini karşıya yükleyin.
*.vault.azure.net | Azure Key Vault gizli dizileri yönetin.
aka.ms/* | Diğer adıyla bağlantılarına erişime izin ver. Azure geçiş gereci güncellemeleri için kullanılır.
download.microsoft.com/download | Microsoft Download 'ten indirmelere izin ver.
*.servicebus.windows.net | Gereç ve Azure geçişi hizmeti arasındaki iletişim.
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com | Azure geçişi hizmeti URL 'Lerine bağlanın.
*.hypervrecoverymanager.windowsazure.com | **VMware aracısız geçişi için kullanılır**<br/><br/> Azure geçişi hizmeti URL 'Lerine bağlanın.
*.blob.core.windows.net |  **VMware aracısız geçişi için kullanılır**<br/><br/>Geçiş için verileri depolamaya yükleyin.




## <a name="collected-data---vmware"></a>Toplanan veriler-VMware

### <a name="collected-performance-data-vmware"></a>Toplanan performans verileri-VMware

Bu, gerecin topladığı ve Azure 'a gönderdiği VMware VM performans verileri aşağıda verilmiştir.

**Veriler** | **Sayaç** | **Değerlendirme etkisi**
--- | --- | ---
CPU kullanımı | CPU. Usage. Average | Önerilen VM boyutu/maliyet
Bellek kullanımı | mem. kullanım. Ortalama | Önerilen VM boyutu/maliyet
Disk okuma üretilen işi (MB/saniye) | virtualDisk. Read. Average | Disk boyutu, depolama maliyeti, VM boyutu için hesaplama
Disk yazma miktarı (MB/saniye) | virtualDisk. Write. Average | Disk boyutu, depolama maliyeti, VM boyutu için hesaplama
Saniye başına disk okuma işlemi | virtualDisk. Numberreadaveryaşlandırılmış. Average | Disk boyutu, depolama maliyeti, VM boyutu için hesaplama
Saniye başına disk yazma işlemleri | virtualDisk. Numberwriteortalama. Ortalama  | Disk boyutu, depolama maliyeti, VM boyutu için hesaplama
NIC okuma üretilen işi (MB/saniye) | net. alınan. Ortalama | VM boyutu için hesaplama
NIC yazma üretilen işi (MB/saniye) | net. iletilmiş. Average  |VM boyutu için hesaplama


### <a name="collected-metadata-vmware"></a>Toplanan meta veriler-VMware

> [!NOTE]
> Azure geçişi gereci tarafından bulunan meta veriler, uygulamalarınızı Azure 'a geçirirken, Azure uygunluk analizi, uygulama bağımlılığı Analizi ve maliyet planlaması gerçekleştirerek uygulamalarınızı doğru boyuta getirmenize yardımcı olmak için kullanılır. Microsoft bu verileri, herhangi bir lisans uyumluluğu denetimine göre kullanmaz.

Bu, gerecin topladığı ve Azure 'a gönderdiği VMware VM meta verilerinin tam listesini aşağıda bulabilirsiniz.

**Veriler** | **Sayaç**
--- | --- 
**Makine ayrıntıları** | 
VM Kimliği | 'nin. Config. ınstanceuuıd 
VM adı | 'nin. Config.Name
vCenter Server KIMLIĞI | VMwareClient. Instance. UUID
VM açıklaması | 'nin. Summary. config. Annotation
Lisans ürün adı | 'nin. Client. ServiceContent. about. LicenseProductName
İşletim sistemi türü | 'nin. SummaryConfig. GuestFullName
Önyükleme türü | 'nin. Config. bellenim
Çekirdek sayısı | 'nin. Config. Hardware. NumCPU
Bellek (MB) | 'nin. Config. Hardware. MemoryMB
Disk sayısı | 'nin. Config. Hardware. Device. ToList (). FindAll (x = > VirtualDisk). Count
Disk boyutu listesi | 'nin. Config. Hardware. Device. ToList (). FindAll (x = > VirtualDisk)
Ağ bağdaştırıcıları listesi | 'nin. Config. Hardware. Device. ToList (). FindAll (x = > Virtualalethernet). Count
CPU kullanımı | CPU. Usage. Average
Bellek kullanımı |mem. kullanım. Ortalama
**Disk başına Ayrıntılar** | 
Disk anahtarı değeri | dis. Anahtar
Dikunit numarası | dis. UnitNumber
Disk denetleyicisi anahtar değeri | dis. ControllerKey. Value
Gigabayt sağlandı | virtualDisk. DeviceInfo. Summary
Disk adı | Disk kullanılarak oluşturulan değer. UnitNumber, disk. Anahtar, disk. ControllerKey. VAlue
Saniye başına okuma işlemi | virtualDisk. Numberreadaveryaşlandırılmış. Average
Saniye başına yazma işlemi | virtualDisk. Numberwriteortalama. Ortalama
Aktarım hızını oku (MB/saniye) | virtualDisk. Read. Average
Yazma üretilen işi (MB/saniye) | virtualDisk. Write. Average
**NIC başına Ayrıntılar** | 
Ağ bağdaştırıcısı adı | 'i. Anahtar
MAC adresi | ((Virtualalether, NIC) NIC). MacAddress
IPv4 adresleri | 'nin. Guest.Net
IPv6 adresleri | 'nin. Guest.Net
Aktarım hızını oku (MB/saniye) | net. alınan. Ortalama
Yazma üretilen işi (MB/saniye) | net. iletilmiş. Average
**Envanter yolu ayrıntıları** | 
Adı | kapsayıcı. GetType (). Ada
Alt nesnenin türü | kapsayıcı. ChildType
Başvuru ayrıntıları | kapsayıcı. MoRef
Üst Ayrıntılar | Container. Parent
VM başına klasör ayrıntıları | ((Klasör) kapsayıcısı). ChildEntity. Type
VM başına veri merkezi ayrıntıları | (Datacenter) kapsayıcısı). VmFolder
Konak klasörü başına veri merkezi ayrıntıları | (Datacenter) kapsayıcısı). HostFolder
Konak başına küme ayrıntıları | ((ClusterComputeResource) kapsayıcısı). Konağının
VM başına ana bilgisayar ayrıntıları | (HostSystem) kapsayıcısı). 'Nın

## <a name="collected-data---hyper-v"></a>Toplanan veriler-Hyper-V

### <a name="collected-performance-data-hyper-v"></a>Toplanan performans verileri-Hyper-V

> [!NOTE]
> Azure geçişi gereci tarafından bulunan meta veriler, uygulamalarınızı Azure 'a geçirirken, Azure uygunluk analizi, uygulama bağımlılığı Analizi ve maliyet planlaması gerçekleştirerek uygulamalarınızı doğru boyuta getirmenize yardımcı olmak için kullanılır. Microsoft bu verileri, herhangi bir lisans uyumluluğu denetimine göre kullanmaz.

Bu, gerecin topladığı ve Azure 'a gönderdiği Hyper VM performans verileri aşağıda verilmiştir.

**Performans sayacı sınıfı** | **Sayaç** | **Değerlendirme etkisi**
--- | --- | ---
Hyper-V hiper yönetici sanal Işlemcisi | % Konuk çalışma zamanı | Önerilen VM boyutu/maliyet
Hyper-V Dinamik Bellek VM | Geçerli basınç (%)<br/> Konuk görünür fiziksel bellek (MB) | Önerilen VM boyutu/maliyet
Hyper-V sanal depolama cihazı | Okunan bayt/saniye | Disk boyutu, depolama maliyeti, VM boyutu için hesaplama
Hyper-V sanal depolama cihazı | Yazılan bayt/saniye | Disk boyutu, depolama maliyeti, VM boyutu için hesaplama
Hyper-V sanal ağ bağdaştırıcısı | Alınan bayt/saniye | VM boyutu için hesaplama
Hyper-V sanal ağ bağdaştırıcısı | Gönderilen bayt/saniye | VM boyutu için hesaplama

- CPU kullanımı, bir VM 'ye bağlı tüm sanal işlemcilerin toplam kullanım toplamıdır.
- Bellek kullanımı (geçerli basınç * Konuk görünür fiziksel bellek)/100.
- Disk ve ağ kullanım değerleri, listelenen Hyper-V performans sayaçlarından toplanır.

### <a name="collected-metadata-hyper-v"></a>Toplanan meta veriler-Hyper-V

Bu, gerecin topladığı ve Azure 'a gönderdiği Hyper-V VM meta verilerinin tam listesidir.

**Veriler** | **WMI sınıfı** | **WMI sınıfı özelliği**
--- | --- | ---
**Makine ayrıntıları** | 
BIOS _ Msvm_BIOSElement seri numarası | Bıino SerialNumber
VM türü (Gen 1 veya 2) | Msvm_VirtualSystemSettingData | VirtualSystemSubType
VM görünen adı | Msvm_VirtualSystemSettingData | ElementName
VM sürümü | Msvm_ProcessorSettingData | VirtualQuantity
Bellek (bayt) | Msvm_MemorySettingData | VirtualQuantity
VM tarafından tüketilen maksimum bellek | Msvm_MemorySettingData | Sınır
Dinamik bellek etkin | Msvm_MemorySettingData | DynamicMemoryEnabled
İşletim sistemi adı/sürümü/FQDN | Msvm_KvpExchangeComponent | Guestıntrinsicexchangeıtems adı verileri
VM güç durumu | Msvm_ComputerSystem | EnabledState
**Disk başına Ayrıntılar** | 
Disk tanımlayıcısı | Msvm_VirtualHardDiskSettingData | Virtualdiskıd
Sanal sabit disk türü | Msvm_VirtualHardDiskSettingData | Tür
Sanal sabit disk boyutu | Msvm_VirtualHardDiskSettingData | Maxınternalsize
Sanal sabit disk üst öğesi | Msvm_VirtualHardDiskSettingData | ParentPath
**NIC başına Ayrıntılar** | 
IP adresleri (yapay NIC 'ler) | Msvm_GuestNetworkAdapterConfiguration | IpAdresleri
DHCP etkin (yapay NIC 'ler) | Msvm_GuestNetworkAdapterConfiguration | DHCPEnabled
NIC KIMLIĞI (yapay NIC 'ler) | Msvm_SyntheticEthernetPortSettingData | InstanceId
NIC MAC adresi (yapay NIC 'ler) | Msvm_SyntheticEthernetPortSettingData | Adres
NIC KIMLIĞI (eski NIC 'ler) | MsvmEmulatedEthernetPortSetting verileri | InstanceId
NIC MAC KIMLIĞI (eski NIC 'ler) | MsvmEmulatedEthernetPortSetting verileri | Adres




## <a name="discovery-and-collection-process"></a>Bulma ve toplama işlemi

Gereç, aşağıdaki işlemi kullanarak vCenter sunucularıyla ve Hyper-V konaklarıyla/kümesiyle iletişim kurar.

1. **Bulmayı Başlat**:
    - Hyper-V gereci üzerinde bulmayı başlattığınızda, WinRM bağlantı noktaları 5985 (HTTP) ve 5986 (HTTPS) üzerindeki Hyper-V konaklarıyla iletişim kurar.
    - VMware gereci üzerinde bulmayı başlattığınızda, varsayılan olarak TCP bağlantı noktası 443 üzerindeki vCenter Server ile iletişim kurar. VCenter sunucusu farklı bir bağlantı noktasını dinliyorsa, bunu gereç Web uygulamasında yapılandırabilirsiniz.
2. **Meta verileri ve performans verilerini toplayın**:
    - Gereç, 5985 ve 5986 bağlantı noktalarında Hyper-V konağı üzerinden Hyper-V VM verilerini toplamak için bir Genel Bilgi Modeli (CıM) oturumu kullanır.
    - Gereç, vCenter Server VMware VM verilerini toplamak için varsayılan olarak bağlantı noktası 443 ile iletişim kurar.
3. **Veri Gönder**: gereç, toplanan verileri Azure geçişi sunucu değerlendirmesini ve Azure geçişi sunucu geçişini SSL bağlantı noktası 443 üzerinden gönderir. Gereç Internet üzerinden Azure 'a bağlanabilir veya ExpressRoute 'u ortak/Microsoft eşlemesi ile birlikte kullanabilirsiniz.
    - Performans verileri için, Gereç gerçek zamanlı kullanım verilerini toplar.
        - Performans verileri her bir performans ölçümü için VMware için 20 saniyede bir ve Hyper-V için her 30 saniyede bir toplanır.
        - Toplanan veriler 10 dakika boyunca tek bir veri noktası oluşturmak için toplanır.
        - En yüksek kullanım değeri tüm 20/30 saniyelik veri noktalarından seçilir ve değerlendirme hesaplaması için Azure 'a gönderilir.
        - Değerlendirme özelliklerinde belirtilen yüzdebirlik değerine (50./90./yüzde/sn) göre, on dakikalık noktaları artan düzende sıralanır ve değerlendirmeyi hesaplamak için uygun yüzdebirlik değeri kullanılır
    - Sunucu geçişi için, Gereç VM verilerini toplamaya başlar ve bunu Azure 'a çoğaltır.
4. **Değerlendirin ve geçirin**: artık Azure geçişi sunucu değerlendirmesini kullanarak gereç tarafından toplanan meta verilerden değerlendirmeler oluşturabilirsiniz. Ayrıca, Azure geçişi sunucu geçişini kullanarak VMware VM 'Leri geçirmeyi daha az VM çoğaltmasını düzenlemek için de başlatabilirsiniz.


![Mimari](./media/migrate-appliance/architecture.png)


## <a name="appliance-upgrades"></a>Gereç yükseltmeleri

Gereç üzerinde çalışan Azure geçiş aracıları güncelleştirildiğinden, Gereç yükseltilir.

- Otomatik güncelleştirme gereç üzerinde varsayılan olarak etkin olduğundan bu otomatik olarak gerçekleşir.
- Aracıları el ile güncelleştirmek için bu varsayılan ayarı değiştirebilirsiniz.
- Otomatik güncelleştirmeyi devre dışı bırakmak için, HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\AzureAppliance kayıt defteri Düzenleyicisi > gidin ve kayıt defteri anahtarını-"otomatik güncelleştirme", 0 (DWORD) olarak ayarlayın.
 
### <a name="set-agent-updates-to-manual"></a>Aracı güncelleştirmelerini el ile olarak ayarla

El ile güncelleştirmeler için, Gereç üzerindeki tüm olmayan aracıların **Güncelleştir** düğmesini kullanarak, aracıdaki tüm aracıları aynı anda güncelleştirdiğinizden emin olun. Güncelleştirme ayarını dilediğiniz zaman yeniden otomatik güncelleştirmelere geçirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

VMware için gereci ayarlamayı [öğrenin](tutorial-assess-vmware.md#set-up-the-appliance-vm) .
Hyper-V için gereci ayarlamayı [öğrenin](tutorial-assess-hyper-v.md#set-up-the-appliance-vm) .

