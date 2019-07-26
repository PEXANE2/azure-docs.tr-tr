---
title: VMware değerlendirmesi ve geçişi için Azure geçişi destek matrisi
description: Azure geçişi hizmetini kullanarak VMware VM 'lerinin değerlendirmesi ve geçirilmesi için destek ayarlarını ve sınırlamaları özetler.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: raynew
ms.openlocfilehash: 00ca474a6cb32c7ad3e47aef750126e958e43501
ms.sourcegitcommit: 57a7d4f67635212f5bf0c56e58fd87c8ec366f2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68372454"
---
# <a name="support-matrix-for-vmware-assessment-and-migration"></a>VMware değerlendirmesi ve geçiş için destek matrisi

Makineleri değerlendirmek ve Microsoft Azure buluta geçirmek için [Azure geçişi hizmetini](migrate-overview.md) kullanabilirsiniz. Bu makalede, şirket içi VMware VM 'lerini değerlendirmek ve geçirmek için destek ayarları ve sınırlamaları özetlenmektedir.


## <a name="vmware-scenarios"></a>VMware senaryoları

Tablo, VMware VM 'Leri için desteklenen senaryoları özetler.

**Dağıtım** | **Ayrıntılar**
--- | ---
**Şirket içi VMware VM 'lerini değerlendirme** | İlk değerlendirmenizi [ayarlayın](tutorial-prepare-vmware.md) .<br/><br/> Büyük ölçekli bir değerlendirme [çalıştırın](scale-vmware-assessment.md) .
**VMware VM 'lerini geçirme** | Aracısız geçiş kullanarak, bazı kısıtlamalarla geçiş yapabilir veya aracı tabanlı bir geçiş kullanabilirsiniz. [Daha fazla bilgi edinin](server-migrate-overview.md)


## <a name="azure-migrate-projects"></a>Azure geçişi projeleri

**Destek** | **Ayrıntılar**
--- | ---
Azure izinleri | Azure geçişi projesi oluşturmak için abonelikte katkıda bulunan veya sahip izinlerinizin olması gerekir.
VMware sınırlamaları  | Tek bir projede en fazla 35.000 VMware VM 'yi değerlendirin.

Bir proje, değerlendirme sınırlarına kadar hem VMware VM 'lerini hem de Hyper-V sanal makinelerini içerebilir.

**Coğrafya** Azure geçişi projesinin oluşturulabileceği birçok coğrafi grafik vardır. Bu coğrafi makinelere yalnızca projeler oluşturabilirsiniz, ancak yine de diğer hedef konumlar için makinelerinizi değerlendirebilir ya da geçirebilirsiniz. Proje Coğrafya yalnızca keşfedilen meta verileri depolamak için kullanılır.


 **Coğrafya** | **Meta veri depolama konumu**
 --- | ---
 Azure Kamu | ABD Devleti Virginia
 Asya Pasifik | Güneydoğu Asya veya Doğu Asya
 Avrupa | Güney Avrupa veya Batı Avrupa
 Birleşik Krallık | UK Güney veya UK Batı
 Amerika Birleşik Devletleri | Orta ABD veya Batı ABD 2


 > [!NOTE]
 > Azure Kamu desteği şu anda yalnızca Azure geçişi 'nin [eski sürümünde](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-versions) sunulmaktadır.


## <a name="assessment-vmware-server-requirements"></a>Değerlendirme-VMware sunucusu gereksinimleri

Bu tablo, VMware sanallaştırma sunucuları için değerlendirme desteğini ve sınırlamalarını özetler.

**Destek** | **Ayrıntılar**
--- | ---
**vCenter sunucusu** | Değerlendirmek istediğiniz VMware VM 'lerinin 5,5, 6,0, 6,5 veya 6,7 çalıştıran bir veya daha fazla vCenter sunucusu tarafından yönetilmesi gerekir.

## <a name="assessment-vcenter-server-permissions"></a>Değerlendirme-vCenter Server izinleri

Yalnızca değerlendirme için, vCenter Server için salt okuma hesabına sahip olmanız gerekir.

## <a name="assessment-appliance-requirements"></a>Değerlendirme-gereç gereksinimleri

