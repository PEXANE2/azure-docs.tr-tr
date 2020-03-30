---
title: Azure HPC Önbellek depolama hedeflerini güncelleştirme
description: Azure HPC Önbellek depolama hedefleri nasıl edinilir?
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 1/08/2020
ms.author: rohogue
ms.openlocfilehash: 5635bfc6ea5faea41b125037c76c0b8635e0f528
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75866998"
---
# <a name="edit-storage-targets"></a>Depolama hedeflerini düzenleme

Önbelleğin **Depolama hedefleri** sayfasından bir depolama hedefini kaldırabilir veya değiştirebilirsiniz.

## <a name="remove-a-storage-target"></a>Depolama hedefini kaldırma

Depolama hedefini kaldırmak için, listede seçin ve **Sil** düğmesini tıklatın.

Bu eylem, bu Azure HPC Önbellek sistemiyle depolama hedef ilişkisini kaldırır, ancak arka uç depolama sistemini değiştirmez. Örneğin, bir Azure Blob depolama kapsayıcısı kullandıysanız, önbellekten sildikten sonra kapsayıcı ve içeriği hala kullanılabilir. Kapsayıcıyı farklı bir Azure HPC Önbelleğine ekleyebilir, bu önbelleğe yeniden ekleyebilir veya Azure portalıyla silebilirsiniz.

Önbellekte depolanan tüm dosya değişiklikleri, depolama hedefi kaldırılmadan önce arka uç depolama sistemine yazılır. Çok fazla değiştirilen veri önbellekteyse, bu işlem bir saat veya daha uzun sürebilir.

## <a name="update-storage-targets"></a>Depolama hedeflerini güncelleştirme

Bazı özelliklerini değiştirmek için depolama hedeflerini değiştirebilirsiniz. Farklı depolama türleri için farklı özellikler değiştirilebilir:

* Blob depolama hedefleri için ad alanı yolunu değiştirebilirsiniz.

* NFS depolama hedefleri için şu özellikleri değiştirebilirsiniz:

  * Ad alanı yolu
  * Kullanım modeli
  * Dışarı Aktarma
  * Dış dizini dışdizini dışa aktarma

Depolama hedefinin adını, türünü veya arka uç depolama sistemini (Blob kapsayıcısı veya NFS ana bilgisayar/IP adresi) düzenleyemezsiniz. Bu özellikleri değiştirmeniz gerekiyorsa, depolama hedefini silin ve yeni değerle bir değiştirme oluşturun.

Depolama hedefini değiştirmek için, ayrıntılar sayfasını açmak için depolama hedef adını tıklatın. Sayfadaki bazı alanlar değiştirilebilir.

![Bir NFS depolama hedefi için edit sayfasının ekran görüntüsü](media/hpc-cache-edit-storage-nfs.png)

## <a name="update-an-nfs-storage-target"></a>NFS depolama hedefini güncelleştirme

Bir NFS depolama hedefi için birkaç özelliği güncelleştirebilirsiniz. (Örnek bir edit sayfası için yukarıdaki ekran görüntüsüne bakın.)

* **Kullanım modeli** - Kullanım modeli önbelleğin verileri nasıl saklar etkilemesini sağlar. Read Daha fazla bilgi için [bir kullanım modeli seçin.](hpc-cache-add-storage.md#choose-a-usage-model)
* **Sanal ad alanı yolu** - İstemcilerin bu depolama hedefini monte etmek için kullandıkları yol. Ayrıntılar için [toplu ad alanını planla'yı](hpc-cache-namespace.md) okuyun.
* **NFS dışa aktarma yolu** - Depolama sistemi dışa aktarma bu ad alanı yolu için kullanılır.
* **Alt dizin yolu** - Bu ad alanı yolu ile ilişkilendirmek için alt dizin (dışa aktarma altında). Bir alt dizini belirtmeniz gerekmiyorsa bu alanı boş bırakın.

Her ad alanı yolunun dışa aktarma ve alt dizinin benzersiz bir birleşimi gerekir. Diğer bir zamanda, arka uç depolama sisteminde aynı dizin için iki farklı istemci bakan yol yapamazsınız.

Değişiklik yaptıktan sonra, depolama hedefini güncelleştirmek için **Tamam'ı** tıklatın veya değişiklikleri atmak için **İptal'i** tıklatın.

## <a name="update-an-azure-blob-storage-target"></a>Azure Blob depolama hedefini güncelleştirme

Blob depolama hedefinin ayrıntılar sayfası, sanal ad alanı yolunu değiştirmenize olanak tanır.

![blob depolama hedefi için edit sayfasının ekran görüntüsü](media/hpc-cache-edit-storage-blob.png)

Tamamlandığında, depolama hedefini güncelleştirmek için **Tamam'ı** tıklatın veya değişiklikleri atmak için **İptal'i** tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

* Bu seçenekler hakkında daha fazla bilgi edinmek için [depolama hedefleri ekle'yi](hpc-cache-add-storage.md) okuyun.
* Sanal yolları kullanma yla ilgili daha fazla ipucu için [toplu ad alanını planla'yı](hpc-cache-namespace.md) okuyun.
