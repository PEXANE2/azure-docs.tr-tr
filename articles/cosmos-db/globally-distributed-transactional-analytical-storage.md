---
title: Azure Cosmos kapsayıcıları için genel olarak dağıtılan işlemsel ve analitik (özel önizleme) depolama
description: Azure Cosmos kapsayıcıları için işlemsel ve analitik depolama ve bunların yapılandırma seçenekleri hakkında bilgi edinin.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/30/2019
ms.reviewer: sngun
ms.openlocfilehash: 1c2b79f8d0641b1a1386329a2add14ded766bf5a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77623389"
---
# <a name="globally-distributed-transactional-and-analytical-storage-for-azure-cosmos-containers"></a>Azure Cosmos kapsayıcıları için genel olarak dağıtılmış işlemsel ve analitik depolama

Azure Cosmos konteyneri dahili olarak iki depolama motoru tarafından yedeklenir : işlemsel depolama motoru ve güncel bir analitik depolama motoru (özel önizlemede). Hem depolama motorları günlük yapılandırılmış ve daha hızlı güncellemeler için yazma-optimize edilmiştir. Ancak, her biri farklı kodlanır:

* **İşlemsel depolama motoru** – Hızlı işlem okumaları ve sorguları için satır odaklı biçimde kodlanır.

* **Analitik depolama motoru** - Hızlı analitik sorgular ve taramalar için columnar formatında kodlanır.

![Depolama motorları ve Azure Cosmos DB API eşleme](./media/globally-distributed-transactional-analytical-storage/storage-engines-api-mapping.png)

İşlemsel depolama motoru yerel SSD'ler tarafından desteklenen, analitik depolama ise ucuz bir küme dışı SSD depolama da depolanır. Aşağıdaki tablo, işlem ve analitik depolama arasındaki önemli farkları yakalar.


|Özellik  |İşlemsel depolama  |Analitik depolama |
|---------|---------|---------|
|Azure Cosmos kapsayıcısı başına maksimum depolama alanı |   Sınırsız      |    Sınırsız     |
|Mantıksal bölüm anahtarı başına maksimum depolama   |   20 GB      |   Sınırsız      |
|Depolama kodlaması  |   Satır odaklı, bir iç biçim kullanarak.   |   Sütun odaklı, Apache Parke formatı kullanarak. |
|Depolama alanı |   Yerel/küme içi SSD'ler tarafından desteklenen çoğaltılmış depolama. |  Ucuz uzak/küme dışı SSD'ler tarafından desteklenen çoğaltılmış depolama.       |
|Dayanıklılık  |    99.99999 (7-9 s)     |  99.99999 (7-9 s)       |
|Verilere erişen API'ler  |   SQL, MongoDB, Cassandra, Gremlin, Tablolar ve vb.       | Apache Spark         |
|Bekletme (Zaman-to-live veya TTL)   |  İlke odaklı, `DefaultTimeToLive` Azure Cosmos kapsayıcısı üzerinde özelliği kullanarak yapılandırılan.       |   İlke odaklı, `ColumnStoreTimeToLive` Azure Cosmos kapsayıcısı üzerinde özelliği kullanarak yapılandırılan.      |
|GB başına fiyat    |   Fiyatlandırma [sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) bakın     |   Fiyatlandırma [sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) bakın        |
|Depolama işlemleri için fiyat    |  Fiyatlandırma [sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) bakın         |   Fiyatlandırma [sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) bakın        |

## <a name="benefits-of-transactional-and-analytical-storage"></a>İşlemsel ve analitik depolamanın yararları

### <a name="no-etl-operations"></a>ETL işlemi yok

Geleneksel analitik boru hatları, her biri bilgi işlem ve depolama katmanlarına ayıklama-Dönüştürme-Yükleme (ETL) işlemleri gerektiren birden çok aşamadan oluşur. Karmaşık veri işleme mimarileri ile sonuçlanır. Bu da depolama ve bilgi işlemin birden fazla aşaması için yüksek maliyetler ve depolama ve bilgi işlem in çeşitli aşamaları arasında aktarılan büyük hacimli veriler nedeniyle yüksek gecikme oranı anlamına gelir.  

Azure Cosmos DB ile ETL işlemleri gerçekleştirmenin genel yükü yoktur. Her Azure Cosmos kapsayıcısı hem işlemsel hem de analitik depolama motorları tarafından desteklenen ve işlem ve analitik depolama altyapısı arasındaki veri aktarımı veritabanı altyapısı içinde ve herhangi bir ağ atlamaları olmadan yapılır. Elde edilen gecikme ve maliyet, geleneksel analitik çözümlere göre önemli ölçüde daha düşüktür. Ayrıca, hem işlemsel hem de analitik iş yükleri için küresel olarak dağıtılmış tek bir depolama sistemi elde edersiniz.  

### <a name="store-multiple-versions-update-and-query-the-analytical-storage"></a>Birden çok sürümü depolama, güncelleştirme ve analitik depolama sorgula

Analitik depolama tamamen güncellenmiştir ve Azure Cosmos kapsayıcısı üzerinde gerçekleşen tüm işlem güncelleştirmelerinin tam sürüm geçmişini içerir.

İşlem depolamasına yapılan herhangi bir güncellemenin 30 saniye içinde analitik depolama tarafından görülemeyen garanti edilir. 

### <a name="globally-distributed-multi-master-analytical-storage"></a>Küresel olarak dağıtılan, çok yaygın analitik depolama

Azure Cosmos hesabınız tek bir bölgeye doğru kapsama aitse, kapsayıcılarda depolanan veriler (işlemsel ve analitik depolamada) da tek bir bölgeye kapsamlıdır. Diğer taraftan, Azure Cosmos hesabı genel olarak dağıtılırsa, kapsayıcılarda depolanan veriler de genel olarak dağıtılır.

Birden çok yazma bölgesiyle yapılandırılan Azure Cosmos hesapları için, kapsayıcıya (hem işlem hem de analitik depolama için) her zaman yerel bölgede gerçekleştirilir ve bu nedenle hızlıdırlar.

Tek veya çok bölgeli Azure Cosmos hesapları için, tek yazma bölgesi (tek kalıp) veya birden çok yazma bölgesi (çok büyük yazar olarak da bilinir) ne olursa olsun, hem işlemsel hem de analitik okuma/sorgular belirli bölgede yerel olarak gerçekleştirilir.

### <a name="performance-isolation-between-transactional-and-analytical-workloads"></a>İşlemsel ve analitik iş yükleri arasında performans yalıtımı

Belirli bir bölgede, işlem iş yükleri kabınızın işlem/satır depolamasına göre çalışır. Diğer taraftan, analitik iş yükleri kabınızın analitik/sütun depolamasına göre çalışır. İki depolama motoru bağımsız olarak çalışır ve iş yükleri arasında sıkı performans yalıtımı sağlar.

İşlemsel iş yükleri, sağlanan iş yükünü (RUS) tüketir. İşlemsel iş yüklerinin aksine, analitik iş yükü çıktısı gerçek tüketimi temel alır. Analitik iş yükleri kaynakları isteğe bağlı olarak tüketir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB'de yaşama zamanı](time-to-live.md)
