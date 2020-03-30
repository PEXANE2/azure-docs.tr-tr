---
title: 'Hızlı başlangıç: Azure Kubernetes Hizmet kümesini dağıtma'
description: Azure CLI'yi kullanarak Azure Kubernetes Hizmeti'nde (AKS) bir Kubernetes kümesini nasıl hızlı bir şekilde oluşturup, bir uygulamayı dağıtmayı ve performansı nasıl izleyeceğinizi öğrenin.
services: container-service
ms.topic: quickstart
ms.date: 09/13/2019
ms.custom:
- H1Hack27Feb2017
- mvc
- devcenter
- seo-javascript-september2019
- seo-javascript-october2019
- seo-python-october2019
ms.openlocfilehash: 0317be6652ff5f03c4c095788ecdde7cba3d1d98
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240467"
---
# <a name="quickstart-deploy-an-azure-kubernetes-service-cluster-using-the-azure-cli"></a>Hızlı başlatma: Azure CLI'yi kullanarak bir Azure Kubernetes Hizmet kümesini dağıtma

Bu hızlı başlangıçta, Azure CLI'yi kullanarak bir Azure Kubernetes Hizmeti (AKS) kümesini dağıtAbilirsiniz. AKS, kümeleri hızla dağıtmanızı ve yönetmenize olanak tanıyan yönetilen bir Kubernetes hizmetidir. Kümede web ön ucu ve Redis örneği içeren çok kapsayıcılı bir uygulama çalıştırılır. Daha sonra, uygulamanızı çalıştıran küme ve bölmelerin durumunu nasıl izleyeceğinizi görürsünüz.

