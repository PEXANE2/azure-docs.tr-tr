---
title: Azure CLI ile çalışma alanları oluşturma
titleSuffix: Azure Machine Learning
description: Yeni bir Azure Machine Learning çalışma alanı oluşturmak için Azure CLI'yi nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 03/05/2020
ms.openlocfilehash: 9a7d0b75140c50df61ff63f350e5b312a6a684c7
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617772"
---
# <a name="create-a-workspace-for-azure-machine-learning-with-azure-cli"></a>Azure CLI ile Azure Machine Learning için bir çalışma alanı oluşturun
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure CLI'yi kullanarak Azure Machine Learning çalışma alanı oluşturmayı öğrenirsiniz. Azure CLI, Azure kaynaklarını yönetmek için komutlar sağlar. CLI'ye yönelik makine öğrenimi uzantısı, Azure Machine Learning kaynaklarıyla çalışmak için komutlar sağlar.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği.** Yoksa, [Azure Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

* Bu belgedeki CLI komutlarını **yerel ortamınızdan**kullanmak için [Azure CLI'ye](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)ihtiyacınız vardır.

    Azure Bulut [Kabuğu'nu](https://azure.microsoft.com//features/cloud-shell/)kullanıyorsanız, CLI'ye tarayıcı üzerinden erişilir ve bulutta yaşar.

## <a name="connect-the-cli-to-your-azure-subscription"></a>CLI'yi Azure aboneliğinize bağlayın

> [!IMPORTANT]
> Azure Bulut Kabuğu'nu kullanıyorsanız, bu bölümü atlayabilirsiniz. Bulut kabuğu, Azure aboneliğinizde oturum açtığınız hesabı kullanarak kimliğinizi otomatik olarak doğrular.

Azure aboneliğinizi CLI'den doğrulamanın birkaç yolu vardır. En temel içinde bir tarayıcı kullanarak interaktif olarak kimlik sağlamaktır. Etkileşimli olarak kimlik doğrulaması yapmak için bir komut satırı veya terminal açın ve aşağıdaki komutu kullanın:

```azurecli-interactive
az login
```

CLI varsayılan tarayıcınızı açabiliyorsa, tarayıcıyı açar ve oturum açma sayfasını yükler. Aksi takdirde, bir tarayıcı açmanız ve komut satırındaki yönergeleri izlemeniz gerekir. Yönergeler, yetkilendirme [https://aka.ms/devicelogin](https://aka.ms/devicelogin) koduna göz atma ve girmeyi içerir.

[!INCLUDE [select-subscription](../../includes/machine-learning-cli-subscription.md)] 

Kimlik doğrulamanın diğer yöntemleri için [Azure CLI ile Oturum Aç'a](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest)bakın.

## <a name="install-the-machine-learning-extension"></a>Makine öğrenimi uzantısını yükleyin

Makine öğrenimi uzantısını yüklemek için aşağıdaki komutu kullanın:

```azurecli-interactive
az extension add -n azure-cli-ml
```

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Azure Machine Learning çalışma alanı aşağıdaki Azure hizmetlerine veya varlıklarına dayanır:

> [!IMPORTANT]
> Varolan bir Azure hizmetini belirtmezseniz, çalışma alanı oluşturma sırasında otomatik olarak oluşturulur. Her zaman bir kaynak grubu belirtmeniz gerekir.

| Hizmet | Varolan bir örneği belirtmek için parametre |
| ---- | ---- |
| **Azure kaynak grubu** | `-g <resource-group-name>`
| **Azure Depolama Hesabı** | `--storage-account <service-id>` |
| **Azure Application Insights** | `--application-insights <service-id>` |
| **Azure Key Vault** | `--keyvault <service-id>` |
| **Azure Container Registry** | `--container-registry <service-id>` |

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure Machine Learning çalışma alanı bir kaynak grubu içinde oluşturulmalıdır. Varolan bir kaynak grubunu kullanabilir veya yeni bir kaynak oluşturabilirsiniz. __Yeni bir kaynak grubu oluşturmak__için aşağıdaki komutu kullanın. Bu `<resource-group-name>` kaynak grubu için kullanılacak adla değiştirin. Bu `<location>` kaynak grubu için kullanılacak Azure bölgesiyle değiştirin:

> [!TIP]
> Azure Machine Learning'in kullanılabildiği bir bölge seçmeniz gerekir. Daha fazla bilgi [için, bölgeye göre mevcut Ürünler'e](https://azure.microsoft.com/global-infrastructure/services/?products=machine-learning-service)bakın.

```azurecli-interactive
az group create --name <resource-group-name> --location <location>
```

Bu komutun yanıtı aşağıdaki JSON'a benzer:

```json
{
  "id": "/subscriptions/<subscription-GUID>/resourceGroups/<resourcegroupname>",
  "location": "<location>",
  "managedBy": null,
  "name": "<resource-group-name>",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": null
}
```

Kaynak gruplarıyla çalışma hakkında daha fazla bilgi için [az grubuna](https://docs.microsoft.com//cli/azure/group?view=azure-cli-latest)bakın.

### <a name="automatically-create-required-resources"></a>Otomatik olarak gerekli kaynakları oluşturun

__Hizmetlerin otomatik olarak oluşturulduğu__yeni bir çalışma alanı oluşturmak için aşağıdaki komutu kullanın:

> [!TIP]
> Bu bölümdeki komutlar temel bir sürüm çalışma alanı oluşturur. Kurumsal çalışma alanı oluşturmak için `--sku enterprise` `az ml workspace create` komutla anahtarı kullanın. Azure Machine Learning sürümleri hakkında daha fazla bilgi için Azure [Machine Learning nedir'e](overview-what-is-azure-ml.md#sku)bakın.

```azurecli-interactive
az ml workspace create -w <workspace-name> -g <resource-group-name>
```

> [!NOTE]
> Çalışma alanı adı büyük/küçük harf duyarsız.

Bu komutun çıktısı aşağıdaki JSON'a benzer:

```json
{
  "applicationInsights": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<application-insight-name>",
  "containerRegistry": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.containerregistry/registries/<acr-name>",
  "creationTime": "2019-08-30T20:24:19.6984254+00:00",
  "description": "",
  "friendlyName": "<workspace-name>",
  "id": "/subscriptions/<service-GUID>/resourceGroups/<resource-group-name>/providers/Microsoft.MachineLearningServices/workspaces/<workspace-name>",
  "identityPrincipalId": "<GUID>",
  "identityTenantId": "<GUID>",
  "identityType": "SystemAssigned",
  "keyVault": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.keyvault/vaults/<key-vault-name>",
  "location": "<location>",
  "name": "<workspace-name>",
  "resourceGroup": "<resource-group-name>",
  "storageAccount": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.storage/storageaccounts/<storage-account-name>",
  "type": "Microsoft.MachineLearningServices/workspaces",
  "workspaceid": "<GUID>"
}
```

### <a name="use-existing-resources"></a>Varolan kaynakları kullanma

Varolan kaynakları kullanan bir çalışma alanı oluşturmak için, kaynakların kimliğini sağlamanız gerekir. Hizmetlerin kimliğini almak için aşağıdaki komutları kullanın:

> [!IMPORTANT]
> Varolan tüm kaynakları belirtmeniz gerekmez. Bir veya daha fazla belirtebilirsiniz. Örneğin, varolan bir depolama hesabı belirtebilirsiniz ve çalışma alanı diğer kaynakları oluşturur.

+ **Azure Depolama Hesabı**:`az storage account show --name <storage-account-name> --query "id"`

    Bu komutun yanıtı aşağıdaki metne benzer ve depolama hesabınıziçin kimliktir:

    `"/subscriptions/<service-GUID>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-account-name>"`

+ **Azure Uygulama Öngörüleri**:

    1. Uygulama öngörüleri uzantısını yükleyin:

        ```azurecli-interactive
        az extension add -n application-insights
        ```

    2. Uygulama öngörü hizmetinin kimliğini alın:

        ```azurecli-interactive
        az monitor app-insights component show --app <application-insight-name> -g <resource-group-name> --query "id"
        ```

        Bu komutun yanıtı aşağıdaki metne benzer ve uygulama öngörüleri hizmetinizin kimliğidir:

        `"/subscriptions/<service-GUID>/resourceGroups/<resource-group-name>/providers/microsoft.insights/components/<application-insight-name>"`

+ **Azure Anahtar Kasası**:`az keyvault show --name <key-vault-name> --query "ID"`

    Bu komutun yanıtı aşağıdaki metne benzer ve anahtar kasanızın kimliğidir:

    `"/subscriptions/<service-GUID>/resourceGroups/<resource-group-name>/providers/Microsoft.KeyVault/vaults/<key-vault-name>"`

+ **Azure Konteyner Kayıt Defteri**:`az acr show --name <acr-name> -g <resource-group-name> --query "id"`

    Bu komutun yanıtı aşağıdaki metne benzer ve kapsayıcı kayıt defterinin kimliğidir:

    `"/subscriptions/<service-GUID>/resourceGroups/<resource-group-name>/providers/Microsoft.ContainerRegistry/registries/<acr-name>"`

    > [!IMPORTANT]
    > Azure Machine Learning çalışma alanıyla kullanılabilmesi için konteyner kayıt defterinin [yönetici hesabının](/azure/container-registry/container-registry-authentication#admin-account) etkinleştirilmiş olması gerekir.

Çalışma alanıyla kullanmak istediğiniz kaynak(lar) için kimlikler aldıktan sonra, temel `az workspace create -w <workspace-name> -g <resource-group-name>` komutu kullanın ve varolan kaynaklar için parametre(ler) ve ID(lar) ekleyin. Örneğin, aşağıdaki komut varolan bir kapsayıcı kayıt defteri kullanan bir çalışma alanı oluşturur:

```azurecli-interactive
az ml workspace create -w <workspace-name> -g <resource-group-name> --container-registry "/subscriptions/<service-GUID>/resourceGroups/<resource-group-name>/providers/Microsoft.ContainerRegistry/registries/<acr-name>"
```

Bu komutun çıktısı aşağıdaki JSON'a benzer:

```json
{
  "applicationInsights": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<application-insight-name>",
  "containerRegistry": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.containerregistry/registries/<acr-name>",
  "creationTime": "2019-08-30T20:24:19.6984254+00:00",
  "description": "",
  "friendlyName": "<workspace-name>",
  "id": "/subscriptions/<service-GUID>/resourceGroups/<resource-group-name>/providers/Microsoft.MachineLearningServices/workspaces/<workspace-name>",
  "identityPrincipalId": "<GUID>",
  "identityTenantId": "<GUID>",
  "identityType": "SystemAssigned",
  "keyVault": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.keyvault/vaults/<key-vault-name>",
  "location": "<location>",
  "name": "<workspace-name>",
  "resourceGroup": "<resource-group-name>",
  "storageAccount": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.storage/storageaccounts/<storage-account-name>",
  "type": "Microsoft.MachineLearningServices/workspaces",
  "workspaceid": "<GUID>"
}
```

## <a name="list-workspaces"></a>Çalışma alanlarını listele

Azure aboneliğiniz için tüm çalışma alanlarını listelemek için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml workspace list
```

Bu komutun çıktısı aşağıdaki JSON'a benzer:

```json
[
  {
    "resourceGroup": "myresourcegroup",
    "subscriptionId": "<subscription-id>",
    "workspaceName": "myml"
  },
  {
    "resourceGroup": "anotherresourcegroup",
    "subscriptionId": "<subscription-id>",
    "workspaceName": "anotherml"
  }
]
```

Daha fazla bilgi için [az ml çalışma alanı listesi](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/workspace?view=azure-cli-latest#ext-azure-cli-ml-az-ml-workspace-list) belgelerine bakın.

## <a name="get-workspace-information"></a>Çalışma alanı bilgileri alma

Çalışma alanı hakkında bilgi almak için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml workspace show -w <workspace-name> -g <resource-group-name>
```

Bu komutun çıktısı aşağıdaki JSON'a benzer:

```json
{
  "applicationInsights": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/application-insight-name>",
  "creationTime": "2019-08-30T18:55:03.1807976+00:00",
  "description": "",
  "friendlyName": "",
  "id": "/subscriptions/<service-GUID>/resourceGroups/<resource-group-name>/providers/Microsoft.MachineLearningServices/workspaces/<workspace-name>",
  "identityPrincipalId": "<GUID>",
  "identityTenantId": "<GUID>",
  "identityType": "SystemAssigned",
  "keyVault": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.keyvault/vaults/<key-vault-name>",
  "location": "<location>",
  "name": "<workspace-name>",
  "resourceGroup": "<resource-group-name>",
  "storageAccount": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.storage/storageaccounts/<storage-account-name>",
  "tags": {},
  "type": "Microsoft.MachineLearningServices/workspaces",
  "workspaceid": "<GUID>"
}
```

Daha fazla bilgi için [az ml çalışma alanı belgeleri göster'e](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/workspace?view=azure-cli-latest#ext-azure-cli-ml-az-ml-workspace-show) bakın.

## <a name="update-a-workspace"></a>Çalışma alanını güncelleştirme

Çalışma alanını güncelleştirmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml workspace update -w <workspace-name> -g <resource-group-name>
```

Bu komutun çıktısı aşağıdaki JSON'a benzer:

```json
{
  "applicationInsights": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/application-insight-name>",
  "creationTime": "2019-08-30T18:55:03.1807976+00:00",
  "description": "",
  "friendlyName": "",
  "id": "/subscriptions/<service-GUID>/resourceGroups/<resource-group-name>/providers/Microsoft.MachineLearningServices/workspaces/<workspace-name>",
  "identityPrincipalId": "<GUID>",
  "identityTenantId": "<GUID>",
  "identityType": "SystemAssigned",
  "keyVault": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.keyvault/vaults/<key-vault-name>",
  "location": "<location>",
  "name": "<workspace-name>",
  "resourceGroup": "<resource-group-name>",
  "storageAccount": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.storage/storageaccounts/<storage-account-name>",
  "tags": {},
  "type": "Microsoft.MachineLearningServices/workspaces",
  "workspaceid": "<GUID>"
}
```

Daha fazla bilgi için [az ml çalışma alanı güncelleştirme](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/workspace?view=azure-cli-latest#ext-azure-cli-ml-az-ml-workspace-update) belgelerine bakın.

## <a name="share-a-workspace-with-another-user"></a>Çalışma alanını başka bir kullanıcıyla paylaşma

Aboneliğinizde bir çalışma alanını başka bir kullanıcıyla paylaşmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml workspace share -w <workspace-name> -g <resource-group-name> --user <user> --role <role>
```

Azure Machine Learning ile rollere dayalı erişim denetimi (RBAC) hakkında daha fazla bilgi için [kullanıcıları ve rolleri yönet'e](how-to-assign-roles.md)bakın.

Daha fazla bilgi için [az ml çalışma alanı paylaşımı](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/workspace?view=azure-cli-latest#ext-azure-cli-ml-az-ml-workspace-share) belgelerine bakın.

## <a name="sync-keys-for-dependent-resources"></a>Bağımlı kaynaklar için eşitleme anahtarları

Çalışma alanınız tarafından kullanılan kaynaklardan birinin erişim anahtarlarını değiştirirseniz, yeni anahtarları çalışma alanıyla eşitlemek için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml workspace sync-keys -w <workspace-name> -g <resource-group-name>
```

Anahtarları değiştirme hakkında daha fazla bilgi için [bkz.](how-to-change-storage-access-key.md)

Daha fazla bilgi için [az ml çalışma alanı eşitleme anahtarları](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/workspace?view=azure-cli-latest#ext-azure-cli-ml-az-ml-workspace-sync-keys) belgelerine bakın.

## <a name="delete-a-workspace"></a>Çalışma alanını silme

Artık gerekkalmadan sonra çalışma alanını silmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml workspace delete -w <workspace-name> -g <resource-group-name>
```

> [!IMPORTANT]
> Çalışma alanını silmek, çalışma alanı tarafından kullanılan uygulama öngörüsünü, depolama hesabını, anahtar kasasını veya kapsayıcı kayıt defterini silmez.

Ayrıca, çalışma alanını ve kaynak grubundaki diğer tüm Azure kaynaklarını silen kaynak grubunu da silebilirsiniz. Kaynak grubunu silmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az group delete -g <resource-group-name>
```

Daha fazla bilgi için [az ml çalışma alanı silme](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/workspace?view=azure-cli-latest#ext-azure-cli-ml-az-ml-workspace-delete) belgelerine bakın.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="resource-provider-errors"></a>Kaynak sağlayıcı hataları

[!INCLUDE [machine-learning-resource-provider](../../includes/machine-learning-resource-provider.md)]

### <a name="moving-the-workspace"></a>Çalışma alanını taşıma

> [!WARNING]
> Azure Machine Learning çalışma alanınızı farklı bir aboneye taşımak veya sahip olan aboneliğiyeni bir kiracıya taşımak desteklenmez. Bunu yapmak hatalara neden olabilir.

### <a name="deleting-the-azure-container-registry"></a>Azure Kapsayıcı Kayıt Defterini Silme

Azure Machine Learning çalışma alanı, bazı işlemler için Azure Kapsayıcı Kayıt Defteri'ni (ACR) kullanır. İlk ihtiyacı olduğunda otomatik olarak bir ACR örneği oluşturur.

[!INCLUDE [machine-learning-delete-acr](../../includes/machine-learning-delete-acr.md)]

## <a name="next-steps"></a>Sonraki adımlar

Makine öğrenimi için Azure CLI uzantısı hakkında daha fazla bilgi için [az ml](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml?view=azure-cli-latest) belgelerine bakın.
