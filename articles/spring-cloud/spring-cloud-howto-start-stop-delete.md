---
title: Azure İlkbahar Bulutu uygulamanızı başlatın, durdurun ve silin | Microsoft Dokümanlar
description: Azure İlkbahar Bulutu uygulamanızı başlatma, durdurma ve silme
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: brendm
ms.openlocfilehash: daa549e248668add54530e90174134c4e0059b3a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76276832"
---
# <a name="start-stop-and-delete-your-azure-spring-cloud-application"></a>Azure İlkbahar Bulutu uygulamanızı başlatın, durdurun ve silin

Bu kılavuzda, Azure portalı veya Azure CLI kullanarak bir uygulamanın Azure Bahar Bulutu'ndaki durumununasıl değiştireceğiniz açıklanmaktadır.

## <a name="using-the-azure-portal"></a>Azure portalını kullanma

Bir uygulamayı dağıttıktan sonra, Azure portalını kullanarak uygulamayı başlatabilir, durdurabilir ve silebilirsiniz.

1. Azure portalındaki Azure İlkbahar Bulutu hizmet örneğinize gidin.
1. Uygulama **Panosu** sekmesini seçin.
1. Durumunu değiştirmek istediğiniz uygulamayı seçin.
1. Bu uygulamaiçin **Genel Bakış** sayfasında **Başlat/Durdur,** **Yeniden Başlat**veya **Sil'i**seçin.

## <a name="using-the-azure-cli"></a>Azure CLI kullanma

> [!NOTE]
> Azure CLI ile isteğe bağlı parametreleri kullanabilir ve varsayılanları yapılandırabilirsiniz. [Referans belgelerimizi](spring-cloud-cli-reference.md)okuyarak Azure CLI hakkında daha fazla bilgi edinin.  

İlk olarak, Azure CLI için Azure İlkbahar Bulutu uzantısını aşağıdaki gibi yükleyin:

```azurecli
az extension add --name spring-cloud
```

Ardından, şu Azure CLI işlemlerinden birini seçin:

* Başvurunuzu başlatmak için:

    ```azurecli
    az spring-cloud app start -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* Başvurunuzu durdurmak için:

    ```azurecli
    az spring-cloud app stop -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* Başvurunuzu yeniden başlatmak için:

    ```azurecli
    az spring-cloud app restart -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* Uygulamanızı silmek için:

    ```azurecli
    az spring-cloud app delete -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```
