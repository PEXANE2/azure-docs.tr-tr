---
title: Azure Red Hat OpenShift 4 özel kümesi oluşturma
description: OpenShift 4 çalıştıran bir Azure Red Hat OpenShift özel kümesi oluşturmayı öğrenin
ms.service: container-service
ms.topic: article
ms.date: 03/12/2020
author: ms-jasondel
ms.author: jasondel
keywords: Aro, OpenShift, az Aro, Red hat, CLI
ms.custom: mvc
ms.openlocfilehash: 581587382c3bfd03ed329672e5c6ca065554d1c7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83727644"
---
# <a name="create-an-azure-red-hat-openshift-4-private-cluster"></a>Azure Red Hat OpenShift 4 özel kümesi oluşturma

Bu makalede, ortamınızı OpenShift 4 çalıştıran Azure Red Hat OpenShift özel kümeleri oluşturacak şekilde hazırlarsınız. Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Önkoşulları kurun ve gerekli sanal ağı ve alt ağları oluşturun
> * Özel API sunucusu uç noktası ve özel giriş denetleyicisi ile küme dağıtma

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğreticide, Azure CLı sürüm 2.0.75 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="before-you-begin"></a>Başlamadan önce

### <a name="install-the-az-aro-extension"></a>' Az Aro ' uzantısını yükler
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

