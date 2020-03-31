---
title: Web hizmetinizle AAD kimliğini kullanma
titleSuffix: Azure Machine Learning
description: Puanlama sırasında bulut kaynaklarına erişmek için Azure Kubernetes Hizmeti'ndeki web hizmetinizle AAD kimliğini kullanın.
services: machine-learning
author: trevorbye
ms.author: trbye
ms.reviewer: aashishb
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 02/10/2020
ms.openlocfilehash: f997aef59e91bed325b84af855a84f43cd639d83
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77122850"
---
# <a name="use-azure-ad-identity-with-your-machine-learning-web-service-in-azure-kubernetes-service"></a>Azure Kubernetes Hizmeti'nde makine öğrenimi web hizmetinizle Azure AD kimliğini kullanın

Bu nasıl yapılır' da, Azure Kubernetes Hizmeti'nde dağıtılmış makine öğrenimi modelinize Azure Active Directory (AAD) kimliğini nasıl atadığınızı öğrenirsiniz. [AAD Pod Identity](https://github.com/Azure/aad-pod-identity) projesi, yönetilen [kimlik](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) ve Kubernetes ilkellerini kullanarak uygulamaların AAD ile bulut kaynaklarına güvenli bir şekilde erişmesine olanak tanır. Bu, web hizmetinizin kimlik bilgilerini gömmek veya belirteçleri doğrudan komut `score.py` dosyanızın içine yönetmeden Azure kaynaklarınıza güvenli bir şekilde erişmesine olanak tanır. Bu makalede, Azure Kubernetes Hizmet kümenizde bir Azure Kimliği oluşturma ve yükleme adımları açıklanır ve kimliği dağıtılan web hizmetinize atayan.

## <a name="prerequisites"></a>Ön koşullar

- [Machine Learning hizmeti için Azure CLI uzantısı,](reference-azure-machine-learning-cli.md)Python için Azure Machine Learning [SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)veya [Azure Machine Learning Visual Studio Code uzantısı.](tutorial-setup-vscode-extension.md)

- Komutu kullanarak AKS `kubectl` kümenize erişin. Daha fazla bilgi için bünyesine bürün: [Kümeye Bağlan](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough#connect-to-the-cluster)

- AKS kümenize dağıtılan bir Azure Machine Learning web hizmeti.

## <a name="create-and-install-an-azure-identity-in-your-aks-cluster"></a>AKS kümenizde Bir Azure Kimliği oluşturma ve yükleme

1. AKS kümenizin RBAC etkin olup olmadığını belirlemek için aşağıdaki komutu kullanın:

    ```azurecli-interactive
    az aks show --name <AKS cluster name> --resource-group <resource group name> --subscription <subscription id> --query enableRbac
    ```

    Bu komut, RBAC `true` etkinse bir değer döndürür. Bu değer, bir sonraki adımda kullanılacak komutu belirler.

1. AKS kümenize [AAD Pod Kimliği](https://github.com/Azure/aad-pod-identity#getting-started) yüklemek için aşağıdaki komutlardan birini kullanın:

    * AKS kümenizde **RBAC** etkinleştirilmişse aşağıdaki komutu kullanın:
    
        ```azurecli-interactive
        kubectl apply -f https://raw.githubusercontent.com/Azure/aad-pod-identity/master/deploy/infra/deployment-rbac.yaml
        ```
    
    * AKS kümenizde **RBAC etkin değilse**aşağıdaki komutu kullanın:
    
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

1. AAD Pod Identity proje sayfasında gösterilen adımları izleyerek [bir Azure Kimliği oluşturun.](https://github.com/Azure/aad-pod-identity#2-create-an-azure-identity)

1. AAD Pod Identity proje sayfasında gösterilen adımları izleyerek [Azure Kimliğini yükleyin.](https://github.com/Azure/aad-pod-identity#3-install-the-azure-identity)

1. AAD Pod Identity proje sayfasında gösterilen adımları izleyerek [Azure Kimlik Bağlama'yı yükleyin.](https://github.com/Azure/aad-pod-identity#5-install-the-azure-identity-binding)

1. Önceki adımda oluşturulan Azure Kimliği AKS kümenizle aynı kaynak grubunda değilse, AAD Pod Identity proje sayfasında gösterilen adımları izleyerek [MIC için İzinleri](https://github.com/Azure/aad-pod-identity#6-set-permissions-for-mic) Ayarla'yı izleyin.

## <a name="assign-azure-identity-to-machine-learning-web-service"></a>Makine öğrenimi web hizmetine Azure Kimliği atama

Aşağıdaki adımlar, önceki bölümde oluşturulan Azure Kimliğini kullanır ve seçici **etiketi**aracılığıyla AKS web hizmetinize atay.

İlk olarak, Azure Kimliğini atamak istediğiniz AKS kümenizde dağıtımınızın adını ve ad alanını tanımlayın. Aşağıdaki komutu çalıştırarak bu bilgileri alabilirsiniz. Ad alanları Azure Machine Learning çalışma alanı adınız olmalı ve dağıtım adınız portalda gösterildiği gibi bitiş noktanız olmalıdır.

```azurecli-interactive
kubectl get deployment --selector=isazuremlapp=true --all-namespaces --show-labels
```

Dağıtım spec düzenleyerek dağıtımınıza Azure Kimlik seçici etiketini ekleyin. Seçici değeri, [Azure Kimlik Bağlamayı Yükle'nin](https://github.com/Azure/aad-pod-identity#5-install-the-azure-identity-binding)adım 5'inde tanımladığınız değer olmalıdır.

```yaml
apiVersion: "aadpodidentity.k8s.io/v1"
kind: AzureIdentityBinding
metadata:
  name: demo1-azure-identity-binding
spec:
  AzureIdentity: <a-idname>
  Selector: <label value to match>
```

Azure Kimliği seçici etiketini eklemek için dağıtımı edin. Aşağıdaki `/spec/template/metadata/labels`bölüme gidin. ' gibi `isazuremlapp: “true”`değerler görmeniz gerekir. Aşağıda gösterildiği gibi aad-pod-identity etiketini ekleyin.

```azurecli-interactive
    kubectl edit deployment/<name of deployment> -n azureml-<name of workspace>
```

```yaml
spec:
  template:
    metadata:
      labels:
      - aadpodidbinding: "<value of Selector in AzureIdentityBinding>"
      ...
```

Etiketin doğru şekilde eklendiğini doğrulamak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
   kubectl get deployment <name of deployment> -n azureml-<name of workspace> --show-labels
```

Tüm bölme durumlarını görmek için aşağıdaki komutu çalıştırın.

```azurecli-interactive
    kubectl get pods -n azureml-<name of workspace>
```

Bölmeler çalışmaya başladığında, bu dağıtıma yönelik web hizmetleri artık kimlik bilgilerini kodunuza katıştırmak zorunda kalmadan Azure Kimliğiniz üzerinden Azure kaynaklarına erişebilecektir. 

## <a name="assign-the-appropriate-roles-to-your-azure-identity"></a>Azure Kimliğinize uygun rolleri atama

Diğer Azure kaynaklarına erişmek için [Azure Yönetilen Kimliğinizi uygun rollerle atayın.](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal) Atadığınız rollerin doğru Veri **Eylemlerine**sahip olduğundan emin olun. Örneğin, Genel [Okuyucu Rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#reader) olmayabilirken, [Depolama Blob Veri Okuyucu Rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader) Depolama Blob'unuzun izinlerini okudu.

## <a name="use-azure-identity-with-your-machine-learning-web-service"></a>Makine öğrenme web hizmetinizle Azure Kimliği'ni kullanın

AKS kümenize bir model dağıtın. Komut `score.py` dosyası, Azure Kimliğinizin erişebildiği Azure kaynaklarına işaret eden işlemler içerebilir. Erişmeye çalıştığınız kaynak için gerekli istemci kitaplığı bağımlılıklarınızı yüklediğinizden emin olun. Aşağıda, hizmetinizden farklı Azure kaynaklarına erişmek için Azure Kimliğinizi nasıl kullanabileceğinize birkaç örnek verilmiştir.

### <a name="access-key-vault-from-your-web-service"></a>Web hizmetinizden Key Vault'a erişin

Azure Kimliğinize **Key Vault**içindeki bir gizliye okuma `score.py` erişimi verdiyseniz, aşağıdaki kodu kullanarak bu kimlikle erişebilirsiniz.

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

### <a name="access-blob-from-your-web-service"></a>Web hizmetinizden Blob'a erişin

Azure Kimliğinizi Bir **Depolama Blob'undaki**verilere okuma `score.py` erişimi verdiyseniz, aşağıdaki kodu kullanarak bu verilere erişebilirsiniz.

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

* Python Azure Identity istemci kitaplığını nasıl kullanacağınız hakkında daha fazla bilgi için GitHub'daki [depoya](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/identity/azure-identity#azure-identity-client-library-for-python) bakın.
* Modelleri Azure Kubernetes Hizmet kümelerine dağıtma yla ilgili ayrıntılı bir kılavuz için [nasıl yapılır'a](how-to-deploy-azure-kubernetes-service.md)bakın.