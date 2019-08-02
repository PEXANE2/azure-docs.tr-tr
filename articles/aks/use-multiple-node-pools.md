---
title: Azure Kubernetes hizmetinde (AKS) birden çok düğüm havuzu kullanma
description: Azure Kubernetes Service (AKS) ' de bir küme için birden çok düğüm havuzu oluşturma ve yönetme hakkında bilgi edinin
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/17/2019
ms.author: mlearned
ms.openlocfilehash: 72f34d9711e1ba4658288bfdeb847632d32d0fcf
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68478336"
---
# <a name="preview---create-and-manage-multiple-node-pools-for-a-cluster-in-azure-kubernetes-service-aks"></a>Önizleme-Azure Kubernetes Service (AKS) ' de bir küme için birden çok düğüm havuzu oluşturma ve yönetme

Azure Kubernetes hizmeti 'nde (AKS), aynı yapılandırmanın düğümleri *düğüm havuzlarında*birlikte gruplandırılır. Bu düğüm havuzları, uygulamalarınızı çalıştıran temel VM 'Leri içerir. Bir *varsayılan düğüm havuzu*oluşturan bir aks kümesi oluşturduğunuzda, ilk düğüm sayısı ve boyutu (SKU) tanımlanmıştır. Farklı işlem veya depolama taleplerine sahip uygulamaları desteklemek için ek düğüm havuzları oluşturabilirsiniz. Örneğin, işlem yoğunluklu uygulamalar için GPU 'Lar sağlamak veya yüksek performanslı SSD depolamaya erişmek için bu ek düğüm havuzlarını kullanın.

Bu makalede bir AKS kümesinde birden çok düğüm havuzu oluşturma ve yönetme konusu gösterilmektedir. Bu özellik şu anda önizleme sürümündedir.

> [!IMPORTANT]
> AKS Önizleme özellikleri self servis, kabul etme özellikleridir. Topluluğumuza geri bildirim ve hata toplamak için sağlanırlar. Önizlemede, bu özellikler üretim kullanımı için tasarlanmamıştır. Genel önizlemede bulunan Özellikler ' en iyi çaba ' desteği altına düşmektedir. AKS teknik destek ekiplerinden yardım yalnızca çalışma saatleri Pasifik saat dilimi (PST) sırasında kullanılabilir. Ek bilgi için lütfen aşağıdaki destek makalelerine bakın:
>
> * [AKS destek Ilkeleri][aks-support-policies]
> * [Azure desteği SSS][aks-faq]

## <a name="before-you-begin"></a>Başlamadan önce

Azure CLı sürüm 2.0.61 veya sonraki bir sürümün yüklü ve yapılandırılmış olması gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][install-azure-cli].

### <a name="install-aks-preview-cli-extension"></a>Aks-Preview CLı uzantısını yükler

Birden çok nodepools kullanmak için, *aks-Preview* CLI uzantısının sürüm 0.4.1 veya üzeri olması gerekir. [Az Extension Add][az-extension-add] komutunu kullanarak *aks-Preview* Azure CLI uzantısını yükledikten sonra [az Extension Update][az-extension-update] komutunu kullanarak kullanılabilir güncelleştirmeleri denetleyin::

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-multiple-node-pool-feature-provider"></a>Birden çok düğüm havuzu Özellik sağlayıcısını Kaydet

Birden çok düğüm havuzu kullanan bir AKS kümesi oluşturmak için, öncelikle aboneliğinizde iki özellik bayrağını etkinleştirin. Çok düğümlü havuz kümeleri, Kubernetes düğümlerinin dağıtımını ve yapılandırmasını yönetmek için bir sanal makine ölçek kümesi (VMSS) kullanır. Aşağıdaki örnekte gösterildiği gibi [az Feature Register][az-feature-register] komutunu kullanarak *Multiagentpoolpreview* ve *VMSSPreview* Özellik bayraklarını kaydedin:

> [!CAUTION]
> Bir abonelik üzerinde bir özelliği kaydettiğinizde, o özelliği şu anda kaydedemezsiniz. Bazı Önizleme özelliklerini etkinleştirdikten sonra, daha sonra abonelikte oluşturulan tüm AKS kümeleri için varsayılanlar kullanılabilir. Üretim aboneliklerinde Önizleme özelliklerini etkinleştirmeyin. Önizleme özelliklerini test etmek ve geri bildirim toplamak için ayrı bir abonelik kullanın.

