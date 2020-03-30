---
author: paulbouwer
ms.topic: include
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 7a22256c88f9cee3ce62c68a2de4a5974d76f026
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77593756"
---
## <a name="download-and-install-the-linkerd-linkerd-client-binary"></a>İndirin ve Linkerd linkerd istemci ikili yükleyin

Linux için Linux veya [Windows Alt Sistemi][install-wsl]üzerinde `curl` bash tabanlı bir kabuk olarak, aşağıdaki gibi Linkerd sürümü indirmek için kullanın:

```bash
# Specify the Linkerd version that will be leveraged throughout these instructions
LINKERD_VERSION=stable-2.6.0

curl -sLO "https://github.com/linkerd/linkerd2/releases/download/$LINKERD_VERSION/linkerd2-cli-$LINKERD_VERSION-linux"
```

İstemci `linkerd` ikilisi istemci makinenizde çalışır ve Linkerd servis ağıyla etkileşim kurmanızı sağlar. Linux için Linux veya Windows Alt `linkerd` Sistemi üzerinde bash tabanlı kabuk linkerd istemci ikili yüklemek [için][install-wsl]aşağıdaki komutları kullanın. Bu komutlar `linkerd` istemci ikilisini standart kullanıcı programı `PATH`konumuna kopyalar.

```bash
sudo cp ./linkerd2-cli-$LINKERD_VERSION-linux /usr/local/bin/linkerd
sudo chmod +x /usr/local/bin/linkerd
```

Linkerd `linkerd` istemci ikilisi için komut satırı tamamlama istiyorsanız, aşağıdaki gibi ayarlayın:

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