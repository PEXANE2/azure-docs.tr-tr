---
title: Web panosu ile bir Azure Kubernetes hizmet kümesini yönetme
description: Azure Kubernetes hizmeti (AKS) kümesini yönetmek için yerleşik Kubernetes Web Kullanıcı arabirimi panosunu nasıl kullanacağınızı öğrenin
services: container-service
author: mlearned
ms.topic: article
ms.date: 06/03/2020
ms.author: mlearned
ms.openlocfilehash: 40de6f4084630839a0161891ff80f7e4cabc1db7
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84345129"
---
# <a name="access-the-kubernetes-web-dashboard-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) ile Kubernetes web panosuna erişme

Kubernetes, temel yönetim işlemleri için kullanılabilen bir Web panosu içerir. Bu pano uygulamalarınızın temel sistem durumunu ve ölçümlerini görüntülemenize, hizmetler oluşturmanıza ve dağıtmanıza ve mevcut uygulamaları düzenlemenize olanak tanır. Bu makalede, Azure CLı kullanarak Kubernetes panosuna nasıl erişebileceğiniz gösterilmektedir ve sonra bazı temel Pano işlemlerinde size kılavuzluk eder.

Kubernetes panosu hakkında daha fazla bilgi için bkz. [Kubernetes Web UI panosu][kubernetes-dashboard]. AKS, açık kaynak panonun 2,0 sürümünü ve üstünü kullanır.

## <a name="before-you-begin"></a>Başlamadan önce

Bu belgede açıklanan adımlarda bir AKS kümesi oluşturduğunuz ve kümeyle bir bağlantı oluşturmuş olduğunuz varsayılmaktadır `kubectl` . AKS kümesi oluşturmanız gerekiyorsa bkz. [aks hızlı][aks-quickstart]başlangıcı.

Ayrıca Azure CLı sürüm 2.6.0 veya üzeri yüklü ve yapılandırılmış olmalıdır. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse bkz. [Azure CLI 'Yı yüklemek][install-azure-cli].

## <a name="start-the-kubernetes-dashboard"></a>Kubernetes panosunu başlatma

> [!WARNING]
> **Yerleşik Pano eklentisi kullanımdan kaldırılması için ayarlanmıştır.** Şu anda, Kubernetes panosu 1,18 'den daha az bir Kubernetes sürümü çalıştıran tüm kümeler için varsayılan olarak etkindir.
> Bu pano eklentisi, Kubernetes 1,18 veya üzeri üzerinde oluşturulan tüm yeni kümeler için varsayılan olarak devre dışı bırakılır. Önizleme aşamasında Kubernetes 1,19 kullanılabilirliği ile başlayarak, AKS artık yönetilen kuin-Dashboard eklentisi yüklemesini desteklememektedir. Eklentinin zaten yüklü olduğu mevcut kümeler etkilenmeyecektir. Kullanıcılar, açık kaynak panosunu Kullanıcı tarafından yüklenen yazılım olarak el ile yüklemeye devam edecektir.

Bir kümede Kubernetes panosunu başlatmak için [az aks zat][az-aks-browse] komutunu kullanın. Bu komut, Kubernetes 1,18 ' den eski herhangi bir sürümü çalıştıran kümeler üzerinde varsayılan olarak bulunan kuin-Dashboard eklenti yüklemesini gerektirir.

Aşağıdaki örnek, *Myresourcegroup*adlı kaynak grubunda *Myakscluster* adlı küme için panoyu açar:

```azurecli
az aks browse --resource-group myResourceGroup --name myAKSCluster
```

Bu komut, geliştirme sisteminiz ile Kubernetes API 'niz arasında bir ara sunucu oluşturur ve Kubernetes panosuna bir Web tarayıcısı açar. Bir Web tarayıcısı Kubernetes panosuna açılmazsa, genellikle Azure CLı 'de belirtilen URL adresini kopyalayıp yapıştırın `http://127.0.0.1:8001` .

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

> [!IMPORTANT]
> If your AKS cluster uses RBAC, a *ClusterRoleBinding* must be created before you can correctly access the dashboard. By default, the Kubernetes dashboard is deployed with minimal read access and displays RBAC access errors. The Kubernetes dashboard does not currently support user-provided credentials to determine the level of access, rather it uses the roles granted to the service account. A cluster administrator can choose to grant additional access to the *kubernetes-dashboard* service account, however this can be a vector for privilege escalation. You can also integrate Azure Active Directory authentication to provide a more granular level of access.
> 
> To create a binding, use the [kubectl create clusterrolebinding][kubectl-create-clusterrolebinding] command. The following example shows how to create a sample binding, however, this sample binding does not apply any additional authentication components and may lead to insecure use. The Kubernetes dashboard is open to anyone with access to the URL. Do not expose the Kubernetes dashboard publicly.
>
> ```console
> kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
> ```
> 
> For more information on using the different authentication methods, see the Kubernetes dashboard wiki on [access controls][dashboard-authentication].
-->

## <a name="login-to-the-dashboard"></a>Panoda oturum açın

> [!IMPORTANT]
> [Kubernetes panosunun v 1.10.1](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1) itibariyle, [Bu sürümdeki bir güvenlik düzeltmesinin](https://github.com/kubernetes/dashboard/pull/3400)olmaması nedeniyle "Kubernetes-Dashboard" hizmet hesabı artık kaynakları almak için kullanılamaz. Sonuç olarak, kimlik doğrulama bilgileri olmayan istekler 401 Yetkisiz bir hata döndürür. Bir hizmet hesabından alınan bir taşıyıcı belirteci, bu [Kubernetes panosu örneğinde](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/#accessing-the-dashboard-ui)olduğu gibi hala kullanılabilir, ancak bu, eski sürümlere kıyasla Pano eklentisinin oturum açma akışını etkiler.

Sunulan ilk ekran bir kubeconfig veya Token gerektiriyor. Her iki seçenek de, bu kaynakları panoda göstermek için kaynak izinleri gerektirir.

![oturum açma ekranı](./media/kubernetes-dashboard/login.png)

**Kubeconfig kullanma**
1. Admin kubeconfig 'i ile ayarlama`az aks get-credentials -a --resource-group <RG_NAME> --name <CLUSTER_NAME>`
1. `Kubeconfig` `Choose kubeconfig file` Dosya seçicisini açmak için seçin ve tıklayın
1. Kubeconfig dosyanızı seçin (varsayılan olarak $HOME/5kube/config değerini alır)
1. Şuna tıklayın: `Sign In`

**Belirteç kullanma**
1. `kubectl config view` öğesini çalıştırın
1. Kümenizin hesabıyla ilişkili istenen belirteci kopyalayın
1. Oturum açma sırasında belirteç seçeneğine yapıştırın
1. Şuna tıklayın: `Sign In`

Başarılı olduktan sonra aşağıdakine benzer bir sayfa görüntülenir.

![Kubernetes Web panosunun genel bakış sayfası](./media/kubernetes-dashboard/dashboard-overview.png)

## <a name="create-an-application"></a>Uygulama oluşturma

Aşağıdaki adımlar birçok kaynak için izinler gerektirir. Bu özellikleri sınarken bir yönetici hesabı kullanmanız önerilir.

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
1. `spec.replica`Değeri, 20. satırın etrafında bulun. Uygulamanın çoğaltma sayısını artırmak için bu değeri *1* ' den *3*' e değiştirin.
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