```azurecli-interactive
az feature register --name MultiAgentpoolPreview --namespace Microsoft.ContainerService
az feature register --name VMSSPreview --namespace Microsoft.ContainerService
```

> [!NOTE]
> *Multiagentpoolpreview* 'ı başarıyla kaydettikten sonra oluşturduğunuz aks kümesi, bu önizleme kümesi deneyimini kullanır. Düzenli, tam olarak desteklenen kümeler oluşturmaya devam etmek için üretim aboneliklerinde Önizleme özelliklerini etkinleştirmeyin. Önizleme özelliklerini test etmek için ayrı bir test veya geliştirme Azure aboneliği kullanın.

Durumun *kayıtlı*gösterilmesi birkaç dakika sürer. [Az Feature List][az-feature-list] komutunu kullanarak kayıt durumunu denetleyebilirsiniz:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/MultiAgentpoolPreview')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/VMSSPreview')].{Name:name,State:properties.state}"
```

Hazırlandığınızda, [az Provider Register][az-provider-register] komutunu kullanarak *Microsoft. Containerservice* kaynak sağlayıcısı kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="limitations"></a>Sınırlamalar

Birden çok düğüm havuzunu destekleyen AKS kümelerini oluşturup yönetirken aşağıdaki sınırlamalar geçerlidir:

* Çoklu düğüm havuzları yalnızca, aboneliğiniz için *Multiagentpoolpreview* ve *VMSSPreview* özelliklerini başarıyla kaydettikten sonra oluşturulan kümeler için kullanılabilir. Bu özellikler başarıyla kaydedilmeden önce, var olan bir AKS kümesiyle oluşturulmuş düğüm havuzları ekleyemez veya yönetemezsiniz.
* İlk düğüm havuzunu silemezsiniz.
* HTTP uygulama yönlendirme eklentisi kullanılamıyor.
* Birçok işlem ile olduğu gibi mevcut bir Kaynak Yöneticisi şablonu kullanarak düğüm havuzları ekleyemez/güncelleştiremez/silemezsiniz. Bunun yerine, bir AKS kümesindeki düğüm havuzlarında değişiklik yapmak için [ayrı bir kaynak yöneticisi şablonu kullanın](#manage-node-pools-using-a-resource-manager-template) .

Bu özellik önizlemedeyken aşağıdaki ek sınırlamalar geçerlidir:

* AKS kümesinde en fazla sekiz düğüm havuzu olabilir.
* AKS kümesi, bu sekiz düğüm havuzunda en fazla 400 düğüme sahip olabilir.
* Tüm düğüm havuzları aynı alt ağda yer almalıdır

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

Başlamak için, tek düğümlü havuz ile bir AKS kümesi oluşturun. Aşağıdaki örnek, *eastus* bölgesinde *myresourcegroup* adlı bir kaynak grubu oluşturmak için [az Group Create][az-group-create] komutunu kullanır. *Myakscluster* adlı bir aks kümesi daha sonra [az aks Create][az-aks-create] komutu kullanılarak oluşturulur. Bir *--Kubernetes-* *1.13.5* , aşağıdaki adımlarda bir düğüm havuzunun nasıl güncelleştiğine göstermek için kullanılır. [Desteklenen Kubernetes sürümünü][supported-versions]belirtebilirsiniz.

```azurecli-interactive
# Create a resource group in East US
az group create --name myResourceGroup --location eastus

# Create a basic single-node AKS cluster
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --enable-vmss \
    --node-count 1 \
    --generate-ssh-keys \
    --kubernetes-version 1.13.5
```

Kümenin oluşturulması birkaç dakika sürer.

Küme kullanıma hazırsa, şu ile `kubectl`kullanılacak küme kimlik bilgilerini almak için [az aks Get-Credentials][az-aks-get-credentials] komutunu kullanın:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="add-a-node-pool"></a>Düğüm havuzu ekleme

Önceki adımda oluşturulan kümenin tek bir düğüm havuzu vardır. [Az aks düğüm havuzu Add][az-aks-nodepool-add] komutunu kullanarak ikinci bir düğüm havuzu ekleyelim. Aşağıdaki örnek *3* düğümleri çalıştıran *mynodepool* adlı bir düğüm havuzu oluşturur:

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 3
```

