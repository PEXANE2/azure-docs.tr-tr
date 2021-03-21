---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: 4ace70abe0112e0fe27d177c02bcb697746c92cc
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "78262591"
---
### <a name="set-the-storage-account-connection"></a>Depolama hesabı bağlantısını ayarla

local.settings.jsdosya üzerinde açın ve `AzureWebJobsStorage` depolama hesabı bağlantı dizesi olan değerini kopyalayın. `AZURE_STORAGE_CONNECTION_STRING`Aşağıdaki Bash komutunu kullanarak ortam değişkenini bağlantı dizesine ayarlayın:

```bash
AZURE_STORAGE_CONNECTION_STRING="<STORAGE_CONNECTION_STRING>"
```

Ortam değişkeninde bağlantı dizesini ayarladığınızda `AZURE_STORAGE_CONNECTION_STRING` , her seferinde kimlik doğrulaması sağlamak zorunda kalmadan depolama hesabınıza erişebilirsiniz.