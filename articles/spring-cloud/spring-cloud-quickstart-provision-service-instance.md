---
title: Hızlı başlangıç-Azure yay bulut hizmetini sağlama
description: Uygulama dağıtımı için Azure Spring Cloud Service örneğinin oluşturulmasını açıklar.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 08/03/2020
ms.custom: devx-track-java
ms.openlocfilehash: c91237e3a14c60e477f58be0bf62f634b462960b
ms.sourcegitcommit: e69bb334ea7e81d49530ebd6c2d3a3a8fa9775c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88951921"
---
# <a name="quickstart-provision-azure-spring-cloud-service"></a>Hızlı başlangıç: Azure yay bulut hizmetini sağlama

Azure portal veya Azure CLı kullanarak Azure Spring Cloud örneğini oluşturabilirsiniz.  Her iki yöntem de aşağıdaki yordamlarda açıklanmıştır.
## <a name="prerequisites"></a>Önkoşullar

* [JDK 8 ' i yükler](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
* [Azure aboneliğine kaydolma](https://azure.microsoft.com/free/)
* Seçim [Azure CLI sürüm 2.0.67 veya üstünü yükleyip](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) Azure Spring Cloud uzantısını şu komutla birlikte yüklersiniz: `az extension add --name spring-cloud`
* Seçim [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053-azure-toolkit-for-intellij/) ve [oturum açmayı](https://docs.microsoft.com/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app#installation-and-sign-in) yükleyip

## <a name="provision-an-instance-of-azure-spring-cloud"></a>Azure yay bulutu örneği sağlama

#### <a name="portal"></a>[Portal](#tab/Azure-portal)

Aşağıdaki yordam Azure portal kullanarak bir Azure Spring Cloud örneği oluşturur.

1. Yeni bir sekmede [Azure Portal](https://ms.portal.azure.com/)açın. 

2. Üst arama kutusundan **Azure yay bulutu**' nı arayın.

3. Sonuçlardan **Azure yay bulutu** ' nı seçin.

    ![ASC simgesi başlangıç](media/spring-cloud-quickstart-launch-app-portal/find-spring-cloud-start.png)

4. Azure yay bulutu sayfasında **+ Ekle**' ye tıklayın.

    ![ASC simgesi ekle](media/spring-cloud-quickstart-launch-app-portal/spring-cloud-add.png)

5. Azure yay bulutu **Oluştur** sayfasında formu doldurun.  Aşağıdaki yönergeleri göz önünde bulundurun:
    - **Abonelik**: Bu kaynak için faturalandırılması istediğiniz aboneliği seçin.
    - **Kaynak grubu**: yeni kaynaklar için yeni kaynak grupları oluşturmak en iyi uygulamadır. Bu, sonraki adımlarda olarak kullanılacak **\<resource group name\>** .
    - **Hizmet Ayrıntıları/adı**: öğesini belirtin **\<service instance name\>** .  Ad 4 ila 32 karakter uzunluğunda olmalı ve yalnızca küçük harf, sayı ve kısa çizgi içermelidir.  Hizmet adının ilk karakteri bir harf olmalıdır ve son karakter bir harf ya da sayı olmalıdır.
    - **Konum**: hizmet örneğinizin konumunu seçin.

    ![ASC portalı başlangıcı](media/spring-cloud-quickstart-launch-app-portal/portal-start.png)

6. **Gözden geçir ve oluştur**’a tıklayın.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-cli-quickstart&step=public-endpoint)

#### <a name="cli"></a>[CLI](#tab/Azure-CLI)

Aşağıdaki yordam Azure Spring Cloud 'ın bir örneğini sağlamak için Azure CLı uzantısını kullanır.

1. Azure CLı 'da oturum açın ve etkin aboneliğinizi seçin. Azure yay bulutu için beyaz listeye eklenen etkin aboneliği seçtiğinizden emin olun

    ```azurecli
    az login
    az account list -o table
    az account set --subscription <Name or ID of subscription, skip if you only have 1 subscription>
    ```

1. Azure yay bulut hizmetiniz için bir ad hazırlayın.  Ad 4 ila 32 karakter uzunluğunda olmalı ve yalnızca küçük harf, sayı ve kısa çizgi içermelidir.  Hizmet adının ilk karakteri bir harf olmalıdır ve son karakter bir harf ya da sayı olmalıdır.

1. Azure yay bulut hizmetinizi içeren bir kaynak grubu oluşturun.

    ```azurecli
    az group create --location eastus --name <resource group name>
    ```

    [Azure Kaynak Grupları](../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinin.

1. Bir Azure CLı penceresi açın ve Azure Spring Cloud 'ın bir örneğini sağlamak için aşağıdaki komutları çalıştırın.

    ```azurecli
    az spring-cloud create -n <service instance name> -g <resource group name>
    ```

    Hizmet örneğinin dağıtılması yaklaşık beş dakika sürer.
---

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Yapılandırma sunucusunu ayarlama](spring-cloud-quickstart-setup-config-server.md)


