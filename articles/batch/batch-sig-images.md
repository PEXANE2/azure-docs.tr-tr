---
title: Özel bir havuz oluşturmak için Paylaşılan Resim Galerisi'ni kullanın - Azure Toplu İş | Microsoft Dokümanlar
description: Uygulamanız için gereksinim duyduğunuz yazılım ve verileri içeren düğümleri hesaplamak için özel görüntüler sağlamak için Paylaşılan Resim Galerisi ile birlikte bir Toplu Iş havuzu oluşturun. Özel görüntüler, Toplu iş yüklerinizi çalıştıracak şekilde bilgi işlem düğümlerini yapılandırmanın etkili bir yoludur.
services: batch
author: LauraBrenner
manager: evansma
ms.service: batch
ms.topic: article
ms.date: 08/28/2019
ms.author: labrenne
ms.openlocfilehash: 2cff6a0e48fc7bf58a642f509fcda6b114e002ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022945"
---
# <a name="use-the-shared-image-gallery-to-create-a-custom-pool"></a>Özel bir havuz oluşturmak için Paylaşılan Resim Galerisi'ni kullanma

Sanal Makine Yapılandırması'nı kullanarak bir Azure Toplu Iş havuzu oluşturduğunuzda, havuzdaki her işlem düğümü için işletim sistemi sağlayan bir VM görüntüsü belirtirsiniz. Desteklenen Azure Marketi görüntüsüyle sanal makineler havuzu oluşturabilir veya Paylaşılan Resim [Galerisi](../virtual-machines/windows/shared-image-galleries.md)ile özel bir resim oluşturabilirsiniz.

## <a name="benefits-of-the-shared-image-gallery"></a>Paylaşılan Resim Galerisinin Avantajları

Özel resminiz için Paylaşılan Resim Galerisi'ni kullandığınızda, işletim sistemi türü ve yapılandırmasının yanı sıra veri disklerinin türü üzerinde denetime sahip siniz. Paylaşılan Resminiz, tüm Toplu İşlem havuzu düğümlerinde kullanılabilir hale gelen uygulamaları ve başvuru verilerini, bunlar sağlanır sağlanır sağlanır sağlanmış olarak içerebilir.

Ayrıca, ortamınız için gerektiği gibi görüntünün birden çok sürümüne de sahip olabilirsiniz. VM oluşturmak için bir resim sürümü kullandığınızda, görüntü sürümü VM için yeni diskler oluşturmak için kullanılır.

