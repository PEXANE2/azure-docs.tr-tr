---
author: paulbouwer
ms.topic: include
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 876e05d7b18ac193edbc9cf842ea2c1bf0555d54
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77593758"
---
## <a name="download-and-install-the-linkerd-linkerd-client-binary"></a>Linkerd linkerd istemci ikilisini indirin ve yükleyin

MacOS üzerinde Bash tabanlı bir kabukta, `curl` Linkerd sürümünü şu şekilde indirmek için kullanın:

```bash
# Specify the Linkerd version that will be leveraged throughout these instructions
LINKERD_VERSION=stable-2.6.0

curl -sLO "https://github.com/linkerd/linkerd2/releases/download/$LINKERD_VERSION/linkerd2-cli-$LINKERD_VERSION-darwin"
```

`linkerd`İstemci ikili dosyası, istemci makinenizde çalışır ve Linkerd hizmet ağıyla etkileşime girebilmeniz için izin verir. Inkerd `linkerd` istemci ikilisini MacOS 'da Bash tabanlı bir kabuğa yüklemek için aşağıdaki komutları kullanın. Bu komutlar, `linkerd` istemci ikilisini içindeki standart Kullanıcı programı konumuna kopyalar `PATH` .

```bash
sudo cp ./linkerd2-cli-$LINKERD_VERSION-darwin /usr/local/bin/linkerd
sudo chmod +x /usr/local/bin/linkerd
```

Linkerd istemci ikilisi için komut satırı tamamlamayı isterseniz `linkerd` aşağıdaki şekilde ayarlayın:

```bash
# Generate the bash completion file and source it in your current shell
mkdir -p ~/completions && linkerd completion bash > ~/completions/linkerd.bash
source ~/completions/linkerd.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/linkerd.bash" >> ~/.bashrc
```
