---
title: Windows 7 sanal makineleri Windows sanal masaüstü (klasik)-Azure
description: Windows sanal masaüstü (klasik) ortamındaki Windows 7 sanal makineleri (VM 'Ler) için sorunları çözme.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: e7f433668c34fb5edc35889adcd604023202ada4
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87286351"
---
# <a name="troubleshoot-windows-7-virtual-machines-in-windows-virtual-desktop-classic"></a>Windows Virtual Desktop 'ta Windows 7 sanal makinelerinin sorunlarını giderme (klasik)

>[!IMPORTANT]
>Bu içerik, Windows sanal masaüstü nesneleri Azure Resource Manager desteklemeyen Windows sanal masaüstü (klasik) için geçerlidir.

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

1. Azure portalını açın.
2. Hata iletisini gördüğünüz sanal makineyi seçin.
3. **Çalıştır komutunu**seçin.
4. Aşağıdaki komutu, `<username>` eklemek istediğiniz kullanıcının adı ile değiştirilmiş şekilde çalıştırın:
   
   ```cmd
   net localgroup "Remote Desktop Users" <username> /add
   ```