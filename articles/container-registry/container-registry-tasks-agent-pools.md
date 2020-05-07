---
title: Görevi çalıştırmak için adanmış havuzu kullan-görevler
description: Azure Container Registry bir görevi çalıştırmak için kayıt defterinizde adanmış bir işlem havuzu (aracı havuzu) ayarlayın.
ms.topic: article
ms.date: 04/30/2020
ms.openlocfilehash: 9eac8d620dd6ce268692494499ade39173fc9b75
ms.sourcegitcommit: 602e6db62069d568a91981a1117244ffd757f1c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82864662"
---
# <a name="run-an-acr-task-on-a-dedicated-agent-pool"></a>Adanmış bir aracı havuzunda ACR görevi çalıştırma

[Azure Container Registry görevlerinizi][acr-tasks] ayrılmış bir işlem ortamında çalıştırmaya olanak tanımak için Azure tarafından YÖNETILEN bir VM Havuzu (*Aracı havuzu*) ayarlayın. Kayıt defterinizde bir veya daha fazla havuz yapılandırdıktan sonra, bir havuzu seçerek hizmetin varsayılan işlem ortamının yerine bir görevi çalıştırabilirsiniz.

Bir aracı havuzu şunları sağlar:

- **Sanal ağ desteği** -bir kapsayıcı kayıt defteri, Anahtar Kasası veya depolama gibi VNET 'teki kaynaklara erişim sağlayan bir Azure VNET 'e bir aracı havuzu atayın.
- **Gereken şekilde ölçeklendirin** işlem yoğunluğu yoğun görevler için bir aracı havuzundaki örnek sayısını artırın veya sıfıra ölçeklendirin. Faturalandırma, ayırmayı temel alır.
- **Esnek seçenekler** -görev iş yükü ihtiyaçlarınızı karşılamak için farklı [Havuz katmanları](#pool-tiers) ve ölçek seçenekleri arasından seçim yapın.
- **Azure Yönetim** -görev havuzları, Azure tarafından düzeltme ve bakım yapılır ve tek tek VM 'lerin bakımını gerektirmeden ayrılmış ayırma sağlar.

Bu özellik **Premium** kapsayıcı kayıt defteri hizmet katmanında kullanılabilir. Kayıt defteri hizmeti katmanları ve limitleri hakkında bilgi için bkz. [Azure Container Registry SKU 'lar][acr-tiers].

> [!IMPORTANT]
> Bu özellik şu anda önizleme aşamasındadır ve bazı [sınırlamalar geçerlidir](#preview-limitations). Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.
>

## <a name="preview-limitations"></a>Önizleme sınırlamaları

- Görev aracısı havuzları Şu anda Linux düğümlerini destekliyor. Windows düğümleri Şu anda desteklenmiyor.
- Görev aracısı havuzları önizlemede şu bölgelerde kullanılabilir: Batı ABD 2, Orta Güney ABD, Doğu ABD 2 ve Doğu ABD.
- Her kayıt defteri için, tüm aracı havuzları için varsayılan toplam vCPU (çekirdek) kotası 16 ' dır. Ek ayırma için bir [destek isteği][open-support-ticket] açın.

## <a name="prerequisites"></a>Ön koşullar

* Bu makalede Azure CLı adımlarını kullanmak için Azure CLı sürüm 2.3.1 veya üzeri gereklidir. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli]. Veya [Azure Cloud Shell](../cloud-shell/quickstart.md)içinde çalıştırın.
* Zaten bir kapsayıcı kayıt defteriniz yoksa, bir önizleme bölgesinde (Premium katman gereklidir) [bir tane oluşturun][create-reg-cli] .

## <a name="pool-tiers"></a>Havuz katmanları

Aracı havuzu katmanları, havuzda örnek başına aşağıdaki kaynakları sağlar.

|Katman  |CPU  |Bellek (GB)  |
|---------|---------|---------|
|S1     |  2       |  3       |
|S2     |  4       |   8      |
|S3     |    8     |   16      |

## <a name="create-and-manage-a-task-agent-pool"></a>Görev Aracısı havuzu oluşturma ve yönetme

### <a name="set-default-registry-optional"></a>Varsayılan kayıt defteri ayarla (isteğe bağlı)

Aşağıdaki Azure CLı komutlarını basitleştirmek için, [az configure][az-configure] komutunu çalıştırarak varsayılan kayıt defterini ayarlayın:

```azurecli
az configure --defaults acr=<registryName>
```

Aşağıdaki örneklerde varsayılan kayıt defteri belirlediğiniz varsayılır. Aksi takdirde, her `--registry <registryName>` `az acr` komutta bir parametre geçirin.

### <a name="create-agent-pool"></a>Aracı Havuzu Oluştur

[Az ACR agentpool Create][az-acr-agentpool-create] komutunu kullanarak bir aracı havuzu oluşturun. Aşağıdaki örnek, bir S2 Pool Havuzu (4 CPU/örnek) oluşturur. Varsayılan olarak, havuz 1 örnek içerir.

```azurecli
az acr agentpool create \
    --name myagentpool \
    --tier S2
```

Bir aracı havuzunun oluşturulması ve diğer havuz yönetimi işlemlerinin tamamlanması birkaç dakika sürer.

### <a name="scale-pool"></a>Ölçek havuzu

[Az ACR agentpool Update][az-acr-agentpool-update] komutuyla havuz boyutunu yukarı veya aşağı ölçeklendirin. Aşağıdaki örnek, havuzu 2 örneğe ölçeklendirir. 0 örneğe ölçeklendirebilirsiniz.

```azurecli
az acr agentpool update \
    --name myagentpool \
    --count 2
```

## <a name="create-pool-in-a-virtual-network"></a>Sanal ağ içinde havuz oluşturma

### <a name="add-firewall-rules"></a>Güvenlik duvarı kuralları ekleme

Görev Aracısı havuzlarının aşağıdaki Azure hizmetlerine erişmesi gerekir. Aşağıdaki güvenlik duvarı kuralları var olan ağ güvenlik gruplarına veya Kullanıcı tanımlı yollara eklenmelidir.

| Yön | Protokol | Kaynak         | Kaynak Bağlantı Noktası | Hedef          | Hedef bağlantı noktası | Kullanılamıyor    |
|-----------|----------|----------------|-------------|----------------------|-----------|---------|
| Giden  | TCP      | VirtualNetwork | Herhangi biri         | Depolama              | 443       | Varsayılan |
| Giden  | TCP      | VirtualNetwork | Herhangi biri         | EventHub             | 443       | Varsayılan |
| Giden  | TCP      | VirtualNetwork | Herhangi biri         | AzureActiveDirectory | 443       | Varsayılan |
| Giden  | TCP      | VirtualNetwork | Herhangi biri         | AzureMonitor         | 443       | Varsayılan |

> [!NOTE]
> Görevleriniz ortak internet 'ten ek kaynaklar gerektiriyorsa, karşılık gelen kuralları ekleyin. Örneğin, Docker Hub 'ından temel görüntüleri çekmek için gereken bir Docker Build görevi çalıştırırsanız veya bir NuGet paketini geri yüklerken bu kurallar gereklidir.

### <a name="create-pool-in-vnet"></a>VNet 'te havuz oluşturma

Aşağıdaki örnek, ağ *myvnet*'in *mysubnet* alt ağında bir aracı havuzu oluşturur:

```azurecli
# Get the subnet ID
subnetId=$(az network vnet subnet show \
        --resource-grop myresourcegroup \
        --vnet-name myvnet \
        --name mysubnetname \
        --query id --output tsv)

az acr agentpool create \
    --name myagentpool \
    --tier S2 \
    --subnet-id $subnetId
```

## <a name="run-task-on-agent-pool"></a>Görevi aracı havuzunda Çalıştır

Aşağıdaki örneklerde, bir görev sıraya alınırken bir aracı havuzunun nasıl belirtilmesinin gösterilmektedir.

> [!NOTE]
> Bir ACR görevinde bir aracı havuzu kullanmak için, havuzun en az 1 örnek içerdiğinden emin olun.
>

### <a name="quick-task-run"></a>Hızlı görev çalıştırma

[Az ACR Build][az-acr-build] komutunu kullanarak aracı havuzunda hızlı bir Çalıştır sıraya alın ve `--agent-pool` parametreyi geçirin:

```azurecli
az acr build \
    --agent-pool myagentpool \
    --image myimage:mytag \
    --file Dockerfile \
    https://github.com/Azure-Samples/acr-build-helloworld-node.git
```

### <a name="automatically-triggered-task"></a>Otomatik olarak tetiklenen görev

Örneğin, aracı havuzunda [az ACR Task Create][az-acr-task-create]ile bir zamanlanmış görev oluşturun ve `--agent-pool` parametresini geçirerek.

```azurecli
az acr task create \
    --name mytask \
    --agent-pool myagentpool \
    --image myimage:mytag \
    --schedule "0 21 * * *" \
    --file Dockerfile \
    --context https://github.com/Azure-Samples/acr-build-helloworld-node.git \
    --commit-trigger-enabled false
```

Görev kurulumunu doğrulamak için [az ACR görev çalıştırmasını][az-acr-task-run]çalıştırın:

```azurecli
az acr task run \
    --name mytask
```

### <a name="query-pool-status"></a>Sorgu havuzu durumu

Aracı havuzunda Şu anda zamanlanan çalışma sayısını bulmak için [az ACR agentpool Show][az-acr-agentpool-show]' ı çalıştırın.

```azurecli
az acr agentpool show \
    --name myagentpool \
    --queue-count
```

## <a name="next-steps"></a>Sonraki adımlar

Bulutta kapsayıcı görüntüsü derlemeleri ve bakım hakkında daha fazla örnek için [ACR görevler öğretici serisini](container-registry-tutorial-quick-task.md)inceleyin.



[acr-tasks]:           container-registry-tasks-overview.md
[acr-tiers]:           container-registry-skus.md
[azure-cli]:           /cli/azure/install-azure-cli
[open-support-ticket]: https://aka.ms/acr/support/create-ticket
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[az-configure]: /cli/azure#az-configure
[az-acr-agentpool-create]: /cli/azure/acr/agentpool#az-acr-agentpool-create
[az-acr-agentpool-update]: /cli/azure/acr/agentpool#az-acr-agentpool-update
[az-acr-agentpool-show]: /cli/azure/acr/agentpool#az-acr-agentpool-show
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[create-reg-cli]: container-registry-get-started-azure-cli.md
