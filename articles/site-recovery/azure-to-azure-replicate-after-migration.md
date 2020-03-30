---
title: Azure Site Kurtarma ile Azure'a geçişten sonra olağanüstü durum kurtarma yı ayarlama
description: Bu makalede, Azure Site Kurtarma kullanarak Azure'a geçişten sonra Azure bölgeleri arasında olağanüstü durum kurtarma ayarlamak için makinelerin nasıl hazırlanacağı açıklanmaktadır.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 11/14/2019
ms.author: raynew
ms.openlocfilehash: 874c282ff878126297dc46ca0e7a4c19910e40a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74159121"
---
# <a name="set-up-disaster-recovery-for-azure-vms-after-migration-to-azure"></a>Azure'a geçişten sonra Azure VM'leri için olağanüstü durumdan kurtarmayı ayarlama 


[Site Kurtarma](site-recovery-overview.md) hizmetini kullanarak [şirket içi makineleri Azure'a geçtiyseniz](tutorial-migrate-on-premises-to-azure.md) ve şimdi ikincil bir Azure bölgesine olağanüstü durum kurtarma için ayarlanmış VM'leri almak istiyorsanız, bu makaleyi izleyin. Makalede, Azure VM aracısının geçirilen VM'lere nasıl yüklendiğinden nasıl emin oluncagerektiği ve geçişten sonra artık gerekmeyen Site Kurtarma Mobilite hizmetinin nasıl kaldırılılabildiğini açıklanmaktadır.



## <a name="verify-migration"></a>Geçişi doğrulama

Olağanüstü durum kurtarmayı ayarlamadan önce, geçişin beklendiği gibi tamamlandığına emin olun. Geçişi başarıyla tamamlamak için, başarısız olduktan sonra, geçirmek istediğiniz her makine için **Tüm geçiş** seçeneğini seçmeniz gerekir. 

## <a name="verify-the-azure-vm-agent"></a>Azure VM aracısını doğrulama

Her Azure VM'nin [Azure VM aracısı](../virtual-machines/extensions/agent-windows.md) yüklü olmalıdır. Azure VM'lerini çoğaltmak için Site Kurtarma aracısına bir uzantı yükler.

- Makine, Site Kurtarma Mobilitesi hizmetinin 9.7.0.0 veya daha sonraki sürümünü çalıştırıyorsa, Azure VM aracısı Windows VM'lerde Mobilite hizmeti tarafından otomatik olarak yüklenir. Mobilite hizmetinin önceki sürümlerinde aracıyı el ile yüklersiniz.
- Linux VM'leri için Azure VM aracısını el ile yüklemeniz gerekir. Yalnızca geçirilen makineye yüklenen Mobilite hizmeti v9.6 veya daha erkenyse Azure VM aracısını yüklemeniz gerekir.


### <a name="install-the-agent-on-windows-vms"></a>Aracıyı Windows VM'lere yükleme

Site Kurtarma mobilite hizmetinin bir sürümünü 9.7.0.0'dan önce çalıştırıyorsanız veya aracıyı el ile yüklemeniz gerekiyorsa, aşağıdakileri yapın:  

1. VM'de yönetici izinlerine sahip olduğundan emin olun.
2. [VM Agent yükleyicisini](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)indirin.
3. Yükleyici dosyasını çalıştırın.

#### <a name="validate-the-installation"></a>Yüklemeyi doğrulama
Aracının yüklü olup olmadığını denetlemek için:

1. Azure VM'de, C:\WindowsAzure\Paketler klasöründe WaAppAgent.exe dosyasını görmeniz gerekir.
2. Dosyayı sağ tıklatın ve **Özellikler'de** **Ayrıntılar** sekmesini seçin.
3. **Ürün Sürümü** alanının 2.6.1198.718 veya daha yüksek olduğunu doğrulayın.

Windows için aracı yükleme hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows)

### <a name="install-the-agent-on-linux-vms"></a>Aracıyı Linux VM'lere yükleyin

Azure [Linux VM](../virtual-machines/extensions/agent-linux.md) aracısını el ile aşağıdaki gibi yükleyin:

1. Makinede yönetici izinleri olduğundan emin olun.
2. Linux VM aracısını dağıtımınızın paket deposundan bir RPM veya DEB paketi kullanarak yüklemenizi şiddetle öneririz. Onaylanan tüm [dağıtım sağlayıcıları,](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) Azure Linux aracı paketini görüntülerine ve depolarına entegre ederler.
    - Aracıyı yalnızca bir dağıtım deposu aracılığıyla güncelleştirmenizi şiddetle öneririz.
    - Linux VM aracısını doğrudan GitHub'dan yüklemenizi ve güncellemenizi önermiyoruz.
    -  Dağıtımınız için en son aracı kullanılamıyorsa, nasıl yüklenire ilişkin talimatlar için dağıtım desteğine başvurun. 

#### <a name="validate-the-installation"></a>Yüklemeyi doğrulama 

1. Azure aracısının Linux VM'de çalışmasını sağlamak için bu komutu çalıştırın: **ps -e.**
2. İşlem çalışmıyorsa, aşağıdaki komutları kullanarak yeniden başlatın:
    - Ubuntu için: **hizmet walinuxagent başlangıç**
    - Diğer dağıtımlar için: **servis waagent başlangıcı**


## <a name="uninstall-the-mobility-service"></a>Mobilite hizmetini kaldırın

1. Aşağıdaki yöntemlerden birini kullanarak Azure VM'den Mobilite hizmetini el ile kaldırın. 
    - Windows için, Denetim Masası'nda **Programlar Ekle/Kaldır**>, **Microsoft Azure Site Kurtarma Mobilite Hizmeti/Ana Hedef sunucusunu**kaldırın. Yükseltilmiş bir komut isteminde çalıştırın:
        ```
        MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
        ```
    - Linux için, kök kullanıcı olarak oturum açın. Bir terminalde **/user/local/ASR'ye**gidin ve aşağıdaki komutu çalıştırın:
        ```
        ./uninstall.sh -Y
        ```
2. Çoğaltmayı yapılandırmadan önce VM'yi yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Azure VM aracısı üzerindeki Site Kurtarma uzantısı için [sorun giderme](site-recovery-extension-troubleshoot.md) incelemesini gözden geçirin.
Azure VM'yi ikinci bir bölgeye [hızla çoğaltın.](azure-to-azure-quickstart.md)
