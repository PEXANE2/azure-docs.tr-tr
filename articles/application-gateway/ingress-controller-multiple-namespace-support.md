---
title: Uygulama Ağ Geçidi Giriş Denetleyicisi için birden çok ad alanı desteketkinleştirme
description: Bu makalede, Bir Uygulama Ağ Geçidi Giriş Denetleyicisi ile Bir Kubernetes kümesinde birden çok ad alanı desteği etkinleştirmek için nasıl bilgi sağlar.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 83650e7cf46ec1dede5f25e32114d6469bab24be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79279929"
---
# <a name="enable-multiple-namespace-support-in-an-aks-cluster-with-application-gateway-ingress-controller"></a>Uygulama Ağ Geçidi Giriş Denetleyicisi ile bir AKS kümesinde birden çok Ad Alanı desteği etkinleştirme

## <a name="motivation"></a>Motivasyon
Kubernetes [Ad Alanları,](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/) bir Kubernetes kümesinin bölünmesini ve daha büyük bir takımın alt gruplarına ayrılmasını mümkün kılar. Bu alt takımlar daha sonra kaynakların, güvenliğin, yapılandırmanın vb. daha hassas denetimleri ile altyapıyı dağıtabilir ve yönetebilir. Kubernetes, her ad alanı içinde bir veya daha fazla giriş kaynağının bağımsız olarak tanımlanmasına olanak tanır.

