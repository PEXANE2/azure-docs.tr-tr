---
title: Uygulama Ağ Geçidi Giriş Denetleyicisorun giderme
description: Bu makalede, Uygulama Ağ Geçidi Giriş Denetleyicisi ile sık sorulan sorular ve/veya sorunları nasıl giderilene ilişkin belgeler sağlanmaktadır.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: a64a9ce5e080308674893273e90a0e83686e339e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73795504"
---
# <a name="troubleshoot-common-questions-or-issues-with-ingress-controller"></a>Giriş Denetleyicisi ile sık sorulan soruları veya sorunları giderme

[Azure Bulut Shell,](https://shell.azure.com/) AKS ve AGIC kurulumunuzla ilgili sorunları gidermenin en kullanışlı yoludur. Shell.azure.com [veya](https://shell.azure.com/) bağlantıyı tıklayarak kabuğunuzu başlatın:

[![Gömme başlatma](https://shell.azure.com/images/launchcloudshell.png "Azure Cloud Shell'i başlatma")](https://shell.azure.com)


## <a name="test-with-a-simple-kubernetes-app"></a>Basit bir Kubernetes uygulaması ile test edin

Aşağıdaki adımlar varsayalım:
  - Gelişmiş Ağ etkin leştirilmiş bir AKS kümeniz var
  - AGIC AKS kümesine yüklendi
  - AKS kümenizle paylaşılan bir VNET'te zaten bir Uygulama Ağ Geçidi'ni

Uygulama Ağ Geçidi + AKS + AGIC yüklemesinin doğru şekilde kurulduğunu doğrulamak için mümkün olan en basit uygulamayı dağıtın:

```bash
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: test-agic-app-pod
  labels:
    app: test-agic-app
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
  name: test-agic-app-service
spec:
  selector:
    app: test-agic-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: test-agic-app-ingress
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
spec:
  rules:
    - host: test.agic.contoso.com
      http:
        paths:
          - path: /
            backend:
              serviceName: test-agic-app-service
              servicePort: 80
EOF
```

Yukarıdaki komut dosyasından tüm satırları aynı anda bir [Azure Bulut Uyrciçine](https://shell.azure.com/)kopyalayın ve yapıştırın. Lütfen tüm komutun kopyalandığından `cat` emin olun - `EOF`son ile başlayan ve dahil olmak üzere .

![apply](./media/application-gateway-ingress-controller-troubleshooting/tsg--apply-config.png)

Uygulamanın AKS kümenizin üzerinde başarılı bir şekilde dağıtılmasından sonra yeni bir Pod, Hizmet ve Giriş olacaktır.

[Cloud Shell](https://shell.azure.com/)ile pod listesini `kubectl get pods -o wide`alın: .
'Test-agic-app-pod' adlı bir bölmenin oluşturulmasını bekliyoruz. Bir IP adresi olacak. Bu adres, AKS ile birlikte kullanılan Uygulama Ağ Geçidi'nin VNET'inde olmalıdır.

![Bakla](./media/application-gateway-ingress-controller-troubleshooting/tsg--get-pods.png)

Hizmet listesini alın: `kubectl get services -o wide`. 'Test-agic-app-service' adında bir hizmet görmeyi bekliyoruz.

![Bakla](./media/application-gateway-ingress-controller-troubleshooting/tsg--get-services.png)

Girişlerin listesini alın: `kubectl get ingress`. 'Test-agic-app-ingress' adlı bir Giriş kaynağıoluşturulmasını bekliyoruz. Kaynağın ana bilgisayar adı 'test.agic.contoso.com' olacaktır.

![Bakla](./media/application-gateway-ingress-controller-troubleshooting/tsg--get-ingress.png)

Kapsüllerden biri AGIC olacak. `kubectl get pods`bölmelerin bir listesini gösterir, bunlardan biri 'giriş-azure' ile başlayacaktır. Başarılı bir dağıtım yaptığımızı `kubectl logs <name-of-ingress-controller-pod>` doğrulamak için bölmenin tüm günlüklerini alın. Başarılı bir dağıtım günlüğe aşağıdaki satırları eklerdi:
```
I0927 22:34:51.281437       1 process.go:156] Applied Application Gateway config in 20.461335266s
I0927 22:34:51.281585       1 process.go:165] cache: Updated with latest applied config.
I0927 22:34:51.282342       1 process.go:171] END AppGateway deployment
```

Alternatif olarak, [Cloud Shell'den](https://shell.azure.com/) yalnızca AGIC bölmesinin `kubectl logs <ingress-azure-....> | grep 'Applied App Gateway config in'`tam `<ingress-azure....>` adı nerede olması gerektiği ile başarılı Uygulama Ağ Geçidi yapılandırmasını gösteren satırları alabiliriz.

Uygulama Ağ Geçidi aşağıdaki yapılandırmayı uygular:

- Dinleyici: ![dinleyici](./media/application-gateway-ingress-controller-troubleshooting/tsg--listeners.png)

- Yönlendirme Kuralı: ![routing_rule](./media/application-gateway-ingress-controller-troubleshooting/tsg--rule.png)

- Arka uç Havuzu:
  - Arka uç adres havuzunda bir IP adresi olacak ve daha önce gözlemlediğimiz `kubectl get pods -o wide` 
 ![Pod'un IP adresiyle backend_pool](./media/application-gateway-ingress-controller-troubleshooting/tsg--backendpools.png)


Son olarak, `cURL` yeni dağıtılan uygulamaya bir HTTP bağlantısı kurmak için [Cloud Shell](https://shell.azure.com/) içinden komutu kullanabiliriz:

1. Uygulama `kubectl get ingress` Ağ Geçidi'nin Genel IP adresini almak için kullanın
2. `curl -I -H 'test.agic.contoso.com' <publitc-ip-address-from-previous-command>` kullan

![Bakla](./media/application-gateway-ingress-controller-troubleshooting/tsg--curl.png)

Bunun bir `HTTP/1.1 200 OK` sonucu, Uygulama Ağ Geçidi + AKS + AGIC sisteminin beklendiği gibi çalıştığını gösterir.


## <a name="inspect-kubernetes-installation"></a>Kubernetes Kurulumunu İncele

### <a name="pods-services-ingress"></a>Podlar, Hizmetler, Giriş
Uygulama Ağ Geçidi Giriş Denetleyicisi (AGIC) sürekli olarak aşağıdaki Kubernetes kaynaklarını izler: [Dağıtım](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#creating-a-deployment) veya [Pod](https://kubernetes.io/docs/concepts/workloads/pods/pod/#what-is-a-pod), [Hizmet](https://kubernetes.io/docs/concepts/services-networking/service/), [Giriş](https://kubernetes.io/docs/concepts/services-networking/ingress/)


AGIC'in beklendiği gibi çalışması için aşağıdakiler yerinde olmalıdır:
  1. AKS bir veya daha fazla sağlıklı **pods**olmalıdır.
     [Bunu Cloud Shell'den](https://shell.azure.com/) `kubectl get pods -o wide --show-labels` doğrulayın, bir `apsnetapp`Pod'unuz varsa , çıktınız şu na benzer:
     ```bash
     delyan@Azure:~$ kubectl get pods -o wide --show-labels

     NAME                   READY   STATUS    RESTARTS   AGE   IP          NODE                       NOMINATED NODE   READINESS GATES   LABELS
     aspnetapp              1/1     Running   0          17h   10.0.0.6    aks-agentpool-35064155-1   <none>           <none>            app=aspnetapp
     ```

  2. Bir veya daha fazla **hizmet,** eşleşen `selector` etiketler aracılığıyla yukarıdaki bölmelere başvurur.
     [Bunu Cloud Shell'den](https://shell.azure.com/) doğrula`kubectl get services -o wide`
     ```bash
     delyan@Azure:~$ kubectl get services -o wide --show-labels

     NAME                TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE   SELECTOR        LABELS
     aspnetapp           ClusterIP   10.2.63.254    <none>        80/TCP    17h   app=aspnetapp   <none>     
     ```

  3. **Giriş**, açıklamalı `kubernetes.io/ingress.class: azure/application-gateway`, yukarıdaki hizmete atıfta bulunarak Bulut [Shell'den](https://shell.azure.com/) bunu doğrulayın`kubectl get ingress -o wide --show-labels`
     ```bash
     delyan@Azure:~$ kubectl get ingress -o wide --show-labels

     NAME        HOSTS   ADDRESS   PORTS   AGE   LABELS
     aspnetapp   *                 80      17h   <none>
     ```

  4. Yukarıdaki giriş ek açıklamalarını `kubectl get ingress aspnetapp -o yaml` görüntüleyin: (girişinizin adı ile değiştirin) `aspnetapp`
     ```bash
     delyan@Azure:~$ kubectl get ingress aspnetapp -o yaml

     apiVersion: extensions/v1beta1
     kind: Ingress
     metadata:
       annotations:
         kubernetes.io/ingress.class: azure/application-gateway
       name: aspnetapp
     spec:
       backend:
         serviceName: aspnetapp
         servicePort: 80
     ```

     Giriş kaynağı ile açıklama `kubernetes.io/ingress.class: azure/application-gateway`yapılmalıdır.
 

### <a name="verify-observed-namespace"></a>Gözlenen Ad Alanını Doğrula

* Kubernetes kümesindeki varolan ad alanlarını alın. Uygulamanız hangi ad alanında çalışıyor? AGIC bu isim alanını izliyor mu? Gözlenen [ad](./ingress-controller-multiple-namespace-support.md#enable-multiple-namespace-support) alanlarını düzgün şekilde yapılandırmaya ilişkin Çoklu Ad Alanı Desteği belgelerine bakın.

    ```bash
    # What namespaces exist on your cluster
    kubectl get namespaces
    
    # What pods are currently running
    kubectl get pods --all-namespaces -o wide
    ```


* AGIC bölmesi `default` ad alanında olmalıdır (bkz. sütun). `NAMESPACE` Sütunda sağlıklı bir bölme olurdu. `Running` `STATUS` En az bir AGIC kapsülü olmalıdır.

    ```bash
    # Get a list of the Application Gateway Ingress Controller pods
    kubectl get pods --all-namespaces --selector app=ingress-azure
    ```


* AGIC bölmesi sağlıklı değilse`STATUS` (yukarıdaki komuttaki `Running`sütun değil):
  - neden anlamak için günlükleri almak:`kubectl logs <pod-name>`
  - bölmenin önceki örneği için:`kubectl logs <pod-name> --previous`
  - daha fazla bağlam elde etmek için pod açıklayın:`kubectl describe pod <pod-name>`


* Kubernetes [Servis](https://kubernetes.io/docs/concepts/services-networking/service/) ve [Giriş](https://kubernetes.io/docs/concepts/services-networking/ingress/) kaynaklarınız var mı?
    
    ```bash
    # Get all services across all namespaces
    kubectl get service --all-namespaces -o wide
    
    # Get all ingress resources across all namespaces
    kubectl get ingress --all-namespaces -o wide
    ```


* Sizin [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) ile açıklamalı `kubernetes.io/ingress.class: azure/application-gateway`mı: ? AGIC sadece bu ek açıklama var Kubernetes Ingress kaynakları için izleyecek.
    
    ```bash
    # Get the YAML definition of a particular ingress resource
    kubectl get ingress --namespace  <which-namespace?>  <which-ingress?>  -o yaml
    ```


* AGIC bazı kritik hatalar için Kubernetes olaylarını yayır. Bunları görüntüleyebilirsiniz:
  - üzerinden terminalinizde`kubectl get events --sort-by=.metadata.creationTimestamp`
  - [Kubernetes Web UI (Pano)](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/) kullanarak tarayıcınızda


## <a name="logging-levels"></a>Günlük Düzeyleri

AGIC'in 3 günlük seviyesi vardır. Düzey 1 varsayılan biridir ve en az sayıda günlük satırı gösterir.
Düzey 5, diğer taraftan, ARM uygulanan config sanitized içeriği de dahil olmak üzere tüm günlükleri görüntüler.

Kubernetes topluluğu [kubectl](https://kubernetes.io/docs/reference/kubectl/cheatsheet/#kubectl-output-verbosity-and-debugging) aracı için 9 seviye günlük oluşturmuştur. Bu depoda benzer anlambilimile bunlardan 3'ü kullanıyoruz:


| Ayrıntı Düzeyi | Açıklama |
|-----------|-------------|
|  1        | Varsayılan günlük düzeyi; başlangıç ayrıntılarını, uyarılarını ve hatalarını gösterir |
|  3        | Olaylar ve değişiklikler hakkında genişletilmiş bilgiler; oluşturulan nesnelerin listeleri |
|  5        | Günlükler marshaled nesneleri; ARM'a uygulanan dezenfekte edilmiş JSON config'ini gösterir |


Ayrıntılı lık düzeyleri `verbosityLevel` [helm-config.yaml](#sample-helm-config-file) dosyasındaki değişken üzerinden ayarlanabilir. JSON config `5` [ARM](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)sevk almak için verbosity düzeyini artırmak:
  - `verbosityLevel: 5` [helm-config.yaml](#sample-helm-config-file) tek başına bir satır eklemek ve yeniden yüklemek
  - ile günlükleri almak`kubectl logs <pod-name>`

### <a name="sample-helm-config-file"></a>Örnek Helm config dosyası
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