**Destek** | **Ayrıntılar**
--- | ---
**ESXi** | Gereç VM 'si, 5,5 veya sonraki bir sürümü çalıştıran bir ESXi konağına dağıtılmalıdır.
**Azure geçişi projesi** | Bir gereç, tek bir projeyle ilişkilendirilebilir.
**vCenter Server** | Bir gereç, vCenter Server en fazla 10.000 VMware VM 'yi bulabilir.<br/> Bir gereç, tek bir vCenter Server bağlanabilir.


## <a name="assessment-url-access-requirements"></a>Değerlendirme-URL erişim gereksinimleri

Azure geçişi gereci internet 'e internet bağlantısı gerektirir.

- Gereci dağıttığınızda Azure geçişi, aşağıdaki tabloda özetlenen URL 'lere bir bağlantı denetimi yapar.
- URL tabanlı bir güvenlik duvarı. Proxy kullanıyorsanız, bu URL 'lere erişim izni verin ve proxy 'nin URL 'Leri ararken alınan CNAME kayıtlarını çözümlediği emin olun.

**URL** | **Ayrıntılar**  
--- | --- |
*.portal.azure.com  | Azure portal Azure geçişi ' ne gidin.
*.windows.net | Azure aboneliğinizde oturum açın.
*.microsoftonline.com | Azure geçişi hizmeti ile iletişim kurmak için gereç için Active Directory uygulamalar oluşturun.
Management.Azure.com | Azure geçişi hizmeti ile iletişim kurmak için gereç için Active Directory uygulamalar oluşturun.
dc.services.visualstudio.com | İç izleme için kullanılan uygulama günlüklerini karşıya yükleyin.
*.vault.azure.net | Azure Key Vault gizli dizileri yönetin.
*.servicebus.windows.net | Gereç ve Azure geçişi hizmeti arasındaki iletişim.
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com <br/> *.hypervrecoverymanager.windowsazure.com | Azure geçişi hizmeti URL 'Lerine bağlanın.
*.blob.core.windows.net | Verileri depolama hesaplarına yükleyin.


## <a name="assessment-port-requirements"></a>Değerlendirme-bağlantı noktası gereksinimleri

**cihaz** | **bağlantı**
--- | ---
Alet | TCP bağlantı noktası 3389 üzerindeki gelen bağlantılar, gereci Uzak Masaüstü bağlantılarına izin vermek için.<br/> 44368 numaralı bağlantı noktasında gereç yönetimi uygulamasına uzaktan erişmek için bağlantı noktası ' deki gelen bağlantılar: https://< gereç-IP-veya-adı >: 44368 <br/>Azure geçişi 'ne bulma ve performans meta verileri göndermek için 443 numaralı bağlantı noktası üzerinden giden bağlantılar.
vCenter sunucusu | TCP bağlantı noktası 443 ' deki gelen bağlantılar, gerecin değerlendirmeler için yapılandırma ve performans meta verilerini toplamasına izin verir. <br/> Gereç, bağlantı noktası 443 ' de varsayılan olarak vCenter 'a bağlanır. VCenter sunucusu farklı bir bağlantı noktasını dinliyorsa, bulmayı ayarlarken bağlantı noktasını değiştirebilirsiniz.


## <a name="agentless-migration-vmware-server-requirements"></a>Aracısız geçiş-VMware sunucusu gereksinimleri

Bu tablo, VMware sanallaştırma sunucuları için değerlendirme desteğini ve sınırlamalarını özetler.

**Destek** | **Ayrıntılar**
--- | ---
**vCenter sunucusu** | Aracısız bir geçiş kullanarak geçireceğiniz VMware VM 'Leri, 5,5, 6,0, 6,5 veya 6,7 çalıştıran bir veya daha fazla vCenter sunucusu tarafından yönetilmelidir.

## <a name="agentless-migration-vcenter-server-permissions"></a>Aracısız geçiş-vCenter Server izinleri

