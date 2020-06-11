---
title: Azure Key Vault’u Kubernetes ile tümleştirme
description: Bu öğreticide, daha sonra Kubernetes pods içine bağlamak için gizli dizi (container Storage Interface) sürücüsünü kullanarak Azure Key Vault gizli dizileri erişir ve bu bilgileri alacaksınız.
author: taytran0
ms.author: t-trtr
ms.service: key-vault
ms.topic: tutorial
ms.date: 06/04/2020
ms.openlocfilehash: 27d602f22aa3915f39f21ac924afa42b98e70720
ms.sourcegitcommit: eeba08c8eaa1d724635dcf3a5e931993c848c633
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84667173"
---
# <a name="tutorial-configure-and-run-the-azure-key-vault-provider-for-secret-store-csi-driver-on-kubernetes"></a>Öğretici: Kubernetes üzerinde gizli depo CSı sürücüsü için Azure Key Vault sağlayıcısını yapılandırın ve çalıştırın

Bu öğreticide, daha sonra Kubernetes pods içine bağlamak için gizli dizi (container Storage Interface) sürücüsünü kullanarak Azure Key Vault gizli dizileri erişir ve bu bilgileri alacaksınız.

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Hizmet sorumlusu oluşturma veya yönetilen kimlikleri kullanma
> * Azure CLı kullanarak bir Azure Kubernetes hizmet kümesi dağıtma
> * Held ve gizlilikler deposunun CSı sürücüsünü yükler
> * Azure Key Vault oluşturma ve gizli dizileri ayarlama
> * Kendi SecretProviderClass nesneniz oluşturma
> * Hizmet sorumlunuzu atayın veya yönetilen kimlikler kullanın
> * Pod 'nizi Key Vault bağlı gizli dosyalarla dağıtın

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu öğreticiye başlamadan önce [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli-windows?view=azure-cli-latest)'yı yükleyebilirsiniz.

## <a name="create-a-service-principal-or-use-managed-identities"></a>Hizmet sorumlusu oluşturma veya yönetilen kimlikleri kullanma

Yönetilen kimlikler kullanmayı planlıyorsanız bir sonraki bölüme geçebilirsiniz.

Azure Key Vault hangi kaynaklara erişilebileceklerini denetlemek için bir hizmet sorumlusu oluşturun. Bu hizmet sorumlusunun erişimi, kendisine atanmış rollerle kısıtlıdır. Bu özellik, hizmet sorumlusunun sırlarınızı nasıl yönetebileceğini denetlemenizi sağlar. Aşağıdaki örnekte, hizmet sorumlusu adı **Contososerviceprincipal**' dır.

```azurecli
az ad sp create-for-rbac --name contosoServicePrincipal --skip-assignment
```
Bu işlem, bir dizi anahtar/değer çifti döndürür:

![Görüntü](../media/kubernetes-key-vault-1.png)

**AppID** ve **parolayı**kopyalayın. Bu kimlik bilgilerine daha sonra ihtiyacınız olacak.



## <a name="deploy-an-azure-kubernetes-service-cluster-using-azure-cli"></a>Azure CLı kullanarak bir Azure Kubernetes hizmet kümesi dağıtma

Azure Cloud Shell kullanmanız gerekmez, Azure CLı yüklü komut Istemeniz (Terminal) olur. 

Bu [Kılavuzu](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough) izleyin ve şu bölümleri tamamlayın: **bir kaynak grubu oluşturun**, **aks kümesi oluşturun**ve **kümeye bağlanın**.

**Note:** Bir hizmet sorumlusu yerine Pod kimliği kullanmayı planlıyorsanız. Aşağıda gösterildiği gibi, Kubernetes kümesini oluştururken etkinleştirdiğinizden emin olun:

```azurecli
az aks create -n contosoAKSCluster -g contosoResourceGroup --kubernetes-version 1.16.9 --node-count 1 --enable-managed-identity
```