Sürüm 0.7 [Azure Uygulama Ağ Geçidi Kubernetes IngressController](https://github.com/Azure/application-gateway-kubernetes-ingress/blob/master/README.md) (AGIC) sürümü nden itibaren olayları yutabilir ve birden çok ad alanı gözlemleyebilir. AKS yöneticisi [Uygulama Ağ Geçidi'ni](https://azure.microsoft.com/services/application-gateway/) giriş olarak kullanmaya karar verirse, tüm ad alanları aynı Uygulama Ağ Geçidi örneğini kullanır. Giriş Denetleyicisi'nin tek bir yüklemesi erişilebilir ad boşluklarını izler ve ilişkili olduğu Uygulama Ağ Geçidi'ni yapılandıracaktır.

AGIC'in 0.7 sürümü, Helm `default` yapılandırmasında açıkça bir veya daha fazla farklı ad alanı olarak değiştirilmediği sürece yalnızca ad alanını gözlemlememeye devam edecektir (aşağıdaki bölüme bakın).

## <a name="enable-multiple-namespace-support"></a>Birden çok ad alanı desteğini etkinleştirme
Birden çok ad alanı desteğini etkinleştirmek için:
1. [helm-config.yaml](#sample-helm-config-file) dosyasını aşağıdaki yollardan birinde değiştirin:
   - `watchNamespace` [helm-config.yaml](#sample-helm-config-file) tamamen anahtar silin - AGIC tüm isim boşlukları gözlemleyecek
   - boş `watchNamespace` bir dize ayarlanmış - AGIC tüm ad boşluklarını gözlemleyecek
   - virgülle ayrılmış birden çok`watchNamespace: default,secondNamespace`ad alanı ekleyin ( ) - AGIC bu ad alanlarını yalnızca gözlemleyecek
2. Ile Miğfer şablonu değişiklikleri uygulayın:`helm install -f helm-config.yaml application-gateway-kubernetes-ingress/ingress-azure`

Birden çok ad alanı gözlemleme özelliğiile dağıtıldıktan sonra, AGIC şunları yapacaktır:
  - tüm erişilebilir ad alanlarından kaynak listesi
  - ile açıklamalı giriş kaynaklarına filtre`kubernetes.io/ingress.class: azure/application-gateway`
  - kombine [Uygulama Ağ Geçidi config](https://github.com/Azure/azure-sdk-for-go/blob/37f3f4162dfce955ef5225ead57216cf8c1b2c70/services/network/mgmt/2016-06-01/network/models.go#L1710-L1744) oluşturmak
  - config'i [ARM](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) aracılığıyla ilişkili Uygulama Ağ Geçidi'ne uygulayın

## <a name="conflicting-configurations"></a>Çakışan Yapılandırmalar
Birden çok ad alanı alan [giriş kaynakları,](https://kubernetes.io/docs/concepts/services-networking/ingress/#the-ingress-resource) AGIC'e tek bir Uygulama Ağ Geçidi için çakışan yapılandırmalar oluşturması talimatını verebilir. (Örneğin aynı etki alanını iddia eden iki giriş.)

Hiyerarşinin üst kısmında - **dinleyiciler** (IP adresi, bağlantı noktası ve ana bilgisayar) ve **yönlendirme kuralları** (bağlayıcı dinleyici, arka uç havuzu ve HTTP ayarları) oluşturulabilir ve birden çok ad alanı/giriş tarafından paylaşılabilir.

Diğer taraftan - yollar, arka uç havuzları, HTTP ayarları ve TLS sertifikaları yalnızca bir ad alanı tarafından oluşturulabilir ve yinelenenler kaldırılır.

Örneğin, aşağıdaki yinelenen giriş kaynaklarını tanımlanan `staging` `production` ad `www.contoso.com`alanlarını göz önünde bulundurun ve aşağıdakiler için:
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: websocket-ingress
  namespace: staging
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
spec:
  rules:
    - host: www.contoso.com
      http:
        paths:
          - backend:
              serviceName: web-service
              servicePort: 80
```

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: websocket-ingress
  namespace: production
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
spec:
  rules:
    - host: www.contoso.com
      http:
        paths:
          - backend:
              serviceName: web-service
              servicePort: 80
```

İlgili Kubernetes ad `www.contoso.com` alanlarına yönlendirilmesi için trafik talep eden iki giriş kaynağına rağmen, trafiğe yalnızca bir arka uç hizmet verebilir. AGIC, kaynaklardan biri için "ilk gelen, ilk hizmet" temelinde bir yapılandırma oluşturacak. Aynı anda iki giriş kaynağı oluşturulursa, alfabedeki daha önceki kaynak önceliklidir. Yukarıdaki örnekten yalnızca `production` giriş için ayarlar oluşturabilecektir. Uygulama Ağ Geçidi aşağıdaki kaynaklarla yapılandırılacaktır:

  - Dinleyici:`fl-www.contoso.com-80`
  - Yönlendirme Kuralı:`rr-www.contoso.com-80`
  - Arka uç Havuzu:`pool-production-contoso-web-service-80-bp-80`
  - HTTP Ayarları:`bp-production-contoso-web-service-80-80-websocket-ingress`
  - Sağlık Sondası:`pb-production-contoso-web-service-80-websocket-ingress`

*Dinleyici* ve *yönlendirme kuralı*dışında, oluşturulan Uygulama Ağ Geçidi kaynaklarının oluşturuldukları`production`ad alanının adını içerdiğini unutmayın.

İki giriş kaynağı AKS kümesine zaman içinde farklı noktalarda getirilirse, AGIC'nin Uygulama Ağ Geçidi'ni yeniden yapılandırdığı `namespace-B` `namespace-A`ve trafiği .'ye yeniden yönlendirirhale geldiği bir senaryoda son bulmaolasılığı yüksektir.

Örneğin, ilk `staging` olarak eklediyseniz, AGIC, trafiği evreleme arka uç havuzuna yönlendirmek için Uygulama Ağ Geçidi'ni yapılandıracaktır. Daha sonraki bir aşamada, giriş tanıtılması, `production` AGIC'in Uygulama Ağ Geçidi'ni `production` yeniden programlamasına neden olur ve bu da trafiği arka uç havuzuna yönlendirmeye başlar.

## <a name="restrict-access-to-namespaces"></a>Erişimi Ad Boşluklarıyla Sınırlandır
Varsayılan olarak AGIC, herhangi bir ad alanı içinde açıklamalı Giriş'e göre Uygulama Ağ Geçidi'ni yapılandıracaktır. Bu davranışı sınırlamak istiyorsanız, aşağıdaki seçeneklere sahipsiniz:
  - ad boşluklarını sınırlandırın, ad boşluklarını açıkça tanımlayarak `watchNamespace` AGIC [helm-config.yaml'deki](#sample-helm-config-file) YAML tuşu ile gözlemlemelidir.
  - AGIC'i belirli ad alanlarıyla sınırlamak için [Role/RoleBinding'i](https://docs.microsoft.com/azure/aks/azure-ad-rbac) kullanın

## <a name="sample-helm-config-file"></a>Örnek Helm config dosyası
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

