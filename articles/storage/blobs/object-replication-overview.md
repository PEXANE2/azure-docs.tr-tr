---
title: Nesne çoğaltmasına genel bakış
titleSuffix: Azure Storage
description: Nesne çoğaltma, blok bloblarını bir kaynak depolama hesabı ve hedef hesap arasında zaman uyumsuz olarak kopyalar. Okuma isteklerindeki gecikme süresini en aza indirmek için, işlem iş yüklerinin verimliliğini artırmak, veri dağıtımını iyileştirmek ve maliyetleri en aza indirmek için nesne çoğaltmasını kullanın.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 09/08/2020
ms.author: tamram
ms.subservice: blobs
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 0d03b2708bfd4aac2565b303ddce44f50be65ef9
ms.sourcegitcommit: f845ca2f4b626ef9db73b88ca71279ac80538559
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89612338"
---
# <a name="object-replication-for-block-blobs"></a>Blok Blobları için nesne çoğaltma

Nesne çoğaltma, blok bloblarını bir kaynak depolama hesabı ve hedef hesap arasında zaman uyumsuz olarak kopyalar. Nesne çoğaltma tarafından desteklenen bazı senaryolar şunlardır:

- **En aza indirme gecikmesi.** Nesne çoğaltma, istemcilerin daha yakın bir fiziksel yakınlık alanında bulunan bir bölgeden veri tüketmesini sağlayarak okuma istekleri için gecikme süresini azaltabilir.
- **İşlem iş yükleri için verimliliği artırın.** İşlem iş yükleri, nesne çoğaltma ile farklı bölgelerde aynı blok blob kümelerini işleyebilir.
- **Veri dağıtımını iyileştirme.** Verileri tek bir konumda işleyebilir veya analiz edebilir ve sonra yalnızca sonuçları ek bölgelere çoğaltabilirsiniz.
- **Maliyetleri iyileştirme.** Verileriniz çoğaltıldıktan sonra, yaşam döngüsü yönetimi ilkelerini kullanarak bunu arşiv katmanına taşıyarak maliyetleri azaltabilirsiniz.

Aşağıdaki diyagramda, nesne çoğaltmanın, blok bloblarını bir bölgedeki kaynak depolama hesabından iki farklı bölgedeki hedef hesaplara nasıl çoğaltacağını gösterir.

:::image type="content" source="media/object-replication-overview/object-replication-diagram.svg" alt-text="Nesne çoğaltmasının nasıl çalıştığını gösteren diyagram":::

Nesne çoğaltmasını yapılandırma hakkında bilgi edinmek için bkz. [nesne çoğaltmasını yapılandırma](object-replication-configure.md).

[!INCLUDE [storage-data-lake-gen2-support](../../../includes/storage-data-lake-gen2-support.md)]

## <a name="prerequisites-for-object-replication"></a>Nesne çoğaltma için önkoşullar

Nesne çoğaltma, aşağıdaki Azure depolama özelliklerinin de etkinleştirilmesini gerektirir:

- [Değişiklik akışı](storage-blob-change-feed.md): kaynak hesapta etkin olmalıdır. Değişiklik akışını etkinleştirmeyi öğrenmek için bkz. [değişiklik akışını etkinleştirme ve devre dışı bırakma](storage-blob-change-feed.md#enable-and-disable-the-change-feed).
- [BLOB sürümü oluşturma](versioning-overview.md): hem kaynak hem de hedef hesaplarda etkinleştirilmelidir. Sürüm oluşturmayı nasıl etkinleştireceğinizi öğrenmek için bkz. [BLOB sürüm oluşturmayı etkinleştirme ve yönetme](versioning-enable.md).

Değişiklik akışı ve BLOB sürümü oluşturma özelliğinin etkinleştirilmesi ek ücret ödemeniz gerekebilir. Daha fazla ayrıntı için [Azure Depolama fiyatlandırması sayfasına](https://azure.microsoft.com/pricing/details/storage/)bakın.

## <a name="object-replication-policies-and-rules"></a>Nesne çoğaltma ilkeleri ve kuralları

Nesne çoğaltmasını yapılandırdığınızda, kaynak depolama hesabı ve hedef hesap belirten bir çoğaltma ilkesi oluşturursunuz. Çoğaltma İlkesi, kaynak kapsayıcısını ve hedef kapsayıcıyı belirten bir veya daha fazla kural içerir ve kaynak kapsayıcısındaki hangi blok bloblarının çoğaltılacağı anlamına gelir.

Nesne çoğaltmasını yapılandırdıktan sonra Azure depolama, kaynak hesabın değişiklik akışını düzenli aralıklarla denetler ve yazma veya silme işlemlerini zaman uyumsuz olarak hedef hesaba çoğaltır. Çoğaltma gecikmesi, çoğaltılan blok Blobun boyutuna bağlıdır.

> [!IMPORTANT]
> Blok Blobu verileri zaman uyumsuz olarak çoğaltıldığından, kaynak hesap ve hedef hesap hemen eşitlenmiş durumda değildir. Şu anda, hedef hesaba veri çoğaltmak için ne kadar süreceğine ilişkin bir SLA yoktur.

### <a name="replication-policies"></a>Çoğaltma ilkeleri

Nesne çoğaltmasını yapılandırdığınızda, Azure depolama kaynak sağlayıcısı aracılığıyla hem kaynak hesapta hem de hedef hesapta bir çoğaltma ilkesi oluşturulur. Çoğaltma İlkesi bir ilke KIMLIĞI tarafından tanımlanır. Çoğaltmanın gerçekleşmesi için kaynak ve hedef hesapların ilke KIMLIĞI aynı olmalıdır.

Bir depolama hesabı, en fazla iki hedef hesap için kaynak hesap olarak görev yapabilir. Kaynak ve hedef hesaplar aynı bölgede veya farklı bölgelerde olabilir. Farklı aboneliklerde ve farklı Azure Active Directory (Azure AD) kiracılarında da bulunabilir. Her kaynak hesap/hedef hesap çifti için yalnızca bir çoğaltma ilkesi oluşturulabilir.

### <a name="replication-rules"></a>Çoğaltma kuralları

Çoğaltma kuralları, Azure depolama 'nın blob 'ları bir kaynak kapsayıcısından hedef kapsayıcıya nasıl çoğaltacağınızı belirtir. Her çoğaltma ilkesi için en fazla 10 çoğaltma kuralı belirtebilirsiniz. Her çoğaltma kuralı tek bir kaynak ve hedef kapsayıcı tanımlar ve her kaynak ve hedef kapsayıcı yalnızca bir kuralda kullanılabilir.

Bir çoğaltma kuralı oluşturduğunuzda, varsayılan olarak yalnızca kaynak kapsayıcıya daha sonra eklenen yeni blok Blobları kopyalanır. Hem yeni hem de var olan blok bloblarının kopyalanmasını belirtebilir veya belirli bir zamandan itibaren oluşturulan blok bloblarını kopyalayan özel bir kopya kapsamı tanımlayabilirsiniz.

Ayrıca, blok bloblarını ön eke göre filtrelemek için bir çoğaltma kuralının bir parçası olarak bir veya daha fazla filtre belirtebilirsiniz. Bir ön ek belirttiğinizde, yalnızca kaynak kapsayıcıda bu önekle eşleşen Bloblar hedef kapsayıcıya kopyalanacaktır.

Kaynak ve hedef kapsayıcıların her ikisi de bir kuralda belirtmeleri için mevcut olmalıdır. Çoğaltma ilkesini oluşturduktan sonra hedef kapsayıcı salt okunur duruma gelir. Hedef kapsayıcıya yazma girişimleri hata kodu 409 (Çakışma) vererek başarısız olur. Ancak, [BLOB katmanını ayarla](/rest/api/storageservices/set-blob-tier) işlemini arşiv katmanına taşımak için hedef kapsayıcıdaki bir blob üzerinde çağırabilirsiniz. Arşiv katmanı hakkında daha fazla bilgi için bkz. [Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md#archive-access-tier).

## <a name="billing"></a>Faturalandırma 

Nesne çoğaltma, kaynak ve hedef hesaplara göre okuma ve yazma işlemlerinde ek maliyetler ve kaynak hesaptan hedef hesaba veri çoğaltma ve değişiklik akışını işlemek için okuma ücretleri için çıkış ücretlerine yol açar.

## <a name="next-steps"></a>Sonraki adımlar

- [Nesne çoğaltmayı yapılandırma](object-replication-configure.md)
- [Azure Blob depolamada akış desteğini değiştirme](storage-blob-change-feed.md)
- [Blob sürüm oluşturmayı etkinleştirme ve yönetme](versioning-enable.md)
