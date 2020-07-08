---
title: Blob sürümü oluşturma (Önizleme)
titleSuffix: Azure Storage
description: Blob Storage sürümü oluşturma (Önizleme), bir nesnenin önceki sürümlerini otomatik olarak korur ve zaman damgalarına göre tanımlar. Yanlışlıkla değiştiriliyorsa veya silinirse verilerinizi kurtarmak için bir Blobun önceki sürümlerini geri yükleyebilirsiniz.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 05/05/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 89d69547d793599fc669927b1a500716a858cc89
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84433598"
---
# <a name="blob-versioning-preview"></a>Blob sürümü oluşturma (Önizleme)

Bir nesnenin önceki sürümlerini otomatik olarak sürdürmek için blob Storage sürümü oluşturma 'yı (Önizleme) etkinleştirebilirsiniz.  Blob sürümü oluşturma etkinleştirildiğinde, yanlışlıkla değiştirildiyse veya silinirse verilerinizi kurtarmak için bir Blobun önceki bir sürümünü geri yükleyebilirsiniz.

Blob sürümü oluşturma, depolama hesabında etkinleştirilir ve depolama hesabındaki tüm Bloblar için geçerlidir. Depolama hesabı için blob sürüm oluşturmayı etkinleştirdikten sonra Azure depolama, depolama hesabındaki her blob için sürümleri otomatik olarak korur.

Microsoft, üst veri koruma için bir Blobun önceki sürümlerini sürdürmek üzere blob sürümlendirme kullanılmasını önerir. Mümkün olduğunda, önceki sürümleri sürdürmek için blob anlık görüntüleri yerine blob sürümü oluşturma 'yı kullanın. Blob anlık görüntüleri, bir Blobun önceki sürümlerini koruduklarında benzer işlevler sağlar, ancak anlık görüntülerin uygulamanız tarafından el ile saklanması gerekir.

> [!IMPORTANT]
> Blob sürümü oluşturma, bir depolama hesabı veya kapsayıcısının yanlışlıkla silinmesini kurtarmanıza yardımcı olamaz. Depolama hesabının yanlışlıkla silinmesini engellemek için, depolama hesabı kaynağında bir **Cannotdelete** kilidi yapılandırın. Azure kaynaklarını kilitleme hakkında daha fazla bilgi için, bkz. [beklenmeyen değişiklikleri engellemek için kaynakları kilitleme](../../azure-resource-manager/management/lock-resources.md).

## <a name="how-blob-versioning-works"></a>Blob sürümü oluşturma nasıl kullanılır?

Bir sürüm, belirli bir noktadaki bir Blobun durumunu yakalar. Blob sürümü oluşturma bir depolama hesabı için etkinleştirildiğinde, blob her değiştirildiğinde veya silindiğinde Azure Storage otomatik olarak bir blob 'un yeni bir sürümünü oluşturur.

Sürüm oluşturma özelliği etkinken bir blob oluşturduğunuzda, yeni blob blob 'un geçerli sürümüdür (veya temel Blobun). Bu blobu daha sonra değiştirirseniz, Azure depolama, değiştirilmeden önce blob durumunu yakalayan bir sürüm oluşturur. Değiştirilen blob yeni geçerli sürüm olur. Blobu her değiştirdiğiniz zaman yeni bir sürüm oluşturulur.

Sürüm oluşturma etkinken bir blobu sildiğinizde, Azure depolama, silinmeden önce blob durumunu yakalayan bir sürüm oluşturur. Blob 'un geçerli sürümü silinir, ancak gerekirse yeniden oluşturulabilmesi için Blobun sürümleri korunur. 

Blob sürümleri sabittir. Mevcut bir blob sürümünün içeriğini veya meta verilerini değiştiremezsiniz.

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

:::image type="content" source="media/versioning-overview/write-operations-blob-versions.png" alt-text="Yazma işlemlerinin sürümlü Blobları nasıl etkilediğini gösteren diyagram":::

> [!NOTE]
> Depolama hesabı için etkinleştirilmeden önce oluşturulan bir Blobun sürüm KIMLIĞI yok. Blob değiştirildiğinde, değiştirilen blob geçerli sürüm olur ve güncelleştirmeden önce Blobun durumunu kaydetmek için bir sürüm oluşturulur. Sürüme, oluşturma süresi olan bir sürüm KIMLIĞI atanır.