**İzinler** | **Ayrıntılar**
--- | ---
Datastore.Browse | Anlık görüntü oluşturma ve silme sorunlarını gidermek için VM günlük dosyalarına göz atmaya izin verin.
Datastore.LowLevelFileOperations | Anlık görüntü oluşturma ve silme sorunlarını gidermek için veri deposu tarayıcısında okuma/yazma/silme/yeniden adlandırma işlemlerine izin verin.
VirtualMachine.Configuration.DiskChangeTracking | Sanal makine disklerinin değişiklik izlemesini etkinleştirme veya devre dışı bırakma, anlık görüntüler arasında değişen veri bloklarını çekme
VirtualMachine.Configuration.DiskLease | VMware vSphere sanal disk geliştirme seti 'ni (VDDK) kullanarak diski okumak için bir VM 'ye yönelik disk Kiralama işlemlerine izin verin.
VirtualMachine.Provisioning.AllowReadOnlyDiskAccess | VDDK kullanarak diski okumak için bir VM 'de disk açmaya izin verin.
VirtualMachine.Provisioning.AllowVirtualMachineDownload  | , Günlükleri indirmek ve hata oluşursa sorunlarını gidermek için bir VM ile ilişkili dosyalardaki okuma işlemlerine izin verir.
VirtualMachine.SnapshotManagement.* | Çoğaltma için VM anlık görüntülerinin oluşturulmasına ve yönetimine izin verin.
Sanal makine. ınteroff. güç kapalı | Azure 'a geçiş sırasında sanal makinenin kapatılmasını sağlar.


## <a name="agentless-migration-vmware-vm-requirements"></a>Aracısız geçiş-VMware VM gereksinimleri

**Destek** | **Ayrıntılar**
--- | ---
**Desteklenen işletim sistemleri** | Azure tarafından desteklenen [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) ve [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) işletim sistemleri, aracısız geçiş kullanılarak geçirilebilir.
**Azure için gereken değişiklikler** | Bazı VM 'Ler, Azure 'da çalışabilecek şekilde değişiklik gerektirebilir. Azure geçişi, bu değişiklikleri aşağıdaki işletim sistemleri için otomatik olarak yapar:<br/> -Red Hat Enterprise Linux 6.5 +, 7.0 +<br/> -CentOS 6.5 +, 7.0 +</br> -SUSE Linux Enterprise Server 12 SP1 +<br/> -Ubuntu 14.04 LTS, 16.04 LTS, 18.04 LTS<br/> -Detem 7, 8<br/><br/> Diğer işletim sistemleri için geçişten önce el ile ayarlamalar yapmanız gerekir. İlgili makaleler, bunun nasıl yapılacağı hakkında yönergeler içerir.
**Linux önyüklemesi** | /Boot ayrılmış bir bölümse, işletim sistemi diskinde bulunmalı ve birden çok diske yayılmamalıdır.<br/> /Boot kök (/) bölümünün parçasıysa, '/' bölümünün işletim sistemi diskinde olması ve diğer disklere yayılmamamakta olması gerekir.
**UEFı önyüklemesi** | UEFı önyüklemesi olan VM 'Ler geçiş için desteklenmez.
**Şifrelenmiş diskler/birimler** | Şifrelenmiş disklere/birimlere sahip VM 'Ler geçiş için desteklenmez.
**RDM/geçiş diskleri** | VM 'Lerin RDM veya PASSTHROUGH diskleri varsa, bu diskler Azure 'a çoğaltılmaz.
**ALACAĞINI** | VM 'Lere birim olarak bağlanmış NFS birimleri çoğaltılmaz.
**Hedef disk** | VM 'Ler yalnızca Azure 'da yönetilen disklere (Standart HHD, Premium SSD) geçirilebilirler.


## <a name="agentless-migration-appliance-requirements"></a>Aracısız geçiş-gereç gereksinimleri


**Destek** | **Ayrıntılar**
--- | ---
**ESXi** | Gereç VM 'si, 5,5 veya sonraki bir sürümü çalıştıran bir ESXi konağına dağıtılmalıdır.
**Azure geçişi projesi** | Bir gereç, tek bir projeyle ilişkilendirilebilir.
**vCenter Server** | Bir gereç, vCenter Server en fazla 10.000 VMware VM 'yi bulabilir.<br/> Bir gereç, tek bir vCenter Server bağlanabilir.
**VDDK** | Azure geçişi sunucu geçişi ile aracısız bir geçiş çalıştırıyorsanız, Gereç VM 'sinde VMware vSphere sanal disk geliştirme seti (VDDK) yüklü olmalıdır.

## <a name="agentless-migration-url-access-requirements"></a>Aracısız geçiş-URL erişim gereksinimleri

Azure geçişi gereci internet 'e internet bağlantısı gerektirir.

