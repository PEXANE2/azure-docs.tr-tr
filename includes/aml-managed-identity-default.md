---
title: dosya dahil etme
description: dosya dahil etme
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 08/24/2020
ms.openlocfilehash: aa8aea66c5481454f0d7d4d118934f5fbf34a911
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2020
ms.locfileid: "89147602"
---
**Varsayılan yönetilen kimlik** , sistem tarafından atanan yönetilen kimlik veya Kullanıcı tarafından atanan ilk yönetilen kimliktir.

Bir çalıştırma sırasında bir kimliğin iki uygulaması vardır:

1. Sistem, kullanıcının depolama takar, kapsayıcı kayıt defteri ve veri depolarını ayarlamak için bir kimlik kullanır.

    * Bu durumda, sistem varsayılan olarak yönetilen kimliği kullanır.

1. Kullanıcı, gönderilen bir çalıştırma için kodun içinden kaynaklara erişmek üzere bir kimlik uygular

    * Bu durumda, kimlik bilgilerini almak için kullanmak istediğiniz yönetilen kimliğe karşılık gelen *client_id* sağlayın.
    * Alternatif olarak, *DEFAULT_IDENTITY_CLIENT_ID* ortam değişkeni aracılığıyla Kullanıcı tarafından atanan KIMLIğIN istemci kimliğini alın.

    Örneğin, varsayılan yönetilen kimliğe sahip bir veri deposu belirteci almak için:

    ```python
    client_id = os.environ.get('DEFAULT_IDENTITY_CLIENT_ID')
    credential = ManagedIdentityCredential(client_id=client_id)
    token = credential.get_token('https://storage.azure.com/')
    ```