Düğüm havuzlarınızın durumunu görmek için [az aks node Pool List][az-aks-nodepool-list] komutunu kullanın ve kaynak grubunuzu ve küme adınızı belirtin:

```azurecli-interactive
az aks nodepool list --resource-group myResourceGroup --cluster-name myAKSCluster -o table
```

Aşağıdaki örnek çıktı, düğüm havuzundaki üç düğüm ile *mynodepool* başarıyla oluşturulduğunu gösterir. Önceki adımda AKS kümesi oluşturulduğunda, *1*düğüm sayısıyla varsayılan bir *nodepool1* oluşturulmuştur.

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup    VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  ---------------  ---------------
VirtualMachineScaleSets  3        110        mynodepool  1.13.5                 100             Linux     Succeeded            myResourceGroup  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.13.5                 100             Linux     Succeeded            myResourceGroup  Standard_DS2_v2
```

> [!TIP]
> Bir düğüm havuzu eklediğinizde bir *Orchestratorversion* veya *VMSize* belirtilmemişse düğümler, aks kümesinin varsayılan değerleri temel alınarak oluşturulur. Bu örnekte, Kubernetes sürüm *1.13.5* ve *Standard_DS2_v2*düğüm boyutudur.

## <a name="upgrade-a-node-pool"></a>Düğüm havuzunu yükseltme

Aks kümeniz ilk adımda oluşturulduysa, bir `--kubernetes-version` *1.13.5* belirtildi. Bu, Kubernetes sürümünü hem denetim düzlemi hem de ilk düğüm havuzu için ayarlar. Denetim düzlemi ve düğüm havuzunun Kubernetes sürümünü yükseltmek için farklı komutlar vardır. Komut, tek bir düğüm havuzunu yükseltmek için kullanıldığında, `az aks nodepool upgrade` denetim düzlemini yükseltmek için kullanılır. `az aks upgrade`

*Mynodepool* , Kubernetes *1.13.7*'e yükseltelim. Aşağıdaki örnekte gösterildiği gibi, düğüm havuzunu yükseltmek için [az aks düğüm havuzu yükseltme][az-aks-nodepool-upgrade] komutunu kullanın:

```azurecli-interactive
az aks nodepool upgrade \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --kubernetes-version 1.13.7 \
    --no-wait
```

> [!Tip]
> Denetim düzlemi 'ni *1.13.7*' e yükseltmek için öğesini `az aks upgrade -k 1.13.7`çalıştırın.

[Az aks düğüm havuzu listesi][az-aks-nodepool-list] komutunu kullanarak düğüm havuzlarınızın durumunu yeniden listeleyin. Aşağıdaki örnek, *mynodepool* 'in *yükseltme* durumunda olduğunu *gösterir:*

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup    VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  ---------------  ---------------
VirtualMachineScaleSets  3        110        mynodepool  1.13.7                 100             Linux     Upgrading            myResourceGroup  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.13.5                 100             Linux     Succeeded            myResourceGroup  Standard_DS2_v2
```

Düğümlerin belirtilen sürüme yükseltilmesi birkaç dakika sürer.

En iyi uygulama olarak, bir AKS kümesindeki tüm düğüm havuzlarını aynı Kubernetes sürümüne yükseltmeniz gerekir. Tek tek düğüm havuzlarını yükseltebilme özelliği, uygulama çalışma süresini korumak için sıralı yükseltme gerçekleştirmenize ve düğüm havuzları arasında dizin zamanlamauygulamanıza olanak tanır.

