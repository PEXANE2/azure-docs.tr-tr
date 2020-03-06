---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 02/27/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: e29cdd56d1c43b3d0e8fc6ca233ac19d8b0004ff
ms.sourcegitcommit: 021ccbbd42dea64d45d4129d70fff5148a1759fd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78357438"
---
Aşağıdaki tabloda, Azure genel amaçlı v1 (GPv1), v2 (GPv2), blob, Premium BlockBlob ve Data Lake Gen2 özellikli depolama hesapları için varsayılan sınırlar açıklanmaktadır. Giriş *sınırı,* bir depolama hesabına gönderilen tüm verileri ifade eder. *Çıkış* sınırı, bir depolama hesabından alınan tüm verileri ifade eder.

| Kaynak | Varsayılan limit |
| --- | --- |
| Standart, Premium ve Data Lake Gen2 hesapları 3 dahil olmak üzere her bir abonelik için bölge başına depolama hesabı sayısı<sup>3</sup> | 250 |
| Maksimum depolama hesabı kapasitesi | 5 PiB <sup>1</sup>|
| Depolama hesabı başına en fazla BLOB kapsayıcısı, blob, dosya paylaşımı, tablo, kuyruk, varlık veya ileti sayısı | Sınır yok |
| Depolama hesabı başına en fazla istek oranı<sup>1</sup> | saniye başına 20.000 istek |
| Depolama hesabı başına en fazla giriş<sup>1</sup> (ABD, Avrupa Bölgesi) | 25 Gbps |
| Depolama hesabı başına en fazla giriş<sup>1</sup> (ABD ve Avrupa dışındaki bölgeler) | RA-GRS/GRS etkinse 5 Gbps, LRS/ZRS<sup>2</sup> Için 10 Gbps |
| Genel amaçlı v2 ve BLOB depolama hesapları için maksimum çıkış (tüm bölgeler) | 50 Gbps |
| Genel amaçlı v1 depolama hesapları için maksimum çıkış (ABD bölgeleri) | RA-GRS/GRS etkinse 20 Gbps, LRS/ZRS<sup>2</sup> Için 30 Gbps |
| Genel amaçlı v1 depolama hesapları için maksimum çıkış (ABD dışı bölgeler) | 10 Gbps, RA-GRS/GRS etkinse, LRS/ZRS<sup>2</sup> Için 15 GB/sn |
| Depolama hesabı başına en fazla sanal ağ kuralı sayısı | 200 |
| Depolama hesabı başına en fazla IP adresi kuralı sayısı | 200 |

<sup>1</sup> Azure depolama standart hesapları, isteğe göre giriş için daha yüksek kapasite sınırlarını ve daha yüksek limitleri destekler. Hesap sınırlarında artış istemek için [Azure desteği](https://azure.microsoft.com/support/faq/)'ne başvurun.

<sup>2</sup> depolama hesabınızın, coğrafi olarak yedekli depolama (RA-GRS) veya coğrafi bölge yedekli depolama (ra-GZRS) ile okuma erişimi etkinse, ikincil konum için çıkış hedefleri birincil konumlarından benzerdir. [Azure depolama çoğaltma](https://docs.microsoft.com/azure/storage/common/storage-redundancy) seçenekleri şunlardır:

[!INCLUDE [azure-storage-redundancy](azure-storage-redundancy.md)]

<sup>3</sup> [Azure Data Lake Storage 2.](../articles/storage/blobs/data-lake-storage-introduction.md) , Azure Blob depolama üzerinde oluşturulmuş, büyük veri analizi için adanmış bir yetenekler kümesidir. Azure depolama ve BLOB depolama sınırlamaları Data Lake Gen2 için geçerlidir.

> [!NOTE]
> Microsoft, çoğu senaryo için genel amaçlı v2 depolama hesabı kullanmanızı önerir. Genel amaçlı v1 veya bir Azure Blob Depolama hesabını, kapalı kalma süresi olmadan ve verileri kopyalamaya gerek kalmadan, genel amaçlı bir v2 hesabına kolayca yükseltebilirsiniz. Daha fazla bilgi için bkz. [genel amaçlı v2 depolama hesabına yükseltme](../articles/storage/common/storage-account-upgrade.md).

Uygulamanızın ihtiyaçları tek bir depolama hesabının ölçeklenebilirlik hedeflerini aşarsa, uygulamanızı birden çok depolama hesabı kullanacak şekilde oluşturabilirsiniz. Daha sonra veri nesnelerinizi bu depolama hesaplarında bölümleyebilirsiniz. Toplu fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure Storage fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).

Tüm depolama hesapları, oluşturulduklarında bağımsız olarak düz bir ağ topolojisi üzerinde çalışır. Azure depolama düz ağ mimarisi ve ölçeklenebilirlik hakkında daha fazla bilgi için bkz. [Microsoft Azure depolama: güçlü tutarlılık Ile yüksek oranda kullanılabilir bir bulut depolama hizmeti](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx). Bir [Data Lake Gen2 hesabı için bir hiyerarşik ad alanı](../articles/storage/blobs/data-lake-storage-namespace.md) , çoklu protokol erişimi için düz ad alanına ek olarak etkinleştirilebilir. Hem düz hem de hiyerarşik ad alanı depolama hesapları, bu makalede özetlenen aynı ölçeklenebilirlik ve performans hedeflerini destekler.
