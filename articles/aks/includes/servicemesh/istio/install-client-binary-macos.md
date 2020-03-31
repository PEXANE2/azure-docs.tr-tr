---
author: paulbouwer
ms.topic: include
ms.date: 11/15/2019
ms.author: pabouwer
ms.openlocfilehash: 74f5b22ccc822a188059b29d9c661a15cf8412bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77594025"
---
## <a name="download-and-install-the-istio-istioctl-client-binary"></a>Istio istioctl istemci ikilisini indirin ve kurun

MacOS üzerinde bash tabanlı bir `curl` kabuk olarak, Istio sürümü `tar` indirmek ve daha sonra aşağıdaki gibi ayıklamak için kullanın:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.4.0

curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-osx.tar.gz" | tar xz
```

İstemci `istioctl` ikilisi istemci makinenizde çalışır ve Istio servis ağıyla etkileşim kurmanızı sağlar. Istio `istioctl` istemci ikilisini MacOS'ta bash tabanlı bir kabuk ta sını yüklemek için aşağıdaki komutları kullanın. Bu komutlar `istioctl` istemci ikilisini standart kullanıcı programı `PATH`konumuna kopyalar.

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