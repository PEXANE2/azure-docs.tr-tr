---
title: Blob sürümü oluşturma
titleSuffix: Azure Storage
description: Blob Storage sürümü oluşturma bir nesnenin önceki sürümlerini otomatik olarak korur ve zaman damgalarına göre tanımlar. Yanlışlıkla değiştiriliyorsa veya silinirse verilerinizi kurtarmak için bir Blobun önceki sürümlerini geri yükleyebilirsiniz.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 08/27/2020
ms.author: tamram
ms.subservice: blobs
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 2e3cfd27d36558587ca35cc1c573999a48092b0d
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89297685"
---
# <a name="blob-versioning"></a>Blob sürümü oluşturma

Bir nesnenin önceki sürümlerini otomatik olarak sürdürmek için blob Storage sürümü oluşturmayı etkinleştirebilirsiniz.  Blob sürümü oluşturma etkinleştirildiğinde, yanlışlıkla değiştirildiyse veya silinirse verilerinizi kurtarmak için bir Blobun önceki bir sürümünü geri yükleyebilirsiniz.

Blob sürümü oluşturma, depolama hesabında etkinleştirilir ve depolama hesabındaki tüm Bloblar için geçerlidir. Depolama hesabı için blob sürüm oluşturmayı etkinleştirdikten sonra Azure depolama, depolama hesabındaki her blob için sürümleri otomatik olarak korur.

Microsoft, üst veri koruma için bir Blobun önceki sürümlerini sürdürmek üzere blob sürümlendirme kullanılmasını önerir. Mümkün olduğunda, önceki sürümleri sürdürmek için blob anlık görüntüleri yerine blob sürümü oluşturma 'yı kullanın. Blob anlık görüntüleri, bir Blobun önceki sürümlerini koruduklarında benzer işlevler sağlar, ancak anlık görüntülerin uygulamanız tarafından el ile saklanması gerekir.

Blob sürümü oluşturmayı nasıl etkinleştireceğinizi öğrenmek için bkz. [BLOB sürüm oluşturmayı etkinleştirme ve yönetme](versioning-enable.md).

> [!IMPORTANT]
> Blob sürümü oluşturma, bir depolama hesabı veya kapsayıcısının yanlışlıkla silinmesini kurtarmanıza yardımcı olamaz. Depolama hesabının yanlışlıkla silinmesini engellemek için, depolama hesabı kaynağında bir **Cannotdelete** kilidi yapılandırın. Azure kaynaklarını kilitleme hakkında daha fazla bilgi için, bkz. [beklenmeyen değişiklikleri engellemek için kaynakları kilitleme](../../azure-resource-manager/management/lock-resources.md).

[!INCLUDE [storage-data-lake-gen2-support](../../../includes/storage-data-lake-gen2-support.md)]

## <a name="how-blob-versioning-works"></a>Blob sürümü oluşturma nasıl kullanılır?

Bir sürüm, belirli bir noktadaki bir Blobun durumunu yakalar. Blob sürümü oluşturma bir depolama hesabı için etkinleştirildiğinde, blob her değiştirildiğinde veya silindiğinde Azure Storage otomatik olarak bir blob 'un yeni bir sürümünü oluşturur.

Sürüm oluşturma özelliği etkinken bir blob oluşturduğunuzda, yeni blob blob 'un geçerli sürümüdür (veya temel Blobun). Bu blobu daha sonra değiştirirseniz, Azure depolama, değiştirilmeden önce blob durumunu yakalayan bir sürüm oluşturur. Değiştirilen blob yeni geçerli sürüm olur. Blobu her değiştirdiğiniz zaman yeni bir sürüm oluşturulur.

Sürüm oluşturma etkinken bir blobu sildiğinizde, Azure depolama, silinmeden önce blob durumunu yakalayan bir sürüm oluşturur. Blob 'un geçerli sürümü silinir, ancak gerekirse yeniden oluşturulabilmesi için Blobun sürümleri korunur. 

Blob sürümleri sabittir. Mevcut bir blob sürümünün içeriğini veya meta verilerini değiştiremezsiniz.

Blob sürümü oluşturma genel amaçlı v2, Blok Blobu ve BLOB depolama hesapları için kullanılabilir. Azure Data Lake Storage 2. ile kullanım için etkinleştirilmiş hiyerarşik bir ad alanı olan depolama hesapları Şu anda desteklenmemektedir.

Azure depolama REST API sürüm 2019-10-10 ve üzeri, blob sürümü oluşturmayı destekler.

