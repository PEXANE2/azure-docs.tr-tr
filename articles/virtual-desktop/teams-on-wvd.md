---
title: Windows Sanal Masaüstünde Microsoft Ekipleri - Azure
description: Windows Sanal Masaüstünde Microsoft Ekipleri nasıl kullanılır?
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 15a4c9b16b102310fd02f8db3a4fb93cff84882b
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81314060"
---
# <a name="use-microsoft-teams-on-windows-virtual-desktop"></a>Windows Virtual masaüstünde Microsoft Ekipleri'ni kullanma

> Şu şekilde uygulanır: Windows 10 ve Windows 10 IoT Enterprise

Sanallaştırılmış ortamlar, artan gecikme süresi, yüksek ana bilgisayar CPU kullanımı ve kötü genel ses ve video performansı da dahil olmak üzere Microsoft Teams gibi işbirliği uygulamaları için benzersiz bir dizi zorluk sunar. VDI ortamlarında Microsoft Ekiplerini kullanma hakkında daha fazla bilgi edinmek [için Sanallaştırılmış Masaüstü Altyapısı için Takımlar'a](https://docs.microsoft.com/microsoftteams/teams-for-vdi)göz atın.

## <a name="prerequisites"></a>Ön koşullar

Microsoft Ekiplerini Windows Sanal Masaüstünde kullanabilmeniz için önce şunları yapmanız gerekir:

- [Windows Desktop istemcisini](connect-windows-7-and-10.md) Microsoft Teams [donanım gereksinimlerini](https://docs.microsoft.com/microsoftteams/hardware-requirements-for-the-teams-app)karşılayan bir Windows 10 aygıtına yükleyin.
- Windows 10 Çok oturumlu veya Windows 10 Enterprise sanal makinesine (VM) bağlanın.
- Ağınızı Microsoft Ekipleri için [hazırlayın.](https://docs.microsoft.com/microsoftteams/prepare-network)

## <a name="use-unoptimized-microsoft-teams"></a>En iyi duruma getirilmemiş Microsoft Ekiplerini kullanma

Windows Sanal Masaüstü ortamlarınızda optimize edilmemiş Microsoft Ekiplerini, Microsoft Teams'in tam sohbet ve işbirliği özelliklerinin yanı sıra sesli arama dan yararlanmak için kullanabilirsiniz. En iyi duruma getirilmemiş aramalar ana bilgisayar CPU'nuzdan daha fazlasını kullandığından, aramalardaki ses kalitesi ana bilgisayar yapılandırmanıza göre değişir.

### <a name="prepare-your-image-for-teams"></a>Resminizi Takımlara hazırlayın

Makine başına ekipler yüklemesini etkinleştirmek için, ana bilgisayarda aşağıdaki kayıt defteri anahtarını ayarlayın:

```shell
  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Teams\IsWVDEnvironment]
  Type: REG_DWORD
  Value: 1
```

### <a name="install-microsoft-teams"></a>Microsoft Ekiplerini Yükleme

Ekipler masaüstü uygulamasını makine başına yükleme yi kullanarak dağıtabilirsiniz. Microsoft Ekiplerini Windows Sanal Masaüstü ortamınıza yüklemek için:

1. Ortamınızla eşleşen [Takımlar MSI paketini](https://docs.microsoft.com/microsoftteams/teams-for-vdi#deploy-the-teams-desktop-app-to-the-vm) indirin. 64 bit işletim sisteminde 64 bit yükleyici kullanmanızı öneririz.
2. MSI'ı ana bilgisayar VM'ye yüklemek için bu komutu çalıştırın.

      ```shell
      msiexec /i <msi_name> /l*v < install_logfile_name> ALLUSER=1
      ```

      Bu, Takımlar'ı Program Dosyaları'na veya Program Dosyalarına (x86) yükler. Bir sonraki oturum açtığınızda ve Takımlar'ı başlattığınızda, uygulama kimlik bilgilerinizi ister.

      > [!NOTE]
      > Kullanıcılar ve yöneticiler şu anda oturum açma sırasında Takımlar için otomatik başlatmayı devre dışı kalamaz.

      Ana bilgisayar VM'den MSI'ı kaldırmak için şu komutu çalıştırın:

      ```shell
      msiexec /passive /x <msi_name> /l*v <uninstall_logfile_name>
      ```

      > [!NOTE]
      > ALLUSER=1 ayarlı MSI ayarlı Takımlar yüklerseniz, otomatik güncelleştirmeler devre dışı bırakılır. Takımları ayda en az bir kez güncellediğinden emin olmamızı öneririz.
      
### <a name="customize-remote-desktop-protocol-properties-for-a-host-pool"></a>Ana bilgisayar havuzu için Uzak Masaüstü Protokolü özelliklerini özelleştirme
Çok monitör deneyimi gibi bir ana bilgisayar havuzunun Uzak Masaüstü Protokolü (RDP) özelliklerini özelleştirmek, Mikrofon ve ses yönlendirmesini etkinleştirmek, kullanıcılarınız için gereksinimlerine göre en iyi deneyimi sunmanıza olanak tanır. **Set-RdsHostPool** cmdlet'teki **-CustomRdpProperty** parametresini kullanarak Windows Sanal Masaüstü'ndeki RDP özelliklerini özelleştirebilirsiniz.
Desteklenen özelliklerin ve bunların varsayılan değerlerinin tam listesi için [desteklenen RDP dosya ayarlarına](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/rdp-files?context=/azure/virtual-desktop/context/context) bakın.
