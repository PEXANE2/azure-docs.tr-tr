---
title: Azure Cosmos DB'de geliştirme ve test için optimizasyon
description: Bu makalede, Azure Cosmos DB'nin hizmetin geliştirilmesi ve test edilmesi için nasıl ücretsiz olarak birden çok seçenek sunduğu açıklanmaktadır.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.openlocfilehash: d2ca7b7e4b637802df6a78c2493e3cc088f09881
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79246688"
---
# <a name="optimize-development-and-testing-cost-in-azure-cosmos-db"></a>Azure Cosmos DB'de geliştirme ve test maliyetlerini iyileştirme

Bu makalede, geliştirme ve test için Azure Cosmos DB'yi ücretsiz olarak kullanmak için farklı seçeneklerin yanı sıra geliştirme veya test hesaplarındamaliyeti optimize etme tekniklerini açıklanmaktadır.

## <a name="azure-cosmos-db-emulator-locally-downloadable-version"></a>Azure Cosmos DB emülatörü (yerel olarak indirilebilir sürüm)

[Azure Cosmos DB emülatörü,](local-emulator.md) Azure Cosmos DB bulut hizmetini taklit eden yerel bir indirilebilir sürümdür. Ağ bağlantınız olmasa ve herhangi bir ücret ödemeden Azure Cosmos DB API'lerini kullanan kod yazabilir ve test edebilirsiniz. Azure Cosmos DB emülatörü, bulut hizmetine yüksek doğrulukla geliştirme amacıyla yerel bir ortam sağlar. Azure aboneliği oluşturmadan uygulamanızı yerel olarak geliştirebilir ve test edebilirsiniz. Uygulamanızı buluta dağıtmaya hazır olduğunuzda, buluttaki Azure Cosmos DB bitiş noktasına bağlanmak için bağlantı dizesini güncelleştirdiğinizde, başka değişiklik gerekmez. Ayrıca, testleri çalıştırmak için Azure DevOps'te [Azure Cosmos DB emülatörü oluşturma göreviyle bir CI/CD ardışık yapı hattı da](tutorial-setup-ci-cd.md) ayarlayabilirsiniz. [Azure Cosmos DB emülatörü](local-emulator.md) makalesini ziyaret ederek işe katılabilirsiniz.

