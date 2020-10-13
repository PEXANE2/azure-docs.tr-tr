---
title: Azure HPC önbellek depolama hedeflerini güncelleştirme
description: Azure HPC önbellek depolama hedeflerini düzenleme
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 09/30/2020
ms.author: v-erkel
ms.openlocfilehash: 70f350204796099e02f7afe829a6e2e1fdf653c8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91613130"
---
# <a name="edit-storage-targets"></a>Depolama hedeflerini düzenleme

Azure portal veya Azure CLı kullanarak depolama hedeflerini kaldırabilir veya değiştirebilirsiniz.

Depolama türüne bağlı olarak, şu depolama hedefi değerlerini değiştirebilirsiniz:

* BLOB depolama hedefleri için ad alanı yolunu değiştirebilirsiniz.

* NFS depolama hedefleri için şu değerleri değiştirebilirsiniz:

  * Ad alanı yolları
  * Bir ad alanı yoluyla ilişkili depolama dışarı veya dışa aktarma alt dizini
  * Kullanım modeli

Depolama hedefinin adını, türünü veya arka uç depolama sistemini (blob kapsayıcısı veya NFS ana bilgisayar adı/IP adresi) düzenleyemezsiniz. Bu özellikleri değiştirmeniz gerekiyorsa, depolama hedefini silin ve yeni değerle bir değiştirme oluşturun.

