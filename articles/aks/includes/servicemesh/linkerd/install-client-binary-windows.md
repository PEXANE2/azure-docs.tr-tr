---
author: paulbouwer
ms.topic: include
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: b7d832ba375925d30a976dfde63a776b5d0742bb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77593757"
---
## <a name="download-and-install-the-linkerd-linkerd-client-binary"></a>İndirin ve Linkerd linkerd istemci ikili yükleyin

Windows'da PowerShell tabanlı bir `Invoke-WebRequest` kabukta, Linkerd yayınını aşağıdaki gibi indirmek için kullanın:

```powershell
# Specify the Linkerd version that will be leveraged throughout these instructions
$LINKERD_VERSION="stable-2.6.0"

# Enforce TLS 1.2
[Net.ServicePointManager]::SecurityProtocol = "tls12"
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/linkerd/linkerd2/releases/download/$LINKERD_VERSION/linkerd2-cli-$LINKERD_VERSION-windows.exe" -OutFile "linkerd2-cli-$LINKERD_VERSION-windows.exe"
```

İstemci `linkerd` ikilisi istemci makinenizde çalışır ve Linkerd servis ağıyla etkileşim kurmanızı sağlar. Windows'da PowerShell tabanlı bir `linkerd` kabuk linkerd istemci ikili yüklemek için aşağıdaki komutları kullanın. Bu komutlar `linkerd` istemci ikilisini Bir Bağlayıcı klasöre kopyalar ve sonra hem hemen (geçerli kabukta) hem `PATH`de kalıcı olarak (kabuk yeniden başlatmalar arasında) sizin aracılığınızla kullanılabilir hale getirir. Bu komutları çalıştırmak için yükseltilmiş (Yönetici) ayrıcalıklara ihtiyacınız yoktur ve kabuğunuzu yeniden başlatmanız gerekmez.

```powershell
# Copy linkerd.exe to C:\Linkerd
New-Item -ItemType Directory -Force -Path "C:\Linkerd"
Copy-Item -Path ".\linkerd2-cli-$LINKERD_VERSION-windows.exe" -Destination "C:\Linkerd\linkerd.exe"

# Add C:\Linkerd to PATH. 
# Make the new PATH permanently available for the current User, and also immediately available in the current shell.
$PATH = [environment]::GetEnvironmentVariable("PATH", "User") + "; C:\Linkerd\"
[environment]::SetEnvironmentVariable("PATH", $PATH, "User") 
[environment]::SetEnvironmentVariable("PATH", $PATH)
```