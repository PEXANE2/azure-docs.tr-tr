---
title: Havuz oluşturmak için paylaşılan görüntü galerisini kullanın-Azure Batch | Microsoft Docs
description: Uygulamanız için ihtiyaç duyduğunuz yazılımı ve verileri içeren işlem düğümlerine özel görüntüler sağlamak için paylaşılan görüntü Galerisi ile bir Batch havuzu oluşturun. Özel görüntüler, işlem düğümlerini toplu iş yüklerinizi çalıştıracak şekilde yapılandırmanın etkili bir yoludur.
services: batch
author: laurenhughes
manager: gwallace
ms.service: batch
ms.topic: article
ms.date: 08/14/2019
ms.author: lahugh
ms.openlocfilehash: 00da17512cbc2e713955ea83c7d9fa7517958169
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69036698"
---
# <a name="use-the-shared-image-gallery-to-create-a-pool"></a>Bir havuz oluşturmak için paylaşılan görüntü galerisini kullanma

Sanal makine yapılandırmasını kullanarak bir Azure Batch havuzu oluşturduğunuzda, havuzdaki her bir işlem düğümü için işletim sistemini sağlayan bir VM görüntüsü belirtirsiniz. Desteklenen bir Azure Marketi görüntüsüne sahip bir sanal makine havuzu oluşturabilir veya [paylaşılan görüntü Galerisi](../virtual-machines/windows/shared-image-galleries.md)ile özel bir görüntü oluşturabilirsiniz.

## <a name="benefits-of-the-shared-image-gallery"></a>Paylaşılan görüntü galerisinin avantajları

Özel görüntünüz için paylaşılan görüntü galerisini kullandığınızda, işletim sistemi türü ve yapılandırmasının yanı sıra veri disklerinin türü üzerinde denetiminiz vardır. Paylaşılan görüntünüz, tüm toplu havuz düğümlerinde kullanılabilir hale gelen uygulamaları ve başvuru verilerini ve bunların sağlandığı anda bu verileri içerebilir.

Ortamınız için gerektiğinde bir görüntünün birden fazla sürümüne de sahip olabilirsiniz. Bir sanal makine oluşturmak için görüntü sürümü kullandığınızda, sanal makine için yeni diskler oluşturmak üzere görüntü sürümü kullanılır. 

