---
title: Portalda bir AKS kümesi oluşturma
titleSuffix: Azure Kubernetes Service
description: Azure portalını kullanarak Bir Kubernetes kümesini nasıl hızlı bir şekilde oluşturup, bir uygulamayı dağıtın ve Azure Kubernetes Hizmeti'nde (AKS) performansı nasıl izleyeceğinizi öğrenin.
services: container-service
ms.topic: quickstart
ms.date: 01/21/2020
ms.custom: mvc, seo-javascript-october2019
ms.openlocfilehash: e4ac5a953b5d88d0074c3cfb7f1bd45331577238
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81392797"
---
# <a name="quickstart-deploy-an-azure-kubernetes-service-aks-cluster-using-the-azure-portal"></a>Hızlı başlatma: Azure portalını kullanarak bir Azure Kubernetes Hizmeti (AKS) kümesini dağıtma

Azure Kubernetes Service (AKS), kümeleri hızla dağıtmanızı ve yönetmenize olanak tanıyan yönetilen bir Kubernetes hizmetidir. Bu hızlı başlangıçta, Azure portalını kullanarak bir AKS kümesi dağıtırsınız. Kümede web ön ucu ve Redis örneği içeren çok kapsayıcılı bir uygulama çalıştırılır. Daha sonra, uygulamanızı çalıştıran küme ve bölmelerin durumunu nasıl izleyeceğinizi görürsünüz.

![Azure Vote örnek uygulamasına göz atma görüntüsü](media/container-service-kubernetes-walkthrough/azure-voting-application.png)

Bu hızlı başlangıç, Kubernetes kavramlarının temel olarak bilindiğini varsayar. Daha fazla bilgi için Azure [Kubernetes Hizmeti (AKS) için Kubernetes temel kavramlarına][kubernetes-concepts]bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

AKS kümesi oluşturmak için aşağıdaki adımları tamamlayın:

1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**’u seçin.

2. **Kapsayıcılar** >  **Kubernetes Servisi**seçin.

3. Temel **Bilgiler** sayfasında, aşağıdaki seçenekleri yapılandırın:
    - **Proje ayrıntıları**: Azure **Aboneliği**seçin ve ardından *myResourceGroup*gibi bir Azure **Kaynak grubu**seçin veya oluşturun.
    - **Küme ayrıntıları**: *myAKSCluster*gibi bir **Kubernetes küme adı**girin. AKS kümesi için **Bir Bölge,** **Kubernetes sürümü**ve **DNS ad öneki** seçin.
    - **Birincil düğüm havuzu**: AKS düğümleri için bir VM **düğümü boyutu** seçin. Bir AKS kümesi dağıtıldıktan sonra VM boyutu *değiştirilemez.* 
            - Kümeye dağıtmak için düğüm sayısını seçin. Bu hızlı başlangıç **Düğüm sayısı** değerini *1* olarak belirleyin. Küme dağıtıldıktan sonra düğüm sayısı *ayarlanabilir*.
    
    ![AKS kümesi oluşturma - temel bilgileri sağlama](media/kubernetes-walkthrough-portal/create-cluster-basics.png)

    **Sonraki'ni seçin:** Tamamlandığında ölçeklendirin.

4. **Ölçek** sayfasında varsayılan seçenekleri saklayın. Ekranın alt kısmında, **İleri: Kimlik Doğrulama'yı**tıklatın.
    > [!CAUTION]
    > Yeni AAD Hizmet İlkeleri oluşturmak, Hizmet Sorumlusu'nun Azure portalında hata ve doğrulama hataları bulamamasına neden olmak için yayılması ve kullanılabilir hale gelmesi birkaç dakika sürebilir. Eğer bu hit lütfen azaltma için [buraya](troubleshooting.md#im-receiving-errors-that-my-service-principal-was-not-found-when-i-try-to-create-a-new-cluster-without-passing-in-an-existing-one) ziyaret edin.

