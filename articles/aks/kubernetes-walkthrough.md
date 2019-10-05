---
title: 'Hızlı başlangıç: Azure Kubernetes hizmet kümesi dağıtma'
description: Hızlı bir şekilde bir Kubernetes kümesi oluşturmayı, uygulamayı dağıtmayı ve Azure CLı kullanarak Azure Kubernetes Service (AKS) içindeki performansı izlemeyi öğrenin.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: quickstart
ms.date: 09/13/2019
ms.author: mlearned
ms.custom: H1Hack27Feb2017, mvc, devcenter, seo-javascript-september2019, seo-javascript-october2019
ms.openlocfilehash: 3a07186856fd05c65f583a94f324df05e348d24f
ms.sourcegitcommit: 4d177e6d273bba8af03a00e8bb9fe51a447196d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71960008"
---
# <a name="quickstart-deploy-an-azure-kubernetes-service-aks-cluster-using-the-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak bir Azure Kubernetes hizmeti (AKS) kümesi dağıtma

Bu hızlı başlangıçta, Azure CLı kullanarak bir Azure Kubernetes hizmeti (AKS) kümesi dağıtırsınız. AKS, kümeleri hızlı bir şekilde dağıtmanıza ve yönetmenize olanak tanıyan bir yönetilen Kubernetes hizmetidir. Bir Web ön ucu ve bir Reda örneği içeren çok kapsayıcılı bir uygulama kümede çalıştırılır. Ardından, uygulamanızı çalıştıran kümenin ve yığınların durumunu nasıl izleyeceğinizi görürsünüz.

Windows Server kapsayıcıları (Şu anda AKS ' de önizlemede) kullanmak istiyorsanız, bkz. [Windows Server kapsayıcılarını destekleyen aks kümesi oluşturma][windows-container-cli].

![Azure oylamaya göz atma görüntüsü](media/container-service-kubernetes-walkthrough/azure-vote.png)

Bu hızlı başlangıçta, Kubernetes kavramlarının temel bir şekilde anlaşıldığı varsayılır. Daha fazla bilgi için bkz. [Azure Kubernetes hizmeti (AKS) Için Kubernetes temel kavramları][kubernetes-concepts].

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu hızlı başlangıç, Azure CLı sürüm 2.0.64 veya üstünü çalıştırıyor olmalıdır. Sürümü bulmak için `az --version` ' yı çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse bkz. [Azure CLI 'Yı yüklemek][azure-cli-install].

> [!Note]
> Bu hızlı başlangıçta komutları yerel olarak çalıştırıyorsanız (Azure Cloud Shell yerine), komutları yönetici olarak çalıştırın.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir gruptur. Bir kaynak grubu oluşturduğunuzda bir konum belirtmeniz istenir. Bu konum, kaynak grubu meta verilerinin depolandığı yerdir, kaynak oluşturma sırasında başka bir bölge belirtmezseniz kaynaklarınızın Azure 'da da çalıştığı yerdir. [Az Group Create][az-group-create] komutunu kullanarak bir kaynak grubu oluşturun.

Aşağıdaki örnek *eastus* konumunda *myresourcegroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Aşağıdaki örnek çıktıda başarıyla oluşturulan kaynak grubu gösterilmektedir:

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

