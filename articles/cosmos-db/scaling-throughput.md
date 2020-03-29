---
title: Azure Cosmos DB'de iş ortalığı ölçekleme
description: Bu makalede, Azure Cosmos DB'nin Azure Cosmos hesabının sağlandığı farklı bölgelerde iş ortalığı nasıl ölçeklendirildiği açıklanmaktadır.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: sngun
ms.reviewer: sngun
ms.openlocfilehash: 440f23afcd08326261be30432ad1f0ecb16f55fd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74873514"
---
# <a name="globally-scale-provisioned-throughput"></a>Sağlanan aktarım hızını küresel olarak ölçeklendirme 

Azure Cosmos DB'de, sağlanan iş ortası istek birimleri/saniye (RU/s veya çoğul form RUs) olarak temsil edilir. RUs, cosmos kapsayıcınıza karşı aşağıdaki resimde gösterildiği gibi hem okuma hem de yazma işlemlerinin maliyetini ölçer:

![İstek Birimleri](./media/scaling-throughput/request-unit-charge-of-read-and-write-operations.png)

Bir Cosmos konteyner veya Cosmos veritabanında RUs sağlayabilir. Bir konteyner üzerinde sağlanan RUS yalnızca o kapsayıcı üzerinde gerçekleştirilen işlemler için kullanılabilir. Bir veritabanında sağlanan RUs, söz konusu veritabanındaki tüm kapsayıcılar arasında paylaşılır (münhasıran atanmış RUs'lu kaplar hariç).

Elastik ölçekleme sağlanan üretim için, verilen RU/s'yi istediğiniz zaman artırabilir veya azaltabilirsiniz. Daha fazla bilgi için, Cosmos kapları ve veritabanlarını elastik olarak ölçeklendirmek için [nasıl sağolunması](set-throughput.md) gerektiğini görün. Genel olarak ölçekleme iş için, istediğiniz zaman Cosmos hesabınızdan bölgeler ekleyebilir veya kaldırabilirsiniz. Daha fazla bilgi için veritabanı [hesabınızdan bölgeler ekle/kaldır'a](how-to-manage-database-account.md#addremove-regions-from-your-database-account)bakın. Bir Cosmos hesabı ile birden fazla bölge ilişkilendirmek birçok senaryoda önemlidir - dünya çapında düşük gecikme ve [yüksek kullanılabilirlik](high-availability.md) elde etmek için.

## <a name="how-provisioned-throughput-is-distributed-across-regions"></a>Sağlanan iş girdisi bölgeler e göre nasıl dağıtılır?

Cosmos'un bir kapsayıcıya (veya *veritabanına) 'R'* RUs'u sağlarsanız, Cosmos DB, Cosmos hesabınızla ilişkili *her* bölgede *'R'* RUs'un kullanılabilir olmasını sağlar. Hesabınıza her yeni bir bölge eklediğinizde, Cosmos DB yeni eklenen bölgede otomatik olarak *'R'* RUs'u karşılar. Cosmos konteynerinize karşı gerçekleştirilen operasyonların her bölgede *'R'* RUs alması garanti edilir. Belirli bir bölgeye seçici olarak RUS atayamazsınız. Cosmos kapsayıcısında (veya veritabanında) sağlanan RUS, Cosmos hesabınızla ilişkili tüm bölgelerde sağlanmıştır.

Bir Cosmos kapsayıcı *'R'* RUs ile yapılandırılmış ve Cosmos hesabı ile ilişkili *'N'* bölgeleri olduğunu varsayarsak, o zaman:

- Cosmos hesabı tek bir yazma bölgesi ile yapılandırılırsa, toplam RUs kapsayıcı = *R* x *N*.

- Cosmos hesabı birden çok yazma bölgesiyle yapılandırılırsa, toplam RUS'lar kapsayıcı = *R* x *(N*+1) üzerinde kullanılabilir. Ek *R RUs* otomatik olarak bölgeler arasında güncelleştirme çakışmaları ve anti-entropi trafiği işlemek için verilir.

[Tutarlılık modeli](consistency-levels.md) seçiminiz de iş bilgililiği etkiler. Daha güçlü tutarlılık düzeylerine (örn. *oturum,* *tutarlı önek* ve *nihai* tutarlılık) daha güçlü tutarlılık düzeylerine (örn. *sınırlı bayatlık* veya *güçlü* tutarlılık) kıyasla yaklaşık 2 kat okuma iş düzeyi alabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Daha sonra bir kapsayıcı veya veritabanında iş lerinizi nasıl yapılandırabileceğinizi öğrenebilirsiniz:

* [Kapsayıcılar ve veritabanları için iş verime alın ve ayarlayın](set-throughput.md) 