### <a name="version-id"></a>Sürüm KIMLIĞI

Her blob sürümü bir sürüm KIMLIĞIYLE tanımlanır. Sürüm KIMLIĞININ değeri, Blobun yazıldığı veya güncelleştirildiği zaman damgasıdır. Sürüm KIMLIĞI, Sürüm oluşturulduğu sırada atanır.

Sürüm KIMLIĞINI sağlayarak bir Blobun belirli bir sürümünde okuma veya silme işlemleri yapabilirsiniz. Sürüm KIMLIĞINI atlarsanız, işlem geçerli sürüme (temel blob) göre davranır.

Blob oluşturmak veya değiştirmek için bir yazma işlemi çağırdığınızda, Azure Storage yanıttaki *x-MS-Version-id* üst bilgisini döndürür. Bu üstbilgi, yazma işlemi tarafından oluşturulan Blobun geçerli sürümü için sürüm KIMLIĞINI içerir.

Sürüm KIMLIĞI, sürümün ömrü boyunca aynı kalır.

### <a name="versioning-on-write-operations"></a>Yazma işlemlerinde sürüm oluşturma

Blob sürüm oluşturma açık olduğunda, bir Blobun her yazma işlemi yeni bir sürüm oluşturur. Yazma işlemleri, [BLOB koyma](/rest/api/storageservices/put-blob), [blok listesini yerleştirme](/rest/api/storageservices/put-block-list), [blobu kopyalama](/rest/api/storageservices/copy-blob)ve [BLOB meta verilerini ayarlama](/rest/api/storageservices/set-blob-metadata)içerir.

Yazma işlemi yeni bir blob oluşturursa, sonuçta elde edilen blob blob 'un geçerli sürümüdür. Yazma işlemi var olan bir Blobun değiştirirse, yeni veriler güncel sürüm olan güncelleştirilmiş blob 'da yakalanır ve Azure Storage, blob 'un önceki durumunu kaydeden bir sürüm oluşturur.

Kolaylık olması için, bu makalede gösterilen diyagramlarda sürüm KIMLIĞI bir basit tamsayı değeri olarak görüntülenir. Gerçekte, sürüm KIMLIĞI bir zaman damgasıdır. Geçerli sürüm mavi olarak gösterilir ve önceki sürümler gri renkte gösterilir.

Aşağıdaki diyagramda, yazma işlemlerinin blob sürümlerini nasıl etkilediği gösterilmektedir. Bir blob oluşturulduğunda bu blob geçerli sürümdür. Aynı blob değiştirildiğinde, Blobun önceki durumunu kaydetmek için yeni bir sürüm oluşturulur ve güncelleştirilmiş blob geçerli sürüm olur.

:::image type="content" source="media/versioning-overview/write-operations-blob-versions.png" alt-text="Yazma işlemlerinin sürümlü Blobları nasıl etkilediğini gösteren diyagram.":::

> [!NOTE]
> Depolama hesabı için etkinleştirilmeden önce oluşturulan bir Blobun sürüm KIMLIĞI yok. Blob değiştirildiğinde, değiştirilen blob geçerli sürüm olur ve güncelleştirmeden önce Blobun durumunu kaydetmek için bir sürüm oluşturulur. Sürüme, oluşturma süresi olan bir sürüm KIMLIĞI atanır.

### <a name="versioning-on-delete-operations"></a>Silme işlemlerinde sürüm oluşturma

Bir blobu sildiğinizde, blob 'un geçerli sürümü önceki bir sürüm olur ve temel blob silinir. Blob silindiğinde, blob 'un var olan tüm önceki sürümleri korunur.

Sürüm KIMLIĞI olmadan [BLOB silme](/rest/api/storageservices/delete-blob) işlemini çağırmak, temel blobu siler. Belirli bir sürümü silmek için, silme işleminde bu sürümün KIMLIĞINI sağlayın.

Aşağıdaki diyagramda, sürümlü bir blob üzerinde silme işleminin etkisi gösterilmektedir:

:::image type="content" source="media/versioning-overview/delete-versioned-base-blob.png" alt-text="Sürümlü blob silme işlemini gösteren diyagram.":::

Blob 'a yeni veri yazmak Blobun yeni bir sürümünü oluşturur. Aşağıdaki diyagramda gösterildiği gibi, var olan tüm sürümler etkilenmez.

:::image type="content" source="media/versioning-overview/recreate-deleted-base-blob.png" alt-text="Silinmeden sonra sürümlü blob 'un yeniden oluşturulmasını gösteren diyagram.":::