### <a name="versioning-on-delete-operations"></a>Silme işlemlerinde sürüm oluşturma

Bir blobu sildiğinizde, blob 'un geçerli sürümü önceki bir sürüm olur ve temel blob silinir. Blob silindiğinde, blob 'un var olan tüm önceki sürümleri korunur.

Sürüm KIMLIĞI olmadan [BLOB silme](/rest/api/storageservices/delete-blob) işlemini çağırmak, temel blobu siler. Belirli bir sürümü silmek için, silme işleminde bu sürümün KIMLIĞINI sağlayın.

Aşağıdaki diyagramda, sürümlü bir blob üzerinde silme işleminin etkisi gösterilmektedir:

:::image type="content" source="media/versioning-overview/delete-versioned-base-blob.png" alt-text="Sürümlü blob silme işlemini gösteren diyagram":::

Blob 'a yeni veri yazmak Blobun yeni bir sürümünü oluşturur. Aşağıdaki diyagramda gösterildiği gibi, var olan tüm sürümler etkilenmez.

:::image type="content" source="media/versioning-overview/recreate-deleted-base-blob.png" alt-text="Silinmeden sonra sürümlü blob 'un yeniden oluşturulmasını gösteren diyagram":::

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

Blob sürüm oluşturmayı etkinleştirmeyi veya devre dışı bırakmayı öğrenmek için bkz. [BLOB sürüm oluşturmayı etkinleştirme veya devre dışı bırakma](versioning-enable.md).

Blob sürüm oluşturmayı devre dışı bırakmak, mevcut blob 'ları, sürümleri veya anlık görüntüleri silmez. Blob sürüm oluşturmayı kapattığınızda, mevcut tüm sürümler depolama hesabınızda erişilebilir kalır. Yeni sürüm daha sonra oluşturulmaz.

Depolama hesabında sürüm oluşturma devre dışı bırakıldıktan sonra bir blob oluşturulup değiştirildiyse, blob 'un üzerine yazılması yeni bir sürüm oluşturur. Güncelleştirilmiş blob artık geçerli sürüm değil ve sürüm KIMLIĞINE sahip değil. Blob 'un sonraki tüm güncelleştirmeleri önceki durumu kaydetmeden verilerinin üzerine yazar.

Sürüm oluşturma devre dışı bırakıldıktan sonra sürüm KIMLIĞINI kullanarak sürümleri okuyabilir veya silebilirsiniz. Ayrıca, sürüm oluşturma devre dışı bırakıldıktan sonra bir Blobun sürümlerini listeleyebilirsiniz.

Aşağıdaki diyagramda, sürüm oluşturma işlemi devre dışı bırakıldıktan sonra bir Blobun nasıl değiştirileceği gösterilmektedir ve sürümü bulunmayan bir blob oluşturulur. Blob ile ilişkili tüm mevcut sürümler korunur.

:::image type="content" source="media/versioning-overview/modify-base-blob-versioning-disabled.png" alt-text="Sürüm oluşturma devre dışı bırakıldıktan sonra değiştirilen temel blobu gösteren diyagram":::

## <a name="blob-versioning-and-soft-delete"></a>Blob sürümü oluşturma ve geçici silme

Blob sürümü oluşturma ve BLOB geçici silme, size en uygun veri koruması sağlamak için birlikte çalışır. Geçici silmeyi etkinleştirdiğinizde, Azure depolama 'nın geçici olarak silinen bir blobu ne kadar süreyle koruyacağını belirtirsiniz. Geçici olarak silinen blob sürümü sistemde kalır ve geçici silme Bekletme dönemi içinde silinebilir. Blob geçici silme hakkında daha fazla bilgi için bkz. [Azure depolama Blobları Için geçici silme](storage-blob-soft-delete.md).

### <a name="deleting-a-blob-or-version"></a>Blob veya sürümü silme

Geçici silme, blob sürümlerini silmek için ek koruma sağlar. Depolama hesabında hem sürüm oluşturma hem de geçici silme etkinse, bir blobu sildiğinizde Azure depolama, blob 'un durumunu silinmeden hemen önce kaydetmek için yeni bir sürüm oluşturur ve geçerli sürümü siler. Yeni sürüm geçici olarak silinmez ve geçici silme bekletme süresi sona erdiğinde kaldırılmaz.