> [!NOTE]
> Kubernetes, standart [anlamsal sürüm](https://semver.org/) oluşturma düzenini kullanır. Sürüm numarası *x. y. z*olarak ifade edilir; burada *x* ana sürümdür, *y* ise ikincil sürümdür ve *z* , yama sürümüdür. Örneğin, sürüm *1.12.6*' de, 1 ana sürümdür, 12 ise ikincil sürümdür ve 6 Düzeltme Eki sürümüdür. Denetim düzleminin Kubernetes sürümü ve ilk düğüm havuzu küme oluşturma sırasında ayarlanır. Tüm ek düğüm havuzlarının, kümeye eklendiğinde Kubernetes sürümü ayarlanmış olmalıdır. Kubernetes sürümleri, düğüm havuzlarının yanı sıra düğüm havuzu ile denetim düzlemi arasında farklılık gösterebilir, ancak aşağıdaki kısıtlamalar geçerlidir:
> 
> * Düğüm havuzu sürümü, denetim düzlemi ile aynı ana sürüme sahip olmalıdır.
> * Düğüm havuzu sürümü, denetim düzlemi sürümünden düşük bir alt sürüm olabilir.
> * Düğüm havuzu sürümü, diğer iki kısıtlama izlendiği sürece herhangi bir yama sürümü olabilir.
> 
> Denetim düzleminin Kubernetes sürümünü yükseltmek için kullanın `az aks upgrade`. Kümenizin yalnızca bir düğüm havuzu varsa, `az aks upgrade` bu komut düğüm havuzunun Kubernetes sürümünü de yükseltir.

## <a name="scale-a-node-pool"></a>Düğüm havuzunu ölçeklendirme

Uygulama iş yükünüz değiştikçe, bir düğüm havuzundaki düğüm sayısını ölçeklendirmeniz gerekebilir. Düğüm sayısı yukarı veya aşağı ölçeklendirilebilirler.

<!--If you scale down, nodes are carefully [cordoned and drained][kubernetes-drain] to minimize disruption to running applications.-->

Düğüm havuzundaki düğüm sayısını ölçeklendirmek için [az aks node Pool Scale][az-aks-nodepool-scale] komutunu kullanın. Aşağıdaki örnek, *mynodepool* içindeki düğüm sayısını *5*olarak ölçeklendirir:

```azurecli-interactive
az aks nodepool scale \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 5 \
    --no-wait
```

[Az aks düğüm havuzu listesi][az-aks-nodepool-list] komutunu kullanarak düğüm havuzlarınızın durumunu yeniden listeleyin. Aşağıdaki örnek, *mynodepool* 'in yeni sayısı *5* düğüm olan *ölçekleme* durumunda olduğunu gösterir:

```console
$ az aks nodepool list -g myResourceGroupPools --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup    VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  ---------------  ---------------
VirtualMachineScaleSets  5        110        mynodepool  1.13.7                 100             Linux     Scaling              myResourceGroup  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.13.5                 100             Linux     Succeeded            myResourceGroup  Standard_DS2_v2
```

Ölçek işleminin tamamlanabilmesi birkaç dakika sürer.

## <a name="delete-a-node-pool"></a>Düğüm havuzunu silme

Artık havuz gerekmiyorsa, onu silebilir ve temel alınan VM düğümlerini kaldırabilirsiniz. Bir düğüm havuzunu silmek için [az aks node Pool Delete][az-aks-nodepool-delete] komutunu kullanın ve düğüm havuzu adını belirtin. Aşağıdaki örnekte, önceki adımlarda oluşturulan *mynoodepool* silinir:

> [!CAUTION]
> Bir düğüm havuzunu sildiğinizde oluşabilecek veri kaybı için kurtarma seçeneği yoktur. Diğer düğüm havuzlarında Eğer Pod zamanlanamaz, bu uygulamalar kullanılamaz. Kullanımda olan uygulamalarda veri yedeklemeleri olmadığında veya kümenizdeki diğer düğüm havuzlarında çalıştırma yeteneğine sahip olmayan bir düğüm havuzunu silmemenizi unutmayın.

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name mynodepool --no-wait
```

[Az aks düğüm havuzu List][az-aks-nodepool-list] komutundan alınan aşağıdaki örnek çıktı, *mynodepool* 'in *silme* durumunda olduğunu gösterir:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup    VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  ---------------  ---------------
VirtualMachineScaleSets  5        110        mynodepool  1.13.7                 100             Linux     Deleting             myResourceGroup  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.13.5                 100             Linux     Succeeded            myResourceGroup  Standard_DS2_v2
```

Düğümlerin ve düğüm havuzunun silinmesi birkaç dakika sürer.

## <a name="specify-a-vm-size-for-a-node-pool"></a>Düğüm havuzu için VM boyutu belirtme

Önceki örneklerde, bir düğüm havuzu oluşturmak için, kümede oluşturulan düğümler için varsayılan bir VM boyutu kullanıldı. Daha yaygın bir senaryo, farklı VM boyutları ve özellikleri olan düğüm havuzları oluşturmanıza yöneliktir. Örneğin, büyük miktarlarda CPU veya belleğe sahip düğümleri veya GPU desteği sağlayan bir düğüm havuzunu içeren düğüm havuzu oluşturabilirsiniz. Sonraki adımda, Kubernetes Scheduler 'a bu düğümlerde çalışabilecek düğüm erişimini nasıl sınırlayacağınızı bildirmek için [tatları ve toleransı kullanırsınız](#schedule-pods-using-taints-and-tolerations) .

Aşağıdaki örnekte, *Standard_NC6* VM boyutunu kullanan bir GPU tabanlı düğüm havuzu oluşturun. Bu VM 'Ler NVıDıA Tesla K80 kartı tarafından desteklenir. Kullanılabilir VM boyutları hakkında daha fazla bilgi için bkz. [Azure 'Da Linux sanal makineleri Için boyutlar][vm-sizes].

