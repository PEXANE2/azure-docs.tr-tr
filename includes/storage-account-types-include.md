---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/05/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 726fc9cc0e4eb5a8f9bed5cca8dd4d3df588ac7c
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79371570"
---
Azure Depolama farklı türde depolama hesapları sunar. Her tür farklı özellikleri destekler ve her türün kendi fiyatlandırma modeli bulunur. Uygulamalarınıza yönelik en iyi hesap türünü belirlemek için depolama hesabı oluşturmadan önce bu farklılıkları göz önünde bulundurun. Depolama hesabı türleri şunlardır:

- **Genel amaçlı v2 hesapları**: Bloblar, dosyalar, kuyruklar ve tablolar için temel depolama hesabı türü. Azure Depolama kullanan çoğu senaryo için önerilir.
- **Genel amaçlı v1 hesapları**: Bloblar, dosyalar, kuyruklar ve tablolar için eski hesap türü. Mümkün olduğunda genel amaçlı v2 hesapları kullanın.
- **BlockBlobStorage hesapları**: Blok blobları ve ekleme bloblarına yönelik premium performans özelliklerine sahip depolama hesapları. Yüksek işlem hızları gerektiren senaryolar, daha küçük nesneleri kullanan senaryolar veya tutarlı şekilde düşük depolama gecikme süresi gerektiren senaryolar için önerilir.
- **FileStorage hesapları**: Premium performans özellikleri olan yalnızca dosya depolama hesapları. Kurumsal veya yüksek performans ölçekli uygulamalar için önerilir.
- **BlobStorage hesapları**: Eski yalnızca blob depolama hesapları. Mümkün olduğunda genel amaçlı v2 hesapları kullanın.

Aşağıdaki tabloda depolama hesabı türleri ve bunların özellikleri açıklanmaktadır:

| Depolama hesabı türü | Desteklenen hizmetler                       | Desteklenen performans katmanları      | Desteklenen erişim katmanları         | Çoğaltma seçenekleri               | Dağıtım modeli<div role="complementary" aria-labelledby="deployment-model"><sup>1</sup></div> | Şifreleme<div role="complementary" aria-labelledby="encryption"><sup>2</sup></div> |
|----------------------|------------------------------------------|-----------------------------|--------------------------------|-----------------------------------|------------------------------|------------------------|
| Genel amaçlı v2   | Blob, Dosya, Kuyruk, Tablo, Disk ve Data Lake 2. Nesil<div role="complementary" aria-labelledby="data-lake-gen2"><sup>6</sup></div>      | Standart, Premium<div role="complementary" aria-labelledby="premium-performance"><sup>5</sup></div> | Sık Erişimli, Seyrek Erişimli, Arşiv<div role="complementary" aria-labelledby="archive"><sup>3</sup></div> | LRS, GRS, RA-GRS, ZRS, GZRS (önizleme), RA-GZRS (önizleme)<div role="complementary" aria-labelledby="zone-redundant-storage"><sup>4</sup></div> | Resource Manager             | Şifreli              |
| Genel amaçlı v1   | Blob, Dosya, Kuyruk, Tablo ve Disk       | Standart, Premium<div role="complementary" aria-labelledby="premium-performance"><sup>5</sup></div> | Yok                            | LRS, GRS, RA-GRS                  | Kaynak Yöneticisi, Klasik    | Şifreli              |
| BlockBlobStorage   | Blob (yalnızca blok blobları ve ek blobları) | Premium                       | Yok                            | LRS, ZRS<div role="complementary" aria-labelledby="zone-redundant-storage"><sup>4</sup></div>                               | Resource Manager             | Şifreli              |
| FileStorage   | Yalnızca dosya | Premium                       | Yok                            | LRS, ZRS<div role="complementary" aria-labelledby="zone-redundant-storage"><sup>4</sup></div>                               | Resource Manager             | Şifreli              |
| BlobStorage         | Blob (yalnızca blok blobları ve ek blobları) | Standart                      | Sık Erişimli, Seyrek Erişimli, Arşiv<div role="complementary" aria-labelledby="archive"><sup>3</sup></div> | LRS, GRS, RA-GRS                  | Resource Manager             | Şifreli              |

<div id="deployment-model"><sup>1</sup>Azure Resource Manager dağıtım modelini kullanmanız önerilir. Bazı konumlarda klasik dağıtım modelini kullanan depolama hesapları oluşturmaya devam edebilirsiniz. Mevcut klasik hesaplar için sunulan destek sürdürülür. Daha fazla bilgi için bkz. <a href="https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model">Azure Resource Manager ile klasik dağıtım karşılaştırması: Dağıtım modellerini ve kaynaklarınızın durumunu anlama</a>.</div><br/>

<div id="encryption"><sup>2</sup>Tüm depolama hesapları, bekleyen yedeklenmiş veriler için Depolama Hizmeti Şifrelemesi (SSE) kullanılarak şifrelenir. Daha fazla bilgi için bkz. <a href="https://docs.microsoft.com/azure/storage/common/storage-service-encryption">Bekleyen Veriler için Azure Depolama Hizmeti Şifrelemesi</a>.</div><br/>

<div id="archive"><sup>3</sup> Arşiv depolama ve blob düzeyinde katman ayarlama, yalnızca blok bloblarını destekler. Arşiv katmanı, depolama hesabı düzeyinde değil ayrı blob düzeyinde sunulur. Daha fazla bilgi için bkz. <a href="https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers">Azure Blob depolama: Sık erişimli, Seyrek erişimli ve Arşiv depolama katmanları</a>.</div><br/>

<div id="zone-redundant-storage"><sup>4</sup>Bölgesel olarak yedekli depolama (ZRS) ve coğrafi alanlar arası yedekli depolama (GZRS/RA-GZRS) (önizleme) yalnızca bazı bölgelerdeki standart genel amaçlı v2, BlockBlobStorage ve FileStorage hesapları için sunulur. Azure Depolama yedekliliği seçeneklerine ilişkin daha fazla bilgi için bkz. <a href="https://docs.microsoft.com/azure/storage/common/storage-redundancy">Azure Depolama yedekliliği</a>.</div><br/>

<div id="premium-performance"><sup>5</sup>Genel amaçlı v2 ve genel amaçlı v1 hesapları için premium performans yalnızca disk ve sayfa blobunda sunulur. Blok veya ekleme bloblarına yönelik premium performans yalnızca BlockBlobStorage hesaplarında sunulur. Dosyalara yönelik premium performans yalnızca FileStorage hesaplarında sunulur.</div><br/>

<div id="data-lake-gen2"><sup>6</sup>Azure Data Lake Storage 2. Nesil, Azure Blob depolamada oluşturulan büyük veri analizi için ayrılmış özellikler kümesidir. Data Lake Storage 2. Nesil yalnızca Hiyerarşik ad alanı özelliğinin etkinleştirildiği Genel amaçlı v2 depolama hesaplarında desteklenir. Data Lake Storage 2. Nesil hakkında daha fazla bilgi almak için bkz. <a href="https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction">Azure Data Lake Storage 2. Nesil’e giriş</a>.</div>