Blob 'un önceki bir sürümünü sildiğinizde, sürüm geçici olarak silinir. Geçici olarak silinen sürüm, depolama hesabının geçici silme ayarlarında belirtilen Bekletme dönemi boyunca tutulur ve geçici silme saklama süresi sona erdiğinde kalıcı olarak silinir.

Bir Blobun önceki bir sürümünü kaldırmak için sürüm KIMLIĞINI belirterek açıkça silin.

Aşağıdaki diyagramda bir Blobu veya blob sürümünü sildiğinizde ne olacağı gösterilmektedir.

:::image type="content" source="media/versioning-overview/soft-delete-historical-version.png" alt-text="Geçici silme etkinken bir sürümü silmeyi gösteren diyagram":::

Bir depolama hesabında hem sürüm oluşturma hem de geçici silme etkinse, bir blob veya blob sürümü değiştirildiğinde veya silindiğinde, geçici olarak silinen anlık görüntü oluşturulmaz.

### <a name="restoring-a-soft-deleted-version"></a>Geçici olarak silinen bir sürümü geri yükleme

Geçici olarak silinen bir blob sürümünü, geçici silme bekletme süresi etkinken sürüm üzerinde geri alma [blobu](/rest/api/storageservices/undelete-blob) işlemini çağırarak geri yükleyebilirsiniz. **Geri alma blobu** işlemi, Blobun tüm geçici silinen sürümlerini geri yükler.

Geçici olarak silinen sürümleri geri alma **blobu** ile geri yükleme, herhangi bir sürümü geçerli sürüm olacak şekilde yükseltemez. Geçerli sürümü geri yüklemek için, önce tüm geçici silinen sürümleri geri yükleyin ve ardından blobu geri yüklemek için önceki bir sürümü kopyalamak üzere [blobu kopyalama](/rest/api/storageservices/copy-blob) işlemini kullanın.

Aşağıdaki diyagramda, **silme blobu** işlemiyle, geçici olarak silinen blob sürümlerinin nasıl geri yükleneceği ve BLOB 'un geçerli sürümünün **BLOB kopyalama** işlemiyle nasıl geri yükleneceği gösterilmektedir.

:::image type="content" source="media/versioning-overview/undelete-version.png" alt-text="Geçici olarak silinen sürümlerin nasıl geri yükleneceğini gösteren diyagram":::

Geçici silme bekletme süresi geçtikten sonra, geçici olarak silinen blob sürümleri kalıcı olarak silinir.

## <a name="blob-versioning-and-blob-snapshots"></a>Blob sürümü oluşturma ve BLOB anlık görüntüleri

Blob anlık görüntüsü, belirli bir zamanda belirli bir noktada alınmış bir Blobun salt okuma kopyasıdır. Blob anlık görüntüleri ve BLOB sürümleri benzerdir, ancak siz veya uygulamanız tarafından el ile oluşturulan bir anlık görüntü, depolama hesabınız için blob sürümü oluşturma etkinken yazma veya silme işleminde otomatik olarak bir blob sürümü oluşturulur.

> [!IMPORTANT]
> Microsoft, blob sürüm oluşturmayı etkinleştirdikten sonra, blok bloblarının anlık görüntülerini almayı durdurmak için uygulamanızı da güncelleştirmenizi önerir. Depolama hesabınız için sürüm oluşturma etkinleştirilmişse, tüm blok blob güncelleştirmeleri ve silmeleri, sürümler tarafından yakalanır ve korunur. Blob sürümü oluşturma etkinse, anlık görüntü alma Blok Blobu verilerinize ek koruma sunmaz ve maliyetleri ve uygulama karmaşıklığını artırabilir.

### <a name="snapshot-a-blob-when-versioning-is-enabled"></a>Sürüm oluşturma etkinken bir Blobun anlık görüntü

Önerilmese de, aynı zamanda sürümlü bir Blobun anlık görüntüsünü alabilirsiniz. Sürüm oluşturmayı etkinleştirdiğinizde uygulamanızın blob görüntülerini almayı durduracak şekilde güncellenemez, uygulamanız hem anlık görüntüleri hem de sürümleri destekleyebilir.

