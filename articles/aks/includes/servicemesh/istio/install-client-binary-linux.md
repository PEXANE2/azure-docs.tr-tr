---
author: paulbouwer
ms.topic: include
ms.date: 11/15/2019
ms.author: pabouwer
ms.openlocfilehash: b310de560f9791e1fc49d54dfbf0789c38d37f57
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77594014"
---
## <a name="download-and-install-the-istio-istioctl-client-binary"></a>Istio istioctl istemci ikilisini indirin ve kurun

Linux için Linux veya [Windows Alt Sistemi][install-wsl]üzerinde `curl` bash tabanlı kabuk olarak, Istio sürümü indirmek ve daha sonra aşağıdaki `tar` gibi ayıklamak için kullanın:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.4.0

curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-linux.tar.gz" | tar xz
```

İstemci `istioctl` ikilisi istemci makinenizde çalışır ve Istio servis ağıyla etkileşim kurmanızı sağlar. Linux için Linux veya Windows Alt `istioctl` Sistemi üzerinde bash tabanlı kabuk istio istemci ikili yüklemek [için][install-wsl]aşağıdaki komutları kullanın. Bu komutlar `istioctl` istemci ikilisini standart kullanıcı programı `PATH`konumuna kopyalar.

```bash
cd istio-$ISTIO_VERSION
sudo cp ./bin/istioctl /usr/local/bin/istioctl
sudo chmod +x /usr/local/bin/istioctl
```

Istio `istioctl` istemci ikilisi için komut satırı tamamlama istiyorsanız, aşağıdaki gibi ayarlayın:

```bash
# Generate the bash completion file and source it in your current shell
mkdir -p ~/completions && istioctl collateral --bash -o ~/completions
source ~/completions/istioctl.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/istioctl.bash" >> ~/.bashrc
```

<!-- LINKS - external -->
[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10