Paylaşılan Görüntü'nün kullanılması, Toplu iş yükünüzü çalıştırmak için havuzunuzun işlem düğümlerini hazırlamada zamandan tasarruf sağlar. Bir Azure Marketi görüntüsünü kullanmak ve sağlamadan sonra her bir işlem düğümüne yazılım yüklemek mümkündür, ancak Paylaşılan Görüntü kullanmak genellikle daha verimlidir. Ayrıca, Paylaşılan Görüntü için birden çok yineleme belirtebilirsiniz, böylece birçok VM'si (600'den fazla VM) içeren havuzlar oluşturduğunuzda, havuz oluşturmada zamandan tasarruf edeyim.

Senaryonuz için yapılandırılan Paylaşılan Görüntü'nün kullanılması çeşitli avantajlar sağlayabilir:

* **Bölgeler arasında aynı görüntüleri kullanın.** Tüm havuzlarıaynı görüntüyü kullanmak böylece farklı bölgelerde Paylaşılan Görüntü yinelemeler oluşturabilirsiniz.
* **İşletim sistemini (OS) yapılandırın.** Görüntünün işletim sistemi diskinin yapılandırmasını özelleştirebilirsiniz.
* **Uygulamaları önceden yükleyin.** İşletim sistemi diskine önceden yükleme uygulamaları, başlangıç göreviyle işlem düğümlerini sağdıktan sonra uygulamaları yüklemekten daha verimli ve daha az hataya açıktır.
* **Büyük miktarda veriyi bir kez kopyalayın.** Yönetilen görüntünün veri disklerine kopyalayarak statik verileri yönetilen Paylaşılan Görüntü'nün bir parçası haline getirin. Bunun yalnızca bir kez yapılması gerekir ve verileri havuzun her düğümü için kullanılabilir hale getirir.
* **Havuzları daha büyük boyutlara büyütün.** Paylaşılan Resim Galerisi ile, daha fazla Paylaşılan Resim yinelemesi ile birlikte özelleştirilmiş görüntüleriile daha büyük havuzlar oluşturabilirsiniz.
* **Özel görüntüden daha iyi performans.** Paylaşılan Görüntüler'i kullanarak, havuzun sabit duruma ulaşması için gereken süre %25'e kadar daha hızlıdır ve VM boşta kalma süresi %30'a kadar daha kısadır.
* **Daha kolay yönetim için görüntü sürümü ve gruplandırma.** Görüntü gruplandırma tanımı, görüntünün neden oluşturulduğu, işletim sistemi nin ne için olduğu ve görüntünün kullanımı hakkında bilgiler içerir. Görüntüleri gruplandırmak daha kolay görüntü yönetimi sağlar. Daha fazla bilgi için [Resim tanımlarına](../virtual-machines/windows/shared-image-galleries.md#image-definitions)bakın.

## <a name="prerequisites"></a>Ön koşullar

* **Bir Azure Batch hesabı.** Toplu Iş hesabı oluşturmak için Azure [portalını](quick-create-portal.md) veya [Azure CLI'yi](quick-create-cli.md)kullanarak Toplu İşlem'e hızlı başlayanlar'a bakın.

* **Paylaşılan Resim Galerisi resmi.** Paylaşılan Resim oluşturmak için yönetilen bir resim kaynağına sahip olmanız veya oluşturmanız gerekir. Görüntü, VM'nin işletim sistemi diskinin anlık görüntülerinden ve isteğe bağlı olarak bağlı veri disklerinden oluşturulmalıdır. Daha fazla bilgi için [bkz.](#prepare-a-managed-image)

> [!NOTE]
> Paylaşılan Resminiz Toplu İşlem hesabıyla aynı abonelikte olmalıdır. Paylaşılan Resminiz, Toplu İş hesabınızla aynı bölgede yinelemeleri olduğu sürece farklı bölgelerde olabilir.

## <a name="prepare-a-managed-image"></a>Yönetilen bir görüntü hazırlama

Azure'da yönetilen bir resim hazırlayabilirsiniz:

* Azure VM'nin işletim sistemi ve veri disklerinin anlık görüntüleri
* Yönetilen disklere sahip genelleştirilmiş bir Azure VM
* Buluta yüklenen genelleştirilmiş şirket içi VHD

Toplu toplu işleri özel bir görüntüyle güvenilir bir şekilde ölçeklendirmek için, *yalnızca* ilk yöntemi kullanarak yönetilen bir görüntü oluşturmanızı öneririz: VM'nin disklerinin anlık görüntülerini kullanarak. VM hazırlamak, anlık görüntü almak ve anlık görüntüden bir görüntü oluşturmak için aşağıdaki adımları izleyin.

### <a name="prepare-a-vm"></a>VM hazırlama

Görüntü için yeni bir VM oluşturuyorsanız, yönetilen görüntünüz için temel görüntü olarak Toplu İşlem tarafından desteklenen birinci taraf bir Azure Marketi görüntüsünü kullanın. Yalnızca birinci taraf görüntüleri temel görüntü olarak kullanılabilir. Azure Toplu İş bölümü tarafından desteklenen Azure Marketi resim başvurularının tam listesini almak için [Liste düğümü aracısı SKUs](/java/api/com.microsoft.azure.batch.protocol.accounts.listnodeagentskus) işlemine bakın.

> [!NOTE]
> Temel resminiz olarak ek lisans ve satın alma koşulları olan bir üçüncü taraf resmi kullanamazsınız. Bu Market görüntüleri hakkında daha fazla bilgi için [Linux](../virtual-machines/linux/cli-ps-findimage.md#deploy-an-image-with-marketplace-terms
) veya [Windows](../virtual-machines/windows/cli-ps-findimage.md#deploy-an-image-with-marketplace-terms
) VM'leri kılavuzuna bakın.

* VM'nin yönetilen bir diskle oluşturulduğundan emin olun. Bu, bir VM oluşturduğunuzda varsayılan depolama ayarıdır.
* Özel Komut Dosyası uzantısı gibi Azure uzantılarını VM'ye yüklemeyin. Görüntü önceden yüklenmiş bir uzantı içeriyorsa, Azure Toplu İşlem havuzunu dağıtırken sorunlarla karşılaşabilir.
* Ekli veri disklerini kullanırken, diskleri kullanmak için bir VM'nin içinden monte edip biçimlendirmeniz gerekir.
* Sağladığınız temel işletim sistemi görüntüsünün varsayılan geçici sürücüyü kullandığından emin olun. Toplu Iş düğümü aracısı şu anda varsayılan geçici sürücüyü bekliyor.
* VM çalışmaya başladıktan sonra RDP (Windows için) veya SSH (Linux için) üzerinden bağlanın. Gerekli herhangi bir yazılımı yükleyin veya istenen verileri kopyalayın.  

### <a name="create-a-vm-snapshot"></a>VM anlık görüntüsü oluşturma

Anlık görüntü, bir VHD'nin tam, salt okunur kopyasıdır. VM'nin işletim sistemi veya veri disklerinin anlık görüntüsünü oluşturmak için Azure portalını veya komut satırı araçlarını kullanabilirsiniz. Anlık görüntü oluşturmak için adımlar ve seçenekler için [Linux](../virtual-machines/linux/snapshot-copy-managed-disk.md) veya [Windows](../virtual-machines/windows/snapshot-copy-managed-disk.md) VM'ler kılavuzuna bakın.

### <a name="create-an-image-from-one-or-more-snapshots"></a>Bir veya daha fazla anlık görüntüden görüntü oluşturma

Anlık görüntüden yönetilen bir görüntü oluşturmak [için, az image create](/cli/azure/image) komutu gibi Azure komut satırı araçlarını kullanın. Bir işletim sistemi disk anlık görüntüsü ve isteğe bağlı olarak bir veya daha fazla veri diski anlık belirterek bir görüntü oluşturun.

### <a name="create-a-shared-image-gallery"></a>Paylaşılan Resim Galerisi Oluşturma

Yönetilen resminizi başarıyla oluşturduktan sonra, özel resminizi kullanılabilir hale getirmek için paylaşılan resim galerisi oluşturmanız gerekir. Resimleriniz için Paylaşılan Resim Galerisi'ni nasıl oluşturabilirsiniz öğrenmek için Azure [CLI ile Paylaşılan Resim Galerisi Oluştur'a](../virtual-machines/linux/shared-images.md) veya [Azure portalını kullanarak Paylaşılan Resim Galerisi Oluştur'a](../virtual-machines/linux/shared-images-portal.md)bakın.

## <a name="create-a-pool-from-a-shared-image-using-the-azure-cli"></a>Azure CLI'yi kullanarak Paylaşılan Görüntü'den havuz oluşturma

Azure CLI'yi kullanarak Paylaşılan Görüntünüzden bir `az batch pool create` havuz oluşturmak için komutu kullanın. Alandaki Paylaşılan Görüntü `--image` Kimliğini belirtin. Os türü ve SKU tarafından belirtilen sürümleri eşleşen emin olun`--node-agent-sku-id`

```azurecli
az batch pool create \
    --id mypool --vm-size Standard_A1_v2 \
    --target-dedicated-nodes 2 \
    --image "/subscriptions/{sub id}/resourceGroups/{resource group name}/providers/Microsoft.Compute/galleries/{gallery name}/images/{image definition name}/versions/{version id}" \
    --node-agent-sku-id "batch.node.ubuntu 16.04"
```

## <a name="create-a-pool-from-a-shared-image-using-c"></a>C kullanarak Paylaşılan Görüntü'den havuz oluşturma #

Alternatif olarak, C# SDK'yı kullanarak Paylaşılan Görüntü'den bir havuz oluşturabilirsiniz.

```csharp
private static VirtualMachineConfiguration CreateVirtualMachineConfiguration(ImageReference imageReference)
{
    return new VirtualMachineConfiguration(
        imageReference: imageReference,
        nodeAgentSkuId: "batch.node.windows amd64");
}

private static ImageReference CreateImageReference()
{
    return new ImageReference(
        virtualMachineImageId: "/subscriptions/{sub id}/resourceGroups/{resource group name}/providers/Microsoft.Compute/galleries/{gallery name}/images/{image definition name}/versions/{version id}");
}

private static void CreateBatchPool(BatchClient batchClient, VirtualMachineConfiguration vmConfiguration)
{
    try
    {
        CloudPool pool = batchClient.PoolOperations.CreatePool(
            poolId: PoolId,
            targetDedicatedComputeNodes: PoolNodeCount,
            virtualMachineSize: PoolVMSize,
            virtualMachineConfiguration: vmConfiguration);

        pool.Commit();
    }
    ...
}
```

## <a name="create-a-pool-from-a-shared-image-using-the-azure-portal"></a>Azure portalını kullanarak Paylaşılan Görüntü'den havuz oluşturma

Azure portalında Paylaşılan Resim'den bir havuz oluşturmak için aşağıdaki adımları kullanın.

1. Azure [portalını](https://portal.azure.com)açın.
1. Toplu **İşlem hesaplarına** gidin ve hesabınızı seçin.
1. Yeni bir havuz oluşturmak için **Havuzlar'ı** ve ardından **Ekle'yi** seçin.
1. Resim **Türü** bölümünde **Paylaşılan Resim Galerisi'ni**seçin.
1. Yönetilen görüntünüz hakkında bilgi içeren kalan bölümleri tamamlayın.
1. **Tamam'ı**seçin.

![Portalla paylaşılan bir görüntüden bir havuz oluşturun.](media/batch-sig-images/create-custom-pool.png)

## <a name="considerations-for-large-pools"></a>Büyük havuzlar için dikkat edilecek hususlar

Paylaşılan Resim kullanarak yüzlerce veya binlerce VM veya daha fazla vm içeren bir havuz oluşturmayı planlıyorsanız, aşağıdaki kılavuzu kullanın.

* **Paylaşılan Resim Galerisi çoğaltma numaraları.**  600'e kadar örneği olan her havuz için en az bir yineleme bulundurmanızı öneririz. Örneğin, 3000 VM'li bir havuz oluşturuyorsanız, görüntünüzün en az 5 kopyasını saklamanız gerekir. Biz her zaman daha iyi performans için minimum gereksinimleri daha fazla çoğaltma tutmak öneririz.

* **Zaman aralarını yeniden boyutlandırın.** Havuzunuzsabit sayıda düğüm içeriyorsa (otomatik ölçeklendirme yapmıyorsa), `resizeTimeout` havuz boyutuna bağlı olarak havuzun özelliğini artırın. Her 1000 VM için önerilen yeniden boyutlandırma zaman en az 15 dakikadır. Örneğin, 2000 VM'li bir havuz için önerilen yeniden boyutlandırma zaman anına en az 30 dakika dır.

## <a name="next-steps"></a>Sonraki adımlar

* Toplu İşleme'ye derinlemesine bir bakış için [bkz.](batch-api-basics.md)