[Az aks düğüm havuzu Add][az-aks-nodepool-add] komutunu yeniden kullanarak bir düğüm havuzu oluşturun. Bu kez, *gpunodepool*adını belirtin ve *Standard_NC6* boyutunu belirtmek için `--node-vm-size` parametresini kullanın:

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name gpunodepool \
    --node-count 1 \
    --node-vm-size Standard_NC6 \
    --no-wait
```

[Az aks düğüm havuzu List][az-aks-nodepool-list] komutundan aşağıdaki örnek çıktı, *Gpunodepool* belirtilen *VMSize*sahip düğümleri *oluşturuyor* olduğunu gösterir:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name         OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup    VmSize
-----------------------  -------  ---------  -----------  ---------------------  --------------  --------  -------------------  ---------------  ---------------
VirtualMachineScaleSets  1        110        gpunodepool  1.13.5                 100             Linux     Creating             myResourceGroup  Standard_NC6
VirtualMachineScaleSets  1        110        nodepool1    1.13.5                 100             Linux     Succeeded            myResourceGroup  Standard_DS2_v2
```

*Gpunodepool* 'nin başarıyla oluşturulması birkaç dakika sürer.

## <a name="schedule-pods-using-taints-and-tolerations"></a>Talara ve toleranları kullanarak Pod zamanlama

Artık kümenizde iki düğüm havuzu vardır; başlangıçta oluşturulan varsayılan düğüm havuzu ve GPU tabanlı düğüm Havuzu. Kümenizdeki düğümleri görüntülemek için [kubectl Get Nodes][kubectl-get] komutunu kullanın. Aşağıdaki örnek çıktıda her düğüm havuzundaki bir düğüm gösterilmektedir:

```console
$ kubectl get nodes

NAME                                 STATUS   ROLES   AGE     VERSION
aks-gpunodepool-28993262-vmss000000  Ready    agent   4m22s   v1.13.5
aks-nodepool1-28993262-vmss000000    Ready    agent   115m    v1.13.5
```

Kubernetes Zamanlayıcı, düğümlerde hangi iş yüklerinin çalıştırılacağını kısıtlamak için tatları ve toleranları kullanabilir.

* Yalnızca belirli yığınların zamanlanabileceğini gösteren bir düğüme bir **taınt** uygulanır.
* Daha sonra bir **tolerans** , düğümün Taint 'e kabul etmesine izin veren bir pod öğesine uygulanır.

Gelişmiş Kubernetes zamanlanmış özelliklerini kullanma hakkında daha fazla bilgi için bkz. [AKS 'de gelişmiş Zamanlayıcı özellikleri Için en iyi yöntemler][taints-tolerations]

Bu örnekte, [kubectl taınt node][kubectl-taint] komutunu kullanarak GPU tabanlı düğümünüz için bir Taint uygulayın. Önceki `kubectl get nodes` komutun çıktısından GPU tabanlı düğümünüz adını belirtin. Taınt bir anahtar olarak uygulanır *: değer* ve ardından zamanlama seçeneği. Aşağıdaki örnek *SKU = GPU* çiftini kullanır ve pod 'yi tanımlar, aksi takdirde *NoSchedule* özelliğine sahiptir:

```console
kubectl taint node aks-gpunodepool-28993262-vmss000000 sku=gpu:NoSchedule
```

Aşağıdaki temel örnek YAML bildirimi, Kubernetes Scheduler 'ın GPU tabanlı düğümde bir NGıNX Pod çalıştırmasına izin vermek için bir tolerans kullanır. Daha uygun olan, ancak veri kümesine karşı bir TensorFlow işi çalıştırmak için yoğun zaman tüketen bir örnek için bkz. [AKS üzerinde işlem yoğunluğu yoğun iş yükleri Için GPU 'Ları kullanma][gpu-cluster].