- Gereci dağıttığınızda Azure geçişi, aşağıdaki tabloda özetlenen URL 'lere bir bağlantı denetimi yapar.
- URL tabanlı bir güvenlik duvarı. Proxy kullanıyorsanız, bu URL 'lere erişim izni verin ve proxy 'nin URL 'Leri ararken alınan CNAME kayıtlarını çözümlediği emin olun.

**URL** | **Ayrıntılar**  
--- | ---
*.portal.azure.com | Azure portal Azure geçişi ' ne gidin.
*.windows.net | Azure aboneliğinizde oturum açın.
*.microsoftonline.com | Azure geçişi hizmeti ile iletişim kurmak için gereç için Active Directory uygulamalar oluşturun.
Management.Azure.com | Azure geçişi hizmeti ile iletişim kurmak için gereç için Active Directory uygulamalar oluşturun.
dc.services.visualstudio.com | İç izleme için kullanılan uygulama günlüklerini karşıya yükleyin.
*.vault.azure.net | Azure Key Vault gizli dizileri yönetin.
*.servicebus.windows.net | Gereç ve Azure geçişi hizmeti arasındaki iletişim.
*.discoverysrv.windowsazure.com<br/> *.migration.windowsazure.com<br/> *.hypervrecoverymanager.windowsazure.com | Azure geçişi hizmeti URL 'Lerine bağlanın.
*.blob.core.windows.net | Verileri depolama hesaplarına yükleyin.


## <a name="agentless-migration-port-requirements"></a>Aracısız geçiş-bağlantı noktası gereksinimleri

**cihaz** | **bağlantı**
--- | ---
Alet | Çoğaltılan verileri Azure 'a yüklemek ve çoğaltma ve geçiş için Azure geçişi ile iletişim kurmak için giden TCP bağlantı noktası 3389.
vCenter sunucusu | TCP bağlantı noktası 443 ' deki gelen bağlantılar, gerecin çoğaltma düzenlemesine olanak tanımak için anlık görüntü oluşturma, veri kopyalama, anlık görüntüleri yayınlama
vSphere/EXSI Konağı | Gereç için TCP bağlantı noktası 902 ' den gelen, verileri anlık görüntülerden çoğaltma.


## <a name="agent-based-migration-vmware-server-requirements"></a>Aracı tabanlı geçiş-VMware sunucusu gereksinimleri

Bu tablo, VMware sanallaştırma sunucuları için değerlendirme desteğini ve sınırlamalarını özetler.

**Destek** | **Ayrıntılar**
--- | ---
**vCenter sunucusu/ESXI** | Geçirebileceğiniz VMware VM 'Leri, 5,5, 6,0, 6,5 veya 6,7 çalıştıran bir veya daha fazla vCenter sunucusu tarafından yönetilmelidir veya vSphere sürümü 5,5, 6,0, 6,5 veya 6,7 ile bir ESXI konağında çalışıyor olmalıdır.

### <a name="agent-based-migration-vcenter-server-permissions"></a>Aracı tabanlı geçiş-vCenter Server izinleri