AKS kümesi oluşturmak için [az aks Create][az-aks-create] komutunu kullanın. Aşağıdaki örnek, bir düğüm ile *Myakscluster* adlı bir küme oluşturur. Kapsayıcılar için Azure Izleyici, *--Enable-addons izleme* parametresi kullanılarak da etkinleştirilir.  Bu işlem birkaç dakika sürer.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myAKSCluster --node-count 1 --enable-addons monitoring --generate-ssh-keys
```

Birkaç dakika sonra komut tamamlanır ve küme hakkında JSON biçimli bilgileri döndürür.

## <a name="connect-to-the-cluster"></a>Kümeye Bağlan

Kubernetes kümesini yönetmek için Kubernetes komut satırı istemcisi olan [kubectl][kubectl]'yi kullanırsınız. Azure Cloud Shell kullanırsanız, `kubectl` zaten yüklüdür. @No__t-0 ' ı yerel olarak yüklemek için [az aks install-cli][az-aks-install-cli] komutunu kullanın:

```azurecli
az aks install-cli
```

@No__t, Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks Get-Credentials][az-aks-get-credentials] komutunu kullanın. Bu komut, kimlik bilgilerini indirir ve Kubernetes CLı 'yi bunları kullanacak şekilde yapılandırır.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenizin bağlantısını doğrulamak için [kubectl Get][kubectl-get] komutunu kullanarak küme düğümlerinin bir listesini döndürün.

```azurecli-interactive
kubectl get nodes
```

Aşağıdaki örnek çıktıda, önceki adımlarda oluşturulan tek düğüm gösterilmektedir. Düğüm *durumunun olduğundan emin olun:*

```output
NAME                       STATUS   ROLES   AGE     VERSION
aks-nodepool1-31718369-0   Ready    agent   6m44s   v1.12.8
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Bir Kubernetes bildirim dosyası, küme için, hangi kapsayıcı görüntülerinin çalıştırılacağı gibi istenen durumu tanımlar. Bu hızlı başlangıçta, Azure oy uygulamasını çalıştırmak için gerekli tüm nesneleri oluşturmak için bir bildirim kullanılır. Bu bildirimde iki [Kubernetes dağıtımı][kubernetes-deployment] vardır-bir örnek Azure oy Python uygulamaları ve diğeri de redin örneği için. İki [Kubernetes hizmeti][kubernetes-service] de oluşturulur; redsıs örneği için bir iç hizmet ve Azure oy uygulamasına internet 'ten erişmek için bir dış hizmet.

> [!TIP]
> Bu hızlı başlangıçta, uygulama bildirimlerinizi el ile oluşturarak AKS kümesine dağıtırsınız. Daha çok gerçek dünyada senaryolar için [Azure dev Spaces][azure-dev-spaces] kullanarak doğrudan aks kümesinde kodunuzun hatalarını hızla yineleyebilirsiniz ve hatalarını ayıklayabilirsiniz. İşletim sistemi platformları ve geliştirme ortamlarında geliştirme alanlarını kullanabilir ve takımınızda başkalarıyla birlikte çalışabilirsiniz.

@No__t-0 adlı bir dosya oluşturun ve aşağıdaki YAML tanımına kopyalayın. Azure Cloud Shell kullanırsanız, bu dosya `vi` veya `nano` kullanılarak bir sanal veya fiziksel sistemde çalışırken oluşturulabilir:

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

```output
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

```output
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

*Dış IP* adresi *bekliyor* durumundan gerçek ortak ıp adresini değiştirdiğinde, `kubectl` izleme işlemini durdurmak için `CTRL-C` ' yi kullanın. Aşağıdaki örnek çıktıda, hizmete atanmış geçerli bir genel IP adresi gösterilmektedir:

```output
azure-vote-front   LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Azure oy uygulamasını çalışırken görmek için, hizmetinizin dış IP adresine bir Web tarayıcısı açın.

![Azure oylamaya göz atma görüntüsü](media/container-service-kubernetes-walkthrough/azure-vote.png)

AKS kümesi oluşturulduğunda, [kapsayıcılar Için Azure izleyici](../azure-monitor/insights/container-insights-overview.md) , hem küme düğümleri hem de pods için sistem durumu ölçümlerini yakalamak üzere etkinleştirilmiştir. Bu sistem durumu ölçümleri Azure portal kullanılabilir.

## <a name="delete-the-cluster"></a>Kümeyi silme

Azure ücretlerinden kaçınmak için gereksiz kaynakları temizlemeniz gerekir.  Küme artık gerekli değilse, [az Group Delete][az-group-delete] komutunu kullanarak kaynak grubunu, kapsayıcı hizmetini ve tüm ilgili kaynakları kaldırın.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz. [aks hizmet sorumlusu konuları ve silme][sp-delete].

## <a name="get-the-code"></a>Kodu alın

Bu hızlı başlangıçta, bir Kubernetes dağıtımı oluşturmak için önceden oluşturulmuş kapsayıcı görüntüleri kullanıldı. İlgili uygulama kodu, Dockerfile ve Kubernetes bildirim dosyası GitHub ' da kullanılabilir.

[https://github.com/Azure-Samples/azure-voting-app-redis][azure-vote-app]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Kubernetes kümesi dağıttınız ve bu kümeye çok kapsayıcılı bir uygulama dağıttınız. Ayrıca AKS kümeniz için [Kubernetes web panosuna erişebilirsiniz][kubernetes-dashboard] .

AKS hakkında daha fazla bilgi edinmek ve dağıtım örneği için bir bütün kod üzerinde gezinmek için Kubernetes küme öğreticisine geçin.

> [!div class="nextstepaction"]
> [AKS öğreticisi][aks-tutorial]

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
