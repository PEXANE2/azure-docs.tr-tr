---
title: Azure Geçişi çoğaltma gereci
description: Aracı tabanlı VMWare geçişi için Azure Geçiş çoğaltma cihazı hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 01/30/2020
ms.openlocfilehash: 85641f514fc4367f02901eb1dd394cfa204c3ec4
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81535222"
---
# <a name="replication-appliance"></a>Çoğaltma cihazı

Bu [makalede, Aracı](migrate-services-overview.md#azure-migrate-server-migration-tool) tabanlı geçiş kullanarak VMware VM'leri, fiziksel makineleri ve özel/genel bulut VM'lerini Azure'a geçirerken Sunucu Geçişi aracı tarafından kullanılan çoğaltma cihazı açıklanmaktadır. 


## <a name="overview"></a>Genel Bakış

Çoğaltma cihazı, VMware VM'lerinin veya fiziksel sunucuların aracı tabanlı geçişini ayarladığınızda dağıtılır. VMware VM veya fiziksel sunucu olarak tek bir şirket içi makine olarak dağıtılır. Çalışır:

- Çoğaltma cihazı : Çoğaltma **cihazı,** şirket içi VMware VM'ler ve Azure'a çoğalan fiziksel sunucular için iletişimi koordine eder ve veri çoğaltmaişlemini yönetir.
- **İşlem sunucusu**: Çoğaltma cihazına varsayılan olarak yüklenen ve aşağıdakileri yapan işlem sunucusu:
    - **Çoğaltma ağ geçidi**: Çoğaltma ağ geçidi görevi görür. Çoğaltma için etkinleştirilen makinelerden çoğaltma verileri alır. Önbelleğe alma, sıkıştırma ve şifreleme ile çoğaltma verilerini optimize eder ve Azure'a gönderir.
    - **Aracı yükleyici**: Mobilite Hizmetinin itme yüklemesini gerçekleştirir. Bu hizmet, geçiş için çoğaltmak istediğiniz her şirket içi makineye yüklenmeli ve çalıştırılmalıdır.

## <a name="appliance-deployment"></a>Cihaz dağıtımı

**Kullanıldığı yerler** | **Şey**
--- |  ---
**VMware VM aracı tabanlı geçiş** | Azure Geçiş hub'ından OVA şablonu indirin ve vM cihazını oluşturmak için vCenter Server'a içe aktarın.
**Fiziksel makine aracısı tabanlı geçiş** | VMware altyapınız yoksa veya OVA şablonu kullanarak bir VMware VM oluşturamıyorsanız, Azure Geçiş hub'ından bir yazılım yükleyicisi indirip beyaz eşya makinesini kurmak için çalıştırın.

> [!NOTE]
> Azure Kamu'da dağıtım yapmaktaysanız, çoğaltma cihazını dağıtmak için yükleme dosyasını kullanın.

## <a name="appliance-requirements"></a>Cihaz gereksinimleri

Çoğaltma cihazını Azure Geçiş hub'ında sağlanan OVA şablonu kullanılarak ayarladığınızda, cihaz Windows Server 2016'yı çalıştırıyor ve destek gereksinimlerine uyuyor. Çoğaltma cihazını fiziksel bir sunucuda el ile ayarlarsanız, gereksinimlerine uyduğundan emin olun.

**Bileşen** | **Gereksinim**
--- | ---
 | **VMware VM cihazı**
PowerCLI | Çoğaltma cihazı Bir VMware VM üzerinde çalışıyorsa [PowerCLI sürüm 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) takılmalıdır.
NIC türü | VMXNET3 (cihaz vmware VM ise)
 | **Donanım ayarları**
CPU çekirdekleri | 8
RAM | 16 GB
Disk sayısı | Üç: Işletim sistemi diski, işlem sunucusu önbellek diski ve bekletme sürücüsü.
Boş disk alanı (önbellek) | 600 GB
Boş disk alanı (bekletme diski) | 600 GB
**Yazılım ayarları** |
İşletim sistemi | Windows Server 2016 veya Windows Server 2012 R2
Lisans | Cihaz, 180 gün boyunca geçerli olan Bir Windows Server 2016 değerlendirme lisansıile birlikte gelir.<br/><br/> Değerlendirme süresi sona ermek üzereyse, yeni bir cihaz indirip dağıtmanızı veya cihaz VM'nin işletim sistemi lisansını etkinleştirmenizi öneririz.
İşletim sistemi yerel ayarı | İngilizce (en-us)
TLS | TLS 1.2 etkinleştirilmelidir.
.NET Framework | .NET Framework 4.6 veya daha sonra (güçlü şifreleme etkin) makineye monte edilmelidir.
MySQL | MySQL cihaza monte edilmelidir.<br/> MySQL yüklü olmalıdır. El ile yükleyebilirsiniz veya Site Kurtarma cihaz dağıtımı sırasında yükleyebilir.
Diğer uygulamalar | Çoğaltma cihazında başka uygulamalar çalıştırmayın.
Windows Server rolleri | Bu rolleri etkinleştirme: <br> - Active Directory Domain Services <br>- İnternet Bilgi Hizmetleri <br> - Hyper-V
Grup ilkeleri | Bu grup ilkelerini etkinleştirme: <br> - Komut istemine erişimi engelleyin. <br> - Kayıt defteri düzenleme araçlarına erişimi engelleyin. <br> - Dosya ekleri için güven mantığı. <br> - Komut Dosyası Yürütme'yi açın. <br> [Daha fazlasını öğrenin](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | - Önceden varolan varsayılan web sitesi yok <br> - 443 nolu bağlantı noktası üzerinde önceden var olan web sitesi/uygulama dinleme <br>- [Anonim kimlik doğrulamayı etkinleştirme](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> - [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) ayarını etkinleştirin
**Ağ ayarları** |
IP adresi türü | Statik
Bağlantı Noktaları | 443 (Denetim kanalı düzenleme)<br>9443 (Veri aktarımı)
NIC türü | VMXNET3

## <a name="mysql-installation"></a>MySQL yüklemesi 

MySQL çoğaltma cihazı makinesine takılmalıdır. Bu yöntemlerden biri kullanılarak yüklenebilir.

**Yöntem** | **Şey**
--- | ---
El ile indirin ve kurun | MySQL uygulamasını & C:\Temp\ASRSetup klasörüne yerleştirin, ardından el ile yükleyin.<br/> Cihazı kurduğunuzda MySQL zaten yüklü olarak gösterecektir.
Online indirme olmadan | MySQL yükleyici uygulamasını C:\Temp\ASRSetup klasörüne yerleştirin. Cihazı yüklediğinizde ve MySQL'i indirmek ve yüklemek için tıklattığınızda, kurulum eklediğiniz yükleyiciyi kullanır.
Azure Geçiş'te karşıdan yükleme ve yükleme | Cihazı yüklediğinizde ve MySQL için istendiğinde **İndir ve yükle'yi**seçin.

## <a name="url-access"></a>URL erişimi

Çoğaltma cihazının Azure genel bulutundaki bu URL'lere erişmesi gerekir.

**URL** | **Şey**
--- | ---
\*.backup.windowsazure.com | Çoğaltılan veri aktarımı ve koordinasyonu için kullanılır
\*.store.core.windows.net | Çoğaltılan veri aktarımı ve koordinasyonu için kullanılır
\*.blob.core.windows.net | Çoğaltılan verileri depolayan depolama hesabına erişmek için kullanılır
\*.hypervrecoverymanager.windowsazure.com | Çoğaltma yönetimi işlemleri ve koordinasyonu için kullanılır
https:\//management.azure.com | Çoğaltma yönetimi işlemleri ve koordinasyonu için kullanılır
*.services.visualstudio.com | Telemetri amaçlı kullanılır (İsteğe bağlıdır)
time.windows.com | Sistem ile genel saat arasındaki saat eşitlemesini denetlemek için kullanılır.
https:\//login.microsoftonline.com <br/> https:\//secure.aadcdn.microsoftonline-p.com <br/> https:\//login.live.com <br/> https:\//graph.windows.net <br/> https:\//login.windows.net <br/> https:\//www.live.com <br/> https:\//www.microsoft.com  | Cihaz kurulumunun bu URL'lere erişmesi gerekir. Azure Active Directory tarafından erişim denetimi ve kimlik yönetimi için kullanılırlar
https:\//dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi | MySQL download tamamlamak için. Birkaç bölgede, karşıdan yükleme CDN URL'sine yönlendirilebilir. Gerekirse CDN URL'ye de izin verildiğinden emin olun.


## <a name="azure-government-url-access"></a>Azure Resmi URL erişimi

Çoğaltma cihazının Azure Kamu'da bu URL'lere erişmesi gerekir.

**URL** | **Şey**
--- | ---
\*.backup.windowsazure.us | Çoğaltılan veri aktarımı ve koordinasyonu için kullanılır
\*.store.core.windows.net | Çoğaltılan veri aktarımı ve koordinasyonu için kullanılır
\*.blob.core.windows.net | Çoğaltılan verileri depolayan depolama hesabına erişmek için kullanılır
\*.hypervrecoverymanager.windowsazure.us | Çoğaltma yönetimi işlemleri ve koordinasyonu için kullanılır
https:\//management.usgovcloudapi.net | Çoğaltma yönetimi işlemleri ve koordinasyonu için kullanılır
*.services.visualstudio.com | Telemetri amaçlı kullanılır (İsteğe bağlıdır)
time.nist.gov | Sistem ile genel saat arasındaki saat eşitlemesini denetlemek için kullanılır.
https:\//login.microsoftonline.com <br/> https:\//secure.aadcdn.microsoftonline-p.com <br/> https:\//login.live.com <br/> https:\//graph.windows.net <br/> https:\//login.windows.net <br/> https:\//www.live.com <br/> https:\//www.microsoft.com  | OVA ile cihaz kurulumu nun bu URL'lere erişmesi gerekir. Azure Active Directory tarafından erişim denetimi ve kimlik yönetimi için kullanılırlar.
https:\//dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi | MySQL download tamamlamak için. Birkaç bölgede, karşıdan yükleme CDN URL'sine yönlendirilebilir. Gerekirse CDN URL'ye de izin verildiğinden emin olun.

## <a name="port-access"></a>Bağlantı noktası erişimi

**Cihaz** | **Bağlantı**
--- | ---
VM'ler | VM'lerde çalışan Mobilite hizmeti, çoğaltma yönetimi için https 443 gelen bağlantı noktasında ki şirket içi çoğaltma cihazıyla (yapılandırma sunucusu) iletişim kurar.<br/><br/> VM'ler çoğaltma verilerini HTTPS 9443 bağlantı noktasındaki işlem sunucusuna (yapılandırma sunucusu makinesinde çalışan) gönderir. Bu bağlantı noktası değiştirilebilir.
Çoğaltma cihazı | Çoğaltma cihazı, https 443 giden bağlantı noktası üzerinden Azure ile çoğaltmayı yönetir.
İşlem sunucusu | İşlem sunucusu çoğaltma verilerini alır, en iyi duruma getirir ve şifreler ve 443 bağlantı noktası üzerinden Azure depolamasına gönderir.<br/> Varsayılan olarak işlem sunucusu çoğaltma cihazı üzerinde çalışır.


## <a name="replication-process"></a>Çoğaltma işlemi

1. Bir VM için çoğaltmayı etkinleştirdiğinizde, belirtilen çoğaltma ilkesini kullanarak Azure depolamasına ilk çoğaltma başlar. 
2. Trafik, Internet üzerinden Azure depolama ortak uç noktalarına çoğalır. Şirket içi bir siteden Azure'a siteden siteye sanal özel ağ (VPN) üzerinden trafiği çoğaltmak desteklenmez.
3. İlk çoğaltma bittikten sonra, delta çoğaltma başlar. Bir makine için izlenen değişiklikler günlüğe kaydedilir.
4. İletişim aşağıdaki gibi olur:
    - VM'ler, çoğaltma yönetimi için HTTPS 443'ün gelen bağlantı noktasındaki çoğaltma cihazıyla iletişim kurar.
    - Çoğaltma cihazı, https 443 giden bağlantı noktası üzerinden Azure ile çoğaltmayı yönetir.
    - VM'ler çoğaltma verilerini HTTPS 9443 gelen bağlantı noktasındaki işlem sunucusuna (çoğaltma cihazında çalışan) gönderir. Bu bağlantı noktası değiştirilebilir.
    - İşlem sunucusu çoğaltma verilerini alır, en iyi duruma getirir ve şifreler ve 443 bağlantı noktası üzerinden Azure depolamasına gönderir.
5. Çoğaltma verileri ilk olarak Azure'daki bir önbellek depolama hesabında yer açar. Bu günlükler işlenir ve veriler Azure yönetilen bir diskte depolanır.

![Mimari](./media/migrate-replication-appliance/architecture.png)

## <a name="appliance-upgrades"></a>Cihaz yükseltmeleri

Cihaz Azure Geçiş merkezinden el ile yükseltilir. Her zaman en son sürümü çalıştırmanızı öneririz.

1. Azure Geçiş > > Sunucularında Azure Geçiş: Sunucu Değerlendirmesi, Altyapı sunucuları, **Yapılandırma sunucularını**tıklatın.
2. **Yapılandırma sunucularında,** çoğaltma cihazının yeni bir sürümü kullanılabilir olduğunda **Agent Version'da** bir bağlantı görüntülenir. 
3. Yükleyiciyi çoğaltma cihazı makinesine indirin ve yükseltmeyi yükleyin. Yükleyici, cihazın üzerinde çalışan sürüm akımını algılar.
 
## <a name="next-steps"></a>Sonraki adımlar

- Aracı tabanlı VMware VM geçişi için çoğaltma cihazını nasıl ayarlayatıyarı [öğrenin.](tutorial-migrate-vmware-agent.md#set-up-the-replication-appliance)
- Fiziksel sunucular için çoğaltma cihazını nasıl ayarlayacağınızı [öğrenin.](tutorial-migrate-physical-virtual-machines.md#set-up-the-replication-appliance)