5. Kimlik **Doğrulama** sayfasında, aşağıdaki seçenekleri yapılandırın:
    - **Hizmet Sorumlusu** alanını **(yeni) varsayılan hizmet ilkesiyle**bırakarak yeni bir hizmet ilkesi oluşturun. Veya varolan bir tanesini kullanmak için *Hizmet Atamasını Yapıla'yı* seçebilirsiniz. Varolan bir tane kullanıyorsanız, SPN istemci kimliğini ve sırrını sağlamanız gerekir.
    - Kubernetes rol tabanlı erişim denetimleri (RBAC) seçeneğini etkinleştirin. Bu, AKS kümenizde dağıtılan Kubernetes kaynaklarına erişim üzerinde daha ince taneli denetim sağlar.

    Alternatif olarak, hizmet sorumlusu yerine yönetilen bir kimlik kullanabilirsiniz. Daha fazla bilgi için yönetilen kimlikleri n için [kullanın.](use-managed-identity.md)

Varsayılan olarak, *Temel* ağ kullanılır ve kapsayıcılar için Azure Monitor etkinleştirilir. Doğrulama tamamlandığında **Gözden Geçir + oluştur'u** ve ardından **Oluştur'u** tıklatın.

AKS kümesini oluşturmak birkaç dakika sürer. Dağıtımınız tamamlandığında **kaynağa git'i**tıklatın veya *myResourceGroup*gibi AKS küme kaynak grubuna göz atın ve *myAKSCluster*gibi AKS kaynağını seçin. AKS küme panosu, bu örnekte olduğu gibi gösterilir:

![Azure portalda örnek AKS panosu](media/kubernetes-walkthrough-portal/aks-portal-dashboard.png)

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Bir Kubernetes kümesini yönetmek için [kubectl][kubectl], Kubernetes komut satırı istemcisi kullanırsınız. `kubectl` istemcisi Azure Cloud Shell’de önceden yüklüdür.

Azure portalının `>_` üst kısmındaki düğmeyi kullanarak Bulut Kabuğu'nu açın.

