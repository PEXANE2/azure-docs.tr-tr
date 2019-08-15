---
title: Azure Kubernetes Service (AKS) kümesinde sanal Kubelet çalıştırma
description: Azure Kubernetes hizmeti (AKS) ile sanal Kubelet 'i kullanarak Azure Container Instances Linux ve Windows kapsayıcıları çalıştırma hakkında bilgi edinin.
services: container-service
author: mlearned
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: mlearned
ms.openlocfilehash: f18992be353d2d6cc739412d98ccd97d5e78d4c7
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "67613870"
---
# <a name="use-virtual-kubelet-with-azure-kubernetes-service-aks"></a>Azure Kubernetes hizmeti (AKS) ile sanal Kubelet kullanma

Azure Container Instances (ACI) Azure 'da kapsayıcılar çalıştırmak için bir barındırılan ortam sağlar. ACI kullanırken, temel işlem altyapısını yönetmeniz gerekmez, Azure bu yönetimi sizin için işler. Kapsayıcıları ACI 'de çalıştırırken, çalışan her kapsayıcı için ikinci olarak ücretlendirilirsiniz.

Azure Container Instances için sanal Kubelet sağlayıcısını kullanırken, hem Linux hem de Windows kapsayıcıları standart bir Kubernetes düğümü gibi bir kapsayıcı örneği üzerinde zamanlanabilir. Bu yapılandırma, hem Kubernetes özelliğinden hem de kapsayıcı örneklerinin yönetim değerine ve maliyet avantajlarından yararlanmanızı sağlar.

> [!NOTE]
> AKS artık, *sanal düğümler*olarak adlandırılan aci üzerinde kapsayıcılar için yerleşik destek içerir. Bu sanal düğümler Şu anda Linux kapsayıcı örneklerini desteklemektedir. Windows kapsayıcı örneklerini zamanlamanız gerekiyorsa, sanal Kubelet 'yi kullanmaya devam edebilirsiniz. Aksi takdirde, bu makalede belirtilen el ile sanal Kubelet yönergeleri yerine sanal düğümleri kullanmanız gerekir. [Azure CLI][virtual-nodes-cli] veya [Azure Portal][virtual-nodes-portal]kullanarak sanal düğümleri kullanmaya başlamanızı sağlayabilirsiniz.
>
> Sanal Kubelet, deneysel açık kaynaklı bir projem ve bu şekilde kullanılmalıdır. Katkıda bulunmak, sorun gidermek ve sanal kubelet hakkında daha fazla bilgi edinmek için bkz. [sanal Kubelet GitHub projesi][vk-github].

## <a name="before-you-begin"></a>Başlamadan önce

Bu belge bir AKS kümeniz olduğunu varsayar. AKS kümesine ihtiyacınız varsa bkz. [Azure Kubernetes hizmeti (aks) hızlı başlangıç][aks-quick-start].

Ayrıca Azure CLı sürüm **2.0.65** veya sonraki bir sürüme de ihtiyacınız vardır. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

Sanal Kubelet 'yi yüklemek için AKS kümenize [Helm][aks-helm] 'i yükleyip yapılandırın. Gerekirse, Tiller 'nin [Kubernetes RBAC ile kullanım için yapılandırıldığından](#for-rbac-enabled-clusters)emin olun.

### <a name="register-container-instances-feature-provider"></a>Container Instances Özellik sağlayıcısını Kaydet

Azure Container Instance (acı) hizmetini daha önce kullanmadıysanız, hizmet sağlayıcısını aboneliğiniz ile kaydedin. Aşağıdaki örnekte gösterildiği gibi [az Provider List][az-provider-list] komutunu kullanarak aci sağlayıcı kaydının durumunu denetleyebilirsiniz:

```azurecli-interactive
az provider list --query "[?contains(namespace,'Microsoft.ContainerInstance')]" -o table
```

Aşağıdaki örnek çıktıda gösterildiği gibi, *Microsoft. Containerınstance* sağlayıcısı *kayıtlı*olarak rapor etmelidir:

```console
Namespace                    RegistrationState
---------------------------  -------------------
Microsoft.ContainerInstance  Registered
```

