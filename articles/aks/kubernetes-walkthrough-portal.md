---
title: Hızlı başlangıç-portalda bir Azure Kubernetes hizmeti (AKS) kümesi oluşturma
description: Hızlı bir şekilde bir Kubernetes kümesi oluşturmayı, bir uygulamayı dağıtmayı ve Azure portal kullanarak Azure Kubernetes hizmeti 'nde (AKS) performansı izlemenizi öğrenin.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: quickstart
ms.date: 5/31/2019
ms.author: mlearned
ms.custom: mvc, seo-javascript-october2019
ms.openlocfilehash: 86d7f74a5a7260c5feb9a41c6b9c3c93d61388a6
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72255547"
---
# <a name="quickstart-deploy-an-azure-kubernetes-service-aks-cluster-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak bir Azure Kubernetes hizmeti (AKS) kümesi dağıtma

Azure Kubernetes hizmeti (AKS), kümelerinizi hızlı bir şekilde dağıtmanıza ve yönetmenize olanak tanıyan bir yönetilen Kubernetes hizmetidir. Bu hızlı başlangıçta, Azure portal kullanarak bir AKS kümesi dağıtırsınız. Bir Web ön ucu ve bir Reda örneği içeren çok kapsayıcılı bir uygulama kümede çalıştırılır. Ardından, uygulamanızı çalıştıran kümenin ve yığınların durumunu nasıl izleyeceğinizi görürsünüz.

![Azure oy örnek uygulamasına göz atma görüntüsü](media/container-service-kubernetes-walkthrough/azure-voting-application.png)

Bu hızlı başlangıçta, Kubernetes kavramlarının temel bir şekilde anlaşıldığı varsayılır. Daha fazla bilgi için bkz. [Azure Kubernetes hizmeti (AKS) Için Kubernetes temel kavramları][kubernetes-concepts].

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure 'da oturum açın

Azure portal https://portal.azure.com ' da oturum açın.

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

Azure portal sol üst köşesinde +-3**Kubernetes hizmeti**@no__t kaynak  > **kapsayıcı** **Oluştur**' u seçin.

AKS kümesi oluşturmak için aşağıdaki adımları izleyin:

1. **Temel bilgiler** sayfasında, aşağıdaki seçenekleri yapılandırın:
   - *Proje ayrıntıları*: bir Azure aboneliği seçin, sonra *Myresourcegroup*gibi bir Azure Kaynak grubu seçin veya oluşturun. *Myakscluster*gibi bir **Kubernetes küme adı**girin.
   - *Küme ayrıntıları*: aks kümesi için bir bölge, Kubernetes sürümü ve DNS adı ön eki seçin.
   - **BIRINCIL düğüm havuzu**: aks düğümleri IÇIN bir VM boyutu seçin. Bir AKS kümesi dağıtıldıktan sonra **VM boyutu değiştirilemez** . 
       - Kümeye dağıtılacak düğüm sayısını seçin. Bu hızlı başlangıç için **düğüm sayısını** *1*olarak ayarlayın. Düğüm sayısı, küme dağıtıldıktan **sonra ayarlanabilir.**
    
     ![AKS kümesi oluşturma-temel bilgileri sağlama](media/kubernetes-walkthrough-portal/create-cluster-basics.png)

     **İleri ' yi seçin: tamamlandığında ölçeklendirin** .

2. **Ölçek** sayfasında, varsayılan seçenekleri değiştirmeyin. Ekranın alt kısmındaki **İleri: kimlik doğrulama**' ya tıklayın.
> [!CAUTION]
> Yeni AAD hizmet sorumlularının oluşturulması, yayma ve kullanılabilir hale gelmesi birkaç dakika sürebilir ve bu durum, Azure portal ' de hizmet sorumlusu hata ve doğrulama hataları oluşmasına neden olur. Bu şekilde karşılaşırsanız, azaltma işlemleri için lütfen [buraya](troubleshooting.md#im-receiving-errors-that-my-service-principal-was-not-found-when-i-try-to-create-a-new-cluster-without-passing-in-an-existing-one) gidin.
3. **Kimlik doğrulama** sayfasında, aşağıdaki seçenekleri yapılandırın:
   - **Hizmet sorumlusu** alanını **(yeni) varsayılan hizmet sorumlusu**ile bırakarak yeni bir hizmet sorumlusu oluşturun. Alternatif olarak, mevcut bir tane kullanmak için *hizmet sorumlusu Yapılandır* ' ı seçebilirsiniz. Mevcut bir tane kullanırsanız, SPN istemci KIMLIĞINI ve parolasını sağlamanız gerekir.
   - Kubernetes rol tabanlı erişim denetimleri (RBAC) için seçeneği etkinleştirin. Bu, AKS kümenizde dağıtılan Kubernetes kaynaklarına erişim üzerinde daha ayrıntılı denetim sağlar.