### <a name="blob-types"></a>Blob türleri

Blob sürümü oluşturma bir depolama hesabı için etkinleştirildiğinde, blok Bloblarındaki tüm yazma ve silme işlemleri yeni bir sürümün oluşturulmasını, [PUT bloğu](/rest/api/storageservices/put-block) işlemi dışında tetikler.

Sayfa Blobları ve ekleme Blobları için, yalnızca bir yazma ve silme işlemi alt kümesi bir sürüm oluşturulmasını tetikler. Bu işlemler şunları içerir:

- [İkili Büyük Nesne Koyma](/rest/api/storageservices/put-blob)
- [Öbek listesini yerleştirme](/rest/api/storageservices/put-block-list)
- [İkili Büyük Nesneyi Silme](/rest/api/storageservices/delete-blob)
- [Blob meta verilerini ayarla](/rest/api/storageservices/set-blob-metadata)
- [İkili Büyük Nesneyi Kopyalama](/rest/api/storageservices/copy-blob)

Aşağıdaki işlemler yeni bir sürümün oluşturulmasını tetiklemez. Bu işlemlerden değişiklikleri yakalamak için el ile anlık görüntü alın:

- [Yerleştirme sayfası](/rest/api/storageservices/put-page) (Sayfa Blobu)
- [Append bloğu](/rest/api/storageservices/append-block) (ekleme Blobu)

Bir Blobun tüm sürümleri aynı blob türünde olmalıdır. Bir Blobun önceki sürümleri varsa, ilk olarak blobu ve tüm sürümlerini silmediğiniz müddetçe, bir tür Blobun başka bir türle üzerine yazamaz.

### <a name="access-tiers"></a>Erişim katmanları

[BLOB katmanını ayarla](/rest/api/storageservices/set-blob-tier) işlemini çağırarak, geçerli sürüm dahil olmak üzere bir blok blobunun herhangi bir sürümünü farklı bir blob erişim katmanına taşıyabilirsiniz. Bir Blobun eski sürümlerini seyrek erişimli veya arşiv katmanına taşıyarak daha düşük kapasite fiyatlarından yararlanabilirsiniz. Daha fazla bilgi için bkz. [Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md).

Blok bloblarını uygun katmana taşıma işlemini otomatik hale getirmek için blob yaşam döngüsü yönetimi 'ni kullanın. Yaşam döngüsü yönetimi hakkında daha fazla bilgi için bkz. [Azure Blob depolama yaşam döngüsünü yönetme](storage-lifecycle-management-concepts.md).

## <a name="enable-or-disable-blob-versioning"></a>Blob sürüm oluşturmayı etkinleştirme veya devre dışı bırakma

Blob sürüm oluşturmayı etkinleştirme veya devre dışı bırakma hakkında bilgi edinmek için bkz. [BLOB sürüm oluşturmayı etkinleştirme ve yönetme](versioning-enable.md).

Blob sürüm oluşturmayı devre dışı bırakmak, mevcut blob 'ları, sürümleri veya anlık görüntüleri silmez. Blob sürüm oluşturmayı kapattığınızda, mevcut tüm sürümler depolama hesabınızda erişilebilir kalır. Yeni sürüm daha sonra oluşturulmaz.

Depolama hesabında sürüm oluşturma devre dışı bırakıldıktan sonra bir blob oluşturulup değiştirildiyse, blob 'un üzerine yazılması yeni bir sürüm oluşturur. Güncelleştirilmiş blob artık geçerli sürüm değil ve sürüm KIMLIĞINE sahip değil. Blob 'un sonraki tüm güncelleştirmeleri önceki durumu kaydetmeden verilerinin üzerine yazar.

Sürüm oluşturma devre dışı bırakıldıktan sonra sürüm KIMLIĞINI kullanarak sürümleri okuyabilir veya silebilirsiniz. Ayrıca, sürüm oluşturma devre dışı bırakıldıktan sonra bir Blobun sürümlerini listeleyebilirsiniz.

Aşağıdaki diyagramda, sürüm oluşturma işlemi devre dışı bırakıldıktan sonra bir Blobun nasıl değiştirileceği gösterilmektedir ve sürümü bulunmayan bir blob oluşturulur. Blob ile ilişkili tüm mevcut sürümler korunur.

