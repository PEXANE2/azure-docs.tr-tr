---
title: 'Hızlı başlangıç: Azure Kubernetes hizmet kümesi dağıtma'
description: Hızlı bir şekilde bir Kubernetes kümesi oluşturmayı, uygulamayı dağıtmayı ve PowerShell kullanarak Azure Kubernetes Service (AKS) içindeki performansı izlemeyi öğrenin.
services: container-service
ms.topic: quickstart
ms.date: 05/26/2020
ms.openlocfilehash: a89f76344e48d5af8c71c5a674a94767795b41a9
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83871484"
---
# <a name="quickstart-deploy-an-azure-kubernetes-service-cluster-using-powershell"></a>Hızlı başlangıç: PowerShell kullanarak bir Azure Kubernetes hizmet kümesi dağıtma

Bu hızlı başlangıçta PowerShell kullanarak bir Azure Kubernetes hizmeti (AKS) kümesi dağıtırsınız. AKS, kümeleri hızlı bir şekilde dağıtmanıza ve yönetmenize olanak tanıyan bir yönetilen Kubernetes hizmetidir. Bir Web ön ucu ve bir Reda örneği içeren çok kapsayıcılı bir uygulama kümede çalıştırılır. Ardından, uygulamanızı çalıştıran kümenin ve yığınların durumunu nasıl izleyeceğinizi görürsünüz.

Windows Server düğüm havuzu oluşturma hakkında daha fazla bilgi edinmek için bkz. [Windows Server kapsayıcılarını destekleyen AKS kümesi oluşturma][windows-container-powershell].

![Azure Kubernetes hizmetinde dağıtılan oylama uygulaması](./media/kubernetes-walkthrough-powershell/voting-app-deployed-in-azure-kubernetes-service.png)

Bu hızlı başlangıç, Kubernetes kavramlarının temel olarak bilindiğini varsayar. Daha fazla bilgi için bkz. [Azure Kubernetes hizmeti (AKS) Için Kubernetes temel kavramları][kubernetes-concepts].

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

PowerShell 'i yerel olarak kullanmayı seçerseniz, bu makale az PowerShell modülünü yüklemenizi ve [Connect-AzAccount](/powershell/module/az.accounts/Connect-AzAccount) cmdlet 'Ini kullanarak Azure hesabınıza bağlanmanızı gerektirir. Az PowerShell modülünü yükleme hakkında daha fazla bilgi için bkz. [yükleme Azure PowerShell][install-azure-powershell].

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Birden çok Azure aboneliğiniz varsa, kaynakların faturalandırılması gereken uygun aboneliği seçin. [Set-AzContext](/powershell/module/az.accounts/set-azcontext) cmdlet 'ini kullanarak belirli BIR abonelik kimliği seçin.

```azurepowershell-interactive
Set-AzContext -SubscriptionId 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[Azure Kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) , Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir gruptur. Bir kaynak grubu oluştururken konum belirtmeniz istenir. Bu konum, kaynak grubu meta verilerinin depolandığı yerdir, kaynak oluşturma sırasında başka bir bölge belirtmezseniz kaynaklarınızın Azure 'da da çalıştığı yerdir. [New-AzResourceGroup][new-azresourcegroup] cmdlet 'ini kullanarak bir kaynak grubu oluşturun.

Aşağıdaki örnek **eastus** bölgesinde **myresourcegroup** adlı bir kaynak grubu oluşturur.

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroup -Location eastus
```

Aşağıdaki örnek çıktıda başarıyla oluşturulan kaynak grubu gösterilmektedir:

```Output
ResourceGroupName : myResourceGroup
Location          : eastus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup
```

## <a name="create-aks-cluster"></a>AKS kümesi oluşturma

`ssh-keygen`BIR SSH anahtar çifti oluşturmak için komut satırı yardımcı programını kullanın. Daha fazla ayrıntı için bkz. [hızlı adımlar: Azure 'Da Linux VM 'ler IÇIN SSH genel-özel anahtar çifti oluşturma ve kullanma](/azure/virtual-machines/linux/mac-create-ssh-keys).

