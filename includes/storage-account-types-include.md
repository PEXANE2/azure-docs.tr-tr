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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371570"
---
Azure Depolama çeşitli depolama hesapları sunar. Her tür farklı özellikleri destekler ve kendi fiyatlandırma modeli vardır. Uygulamalarınız için en iyi hesap türünü belirlemek için bir depolama hesabı oluşturmadan önce bu farklılıkları göz önünde bulundurun. Depolama hesabı türleri şunlardır:

- **Genel amaçlı v2 hesapları**: Blobs, dosyalar, kuyruklar ve tablolar için temel depolama hesabı türü. Azure Depolama'yı kullanarak çoğu senaryo için önerilir.
- **Genel amaçlı v1 hesapları**: Blobs, dosyalar, kuyruklar ve tablolar için eski hesap türü. Bunun yerine mümkün olduğunda genel amaçlı v2 hesaplarını kullanın.
- **BlockBlobStorage hesapları**: Blok lekeleri ve ek blob'lar için üstün performans özelliklerine sahip depolama hesapları. Yüksek işlem oranlarına sahip senaryolar veya daha küçük nesneler kullanan veya sürekli olarak düşük depolama gecikmesi gerektiren senaryolar için önerilir.
- **FileStorage hesapları**: Üstün performans özelliklerine sahip yalnızca dosyalara özel depolama hesapları. Kurumsal veya yüksek performanslı ölçek uygulamaları için önerilir.
- **BlobStorage hesapları**: Eski Blob depolama hesapları. Bunun yerine mümkün olduğunda genel amaçlı v2 hesaplarını kullanın.

Aşağıdaki tabloda depolama hesapları nın türleri ve yetenekleri açıklanmaktadır:

| Depolama hesabı türü | Desteklenen hizmetler                       | Desteklenen performans katmanları      | Desteklenen erişim katmanları         | Çoğaltma seçenekleri               | Dağıtım modeli<div role="complementary" aria-labelledby="deployment-model"><sup>1</sup></div> | Şifreleme<div role="complementary" aria-labelledby="encryption"><sup>2</sup></div> |
|----------------------|------------------------------------------|-----------------------------|--------------------------------|-----------------------------------|------------------------------|------------------------|
| Genel amaçlı V2   | Blob, Dosya, Sıra, Tablo, Disk ve Veri Gölü Gen2<div role="complementary" aria-labelledby="data-lake-gen2"><sup>6</sup></div>      | Standart, Premium<div role="complementary" aria-labelledby="premium-performance"><sup>5</sup></div> | Sıcak, Serin, Arşiv<div role="complementary" aria-labelledby="archive"><sup>3</sup></div> | LRS, GRS, RA-GRS, ZRS, GZRS (önizleme), RA-GZRS (önizleme)<div role="complementary" aria-labelledby="zone-redundant-storage"><sup>4</sup></div> | Resource Manager             | Şifreli              |
| Genel amaçlı V1   | Blob, Dosya, Sıra, Tablo ve Disk       | Standart, Premium<div role="complementary" aria-labelledby="premium-performance"><sup>5</sup></div> | Yok                            | LRS, GRS, RA-GRS                  | Kaynak Yöneticisi, Klasik    | Şifreli              |
| BlockBlobDepolama   | Blob (blok lekeleri ve sadece ek blobs) | Premium                       | Yok                            | LRS, ZRS<div role="complementary" aria-labelledby="zone-redundant-storage"><sup>4</sup></div>                               | Resource Manager             | Şifreli              |
| Dosya Depolama   | Yalnızca dosya | Premium                       | Yok                            | LRS, ZRS<div role="complementary" aria-labelledby="zone-redundant-storage"><sup>4</sup></div>                               | Resource Manager             | Şifreli              |
| BlobDepolama         | Blob (blok lekeleri ve sadece ek blobs) | Standart                      | Sıcak, Serin, Arşiv<div role="complementary" aria-labelledby="archive"><sup>3</sup></div> | LRS, GRS, RA-GRS                  | Resource Manager             | Şifreli              |

<div id="deployment-model"><sup>1.1.2</sup> Azure Kaynak Yöneticisi dağıtım modelini kullanman önerilir. Klasik dağıtım modelini kullanan depolama hesapları bazı konumlarda oluşturulabilir ve varolan klasik hesaplar desteklenmeye devam etmektedir. Daha fazla bilgi için azure <a href="https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model">kaynak yöneticisi ve klasik dağıtım bölümüne bakın: Dağıtım modellerini ve kaynaklarınızın durumunu anlayın.</a></div><br/>

<div id="encryption"><sup>2.000</sup> Tüm depolama hesapları, veri için Depolama Hizmeti Şifrelemesi (SSE) kullanılarak şifrelenir. Daha fazla bilgi <a href="https://docs.microsoft.com/azure/storage/common/storage-service-encryption">için, Veriler için Azure Depolama Hizmeti Şifrelemesi'ne</a>bakın.</div><br/>

<div id="archive"><sup>3</sup> Arşiv depolama ve blob düzeyinde katmanlama yalnızca blok blobs destekler. Arşiv katmanı, depolama hesabı düzeyinde değil, yalnızca tek bir blob düzeyinde kullanılabilir. Daha fazla bilgi için Azure <a href="https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers">Blob depolama alanı: Sıcak, Serin ve Arşiv depolama katmanlarına</a>bakın.</div><br/>

<div id="zone-redundant-storage"><sup>4.2.2</sup> Bölge yedekli depolama (ZRS) ve coğrafi bölge yedekli depolama (GZRS/RA-GZRS) (önizleme) yalnızca belirli bölgelerdeki standart genel amaçlı V2, BlockBlobStorage ve FileStorage hesapları için kullanılabilir. Azure Depolama artıklığı seçenekleri hakkında daha fazla bilgi için Bkz. <a href="https://docs.microsoft.com/azure/storage/common/storage-redundancy">Azure Depolama artıklığı.</a></div><br/>

<div id="premium-performance"><sup>5.000</sup> Genel amaçlı v2 ve genel amaçlı v1 hesapları için premium performans yalnızca disk ve sayfa blob'u için kullanılabilir. Blok veya ek blob'lar için üstün performans yalnızca BlockBlobStorage hesaplarında mevcuttur. Dosyalar için premium performans yalnızca FileStorage hesaplarında kullanılabilir.</div><br/>

<div id="data-lake-gen2"><sup>6.000</sup> Azure Veri Gölü Depolama Gen2, Azure Blob depolamasına dayalı büyük veri analitiğine adanmış bir dizi özelliktir. Veri Gölü Depolama Gen2 yalnızca Hiyerarşik ad alanı etkin genel amaçlı V2 depolama hesaplarında desteklenir. Veri Gölü Depolama Gen2 hakkında daha fazla bilgi için, <a href="https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction">Azure Veri Gölü Depolama Gen2'ye Giriş 'e</a>bakın.</div>