:::image type="content" source="media/versioning-overview/modify-base-blob-versioning-disabled.png" alt-text="Sürüm oluşturma devre dışı bırakıldıktan sonra değiştirilen temel blobu gösteren diyagram.":::

## <a name="blob-versioning-and-soft-delete"></a>Blob sürümü oluşturma ve geçici silme

Blob sürümü oluşturma ve BLOB geçici silme, size en uygun veri koruması sağlamak için birlikte çalışır. Geçici silmeyi etkinleştirdiğinizde, Azure depolama 'nın geçici olarak silinen bir blobu ne kadar süreyle koruyacağını belirtirsiniz. Geçici olarak silinen blob sürümü sistemde kalır ve geçici silme Bekletme dönemi içinde silinebilir. Blob geçici silme hakkında daha fazla bilgi için bkz. [Azure depolama Blobları Için geçici silme](storage-blob-soft-delete.md).

### <a name="deleting-a-blob-or-version"></a>Blob veya sürümü silme

Geçici silme, blob sürümlerini silmek için ek koruma sağlar. Depolama hesabında hem sürüm oluşturma hem de geçici silme etkinse, bir blobu sildiğinizde Azure depolama, blob 'un durumunu silinmeden hemen önce kaydetmek için yeni bir sürüm oluşturur ve geçerli sürümü siler. Yeni sürüm geçici olarak silinmez ve geçici silme bekletme süresi sona erdiğinde kaldırılmaz.

Blob 'un önceki bir sürümünü sildiğinizde, sürüm geçici olarak silinir. Geçici olarak silinen sürüm, depolama hesabının geçici silme ayarlarında belirtilen Bekletme dönemi boyunca tutulur ve geçici silme saklama süresi sona erdiğinde kalıcı olarak silinir.

Bir Blobun önceki bir sürümünü kaldırmak için sürüm KIMLIĞINI belirterek açıkça silin.

Aşağıdaki diyagramda bir Blobu veya blob sürümünü sildiğinizde ne olacağı gösterilmektedir.

:::image type="content" source="media/versioning-overview/soft-delete-historical-version.png" alt-text="Geçici silme özelliği etkinken bir sürümü silmeyi gösteren diyagram.":::

Bir depolama hesabında hem sürüm oluşturma hem de geçici silme etkinse, bir blob veya blob sürümü değiştirildiğinde veya silindiğinde, geçici olarak silinen anlık görüntü oluşturulmaz.

### <a name="restoring-a-soft-deleted-version"></a>Geçici olarak silinen bir sürümü geri yükleme

Geçici olarak silinen bir blob sürümünü, geçici silme bekletme süresi etkinken sürüm üzerinde geri alma [blobu](/rest/api/storageservices/undelete-blob) işlemini çağırarak geri yükleyebilirsiniz. **Geri alma blobu** işlemi, Blobun tüm geçici silinen sürümlerini geri yükler.

Geçici olarak silinen sürümleri geri alma **blobu** ile geri yükleme, herhangi bir sürümü geçerli sürüm olacak şekilde yükseltemez. Geçerli sürümü geri yüklemek için, önce tüm geçici silinen sürümleri geri yükleyin ve ardından blobu geri yüklemek için önceki bir sürümü kopyalamak üzere [blobu kopyalama](/rest/api/storageservices/copy-blob) işlemini kullanın.

Aşağıdaki diyagramda, **silme blobu** işlemiyle, geçici olarak silinen blob sürümlerinin nasıl geri yükleneceği ve BLOB 'un geçerli sürümünün **BLOB kopyalama** işlemiyle nasıl geri yükleneceği gösterilmektedir.

:::image type="content" source="media/versioning-overview/undelete-version.png" alt-text="Geçici olarak silinen sürümlerin nasıl geri yükleneceğini gösteren diyagram.":::

Geçici silme bekletme süresi geçtikten sonra, geçici olarak silinen blob sürümleri kalıcı olarak silinir.

## <a name="blob-versioning-and-blob-snapshots"></a>Blob sürümü oluşturma ve BLOB anlık görüntüleri

Blob anlık görüntüsü, belirli bir zamanda belirli bir noktada alınmış bir Blobun salt okuma kopyasıdır. Blob anlık görüntüleri ve BLOB sürümleri benzerdir, ancak siz veya uygulamanız tarafından el ile oluşturulan bir anlık görüntü, depolama hesabınız için blob sürümü oluşturma etkinken yazma veya silme işleminde otomatik olarak bir blob sürümü oluşturulur.

