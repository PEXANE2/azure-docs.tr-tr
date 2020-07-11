---
title: Öğretici-Azure Red Hat OpenShift 4 kümesi oluşturma
description: Azure CLı kullanarak Microsoft Azure Red Hat OpenShift kümesi oluşturmayı öğrenin
author: sakthi-vetrivel
ms.author: suvetriv
ms.topic: tutorial
ms.service: container-service
ms.date: 04/24/2020
ms.openlocfilehash: b78364cef6bfd6cf91e6edf81fd57fa5912125db
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86260675"
---
# <a name="tutorial-create-an-azure-red-hat-openshift-4-cluster"></a>Öğretici: Azure Red Hat OpenShift 4 kümesi oluşturma

Bu öğreticide, üç bölümden biri olmak üzere, ortamınızı OpenShift 4 çalıştıran bir Azure Red Hat Openshıft kümesi oluşturacak ve bir küme oluşturacak şekilde hazırlarsınız. Şunları öğrenirsiniz:
> [!div class="checklist"]
> * Önkoşulları kurun ve gerekli sanal ağı ve alt ağları oluşturun
> * Küme dağıtma

## <a name="before-you-begin"></a>Başlamadan önce

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğreticide, Azure CLı sürüm 2.0.75 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

### <a name="verify-your-permissions"></a>İzinlerinizi doğrulama

Azure Red Hat OpenShift kümesi oluşturmak için Azure aboneliğinizde, Azure Active Directory Kullanıcı veya hizmet sorumlusu 'nda aşağıdaki izinleri doğrulayın:

|İzinler|VNet 'i içeren kaynak grubu|Kullanıcı yürütülüyor`az aro create`|Hizmet sorumlusu olarak geçildi`–client-id`|
|----|:----:|:----:|:----:|
|**Kullanıcı Erişimi Yöneticisi**|X|X| |
|**Katkıda Bulunan**|X|X|X|

### <a name="install-the-az-aro-extension"></a>Uzantıyı yükler `az aro`
Uzantı, Azure `az aro` CLI kullanarak doğrudan komut satırından Azure Red Hat OpenShift kümelerini oluşturmanıza, erişimlerinize ve silmesine izin verir.

Uzantıyı yüklemek için aşağıdaki komutu çalıştırın `az aro` .

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