**İzinler** | **Ayrıntılar**
--- | ---
DataStore. AllocateSpace | Bir VM, anlık görüntü, kopya veya sanal disk için bir veri deposunda alan ayırmaya izin verin.
Datastore.Browse | Anlık görüntü oluşturma ve silme sorunlarını gidermek için VM günlük dosyalarına göz atmaya izin verin.
Datastore.LowLevelFileOperations | Anlık görüntü oluşturma/silme sorunlarını gidermek için veri deposu tarayıcısında okuma, yazma, silme ve yeniden adlandırma işlemlerine izin verin.
DataStore. UpdateVirtualMachineFiles | Veri deposu yeniden açıldıktan sonra bir veri deposundaki VM dosyalarına yol güncelleştirmesine izin verin.
Ağ. Atamaağı | Bir VM kaynağına ağ atamaya izin verin.
Atama Virtualmachinetoresourcepool | Bir VM 'nin bir kaynak havuzuna atanmasına izin verin.
Resource. MigratePoweredOffVirtualMachine | Kapalı bir sanal makinenin farklı bir kaynak havuzuna veya konağa geçirilmesine izin verin.
Resource. MigratePoweredOnVirtualMachine | Açık bir sanal makinenin vMotion 'ı kullanarak, farklı bir kaynak havuzuna veya konağa geçişe izin verin.
Tasks. CreateTask | Uzantının Kullanıcı tanımlı bir görev oluşturmasına izin verin.
Tasks. UpdateTask | Uzantının Kullanıcı tanımlı bir görevi güncelleştirmesine izin verin.
VirtualMachine. Configuration. | VM seçeneklerini ve cihazlarını yapılandırmaya izin verin.
Sanal makine. Inter, AnswerQuestion | VM durum geçişleri veya çalışma zamanı hatalarıyla ilgili sorunların çözümlenmesine izin verin.
Sanal makine. ınterndeviceconnection | Bir VM 'nin ayırt bağlanılabilir sanal cihazlarının bağlı durumunun değiştirilmesine izin verin.
Sanal makine. Inter-ConfigureCDMedia | Bir sanal DVD veya CD-ROM cihazının yapılandırılmasına izin verin.
Sanal makine. ınterıconfigurefloppymedia | Sanal disket cihazının yapılandırılmasına izin verin.
Sanal makine. Inter, PowerOff | Azure 'a geçiş sırasında sanal makinenin kapatılmasını sağlar.
Sanal makine. Inter, PowerOn | Kapalı bir VM 'nin kullanılmasına izin verin ve askıya alınmış bir VM 'yi devam ettirin.
Sanal makine. Inter, Vmwarearaçları yüklemesi | VMware araçları CD yükleyicisinin Konuk işletim sistemi için bir CD-ROM olarak bağlanmasına ve takılmasını sağlar.
VirtualMachine. Inventory. CreateNew | VM oluşturulmasına ve gerekli kaynakların ayrılmasına izin verin.
VirtualMachine. Inventory. Register | Mevcut bir VM 'yi bir vCenter Server veya konak envanterine eklemeye izin verin.
VirtualMachine. Inventory. Unregister | Bir vCenter Server veya konak envanterinden bir VMe kaydının kaydının tamamlanmasına izin verin.
VirtualMachine. sağlama. AllowVirtualMachineFilesUpload | VMX, diskler, Günlükler ve NVRAM dahil olmak üzere bir VM ile ilişkili dosyalarda yazma işlemlerine izin verin.
VirtualMachine.Provisioning.AllowVirtualMachineDownload | Sorunları gidermeye yönelik günlükleri indirmek için bir VM ile ilişkili dosyalardaki okuma işlemlerine izin verin.
VirtualMachine. SnapshotManagement. RemoveSnapshot | Anlık görüntü geçmişinden anlık görüntünün kaldırılmasına izin ver.

## <a name="agent-based-migration-replication-appliance-requirements"></a>Aracı tabanlı geçiş-çoğaltma gereç gereksinimleri

Azure geçişi sunucu geçişi ile birlikte VMware VM 'Leri ve fiziksel sunucuları üzerinde aracı tabanlı geçiş için kullanılan [çoğaltma](migrate-replication-appliance.md) gereci gereksinimleri tabloda özetlenmiştir.

> [!NOTE]
> Azure geçişi hub 'ında belirtilen OVA şablonunu kullanarak çoğaltma gerecini ayarladığınızda, Gereç Windows Server 2016 ' i çalıştırır ve destek gereksinimlerine uyar. Çoğaltma gerecini fiziksel bir sunucuda el ile ayarlarsanız, gereksinimlerle uyumlu olduğundan emin olun.



**Bileşen** | **Gereksinim**
--- | ---
 | **VMware ayarları** (VMware VM gereci)
**PowerCLI** | Çoğaltma gereci bir VMware VM üzerinde çalışıyorsa [PowerCLI sürüm 6,0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) 'nin yüklenmesi gerekir.
**NIC türü** | VMXNET3 (gereç bir VMware sanal makinesi ise)
 | **Donanım ayarları**
