---
author: paulbouwer
ms.topic: include
ms.date: 11/15/2019
ms.author: pabouwer
ms.openlocfilehash: b310de560f9791e1fc49d54dfbf0789c38d37f57
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77594014"
---
## <a name="download-and-install-the-istio-istioctl-client-binary"></a>Istio istioctl istemci ikilisini indirme ve yükleme

Linux veya [Linux Için Windows alt sistemi][install-wsl]üzerinde Bash tabanlı bir kabukta, istio sürümünü indirmek için `curl` kullanın ve ardından `tar` ile aşağıdaki şekilde ayıklayın:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.4.0

curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-linux.tar.gz" | tar xz
```

İstemci ikili dosyası `istioctl` istemci makinenizde çalışır ve bu hizmet ağıyla etkileşim kurmanıza olanak tanır. İstemci ikilisini Linux veya [Linux Için Windows alt sistemi][install-wsl]üzerinde Bash tabanlı bir kabuğa `istioctl` yüklemek için aşağıdaki komutları kullanın. Bu komutlar `istioctl` istemci ikilisini `PATH`standart Kullanıcı programı konumuna kopyalar.

```bash
cd istio-$ISTIO_VERSION
sudo cp ./bin/istioctl /usr/local/bin/istioctl
sudo chmod +x /usr/local/bin/istioctl
```

Istio `istioctl` istemci ikilisi için komut satırı tamamlamayı tercih ediyorsanız, bunu aşağıdaki şekilde ayarlayın:

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