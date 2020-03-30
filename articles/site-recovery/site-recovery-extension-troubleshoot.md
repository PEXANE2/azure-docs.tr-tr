---
title: Azure Site Kurtarma ile olağanüstü durum kurtarma için Azure VM uzantısısorun giderme
description: Azure Site Kurtarma ile olağanüstü durum kurtarma için Azure VM uzantısı ile ilgili sorunları giderin.
author: sideeksh
manager: rochakm
ms.topic: troubleshooting
ms.date: 11/27/2018
ms.openlocfilehash: a780a42179a0bacf0e4a12ba1e75ae84943539b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77190719"
---
# <a name="troubleshoot-azure-vm-extension-issues"></a>Azure VM uzantı sorunları

Bu makalede, VM aracısı ve uzantısı ile ilgili Azure Site Kurtarma hatalarını çözmenize yardımcı olabilecek sorun giderme adımları sağlanmaktadır.


## <a name="azure-site-recovery-extension-time-out"></a>Azure Site Kurtarma uzantısı zaman ları  

Hata iletisi: "Görev yürütme, uzantı çalışmasının başlatılması için izleme sırasında zaman doldu"<br>
Hata kodu: "151076"

 Azure Site Kurtarma, etkinleştirme koruma işinin bir parçası olarak sanal makineye bir uzantı yükledi. Aşağıdaki koşullardan herhangi biri korumanın tetiklenmesini engelleyebilir ve işin başarısız olmasına neden olabilir. Aşağıdaki sorun giderme adımlarını tamamlayın ve ardından işleminizi yeniden deneyin:

- [Aracı VM'de yüklü, ancak yanıt vermiyor (Windows VM'ler için)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
- [VM yüklü aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
- [Site Kurtarma uzantısı güncelleştirilemez veya yüklenmez](#the-site-recovery-extension-fails-to-update-or-load)

Hata iletisi: "Önceki Site Kurtarma uzantısı işlemi beklenenden daha fazla zaman alıyor."<br>
Hata kodu: "150066"

- [Aracı VM'de yüklü, ancak yanıt vermiyor (Windows VM'ler için)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
- [VM yüklü aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
- [Site Kurtarma uzantısı durumu yanlış](#the-site-recovery-extension-fails-to-update-or-load)

## <a name="protection-fails-because-the-vm-agent-is-unresponsive"></a>VM aracısı yanıt vermiyor diye koruma başarısız olur

Hata iletisi: "Görev yürütme, uzantı çalışmasının başlatılması için izleme sırasında zaman doldu."<br>
Hata kodu: "151099"

Sanal makinedeki Azure konuk aracısı hazır durumda değilse, bu hata olabilir.

[Azure portalında](https://portal.azure.com/)Azure konuk aracısının durumunu kontrol edebilirsiniz. Korumaya çalıştığınız sanal makineye gidin ve **VM** > **Ayarlar** > **Özellikleri** > **Aracısı durumundaki**durumu kontrol edin. Çoğu zaman, aracının durumu sanal makineyi yeniden başlatıldıktan sonra hazırdır. Ancak, yeniden başlatamıyorsanız veya sorunla hala karşı karşıyaysanız, aşağıdaki sorun giderme adımlarını tamamlayın:

- [Aracı VM'de yüklü, ancak yanıt vermiyor (Windows VM'ler için)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
- [VM yüklü aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)


Hata iletisi: "Görev yürütme, uzantı çalışmasının başlatılması için izleme sırasında zaman doldu."<br>
Hata kodu: "151095"

Bu hata, Linux makinesindeki aracı sürümü güncel olmadığında oluşur. Aşağıdaki sorun giderme adımını tamamlayın:

- [VM yüklü aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)  

## <a name="causes-and-solutions"></a>Nedenleri ve çözümleri

### <a name="the-agent-is-installed-in-the-vm-but-its-unresponsive-for-windows-vms"></a><a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Aracı VM'de yüklü, ancak yanıt vermiyor (Windows VM'ler için)

#### <a name="solution"></a>Çözüm
VM aracısı bozulmuş veya hizmet durdurulmuş olabilir. VM aracısını yeniden yüklemek en son sürümü niçin elde edinmesine yardımcı olur. Ayrıca, hizmetle iletişimin yeniden başlatılmasına yardımcı olur.

1. Windows Azure Konuk Aracıhizmetinin VM hizmetlerinde (services.msc) çalışıp çalışmadığını belirleyin. Windows Azure Konuk Aracı hizmetini yeniden başlatın.    
1. Windows Azure Konuk Aracıhizmeti hizmetlerde görünmüyorsa Denetim Masası'nı açın. Windows Guest Agent hizmetinin yüklü olup olmadığını görmek için **Programlar ve Özellikler'e** gidin.
1. **Programlar ve Özellikler'de**Windows Azure Konuk Aracısı görünüyorsa, Windows Azure Konuk Aracısını kaldırın.
1. [Ajan MSI'ın en son sürümünü indirin](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)ve kurun. Yüklemeyi tamamlamak için yönetici haklarına ihtiyacınız var.
1. Windows Azure Konuk Aracıhizmetinin hizmetlerde göründüğünü doğrulayın.
1. Koruma işini yeniden başlatın.

Ayrıca, [Microsoft .NET 4.5'in](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) VM'de yüklü olduğunu doğrulayın. VM aracısının hizmetle iletişim kurması için .NET 4.5'e ihtiyacınız vardır.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>VM yüklü aracı (Linux VM'ler için) güncel değil

#### <a name="solution"></a>Çözüm
Linux VM'leri için aracıyla veya uzantılarla ilgili hataların çoğu, eski bir VM aracıyı etkileyen sorunlardan kaynaklanır. Bu sorunu gidermek için aşağıdaki genel yönergeleri izleyin:

1. [Linux VM aracısını güncellemek](../virtual-machines/linux/update-agent.md)için yönergeleri izleyin.

   > [!NOTE]
   > Aracıyı yalnızca bir dağıtım deposu aracılığıyla güncelleştirmenizi *şiddetle öneririz.* Aracı kodunu doğrudan GitHub'dan indirmenizi ve güncellemenizi önermiyoruz. Dağıtımınız için en son aracı kullanılamıyorsa, nasıl yüklenirseniz hakkında talimatlar için dağıtım desteğine başvurun. En son aracıyı kontrol etmek için GitHub deposundaki [Windows Azure Linux aracısı](https://github.com/Azure/WALinuxAgent/releases) sayfasına gidin.

1. Azure aracısının aşağıdaki komutu çalıştırarak VM'de çalıştığını sağlayın:`ps -e`

   İşlem çalışmıyorsa, aşağıdaki komutları kullanarak yeniden başlatın:

   - Ubuntu için:`service walinuxagent start`
   - Diğer dağıtımlar için:`service waagent start`

1. [Otomatik yeniden başlatma aracısını yapılandırın.](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash)
1. Sanal makinenin korunmasını etkinleştirin.

### <a name="the-site-recovery-extension-fails-to-update-or-load"></a>Site Kurtarma uzantısı güncelleştirilemez veya yüklenmez

Uzantı durumu "Boş", "Hazır Değil" veya "Geçiş" olarak gösterir.

#### <a name="solution"></a>Çözüm

Uzantıyı kaldırın ve işlemi yeniden başlatın.

Uzantıyı kaldırmak için:

1. Azure [portalında](https://portal.azure.com/)Yedekleme hatası yaşayan VM'ye gidin.
1. **Ayarlar'ı**seçin.
1. **Uzantılar**'ı seçin.
1. **Site Kurtarma Uzantısı'nı**seçin.
1. **Kaldır**'ı seçin.

Linux VM için, VMSnapshot uzantısı Azure portalında görünmüyorsa, [Azure Linux Aracısını güncelleyin.](../virtual-machines/linux/update-agent.md) O zaman korumayı çalıştır.

Bu adımları tamamladığınızda, uzantın koruma sırasında yeniden yüklenmesine neden olur.