## <a name="azure-cosmos-db-free-tier"></a>Azure Cosmos DB ücretsiz katman 
Azure Cosmos DB ücretsiz katman, uygulamalarınızı başlatmayı, geliştirmeyi ve test etmeyi ve hatta küçük üretim iş yüklerini ücretsiz olarak çalıştırmayı kolaylaştırır. Bir hesapta ücretsiz katman etkinleştirildiğinde, hesapta ilk 400 RU/s ve 5 GB depolama alanını ücretsiz alırsınız. Ayrıca, veritabanı düzeyinde 400 RU/s'yi paylaşan 25 kapsayıcıiçeren ve tümü ücretsiz katman (ücretsiz katman hesabında paylaşılan 5 paylaşılan iş düzeyi veritabanlarını sınırla) kapsamında paylaşılan bir iş düzeyi veritabanı oluşturabilirsiniz. Ücretsiz katman, hesabın ömrü boyunca süresiz olarak devam eder ve sınırsız depolama ve iş fazlası (RU/s), SLA'lar, yüksek kullanılabilirlik, tüm Azure bölgelerinde anahtar teslimküresel dağıtım ve daha fazlası dahil olmak üzere normal bir Azure Cosmos DB hesabının tüm avantaj ve [özellikleriyle](introduction.md#key-benefits) birlikte gelir. Azure aboneliği başına en fazla bir ücretsiz katman hesabınız olabilir ve hesabı oluştururken kabul etmeniz gerekir. Başlamak için, [ücretsiz katman etkin leştirilmiş yeni bir hesap oluşturun.](create-cosmosdb-resources-portal.md) Daha ayrıntılı bilgi edinmek için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) bakın. 

## <a name="try-azure-cosmos-db-for-free"></a>Azure Cosmos DB’yi ücretsiz deneyin

[Azure Cosmos DB'yi ücretsiz olarak deneyin,](https://azure.microsoft.com/try/cosmosdb/) azure hesabına kaydolmadan veya kredi kartınızı kullanmadan bulutta Azure Cosmos DB ile denemeler yapmanızı sağlayan ücretsiz bir deneyimdir. Azure Cosmos DB hesaplarını deneyin, şu anda 30 gün olmak üzere sınırlı bir süre için kullanılabilir. İstediğiniz zaman yenileyebilirsiniz. Azure Cosmos DB hesaplarını deneyin, Azure Cosmos DB'yi değerlendirmeyi, bir uygulama oluşturmayı ve test etmeyi veya Hızlı Başlangıçları veya öğreticileri kullanmayı kolaylaştırır. Ayrıca bir demo oluşturabilir, birim testi yapabilir, hatta çok bölgeli bir hesap oluşturabilir ve herhangi bir ücret emeden bir uygulama çalıştırabilirsiniz. Azure Cosmos DB'yi Deneyin hesabında, en fazla 25 kapsayıcı ve 20.000 RU/s'lik bir verim içeren paylaşılan bir iş veri tabanı veya 5000 RU/s'ye kadar bir kapsayıcı nız olabilir. Başlamak için ücretsiz sayfa [için Azure Cosmos DB'yi deneyin](https://azure.microsoft.com/try/cosmosdb/) sayfasına bakın.

## <a name="azure-free-account"></a>Ücretsiz Azure hesabı

Azure Cosmos DB, Azure kredileri ve kaynakları belirli bir süre için ücretsiz olarak sunan [Azure ücretsiz hesabına](https://azure.microsoft.com/free)dahildir. Azure Cosmos DB için özel olarak bu ücretsiz hesap, tüm yıl boyunca 5 GB depolama alanı ve 400 RUs'luk garantili iş ortası sunar. Bu deneyim, herhangi bir geliştiricinin Azure Cosmos DB'nin özelliklerini kolayca sınamasını veya sıfır maliyetle diğer Azure hizmetleriyle tümleştirmesini sağlar. Azure ücretsiz hesabı yla, ilk 30 gün içinde harcayabileceğiniz 200 TL'lik bir kredi alırsınız. Yükseltmeyi seçene kadar hizmetleri kullanmaya başlasanız bile ücretlendirilmezsiniz. Başlamak için [Azure ücretsiz hesap](https://azure.microsoft.com/free) sayfasını ziyaret edin.

## <a name="use-shared-throughput-databases"></a>Paylaşılan iş oluşturma veritabanlarını kullanma

Paylaşılan bir [iş veri tabanında,](set-throughput.md#set-throughput-on-a-database)veritabanı içindeki tüm kapsayıcılar veritabanının sağlanan iş veritabanını (RU/s) paylaşır. Örneğin, 400 RU/s içeren bir veritabanı sağlarsanız ve dört kapsayıcınız varsa, dört kapsayıcı da 400 RU/s'yi paylaşır. Her kapsayıcıya daha az sıklıkta erişilebildiği ve bu nedenle en az 400 RU/s'den daha düşük ihtiyaç duyacakları bir geliştirme veya sınama ortamında, kapsayıcıları paylaşılan bir üretim veri tabanına koymak maliyeti optimize etmeye yardımcı olabilir. 

Örneğin, geliştirme veya test hesabınızın dört kapsayıcısı olduğunu varsayalım. Özel iş bünyesiyle (en az 400 RU/s) dört kapsayıcı oluşturursanız, toplam RU/s'leriniz 1600 RU/s olacaktır. Buna karşılık, paylaşılan bir iş veri tabanı (en az 400 RU/s) oluşturur ve konteynerlerinizi oraya koyarsanız, toplam RU/s'leriniz sadece 400 RU/s olacaktır. Genel olarak, paylaşılan iş verme veritabanları, tek bir kapsayıcıda garantili iş başına ihtiyacınız olmayan senaryolar için harikadır.  Paylaşılan iş oluşturma veritabanları hakkında daha fazla bilgi [edinin.](set-throughput.md#set-throughput-on-a-database)

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerle emülatör veya ücretsiz Azure Cosmos DB hesaplarını kullanmaya başlayabilirsiniz:

* [Geliştirme ve test için Optimizasyon](optimize-dev-test.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB faturanızı anlama](understand-your-bill.md) hakkında daha fazla bilgi edinin
* [İş memat maliyetini optimize etme](optimize-cost-throughput.md) hakkında daha fazla bilgi edinin
* [Depolama maliyetini optimize etme](optimize-cost-storage.md) hakkında daha fazla bilgi edinin
* [Okuma ve yazma maliyetini optimize etme](optimize-cost-reads-writes.md) hakkında daha fazla bilgi edinin
* [Sorguların maliyetini optimize etme](optimize-cost-queries.md) hakkında daha fazla bilgi edinin
* [Çok bölgeli Azure Cosmos hesaplarının maliyetini optimize etme](optimize-cost-regions.md) hakkında daha fazla bilgi edinin

