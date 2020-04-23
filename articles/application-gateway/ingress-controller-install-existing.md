---
title: Varolan bir Uygulama Ağ Geçidi olan bir giriş denetleyicisi oluşturma
description: Bu makalede, varolan bir Uygulama Ağ Geçidi ile bir Uygulama Ağ Geçidi Giriş Denetleyicisi nasıl dağıtılanın hakkında bilgi sağlar.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 949f1b3ee3db72e1c541c3dd4c5f74f364f1b514
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81869888"
---
# <a name="install-an-application-gateway-ingress-controller-agic-using-an-existing-application-gateway"></a>Varolan bir Uygulama Ağ Geçidi ni kullanarak Bir Uygulama Ağ Geçidi Giriş Denetleyicisi (AGIC) yükleme

Uygulama Ağ Geçidi Denetleyicisi (AGIC), Kubernetes kümenizdeki bir bölmedir.
AGIC, Kubernetes [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) kaynaklarını izler ve Kubernetes kümesinin durumuna göre Uygulama Ağ Geçidi config'i oluşturur ve uygular.

## <a name="outline"></a>Anahat:
- [Önkoşullar](#prerequisites)
- [Azure Kaynak Yöneticisi Kimlik Doğrulaması (ARM)](#azure-resource-manager-authentication)
    - Seçenek 1: [Aad-pod kimliğini ayarlama](#set-up-aad-pod-identity) ve ARM'larda Azure Kimliği oluşturma
    - Seçenek 2: [Hizmet Sorumlusu Kullanma](#using-a-service-principal)
- [Miğferkullanarak Giriş Denetleyicisi Yükleme](#install-ingress-controller-as-a-helm-chart)
- [Çoklu küme / Paylaşılan Uygulama Ağ Geçidi](#multi-cluster--shared-application-gateway): Agic'i, Uygulama Ağ Geçidi'nin bir veya daha fazla AKS kümeleri ve/veya diğer Azure bileşenleri arasında paylaşıldığı bir ortama yükleyin.

## <a name="prerequisites"></a>Önkoşullar
Bu belge, aşağıdaki araçları ve altyapıyı zaten yüklü olarak sahip olduğunuzu varsayar:
- [Gelişmiş Ağ](https://docs.microsoft.com/azure/aks/configure-azure-cni) etkin [AKS](https://azure.microsoft.com/services/kubernetes-service/)
- [AKS](https://docs.microsoft.com/azure/application-gateway/create-zone-redundant) ile aynı sanal ağda Uygulama Ağ Geçidi v2
- [AKS](https://github.com/Azure/aad-pod-identity) kümenizde yüklü AAD Pod Kimliği
- [Cloud Shell,](https://shell.azure.com/) CLI'ye `az` `kubectl`sahip ve `helm` yüklü olan Azure kabuk ortamıdır. Bu araçlar aşağıdaki komutlar için gereklidir.

LÜTFEN AGIC'i yüklemeden önce __Application Gateway'inizin yapılandırmasına destek__ verin:
  1. [Azure portalını](https://portal.azure.com/) kullanma `Application Gateway` örneğinize gidin
  2. düğmesine `Export template` tıklayın`Download`

İndirdiğiniz zip dosyası, gerekli olması halinde App Gateway'i geri yüklemek için kullanabileceğiniz JSON şablonları, bash ve PowerShell komut dosyalarına sahip olacak

## <a name="install-helm"></a>Miğferi Yükle
[Helm,](https://docs.microsoft.com/azure/aks/kubernetes-helm) Kubernetes'in paket yöneticisi. `application-gateway-kubernetes-ingress` Paketi yüklemek için onu kullanacağız.
Helm'i yüklemek için [Bulut Kabuğu'nu](https://shell.azure.com/) kullanın:

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

## <a name="azure-resource-manager-authentication"></a>Azure Kaynak Yöneticisi Kimlik Doğrulaması

AGIC, Kubernetes API sunucusu ve Azure Kaynak Yöneticisi ile iletişim kurar. Bu API'lere erişmek için bir kimlik gerekir.

## <a name="set-up-aad-pod-identity"></a>AAD Pod Kimliğini Ayarlama

[AAD Pod Identity,](https://github.com/Azure/aad-pod-identity) AKS'nizde de çalışan AGIC'e benzer bir denetleyicidir. Azure Active Directory kimliklerini Kubernetes bölmelerinize bağlar. Kubernetes bölmesindeki bir uygulamanın diğer Azure bileşenleriyle iletişim kurabilmesi için kimlik gereklidir. Burada özel durumda, [ARM'a](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)HTTP isteklerini yapmak için AGIC bölmesi için yetkilendirmeye ihtiyacımız var.

Bu bileşeni AKS'nize eklemek için [AAD Pod Identity yükleme yönergelerini](https://github.com/Azure/aad-pod-identity#deploy-the-azure-aad-identity-infra) izleyin.

Daha sonra bir Azure kimliği oluşturmamız ve ona ARM'a izin vermemiz gerekir.
Aşağıdaki komutların tümlerini çalıştırmak ve bir kimlik oluşturmak için [Bulut Kabuğu'nu](https://shell.azure.com/) kullanın:

1. **AKS düğümleri ile aynı kaynak grubunda**bir Azure kimliği oluşturun. Doğru kaynak grubunu seçmek önemlidir. Aşağıdaki komutta gerekli kaynak grubu AKS portal bölmesinde başvurulan bir *grup değildir.* Bu, sanal makinelerin `aks-agentpool` kaynak grubudur. Genellikle bu kaynak grubu `MC_` AKS ile başlar ve aks adınızı içerir. Örneğin:`MC_resourceGroup_aksABCD_westus`

    ```azurecli
    az identity create -g <agent-pool-resource-group> -n <identity-name>
    ```

1. Aşağıdaki rol atama komutları için `principalId` yeni oluşturulan kimlik için edinmemiz gerekir:

    ```azurecli
    az identity show -g <resourcegroup> -n <identity-name>
    ```

1. Uygulama Ağ `Contributor` Geçidinize kimlik erişimi verin. Bunun için Uygulama Ağ Geçidi kimliğine ihtiyacınız var, bu da şuna benzer:`/subscriptions/A/resourceGroups/B/providers/Microsoft.Network/applicationGateways/C`

    Aboneliğinizdeki Uygulama Ağ Geçidi işlerinin listesini:`az network application-gateway list --query '[].id'`

    ```azurecli
    az role assignment create \
        --role Contributor \
        --assignee <principalId> \
        --scope <App-Gateway-ID>
    ```

1. Uygulama Ağ `Reader` Geçidi kaynak grubuna kimlik erişimi verin. Kaynak grubu kimliği aşağıdaki `/subscriptions/A/resourceGroups/B`gibi görünür: . Tüm kaynak gruplarını şu kaynaklarla edinebilirsiniz:`az group list --query '[].id'`

    ```azurecli
    az role assignment create \
        --role Reader \
        --assignee <principalId> \
        --scope <App-Gateway-Resource-Group-ID>
    ```

## <a name="using-a-service-principal"></a>Hizmet Sorumlusu Kullanma
Bir Kubernetes sırrı ile ARM'a AGIC erişimi sağlamak da mümkündür.

1. Active Directory Service Principal oluşturun ve base64 ile kodlayın. Temel64 kodlama JSON blob Kubernetes kaydedilmesi için gereklidir.

```azurecli
az ad sp create-for-rbac --sdk-auth | base64 -w0
```

2. `helm-config.yaml` Dosyaya base64 kodlanmış JSON blob ekleyin. Daha `helm-config.yaml` fazla bilgi sonraki bölümde yer almaktadır.
```yaml
armAuth:
    type: servicePrincipal
    secretJSON: <Base64-Encoded-Credentials>
```

## <a name="install-ingress-controller-as-a-helm-chart"></a>Giriş Denetleyiciyi Miğfer Grafiğine Yükleme
İlk birkaç adımda, Kubernetes kümenize Helm's Tiller'ı yerleştirdik. AGIC Miğfer paketini yüklemek için [Cloud Shell'i](https://shell.azure.com/) kullanın:

1. Dümen `application-gateway-kubernetes-ingress` reposu ekleyin ve bir dümen güncellemesi gerçekleştirin

    ```bash
    helm repo add application-gateway-kubernetes-ingress https://appgwingress.blob.core.windows.net/ingress-azure-helm-package/
    helm repo update
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
    #    secretJSON: <<Generate this value with: "az ad sp create-for-rbac --sdk-auth | base64 -w0" >>
    
    ################################################################################
    # Specify if the cluster is RBAC enabled or not
    rbac:
        enabled: false # true/false
    
    # Specify aks cluster related information. THIS IS BEING DEPRECATED.
    aksClusterConfiguration:
        apiServerAddress: <aks-api-server-address>
    ```

1. Helm-config.yaml'ı edin `appgw` ve değerleri `armAuth`doldurun ve .
    ```bash
    nano helm-config.yaml
    ```

    > [!NOTE] 
    > Önceki `<identity-resource-id>` `<identity-client-id>` bölümde kurettiğiniz Azure AD Kimliği'nin özellikleri dir. Aşağıdaki komutu çalıştırarak bu bilgileri `az identity show -g <resourcegroup> -n <identity-name>`alabilirsiniz: , üst düzey AKS küme nesnesi, Uygulama Ağ Geçidi ve Yönetilen Tanım dağıtılır kaynak grubu nerede. `<resourcegroup>`

1. Önceki adımdaki `helm-config.yaml` yapılandırmayla Miğfer grafiğini `application-gateway-kubernetes-ingress` yükleme

    ```bash
    helm install -f <helm-config.yaml> application-gateway-kubernetes-ingress/ingress-azure
    ```

    Alternatif olarak, ve `helm-config.yaml` Helm komutunu tek bir adımda birleştirebilirsiniz:
    ```bash
    helm install ./helm/ingress-azure \
         --name ingress-azure \
         --namespace default \
         --debug \
         --set appgw.name=applicationgatewayABCD \
         --set appgw.resourceGroup=your-resource-group \
         --set appgw.subscriptionId=subscription-uuid \
         --set appgw.shared=false \
         --set armAuth.type=servicePrincipal \
         --set armAuth.secretJSON=$(az ad sp create-for-rbac --sdk-auth | base64 -w0) \
         --set rbac.enabled=true \
         --set verbosityLevel=3 \
         --set kubernetes.watchNamespace=default \
         --set aksClusterConfiguration.apiServerAddress=aks-abcdefg.hcp.westus2.azmk8s.io
    ```

1. Düzgün başlatıp başlatılmadın doğrulamak için yeni oluşturulan bölmenin günlüğünü denetleyin

Bir AKS hizmetini HTTP veya HTTPS üzerinden, bir Azure Uygulama Ağ Geçidi kullanarak internete nasıl maruz bırakabileceğinizi anlamak için [bu nasıl yapılabilir kılavuzuna](ingress-controller-expose-service-over-http-https.md) bakın.



## <a name="multi-cluster--shared-application-gateway"></a>Çok kümeli / Paylaşılan Uygulama Ağ Geçidi
Varsayılan olarak AGIC, bağlı olduğu Uygulama Ağ Geçidi'nin tam mülkiyetini üstlenir. AGIC sürümü 0.8.0 ve daha sonra diğer Azure bileşenleriile tek bir Uygulama Ağ Geçidi paylaşabilirsiniz. Örneğin, Sanal Makine Ölçeği Kümesi'nde barındırılan bir uygulama nın yanı sıra aks kümesi için de aynı Uygulama Ağ Geçidi'ni kullanabiliriz.

Bu ayarı etkinleştirmeden önce lütfen __Application Gateway yapılandırmanızı yedekleyin:__
  1. [Azure portalını](https://portal.azure.com/) kullanma `Application Gateway` örneğinize gidin
  2. düğmesine `Export template` tıklayın`Download`

İndirdiğiniz zip dosyasında Uygulama Ağ Geçidi'ni geri yüklemek için kullanabileceğiniz JSON şablonları, bash ve PowerShell komut dosyaları bulunacaktır

### <a name="example-scenario"></a>Örnek Senaryo
İki web sitesi için trafiği yöneten hayali bir Uygulama Ağ Geçidi'ne bakalım:
  - `dev.contoso.com`- Uygulama Ağ Geçidi ve AGIC kullanarak, yeni bir AKS barındırılan
  - `prod.contoso.com`- Azure [Sanal Makine Ölçeği Setinde](https://azure.microsoft.com/services/virtual-machine-scale-sets/) barındırılan

Varsayılan ayarlarla AGIC, işaret edilen Uygulama Ağ Geçidi'nin %100 sahipliğini varsayar. AGIC, App Gateway'in tüm yapılandırmasının üzerine yazar. Kubernetes Ingress'te tanımlamadan `prod.contoso.com` (Application Gateway'de) manuel olarak bir dinleyici oluşturursak, AGIC `prod.contoso.com` config'i saniyeler içinde siler.

AGIC'i kurmak `prod.contoso.com` ve Sanal Makine Ölçeği Seti makinelerimizden hizmet vermek için `dev.contoso.com` AGIC'i yalnızca yapılandırmaya kısıtlamamız gerekir. Bu, aşağıdaki [CRD'nin](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)anlık olarak kolaylaştırılması ile kolaylaştırılır:

```bash
cat <<EOF | kubectl apply -f -
apiVersion: "appgw.ingress.k8s.io/v1"
kind: AzureIngressProhibitedTarget
metadata:
  name: prod-contoso-com
spec:
  hostname: prod.contoso.com
EOF
```

Yukarıdaki komut bir `AzureIngressProhibitedTarget` nesne oluşturur. Bu, AGIC'i (sürüm 0.8.0 ve sonraki sürüm) `prod.contoso.com` Application Gateway config'in varlığından haberdar eder ve bu ana bilgisayar adı ile ilgili yapılandırmayı değiştirmemesi için açıkça talimat verir.


### <a name="enable-with-new-agic-installation"></a>Yeni AGIC kurulumu ile etkinleştirme
AGIC'i (sürüm 0.8.0 ve sonrası) Uygulama Ağ Geçidi `helm-config.yaml` yapılandırmasının bir alt kümesiyle sınırlamak için şablonu değiştirin.
`appgw:` Bölümün altında, `shared` anahtar ekleyin ve `true`'ye ayarlayın.

```yaml
appgw:
    subscriptionId: <subscriptionId>    # existing field
    resourceGroup: <resourceGroupName>  # existing field
    name: <applicationGatewayName>      # existing field
    shared: true                        # <<<<< Add this field to enable shared Application Gateway >>>>>
```

Miğfer değişikliklerini uygulayın:
  1. CRD'nin `AzureIngressProhibitedTarget` aşağıdakilerle yüklü olduğundan emin olun:
      ```bash
      kubectl apply -f https://raw.githubusercontent.com/Azure/application-gateway-kubernetes-ingress/ae695ef9bd05c8b708cedf6ff545595d0b7022dc/crds/AzureIngressProhibitedTarget.yaml
      ```
  2. Güncelleme Helm:
      ```bash
      helm upgrade \
          --recreate-pods \
          -f helm-config.yaml \
          ingress-azure application-gateway-kubernetes-ingress/ingress-azure
      ```

Sonuç olarak AKS `AzureIngressProhibitedTarget` adlı `prohibit-all-targets`yeni bir örnek olacaktır:
```bash
kubectl get AzureIngressProhibitedTargets prohibit-all-targets -o yaml
```

Adından `prohibit-all-targets`da anlaşılacağı gibi nesne, AGIC'in *herhangi bir* ana bilgisayar ve yol için config'i değiştirmesini yasaklar.
Miğfer `appgw.shared=true` yükleme agic dağıtacak, ancak Uygulama Ağ Geçidi herhangi bir değişiklik yapmaz.


### <a name="broaden-permissions"></a>İzinleri genişletme
Helm ile `appgw.shared=true` ve `prohibit-all-targets` varsayılan bloklar AGIC herhangi bir config uygulanmasını beri.

AGIC izinlerini şu şekilde genişletin:
1. Özel kurulumunuzla yeni `AzureIngressProhibitedTarget` bir kurulum oluşturun:
    ```bash
    cat <<EOF | kubectl apply -f -
    apiVersion: "appgw.ingress.k8s.io/v1"
    kind: AzureIngressProhibitedTarget
    metadata:
      name: your-custom-prohibitions
    spec:
      hostname: your.own-hostname.com
    EOF
    ```

2. Yalnızca kendi özel yasağınızı oluşturduktan sonra, çok geniş olan varsayılan yasağı silebilirsiniz:

    ```bash
    kubectl delete AzureIngressProhibitedTarget prohibit-all-targets
    ```

### <a name="enable-for-an-existing-agic-installation"></a>Varolan bir AGIC yüklemesi için etkinleştirme
Zaten çalışan bir AKS, Uygulama Ağ Geçidi ve bizim küme de yapılandırılmış AGIC olduğunu varsayalım. Biz `prod.contosor.com` bir Ingress var ve başarıyla AKS bunun için trafik hizmet vermektedir. Mevcut Uygulama `staging.contoso.com` Ağ Geçidimize eklemek istiyoruz, ancak [vm'de](https://azure.microsoft.com/services/virtual-machines/)barındırmamız gerekiyor. Biz mevcut Uygulama Ağ Geçidi yeniden kullanmak ve el ile bir dinleyici `staging.contoso.com`ve arka uç havuzları için yapılandıracağız. Ama el ile Uygulama Ağ Geçidi config tweaking [(portal](https://portal.azure.com)üzerinden , [ARM API](https://docs.microsoft.com/rest/api/resources/) veya [Terraform](https://www.terraform.io/)) tam mülkiyet AGIC varsayımları ile çakışacak. Değişiklikleri uyguladıktan kısa bir süre sonra, AGIC bunları üzerine yazacak veya silecek.

AGIC'in bir yapılandırma alt kümesinde değişiklik yapmasını yasaklayabiliriz.

1. Bir `AzureIngressProhibitedTarget` nesne oluşturma:
    ```bash
    cat <<EOF | kubectl apply -f -
    apiVersion: "appgw.ingress.k8s.io/v1"
    kind: AzureIngressProhibitedTarget
    metadata:
      name: manually-configured-staging-environment
    spec:
      hostname: staging.contoso.com
    EOF
    ```

2. Yeni oluşturulan nesneyi görüntüleyin:
    ```bash
    kubectl get AzureIngressProhibitedTargets
    ```

3. Portal üzerinden Uygulama Ağ Geçidi config değiştirin - dinleyici, yönlendirme kuralları, backends vb ekleyin. Oluşturduğumuz yeni nesne`manually-configured-staging-environment`( ) agic'in Uygulama Ağ `staging.contoso.com`Geçidi yapılandırmasına ilişkin olarak üzerine yazmasını yasaklayacaktır.
