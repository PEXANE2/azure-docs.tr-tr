---
title: Windows sanal masaüstündeki Microsoft ekipleri-Azure
description: Microsoft ekiplerini Windows sanal masaüstü 'nde kullanma.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: a860b005457c6e02187423a3ffbbc63fe7c758b1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82187537"
---
# <a name="use-microsoft-teams-on-windows-virtual-desktop"></a>Microsoft ekiplerini Windows sanal masaüstü 'nde kullanma

> Uygulama hedefi: Windows 10 ve Windows 10 IoT Enterprise

Sanallaştırılmış ortamlar, daha fazla gecikme, yüksek ana bilgisayar CPU kullanımı ve genel ses ve video performansı gibi Microsoft ekipleri gibi işbirliği uygulamalarına yönelik benzersiz bir zorluk kümesi sunar. VDı ortamlarında Microsoft ekiplerini kullanma hakkında daha fazla bilgi edinmek için, [sanallaştırılmış Masaüstü Altyapısı Için ekipler](https://docs.microsoft.com/microsoftteams/teams-for-vdi)' ı inceleyin.

## <a name="prerequisites"></a>Ön koşullar

Windows sanal masaüstü 'nde Microsoft ekipleri kullanabilmeniz için şu işlemleri yapmanız gerekir:

- [Windows Masaüstü Istemcisini](connect-windows-7-and-10.md) Microsoft ekipleri [donanım gereksinimlerini](https://docs.microsoft.com/microsoftteams/hardware-requirements-for-the-teams-app)karşılayan bir Windows 10 cihazına yükler.
- Windows 10 çoklu oturum veya Windows 10 Enterprise sanal makinesine (VM) bağlanın.
- [Ağınızı](https://docs.microsoft.com/microsoftteams/prepare-network) Microsoft ekipleri için hazırlayın.

## <a name="use-unoptimized-microsoft-teams"></a>En iyi duruma getirilmiş Microsoft ekiplerini kullanma

Microsoft ekiplerinizin sohbet ve işbirliği özelliklerinden yararlanmak için Windows sanal masaüstü ortamlarınızdaki Microsoft ekiplerini kullanabilirsiniz. Windows sanal masaüstü, VDı ses/video (AV) iyileştirmeleri üzerinde takımları desteklemez. Çağrılar ve toplantılar desteklenmez. Kuruluşunuzun ilkeleri izin verir, hala ses çağrıları yapabilir ve ses ile toplantılara katılabilir, ancak en iyi duruma getirilmiş ses kalitesi, ana bilgisayar yapılandırmanıza göre farklılık gösterir ve güvenilir olmayabilir. Daha fazla bilgi edinmek için bkz. [VDI performans değerlendirmesi üzerinde takımlar](https://docs.microsoft.com/microsoftteams/teams-for-vdi#teams-on-vdi-performance-considerations).

### <a name="prepare-your-image-for-teams"></a>Görüntünüzü takımlar için hazırlama

Ekipte makine başına yüklemeyi etkinleştirmek için konakta aşağıdaki kayıt defteri anahtarını ayarlayın:

```shell
  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Teams\IsWVDEnvironment]
  Type: REG_DWORD
  Value: 1
```

### <a name="install-microsoft-teams"></a>Microsoft ekipleri 'nı yükler

Ekipler masaüstü uygulamasını makine başına yükleme kullanarak dağıtabilirsiniz. Microsoft ekiplerinizi Windows sanal masaüstü ortamınıza yüklemek için:

1. Ortamınızla eşleşen [TAKıMLAR MSI paketini](https://docs.microsoft.com/microsoftteams/teams-for-vdi#deploy-the-teams-desktop-app-to-the-vm) indirin. 64 bitlik bir yükleyicinin bir 64 bit işletim sisteminde kullanılması önerilir.
2. MSI 'yi konak VM 'ye yüklemek için bu komutu çalıştırın.

      ```shell
      msiexec /i <msi_name> /l*v < install_logfile_name> ALLUSER=1
      ```

      Bu, takımları program dosyalarına veya program dosyalarına (x86) yükler. Bir sonraki oturum açışınızda ve takımları başlattığınızda, uygulama kimlik bilgilerinizi ister.

      > [!NOTE]
      > Kullanıcılar ve Yöneticiler, oturum açma sırasında şu anda takımlar için otomatik başlatmayı devre dışı bırakamıyorum.

      MSI 'yi konak VM 'den kaldırmak için şu komutu çalıştırın:

      ```shell
      msiexec /passive /x <msi_name> /l*v <uninstall_logfile_name>
      ```

      > [!NOTE]
      > Ekipleri ALLUSER = 1 MSI ayarı ile yüklerseniz otomatik güncelleştirmeler devre dışı bırakılır. Ekipleri ayda en az bir kez güncelleştirdiğinizden emin olmanızı öneririz. Ekipler masaüstü uygulamasını dağıtma hakkında daha fazla bilgi edinmek için, [takımlar masaüstü UYGULAMASıNı VM 'ye dağıtmayı](https://docs.microsoft.com/microsoftteams/teams-for-vdi#deploy-the-teams-desktop-app-to-the-vm)inceleyin.

## <a name="customize-remote-desktop-protocol-properties-for-a-host-pool"></a>Bir konak havuzu için Uzak Masaüstü Protokolü özelliklerini özelleştirme
Çoklu izleyici deneyimi gibi bir konak havuzunun Uzak Masaüstü Protokolü (RDP) özelliklerini özelleştirmek, mikrofon ve ses yeniden yönlendirmeyi etkinleştirmek, kullanıcılarınız için gereksinimlerinize göre en iyi deneyimi sunmanızı sağlar. **Set-RdsHostPool** cmdlet 'inde **-customrdpproperty** parametresini kullanarak Windows sanal masaüstündeki RDP özelliklerini özelleştirebilirsiniz.
Desteklenen özelliklerin tam listesi ve bunların varsayılan değerleri için bkz. [desteklenen RDP dosyası ayarları](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/rdp-files?context=/azure/virtual-desktop/context/context) .
