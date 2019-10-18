---
author: paulbouwer
ms.service: container-service
ms.topic: include
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 1c63d02a2207a396183be4fdcc44074977d37b1a
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72530374"
---
## <a name="download-and-install-the-istio-istioctl-client-binary"></a>Istio istioctl istemci ikilisini indirme ve yükleme

Windows üzerinde PowerShell tabanlı bir kabukta, Istio sürümünü indirmek için `Invoke-WebRequest` kullanın ve ardından `Expand-Archive` ile aşağıdaki şekilde ayıklayın:

```powershell
# Specify the Istio version that will be leveraged throughout these instructions
$ISTIO_VERSION="1.3.2"

# Enforce TLS 1.2
[Net.ServicePointManager]::SecurityProtocol = "tls12"
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-win.zip" -OutFile "istio-$ISTIO_VERSION.zip"
Expand-Archive -Path "istio-$ISTIO_VERSION.zip" -DestinationPath .
```

İstemci ikili dosyası `istioctl` istemci makinenizde çalışır ve bu hizmet ağıyla etkileşim kurmanıza olanak tanır. Windows üzerinde PowerShell tabanlı bir kabuğa `istioctl` istemci ikilisini yüklemek için aşağıdaki komutları kullanın. Bu komutlar, `istioctl` istemci ikilisini bir Istio klasörüne kopyalar ve ardından, `PATH` üzerinden (geçerli kabukta) ve kalıcı olarak (kabukta yeniden başlatmalar arasında) kullanılabilir hale getirir. Bu komutları çalıştırmak için yükseltilmiş (yönetici) ayrıcalıklarına ihtiyacınız yoktur ve kabuğunu yeniden başlatmanız gerekmez.

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