Paylaşılan bir görüntü kullanmak, havuzunuzun işlem düğümlerini toplu iş yükünüzü çalıştırmak için hazırlarken zaman kazandırır. Bir Azure Market görüntüsü kullanmak ve sağlama sonrasında her işlem düğümüne yazılım yüklemek mümkündür, ancak paylaşılan bir görüntü kullanmak genellikle daha etkilidir. Ayrıca, çok sayıda VM (600 VM 'den fazla) olan havuzlar oluştururken, havuz oluşturma sırasında zaman kazanabilirsiniz. bu sayede paylaşılan görüntü için birden çok çoğaltma belirtebilirsiniz.

Senaryonuz için yapılandırılmış paylaşılan bir görüntünün kullanılması çeşitli avantajlar sağlayabilir:

* **Bölgeler genelinde aynı görüntüleri kullanın.** Farklı bölgelerde paylaşılan görüntü çoğaltmaları oluşturabilirsiniz, böylece tüm havuzlarınız aynı görüntüyü kullanır.
* **İşletim sistemini (OS) yapılandırın.** Görüntünün işletim sistemi diskinin yapılandırmasını özelleştirebilirsiniz.
* **Uygulamaları önceden yükler.** İşletim düğümleri bir başlangıç göreviyle sağlandıktan sonra uygulamaları yüklemekten daha verimli ve daha az hata, işletim sistemi diskine önceden yükleme.
* **Büyük miktarlarda veriyi bir kez kopyalayın.** Yönetilen bir görüntünün veri disklerine kopyalayarak, yönetilen paylaşılan görüntünün statik veri parçasını oluşturun. Bu yalnızca bir kez yapılmalıdır ve verileri havuzun her bir düğümü için kullanılabilir hale getirir.
* **Daha büyük boyutlarda havuzları büyütün.** Paylaşılan görüntü Galerisi ile, daha fazla paylaşılan görüntü çoğaltmalarıyla birlikte özelleştirilmiş görüntülerinize sahip daha büyük havuzlar da oluşturabilirsiniz.
* **Özel görüntüden daha iyi performans.** Paylaşılan görüntüleri kullanarak havuzun kararlı duruma ulaşması için gereken süre% 25 daha hızlı ve VM boşta kalma gecikmesi% 30 ' a kadar daha kısadır.
* **Daha kolay yönetim için görüntü sürümü oluşturma ve gruplama.** Görüntü gruplandırma tanımı, görüntünün neden oluşturulduğu, ne kadar işletim sistemi olduğu ve görüntüyü kullanma hakkında bilgi içeren bilgileri içerir. Görüntülerin gruplandırılması, daha kolay görüntü yönetimine olanak tanır. Daha fazla bilgi için bkz. [görüntü tanımları](../virtual-machines/windows/shared-image-galleries.md#image-definitions).

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure Batch hesabı.** Batch hesabı oluşturmak için, [Azure Portal](quick-create-portal.md) veya [Azure CLI](quick-create-cli.md)kullanarak Batch hızlı başlangıçlara bakın.

* **Paylaşılan görüntü Galerisi görüntüsü**. Paylaşılan bir görüntüyü hazırlama hakkında daha fazla bilgi ve adım için bkz. [Azure CLI Ile paylaşılan görüntü galerisi oluşturma](../virtual-machines/linux/shared-images.md) veya [Azure Portal kullanarak paylaşılan görüntü galerisi oluşturma](../virtual-machines/linux/shared-images-portal.md).

> [!NOTE]
> Paylaşılan görüntünüz Batch hesabıyla aynı abonelikte olmalıdır. Paylaşılan görüntünüz, Batch hesabınızla aynı bölgede çoğaltmalar olduğu sürece farklı bölgelerde olabilir.

## <a name="create-a-pool-from-a-shared-image-using-the-azure-cli"></a>Azure CLı kullanarak paylaşılan görüntüden havuz oluşturma

Azure CLI kullanarak paylaşılan görüntinizden bir havuz oluşturmak için `az batch pool create` komutunu kullanın. `--image` Alanda paylaşılan görüntü kimliğini belirtin. İşletim sistemi türü ve SKU 'sunun, tarafından belirtilen sürümlerle eşleştiğinden emin olun`--node-agent-sku-id`

```azurecli
az batch pool create \
    --id mypool --vm-size Standard_A1_v2 \
    --target-dedicated-nodes 2 \
    --image "/subscriptions/{sub id}/resourceGroups/{resource group name}/providers/Microsoft.Compute/galleries/{gallery name}/images/{image definition name}/versions/{version id}" \
    --node-agent-sku-id "batch.node.ubuntu 16.04"
```

## <a name="create-a-pool-from-a-shared-image-using-c"></a>Kullanarak paylaşılan görüntüden havuz oluşturmaC#

Alternatif olarak, C# SDK kullanarak paylaşılan görüntüden bir havuz oluşturabilirsiniz.

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

## <a name="considerations-for-large-pools"></a>Büyük havuzlara dikkat edilecek noktalar

Paylaşılan bir görüntü kullanarak yüzlerce veya binlerce VM veya daha fazlasını içeren bir havuz oluşturmayı planlıyorsanız, aşağıdaki kılavuzu kullanın.

* **Paylaşılan görüntü Galerisi çoğaltma numaraları.**  En fazla 600 örneği olan her havuz için en az bir çoğaltma tutmanız önerilir. Örneğin, 3000 VM 'Ler içeren bir havuz oluşturuyorsanız, resminizin en az 5 çoğaltmasını tutmanız gerekir. Daha iyi performans için en düşük gereksinimlerden her zaman daha fazla çoğaltma tutulması önerilir.

* **Yeniden boyutlandırma zaman aşımı** Havuzunuz sabit sayıda düğüm içeriyorsa (otomatik ölçeklendirme yoksa) havuz boyutuna bağlı olarak havuzun `resizeTimeout` özelliğini arttırın. Her 1000 VM için önerilen yeniden boyutlandırma zaman aşımı en az 15 dakikadır. Örneğin, 2000 VM içeren bir havuz için önerilen yeniden boyutlandırma zaman aşımı süresi en az 30 dakikadır.

## <a name="next-steps"></a>Sonraki adımlar

* Toplu Işe yönelik ayrıntılı genel bakış için bkz. [Batch ile büyük ölçekli paralel işlem çözümleri geliştirme](batch-api-basics.md).
