---
title: 'Hızlı başlangıç: Azure portal kullanarak AKS kümesi dağıtma'
titleSuffix: Azure Kubernetes Service
description: Hızlı bir şekilde bir Kubernetes kümesi oluşturmayı, bir uygulamayı dağıtmayı ve Azure portal kullanarak Azure Kubernetes hizmeti 'nde (AKS) performansı izlemenizi öğrenin.
services: container-service
ms.topic: quickstart
ms.date: 01/13/2021
ms.custom: mvc, seo-javascript-october2019, contperfq3
ms.openlocfilehash: 5f758c0bc50b2d4f22b3dbf0efaa4ecbc3f334cb
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102507815"
---
# <a name="quickstart-deploy-an-azure-kubernetes-service-aks-cluster-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak bir Azure Kubernetes hizmeti (AKS) kümesi dağıtma

Azure Kubernetes hizmeti (AKS), kümelerinizi hızlı bir şekilde dağıtmanıza ve yönetmenize olanak tanıyan bir yönetilen Kubernetes hizmetidir. Bu hızlı başlangıçta, Azure portalını kullanarak bir AKS kümesi dağıtırsınız. Bir Web ön ucu ve bir Reda örneği içeren çok kapsayıcılı bir uygulama kümede çalıştırılır. Ardından, uygulamanızı çalıştıran kümenin ve yığınların durumunu nasıl izleyeceğinizi görürsünüz.

![Azure Vote örnek uygulamasına göz atma görüntüsü](media/container-service-kubernetes-walkthrough/azure-voting-application.png)

Bu hızlı başlangıç, Kubernetes kavramlarının temel olarak bilindiğini varsayar. Daha fazla bilgi için bkz. [Azure Kubernetes hizmeti (AKS) Için Kubernetes temel kavramları][kubernetes-concepts].

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

AKS kümesi oluşturmak için aşağıdaki adımları tamamlayın:

1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**’u seçin.

2. **Kapsayıcılar** >  **Kubernetes Hizmeti** seçeneklerini belirleyin.

3. **Temel Bilgiler** sayfasında aşağıdaki seçenekleri yapılandırın:
    - **Proje ayrıntıları**: bir Azure **aboneliği** seçin, sonra *Myresourcegroup* gibi bir Azure **kaynak grubu** seçin veya oluşturun.
    - **Küme ayrıntıları**: *Myakscluster* gibi bir **Kubernetes küme adı** girin. AKS kümesi için bir **bölge** ve **Kubernetes sürümü** seçin.
    - **Birincil düğüm havuzu**: aks düğümleri IÇIN bir VM **düğüm boyutu** seçin. Bir AKS kümesi dağıtıldıktan sonra *VM boyutu değiştirilemez* .
            -Kümeye dağıtılacak düğüm sayısını seçin. Bu hızlı başlangıç **Düğüm sayısı** değerini *1* olarak belirleyin. Küme dağıtıldıktan sonra düğüm sayısı *ayarlanabilir*.
    
    ![AKS kümesi oluşturma - temel bilgileri sağlama](media/kubernetes-walkthrough-portal/create-cluster-basics.png)

    **İleri ' yi seçin: tamamlandığında düğüm havuzları** .

4. **Düğüm havuzları** sayfasında, varsayılan seçenekleri değiştirmeyin. Ekranın alt kısmındaki **İleri: kimlik doğrulama**' ya tıklayın.
    > [!CAUTION]
    > Yeni küme kimliği oluşturmak, yaymak ve kullanılabilir hale gelmesi, Azure portal ' de hizmet sorumlusu hata ve doğrulama hataları oluşmasına neden olacak birkaç dakika sürebilir. Bu duruma ulaşırsanız, azaltma için [Genel Azure Kubernetes hizmet sorunlarını giderme](troubleshooting.md#received-an-error-saying-my-service-principal-wasnt-found-or-is-invalid-when-i-try-to-create-a-new-cluster) makalesini ziyaret edin.

5. **Kimlik Doğrulaması** sayfasında aşağıdaki seçenekleri yapılandırın:
    - **Kimlik doğrulama** alanını **sistem tarafından alınan yönetilen kimlikle** bırakarak yeni bir küme kimliği oluşturun. Alternatif olarak, hizmet sorumlusu kullanmak için **hizmet sorumlusu** ' nı seçebilirsiniz. Varsayılan hizmet sorumlusu oluşturmak için *(yeni) varsayılan hizmet sorumlusu* ' nı seçin veya mevcut olanı kullanmak için *hizmet sorumlusu* ' nı yapılandırın. Mevcut bir tane kullanırsanız, SPN istemci KIMLIĞINI ve parolasını sağlamanız gerekir.
    - Kubernetes rol tabanlı erişim denetimi (Kubernetes RBAC) için seçeneği etkinleştirin. Böylece, AKS kümenizde görüntülenen Kubernetes kaynaklarına erişim konusunda daha ayrıntılı denetim sunulur.

