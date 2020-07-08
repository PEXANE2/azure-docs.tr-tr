---
title: Azure HPC önbellek depolama hedeflerini güncelleştirme
description: Azure HPC önbellek depolama hedeflerini düzenleme
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 1/08/2020
ms.author: v-erkel
ms.openlocfilehash: 0fa8be58db9754c652d6e1ee5349c950a1c19109
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85513836"
---
# <a name="edit-storage-targets"></a>Depolama hedeflerini düzenleme

Bir depolama hedefini önbelleğin **depolama hedefleri** sayfasından kaldırabilir veya değiştirebilirsiniz.

> [!TIP]
> [Azure HPC önbelleğini yönetme videosu](https://azure.microsoft.com/resources/videos/managing-hpc-cache/) , bir depolama hedefinin nasıl düzenleneceğini gösterir.

## <a name="remove-a-storage-target"></a>Depolama hedefini kaldırma

Bir depolama hedefini kaldırmak için listeden seçin ve **Sil** düğmesine tıklayın.

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

Bir depolama hedefini değiştirmek için depolama hedefi adına tıklayarak Ayrıntılar sayfasını açın. Sayfadaki bazı alanlar düzenlenebilir.

![NFS depolama hedefi için düzenleme sayfasının ekran görüntüsü](media/hpc-cache-edit-storage-nfs.png)

## <a name="update-an-nfs-storage-target"></a>NFS depolama hedefini güncelleştirme

NFS depolama hedefi için, birkaç özelliği güncelleştirebilirsiniz. (Örnek düzenleme sayfası için yukarıdaki ekran görüntüsüne bakın.)

* **Kullanım modeli** -kullanım modeli, önbelleğin verileri nasıl koruduğunu etkiler. Daha fazla bilgi edinmek için [bir kullanım modeli seçin](hpc-cache-add-storage.md#choose-a-usage-model) .
* **Sanal ad alanı yolu** -istemcilerin bu depolama hedefini bağlamak için kullandığı yol. Ayrıntılar için [toplanan ad alanı planını](hpc-cache-namespace.md) okuyun.
* **NFS dışa aktarma yolu** -bu ad alanı yolu için kullanılacak depolama sistemi dışa aktarma.
* **Alt dizin yolu** -bu ad alanı yoluyla ilişkilendirilecek alt dizin (dışarı aktarma altında). Bir alt dizin belirtmeniz gerekmiyorsa bu alanı boş bırakın.

Her ad alanı yolu, bir dışarı aktarma ve alt dizinin benzersiz bir birleşimini gerektirir. Diğer bir deyişle, arka uç depolama sisteminde tam olarak aynı dizine sahip iki farklı istemciye yönelik yol yapamazsınız.

Değişiklik yaptıktan sonra, depolama hedefini güncelleştirmek için **Tamam** ' ı tıklatın veya değişiklikleri atmak için **iptal** ' i tıklatın.

## <a name="update-an-azure-blob-storage-target"></a>Azure Blob depolama hedefini güncelleştirme

BLOB depolama hedefinin Ayrıntılar sayfası, sanal ad alanı yolunu değiştirmenize olanak sağlar.

![BLOB depolama hedefi için düzenleme sayfasının ekran görüntüsü](media/hpc-cache-edit-storage-blob.png)

İşiniz bittiğinde, depolama hedefini güncelleştirmek için **Tamam** ' ı tıklatın veya değişiklikleri atmak için **iptal** ' i tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

* Bu seçenekler hakkında daha fazla bilgi edinmek için [depolama hedefleri ekleme](hpc-cache-add-storage.md) bölümünü okuyun.
* Sanal yolları kullanma hakkında daha fazla ipucu için [toplanan ad alanı planını](hpc-cache-namespace.md) okuyun.