Windows Server kapsayıcılarını kullanmak istiyorsanız (şu anda AKS'de önizlemede), bkz. [Windows Server kapsayıcılarını destekleyen bir AKS kümesi oluştur.][windows-container-cli]

![Azure Kubernetes Hizmeti'nde dağıtılan oylama uygulaması](./media/container-service-kubernetes-walkthrough/voting-app-deployed-in-azure-kubernetes-service.png)

Bu hızlı başlangıç, Kubernetes kavramlarının temel olarak bilindiğini varsayar. Daha fazla bilgi için Azure [Kubernetes Hizmeti (AKS) için Kubernetes temel kavramlarına][kubernetes-concepts]bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu hızlı başlatma, Azure CLI sürümünü 2.0.64 veya daha yeni bir şekilde çalıştırmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

> [!NOTE]
> Komutları bu hızlı başlatmada yerel olarak çalıştırıyorsanız (Azure Bulut Bulutu yerine), komutları yönetici olarak çalıştırdığınızdan emin olun.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir gruptur. Bir kaynak grubu oluştururken konum belirtmeniz istenir. Bu konum, kaynak grubu meta verilerinin depolandığı yerdir, kaynak oluşturma sırasında başka bir bölge belirtmezseniz kaynaklarınızın Azure'da çalıştığı yerdir. az grubu oluşturma komutunu kullanarak bir kaynak grubu [oluşturun.][az-group-create]

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Aşağıdaki örnek çıktı, başarıyla oluşturulan kaynak grubunu gösterir:

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup",
  "location": "eastus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-aks-cluster"></a>AKS kümesi oluşturma

AKS kümesi oluşturmak için [az aks create][az-aks-create] komutunu kullanın. Aşağıdaki örnekte, bir düğüm ile *myAKSCluster* adlı bir küme oluşturulmuştur. *--enable-addons monitoring* parametresiyle Kapsayıcılar için Azure İzleyici de etkinleştirilmiştir.  Bu işlem tamamlanması birkaç dakika sürer.

> [!NOTE]
> AKS kümesi oluşturulurken AKS kaynaklarını depolamak için otomatik olarak ikinci bir kaynak grubu oluşturulur. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/aks/faq#why-are-two-resource-groups-created-with-aks)

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myAKSCluster --node-count 1 --enable-addons monitoring --generate-ssh-keys
```

Birkaç dakika sonra komut tamamlar ve küme hakkında JSON biçimlendirilmiş bilgileri döndürür.

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Bir Kubernetes kümesini yönetmek için [kubectl][kubectl], Kubernetes komut satırı istemcisi kullanırsınız. Azure Bulut Su Şur'u kullanıyorsanız, `kubectl` zaten yüklenmiş. Yerel `kubectl` olarak yüklemek için [az aks install-cli][az-aks-install-cli] komutunu kullanın:

```azurecli
az aks install-cli
```

`kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][az-aks-get-credentials] komutunu kullanın. Bu komut kimlik bilgilerini karşıdan yükler ve Kubernetes CLI'yi bunları kullanacak şekilde yapılandırır.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```azurecli-interactive
kubectl get nodes
```

Aşağıdaki örnekte önceki adımlarda oluşturulan tek düğüm gösterilmiştir. Düğümün durumunun *Hazır*olduğundan emin olun:

```output
NAME                       STATUS   ROLES   AGE     VERSION
aks-nodepool1-31718369-0   Ready    agent   6m44s   v1.12.8
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Kubernetes bildirim dosyası, küme için hangi kapsayıcı görüntülerinin çalıştırılması gibi istenen durumu tanımlar. Bu hızlı başlangıçta, Azure Vote uygulamasını çalıştırmak için gerekli tüm nesneleri oluşturmak için bir bildirim kullanılır. Bu bildirim, biri örnek Azure Vote Python uygulamaları, diğeri ise Redis örneği için olmak üzere iki [Kubernetes dağıtımı][kubernetes-deployment] içerir. Redis örneği için bir dahili hizmet ve Azure Oylaması uygulamasına internetten erişmek için harici bir hizmet olmak üzere iki [Kubernetes Hizmeti][kubernetes-service] de oluşturulur.

> [!TIP]
> Bu hızlı başlangıçta, uygulama bildirimlerini el ile oluşturup AKS kümesine dağıtacaksınız. Daha fazla gerçek dünya senaryolarında kodunuzu doğrudan AKS kümesinde hızlıca yineleyip hatalarını ayıklamak için [Azure Dev Spaces][azure-dev-spaces]’ı kullanabilirsiniz. Dev Spaces’ı işletim sistemi platformları ile geliştirme ortamlarında kullanabilir ve ekibinizdeki diğer kişilerle birlikte çalışabilirsiniz.

Adlandırılmış `azure-vote.yaml` bir dosya oluşturun ve aşağıdaki YAML tanımında kopyalayın. Azure Bulut Kabuğu'nu kullanıyorsanız, bu `vi` dosya `nano` sanal veya fiziksel bir sistem üzerinde çalışıyormuş gibi oluşturulabilir:

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

```azurecli-interactive
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

![Azure Kubernetes Hizmeti'nde dağıtılan oylama uygulaması](./media/container-service-kubernetes-walkthrough/voting-app-deployed-in-azure-kubernetes-service.png)

AKS kümesi oluşturulduğunda, [kapsayıcılar için Azure Monitor](../azure-monitor/insights/container-insights-overview.md) hem küme düğümleri hem de bölmeler için sistem durumu ölçümlerini yakalama olanağı sağladı. Bu sistem durumu ölçümleri Azure portaldan kullanılabilir.

## <a name="delete-the-cluster"></a>Küme silme

Azure ücretlerinden kaçınmak için gereksiz kaynakları temizlemeniz gerekir.  Kümeye artık ihtiyacınız yoksa [az group delete][az-group-delete] komutunu kullanarak kaynak grubunu, kapsayıcı hizmetini ve ilgili tüm kaynakları kaldırın.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz. [AKS hizmet sorumlusuyla ilgili önemli noktalar ve silme][sp-delete].

## <a name="get-the-code"></a>Kodu alma

Bu hızlı başlatmada, önceden oluşturulmuş kapsayıcı görüntüleri bir Kubernetes dağıtımı oluşturmak için kullanılmıştır. İlgili uygulama kodu, Dockerfile ve Kubernetes bildirim dosyası GitHub'da bulunur.

[https://github.com/Azure-Samples/azure-voting-app-redis][azure-vote-app]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Kubernetes kümesi dağıtıp ve bu kümeye çok kapsayıcılı bir uygulama dağıttınız. AKS kümeniz [için Kubernetes web panosuna][kubernetes-dashboard] da erişebilirsiniz.

AKS hakkında daha fazla bilgi ve dağıtım örneği için tam kod açıklaması için Kubernetes küme öğreticisine geçin.

> [!div class="nextstepaction"]
> [AKS eğitimi][aks-tutorial]

<!-- LINKS - external -->
[azure-vote-app]: https://github.com/Azure-Samples/azure-voting-app-redis.git
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[azure-dev-spaces]: https://docs.microsoft.com/azure/dev-spaces/

<!-- LINKS - internal -->
[kubernetes-concepts]: concepts-clusters-workloads.md
[aks-monitor]: https://aka.ms/coingfonboarding
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-browse]: /cli/azure/aks?view=azure-cli-latest#az-aks-browse
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli-install]: /cli/azure/install-azure-cli
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[azure-portal]: https://portal.azure.com
[kubernetes-deployment]: concepts-clusters-workloads.md#deployments-and-yaml-manifests
[kubernetes-service]: concepts-network.md#services
[kubernetes-dashboard]: kubernetes-dashboard.md
[windows-container-cli]: windows-container-cli.md
