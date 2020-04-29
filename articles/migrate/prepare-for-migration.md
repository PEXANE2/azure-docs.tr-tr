---
title: Azure geçişi ile makineleri geçişe hazırlama
description: Azure geçişi ile geçiş için şirket içi makineleri nasıl hazırlayacağınızı öğrenin.
ms.topic: tutorial
ms.date: 02/17/2020
ms.custom: MVC
ms.openlocfilehash: eba177a254606bb847e0866ae48281a889c53f9b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "78927481"
---
# <a name="prepare-on-premises-machines-for-migration-to-azure"></a>Azure 'a geçiş için şirket içi makineleri hazırlama

Bu makalede, [Azure geçişi: sunucu geçişi](migrate-services-overview.md#azure-migrate-server-migration-tool)' ni kullanarak şirket Içi makineleri Azure 'a geçirmeye başlamadan önce nasıl hazırlayacağınız açıklanır.

Bu makalede şunları yapacaksınız:
> [!div class="checklist"]
> * Geçiş sınırlamalarını doğrulayın.
> * İşletim sistemi gereksinimlerini ve destek sınırlamalarını denetleyin.
> * Geçirmek istediğiniz makineler için URL ve bağlantı noktası erişimini gözden geçirin.
> * Geçişe başlamadan önce yapmanız gerekebilecek değişiklikleri gözden geçirin.
> * Sürücü harflerinin geçişten sonra korunması için ayarları yapılandırın.
> * Geçiş sonrasında Azure VM 'lerine bağlanabilmeniz için makineleri hazırlayın.

## <a name="verify-migration-limitations"></a>Geçiş sınırlamalarını doğrulama

- Azure geçişi sunucu geçişini kullanarak tek bir Azure geçişi projesinde en fazla 35.000 VMware VM/Hyper-V VM 'yi değerlendirebilirsiniz. Bir proje, VMware VM 'lerini ve Hyper-V VM 'lerini her biri için sınırlara kadar birleştirebilir.
- Geçiş için bir seferde en fazla 10 VM seçebilirsiniz. Daha fazla çoğaltma yapmanız gerekiyorsa, 10 ' un gruplar halinde çoğaltın.
- VMware aracısız geçiş için aynı anda en fazla 100 çoğaltma çalıştırabilirsiniz.

## <a name="verify-operating-system-requirements"></a>İşletim sistemi gereksinimlerini doğrulama

- [Windows işletim sistemlerinin](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) Azure 'da desteklendiğini doğrulayın.
- Azure 'da [Linux dağıtımlarınızın](../virtual-machines/linux/endorsed-distros.md) desteklendiğini doğrulayın.

## <a name="see-whats-supported"></a>Bkz. desteklenen özellikler

VMware VM 'Leri için, sunucu geçişi [aracısız veya aracı tabanlı geçişi](server-migrate-overview.md)destekler.

- **VMware VM 'leri**: VMware VM 'leri için [geçiş gereksinimlerini ve desteğini](migrate-support-matrix-vmware-migration.md) doğrulayın.
- **Hyper-v VM 'leri**: Hyper-v VM 'lerinin [geçiş gereksinimlerini ve desteğini](migrate-support-matrix-hyper-v-migration.md) doğrulayın.
- **Fiziksel makineler**: şirket içi fiziksel makineler ve diğer sanallaştırılmış sunucular için [geçiş gereksinimlerini ve desteğini](migrate-support-matrix-physical-migration.md) doğrulayın. 

## <a name="review-url-and-port-access"></a>URL ve bağlantı noktası erişimini gözden geçirin

Makinelerin geçiş sırasında internet erişimi olması gerekebilir.

- **Azure geçişi**gereci: Azure geçişi gerecinin aracısız geçiş sırasında erişmesi Için gereken [URL 'leri](migrate-appliance.md#url-access) ve [bağlantı noktalarını](migrate-support-matrix-vmware-migration.md#agentless-ports) gözden geçirin.
- **VMware VM Aracısı tabanlı geçiş**: VMware VM Aracısı tabanlı geçiş sırasında çoğaltma gerecinin kullandığı [URL 'leri](migrate-replication-appliance.md#url-access) ve [bağlantı noktalarını](migrate-replication-appliance.md#port-access) gözden geçirin. 
- **Hyper-v Konakları**: Hyper-v konaklarının geçiş sırasında erişmesi gereken [URL 'leri ve bağlantı noktalarını](migrate-support-matrix-hyper-v-migration.md#hyper-v-hosts) gözden geçirin. 
- **Fiziksel sunucular**: fiziksel sunucu geçişi sırasında çoğaltma gerecinin kullandığı [URL 'leri](migrate-replication-appliance.md#url-access) ve [bağlantı noktalarını](migrate-replication-appliance.md#port-access) gözden geçirin.

## <a name="verify-required-changes-before-migrating"></a>Geçirmeden önce gerekli değişiklikleri doğrulayın

Bazı VM 'Ler, Azure 'da çalışabilecek şekilde değişiklik yapılmasını gerektirebilir. Azure geçişi, bu işletim sistemlerini çalıştıran VM 'Ler için bu değişiklikleri otomatik olarak yapar:

- Red Hat Enterprise Linux 7.0 +, 6.5 +
- CentOS 7.0 +, 6.5 +
- SUSE Linux Enterprise Server 12 SP1 +
- Ubuntu 18.04 LTS, 16.04 LTS, 14.04 LTS
- 8, 7

Diğer işletim sistemleri için, geçiş işleminden önce makineleri el ile hazırlamanız gerekir. 

### <a name="prepare-windows-machines"></a>Windows makinelerini hazırlama

Bir Windows makinesini geçiriyorsanız geçişten önce aşağıdaki değişiklikleri yapın. Değişiklikleri yapmadan önce VM 'yi geçirirseniz, VM Azure 'da önyüklenemeyebilir.

1. Azure VM için [Azure seri konsolunu etkinleştirin](../virtual-machines/troubleshooting/serial-console-windows.md) . Konsolunun etkinleştirilmesi, sorun gidermenize yardımcı olur. VM 'yi yeniden başlatmanız gerekmez. Azure VM, disk görüntüsü kullanılarak önyüklenir. Disk görüntüsü önyüklemesi, yeni VM için bir yeniden başlatma ile eşdeğerdir. 
2. Windows Server 2003 çalıştıran makineleri geçiriyorsanız, Hyper-V Konuk Tümleştirme Hizmetleri 'ni VM işletim sistemine yükleyebilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#install-or-update-integration-services).

### <a name="prepare-linux-machines"></a>Linux makinelerini hazırlama

1. Hyper-V Linux Tümleştirme Hizmetleri 'ni yükler. Linux dağıtımların çoğu yeni sürümü, varsayılan olarak Hyper-V Linux Tümleştirme Hizmetleri içerir.
2. Linux init görüntüsünü, gerekli Hyper-V sürücülerini içerecek şekilde yeniden derleyin. İnit görüntüsünü yeniden oluşturmak, sanal makinenin Azure 'da önyüklenmesini sağlar (yalnızca bazı dağıtımlarla gereklidir).
3. [Azure seri konsol günlüğünü etkinleştirin](../virtual-machines/troubleshooting/serial-console-linux.md). Konsol günlüğünü etkinleştirmek, sorun gidermenize yardımcı olur. VM 'yi yeniden başlatmanız gerekmez. Azure VM, disk görüntüsü kullanılarak önyüklenir. Disk görüntüsü önyüklemesi, yeni VM için bir yeniden başlatma ile eşdeğerdir.
4. Cihaz eşleme dosyasını cihaz adı-birim ilişkilendirmeleriyle güncelleştirin, böylece kalıcı cihaz tanımlayıcılarını kullanırsınız.
5. Kalıcı birim tanımlayıcılarını kullanmak için fstab dosyasındaki girişleri güncelleştirin.
6. Arabirim adlarını ayrılan udev kurallarını MAC adresine göre kaldırın ve bu şekilde devam edin.
7. DHCP 'den bir IP adresi almak için ağ arabirimlerini güncelleştirin.

[Azure 'da bir LINUX VM çalıştırma adımları](../virtual-machines/linux/create-upload-generic.md)hakkında daha fazla bilgi edinin ve popüler Linux dağıtımlarından bazılarına ilişkin yönergeler edinin.

## <a name="preserve-drive-letters-after-migration"></a>Geçişten sonra sürücü harflerini koru

Şirket içi bir makineyi Microsoft Azure geçirdiğinizde, ek veri disklerinin sürücü harfleri özgün değerlerinden değişebilir. 

Varsayılan olarak, Azure VM 'Leri, geçici depolama olarak kullanılmak üzere D sürücüsüne atanır. Bu sürücü ataması, diğer tüm bağlı depolama sürücüsü atamalarının bir harfle artımına neden olur. Örneğin, şirket içi yüklemeniz uygulama yüklemeleri için D sürücüsüne atanan bir veri diski kullanıyorsa, VM 'yi Azure 'a geçirdikten sonra, bu sürücüye yönelik atama E sürücüsüne kadar artar. Bu otomatik atamayı engellemek ve Azure 'un bir sonraki boş sürücü harfini geçici birimine atamasını sağlamak için, depolama alanı ağı (SAN) ilkesini **OnlineAll**olarak ayarlayın:

1. Şirket içi makinede (ana sunucu değil), yükseltilmiş bir komut istemi açın.
2. **DiskPart**girin.
3. **San**girin. Konuk işletim sisteminin sürücü harfi tutulmazsa, **çevrimdışı** olarak veya **çevrimdışı paylaşılan** bir şekilde yüklenir.
4. **DISKPART** Isteminde **San Ilkesi = OnlineAll**yazın. Bu ayar, disklerin çevrimiçi hale gelmesini sağlar ve her iki diski de okuyup yazabilmenizi sağlar.
5. Test geçişi sırasında, sürücü harflerinin korunacağını doğrulayabilirsiniz.

## <a name="check-azure-vm-requirements"></a>Azure VM gereksinimlerini denetleme

Azure 'a çoğaltılan şirket içi makineler, işletim sistemi ve mimari, diskler, ağ ayarları ve VM adlandırması için Azure VM gereksinimleriyle uyumlu olmalıdır. Geçirmeden önce [VMware VM 'leri ve fiziksel sunucular](migrate-support-matrix-vmware-migration.md#azure-vm-requirements)ve [Hyper-V VM](migrate-support-matrix-hyper-v-migration.md#azure-vm-requirements) 'lerinin gereksinimlerini doğrulayın.

## <a name="prepare-to-connect-after-migration"></a>Geçişten sonra bağlanmaya hazırlanma

Azure VM 'Leri Azure 'a geçiş sırasında oluşturulur. Geçişten sonra, yeni Azure VM 'lerine bağlanabiliyor olmanız gerekir. Başarılı bir şekilde bağlanmak için birden çok adım gerekiyor.

### <a name="prepare-to-connect-to-azure-windows-vms"></a>Azure Windows VM 'lerine bağlanmaya hazırlanma

Şirket içi Windows makinelerde:

1. Windows ayarlarını yapılandırın. Ayarlar, statik kalıcı yolların veya WinHTTP proxy 'sinin kaldırılmasını içerir.
2. [Gerekli hizmetlerin](../virtual-machines/windows/prepare-for-upload-vhd-image.md#check-the-windows-services) çalıştığından emin olun.
3. Uzak Masaüstü 'nü (RDP) Şirket içi makineye uzak bağlantılara izin verecek şekilde etkinleştirin. [RDP 'yi etkinleştirmek Için PowerShell](../virtual-machines/windows/prepare-for-upload-vhd-image.md#update-remote-desktop-registry-settings)'i nasıl kullanacağınızı öğrenin.
4. Geçişten sonra Internet üzerinden bir Azure VM 'sine erişmek için, şirket içi makinedeki Windows Güvenlik Duvarı 'nda ortak profilde TCP ve UDP 'ye izin verin ve RDP 'yi tüm profiller için izin verilen bir uygulama olarak ayarlayın.
5. Geçişten sonra siteden siteye VPN üzerinden bir Azure VM 'ye erişmek istiyorsanız, şirket içi makinedeki Windows Güvenlik Duvarı 'nda etki alanı ve özel profiller için RDP 'ye izin verin. [RDP trafiğine nasıl izin vereceğinizi](../virtual-machines/windows/prepare-for-upload-vhd-image.md#configure-windows-firewall-rules)öğrenin. 
6. Geçiş yaparken şirket içi VM 'de bekleyen bir Windows güncelleştirmesi olmadığından emin olun. Varsa, güncelleştirmeler geçiş sonrasında Azure VM 'ye yüklenmeye başlayabilir ve güncelleştirmeler tamamlanana kadar VM 'de oturum açamazsınız.

### <a name="prepare-to-connect-with-linux-azure-vms"></a>Linux Azure VM 'leriyle bağlanmaya hazırlanma

Şirket içi Linux makinelerde:

1. Secure Shell hizmetinin sistem önyüklemesi sırasında otomatik olarak başlayacak şekilde ayarlandığından emin olun.
2. Güvenlik duvarı kurallarının SSH bağlantısına izin verdiğinden emin olun.

### <a name="configure-azure-vms-after-migration"></a>Geçiş sonrasında Azure VM 'lerini yapılandırma

Geçişten sonra, oluşturulan Azure VM 'lerde şu adımları uygulayın:

1. SANAL makineye internet üzerinden bağlanmak için, VM 'ye bir genel IP adresi atayın. Azure VM için, şirket içi makineniz için kullanılandan farklı bir genel IP adresi kullanmanız gerekir. [Daha fazla bilgi edinin](../virtual-network/virtual-network-public-ip-address.md).
2. VM 'deki ağ güvenlik grubu (NSG) kurallarının RDP veya SSH bağlantı noktasına gelen bağlantılara izin verin.
3. VM 'yi görüntülemek için [önyükleme tanılamalarını](../virtual-machines/troubleshooting/boot-diagnostics.md#enable-boot-diagnostics-on-existing-virtual-machine) denetleyin.

> [!NOTE]
> Azure savunma hizmeti, Azure VM 'lerine özel RDP ve SSH erişimi sağlar. Bu hizmet hakkında [daha fazla bilgi edinin](../bastion/bastion-overview.md) .

## <a name="next-steps"></a>Sonraki adımlar

[VMware VM](server-migrate-overview.md) 'lerini Azure 'a geçirmek için kullanmak istediğiniz yöntemi belirleyin veya [Hyper-V VM](tutorial-migrate-hyper-v.md) 'lerini veya [fiziksel sunucuları ya da sanallaştırılmış veya bulut VM](tutorial-migrate-physical-virtual-machines.md)'lerini geçirmeye başlayın.
