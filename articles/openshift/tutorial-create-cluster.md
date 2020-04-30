---
title: Öğretici-Azure Red Hat OpenShift 4 kümesi oluşturma
description: Azure CLı kullanarak Microsoft Azure Red Hat OpenShift kümesi oluşturmayı öğrenin
author: sakthi-vetrivel
ms.author: suvetriv
ms.topic: tutorial
ms.service: container-service
ms.date: 04/24/2020
ms.openlocfilehash: d9b02c11c055b4b072c5f8a1ff47e44001ec4580
ms.sourcegitcommit: eaec2e7482fc05f0cac8597665bfceb94f7e390f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82509729"
---
# <a name="tutorial-create-an-azure-red-hat-openshift-4-cluster"></a>Öğretici: Azure Red Hat OpenShift 4 kümesi oluşturma

Bu öğreticide, üç bölümden biri olmak üzere, ortamınızı OpenShift 4 çalıştıran bir Azure Red Hat Openshıft kümesi oluşturacak ve bir küme oluşturacak şekilde hazırlarsınız. Şunları öğrenirsiniz:
> [!div class="checklist"]
> * Önkoşulları kurun ve gerekli sanal ağı ve alt ağları oluşturun
> * Küme dağıtma

## <a name="before-you-begin"></a>Başlamadan önce

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğreticide, Azure CLı sürüm 2.0.75 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

### <a name="install-the-az-aro-extension"></a>`az aro` Uzantıyı yükler
`az aro` Uzantı, Azure CLI kullanarak doğrudan komut satırından Azure Red Hat OpenShift kümelerini oluşturmanıza, erişimlerinize ve silmesine izin verir.

