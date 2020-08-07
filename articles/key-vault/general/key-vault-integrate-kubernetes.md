---
title: Azure Key Vault’u Kubernetes ile tümleştirme
description: Bu öğreticide, Kubernetes pods 'ye bağlamak için gizli dizi kapsayıcısı depolama arabirimi (CSı) sürücüsünü kullanarak Azure anahtar kasanızdan gizli dizi ve gizli dizileri alırsınız.
author: taytran0
ms.author: t-trtr
ms.service: key-vault
ms.topic: tutorial
ms.date: 06/04/2020
ms.openlocfilehash: e70ee75344a939ea1632df3549d796617c7596af
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87902006"
---
# <a name="tutorial-configure-and-run-the-azure-key-vault-provider-for-the-secrets-store-csi-driver-on-kubernetes"></a>Öğretici: Kubernetes 'te gizli dizi için Azure Key Vault sağlayıcıyı yapılandırma ve çalıştırma

Bu öğreticide, gizli dizileri Kubernetes pods 'ye bağlamak için gizli dizi kapsayıcısı depolama arabirimi (CSı) sürücüsünü kullanarak Azure anahtar kasanızdan gizli dizi ve gizli dizi bilgileri alabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Hizmet sorumlusu oluşturun veya yönetilen kimlikleri kullanın.
> * Azure CLı kullanarak bir Azure Kubernetes hizmeti (AKS) kümesi dağıtın.
> * Held ve gizlilikler deposunun CSı sürücüsünü yükler.
> * Azure Anahtar Kasası oluşturun ve gizli dizilerinizi ayarlayın.
> * Kendi SecretProviderClass nesneniz oluşturun.
> * Hizmet sorumlunuzu atayın veya yönetilen kimlikler kullanın.
> * Ana kasanızdan bağlı gizli dizileri kullanarak Pod 'nizi dağıtın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

* Bu öğreticiye başlamadan önce [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli-windows?view=azure-cli-latest)'yı yükleyebilirsiniz.

## <a name="create-a-service-principal-or-use-managed-identities"></a>Hizmet sorumlusu oluşturma veya yönetilen kimlikleri kullanma

Yönetilen kimlikler kullanmayı planlıyorsanız bir sonraki bölüme geçebilirsiniz.

Azure anahtar kasanızdan hangi kaynaklara erişilebileceklerini denetlemek için bir hizmet sorumlusu oluşturun. Bu hizmet sorumlusunun erişimi, kendisine atanmış rollerle kısıtlıdır. Bu özellik, hizmet sorumlusunun sırlarınızı nasıl yönetebileceğini denetlemenizi sağlar. Aşağıdaki örnekte, hizmet sorumlusunun adı *Contososerviceprincipal*' dır.

```azurecli
az ad sp create-for-rbac --name contosoServicePrincipal --skip-assignment
```
Bu işlem bir dizi anahtar/değer çiftini döndürür:

![ContosoServicePrincipal için AppID ve parolayı gösteren ekran görüntüsü](../media/kubernetes-key-vault-1.png)

**AppID** ve **parola** kimlik bilgilerini daha sonra kullanmak üzere kopyalayın.

## <a name="deploy-an-azure-kubernetes-service-aks-cluster-by-using-the-azure-cli"></a>Azure CLı kullanarak bir Azure Kubernetes hizmeti (AKS) kümesi dağıtma

Azure Cloud Shell kullanmanız gerekmez. Azure CLı yüklü olan komut isteminize (Terminal) sahip olmanız yeterli olacaktır. 

[Azure CLI kullanarak bir Azure Kubernetes hizmet kümesi dağıtma](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough)bölümünde "kaynak grubu oluşturma", "aks kümesi oluşturma" ve "kümeye bağlanma" bölümlerine gidin. 

> [!NOTE] 
> Hizmet sorumlusu yerine Pod kimliği kullanmayı planlıyorsanız, aşağıdaki komutta gösterildiği gibi Kubernetes kümesini oluştururken etkinleştirin:
>
> ```azurecli
> az aks create -n contosoAKSCluster -g contosoResourceGroup --kubernetes-version 1.16.9 --node-count 1 --enable-managed-identity
> ```

