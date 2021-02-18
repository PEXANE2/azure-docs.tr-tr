---
title: Azure Data Lake Storage 2. için erişim denetimi modeli | Microsoft Docs
description: Hiyerarşik bir ad alanı olan hesaplarda kapsayıcı, dizin ve dosya düzeyi erişimini nasıl yapılandıracağınızı öğrenin.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: normesta
ms.openlocfilehash: e5f62456b85791bad5bb66f3abf67e523558d76e
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "100650397"
---
# <a name="access-control-model-in-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. 'de erişim denetimi modeli

Data Lake Storage 2. aşağıdaki yetkilendirme mekanizmalarını destekler:

- Paylaşılan anahtar yetkilendirmesi
- Paylaşılan erişim imzası (SAS) yetkilendirmesi
- Rol tabanlı erişim denetimi (Azure RBAC)
- Erişim denetim listeleri (ACL)

[Paylaşılan anahtar ve SAS yetkilendirmesi](#shared-key-and-shared-access-signature-sas-authorization) , Azure Active Directory (Azure AD) içinde bir kimliğe sahip olmalarını gerektirmeden bir kullanıcıya (veya uygulamaya) erişim verir. Bu iki kimlik doğrulama biçimi ile Azure RBAC ve ACL 'Lerin etkisi yoktur.

Azure RBAC ve ACL 'nin her ikisi de kullanıcının (veya uygulamanın) Azure AD 'de bir kimliğe sahip olmasını gerektirir.  Azure RBAC, bir depolama hesabındaki **Tüm** verilere okuma veya yazma erişimi gibi depolama hesabı verilerine "kaba Grey" erişimi vermenizi sağlar, ancak ACL 'ler belirli bir dizin veya dosyaya yazma erişimi gibi "hassas" erişim sağlamanıza olanak tanır.  

Bu makale, Azure RBAC ve ACL 'Lere odaklanılır ve depolama hesabı kaynakları için yetkilendirme kararları almak üzere sistemin bunları bir araya getirme şeklini alır.

<a id="role-based-access-control"></a>

## <a name="role-based-access-control-azure-rbac"></a>Rol tabanlı erişim denetimi (Azure RBAC)

Azure RBAC, [güvenlik sorumlularına](../../role-based-access-control/overview.md#security-principal)izin kümelerini uygulamak için rol atamaları kullanır. Güvenlik sorumlusu, Azure Active Directory (AD) ' de tanımlanan bir Kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimliği temsil eden bir nesnedir. Bir izin kümesi, bir depolama hesabındaki **Tüm** verilere veya bir kapsayıcıdaki **Tüm** verilere okuma veya yazma erişimi gibi bir güvenlik sorumlusuna "kaba bir" erişim düzeyi verebilir. 

Aşağıdaki roller, bir güvenlik sorumlusunun bir depolama hesabındaki verilere erişmesine izin verir.

|Rol|Açıklama|
|--|--|
| [Depolama Blob Verileri Sahibi](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner) | BLOB depolama kapsayıcıları ve verilerine tam erişim. Bu erişim, güvenlik sorumlusunun sahibi bir öğe ayarlamaya ve tüm öğelerin ACL 'Lerini değiştirmesine izin verir. |
| [Depolama Blob Verileri Katkıda Bulunanı](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner) | BLOB depolama kapsayıcıları ve Blobları için okuma, yazma ve silme erişimi. Bu erişim, güvenlik sorumlusunun bir öğenin sahipliğini ayarlamaya izin vermez, ancak güvenlik sorumlusu tarafından sahip olunan öğelerin ACL 'sini değiştirebilir. |
| [Depolama Blob Verileri Okuyucusu](../../role-based-access-control/built-in-roles.md#storage-blob-data-reader) | BLOB depolama kapsayıcıları ve bloblarını okuyun ve listeleyin. |

[Sahip](../../role-based-access-control/built-in-roles.md#owner), [katkıda bulunan](../../role-based-access-control/built-in-roles.md#contributor), [okuyucu](../../role-based-access-control/built-in-roles.md#reader)ve [depolama hesabı katılımcısı](../../role-based-access-control/built-in-roles.md#storage-account-contributor) gibi roller, bir güvenlik sorumlusunun bir depolama hesabını yönetmesine izin verir, ancak bu hesap içindeki verilere erişim sağlamaz. Ancak, bu roller ( **okuyucu** hariç), verilere erişmek için çeşitli istemci araçlarında kullanılabilecek depolama anahtarlarına erişim elde edebilir.

## <a name="access-control-lists-acls"></a>Erişim denetim listeleri (ACL’ler)

ACL 'Ler, dizinlere ve dosyalara "daha hassas" bir erişim düzeyi uygulamanıza olanak sağlar. *ACL* , bir dizi *ACL girişi* içeren bir izin yapısıdır. Her ACL girişi güvenlik sorumlusunu bir erişim düzeyiyle ilişkilendirir.  Daha fazla bilgi için, bkz. [Azure Data Lake Storage 2. erişim denetim listeleri (ACL 'ler)](data-lake-storage-access-control.md).

## <a name="how-permissions-are-evaluated"></a>İzinler nasıl değerlendirilir

Güvenlik sorumlusu tabanlı yetkilendirme sırasında izinler aşağıdaki sırayla değerlendirilir.

: bunlardan biri: &nbsp; &nbsp; Azure rol atamaları önce değerlendirilir ve tüm ACL atamalarından önceliklidir.

: iki: &nbsp; &nbsp; işlem Azure rol ataması temelinde tam Yetkilendirirse, ACL 'ler hiç değerlendirilmez.

: üç: &nbsp; &nbsp; işlem tam olarak yetkilendirilmemişse, ACL 'ler değerlendirilir.

> [!div class="mx-imgBorder"]
> ![Data Lake depolama izin akışı](./media/control-access-permissions-data-lake-storage/data-lake-storage-permissions-flow.png)

Erişim izinlerinin sistem tarafından değerlendirilme biçimi nedeniyle, bir rol ataması tarafından zaten verilmiş olan erişimi **kısıtlamak** IÇIN bir **ACL kullanamazsınız.** Bunun nedeni, sistemin önce Azure rol atamalarını değerlendirdiği ve atamanın yeterli erişim izni verdiğini belirten ACL 'Ler yoksayılır. 

Aşağıdaki diyagramda, üç yaygın işlem için izin akışı gösterilmektedir: dizin içeriğini listeleme, dosya okuma ve dosya yazma.

> [!div class="mx-imgBorder"]
> ![Data Lake depolama izin akışı örneği](./media/control-access-permissions-data-lake-storage/data-lake-storage-permissions-example.png)

## <a name="permissions-table-combining-azure-rbac-and-acl"></a>İzinler tablosu: Azure RBAC ve ACL 'yi birleştirme

Aşağıdaki tabloda, bir güvenlik sorumlusunun **işlem** sütununda listelenen işlemleri gerçekleştirebilmesi için Azure ROLLERINI ve ACL girdilerini nasıl birleştirebileceğiniz gösterilmektedir. Bu tablo, kurgusal bir dizin hiyerarşisinin her düzeyini temsil eden bir sütun gösterir. Kapsayıcının kök dizini () için bir sütun `/` , **Oregon** adlı bir alt dizin, **İstanbul** adlı MARI dizininin bir alt dizini ve **Data.txt** adlı Portland dizinindeki bir metin dosyası. Bu sütunlarda görünen, izin vermek için gereken ACL girişinin [kısa biçimli](data-lake-storage-access-control.md#short-forms-for-permissions) temsilleridir. İşlemi gerçekleştirmek **için BIR ACL** girişi gerekmiyorsa yok (_uygulanamaz_) sütununda görüntülenir.

|    İşlem             | Atanan Azure rolü               |    /        | 'Daki     | Portland | Data.txt |             
|--------------------------|----------------------------------|-------------|-------------|-----------|----------|
| Data.txt oku            |   Depolama Blob Verileri Sahibi        | Yok      | Yok      | Yok       | Yok    |  
|                          |   Depolama Blob Verileri Katkıda Bulunanı  | Yok      | Yok      | Yok       | Yok    |
|                          |   Depolama Blob Verileri Okuyucusu       | Yok      | Yok      | Yok       | Yok    |
|                          |   Yok                           | `--X`    | `--X`    | `--X`     | `R--`  |
| Data.txt sonuna ekle       |   Depolama Blob Verileri Sahibi        | Yok      | Yok      | Yok       | Yok    |
|                          |   Depolama Blob Verileri Katkıda Bulunanı  | Yok      | Yok      | Yok       | Yok    |
|                          |   Depolama Blob Verileri Okuyucusu       | `--X`    | `--X`    | `--X`     | `-W-`  |
|                          |   Yok                           | `--X`    | `--X`    | `--X`     | `RW-`  |
| Data.txt Sil          |   Depolama Blob Verileri Sahibi        | Yok      | Yok      | Yok       | Yok    |
|                          |   Depolama Blob Verileri Katkıda Bulunanı  | Yok      | Yok      | Yok       | Yok    |
|                          |   Depolama Blob Verileri Okuyucusu       | `--X`    | `--X`    | `-WX`     | Yok    |
|                          |   Yok                           | `--X`    | `--X`    | `-WX`     | Yok    |
| Data.txt oluştur          |   Depolama Blob Verileri Sahibi        | Yok      | Yok      | Yok       | Yok    |
|                          |   Depolama Blob Verileri Katkıda Bulunanı  | Yok      | Yok      | Yok       | Yok    |
|                          |   Depolama Blob Verileri Okuyucusu       | `--X`    | `--X`    | `-WX`     | Yok    |
|                          |   Yok                           | `--X`    | `--X`    | `-WX`     | Yok    |
| Listele                   |   Depolama Blob Verileri Sahibi        | Yok      | Yok      | Yok       | Yok    |
|                          |   Depolama Blob Verileri Katkıda Bulunanı  | Yok      | Yok      | Yok       | Yok    |
|                          |   Depolama Blob Verileri Okuyucusu       | Yok      | Yok      | Yok       | Yok    |
|                          |   Yok                           | `R-X`    | Yok      | Yok       | Yok    |
| Liste/Oregon/            |   Depolama Blob Verileri Sahibi        | Yok      | Yok      | Yok       | Yok    |
|                          |   Depolama Blob Verileri Katkıda Bulunanı  | Yok      | Yok      | Yok       | Yok    |
|                          |   Depolama Blob Verileri Okuyucusu       | Yok      | Yok      | Yok       | Yok    |
|                          |   Yok                           | `--X`    | `R-X`    | Yok       | Yok    |
| Liste/Oregon/Portland/   |   Depolama Blob Verileri Sahibi        | Yok      | Yok      | Yok       | Yok    |
|                          |   Depolama Blob Verileri Katkıda Bulunanı  | Yok      | Yok      | Yok       | Yok    |
|                          |   Depolama Blob Verileri Okuyucusu       | Yok      | Yok      | Yok       | Yok    |
|                          |   Yok                           | `--X`    | `--X`    | `R-X`     | Yok    |


> [!NOTE] 
> Azure Depolama Gezgini bir kapsayıcının içeriğini görüntülemek için güvenlik sorumlularının [Azure ad kullanarak Depolama Gezgini oturum açması](../../vs-azure-tools-storage-manage-with-storage-explorer.md?tabs=windows#add-a-resource-via-azure-ad)gerekir ve (en azından) bir kapsayıcının kök klasörüne () okuma erişimi (R--) vardır `\` . Bu izin düzeyi, kök klasörün içeriğini listeleme özelliği sunar. Kök klasörün içeriğinin görünür olmasını istemiyorsanız, bunları [okuyucu](../../role-based-access-control/built-in-roles.md#reader) rolüne atayabilirsiniz. Bu rolle, bu rol, bir hesaptaki kapsayıcıları listeleyebilir, ancak kapsayıcı içerikleriyle kalmaz. Daha sonra ACL 'Leri kullanarak belirli dizinlere ve dosyalara erişim izni verebilirsiniz.   

## <a name="security-groups"></a>Güvenlik grupları

[!INCLUDE [Security groups](../../../includes/azure-storage-data-lake-groups.md)]

## <a name="limits-on-azure-role-assignments-and-acl-entries"></a>Azure rol atamaları ve ACL girdileri sınırları

Grupları kullanarak, abonelik başına maksimum rol ataması sayısını ve dosya veya dizin başına en fazla ACL girişi sayısını aşmanız daha düşüktür. Aşağıdaki tabloda bu sınırlar açıklanmaktadır.

[!INCLUDE [Security groups](../../../includes/azure-storage-data-lake-rbac-acl-limits.md)] 

## <a name="shared-key-and-shared-access-signature-sas-authorization"></a>Paylaşılan anahtar ve paylaşılan erişim Imzası (SAS) yetkilendirmesi

Azure Data Lake Storage 2. Ayrıca, kimlik doğrulaması için [paylaşılan anahtar](/rest/api/storageservices/authorize-with-shared-key) ve [SAS](../common/storage-sas-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) yöntemlerini destekler. Bu kimlik doğrulama yöntemlerinin bir özelliği, çağıran ile hiçbir kimliğin ilişkilendirilmediği ve bu nedenle güvenlik sorumlusu izin tabanlı yetkilendirmenin gerçekleştirilemesinden oluşur.

Paylaşılan anahtar söz konusu olduğunda, çağıran, veriler dahil olmak üzere tüm kaynaklardaki tüm işlemlere tam erişim, sahibi ayarlama ve ACL 'Leri değiştirme gibi ' Süper Kullanıcı ' erişimini etkili bir şekilde alır.

SAS belirteçleri, belirtecin bir parçası olarak izin verilen izinleri içerir. SAS belirtecine dahil edilen izinler, tüm yetkilendirme kararlarında etkili bir şekilde uygulanır, ancak başka bir ACL denetimi gerçekleştirilmez.

## <a name="next-steps"></a>Sonraki adımlar

Erişim denetim listeleri hakkında daha fazla bilgi için  [Azure Data Lake Storage 2. Içindeki erişim denetim listeleri (ACL 'ler)](data-lake-storage-access-control.md)konusuna bakın.