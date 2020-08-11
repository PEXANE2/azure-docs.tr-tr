---
title: Bloblar için geçici silme
titleSuffix: Azure Storage
description: Blob 'lar için geçici silme, verilerinizi korur, böylece bir uygulama veya başka bir depolama hesabı kullanıcısı tarafından yanlışlıkla değiştirildiğinde veya silindiğinde verilerinizi daha kolay bir şekilde kurtarabilirsiniz.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 2e390c9d5d2fa7c6551ed661c6c25096732eefd5
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88057422"
---
# <a name="soft-delete-for-blobs"></a>Bloblar için geçici silme

Bloblara yönelik geçici silme, verilerinizin yanlışlıkla veya yanlışlıkla değiştirilmesini veya silinmesini önler. Blob 'lar için geçici silme, bir depolama hesabı için etkinleştirildiğinde, blob 'lar, blob sürümleri (Önizleme) ve bu depolama hesabındaki anlık görüntüler, sizin belirttiğiniz bir bekletme dönemi içinde silindikten sonra kurtarılabilir.

Verilerinizin yanlışlıkla bir uygulama veya başka bir depolama hesabı kullanıcısı tarafından değiştirilmesi veya silinmesi olasılığı varsa, Microsoft geçici silme özelliğini etkinleştirmeyi önerir. Geçici silmeyi etkinleştirme hakkında daha fazla bilgi için bkz. [Bloblar için geçici silmeyi etkinleştirme ve yönetme](soft-delete-enable.md).

[!INCLUDE [storage-data-lake-gen2-support](../../../includes/storage-data-lake-gen2-support.md)]

## <a name="about-soft-delete-for-blobs"></a>Bloblar için geçici silme hakkında

Depolama hesabında Bloblar için geçici silme etkinleştirildiğinde, nesneleri silindikten sonra, belirtilen veri saklama süresi içinde kurtarabilirsiniz. Bu koruma, bir üzerine yazma sonucu olarak silinen blob 'ları (Blok Blobu, ekleme Blobları veya sayfa Blobları) genişletir.

Blob geçici silme etkinken ancak blob sürümü oluşturma (Önizleme) etkin durumdayken mevcut bir Blobun veya anlık görüntüdeki veriler silinirse, üzerine yazılan verilerin durumunu kaydetmek için geçici olarak silinen bir anlık görüntü oluşturulur. Belirtilen bekletme döneminin süresi dolduktan sonra nesne kalıcı olarak silinir.

