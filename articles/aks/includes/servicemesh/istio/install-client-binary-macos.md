---
author: paulbouwer
ms.service: container-service
ms.topic: include
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 06479b4396ab26c280cc6246d774bc30b5ea1c76
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72530400"
---
## <a name="download-and-install-the-istio-istioctl-client-binary"></a>Istio istioctl istemci ikilisini indirme ve yükleme

MacOS üzerinde Bash tabanlı bir kabukta, Istio sürümünü indirmek için `curl` kullanın ve ardından `tar` ile aşağıdaki şekilde ayıklayın:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.3.2

curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-osx.tar.gz" | tar xz
```

İstemci ikili dosyası `istioctl` istemci makinenizde çalışır ve bu hizmet ağıyla etkileşim kurmanıza olanak tanır. MacOS `istioctl` istemci ikilisini MacOS 'da Bash tabanlı bir kabuğa yüklemek için aşağıdaki komutları kullanın. Bu komutlar `istioctl` istemci ikilisini `PATH` standart Kullanıcı programı konumuna kopyalar.

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