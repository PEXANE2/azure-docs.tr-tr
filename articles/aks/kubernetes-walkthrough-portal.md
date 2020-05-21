---
title: Portalda AKS kümesi oluşturma
titleSuffix: Azure Kubernetes Service
description: Hızlı bir şekilde bir Kubernetes kümesi oluşturmayı, bir uygulamayı dağıtmayı ve Azure portal kullanarak Azure Kubernetes hizmeti 'nde (AKS) performansı izlemenizi öğrenin.
services: container-service
ms.topic: quickstart
ms.date: 01/21/2020
ms.custom: mvc, seo-javascript-october2019
ms.openlocfilehash: d904be260db8fe6170f57d438d3be6d306864d89
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83725117"
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

2. **Containers**  >   **Kubernetes hizmeti**kapsayıcıları ' nı seçin.

3. **Temel bilgiler** sayfasında, aşağıdaki seçenekleri yapılandırın:
    - **Proje ayrıntıları**: bir Azure **aboneliği**seçin, sonra *Myresourcegroup*gibi bir Azure **kaynak grubu**seçin veya oluşturun.
    - **Küme ayrıntıları**: *Myakscluster*gibi bir **Kubernetes küme adı**girin. AKS kümesi için bir **bölge**, **Kubernetes sürümü**ve **DNS adı ön eki** seçin.
    - **Birincil düğüm havuzu**: aks düğümleri IÇIN bir VM **düğüm boyutu** seçin. Bir AKS kümesi dağıtıldıktan sonra *VM boyutu değiştirilemez* . 
            -Kümeye dağıtılacak düğüm sayısını seçin. Bu hızlı başlangıç **Düğüm sayısı** değerini *1* olarak belirleyin. Küme dağıtıldıktan sonra düğüm sayısı *ayarlanabilir*.
    
    ![AKS kümesi oluşturma - temel bilgileri sağlama](media/kubernetes-walkthrough-portal/create-cluster-basics.png)

    **İleri ' yi seçin: tamamlandığında ölçeklendirin** .

4. **Ölçek** sayfasında, varsayılan seçenekleri değiştirmeyin. Ekranın alt kısmındaki **İleri: kimlik doğrulama**' ya tıklayın.
    > [!CAUTION]
    > Yeni AAD hizmet sorumlularının oluşturulması, yayma ve kullanılabilir hale gelmesi birkaç dakika sürebilir ve bu durum, Azure portal ' de hizmet sorumlusu hata ve doğrulama hataları oluşmasına neden olur. Bu alana ulaşırsanız, azaltma için lütfen [buraya](troubleshooting.md#received-an-error-saying-my-service-principal-wasnt-found-or-is-invalid-when-i-try-to-create-a-new-cluster) gidin.

5. **Kimlik doğrulama** sayfasında, aşağıdaki seçenekleri yapılandırın:
    - **Hizmet sorumlusu** alanını **(yeni) varsayılan hizmet sorumlusu**ile bırakarak yeni bir hizmet sorumlusu oluşturun. Alternatif olarak, mevcut bir tane kullanmak için *hizmet sorumlusu Yapılandır* ' ı seçebilirsiniz. Mevcut bir tane kullanırsanız, SPN istemci KIMLIĞINI ve parolasını sağlamanız gerekir.
    - Kubernetes rol tabanlı erişim denetimleri (RBAC) seçeneğini etkinleştirin. Bu, AKS kümenizde dağıtılan Kubernetes kaynaklarına erişim üzerinde daha ayrıntılı denetim sağlar.

    Alternatif olarak, bir hizmet sorumlusu yerine yönetilen bir kimlik de kullanabilirsiniz. Daha fazla bilgi için bkz. [Yönetilen kimlikler kullanma](use-managed-identity.md) .

Varsayılan olarak, *temel* ağ kullanılır ve kapsayıcılar Için Azure izleyici etkindir. **Gözden geçir + oluştur** ' a tıklayın ve doğrulama tamamlandığında **Oluştur** ' a tıklayın.

AKS kümesini oluşturmak birkaç dakika sürer. Dağıtımınız tamamlandığında **Kaynağa Git ' e**tıklayın veya *MYRESOURCEGROUP*gibi aks kümesi kaynak grubuna gidin ve *myakscluster*gibi aks kaynağını seçin. AKS kümesi panosu, aşağıdaki örnekte olduğu gibi gösterilir:

![Azure portalda örnek AKS panosu](media/kubernetes-walkthrough-portal/aks-portal-dashboard.png)

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Kubernetes kümesini yönetmek için Kubernetes komut satırı istemcisi olan [kubectl][kubectl]'yi kullanırsınız. `kubectl` istemcisi Azure Cloud Shell’de önceden yüklüdür.

`>_`Azure Portal en üstündeki düğmeyi kullanarak Cloud Shell açın.