Varsayılan olarak, *temel* ağ kullanılır ve kapsayıcılar Için Azure izleyici etkindir. **Gözden geçir + oluştur** ' a tıklayın ve doğrulama tamamlandığında **Oluştur** ' a tıklayın.

AKS kümesini oluşturmak birkaç dakika sürer. Dağıtımınız tamamlandığında **Kaynağa Git ' e**tıklayın veya *MYRESOURCEGROUP*gibi aks kümesi kaynak grubuna gidin ve *myakscluster*gibi aks kaynağını seçin. AKS kümesi panosu, aşağıdaki örnekte olduğu gibi gösterilir:

![Azure portal örnek AKS panosu](media/kubernetes-walkthrough-portal/aks-portal-dashboard.png)

## <a name="connect-to-the-cluster"></a>Kümeye Bağlan

Kubernetes kümesini yönetmek için Kubernetes komut satırı istemcisi olan [kubectl][kubectl]'yi kullanırsınız. @No__t-0 istemcisi Azure Cloud Shell önceden yüklenir.

Azure portal en üstündeki `>_` düğmesini kullanarak Cloud Shell açın.

![Portalda Azure Cloud Shell açın](media/kubernetes-walkthrough-portal/aks-cloud-shell.png)

@No__t, Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks Get-Credentials][az-aks-get-credentials] komutunu kullanın. Bu komut, kimlik bilgilerini indirir ve Kubernetes CLı 'yi bunları kullanacak şekilde yapılandırır. Aşağıdaki örnek, *Myresourcegroup*adlı kaynak grubundaki *Myakscluster* küme adı için kimlik bilgilerini alır:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenizin bağlantısını doğrulamak için [kubectl Get][kubectl-get] komutunu kullanarak küme düğümlerinin bir listesini döndürün.

```azurecli-interactive
kubectl get nodes
```

Aşağıdaki örnek çıktıda, önceki adımlarda oluşturulan tek düğüm gösterilmektedir. Düğüm *durumunun olduğundan emin olun:*

```
NAME                       STATUS    ROLES     AGE       VERSION
aks-agentpool-14693408-0   Ready     agent     15m       v1.11.5
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Bir Kubernetes bildirim dosyası, küme için, hangi kapsayıcı görüntülerinin çalıştırılacağı gibi istenen durumu tanımlar. Bu hızlı başlangıçta, Azure oy uygulamasını çalıştırmak için gerekli tüm nesneleri oluşturmak için bir bildirim kullanılır. Bu bildirimde iki [Kubernetes dağıtımı][kubernetes-deployment] vardır-bir örnek Azure oy Python uygulamaları ve diğeri de redin örneği için. İki [Kubernetes hizmeti][kubernetes-service] de oluşturulur; redsıs örneği için bir iç hizmet ve Azure oy uygulamasına internet 'ten erişmek için bir dış hizmet.

> [!TIP]
> Bu hızlı başlangıçta, uygulama bildirimlerinizi el ile oluşturarak AKS kümesine dağıtırsınız. Daha çok gerçek dünyada senaryolar için [Azure dev Spaces][azure-dev-spaces] kullanarak doğrudan aks kümesinde kodunuzun hatalarını hızla yineleyebilirsiniz ve hatalarını ayıklayabilirsiniz. İşletim sistemi platformları ve geliştirme ortamlarında geliştirme alanlarını kullanabilir ve takımınızda başkalarıyla birlikte çalışabilirsiniz.

Cloud Shell 'de, `nano` veya `vi` ' i kullanarak `azure-vote.yaml` adlı bir dosya oluşturun ve aşağıdaki YAML tanımına kopyalayın:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-back
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": linux
      containers:
      - name: azure-vote-back
        image: redis
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-front
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": linux
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:v1
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

[Kubectl Apply][kubectl-apply] komutunu kullanarak uygulamayı dağıtın ve YAML bildiriminizde adı belirtin:

```azurecli-interactive
kubectl apply -f azure-vote.yaml
```

Aşağıdaki örnek çıktıda başarıyla oluşturulan dağıtımlar ve hizmetler gösterilmektedir:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>Uygulamayı test etme

Uygulama çalıştığında, bir Kubernetes hizmeti, uygulamanın ön ucuna internet 'e koyar. Bu işlemin tamamlanması birkaç dakika sürebilir.

İlerlemeyi izlemek için, [kubectl Get Service][kubectl-get] komutunu `--watch` bağımsız değişkeniyle birlikte kullanın.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

Başlangıçta *Azure-oyönme* hizmeti IÇIN *dış IP* , *Beklemede*olarak gösterilir.

```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