> [!IMPORTANT]
> Microsoft, blob sürüm oluşturmayı etkinleştirdikten sonra, blok bloblarının anlık görüntülerini almayı durdurmak için uygulamanızı da güncelleştirmenizi önerir. Depolama hesabınız için sürüm oluşturma etkinleştirilmişse, tüm blok blob güncelleştirmeleri ve silmeleri, sürümler tarafından yakalanır ve korunur. Blob sürümü oluşturma etkinse, anlık görüntü alma Blok Blobu verilerinize ek koruma sunmaz ve maliyetleri ve uygulama karmaşıklığını artırabilir.

### <a name="snapshot-a-blob-when-versioning-is-enabled"></a>Sürüm oluşturma etkinken bir Blobun anlık görüntü

Önerilmese de, aynı zamanda sürümlü bir Blobun anlık görüntüsünü alabilirsiniz. Sürüm oluşturmayı etkinleştirdiğinizde uygulamanızın blob görüntülerini almayı durduracak şekilde güncellenemez, uygulamanız hem anlık görüntüleri hem de sürümleri destekleyebilir.

Sürümlü bir Blobun anlık görüntüsünü aldığınızda, anlık görüntünün oluşturulduğu anda yeni bir sürüm oluşturulur. Bir anlık görüntü çekilirken yeni bir geçerli sürüm de oluşturulur.

Aşağıdaki diyagramda, sürümlü bir Blobun anlık görüntüsünü alırken ne olacağı gösterilmektedir. Diyagramda, sürüm KIMLIĞI 2 ve 3 olan blob sürümleri ve anlık görüntüler aynı verileri içerir.

:::image type="content" source="media/versioning-overview/snapshot-versioned-blob.png" alt-text="Sürümlü bir Blobun anlık görüntülerini gösteren diyagram.":::

## <a name="authorize-operations-on-blob-versions"></a>Blob sürümlerinde yetkilendirme işlemleri

Aşağıdaki yaklaşımlardan birini kullanarak blob sürümlerine erişim yetkisi verebilirsiniz:

- Rol tabanlı erişim denetimi 'ni (RBAC) kullanarak Azure Active Directory (Azure AD) güvenlik sorumlusu için izin verin. Microsoft, üstün güvenlik ve kullanım kolaylığı için Azure AD 'nin kullanılmasını önerir. Azure AD 'yi blob işlemleriyle kullanma hakkında daha fazla bilgi için bkz. [Azure Active Directory kullanarak bloblara ve kuyruklara erişim yetkisi verme](../common/storage-auth-aad.md).
- Blob sürümlerine erişim yetkisi vermek için paylaşılan erişim imzasını (SAS) kullanarak. `bv`Belirli bir sürümdeki işlemler için BIR SAS belirteci oluşturmak üzere, bir blob sürümünü temsil eden imzalı kaynak türü için sürüm kimliğini belirtin. Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz. [paylaşılan erişim imzaları (SAS) kullanarak Azure depolama kaynaklarına sınırlı erişim verme](../common/storage-sas-overview.md).
- Paylaşılan anahtarla blob sürümlerine karşı işlemleri yetkilendirmek için hesap erişim anahtarlarını kullanarak. Daha fazla bilgi için bkz. [paylaşılan anahtarla yetkilendirme](/rest/api/storageservices/authorize-with-shared-key).

Blob sürümü oluşturma, verilerinizi yanlışlıkla veya kötü amaçlı olarak silinmeye karşı korumak için tasarlanmıştır. Korumayı iyileştirmek için bir blob sürümünün silinmesi özel izinler gerektirir. Aşağıdaki bölümlerde bir blob sürümünü silmek için gereken izinler açıklanır.

### <a name="rbac-action-to-delete-a-blob-version"></a>Blob sürümünü silmek için RBAC eylemi

Aşağıdaki tabloda, hangi RBAC eylemlerinin bir blob veya blob sürümünü silmenin desteklediği gösterilmektedir.

| Description | Blob hizmeti işlemi | RBAC verileri eylemi gerekiyor | RBAC yerleşik rol desteği |
|----------------------------------------------|------------------------|---------------------------------------------------------------------------------------|-------------------------------|
| Blobun geçerli sürümü siliniyor | İkili Büyük Nesneyi Silme | **Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Bloblar/Sil** | Depolama Blob Verileri Katkıda Bulunanı |
| Bir sürümü silme | İkili Büyük Nesneyi Silme | **Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Bloblar/deleteBlobVersion/Action** | Depolama Blobu veri sahibi |

