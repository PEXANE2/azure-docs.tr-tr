---
title: Azure Data Box sınırları | Microsoft Docs
description: Microsoft Azure Data Box bileşenleri ve bağlantıları için sistem sınırlarını ve önerilen boyutları açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 07/10/2020
ms.author: alkohli
ms.openlocfilehash: 7d699fc47fa0a0cb57d103ff42ff17bdc0f3a82b
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86202678"
---
# <a name="azure-data-box-limits"></a>Azure Data Box sınırları

Microsoft Azure Data Box dağıtıp işletirken bu limitleri göz önünde bulundurun. Aşağıdaki tabloda Data Box için bu sınırlar açıklanmaktadır.

## <a name="data-box-service-limits"></a>Hizmet sınırlarını Data Box

[!INCLUDE [data-box-service-limits](../../includes/data-box-service-limits.md)]

## <a name="data-box-limits"></a>Data Box sınırları

- Data Box, hem içeri hem de dışarı aktarma için en fazla 500.000.000 dosya saklayabilir.
- Data Box buluttaki en fazla 512 kapsayıcıyı veya paylaşımı destekler. Kullanıcı paylaşımı içindeki en üst düzey dizinler, bulutta kapsayıcılar veya Azure dosya paylaşımları haline gelir. 
- Data Box kullanım kapasitesi, ReFS meta veri alanı tüketimine göre 80 TB 'den az olabilir.

## <a name="azure-storage-limits"></a>Azure depolama sınırları

[!INCLUDE [data-box-storage-limits](../../includes/data-box-storage-limits.md)]

## <a name="data-upload-caveats"></a>Veri yükleme uyarıları


### <a name="for-import-order"></a>İçeri aktarma sırası için

İçeri aktarma sırası için uyarıları Data Box şunlar vardır:

[!INCLUDE [data-box-data-upload-caveats](../../includes/data-box-data-upload-caveats.md)]

## <a name="for-export-order"></a>Dışa aktarma sırası için

Bir dışarı aktarma siparişi için Data Box uyarılar şunları içerir:

- Data Box, Windows tabanlı bir aygıttır ve büyük/küçük harfe duyarlı dosya adlarını desteklemez. Örneğin, Azure 'da yalnızca büyük küçük harf bakımından farklı adlara sahip iki farklı dosya olabilir. Bu tür dosyaları, cihazda üzerine yazılacak şekilde dışa aktarmak için veri kutusunu kullanmayın.
- Giriş dosyalarında veya aynı verilere başvuran etiketlerde yinelenen etiketlere sahipseniz Data Box dışa aktarma dosyaları atlayabilir veya üzerine yazabilir. Azure portal gösterilen dosya sayısı ve veri boyutu, cihazdaki verilerin gerçek boyutundan farklı olmalıdır. 
- Data Box, SMB üzerinden Windows tabanlı sisteme veri aktarır ve dosyalar ve klasörler için SMB sınırlamalarıyla sınırlıdır. Desteklenmeyen adlara sahip dosyalar ve klasörler aktarılmaz.
- Önekden kapsayıcıya bir 1:1 eşlemesi vardır.
- En büyük dosya adı boyutu 1024 karakter dosyalarıdır, bu uzunluğu aşan dosya adları aktarılmaz.
- *XML* dosyasındaki (sıra oluşturma sırasında karşıya yüklenen) çoğaltılan önekler verilir. Yinelenen ön ekler yok sayılır.
- Sayfa Blobları ve kapsayıcı adları büyük/küçük harfe duyarlıdır; bu nedenle, büyük/küçük harf yanlış olursa, blob ve/veya kapsayıcı bulunamacaktır.
 

## <a name="azure-storage-account-size-limits"></a>Azure depolama hesabı boyut sınırları

[!INCLUDE [data-box-storage-account-size-limits](../../includes/data-box-storage-account-size-limits.md)]

## <a name="azure-object-size-limits"></a>Azure nesne boyutu sınırları

[!INCLUDE [data-box-object-size-limits](../../includes/data-box-object-size-limits.md)]

## <a name="azure-block-blob-page-blob-and-file-naming-conventions"></a>Azure Blok Blobu, Sayfa Blobu ve dosya adlandırma kuralları

[!INCLUDE [data-box-naming-conventions](../../includes/data-box-naming-conventions.md)]