Adlı `gpu-toleration.yaml` bir dosya oluşturun ve aşağıdaki örnekteki YAML 'yi kopyalayın:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - image: nginx:1.15.9
    name: mypod
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 1
        memory: 2G
  tolerations:
  - key: "sku"
    operator: "Equal"
    value: "gpu"
    effect: "NoSchedule"
```

Şu `kubectl apply -f gpu-toleration.yaml` komutu kullanarak Pod 'u zamanlayın:

```console
kubectl apply -f gpu-toleration.yaml
```

Pod 'u zamanlamak ve NGıNX görüntüsünü çekmek birkaç saniye sürer. Pod durumunu görüntülemek için [kubectl betimleyen Pod][kubectl-describe] komutunu kullanın. Aşağıdaki sıkıştırılmış örnek çıktı *SKU = GPU: NoSchedule* toleranation ' ı gösterir. Olaylar bölümünde Zamanlayıcı, Pod 'yi *aks-gpunodepool-28993262-vmss000000* GPU tabanlı düğüme atamıştır:

```console
$ kubectl describe pod mypod

[...]
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
                 sku=gpu:NoSchedule
Events:
  Type    Reason     Age    From                                          Message
  ----    ------     ----   ----                                          -------
  Normal  Scheduled  4m48s  default-scheduler                             Successfully assigned default/mypod to aks-gpunodepool-28993262-vmss000000
  Normal  Pulling    4m47s  kubelet, aks-gpunodepool-28993262-vmss000000  pulling image "nginx:1.15.9"
  Normal  Pulled     4m43s  kubelet, aks-gpunodepool-28993262-vmss000000  Successfully pulled image "nginx:1.15.9"
  Normal  Created    4m40s  kubelet, aks-gpunodepool-28993262-vmss000000  Created container
  Normal  Started    4m40s  kubelet, aks-gpunodepool-28993262-vmss000000  Started container
```

Yalnızca bu taınt uygulanmış olan bir düğüm, *gpunodepool*içindeki düğümlerde zamanlanabilir. Diğer Pod 'lar *nodepool1* düğüm havuzunda zamanlanır. Ek düğüm havuzları oluşturursanız, bu düğüm kaynakları üzerinde hangi yığınların zamanlanabileceği ile ilgili ek litre ve toleransyonlar kullanabilirsiniz.

## <a name="manage-node-pools-using-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak düğüm havuzlarını yönetme

Kaynakları oluşturmak ve yönetmek için bir Azure Resource Manager şablonu kullandığınızda, genellikle şablonunuzda ayarları güncelleştirebilir ve kaynağı güncelleştirmek için yeniden dağıtabilirsiniz. AKS içindeki düğüm havuzlarıyla, AKS kümesi oluşturulduktan sonra ilk düğüm havuzu profili güncelleştirilemiyor. Bu davranış, mevcut bir Kaynak Yöneticisi şablonunu güncelleştiremeyeceğiniz, düğüm havuzlarında değişiklik yapamayacağı ve yeniden dağımeyeceğiniz anlamına gelir. Bunun yerine, yalnızca mevcut bir AKS kümesi için aracı havuzlarını güncelleştiren ayrı bir Kaynak Yöneticisi şablonu oluşturmanız gerekir.

Gibi bir şablon `aks-agentpools.json` oluşturun ve aşağıdaki örnek bildirimi yapıştırın. Bu örnek şablon aşağıdaki ayarları yapılandırır:

* *Myagentpool* adlı *Linux* aracı havuzunu üç düğüm çalıştıracak şekilde güncelleştirir.
* Düğüm havuzundaki düğümleri Kubernetes sürüm *1.13.5*çalıştıracak şekilde ayarlar.
* Düğüm boyutunu *Standard_DS2_v2*olarak tanımlar.

Gerektiğinde düğüm havuzlarını güncelleştirme, ekleme veya silme gereksinimi olarak bu değerleri düzenleyin:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "clusterName": {
      "type": "string",
      "metadata": {
        "description": "The name of your existing AKS cluster."
      }
    },
    "location": {
      "type": "string",
      "metadata": {
        "description": "The location of your existing AKS cluster."
      }
    },
    "agentPoolName": {
      "type": "string",
      "defaultValue": "myagentpool",
      "metadata": {
        "description": "The name of the agent pool to create or update."
      }
    },
    "vnetSubnetId": {
      "type": "string",
      "defaultValue": "",
      "metadata": {
        "description": "The Vnet subnet resource ID for your existing AKS cluster."
      }
    }
  },
  "variables": {
    "apiVersion": {
      "aks": "2019-04-01"
    },
    "agentPoolProfiles": {
      "maxPods": 30,
      "osDiskSizeGB": 0,
      "agentCount": 3,
      "agentVmSize": "Standard_DS2_v2",
      "osType": "Linux",
      "vnetSubnetId": "[parameters('vnetSubnetId')]"
    }
  },
  "resources": [
    {
      "apiVersion": "2019-04-01",
      "type": "Microsoft.ContainerService/managedClusters/agentPools",
      "name": "[concat(parameters('clusterName'),'/', parameters('agentPoolName'))]",
      "location": "[parameters('location')]",
      "properties": {
            "maxPods": "[variables('agentPoolProfiles').maxPods]",
            "osDiskSizeGB": "[variables('agentPoolProfiles').osDiskSizeGB]",
            "count": "[variables('agentPoolProfiles').agentCount]",
            "vmSize": "[variables('agentPoolProfiles').agentVmSize]",
            "osType": "[variables('agentPoolProfiles').osType]",
            "storageProfile": "ManagedDisks",
      "type": "VirtualMachineScaleSets",
            "vnetSubnetID": "[variables('agentPoolProfiles').vnetSubnetId]",
            "orchestratorVersion": "1.13.5"
      }
    }
  ]
}
```

