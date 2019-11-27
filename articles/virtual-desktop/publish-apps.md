---
title: Windows sanal masaüstü-Azure 'da yerleşik uygulamaları yayımlama
description: Windows sanal masaüstü 'nde modern uygulamalar yayımlama.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 11/25/2019
ms.author: helohr
ms.openlocfilehash: 26ebcc7f2196267719a5f49aeeb8de00d48b9c3e
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74483738"
---
# <a name="publish-built-in-apps-in-windows-virtual-desktop"></a>Windows sanal masaüstünde yerleşik uygulamaları yayımlama

Bu makalede, Windows sanal masaüstü ortamınızda uygulamaların nasıl yayımlanacağı açıklanır.

## <a name="publish-built-in-apps"></a>Yerleşik uygulamaları yayımlama

Yerleşik bir uygulama yayımlamak için:

1. Konak havuzunuzdaki sanal makinelerden birine bağlanın.
2. [Bu makaledeki](https://docs.microsoft.com/powershell/module/appx/get-appxpackage?view=win10-ps)yönergeleri izleyerek yayımlamak Istediğiniz uygulamanın **PackageFamilyName** alın.
3. Son olarak, aşağıdaki cmdlet 'i, önceki adımda bulduğunuz **PackageFamilyName** tarafından değiştirilmiş `<PackageFamilyName>` ile çalıştırın:
   
   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -FriendlyName <remoteappname> -FilePath "shell:appsFolder\<PackageFamilyName>!App"
   ```

>[!NOTE]
> Windows sanal masaüstü yalnızca `C:\Program Files\Windows Apps`ile başlayan bir konum yüklerken uygulama yayımlamayı destekler.

## <a name="update-app-icons"></a>Uygulama simgelerini Güncelleştir

Bir uygulamayı yayımladıktan sonra, normal simge resmi yerine varsayılan Windows uygulaması simgesine sahip olur. Simgeyi normal simgesine değiştirmek için, istediğiniz simgenin görüntüsünü bir ağ paylaşımında koyun. Desteklenen görüntü biçimleri PNG, BMP, GIF, JPG, JPEG ve ICO.

## <a name="publish-microsoft-edge"></a>Microsoft Edge 'i Yayımla

Microsoft Edge 'i yayımlamak için kullandığınız işlem, diğer uygulamalar için yayımlama işleminden biraz farklıdır. Microsoft Edge 'i varsayılan giriş sayfası ile yayımlamak için şu cmdlet 'i çalıştırın:

```powershell
New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -FriendlyName <remoteappname> -FilePath "shell:Appsfolder\Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge" 
```