![Azure Cloud Shell'i portalda açma](media/kubernetes-walkthrough-portal/aks-cloud-shell.png)

`kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][az-aks-get-credentials] komutunu kullanın. Bu komut, kimlik bilgilerini indirir ve Kubernetes CLı 'yi bunları kullanacak şekilde yapılandırır. Aşağıdaki örnek *myResourceGroup* adlı kaynak grubu içindeki *myAKSCluster* adlı kümenin kimlik bilgilerini alır:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```console
kubectl get nodes
```

Aşağıdaki örnekte önceki adımlarda oluşturulan tek düğüm gösterilmiştir. Düğüm *durumunun olduğundan emin olun:*

```output
NAME                       STATUS    ROLES     AGE       VERSION
aks-agentpool-14693408-0   Ready     agent     15m       v1.11.5
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Bir Kubernetes bildirim dosyası, küme için, hangi kapsayıcı görüntülerinin çalıştırılacağı gibi istenen durumu tanımlar. Bu hızlı başlangıçta, Azure Vote uygulamasını çalıştırmak için gerekli tüm nesneleri oluşturmak için bir bildirim kullanılır. Bu bildirimde iki [Kubernetes dağıtımı][kubernetes-deployment] vardır-bir örnek Azure oy Python uygulamaları ve diğeri de redin örneği için. İki [Kubernetes hizmeti][kubernetes-service] de oluşturulur; redsıs örneği için bir iç hizmet ve Azure oy uygulamasına internet 'ten erişmek için bir dış hizmet.

> [!TIP]
> Bu hızlı başlangıçta, uygulama bildirimlerini el ile oluşturup AKS kümesine dağıtacaksınız. Daha fazla gerçek dünya senaryolarında kodunuzu doğrudan AKS kümesinde hızlıca yineleyip hatalarını ayıklamak için [Azure Dev Spaces][azure-dev-spaces]’ı kullanabilirsiniz. Dev Spaces’ı işletim sistemi platformları ile geliştirme ortamlarında kullanabilir ve ekibinizdeki diğer kişilerle birlikte çalışabilirsiniz.

Cloud Shell, `nano azure-vote.yaml` `vi azure-vote.yaml` adlı bir dosya oluşturmak için ya da komutunu kullanın `azure-vote.yaml` . Ardından aşağıdaki YAML tanımına kopyalayın:

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

## <a name="test-the-application"></a>Uygulamayı test etme

Uygulama çalıştığında, bir Kubernetes hizmeti, uygulamanın ön ucuna internet 'e koyar. Bu işlemin tamamlanması birkaç dakika sürebilir.

İlerlemeyi izlemek için, [kubectl Get Service][kubectl-get] komutunu bağımsız değişkeniyle birlikte kullanın `--watch` .

```console
kubectl get service azure-vote-front --watch
```

Başlangıçta *Azure-oyönme* hizmeti IÇIN *dış IP* , *Beklemede*olarak gösterilir.

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
1. *Ad alanı* özelliğini ve ardından *\<kube-system hariç tümünü\> seçin*
1. **Kapsayıcılar** bölümünü görüntüleyin.

Aşağıdaki örnekte olduğu gibi *azure-vote-back* ve *azure-vote-front* kapsayıcıları görüntülenir:

![AKS'de çalışan kapsayıcıların durumunu görüntüleme](media/kubernetes-walkthrough-portal/monitor-containers.png)

Pod 'un günlüklerini görmek için `azure-vote-front` kapsayıcılar listesinin açılan listesinden **kapsayıcı günlüklerini görüntüle** ' yi seçin. Bu günlükler, kapsayıcıdaki *stdout* ve *stderr* akışlarını içerir.

![AKS'deki kapsayıcı günlüklerini görüntüleme](media/kubernetes-walkthrough-portal/monitor-container-logs.png)

## <a name="delete-cluster"></a>Kümeyi silme

Küme artık gerekli olmadığında, tüm ilişkili kaynaklarla birlikte küme kaynağını silin. AKS kümesi panosunda **Sil** düğmesi seçilerek Azure portalında bu işlem tamamlanabilir. Alternatif olarak, [az aks Delete][az-aks-delete] komutu Cloud Shell kullanılabilir:

```azurecli-interactive
az aks delete --resource-group myResourceGroup --name myAKSCluster --no-wait
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz. [AKS hizmet sorumlusuyla ilgili önemli noktalar ve silme][sp-delete]. Yönetilen bir kimlik kullandıysanız, kimlik platform tarafından yönetilir ve kaldırma gerektirmez.

## <a name="get-the-code"></a>Kodu alma

Bu hızlı başlangıçta, bir Kubernetes dağıtımı oluşturmak için önceden oluşturulmuş kapsayıcı görüntüleri kullanıldı. İlgili uygulama kodu, Dockerfile ve Kubernetes bildirim dosyası GitHub'da bulunur.

[https://github.com/Azure-Samples/azure-voting-app-redis][azure-vote-app]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Kubernetes kümesi dağıtıp ve bu kümeye çok kapsayıcılı bir uygulama dağıttınız.

AKS hakkında daha fazla bilgi ve dağıtım örneği için tam kod açıklaması için Kubernetes küme öğreticisine geçin.

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