### <a name="shared-access-signature-sas-parameters"></a>Paylaşılan erişim imzası (SAS) parametreleri

Blob sürümü için imzalanan kaynak `bv` . Daha fazla bilgi için bkz. [HIZMET SAS oluşturma](/rest/api/storageservices/create-service-sas) veya [Kullanıcı temsili SAS oluşturma](/rest/api/storageservices/create-user-delegation-sas).

Aşağıdaki tabloda bir blob sürümünü silmek için SAS üzerinde gereken izin gösterilmektedir.

| **İzin** | **URI simgesi** | **İzin verilen işlemler** |
|----------------|----------------|------------------------|
| Sil         | x              | Blob sürümünü silin. |

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Blob sürümü oluşturma özelliğinin etkinleştirilmesi hesabınıza ek veri depolama ücretleri oluşmasına neden olabilir. Uygulamanızı tasarlarken, maliyetleri en aza indirmek için bu ücretlerin nasıl tahakkuk edebileceğini bilmeniz önemlidir.

Blob anlık görüntüleri gibi BLOB sürümleri, etkin verilerle aynı hızda faturalandırılır. Sürümlerin faturalandırılması, temel Blobun veya sürümlerinin (ya da anlık görüntülerinin) açık olarak katmanı ayarlamış olmanıza bağlı olarak değişir. Blob katmanları hakkında daha fazla bilgi için bkz. [Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md).

Bir Blobun veya sürümün katmanını değiştirmediyseniz, söz konusu blob, sürümleri ve sahip olabileceği tüm anlık görüntülerle ilgili benzersiz veri blokları için faturalandırılırsınız. Daha fazla bilgi için bkz. [BLOB katmanı açıkça ayarlanmamışsa faturalandırma](#billing-when-the-blob-tier-has-not-been-explicitly-set).

Bir Blobun veya sürümün katmanını değiştirdiyseniz, blob ve sürümün son olarak aynı katmanda olup olmamasından bağımsız olarak tüm nesne için faturalandırılırsınız. Daha fazla bilgi için bkz. [BLOB katmanı açıkça ayarlandığında faturalama](#billing-when-the-blob-tier-has-been-explicitly-set).

> [!NOTE]
> Genellikle üzerine yazılacak veriler için sürüm oluşturmanın etkinleştirilmesi, depolama kapasitesi ücretlerine yol açabilir ve listeleme işlemleri sırasında gecikme süresini artırabilir. Bu kaygıları azaltmak için, sık sık oluşan verileri, sürüm oluşturma devre dışı olan ayrı bir depolama hesabında depolayın.

Blob anlık görüntülerinin faturalama ayrıntıları hakkında daha fazla bilgi için bkz. [BLOB anlık görüntüleri](snapshots-overview.md).

### <a name="billing-when-the-blob-tier-has-not-been-explicitly-set"></a>Blob katmanı açıkça ayarlanmamışsa faturalandırma

Bir temel blob veya sürümlerinin herhangi birinin blob katmanını açıkça ayarlamazsanız, blob, sürümleri ve sahip olabileceği tüm anlık görüntülerle ilgili benzersiz bloklar veya sayfalar için ücretlendirilirsiniz. Blob ve sürümleri üzerinde paylaşılan veriler yalnızca bir kez ücretlendirilir. Bir blob güncelleştirilirken, bir temel blob içindeki veriler, sürümlerinde depolanan verilerden farklıdır ve benzersiz veriler blok veya sayfa başına ücretlendirilir.

Blok Blobu içindeki bir bloğu değiştirdiğinizde, bu blok daha sonra benzersiz bir blok olarak ücretlendirilir. Bu, blok aynı blok KIMLIĞINE ve önceki sürümde aynı verilere sahip olsa bile geçerlidir. Blok yeniden gerçekleştirildikten sonra, önceki sürümdeki karşılığından itibaren önem altına alınır ve veriler için ücretlendirilirsiniz. Aynı verilerle güncelleştirilmiş bir sayfa blobunun bir sayfası için de aynı değer geçerlidir.

Blob depolamada, iki Blobun aynı verileri içerip içermediğini belirleme yolu yoktur. Karşıya yüklenen ve yürütülen her bir blok, aynı verilere ve aynı blok KIMLIĞINE sahip olsa bile benzersiz olarak değerlendirilir. Ücretler benzersiz bloklar için tahakkuk ettiğinden, sürüm oluşturma etkinken bir blob 'u güncelleştirme, ek benzersiz bloklar ve ek ücretler elde edilmesine dikkat etmeniz önemlidir.

Blob sürümü oluşturma etkinken, blok Bloblarındaki güncelleştirme işlemlerini, en az olası blok sayısını güncelleştirecek şekilde çağırın. Bloklar üzerinde ayrıntılı denetime izin veren yazma işlemleri, [PUT bloğu](/rest/api/storageservices/put-block) ve [yerleştirme engellenenler listesi](/rest/api/storageservices/put-block-list)' dir. Diğer taraftan [BLOB 'U koy](/rest/api/storageservices/put-blob) işlemi, bir Blobun tüm içeriğini değiştirir ve bu nedenle ek ücretlere neden olabilir.

Aşağıdaki senaryolar, blob katmanı açıkça ayarlanmamışsa bir Blok Blobu ve sürümlerinin ücretleri için nasıl tahakkuk olduğunu gösterir.

#### <a name="scenario-1"></a>1\. Senaryo

Senaryo 1 ' de, blob 'un önceki bir sürümü vardır. Sürüm oluşturulduktan sonra blob güncelleştirilmedi, bu nedenle ücretler yalnızca 1, 2 ve 3 benzersiz blokları için ücretlendirilir.

![Temel Blobun ve önceki sürümdeki benzersiz bloklar için faturalandırmayı gösteren diyagram 1.](./media/versioning-overview/versions-billing-scenario-1.png)

#### <a name="scenario-2"></a>2\. Senaryo

Senaryo 2 ' de, blobdaki bir blok (diyagramdaki blok 3) güncelleştirildi. Güncelleştirilmiş blok aynı verileri ve aynı KIMLIĞI içerse de, önceki sürümde blok 3 ile aynı değildir. Sonuç olarak, hesap dört blok için ücretlendirilir.

![Temel Blobun ve önceki sürümdeki benzersiz bloklar için faturalandırmayı gösteren diyagram 2.](./media/versioning-overview/versions-billing-scenario-2.png)

#### <a name="scenario-3"></a>3\. Senaryo

Senaryo 3 ' te blob güncelleştirildi, ancak sürüm değil. Blok 3, temel Blobun içindeki blok 4 ile değiştirilmiştir, ancak önceki sürüm hala blok 3 ' ü yansıtır. Sonuç olarak, hesap dört blok için ücretlendirilir.

![Temel Blobun ve önceki sürümdeki benzersiz bloklar için faturalandırmayı gösteren diyagram 3.](./media/versioning-overview/versions-billing-scenario-3.png)

#### <a name="scenario-4"></a>4\. Senaryo

Senaryo 4 ' te, temel blob tamamen güncelleştirilmiştir ve özgün bloklarından hiçbirini içermez. Sonuç olarak, hesap, &mdash; temel Blobun dört benzersiz blok ve önceki sürümde dört adet ücretlendirilir. Bu senaryo, [BLOB 'U koy](/rest/api/storageservices/put-blob) işlemi ile bir blob 'a yazıyorsanız, bu durum temel Blobun tüm içeriğinin yerini almıştır.