1. **[Red Hat Openshıft Cluster Manager portala gidin](https://cloud.redhat.com/openshift/install/azure/aro-provisioned) ve oturum açın.**

   Red Hat hesabınızda oturum açmanız veya iş e-postanızı kullanarak yeni bir Red Hat hesabı oluşturmanız ve hüküm ve koşulları kabul etmeniz gerekir.

2. **Çekme gizliliğini Indir ' e tıklayın.**

Kaydedilen `pull-secret.txt` dosyayı güvenli bir yerde tutun-bu, her küme oluşturmada kullanılacaktır.

`az aro create`Komutunu çalıştırırken, parametresini kullanarak çekme gizli dizinizi başvurabilirsiniz `--pull-secret @pull-secret.txt` . `az aro create`Dosyanızı depoladığınız dizinden yürütün `pull-secret.txt` . Aksi takdirde, `@pull-secret.txt` ile değiştirin `@<path-to-my-pull-secret-file` .

Çekme sırlarınızı kopyalıyorsunuz veya başka betiklerin içine başvuruyorsa, çekme gizli anahtarı geçerli bir JSON dizesi olarak biçimlendirilmelidir.

### <a name="create-a-virtual-network-containing-two-empty-subnets"></a>İki boş alt ağ içeren bir sanal ağ oluşturun

Ardından, iki boş alt ağ içeren bir sanal ağ oluşturacaksınız.

1. **Aşağıdaki değişkenleri ayarlayın.**

   ```console
   LOCATION=eastus                 # the location of your cluster
   RESOURCEGROUP="v4-$LOCATION"    # the name of the resource group where you want to create your cluster
   CLUSTER=aro-cluster             # the name of your cluster
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

Bir küme oluşturmak için aşağıdaki komutu çalıştırın. İsteğe bağlı olarak, kümenizin Red Hat kapsayıcısı kayıt defterlerine ek içerikle birlikte erişmesine olanak tanıyan [Red hat çekme gizli anahtarını geçirebilirsiniz](#get-a-red-hat-pull-secret-optional) .

>[!NOTE]
> Komutları kopyalayıp yapıştırıyorsanız ve isteğe bağlı parametrelerden birini kullanıyorsanız, ilk diyez etiketlerini ve sondaki açıklama metnini silmeyi unutmayın. Ayrıca, sonundaki ters eğik çizgiyle komutun önceki satırındaki bağımsız değişkenini kapatın.

```azurecli-interactive
az aro create \
  --resource-group $RESOURCEGROUP \
  --name $CLUSTER \
  --vnet aro-vnet \
  --master-subnet master-subnet \
  --worker-subnet worker-subnet \
  --apiserver-visibility Private \
  --ingress-visibility Private
  # --domain foo.example.com # [OPTIONAL] custom domain
  # --pull-secret @pull-secret.txt # [OPTIONAL]
```

Komutu yürüttükten sonra `az aro create` , normalde bir küme oluşturmak yaklaşık 35 dakika sürer.

>[!IMPORTANT]
> Özel bir etki alanı belirtmeyi seçerseniz (örneğin, **foo.example.com**) OpenShift Konsolu `https://console-openshift-console.apps.foo.example.com` yerleşik etki alanı yerine, gibi bir URL 'de kullanılabilir `https://console-openshift-console.apps.<random>.<location>.aroapp.io` .
>
> Varsayılan olarak, Openshıft, üzerinde oluşturulan tüm yollar için otomatik olarak imzalanan sertifikalar kullanır `*.apps.<random>.<location>.aroapp.io` .  Özel DNS ' i seçerseniz, kümeye bağlandıktan sonra, giriş denetleyicinizin ve [API sunucunuz için özel CA](https://docs.openshift.com/container-platform/4.3/authentication/certificates/api-server.html) [IÇIN özel bir CA yapılandırmak](https://docs.openshift.com/container-platform/4.3/authentication/certificates/replacing-default-ingress-certificate.html) üzere OpenShift belgelerini izlemeniz gerekir.

## <a name="connect-to-the-private-cluster"></a>Özel kümeye Bağlan

Kullanıcıyı kullanarak kümede oturum açabilirsiniz `kubeadmin` .  Kullanıcı parolasını bulmak için aşağıdaki komutu çalıştırın `kubeadmin` .

```azurecli-interactive
az aro list-credentials \
  --name $CLUSTER \
  --resource-group $RESOURCEGROUP
```

Aşağıdaki örnek çıktı, parolasının içinde olacağını gösterir `kubeadminPassword` .

```json
{
  "kubeadminPassword": "<generated password>",
  "kubeadminUsername": "kubeadmin"
}
```

Aşağıdaki komutu çalıştırarak küme konsolu URL 'sini bulabilirsiniz, şöyle görünür`https://console-openshift-console.apps.<random>.<region>.aroapp.io/`

```azurecli-interactive
 az aro show \
    --name $CLUSTER \
    --resource-group $RESOURCEGROUP \
    --query "consoleProfile.url" -o tsv
```

>[!IMPORTANT]
> Özel bir Azure Red Hat OpenShift kümesine bağlanmak için, oluşturduğunuz sanal ağda veya kümenin dağıtıldığı sanal ağla eşlenmiş bir sanal ağda bulunan bir ana bilgisayardan aşağıdaki adımı gerçekleştirmeniz gerekir. [peered](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)

Konsol URL 'sini bir tarayıcıda başlatın ve kimlik bilgilerini kullanarak oturum açın `kubeadmin` .

![Azure Red Hat OpenShift oturum açma ekranı](media/aro4-login.png)

## <a name="install-the-openshift-cli"></a>OpenShift CLı 'yı yükler

OpenShift Web konsolunda oturum **açtıktan sonra, üzerine tıklayın.** sağ üst köşedeki ve ardından **komut satırı araçlarında**. Makinenize uygun yayını indirin.

![Azure Red Hat OpenShift oturum açma ekranı](media/aro4-download-cli.png)

Ayrıca, makinenizde makinenize uygun olan CLı 'nın en son sürümünü yükleyebilirsiniz <https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/> .

## <a name="connect-using-the-openshift-cli"></a>OpenShift CLı kullanarak bağlanma

API sunucusunun adresini alın.

```azurecli-interactive
apiServer=$(az aro show -g $RESOURCEGROUP -n $CLUSTER --query apiserverProfile.url -o tsv)
```

>[!IMPORTANT]
> Özel bir Azure Red Hat OpenShift kümesine bağlanmak için, oluşturduğunuz sanal ağda veya kümenin dağıtıldığı sanal ağla eşlenmiş bir sanal ağda bulunan bir ana bilgisayardan aşağıdaki adımı gerçekleştirmeniz gerekir. [peered](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)

Aşağıdaki komutu kullanarak OpenShift kümesinin API sunucusunda oturum açın. **\<kubeadmin password>** Yeni aldığınız parolayla değiştirin.

```azurecli-interactive
oc login $apiServer -u kubeadmin -p <kubeadmin password>
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, OpenShift 4 çalıştıran bir Azure Red Hat OpenShift kümesi dağıtıldı. Şunları öğrendiniz:

> [!div class="checklist"]
> * Önkoşulları kurun ve gerekli sanal ağı ve alt ağları oluşturun
> * Küme dağıtma
> * Kullanıcıyı kullanarak kümeye bağlanma `kubeadmin`

Azure Active Directory kullanarak kümeyi kimlik doğrulaması için yapılandırmayı öğrenmek üzere bir sonraki makaleye ilerleyin.


* [Komut satırını kullanarak Azure Active Directory kimlik doğrulamasını yapılandırma](configure-azure-ad-cli.md)


* [Azure portal ve OpenShift web konsolunu kullanarak Azure Active Directory kimlik doğrulamasını yapılandırma](configure-azure-ad-cli.md)
