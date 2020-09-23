---
title: Azure Spring Cloud uygulamanızı başlatma, durdurma ve silme | Microsoft Docs
description: Azure Spring Cloud uygulamanızı başlatma, durdurma ve silme
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: brendm
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: 479daa9a94247ada30c54a9c5df3471035765087
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90908278"
---
# <a name="start-stop-and-delete-your-azure-spring-cloud-application"></a>Azure Spring Cloud uygulamanızı başlatma, durdurma ve silme

**Bu makale şu şekilde geçerlidir:** ✔️ Java ✔️ C #

Bu kılavuzda, Azure Spring Cloud 'da Azure portal veya Azure CLı kullanarak bir uygulamanın durumunun nasıl değiştirileceği açıklanmaktadır.

## <a name="using-the-azure-portal"></a>Azure portalını kullanma

Bir uygulamayı dağıttıktan sonra, Azure portal kullanarak başlatabilir, durdurabilir ve silebilirsiniz.

1. Azure portal Azure Spring Cloud Service örneğine gidin.
1. **Uygulama panosu** sekmesini seçin.
1. Durumunu değiştirmek istediğiniz uygulamayı seçin.
1. Bu uygulamanın **genel bakış** sayfasında **Başlat/Durdur**, **Yeniden Başlat**veya **Sil**' i seçin.

## <a name="using-the-azure-cli"></a>Azure CLI'yı kullanma

> [!NOTE]
> İsteğe bağlı parametreleri kullanabilir ve Varsayılanları Azure CLı ile yapılandırabilirsiniz. [Başvuru belgelerimizi](/cli/azure/ext/spring-cloud/spring-cloud?view=azure-cli-latest&preserve-view=true)okuyarak Azure CLI hakkında daha fazla bilgi edinin.  

İlk olarak, Azure CLı için Azure yay bulutu uzantısını aşağıdaki şekilde yüklemeniz gerekir:

```azurecli
az extension add --name spring-cloud
```

Ardından, şu Azure CLı işlemlerinden birini seçin:

* Uygulamanızı başlatmak için:

    ```azurecli
    az spring-cloud app start -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* Uygulamanızı durdurmak için:

    ```azurecli
    az spring-cloud app stop -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* Uygulamanızı yeniden başlatmak için:

    ```azurecli
    az spring-cloud app restart -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* Uygulamanızı silmek için:

    ```azurecli
    az spring-cloud app delete -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```
