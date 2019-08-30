---
title: Web panosu ile bir Azure Kubernetes hizmet kümesini yönetme
description: Azure Kubernetes hizmeti (AKS) kümesini yönetmek için yerleşik Kubernetes Web Kullanıcı arabirimi panosunu nasıl kullanacağınızı öğrenin
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 10/08/2018
ms.author: mlearned
ms.openlocfilehash: 5aa8268fee7d43ad13ea8710760ba493683f502e
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70126871"
---
# <a name="access-the-kubernetes-web-dashboard-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) ile Kubernetes web panosuna erişme

Kubernetes, temel yönetim işlemleri için kullanılabilen bir Web panosu içerir. Bu pano uygulamalarınızın temel sistem durumunu ve ölçümlerini görüntülemenize, hizmetler oluşturmanıza ve dağıtmanıza ve mevcut uygulamaları düzenlemenize olanak tanır. Bu makalede, Azure CLı kullanarak Kubernetes panosuna nasıl erişebileceğiniz gösterilmektedir ve sonra bazı temel Pano işlemlerinde size kılavuzluk eder.

Kubernetes panosu hakkında daha fazla bilgi için bkz. [Kubernetes Web UI panosu][kubernetes-dashboard].

## <a name="before-you-begin"></a>Başlamadan önce

Bu belgede açıklanan adımlarda bir aks kümesi oluşturduğunuz ve kümeyle bir `kubectl` bağlantı oluşturmuş olduğunuz varsayılmaktadır. AKS kümesi oluşturmanız gerekiyorsa bkz. [aks hızlı][aks-quickstart]başlangıcı.

