---
author: paulbouwer
ms.topic: include
ms.date: 10/02/2020
ms.author: pabouwer
ms.openlocfilehash: 5dc9686e4a9994a085cc9f4a4631e66b05d7949d
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91666742"
---
## <a name="download-and-install-the-istio-istioctl-client-binary"></a>Istio istioctl istemci ikilisini indirme ve yükleme

MacOS 'ta Bash tabanlı bir kabukta, `curl` Istio sürümünü indirmek için kullanın ve ardından aşağıdaki gibi ile ayıklayın `tar` :

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.7.3

curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istioctl-$ISTIO_VERSION-osx.tar.gz" | tar xz
```

`istioctl`İstemci ikilisi istemci makinenizde çalışır ve AKS kümenize Istio 'yu yüklemenize ve yönetmenize olanak sağlar. `istioctl`MacOS 'da Bash tabanlı bir kabuğa Istio Client ikilisini yüklemek için aşağıdaki komutları kullanın. Bu komutlar, `istioctl` istemci ikilisini içindeki standart Kullanıcı programı konumuna kopyalar `PATH` .

```bash
sudo mv ./istioctl /usr/local/bin/istioctl
sudo chmod +x /usr/local/bin/istioctl
```

Istio istemci ikilisi için komut satırı tamamlamayı isterseniz `istioctl` , bunu aşağıdaki şekilde ayarlayın:

```bash
# Generate the bash completion file and source it in your current shell
mkdir -p ~/completions && istioctl collateral --bash -o ~/completions
source ~/completions/istioctl.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/istioctl.bash" >> ~/.bashrc
```