Sürümlü bir Blobun anlık görüntüsünü aldığınızda, anlık görüntünün oluşturulduğu anda yeni bir sürüm oluşturulur. Bir anlık görüntü çekilirken yeni bir geçerli sürüm de oluşturulur.

Aşağıdaki diyagramda, sürümlü bir Blobun anlık görüntüsünü alırken ne olacağı gösterilmektedir. Diyagramda, sürüm KIMLIĞI 2 ve 3 olan blob sürümleri ve anlık görüntüler aynı verileri içerir.

:::image type="content" source="media/versioning-overview/snapshot-versioned-blob.png" alt-text="Sürümlü bir Blobun anlık görüntülerini gösteren diyagram":::

## <a name="authorize-operations-on-blob-versions"></a>Blob sürümlerinde yetkilendirme işlemleri

Aşağıdaki yaklaşımlardan birini kullanarak blob sürümlerine erişim yetkisi verebilirsiniz:

- Rol tabanlı erişim denetimi 'ni (RBAC) kullanarak Azure Active Directory (Azure AD) güvenlik sorumlusu için izin verin. Microsoft, üstün güvenlik ve kullanım kolaylığı için Azure AD 'nin kullanılmasını önerir. Azure AD 'yi blob işlemleriyle kullanma hakkında daha fazla bilgi için bkz. [Azure Active Directory kullanarak bloblara ve kuyruklara erişim yetkisi verme](../common/storage-auth-aad.md).
- Blob sürümlerine erişim yetkisi vermek için paylaşılan erişim imzasını (SAS) kullanarak. `bv`Belirli bir sürümdeki işlemler için BIR SAS belirteci oluşturmak üzere, bir blob sürümünü temsil eden imzalı kaynak türü için sürüm kimliğini belirtin. Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz. [paylaşılan erişim imzaları (SAS) kullanarak Azure depolama kaynaklarına sınırlı erişim verme](../common/storage-sas-overview.md).
- Paylaşılan anahtarla blob sürümlerine karşı işlemleri yetkilendirmek için hesap erişim anahtarlarını kullanarak. Daha fazla bilgi için bkz. [paylaşılan anahtarla yetkilendirme](/rest/api/storageservices/authorize-with-shared-key).

Blob sürümü oluşturma, verilerinizi yanlışlıkla veya kötü amaçlı olarak silinmeye karşı korumak için tasarlanmıştır. Korumayı iyileştirmek için bir blob sürümünün silinmesi özel izinler gerektirir. Aşağıdaki bölümlerde bir blob sürümünü silmek için gereken izinler açıklanır.

### <a name="rbac-action-to-delete-a-blob-version"></a>Blob sürümünü silmek için RBAC eylemi

Aşağıdaki tabloda, hangi RBAC eylemlerinin bir blob veya blob sürümünü silmenin desteklediği gösterilmektedir.

| Açıklama | Blob hizmeti işlemi | RBAC verileri eylemi gerekiyor | RBAC yerleşik rol desteği |
|----------------------------------------------|------------------------|---------------------------------------------------------------------------------------|-------------------------------|
| Blobun geçerli sürümü siliniyor | İkili Büyük Nesneyi Silme | **Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Bloblar/Sil** | Depolama Blobu veri Katılımcısı |
| Bir sürümü silme | İkili Büyük Nesneyi Silme | **Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Bloblar/deleteBlobVersion/Action** | Depolama Blobu veri sahibi |

### <a name="shared-access-signature-sas-parameters"></a>Paylaşılan erişim imzası (SAS) parametreleri

Blob sürümü için imzalanan kaynak `bv` . Daha fazla bilgi için bkz. [HIZMET SAS oluşturma](/rest/api/storageservices/create-service-sas) veya [Kullanıcı temsili SAS oluşturma](/rest/api/storageservices/create-user-delegation-sas).

Aşağıdaki tabloda bir blob sürümünü silmek için SAS üzerinde gereken izin gösterilmektedir.

| **İzin** | **URI simgesi** | **İzin verilen işlemler** |
|----------------|----------------|------------------------|
| Sil         | x              | Blob sürümünü silin. |

## <a name="about-the-preview"></a>Önizleme hakkında

Blob sürümü oluşturma, önizleme aşamasında aşağıdaki bölgelerde kullanılabilir:

- Orta Fransa
- Doğu Kanada
- Orta Kanada

Önizleme yalnızca üretim dışı kullanım için tasarlanmıştır.

Azure depolama REST API sürüm 2019-10-10 ve üzeri, blob sürümü oluşturmayı destekler.

### <a name="storage-account-support"></a>Depolama hesabı desteği

Blob sürümü oluşturma, aşağıdaki depolama hesabı türleri için kullanılabilir:

- Genel amaçlı v2 depolama hesapları
- BLOB depolama hesaplarını engelle
- Blob Storage hesapları

Depolama Hesabınız genel amaçlı bir v1 hesabıdır, genel amaçlı v2 hesabına yükseltmek için Azure portal kullanın. Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../common/storage-account-overview.md).

Azure Data Lake Storage 2. ile kullanım için etkinleştirilmiş hiyerarşik bir ad alanı olan depolama hesapları Şu anda desteklenmemektedir.

### <a name="register-for-the-preview"></a>Önizlemeye kaydolun

Blob sürüm oluşturma önizlemesine kaydolmak için, özelliği aboneliğinize kaydetme isteği göndermek üzere PowerShell veya Azure CLı kullanın. İsteğiniz onaylandıktan sonra, yeni veya mevcut genel amaçlı v2, BLOB depolama veya Premium Blok Blobu depolama hesaplarıyla blob sürümü oluşturmayı etkinleştirebilirsiniz.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

PowerShell 'e kaydolmak için [Get-AzProviderFeature](/powershell/module/az.resources/get-azproviderfeature) komutunu çağırın.

```powershell
# Register for blob versioning (preview)
Register-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName Versioning

# Refresh the Azure Storage provider namespace
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı ile kaydolmak için [az Feature Register](/cli/azure/feature#az-feature-register) komutunu çağırın.

```azurecli
az feature register --namespace Microsoft.Storage \
    --name Versioning
```

---

### <a name="check-the-status-of-your-registration"></a>Kaydlarınızın durumunu denetleyin

Kaydlarınızın durumunu denetlemek için PowerShell veya Azure CLı kullanın.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Kayıt durumunuzu PowerShell 'e göre denetlemek için [Get-AzProviderFeature](/powershell/module/az.resources/get-azproviderfeature) komutunu çağırın.

```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName Versioning
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı ile kaydlarınızın durumunu denetlemek için [az Feature](/cli/azure/feature#az-feature-show) komutunu çağırın.

```azurecli
az feature show --namespace Microsoft.Storage \
    --name Versioning
```

---

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Blob sürümü oluşturma özelliğinin etkinleştirilmesi hesabınıza ek veri depolama ücretleri oluşmasına neden olabilir. Uygulamanızı tasarlarken, maliyetleri en aza indirmek için bu ücretlerin nasıl tahakkuk edebileceğini bilmeniz önemlidir.

Blob anlık görüntüleri gibi BLOB sürümleri, etkin verilerle aynı hızda faturalandırılır. Bir sürüm, temel Blobun blok veya sayfalarını paylaşıyorsa, yalnızca sürüm ve temel blob arasında paylaşılmayan ek bloklar veya sayfalar için ödeme yaparsınız.

> [!NOTE]
> Genellikle üzerine yazılacak veriler için sürüm oluşturmanın etkinleştirilmesi, depolama kapasitesi ücretlerine yol açabilir ve listeleme işlemleri sırasında gecikme süresini artırabilir. Bu kaygıları azaltmak için, sık sık oluşan verileri, sürüm oluşturma devre dışı olan ayrı bir depolama hesabında depolayın.

### <a name="important-billing-considerations"></a>Önemli faturalandırma konuları

Blob sürüm oluşturmayı etkinleştirirken aşağıdaki noktaları dikkate aldığınızdan emin olun:

- Depolama hesabınız, blob 'ta veya blob 'un önceki bir sürümünde olup olmadıkları bağımsız olarak benzersiz bloklar veya sayfalar için ücretler doğurur. Hesabınız, temel aldıkları blobu güncelleştirene kadar bir blob ile ilişkili sürümler için ek ücret uygulamaz. Blobu güncelleştirdikten sonra önceki sürümlerden de ayrılmış olur. Bu durumda, her blob veya sürümdeki benzersiz bloklar veya sayfalar için ücretlendirilirsiniz.
- Blok Blobu içindeki bir bloğu değiştirdiğinizde, bu blok daha sonra benzersiz bir blok olarak ücretlendirilir. Bu, blok aynı blok KIMLIĞINE ve sürümde aynı veriye sahip olsa bile geçerlidir. Blok yeniden gerçekleştirildikten sonra, herhangi bir sürümdeki karşılığından bağımsız kalır ve verileri için ücretlendirilirsiniz. Aynı verilerle güncelleştirilmiş bir sayfa blobunun bir sayfası için de aynı değer geçerlidir.
- Blob depolamada, iki Blobun aynı verileri içerip içermediğini belirleme yolu yoktur. Karşıya yüklenen ve yürütülen her bir blok, aynı verilere ve aynı blok KIMLIĞINE sahip olsa bile benzersiz olarak değerlendirilir. Ücretler benzersiz bloklar için tahakkuk ettiğinden, sürüm oluşturma etkinken bir blob 'u güncelleştirmek, ek benzersiz bloklara ve ek ücretler oluşmasına neden olur.
- Blob sürümü oluşturma etkinleştirildiğinde, blok Blobları üzerinde güncelleştirme işlemlerini, en az olası blok sayısını güncelleştirecek şekilde tasarlayın. Bloklar üzerinde ayrıntılı denetime izin veren yazma işlemleri, [PUT bloğu](/rest/api/storageservices/put-block) ve [yerleştirme engellenenler listesi](/rest/api/storageservices/put-block-list)' dir. Diğer taraftan [BLOB 'U koy](/rest/api/storageservices/put-blob) işlemi, bir Blobun tüm içeriğini değiştirir ve bu nedenle ek ücretlere neden olabilir.