> [!TIP]
> [Azure HPC önbelleğini yönetme videosu](https://azure.microsoft.com/resources/videos/managing-hpc-cache/) , Azure Portal bir depolama hedefinin nasıl düzenleneceğini gösterir.

## <a name="remove-a-storage-target"></a>Depolama hedefini kaldırma

### <a name="portal"></a>[Portal](#tab/azure-portal)

Bir depolama hedefini kaldırmak için **depolama hedefleri** sayfasını açın. Listeden depolama hedefini seçin ve **Sil** düğmesine tıklayın.

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

Depolama hedefini silme, depolama sisteminin bu Azure HPC önbellek sistemiyle ilişkilendirmesini kaldırır, ancak arka uç depolama sistemini değiştirmez. Örneğin, bir Azure Blob depolama kapsayıcısı kullandıysanız, kapsayıcı ve içeriği önbellekten silindikten sonra hala mevcut olur. Kapsayıcıyı farklı bir Azure HPC önbelleğine ekleyebilir, bu önbelleğe yeniden ekleyebilir veya Azure portal ile silebilirsiniz.

Önbellekte depolanan tüm dosya değişiklikleri, depolama hedefi kaldırılmadan önce arka uç depolama sistemine yazılır. Bu işlem bir saat veya daha fazla değişiklik zaman alabilir ve çok sayıda değiştirilen veri önbellekte bulunur.

## <a name="change-a-blob-storage-targets-namespace-path"></a>BLOB depolama hedefinin ad alanı yolunu değiştirme

Ad alanı yolları, istemcilerin bu depolama hedefini bağlamak için kullandığı yollardır. (Daha fazla bilgi edinmek için [toplanan ad alanını planlayın](hpc-cache-namespace.md) ve [toplanmış ad alanını ayarlayın](add-namespace-paths.md)).

Ad alanı yolu, Azure Blob depolama hedefi üzerinde yapabileceğiniz tek güncelleştirmedir. Bunu değiştirmek için Azure portal veya Azure CLı 'yi kullanın.

### <a name="portal"></a>[Portal](#tab/azure-portal)

Azure HPC önbelleğiniz için **ad alanı** sayfasını kullanın. Ad alanı sayfası, [toplanan ad alanını ayarlama](add-namespace-paths.md)makalesinde daha ayrıntılı olarak açıklanmıştır.

Değiştirmek istediğiniz yolun adına tıklayın ve görüntülenen düzenleme penceresinde yeni yolu oluşturun.

![Blob ad alanı yoluna tıkladıktan sonra ad alanı sayfasının ekran görüntüsü-düzenleme alanları sağdaki bir bölmede görüntülenir](media/edit-namespace-blob.png)

Değişiklik yaptıktan sonra, depolama hedefini güncelleştirmek için **Tamam** ' ı tıklatın veya değişiklikleri atmak için **iptal** ' i tıklatın.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[!INCLUDE [cli-reminder.md](includes/cli-reminder.md)]

Azure CLı ile bir BLOB depolama hedefinin ad alanını değiştirmek için [az HPC-Cache blob-Storage-Target Update](/cli/azure/ext/hpc-cache/hpc-cache/blob-storage-target#ext-hpc-cache-az-hpc-cache-blob-storage-target-update)komutunu kullanın. Yalnızca `--virtual-namespace-path` değer değiştirilebilir.

  ```azurecli
  az hpc-cache blob-storage-target update --cache-name cache-name --name target-name \
    --resource-group rg --virtual-namespace-path "/new-path"
  ```

---

## <a name="update-an-nfs-storage-target"></a>NFS depolama hedefini güncelleştirme

NFS depolama hedefleri için, sanal ad alanı yollarını değiştirebilir veya ekleyebilir, bir ad alanı yolunun işaret ettiği NFS dışarı aktarma veya alt dizin değerlerini değiştirebilir ve kullanım modelini değiştirebilirsiniz.

Ayrıntılar aşağıda verilmiştir:

* [Toplanmış ad alanı değerlerini Değiştir](#change-aggregated-namespace-values) (sanal ad alanı yolu, dışarı aktarma ve dışarı aktarma alt dizini)
* [Kullanım modelini değiştirme](#change-the-usage-model)

### <a name="change-aggregated-namespace-values"></a>Toplanmış ad alanı değerlerini Değiştir

İstemciye yönelik ad alanı yolunu, depolama dışarı aktarmayı ve dışa aktarma alt dizinini (kullanılıyorsa) değiştirmek için Azure portal veya Azure CLı kullanabilirsiniz.

Tek bir depolama hedefinde birden çok geçerli yol oluşturma hakkında bir anımsatıcı gerekiyorsa, [NFS ad alanı yolları Ekle](add-namespace-paths.md#nfs-namespace-paths) bölümündeki yönergeleri okuyun.

### <a name="portal"></a>[Portal](#tab/azure-portal)

Ad alanı değerlerini güncelleştirmek için Azure HPC önbelleğiniz için **ad alanı** sayfasını kullanın. Bu sayfa, [toplanan ad alanını ayarlama](add-namespace-paths.md)makalesinde daha ayrıntılı olarak açıklanmıştır.

![NFS güncelleştirme sayfası sağ tarafta açık olan Portal ad alanı sayfasının ekran görüntüsü](media/update-namespace-nfs.png)

1. Değiştirmek istediğiniz yolun adına tıklayın.
1. Yeni sanal yol, dışarı aktarma veya alt dizin değerlerini yazmak için düzenleme penceresini kullanın.
1. Değişiklik yaptıktan sonra, depolama hedefini güncelleştirmek için **Tamam** ' a veya değişiklikleri atmak için **iptal** ' e tıklayın.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[!INCLUDE [cli-reminder.md](includes/cli-reminder.md)]

``--junction``Ad alanı yolunu, NFS dışarı aktarmayı veya dışarı aktarma alt dizinini değiştirmek için [az HPC-Cache NFS-Storage-Target Update](/cli/azure/ext/hpc-cache/hpc-cache/nfs-storage-target) komutundaki seçeneğini kullanın.

``--junction``Parametresi şu değerleri kullanır:

* ``namespace-path`` -İstemciye yönelik sanal dosya yolu
* ``nfs-export`` -İstemciye yönelik yol ile ilişkilendirilecek depolama sistemi dışa aktarma
* ``target-path`` (isteğe bağlı)-gerekirse, dışarı aktarmanın alt dizini

Örnek: ``--junction namespace-path="/nas-1" nfs-export="/datadisk1" target-path="/test"``

Deyimdeki her yol için üç değeri sağlamanız gerekir ``--junction`` . Değiştirmek istemediğiniz değerler için mevcut değerleri kullanın.

Tüm güncelleştirme komutlarında önbellek adı, depolama hedefi adı ve kaynak grubu da gereklidir.

Örnek komut:

```azurecli
az hpc-cache nfs-storage-target update --cache-name mycache \
  --name st-name --resource-group doc-rg0619 \
  --junction namespace-path="/new-path" nfs-export="/my-export" target-path="my-subdirectory"
```

---

### <a name="change-the-usage-model"></a>Kullanım modelini değiştirme

Kullanım modeli, önbelleğin verileri nasıl koruduğunu etkiler. Daha fazla bilgi edinmek için [bir kullanım modeli seçin](hpc-cache-add-storage.md#choose-a-usage-model) .

Bir NFS depolama hedefinin kullanım modelini değiştirmek için, aşağıdaki yöntemlerden birini kullanın.

### <a name="portal"></a>[Portal](#tab/azure-portal)

Kullanım modelini Azure portal **depolama hedefleri** sayfasından değiştirin. Değiştirilecek depolama hedefinin adına tıklayın.

![NFS depolama hedefi için düzenleme sayfasının ekran görüntüsü](media/edit-storage-nfs.png)

Yeni bir kullanım modeli seçmek için açılan seçiciyi kullanın. Depolama hedefini güncelleştirmek için **Tamam** ' ı tıklatın veya değişiklikleri atmak için **iptal** ' i tıklatın.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[!INCLUDE [cli-reminder.md](includes/cli-reminder.md)]

[Az HPC-Cache NFS-Storage-Target Update](/cli/azure/ext/hpc-cache/hpc-cache/nfs-storage-target?view=azure-cli-latest#ext-hpc-cache-az-hpc-cache-nfs-storage-target-update) komutunu kullanın.

Güncelleştirme komutu, NFS depolama hedefi eklemek için kullandığınız komutla neredeyse aynıdır. Ayrıntılar ve örnekler için [NFS depolama hedefi oluşturma](hpc-cache-add-storage.md#create-an-nfs-storage-target) bölümüne bakın.

Kullanım modelini değiştirmek için, ``--nfs3-usage-model`` seçeneğini güncelleştirin. Örnek: ``--nfs3-usage-model WRITE_WORKLOAD_15``

Önbellek adı, depolama hedefi adı ve kaynak grubu değerleri de gereklidir.

Kullanım modellerinin adlarını doğrulamak istiyorsanız, [az HPC-Cache Usage-model List](/cli/azure/ext/hpc-cache/hpc-cache/usage-model#ext-hpc-cache-az-hpc-cache-usage-model-list)komutunu kullanın.

Önbellek durdurulmuşsa veya sağlıklı durumda değilse güncelleştirme, önbellek sağlıklı olduktan sonra uygulanır.

---

## <a name="next-steps"></a>Sonraki adımlar

* Bu seçenekler hakkında daha fazla bilgi edinmek için [depolama hedefleri ekleme](hpc-cache-add-storage.md) bölümünü okuyun.
* Sanal yolları kullanma hakkında daha fazla ipucu için [toplanan ad alanı planını](hpc-cache-namespace.md) okuyun.