1. [Path ortam değişkeninizi](https://www.java.com/en/download/help/path.xml) indirdiğiniz *kubectl.exe* dosyası olarak ayarlayın.
1. Aşağıdaki komutu kullanarak Kubernetes sürümünüzü denetleyin ve istemci ve sunucu sürümünü çıkartır. İstemci sürümü, yüklediğiniz *kubectl.exe* dosyasıdır ve sunucu sürümü, kümenizin üzerinde çalıştığı Azure Kubernetes hizmetidir (aks).
    ```azurecli
    kubectl version
    ```
1. Kubernetes sürümünüzün 1.16.0 veya üzeri olduğundan emin olun. Aşağıdaki komut hem Kubernetes kümesini hem de düğüm havuzunu yükseltir. Komutun yürütülmesi birkaç dakika sürebilir. Bu örnekte, kaynak grubu *Contosoresourcegroup*ve Kubernetes kümesi *Contosoakscluster*' dir.
    ```azurecli
    az aks upgrade --kubernetes-version 1.16.9 --name contosoAKSCluster --resource-group contosoResourceGroup
    ```
1. Oluşturduğunuz AKS kümesinin meta verilerini göstermek için aşağıdaki komutu kullanın. Daha sonra kullanmak üzere **PrincipalId**, **ClientID**, **SubscriptionID**ve **noderesourcegroup** 'u kopyalayın. ASK kümesi, Yönetilen kimlikler etkinleştirilmiş olarak oluşturulmadıysa, **PrincipalId** ve **ClientID** null olur. 

    ```azurecli
    az aks show --name contosoAKSCluster --resource-group contosoResourceGroup
    ```

    Çıktıda her iki parametre de vurgulanır:
    
    ![Azure CLı 'nin PrincipalId ve ClientID değerleri ile ekran görüntüsü, ](../media/kubernetes-key-vault-2.png) ![ SubscriptionID ve nodeResourceGroup değerleriyle vurgulanan Azure CLI ekran görüntüsü](../media/kubernetes-key-vault-3.png)
    
## <a name="install-helm-and-the-secrets-store-csi-driver"></a>Held ve gizlilikler deposunun CSı sürücüsünü yükleyip

Gizli anahtar deposu CSı sürücüsünü yüklemek için öncelikle [Held](https://helm.sh/docs/intro/install/)'yi yüklemeniz gerekir.

Gizli dizileri [depolama CSI](https://github.com/Azure/secrets-store-csi-driver-provider-azure/blob/master/charts/csi-secrets-store-provider-azure/README.md) sürücü arabirimi sayesinde, Azure Anahtar Kasası Örneğinizde depolanan gizli dizileri alabilir ve sonra gizli Içeriği Kubernetes pods 'ye bağlamak için sürücü arabirimini kullanabilirsiniz.

1. Held sürümünün v3 veya üzeri olduğundan emin olun:
    ```azurecli
    helm version
    ```
1. Sürücü için gizli anahtar deposu CSı sürücüsünü ve Azure Key Vault sağlayıcısını yükler:
    ```azurecli
    helm repo add csi-secrets-store-provider-azure https://raw.githubusercontent.com/Azure/secrets-store-csi-driver-provider-azure/master/charts

    helm install csi-secrets-store-provider-azure/csi-secrets-store-provider-azure --generate-name
    ```

## <a name="create-an-azure-key-vault-and-set-your-secrets"></a>Azure Anahtar Kasası oluşturma ve sırlarınızı ayarlama

Kendi anahtar kasanızı oluşturmak ve sırlarınızı ayarlamak için [Azure CLI kullanarak Azure Key Vault bir gizli anahtarı ayarlama ve alma](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-cli)bölümündeki yönergeleri izleyin.

> [!NOTE] 
> Azure Cloud Shell kullanmanız veya yeni bir kaynak grubu oluşturmanız gerekmez. Daha önce Kubernetes kümesi için oluşturduğunuz kaynak grubunu kullanabilirsiniz.

## <a name="create-your-own-secretproviderclass-object"></a>Kendi SecretProviderClass nesneniz oluşturma

Özel bir SecretProviderClass nesneniz olan gizli dizi parametrelerini sağlayıcıya özgü parametrelerle oluşturmak için [Bu şablonu kullanın](https://github.com/Azure/secrets-store-csi-driver-provider-azure/blob/master/examples/v1alpha1_secretproviderclass.yaml). Bu nesne, anahtar kasanıza kimlik erişimi sağlar.

Örnek SecretProviderClass YAML dosyasında eksik parametreleri girin. Aşağıdaki parametreler gereklidir:

* **Useratandıdentityıd**: hizmet SORUMLUSUNUN istemci kimliği
* **Keyvaultname**: anahtar kasanızın adı
* **nesneler**: bağlamak istediğiniz tüm gizli içerik için kapsayıcı
    * **ObjectName**: gizli içeriğin adı
    * **ObjectType**: nesne türü (gizli, anahtar, sertifika)
* **resourceGroup**: kaynak grubunun adı
* **SubscriptionID**: anahtar kasanızın abonelik kimliği
* **Tenantıd**: anahtar kasaınızın Kiracı kimliği veya dizin kimliği

Güncelleştirilmiş şablon aşağıdaki kodda gösterilmiştir. Bunu bir YAML dosyası olarak indirin ve gerekli alanları girin. Bu örnekte, Anahtar Kasası **contosoKeyVault5**' dir. İki gizli dizi vardır, **secret1** ve **secret2**.

> [!NOTE] 
> Yönetilen kimlikler kullanıyorsanız, **Usepodidentity** değerini *true*olarak ayarlayın ve **useratandidentityıd** değerini tırnak işareti (**""**) olarak ayarlayın. 

```yaml
apiVersion: secrets-store.csi.x-k8s.io/v1alpha1
kind: SecretProviderClass
metadata:
  name: azure-kvname
spec:
  provider: azure
  parameters:
    usePodIdentity: "false"                   # [REQUIRED] Set to "true" if using managed identities
    useVMManagedIdentity: "false"             # [OPTIONAL] if not provided, will default to "false"
    userAssignedIdentityID: "servicePrincipalClientID"       # [REQUIRED] If you're using a service principal, use the client id to specify which user-assigned managed identity to use. If you're using a user-assigned identity as the VM's managed identity, specify the identity's client id. If the value is empty, it defaults to use the system-assigned identity on the VM
                                                             #     az ad sp show --id http://contosoServicePrincipal --query appId -o tsv
                                                             #     the preceding command will return the client ID of your service principal
    keyvaultName: "contosoKeyVault5"          # [REQUIRED] the name of the key vault
                                              #     az keyvault show --name contosoKeyVault5
                                              #     the preceding command will display the key vault metadata, which includes the subscription ID, resource group name, key vault 
    cloudName: ""                             # [OPTIONAL for Azure] if not provided, Azure environment will default to AzurePublicCloud
    objects:  |
      array:
        - |
          objectName: secret1                 # [REQUIRED] object name
                                              #     az keyvault secret list --vault-name “contosoKeyVault5”
                                              #     the above command will display a list of secret names from your key vault
          objectType: secret                  # [REQUIRED] object types: secret, key, or cert
          objectVersion: ""                   # [OPTIONAL] object versions, default to latest if empty
        - |
          objectName: secret2
          objectType: secret
          objectVersion: ""
    resourceGroup: "contosoResourceGroup"     # [REQUIRED] the resource group name of the key vault
    subscriptionId: "subscriptionID"          # [REQUIRED] the subscription ID of the key vault
    tenantId: "tenantID"                      # [REQUIRED] the tenant ID of the key vault
```
Aşağıdaki görüntüde, **az keykasa Show--Name contosoKeyVault5** for the ilgili vurgulanan meta verilerle konsol çıktısı gösterilmektedir:

!["Az keykasa Show--Name contosoKeyVault5" için konsol çıktısını gösteren ekran görüntüsü](../media/kubernetes-key-vault-4.png)

## <a name="assign-your-service-principal-or-use-managed-identities"></a>Hizmet sorumlunuzu atayın veya yönetilen kimlikler kullanın

### <a name="assign-a-service-principal"></a>Hizmet sorumlusu atama

Hizmet sorumlusu kullanıyorsanız, anahtar kasanıza erişmek ve gizli dizileri almak için izin verin. Aşağıdaki komutu gerçekleştirerek *okuyucu* rolünü atayın ve anahtar kasaınızdan gizli dizileri *almak* için hizmet sorumlusu izinleri verin:

1. Hizmet sorumlunuzu mevcut anahtar kasanıza atayın. **$AZURE _CLIENT_ID** parametresi, hizmet sorumlunuzu oluşturduktan sonra kopyaladığınız **AppID** 'dir.
    ```azurecli
    az role assignment create --role Reader --assignee $AZURE_CLIENT_ID --scope /subscriptions/$SUBID/resourcegroups/$KEYVAULT_RESOURCE_GROUP/providers/Microsoft.KeyVault/vaults/$KEYVAULT_NAME
    ```

    Komutun çıktısı aşağıdaki görüntüde gösterilmiştir: 

    ![PrincipalId değerini gösteren ekran görüntüsü](../media/kubernetes-key-vault-5.png)

1. Gizli dizileri almak için hizmet sorumlusu izinleri verin:
    ```azurecli
    az keyvault set-policy -n $KEYVAULT_NAME --secret-permissions get --spn $AZURE_CLIENT_ID
    ```

1. Artık hizmet sorumlunuzu anahtar kasaınızdan gizli dizileri okuma izinleriyle yapılandırdık. **$AZURE _CLIENT_SECRET** , hizmet sorumlunun parolasıdır. Bir Kubernetes gizli anahtarı olarak, hizmet sorumlusu kimlik bilgilerinizi gizli dizi olarak ekleyin:
    ```azurecli
    kubectl create secret generic secrets-store-creds --from-literal clientid=$AZURE_CLIENT_ID --from-literal clientsecret=$AZURE_CLIENT_SECRET
    ```

> [!NOTE] 
> Kubernetes Pod 'u dağıtıyorsanız ve geçersiz bir Istemci gizli KIMLIĞI hakkında hata alırsanız, zaman aşımına uğradı veya sıfırlanarak daha eski bir Istemci gizli KIMLIĞINIZ olabilir. Bu sorunu çözmek için, gizli dizi *-kimlik bilgileri* gizli anahtarını silin ve geçerli ISTEMCI parolası kimliğiyle yeni bir tane oluşturun. Gizli dizileri silme *-Mağaza-kimlik bilgileri*, aşağıdaki komutu çalıştırın:
>
> ```azurecli
> kubectl delete secrets secrets-store-creds
> ```

Hizmet sorumlusunun Istemci gizli KIMLIĞINI unuttuysanız, aşağıdaki komutu kullanarak sıfırlayabilirsiniz:

```azurecli
az ad sp credential reset --name contosoServicePrincipal --credential-description "APClientSecret" --query password -o tsv
```

### <a name="use-managed-identities"></a>Yönetilen kimlikleri kullanma

Yönetilen kimlikler kullanıyorsanız, oluşturduğunuz AKS kümesine belirli roller atayın. 

1. Kullanıcı tarafından atanan yönetilen kimlik oluşturmak, listelemek veya okumak için, AKS kümenizin [yönetilen kimlik operatörü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#managed-identity-operator) rolüne atanması gerekir. **$ClientID** Kubernetes kümesinin ClientID 'si olduğundan emin olun. Kapsam için, bu, özellikle AKS kümesi oluşturulduğunda yapılmış olan düğüm kaynak grubu Azure abonelik hizmetiniz altında olacaktır. Bu kapsam, aşağıda atanan rollerden yalnızca o gruptaki kaynakların etkilendiğinden emin olur. 

    ```azurecli
    az role assignment create --role "Managed Identity Operator" --assignee $clientId --scope /subscriptions/$SUBID/resourcegroups/$NODE_RESOURCE_GROUP
    
    az role assignment create --role "Virtual Machine Contributor" --assignee $clientId --scope /subscriptions/$SUBID/resourcegroups/$NODE_RESOURCE_GROUP
    ```

1. Azure Active Directory (Azure AD) kimliğini AKS 'e yükler.
    ```azurecli
    helm repo add aad-pod-identity https://raw.githubusercontent.com/Azure/aad-pod-identity/master/charts

    helm install pod-identity aad-pod-identity/aad-pod-identity
    ```

1. Bir Azure AD kimliği oluşturun. Çıktıda, daha sonra kullanmak üzere **ClientID** ve **PrincipalId** ' yi kopyalayın.
    ```azurecli
    az identity create -g $resourceGroupName -n $identityName
    ```

1. Anahtar kasanızın önceki adımında oluşturduğunuz Azure AD kimliğine *okuyucu* rolünü atayın ve sonra anahtar kasaınızdan gizli dizileri almak için kimlik izinlerini verin. Azure AD kimliğinden **ClientID** ve **PrincipalId** ' i kullanın.
    ```azurecli
    az role assignment create --role "Reader" --assignee $principalId --scope /subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/contosoResourceGroup/providers/Microsoft.KeyVault/vaults/contosoKeyVault5

    az keyvault set-policy -n contosoKeyVault5 --secret-permissions get --spn $clientId
    ```

## <a name="deploy-your-pod-with-mounted-secrets-from-your-key-vault"></a>Ana kasanızdan bağlı gizli dizileri kullanarak Pod 'nizi dağıtın

SecretProviderClass nesneniz yapılandırmak için aşağıdaki komutu çalıştırın:
```azurecli
kubectl apply -f secretProviderClass.yaml
```

### <a name="use-a-service-principal"></a>Hizmet sorumlusu kullanma

Hizmet sorumlusu kullanıyorsanız, Kubernetes yığınlarınızı SecretProviderClass ve daha önce yapılandırdığınız gizli dizileri-Store-creds ile dağıtmak için aşağıdaki komutu kullanın. Dağıtım şablonları aşağıda verilmiştir:
* [Linux](https://github.com/Azure/secrets-store-csi-driver-provider-azure/blob/master/examples/nginx-pod-secrets-store-inline-volume-secretproviderclass.yaml) için
* [Windows](https://github.com/Azure/secrets-store-csi-driver-provider-azure/blob/master/examples/windows-pod-secrets-store-inline-volume-secret-providerclass.yaml) için

```azurecli
kubectl apply -f updateDeployment.yaml
```

### <a name="use-managed-identities"></a>Yönetilen kimlikleri kullanma

Yönetilen kimlikler kullanıyorsanız, kümenizde daha önce oluşturduğunuz kimliğe başvuran bir *AzureIdentity* oluşturun. Daha sonra, oluşturduğunuz AzureIdentity 'e başvuran bir *AzureIdentityBinding* oluşturun. Aşağıdaki şablondaki parametreleri doldurun ve sonra da *Podidentityandbinding. YAML*olarak kaydedin.  

```yml
apiVersion: aadpodidentity.k8s.io/v1
kind: AzureIdentity
metadata:
    name: "azureIdentityName"               # The name of your Azure identity
spec:
    type: 0                                 # Set type: 0 for managed service identity
    resourceID: /subscriptions/<SUBID>/resourcegroups/<RESOURCEGROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<AZUREIDENTITYNAME>
    clientID: "managedIdentityClientId"     # The clientId of the Azure AD identity that you created earlier
---
apiVersion: aadpodidentity.k8s.io/v1
kind: AzureIdentityBinding
metadata:
    name: azure-pod-identity-binding
spec:
    azureIdentity: "azureIdentityName"      # The name of your Azure identity
    selector: azure-pod-identity-binding-selector
```
    
Bağlamayı yürütmek için aşağıdaki komutu çalıştırın:

```azurecli
kubectl apply -f podIdentityAndBinding.yaml
```

Sonra, Pod öğesini dağıtırsınız. Aşağıdaki kod, önceki adımdan Pod kimlik bağlamasını kullanan dağıtım YAML dosyasıdır. Bu dosyayı *Pod Bindingdeployment. YAML*olarak kaydedin.

```yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-secrets-store-inline
  labels:
    aadpodidbinding: azure-pod-identity-binding-selector
spec:
  containers:
    - name: nginx
      image: nginx
      volumeMounts:
        - name: secrets-store-inline
          mountPath: "/mnt/secrets-store"
          readOnly: true
  volumes:
    - name: secrets-store-inline
      csi:
        driver: secrets-store.csi.k8s.io
        readOnly: true
        volumeAttributes:
          secretProviderClass: azure-kvname
```

Pod 'nizi dağıtmak için aşağıdaki komutu çalıştırın:

```azurecli
kubectl apply -f podBindingDeployment.yaml
```

### <a name="check-the-pod-status-and-secret-content"></a>Pod durum ve gizli içeriğini denetleme 

Dağıttığınız bir pod 'yi göstermek için aşağıdaki komutu çalıştırın:
```azurecli
kubectl get pods
```

Pod uygulamanızın durumunu denetlemek için şu komutu çalıştırın:
```azurecli
kubectl describe pod/nginx-secrets-store-inline
```

![Pod 'un "çalışıyor" durumunu görüntüleyen ve tüm olayları "normal" olarak gösteren Azure CLı çıkışının ekran görüntüsü ](../media/kubernetes-key-vault-6.png)

Çıkış penceresinde, dağıtılan Pod, *çalışıyor* durumunda olmalıdır. Alttaki **Olaylar** bölümünde tüm olay türleri *normal*olarak görüntülenir.

Pod 'un çalıştığını doğruladıktan sonra Pod 'un anahtar kasasındaki gizli dizileri içerdiğini doğrulayabilirsiniz.

Pod 'da bulunan tüm gizli dizileri göstermek için aşağıdaki komutu çalıştırın:
```azurecli
kubectl exec -it nginx-secrets-store-inline -- ls /mnt/secrets-store/
```

Belirli bir gizli dizi içeriğini göstermek için aşağıdaki komutu çalıştırın:
```azurecli
kubectl exec -it nginx-secrets-store-inline -- cat /mnt/secrets-store/secret1
```

Gizli dizi içeriğinin görüntülendiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Anahtar kasanızın kurtarılabilir olduğundan emin olmak için, bkz.:
> [!div class="nextstepaction"]
> [Geçici silme özelliğini aç](https://docs.microsoft.com/azure/key-vault/general/soft-delete-cli)
