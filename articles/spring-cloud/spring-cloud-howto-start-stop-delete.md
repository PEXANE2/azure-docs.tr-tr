---
title: Azure Spring Cloud uygulamanızı başlatma, durdurma ve silme | Microsoft Docs
description: Azure Spring Cloud uygulamanızı başlatma, durdurma ve silme
services: spring-cloud
author: v-vasuke
manager: jeconnoc
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/07/2019
ms.author: v-vasuke
ms.openlocfilehash: f7f76644d13c20704d2c3bd908176ac452df2a20
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72039087"
---
# <a name="how-to-start-stop-and-delete-your-azure-spring-cloud-application"></a>Azure Spring Cloud uygulamanızı başlatma, durdurma ve silme

Bu kılavuzda, Azure portal veya CLı kullanarak Azure Spring Cloud 'da bir uygulamanın durumunun nasıl değiştirileceği açıklanmaktadır.

## <a name="using-the-azure-portal"></a>Azure portalını kullanma

Uygulamanızı dağıttıktan sonra, Azure portal kullanarak **başlatabilir**, **durdurabilir**ve **silebilirsiniz** .

1. Azure portal Azure Spring Cloud Service örneğine gidin.
1. **Uygulama panosu** sekmesini seçin.
1. Durumunu değiştirmek istediğiniz uygulamayı seçin.
2. Bu uygulamanın **genel bakış** sayfasında, uygulamayı **başlatmak/durdurmak**, **yeniden başlatmak**ve **silmek** için düğmeler bulun.

## <a name="using-the-azure-cli"></a>Azure CLI kullanma

> [!NOTE]
> İsteğe bağlı parametreleri kullanabilir ve Varsayılanları Azure CLı ile yapılandırabilirsiniz. [Başvuru belgelerimizi](spring-cloud-cli-reference.md)okuyarak hakkında daha fazla bilgi edinin.

* Uygulamanızı başlatmak için:
    ```Azure CLI
    az spring-cloud app start -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* Uygulamanızı durdurmak için:
    ```Azure CLI
    az spring-cloud app stop -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* Uygulamanızı yeniden başlatmak için:
    ```Azure CLI
    az spring-cloud app restart -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* Uygulamanızı silmek için:
    ```Azure CLI
    az spring-cloud app delete -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```
