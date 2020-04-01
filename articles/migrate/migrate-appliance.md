---
title: Azure Geçişi gereci
description: Sunucu değerlendirme ve geçişte kullanılan Azure Geçiş cihazına genel bir bakış sağlar.
ms.topic: conceptual
ms.date: 03/23/2020
ms.openlocfilehash: bccf4738d46b65f2d149eafc8e69591141d7d073
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437583"
---
# <a name="azure-migrate-appliance"></a>Azure Geçişi gereci

Bu makalede, Azure Geçiş cihazı için ön koşullar ve destek gereksinimleri özetlenmiştir. 

## <a name="deployment-scenarios"></a>Dağıtım senaryoları

Azure Geçir cihazı aşağıdaki senaryolarda kullanılır.

**Senaryo** | **Araç** | **Kullanıldığı yerler** 
--- | --- | ---
**VMware VM değerlendirmesi** | Azure Geçiş:Sunucu Değerlendirmesi | VMware VM'leri keşfedin<br/><br/> Makine uygulamalarını ve bağımlılıklarını keşfedin<br/><br/> Değerlendirmeler için makine meta verilerini ve performans meta verilerini toplayın.
**VMware VM aracısız geçiş** | Azure Geçir:Sunucu Geçişi | VMware VM'leri keşfedin <br/><br/> VMware VM'leri aracısız geçişle çoğaltın.
**Hiper-V VM değerlendirmesi** | Azure Geçiş:Sunucu Değerlendirmesi | Hyper-V VM'leri Keşfedin<br/><br/> Değerlendirmeler için makine meta verilerini ve performans meta verilerini toplayın.
**Fiziksel makine değerlendirmesi** |  Azure Geçiş:Sunucu Değerlendirmesi |  Fiziksel sunucuları (veya fiziksel sunucu olarak değerlendirdiğiniz VM'leri) keşfedin.<br/><br/> Değerlendirmeler için makine meta verilerini ve performans meta verilerini toplayın.

## <a name="appliance---vmware"></a>Cihaz - VMware 

Aşağıdaki tablo, VMware için Azure Geçir cihaz gereksinimlerini özetler.

**Gereksinim** | **VMware** 
--- | ---
**Cihaz bileşenleri** | Cihaz aşağıdaki bileşenlere sahiptir:<br/><br/> - **Yönetim uygulaması**: Cihaz dağıtımı sırasında kullanıcı girişi için kullanılan bir web uygulamasıdır. Azure'a geçiş için makineleri değerlendirirken kullanılır.<br/> - **Bulma aracısı**: Aracı makine yapılandırma verilerini toplar. Azure'a geçiş için makineleri değerlendirirken kullanılır.<br/>- **Değerlendirme aracısı**: Aracı performans verilerini toplar. Azure'a geçiş için makineleri değerlendirirken kullanılır.<br/>- **Otomatik güncelleme hizmeti**: Cihaz bileşenlerini günceller (her 24 saatte bir çalışır).<br/>- **DRA aracısı**: VM çoğaltmayı yönetir ve çoğaltılan makinelerle Azure arasındaki iletişimi koordine eder. Yalnızca Aracısız geçiş kullanarak VMware VM'leri Azure'a kopyalarken kullanılır.<br/>- **Ağ Geçidi**: Çoğaltılan verileri Azure'a gönderir. Yalnızca Aracısız geçiş kullanarak VMware VM'leri Azure'a kopyalarken kullanılır.
**Desteklenen dağıtım** | OVA şablonu kullanarak VMware VM olarak dağıtın.<br/><br/> PowerShell kurulum komut dosyasını kullanarak VMware VM veya fiziksel makine olarak dağıtın.
**Proje desteği** |  Bir cihaz tek bir proje ile ilişkilendirilebilir. <br/> Herhangi bir sayıda cihaz tek bir projeyle ilişkilendirilebilir.<br/> 
**Bulma sınırları** | Bir cihaz vCenter Server'da 10.000 VMware VM'ye kadar keşfedebilir.<br/> Bir cihaz tek bir vCenter Server'a bağlanabilir.
**OVA şablonu** | Portaldan veya https://aka.ms/migrate/appliance/vmware.<br/><br/> İndirme boyutu 11,2 GB'dır.<br/><br/> İndirilen cihaz şablonu, 180 gün boyunca geçerli olan Bir Windows Server 2016 değerlendirme lisansıyla birlikte gelir. Değerlendirme süresi sona ermek üzereyse, yeni bir cihaz indirip dağıtmanızı veya cihaz VM'nin işletim sistemi lisansını etkinleştirmenizi öneririz.
**PowerShell betiği** | Script [indir](https://go.microsoft.com/fwlink/?linkid=2105112).<br/><br/> 
**Yazılım/ donanım** |  Cihaz, Windows Server 2016, 32 GB RAM, 8 vCPUs, yaklaşık 80 GB disk depolama ve harici sanal anahtar ile makinede çalıştırılmalıdır.<br/> Cihaz, doğrudan veya bir proxy üzerinden internet erişimi gerektirir.<br/><br/> Cihazı bir VMware VM üzerinde çalıştırırsanız, vCenter Server'da gereksinimleri karşılayan bir VM ayırmak için yeterli kaynağa ihtiyacınız vardır.<br/><br/> Cihazı fiziksel bir makinede çalıştırıyorsanız, Windows Server 2016'yı çalıştırdığından ve donanım gereksinimlerini karşıladığından emin olun. 
**VMware gereksinimleri** | Cihazı VMware VM olarak dağıtırsanız, 5.5 veya sonraki sürümlerde çalışan bir ESXi ana bilgisayara dağıtılması gerekir.<br/><br/> vCenter Server 5,5, 6,0, 6,5 veya 6,7 çalıştırıyor.
**VDDK (aracısız geçiş)** | Cihazı VMware VM olarak dağıtıyorsanız ve aracısız geçiş çalıştırıyorsanız, VMware vSphere VDDK cihaz VM'ye monte edilmelidir.
**Karma değer-OVA** | OVA şablonu karma değerlerini [doğrulayın.](tutorial-assess-vmware.md#verify-security)
**Karma değer-PowerShell komut dosyası** | PowerShell komut dosyası karma değerlerini [doğrulayın.](deploy-appliance-script.md#verify-file-security)




## <a name="appliance---hyper-v"></a>Cihaz - Hyper-V

**Gereksinim** | **Hyper-V** 
--- | ---
**Cihaz bileşenleri** | Cihaz aşağıdaki bileşenlere sahiptir:<br/><br/>- **Yönetim uygulaması**: Cihaz dağıtımı sırasında kullanıcı girişi için kullanılan bir web uygulamasıdır. Azure'a geçiş için makineleri değerlendirirken kullanılır.<br/> - **Bulma aracısı**: Aracı makine yapılandırma verilerini toplar. Azure'a geçiş için makineleri değerlendirirken kullanılır.<br/>- **Değerlendirme aracısı**: Aracı performans verilerini toplar. Azure'a geçiş için makineleri değerlendirirken kullanılır.<br/>- **Otomatik güncelleme hizmeti**: Cihaz bileşenlerini günceller (her 24 saatte bir çalışır).
**Desteklenen dağıtım** | VHD şablonu kullanarak Hyper-V VM olarak dağıtın.<br/><br/> PowerShell kurulum komut dosyası kullanarak Hyper-V VM veya fiziksel makine olarak dağıtın.
**Proje desteği** |  Bir cihaz tek bir proje ile ilişkilendirilebilir. <br/> Herhangi bir sayıda cihaz tek bir projeyle ilişkilendirilebilir.<br/> 
**Bulma sınırları** | Bir cihaz 5000'e kadar Hyper-V VM keşfedebilir.<br/> Bir cihaz en fazla 300 Hyper-V ana bilgisayara bağlanabilir.
**VHD şablonu** | VHD dahil olmak üzere sıkıştırılmış klasör. Portaldan veya https://aka.ms/migrate/appliance/hyperv.<br/><br/> İndirme boyutu 10 GB'dır.<br/><br/> İndirilen cihaz şablonu, 180 gün boyunca geçerli olan Bir Windows Server 2016 değerlendirme lisansıyla birlikte gelir. Değerlendirme süresi sona ermek üzereyse, yeni bir cihaz indirip dağıtmanızı veya cihaz VM'nin işletim sistemi lisansını etkinleştirmenizi öneririz.
**PowerShell betiği** | Script [indir](https://go.microsoft.com/fwlink/?linkid=2105112).<br/><br/> 
**Yazılım/ donanım***   |  Cihaz, Windows Server 2016, 32 GB RAM, 8 vCPUs, yaklaşık 80 GB disk depolama ve harici sanal anahtar ile makinede çalıştırılmalıdır.<br/> Cihazın statik veya dinamik bir IP adresine ihtiyacı vardır ve doğrudan veya proxy aracılığıyla internet erişimi gerektirir.<br/><br/> Cihazı Hyper-V VM olarak çalıştırarsanız, Hyper-V ana bilgisayarında 16 GB RAM, 8 vCPUs, yaklaşık 80 GB depolama alanı ve vm cihazı için harici bir anahtar ayırmak için yeterli kaynağa ihtiyacınız vardır.<br/><br/> Cihazı fiziksel bir makinede çalıştırıyorsanız, Windows Server 2016'yı çalıştırdığından ve donanım gereksinimlerini karşıladığından emin olun. 
**Hiper-V gereksinimleri** | Cihazı VHD şablonuyla dağıtırsanız, Azure Geçiş tarafından sağlanan VM cihazı Hyper-V VM sürüm 5.0'dır.<br/><br/> Hyper-V ana bilgisayar Windows Server 2012 R2 veya daha sonra çalışıyor olmalıdır. 
**Karma değer-VHD** | VHD şablon karma değerlerini [doğrulayın.](tutorial-assess-hyper-v.md#verify-security)
**Karma değer-PowerShell komut dosyası** | PowerShell komut dosyası karma değerlerini [doğrulayın.](deploy-appliance-script.md#verify-file-security)


## <a name="appliance---physical"></a>Cihaz - Fiziksel

**Gereksinim** | **Fiziksel** 
--- | ---
**Cihaz bileşenleri** | Cihaz aşağıdaki bileşenlere sahiptir: <br/><br/> - **Yönetim uygulaması**: Cihaz dağıtımı sırasında kullanıcı girişi için kullanılan bir web uygulamasıdır. Azure'a geçiş için makineleri değerlendirirken kullanılır.<br/> - **Bulma aracısı**: Aracı makine yapılandırma verilerini toplar. Azure'a geçiş için makineleri değerlendirirken kullanılır.<br/>- **Değerlendirme aracısı**: Aracı performans verilerini toplar. Azure'a geçiş için makineleri değerlendirirken kullanılır.<br/>- **Otomatik güncelleme hizmeti**: Cihaz bileşenlerini günceller (her 24 saatte bir çalışır).
**Desteklenen dağıtım** | PowerShell yükleme komut dosyasını kullanarak özel bir fiziksel makine veya VM olarak dağıtın.
**Proje desteği** |  Bir cihaz tek bir proje ile ilişkilendirilebilir. <br/> Herhangi bir sayıda cihaz tek bir projeyle ilişkilendirilebilir.<br/> 
**Bulma sınırları** | Bir cihaz en fazla 250 fiziksel sunucu keşfedebilir.
**PowerShell betiği** | Komut dosyasını (AzureMigrateInstaller.ps1) portaldan sıkıştırılmış bir klasöre indirin. [Daha fazla bilgi edinin](tutorial-assess-physical.md#set-up-the-appliance). Alternatif olarak, [doğrudan indirin.](https://go.microsoft.com/fwlink/?linkid=2105112)<br/><br/> İndirme boyutu 59,7 MB'dır.
**Yazılım/ donanım** |  Cihaz, Windows Server 2016, 32 GB RAM, 8 vCPUs, yaklaşık 80 GB disk depolama ve harici sanal anahtar ile makinede çalıştırılmalıdır.<br/> Cihazın statik veya dinamik bir IP adresine ihtiyacı vardır ve doğrudan veya proxy aracılığıyla internet erişimi gerektirir.<br/><br/> Cihazı fiziksel bir makinede çalıştırıyorsanız, Windows Server 2016'yı çalıştırdığından ve donanım gereksinimlerini karşıladığından emin olun. 
**Karma değeri** | PowerShell komut dosyası karma değerlerini [doğrulayın.](deploy-appliance-script.md#verify-file-security)

## <a name="url-access"></a>URL erişimi

Azure Geçir cihazının Internet bağlantısına ihtiyacı vardır.

- Cihazı dağıttığınızda, Azure Geçir aşağıdaki tabloda özetlenen URL'lere bağlantı denetimi yapar.
- Internet'e bağlanmak için URL tabanlı bir proxy kullanıyorsanız, proxy'nin URL'lere bakarken alınan CNAME kayıtlarını çözdüğünden emin olmak için bu URL'lere erişime izin vermeniz gerekir.

**URL** | **Şey**  
--- | --- |
*.portal.azure.com  | Azure portalına gidin.
*.windows.net <br/> *.msftauth.net <br/> *.msauth.net <br/> *.microsoft.com <br/> *.live.com | Azure aboneliğinizde oturum açın.
*.microsoftonline.com <br/> *.microsoftonline-p.com | Cihazın Azure Geçiş ile iletişim kurması için Azure Active Directory (AD) uygulamaları oluşturun.
management.azure.com | Azure Geçiş hizmetiyle iletişim kurması için cihazın Azure AD uygulamaları oluşturun.
dc.services.visualstudio.com | Dahili izleme için kullanılan uygulama günlüklerini yükleyin.
*.vault.azure.net | Azure Anahtar Kasası'nda sırları yönetin.
aka.ms/* | Aka bağlantılarına erişime izin verin. Azure Geçiş cihazı güncelleştirmeleri için kullanılır.
download.microsoft.com/download | Microsoft'tan indirmelere izin verin.
*.servicebus.windows.net | Cihaz ve Azure Geçiş hizmeti arasındaki iletişim.
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com | Azure Geçiş hizmeti URL'lerine bağlanın.
*.hypervrecoverymanager.windowsazure.com | **VMware aracısız geçiş için kullanılır**<br/><br/> Azure Geçiş hizmeti URL'lerine bağlanın.
*.blob.core.windows.net |  **VMware aracısız geçiş için kullanılır**<br/><br/>Geçiş için depolamaya veri yükleyin.




## <a name="collected-data---vmware"></a>Toplanan veriler - VMware

Cihaz meta veriler, performans verileri ve bağımlılık analizi verileri toplar (aracısız [bağımlılık çözümlemesi](concepts-dependency-visualization.md) kullanılıyorsa).

### <a name="metadata"></a>Meta Veriler

Azure Geçir cihazı tarafından keşfedilen meta veriler, makinelerin ve uygulamaların Azure'a, doğru boyuttaki makinelere ve uygulamalara geçişe hazır olup olmadığını anlamanıza, maliyetleri planlar ve uygulama bağımlılıklarını analiz etmenize yardımcı olur. Microsoft bu verileri herhangi bir lisans uyumluluk denetiminde kullanmaz.

Burada, cihazın toplayıp Azure'a gönderdiği VMware VM meta verilerinin tam listesi vebu veriler yer almaktadır.

**Veri** | **Sayaç**
--- | --- 
**Makine detayları** | 
VM Kimliği | Vm. Config.InstanceUuid 
VM adı | Vm. Config.Name
vCenter Sunucu Kimliği | VMwareClient.Instance.Uuid
VM açıklaması | Vm. Özet.Config.Annotation
Lisans ürün adı | Vm. Client.ServiceContent.About.LicenseProductName
İşletim sistemi türü | Vm. SummaryConfig.GuestFullName
Önyükleme türü | Vm. Config.Firmware
Çekirdek sayısı | Vm. Config.Hardware.NumCPU
Bellek (MB) | Vm. Config.Hardware.MemoryMB
Disk sayısı | Vm. Config.Hardware.Device.ToList(). FindAll(x => VirtualDisk'tir.say
Disk boyutu listesi | Vm. Config.Hardware.Device.ToList(). FindAll(x => VirtualDisk olduğunu)
Ağ bağdaştırıcıları listesi | Vm. Config.Hardware.Device.ToList(). FindAll(x => VirtualEthernet'tir.say
CPU kullanımı | cpu.usage.average
Bellek kullanımı |mem.usage.average
**Disk ayrıntıları başına** | 
Disk anahtar değeri | Disk. Anahtar
Dikunit numarası | Disk. Birim Numarası
Disk denetleyici anahtar değeri | Disk. ControllerKey.Value
Gigabayt lar sağlanmış | virtualDisk.DeviceInfo.Summary
Disk adı | Disk kullanılarak oluşturulan değer. UnitNumber, disk. Anahtar, disk. ControllerKey.VAlue
İşlemleri saniyede okuma | virtualDisk.numberReadAveraged.average
Saniyede yazma işlemleri | virtualDisk.numberWriteAveraged.average
Okuma iş sayısı (MB/per saniyede) | virtualDisk.read.average
Yazma iş başı (mb/per saniye) | virtualDisk.write.average
**NIC ayrıntıları başına** | 
Ağ bağdaştırıcısı adı | Nıc. Anahtar
MAC adresi | ((VirtualEthernetCard)nic). MacAddress
IPv4 adresleri | Vm. Guest.Net
IPv6 adresleri | Vm. Guest.Net
Okuma iş sayısı (MB/per saniyede) | net.received.average
Yazma iş başı (mb/per saniye) | net.transmitted.average
**Stok yolu ayrıntıları** | 
Adı | Kapsayıcı. GetType(). Adı
Alt nesne türü | Kapsayıcı. Çocuk Tipi
Referans ayrıntıları | Kapsayıcı. Moref
Üst bilgiler | Konteyner.Üst
VM başına klasör ayrıntıları | ((Klasör)kapsayıcı). ChildEntity.Type
VM başına veri merkezi ayrıntıları | ((Datacenter)konteyner). VmFolder
Ana bilgisayar klasörü başına veri merkezi ayrıntıları | ((Datacenter)konteyner). HostFolder
Ana bilgisayar başına küme ayrıntıları | ((ClusterComputeResource)konteyner). Ana bilgisayar
VM başına ana bilgisayar ayrıntıları | ((HostSystem)konteyner). Vm

### <a name="performance-data"></a>Performans verileri


İşte, cihazın toplayıp Azure'a gönderdiği VMware VM performans verileri.

**Veri** | **Sayaç** | **Değerlendirme etkisi**
--- | --- | ---
CPU kullanımı | cpu.usage.average | Önerilen VM boyutu/maliyeti
Bellek kullanımı | mem.usage.average | Önerilen VM boyutu/maliyeti
Disk okuma iş tüsubi (MB/saniye) | virtualDisk.read.average | Disk boyutu, depolama maliyeti, VM boyutu için hesaplama
Disk iş bittirme (mb/per saniye) yazar | virtualDisk.write.average | Disk boyutu, depolama maliyeti, VM boyutu için hesaplama
Saniyede disk okuma işlemleri | virtualDisk.numberReadAveraged.average | Disk boyutu, depolama maliyeti, VM boyutu için hesaplama
Disk saniyede işlem yazar | virtualDisk.numberWriteAveraged.average  | Disk boyutu, depolama maliyeti, VM boyutu için hesaplama
NIC okuma iş tüsubi (MB/saniye) | net.received.average | VM boyutu için hesaplama
NIC iş bittirme (saniyede MB) yazar | net.transmitted.average  |VM boyutu için hesaplama

### <a name="app-dependencies-metadata"></a>Uygulama bağımlılıkları meta verileri

Aracısız bağımlılık çözümlemesi bağlantı ve işlem verilerini toplar.

#### <a name="connection-data"></a>Bağlantı verileri

Aracısız bağımlılık analizi için etkinleştirilen her VM'den cihazın topladığı bağlantı verileri aşağıda veda eder. Bu veriler Azure'a gönderilir.

**Veri** | **Kullanılan komut** 
--- | --- 
Yerel bağlantı noktası | Netstat
Yerel IP adresi | Netstat
Uzak bağlantı noktası | Netstat
Uzak IP adresi | Netstat
TCP bağlantı durumu | Netstat
İşlem Kimliği | Netstat
Hayır. etkin bağlantıların | Netstat

#### <a name="process-data"></a>Veri işleme
Aracısız bağımlılık çözümlemesi için etkinleştirilen her VM'den cihazın topladığı işlem verileri aşağıda veda eder. Bu veriler Azure'a gönderilir.

**Veri** | **WMI sınıfı** | **WMI sınıfı özellik**
--- | --- | ---
İşlem adı | Win32_Process | Çalıştırılabilir Yol
İşlem bağımsız değişkenleri | Win32_Process | Commandline
Uygulama adı | Win32_Process | VersionInfo.ProductName ExecutablePath özelliğinin parametresi

#### <a name="linux-vm-data"></a>Linux VM verileri

Aracısız bağımlılık analizi için etkinleştirilen her Linux VM'den cihazın topladığı bağlantı ve işlem verileri aşağıda verilmiştir. Bu veriler Azure'a gönderilir.

**Veri** | **Kullanılan komut** 
--- | ---
Yerel bağlantı noktası | Netstat 
Yerel IP adresi | Netstat 
Uzak bağlantı noktası | Netstat 
Uzak IP adresi | Netstat 
TCP bağlantı durumu | Netstat 
Hayır. etkin bağlantıların | Netstat
İşlem Kimliği  | Netstat 
İşlem adı | Ps
İşlem bağımsız değişkenleri | Ps
Uygulama adı | dpkg veya rpm



## <a name="collected-data---hyper-v"></a>Toplanan veriler - Hyper-V

Cihaz meta veriler, performans verileri ve bağımlılık analizi verileri toplar (aracısız [bağımlılık çözümlemesi](concepts-dependency-visualization.md) kullanılıyorsa).

### <a name="metadata"></a>Meta Veriler
Azure Geçir cihazı tarafından keşfedilen meta veriler, makinelerin ve uygulamaların Azure'a, doğru boyuttaki makinelere ve uygulamalara geçişe hazır olup olmadığını anlamanıza, maliyetleri planlar ve uygulama bağımlılıklarını analiz etmenize yardımcı olur. Microsoft bu verileri herhangi bir lisans uyumluluk denetiminde kullanmaz.

Cihazın toplayıp Azure'a gönderdiği Hyper-V VM meta verilerinin tam listesi aşağıda veda edebilirsiniz.

**Veri* | **WMI SINIF** | **WMI SINIF ÖZELLİkLerİ**
--- | --- | ---
**Makine detayları** | 
BIOS _ Msvm_BIOSElement seri numarası | BIOSSerialNumber
VM tipi (Gen 1 veya 2) | Msvm_VirtualSystemSettingData | VirtualSystemSubType
VM ekran adı | Msvm_VirtualSystemSettingData | ElementName
VM sürümü | Msvm_ProcessorSettingData | Sanal Miktar
Bellek (bayt) | Msvm_MemorySettingData | Sanal Miktar
VM tarafından tüketilebilen maksimum bellek | Msvm_MemorySettingData | Sınır
Dinamik bellek etkin | Msvm_MemorySettingData | DynamicMemoryEnabled
İşletim sistemi adı/sürüm/FQDN | Msvm_KvpExchangeComponent | GuestIntrinsicExchangeItems Adı Veri
VM güç durumu | Msvm_ComputerSystem | EnabledState
**Disk ayrıntıları başına** | 
Disk tanımlayıcısı | Msvm_VirtualHardDiskSettingData | VirtualDiskId
Sanal sabit disk türü | Msvm_VirtualHardDiskSettingData | Tür
Sanal sabit disk boyutu | Msvm_VirtualHardDiskSettingData | MaxInternalSize
Sanal sabit disk üst öğesi | Msvm_VirtualHardDiskSettingData | ParentPath
**NIC ayrıntıları başına** | 
IP adresleri (sentetik NIC'ler) | Msvm_GuestNetworkAdapterConfiguration | IPAdresleri
DHCP etkin (sentetik NIC'ler) | Msvm_GuestNetworkAdapterConfiguration | DHCPEtkin
NIC KIMLIĞI (sentetik NIC'ler) | Msvm_SyntheticEthernetPortSettingData | InstanceID
NIC MAC adresi (sentetik NIC'ler) | Msvm_SyntheticEthernetPortSettingData | Adres
NIC Kimliği (eski NIC'ler) | MsvmEmulatedEthernetPortSetting Verileri | InstanceID
NIC MAC KIMLIĞI (eski NIC'ler) | MsvmEmulatedEthernetPortSetting Verileri | Adres

### <a name="performance-data"></a>Performans verileri

İşte, cihazın toplayıp Azure'a gönderdiği Hyper VM performans verileri.

**Performans sayacı sınıfı** | **Sayaç** | **Değerlendirme etkisi**
--- | --- | ---
Hyper-V Hypervisor Sanal İşlemci | % Misafir Çalışma Süresi | Önerilen VM boyutu/maliyeti
Hyper-V Dinamik Bellek VM | Akım Basıncı (%)<br/> Misafir Görünür Fiziksel Bellek (MB) | Önerilen VM boyutu/maliyeti
Hyper-V Sanal Depolama Cihazı | Bayt/Saniye oku | Disk boyutu, depolama maliyeti, VM boyutu için hesaplama
Hyper-V Sanal Depolama Cihazı | Bayt/Saniye Yaz | Disk boyutu, depolama maliyeti, VM boyutu için hesaplama
Hyper-V Sanal Ağ Adaptörü | Alınan Baytlar/İkinci | VM boyutu için hesaplama
Hyper-V Sanal Ağ Adaptörü | Gönderilen Bayt/İkinci | VM boyutu için hesaplama

- CPU kullanımı, VM'ye bağlı tüm sanal işlemciler için tüm kullanımların toplamıdır.
- Bellek kullanımı (Geçerli Basınç * Konuk Görünür Fiziksel Bellek) / 100.
- Disk ve ağ kullanım değerleri listelenen Hyper-V performans sayaçlarından toplanır.

## <a name="appliance-upgrades"></a>Cihaz yükseltmeleri

Cihaz üzerinde çalışan Azure Geçiş aracıları güncelleştirildikçe cihaz yükseltilir. Cihazda varsayılan olarak otomatik güncelleştirme etkin olduğundan bu otomatik olarak gerçekleşir. Aracıları el ile güncelleştirmek için bu varsayılan ayarı değiştirebilirsiniz.

- **Otomatik güncelleştirmeyi kapatın**: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureAppliance "AutoUpdate" tuşunu 0 (DWORD) olarak ayarlayarak kayıt defterinde otomatik güncelleştirmeyi kapatırsınız. Manuel güncellemeler kullanmaya karar verirseniz, cihazdaki her eski aracı için **Güncelleştirme** düğmesini kullanarak cihazdaki tüm aracıları aynı anda güncellemeniz önemlidir.
- **El ile güncelleştirin**: Manuel güncellemeler için, cihazdaki her eski aracı için **Güncelleme** düğmesini kullanarak cihazdaki tüm aracıları güncellediğinizden emin olun. Güncelleme ayarını istediğiniz zaman otomatik güncelleştirmelere geri döndürebilirsiniz.

![Cihazı otomatik olarak güncelleyin](./media/migrate-appliance/autoupdate.png)

## <a name="next-steps"></a>Sonraki adımlar

- VMware için cihazı [nasıl](how-to-set-up-appliance-vmware.md) ayarlayacağım öğrenin.
- Hyper-V için cihazı [nasıl](how-to-set-up-appliance-hyper-v.md) ayarlayın öğrenin.
- Cihazı fiziksel sunucular için nasıl ayarlayacağınızı [öğrenin.](how-to-set-up-appliance-physical.md)

