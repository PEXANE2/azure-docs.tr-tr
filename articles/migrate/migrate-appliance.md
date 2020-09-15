---
title: Azure Geçişi gereci
description: Azure geçişi gereci desteğinin bir özetini sağlar.
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: e2bd3f2fa40bbf31713393f18a04624d70cbd244
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90084779"
---
# <a name="azure-migrate-appliance"></a>Azure Geçişi gereci

Bu makale, Azure geçişi gereci için önkoşulları ve destek gereksinimlerini özetler. 

## <a name="deployment-scenarios"></a>Dağıtım senaryoları

Azure geçişi gereci aşağıdaki senaryolarda kullanılır.

**Senaryo** | **Araç** | **Kullanıldığı yerler** 
--- | --- | ---
**VMware VM değerlendirmesi** | Azure geçişi: Sunucu değerlendirmesi | VMware VM 'lerini bulma<br/><br/> Makine uygulamalarını ve bağımlılıklarını bulma<br/><br/> Değerlendirmeler için makine meta verilerini ve performans meta verilerini toplayın.
**VMware VM aracısız geçişi** | Azure geçişi: sunucu geçişi | VMware VM 'lerini bulma <br/><br/> VMware VM 'lerini aracısız geçişle çoğaltın.
**Hyper-V VM değerlendirmesi** | Azure geçişi: Sunucu değerlendirmesi | Hyper-V VM 'lerini bulma<br/><br/> Değerlendirmeler için makine meta verilerini ve performans meta verilerini toplayın.
**Fiziksel makine değerlendirmesi** |  Azure geçişi: Sunucu değerlendirmesi |  Fiziksel sunucuları (veya fiziksel sunucu olarak kabul ettiğiniz VM 'Leri) bulun.<br/><br/> Değerlendirmeler için makine meta verilerini ve performans meta verilerini toplayın.

## <a name="deployment-methods"></a>Dağıtım yöntemleri

Gereç birkaç yöntem kullanılarak dağıtılabilir:

- Gereç, VMware VM 'leri ve Hyper-V VM 'Leri (VMware için OVA şablonu veya Hyper-V için VHD) kullanılarak dağıtılabilir.
- Bir şablon kullanmak istemiyorsanız, bir PowerShell betiği kullanarak VMware veya Hyper-V için gereci dağıtabilirsiniz.
- Azure Kamu 'da gereci bir komut dosyası kullanarak dağıtmanız gerekir.
- Fiziksel sunucular için, her zaman gereci bir komut dosyası kullanarak dağıtırsınız.
- İndirme bağlantıları aşağıdaki tablolarda bulunur.


## <a name="appliance---vmware"></a>Gereç-VMware 

Aşağıdaki tabloda, VMware için Azure geçişi gereç gereksinimleri özetlenmektedir.

**Gereksinim** | **VMware** 
--- | ---
**İzinler** | Gereç Web uygulamasına yerel olarak veya uzaktan erişmek için, Gereç makinesinde bir etki alanı yöneticisi veya yerel yönetici olmanız gerekir.
**Gereç bileşenleri** | Gereç aşağıdaki bileşenlere sahiptir:<br/><br/> - **Yönetim uygulaması**: Bu, Gereç dağıtımı sırasında Kullanıcı girişi için bir Web uygulamasıdır. Makineleri Azure 'a geçiş için değerlendirmek için kullanılır.<br/> - **Keşif Aracısı**: aracı makine yapılandırma verilerini toplar. Makineleri Azure 'a geçiş için değerlendirmek için kullanılır.<br/>- **Toplayıcı Aracısı**: aracı performans verilerini toplar. Makineleri Azure 'a geçiş için değerlendirmek için kullanılır.<br/>- **Otomatik güncelleştirme hizmeti**: gereç bileşenlerini güncelleştirir (24 saatte bir çalışır).<br/>- **DRA Aracısı**: VM çoğaltmasını düzenleyin ve çoğaltılan makineler ile Azure arasındaki iletişimi koordine edin. Yalnızca VMware VM 'Leri aracısız geçiş kullanılarak Azure 'a çoğaltılırken kullanılır.<br/>- **Ağ geçidi**: çoğaltılan verileri Azure 'a gönderir. Yalnızca VMware VM 'Leri aracısız geçiş kullanılarak Azure 'a çoğaltılırken kullanılır.
**Desteklenen Dağıtım** | OVA şablonunu kullanarak VMware VM olarak dağıtın.<br/><br/> PowerShell yükleme betiği kullanarak VMware VM veya fiziksel makine olarak dağıtın.
**Proje desteği** |  Bir gereç, tek bir projeyle ilişkilendirilebilir. <br/> Herhangi bir sayıda gereç, tek bir projeyle ilişkilendirilebilir.<br/> 
**Bulma sınırları** | Bir gereç, vCenter Server en fazla 10.000 VMware VM 'yi bulabilir.<br/> Bir gereç, tek bir vCenter Server bağlanabilir.
**OVA şablonu** | Portalından veya [buradan](https://go.microsoft.com/fwlink/?linkid=2140333) indirin<br/><br/> İndirme boyutu 11,6 GB 'dir.<br/><br/> İndirilen gereç şablonu, 180 gün için geçerli olan bir Windows Server 2016 değerlendirme lisansıyla birlikte gelir. Değerlendirme süresi sona ermeden yakın ise, yeni bir gereç indirmeniz ve dağıtmanız ya da gereç sanal makinesinin işletim sistemi lisansını etkinleştirmenizi öneririz.
**PowerShell betiği** | Bu [makaleye](./deploy-appliance-script.md#set-up-the-appliance-for-vmware)başvurun.<br/><br/> 
**Yazılım/donanım** |  Gereç, Windows Server 2016, 32-GB RAM, 8 vCPU ve 80 GB disk depolaması ve harici bir sanal anahtar ile makine üzerinde çalışmalıdır.<br/> Gereç doğrudan veya bir ara sunucu üzerinden internet erişimi gerektirir.<br/><br/> Gereci bir VMware VM üzerinde çalıştırırsanız, gereksinimleri karşılayan bir VM 'yi ayırmak için vCenter Server yeterli kaynaklara sahip olmanız gerekir.<br/><br/> Gereci fiziksel bir makinede çalıştırırsanız, Windows Server 2016 ' in çalıştığından ve donanım gereksinimlerini karşıladığından emin olun.
**VMware gereksinimleri** | Gereci bir VMware VM 'si olarak dağıtırsanız, sürüm 5,5 veya sonraki bir sürümünü çalıştıran bir ESXi konağına dağıtılması gerekir.<br/><br/> 5,5, 6,0, 6,5 veya 6,7 vCenter Server çalışıyor.
**VDDK (aracısız geçiş)** | Gereci bir VMware sanal makinesi olarak dağıtırsanız ve aracısız bir geçiş çalıştırıyorsanız, VMware vSphere VDDK 'nin gereç sanal makinesine yüklenmesi gerekir.
**Karma değeri-OVA** | OVA şablonu karma değerlerini [doğrulayın](tutorial-assess-vmware.md#verify-security) .
**Karma değeri-PowerShell betiği** | PowerShell betiği karma değerlerini [doğrulayın](deploy-appliance-script.md#verify-file-security) .




## <a name="appliance---hyper-v"></a>Gereç-Hyper-V

**Gereksinim** | **Hyper-V** 
--- | ---
**İzinler** | Gereç Web uygulamasına yerel olarak veya uzaktan erişmek için, Gereç makinesinde bir etki alanı yöneticisi veya yerel yönetici olmanız gerekir.
**Gereç bileşenleri** | Gereç aşağıdaki bileşenlere sahiptir:<br/><br/>- **Yönetim uygulaması**: Bu, Gereç dağıtımı sırasında Kullanıcı girişi için bir Web uygulamasıdır. Makineleri Azure 'a geçiş için değerlendirmek için kullanılır.<br/> - **Keşif Aracısı**: aracı makine yapılandırma verilerini toplar. Makineleri Azure 'a geçiş için değerlendirmek için kullanılır.<br/>- **Toplayıcı Aracısı**: aracı performans verilerini toplar. Makineleri Azure 'a geçiş için değerlendirmek için kullanılır.<br/>- **Otomatik güncelleştirme hizmeti**: gereç bileşenlerini güncelleştirir (24 saatte bir çalışır).
**Desteklenen Dağıtım** | Bir VHD şablonu kullanarak Hyper-V VM olarak dağıtın.<br/><br/> PowerShell yükleme betiği kullanarak Hyper-V VM veya fiziksel makine olarak dağıtın.
**Proje desteği** |  Bir gereç, tek bir projeyle ilişkilendirilebilir. <br/> Herhangi bir sayıda gereç, tek bir projeyle ilişkilendirilebilir.<br/> 
**Bulma sınırları** | Bir gereç, 5000 adede kadar Hyper-V VM 'Leri bulabilir.<br/> Bir gereç, 300 adede kadar Hyper-V konaklarına bağlanabilir.
**VHD şablonu** | VHD dahil olmak üzere daraltılmış klasör. Portalından veya [buradan](https://go.microsoft.com/fwlink/?linkid=2140422)indirin.<br/><br/> İndirme boyutu 10,4 GB 'dir.<br/><br/> İndirilen gereç şablonu, 180 gün için geçerli olan bir Windows Server 2016 değerlendirme lisansıyla birlikte gelir. Değerlendirme süresi sona ermeden yakın ise, yeni bir gereç indirmeniz ve dağıtmanız ya da gereç sanal makinesinin işletim sistemi lisansını etkinleştirmenizi öneririz.
**PowerShell betiği** | Bu [makaleye](./deploy-appliance-script.md#set-up-the-appliance-for-hyper-v)başvurun.<br/><br/> 
**Yazılım/donanım***   |  Gereç, Windows Server 2016, 16 GB RAM, 8 vCPU, 80 GB disk depolaması ve harici bir sanal anahtar ile makine üzerinde çalışmalıdır.<br/> Gereç statik veya dinamik bir IP adresine gerek duyar ve doğrudan ya da bir proxy üzerinden internet erişimi gerektirir.<br/><br/> Gereci bir Hyper-V VM 'si olarak çalıştırırsanız, Hyper-V konağında, 16 GB RAM, 8 vCPU, 80 GB depolama alanı etrafında ve gereç VM 'si için bir harici anahtar ayırmak üzere yeterli kaynaklara sahip olmanız gerekir.<br/><br/> Gereci fiziksel bir makinede çalıştırırsanız, Windows Server 2016 ' in çalıştığından ve donanım gereksinimlerini karşıladığından emin olun. 
**Hyper-V gereksinimleri** | Gereci VHD şablonuyla dağıtırsanız, Azure geçişi tarafından sunulan gereç sanal makinesi, Hyper-V VM sürüm 5,0 ' dir.<br/><br/> Hyper-V konağı Windows Server 2012 R2 veya üstünü çalıştırmalıdır. 
**Karma değeri-VHD** | VHD şablonu karma değerleri.
**Karma değeri-PowerShell betiği** | PowerShell betiği karma değerlerini [doğrulayın](deploy-appliance-script.md#verify-file-security) .


## <a name="appliance---physical"></a>Gereç-fiziksel

**Gereksinim** | **Fiziksel** 
--- | ---
**İzinler** | Gereç Web uygulamasına yerel olarak veya uzaktan erişmek için, Gereç makinesinde bir etki alanı yöneticisi veya yerel yönetici olmanız gerekir.
**Gereç bileşenleri** | Gereç aşağıdaki bileşenlere sahiptir: <br/><br/> - **Yönetim uygulaması**: Bu, Gereç dağıtımı sırasında Kullanıcı girişi için bir Web uygulamasıdır. Makineleri Azure 'a geçiş için değerlendirmek için kullanılır.<br/> - **Keşif Aracısı**: aracı makine yapılandırma verilerini toplar. Makineleri Azure 'a geçiş için değerlendirmek için kullanılır.<br/>- **Toplayıcı Aracısı**: aracı performans verilerini toplar. Makineleri Azure 'a geçiş için değerlendirmek için kullanılır.<br/>- **Otomatik güncelleştirme hizmeti**: gereç bileşenlerini güncelleştirir (24 saatte bir çalışır).
**Desteklenen Dağıtım** | PowerShell yükleme betiği kullanarak adanmış fiziksel makine veya VM olarak dağıtın. Betik, portaldan indirilebilir.
**Proje desteği** |  Bir gereç, tek bir projeyle ilişkilendirilebilir. <br/> Herhangi bir sayıda gereç, tek bir projeyle ilişkilendirilebilir.<br/> 
**Bulma sınırları** | Bir gereç, en fazla 1000 fiziksel sunucu bulabilir.
**PowerShell betiği** | Betiği (AzureMigrateInstaller.ps1) portaldan veya [buradan](https://go.microsoft.com/fwlink/?linkid=2140334)bir daraltılmış klasöre indirin. [Daha fazla bilgi edinin](tutorial-discover-physical.md).<br/><br/> İndirme boyutu 85 MB 'tır.
**Yazılım/donanım** |  Gereç, Windows Server 2016, 16 GB RAM, 8 vCPU, 80 GB disk depolaması ve harici bir sanal anahtar ile makine üzerinde çalışmalıdır.<br/> Gereç statik veya dinamik bir IP adresine gerek duyar ve doğrudan ya da bir proxy üzerinden internet erişimi gerektirir.<br/><br/> Gereci fiziksel bir makinede çalıştırırsanız, Windows Server 2016 ' in çalıştığından ve donanım gereksinimlerini karşıladığından emin olun.<br/> Gereci Windows Server 2019 ile bir makinede çalıştırmak desteklenmez.
**Karma değeri** | PowerShell betiği karma değerlerini [doğrulayın](deploy-appliance-script.md#verify-file-security) .

## <a name="url-access"></a>URL erişimi

Azure geçişi gereci internet bağlantısı gerektirir.

- Gereci dağıtırken, Azure geçişi gerekli URL 'lere bir bağlantı denetimi yapar.
- Listedeki tüm URL 'Lere erişim izni vermeniz gerekir. Yalnızca değerlendirme yapıyorsanız, yalnızca VMware aracısız geçişi için gerekli olarak işaretlenen URL 'Leri atlayabilirsiniz.
-  İnternet 'e bağlanmak için URL tabanlı bir ara sunucu kullanıyorsanız, proxy 'nin URL 'Leri ararken alınan CNAME kayıtlarını çözümlediği emin olun.

### <a name="public-cloud-urls"></a>Genel bulut URL 'Leri

**URL** | **Ayrıntılar**  
--- | --- |
*.portal.azure.com  | Azure portalına gidin.
*.windows.net <br/> *.msftauth.net <br/> *.msauth.net <br/> *.microsoft.com <br/> *.live.com <br/> *. office.com | Azure aboneliğinizde oturum açın.
*.microsoftonline.com <br/> *.microsoftonline-p.com | Gereç için Azure geçişi ile iletişim kurmak üzere Azure Active Directory (AD) uygulamalar oluşturun.
management.azure.com | Azure geçişi hizmeti ile iletişim kurmak için gereç için Azure AD uygulamaları oluşturun.
*.services.visualstudio.com | İç izleme için kullanılan uygulama günlüklerini karşıya yükleyin.
*.vault.azure.net | Azure Key Vault gizli dizileri yönetin. Note: çoğaltılacak makinelerin buna erişimi olduğundan emin olun.
aka.ms/* | Diğer adıyla bağlantılarına erişime izin ver. Azure geçiş gereci güncellemeleri için kullanılır.
download.microsoft.com/download | Microsoft Download 'ten indirmelere izin ver.
*.servicebus.windows.net | Gereç ve Azure geçişi hizmeti arasındaki iletişim.
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com | Azure geçişi hizmeti URL 'Lerine bağlanın.
*.hypervrecoverymanager.windowsazure.com | **VMware aracısız geçişi için kullanılır**<br/><br/> Azure geçişi hizmeti URL 'Lerine bağlanın.
*.blob.core.windows.net |  **VMware aracısız geçişi için kullanılır**<br/><br/>Geçiş için verileri depolamaya yükleyin.

### <a name="government-cloud-urls"></a>Kamu Bulutu URL 'Leri

**URL** | **Ayrıntılar**  
--- | --- |
*. portal.azure.us  | Azure portalına gidin.
graph.windows.net | Azure aboneliğinizde oturum açın.
login.microsoftonline.us  | Gereç için Azure geçişi ile iletişim kurmak üzere Azure Active Directory (AD) uygulamalar oluşturun.
management.usgovcloudapi.net | Azure geçişi hizmeti ile iletişim kurmak için gereç için Azure AD uygulamaları oluşturun.
*.services.visualstudio.com | İç izleme için kullanılan uygulama günlüklerini karşıya yükleyin.
*. vault.usgovcloudapi.net | Azure Key Vault gizli dizileri yönetin.
aka.ms/* | Diğer adıyla bağlantılarına erişime izin ver. Azure geçiş gereci güncellemeleri için kullanılır.
download.microsoft.com/download | Microsoft Download 'ten indirmelere izin ver.
*. servicebus.usgovcloudapi.net  | Gereç ve Azure geçişi hizmeti arasındaki iletişim.
*. discoverysrv.windowsazure.us <br/> *. migration.windowsazure.us | Azure geçişi hizmeti URL 'Lerine bağlanın.
*. hypervrecoverymanager.windowsazure.us | **VMware aracısız geçişi için kullanılır**<br/><br/> Azure geçişi hizmeti URL 'Lerine bağlanın.
*. blob.core.usgovcloudapi.net  |  **VMware aracısız geçişi için kullanılır**<br/><br/>Geçiş için verileri depolamaya yükleyin.
*. applicationinsights.us | İç izleme için kullanılan uygulama günlüklerini karşıya yükleyin.





## <a name="collected-data---vmware"></a>Toplanan veriler-VMware

Gereç meta verileri, performans verilerini ve bağımlılık analizi verilerini (aracısız [bağımlılık Analizi](concepts-dependency-visualization.md) kullanılıyorsa) toplar.

### <a name="metadata"></a>Meta Veriler

Azure geçişi gereci tarafından bulunan meta veriler, makinelerin ve uygulamaların Azure 'a geçiş için hazır olup olmadığını, makine ve uygulamaları doğru boyuta getirmek, planlama maliyetlerini ve uygulama bağımlılıklarını analiz etmenize yardımcı olur. Microsoft bu verileri hiçbir lisans uyumluluğu denetimine kullanmaz.

Bu, gerecin topladığı ve Azure 'a gönderdiği VMware VM meta verilerinin tam listesini aşağıda bulabilirsiniz.

**VERILERI** | **SAYAÇ**
--- | --- 
**Makine ayrıntıları** | 
VM Kimliği | vm.Config. Instanceuuıd 
VM adı | vm.Config. Ada
vCenter Server KIMLIĞI | VMwareClient. Instance. UUID
VM açıklaması | vm.Summary.Config. Ek açıklama
Lisans ürün adı | 'nin. Client. ServiceContent. about. LicenseProductName
İşletim sistemi türü | 'nin. SummaryConfig. GuestFullName
Önyükleme türü | vm.Config. Yazılımında
Çekirdek sayısı | vm.Config. Hardware. NumCPU
Bellek (MB) | vm.Config. Hardware. MemoryMB
Disk sayısı | vm.Config. Hardware. Device. ToList (). FindAll (x => VirtualDisk). Count
Disk boyutu listesi | vm.Config. Hardware. Device. ToList (). FindAll (x => VirtualDisk)
Ağ bağdaştırıcıları listesi | vm.Config. Hardware. Device. ToList (). FindAll (x => Virtualalethernet). Count
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
Name | kapsayıcı. GetType (). Ada
Alt nesnenin türü | kapsayıcı. ChildType
Başvuru ayrıntıları | kapsayıcı. MoRef
Üst Ayrıntılar | Container. Parent
VM başına klasör ayrıntıları | ((Klasör) kapsayıcısı). ChildEntity. Type
VM başına veri merkezi ayrıntıları | (Datacenter) kapsayıcısı). VmFolder
Konak klasörü başına veri merkezi ayrıntıları | (Datacenter) kapsayıcısı). HostFolder
Konak başına küme ayrıntıları | ((ClusterComputeResource) kapsayıcısı). Konağının
VM başına ana bilgisayar ayrıntıları | (HostSystem) kapsayıcısı). 'Nın

### <a name="performance-data"></a>Performans verileri


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


### <a name="installed-apps-metadata"></a>Yüklü uygulamalar meta verileri

Uygulama bulma, yüklü uygulamaları ve işletim sistemi verilerini toplar.

#### <a name="windows-vm-apps-data"></a>Windows VM Apps verileri

Uygulamanın, uygulama bulma için etkinleştirilen her bir VM 'den topladığı yüklü uygulama verileri aşağıda verilmiştir. Bu veriler Azure 'a gönderilir.

**Veriler** | **Kayıt Defteri Konumu** | **Key**
--- | --- | ---
Uygulama Adı  | HKLM: \ Software\Microsoft\Windows\CurrentVersion\Uninstall\* <br/> HKLM: \ Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*  | DisplayName
Sürüm  | HKLM: \ Software\Microsoft\Windows\CurrentVersion\Uninstall\*  <br/> HKLM: \ Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*  | DisplayVersion 
Sağlayıcı  | HKLM: \ Software\Microsoft\Windows\CurrentVersion\Uninstall\*  <br/> HKLM: \ Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*  | Publisher

#### <a name="windows-vm-features-data"></a>Windows VM özellikleri verileri

Uygulamanın, uygulama bulma için etkinleştirilen her bir VM 'den topladığı Özellikler verileri aşağıda verilmiştir. Bu veriler Azure 'a gönderilir.

**Veriler**  | **PowerShell cmdlet 'i** | **Özellik**
--- | --- | ---
Name  | Get-WindowsFeature  | Name
Özellik türü | Get-WindowsFeature  | FeatureType
Üst  | Get-WindowsFeature  | Üst

#### <a name="windows-vm-sql-server-metadata"></a>Windows VM SQL Server meta verileri

Uygulamanın, uygulama bulma için etkinleştirilmiş Microsoft SQL Server çalıştıran VM 'lerden topladığı SQL Server meta verileri aşağıda verilmiştir. Bu veriler Azure 'a gönderilir.

**Veriler**  | **Kayıt Defteri Konumu**  | **Key**
--- | --- | ---
Name  | HKLM: \ SOFTWARE\Microsoft\Microsoft SQL Server\ınstance Names\SQL  | ınstalınstalstance
Sürüm  | HKLM: \ SOFTWARE\Microsoft\Microsoft SQL Server \\ \<InstanceName> \setup  | Sürüm 
Hizmet Paketi  | HKLM: \ SOFTWARE\Microsoft\Microsoft SQL Server \\ \<InstanceName> \setup  | SP2
Sürüm  | HKLM: \ SOFTWARE\Microsoft\Microsoft SQL Server \\ \<InstanceName> \setup  | Sürüm 

#### <a name="windows-vm-operating-system-data"></a>Windows VM işletim sistemi verileri

Bu, gerecin uygulama bulma için etkin her VM 'yi topladığı işletim sistemi verileri. Bu veriler Azure 'a gönderilir.

Veriler  | WMI sınıfı  | WMI sınıfı özelliği
--- | --- | ---
Name  | Win32_operatingsystem  | Başlık
Sürüm  | Win32_operatingsystem  | Sürüm
Mimari  | Win32_operatingsystem  | OSArchitecture

#### <a name="linux-vm-apps-data"></a>Linux VM Apps verileri

Uygulamanın, uygulama bulma için etkinleştirilen her bir VM 'den topladığı yüklü uygulama verileri aşağıda verilmiştir. VM 'nin işletim sistemine bağlı olarak, bir veya daha fazla komut çalıştırılır. Bu veriler Azure 'a gönderilir.

Veriler  | Komut
--- | --- 
Name | RPM, dpkg-Query, Snap
Sürüm | RPM, dpkg-Query, Snap
Sağlayıcı | RPM, dpkg-Query, Snap

#### <a name="linux-vm-operating-system-data"></a>Linux VM işletim sistemi verileri

Bu, gerecin uygulama bulma için etkin her VM 'yi topladığı işletim sistemi verileri. Bu veriler Azure 'a gönderilir.

**Veriler**  | **Komut** 
--- | --- | ---
Name <br/> sürüm | Aşağıdaki dosyalardan bir veya daha fazlası toplanmıştır:<br/> <br/>/etc/OS-Release  <br> /usr/lib/OS-Release  <br> /etc/Enterprise-Release  <br> /etc/redhat-release  <br> /etc/Oracle-Release  <br> /etc/SuSE-release  <br> /etc/LSB-Release  <br> /etc/debian_version 
Mimari | uname


### <a name="app-dependencies-metadata"></a>Uygulama bağımlılıkları meta verileri

Aracısız bağımlılık analizi, bağlantı ve işlem verilerini toplar.

#### <a name="windows-vm-app-dependencies-data"></a>Windows VM uygulama bağımlılıkları verileri

Bu, gerecin aracısız bağımlılık analizi için etkinleştirilen her bir VM 'den topladığı bağlantı verileri aşağıda verilmiştir. Bu veriler Azure 'a gönderilir.

**Veriler** | **Kullanılan komut** 
--- | --- 
Yerel bağlantı noktası | Netstat
Yerel IP adresi | Netstat
Uzak bağlantı noktası | Netstat
Uzak IP adresi | Netstat
TCP bağlantı durumu | Netstat
İşlem Kimliği | Netstat
Etkin bağlantı sayısı | Netstat


Bu, gerecin aracısız bağımlılık analizi için etkinleştirilen her bir VM 'den topladığı işlem verileri aşağıda verilmiştir. Bu veriler Azure 'a gönderilir.

**Veriler** | **WMI sınıfı** | **WMI sınıfı özelliği**
--- | --- | ---
İşlem adı | Win32_Process | ExecutablePath
İşlem bağımsız değişkenleri | Win32_Process | CommandLine
Uygulama adı | Win32_Process | ExecutablePath özelliğinin VersionInfo. ProductName parametresi

#### <a name="linux-vm-app-dependencies-data"></a>Linux VM uygulama bağımlılıkları verileri

Bu, gerecin aracısız bağımlılık analizi için etkinleştirilen her bir Linux VM 'den topladığı bağlantı ve işleme verilerini aşağıda bulabilirsiniz. Bu veriler Azure 'a gönderilir.

**Veriler** | **Kullanılan komut** 
--- | ---
Yerel bağlantı noktası | Netstat 
Yerel IP adresi | Netstat 
Uzak bağlantı noktası | Netstat 
Uzak IP adresi | Netstat 
TCP bağlantı durumu | Netstat 
Etkin bağlantı sayısı | Netstat
İşlem Kimliği  | Netstat 
İşlem adı | PS
İşlem bağımsız değişkenleri | PS
Uygulama adı | dpkg veya rpm



## <a name="collected-data---hyper-v"></a>Toplanan veriler-Hyper-V

Gereç meta verileri, performans verilerini ve bağımlılık analizi verilerini (aracısız [bağımlılık Analizi](concepts-dependency-visualization.md) kullanılıyorsa) toplar.

### <a name="metadata"></a>Meta Veriler
Azure geçişi gereci tarafından bulunan meta veriler, makinelerin ve uygulamaların Azure 'a geçiş için hazır olup olmadığını, makine ve uygulamaları doğru boyuta getirmek, planlama maliyetlerini ve uygulama bağımlılıklarını analiz etmenize yardımcı olur. Microsoft bu verileri hiçbir lisans uyumluluğu denetimine kullanmaz.

Bu, gerecin topladığı ve Azure 'a gönderdiği Hyper-V VM meta verilerinin tam listesidir.

**VERILERI** | **WMı SıNıFı** | **WMı SıNıFı ÖZELLIĞI**
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
DHCP etkin (yapay NIC 'ler) | Msvm_GuestNetworkAdapterConfiguration | DHCPEtkin
NIC KIMLIĞI (yapay NIC 'ler) | Msvm_SyntheticEthernetPortSettingData | InstanceID
NIC MAC adresi (yapay NIC 'ler) | Msvm_SyntheticEthernetPortSettingData | Adres
NIC KIMLIĞI (eski NIC 'ler) | MsvmEmulatedEthernetPortSetting verileri | InstanceID
NIC MAC KIMLIĞI (eski NIC 'ler) | MsvmEmulatedEthernetPortSetting verileri | Adres

### <a name="performance-data"></a>Performans verileri

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


## <a name="collected-data---physical"></a>Toplanan veriler-fiziksel

Gereç meta verileri, performans verilerini ve bağımlılık analizi verilerini (aracısız [bağımlılık Analizi](concepts-dependency-visualization.md) kullanılıyorsa) toplar.

### <a name="windows-metadata"></a>Windows meta verileri

Azure geçişi gereci tarafından bulunan meta veriler, makinelerin ve uygulamaların Azure 'a geçiş için hazır olup olmadığını, makine ve uygulamaları doğru boyuta getirmek, planlama maliyetlerini ve uygulama bağımlılıklarını analiz etmenize yardımcı olur. Microsoft bu verileri hiçbir lisans uyumluluğu denetimine kullanmaz.

Bu, gerecin topladığı ve Azure 'a gönderdiği Windows Server meta verilerinin tam listesidir.

**VERILERI** | **WMı SıNıFı** | **WMı SıNıFı ÖZELLIĞI**
--- | --- | ---
FQDN | Win32_ComputerSystem | Etki alanı, ad, PartOfDomain
İşlemci çekirdek sayısı | Win32_PRocessor | Numberofçekirdekler
Ayrılan bellek | Win32_ComputerSystem | TotalPhysicalMemory
BIOS seri numarası | Win32_ComputerSystemProduct | Identifyingnumber
BıOS GUıD 'SI | Win32_ComputerSystemProduct | EDIN
Önyükleme türü | Win32_DiskPartition | Tür = GPT olan bölümü denetle **:** EFı/BIOS için sistem
İşletim sistemi adı | Win32_OperatingSystem | Başlık
İşletim sistemi sürümü |Win32_OperatingSystem | Sürüm
İşletim sistemi mimarisi | Win32_OperatingSystem | OSArchitecture
Disk sayısı | Win32_DiskDrive | Model, boyut, DeviceID, MediaType, ad
Disk boyutu | Win32_DiskDrive | Boyut
NIC listesi | Win32_NetworkAdapterConfiguration | Açıklama, Dizin
NIC IP adresi | Win32_NetworkAdapterConfiguration | IPAddress
NIC MAC adresi | Win32_NetworkAdapterConfiguration | MACAddress

### <a name="linux-metadata"></a>Linux meta verileri

Bu, gerecin topladığı ve Azure 'a gönderdiği Linux sunucu meta verilerinin tam listesidir.

**VERILERI** | **'Un** 
--- | --- 
FQDN | Cat/proc/sys/kernel/hostname, ana bilgisayar adı-f
İşlemci çekirdek sayısı |  /proc/cpuinfo \| awk '/^ işlemci/{print $3} ' \| WC-l
Ayrılan bellek | Cat/proc/meminfo \| grep MemTotal \| awk ' {printf "%. 0f", $2/1024} '
BIOS seri numarası | lshw \| grep "seri:" \| baş-N1 \| awk ' {Print $2} ' <br/> /usr/sbin/dmidecode-t 1 \| grep ' Serial ' \| awk ' {$1 = ""; $2 = ""; Print} '
BıOS GUıD 'SI | Cat/sys/Class/DMI/ID/product_uuid
Önyükleme türü | [-d/sys/firmware/EFI]  && Echo EFı \| \| echo BIOS
İşletim sistemi adı/sürümü | Bu dosyalara işletim sistemi sürümü ve adı için erişirsiniz:<br/><br/> /etc/OS-Release<br/> /usr/lib/OS-Release <br/> /etc/Enterprise-Release <br/> /etc/redhat-release<br/> /etc/Oracle-Release<br/>  /etc/SuSE-release<br/>  /etc/LSB-Release  <br/> /etc/debian_version
İşletim sistemi mimarisi | UNAME-a
Disk sayısı | Fdisk-l \| egrep ' disk. * bayt ' \| awk ' {Print $2} ' \| Kes-F1-d ': '
Önyükleme diski | df/Boot \| SED-n 2p \| awk ' {Print $1} '
Disk boyutu | Fdisk-l \| egrep ' disk. * bayt ' \| egrep $disk: \| awk ' {Print $5} '
NIC listesi | IP-o-4 addr \| awk ' {print $2} ' göster
NIC IP adresi | IP addr $nic \| grep Inet \| awk ' {Print $2} ' \| Kes-F1-d "/" 
NIC MAC adresi | IP addr $Nic \| grep istatistiklerinden  \| awk ' {Print $2} ' öğesini göster

### <a name="windows-performance-data"></a>Windows performans verileri

Bu, gerecin topladığı ve Azure 'a gönderdiği Windows Server performans verileri aşağıda verilmiştir.

**Veriler** | **WMI sınıfı** | **WMI sınıfı özelliği**
--- | --- | ---
CPU kullanımı | Win32_PerfFormattedData_PerfOS_Processor | PercentIdleTime
Bellek kullanımı | Win32_PerfFormattedData_PerfOS_Memory | Availablembayt
NIC sayısı | Win32_PerfFormattedData_Tcpip_NetworkInterface | Ağ aygıtı sayısını alın.
NIC başına alınan veri | Win32_PerfFormattedData_Tcpip_NetworkInterface  | BytesReceivedPerSec
NIC başına aktarılan veriler | BWin32_PerfFormattedData_Tcpip_NetworkInterface | BytesSentPersec
Disk sayısı | BWin32_PerfFormattedData_PerfDisk_PhysicalDisk | Disk sayısı
Disk ayrıntıları | Win32_PerfFormattedData_PerfDisk_PhysicalDisk | DiskWritesPerSec, DiskWriteBytesPerSec, DiskReadsPerSec, DiskReadBytesPerSec.

### <a name="linux-performance-data"></a>Linux performans verileri

Bu, gerecin topladığı ve Azure 'a gönderdiği Linux sunucu performansı verileri.

**Veriler** | **Linux** 
--- | --- 
CPU kullanımı | Cat/proc/stat/| grep ' CPU '/proc/stat
Bellek kullanımı | ücretsiz \| grep mem \| awk ' {Print $3/$ 2 * 100,0} '
NIC sayısı | lshw sınıfı ağ \| grep ETH [0-60] \| WC-l
NIC başına alınan veri | Cat/sys/Class/net/ETH $ NIC/istatistikler/rx_bytes
NIC başına aktarılan veriler | Cat/sys/Class/net/ETH $ NIC/istatistikler/tx_bytes
Disk sayısı | Fdisk-l \| egrep ' disk. * bayt ' \| awk ' {Print $2} ' \| Kes-F1-d ': '
Disk ayrıntıları | Cat/proc/diskstats


## <a name="appliance-upgrades"></a>Gereç yükseltmeleri

Gereç üzerinde çalışan Azure geçiş aracıları güncelleştirildiğinden, Gereç yükseltilir. Otomatik güncelleştirme, Gereç üzerinde varsayılan olarak etkinleştirildiğinden, bu otomatik olarak gerçekleşir. Bu varsayılan ayarı, Gereç hizmetlerini el ile güncelleştirmek için değiştirebilirsiniz.

### <a name="turn-off-auto-update"></a>Otomatik güncelleştirmeyi devre dışı bırakma

1. Gereci çalıştıran makinede, kayıt defteri düzenleyicisini açın.
2. **HKEY_LOCAL_MACHINE \software\microsoft\azureappliance**adresine gidin.
3. Otomatik güncelleştirmeyi devre dışı bırakmak için, DWORD değeri 0 olan bir kayıt defteri anahtarı **Otomatik güncelleştirme** anahtarı oluşturun.

    ![Kayıt defteri anahtarını ayarla](./media/migrate-appliance/registry-key.png)


### <a name="turn-on-auto-update"></a>Otomatik güncelleştirmeyi Aç

Aşağıdaki yöntemlerden birini kullanarak otomatik güncelleştirme 'yi açabilirsiniz:

- HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\AzureAppliance. 'den otomatik güncelleştirme kayıt defteri anahtarını silerek
- Otomatik güncelleştirmeyi açmak için **önkoşulları ayarlama** bölmesindeki en son güncelleştirme denetimlerinden **gereç hizmetlerini görüntüle** ' ye tıklayın.

Kayıt defteri anahtarını silmek için:

1. Gereci çalıştıran makinede, kayıt defteri düzenleyicisini açın.
2. **HKEY_LOCAL_MACHINE \software\microsoft\azureappliance**adresine gidin.
3. Daha önce otomatik güncelleştirmeyi devre dışı bırakmak için oluşturulmuş kayıt **defteri anahtarını silin**.

Gereç Configuration Manager açmak için, bulma tamamlandıktan sonra:

1. Gereç Yapılandırma Yöneticisi ' nde, **önkoşulları ayarla** paneli ' ne gidin
2. En son güncelleştirmeler denetiminde **gereç hizmetlerini görüntüle** ' ye tıklayın ve otomatik güncelleştirmeyi açmak için bağlantıya tıklayın.

    ![Otomatik güncelleştirmeleri aç](./media/migrate-appliance/autoupdate-off.png)

### <a name="check-the-appliance-services-version"></a>Gereç Hizmetleri sürümünü denetleyin

Aşağıdaki yöntemlerden birini kullanarak gereç Hizmetleri sürümünü denetleyebilirsiniz:

- Gereç Yapılandırma Yöneticisi 'nde **önkoşulları ayarla** paneline gidin.
- Gereç makinesinde, **Denetim Masası**  >  **Programlar ve Özellikler**' de.

Gereç yapılandırma yöneticisini iade etmek için:

1. Gereç Yapılandırma Yöneticisi ' nde, **önkoşulları ayarla** paneli ' ne gidin
2. En son güncelleştirmeler denetiminde **gereç hizmetlerini görüntüle**' ye tıklayın.

    ![Sürümü denetle](./media/migrate-appliance/versions.png)

Denetim masasını denetlemek için:

1. Gereç üzerinde **Başlat**  >  **Denetim Masası**  >  **Programlar ve Özellikler** ' e tıklayın.
2. Listedeki gereç Hizmetleri sürümlerini denetleyin.

    ![Denetim Masası 'nda sürümü denetle](./media/migrate-appliance/programs-features.png)

### <a name="manually-update-an-older-version"></a>Daha eski bir sürümü el ile güncelleştirme

Bileşenlerden herhangi biri için daha eski bir sürüm çalıştırıyorsanız, hizmeti kaldırmanız ve en son sürüme el ile güncelleştirmeniz gerekir.

1. En son gereç hizmeti sürümlerini denetlemek için dosyadaki LatestComponents.js[indirin](https://aka.ms/latestapplianceservices) .
2.    İndirdikten sonra dosyayı Not defteri 'nde LatestComponents.jsaçın.
3. Dosyadaki en son hizmet sürümünü ve bunun için karşıdan yükleme bağlantısını bulun. Örnek:

    "Ad": "ASRMigrationWebApp", "DownloadLink": " https://download.microsoft.com/download/f/3/4/f34b2eb9-cc8d-4978-9ffb-17321ad9b7ed/MicrosoftAzureApplianceConfigurationManager.msi ", "Version": "6.0.211.2", "Md5Hash": "e00a742acc35e78a64a6a81e75469b84"

4.    Güncel olmayan bir hizmetin, dosyadaki indirme bağlantısını kullanarak en son sürümünü indirin.
5. İndirdikten sonra, indirilen MSI bütünlüğünü doğrulamak için yönetici komut penceresinde aşağıdaki komutu çalıştırın.

    ``` C:\>Get-FileHash -Path <file_location> -Algorithm [Hashing Algorithm] ``` Örneğin: C: \> certutil-HashFile C:\Users\public\downloads\MicrosoftAzureApplianceConfigurationManager.MSI MD5

5. Komut çıktısının dosyadaki hizmet için karma değer girdisiyle eşleşip eşleşmediğini denetleyin (örneğin, yukarıdaki MD5 karma değeri).
6. Şimdi, hizmeti yüklemek için MSI 'yi çalıştırın. Bu sessiz bir yüklemedir ve yükleme penceresi bittikten sonra kapanır.
7. Yükleme tamamlandıktan sonra, **Denetim Masası**  >  **Programlar ve Özellikler**' de hizmetin sürümünü denetleyin. Hizmet sürümü artık json dosyasında gösterilen en son sürüme yükseltilmelidir.



## <a name="next-steps"></a>Sonraki adımlar

- VMware için gereci ayarlamayı [öğrenin](how-to-set-up-appliance-vmware.md) .
- Hyper-V için gereci ayarlamayı [öğrenin](how-to-set-up-appliance-hyper-v.md) .
- Fiziksel sunucular için gereci ayarlamayı [öğrenin](how-to-set-up-appliance-physical.md) .