Ayrıca Azure CLI sürüm 2.0.46 veya üzerini yüklemiş ve yapılandırmış olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse bkz. [Azure CLI 'Yı yüklemek][install-azure-cli].

## <a name="start-the-kubernetes-dashboard"></a>Kubernetes panosunu başlatma

Kubernetes panosunu başlatmak için [az aks zat][az-aks-browse] komutunu kullanın. Aşağıdaki örnek, *Myresourcegroup*adlı kaynak grubunda *Myakscluster* adlı küme için panoyu açar:

```azurecli
az aks browse --resource-group myResourceGroup --name myAKSCluster
```

Bu komut, geliştirme sisteminiz ile Kubernetes API 'niz arasında bir ara sunucu oluşturur ve Kubernetes panosuna bir Web tarayıcısı açar. Bir Web tarayıcısı Kubernetes panosuna açılmazsa, genellikle `http://127.0.0.1:8001`Azure CLı 'de belirtilen URL adresini kopyalayıp yapıştırın.

![Kubernetes Web panosunun oturum açma sayfası](./media/kubernetes-dashboard/dashboard-login.png)

Kümenizin panosunda oturum açmak için aşağıdaki seçenekleriniz vardır:

* Bir [kubeconfig dosyası][kubeconfig-file]. [Az aks Get-Credentials][az-aks-get-credentials]komutunu kullanarak bir kubeconfig dosyası oluşturabilirsiniz.
* [Hizmet hesabı belirteci][aks-service-accounts] veya kullanıcı belirteci gibi bir belirteç. [AAD etkin kümeler][aad-cluster]üzerinde bu BELIRTEÇ bir AAD belirteci olur. Kubeconfig `kubectl config view` dosyanızdaki belirteçleri listelemek için öğesini kullanabilirsiniz. AKS kümesiyle kullanılmak üzere bir AAD belirteci oluşturma hakkında daha fazla bilgi için bkz. [Azure CLI kullanarak Azure Kubernetes hizmeti ile Azure Active Directory tümleştirme][aad-cluster].
* *Atla*' ya tıkladığınızda kullanılan varsayılan pano hizmeti hesabı.

> [!WARNING]
> Kullanılan kimlik doğrulama yönteminden bağımsız olarak, Kubernetes panosunu herkese açık bir şekilde kullanıma sunmayın.
> 
> Kubernetes panosu için kimlik doğrulaması ayarlarken, varsayılan pano hizmeti hesabı üzerinde bir belirteç kullanmanız önerilir. Bir belirteç, her kullanıcının kendi izinlerini kullanmasına izin verir. Varsayılan pano hizmeti hesabını kullanmak, bir kullanıcının kendi izinlerini atlamasına ve bunun yerine hizmet hesabını kullanmasına izin verebilir.
> 
> Varsayılan pano hizmeti hesabını kullanmayı seçerseniz ve AKS kümeniz RBAC kullanıyorsa, panoya doğru şekilde erişebilmeniz için bir *Clusterrolebinding* oluşturulması gerekir. Varsayılan olarak, Kubernetes panosu, en az okuma erişimiyle dağıtılır ve RBAC erişim hatalarını görüntüler. Bir Küme Yöneticisi, *Kubernetes-Dashboard* hizmet hesabına ek erişim izni vermeyi seçebilir ancak bu, ayrıcalık yükseltme için bir vektör olabilir. Ayrıca, daha ayrıntılı bir erişim düzeyi sağlamak için Azure Active Directory kimlik doğrulamasını tümleştirebilirsiniz.
>
> Bir bağlama oluşturmak için aşağıdaki örnekte gösterildiği gibi [kubectl Create clusterrolebinding][kubectl-create-clusterrolebinding] komutunu kullanın. **Bu örnek bağlama, ek kimlik doğrulama bileşenleri uygulamaz ve güvenli olmayan kullanıma yol açabilir.**
>
> ```console
> kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
> ```
> 
> Artık, RBAC özellikli kümenizdeki Kubernetes panosuna erişebilirsiniz. Kubernetes panosunu başlatmak için, önceki adımda açıklandığı gibi [az aks zat][az-aks-browse] komutunu kullanın.
>
> Kümeniz RBAC kullanmıyorsa, *Clusterrolebinding*oluşturulması önerilmez.
> 
> Farklı kimlik doğrulama yöntemlerini kullanma hakkında daha fazla bilgi için [erişim denetimlerinde][dashboard-authentication]Kubernetes Pano wiki bölümüne bakın.

Oturum açmak için bir yöntem seçtikten sonra Kubernetes panosu görüntülenir. *Belirteç* veya *atlama*kullanmayı seçerseniz, Kubernetes panosu kümeye erişmek için o anda oturum açmış kullanıcının izinlerini kullanacaktır.

![Kubernetes Web panosunun genel bakış sayfası](./media/kubernetes-dashboard/dashboard-overview.png)

## <a name="create-an-application"></a>Uygulama oluşturma

Kubernetes panosunun yönetim görevlerinin karmaşıklığını ne şekilde azaltabilmesine bakmak için bir uygulama oluşturalım. Metin girişi, YAML dosyası veya grafik Sihirbazı aracılığıyla Kubernetes panosundan bir uygulama oluşturabilirsiniz.

Bir uygulama oluşturmak için aşağıdaki adımları izleyin:

1. Sağ üst penceredeki **Oluştur** düğmesini seçin.
1. Grafik Sihirbazı 'nı kullanmak için, **bir uygulama oluşturmayı**seçin.
1. Dağıtım için *NGINX* gibi bir ad sağlayın
1. Kullanılacak kapsayıcı resminin adını girin, örneğin *NGINX: 1.15.5*
1. Web trafiği için 80 bağlantı noktasını kullanıma sunmak üzere bir Kubernetes hizmeti oluşturursunuz. **Hizmet**altında, **dış**' i seçin ve hem bağlantı noktası hem de hedef bağlantı noktası için **80** girin.
1. Hazırlandığınızda, uygulamayı oluşturmak için **Dağıt** ' ı seçin.

![Kubernetes Web panosunda uygulama dağıtma](./media/kubernetes-dashboard/create-app.png)

Kubernetes hizmetine genel bir dış IP adresinin atanması için bir dakika veya iki zaman alır. Sol boyuttaki **bulma ve yük dengeleme** altında **Hizmetler**' i seçin. Aşağıdaki örnekte gösterildiği gibi, uygulamanızın hizmeti *dış uç noktalar*dahil olmak üzere listelenir:

![Hizmet ve uç noktaların listesini görüntüle](./media/kubernetes-dashboard/view-services.png)

Varsayılan NGıNX sayfasına bir Web tarayıcı penceresi açmak için uç nokta adresini seçin:

![Dağıtılan uygulamanın varsayılan NGıNX sayfasını görüntüleme](./media/kubernetes-dashboard/default-nginx.png)

## <a name="view-pod-information"></a>Pod bilgilerini görüntüle

Kubernetes panosu, Günlükler gibi temel izleme ölçümleri ve sorun giderme bilgileri sağlayabilir.

Uygulama yığınlarınız hakkında daha fazla bilgi görmek için sol taraftaki menüden **pods** ' yi seçin. Kullanılabilir Pod 'lerin listesi gösterilir. Kaynak tüketimi gibi bilgileri görüntülemek için *NGINX* Pod 'nizi seçin:

![Pod bilgilerini görüntüle](./media/kubernetes-dashboard/view-pod-info.png)

## <a name="edit-the-application"></a>Uygulamayı düzenleme

Uygulamaları oluşturmanın ve görüntülemenin yanı sıra, Kubernetes panosu, uygulama dağıtımlarını düzenlemek ve güncelleştirmek için kullanılabilir. Uygulama için ek artıklık sağlamak üzere NGıNX çoğaltmalarının sayısını artalım.

Bir dağıtımı düzenlemek için:

1. Sol taraftaki menüden **dağıtımlar** ' ı seçin ve ardından *NGINX* dağıtımınızı seçin.
1. Sağ üst gezinti çubuğunda **Düzenle** ' yi seçin.
1. `spec.replica` Değeri, 20. satırın etrafında bulun. Uygulamanın çoğaltma sayısını artırmak için bu değeri *1* ' den *3*' e değiştirin.
1. Hazırsanız **Güncelleştir** ' i seçin.

![Kopyaların sayısını güncelleştirmek için dağıtımı düzenleyin](./media/kubernetes-dashboard/edit-deployment.png)

Yeni Pod 'lerin bir çoğaltma kümesi içinde oluşturulması birkaç dakika sürer. Sol taraftaki menüde, **çoğaltma kümeleri**' ni seçin ve ardından *NGINX* çoğaltma kümesini seçin. Aşağıdaki örnek çıktıda gösterildiği gibi, Pod listesi artık güncelleştirilmiş çoğaltma sayısını yansıtır:

![Çoğaltma kümesi hakkındaki bilgileri görüntüle](./media/kubernetes-dashboard/view-replica-set.png)

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes panosu hakkında daha fazla bilgi için bkz. [Kubernetes Web UI panosu][kubernetes-dashboard].

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
