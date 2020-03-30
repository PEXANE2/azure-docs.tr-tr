---
title: Windows 7 sanal makineler Windows Sanal Masaüstü - Azure
description: Windows Sanal Masaüstü ortamında Windows 7 sanal makineler (VM) sorunları nın nasıl giderilir?
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 09/23/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: a2ff3f6fa9896e45ecd6ab40d40d46a046edf1cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127378"
---
# <a name="troubleshoot-windows-7-virtual-machines-in-windows-virtual-desktop"></a>Windows Sanal Masaüstü’nde Windows 7 sanal makinesiyle ilgili sorunları giderme

Windows Sanal Masaüstü oturumunu yapılandırırken yaşadığınız sorunları gidermek için bu makaleyi kullanın sanal makineler (VM'ler).

## <a name="known-issues"></a>Bilinen sorunlar

Windows Sanal Masaüstü Bilgisayarlardaki Windows 7 aşağıdaki özellikleri desteklemez:

- Sanallaştırılmış uygulamalar (RemoteApps)
- Saat dilimi yeniden yönlendirme
- Otomatik DPI ölçekleme

Windows Sanal Masaüstü yalnızca Windows 7 için tam masaüstü bilgisayarlara sanallaştırabilir.

Otomatik DPI ölçekleme desteklenmese de, Uzak Masaüstü istemcisindeki simgeye sağ tıklayarak ve **Çözünürlük'ü**seçerek sanal makinenizdeki çözünürlüğü el ile değiştirebilirsiniz.

## <a name="error-cant-access-the-remote-desktop-user-group"></a>Hata: Uzak Masaüstü Kullanıcı grubuna erişemiyorum

Windows Sanal Masaüstü Uzak Masaüstü Kullanıcısı grubunda sizi veya kullanıcılarınızın kimlik bilgilerini bulamazsa, aşağıdaki hata iletilerinden birini görebilirsiniz:

- "Bu kullanıcı Uzak Masaüstü Kullanıcı grubunun bir üyesi değildir"
- "Uzak Masaüstü Hizmetleri aracılığıyla oturum açma izni niz olmalıdır"

Bu hatayı düzeltmek için, kullanıcıyı Uzak Masaüstü Kullanıcı grubuna ekleyin:

1. Azure portalı açın.
2. Hata iletisini gördüğünüz sanal makineyi seçin.
3. **Bir komutu çalıştır'ı**seçin.
4. Eklemek istediğiniz kullanıcının adı ile `<username>` değiştirilen aşağıdaki komutu çalıştırın:
   
   ```cmd
   net localgroup "Remote Desktop Users" <username> /add
   ```