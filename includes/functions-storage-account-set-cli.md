---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: 4ace70abe0112e0fe27d177c02bcb697746c92cc
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78262591"
---
### <a name="set-the-storage-account-connection"></a>Depolama hesabı bağlantısını ayarlama

local.settings.json dosyasını açın ve `AzureWebJobsStorage`Depolama hesabı bağlantı dizesi olan değerini kopyalayın. Bu `AZURE_STORAGE_CONNECTION_STRING` Bash komutunu kullanarak ortam değişkenini bağlantı dizesine ayarlayın:

```bash
AZURE_STORAGE_CONNECTION_STRING="<STORAGE_CONNECTION_STRING>"
```

Ortam değişkenindeki bağlantı `AZURE_STORAGE_CONNECTION_STRING` dizesini ayarladığınızda, her seferinde kimlik doğrulaması sağlamak zorunda kalmadan Depolama hesabınıza erişebilirsiniz.