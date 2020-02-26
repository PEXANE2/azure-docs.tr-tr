---
author: paulbouwer
ms.topic: include
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 7a22256c88f9cee3ce62c68a2de4a5974d76f026
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77593756"
---
## <a name="download-and-install-the-linkerd-linkerd-client-binary"></a>Linkerd linkerd istemci ikilisini indirin ve yükleyin

Linux veya [Linux Için Windows alt sistemi][install-wsl]üzerinde Bash tabanlı bir kabukta, Linkerd sürümünü şu şekilde indirmek için `curl` kullanın:

```bash
# Specify the Linkerd version that will be leveraged throughout these instructions
LINKERD_VERSION=stable-2.6.0

curl -sLO "https://github.com/linkerd/linkerd2/releases/download/$LINKERD_VERSION/linkerd2-cli-$LINKERD_VERSION-linux"
```

`linkerd` istemci ikili dosyası, istemci makinenizde çalışır ve Linkerd hizmet ağıyla etkileşime girebilmeniz için izin verir. Lınkerd `linkerd` istemci ikilisini Linux veya [Linux Için Windows alt sistemi][install-wsl]üzerinde Bash tabanlı bir kabuğa yüklemek için aşağıdaki komutları kullanın. Bu komutlar `linkerd` istemci ikilisini `PATH`standart Kullanıcı programı konumuna kopyalar.

```bash
sudo cp ./linkerd2-cli-$LINKERD_VERSION-linux /usr/local/bin/linkerd
sudo chmod +x /usr/local/bin/linkerd
```

Linkerd `linkerd` istemci ikilisi için komut satırı tamamlamayı isterseniz aşağıdaki şekilde ayarlayın:

```bash
# Generate the bash completion file and source it in your current shell
mkdir -p ~/completions && linkerd completion bash > ~/completions/linkerd.bash
source ~/completions/linkerd.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/linkerd.bash" >> ~/.bashrc
```

<!-- LINKS - external -->
[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10