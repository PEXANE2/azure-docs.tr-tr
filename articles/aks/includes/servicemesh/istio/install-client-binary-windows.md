---
author: paulbouwer
ms.topic: include
ms.date: 11/15/2019
ms.author: pabouwer
ms.openlocfilehash: 562382cc1cfb6adb7e65d76e717df4c4e2962ba7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77594002"
---
## <a name="download-and-install-the-istio-istioctl-client-binary"></a>Istio istioctl istemci ikilisini indirin ve kurun

Windows'da PowerShell tabanlı bir `Invoke-WebRequest` kabukta, Istio yayınını `Expand-Archive` indirmek ve ardından aşağıdaki gibi ayıklamak için kullanın:

```powershell
# Specify the Istio version that will be leveraged throughout these instructions
$ISTIO_VERSION="1.4.0"

# Enforce TLS 1.2
[Net.ServicePointManager]::SecurityProtocol = "tls12"
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-win.zip" -OutFile "istio-$ISTIO_VERSION.zip"
Expand-Archive -Path "istio-$ISTIO_VERSION.zip" -DestinationPath .
```

İstemci `istioctl` ikilisi istemci makinenizde çalışır ve Istio servis ağıyla etkileşim kurmanızı sağlar. Windows'da PowerShell tabanlı bir `istioctl` kabuk istio istemci ikili yüklemek için aşağıdaki komutları kullanın. Bu komutlar `istioctl` istemci ikilisini bir Istio klasörüne kopyalar ve sonra hem hemen (geçerli kabukta) `PATH`hem de kalıcı olarak (kabuk yeniden başlatmalar arasında) sizin aracılığınızla kullanılabilir hale getirir. Bu komutları çalıştırmak için yükseltilmiş (Yönetici) ayrıcalıklara ihtiyacınız yoktur ve kabuğunuzu yeniden başlatmanız gerekmez.

```powershell
# Copy istioctl.exe to C:\Istio
cd istio-$ISTIO_VERSION
New-Item -ItemType Directory -Force -Path "C:\Istio"
Copy-Item -Path .\bin\istioctl.exe -Destination "C:\Istio\"

# Add C:\Istio to PATH. 
# Make the new PATH permanently available for the current User, and also immediately available in the current shell.
$PATH = [environment]::GetEnvironmentVariable("PATH", "User") + "; C:\Istio\"
[environment]::SetEnvironmentVariable("PATH", $PATH, "User") 
[environment]::SetEnvironmentVariable("PATH", $PATH)
```