---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/23/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 1a9cebe334b266dfcf7f06608fd6a526ded7179b
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "74004208"
---
Azure depolama, birkaç depolama hesabı türü sunar. Her tür farklı özellikleri destekler ve kendi fiyatlandırma modeline sahiptir. Uygulamalarınız için en iyi hesap türünü belirleyebilmek üzere bir depolama hesabı oluşturmadan önce bu farklılıkları göz önünde bulundurun. Depolama hesaplarının türleri şunlardır:

- **Genel amaçlı v2 hesapları**: Bloblar, dosyalar, kuyruklar ve tablolar için temel depolama hesabı türü. Azure Storage kullanan çoğu senaryo için önerilir.
- **Genel amaçlı v1 hesapları**: Bloblar, dosyalar, kuyruklar ve tablolar için eski hesap türü. Mümkün olduğunda bunun yerine genel amaçlı v2 hesaplarını kullanın.
- **Blockblobstorage hesapları**: yalnızca BLOB depolama hesapları Premium performans özelliklerine sahip. Yüksek işlem hızlarıyla, daha küçük nesneler kullanılarak veya sürekli olarak düşük depolama gecikmesi gerektiren senaryolar için önerilir.
- **FileStorage hesapları**: yalnızca dosyalar, Premium performans özelliklerine sahip depolama hesaplarıdır. Kurumsal veya yüksek performanslı ölçekli uygulamalar için önerilir.
- **Blobstorage hesapları**: eski BLOB depolama hesapları. Mümkün olduğunda bunun yerine genel amaçlı v2 hesaplarını kullanın.

Aşağıdaki tabloda depolama hesabı türleri ve bunların özellikleri açıklanmaktadır:

| Depolama hesabı türü | Desteklenen hizmetler                       | Desteklenen performans katmanları      | Desteklenen erişim katmanları         | Çoğaltma seçenekleri               | Dağıtım modeli<div role="complementary" aria-labelledby="deployment-model"><sup>1</sup></div> | Şifreleme<div role="complementary" aria-labelledby="encryption"><sup>2</sup></div> |
|----------------------|------------------------------------------|-----------------------------|--------------------------------|-----------------------------------|------------------------------|------------------------|
| Genel amaçlı v2   | Blob, dosya, kuyruk, tablo ve disk       | Standart, Premium<div role="complementary" aria-labelledby="premium-performance"><sup>5</sup></div> | Sık erişimli, seyrek erişimli Arşiv<div role="complementary" aria-labelledby="archive"><sup>3</sup></div> | LRS, GRS, RA-GRS, ZRS, GZRS (Önizleme), RA-GZRS (Önizleme)<div role="complementary" aria-labelledby="zone-redundant-storage"><sup>4</sup></div> | Resource Manager             | Şifreli              |
| Genel amaçlı v1   | Blob, dosya, kuyruk, tablo ve disk       | Standart, Premium<div role="complementary" aria-labelledby="premium-performance"><sup>5</sup></div> | Yok                            | LRS, GRS, RA-GRS                  | Kaynak Yöneticisi, klasik    | Şifreli              |
| BlockBlobStorage   | Blob (yalnızca blok Blobları ve ekleme Blobları) | Premium                       | Yok                            | LRS, ZRS<div role="complementary" aria-labelledby="zone-redundant-storage"><sup>4</sup></div>                               | Resource Manager             | Şifreli              |
| Dosya depolama   | Yalnızca dosyalar | Premium                       | Yok                            | LRS, ZRS<div role="complementary" aria-labelledby="zone-redundant-storage"><sup>4</sup></div>                               | Resource Manager             | Şifreli              |
| BlobStorage         | Blob (yalnızca blok Blobları ve ekleme Blobları) | Standart                      | Sık erişimli, seyrek erişimli Arşiv<div role="complementary" aria-labelledby="archive"><sup>3</sup></div> | LRS, GRS, RA-GRS                  | Resource Manager             | Şifreli              |

<div id="deployment-model"><sup>1</sup> Azure Resource Manager dağıtım modelinin kullanılması önerilir. Klasik dağıtım modelini kullanan depolama hesapları bazı konumlarda hala oluşturulabilir ve klasik mevcut hesaplar desteklenmeye devam eder. Daha fazla bilgi için bkz <a href="https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model">. Azure Resource Manager ile klasik dağıtım: dağıtım modellerini ve kaynaklarınızın durumunu anlayın</a>.</div>

<div id="encryption"><sup>2</sup> Tüm depolama hesapları, bekleyen veriler için Depolama Hizmeti Şifrelemesi (SSE) kullanılarak şifrelenir. Daha fazla bilgi için bkz. <a href="https://docs.microsoft.com/azure/storage/common/storage-service-encryption">bekleyen veriler Için Azure depolama hizmeti şifrelemesi</a>.</div>

<div id="archive"><sup>3</sup> Arşiv katmanı, depolama hesabı düzeyinde değil yalnızca tek bir blob düzeyinde kullanılabilir. Yalnızca blok Blobları ve ekleme Blobları arşivlenebilir. Daha fazla bilgi için bkz. <a href="https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers">Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv depolama katmanları</a>.</div>

<div id="zone-redundant-storage"><sup>4</sup> Bölgesel olarak yedekli depolama (ZRS) ve coğrafi bölge yedekli depolama (GZRS/RA-GZRS) (Önizleme) yalnızca belirli bölgelerde standart genel amaçlı v2, BlockBlobStorage ve FileStorage hesapları için kullanılabilir. ZRS hakkında daha fazla bilgi için bkz. bölgesel olarak <a href="https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs">yedekli depolama (ZRS): yüksek oranda kullanılabilir Azure depolama uygulamaları</a>. GZRS/RA-GZRS hakkında daha fazla bilgi için bkz. <a href="https://docs.microsoft.com/azure/storage/common/storage-redundancy-gzrs">coğrafi bölge yedekli depolama, yüksek oranda kullanılabilirlik ve en yüksek dayanıklılık (Önizleme) için</a>. Diğer çoğaltma seçenekleri hakkında daha fazla bilgi için bkz. <a href="https://docs.microsoft.com/azure/storage/common/storage-redundancy">Azure Storage çoğaltma</a>.</div>

<div id="premium-performance"><sup>5</sup> Genel amaçlı v2 ve genel amaçlı v1 hesapları için Premium performans, yalnızca disk ve Sayfa Blobu için kullanılabilir.</div>
