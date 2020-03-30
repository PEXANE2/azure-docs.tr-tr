---
title: Yeni bir Uygulama Ağ Geçidi ile giriş denetleyicisi oluşturma
description: Bu makalede, yeni bir Uygulama Ağ Geçidi ile bir Uygulama Ağ Geçidi Giriş Denetleyicisi nasıl dağıtılanın hakkında bilgi sağlar.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: a0bc6aef1becd53217be0eeb8c865b5c78a5d69f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80239458"
---
# <a name="how-to-install-an-application-gateway-ingress-controller-agic-using-a-new-application-gateway"></a>Yeni Bir Uygulama Ağ Geçidi Kullanarak Uygulama Ağ Geçidi Denetleyicisi (AGIC) Nasıl Yüklenir?

Aşağıdaki yönergelerde Uygulama Ağ Geçidi Giriş Denetleyicisi (AGIC) önceden varolan bileşenleri olmayan bir ortamda yüklenir.

## <a name="required-command-line-tools"></a>Gerekli Komut Satırı Araçları

Aşağıdaki tüm komut satırı işlemleri için [Azure Cloud Shell'in](https://shell.azure.com/) kullanılmasını öneririz. Shell.azure.com'dan veya bağlantıyı tıklatarak kabuğunuzu başlatın:

[![Gömme başlatma](https://shell.azure.com/images/launchcloudshell.png "Azure Cloud Shell'i başlatma")](https://shell.azure.com)

Alternatif olarak, aşağıdaki simgeyi kullanarak Azure portalından Cloud Shell'i başlatın:

![Portal lansmanı](./media/application-gateway-ingress-controller-install-new/portal-launch-icon.png)

[Azure Bulut Shell'iniz](https://shell.azure.com/) zaten gerekli tüm araçlara sahiptir. Başka bir ortam kullanmayı seçerseniz, lütfen aşağıdaki komut satırı araçlarının yüklendiğinden emin olun:

* `az`- Azure CLI: [yükleme talimatları](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
* `kubectl`- Kubernetes komut satırı aracı: [kurulum talimatları](https://kubernetes.io/docs/tasks/tools/install-kubectl)
* `helm`- Kubernetes paket yöneticisi: [kurulum talimatları](https://github.com/helm/helm/releases/latest)
* `jq`- komut satırı JSON işlemci: [kurulum talimatları](https://stedolan.github.io/jq/download/)


## <a name="create-an-identity"></a>Kimlik Oluşturma

Azure Etkin Dizin (AAD) [hizmet ana nesnesi](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals#service-principal-object)oluşturmak için aşağıdaki adımları izleyin. Lütfen , `appId` `password`ve `objectId` değerleri kaydedin - bunlar aşağıdaki adımlarda kullanılacaktır.

1. AD hizmet sorumlusu oluşturma[(RBAC hakkında daha fazla bilgi edinin):](https://docs.microsoft.com/azure/role-based-access-control/overview)
    ```azurecli
    az ad sp create-for-rbac --skip-assignment -o json > auth.json
    appId=$(jq -r ".appId" auth.json)
    password=$(jq -r ".password" auth.json)
    ```
    JSON çıktısından alınan `appId` ve `password` değerler aşağıdaki adımlarda kullanılacaktır


1. Yeni `appId` hizmet ilkesini almak için önceki `objectId` komutun çıktısını kullanın:
    ```azurecli
    objectId=$(az ad sp show --id $appId --query "objectId" -o tsv)
    ```
    Bu komutun çıktısı `objectId`, aşağıdaki Azure Kaynak Yöneticisi şablonunda kullanılacaktır

1. Azure Kaynak Yöneticisi şablon dağıtımında daha sonra kullanılacak parametre dosyasını oluşturun.
    ```bash
    cat <<EOF > parameters.json
    {
      "aksServicePrincipalAppId": { "value": "$appId" },
      "aksServicePrincipalClientSecret": { "value": "$password" },
      "aksServicePrincipalObjectId": { "value": "$objectId" },
      "aksEnableRBAC": { "value": false }
    }
    EOF
    ```
    **RBAC** etkin bir küme dağıtmak `aksEnabledRBAC` için, alanı`true`

## <a name="deploy-components"></a>Bileşenleri Dağıt
Bu adım, aboneliğinize aşağıdaki bileşenleri ekler:

- [Azure Kubernetes Servisi](https://docs.microsoft.com/azure/aks/intro-kubernetes)
- [Uygulama Ağ Geçidi](https://docs.microsoft.com/azure/application-gateway/overview) v2
- 2 alt [ağlı](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) [Sanal Ağ](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)
- [Genel IP Adresi](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address)
- [AAD Pod Kimliği](https://github.com/Azure/aad-pod-identity/blob/master/README.md) tarafından kullanılacak [yönetilen kimlik](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)

1. Azure Kaynak Yöneticisi şablonundan indirin ve şablonu gerektiği gibi değiştirin.
    ```bash
    wget https://raw.githubusercontent.com/Azure/application-gateway-kubernetes-ingress/master/deploy/azuredeploy.json -O template.json
    ```

1. Azure Kaynak Yöneticisi şablonuna 'yı kullanarak `az cli`dağıtın. Bu işlem 5 dakika kadar sürebilir.
    ```azurecli
    resourceGroupName="MyResourceGroup"
    location="westus2"
    deploymentName="ingress-appgw"

    # create a resource group
    az group create -n $resourceGroupName -l $location

    # modify the template as needed
    az group deployment create \
            -g $resourceGroupName \
            -n $deploymentName \
            --template-file template.json \
            --parameters parameters.json
    ```

1. Dağıtım tamamlandıktan sonra, dağıtım çıktısını `deployment-outputs.json`' adlı bir dosyaya indirin.
    ```azurecli
    az group deployment show -g $resourceGroupName -n $deploymentName --query "properties.outputs" -o json > deployment-outputs.json
    ```

## <a name="set-up-application-gateway-ingress-controller"></a>Uygulama Ağ Geçidi Giriş Denetleyicisi'ni ayarlama

Önceki bölümdeki talimatlarla, yeni bir AKS kümesi ve Uygulama Ağ Geçidi oluşturduk ve yapılandırıldık. Yeni Kubernetes altyapımıza örnek bir uygulama ve giriş denetleyicisi dağıtmaya hazırız.

### <a name="setup-kubernetes-credentials"></a>Kurulum Kubernetes Kimlik Bilgileri
Aşağıdaki adımlar için, yeni Kubernetes kümemize bağlanmak için kullanacağımız [kubectl](https://kubectl.docs.kubernetes.io/) komutunu kurmamız gerekir. [Cloud](https://shell.azure.com/) Shell `kubectl` zaten yüklendi. Kubernetes için kimlik bilgileri almak için CLI'yi kullanacağız. `az`

Yeni dağıtılan AKS için kimlik bilgileri alın ([devamı:](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough#connect-to-the-cluster)
```azurecli
# use the deployment-outputs.json created after deployment to get the cluster name and resource group name
aksClusterName=$(jq -r ".aksClusterName.value" deployment-outputs.json)
resourceGroupName=$(jq -r ".resourceGroupName.value" deployment-outputs.json)

az aks get-credentials --resource-group $resourceGroupName --name $aksClusterName
```

### <a name="install-aad-pod-identity"></a>AAD Pod Kimliğini Yükleme
  Azure Etkin Dizin Bölmesi Bölmesi Kimliği, [Azure Kaynak Yöneticisi'ne (ARM)](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)belirteç tabanlı erişim sağlar.

  [AAD Pod Identity,](https://github.com/Azure/aad-pod-identity) Kubernetes kümenize aşağıdaki bileşenleri ekler:
   * Kubernetes [CRDs](https://kubernetes.io/docs/tasks/access-kubernetes-api/custom-resources/custom-resource-definitions/): `AzureIdentity`, `AzureAssignedIdentity`,`AzureIdentityBinding`
   * [Yönetilen Kimlik Denetleyicisi (MIC)](https://github.com/Azure/aad-pod-identity#managed-identity-controllermic) bileşeni
   * [Düğüm Yönetilen Kimlik (NMI)](https://github.com/Azure/aad-pod-identity#node-managed-identitynmi) bileşeni


Kümenize AAD Pod Kimliğini yüklemek için:

   - *RBAC etkin* AKS kümesi

     ```bash
     kubectl create -f https://raw.githubusercontent.com/Azure/aad-pod-identity/master/deploy/infra/deployment-rbac.yaml
     ```

   - *RBAC devre dışı* AKS kümesi

     ```bash
     kubectl create -f https://raw.githubusercontent.com/Azure/aad-pod-identity/master/deploy/infra/deployment.yaml
     ```

### <a name="install-helm"></a>Miğferi Yükle
[Helm,](https://docs.microsoft.com/azure/aks/kubernetes-helm) Kubernetes'in paket yöneticisi. `application-gateway-kubernetes-ingress` Paketi yüklemek için aşağıdakilerden yararlanacağız:

1. [Helm'i](https://docs.microsoft.com/azure/aks/kubernetes-helm) yükleyin ve `application-gateway-kubernetes-ingress` dümen paketi eklemek için aşağıdakileri çalıştırın:

    - *RBAC etkin* AKS kümesi

        ```bash
        kubectl create serviceaccount --namespace kube-system tiller-sa
        kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller-sa
        helm init --tiller-namespace kube-system --service-account tiller-sa
        ```

    - *RBAC devre dışı* AKS kümesi

        ```bash
        helm init
        ```

1. AGIC Miğfer deposunu ekleyin:
    ```bash
    helm repo add application-gateway-kubernetes-ingress https://appgwingress.blob.core.windows.net/ingress-azure-helm-package/
    helm repo update
    ```

### <a name="install-ingress-controller-helm-chart"></a>Ingress Controller Dümen Grafiği ni yükleyin

1. Yukarıda `deployment-outputs.json` oluşturulan dosyayı kullanın ve aşağıdaki değişkenleri oluşturun.
    ```bash
    applicationGatewayName=$(jq -r ".applicationGatewayName.value" deployment-outputs.json)
    resourceGroupName=$(jq -r ".resourceGroupName.value" deployment-outputs.json)
    subscriptionId=$(jq -r ".subscriptionId.value" deployment-outputs.json)
    identityClientId=$(jq -r ".identityClientId.value" deployment-outputs.json)
    identityResourceId=$(jq -r ".identityResourceId.value" deployment-outputs.json)
    ```
1. AGIC'i yapılandıracak helm-config.yaml'yi indirin:
    ```bash
    wget https://raw.githubusercontent.com/Azure/application-gateway-kubernetes-ingress/master/docs/examples/sample-helm-config.yaml -O helm-config.yaml
    ```
    Veya aşağıdaki YAML dosyasını kopyalayın: 
    
    ```yaml
    # This file contains the essential configs for the ingress controller helm chart

    # Verbosity level of the App Gateway Ingress Controller
    verbosityLevel: 3
    
    ################################################################################
    # Specify which application gateway the ingress controller will manage
    #
    appgw:
        subscriptionId: <subscriptionId>
        resourceGroup: <resourceGroupName>
        name: <applicationGatewayName>
    
        # Setting appgw.shared to "true" will create an AzureIngressProhibitedTarget CRD.
        # This prohibits AGIC from applying config for any host/path.
        # Use "kubectl get AzureIngressProhibitedTargets" to view and change this.
        shared: false
    
    ################################################################################
    # Specify which kubernetes namespace the ingress controller will watch
    # Default value is "default"
    # Leaving this variable out or setting it to blank or empty string would
    # result in Ingress Controller observing all acessible namespaces.
    #
    # kubernetes:
    #   watchNamespace: <namespace>
    
    ################################################################################
    # Specify the authentication with Azure Resource Manager
    #
    # Two authentication methods are available:
    # - Option 1: AAD-Pod-Identity (https://github.com/Azure/aad-pod-identity)
    armAuth:
        type: aadPodIdentity
        identityResourceID: <identityResourceId>
        identityClientID:  <identityClientId>
    
    ## Alternatively you can use Service Principal credentials
    # armAuth:
    #    type: servicePrincipal
    #    secretJSON: <<Generate this value with: "az ad sp create-for-rbac --subscription <subscription-uuid> --sdk-auth | base64 -w0" >>
    
    ################################################################################
    # Specify if the cluster is RBAC enabled or not
    rbac:
        enabled: false # true/false
    
    # Specify aks cluster related information. THIS IS BEING DEPRECATED.
    aksClusterConfiguration:
        apiServerAddress: <aks-api-server-address>
    ```

1. Yeni indirilen helm-config.yaml'ı edin ve `appgw` `armAuth`bölümleri doldurun ve .
    ```bash
    sed -i "s|<subscriptionId>|${subscriptionId}|g" helm-config.yaml
    sed -i "s|<resourceGroupName>|${resourceGroupName}|g" helm-config.yaml
    sed -i "s|<applicationGatewayName>|${applicationGatewayName}|g" helm-config.yaml
    sed -i "s|<identityResourceId>|${identityResourceId}|g" helm-config.yaml
    sed -i "s|<identityClientId>|${identityClientId}|g" helm-config.yaml

    # You can further modify the helm config to enable/disable features
    nano helm-config.yaml
    ```

   Değerler:
     - `verbosityLevel`: AGIC günlük altyapısının özkaynak düzeyini ayarlar. Olası değerler için [Günlük Düzeyleri'ne](https://github.com/Azure/application-gateway-kubernetes-ingress/blob/463a87213bbc3106af6fce0f4023477216d2ad78/docs/troubleshooting.md#logging-levels) bakın.
     - `appgw.subscriptionId`: Uygulama Ağ Geçidi'nin bulunduğu Azure Abonelik Kimliği. Örnek: `a123b234-a3b4-557d-b2df-a0bc12de1234`
     - `appgw.resourceGroup`: Uygulama Ağ Geçidi'nin oluşturulduğu Azure Kaynak Grubu'nun adı. Örnek: `app-gw-resource-group`
     - `appgw.name`: Uygulama Ağ Geçidinin Adı. Örnek: `applicationgatewayd0f0`
     - `appgw.shared`: Bu boolean bayrak varsayılan `false`olmalıdır . Paylaşılan `true` Uygulama Ağ [Geçidine](https://github.com/Azure/application-gateway-kubernetes-ingress/blob/072626cb4e37f7b7a1b0c4578c38d1eadc3e8701/docs/setup/install-existing.md#multi-cluster--shared-app-gateway)ihtiyacınız olması için ayarlayın.
     - `kubernetes.watchNamespace`: AGIC'in izlemesi gereken isim alanını belirtin. Bu, tek bir dize değeri veya virgülle ayrılmış ad alanları listesi olabilir.
    - `armAuth.type`: olabilir `aadPodIdentity` veya`servicePrincipal`
    - `armAuth.identityResourceID`: Azure Yönetilen Kimliğin Kaynak Kimliği
    - `armAuth.identityClientId`: Kimliğin Müşteri Kimliği. Kimlik hakkında daha fazla bilgi için aşağıya bakın
    - `armAuth.secretJSON`: Yalnızca Servis Müdürü Gizli türü `armAuth.type` seçildiğinde `servicePrincipal`(ayarlandığında) gereklidir 


   > [!NOTE]
   > Kimlik `identityResourceID` `identityClientID` [Oluşturma](https://github.com/Azure/application-gateway-kubernetes-ingress/blob/072626cb4e37f7b7a1b0c4578c38d1eadc3e8701/docs/setup/install-new.md#create-an-identity) adımları sırasında oluşturulan ve aşağıdaki komut kullanılarak yeniden elde edilebilen değerler şunlardır:
   > ```azurecli
   > az identity show -g <resource-group> -n <identity-name>
   > ```
   > `<resource-group>`yukarıdaki komutta Uygulama Ağ Geçidi'nin kaynak grubu yer alır. `<identity-name>`oluşturulan kimliğin adıdır. Belirli bir abonelik için tüm kimlikler şu şekilde listelenebilir:`az identity list`


1. Uygulama Ağ Geçidi giriş denetleyici paketini yükleyin:

    ```bash
    helm install -f helm-config.yaml application-gateway-kubernetes-ingress/ingress-azure
    ```

## <a name="install-a-sample-app"></a>Örnek Bir Uygulama Yükleme
Artık Application Gateway, AKS ve AGIC yüklü olduğumuza göre [Azure Cloud Shell](https://shell.azure.com/)üzerinden örnek bir uygulama yükleyebiliriz:

```yaml
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: aspnetapp
  labels:
    app: aspnetapp
spec:
  containers:
  - image: "mcr.microsoft.com/dotnet/core/samples:aspnetapp"
    name: aspnetapp-image
    ports:
    - containerPort: 80
      protocol: TCP

---

apiVersion: v1
kind: Service
metadata:
  name: aspnetapp
spec:
  selector:
    app: aspnetapp
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80

---

apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: aspnetapp
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
spec:
  rules:
  - http:
      paths:
      - path: /
        backend:
          serviceName: aspnetapp
          servicePort: 80
EOF
```

Alternatif olarak şunları yapabilirsiniz:

* Yukarıdaki YAML dosyasını indirin:

```bash
curl https://raw.githubusercontent.com/Azure/application-gateway-kubernetes-ingress/master/docs/examples/aspnetapp.yaml -o aspnetapp.yaml
```

* YAML dosyasını uygulayın:

```bash
kubectl apply -f aspnetapp.yaml
```


## <a name="other-examples"></a>Diğer Örnekler
Bu [nasıl yapılsın kılavuzu,](ingress-controller-expose-service-over-http-https.md) bir AKS hizmetini HTTP veya HTTPS üzerinden Uygulama Ağ Geçidi ile Internet'e nasıl açıkhale bırakılabilmek le ilgili daha fazla örnek içerir.