Bir AKS kümesi oluşturmak için [New-AzAks][new-azaks] cmdlet 'ini kullanın. Aşağıdaki örnekte, bir düğüm ile **myAKSCluster** adlı bir küme oluşturulmuştur. Kapsayıcılar için Azure Izleyici de varsayılan olarak etkinleştirilmiştir. Bu, tamamlanacak birkaç dakika sürer.

> [!NOTE]
> AKS kümesi oluştururken, AKS kaynaklarını depolamak için otomatik olarak ikinci bir kaynak grubu oluşturulur. Daha fazla bilgi için bkz. [neden AKS ile iki kaynak grubu oluşturulur?](https://docs.microsoft.com/azure/aks/faq#why-are-two-resource-groups-created-with-aks)

```azurepowershell-interactive
New-AzAks -ResourceGroupName myResourceGroup -Name myAKSCluster -NodeCount 1
```

Birkaç dakika sonra komut tamamlanır ve küme hakkında bilgi döndürür.

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Kubernetes kümesini yönetmek için Kubernetes komut satırı istemcisi olan [kubectl][kubectl]'yi kullanırsınız. Azure Cloud Shell kullanıyorsanız, `kubectl` zaten yüklüdür. `kubectl`Yerel olarak yüklemek için `Install-AzAksKubectl` cmdlet 'ini kullanın:

```azurepowershell
Install-AzAksKubectl
```

`kubectl`Kubernetes kümenize bağlanacak şekilde yapılandırmak Için [Import-AzAksCredential][import-azakscredential] cmdlet 'ini kullanın. Aşağıdaki örnek, kimlik bilgilerini indirir ve Kubernetes CLı 'yi bunları kullanacak şekilde yapılandırır.

```azurepowershell-interactive
Import-AzAksCredential -ResourceGroupName myResourceGroup -Name myAKSCluster
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```azurepowershell-interactive
.\kubectl get nodes
```

Aşağıdaki örnekte önceki adımlarda oluşturulan tek düğüm gösterilmiştir. Düğüm **durumunun olduğundan emin olun:**

```Output
NAME                       STATUS   ROLES   AGE     VERSION
aks-nodepool1-31718369-0   Ready    agent   6m44s   v1.15.10
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Bir Kubernetes bildirim dosyası, küme için, hangi kapsayıcı görüntülerinin çalıştırılacağı gibi istenen durumu tanımlar. Bu hızlı başlangıçta, Azure Vote uygulamasını çalıştırmak için gerekli tüm nesneleri oluşturmak için bir bildirim kullanılır. Bu bildirimde iki [Kubernetes dağıtımı][kubernetes-deployment] vardır-bir örnek Azure oy Python uygulamaları ve diğeri de redin örneği için. İki [Kubernetes hizmeti de oluşturulmuştur-Redsıs örneği için bir iç hizmet ve Azure oy uygulamasına internet 'ten erişmek için bir dış hizmet.

> [!TIP]
> Bu hızlı başlangıçta, uygulama bildirimlerini el ile oluşturup AKS kümesine dağıtacaksınız.
> Daha fazla gerçek dünya senaryolarında kodunuzu doğrudan AKS kümesinde hızlıca yineleyip hatalarını ayıklamak için [Azure Dev Spaces][azure-dev-spaces]’ı kullanabilirsiniz. Dev Spaces’ı işletim sistemi platformları ile geliştirme ortamlarında kullanabilir ve ekibinizdeki diğer kişilerle birlikte çalışabilirsiniz.

`azure-vote.yaml`Aşağıdaki YAML tanımında adlı bir dosya oluşturun ve kopyalayın. Azure Cloud Shell kullanırsanız, bu dosya kullanılarak `vi` veya `nano` bir sanal veya fiziksel sistemde çalışırken oluşturulabilir:

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

```azurepowershell-interactive
.\kubectl apply -f azure-vote.yaml
```

Aşağıdaki örnek çıktıda başarıyla oluşturulan dağıtımlar ve hizmetler gösterilmektedir:

```Output
deployment.apps/azure-vote-back created
service/azure-vote-back created
deployment.apps/azure-vote-front created
service/azure-vote-front created
```

## <a name="test-the-application"></a>Uygulamayı test etme

Uygulama çalıştığında bir Kubernetes hizmeti, uygulamanın ön uç noktasını Internet 'e sunar.
Bu işlemin tamamlanması birkaç dakika sürebilir.

İlerlemeyi izlemek için, [kubectl Get Service][kubectl-get] komutunu bağımsız değişkeniyle birlikte kullanın `--watch` .

```azurepowershell-interactive
.\kubectl get service azure-vote-front --watch
```

Başlangıçta **Azure-oyönme** hizmeti IÇIN **dış IP** , **Beklemede**olarak gösterilir.

```Output
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

**Dış IP** adresi **bekliyor** durumundan gerçek ortak IP adresi olarak değiştiğinde, `CTRL-C` izleme işlemini durdurmak için kullanın `kubectl` . Aşağıdaki örnek çıktıda, hizmete atanmış geçerli bir genel IP adresi gösterilmektedir:

```Output
azure-vote-front   LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Azure oy uygulamasını çalışırken görmek için, hizmetinizin dış IP adresine bir Web tarayıcısı açın.

![Azure Kubernetes hizmetinde dağıtılan oylama uygulaması](./media/kubernetes-walkthrough-powershell/voting-app-deployed-in-azure-kubernetes-service.png)

AKS kümesi oluşturulduğunda, [kapsayıcılar Için Azure izleyici](../azure-monitor/insights/container-insights-overview.md) , hem küme düğümleri hem de pods için sistem durumu ölçümlerini yakalamak üzere etkinleştirilmiştir. Bu sistem durumu ölçümleri Azure portaldan kullanılabilir.

## <a name="delete-the-cluster"></a>Küme silme

Azure ücretlerinden kaçınmak için gereksiz kaynakları temizlemeniz gerekir. Küme artık gerekli olmadığında, kaynak grubunu, kapsayıcı hizmetini ve ilgili tüm kaynakları kaldırmak için [Remove-AzResourceGroup][remove-azresourcegroup] cmdlet 'ini kullanın.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz. [AKS hizmet sorumlusuyla ilgili önemli noktalar ve silme][sp-delete]. Yönetilen bir kimlik kullandıysanız, kimlik platform tarafından yönetilir ve kaldırma gerektirmez.

## <a name="get-the-code"></a>Kodu alma

Bu hızlı başlangıçta, bir Kubernetes dağıtımı oluşturmak için önceden oluşturulmuş kapsayıcı görüntüleri kullanıldı. İlgili uygulama kodu, Dockerfile ve Kubernetes bildirim dosyası GitHub'da bulunur.

[https://github.com/Azure-Samples/azure-voting-app-redis][azure-vote-app]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Kubernetes kümesi dağıtıp ve bu kümeye çok kapsayıcılı bir uygulama dağıttınız. Ayrıca AKS kümeniz için [Kubernetes web panosuna erişebilirsiniz][kubernetes-dashboard] .

AKS hakkında daha fazla bilgi ve dağıtım örneği için tam kod açıklaması için Kubernetes küme öğreticisine geçin.

> [!div class="nextstepaction"]
> [AKS öğreticisi][aks-tutorial]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[azure-dev-spaces]: https://docs.microsoft.com/azure/dev-spaces/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[azure-vote-app]: https://github.com/Azure-Samples/azure-voting-app-redis.git

<!-- LINKS - internal -->
[windows-container-powershell]: windows-container-powershell.md
[kubernetes-concepts]: concepts-clusters-workloads.md
[install-azure-powershell]: /powershell/azure/install-az-ps
[new-azresourcegroup]: /powershell/module/az.resources/new-azresourcegroup
[new-azaks]: /powershell/module/az.aks/new-azaks
[import-azakscredential]: /powershell/module/az.aks/import-azakscredential
[kubernetes-deployment]: concepts-clusters-workloads.md#deployments-and-yaml-manifests
[kubernetes-service]: concepts-network.md#services
[remove-azresourcegroup]: /powershell/module/az.resources/remove-azresourcegroup
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[kubernetes-dashboard]: kubernetes-dashboard.md
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
