---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/13/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 6cd883289513091ff1a57a130b12e25e012c1160
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334619"
---
Aşağıdaki tabloda Azure genel amaçlı v1, v2, Blob depolama, blok blob depolama ve Veri Gölü Depolama Gen2 etkin depolama hesapları için varsayılan sınırları açıklanmaktadır. Giriş *ingress* sınırı, bir depolama hesabına gönderilen tüm verileri ifade eder. *Çıkış* sınırı, bir depolama hesabından alınan tüm verileri ifade eder.

| Kaynak | Sınır |
| --- | --- |
| Standart, premium ve Data Lake Storage Gen2 özellikli depolama hesapları da dahil olmak üzere abonelik başına bölge başına depolama hesabı sayısı. <sup>3.2.2</sup> | 250 |
| Maksimum depolama hesabı kapasitesi | 5 PiB <sup>1</sup>|
| Depolama hesabı başına maksimum sayıda blob kapsayıcısı, blob, dosya paylaşımları, tablolar, kuyruklar, varlıklar veya iletisayısı | Sınır yok |
| Depolama hesabı başına maksimum istek oranı<sup>1</sup> | Saniyede 20.000 istek |
| Depolama hesabı başına maksimum giriş<sup>1</sup> (ABD, Avrupa bölgeleri) | 25 Gbps |
| Depolama hesabı başına maksimum giriş<sup>1</sup> (ABD ve Avrupa dışındaki bölgeler) | RA-GRS/GRS etkinse 5 Gbps, LRS/ZRS için 10 Gbps<sup>2</sup> |
| Genel amaçlı v2 ve Blob depolama hesapları için maksimum çıkış (tüm bölgeler) | 50 Gbps |
| Genel amaçlı v1 depolama hesapları (ABD bölgeleri) için maksimum çıkış | RA-GRS/GRS etkinse 20 Gbps, LRS/ZRS için 30 Gbps<sup>2</sup> |
| Genel amaçlı v1 depolama hesapları (ABD dışı bölgeler) için maksimum çıkış | RA-GRS/GRS etkinse 10 Gbps, LRS/ZRS için 15 Gbps<sup>2</sup> |
| Depolama hesabı başına maksimum sanal ağ kuralı sayısı | 200 |
| Depolama hesabı başına maksimum IP adresi kuralı sayısı | 200 |

<sup>1</sup> Azure Depolama standart hesapları, istek üzerine giriş için daha yüksek kapasite sınırlarını ve daha yüksek sınırları destekler. Hesap sınırlarının artırılmasını istemek için [Azure Desteği'ne](https://azure.microsoft.com/support/faq/)başvurun.

<sup>2</sup> Depolama hesabınız, coğrafi yedekli depolama (RA-GRS) veya coğrafi bölge yedekli depolama (RA-GZRS) ile okuma erişimi etkinleştirilmişse, ikincil konumun çıkış hedefleri birincil konumdakilerle aynıdır. [Azure Depolama çoğaltma](https://docs.microsoft.com/azure/storage/common/storage-redundancy) seçenekleri şunlardır:

[!INCLUDE [azure-storage-redundancy](azure-storage-redundancy.md)]

<sup>3</sup> [Azure Veri Gölü Depolama Gen2,](../articles/storage/blobs/data-lake-storage-introduction.md) Azure Blob depolamasına dayalı büyük veri analitiğine adanmış bir dizi özelliktir. Azure Depolama ve blob depolama sınırlamaları Data Lake Storage Gen2 için geçerlidir.

> [!NOTE]
> Microsoft, çoğu senaryo için genel amaçlı bir v2 depolama hesabı kullanmanızı önerir. Genel amaçlı bir v1 veya Azure Blob depolama hesabını, kesinti olmaksızın ve veri kopyalamaya gerek kalmadan genel amaçlı bir v2 hesabına kolayca yükseltebilirsiniz. Daha fazla bilgi için genel [amaçlı v2 depolama hesabına yükseltme](../articles/storage/common/storage-account-upgrade.md)konusuna bakın.

Uygulamanızın gereksinimleri tek bir depolama hesabının ölçeklenebilirlik hedeflerini aşıyorsa, uygulamanızı birden çok depolama hesabı kullanmak üzere oluşturabilirsiniz. Daha sonra veri nesnelerinizi bu depolama hesapları arasında bölümlere aktarabilirsiniz. Toplu fiyatlandırma hakkında bilgi için Azure [Depolama fiyatlandırması'na](https://azure.microsoft.com/pricing/details/storage/)bakın.

Tüm depolama hesapları, ne zaman oluşturulduklarına bakılmaksızın düz bir ağ topolojisi üzerinde çalışır. Azure Depolama düz ağ mimarisi ve ölçeklenebilirlik hakkında daha fazla bilgi için [Bkz.](https://docs.microsoft.com/archive/blogs/hanuk/windows-azures-flat-network-storage-to-enable-higher-scalability-targets) 
