---
title: Hızlı başlangıç-Azure yay bulut hizmetini sağlama
description: Uygulama dağıtımı için Azure Spring Cloud Service örneğinin oluşturulmasını açıklar.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 09/08/2020
ms.custom: devx-track-java
zone_pivot_groups: programming-languages-spring-cloud
ms.openlocfilehash: 16d40c334d51a66df4a4d2d56e2fa2379dda3726
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90905413"
---
# <a name="quickstart-provision-azure-spring-cloud-service"></a>Hızlı başlangıç: Azure yay bulut hizmetini sağlama

::: zone pivot="programming-language-csharp"
Bu hızlı başlangıçta Azure yay bulut hizmeti 'nin bir örneğini sağlamak için Azure CLı 'yi kullanırsınız.

## <a name="prerequisites"></a>Önkoşullar

* Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [.NET Core 3,1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1). Azure yay bulut hizmeti, .NET Core 3,1 ve sonraki sürümlerini destekler.
* [Azure CLI sürüm 2.0.67 veya üzeri](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true).
* [Git](https://git-scm.com/).

## <a name="install-azure-cli-extension"></a>Azure CLI uzantısını yükleme

Azure CLı sürümünüzün 2.0.67 veya üzeri olduğunu doğrulayın:

```azurecli
az --version
```

Aşağıdaki komutu kullanarak Azure CLı için Azure yay bulutu uzantısını yükler:

```azurecli
az extension add --name spring-cloud
```

## <a name="log-in-to-azure"></a>Azure'da oturum açma

1. Azure CLı 'da oturum açın.

    ```azurecli
    az login
    ```

1. Birden fazla aboneliğiniz varsa, bu hızlı başlangıç için kullanmak istediğiniz birini seçin.

   ```azurecli
   az account list -o table
   ```

   ```azurecli
   az account set --subscription <Name or ID of a subscription from the last step>
   ```

## <a name="provision-an-instance-of-azure-spring-cloud"></a>Azure yay bulutu örneği sağlama

1. Azure yay bulut hizmetinizi içeren bir [kaynak grubu](../azure-resource-manager/management/overview.md) oluşturun. Kaynak grubu adı alfasayısal, alt çizgi, parantezler, kısa çizgi, nokta (bitiş dışındakiler) ve Unicode karakterler içerebilir.

   ```azurecli
   az group create --location eastus --name <resource group name>
   ```

1. Azure Spring Cloud Service 'in bir örneğini sağlayın. Hizmet örneği adı 4 ila 32 karakter uzunluğunda olmalı ve yalnızca küçük harf, sayı ve kısa çizgi içermelidir. Hizmet adının ilk karakteri bir harf olmalıdır ve son karakter bir harf ya da sayı olmalıdır.

    ```azurecli
    az spring-cloud create -n <service instance name> -g <resource group name>
    ```

    Bu komutun tamamlanması birkaç dakika sürebilir.

1. Aşağıdaki komutlarda bu değerleri tekrar tekrar belirtmeniz gerekmiyorsa, varsayılan kaynak grubu adınızı ve hizmet örneği adınızı ayarlayın.

   ```azurecli
   az configure --defaults group=<resource group name>
   ```

   ```azurecli
   az configure --defaults spring-cloud=<service instance name>
   ```
::: zone-end

::: zone pivot="programming-language-java"
Azure portal veya Azure CLı kullanarak Azure Spring Cloud örneğini oluşturabilirsiniz.  Her iki yöntem de aşağıdaki yordamlarda açıklanmıştır.
## <a name="prerequisites"></a>Önkoşullar

* [JDK 8 ' i yükler](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable&preserve-view=true)
* [Azure aboneliğine kaydolma](https://azure.microsoft.com/free/)
* Seçim [Azure CLI sürüm 2.0.67 veya üstünü yükleyip](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) Azure Spring Cloud uzantısını şu komutla birlikte yüklersiniz: `az extension add --name spring-cloud`
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

1. Azure CLı 'da oturum açın ve etkin aboneliğinizi seçin.

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
::: zone-end

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu serideki bir sonraki hızlı başlangıca devam etmek istiyorsanız, bu adımı atlayın.

Bu hızlı başlangıçta, abonelikleriniz varsa ücretlendirmeye devam edecek Azure kaynakları oluşturdunuz. Sonraki hızlı başlangıca devam etmeyi düşünmüyorsanız ve gelecekte bu kaynaklara ihtiyaç duymazsanız, portalı kullanarak veya Cloud Shell aşağıdaki komutu çalıştırarak kaynak grubunu silin:

```azurecli
az group delete --name <your resource group name; for example: helloworld-1558400876966-rg> --yes
```

Bu hızlı başlangıçta, varsayılan kaynak grubu adını da ayarlarsınız. Sonraki hızlı başlangıca devam etmeyi planlamıyorsanız, aşağıdaki CLı komutunu çalıştırarak bu varsayılanı kaldırın:

```azurecli
az configure --defaults group=
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yapılandırma sunucusu ayarlama](spring-cloud-quickstart-setup-config-server.md)