1. **[Red Hat Openshıft Küme Yöneticisi portalınıza gidin](https://cloud.redhat.com/openshift/install/azure/aro-provisioned) ve oturum açın.**

   Red Hat hesabınızda oturum açmanız veya iş e-postanızı kullanarak yeni bir Red Hat hesabı oluşturmanız ve hüküm ve koşulları kabul etmeniz gerekir.

2. **Çekme gizliliğini Indir ' e tıklayın.**

Kaydedilen `pull-secret.txt` dosyayı güvenli bir yerde tutun-bu, her küme oluşturmada kullanılacaktır.

`az aro create`Komutunu çalıştırırken, parametresini kullanarak çekme gizli dizinizi başvurabilirsiniz `--pull-secret @pull-secret.txt` . `az aro create`Dosyanızı depoladığınız dizinden yürütün `pull-secret.txt` . Aksi takdirde, `@pull-secret.txt` ile değiştirin `@<path-to-my-pull-secret-file>` .

Çekme sırlarınızı kopyalıyorsunuz veya başka betiklerin içine başvuruyorsa, çekme gizli anahtarı geçerli bir JSON dizesi olarak biçimlendirilmelidir.

### <a name="prepare-a-custom-domain-for-your-cluster-optional"></a>Kümeniz için özel bir etki alanı hazırlama (isteğe bağlı)

`az aro create`Komutunu çalıştırırken, parametresini kullanarak kümeniz için özel bir etki alanı belirtebilirsiniz `--domain foo.example.com` .

Kümeniz için özel bir etki alanı sağlarsanız aşağıdaki noktaları dikkate alın:

* Kümenizi oluşturduktan sonra, DNS sunucunuzda belirtilen 2 DNS A kayıt oluşturmanız gerekir `--domain` :
    * **API-API** sunucusuna işaret ediyor
    * ** \* . Apps** -girişi gösterme
    * Şu komutu yürüterek bu değerleri alın: `az aro show -n -g --query '{api:apiserverProfile.ip, ingress:ingressProfiles[0].ip}'` .

* OpenShift konsolu, `https://console-openshift-console.apps.foo.example.com` yerleşik etki alanı yerine, gibi BIR URL 'de kullanılabilir olacaktır `https://console-openshift-console.apps.<random>.<location>.aroapp.io` .

* Varsayılan olarak, Openshıft, üzerinde oluşturulan tüm yollar için otomatik olarak imzalanan sertifikalar kullanır `*.apps.<random>.<location>.aroapp.io` .  Kümeye bağlandıktan sonra özel DNS kullanmayı seçerseniz, giriş [denetleyiciniz için özel bır CA](https://docs.openshift.com/container-platform/4.3/authentication/certificates/replacing-default-ingress-certificate.html) ve [API sunucunuz IÇIN özel bir CA](https://docs.openshift.com/container-platform/4.3/authentication/certificates/api-server.html)yapılandırmak üzere OpenShift belgelerini izlemeniz gerekir.

### <a name="create-a-virtual-network-containing-two-empty-subnets"></a>İki boş alt ağ içeren bir sanal ağ oluşturun

Ardından, iki boş alt ağ içeren bir sanal ağ oluşturacaksınız.

1. **Komut yürütmek istediğiniz kabuk ortamında aşağıdaki değişkenleri ayarlayın `az` .**

   ```console
   LOCATION=eastus                 # the location of your cluster
   RESOURCEGROUP=aro-rg            # the name of the resource group where you want to create your cluster
   CLUSTER=cluster                 # the name of your cluster
   ```

1. **Bir kaynak grubu oluşturun.**

    Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir gruptur. Bir kaynak grubu oluştururken konum belirtmeniz istenir. Bu konum, kaynak grubu meta verilerinin depolandığı yerdir, kaynak oluşturma sırasında başka bir bölge belirtmezseniz kaynaklarınızın Azure 'da da çalıştığı yerdir. [Az Group Create](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create) komutunu kullanarak bir kaynak grubu oluşturun.

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

    Daha önce oluşturduğunuz kaynak grubunda yeni bir sanal ağ oluşturun:

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

Bir küme oluşturmak için aşağıdaki komutu çalıştırın. Aşağıdaki seçeneklerden birini kullanmayı seçerseniz, komutu uygun şekilde değiştirin:
* İsteğe bağlı olarak, kümenizin Red Hat kapsayıcısı kayıt defterlerine ek içerikle birlikte erişmesine olanak tanıyan [Red hat çekme gizli anahtarını geçirebilirsiniz](#get-a-red-hat-pull-secret-optional) . `--pull-secret @pull-secret.txt`Komutunuz için bağımsız değişkenini ekleyin.
* İsteğe bağlı olarak, [özel bir etki alanı kullanabilirsiniz](#prepare-a-custom-domain-for-your-cluster-optional). `--domain foo.example.com` `foo.example.com` Kendi özel etki alanınız ile değiştirerek, Komutunuz için bağımsız değişkeni ekleyin.

> [!NOTE]
> Komutunuz için herhangi bir isteğe bağlı bağımsız değişken ekliyorsanız, sonundaki ters eğik çizgiyle komutun önceki satırındaki bağımsız değişkenini kapatmayı unutmayın.

```azurecli-interactive
az aro create \
  --resource-group $RESOURCEGROUP \
  --name $CLUSTER \
  --vnet aro-vnet \
  --master-subnet master-subnet \
  --worker-subnet worker-subnet
```

Komutu yürüttükten sonra `az aro create` , normalde bir küme oluşturmak yaklaşık 35 dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]
> * Önkoşulları ayarlama ve gerekli sanal ağı ve alt ağları oluşturma
> * Küme dağıtma

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift kümesine bağlanma](tutorial-connect-cluster.md)
