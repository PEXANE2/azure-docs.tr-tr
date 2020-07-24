---
title: Azure HPC önbellek depolama hedeflerini güncelleştirme
description: Azure HPC önbellek depolama hedeflerini düzenleme
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 07/02/2020
ms.author: v-erkel
ms.openlocfilehash: f11e12c4f30977514e04b09c7e1c3012eb7888a7
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87092465"
---
# <a name="edit-storage-targets"></a>Depolama hedeflerini düzenleme

Bir depolama hedefini önbelleğin **depolama hedefleri** portalı sayfasından veya Azure CLI kullanarak kaldırabilir veya değiştirebilirsiniz.

> [!TIP]
> [Azure HPC önbelleğini yönetme videosu](https://azure.microsoft.com/resources/videos/managing-hpc-cache/) , Azure Portal bir depolama hedefinin nasıl düzenleneceğini gösterir.

## <a name="remove-a-storage-target"></a>Depolama hedefini kaldırma

### <a name="portal"></a>[Portal](#tab/azure-portal)

Bir depolama hedefini kaldırmak için listeden seçin ve **Sil** düğmesine tıklayın.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[!INCLUDE [cli-reminder.md](includes/cli-reminder.md)]

Önbellekten bir depolama hedefini silmek için [az HPC-Cache Storage-Target Remove](/cli/azure/ext/hpc-cache/hpc-cache/storage-target#ext-hpc-cache-az-hpc-cache-storage-target-remove) kullanın.

```azurecli
$ az hpc-cache storage-target remove --resource-group cache-rg --cache-name doc-cache0629 --name blob1

{- Finished ..
  "endTime": "2020-07-09T21:45:06.1631571+00:00",
  "name": "2f95eac1-aded-4860-b19c-3f089531a7ec",
  "startTime": "2020-07-09T21:43:38.5461495+00:00",
  "status": "Succeeded"
}
```

---

Bu eylem, bu Azure HPC önbellek sistemiyle depolama hedefi ilişkilendirmesini kaldırır, ancak arka uç depolama sistemini değiştirmez. Örneğin, bir Azure Blob depolama kapsayıcısı kullandıysanız, kapsayıcı ve içeriği önbellekten silindikten sonra hala mevcut olur. Kapsayıcıyı farklı bir Azure HPC önbelleğine ekleyebilir, bu önbelleğe yeniden ekleyebilir veya Azure portal ile silebilirsiniz.

Önbellekte depolanan tüm dosya değişiklikleri, depolama hedefi kaldırılmadan önce arka uç depolama sistemine yazılır. Bu işlem bir saat veya daha fazla değişiklik zaman alabilir ve çok sayıda değiştirilen veri önbellekte bulunur.

## <a name="update-storage-targets"></a>Depolama hedeflerini güncelleştirme

Bazı özelliklerini değiştirmek için depolama hedeflerini düzenleyebilirsiniz. Farklı depolama türleri için farklı özellikler düzenlenebilir:

* BLOB depolama hedefleri için ad alanı yolunu değiştirebilirsiniz.

* NFS depolama hedefleri için şu özellikleri değiştirebilirsiniz:

  * Ad alanı yolu
  * Kullanım modeli
  * Dışarı Aktarma
  * Alt dizini dışarı aktar

Depolama hedefinin adını, türünü veya arka uç depolama sistemini (blob kapsayıcısı veya NFS ana bilgisayar adı/IP adresi) düzenleyemezsiniz. Bu özellikleri değiştirmeniz gerekiyorsa, depolama hedefini silin ve yeni değerle bir değiştirme oluşturun.

Azure portal, depolama hedefi adına tıklayıp ayrıntılar sayfasını açarak hangi alanların düzenlenebilir olduğunu görebilirsiniz. Azure CLı ile depolama hedeflerini de değiştirebilirsiniz.

![NFS depolama hedefi için düzenleme sayfasının ekran görüntüsü](media/hpc-cache-edit-storage-nfs.png)

## <a name="update-an-nfs-storage-target"></a>NFS depolama hedefini güncelleştirme

NFS depolama hedefi için, birkaç özelliği güncelleştirebilirsiniz. (Örnek düzenleme sayfası için yukarıdaki ekran görüntüsüne bakın.)

* **Kullanım modeli** -kullanım modeli, önbelleğin verileri nasıl koruduğunu etkiler. Daha fazla bilgi edinmek için [bir kullanım modeli seçin](hpc-cache-add-storage.md#choose-a-usage-model) .
* **Sanal ad alanı yolu** -istemcilerin bu depolama hedefini bağlamak için kullandığı yol. Ayrıntılar için [toplanan ad alanı planını](hpc-cache-namespace.md) okuyun.
* **NFS dışa aktarma yolu** -bu ad alanı yolu için kullanılacak depolama sistemi dışa aktarma.
* **Alt dizin yolu** -bu ad alanı yoluyla ilişkilendirilecek alt dizin (dışarı aktarma altında). Bir alt dizin belirtmeniz gerekmiyorsa bu alanı boş bırakın.

Her ad alanı yolu, bir dışarı aktarma ve alt dizinin benzersiz bir birleşimini gerektirir. Diğer bir deyişle, arka uç depolama sisteminde tam olarak aynı dizine sahip iki farklı istemciye yönelik yol yapamazsınız.

### <a name="portal"></a>[Portal](#tab/azure-portal)

Değişiklik yaptıktan sonra, depolama hedefini güncelleştirmek için **Tamam** ' ı tıklatın veya değişiklikleri atmak için **iptal** ' i tıklatın.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[!INCLUDE [cli-reminder.md](includes/cli-reminder.md)]

Bir depolama hedefi için kullanım modelini, sanal ad alanı yolunu ve NFS dışarı aktarma veya alt dizin değerlerini değiştirmek için [az NFS-Storage-Target](/cli/azure/ext/hpc-cache/hpc-cache/nfs-storage-target) komutunu kullanın.

* Kullanım modelini değiştirmek için ``--nfs3-usage-model`` seçeneğini kullanın. Örnek: ``--nfs3-usage-model WRITE_WORKLOAD_15``

* Ad alanı yolunu, dışarı aktarmayı veya dışa aktarma alt dizinini değiştirmek için ``--junction`` seçeneğini kullanın.

  ``--junction``Parametresi şu değerleri kullanır:

  * ``namespace-path``-İstemciye yönelik sanal dosya yolu
  * ``nfs-export``-İstemciye yönelik yol ile ilişkilendirilecek depolama sistemi dışa aktarma
  * ``target-path``(isteğe bağlı)-gerekirse, dışarı aktarmanın alt dizini

  Örnek: ``--junction namespace-path="/nas-1" nfs-export="/datadisk1" target-path="/test"``

Tüm güncelleştirme komutlarında önbellek adı, depolama hedefi adı ve kaynak grubu gereklidir.

Komut örneği: <!-- having problem testing this -->

```azurecli
az hpc-cache nfs-storage-target update --cache-name mycache \
    --name rivernfs0 --resource-group doc-rg0619 \
    --nfs3-usage-model READ_HEAVY_INFREQ
```

Önbellek durdurulmuşsa veya sağlıklı durumda değilse, güncelleştirme önbelleğin sağlıklı olduktan sonra geçerli olur.

---

## <a name="update-an-azure-blob-storage-target"></a>Azure Blob depolama hedefini güncelleştirme

BLOB depolama hedefi için, sanal ad alanı yolunu değiştirebilirsiniz.

### <a name="portal"></a>[Portal](#tab/azure-portal)

BLOB depolama hedefinin Ayrıntılar sayfası, sanal ad alanı yolunu değiştirmenize olanak sağlar.

![BLOB depolama hedefi için düzenleme sayfasının ekran görüntüsü](media/hpc-cache-edit-storage-blob.png)

İşiniz bittiğinde, depolama hedefini güncelleştirmek için **Tamam** ' ı tıklatın veya değişiklikleri atmak için **iptal** ' i tıklatın.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[!INCLUDE [cli-reminder.md](includes/cli-reminder.md)]

Bir hedefin ad alanı yolunu güncelleştirmek için [az HPC-Cache blob-Storage-Target Update](/cli/azure/ext/hpc-cache/hpc-cache/blob-storage-target#ext-hpc-cache-az-hpc-cache-blob-storage-target-update) kullanın.

```azurecli
az hpc-cache blob-storage-target update --cache-name cache-name --name target-name \
    --resource-group rg --storage-account "/subscriptions/<subscription_ID>/resourceGroups/erinazcli/providers/Microsoft.Storage/storageAccounts/rg"  \
    --container-name "container-name" --virtual-namespace-path "/new-path"
```

Önbellek durdurulmuşsa veya sağlıklı durumda değilse güncelleştirme, önbellek sağlıklı olduktan sonra uygulanır.

---

## <a name="next-steps"></a>Sonraki adımlar

* Bu seçenekler hakkında daha fazla bilgi edinmek için [depolama hedefleri ekleme](hpc-cache-add-storage.md) bölümünü okuyun.
* Sanal yolları kullanma hakkında daha fazla ipucu için [toplanan ad alanı planını](hpc-cache-namespace.md) okuyun.