![Temel Blobun ve önceki sürümdeki benzersiz bloklar için faturalandırmayı gösteren diyagram 4.](./media/versioning-overview/versions-billing-scenario-4.png)

### <a name="billing-when-the-blob-tier-has-been-explicitly-set"></a>Blob katmanı açıkça ayarlandığında faturalandırma

Blob katmanını bir blob veya sürüm (veya anlık görüntü) için açıkça ayarladıysanız, yeni katmandaki nesnenin tam içerik uzunluğu için ücretlendirilirsiniz. Bu durumda, blokları orijinal katmanda bir nesneyle paylaştığından bağımsız olarak. Orijinal katmanda en eski sürümün tam içerik uzunluğu için de ücretlendirilirsiniz. Özgün katmanda kalan diğer önceki sürümler veya anlık görüntüler, [BLOB katmanı açıkça ayarlanmamışsa faturalandırma](#billing-when-the-blob-tier-has-not-been-explicitly-set)bölümünde açıklandığı gibi, PAYLAŞILABİLECEKLERİ benzersiz bloklar için ücretlendirilir.

#### <a name="moving-a-blob-to-a-new-tier"></a>Bir blobu yeni katmana taşıma

Aşağıdaki tabloda, yeni katmana taşındığında bir blob veya sürüm için faturalandırma davranışı açıklanmaktadır.

| Blob katmanı açık olarak ayarlandığında... | Ardından, için faturalandırılırsınız... |
|-|-|
| Önceki sürüme sahip bir temel blob | Yeni katmandaki temel blob ve orijinal katmandaki en eski sürüm ve diğer sürümlerdeki benzersiz bloklar. <sup>1</sup> |
| Önceki sürüme ve anlık görüntüye sahip bir temel blob | Yeni katmandaki temel blob, orijinal katmandaki en eski sürüm ve orijinal katmandaki en eski anlık görüntü ve diğer sürümlerdeki veya anlık görüntü<sup>1</sup>' deki tüm benzersiz bloklar. |
| Önceki bir sürüm | Yeni katmandaki sürüm ve orijinal katmandaki temel blob ve diğer sürümlerdeki benzersiz bloklar. <sup>1</sup> |

<sup>1</sup> Özgün katmanlarından taşınmayan başka bir önceki sürüm veya anlık görüntü varsa, bu sürümler veya anlık görüntüler, [BLOB katmanı açıkça ayarlanmamışsa faturalandırma](#billing-when-the-blob-tier-has-not-been-explicitly-set)bölümünde açıklandığı gibi, içerdikleri benzersiz blok sayısına göre ücretlendirilir.

Aşağıdaki diyagramda, sürümlü bir blob farklı bir katmana taşındığında nesnelerin nasıl faturalandırılabileceği gösterilmektedir.

:::image type="content" source="media/versioning-overview/versioning-billing-tiers.png" alt-text="Sürümlü bir blob açık şekilde katmanlandığı zaman nesnelerin nasıl faturalandırıldığını gösteren diyagram.":::

Bir blob, sürüm veya anlık görüntü için katmanı açıkça ayarlamak geri alınamaz. Bir blobu yeni bir katmana taşır ve sonra özgün katmanına geri taşırsanız, özgün katmandaki diğer nesnelerle blokları paylaşsa bile nesnenin tam içerik uzunluğu için ücretlendirilirsiniz.

Blob, sürüm veya anlık görüntünün katmanını açıkça ayarlamış işlemler şunlardır:

- [Blob Katmanını Ayarla](/rest/api/storageservices/set-blob-tier)
- [Blobu](/rest/api/storageservices/put-blob) belirtilen katmana yerleştir
- Belirtilen katmana sahip [blok listesini yerleştir](/rest/api/storageservices/put-block-list)
- Belirtilen katmana sahip [blobu Kopyala](/rest/api/storageservices/copy-blob)

#### <a name="deleting-a-blob-when-soft-delete-is-enabled"></a>Geçici silme etkinken bir blobu silme

Blob geçici silme etkinleştirildiğinde, katmanı açıkça ayarlanmış olan bir temel blobu siler veya üzerine yazdığınızda, geçici olarak silinen blob 'un önceki sürümleri tam içerik uzunluğuna göre faturalandırılır. Blob sürümü oluşturma ve geçici silme işlemlerinin birlikte çalışması hakkında daha fazla bilgi için bkz. [BLOB sürümü oluşturma ve geçici silme](#blob-versioning-and-soft-delete).

Aşağıdaki tabloda, sürüm oluşturma 'nın etkin veya devre dışı olmasına bağlı olarak, geçici olarak silinen bir blob için faturalandırma davranışı açıklanmaktadır. Sürüm oluşturma etkinleştirildiğinde bir blob geçici olarak silindiğinde bir sürüm oluşturulur. Sürüm oluşturma devre dışı bırakıldığında, bir blobu geçici olarak silmek, yumuşak silme anlık görüntüsü oluşturur.

| Katman açıkça ayarlanmış bir temel Blobun üzerine yazdığınızda... | Ardından, için faturalandırılırsınız... |
|-|-|
| Blob geçici silme ve sürüm oluşturma özelliği etkinse | Katman ne olursa olsun, tüm mevcut sürümler tam içerik uzunluğuna sahiptir. |
| Blob geçici silme etkinse ancak sürüm oluşturma devre dışıysa | Katmanından bağımsız olarak tam içerik uzunluğundaki tüm mevcut geçici ekran anlık görüntüleri. |

## <a name="see-also"></a>Ayrıca bkz.

- [Blob sürüm oluşturmayı etkinleştirme ve yönetme](versioning-enable.md)
- [Blob 'un anlık görüntüsünü oluşturma](/rest/api/storageservices/creating-a-snapshot-of-a-blob)
- [Azure depolama Blobları için geçici silme](storage-blob-soft-delete.md)
