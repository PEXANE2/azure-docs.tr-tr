---
title: Azure Site Recovery aracılarıyla ilgili sorunları giderme | Microsoft Docs '
description: Azure Site Recovery Aracısı hatalarının belirtileri, nedenleri ve çözümleri hakkında bilgi sağlar.
author: carmonmills
manager: rochakm
ms.service: site-recovery
ms.topic: troubleshooting
ms.date: 11/27/2018
ms.author: carmonm
ms.openlocfilehash: 0de5a9843b8029c1e1926ae296f43fc95b48106c
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75930123"
---
# <a name="troubleshoot-issues-with-the-azure-site-recovery-agent"></a>Azure Site Recovery aracısında sorun giderme

Bu makalede, VM Aracısı ve uzantısıyla ilgili Azure Site Recovery hatalarını çözmenize yardımcı olabilecek sorun giderme adımları sunulmaktadır.


## <a name="azure-site-recovery-extension-time-out"></a>Azure Site Recovery uzantısı zaman aşımı  

Hata iletisi: "uzantı işleminin başlatılması izlenirken görev yürütme zaman aşımına uğradı"<br>
Hata kodu: "151076"

 Korumayı etkinleştirme işinin bir parçası olarak sanal makineye bir uzantı Azure Site Recovery. Aşağıdaki koşullardan herhangi biri, korumanın tetiklenmesi ve işin başarısız olmasına engel olabilir. Aşağıdaki sorun giderme adımlarını tamamlayıp işleminizi yeniden deneyin:

**Neden 1: [Aracı VM 'ye yüklendi, ancak yanıt vermiyor (Windows VM 'leri için)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**     
**Neden 2: [VM 'de yüklü olan aracı güncel değil (Linux VM 'ler için)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Neden 3: [Site Recovery uzantısı güncelleştirilemiyor veya yüklenemiyor](#the-site-recovery-extension-fails-to-update-or-load)**  

Hata iletisi: "önceki Site Recovery uzantısı işlemi beklenenden uzun sürüyor."<br>
Hata kodu: "150066"<br>

**Neden 1: [Aracı VM 'ye yüklendi, ancak yanıt vermiyor (Windows VM 'leri için)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**     
**Neden 2: [VM 'de yüklü olan aracı güncel değil (Linux VM 'ler için)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Neden 3: [Site Recovery uzantısı durumu yanlış](#the-site-recovery-extension-fails-to-update-or-load)**  

## <a name="protection-fails-because-the-vm-agent-is-unresponsive"></a>VM Aracısı yanıt vermediği için koruma başarısız oluyor

Hata iletisi: "uzantı işleminin başlatılması izlenirken görev yürütme zaman aşımına uğradı."<br>
Hata kodu: "151099"<br>

Bu hata, sanal makinedeki Azure Konuk Aracısı, Ready durumunda değilse oluşabilir.
Azure Konuk aracısının durumunu [Azure Portal](https://portal.azure.com/)denetleyebilirsiniz. Korumaya çalıştığınız sanal makineye gidin ve "VM > ayarları > Özellikler > Aracı durumu" içindeki durumu denetleyin. Çoğu zaman, sanal makine yeniden başlatıldıktan sonra aracının durumu kullanılabilir hale gelir. Ancak yeniden başlatma olası bir seçenek değilse veya sorunu yaşamaya devam ediyorsanız, aşağıdaki sorun giderme adımlarını izleyin.

**Neden 1: [Aracı VM 'ye yüklendi, ancak yanıt vermiyor (Windows VM 'leri için)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**     
**Neden 2: [VM 'de yüklü olan aracı güncel değil (Linux VM 'ler için)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  


Hata iletisi: "uzantı işleminin başlatılması izlenirken görev yürütme zaman aşımına uğradı."<br>
Hata kodu: "151095"<br>

Bu, Linux makinesindeki aracı sürümü eski olduğunda meydana gelir. Lütfen aşağıdaki sorun giderme adımını doldurun.<br>
  **Neden 1: [VM 'de yüklü olan aracı güncel değil (Linux VM 'ler için)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
## <a name="causes-and-solutions"></a>Nedenler ve çözümler

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Aracı VM 'ye yüklendi, ancak yanıt vermiyor (Windows VM 'Leri için)

#### <a name="solution"></a>Çözüm
VM Aracısı bozulmuş olabilir veya hizmet durdurulmuş olabilir. VM Aracısı 'nı yeniden yüklemek en son sürümü almaya yardımcı olur. Hizmet ile iletişimin yeniden başlatılmasına de yardımcı olur.

1. "Windows Azure Konuk Aracısı hizmeti" nin VM hizmetlerinde (Services. msc) çalışıp çalışmadığını belirleme. "Windows Azure Konuk Aracısı hizmeti" ni yeniden başlatmayı deneyin.    
2. Windows Azure Konuk Aracısı hizmeti Hizmetler 'de görülemiyorsa, Denetim Masası 'nda, Windows Konuk Aracısı hizmetinin yüklenip yüklenmediğini öğrenmek için **Programlar ve Özellikler** ' e gidin.
4. Windows Azure Konuk Aracısı **Programlar ve Özellikler**' de görünürse, Windows Konuk aracısını kaldırın.
5. [Aracı MSI ' nın en son sürümünü](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)indirip yükleyin. Yüklemeyi gerçekleştirmek için yönetici haklarına sahip olmanız gerekir.
6. Windows Azure Konuk Aracısı hizmetlerinin hizmetler 'de göründüğünü doğrulayın.
7. Koruma işini yeniden başlatın.

Ayrıca, VM 'de [Microsoft .NET 4,5 ' nin yüklü](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) olduğunu doğrulayın. VM aracısının hizmetle iletişim kurması için .NET 4,5 gerekir.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>VM 'de yüklü olan aracı güncel değil (Linux VM 'Leri için)

#### <a name="solution"></a>Çözüm
Linux VM 'Leri için aracıyla ilgili veya uzantı ile ilgili çoğu başarısızlık, süresi geçmiş bir VM aracısını etkileyen sorunlardan kaynaklanır. Bu sorunu gidermek için aşağıdaki genel yönergeleri izleyin:

1. [LINUX VM aracısını güncelleştirme](../virtual-machines/linux/update-agent.md)yönergelerini izleyin.

   > [!NOTE]
   > Aracıyı yalnızca bir dağıtım deposu aracılığıyla güncelleştirmenizi *önemle tavsiye* ederiz. Doğrudan GitHub 'dan aracı kodunu indirmenizi ve güncelleştirmeyi önermiyoruz. Dağıtım için en son aracı kullanılamıyorsa, nasıl yükleneceğine ilişkin yönergeler için dağıtım desteğiyle iletişim kurun. En son aracıyı denetlemek için GitHub deposundaki [Windows Azure Linux Aracısı](https://github.com/Azure/WALinuxAgent/releases) sayfasına gidin.

2. Şu komutu çalıştırarak Azure aracısının VM 'de çalıştığından emin olun: `ps -e`

   İşlem çalışmıyorsa, aşağıdaki komutları kullanarak yeniden başlatın:

   * Ubuntu için: `service walinuxagent start`
   * Diğer dağıtımlar için: `service waagent start`

3. [Otomatik yeniden başlatma aracısını yapılandırın](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Sanal makinenin korunmasını etkinleştirin.



### <a name="the-site-recovery-extension-fails-to-update-or-load"></a>Site Recovery uzantısı güncelleştirilemiyor veya yüklenemiyor
Uzantı durumu "Empty" ise, ' NotReady ' veya geçiş işlemi.

#### <a name="solution"></a>Çözüm

Uzantıyı kaldırın ve işlemi yeniden başlatın.

Uzantıyı kaldırmak için:

1. [Azure Portal](https://portal.azure.com/), yedekleme hatası yaşayan VM 'ye gidin.
2. Seçin **ayarları**.
3. **Uzantılar**'ı seçin.
4. **Site Recovery uzantısı**' nı seçin.
5. **Kaldır**'ı seçin.

Linux VM için, VMSnapshot uzantısı Azure portal görünmüyorsa, [Azure Linux aracısını güncelleştirin](../virtual-machines/linux/update-agent.md)ve ardından korumayı çalıştırın. 

Bu adımların tamamlanması, uzantının koruma sırasında yeniden yüklenmesine neden olur.


