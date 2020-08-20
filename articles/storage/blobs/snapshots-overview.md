---
title: Blob anlık görüntüleri
titleSuffix: Azure Storage
description: Blob verilerini belirli bir anda yedeklemek için Blobun salt okunurdur bir anlık görüntü oluşturmayı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 08/19/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 4c6c2774e0d71ec33449565efab797c040aa264f
ms.sourcegitcommit: 628be49d29421a638c8a479452d78ba1c9f7c8e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88640608"
---
# <a name="blob-snapshots"></a>Blob anlık görüntüleri

Anlık görüntü, bir Blobun zaman içinde alınmış bir salt okunurdur.

> [!NOTE]
> Blob sürümü oluşturma (Önizleme), bir Blobun önceki sürümlerini korumak için alternatif bir yol sunar. Daha fazla bilgi için bkz. [BLOB sürüm oluşturma (Önizleme)](versioning-overview.md).

## <a name="about-blob-snapshots"></a>Blob anlık görüntüleri hakkında

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

Blob URI 'si, blob URI 'sinin, anlık görüntünün alındığı saati belirtmek için blob URI 'sine eklenmiş bir **Tarih saat** değeri olması dışında, kendi temel blobuna benzer. Örneğin, bir Sayfa Blobu URI 'SI ise `http://storagesample.core.blob.windows.net/mydrives/myvhd` , anlık görüntü URI 'si öğesine benzerdir `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z` .

> [!NOTE]
> Tüm anlık görüntüler, temel Blobun URI 'sini paylaşır. Temel blob ve anlık görüntü arasındaki tek ayrım, eklenen **Tarih saat** değeridir.
>

Blob herhangi bir sayıda anlık görüntüye sahip olabilir. Anlık görüntüler, bağımsız olarak ya da temel Blobun [BLOB silme](/rest/api/storageservices/delete-blob) işleminin bir parçası olarak açıkça silinene kadar kalır. Geçerli anlık görüntülerinizi izlemek için temel bloba ilişkili anlık görüntüleri numaralandırabilirsiniz.

Bir Blobun anlık görüntüsünü oluşturduğunuzda, Blobun sistem özellikleri aynı değerlerle anlık görüntüye kopyalanır. Temel Blobun meta verileri, oluşturduğunuz sırada anlık görüntü için ayrı meta veriler belirtmediğiniz müddetçe anlık görüntüye de kopyalanır. Anlık görüntü oluşturduktan sonra, bunu okuyabilir, kopyalayabilir veya silebilirsiniz, ancak değiştiremezsiniz.

Temel blob ile ilişkili tüm kiralamalar anlık görüntüyü etkilemez. Anlık görüntü üzerinde kira elde edilemez.

Bir VHD dosyası, bir VM diskinin geçerli bilgilerini ve durumunu depolamak için kullanılır. VM 'nin içinden bir diski ayırabilir veya VM 'yi kapatabilir ve sonra VHD dosyasının anlık görüntüsünü alabilirsiniz. Bu anlık görüntü dosyasını daha sonra bu noktada VHD dosyasını almak ve VM 'yi yeniden oluşturmak için kullanabilirsiniz.

## <a name="understand-how-snapshots-accrue-charges"></a>Anlık görüntülerin ücretleri nasıl tahakkuk ettir öğrenin

Bir Blobun salt okunurdur kopyası olan bir anlık görüntü oluşturmak, hesabınıza ek veri depolama ücretleri oluşmasına neden olabilir. Uygulamanızı tasarlarken, maliyetleri en aza indirmek için bu ücretlerin nasıl tahakkuk edebileceğini bilmeniz önemlidir.

### <a name="important-billing-considerations"></a>Önemli faturalandırma konuları

Aşağıdaki liste, bir anlık görüntü oluştururken dikkate alınması gereken önemli noktaları içerir.

