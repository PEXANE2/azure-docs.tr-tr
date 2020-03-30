---
title: Web panosuyla Azure Kubernetes Hizmet kümesini yönetme
description: Learn how to use the built-in Kubernetes web UI dashboard to manage an Azure Kubernetes Service (AKS) cluster
services: container-service
ms.topic: article
ms.date: 10/08/2018
ms.openlocfilehash: 15fcf765be0a754575713eebcdaa7d68e1c299b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77595357"
---
# <a name="access-the-kubernetes-web-dashboard-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti (AKS)'deki Kubernetes web panosuna erişin

Kubernetes, temel yönetim işlemleri için kullanılabilecek bir web panosu içerir. Bu pano, uygulamalarınız için temel sistem durumu durumunu ve ölçümlerini görüntülemenize, hizmetler oluşturmanıza ve dağıtmanıza ve varolan uygulamaları nızı görüntülemenize olanak tanır. Bu makalede, Azure CLI'yi kullanarak Kubernetes panosuna nasıl erişilenler gösterilmektedir ve ardından bazı temel pano işlemlerinde size yol gösterir.

Kubernetes panosu hakkında daha fazla bilgi için [Kubernetes Web Kullanıcı Arabirimi Panosu'na][kubernetes-dashboard]bakın.

## <a name="before-you-begin"></a>Başlamadan önce

Bu belgede ayrıntılı adımlar, bir AKS kümesi oluşturduğunuzu `kubectl` ve kümeyle bir bağlantı oluşturduğunuzu varsayar. Bir AKS kümesi oluşturmanız gerekiyorsa, [AKS hızlı başlat'ına][aks-quickstart]bakın.

Ayrıca Azure CLI sürüm 2.0.46 veya üzerini yüklemiş ve yapılandırmış olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye][install-azure-cli]bakın.

## <a name="start-the-kubernetes-dashboard"></a>Kubernetes panosunu başlatın

Kubernetes panosunu başlatmak için [az aks gözatma][az-aks-browse] komutunu kullanın. Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubunda *myAKSCluster* adlı kümenin panosunu açar:

```azurecli
az aks browse --resource-group myResourceGroup --name myAKSCluster
```

Bu komut, geliştirme sisteminiz ve Kubernetes API'niz arasında bir proxy oluşturur ve Kubernetes panosuna bir web tarayıcısı açar. Bir web tarayıcısı Kubernetes panosuna açılmıyorsa, genellikle Azure CLI'de belirtilen `http://127.0.0.1:8001`URL adresini kopyalayıp yapıştırın.

<!--
![The login page of the Kubernetes web dashboard](./media/kubernetes-dashboard/dashboard-login.png)

You have the following options to sign in to your cluster's dashboard:

* A [kubeconfig file][kubeconfig-file]. You can generate a kubeconfig file using [az aks get-credentials][az-aks-get-credentials].
* A token, such as a [service account token][aks-service-accounts] or user token. On [AAD-enabled clusters][aad-cluster], this token would be an AAD token. You can use `kubectl config view` to list the tokens in your kubeconfig file. For more details on creating an AAD token for use with an AKS cluster see [Integrate Azure Active Directory with Azure Kubernetes Service using the Azure CLI][aad-cluster].
* The default dashboard service account, which is used if you click *Skip*.

> [!WARNING]
> Never expose the Kubernetes dashboard publicly, regardless of the authentication method used.
> 
> When setting up authentication for the Kubernetes dashboard, it is recommended that you use a token over the default dashboard service account. A token allows each user to use their own permissions. Using the default dashboard service account may allow a user to bypass their own permissions and use the service account instead.
> 
> If you do choose to use the default dashboard service account and your AKS cluster uses RBAC, a *ClusterRoleBinding* must be created before you can correctly access the dashboard. By default, the Kubernetes dashboard is deployed with minimal read access and displays RBAC access errors. A cluster administrator can choose to grant additional access to the *kubernetes-dashboard* service account, however this can be a vector for privilege escalation. You can also integrate Azure Active Directory authentication to provide a more granular level of access.
>
> To create a binding, use the [kubectl create clusterrolebinding][kubectl-create-clusterrolebinding] command as shown in the following example. **This sample binding does not apply any additional authentication components and may lead to insecure use.**
>
> ```console
> kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
> ```
> 
> You can now access the Kubernetes dashboard in your RBAC-enabled cluster. To start the Kubernetes dashboard, use the [az aks browse][az-aks-browse] command as detailed in the previous step.
>
> If your cluster does not use RBAC, it is not recommended to create a *ClusterRoleBinding*.
> 
> For more information on using the different authentication methods, see the Kubernetes dashboard wiki on [access controls][dashboard-authentication].

After you choose a method to sign in, the Kubernetes dashboard is displayed. If you chose to use *token* or *skip*, the Kubernetes dashboard will use the permissions of the currently logged in user to access the cluster.
-->

> [!IMPORTANT]
> AKS kümeniz RBAC kullanıyorsa, panoya doğru bir şekilde erişemeden önce bir *ClusterRoleBinding* oluşturulması gerekir. Varsayılan olarak, Kubernetes panosu en az okuma erişimiyle dağıtılır ve RBAC erişim hatalarını görüntüler. Kubernetes panosu şu anda erişim düzeyini belirlemek için kullanıcı tarafından sağlanan kimlik bilgilerini desteklemez, daha çok hizmet hesabına verilen rolleri kullanır. Bir küme yöneticisi *kubernetes-pano* hizmet hesabına ek erişim vermeyi seçebilir, ancak bu ayrıcalık yükseltme için bir vektör olabilir. Ayrıca, daha parçalı bir erişim düzeyi sağlamak için Azure Active Directory kimlik doğrulamasını da entegre edebilirsiniz.
> 
> Bağlayıcı oluşturmak için [kubectl clusterrolebinding][kubectl-create-clusterrolebinding] komutunu kullanın. Aşağıdaki örnek bağlamanın nasıl oluşturulacağı gösterilmektedir, ancak bu örnek bağlama herhangi bir ek kimlik doğrulama bileşeni uygulamaz ve güvenli olmayan kullanıma neden olabilir. Kubernetes panosu URL erişimi olan herkese açıktır. Kubernetes panosunu herkese açık bir şekilde ifşa etmeyin.
>
> ```console
> kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
> ```
> 
> Farklı kimlik doğrulama yöntemlerini kullanma hakkında daha fazla bilgi için [erişim denetimleri][dashboard-authentication]üzerindeki Kubernetes pano wiki'ye bakın.

![Kubernetes web panosunun genel bakış sayfası](./media/kubernetes-dashboard/dashboard-overview.png)

## <a name="create-an-application"></a>Uygulama oluşturma

Kubernetes panosunun yönetim görevlerinin karmaşıklığını nasıl azaltabileceğini görmek için bir uygulama oluşturalım. Metin girişi, bir YAML dosyası veya grafik sihirbazı aracılığıyla Kubernetes panosundan bir uygulama oluşturabilirsiniz.

Bir uygulama oluşturmak için aşağıdaki adımları tamamlayın:

1. Sağ üst penceredeki **Oluştur** düğmesini seçin.
1. Grafik sihirbazını kullanmak için **bir uygulama oluşturmayı**seçin.
1. *Nginx* gibi dağıtım için bir ad sağlama
1. *Nginx:1.15.5* gibi kullanılacak kapsayıcı görüntüsünün adını girin
1. Web trafiği için bağlantı noktası 80'i ortaya çıkarmak için bir Kubernetes hizmeti oluşturursunuz. **Hizmet**altında, **Dış'ı**seçin, ardından hem bağlantı noktası hem de hedef bağlantı noktası için **80** girin.
1. Hazır olduğunuzda, uygulamayı oluşturmak için **Dağıt'ı** seçin.

![Kubernetes web panosunda bir uygulama dağıtma](./media/kubernetes-dashboard/create-app.png)

Ortak bir harici IP adresinin Kubernetes hizmetine atanması bir veya iki dakika sürer. Sol boyutta, **Discovery ve Load Balancing'in** altında belirli **Hizmetler.** Uygulamanızın hizmeti, aşağıdaki örnekte gösterildiği gibi *Dış uç noktaları*da dahil olmak üzere listelenir:

![Hizmet listesini ve uç noktaları görüntüleme](./media/kubernetes-dashboard/view-services.png)

Varsayılan NGINX sayfasına bir web tarayıcısı penceresi açmak için bitiş noktası adresini seçin:

![Dağıtılan uygulamanın varsayılan NGINX sayfasını görüntüleme](./media/kubernetes-dashboard/default-nginx.png)

## <a name="view-pod-information"></a>Bölme bilgilerini görüntüleme

Kubernetes panosu temel izleme ölçümleri ve günlükler gibi sorun giderme bilgileri sağlayabilir.

Uygulama bölmeleriniz hakkında daha fazla bilgi görmek için sol menüde **Pods'u** seçin. Kullanılabilir bölmelerin listesi gösterilir. Kaynak tüketimi gibi bilgileri görüntülemek için *nginx* bölmenizi seçin:

![Bölme bilgilerini görüntüleme](./media/kubernetes-dashboard/view-pod-info.png)

## <a name="edit-the-application"></a>Uygulamayı edin

Uygulamaları oluşturmanın ve görüntülemenin yanı sıra, Kubernetes panosu uygulama dağıtımlarını güncellemek ve güncellemek için de kullanılabilir. Uygulama için ek artıklık sağlamak için NGINX yinelemelerinin sayısını artıralım.

Dağıtımı yeniden yapmak için:

1. Sol menüde **Dağıtımlar'ı** seçin ve ardından *nginx* dağıtımınızı seçin.
1. Sağ üst gezinti çubuğunda **Edit'i** seçin.
1. `spec.replica` Değeri 20. Uygulamaiçin yineleme sayısını artırmak için bu değeri *1'den* *3'e*değiştirin.
1. Hazır olduğunda **Güncelleştir'i** seçin.

![Yineleme sayısını güncelleştirmek için dağıtımı yeniden edin](./media/kubernetes-dashboard/edit-deployment.png)

Yeni bölmelerin bir yineleme kümesi içinde oluşturulması birkaç dakika sürer. Sol menüde **Çoğaltma Kümeleri'ni**seçin ve ardından *nginx* çoğaltma setinizi seçin. Bakla listesi şimdi aşağıdaki örnek çıktıda gösterildiği gibi güncelleştirilmiş yineleme sayısını yansıtır:

![Yineleme kümesindeki bilgileri görüntüleme](./media/kubernetes-dashboard/view-replica-set.png)

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes panosu hakkında daha fazla bilgi için [Kubernetes Web Kullanıcı Arabirimi Panosu'na][kubernetes-dashboard]bakın.

<!-- LINKS - external -->
[dashboard-authentication]: https://github.com/kubernetes/dashboard/wiki/Access-control
[kubeconfig-file]: https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/
[kubectl-create-clusterrolebinding]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-clusterrolebinding-em-
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubernetes-dashboard]: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

<!-- LINKS - internal -->
[aad-cluster]: ./azure-ad-integration-cli.md
[aks-quickstart]: ./kubernetes-walkthrough.md
[aks-service-accounts]: ./concepts-identity.md#kubernetes-service-accounts
[az-account-get-access-token]: /cli/azure/account?view=azure-cli-latest#az-account-get-access-token
[az-aks-browse]: /cli/azure/aks#az-aks-browse
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[install-azure-cli]: /cli/azure/install-azure-cli