1. [Path ortam değişkeninizi](https://www.java.com/en/download/help/path.xml) indirilen "kubectl.exe" dosyası olarak ayarlayın.
1. Aşağıdaki komutu kullanarak Kubernetes sürümünüzü kontrol edin. Bu komut, istemci ve sunucu sürümünün çıktısını alacak. Sunucu sürümü, kümenizin üzerinde çalıştığı Azure Kubernetes hizmetleri olduğunda, istemci sürümü yüklediğiniz "kubectl.exe" dir.
    ```azurecli
    kubectl version
    ```
1. Kubernetes sürümünüzün **v 1.16.0** veya daha büyük olduğundan emin olun. Bu komut, hem Kubernetes kümesini hem de düğüm havuzunu yükseltecektir. Yürütülmesi birkaç dakika sürebilir. Bu örnekte, kaynak grubu **Contosoresourcegroup** ve Kubernetes kümesi **Contosoakscluster**olur.
    ```azurecli
    az aks upgrade --kubernetes-version 1.16.9 --name contosoAKSCluster --resource-group contosoResourceGroup
    ```
1. Aşağıdaki komutu kullanarak oluşturduğunuz AKS kümesinin meta verilerini görüntüleyin. **PrincipalId**, **ClientID**, **SubscriptionID**ve **noderesourcegroup**' u kopyalayın.
    ```azurecli
    az aks show --name contosoAKSCluster --resource-group contosoResourceGroup
    ```

    Bu, her iki parametrenin de vurgulandığı çıktıdır.
    
    ![Görüntü ](../media/kubernetes-key-vault-2.png) ![ resmi](../media/kubernetes-key-vault-3.png)
    
## <a name="install-helm-and-secrets-store-csi-driver"></a>Held ve gizlilikler deposunun CSı sürücüsünü yükler

Gizli anahtar deposu CSı sürücüsünü yüklemek için [Held](https://helm.sh/docs/intro/install/) 'yi yüklemeniz gerekir.

Gizli dizileri [deposu CSI](https://github.com/Azure/secrets-store-csi-driver-provider-azure/blob/master/charts/csi-secrets-store-provider-azure/README.md) sürücü arabirimi, bir Azure Key Vault örneğinde depolanan gizli içerik almanızı sağlar ve bu gizli Içeriği Kubernetes parçalara bağlamak için sürücü arabirimini kullanır.

1. Held sürümünü denetleyin ve bunun v3 veya daha yüksek olduğundan emin olun:
    ```azurecli
    helm version
    ```
1. Sürücü için gizli anahtar deposu CSı sürücüsünü ve Azure Key Vault sağlayıcısını yükler:
    ```azurecli
    helm repo add csi-secrets-store-provider-azure https://raw.githubusercontent.com/Azure/secrets-store-csi-driver-provider-azure/master/charts

    helm install csi-secrets-store-provider-azure/csi-secrets-store-provider-azure --generate-name
    ```

## <a name="create-an-azure-key-vault-and-set-secrets"></a>Azure Key Vault oluşturma ve gizli dizileri ayarlama

Kendi Key Vault oluşturup gizli dizilerinizi ayarlamak için bu [Kılavuzu](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-cli) izleyin.

**Note:** Azure Cloud Shell kullanmanız veya yeni bir kaynak grubu oluşturmanız gerekmez. Daha önce Kubernetes kümesi için oluşturulan kaynak grubunun kullanılması iyidir.

## <a name="create-your-own-secretproviderclass-object"></a>Kendi SecretProviderClass nesneniz oluşturma

Gizli anahtar deposu CSı sürücüsü için sağlayıcıya özgü parametreler sağlamak üzere kendi özel SecretProviderClass nesneniz oluşturmak için bu [şablonu](https://github.com/Azure/secrets-store-csi-driver-provider-azure/blob/master/examples/v1alpha1_secretproviderclass.yaml) kullanın. Bu nesne, Key Vault kimlik erişimi sağlar.

Belirtilen örnek SecretProviderClass YAML dosyasını kullanma. Eksik parametreleri doldureceğiz. Aşağıdaki parametreler gereklidir:

1.  **Useratandidentityıd:** Hizmet sorumlusu istemci KIMLIĞI
1.  **Keyvaultname:** Key Vault adı
1.  **nesneler:** Bu nesne, bağlamak istediğiniz tüm gizli içeriği içerir
    1.  **ObjectName:** Gizli içeriğin adı
    1.  **ObjectType:** Nesne türü (gizli, anahtar, sertifika)
1.  **resourceGroup:** Kaynak grubunun adı
1.  **abonelik kimliği:** Key Vault abonelik KIMLIĞI
1.  **Tenantıd:** Key Vault kiracı KIMLIĞI (diğer bir deyişle, dizin KIMLIĞI)

Güncelleştirilmiş şablon aşağıda verilmiştir. YAML dosyası olarak indirin ve ilgili gerekli alanları girin. Bu örnekte, Key Vault **contosoKeyVault5** ve iki gizli dizi vardır, **secret1** ve **secret2**.

**Note:** Yönetilen kimlikler kullanıyorsanız, **Usepodidentity** alanının **doğru** olması gerekir ve bu alanı yalnızca tırnak Işaretleriyle birlikte **useratandıdentityıd** **""** olarak bırakın. 

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
    userAssignedIdentityID: "servicePrincipalClientID"       # [REQUIRED] If using a Service Principal, use the client id to specify which user assigned managed identity to use. If using a user assigned identity as the VM's managed identity, then specify the identity's client id. If empty, then defaults to use the system assigned identity on the VM
                                                             #     az ad sp show --id http://contosoServicePrincipal --query appId -o tsv
                                                             #     the above command will return the Client ID of your service principal
    keyvaultName: "contosoKeyVault5"          # [REQUIRED] the name of the Key Vault
                                              #     az keyvault show --name contosoKeyVault5
                                              #     the above command will displays the Key Vault metadata, which includes the subscription ID, resource group name, Key Vault 
    cloudName: ""                             # [OPTIONAL for Azure] if not provided, azure environment will default to AzurePublicCloud
    objects:  |
      array:
        - |
          objectName: secret1                 # [REQUIRED] object name
                                              #     az keyvault secret list --vault-name “contosoKeyVault5”
                                              #     the above command will display a list of secret names from your Key Vault
          objectType: secret                  # [REQUIRED] object types: secret, key or cert
          objectVersion: ""                   # [OPTIONAL] object versions, default to latest if empty
        - |
          objectName: secret2
          objectType: secret
          objectVersion: ""
    resourceGroup: "contosoResourceGroup"     # [REQUIRED] the resource group name of the Key Vault
    subscriptionId: "subscriptionID"          # [REQUIRED] the subscription ID of the Key Vault
    tenantId: "tenantID"                      # [REQUIRED] the tenant ID of the Key Vault
```
Aşağıda, ilgili vurgulanan meta verilerle "az keykasa Show--Name contosoKeyVault5" için konsol çıktısı verilmiştir:

![Görüntü](../media/kubernetes-key-vault-4.png)

## <a name="assign-your-service-principal-or-use-managed-identities"></a>Hizmet sorumlunuzu atayın veya yönetilen kimlikler kullanın

### <a name="using-service-principal"></a>Hizmet sorumlusu kullanma

Hizmet sorumlusu kullanılıyorsa. Key Vault erişmek ve gizli dizileri almak için hizmet sorumlusuna izin vermeniz gerekir. Aşağıdaki adımları tamamlayarak **"okuyucu"** rolünü atayın ve hizmet sorumlusu için Key Vault gizli dizileri **"alın** " iznini verin:

1. Mevcut Key Vault hizmet sorumlusu atayın. **$AZURE _CLIENT_ID** parametresi, hizmet sorumlunuzu oluşturduktan sonra kopyaladığınız **uygulama** uygulamadır.
    ```azurecli
    az role assignment create --role Reader --assignee $AZURE_CLIENT_ID --scope /subscriptions/$SUBID/resourcegroups/$KEYVAULT_RESOURCE_GROUP/providers/Microsoft.KeyVault/vaults/$KEYVAULT_NAME
    ```

    Komutun çıktısı aşağıda verilmiştir: 

    ![Görüntü](../media/kubernetes-key-vault-5.png)

1. Gizli dizileri almak için hizmet sorumlusu izni verin:
    ```azurecli
    az keyvault set-policy -n $KEYVAULT_NAME --secret-permissions get --spn $AZURE_CLIENT_ID
    ```

1. Artık hizmet Sorumlunuzu Key Vault gizli dizileri okuma izni verecek şekilde yapılandırdığınıza göre. **$AZURE _CLIENT_SECRET** , hizmet sorumlunun **parolasıdır** . Hizmet sorumlusu kimlik bilgilerinizi gizli dizi olarak gizli bir Kubernetes olarak ekleyin:
    ```azurecli
    kubectl create secret generic secrets-store-creds --from-literal clientid=$AZURE_CLIENT_ID --from-literal clientsecret=$AZURE_CLIENT_SECRET
    ```

**Note:** Daha sonra, Kubernetes Pod 'u geçersiz bir Istemci gizli KIMLIĞI hakkında dağıtmada hata alırsanız. Zaman aşımına uğradı veya sıfırlandığında, daha eski bir Istemci gizli KIMLIĞINIZ olabilir. Bu sorunu çözmek için gizli dizilerinizi "gizlilikler-Store-creds" olarak silin ve geçerli Istemci parolası KIMLIĞIYLE yeni bir tane oluşturun. "Gizlilikler-Store-creds" dosyanızı silmek için aşağıdaki komutu çalıştırın:
```azurecli
kubectl delete secrets secrets-store-creds
```

Hizmet sorumlusunun Istemci gizli KIMLIĞINI unuttuysanız, aşağıdaki komutu kullanarak sıfırlayabilirsiniz:

```azurecli
az ad sp credential reset --name contosoServicePrincipal --credential-description "APClientSecret" --query password -o tsv
```

### <a name="using-managed-identities"></a>Yönetilen kimlikleri kullanma

Yönetilen kimlikler kullanılıyorsa, oluşturduğunuz AKS kümesine belirli roller atayın. 
1. Kullanıcı tarafından atanan yönetilen kimlik oluşturmak/listelemek/okumak için, AKS kümenizin [yönetilen kimlik katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolüne atanması gerekir. **$ClientID** Kubernetes kümesinin olduğundan emin olun.

    ```azurecli
    az role assignment create --role "Managed Identity Contributor" --assignee $clientId --scope /subscriptions/$SUBID/resourcegroups/$NODE_RESOURCE_GROUP
    
    az role assignment create --role "Virtual Machine Contributor" --assignee $clientId --scope /subscriptions/$SUBID/resourcegroups/$NODE_RESOURCE_GROUP
    ```

1. Azure Active Directory (AAD) kimliğini AKS 'e yükler.
    ```azurecli
    helm repo add aad-pod-identity https://raw.githubusercontent.com/Azure/aad-pod-identity/master/charts

    helm install pod-identity aad-pod-identity/aad-pod-identity
    ```

1. AAD kimliği oluşturma. **ClientID** ve **PrincipalId**değerini kopyalayın.
    ```azurecli
    az identity create -g $resourceGroupName -n $identityName
    ```

1. Key Vault için yeni oluşturduğunuz AAD kimliğine okuyucu rolünü atayın. Daha sonra Key Vault, kimlik bilgilerinizi, parolaları almak için izin verin. Yeni oluşturduğunuz Azure kimliğinden **ClientID** ve **PrincipalId** ' i kullanacaksınız.
    ```azurecli
    az role assignment create --role "Reader" --assignee $principalId --scope /subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/contosoResourceGroup/providers/Microsoft.KeyVault/vaults/contosoKeyVault5

    az keyvault set-policy -n contosoKeyVault5 --secret-permissions get --spn $clientId
    ```

## <a name="deploy-your-pod-with-mounted-secrets-from-key-vault"></a>Pod 'nizi Key Vault bağlı gizli dosyalarla dağıtın

Aşağıdaki komut, SecretProviderClass nesnenizin yapılandırmasını sağlayacaktır:
```azurecli
kubectl apply -f secretProviderClass.yaml
```

### <a name="using-service-principal"></a>Hizmet sorumlusu kullanma

Hizmet sorumlusu kullanılıyorsa. Aşağıdaki komut, Kubernetes Pod 'nizi secretproviderclass ve yapılandırdığınız gizli dizileri-Store-creds ile dağıtır. [Linux](https://github.com/Azure/secrets-store-csi-driver-provider-azure/blob/master/examples/nginx-pod-secrets-store-inline-volume-secretproviderclass.yaml) ve [Windows](https://github.com/Azure/secrets-store-csi-driver-provider-azure/blob/master/examples/windows-pod-secrets-store-inline-volume-secret-providerclass.yaml) dağıtımı için şablon aşağıda verilmiştir.
```azurecli
kubectl apply -f updateDeployment.yaml
```

### <a name="using-managed-identities"></a>Yönetilen kimlikleri kullanma

Yönetilen kimlikler kullanılıyorsa. Daha önce oluşturduğunuz kimliğe başvuracak, kümenizde bir **AzureIdentity** oluşturacaksınız. Ardından, oluşturduğunuz **AzureIdentity** 'e başvuracak bir **AzureIdentityBinding** oluşturun. Aşağıdaki şablonu kullanın, karşılık gelen parametreleri doldurun ve **Podidentityandbinding. YAML**olarak kaydedin.  
```yml
apiVersion: aadpodidentity.k8s.io/v1
kind: AzureIdentity
metadata:
    name: "azureIdentityName"               # The name of your Azure Idenity
spec:
    type: 0                                 # Set type: 0 for Managed Service Identity
    resourceID: /subscriptions/<SUBID>/resourcegroups/<RESOURCEGROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<AZUREIDENTITYNAME>
    clientID: "managedIdentityClientId"     # The clientId of your Azure Identity that you created earlier
---
apiVersion: aadpodidentity.k8s.io/v1
kind: AzureIdentityBinding
metadata:
    name: azure-pod-identity-binding
spec:
    azureIdentity: "azureIdentityName"      # The name of your Azure Idenity
    selector: azure-pod-identity-binding-selector
```
    
Bağlamayı yürütmek için aşağıdaki komutu çalıştırın:

```azurecli
kubectl apply -f podIdentityAndBinding.yaml
```

Daha sonra Pod 'un gerçek dağıtımı. Aşağıda, son adımdan Pod kimlik bağlamasını kullanacak olan dağıtım YAML dosyası verilmiştir. Bu dosyayı **Pod Bindingdeployment. YAML**olarak kaydedin.

```yml
kind: Pod
apiVersion: v1
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
### <a name="check-status-and-secret-content"></a>Durumu ve gizli içeriği denetleyin 
Dağıttığınız Pod 'leri göstermek için:
```azurecli
kubectl get pods
```

Pod uygulamanızın durumunu denetlemek için şu komutu kullanın:
```azurecli
kubectl describe pod/nginx-secrets-store-inline
```

![Görüntü](../media/kubernetes-key-vault-6.png)

Dağıtılan Pod, "çalışıyor" durumunda olmalıdır. En alttaki "olaylar" bölümünde, sol taraftaki tüm olay türleri "normal" olarak sınıflandırılır.
Pod 'un çalıştığını doğruladıktan sonra, Pod 'larınızın Key Vault gizli dizileri olduğunu doğrulayabilirsiniz.

Pod 'un sahip olduğu tüm gizli dizileri göstermek için:
```azurecli
kubectl exec -it nginx-secrets-store-inline -- ls /mnt/secrets-store/
```

Belirli bir gizli dizi içeriğini almak için:
```azurecli
kubectl exec -it nginx-secrets-store-inline -- cat /mnt/secrets-store/secret1
```

Görünen gizli dizinin içeriğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Key Vault kurtarılabilir olduğundan emin olun:
> [!div class="nextstepaction"]
> [Geçici silme özelliğini aç](https://docs.microsoft.com/azure/key-vault/general/soft-delete-clid)
