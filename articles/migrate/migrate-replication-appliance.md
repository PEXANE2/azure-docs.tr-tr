---
title: Azure Geçişi çoğaltma gereci
description: Aracı tabanlı VMWare geçişi için Azure geçişi çoğaltma gereci hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 01/30/2020
ms.openlocfilehash: 4521fce6310b319d155a2f0c418cd934be7e2cb8
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78388967"
---
# <a name="replication-appliance"></a>Çoğaltma gereci

Bu makalede, [Azure geçişi](migrate-services-overview.md#azure-migrate-server-migration-tool) tarafından kullanılan çoğaltma gereci açıklanmaktadır: VMware VM 'leri, fiziksel makineler ve özel/genel bulut VM 'lerini, aracı tabanlı geçiş kullanarak Azure 'a geçirme. 


## <a name="overview"></a>Genel Bakış

VMware VM 'Leri veya fiziksel sunucuları için aracı tabanlı geçiş ayarladığınızda, çoğaltma gereci dağıtılır. VMware VM ya da fiziksel sunucu olarak tek bir şirket içi makine olarak dağıtılır. Çalışır:

- **Çoğaltma**gereci: çoğaltma gereci iletişimleri koordine eder ve Azure 'a çoğaltılan şirket Içi VMware VM 'leri ve fiziksel sunucular için veri çoğaltmasını yönetir.
- **İşlem sunucusu**: çoğaltma gereci üzerinde varsayılan olarak yüklenen işlem sunucusu ve şunları yapar:
    - **Çoğaltma ağ geçidi**: çoğaltma ağ geçidi olarak davranır. Çoğaltma için etkinleştirilen makinelerden çoğaltma verilerini alır. Önbelleğe alma, sıkıştırma ve şifreleme ile çoğaltma verilerini iyileştirir ve Azure 'a gönderir.
    - **Aracı yükleyicisi**: Mobility hizmetinin anında yüklenmesini gerçekleştirir. Bu hizmet, geçiş için çoğaltmak istediğiniz her şirket içi makinede yüklü ve çalışır olmalıdır.

## <a name="appliance-deployment"></a>Gereç dağıtımı

**Kullanıldığı yer** | **Ayrıntılar**
--- |  ---
VMware VM Aracısı tabanlı geçiş | OVA şablonunu Azure geçiş hub 'ından indirir ve gereç sanal makinesini oluşturmak için vCenter Server içeri aktarabilirsiniz.
Fiziksel makine Aracısı tabanlı geçiş | Bir VMware altyapınız yoksa veya bir OVA şablonu kullanarak bir VMware VM 'si oluşturmazsanız, Azure geçiş hub 'ından bir yazılım yükleyicisi indirir ve gereci makinesini kurmak için çalıştırırsınız.

## <a name="appliance-requirements"></a>Gereç gereksinimleri

Azure geçişi hub 'ında belirtilen OVA şablonunu kullanarak çoğaltma gerecini ayarladığınızda, Gereç Windows Server 2016 ' i çalıştırır ve destek gereksinimlerine uyar. Çoğaltma gerecini fiziksel bir sunucuda el ile ayarlarsanız, gereksinimlerle uyumlu olduğundan emin olun.

**Bileşen** | **Gereksinim**
--- | ---
 | **VMware VM gereci**
PowerCLI | Çoğaltma gereci bir VMware VM üzerinde çalışıyorsa [PowerCLI sürüm 6,0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) 'nin yüklenmesi gerekir.
NIC türü | VMXNET3 (gereç bir VMware sanal makinesi ise)
 | **Donanım ayarları**
CPU çekirdekleri | 8
RAM | 16 GB
Disk sayısı | Üç: işletim sistemi diski, işlem sunucusu önbellek diski ve bekletme sürücüsü.
Boş disk alanı (önbellek) | 600 GB
Boş disk alanı (bekletme diski) | 600 GB
**Yazılım ayarları** |
İşletim sistemi | Windows Server 2016 veya Windows Server 2012 R2
Lisans | Gereç, 180 gün için geçerli olan bir Windows Server 2016 değerlendirme lisansıyla gelir.<br/><br/> Değerlendirme süresi sona ermeden yakın ise, yeni bir gereç indirmeniz ve dağıtmanız ya da gereç sanal makinesinin işletim sistemi lisansını etkinleştirmenizi öneririz.
İşletim sistemi yerel ayarı | İngilizce (en-us)
TLS | TLS 1,2 etkinleştirilmelidir.
.NET Framework | .NET Framework 4,6 veya üzeri bir sürüm makinede yüklü olmalıdır (güçlü şifreleme etkin.
MySQL | MySQL 'in gereç üzerinde yüklü olması gerekir.<br/> MySQL yüklenmelidir. El ile yükleyebilirsiniz veya Site Recovery gereç dağıtımı sırasında yükleyebilir.
Diğer uygulamalar | Çoğaltma gereci üzerinde başka uygulamalar çalıştırmayın.
Windows Server rolleri | Bu rolleri etkinleştirmeyin: <br> - Active Directory Domain Services <br>- İnternet Bilgi Hizmetleri <br> - Hyper-V
Grup ilkeleri | Bu grup ilkelerini etkinleştirmeyin: <br> -Komut istemine erişimi engelleyin. <br> -Kayıt defteri düzenlemesi araçlarına erişimi engelleyin. <br> -Dosya ekleri için güven mantığı. <br> -Betik yürütmeyi açın. <br> [Daha fazla bilgi](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | -Önceden var olan varsayılan Web sitesi yok <br> -Var olan bir Web sitesi/uygulama dinleme 443 bağlantı noktasında dinleniyor <br>- [Anonim kimlik doğrulamasını](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) etkinleştir <br> - [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) ayarını etkinleştir
**Ağ ayarları** |
IP adresi türü | Statik
Bağlantı Noktaları | 443 (Denetim kanalı düzenleme)<br>9443 (Veri aktarımı)
NIC türü | VMXNET3

## <a name="mysql-installation"></a>MySQL yüklemesi 

MySQL, çoğaltma gereç makinesinde yüklü olmalıdır. Bu yöntemlerin biri kullanılarak yüklenebilir.

**Yöntem** | **Ayrıntılar**
--- | ---
El ile indir ve yükle | MySQL uygulamasını indirin & C:\Temp\ASRSetup klasörüne yerleştirip el ile yükleyin.<br/> Gereç 'yi ayarlarken, MySQL zaten yüklü olarak gösterilir.
Çevrimiçi indirme olmadan | MySQL Yükleyici uygulamasını C:\temp\asrsetupfolder klasörüne yerleştirin. Gereci yüklerken ve MySQL 'i indirip yüklemek için tıkladığınızda, kurulum eklediğiniz yükleyiciyi kullanır.
Azure geçişi 'ne indirme ve yükleme | Gereci yüklerken ve MySQL istendiğinde, **indir ve yükle**' yi seçin.

## <a name="url-access"></a>URL erişimi

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
https:\//login.microsoftonline.com <br/> https:\//secure.aadcdn.microsoftonline-p.com <br/> https:\//login.live.com <br/> https:\//graph.windows.net <br/> https:\//login.windows.net <br/> https:\//www.live.com <br/> https:\//www.microsoft.com  | OVF kurulumunun bu URL 'lere erişmesi gerekiyor. Erişim denetimi ve kimlik yönetimi için Azure Active Directory tarafından kullanılır
https:\//dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi | MySQL indirme işleminin tamamlanabilmesi için

## <a name="port-access"></a>Bağlantı noktası erişimi

**Aygıtların** | **Bağlanma**
--- | ---
VM'ler | VM 'lerde çalışan Mobility hizmeti, çoğaltma yönetimi için HTTPS 443 gelen bağlantı noktasında şirket içi çoğaltma gereci (yapılandırma sunucusu) ile iletişim kurar.<br/><br/> VM 'Ler, çoğaltma verilerini işlem sunucusuna (yapılandırma sunucusu makinesinde çalışan), HTTPS 9443 gelen bağlantı noktası üzerinden gönderir. Bu bağlantı noktası değiştirilebilir.
Çoğaltma gereci | Çoğaltma gereci, HTTPS 443 giden bağlantı noktası üzerinden Azure ile çoğaltmayı düzenler.
İşlem sunucusu | İşlem sunucusu çoğaltma verilerini alır, bu verileri iyileştirir ve şifreler ve 443 giden bağlantı noktası üzerinden Azure depolama 'ya gönderir.<br/> Varsayılan olarak, işlem sunucusu çoğaltma gereci üzerinde çalışır.


## <a name="replication-process"></a>Çoğaltma işlemi

1. Bir sanal makine için çoğaltmayı etkinleştirdiğinizde, Azure Storage 'a ilk çoğaltma, belirtilen çoğaltma ilkesi kullanılarak başlar. 
2. Trafik, internet üzerinden Azure depolama genel uç noktalarına çoğaltılır. Şirket içi siteden Azure 'a siteden siteye sanal özel ağ (VPN) üzerinden trafik çoğaltma desteklenmez.
3. İlk çoğaltma tamamlandıktan sonra Delta çoğaltma başlar. Bir makine için izlenen değişiklikler günlüğe kaydedilir.
4. İletişim şu şekilde gerçekleşir:
    - VM 'Ler, çoğaltma yönetimi için HTTPS 443 gelen bağlantı noktasında çoğaltma gereci ile iletişim kurar.
    - Çoğaltma gereci, HTTPS 443 giden bağlantı noktası üzerinden Azure ile çoğaltmayı düzenler.
    - VM 'Ler, çoğaltma verilerini işlem sunucusuna (çoğaltma gereci üzerinde çalışır), HTTPS 9443 gelen bağlantı noktası üzerinden gönderir. Bu bağlantı noktası değiştirilebilir.
    - İşlem sunucusu çoğaltma verilerini alır, bunları iyileştirir ve şifreler ve 443 giden bağlantı noktası üzerinden Azure depolama 'ya gönderir.
5. Çoğaltma verileri günlüğü, ilk olarak Azure 'da bir önbellek depolama hesabında yer açar. Bu Günlükler işlenir ve veriler bir Azure yönetilen diskinde depolanır.

![Mimari](./media/migrate-replication-appliance/architecture.png)

## <a name="appliance-upgrades"></a>Gereç yükseltmeleri

Gereç, Azure geçiş hub 'ından el ile yükseltilir. En son sürümü her zaman çalıştırmanızı öneririz.

1. Azure geçişi > sunucuları > Azure geçişi: Sunucu değerlendirmesi, altyapı sunucuları, **yapılandırma sunucuları**' na tıklayın.
2. **Yapılandırma sunucularında**, çoğaltma gerecinin yeni bir sürümü kullanılabilir olduğunda **Aracı sürümünde** bir bağlantı görüntülenir. 
3. Yükleyiciyi, çoğaltma gereci makinesine indirin ve yükseltmeyi yükleyin. Yükleyici, Gereç üzerinde çalışan geçerli sürümü algılar.
 
## <a name="next-steps"></a>Sonraki adımlar

- Aracı tabanlı VMware VM geçişi için çoğaltma gerecini ayarlamayı [öğrenin](tutorial-migrate-vmware-agent.md#set-up-the-replication-appliance) .
- Fiziksel sunucular için çoğaltma gerecini ayarlamayı [öğrenin](tutorial-migrate-physical-virtual-machines.md#set-up-the-replication-appliance) .
