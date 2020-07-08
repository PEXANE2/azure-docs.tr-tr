---
title: Windows sanal masaüstü-Azure 'da yerleşik uygulamaları yayımlama
description: Windows sanal masaüstü 'nde yerleşik uygulamaları yayımlama.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 11416eb06e29b4621c1949f193318d32d76cdde3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85212726"
---
# <a name="publish-built-in-apps-in-windows-virtual-desktop"></a>Windows sanal masaüstünde yerleşik uygulamaları yayımlama

>[!IMPORTANT]
>Bu içerik, Azure Resource Manager Windows sanal masaüstü nesneleriyle Spring 2020 güncelleştirmesine yöneliktir. Windows sanal masaüstü Fall 2019 sürümünü Azure Resource Manager nesneleri olmadan kullanıyorsanız, [Bu makaleye](./virtual-desktop-fall-2019/publish-apps-2019.md)bakın.
>
> Windows sanal masaüstü Spring 2020 güncelleştirmesi şu anda genel önizlemededir. Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve bunu üretim iş yükleri için kullanmanızı önermiyoruz. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede, Windows sanal masaüstü ortamınızda uygulamaların nasıl yayımlanacağı açıklanır.

## <a name="publish-built-in-apps"></a>Yerleşik uygulamaları yayımlama

Yerleşik bir uygulama yayımlamak için:

1. Konak havuzunuzdaki sanal makinelerden birine bağlanın.
2. [Bu makaledeki](/powershell/module/appx/get-appxpackage?view=win10-ps/)yönergeleri izleyerek yayımlamak Istediğiniz uygulamanın **PackageFamilyName** alın.
3. Son olarak, `<PackageFamilyName>` önceki adımda bulduğunuz **PackageFamilyName** ile değiştirilmiş aşağıdaki cmdlet 'i çalıştırın:

   ```powershell
   New-AzWvdApplication -Name <applicationname> -ResourceGroupName <resourcegroupname> -ApplicationGroupName <appgroupname> -FilePath "shell:appsFolder\<PackageFamilyName>!App" -CommandLineSetting <Allow|Require|DoNotAllow> -IconIndex 0 -IconPath <iconpath> -ShowInPortal:$true
   ```

>[!NOTE]
> Windows sanal masaüstü, uygulamaları yalnızca ile başlayan bir konum yüklerken yayımlamayı destekler `C:\Program Files\WindowsApps` .

## <a name="update-app-icons"></a>Uygulama simgelerini Güncelleştir

Bir uygulamayı yayımladıktan sonra, normal simge resmi yerine varsayılan Windows uygulaması simgesine sahip olur. Simgeyi normal simgesine değiştirmek için, istediğiniz simgenin görüntüsünü bir ağ paylaşımında koyun. Desteklenen görüntü biçimleri PNG, BMP, GIF, JPG, JPEG ve ICO.

## <a name="publish-microsoft-edge"></a>Microsoft Edge 'i Yayımla

Microsoft Edge 'i yayımlamak için kullandığınız işlem, diğer uygulamalar için yayımlama işleminden biraz farklıdır. Microsoft Edge 'i varsayılan giriş sayfası ile yayımlamak için şu cmdlet 'i çalıştırın:

```powershell
New-AzWvdApplication -Name -ResourceGroupName -ApplicationGroupName -FilePath "shell:Appsfolder\Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge" -CommandLineSetting <Allow|Require|DoNotAllow> -iconPath "C:\Windows\SystemApps\Microsoft.MicrosoftEdge_8wekyb3d8bbwe\microsoftedge.exe" -iconIndex 0 -ShowInPortal:$true
```

## <a name="next-steps"></a>Sonraki adımlar

- Kullanıcıların [Windows sanal masaüstü kullanıcıları için özet akışını özelleştirmek](customize-feed-for-virtual-desktop-users.md)üzere uygulamaların nasıl görüntülendiğini düzenlemek için akışların nasıl yapılandırılacağı hakkında bilgi edinin.
- Msix uygulama iliştirme özelliği hakkında daha fazla bilgi [edinin.](app-attach.md)

