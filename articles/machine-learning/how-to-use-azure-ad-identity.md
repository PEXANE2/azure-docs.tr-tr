---
title: Web hizmetinizdeki AAD kimliğini kullanın
titleSuffix: Azure Machine Learning
description: Puanlama sırasında bulut kaynaklarına erişmek için Azure Kubernetes hizmetinde Web hizmetinizdeki AAD kimliğini kullanın.
services: machine-learning
ms.author: larryfr
author: BlackMist
ms.reviewer: aashishb
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.date: 02/10/2020
ms.openlocfilehash: aa434a4e19321e88e388661ccb488f15c98d3a0f
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87078072"
---
# <a name="use-azure-ad-identity-with-your-machine-learning-web-service-in-azure-kubernetes-service"></a>Azure Kubernetes hizmetinde, Machine Learning Web hizmeti ile Azure AD kimliğini kullanma

Bu nasıl yapılır, Azure Kubernetes hizmetindeki dağıtılmış makine öğrenimi modelinize bir Azure Active Directory (AAD) kimliği atamayı öğrenirsiniz. [AAD Pod Identity](https://github.com/Azure/aad-pod-identity) projesi, [yönetilen bir kimlik](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) ve Kubernetes temel ÖĞELERINI kullanarak, uygulamaların AAD ile güvenli bir şekilde bulut kaynaklarına erişmesini sağlar. Bu, Web hizmetinizin kimlik bilgilerini eklemek veya belirteçleri doğrudan komut dosyanızın içinde yönetmek zorunda kalmadan Azure kaynaklarınıza güvenli bir şekilde erişmesini sağlar `score.py` . Bu makalede, Azure Kubernetes hizmet kümenizde bir Azure kimliği oluşturma ve kurma adımları açıklanmakta ve bu kimlik dağıtılan Web hizmetinize atanır.

## <a name="prerequisites"></a>Önkoşullar

- [Machine Learning hizmeti Için Azure CLI uzantısı](reference-azure-machine-learning-cli.md), [Python için Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)veya [Azure Machine Learning Visual Studio Code uzantısı](tutorial-setup-vscode-extension.md).

- Komutunu kullanarak AKS kümenize erişin `kubectl` . Daha fazla bilgi için bkz [. kümeye bağlanma](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough#connect-to-the-cluster)

- AKS kümenize dağıtılan bir Azure Machine Learning Web hizmeti.

## <a name="create-and-install-an-azure-identity-in-your-aks-cluster"></a>AKS kümenizde bir Azure kimliği oluşturma ve yüklemeyi

1. AKS kümenizin RBAC 'nin etkin olup olmadığını anlamak için aşağıdaki komutu kullanın:

    ```azurecli-interactive
    az aks show --name <AKS cluster name> --resource-group <resource group name> --subscription <subscription id> --query enableRbac
    ```

    Bu komut, RBAC etkinse bir değeri döndürür `true` . Bu değer, bir sonraki adımda kullanılacak komutu belirler.

1. AKS kümenize [AAD Pod kimliği](https://github.com/Azure/aad-pod-identity#getting-started) yüklemek için aşağıdaki komutlardan birini kullanın:

    * AKS kümenizde **RBAC etkinse** aşağıdaki komutu kullanın:
    
        ```azurecli-interactive
        kubectl apply -f https://raw.githubusercontent.com/Azure/aad-pod-identity/master/deploy/infra/deployment-rbac.yaml
        ```
    
    * AKS kümenizde **RBAC etkin değilse**, aşağıdaki komutu kullanın:
    
        ```azurecli-interactive
        kubectl apply -f https://raw.githubusercontent.com/Azure/aad-pod-identity/master/deploy/infra/deployment.yaml
        ```
    
        Komutun çıktısı aşağıdaki metne benzer:

        ```text
        customresourcedefinition.apiextensions.k8s.io/azureassignedidentities.aadpodidentity.k8s.io created
        customresourcedefinition.apiextensions.k8s.io/azureidentitybindings.aadpodidentity.k8s.io created
        customresourcedefinition.apiextensions.k8s.io/azureidentities.aadpodidentity.k8s.io created
        customresourcedefinition.apiextensions.k8s.io/azurepodidentityexceptions.aadpodidentity.k8s.io created
        daemonset.apps/nmi created
        deployment.apps/mic created
        ```

1. AAD Pod Identity proje sayfasında gösterilen adımları izleyerek [bir Azure kimliği oluşturun](https://github.com/Azure/aad-pod-identity#2-create-an-azure-identity) .

1. AAD Pod Identity proje sayfasında gösterilen adımları izleyerek [Azure kimliğini yükler](https://github.com/Azure/aad-pod-identity#3-install-the-azure-identity) .

1. AAD Pod Identity proje sayfasında gösterilen adımları izleyerek [Azure kimlik bağlamasını yükler](https://github.com/Azure/aad-pod-identity#5-install-the-azure-identity-binding) .

1. Önceki adımda oluşturulan Azure kimliği AKS kümeniz ile aynı kaynak grubunda değilse, AAD Pod kimlik projesi sayfasında gösterilen adımları izleyerek [MIC Için Izinleri ayarla](https://github.com/Azure/aad-pod-identity#6-set-permissions-for-mic) ' yı izleyin.

## <a name="assign-azure-identity-to-machine-learning-web-service"></a>Machine Learning Web hizmetine Azure kimliği atama

Aşağıdaki adımlarda, önceki bölümde oluşturulan Azure kimliği kullanılır ve bir **seçici etiketi**aracılığıyla aks Web hizmetinize atanır.

İlk olarak, Azure kimliğini atamak istediğiniz AKS kümenizdeki dağıtımınızın adını ve ad alanını tanımlama. Aşağıdaki komutu çalıştırarak bu bilgileri alabilirsiniz. Ad alanları Azure Machine Learning çalışma alanı adınızın olması ve dağıtım adınızın, portalda gösterilen uç nokta adı olması gerekir.

```azurecli-interactive
kubectl get deployment --selector=isazuremlapp=true --all-namespaces --show-labels
```

Dağıtım belirtimini düzenleyerek Azure Identity Selector etiketini dağıtımınıza ekleyin. Seçici değeri, [Azure kimlik bağlamasını yüklemenin](https://github.com/Azure/aad-pod-identity#5-install-the-azure-identity-binding)5. adımında tanımladığınız bir değer olmalıdır.

```yaml
apiVersion: "aadpodidentity.k8s.io/v1"
kind: AzureIdentityBinding
metadata:
  name: demo1-azure-identity-binding
spec:
  AzureIdentity: <a-idname>
  Selector: <label value to match>
```

Azure kimlik Seçicisi etiketini eklemek için dağıtımı düzenleyin. Altında aşağıdaki bölüme gidin `/spec/template/metadata/labels` . Gibi değerler görmeniz gerekir `isazuremlapp: “true”` . Aşağıda gösterildiği gibi AAD-Pod Identity etiketini ekleyin.

```azurecli-interactive
    kubectl edit deployment/<name of deployment> -n azureml-<name of workspace>
```

```yaml
spec:
  template:
    metadata:
      labels:
       aadpodidbinding: "<value of Selector in AzureIdentityBinding>"
      ...
```

Etiketin doğru eklendiğini doğrulamak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
   kubectl get deployment <name of deployment> -n azureml-<name of workspace> --show-labels
```

Tüm Pod durumlarını görmek için aşağıdaki komutu çalıştırın.

```azurecli-interactive
    kubectl get pods -n azureml-<name of workspace>
```

Pod 'ler çalışır duruma getirildikten sonra, bu dağıtımın Web Hizmetleri Azure kaynaklarınıza Azure kimliğiniz üzerinden, kimlik bilgilerini kodunuza eklemek zorunda kalmadan erişebilecek. 

## <a name="assign-the-appropriate-roles-to-your-azure-identity"></a>Azure kimliğinize uygun rolleri atayın

Diğer Azure kaynaklarına erişmek için [Azure yönetilen kimliğinizi uygun rollerle atayın](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal) . Atadığınız rollerin doğru **veri eylemlerine**sahip olduğundan emin olun. Örneğin, genel [okuyucu rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#reader) , [Depolama Blobu veri okuyucusu rolünün](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader) Depolama Blobu için okuma izinlerine sahip olur.

## <a name="use-azure-identity-with-your-machine-learning-web-service"></a>Machine Learning Web hizmeti ile Azure kimliğini kullanma

AKS kümenize bir model dağıtın. `score.py`Betik, Azure kimliğinizin erişebileceği Azure kaynaklarına işaret eden işlemleri içerebilir. Erişmeye çalıştığınız kaynak için gerekli istemci kitaplığı bağımlılıklarınızı yüklediğinizden emin olun. Aşağıda, Azure kimliğinizi hizmetinize farklı Azure kaynaklarına erişmek için nasıl kullanabileceğinizi gösteren birkaç örnek verilmiştir.

### <a name="access-key-vault-from-your-web-service"></a>Web hizmetinizden erişim Key Vault

**Key Vault**içinde bir gizli dizi Için Azure kimlik okuma erişimi verildiyse, `score.py` aşağıdaki kodu kullanarak buna erişebilirsiniz.

```python
from azure.identity import DefaultAzureCredential
from azure.keyvault.secrets import SecretClient

my_vault_name = "yourkeyvaultname"
my_vault_url = "https://{}.vault.azure.net/".format(my_vault_name) 
my_secret_name = "sample-secret"

# This will use your Azure Managed Identity
credential = DefaultAzureCredential()
secret_client = SecretClient(
    vault_url=my_vault_url,
    credential=credential)
secret = secret_client.get_secret(my_secret_name)
```

> [!IMPORTANT]
> Bu örnek, DefaultAzureCredential kullanır. Belirli bir erişim ilkesi kullanarak kimlik erişiminizin olması için bkz. [4. Bölüm: gizli dizi Azure Key Vault parolayı alma](../key-vault/general/authentication.md#part-4-retrieve-the-secret-from-your-azure-key-vault-in-an-application-python).

### <a name="access-blob-from-your-web-service"></a>Web hizmetinizden blob 'a erişin

Azure kimlik 'e bir **Depolama Blobu**içindeki verilere yönelik okuma erişimi verildiyse, `score.py` aşağıdaki kodu kullanarak buna erişebilirsiniz.

```python
from azure.identity import DefaultAzureCredential
from azure.storage.blob import BlobServiceClient

my_storage_account_name = "yourstorageaccountname"
my_storage_account_url = "https://{}.blob.core.windows.net/".format(my_storage_account_name)

# This will use your Azure Managed Identity
credential = DefaultAzureCredential()
blob_service_client = BlobServiceClient(
    account_url=my_storage_account_url,
    credential=credential
)
blob_client = blob_service_client.get_blob_client(container="some-container", blob="some_text.txt")
blob_data = blob_client.download_blob()
blob_data.readall()
```

## <a name="next-steps"></a>Sonraki adımlar

* Python Azure Identity istemci kitaplığını kullanma hakkında daha fazla bilgi için GitHub 'daki [depoya](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/identity/azure-identity#azure-identity-client-library-for-python) bakın.
* Azure Kubernetes hizmet kümelerine modeller dağıtmaya ilişkin ayrıntılı bir kılavuz için bkz. [nasıl yapılır](how-to-deploy-azure-kubernetes-service.md).