Depolama hesabında blob sürümü oluşturma ve BLOB geçici silme etkinse, bir blob silindiğinde, geçici olarak silinen bir anlık görüntü yerine yeni bir sürüm oluşturulur. Yeni sürüm geçici olarak silinmez ve geçici silme bekletme süresi sona erdiğinde kaldırılmaz. Bir Blobun geçici olarak silinen sürümleri, [geri alma blobu](/rest/api/storageservices/undelete-blob) işlemini çağırarak Bekletme dönemi içinde geri yüklenebilir. Blob, daha sonra [BLOB kopyalama](/rest/api/storageservices/copy-blob) işlemini çağırarak sürümlerinden birinden geri yüklenebilir. Blob sürümü oluşturma ve geçici silme hakkında daha fazla bilgi için bkz. [BLOB sürümü oluşturma ve geçici silme](versioning-overview.md#blob-versioning-and-soft-delete).

Geçici olarak listelenmediği takdirde geçici olarak silinen nesneler görünmez.

Blob geçici silme, geriye dönük olarak uyumludur, bu yüzden bu özelliğin sağladığı korumalardan yararlanmak için uygulamalarınızda herhangi bir değişiklik yapmanız gerekmez. Ancak, [veri kurtarma](#recovery) yeni bir **geri alma blob** API 'si sunar.

Blob geçici silme, hem yeni hem de mevcut genel amaçlı v2, genel amaçlı v1 ve BLOB depolama hesapları için kullanılabilir. Hem standart hem de Premium hesap türleri desteklenir. Blob geçici silme, sık erişimli, seyrek erişimli ve arşiv dahil tüm depolama katmanları için kullanılabilir. Geçici silme, kapsamakta olan sayfa Blobları olan yönetilmeyen diskler için kullanılabilir ancak yönetilen diskler için kullanılamaz.

### <a name="configuration-settings"></a>Yapılandırma ayarları

Yeni bir hesap oluşturduğunuzda, geçici silme varsayılan olarak devre dışıdır. Geçici silme Ayrıca, mevcut depolama hesapları için varsayılan olarak devre dışıdır. Her zaman bir depolama hesabı için geçici silmeyi etkinleştirebilir veya devre dışı bırakabilirsiniz.

Geçici silmeyi etkinleştirdiğinizde, saklama süresini yapılandırmanız gerekir. Saklama süresi, geçici olarak silinen verilerin saklanacağı ve kurtarılmasına uygun olan süreyi gösterir. Açıkça silinen nesneler için, veri silindiğinde Bekletme dönemi saati başlar. Verilerin üzerine yazıldığında geçici silme özelliği tarafından oluşturulan geçici olarak silinen sürümler veya anlık görüntüler için, saat, sürüm veya anlık görüntü oluşturulduğunda başlar. Bekletme dönemi 1 ila 365 gün arasında olabilir.

Geçici silme bekletme süresini istediğiniz zaman değiştirebilirsiniz. Güncelleştirilmiş bir saklama süresi yalnızca yeni silinen veriler için geçerlidir. Daha önce silinen verilerin süresi, verilerin silindiği zaman yapılandırılan bekletme dönemine göre dolar. Geçici olarak silinen bir nesneyi silmeye çalışmak, süre sonu süresini etkilemez.

Geçici silme devre dışı bırakırsanız, bu özellik etkinken kaydedilen depolama hesabınızda geçici olarak silinen verilere erişmeye ve kurtarmaya devam edebilirsiniz.

### <a name="saving-deleted-data"></a>Silinen veriler kaydediliyor

Geçici silme, verilerinizi nesnelerin silindiği veya üzerine yazıldığı birçok durumda korur.

Blob **koyma**, **PUT blok listesi**veya **kopyalama blobu**kullanarak bir Blobun üzerine yazıldığında, yazma işleminden önce blob 'un durumunun bir sürümü veya anlık görüntüsü otomatik olarak oluşturulur. Bu nesne, geçici olarak silinen nesneler açıkça listelenmediği takdirde görünmez. Geçici silinen nesneleri nasıl listeleyeceğinizi öğrenmek için [Kurtarma](#recovery) bölümüne bakın.

![Blob 'un, put blok listesi veya kopyalama blobu kullanılarak üzerine yazıldığı için, blob 'ların nasıl depolandığını gösteren diyagram.](media/soft-delete-blob-overview/storage-blob-soft-delete-overwrite.png)

*Geçici olarak silinen veriler gri, etkin veriler mavi olur. Daha eski veriler altında daha yeni yazılmış veriler görüntülenir. B1 ile B0 'ın üzerine yazıldığında, B0 'nin geçici olarak silinmiş bir anlık görüntüsü oluşturulur. B2 ile B1 üzerine yazıldığında, B1 'nin geçici olarak silinen bir anlık görüntüsü oluşturulur.*

> [!NOTE]  
> Geçici silme yalnızca hedef Blobun hesabı için açık olan kopyalama işlemleri için korumayı geçersiz kılar.

> [!NOTE]  
> Geçici silme, arşiv katmanındaki Bloblar için korumanın üzerine yazmaz. Arşivdeki bir Blobun herhangi bir katmanda yeni bir blob ile üzerine yazılırsa, üzerine yazılan Blobun kalıcı olarak zaman aşımına uğradı.

Bir anlık görüntüde **silme blobu** çağrıldığında, bu anlık görüntü geçici olarak silinmiş olarak işaretlenir. Yeni bir anlık görüntü oluşturulmaz.

![Blob 'un anlık görüntülerinin silme blobu kullanılırken geçici olarak silinme şeklini gösteren diyagram.](media/soft-delete-blob-overview/storage-blob-soft-delete-explicit-delete-snapshot.png)

*Geçici olarak silinen veriler gri, etkin veriler mavi olur. Daha eski veriler altında daha yeni yazılmış veriler görüntülenir. **Anlık görüntü blobu** çağrıldığında, B0 bir anlık görüntü haline gelir ve B1, Blobun etkin durumudur. B0 anlık görüntüsü silindiğinde, geçici olarak silinmiş olarak işaretlenir.*

**BLOB silme** bir temel blob 'da (kendi anlık görüntü olmayan herhangi bir BLOB) çağrıldığında, bu blob geçici olarak silindi olarak işaretlenir. Önceki davranışla tutarlı, etkin anlık görüntülere sahip bir bloba **silme blobu** çağırma bir hata döndürüyor. Geçici olarak silinen anlık görüntülerle blob üzerinde **silme blobu** çağırma bir hata döndürmez. Geçici silme açıkken bir blobu ve tüm anlık görüntülerini tek işlemle silebilirsiniz. Bunu yaptığınızda temel blob ve anlık görüntüler geçici olarak silinir.

![Bir temel bloba silme blogu çağrıldığında ne olacağını gösteren diyagram.](media/soft-delete-blob-overview/storage-blob-soft-delete-explicit-include.png)

*Geçici olarak silinen veriler gri, etkin veriler mavi olur. Daha eski veriler altında daha yeni yazılmış veriler görüntülenir. Burada, B2 ve ilişkili tüm anlık görüntüleri silmek için bir **silme blobu** çağrısı yapılır. Etkin blob, B2 ve tüm ilişkili anlık görüntüler, geçici olarak silinmiş olarak işaretlenir.*

> [!NOTE]  
> Geçici olarak silinen bir Blobun üzerine yazıldığında, yazma işleminden önce blob 'un durumunun geçici olarak silinmiş bir anlık görüntüsü otomatik olarak oluşturulur. Yeni blob, üzerine yazılan Blobun katmanını devralır.

Geçici silme, verileri kapsayıcı veya hesap silme durumlarında veya blob meta verilerinin ve BLOB özelliklerinin üzerine yazıldığında kaydetmez. Bir depolama hesabını hatalı silinmeye karşı korumak için Azure Resource Manager kullanarak bir kilit yapılandırabilirsiniz. Daha fazla bilgi için, [beklenmeyen değişiklikleri engellemek üzere kaynakları kilitleme](../../azure-resource-manager/management/lock-resources.md)Azure Resource Manager makalesine bakın.

Aşağıdaki tabloda, geçici silme açıkken beklenen davranışın ayrıntıları verilmiştir:

| REST API işlemi | Kaynak türü | Description | Davranış değişikliği |
|--------------------|---------------|-------------|--------------------|
| [Silme](/rest/api/storagerp/StorageAccounts/Delete) | Hesap | İçerdiği tüm kapsayıcılar ve BLOB 'lar dahil olmak üzere depolama hesabını siler.                           | Değişiklik yok. Silinen hesaptaki kapsayıcılar ve Bloblar kurtarılamaz. |
| [Kapsayıcıyı Silme](/rest/api/storageservices/delete-container) | Kapsayıcı | Kapsayıcı, içerdiği tüm Bloblar dahil olmak üzere siler. | Değişiklik yok. Silinen kapsayıcıdaki Bloblar kurtarılamaz. |
| [İkili Büyük Nesne Koyma](/rest/api/storageservices/put-blob) | Blok, ekleme ve sayfa Blobları | Yeni bir blob oluşturur veya bir kapsayıcı içinde var olan bir blobu değiştirir | Var olan bir Blobun değiştirmek için kullanılırsa, çağrıdan önce blob 'un durumunun bir anlık görüntüsü otomatik olarak oluşturulur. Bu, daha önce ve yalnızca aynı türdeki bir blob (blok, ekleme veya sayfa) ile değiştiriliyorsa, daha önce geçici olarak silinen bir blob için de geçerlidir. Farklı türdeki bir Blobun değiştirildiyse, var olan tüm geçici silinen verilerin geçerliliği kalıcı olarak dolacak. |
| [İkili Büyük Nesneyi Silme](/rest/api/storageservices/delete-blob) | Blok, ekleme ve sayfa Blobları | Silinmek üzere bir blob veya blob anlık görüntüsü işaretler. Çöp toplama sırasında blob veya anlık görüntü daha sonra silinir | Blob anlık görüntüsünü silmek için kullanılırsa, bu anlık görüntü geçici olarak silinmiş olarak işaretlenir. Bir blobu silmek için kullanılırsa, bu blob geçici olarak silindi olarak işaretlenir. |
| [İkili Büyük Nesneyi Kopyalama](/rest/api/storageservices/copy-blob) | Blok, ekleme ve sayfa Blobları | Kaynak blobu, aynı depolama hesabındaki veya başka bir depolama hesabındaki bir hedef bloba kopyalar. | Var olan bir Blobun değiştirmek için kullanılırsa, çağrıdan önce blob 'un durumunun bir anlık görüntüsü otomatik olarak oluşturulur. Bu, daha önce ve yalnızca aynı türdeki bir blob (blok, ekleme veya sayfa) ile değiştiriliyorsa, daha önce geçici olarak silinen bir blob için de geçerlidir. Farklı türdeki bir Blobun değiştirildiyse, var olan tüm geçici silinen verilerin geçerliliği kalıcı olarak dolacak. |
| [Yerleştirme bloğu](/rest/api/storageservices/put-block) | Blok blobları | Bir blok blobunun parçası olarak kaydedilecek yeni bir blok oluşturur. | Etkin olan bir Blobun blok yürütmek için kullanılırsa değişiklik yapılmaz. Geçici olarak silinen bir Blobun blok yürütmek için kullanılırsa, yeni bir blob oluşturulur ve geçici olarak silinen Blobun durumunu yakalamak için bir anlık görüntü oluşturulur. |
| [Öbek listesini yerleştirme](/rest/api/storageservices/put-block-list) | Blok blobları | Blok Blobu oluşturan blok kimlikleri kümesini belirterek bir blobu kaydeder. | Var olan bir Blobun değiştirmek için kullanılırsa, çağrıdan önce blob 'un durumunun bir anlık görüntüsü otomatik olarak oluşturulur. Bu, daha önce ve yalnızca Blok Blobu ise, daha önce geçici olarak silinmiş bir blob için de geçerlidir. Farklı türdeki bir Blobun değiştirildiyse, var olan tüm geçici silinen verilerin geçerliliği kalıcı olarak dolacak. |
| [Yerleştirme sayfası](/rest/api/storageservices/put-page) | Sayfa blobları | Sayfa blobuna bir sayfa aralığı yazar. | Değişiklik yok. Bu işlem kullanılarak üzerine yazılan veya temizlenmemiş Sayfa Blobu verileri kaydedilmez ve kurtarılamaz. |
| [Ekleme bloğu](/rest/api/storageservices/append-block) | Ekleme Blobları | Bir ekleme blobunun sonuna bir veri bloğu Yazar | Değişiklik yok. |
| [Blob özelliklerini ayarla](/rest/api/storageservices/set-blob-properties) | Blok, ekleme ve sayfa Blobları | Blob için tanımlanan sistem özellikleri için değerleri ayarlar. | Değişiklik yok. Üzerine yazılan blob özellikleri kurtarılabilir değil. |
| [Blob meta verilerini ayarla](/rest/api/storageservices/set-blob-metadata) | Blok, ekleme ve sayfa Blobları | Belirtilen blob için Kullanıcı tanımlı meta verileri bir veya daha fazla ad-değer çifti olarak ayarlar. | Değişiklik yok. Üzerine yazılan blob meta verileri kurtarılamaz. |

Sayfa Blobu aralıklarının üzerine yazmak veya onları temizlemek için **PUT sayfasının** çağrılması, otomatik olarak anlık görüntü oluşturmaz. Sanal makine diskleri sayfa Blobları tarafından desteklenir ve veri yazmak için **PUT sayfasını** kullanır.

### <a name="recovery"></a>Kurtarma

Geçici olarak silinen bir temel blob üzerinde [geri alma blobu](/rest/api/storageservices/undelete-blob) işleminin çağrılması, onu ve ilişkili tüm yazılımla silinen anlık görüntüleri etkin olarak geri yükler. Etkin bir temel blob üzerinde **geri alma blobu** işleminin çağrılması, tüm ilişkili geçici silinen anlık görüntüleri etkin olarak geri yükler. Anlık görüntüler etkin olarak geri yüklendiğinde, Kullanıcı tarafından oluşturulan anlık görüntüler gibi görünür. Bunlar temel Blobun üzerine yazmaz.

Bir blobu belirli bir geçici silinen anlık görüntüye geri yüklemek için, temel bloba **geri alma blobu** çağırabilirsiniz. Ardından, anlık görüntüyü şimdi etkin Blobun üzerine kopyalayabilirsiniz. Ayrıca, anlık görüntüyü yeni bir bloba kopyalayabilirsiniz.

![Silmeyi geri alma blobu kullanıldığında ne olacağını gösteren diyagram.](media/soft-delete-blob-overview/storage-blob-soft-delete-recover.png)

*Geçici olarak silinen veriler gri, etkin veriler mavi olur. Daha eski veriler altında daha yeni yazılmış veriler görüntülenir. Burada, blob B üzerinde **geri alma blobu** çağrılır, böylece temel blob, B1 ve tüm ilişkili anlık görüntüler, burada yalnızca B0 etkin olarak geri yüklenir. İkinci adımda, B0 temel blob üzerinden kopyalanır. Bu kopyalama işlemi B1 'nin geçici olarak silinmiş bir anlık görüntüsünü oluşturur.*

Geçici silinen Blobları ve BLOB anlık görüntülerini görüntülemek için, silinen verileri **liste bloblarına**dahil etme seçeneğini belirleyebilirsiniz. Yalnızca geçici olarak silinen temel Blobları görüntülemeyi veya Ayrıca, geçici olarak silinen blob anlık görüntülerini da dahil etmek seçebilirsiniz. Tüm geçici silinen veriler için, verilerin ne zaman silindiğini ve verilerin kalıcı olarak süresi dolmadan önce geçen gün sayısını görüntüleyebilirsiniz.

### <a name="example"></a>Örnek

Aşağıda, yumuşak silme açıkken *HelloWorld* adlı bir blobu karşıya yükleyen, üzerine yazan, anlık görüntülerle, silen ve geri yükleyen bir .net betiğinin konsol çıktısı verilmiştir:

```bash
Upload:
- HelloWorld (is soft deleted: False, is snapshot: False)

Overwrite:
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)

Snapshot:
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)

Delete (including snapshots):
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: True, is snapshot: False)

Undelete:
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)

Copy a snapshot over the base blob:
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)
```

Bu çıktıyı üreten uygulamanın işaretçisi için [sonraki adımlar](#next-steps) bölümüne bakın.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Tüm geçici silinen veriler, etkin verilerle aynı hızda faturalandırılır. Yapılandırılan saklama süresinden sonra kalıcı olarak silinen veriler için ücretlendirilmeyecektir. Anlık görüntülere ve bunların nasıl tahakkuk ettikleri hakkında daha ayrıntılı bilgi edinmek için bkz. [anlık görüntülerin nasıl tahakkuk ettirildiğini anlama](storage-blob-snapshots.md).

Otomatik anlık görüntü oluşturma ile ilgili işlemler için faturalandırılmaz. Yazma işlemleri için geri alma **BLOB** işlemleri için faturalandırılırsınız.

Genel olarak Azure Blob depolama fiyatları hakkında daha fazla ayrıntı için [Azure Blob depolama fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/storage/blobs/)göz atın.

Geçici silme özelliğini ilk kez açtığınızda, Microsoft, özelliğin faturanızı nasıl etkileyeceğini daha iyi anlamak için kısa bir bekletme süresi kullanılmasını önerir.

Sık sık üzerine yazılan veriler için geçici silme özelliğinin etkinleştirilmesi, depolama kapasitesi ücretlerine neden olabilir ve Bloblar listelenirken gecikme süresini artırabilir. Sık kullanılan verileri, geçici olarak silmenin devre dışı bırakıldığı ayrı bir depolama hesabında depolayarak bu ek maliyeti ve gecikmeyi azaltabilirsiniz.

## <a name="faq"></a>SSS

### <a name="can-i-use-the-set-blob-tier-api-to-tier-blobs-with-soft-deleted-snapshots"></a>Blob katmanı API 'sini, geçici olarak silinen anlık görüntülerle katman Blobları olarak kullanabilir miyim?

Evet. Geçici olarak silinen anlık görüntüler orijinal katmanda kalır, ancak temel blob yeni katmana taşınır.

### <a name="premium-storage-accounts-have-a-per-blob-snapshot-limit-of-100-do-soft-deleted-snapshots-count-toward-this-limit"></a>Premium Depolama hesaplarında 100 blob anlık görüntü sınırı vardır. Geçici olarak silinen anlık görüntü sayısı bu sınıra doğru mı?

Hayır, geçici olarak silinen anlık görüntüler bu sınıra doğru sayılmaz.

### <a name="if-i-delete-an-entire-account-or-container-with-soft-delete-turned-on-will-all-associated-blobs-be-saved"></a>Geçici silme özelliği açık olan bir hesabın tamamını veya kapsayıcıyı silersem, ilişkili tüm Bloblar kaydedilecek mi?

Hayır, tüm bir hesabı veya kapsayıcıyı silerseniz, ilişkili tüm Bloblar kalıcı olarak silinir. Bir depolama hesabını yanlışlıkla silmelerden koruma hakkında daha fazla bilgi için bkz. [kaynakları kilitleme, beklenmeyen değişiklikleri önleme](../../azure-resource-manager/management/lock-resources.md).

### <a name="can-i-view-capacity-metrics-for-deleted-data"></a>Silinen veriler için kapasite ölçümlerini görüntüleyebilir miyim?

Geçici olarak silinen veriler, toplam depolama hesabı kapasitenizin bir parçası olarak dahil edilmiştir. Depolama kapasitesini izleme ve izleme hakkında daha fazla bilgi için bkz. [depolama Analizi](../common/storage-analytics.md).

### <a name="can-i-read-and-copy-out-soft-deleted-snapshots-of-my-blob"></a>Blobun geçici olarak silinen anlık görüntülerini okuyabilir ve kopyalayabilir miyim?  

Evet, ancak önce blob üzerinde geri alma işlemini çağırmanız gerekir.

### <a name="is-soft-delete-available-for-virtual-machine-disks"></a>Sanal makine diskleri için geçici silme kullanılabilir mi?  

Hafif silme, hem Premium hem de standart yönetilmeyen diskler için kullanılabilir ve bu, kapakların altındaki sayfa Blobları olur. Geçici silme, yalnızca **BLOB silme**, **BLOB koyma**, **blok listesini yerleştirme**ve **BLOB işlemlerini kopyalama** ile silinen verileri kurtarmanıza yardımcı olur. **PUT sayfasına** yapılan bir çağrı ile üzerine yazılan veriler kurtarılamaz.

Azure sanal makinesi, **yerleştirme**için çağrılar kullanılarak yönetilmeyen bir diske yazar. bu nedenle, Azure VM 'den yönetilmeyen bir diske yazma işlemini geri almak için geçici silme kullanmak desteklenen bir senaryo değildir.

### <a name="do-i-need-to-change-my-existing-applications-to-use-soft-delete"></a>Geçici silme kullanmak için mevcut uygulamalarımı değiştirmem gerekiyor mu?

Kullanmakta olduğunuz API sürümüne bakılmaksızın geçici silme avantajından yararlanmak mümkündür. Ancak, geçici olarak silinen blob 'ları ve BLOB anlık görüntülerini listelemek ve kurtarmak için [Azure depolama REST API](/rest/api/storageservices/Versioning-for-the-Azure-Storage-Services) veya daha büyük sürümlerini 2017-07-29 kullanmanız gerekir. Microsoft, her zaman Azure Storage API 'sinin en son sürümünü kullanmasını önerir.

## <a name="next-steps"></a>Sonraki adımlar

- [Bloblar için geçici silmeyi etkinleştir](soft-delete-enable.md)
- [Blob sürümü oluşturma (Önizleme)](versioning-overview.md)
