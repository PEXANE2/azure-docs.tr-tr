---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: 4ace70abe0112e0fe27d177c02bcb697746c92cc
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "78262591"
---
### <a name="set-the-storage-account-connection"></a>Depolama hesabı bağlantısını ayarla

Local. Settings. json dosyasını açın ve depolama hesabı bağlantı dizesi olan `AzureWebJobsStorage`değerini kopyalayın. Aşağıdaki Bash komutunu kullanarak `AZURE_STORAGE_CONNECTION_STRING` ortam değişkenini bağlantı dizesine ayarlayın:

```bash
AZURE_STORAGE_CONNECTION_STRING="<STORAGE_CONNECTION_STRING>"
```

`AZURE_STORAGE_CONNECTION_STRING` Ortam değişkeninde bağlantı dizesini ayarladığınızda, her seferinde kimlik doğrulaması sağlamak zorunda kalmadan depolama hesabınıza erişebilirsiniz.