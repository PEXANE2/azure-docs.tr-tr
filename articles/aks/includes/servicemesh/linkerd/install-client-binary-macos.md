---
author: paulbouwer
ms.topic: include
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 876e05d7b18ac193edbc9cf842ea2c1bf0555d54
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77593758"
---
## <a name="download-and-install-the-linkerd-linkerd-client-binary"></a>İndirin ve Linkerd linkerd istemci ikili yükleyin

MacOS üzerinde bash tabanlı bir `curl` kabuk, aşağıdaki gibi Linkerd sürümü indirmek için kullanın:

```bash
# Specify the Linkerd version that will be leveraged throughout these instructions
LINKERD_VERSION=stable-2.6.0

curl -sLO "https://github.com/linkerd/linkerd2/releases/download/$LINKERD_VERSION/linkerd2-cli-$LINKERD_VERSION-darwin"
```

İstemci `linkerd` ikilisi istemci makinenizde çalışır ve Linkerd servis ağıyla etkileşim kurmanızı sağlar. Linkerd `linkerd` istemci ikilisini MacOS'ta bash tabanlı bir kabuk ta sını yüklemek için aşağıdaki komutları kullanın. Bu komutlar `linkerd` istemci ikilisini standart kullanıcı programı `PATH`konumuna kopyalar.

```bash
sudo cp ./linkerd2-cli-$LINKERD_VERSION-darwin /usr/local/bin/linkerd
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
