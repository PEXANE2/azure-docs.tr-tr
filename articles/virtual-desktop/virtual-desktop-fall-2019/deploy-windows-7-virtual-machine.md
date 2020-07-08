---
title: Windows 7 sanal makinesini dağıtma Windows sanal masaüstü-Azure
description: Windows sanal masaüstü 'nde Windows 7 sanal makinesini yapılandırma ve dağıtma.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: eafe2050f834fdd9aecba492c7121be9c1e121e2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85206011"
---
# <a name="deploy-a-windows-7-virtual-machine-on-windows-virtual-desktop"></a>Windows Sanal Masaüstü’nde Windows 7 sanal makinesi dağıtma

>[!IMPORTANT]
>Bu içerik, Windows sanal masaüstü nesneleri Azure Resource Manager desteklemeyen sonbahar 2019 sürümü için geçerlidir.

Windows sanal masaüstü üzerinde bir Windows 7 sanal makinesi (VM) dağıtma işlemi, Windows 'un sonraki sürümlerini çalıştıran VM 'lerden biraz farklıdır. Bu kılavuzda, Windows 7 ' nin nasıl dağıtılacağı açıklanır.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, bir konak havuzu oluşturmak için [PowerShell ile konak havuzu oluşturma](create-host-pools-powershell-2019.md) ' daki yönergeleri izleyin. Bundan sonra, Masaüstü uygulama grubuna bir veya daha fazla kullanıcı atamak için [Azure Marketi 'nde konak havuzları oluşturma](create-host-pools-azure-marketplace-2019.md#optional-assign-additional-users-to-the-desktop-application-group) ' daki yönergeleri izleyin.

## <a name="configure-a-windows-7-virtual-machine"></a>Windows 7 sanal makinesini yapılandırma

Önkoşulları tamamladıktan sonra, Windows 7 VM 'nizi Windows sanal masaüstü 'nde dağıtıma yapılandırmaya hazırsınız demektir.

Windows sanal masaüstü 'nde Windows 7 VM ayarlamak için:

1. Azure portal oturum açın ve Windows 7 Enterprise görüntüsünü arayın ya da kendi özelleştirilmiş Windows 7 Enterprise (x64) görüntünüzü karşıya yükleyin.
2. Windows 7 Enterprise ile bir veya birden çok sanal makineyi konak işletim sistemi olarak dağıtın. Sanal makinelerin Uzak Masaüstü Protokolü (RDP) izin ver (TCP/3389 bağlantı noktası) olduğundan emin olun.
3. RDP kullanarak Windows 7 Enterprise konağına bağlanın ve dağıtımınızı yapılandırırken tanımladığınız kimlik bilgileriyle kimlik doğrulaması yapın.
4. Ana bilgisayara RDP ile bağlanırken kullandığınız hesabı "uzak masaüstü kullanıcısı" grubuna ekleyin. Bunu yapmazsanız, Active Directory etki alanınıza katladıktan sonra VM 'ye bağlanamadınız.
5. SANAL makinenizde Windows Update gidin.
6. Tüm Windows güncelleştirmelerini önemli kategoride yükler.
7. Tüm Windows güncelleştirmelerini Isteğe bağlı kategoride (dil paketleri hariç) yükler. Bu, bu yönergeleri tamamlayabilmeniz için ihtiyaç duyduğunuz Uzak Masaüstü Protokolü 8,0 güncelleştirmesini ([KB2592687](https://www.microsoft.com/download/details.aspx?id=35387)) yüklüyor.
8. Yerel Grup İlkesi Düzenleyicisi açın ve **Computer Configuration**  >  **Administrative Templates**  >  **Windows Components**  >  **Remote Desktop Services**  >  **Uzak Masaüstü oturumu ana bilgisayarı**  >  **uzak oturum ortamı**Uzak Masaüstü Hizmetleri Windows bileşenleri Yönetim Şablonları bilgisayar yapılandırması ' na gidin.
9. Uzak Masaüstü Protokolü 8,0 ilkesini etkinleştirin.
10. Bu sanal makineyi Active Directory etki alanınıza ekleyin.
11. Aşağıdaki komutu çalıştırarak sanal makineyi yeniden başlatın:

     ```cmd
     shutdown /r /t 0
     ```

12. Kayıt belirteci almak için [buradaki](/powershell/module/windowsvirtualdesktop/export-rdsregistrationinfo/) yönergeleri izleyin.
13. Windows [7 Için Windows sanal masaüstü aracısını indirin](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3JZCm).
14. Windows [7 Için Windows sanal masaüstü Aracısı Yöneticisi 'Ni indirin](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3K2e3).
15. Windows sanal masaüstü Aracısı yükleyicisini açın ve yönergeleri izleyin. İstendiğinde, 12. adımda oluşturduğunuz kayıt anahtarını verin.
16. Windows sanal masaüstü Aracısı yöneticisini açın ve yönergeleri izleyin.
17. İsteğe bağlı olarak, VM 'ye doğrudan Uzak Masaüstü Protokolü erişimi kaldırmak için TCP/3389 bağlantı noktasını engelleyin.
18. İsteğe bağlı olarak, .NET Framework 'ün en az sürüm 4.7.2 olduğunu doğrulayın. Özel bir görüntü oluşturuyorsanız bu özellikle önemlidir.

## <a name="next-steps"></a>Sonraki adımlar

Windows sanal masaüstü dağıtımınız artık kullanıma hazırdır. Başlamak için [Windows sanal masaüstü istemcisinin en son sürümünü indirin](https://aka.ms/wvd/clients/windows) .

Windows sanal masaüstündeki Windows 7 ' de bilinen sorunların ve sorun giderme yönergelerinin bir listesi için, Windows [sanal masaüstündeki Windows 7 sanal makinelerinde sorun](troubleshoot-windows-7-vm.md)giderme makalesindeki sorun giderme makalemizi inceleyin.