Varsayılan olarak, *temel* ağ kullanılır ve kapsayıcılar Için Azure izleyici etkindir. Doğrulama tamamlandığında **İncele ve oluştur**’a ve sonra **Oluştur**’a tıklayın.

AKS kümesinin oluşturulması birkaç dakika sürer. Dağıtımınız tamamlandığında **Kaynağa Git ' e** tıklayın veya *MYRESOURCEGROUP* gibi aks kümesi kaynak grubuna gidin ve *myakscluster* gibi aks kaynağını seçin. AKS kümesi panosu, aşağıdaki örnekte olduğu gibi gösterilir:

![Azure portalda örnek AKS panosu](media/kubernetes-walkthrough-portal/aks-portal-dashboard.png)

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Kubernetes kümesini yönetmek için Kubernetes komut satırı istemcisi olan [kubectl][kubectl]'yi kullanırsınız. `kubectl` istemcisi Azure Cloud Shell’de önceden yüklüdür.

`>_`Azure Portal en üstündeki düğmeyi kullanarak Cloud Shell açın.

![Azure Cloud Shell'i portalda açma](media/kubernetes-walkthrough-portal/aks-cloud-shell.png)

> [!NOTE]
> Bu işlemleri yerel bir Shell yüklemesinde gerçekleştirmek için önce Azure CLı 'nin yüklü olduğunu doğrulamanız ve sonra komut aracılığıyla Azure 'a bağlanmanız gerekir `az login` .

`kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][az-aks-get-credentials] komutunu kullanın. Bu komut, kimlik bilgilerini indirir ve Kubernetes CLı 'yi bunları kullanacak şekilde yapılandırır. Aşağıdaki örnek *myResourceGroup* adlı kaynak grubu içindeki *myAKSCluster* adlı kümenin kimlik bilgilerini alır:

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenizin bağlantısını doğrulamak için `kubectl get` komutunu kullanarak küme düğümlerinin bir listesini döndürün.

```console
kubectl get nodes
```

Aşağıdaki örnekte önceki adımlarda oluşturulan tek düğüm gösterilmiştir. Düğüm *durumunun olduğundan emin olun:*

```output
NAME                       STATUS    ROLES     AGE       VERSION
aks-agentpool-14693408-0   Ready     agent     15m       v1.11.5
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Bir Kubernetes bildirim dosyası, küme için, hangi kapsayıcı görüntülerinin çalıştırılacağı gibi istenen durumu tanımlar. Bu hızlı başlangıçta, Azure Vote uygulamasını çalıştırmak için gerekli tüm nesneleri oluşturmak için bir bildirim kullanılır. Bu bildirimde iki Kubernetes dağıtımı vardır-bir örnek Azure oy Python uygulamaları ve diğeri de redin örneği için. İki Kubernetes hizmeti de oluşturulur; Redsıs örneği için bir iç hizmet ve Azure oy uygulamasına internet 'ten erişmek için bir dış hizmet.

Cloud Shell, veya gibi adlı bir dosya oluşturmak için bir düzenleyici kullanın `azure-vote.yaml` `code azure-vote.yaml` `nano azure-vote.yaml` `vi azure-vote.yaml` . Ardından aşağıdaki YAML tanımına kopyalayın:

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
        image: mcr.microsoft.com/oss/bitnami/redis:6.0.8
        env:
        - name: ALLOW_EMPTY_PASSWORD
          value: "yes"
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
        image: mcr.microsoft.com/azuredocs/azure-vote-front:v1
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

Komutunu kullanarak uygulamayı dağıtın `kubectl apply` ve YAML bildirimin adını belirtin:

```console
kubectl apply -f azure-vote.yaml
```

Aşağıdaki örnek çıktıda başarıyla oluşturulan dağıtımlar ve hizmetler gösterilmektedir:

```output
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>Uygulamayı test edin

Uygulama çalıştığında, bir Kubernetes hizmeti, uygulamanın ön ucuna internet 'e koyar. Bu işlemin tamamlanması birkaç dakika sürebilir.

İlerlemeyi izlemek için `kubectl get service` komutunu `--watch` bağımsız değişkeniyle birlikte kullanın.

```console
kubectl get service azure-vote-front --watch
```

Başlangıçta *Azure-oyönme* hizmeti IÇIN *dış IP* , *Beklemede* olarak gösterilir.

```output
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

*Dış IP* adresi *bekliyor* durumundan gerçek ortak IP adresi olarak değiştiğinde, `CTRL-C` izleme işlemini durdurmak için kullanın `kubectl` . Aşağıdaki örnek çıktıda, hizmete atanmış geçerli bir genel IP adresi gösterilmektedir:

```output
azure-vote-front   LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Azure oy uygulamasını çalışırken görmek için, hizmetinizin dış IP adresine bir Web tarayıcısı açın.

![Azure Vote örnek uygulamasına göz atma görüntüsü](media/container-service-kubernetes-walkthrough/azure-voting-application.png)

## <a name="monitor-health-and-logs"></a>Sistem durumunu ve günlükleri izleme

Kümeyi oluşturduğunuzda, kapsayıcılar için Azure Izleyici etkinleştirilmiştir. Bu izleme özelliği hem AKS kümesi hem de kümede çalışan pod'lar için sistem durumu ölçümleri sağlar.

Azure portalında bu verilerin doldurulması birkaç dakika sürebilir. Azure Vote pod'larının geçerli durumunu, çalışma süresini ve kaynak kullanımını görmek için Azure portalda *myAKSCluster* gibi bir AKS kaynağına geri gidin. Sistem durumuna aşağıdaki şekilde erişebilirsiniz:

1. Sol taraftaki **izleme** altında **Öngörüler** ' i seçin.
1. Üst taraftan **+ Filtre Ekle**'yi seçin
1. Özellik olarak *ad alanı* ' nı seçin ve ardından şunu seçin *\<All but kube-system\>*
1. **Kapsayıcılar** bölümünü görüntüleyin.

Aşağıdaki örnekte olduğu gibi *azure-vote-back* ve *azure-vote-front* kapsayıcıları görüntülenir:

![AKS'de çalışan kapsayıcıların durumunu görüntüleme](media/kubernetes-walkthrough-portal/monitor-containers.png)

Pod 'un günlüklerini görmek için `azure-vote-front` kapsayıcılar listesinin açılan listesinden **kapsayıcı günlüklerini görüntüle** ' yi seçin. Bu günlükler, kapsayıcıdaki *stdout* ve *stderr* akışlarını içerir.

![AKS'deki kapsayıcı günlüklerini görüntüleme](media/kubernetes-walkthrough-portal/monitor-container-logs.png)

## <a name="delete-cluster"></a>Kümeyi silme

Küme artık gerekli olmadığında, tüm ilişkili kaynaklarla birlikte küme kaynağını silin. AKS kümesi panosunda **Sil** düğmesi seçilerek Azure portalında bu işlem tamamlanabilir. Alternatif olarak, [az aks Delete][az-aks-delete] komutu Cloud Shell kullanılabilir:

```azurecli
az aks delete --resource-group myResourceGroup --name myAKSCluster --no-wait
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz. [AKS hizmet sorumlusuyla ilgili önemli noktalar ve silme][sp-delete]. Yönetilen bir kimlik kullandıysanız, kimlik platform tarafından yönetilir ve kaldırma gerektirmez.

## <a name="get-the-code"></a>Kodu alma

Bu hızlı başlangıçta, bir Kubernetes dağıtımı oluşturmak için önceden oluşturulmuş kapsayıcı görüntüleri kullanıldı. İlgili uygulama kodu, Dockerfile ve Kubernetes bildirim dosyası GitHub'da bulunur.

[https://github.com/Azure-Samples/azure-voting-app-redis][azure-vote-app]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Kubernetes kümesi dağıtıp ve bu kümeye çok kapsayıcılı bir uygulama dağıttınız.

Bir uygulama oluşturma, Azure Container Registry dağıtma, çalışan bir uygulamayı güncelleştirme ve kümenizin ölçeklendirilmesi ve yükseltilmesi dahil olmak üzere eksiksiz bir örnek aracılığıyla AKS hakkında daha fazla bilgi edinmek için Kubernetes küme öğreticisine geçin.

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
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-aks-delete]: /cli/azure/aks#az-aks-delete
[aks-monitor]: ../azure-monitor/containers/container-insights-overview.md
[aks-network]: ./concepts-network.md
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[http-routing]: ./http-application-routing.md
[sp-delete]: kubernetes-service-principal.md#additional-considerations