---
title: Azure Kubernetes hizmetlerindeki küme yapılandırması (AKS)
description: Azure Kubernetes hizmeti 'nde (AKS) bir kümeyi yapılandırmayı öğrenin
services: container-service
ms.topic: conceptual
ms.date: 08/06/2020
ms.author: jpalma
author: palma21
ms.openlocfilehash: daffcbf0a2ceb6f28cbb539906d4c6387840aa20
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "88752090"
---
# <a name="configure-an-aks-cluster"></a>AKS kümesini yapılandırma

AKS kümesi oluşturmanın bir parçası olarak, Küme yapılandırmanızı gereksinimlerinize uyacak şekilde özelleştirmeniz gerekebilir. Bu makalede AKS kümenizi özelleştirmek için birkaç seçenek sunulmaktadır.

## <a name="os-configuration-preview"></a>İşletim sistemi yapılandırması (Önizleme)

AKS artık, önizleme aşamasında düğüm işletim sistemi (OS) olarak Ubuntu 18,04 ' i desteklemektedir. Önizleme dönemi boyunca hem Ubuntu 16,04 hem de Ubuntu 18,04 kullanılabilir.

> [!IMPORTANT]
> Düğüm havuzları, Kubernetes v 1.18 üzerinde veya daha fazla varsayılan olarak gerekli bir `AKS Ubuntu 18.04` düğüm görüntüsüne oluşturulur. Desteklenen bir Kubernetes sürümündeki düğüm havuzları, `AKS Ubuntu 16.04` düğüm görüntüsü olarak 1,18 'den azdır, ancak `AKS Ubuntu 18.04` düğüm havuzu Kubernetes sürümü v 1.18 veya üzeri olarak güncelleştirildikten sonra olarak güncelleştirilecektir.
> 
> 1,18 veya üzeri kümeler kullanılmadan önce AKS Ubuntu 18,04 düğüm havuzlarındaki iş yüklerinizi test etmek önemle önerilir. [Ubuntu 18,04 düğüm havuzlarını test](#use-aks-ubuntu-1804-existing-clusters-preview)etme hakkında bilgi edinin.

Aşağıdaki kaynakların yüklü olması gerekir:

- [Azure CLI][azure-cli-install], sürüm 2.2.0 veya üzeri
- Aks-Preview 0.4.35 uzantısı

Aks-Preview 0.4.35 uzantısını veya üstünü yüklemek için aşağıdaki Azure CLı komutlarını kullanın:

```azurecli
az extension add --name aks-preview
az extension list
```

Özelliği kaydedin `UseCustomizedUbuntuPreview` :

```azurecli
az feature register --name UseCustomizedUbuntuPreview --namespace Microsoft.ContainerService
```

Durumun **kayıtlı**olarak gösterilmesi birkaç dakika sürebilir. [Az Feature List](/cli/azure/feature?view=azure-cli-latest#az-feature-list) komutunu kullanarak kayıt durumunu kontrol edebilirsiniz:

```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/UseCustomizedUbuntuPreview')].{Name:name,State:properties.state}"
```

Durum kayıtlı olarak görünüyorsa, `Microsoft.ContainerService` [az Provider Register](/cli/azure/provider?view=azure-cli-latest#az-provider-register) komutunu kullanarak kaynak sağlayıcının kaydını yenileyin:

```azurecli
az provider register --namespace Microsoft.ContainerService
```

### <a name="use-aks-ubuntu-1804-on-new-clusters-preview"></a>Yeni kümeler üzerinde AKS Ubuntu 18,04 kullanma (Önizleme)

Kümeyi, küme oluşturulduğunda Ubuntu 18,04 kullanacak şekilde yapılandırın. `--aks-custom-headers`Ubuntu 18,04 ' i varsayılan işletim sistemi olarak ayarlamak için bayrağını kullanın.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804
```

AKS Ubuntu 16,04 görüntüsüyle kümeler oluşturmak istiyorsanız, özel etiketi atlayarak bunu yapabilirsiniz `--aks-custom-headers` .

### <a name="use-aks-ubuntu-1804-existing-clusters-preview"></a>AKS Ubuntu 18,04 mevcut kümelerini kullanma (Önizleme)

Ubuntu 18,04 kullanmak için yeni bir düğüm havuzu yapılandırın. `--aks-custom-headers`Bu düğüm havuzu Için Ubuntu 18,04 ' ı varsayılan işletim sistemi olarak ayarlamak için bayrağını kullanın.

```azurecli
az aks nodepool add --name ubuntu1804 --cluster-name myAKSCluster --resource-group myResourceGroup --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804
```

AKS Ubuntu 16,04 görüntüsüyle düğüm havuzları oluşturmak istiyorsanız, özel etiketi atlayarak bunu yapabilirsiniz `--aks-custom-headers` .


## <a name="container-runtime-configuration-preview"></a>Kapsayıcı çalışma zamanı yapılandırması (Önizleme)

Kapsayıcı çalışma zamanı, kapsayıcıları yürüten ve bir düğümdeki kapsayıcı görüntülerini yöneten yazılımdır. Çalışma zamanı,, Linux veya Windows üzerinde kapsayıcıları çalıştırmak için, özel sys çağrıları veya işletim sistemi (OS) işlevine özgü işlevselliğin sağlanmasına yardımcı olur. Bugün AKS, kapsayıcı çalışma zamanı olarak [Moby](https://mobyproject.org/) (yukarı akış Docker) kullanıyor. 
    
![Docker CRı 1](media/cluster-configuration/docker-cri.png)

[`Containerd`](https://containerd.io/) , kapsayıcıları yürütmek ve bir düğümdeki görüntüleri yönetmek için gereken minimum işlev kümesini sağlayan bir [OCI](https://opencontainers.org/) (açık kapsayıcı girişimi) ile uyumlu bir çekirdek kapsayıcı çalışma zamanı. , Mart 2017 ' de Cloud Native COMPUTE Foundation 'a (CNCF) [bağlandı](https://www.cncf.io/announcement/2017/03/29/containerd-joins-cloud-native-computing-foundation/) . AKS 'in bugün kullandığı güncel Moby sürümü, `containerd` yukarıda gösterildiği gibi, üzerine kurulmuştur ve üzerinde yerleşiktir. 

Bir containerd tabanlı düğüm ve düğüm havuzları ile konuşmak yerine, `dockershim` kubelet, `containerd` DOCKER cri uygulamasıyla karşılaştırıldığında akıştaki ek atlamaları kaldırarak doğrudan CRI (container Runtime Interface) eklentisi aracılığıyla konuşuyor. Bu nedenle, daha iyi Pod başlangıç gecikmesi ve daha az kaynak (CPU ve bellek) kullanımı görürsünüz.

`containerd`AKS düğümleri için kullanarak, Pod başlangıç gecikmesi artar ve kapsayıcı çalışma zamanına göre düğüm kaynak tüketimi azalır. Bu geliştirmeler, kubelet 'in, `containerd` Moby/Docker mimarisi kuı eklentisini kullanarak doğrudan CRI eklentisi aracılığıyla konuştukça, bu yeni mimari tarafından etkinleştirilir `dockershim` ve bu `containerd` sayede akışta ek atlamaları vardır.

![Docker CRı 2](media/cluster-configuration/containerd-cri.png)

`Containerd` AKS 'deki Kubernetes 'in her bir GA sürümünde ve v 1,10 üzerindeki her yukarı akış Kubernetes sürümünde çalışarak, tüm Kubernetes ve AKS özelliklerini destekler.

> [!IMPORTANT]
> `containerd`AKS üzerinde genel kullanıma sunulduğunda, yeni kümelerde kapsayıcı çalışma zamanı için varsayılan ve tek seçeneği kullanılabilir. Moby nodepools ve kümelerini kullanmaya devam edebilirsiniz, ancak bu, destek düşene kadar desteklenen eski sürümlerde. 
> 
> `containerd`Bu kapsayıcı çalışma zamanına sahip yeni kümeler yükseltmeden veya oluşturmadan önce iş yüklerinizi düğüm havuzlarında test etmenizi öneririz.

### <a name="use-containerd-as-your-container-runtime-preview"></a>`containerd`Kapsayıcı çalışma zamanı olarak kullanın (Önizleme)

Aşağıdaki önkoşulların olması gerekir:

- [Azure CLI][azure-cli-install], sürüm 2.8.0 veya üzeri yüklü
- Aks-Preview uzantısı sürüm 0.4.53 veya üzeri
- `UseCustomizedContainerRuntime`Özellik bayrağı kaydedildi
- `UseCustomizedUbuntuPreview`Özellik bayrağı kaydedildi

Aks-Preview 0.4.53 uzantısını veya üstünü yüklemek için aşağıdaki Azure CLı komutlarını kullanın:

```azurecli
az extension add --name aks-preview
az extension list
```

`UseCustomizedContainerRuntime`Ve özelliklerini kaydedin `UseCustomizedUbuntuPreview` :

```azurecli
az feature register --name UseCustomizedContainerRuntime --namespace Microsoft.ContainerService
az feature register --name UseCustomizedUbuntuPreview --namespace Microsoft.ContainerService

```

Durumun **kayıtlı**olarak gösterilmesi birkaç dakika sürebilir. [Az Feature List](/cli/azure/feature?view=azure-cli-latest#az-feature-list) komutunu kullanarak kayıt durumunu kontrol edebilirsiniz:

```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/UseCustomizedContainerRuntime')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/UseCustomizedUbuntuPreview')].{Name:name,State:properties.state}"
```

Durum kayıtlı olarak görünüyorsa, `Microsoft.ContainerService` [az Provider Register](/cli/azure/provider?view=azure-cli-latest#az-provider-register) komutunu kullanarak kaynak sağlayıcının kaydını yenileyin:

```azurecli
az provider register --namespace Microsoft.ContainerService
```  

### <a name="use-containerd-on-new-clusters-preview"></a>`containerd`Yeni kümeler üzerinde kullan (Önizleme)

Kümeyi, küme oluşturulduğunda kullanılacak şekilde yapılandırın `containerd` . `--aks-custom-headers` `containerd` Kapsayıcı çalışma zamanı olarak ayarlamak için bayrağını kullanın.

> [!NOTE]
> `containerd`Çalışma zamanı yalnızca AKS Ubuntu 18,04 görüntüsünü kullanan düğümlerde ve düğüm havuzlarında desteklenir.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804,ContainerRuntime=containerd
```

Moby (Docker) çalışma zamanına sahip kümeler oluşturmak istiyorsanız, özel etiketi atlayarak bunu yapabilirsiniz `--aks-custom-headers` .

### <a name="use-containerd-on-existing-clusters-preview"></a>`containerd`Mevcut kümeler üzerinde kullan (Önizleme)

Kullanılacak yeni bir düğüm havuzu yapılandırın `containerd` . `--aks-custom-headers` `containerd` Bu düğüm havuzu için çalışma zamanı olarak ayarlamak üzere bayrağını kullanın.

```azurecli
az aks nodepool add --name ubuntu1804 --cluster-name myAKSCluster --resource-group myResourceGroup --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804,ContainerRuntime=containerd
```

Moby (Docker) çalışma zamanı ile düğüm havuzları oluşturmak istiyorsanız, özel etiketi atlayarak bunu yapabilirsiniz `--aks-custom-headers` .


### <a name="containerd-limitationsdifferences"></a>`Containerd` sınırlamalar/farklar

* `containerd`Kapsayıcı çalışma zamanı olarak kullanmak için, temel işletim sistemi görüntünüz olarak AKS Ubuntu 18,04 kullanmanız gerekir.
* Docker araç takımı düğümlerde hala mevcut olsa da, Kubernetes `containerd` kapsayıcı çalışma zamanı olarak kullanır. Bu nedenle, Moby/Docker düğümlerde Kubernetes tarafından oluşturulan kapsayıcıları yönetmediğinden Docker komutlarını (gibi `docker ps` ) veya Docker API 'sini kullanarak Kapsayıcılarınızı görüntüleyemez veya bunlarla etkileşime giremezseniz.
* İçin `containerd` , [`crictl`](https://kubernetes.io/docs/tasks/debug-application-cluster/crictl) Kubernetes düğümlerinde (örneğin,) pods, kapsayıcılar ve kapsayıcı görüntüleri **sorunlarını gidermeye** yönelik Docker CLI yerine değiştirme CLI olarak kullanmanızı öneririz `crictl ps` . 
   * Docker CLı 'nin bütün işlevlerini sağlamaz. Yalnızca sorun gidermeye yöneliktir.
   * `crictl` , pods gibi kavramların yanı sıra mevcut olan kapsayıcıların daha fazla Kubernetes görünümünü sunar.
* `Containerd` standartlaştırılmış günlük biçimini kullanarak günlük kaydı yapar `cri` (Şu anda Docker 'ın JSON sürücüsünden aldığınız verilerden farklıdır). Günlük çözümünüzün `cri` günlük biçimini desteklemesi gerekir ( [kapsayıcılar Için Azure izleyici](../azure-monitor/insights/container-insights-enable-new-cluster.md)gibi)
* Artık Docker altyapısına erişemez veya Docker- `/var/run/docker.sock` ın-Docker (Dintıd) kullanabilirsiniz.
  * Şu anda Docker altyapısından uygulama günlükleri veya izleme verileri ayıklandıysanız, lütfen bunun yerine [kapsayıcılar Için Azure izleyici](../azure-monitor/insights/container-insights-enable-new-cluster.md) gibi bir şey kullanın. Ayrıca AKS, kararsızlığa neden olabilecek aracı düğümlerinde bant dışı komutların çalıştırılmasını desteklemez.
  * Moby/Docker kullanırken, görüntülerin oluşturulması ve yukarıdaki yöntemler aracılığıyla Docker altyapısının doğrudan kullanılmasıyla kesinlikle önerilmez. Kubernetes, bu tüketilen kaynakların tamamen farkında değildir ve bu yaklaşımlar [burada](https://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/) [ve burada ayrıntılı olarak ayrıntılı](https://securityboulevard.com/2018/05/escaping-the-whale-things-you-probably-shouldnt-do-with-docker-part-1/)bir şekilde ortaya bulunur.
* Görüntü oluşturma-görüntü oluşturmak için önerilen yaklaşım [ACR görevlerini](../container-registry/container-registry-quickstart-task-cli.md)kullanmaktır. Alternatif bir yaklaşım, [Docker buildx](https://github.com/docker/buildx)gibi daha güvenli küme içi seçenekler kullanmaktır.

## <a name="generation-2-virtual-machines-preview"></a>2. nesil sanal makineler (Önizleme)

Azure [2. nesil (Gen2) sanal makineleri (VM)](../virtual-machines/windows/generation-2.md)destekler. 2. nesil VM 'Ler, 1. nesil VM 'lerde desteklenmeyen önemli özellikleri destekler (Gen1). Bu özellikler, artan bellek, Intel Software Guard uzantıları (Intel SGX) ve sanallaştırılmış kalıcı bellek (vPMEM) içerir.

2. nesil sanal makineler, 1. nesil VM 'Ler tarafından kullanılan BIOS tabanlı mimaride değil, yeni UEFı tabanlı önyükleme mimarisini kullanır.
Yalnızca belirli SKU 'Lar ve boyutlar Gen2 VM 'Leri destekler. SKU 'nuzun Gen2 destekleyip desteklemediğini veya gerektirip gerektirmediğini görmek için [Desteklenen boyutlar listesini](../virtual-machines/windows/generation-2.md#generation-2-vm-sizes)kontrol edin.

Ayrıca, AKS Gen2 VM 'lerinde Gen2 desteği olan tüm VM görüntüleri, yeni [aks Ubuntu 18,04 görüntüsünü](#os-configuration-preview)kullanır. Bu görüntü tüm Gen2 SKU 'Larını ve boyutlarını destekler.

Önizleme sırasında Gen2 VM 'Leri kullanmak için şunları yapmanız gerekir:
- `aks-preview`CLI uzantısı yüklendi.
- `Gen2VMPreview`Özellik bayrağı kaydedildi.

Özelliği kaydedin `Gen2VMPreview` :

```azurecli
az feature register --name Gen2VMPreview --namespace Microsoft.ContainerService
```

Durumun **kayıtlı**olarak gösterilmesi birkaç dakika sürebilir. [Az Feature List](/cli/azure/feature?view=azure-cli-latest#az-feature-list) komutunu kullanarak kayıt durumunu kontrol edebilirsiniz:

```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/Gen2VMPreview')].{Name:name,State:properties.state}"
```

Durum kayıtlı olarak görünüyorsa, `Microsoft.ContainerService` [az Provider Register](/cli/azure/provider?view=azure-cli-latest#az-provider-register) komutunu kullanarak kaynak sağlayıcının kaydını yenileyin:

```azurecli
az provider register --namespace Microsoft.ContainerService
```

Aks-Preview CLı uzantısını yüklemek için aşağıdaki Azure CLı komutlarını kullanın:

```azurecli
az extension add --name aks-preview
```

Aks-Preview CLı uzantısını güncelleştirmek için aşağıdaki Azure CLı komutlarını kullanın:

```azurecli
az extension update --name aks-preview
```

### <a name="use-gen2-vms-on-new-clusters-preview"></a>Yeni kümeler üzerinde Gen2 VM 'Leri kullanma (Önizleme)
Kümeyi, küme oluşturulduğunda seçili SKU için Gen2 VM 'Leri kullanacak şekilde yapılandırın. `--aks-custom-headers`Yeni bir kümede VM oluşturma olarak Gen2 ayarlamak için bayrağını kullanın.

```azure-cli
az aks create --name myAKSCluster --resource-group myResourceGroup -s Standard_D2s_v3 --aks-custom-headers usegen2vm=true
```

1. nesil (Gen1) VM 'Leri kullanarak düzenli bir küme oluşturmak istiyorsanız, özel etiketi atlayarak bunu yapabilirsiniz `--aks-custom-headers` . Ayrıca, aşağıdaki gibi daha fazla Gen1 veya Gen2 VM eklemeyi de seçebilirsiniz.

### <a name="use-gen2-vms-on-existing-clusters-preview"></a>Mevcut kümeler üzerinde Gen2 VM 'Leri kullanma (Önizleme)
Yeni bir düğüm havuzunu Gen2 VM 'Leri kullanacak şekilde yapılandırın. `--aks-custom-headers`Gen2 'i bu düğüm havuzu IÇIN VM oluşturma olarak ayarlamak için bayrağını kullanın.

```azure-cli
az aks nodepool add --name gen2 --cluster-name myAKSCluster --resource-group myResourceGroup -s Standard_D2s_v3 --aks-custom-headers usegen2vm=true
```

Normal Gen1 düğüm havuzları oluşturmak istiyorsanız, özel etiketi atlayarak bunu yapabilirsiniz `--aks-custom-headers` .


## <a name="ephemeral-os-preview"></a>Kısa ömürlü işletim sistemi (Önizleme)

Varsayılan olarak, bir Azure sanal makinesi için işletim sistemi diski, sanal makinenin başka bir konağa yeniden konumlandırılması gereken veri kaybını önlemek için Azure depolama 'ya otomatik olarak çoğaltılır. Ancak, kapsayıcılar yerel durumunun kalıcı olmasını sağlayacak şekilde tasarlanmadığından, bu davranış, daha yavaş düğüm sağlama ve daha yüksek okuma/yazma gecikme süresi dahil olmak üzere bazı dezavantajları sağlarken sınırlı bir değer sunar.

Bunun aksine, kısa ömürlü işletim sistemi diskleri yalnızca, geçici bir disk gibi yalnızca ana makine üzerinde depolanır. Bu, daha hızlı okuma/yazma gecikmesi sağlar ve daha hızlı düğüm ölçekleme ve küme yükseltmeleriyle birlikte.

Geçici disk gibi, daha kısa bir işletim sistemi diski sanal makinenin fiyatına dahildir, bu nedenle ek depolama ücreti ödemeniz gerekmez.

Özelliği kaydedin `EnableEphemeralOSDiskPreview` :

```azurecli
az feature register --name EnableEphemeralOSDiskPreview --namespace Microsoft.ContainerService
```

Durumun **kayıtlı**olarak gösterilmesi birkaç dakika sürebilir. [Az Feature List](/cli/azure/feature?view=azure-cli-latest#az-feature-list) komutunu kullanarak kayıt durumunu kontrol edebilirsiniz:

```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/EnableEphemeralOSDiskPreview')].{Name:name,State:properties.state}"
```

Durum kayıtlı olarak görünüyorsa, `Microsoft.ContainerService` [az Provider Register](/cli/azure/provider?view=azure-cli-latest#az-provider-register) komutunu kullanarak kaynak sağlayıcının kaydını yenileyin:

```azurecli
az provider register --namespace Microsoft.ContainerService
```

Aks-Preview CLı uzantısını yüklemek için aşağıdaki Azure CLı komutlarını kullanın:

```azurecli
az extension add --name aks-preview
```

Aks-Preview CLı uzantısını güncelleştirmek için aşağıdaki Azure CLı komutlarını kullanın:

```azurecli
az extension update --name aks-preview
```

### <a name="use-ephemeral-os-on-new-clusters-preview"></a>Yeni kümelerde kısa ömürlü işletim sistemi kullan (Önizleme)

Kümeyi, küme oluşturulduğunda kısa ömürlü işletim sistemi disklerini kullanacak şekilde yapılandırın. `--aks-custom-headers`Yeni küme için işletim sistemi disk türü olarak kısa ömürlü işletim sistemi ayarlamak için bayrağını kullanın.

```azure-cli
az aks create --name myAKSCluster --resource-group myResourceGroup -s Standard_DS3_v2 --aks-custom-headers EnableEphemeralOSDisk=true
```

Ağa bağlı işletim sistemi disklerini kullanarak düzenli bir küme oluşturmak istiyorsanız, özel etiketi atlayarak bunu yapabilirsiniz `--aks-custom-headers` . Ayrıca, aşağıdaki şekilde daha kısa ömürlü işletim sistemi düğüm havuzları eklemeyi de seçebilirsiniz.

### <a name="use-ephemeral-os-on-existing-clusters-preview"></a>Mevcut kümelerde kısa ömürlü işletim sistemi kullan (Önizleme)
Kısa ömürlü işletim sistemi disklerini kullanmak için yeni bir düğüm havuzu yapılandırın. `--aks-custom-headers`Bu düğüm havuzu için işletim sistemi disk türü olarak işletim sistemi disk türü olarak ayarlamak için bayrağını kullanın.

```azure-cli
az aks nodepool add --name ephemeral --cluster-name myAKSCluster --resource-group myResourceGroup -s Standard_DS3_v2 --aks-custom-headers EnableEphemeralOSDisk=true
```

> [!IMPORTANT]
> Kısa ömürlü IŞLETIM sistemiyle VM ve örnek görüntülerini VM önbelleğinin boyutuna dağıtabilirsiniz. AKS durumunda, varsayılan işletim sistemi disk yapılandırması 100GiB kullanır, bu da 100 GiB 'den büyük bir önbelleğe sahip bir VM boyutuna ihtiyacınız olduğu anlamına gelir. Varsayılan Standard_DS2_v2, 86 GiB önbellek boyutuna sahiptir ve bu değer yeterince büyük değildir. Standard_DS3_v2, yeterince büyük olan 172 GiB önbellek boyutuna sahiptir. Ayrıca, kullanarak işletim sistemi diskinin varsayılan boyutunu azaltabilirsiniz `--node-osdisk-size` . AKS görüntülerinin en küçük boyutu 30GiB ' dir. 

Ağa bağlı işletim sistemi diskleri ile düğüm havuzları oluşturmak istiyorsanız, özel etiketi atlayarak bunu yapabilirsiniz `--aks-custom-headers` .

## <a name="custom-resource-group-name"></a>Özel kaynak grubu adı

Azure 'da bir Azure Kubernetes hizmet kümesi dağıttığınızda, çalışan düğümleri için ikinci bir kaynak grubu oluşturulur. Varsayılan olarak AKS, düğüm kaynak grubunu adı verir `MC_resourcegroupname_clustername_location` , ancak kendi adınızı de sağlayabilirsiniz.

Kendi kaynak grubu adınızı belirtmek için, aks-Preview Azure CLı uzantısı sürüm 0.3.2 veya üstünü yüklemelisiniz. Azure CLı 'yı kullanarak `--node-resource-group` `az aks create` kaynak grubu için özel bir ad belirtmek üzere komutun parametresini kullanın. AKS kümesi dağıtmak için bir Azure Resource Manager şablonu kullanıyorsanız, özelliğini kullanarak kaynak grubu adını tanımlayabilirsiniz `nodeResourceGroup` .

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --node-resource-group myNodeResourceGroup
```

İkincil kaynak grubu, kendi aboneliğinizde Azure Kaynak sağlayıcısı tarafından otomatik olarak oluşturulur. Yalnızca küme oluşturulduğunda özel kaynak grubu adı belirtebilirsiniz. 

Düğüm kaynak grubuyla çalışırken şunları yapamazsınız:

- Düğüm kaynak grubu için mevcut bir kaynak grubu belirtin.
- Düğüm kaynak grubu için farklı bir abonelik belirtin.
- Küme oluşturulduktan sonra düğüm kaynak grubu adını değiştirin.
- Düğüm kaynak grubu içindeki yönetilen kaynakların adlarını belirtin.
- Düğüm kaynak grubu içinde yönetilen kaynakların Azure tarafından oluşturulan etiketlerini değiştirin veya silin.

## <a name="next-steps"></a>Sonraki adımlar

- `Kured`Kümenizdeki [Linux düğümlerine güvenlik ve çekirdek güncelleştirmeleri uygulamak](node-updates-kured.md) için kullanmayı öğrenin.
- Kümenizi, Kubernetes 'in en son sürümüne nasıl yükselteceğinizi öğrenmek için bkz. [Azure Kubernetes hizmeti (AKS) kümesini yükseltme](upgrade-cluster.md) .
- [ `containerd` Ve Kubernetes](https://kubernetes.io/blog/2018/05/24/kubernetes-containerd-integration-goes-ga/) hakkında daha fazla bilgi edinin
- Bazı yaygın AKS sorularına cevap bulmak için [aks hakkında sık sorulan soruların](faq.md) listesine bakın.
- [Kısa ömürlü işletim sistemi diskleri](../virtual-machines/ephemeral-os-disks.md)hakkında daha fazla bilgi edinin.


<!-- LINKS - internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
