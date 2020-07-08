---
title: dosya dahil etme
description: dosya dahil etme
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 05/20/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: e2540bceab17e6f37fd94b28df3814ccffa1c81e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84466937"
---
Aşağıdaki tabloda, Azure genel amaçlı v1, v2, BLOB depolama ve blok BLOB depolama hesapları için varsayılan sınırlar açıklanmaktadır. Giriş *sınırı,* bir depolama hesabına gönderilen tüm verileri ifade eder. *Çıkış* sınırı, bir depolama hesabından alınan tüm verileri ifade eder.

| Kaynak | Sınır |
| --- | --- |
| Standart ve Premium Depolama hesapları dahil olmak üzere, abonelik başına bölge başına depolama hesabı sayısı.| 250 |
| Maksimum depolama hesabı kapasitesi | 5 PiB <sup>1</sup>|
| Depolama hesabı başına en fazla BLOB kapsayıcısı, blob, dosya paylaşımı, tablo, kuyruk, varlık veya ileti sayısı | Sınır yok |
| Depolama hesabı başına en fazla istek oranı<sup>1</sup> | saniye başına 20.000 istek |
| Depolama hesabı başına en fazla giriş<sup>1</sup> (ABD, Avrupa Bölgesi) | 10 Gbps |
| Depolama hesabı başına en fazla giriş<sup>1</sup> (ABD ve Avrupa dışındaki bölgeler) | RA-GRS/GRS etkinse 5 Gbps, LRS/ZRS<sup>2</sup> Için 10 Gbps |
| Genel amaçlı v2 ve BLOB depolama hesapları için maksimum çıkış (tüm bölgeler) | 50 Gbps |
| Genel amaçlı v1 depolama hesapları için maksimum çıkış (ABD bölgeleri) | RA-GRS/GRS etkinse 20 Gbps, LRS/ZRS<sup>2</sup> Için 30 Gbps |
| Genel amaçlı v1 depolama hesapları için maksimum çıkış (ABD dışı bölgeler) | 10 Gbps, RA-GRS/GRS etkinse, LRS/ZRS<sup>2</sup> Için 15 GB/sn |
| Depolama hesabı başına en fazla sanal ağ kuralı sayısı | 200 |
| Depolama hesabı başına en fazla IP adresi kuralı sayısı | 200 |

<sup>1</sup> Azure depolama standart hesapları, isteğe göre giriş için daha yüksek kapasite sınırlarını ve daha yüksek limitleri destekler. Hesap sınırlarında artış istemek için [Azure desteği](https://azure.microsoft.com/support/faq/)'ne başvurun.

<sup>2</sup> depolama hesabınızın, coğrafi olarak yedekli depolama (RA-GRS) veya coğrafi bölge yedekli depolama (ra-GZRS) ile okuma erişimi etkinse, ikincil konum için çıkış hedefleri birincil konumlarından benzerdir. [Azure depolama çoğaltma](https://docs.microsoft.com/azure/storage/common/storage-redundancy) seçenekleri şunlardır:

[!INCLUDE [azure-storage-redundancy](azure-storage-redundancy.md)]

<sup>3</sup> [Azure Data Lake Storage 2.](../articles/storage/blobs/data-lake-storage-introduction.md) , Azure Blob depolama üzerinde oluşturulmuş, büyük veri analizi için adanmış bir yetenekler kümesidir.

> [!NOTE]
> Microsoft, çoğu senaryo için genel amaçlı v2 depolama hesabı kullanmanızı önerir. Genel amaçlı v1 veya bir Azure Blob Depolama hesabını, kapalı kalma süresi olmadan ve verileri kopyalamaya gerek kalmadan, genel amaçlı bir v2 hesabına kolayca yükseltebilirsiniz. Daha fazla bilgi için bkz. [genel amaçlı v2 depolama hesabına yükseltme](../articles/storage/common/storage-account-upgrade.md).

Uygulamanızın ihtiyaçları tek bir depolama hesabının ölçeklenebilirlik hedeflerini aşarsa, uygulamanızı birden çok depolama hesabı kullanacak şekilde oluşturabilirsiniz. Daha sonra veri nesnelerinizi bu depolama hesaplarında bölümleyebilirsiniz. Toplu fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure Storage fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).

Tüm depolama hesapları, oluşturulduklarında bağımsız olarak düz bir ağ topolojisi üzerinde çalışır. Azure depolama düz ağ mimarisi ve ölçeklenebilirlik hakkında daha fazla bilgi için bkz. [Microsoft Azure depolama: güçlü tutarlılık Ile yüksek oranda kullanılabilir bir bulut depolama hizmeti](https://docs.microsoft.com/archive/blogs/hanuk/windows-azures-flat-network-storage-to-enable-higher-scalability-targets). 
