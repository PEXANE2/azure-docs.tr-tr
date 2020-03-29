---
title: Windows 7 sanal makine Windows Sanal Masaüstü dağıtma - Azure
description: Windows Sanal Masaüstü'nde bir Windows 7 sanal makineyi yapılandırma ve dağıtma.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 10/03/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 158dd9d6a38cc1aaf7667c0b16518e23b3f7cecf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79366692"
---
# <a name="deploy-a-windows-7-virtual-machine-on-windows-virtual-desktop"></a>Windows Sanal Masaüstü’nde Windows 7 sanal makinesi dağıtma

Windows Sanal Masaüstü'nde bir Windows 7 sanal makine (VM) dağıtma işlemi, Windows'un sonraki sürümlerini çalıştıran VM'lerden biraz farklıdır. Bu kılavuz, Windows 7'yi nasıl dağıtacağınızı size söyleyecektir.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, ana bilgisayar havuzu oluşturmak için [PowerShell ile bir ana bilgisayar havuzu oluştur'daki](create-host-pools-powershell.md) yönergeleri izleyin. Bundan sonra, bir veya daha fazla kullanıcıyı masaüstü uygulama grubuna atamak için [Azure Marketi'ndeki ana bilgisayar oluşturma havuzlarındaki](create-host-pools-azure-marketplace.md#optional-assign-additional-users-to-the-desktop-application-group) yönergeleri izleyin.

## <a name="configure-a-windows-7-virtual-machine"></a>Windows 7 sanal makineyi yapılandırma

Ön koşulları yaptıktan sonra, Windows Sanal Masaüstü'nde dağıtım için Windows 7 VM'nizi yapılandırmaya hazırsınız.

Windows Sanal Masaüstü'nde Windows 7 VM ayarlamak için:

1. Azure portalında oturum açın ve Windows 7 Enterprise resmini arayın veya kendi özelleştirilmiş Windows 7 Enterprise (x64) resminizi yükleyin.  
2. Ana bilgisayar işletim sistemi olarak Windows 7 Enterprise ile bir veya birden çok sanal makine dağıtın. Sanal makinelerin Uzak Masaüstü Protokolüne (RDP) (TCP/3389 bağlantı noktası) izin aldığından emin olun.
3. RDP'yi kullanarak Windows 7 Enterprise ana bilgisayara bağlanın ve dağıtımınızı yapılandırırken tanımladığınız kimlik bilgileriyle kimlik doğrulaması yapmak. 
4. RDP ile ana bilgisayara bağlanırken kullandığınız hesabı "Uzak Masaüstü Kullanıcısı" grubuna ekleyin. Bunu yapmazsanız, Etkin Dizin etki alanınıza katıldıktan sonra VM'ye bağlanamayabilirsiniz.
5. VM'inizde Windows Update'e gidin.
6. Tüm Windows Güncelleştirmelerini Önemli kategoriye yükleyin.
7. Tüm Windows Güncelleştirmelerini İsteğe Bağlı kategoriye yükleyin (dil paketleri hariç). Bu, bu yönergeleri tamamlamanız gereken Uzak Masaüstü Protokolü 8.0 güncelleştirmesini[(KB2592687)](https://www.microsoft.com/download/details.aspx?id=35387)yükler.
8. Yerel Grup İlkesi Düzenleyicisini açın ve **Bilgisayar Yapılandırma** > **Sıdması İdari Şablonlar** > **Windows Bileşenleri** > **Uzak Masaüstü Hizmetleri** > **Uzak Masaüstü Oturum Barındırma** > **Uzak Oturum Ortamına**gidin.
9. Uzak Masaüstü Protokolü 8.0 ilkesini etkinleştirin.
10. Bu VM'ye Active Directory etki alanınıza katılın.
11. Aşağıdaki komutu çalıştırarak sanal makineyi yeniden başlatın:
    
     ```cmd
     shutdown /r /t 0
     ```
    
12. Bir kayıt belirteci almak için [burada](/powershell/module/windowsvirtualdesktop/export-rdsregistrationinfo/) yönergeleri izleyin.
13. [Windows 7 için Windows Sanal Masaüstü Aracısını indirin.](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3JZCm)
14. [Windows 7 için Windows Sanal Masaüstü Aracı Yöneticisi'ni indirin.](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3K2e3)
15. Windows Virtual Desktop Agent yükleyicisini açın ve yönergeleri izleyin. İstendiğinde, adım 12'de oluşturduğunuz kayıt anahtarını verin.
16. Windows Sanal Masaüstü yükleyicisini açın ve yönergeleri izleyin.
17. İsteğe bağlı olarak, VM'ye doğrudan Uzak Masaüstü Protokolü erişimini kaldırmak için TCP/3389 bağlantı noktasını engelleyin.

## <a name="next-steps"></a>Sonraki adımlar

Windows Sanal Masaüstü dağıtımınız artık kullanıma hazır. Başlamak için [Windows Sanal Masaüstü istemcisinin en son sürümünü indirin.](https://aka.ms/wvd/clients/windows)

Windows Sanal Masaüstü'nde Windows 7 için bilinen sorunlar ve sorun giderme yönergelerinin bir listesi için, [Windows Sanal Masaüstü'ndeki Sorun Giderme Windows 7 sanal makinelerinde](troubleshoot-windows-7-vm.md)sorun giderme makalemize bakın.