### <a name="versioning-billing-scenarios"></a>Sürüm faturalama senaryoları

Aşağıdaki senaryolarda, giderlerin bir Blok Blobu ve sürümleri için nasıl tahakkuk olduğu gösterilmektedir.

#### <a name="scenario-1"></a>1\. Senaryo

Senaryo 1 ' de, blob 'un önceki bir sürümü vardır. Sürüm oluşturulduktan sonra blob güncelleştirilmedi, bu nedenle ücretler yalnızca 1, 2 ve 3 benzersiz blokları için ücretlendirilir.

![Azure depolama kaynakları](./media/versioning-overview/versions-billing-scenario-1.png)

#### <a name="scenario-2"></a>2\. Senaryo

Senaryo 2 ' de, blobdaki bir blok (diyagramdaki blok 3) güncelleştirildi. Güncelleştirilmiş blok aynı verileri ve aynı KIMLIĞI içerse de, önceki sürümde blok 3 ile aynı değildir. Sonuç olarak, hesap dört blok için ücretlendirilir.

![Azure depolama kaynakları](./media/versioning-overview/versions-billing-scenario-2.png)

#### <a name="scenario-3"></a>3\. Senaryo

Senaryo 3 ' te blob güncelleştirildi, ancak sürüm değil. Blok 3, temel Blobun içindeki blok 4 ile değiştirilmiştir, ancak önceki sürüm hala blok 3 ' ü yansıtır. Sonuç olarak, hesap dört blok için ücretlendirilir.

![Azure depolama kaynakları](./media/versioning-overview/versions-billing-scenario-3.png)

#### <a name="scenario-4"></a>4\. Senaryo

Senaryo 4 ' te, temel blob tamamen güncelleştirilmiştir ve özgün bloklarından hiçbirini içermez. Sonuç olarak, hesap, &mdash; temel Blobun dört benzersiz blok ve önceki sürümde dört adet ücretlendirilir. Bu senaryo, blob 'u koy işlemi ile bir blob 'a yazıyorsanız, bu durum temel Blobun tüm içeriğinin yerini almıştır.

![Azure depolama kaynakları](./media/versioning-overview/versions-billing-scenario-4.png)

## <a name="see-also"></a>Ayrıca bkz.

- [Blob sürümü oluşturmayı etkinleştirme](versioning-enable.md)
- [Blob 'un anlık görüntüsünü oluşturma](/rest/api/storageservices/creating-a-snapshot-of-a-blob)
- [Azure depolama Blobları için geçici silme](storage-blob-soft-delete.md)
