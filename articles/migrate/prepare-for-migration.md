---
title: Azure Geçiş ile geçiş için makineler hazırlama
description: Azure Geçişi ile şirket içi makineleri geçiş için nasıl hazırlayacağınızı öğrenin.
ms.topic: tutorial
ms.date: 02/17/2020
ms.custom: MVC
ms.openlocfilehash: eba177a254606bb847e0866ae48281a889c53f9b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78927481"
---
# <a name="prepare-on-premises-machines-for-migration-to-azure"></a>Azure'a geçiş için şirket içi makineleri hazırlayın

Bu makalede, [Azure Geçişi: Sunucu Geçişi'ni](migrate-services-overview.md#azure-migrate-server-migration-tool)kullanarak azure'a geçirinmeden önce şirket içi makinelerin nasıl hazırlanacağı açıklanmaktadır.

Bu makalede şunları yapacaksınız:
> [!div class="checklist"]
> * Geçiş sınırlamalarını doğrulayın.
> * İşletim sistemi gereksinimlerini ve destek sınırlamalarını kontrol edin.
> * Geçirmek istediğiniz makinelerin URL'sini ve bağlantı noktası erişimini gözden geçirin.
> * Geçişe başlamadan önce yapmanız gereken değişiklikleri gözden geçirin.
> * Taşıma harfleri geçişten sonra korunsun diye ayarları yapılandırın.
> * Geçişten sonra Azure VM'lerine bağlanabilmeniz için makineler hazırlayın.

## <a name="verify-migration-limitations"></a>Geçiş sınırlamalarını doğrulama

- Tek bir Azure Geçiş projesinde 35.000 VMware VM/Hyper-V VM'ye kadar, Azure Geçiş Sunucusu Geçişi'ni kullanarak değerlendirebilirsiniz. Bir proje, VMware VM'leri ve Hyper-V VM'leri her biri için sınırlara kadar birleştirebilir.
- Geçiş için bir defada en fazla 10 VM seçebilirsiniz. Daha fazla çoğaltmanız gerekiyorsa, 10'dan oluşan gruplar halinde çoğaltın.
- VMware aracısız geçiş için, aynı anda 100 çoğaltma kadar çalıştırabilirsiniz.

## <a name="verify-operating-system-requirements"></a>İşletim sistemi gereksinimlerini doğrulama

- [Windows işletim sistemlerinizin](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) Azure'da desteklenerek desteklendirilenden doğrulayın.
- [Linux dağıtımlarınızın](../virtual-machines/linux/endorsed-distros.md) Azure'da desteklendiğini doğrulayın.

## <a name="see-whats-supported"></a>Desteklenenleri görün

VMware VM'ler için Sunucu Geçişi [aracısız veya aracı tabanlı geçişi](server-migrate-overview.md)destekler.

- **VMware VM'ler : VMware**VM'ler için [geçiş gereksinimlerini ve desteği](migrate-support-matrix-vmware-migration.md) doğrulayın.
- **Hyper-V VM'ler : Hyper-V**VM'ler için [geçiş gereksinimlerini ve desteği](migrate-support-matrix-hyper-v-migration.md) doğrulayın.
- **Fiziksel makineler**: Şirket içi fiziksel makineler ve diğer sanallaştırılmış sunucular için [geçiş gereksinimlerini ve desteği](migrate-support-matrix-physical-migration.md) doğrulayın. 

## <a name="review-url-and-port-access"></a>URL ve bağlantı noktası erişimini gözden geçirin

Makineler geçiş sırasında internet erişimi gerekebilir.

- **Azure Geçir cihazı**: Azure Geçiş cihazının aracısız geçiş sırasında erişmesi gereken [URL'leri](migrate-appliance.md#url-access) ve [bağlantı noktalarını](migrate-support-matrix-vmware-migration.md#agentless-ports) gözden geçirin.
- **VMware VM aracı tabanlı geçiş**: Çoğaltma cihazının VMware VM aracı tabanlı geçiş sırasında kullandığı [URL'leri](migrate-replication-appliance.md#url-access) ve [bağlantı noktalarını](migrate-replication-appliance.md#port-access) gözden geçirin. 
- **Hyper-V ana bilgisayarları**: Geçiş sırasında Hyper-V ana bilgisayarlarının erişmeleri gereken [URL'leri ve bağlantı noktalarını](migrate-support-matrix-hyper-v-migration.md#hyper-v-hosts) gözden geçirin. 
- **Fiziksel sunucular**: Çoğaltma cihazının fiziksel sunucu geçişi sırasında kullandığı [URL'leri](migrate-replication-appliance.md#url-access) ve [bağlantı noktalarını](migrate-replication-appliance.md#port-access) gözden geçirin.

## <a name="verify-required-changes-before-migrating"></a>Geçiş yapmadan önce gerekli değişiklikleri doğrulama

Bazı VM'ler Azure'da çalıştırabilmeleri için değişiklik gerektirebilir. Azure Geçiş, bu işletim sistemlerini çalıştıran VM'ler için bu değişiklikleri otomatik olarak yapar:

- Kırmızı Şapka Kurumsal Linux 7.0+, 6.5+
- CentOS 7.0+, 6.5+
- SUSE Linux Kurumsal Sunucu 12 SP1+
- Ubuntu 18.04LTS, 16.04LTS, 14.04LTS
- Debian 8, 7

Diğer işletim sistemleri için, taşımadan önce makineleri el ile hazırlamanız gerekir. 

### <a name="prepare-windows-machines"></a>Windows makinelerini hazırlayın

Bir Windows makinesini geçirtiyorsanız, geçişten önce aşağıdaki değişiklikleri yapın. Değişiklikleri yapmadan önce VM'yi geçiş yaparsanız, VM Azure'da önyükleme yapmayabilir.

1. Azure VM için Azure Seri Konsolu'nu [etkinleştirin.](../virtual-machines/troubleshooting/serial-console-windows.md) Konsolu etkinleştirmek sorun gidermenize yardımcı olur. VM'yi yeniden başlatmanız gerekmez. Azure VM disk görüntüsünü kullanarak önyükleme yapacaktır. Disk görüntüsü önyüklemesi yeni VM için yeniden başlatmaya eşdeğerdir. 
2. Windows Server 2003 çalıştıran makineleri geçirin, Hyper-V Guest Integration Services'ı VM işletim sistemine yükleyin. [Daha fazla bilgi edinin](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#install-or-update-integration-services).

### <a name="prepare-linux-machines"></a>Linux makinelerini hazırlayın

1. Hyper-V Linux Entegrasyon Hizmetlerini yükleyin. Linux dağıtımlarının çoğu yeni sürümü varsayılan olarak Hyper-V Linux Entegrasyon Hizmetleri içerir.
2. Gerekli Hyper-V sürücülerini içerebilmek için Linux init görüntüsünü yeniden oluşturun. Init görüntüsünün yeniden oluşturulması, VM'nin Azure'da önyükleme sini sağlar (yalnızca bazı dağıtımlarda gereklidir).
3. [Azure Seri Konsol günlüğe kaydetmeyi etkinleştirin.](../virtual-machines/troubleshooting/serial-console-linux.md) Konsol günlüğe kaydetmeyi etkinleştirmek sorun gidermenize yardımcı olur. VM'yi yeniden başlatmanız gerekmez. Azure VM disk görüntüsünü kullanarak önyükleme yapacaktır. Disk görüntüsü önyüklemesi yeni VM için yeniden başlatmaya eşdeğerdir.
4. Aygıt eşlemi dosyasını aygıt adı-birim ilişkilendirmeleriyle güncelleştirin, böylece kalıcı aygıt tanımlayıcıları kullanırsınız.
5. Kalıcı birim tanımlayıcılarını kullanmak için fstab dosyasındaki girişleri güncelleştirin.
6. MAC adresine göre arabirim adlarını rezerve eden udev kurallarını kaldırın ve benzeri.
7. DHCP'den bir IP adresi almak için ağ arabirimlerini güncelleştirin.

Azure'da [Bir Linux VM çalıştırma adımları](../virtual-machines/linux/create-upload-generic.md)hakkında daha fazla bilgi edinin ve popüler Linux dağıtımlarından bazıları için yönergeler alın.

## <a name="preserve-drive-letters-after-migration"></a>Geçişten sonra sürücü harflerini koruma

Şirket içi bir makineyi Microsoft Azure'a geçirdiğinizde, ek veri disklerinin sürücü harfleri özgün değerlerinden değişebilir. 

Varsayılan olarak, Azure VM'leri geçici depolama alanı olarak kullanmak üzere D sürücüsüolarak atanır. Bu sürücü ataması, diğer tüm ekli depolama sürücüsü atamalarının tek harfle artışalmasına neden olur. Örneğin, şirket içi yüklemeniz uygulama yüklemeleri için D'yi sürmek üzere atanmış bir veri diski kullanıyorsa, VM'yi Azure'a geçirttikten sonra bu sürücünün E sürücüsüne atanması için atama yapılır. Bu otomatik atamayı önlemek ve Azure'un bir sonraki ücretsiz sürücü mektubunu geçici hacmine atadığından emin olmak için depolama alanı ağı (SAN) ilkesini **ÇevrimiçiTümü**olarak ayarlayın:

1. Şirket içi makinede (ana bilgisayar sunucusu nda değil), yükseltilmiş bir komut istemi açın.
2. **Disk bölümü**girin.
3. **SAN**girin . Konuk işletim sisteminin sürücü harfi korunmuyorsa, **Çevrimdışı Tüm** veya **Çevrimdışı Paylaşılan** döndürülür.
4. **DISKPART** isteminde **SAN Policy=OnlineAll**girin. Bu ayar, disklerin çevrimiçi olmasını ve her iki diske de okuma ve yazma nızı sağlar.
5. Test geçişi sırasında sürücü harflerinin korunduğunu doğrulayabilirsiniz.

## <a name="check-azure-vm-requirements"></a>Azure VM gereksinimlerini kontrol edin

Azure'da çoğaltacağınız şirket içi makineler, işletim sistemi ve mimarisi, diskler, ağ ayarları ve VM adlandırma için Azure VM gereksinimlerine uygun olmalıdır. VMware [VM'ler ve fiziksel sunucular](migrate-support-matrix-vmware-migration.md#azure-vm-requirements)ve [Hyper-V VM'ler](migrate-support-matrix-hyper-v-migration.md#azure-vm-requirements) için gereksinimleri geçirmeden önce doğrulayın.

## <a name="prepare-to-connect-after-migration"></a>Geçişten sonra bağlanmaya hazırlanın

Azure'a geçiş sırasında Azure VM'leri oluşturulur. Geçişten sonra, yeni Azure VM'lerine bağlanabilmelisin. Başarılı bir şekilde bağlanmak için birden çok adım gerekir.

### <a name="prepare-to-connect-to-azure-windows-vms"></a>Azure Windows VM'lerine bağlanmaya hazırlanın

Şirket içi Windows makineleri:

1. Windows ayarlarını yapılandırın. Ayarlar arasında statik kalıcı yolları veya WinHTTP proxy'yi kaldırma içerir.
2. Gerekli [hizmetlerin](../virtual-machines/windows/prepare-for-upload-vhd-image.md#check-the-windows-services) tükendiğini unutmayın.
3. Şirket içi makineye uzaktan bağlantı sağlamak için uzak masaüstünü (RDP) etkinleştirin. [RDP'yi etkinleştirmek için PowerShell'i](../virtual-machines/windows/prepare-for-upload-vhd-image.md#update-remote-desktop-registry-settings)nasıl kullanacağınızı öğrenin.
4. Geçişten sonra bir Azure VM'ye internet üzerinden erişmek için, şirket içi makinedeki Windows Güvenlik Duvarı'nda, Genel profilde TCP ve UDP'ye izin verin ve RDP'yi tüm profiller için izin verilen bir uygulama olarak ayarlayın.
5. Bir Azure VM'ine geçişten sonra siteden siteye VPN üzerinden erişmek istiyorsanız, şirket içi makinedeki Windows Güvenlik Duvarı'nda Etki Alanı ve Özel profiller için RDP'ye izin verin. [RDP trafiğine](../virtual-machines/windows/prepare-for-upload-vhd-image.md#configure-windows-firewall-rules)nasıl izin verilebildiğini öğrenin. 
6. Geçiş yaptığınızda şirket içi VM'de bekleyen Windows güncelleştirmeleri olmadığından emin olun. Varsa, geçişten sonra Azure VM'de güncelleştirmeler yüklenmeye başlayabilir ve güncelleştirmeler bitene kadar VM'de oturum açamazsınız.

### <a name="prepare-to-connect-with-linux-azure-vms"></a>Linux Azure VM'leri ile bağlanmaya hazırlanın

Şirket içi Linux makineleri:

1. Secure Shell hizmetinin sistem önyüklemede otomatik olarak başlayıp başlatılmaya ayarlı olduğundan kontrol edin.
2. Güvenlik duvarı kurallarının SSH bağlantısına izin verdiğinden emin olun.

### <a name="configure-azure-vms-after-migration"></a>Geçişten sonra Azure VM'lerini yapılandırma

Geçişten sonra, oluşturulan Azure VM'lerinde aşağıdaki adımları tamamlayın:

1. VM'ye internet üzerinden bağlanmak için VM'ye genel bir IP adresi atayın. Azure VM için şirket içi makinenizde kullandığınızdan farklı bir genel IP adresi kullanmanız gerekir. [Daha fazla bilgi edinin](../virtual-network/virtual-network-public-ip-address.md).
2. VM'deki ağ güvenlik grubu (NSG) kurallarının RDP veya SSH bağlantı noktasına gelen bağlantılara izin verdiğini denetleyin.
3. VM'yi görüntülemek için [önyükleme tanılamalarını](../virtual-machines/troubleshooting/boot-diagnostics.md#enable-boot-diagnostics-on-existing-virtual-machine) denetleyin.

> [!NOTE]
> Azure Bastion hizmeti, Azure VM'lerine özel RDP ve SSH erişimi sunar. Bu hizmet hakkında [daha fazla bilgi edinin.](../bastion/bastion-overview.md)

## <a name="next-steps"></a>Sonraki adımlar

[VMware VM'leri](server-migrate-overview.md) Azure'a geçirmek veya [Hyper-V V M'leri](tutorial-migrate-hyper-v.md) veya [fiziksel sunucuları veya sanallaştırılmış veya bulut VM'leri](tutorial-migrate-physical-virtual-machines.md)geçirmeye başlamak için hangi yöntemi kullanmak istediğinize karar verin.