`az aro` Uzantıyı yüklemek için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az extension add -n aro --index https://az.aroapp.io/stable
```

Zaten uzantı yüklüyse, aşağıdaki komutu çalıştırarak güncelleştirebilirsiniz.

```azurecli-interactive
az extension update -n aro --index https://az.aroapp.io/stable
```

### <a name="register-the-resource-provider"></a>Kaynak sağlayıcısını kaydetme

Daha sonra, `Microsoft.RedHatOpenShift` kaynak sağlayıcısını aboneliğinize kaydetmeniz gerekir.

```azurecli-interactive
az provider register -n Microsoft.RedHatOpenShift --wait
```

Uzantının kayıtlı olduğunu doğrulayın.

```azurecli-interactive
az -v
```

  Aşağıdakine benzer bir çıktı almalısınız.

```output
...
Extensions:
aro                                1.0.0
...
```

### <a name="get-a-red-hat-pull-secret-optional"></a>Red hat çekme gizli anahtarı alma (isteğe bağlı)

Red hat çekme gizli dizisi, kümenizin ek içerikle birlikte Red Hat kapsayıcısı kayıt defterlerine erişmesine olanak sağlar. Bu adım isteğe bağlıdır, ancak önerilir.

Çekme https://cloud.redhat.com/openshift/install/azure/aro-provisioned *gizliliğini indirin*' a gidip ve sonra çekme gizli dizesini edinin.

Red Hat hesabınızda oturum açmanız veya iş e-postanızı kullanarak yeni bir Red Hat hesabı oluşturmanız ve hüküm ve koşulları kabul etmeniz gerekir.

Kaydedilen `pull-secret.txt` dosyayı güvenli bir yerde tutun-bu, her küme oluşturmada kullanılacaktır.

### <a name="create-a-virtual-network-containing-two-empty-subnets"></a>İki boş alt ağ içeren bir sanal ağ oluşturun

Ardından, iki boş alt ağ içeren bir sanal ağ oluşturacaksınız.

1. **Aşağıdaki değişkenleri ayarlayın.**

   ```console
   LOCATION=eastus                 # the location of your cluster
   RESOURCEGROUP=aro-rg            # the name of the resource group where you want to create your cluster
   CLUSTER=cluster                 # the name of your cluster
   ```

1. **Kaynak grubu oluşturma**

    Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir gruptur. Bir kaynak grubu oluştururken konum belirtmeniz istenir. Bu konum, kaynak grubu meta verilerinin depolandığı yerdir, kaynak oluşturma sırasında başka bir bölge belirtmezseniz kaynaklarınızın Azure 'da da çalıştığı yerdir. [Az Group Create] [az-Group-Create] komutunu kullanarak bir kaynak grubu oluşturun.

    ```azurecli-interactive
    az group create --name $RESOURCEGROUP --location $LOCATION
    ```

    Aşağıdaki örnek çıktıda başarıyla oluşturulan kaynak grubu gösterilmektedir:

    ```json
    {
    "id": "/subscriptions/<guid>/resourceGroups/aro-rg",
    "location": "eastus",
    "managedBy": null,
    "name": "aro-rg",
    "properties": {
        "provisioningState": "Succeeded"
    },
    "tags": null
    }
    ```

2. **Bir sanal ağ oluşturun.**

    OpenShift 4 çalıştıran Azure Red Hat OpenShift kümeleri, ana ve çalışan düğümleri için iki boş alt ağa sahip bir sanal ağ gerektirir.

    Daha önce oluşturduğunuz kaynak grubunda yeni bir sanal ağ oluşturun.

    ```azurecli-interactive
    az network vnet create \
    --resource-group $RESOURCEGROUP \
    --name aro-vnet \
    --address-prefixes 10.0.0.0/22
    ```

    Aşağıdaki örnek çıktıda başarıyla oluşturulan sanal ağ gösterilmektedir:

    ```json
    {
    "newVNet": {
        "addressSpace": {
        "addressPrefixes": [
            "10.0.0.0/22"
        ]
        },
        "id": "/subscriptions/<guid>/resourceGroups/aro-rg/providers/Microsoft.Network/virtualNetworks/aro-vnet",
        "location": "eastus",
        "name": "aro-vnet",
        "provisioningState": "Succeeded",
        "resourceGroup": "aro-rg",
        "type": "Microsoft.Network/virtualNetworks"
    }
    }
    ```

3. **Ana düğümler için boş bir alt ağ ekleyin.**

    ```azurecli-interactive
    az network vnet subnet create \
    --resource-group $RESOURCEGROUP \
    --vnet-name aro-vnet \
    --name master-subnet \
    --address-prefixes 10.0.0.0/23 \
    --service-endpoints Microsoft.ContainerRegistry
    ```

4. **Çalışan düğümleri için boş bir alt ağ ekleyin.**

    ```azurecli-interactive
    az network vnet subnet create \
    --resource-group $RESOURCEGROUP \
    --vnet-name aro-vnet \
    --name worker-subnet \
    --address-prefixes 10.0.2.0/23 \
    --service-endpoints Microsoft.ContainerRegistry
    ```

5. **Ana alt ağda [alt ağ özel uç nokta Ilkelerini devre dışı bırakın](https://docs.microsoft.com/azure/private-link/disable-private-link-service-network-policy) .** Bu, kümeye bağlanıp yönetebilmek için gereklidir.

    ```azurecli-interactive
    az network vnet subnet update \
    --name master-subnet \
    --resource-group $RESOURCEGROUP \
    --vnet-name aro-vnet \
    --disable-private-link-service-network-policies true
    ```

## <a name="create-the-cluster"></a>Kümeyi oluşturma

Bir küme oluşturmak için aşağıdaki komutu çalıştırın. İsteğe bağlı olarak, kümenizin Red Hat kapsayıcısı kayıt defterlerine ek içerikle birlikte erişmesine olanak tanıyan bir çekme gizli anahtarı geçirebilirsiniz. [Red Hat OpenShift kümesi Yöneticisi](https://cloud.redhat.com/openshift/install/azure/installer-provisioned) ' ne gidip **çekme gizliliğini Kopyala**' ya tıklayarak çekme gizli dizinizi erişin.

```azurecli-interactive
az aro create \
  --resource-group $RESOURCEGROUP \
  --name $CLUSTER \
  --vnet aro-vnet \
  --master-subnet master-subnet \
  --worker-subnet worker-subnet
  # --domain foo.example.com # [OPTIONAL] custom domain
  # --pull-secret '$(< pull-secret.txt)' # [OPTIONAL]
```
>[!NOTE]
> Genellikle bir küme oluşturmak yaklaşık 35 dakika sürer.

>[!IMPORTANT]
> Özel bir etki alanı belirtmeyi seçerseniz (örneğin, **foo.example.com**) OpenShift konsolu yerleşik etki alanı `https://console-openshift-console.apps.foo.example.com` `https://console-openshift-console.apps.<random>.<location>.aroapp.io`yerine, gibi bir URL 'de kullanılabilir.
>
> Varsayılan olarak, Openshıft, üzerinde `*.apps.<random>.<location>.aroapp.io`oluşturulan tüm yollar için otomatik olarak imzalanan sertifikalar kullanır.  Kümeye bağlandıktan sonra özel DNS kullanmayı seçerseniz, giriş [denetleyiciniz için özel bır CA](https://docs.openshift.com/container-platform/4.3/authentication/certificates/replacing-default-ingress-certificate.html) ve [API sunucunuz IÇIN özel bir CA](https://docs.openshift.com/container-platform/4.3/authentication/certificates/api-server.html)yapılandırmak üzere OpenShift belgelerini izlemeniz gerekir.
>

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]
> * Önkoşulları kurun ve gerekli sanal ağı ve alt ağları oluşturun
> * Küme dağıtma

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift kümesine bağlanma](tutorial-connect-cluster.md)