*Dış IP* adresi *bekliyor* durumundan gerçek ortak ıp adresini değiştirdiğinde, `kubectl` izleme işlemini durdurmak için `CTRL-C` ' yi kullanın. Aşağıdaki örnek çıktıda, hizmete atanmış geçerli bir genel IP adresi gösterilmektedir:

```
azure-vote-front   LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Azure oy uygulamasını çalışırken görmek için, hizmetinizin dış IP adresine bir Web tarayıcısı açın.

![Azure oy örnek uygulamasına göz atma görüntüsü](media/container-service-kubernetes-walkthrough/azure-voting-application.png)

## <a name="monitor-health-and-logs"></a>Sistem durumunu ve günlükleri izleme

Kümeyi oluşturduğunuzda, kapsayıcılar için Azure Izleyici etkinleştirilmiştir. Bu izleme özelliği, küme üzerinde çalışan aks kümesi ve pod için sistem durumu ölçümleri sağlar.

Bu verilerin Azure portal doldurması birkaç dakika sürebilir. Azure oyları için geçerli durumu, çalışma süresini ve kaynak kullanımını görmek için, Azure portal *Myakscluster*gibi aks kaynağına geri gidin. Daha sonra sistem durumuna aşağıdaki gibi erişebilirsiniz:

1. Sol taraftaki **izleme** altında **Öngörüler** ' i seçin.
1. Üst kısımda, seçin **+ Filtre Ekle**
1. Özellik olarak *ad alanı* ' nı seçin, sonra *\<All, kuas-sistem @ no__t-3* ' ü seçin.
1. **Kapsayıcıları**görüntülemeyi seçin.

Aşağıdaki örnekte gösterildiği gibi *Azure-oy-geri* ve *Azure-oyönü ön* kapsayıcıları görüntülenir:

![AKS 'de çalışan kapsayıcıların sistem durumunu görüntüleme](media/kubernetes-walkthrough-portal/monitor-containers.png)

@No__t-0 Pod için günlükleri görmek üzere kapsayıcılar listesinin açılan listesinden **kapsayıcı günlüklerini görüntüle** ' yi seçin. Bu Günlükler, kapsayıcıdan *stdout* ve *stderr* akışlarını içerir.

![AKS 'teki kapsayıcılar günlüklerini görüntüleme](media/kubernetes-walkthrough-portal/monitor-container-logs.png)

## <a name="delete-cluster"></a>Kümeyi Sil

Küme artık gerekli olmadığında, tüm ilişkili kaynakları silen küme kaynağını silin. Bu işlem, AKS kümesi panosundaki **Sil** düğmesi seçilerek Azure Portal tamamlanabilir. Alternatif olarak, [az aks Delete][az-aks-delete] komutu Cloud Shell kullanılabilir:

```azurecli-interactive
az aks delete --resource-group myResourceGroup --name myAKSCluster --no-wait
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz. [aks hizmet sorumlusu konuları ve silme][sp-delete].

## <a name="get-the-code"></a>Kodu alın

Bu hızlı başlangıçta, bir Kubernetes dağıtımı oluşturmak için önceden oluşturulmuş kapsayıcı görüntüleri kullanıldı. İlgili uygulama kodu, Dockerfile ve Kubernetes bildirim dosyası GitHub ' da kullanılabilir.

[https://github.com/Azure-Samples/azure-voting-app-redis][azure-vote-app]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Kubernetes kümesi dağıttınız ve bu kümeye çok kapsayıcılı bir uygulama dağıttınız.

AKS hakkında daha fazla bilgi edinmek ve dağıtım örneği için bir bütün kod üzerinde gezinmek için Kubernetes küme öğreticisine geçin.

> [!div class="nextstepaction"]
> [AKS öğreticisi][aks-tutorial]

<!-- LINKS - external -->
[azure-vote-app]: https://github.com/Azure-Samples/azure-voting-app-redis.git
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-documentation]: https://kubernetes.io/docs/home/

<!-- LINKS - internal -->
[kubernetes-concepts]: concepts-clusters-workloads.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-delete]: /cli/azure/aks#az-aks-delete
[aks-monitor]: ../monitoring/monitoring-container-health.md
[aks-network]: ./concepts-network.md
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[http-routing]: ./http-application-routing.md
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[azure-dev-spaces]: https://docs.microsoft.com/azure/dev-spaces/
[kubernetes-deployment]: concepts-clusters-workloads.md#deployments-and-yaml-manifests
[kubernetes-service]: concepts-network.md#services