Bu şablonu, aşağıdaki örnekte gösterildiği gibi [az Group Deployment Create][az-group-deployment-create] komutunu kullanarak dağıtın. Mevcut AKS kümesi adı ve konumu sorulur:

```azurecli-interactive
az group deployment create \
    --resource-group myResourceGroup \
    --template-file aks-agentpools.json
```

Kaynak Yöneticisi şablonunuzda tanımladığınız düğüm havuzu ayarlarına ve işlemlerine bağlı olarak AKS kümenizin güncelleştirilmesi birkaç dakika sürebilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede, GPU tabanlı düğümleri içeren bir AKS kümesi oluşturdunuz. Gereksiz maliyeti azaltmak için, *gpunodepool*veya tüm aks kümesini silmek isteyebilirsiniz.

GPU tabanlı düğüm havuzunu silmek için, aşağıdaki örnekte gösterildiği gibi [az aks nodepool Delete][az-aks-nodepool-delete] komutunu kullanın:

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name gpunodepool
```

Kümeyi silmek için, AKS kaynak grubunu silmek için [az Group Delete][az-group-delete] komutunu kullanın:

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir AKS kümesinde birden çok düğüm havuzu oluşturmayı ve yönetmeyi öğrendiniz. Düğüm havuzlarının tamamında nasıl denetim yapılacağı hakkında daha fazla bilgi için bkz. [AKS 'de gelişmiş Zamanlayıcı özellikleri Için en iyi yöntemler][operator-best-practices-advanced-scheduler].

Windows Server kapsayıcısı düğüm havuzlarını oluşturmak ve kullanmak için bkz. [AKS 'de Windows Server kapsayıcısı oluşturma][aks-windows].

<!-- EXTERNAL LINKS -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-taint]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#taint
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- INTERNAL LINKS -->
[azure-cli-install]: /cli/azure/install-azure-cli
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-nodepool-add]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-add
[az-aks-nodepool-list]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-list
[az-aks-nodepool-upgrade]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-upgrade
[az-aks-nodepool-scale]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-scale
[az-aks-nodepool-delete]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-delete
[vm-sizes]: ../virtual-machines/linux/sizes.md
[taints-tolerations]: operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations
[gpu-cluster]: gpu-cluster.md
[az-group-delete]: /cli/azure/group#az-group-delete
[install-azure-cli]: /cli/azure/install-azure-cli
[supported-versions]: supported-kubernetes-versions.md
[operator-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[aks-windows]: windows-container-cli.md
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
