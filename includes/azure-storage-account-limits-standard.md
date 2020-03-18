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
ms.openlocfilehash: b5de45086b324006a3dafdef5c80689923650759
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79381645"
---
Aşağıdaki tabloda Azure genel amaçlı v1, v2, Blob depolama, blok blobu depolama ve Data Lake Storage 2. Nesil’in etkinleştirildiği depolama hesaplarına yönelik varsayılan sınırlar açıklanmaktadır. *Giriş* sınırı bir depolama hesabına gönderilen tüm verileri belirtir. *Çıkış* sınırı bir depolama hesabından alınan tüm verileri ifade eder.

| Kaynak | Varsayılan limit |
| --- | --- |
| Standart, premium ve Data Lake Storage 2. Nesil etkinleştirilmiş depolama hesapları da dahil olmak üzere her bölgedeki abonelik başına depolama hesabı sayısı.<sup>3</sup> | 250 |
| En fazla depolama hesabı kapasitesi | 5 PiB <sup>1</sup>|
| Depolama hesabı başına blob kapsayıcısı, blob, dosya paylaşımı, tablo, kuyruk, varlık veya ileti sayısı üst sınırı | Sınırsız |
| Depolama hesabı başına<sup>1</sup>istek hızı üst sınırı | Saniyede 20.000 istek |
| Depolama hesabı başına<sup>1</sup> giriş üst sınırı (ABD, Avrupa bölgeleri) | 25 Gb/sn |
| Depolama hesabı başına<sup>1</sup> çıkış üst sınırı (ABD, Avrupa dışındaki bölgeler) | RA-GRS/GRS etkinleştirildiğinde 5 Gb/sn, LRS/ZRS2 için 10 Gb/sn <sup>2</sup> |
| Genel amaçlı v2 ve Blob depolama hesapları için çıkış üst sınırı (tüm bölgeler) | 50 Gb/s |
| Genel amaçlı v1 depolama hesapları için çıkış üst sınırı (ABD bölgeleri) | RA-GRS/GRS etkinleştirildiğinde 20 Gb/sn, LRS/ZRS2 için 30 Gb/sn <sup>2</sup> |
| Genel amaçlı v1 depolama hesapları için çıkış üst sınırı (ABD dışındaki bölgeler) | RA-GRS/GRS etkinleştirildiğinde 10 Gb/sn, LRS/ZRS2 için 15 GB/sn<sup>2</sup> |
| Depolama hesabı başına sanal ağ kuralı sayısı üst sınırı | 200 |
| Depolama hesabı başına IP adresi kuralı üst sınırı | 200 |

<sup>1</sup> Azure Depolama standart hesapları, istek üzerine sunulan daha yüksek kapasite ve giriş sınırlarını destekler. Hesap sınırlarında artış istemek için [Azure Desteği](https://azure.microsoft.com/support/faq/)’ne ulaşın.

<sup>2</sup> Depolama hesabınızda coğrafi olarak yedekli depolama (RA-GRS) veya coğrafi alanlar arası yedekli depolama (RA-GZRS) ile okuma erişim etkinleştirildiyse, ikincil bölgeye yönelik çıkış hedefleri de birincil bölgenin hedefleriyle aynı olur. [Azure depolama çoğaltması](https://docs.microsoft.com/azure/storage/common/storage-redundancy) seçenekleri şunları içerir:

[!INCLUDE [azure-storage-redundancy](azure-storage-redundancy.md)]

<sup>3</sup> [Azure Data Lake Storage 2. Nesil](../articles/storage/blobs/data-lake-storage-introduction.md), Azure Blob depolama temelinde oluşturulan, büyük veri analizi için ayrılmış özellikler kümesidir. Azure Depolama ve blob depolama sınırları Data Lake Storage 2. Nesil için geçerlidir.

> [!NOTE]
> Microsoft, çoğu senaryo için genel amaçlı v2 depolama hesabı kullanmanızı önerir. Genel amaçlı v1 veya Azure Blobu depolama hesabını kapalı kalma süresi ve verileri kopyalama gereksinimi olmadan kolayca bir genel amaçlı v2 hesabına yükseltebilirsiniz. Daha fazla bilgi edinmek için bkz. [Genel amaçlı v2 depolama hesabına yükseltme](../articles/storage/common/storage-account-upgrade.md).

Uygulamanızın ihtiyaçları tek bir depolama hesabının ölçeklenebilirlik hedeflerini aşarsa uygulamanızı birden fazla depolama hesabı kullanacak şekilde oluşturabilirsiniz. Ardından, veri nesnelerinizi bu depolama hesaplarında bölümleyebilirsiniz. Toplu fiyatlandırma hakkında bilgi edinmek için bkz. [Azure Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).

Tüm depolama hesapları oluşturulma zamanlarından bağımsız olarak düz bir ağ topolojisinde çalışır. Azure Depolama düz ağ mimarisi ve ölçeklenebilirlik hakkında daha fazla edinmek için bkz. [Microsoft Azure Depolama: Güçlü Tutarlılığa Sahip Olan Yüksek Oranda Kullanılabilir Bulut Depolama Hizmeti](https://docs.microsoft.com/archive/blogs/hanuk/windows-azures-flat-network-storage-to-enable-higher-scalability-targets). 
