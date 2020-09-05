---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 09/03/2020
ms.author: msmbaldwin
ms.openlocfilehash: 9440a33858cb70389e86621ae76be9c08ec56d61
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89482155"
---
1. [Etkin aboneliği olan bir Azure hesabınız](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)olduğundan emin olun.

1. [Python 2.7 + veya 3.5.3 +](https://www.python.org/downloads)' ü yükler.

1. [Azure CLI](/cli/azure/install-azure-cli)'yı yükler.

1. Yerel bir hizmet sorumlusu oluşturup, ortam değişkenleri aracılığıyla Python için Azure Key Vault Istemcisi tarafından kullanılabilir hale getirmek için [yerel geliştirme için kimlik doğrulamasını yapılandırma](/azure/developer/python/configure-local-development-environment?tabs=cmd#configure-authentication)konusundaki yönergeleri izleyin. 

    Kodu doğrudan Azure üzerinde çalıştırırken, uygulama yönetilen kimlik kullanıyorsa ayrı bir hizmet sorumlusu gerekli değildir.

1. Bir Terminal veya komut isteminde uygun bir proje klasörü oluşturun ve ardından [Python sanal ortamlarını kullanma](/azure/developer/python/configure-local-development-environment?tabs=cmd#use-python-virtual-environments) bölümünde açıklandığı gibi bir Python sanal ortamı oluşturun ve etkinleştirin

1. Azure Active Directory Identity Library 'yi yükler:

    ```terminal
    pip install azure.identity
    ```
