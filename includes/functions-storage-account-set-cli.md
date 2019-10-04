---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: 89e2e64910e33d43c107ee88137de718d020d7d2
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71839106"
---
Local. Settings. json dosyasını açın ve depolama hesabı bağlantı dizesi olan `AzureWebJobsStorage` değerini kopyalayın. Bu Bash komutunu kullanarak `AZURE_STORAGE_CONNECTION_STRING` ortam değişkenini bağlantı dizesine ayarlayın:

```azurecli-interactive
AZURE_STORAGE_CONNECTION_STRING="<STORAGE_CONNECTION_STRING>"
```

@No__t-0 ortam değişkeninde bağlantı dizesini ayarladığınızda, her seferinde kimlik doğrulaması sağlamak zorunda kalmadan depolama hesabınıza erişebilirsiniz.