CPU çekirdekleri | 8
RAM | 16 GB
Disk sayısı | Ünden İşletim sistemi diski, işlem sunucusu önbellek diski ve bekletme sürücüsü.
Boş disk alanı (önbellek) | 600 GB
Boş disk alanı (bekletme diski) | 600 GB
**Yazılım ayarları** |
İşletim sistemi | Windows Server 2016 veya Windows Server 2012 R2
İşletim sistemi yerel ayarı | İngilizce (en-us)
IOCTL | TLS 1,2 etkinleştirilmelidir.
.NET Framework | .NET Framework 4,6 veya üzeri bir sürüm makinede yüklü olmalıdır (güçlü şifreleme etkin.
MySQL | MySQL 'in gereç üzerinde yüklü olması gerekir.<br/> MySQL yüklenmelidir. El ile yükleyebilirsiniz veya Site Recovery gereç dağıtımı sırasında yükleyebilir.
Diğer uygulamalar | Çoğaltma gereci üzerinde diğer uygulamaları çalıştırmamanız gerekir.
Windows Server rolleri | Bu rolleri etkinleştirmeyin: <br> - Active Directory Domain Services <br>- İnternet Bilgi Hizmetleri <br> - Hyper-V
Grup ilkeleri | Bu grup ilkelerini etkinleştirmeyin: <br> -Komut istemine erişimi engelleyin. <br> -Kayıt defteri düzenlemesi araçlarına erişimi engelleyin. <br> -Dosya ekleri için güven mantığı. <br> -Betik yürütmeyi açın. <br> [Daha fazla bilgi edinin](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | -Önceden var olan varsayılan Web sitesi yok <br> -Önceden tanımlı bir Web sitesi/uygulama, 443 bağlantı noktasında dinleniyor <br>- [Anonim kimlik doğrulamasını](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) etkinleştir <br> - [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) ayarını etkinleştir
**Ağ ayarları** |
IP adresi türü | Statik
Bağlantı Noktaları | 443 (Denetim kanalı düzenleme)<br>9443 (Veri aktarımı)
NIC türü | VMXNET3

### <a name="replication-appliance-url-access"></a>Çoğaltma gereç URL 'SI erişimi

Çoğaltma gerecinin bu URL 'Lere erişmesi gerekir.

**URL** | **Ayrıntılar**
--- | ---
\*.backup.windowsazure.com | Çoğaltılan veri aktarımı ve düzenlemesi için kullanılır
\*.store.core.windows.net | Çoğaltılan veri aktarımı ve düzenlemesi için kullanılır
\*.blob.core.windows.net | Çoğaltılan verileri depolayan depolama hesabına erişmek için kullanılır
\*.hypervrecoverymanager.windowsazure.com | Çoğaltma yönetimi işlemleri ve düzenlemesi için kullanılır
https:\//management.azure.com | Çoğaltma yönetimi işlemleri ve düzenlemesi için kullanılır
*.services.visualstudio.com | Telemetri amaçları için kullanılır (isteğe bağlıdır)
time.nist.gov | Sistem ile genel saat arasındaki saat eşitlemesini denetlemek için kullanılır.
time.windows.com | Sistem ile genel saat arasındaki saat eşitlemesini denetlemek için kullanılır.
https:\//login.microsoftonline.com <br/> https:\//secure.aadcdn.microsoftonline-p.com <br/> https:\//Login.Live.com <br/> https:\//Graph.Windows.net <br/> https:\//login.windows.net <br/> https:\//www.Live.com <br/> https:\//www.Microsoft.com  | OVF kurulumunun bu URL 'lere erişmesi gerekiyor. Erişim denetimi ve kimlik yönetimi için Azure Active Directory tarafından kullanılır
https:\//dev.MySQL.com/Get/downloads/MySQLInstaller/MySQL-installer-Community-5.7.20.0.msi | MySQL indirme işleminin tamamlanabilmesi için


#### <a name="mysql-installation-options"></a>MySQL yükleme seçenekleri

MySQL, bu yöntemlerden birini kullanarak çoğaltma gereci üzerine yüklenebilir.

**Yükleme** | **Ayrıntılar**
--- | ---
El ile indir ve yükle | MySQL uygulamasını indirin & C:\Temp\ASRSetup klasörüne yerleştirip el ile yükleyin.<br/> Gereç 'yi ayarlarken, MySQL zaten yüklü olarak gösterilir.
Çevrimiçi indirme | MySQL Yükleyici uygulamasını C:\temp\asrsetupfolder klasörüne yerleştirin. Gereci yüklerken ve MySQL 'i indirip yüklemek için tıkladığınızda, kurulum eklediğiniz yükleyiciyi kullanır.
Azure geçişi 'nden indirin | Gereci yüklerken ve MySQL istendiğinde, **indir ve yükle**' yi seçin.



## <a name="agent-based-migration-vmware-vm-requirements"></a>Aracı tabanlı geçiş-VMware VM gereksinimleri

**Destek** | **Ayrıntılar**
--- | ---
**Makine iş yükü** | Azure geçişi, desteklenen bir makinede çalışan herhangi bir iş yükünün (Active Directory, SQL Server vb.) geçirilmesini destekler.
**İşletim sistemleri** | En son bilgiler için Site Recovery [işletim sistemi desteğini](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines) gözden geçirin. Azure geçişi, özdeş VM işletim sistemi desteği sağlar.
**Linux dosya sistemi/Konuk depolama** | En son bilgiler için Site Recovery [Linux dosya sistemi desteğini](../site-recovery/vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) gözden geçirin. Azure geçişi 'nin aynı Linux dosya sistemi desteği vardır.
**Ağ/depolama** | En son bilgiler için, Site Recovery için [ağ](../site-recovery/vmware-physical-azure-support-matrix.md#network) ve [depolama](../site-recovery/vmware-physical-azure-support-matrix.md#storage) önkoşullarını gözden geçirin. Azure geçişi, aynı ağ/depolama gereksinimlerini sağlar.
**Azure gereksinimleri** | En son bilgiler için Site Recovery [Azure ağ](../site-recovery/vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover), [depolama](../site-recovery/vmware-physical-azure-support-matrix.md#azure-storage)ve [işlem](../site-recovery/vmware-physical-azure-support-matrix.md#azure-compute) gereksinimlerini gözden geçirin. Azure geçişi, VMware geçişi için aynı gereksinimlere sahiptir.
**Ulaşım hizmeti** | Taşımak istediğiniz her VM 'ye Mobility hizmeti aracısının yüklenmesi gerekir.
**Hedef disk** | VM 'Ler yalnızca Azure 'da yönetilen disklere (Standart HHD, Premium SSD) geçirilebilirler.



## <a name="agent-based-migration-url-access-requirements"></a>Aracı tabanlı geçiş-URL erişim gereksinimleri

VMware VM 'lerinde çalışan Mobility hizmeti internet 'e internet bağlantısı gerektirir.

- Mobility hizmetini dağıttığınızda, aşağıdaki tabloda özetlenen URL 'lere bir bağlantı denetimi yapılır.
- URL tabanlı bir güvenlik duvarı. Proxy kullanıyorsanız, bu URL 'lere erişim izni verin ve proxy 'nin URL 'Leri ararken alınan CNAME kayıtlarını çözümlediği emin olun.

**URL** | **Ayrıntılar**  
--- | ---
*.portal.azure.com | Azure portal Azure geçişi ' ne gidin.
*.windows.net | Azure aboneliğinizde oturum açın.
*.microsoftonline.com | Azure geçişi hizmeti ile iletişim kurmak için gereç için Active Directory uygulamalar oluşturun.
Management.Azure.com | Azure geçişi hizmeti ile iletişim kurmak için gereç için Active Directory uygulamalar oluşturun.
dc.services.visualstudio.com | İç izleme için kullanılan uygulama günlüklerini karşıya yükleyin.
*.vault.azure.net | Azure Key Vault gizli dizileri yönetin.
*.servicebus.windows.net | Gereç ve Azure geçişi hizmeti arasındaki iletişim.
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com <br/> *.hypervrecoverymanager.windowsazure.com | Azure geçişi hizmeti URL 'Lerine bağlanın.
*.blob.core.windows.net | Verileri depolama hesaplarına yükleyin.

## <a name="agent-based-migration-port-requirements"></a>Aracı tabanlı geçiş-bağlantı noktası gereksinimleri

**cihaz** | **bağlantı**
--- | ---
VM | VM 'lerde çalışan Mobility hizmeti, çoğaltma yönetimi için HTTPS 443 gelen bağlantı noktasında şirket içi yapılandırma sunucusu ile iletişim kurar.<br/><br/> VM 'Ler, çoğaltma verilerini işlem sunucusuna (yapılandırma sunucusu makinesinde çalışan), HTTPS 9443 gelen bağlantı noktası üzerinden gönderir. Bu bağlantı noktası değiştirilebilir.
Çoğaltma gereci | Çoğaltma gereci, HTTPS 443 giden bağlantı noktası üzerinden Azure ile çoğaltmayı düzenler.
İşlem sunucusu | İşlem sunucusu çoğaltma verilerini alır, bunları iyileştirir ve şifreler ve 443 giden bağlantı noktası üzerinden Azure depolama 'ya gönderir.<br/> Varsayılan olarak, işlem sunucusu çoğaltma gereci üzerinde çalışır.

## <a name="azure-vm-requirements"></a>Azure VM gereksinimleri

Azure 'a çoğaltılan tüm şirket içi VM 'Lerin bu tabloda özetlenen Azure VM gereksinimlerini karşılaması gerekir. Site Recovery, çoğaltma için bir önkoşul denetimi çalıştırdığında, bazı gereksinimler karşılanmazsa denetim başarısız olur.

**Bileşen** | **Gereksinimler** | **Ayrıntılar**
--- | --- | ---
Konuk işletim sistemi | [Aracısız çoğaltma kullanarak VMware VM 'leri](#agentless-migration-vmware-vm-requirements)için desteklenen işletim sistemlerini ve [aracı tabanlı çoğaltma kullanan VMware VM](#agent-based-migration-vmware-vm-requirements)'lerini doğrulayın.<br/> Desteklenen bir işletim sistemi üzerinde çalışan herhangi bir iş yükünü geçirebilirsiniz. | Desteklenmiyorsa denetim başarısız olur.
Konuk işletim sistemi mimarisi | 64 bit. | Desteklenmiyorsa denetim başarısız olur.
İşletim sistemi disk boyutu | 2\.048 GB 'a kadar. | Desteklenmiyorsa denetim başarısız olur.
İşletim sistemi disk sayısı | 1\. | Desteklenmiyorsa denetim başarısız olur.
Veri diski sayısı | 64 veya daha az. | Desteklenmiyorsa denetim başarısız olur.
Veri diski boyutu | 4\.095 GB 'a kadar | Desteklenmiyorsa denetim başarısız olur.
Ağ bağdaştırıcıları | Birden çok bağdaştırıcı desteklenir. |
Paylaşılan VHD | Desteklenmiyor. | Desteklenmiyorsa denetim başarısız olur.
FC diski | Desteklenmiyor. | Desteklenmiyorsa denetim başarısız olur.
BitLocker | Desteklenmiyor. | Bir makine için çoğaltmayı etkinleştirmeden önce BitLocker devre dışı bırakılmalıdır.
VM adı | 1 ile 63 karakter arasında.<br/> Harfler, sayılar ve kısa çizgilerden oluşabilir.<br/><br/> Makine adı bir harf veya sayıyla başlamalı ve bitmelidir. |  Site Recovery makine özelliklerindeki değeri güncelleştirin.
Geçişten sonra Bağlan-Windows | Geçişten sonra Windows çalıştıran Azure VM 'lerine bağlanmak için:<br/> -Geçişten önce şirket içi VM 'de RDP 'yi etkinleştirin. TCP ve UDP kurallarının **Ortak** profil için eklendiğinden ve tüm profillerde **Windows Güvenlik Duvarı** > **İzin Verilen Uygulamalar** içinde RDP’ye izin verildiğinden emin olun.<br/> Siteden siteye VPN erişimi için, RDP 'yi etkinleştirin ve **etki alanı ve özel** ağlar için **Windows Güvenlik Duvarı** -> **izin verilen uygulamalar ve Özellikler** ' de RDP 'ye izin verin. Ayrıca, işletim sisteminin SAN ilkesinin **OnlineAll**olarak ayarlandığından emin olun. [Daha fazla bilgi edinin](https://support.microsoft.com/kb/3031135). |
Geçişten sonra Bağlan-Linux | SSH kullanarak geçişten sonra Azure VM 'lerine bağlanmak için:<br/> Geçişten önce, şirket içi makinede, Secure Shell hizmetinin başlangıç olarak ayarlandığını ve Güvenlik Duvarı kurallarının bir SSH bağlantısına izin vermeyi kontrol edin.<br/> Yük devretmenin ardından Azure VM 'de, yük devredilen VM 'deki ağ güvenlik grubu kuralları için SSH bağlantı noktasına gelen bağlantılara ve bağlı olduğu Azure alt ağına izin verin. Ayrıca, VM için bir genel IP adresi ekleyin. |  


## <a name="next-steps"></a>Sonraki adımlar

VMware değerlendirmesi ve geçiş [Için hazırlanma](tutorial-prepare-vmware.md) .
