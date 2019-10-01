---
title: Windows Virtual Desktop 'ta Windows 7 sanal makinelerinin sorunlarını giderme-Azure
description: Windows Sanal Masaüstü ortamındaki Windows 7 sanal makineleri (VM 'Ler) için sorunları çözme.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 09/23/2019
ms.author: helohr
ms.openlocfilehash: 7fdb46ac7cffc4eb6a791304c7a6b58378806296
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71679899"
---
# <a name="troubleshoot-windows-7-virtual-machines-in-windows-virtual-desktop"></a>Windows sanal masaüstündeki Windows 7 sanal makinelerinin sorunlarını giderme

Windows sanal masaüstü oturumu ana bilgisayarı sanal makinelerini (VM 'Ler) yapılandırırken karşılaştığınız sorunları gidermek için bu makaleyi kullanın.

## <a name="known-issues"></a>Bilinen sorunlar

Windows sanal masaüstleri üzerinde Windows 7 aşağıdaki özellikleri desteklemez:

- Sanallaştırılmış uygulamalar (RemoteApps)
- Saat dilimi yönlendirmesi
- Otomatik DPı ölçeklendirme

Windows sanal masaüstü, Windows 7 için yalnızca tam masaüstlerini sanallaştırtırabilir.

Otomatik DPı ölçeklendirme desteklenirken, uzak masaüstü istemcisinde simgeye sağ tıklayıp **Çözümleme**' yi seçerek sanal makinenizde çözümü el ile değiştirebilirsiniz.

## <a name="error-cant-access-the-remote-desktop-user-group"></a>Hata: Uzak Masaüstü kullanıcı grubuna erişilemiyor

Windows sanal masaüstü, uzak masaüstü kullanıcı grubunda sizi veya kullanıcılarınızın kimlik bilgilerini bulamazsa, aşağıdaki hata iletilerinden birini görebilirsiniz:

- "Bu Kullanıcı, uzak masaüstü Kullanıcı grubunun bir üyesi değil"
- "Uzak Masaüstü Hizmetleri aracılığıyla oturum açmak için izin verilmelidir"

Bu hatayı onarmak için kullanıcıyı uzak masaüstü kullanıcı grubuna ekleyin:

1. Azure portal açın.
2. Hata iletisini gördüğünüz sanal makineyi seçin.
3. **Çalıştır komutunu**seçin.
4. @No__t-0 ile, eklemek istediğiniz kullanıcının adı ile değiştirilmiş olan aşağıdaki komutu çalıştırın:
   
   ```cmd
   localgroup "Remote Desktop Users" <username> /add
   ```