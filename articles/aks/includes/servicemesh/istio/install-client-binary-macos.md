---
author: paulbouwer
ms.topic: include
ms.date: 11/15/2019
ms.author: pabouwer
ms.openlocfilehash: 74f5b22ccc822a188059b29d9c661a15cf8412bf
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77594025"
---
## <a name="download-and-install-the-istio-istioctl-client-binary"></a>Istio istioctl istemci ikilisini indirme ve yükleme

MacOS 'ta Bash tabanlı bir kabukta, `curl` Istio sürümünü indirmek için kullanın ve ardından aşağıdaki gibi ile ayıklayın `tar` :

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.4.0

curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-osx.tar.gz" | tar xz
```

`istioctl`İstemci ikili dosyası, istemci makinenizde çalışır ve Istio hizmeti ağıyla etkileşime girebilmeniz için sağlar. `istioctl`MacOS 'da Bash tabanlı bir kabuğa Istio Client ikilisini yüklemek için aşağıdaki komutları kullanın. Bu komutlar, `istioctl` istemci ikilisini içindeki standart Kullanıcı programı konumuna kopyalar `PATH` .

```bash
cd istio-$ISTIO_VERSION
sudo cp ./bin/istioctl /usr/local/bin/istioctl
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