- Depolama hesabınız, blob 'ta mi yoksa anlık görüntüde olsun, benzersiz bloklar veya sayfalar için ücretler uygular. Hesabınız, temel aldıkları blobu güncelleştirene kadar bir blob ile ilişkili anlık görüntüler için ek ücret uygulamaz. Temel blobu güncelleştirdikten sonra, anlık görüntülerinden ayrılan olur. Bu durumda, her blob veya anlık görüntüde benzersiz bloklar veya sayfalar için ücretlendirilirsiniz.
- Blok Blobu içindeki bir bloğu değiştirdiğinizde, bu blok daha sonra benzersiz bir blok olarak ücretlendirilir. Bu, blok aynı blok KIMLIĞINE ve anlık görüntüdeki verilerle aynı verilere sahip olsa da geçerlidir. Blok yeniden gerçekleştirildikten sonra, herhangi bir anlık görüntüde onun karşılığından ayrılan bir işlem olur ve veriler için ücretlendirilirsiniz. Aynı verilerle güncelleştirilmiş bir sayfa blobunun bir sayfası için de aynı değer geçerlidir.
- Blob 'un tüm içeriğinin üzerine yazan bir yöntemi çağırarak bir blok Blobun güncelleştirilmesi, Blobun tüm blokların yerini alır. Bu bloba ilişkili bir anlık görüntü varsa, temel blob ve anlık görüntüdeki tüm bloklar artık birbirinden kalır ve her iki blobdaki tüm bloklar için ücretlendirilirsiniz. Bu, temel bloba ve anlık görüntüdeki veriler aynı kalabilse bile geçerlidir.
- Azure Blob hizmeti, iki Blobun aynı verileri içerip içermediğini belirleme anlamına gelir. Karşıya yüklenen ve yürütülen her bir blok, aynı verilere ve aynı blok KIMLIĞINE sahip olsa bile benzersiz olarak değerlendirilir. Ücretler benzersiz bloklar için tahakkuk ettiğinden, anlık görüntü sonucu olan bir Blobun ek benzersiz bloklar ve ek ücretler ile güncelleştirilmesini göz önünde bulundurmanız önemlidir.

### <a name="minimize-costs-with-snapshot-management"></a>Anlık görüntü yönetimi ile maliyetleri en aza indirme

Ek ücretlerden kaçınmak için anlık görüntülerinizi dikkatle yönetmeniz önerilir. Anlık görüntülerinizin depolanması tarafından tahakkuk eden maliyetleri en aza indirmeye yardımcı olması için bu en iyi yöntemleri izleyebilirsiniz:

- Aynı verilerle güncelleştirseniz bile blob 'u her güncelleştirdiğinizde bir blob ile ilişkili anlık görüntüleri silin ve yeniden oluşturun, çünkü uygulama tasarımınız anlık görüntüleri korumanızı gerektirmez. Blobun anlık görüntülerini silip yeniden oluşturarak Blobun ve anlık görüntülerin birbirinden yönlendirilmemesini sağlayabilirsiniz.
- Bir Blobun anlık görüntülerini yaşıyorsanız, blobu güncelleştirdiğinizde tüm Blobun üzerine yazmış olan yöntemlerin çağrılmasını önleyin. Bunun yerine, maliyetleri düşük tutmak için mümkün olan en az sayıda blok güncelleştirin.

### <a name="snapshot-billing-scenarios"></a>Anlık görüntü faturalandırma senaryoları

Aşağıdaki senaryolarda, giderlerin bir Blok Blobu ve anlık görüntüleri için nasıl tahakkuk olduğu gösterilmektedir.

#### <a name="scenario-1"></a>1\. Senaryo

Senaryo 1 ' de, anlık görüntü alındıktan sonra temel blob güncelleştirilmedi, bu nedenle ücretler yalnızca 1, 2 ve 3 benzersiz bloklar için ücretlendirilir.

![Azure depolama kaynakları](./media/snapshots-overview/storage-blob-snapshots-billing-scenario-1.png)

#### <a name="scenario-2"></a>2\. Senaryo

Senaryo 2 ' de, temel blob güncelleştirildi, ancak anlık görüntü yok. 2. blok güncelleştirildi ve aynı verileri ve aynı KIMLIĞI içerse de, anlık görüntüde blok 3 ile aynı değildir. Sonuç olarak, hesap dört blok için ücretlendirilir.

![Azure depolama kaynakları](./media/snapshots-overview/storage-blob-snapshots-billing-scenario-2.png)

#### <a name="scenario-3"></a>3\. Senaryo

Senaryo 3 ' te, temel blob güncelleştirildi, ancak anlık görüntü yok. Blok 3, temel Blobun içindeki blok 4 ile değiştirilmiştir, ancak anlık görüntü hala blok 3 ' ü yansıtır. Sonuç olarak, hesap dört blok için ücretlendirilir.

![Azure depolama kaynakları](./media/snapshots-overview/storage-blob-snapshots-billing-scenario-3.png)

#### <a name="scenario-4"></a>4\. Senaryo

Senaryo 4 ' te, temel blob tamamen güncelleştirilmiştir ve özgün bloklarından hiçbirini içermez. Sonuç olarak, hesap tüm sekiz benzersiz blok için ücretlendirilir.

![Azure depolama kaynakları](./media/snapshots-overview/storage-blob-snapshots-billing-scenario-4.png)

> [!TIP]
> Tüm Blobun üzerine yazacak yöntemlerin çağrılmasını önleyin ve bunun yerine maliyetleri düşük tutmak için ayrı blokları güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar

- [.NET 'te blob anlık görüntüsü oluşturma ve yönetme](snapshots-manage-dotnet.md)
- [Artımlı anlık görüntülerle Azure yönetilmeyen VM disklerini yedekleme](../../virtual-machines/windows/incremental-snapshots.md)
