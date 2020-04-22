---
author: paulbouwer
ms.topic: include
ms.date: 11/15/2019
ms.author: pabouwer
ms.openlocfilehash: e26a2c214a03243d6507296c1e981706be8c56db
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81736813"
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
# Make the new PATH permanently available for the current User
$USER_PATH = [environment]::GetEnvironmentVariable("PATH", "User") + ";C:\Istio\"
[environment]::SetEnvironmentVariable("PATH", $USER_PATH, "User")
# Make the new PATH immediately available in the current shell
$env:PATH += ";C:\Istio\"
```