Sağlayıcı *Notregistered*olarak gösteriyorsa, aşağıdaki örnekte gösterildiği gibi [az Provider Register][az-provider-register] kullanarak sağlayıcıyı kaydedin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerInstance
```

### <a name="for-rbac-enabled-clusters"></a>RBAC özellikli kümeler için

AKS kümeniz RBAC etkinse, Tiller ile kullanmak için bir hizmet hesabı ve rol bağlama oluşturmanız gerekir. Daha fazla bilgi için bkz. [Helm rol tabanlı erişim denetimi][helm-rbac]. Bir hizmet hesabı ve rol bağlama oluşturmak için *RBAC-sanal-kubelet. YAML* adlı bir dosya oluşturun ve aşağıdaki tanımı yapıştırın:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

Hizmet hesabını ve bağlamayı [kubectl Apply][kubectl-apply] ile uygulayın ve aşağıdaki örnekte gösterildiği gibi *RBAC-sanal-kubelet. YAML* dosyanızı belirtin:

```console
$ kubectl apply -f rbac-virtual-kubelet.yaml

clusterrolebinding.rbac.authorization.k8s.io/tiller created
```

Held 'yi Tiller hizmet hesabını kullanacak şekilde yapılandırın:

```console
helm init --service-account tiller
```

Artık, sanal Kubelet 'i AKS kümenize yüklemeye devam edebilirsiniz.

## <a name="installation"></a>Yükleme

Sanal Kubelet yüklemek için [az aks Install-Connector][aks-install-connector] komutunu kullanın. Aşağıdaki örnek hem Linux hem de Windows bağlayıcısını dağıtır.

```azurecli-interactive
az aks install-connector \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --connector-name virtual-kubelet \
    --os-type Both
```

Bu bağımsız değişkenler, [az aks Install-Connector][aks-install-connector] komutu için kullanılabilir.

| Değişkendir | Açıklama | Gerekli |
|---|---|:---:|
| `--connector-name` | ACı bağlayıcısının adı.| Evet |
| `--name``-n` | Yönetilen kümenin adı. | Evet |
| `--resource-group``-g` | Kaynak grubunun adı. | Evet |
| `--os-type` | Kapsayıcı örnekleri işletim sistemi türü. İzin verilen değerler: İkisi, Linux, Windows. Varsayılan: Linux. | Hayır |
| `--aci-resource-group` | ACı kapsayıcı gruplarının oluşturulacağı kaynak grubu. | Hayır |
| `--location``-l` | ACı kapsayıcı gruplarının oluşturulacağı konum. | Hayır |
| `--service-principal` | Azure API 'Lerinde kimlik doğrulaması için kullanılan hizmet sorumlusu. | Hayır |
| `--client-secret` | Hizmet sorumlusu ile ilişkili gizli dizi. | Hayır |
| `--chart-url` | ACı bağlayıcısını yükleyen bir Helm grafiğinin URL 'SI. | Hayır |
| `--image-tag` | Sanal kubelet kapsayıcı resminin resim etiketi. | Hayır |

## <a name="validate-virtual-kubelet"></a>Sanal Kubelet 'i doğrulama

Sanal Kubelet 'nin yüklendiğini doğrulamak için, [kubectl Get Nodes][kubectl-get] komutunu kullanarak Kubernetes düğümlerinin bir listesini döndürün:

```console
$ kubectl get nodes

NAME                                             STATUS   ROLES   AGE   VERSION
aks-nodepool1-56577038-0                         Ready    agent   11m   v1.12.8
virtual-kubelet-virtual-kubelet-linux-eastus     Ready    agent   39s   v1.13.1-vk-v0.9.0-1-g7b92d1ee-dev
virtual-kubelet-virtual-kubelet-windows-eastus   Ready    agent   37s   v1.13.1-vk-v0.9.0-1-g7b92d1ee-dev
```

## <a name="run-linux-container"></a>Linux kapsayıcısını Çalıştır

Aşağıdaki YAML 'de `virtual-kubelet-linux.yaml` adlı bir dosya oluşturun ve kopyalayın. Düğüm üzerinde kapsayıcıyı zamanlamak için bir [Nodeselector][node-selector] ve [toleranation][toleration] kullanıldığını unutmayın.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aci-helloworld
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aci-helloworld
  template:
    metadata:
      labels:
        app: aci-helloworld
    spec:
      containers:
      - name: aci-helloworld
        image: microsoft/aci-helloworld
        ports:
        - containerPort: 80
      nodeSelector:
        beta.kubernetes.io/os: linux
        kubernetes.io/role: agent
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Equal
        value: azure
        effect: NoSchedule
```

Uygulamayı [kubectl Create][kubectl-create] komutuyla çalıştırın.

```console
kubectl create -f virtual-kubelet-linux.yaml
```

