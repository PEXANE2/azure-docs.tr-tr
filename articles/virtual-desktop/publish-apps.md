---
title: Windows Sanal Masaüstü'nde yerleşik uygulamalar yayınlama - Azure
description: Windows Sanal Masaüstü'nde yerleşik uygulamalar nasıl yayınlanır?
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 12/03/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: a697c9a62e52e82a550969e1852abd1489ed59b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127747"
---
# <a name="publish-built-in-apps-in-windows-virtual-desktop"></a>Windows Sanal Masaüstü'nde yerleşik uygulamalar yayınlama

Bu makalede, Windows Sanal Masaüstü ortamınızda uygulamaları nasıl yayımlayacağınızı anlatacağım.

## <a name="publish-built-in-apps"></a>Yerleşik uygulamaları yayımlama

Yerleşik bir uygulama yayınlamak için:

1. Ana bilgisayar havuzunuzdaki sanal makinelerden birine bağlanın.
2. [Bu makaledeki](/powershell/module/appx/get-appxpackage?view=win10-ps/)talimatları izleyerek yayınlamak istediğiniz uygulamanın **PackageFamilyName'sini** alın.
3. Son olarak, bir önceki `<PackageFamilyName>` adımda bulduğunuz **PackageFamilyName** ile değiştirilen aşağıdaki cmdlet'i çalıştırın:
   
   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -FriendlyName <remoteappname> -FilePath "shell:appsFolder\<PackageFamilyName>!App"
   ```

>[!NOTE]
> Windows Sanal Masaüstü yalnızca yükleme konumları `C:\Program Files\Windows Apps`ile başlayan uygulamaları yayımlama yı destekler.

## <a name="update-app-icons"></a>Uygulama simgelerini güncelleştir

Bir uygulamayı yayımladıktan sonra, normal simge resmi yerine varsayılan Windows uygulaması simgesine sahip olur. Simgeyi normal simgesine değiştirmek için, istediğiniz simgenin görüntüsünü ağ paylaşımına koyun. Desteklenen görüntü biçimleri PNG, BMP, GIF, JPG, JPEG ve ICO'dur.

## <a name="publish-microsoft-edge"></a>Microsoft Edge yayımla

Microsoft Edge'i yayımlamak için kullandığınız işlem, diğer uygulamaların yayımlama işleminden biraz farklıdır. Varsayılan ana sayfayla Microsoft Edge'i yayımlamak için şu cmdlet'i çalıştırın:

```powershell
New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -FriendlyName <remoteappname> -FilePath "shell:Appsfolder\Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge" 
```

## <a name="next-steps"></a>Sonraki adımlar

- [Windows Virtual Desktop kullanıcıları için akışı Özelleştir'de](customize-feed-for-virtual-desktop-users.md)kullanıcıların uygulamalarının nasıl görüntüleneceğini düzenlemek için akışları nasıl yapılandırabilirsiniz hakkında bilgi edinin.
- [Kurulum MSIX app attach'daki](app-attach.md)MSIX uygulama ekleme özelliği hakkında bilgi edinin.