![Azure Cloud Shell'i portalda açma](media/kubernetes-walkthrough-portal/aks-cloud-shell.png)

`kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][az-aks-get-credentials] komutunu kullanın. Bu komut kimlik bilgilerini karşıdan yükler ve Kubernetes CLI'yi bunları kullanacak şekilde yapılandırır. Aşağıdaki örnek *myResourceGroup* adlı kaynak grubu içindeki *myAKSCluster* adlı kümenin kimlik bilgilerini alır:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```console
kubectl get nodes
```

Aşağıdaki örnekte önceki adımlarda oluşturulan tek düğüm gösterilmiştir. Düğümün durumunun *Hazır*olduğundan emin olun:

```output
NAME                       STATUS    ROLES     AGE       VERSION
aks-agentpool-14693408-0   Ready     agent     15m       v1.11.5
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Kubernetes bildirim dosyası, küme için hangi kapsayıcı görüntülerinin çalıştırılması gibi istenen durumu tanımlar. Bu hızlı başlangıçta, Azure Vote uygulamasını çalıştırmak için gerekli tüm nesneleri oluşturmak için bir bildirim kullanılır. Bu bildirim, biri örnek Azure Vote Python uygulamaları, diğeri ise Redis örneği için olmak üzere iki [Kubernetes dağıtımı][kubernetes-deployment] içerir. Redis örneği için bir dahili hizmet ve Azure Oylaması uygulamasına internetten erişmek için harici bir hizmet olmak üzere iki [Kubernetes Hizmeti][kubernetes-service] de oluşturulur.

> [!TIP]
> Bu hızlı başlangıçta, uygulama bildirimlerini el ile oluşturup AKS kümesine dağıtacaksınız. Daha fazla gerçek dünya senaryolarında kodunuzu doğrudan AKS kümesinde hızlıca yineleyip hatalarını ayıklamak için [Azure Dev Spaces][azure-dev-spaces]’ı kullanabilirsiniz. Dev Spaces’ı işletim sistemi platformları ile geliştirme ortamlarında kullanabilir ve ekibinizdeki diğer kişilerle birlikte çalışabilirsiniz.

Bulut Kabuğu'nda, adlı `nano azure-vote.yaml` `azure-vote.yaml` `vi azure-vote.yaml` bir dosya oluşturmak için ya komutu kullanın. Sonra aşağıdaki YAML tanımında kopyalayın:

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

[Kubectl uygula][kubectl-apply] komutunu kullanarak uygulamayı dağıtın ve YAML manifestonuzun adını belirtin:

```console
kubectl apply -f azure-vote.yaml
```

Aşağıdaki örnek çıktı, başarıyla oluşturulan Dağıtımları ve Hizmetleri gösterir:

```output
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>Uygulamayı test etme

Uygulama çalıştığında, bir Kubernetes hizmeti uygulamanın ön ucunu internete maruz bırakır. Bu işlemin tamamlanması birkaç dakika sürebilir.

İlerlemeyi izlemek için, bağımsız değişkenle birlikte `--watch` [kubectl get service][kubectl-get] komutunu kullanın.

```console
kubectl get service azure-vote-front --watch
```

Başlangıçta *azure-vote-front* hizmeti için *EXTERNAL-IP* *beklemede*olarak gösterilir.

```output
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

*EXTERNAL-IP* adresi *beklemeden* gerçek bir genel IP `CTRL-C` adresine `kubectl` değiştiğinde, izleme işlemini durdurmak için kullanın. Aşağıdaki örnek çıktı, hizmete atanan geçerli bir genel IP adresini gösterir:

```output
azure-vote-front   LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Azure Oylaması uygulamasını iş başında görmek için, hizmetin dış IP adresine bir web tarayıcısı açın.

![Azure Vote örnek uygulamasına göz atma görüntüsü](media/container-service-kubernetes-walkthrough/azure-voting-application.png)

## <a name="monitor-health-and-logs"></a>Sistem durumunu ve günlükleri izleme

Kümeyi oluşturduğunuzda, kapsayıcılar için Azure Monitörü etkinleştirildi. Bu izleme özelliği hem AKS kümesi hem de kümede çalışan pod'lar için sistem durumu ölçümleri sağlar.

Azure portalında bu verilerin doldurulması birkaç dakika sürebilir. Azure Vote pod'larının geçerli durumunu, çalışma süresini ve kaynak kullanımını görmek için Azure portalda *myAKSCluster* gibi bir AKS kaynağına geri gidin. Sistem durumuna aşağıdaki şekilde erişebilirsiniz:

1. Sol tarafta **İzleme** **altında, Insights'ı** seçin
1. Üst taraftan **+ Filtre Ekle**'yi seçin
1. *Ad alanı* özelliğini ve ardından *\<kube-system hariç tümünü\> seçin*
1. **Kapsayıcılar** bölümünü görüntüleyin.

Aşağıdaki örnekte olduğu gibi *azure-vote-back* ve *azure-vote-front* kapsayıcıları görüntülenir:

![AKS'de çalışan kapsayıcıların durumunu görüntüleme](media/kubernetes-walkthrough-portal/monitor-containers.png)

`azure-vote-front` Bölmegünlüklerini görmek için kapsayıcılar listesinin açılır tarafından görüntüle **kapsayıcı günlüklerini** seçin. Bu günlükler, kapsayıcıdaki *stdout* ve *stderr* akışlarını içerir.

![AKS'deki kapsayıcı günlüklerini görüntüleme](media/kubernetes-walkthrough-portal/monitor-container-logs.png)

## <a name="delete-cluster"></a>Kümeyi silme

Küme artık gerekli olmadığında, tüm ilişkili kaynaklarla birlikte küme kaynağını silin. AKS kümesi panosunda **Sil** düğmesi seçilerek Azure portalında bu işlem tamamlanabilir. Alternatif olarak, [az aks silme][az-aks-delete] komutu Bulut Kabuğu'nda kullanılabilir:

```azurecli-interactive
az aks delete --resource-group myResourceGroup --name myAKSCluster --no-wait
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz. [AKS hizmet sorumlusuyla ilgili önemli noktalar ve silme][sp-delete]. Yönetilen bir kimlik kullandıysanız, kimlik platform tarafından yönetilir ve kaldırılmasını gerektirmez.

## <a name="get-the-code"></a>Kodu alma

Bu hızlı başlatmada, önceden oluşturulmuş kapsayıcı görüntüleri bir Kubernetes dağıtımı oluşturmak için kullanılmıştır. İlgili uygulama kodu, Dockerfile ve Kubernetes bildirim dosyası GitHub'da bulunur.

[https://github.com/Azure-Samples/azure-voting-app-redis][azure-vote-app]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Kubernetes kümesi dağıtıp ve bu kümeye çok kapsayıcılı bir uygulama dağıttınız.

AKS hakkında daha fazla bilgi ve dağıtım örneği için tam kod açıklaması için Kubernetes küme öğreticisine geçin.

> [!div class="nextstepaction"]
> [AKS eğitimi][aks-tutorial]

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
