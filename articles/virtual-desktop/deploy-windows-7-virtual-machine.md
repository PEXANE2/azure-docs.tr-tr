---
title: Windows Virtual Desktop 'ta Windows 7 sanal makinesi dağıtma-Azure
description: Windows sanal masaüstü 'nde Windows 7 sanal makinesini yapılandırma ve dağıtma.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: helohr
ms.openlocfilehash: e7f565a995e4c2a5338f08437b0dd336846ba154
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71679925"
---
# <a name="deploy-a-windows-7-virtual-machine-on-windows-virtual-desktop"></a>Windows sanal masaüstü 'nde Windows 7 sanal makinesini dağıtma

Windows sanal masaüstü üzerinde bir Windows 7 sanal makinesi (VM) dağıtma işlemi, Windows 'un sonraki sürümlerini çalıştıran VM 'lerden biraz farklıdır. Bu kılavuzda, Windows 7 ' nin nasıl dağıtılacağı açıklanır.

## <a name="prerequisites"></a>Prerequisites

Başlamadan önce, bir konak havuzu oluşturmak için [PowerShell ile konak havuzu oluşturma](create-host-pools-powershell.md) ' daki yönergeleri izleyin. Bundan sonra, Masaüstü uygulama grubuna bir veya daha fazla kullanıcı atamak için [Azure Marketi 'nde konak havuzları oluşturma](create-host-pools-azure-marketplace.md#optional-assign-additional-users-to-the-desktop-application-group) ' daki yönergeleri izleyin.

## <a name="configure-a-windows-7-virtual-machine"></a>Windows 7 sanal makinesini yapılandırma

Önkoşulları tamamladıktan sonra, Windows 7 VM 'nizi Windows sanal masaüstü 'nde dağıtıma yapılandırmaya hazırsınız demektir.

Windows sanal masaüstü 'nde Windows 7 VM ayarlamak için:

1. Azure portal oturum açın ve Windows 7 Enterprise görüntüsünü arayın ya da kendi özelleştirilmiş Windows 7 Enterprise (x64) görüntünüzü karşıya yükleyin.  
2. Windows 7 Enterprise ile bir veya birden çok sanal makineyi konak işletim sistemi olarak dağıtın. Sanal makinelerin Uzak Masaüstü Protokolü (RDP) izin ver (TCP/3389 bağlantı noktası) olduğundan emin olun.
3. RDP kullanarak Windows 7 Enterprise konağına bağlanın ve dağıtımınızı yapılandırırken tanımladığınız kimlik bilgileriyle kimlik doğrulaması yapın. 
4. Ana bilgisayara RDP ile bağlanırken kullandığınız hesabı "uzak masaüstü kullanıcısı" grubuna ekleyin. Bunu yapmazsanız, Active Directory etki alanınıza katladıktan sonra VM 'ye bağlanamadınız.
5. SANAL makinenizde Windows Update gidin.
6. Tüm Windows güncelleştirmelerini önemli kategoride yükler.
7. Tüm Windows güncelleştirmelerini Isteğe bağlı kategoride (dil paketleri hariç) yükler. Bu, bu yönergeleri tamamlayabilmeniz için ihtiyaç duyduğunuz Uzak Masaüstü Protokolü 8,0 güncelleştirmesini ([KB2592687](https://www.microsoft.com/download/details.aspx?id=35393)) yüklüyor.
8. Yerel Grup İlkesi Düzenleyicisi açın ve **bilgisayar yapılandırması**' na gidin  > **Yönetim Şablonları** > **Windows bileşenleri** > **Uzak Masaüstü Hizmetleri**@no__t **-7 Uzak Masaüstü oturumu ana bilgisayarı**  > **uzak oturum ortamı**.
9. Uzak Masaüstü Protokolü 8,0 ilkesini etkinleştirin.
10. Aşağıdaki komutu çalıştırarak sanal makineyi yeniden başlatın:
    
     ```cmd
     shutdown /r /t 0
     ```
    
## <a name="next-steps"></a>Sonraki adımlar

Windows 7 VM 'niz artık Windows sanal masaüstü 'nde dağıtıma hazırdır. Dağıtımı tamamlaması için [sanal makineleri Windows sanal masaüstü Aracısı yüklemelerine hazırlama](create-host-pools-powershell.md#prepare-the-virtual-machines-for-windows-virtual-desktop-agent-installations) bölümündeki yönergeleri izleyin.

Windows sanal masaüstündeki Windows 7 ' de bilinen sorunların ve sorun giderme yönergelerinin bir listesi için, Windows [sanal masaüstündeki Windows 7 sanal makinelerinde sorun](troubleshoot-windows-7-vm.md)giderme makalesindeki sorun giderme makalemizi inceleyin.