Zamanlanan düğümle birlikte Pod listesini çıkarmak için `-o wide` bağımsız değişkenle [kubectl Get Pod][kubectl-get] komutunu kullanın. `aci-helloworld` Pod 'ın `virtual-kubelet-virtual-kubelet-linux` düğüm üzerinde zamanlandığına dikkat edin.

```console
$ kubectl get pods -o wide

NAME                              READY   STATUS    RESTARTS   AGE     IP               NODE
aci-helloworld-7b9ffbf946-rx87g   1/1     Running   0          22s     52.224.147.210   virtual-kubelet-virtual-kubelet-linux-eastus
```

## <a name="run-windows-container"></a>Windows kapsayıcısı çalıştırma

Aşağıdaki YAML 'de `virtual-kubelet-windows.yaml` adlı bir dosya oluşturun ve kopyalayın. Düğüm üzerinde kapsayıcıyı zamanlamak için bir [Nodeselector][node-selector] ve [toleranation][toleration] kullanıldığını unutmayın.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nanoserver-iis
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nanoserver-iis
  template:
    metadata:
      labels:
        app: nanoserver-iis
    spec:
      containers:
      - name: nanoserver-iis
        image: microsoft/iis:nanoserver
        ports:
        - containerPort: 80
      nodeSelector:
        beta.kubernetes.io/os: windows
        kubernetes.io/role: agent
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Equal
        value: azure
        effect: NoSchedule
```

Uygulamayı [kubectl Create][kubectl-create] komutuyla çalıştırın.

```console
kubectl create -f virtual-kubelet-windows.yaml
```

Zamanlanan düğümle birlikte Pod listesini çıkarmak için `-o wide` bağımsız değişkenle [kubectl Get Pod][kubectl-get] komutunu kullanın. `nanoserver-iis` Pod 'ın `virtual-kubelet-virtual-kubelet-windows` düğüm üzerinde zamanlandığına dikkat edin.

```console
$ kubectl get pods -o wide

NAME                              READY   STATUS    RESTARTS   AGE     IP               NODE
nanoserver-iis-5d999b87d7-6h8s9   1/1     Running   0          47s     52.224.143.39    virtual-kubelet-virtual-kubelet-windows-eastus
```

## <a name="remove-virtual-kubelet"></a>Sanal Kubelet 'yi kaldır

Sanal Kubelet 'i kaldırmak için [az aks Remove-Connector][aks-remove-connector] komutunu kullanın. Bağımsız değişken değerlerini bağlayıcının adı, AKS kümesi ve AKS kümesi kaynak grubu ile değiştirin.

```azurecli-interactive
az aks remove-connector \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --connector-name virtual-kubelet \
    --os-type Both
```

> [!NOTE]
> Hem işletim sistemi bağlayıcılarını kaldırma hatalarıyla karşılaşırsanız veya yalnızca Windows veya Linux işletim sistemi bağlayıcısını kaldırmak istiyorsanız, işletim sistemi türünü el ile belirtebilirsiniz. Önceki `--os-type` `Windows` `Linux`komuta parametresiniekleyinveveyabelirtin.`az aks remove-connector`

## <a name="next-steps"></a>Sonraki adımlar

Sanal Kubelet ile ilgili olası sorunlar için, [bilinen süslemeler ve geçici çözümler][vk-troubleshooting]bölümüne bakın. Sanal Kubelet ile ilgili sorunları bildirmek için [bir GitHub sorunu açın][vk-issues].

Sanal kubelet [GitHub projesinde][vk-github]sanal kubelet hakkında daha fazla bilgi edinin.

<!-- LINKS - internal -->
[aks-quick-start]: ./kubernetes-walkthrough.md
[aks-remove-connector]: /cli/azure/aks#az-aks-remove-connector
[az-container-list]: /cli/azure/aks#az-aks-list
[aks-install-connector]: /cli/azure/aks#az-aks-install-connector
[virtual-nodes-cli]: virtual-nodes-cli.md
[virtual-nodes-portal]: virtual-nodes-portal.md
[aks-helm]: kubernetes-helm.md
[az-provider-list]: /cli/azure/provider#az-provider-list
[az-provider-register]: /cli/azure/provider#az-provider-register

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[vk-github]: https://github.com/virtual-kubelet/virtual-kubelet
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[vk-troubleshooting]: https://github.com/virtual-kubelet/virtual-kubelet#known-quirks-and-workarounds
[vk-issues]: https://github.com/virtual-kubelet/virtual-kubelet/issues
