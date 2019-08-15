---
title: Arşiv katmanından blob verilerini yeniden doldurma
description: Verilere erişebilmek için bloblarınızı arşiv depolamadan yeniden doldurma.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/07/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.reviewer: hux
ms.openlocfilehash: 2e7d56a1461dfd89a7309288aadb0ba245d0f885
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68958219"
---
# <a name="rehydrate-blob-data-from-the-archive-tier"></a>Arşiv katmanından blob verilerini yeniden doldurma

Blob, arşiv erişim katmanında olduğunda çevrimdışı olarak kabul edilir ve okunamaz veya değiştirilemez. Blob meta verileri çevrimiçi ve kullanılabilir durumda kalır ve bu da blobu ve özelliklerini listelemenize olanak sağlar. Blob verilerini okuma ve değiştirme yalnızca sık erişimli veya seyrek erişimli gibi çevrimiçi katmanlarla kullanılabilir. Arşiv erişim katmanında depolanan verileri almak ve erişmek için iki seçenek vardır.

1. Arşivlenmiş bir Blobun [çevrimiçi bir katmana yeniden](#rehydrate-an-archived-blob-to-an-online-tier) doldurma-arşivlenmiş bir blob 'U, [BLOB katmanını ayarla](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) işlemini kullanarak katmanını değiştirerek sık erişimli veya seyrek erişimli olarak yeniden doldurma.
2. [Arşivlenmiş bir blobu çevrimiçi katmana kopyalama](#copy-an-archived-blob-to-an-online-tier) - [BLOB kopyalama](https://docs.microsoft.com/rest/api/storageservices/copy-blob) işlemini kullanarak arşivlenmiş bir Blobun yeni bir kopyasını oluşturun. Farklı bir blob adı ve sık erişimli veya seyrek erişimli hedef katmanı belirtin.

 Katmanlar hakkında daha fazla bilgi için bkz. [Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md).

## <a name="rehydrate-an-archived-blob-to-an-online-tier"></a>Arşivlenmiş bir Blobun çevrimiçi bir katmana yeniden doldurma

[!INCLUDE [storage-blob-rehydration](../../../includes/storage-blob-rehydrate-include.md)]

## <a name="copy-an-archived-blob-to-an-online-tier"></a>Arşivlenmiş bir blobu çevrimiçi katmana kopyalama

Bir blobu yeniden doldurma istemiyorsanız, bir [BLOB kopyalama](https://docs.microsoft.com/rest/api/storageservices/copy-blob) işlemi seçebilirsiniz. Sık erişimli veya seyrek erişimli katmanda yeni blob üzerinde çalışırken özgün Blobun arşiv içinde değiştirilmemiş olarak kalır. Kopyalama işlemini kullanırken, isteğe bağlı *x-MS-rehibulunan öncelik* özelliğini standart veya yüksek (Önizleme) olarak ayarlayabilirsiniz.

Arşiv blob 'ları yalnızca çevrimiçi hedef katmanlara kopyalanabilir. Arşiv blobunun başka bir arşiv blobuna kopyalanması desteklenmez.

Bir Blobun arşivden kopyalanması zaman alır. Arka planda **kopyalama blobu** işlemi, hedef katmanda yeni bir çevrimiçi blob oluşturmak için Arşiv kaynak blobundan geçici olarak yeniden doldurma işlemidir. Bu yeni blob, arşivden geçici olarak yeniden doldurma tamamlanana ve veriler yeni bloba yazıldıktan kadar kullanılamaz.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Blob 'ların arşiv dışına, sık veya seyrek erişimli katmanlara yeniden doldurma işlemi, okuma işlemleri ve veri alımı olarak ücretlendirilir. Yüksek öncelikli (Önizleme) kullanımı, standart önceliğe kıyasla daha yüksek işlem ve veri alımı maliyetlerine sahiptir. Yüksek öncelikli yeniden doldurma işlemi faturanızda ayrı bir satır öğesi olarak gösterilir. Birkaç gigabaytlık bir arşiv blobu döndürmek için yüksek öncelikli bir istek 5 saatten fazla sürerse, yüksek öncelikli alma ücreti ücretlendirilmezsiniz. Ancak standart alma ücretleri de geçerlidir.

Blobların arşivden sık veya seyrek erişimli katmanlara kopyalanması, okuma işlemleri ve veri alımı olarak ücretlendirilir. Yeni kopyanın oluşturulması için bir yazma işlemi ücretlendirilir. Kaynak blobu arşiv katmanında değiştirilmemiş olarak kaldığı için, erken silme ücretleri bir çevrimiçi bloba kopyalanırken uygulanmaz. Yüksek öncelikli ücretler geçerlidir.

Arşiv katmanındaki Bloblar en az 180 gün önce depolanmalıdır. Arşivlenmiş blob 'ları 180 günden önce silmek veya yeniden doldurma işlemi erken silme ücretleri oluşturacak.

> [!NOTE]
> Blok Blobları ve veri yeniden doldurma fiyatlandırması hakkında daha fazla bilgi için bkz. [Azure Storage fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/blobs/). Giden veri aktarımı ücretleri hakkında daha fazla bilgi için bkz. [veri aktarımları fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="next-steps"></a>Sonraki Adımlar

* [BLOB depolama katmanları hakkında bilgi edinin](storage-blob-storage-tiers.md)
* [Blob depolama ve GPv2 hesaplarında bölgeye göre sık erişimli, seyrek erişimli ve arşiv fiyatlarını denetleme](https://azure.microsoft.com/pricing/details/storage/)
* [Azure Blob depolama yaşam döngüsünü yönetme](storage-lifecycle-management-concepts.md)
* [Veri aktarımı fiyatlandırmasını denetleme](https://azure.microsoft.com/pricing/details/data-transfers/)
