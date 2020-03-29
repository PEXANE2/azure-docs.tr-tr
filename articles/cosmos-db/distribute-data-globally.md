---
title: Azure Cosmos DB ile verileri genel olarak dağıtma
description: Küresel olarak dağıtılan, çok modelli veritabanı hizmeti olan Azure Cosmos DB'nin küresel veritabanlarını kullanarak gezegen ölçeğinde coğrafi çoğaltma, çoklu master, başarısızlık ve veri kurtarma hakkında bilgi edinin.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.openlocfilehash: 84cd201cd758293082a61a87528332c7d8c58811
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76264314"
---
# <a name="global-data-distribution-with-azure-cosmos-db---overview"></a>Azure Cosmos DB ile küresel veri dağıtımı - Genel bakış

Bugünün uygulamaları son derece duyarlı ve her zaman online olması gereklidir. Düşük gecikme süresi ve yüksek kullanılabilirlik elde etmek için, bu uygulamaların örneklerinin kullanıcılarına yakın veri merkezlerinde dağıtılması gerekir. Bu uygulamalar genellikle birden çok veri merkezinde dağıtılır ve genel olarak dağıtılır. Genel olarak dağıtılmış uygulamalar, uygulamaların kullanıcılarına yakın olan veri kopyalarında çalışmasına olanak tanımak için verileri dünyanın her yerinde saydam olarak çoğaltabilen veritabanlarına ihtiyaç duyar. 

Azure Cosmos DB, düşük gecikme süresi, elastik iş lenebilirliği, veri tutarlılığı için iyi tanımlanmış semantik ve yüksek kullanılabilirlik sağlamak üzere tasarlanmış, küresel olarak dağıtılan bir veritabanı hizmetidir. Kısacası, uygulamanızın dünyanın herhangi bir yerinde hızlı yanıt süresine ihtiyacı varsa, her zaman çevrimiçi olması gerekiyorsa ve sınırsız ve esnek ölçeklenebilirlik gerektiren bir ürün elde etmek gerekirse, uygulamanızı Azure Cosmos DB'de oluşturmalısınız.

Veritabanlarınızı genel olarak dağıtılacak ve Azure bölgelerinden herhangi birinde kullanılabilecek şekilde yapılandırabilirsiniz. Gecikme yi azaltmak için verileri kullanıcılarınızın bulunduğu yere yakın yerleştirin. Gerekli bölgeleri seçmek, uygulamanızın genel erişimine ve kullanıcılarınızın bulunduğu yere bağlıdır. Cosmos DB, verileri Cosmos hesabınızla ilişkili tüm bölgelere saydam olarak çoğaltır. Genel olarak dağıtılan Azure Cosmos veritabanınızın ve uygulamanızın yerel olarak okuyup yazabileceği kapsayıcılarınızın tek bir sistem görüntüsünü sağlar. 

Azure Cosmos DB ile hesabınızla ilişkili bölgeleri istediğiniz zaman ekleyebilir veya kaldırabilirsiniz. Bir bölge eklemek veya kaldırmak için uygulamanızın duraklatılması veya yeniden dağıtılması gerekmez. Hizmetin yerel olarak sağladığı çoklu homing yetenekleri nedeniyle her zaman kullanılabilir olmaya devam eder.

![Yüksek kullanılabilir dağıtım topolojisi](./media/distribute-data-globally/deployment-topology.png)

## <a name="key-benefits-of-global-distribution"></a>Küresel dağılımın temel faydaları

**Genel aktif uygulamalar oluşturun.** Onun yeni çok ana çoğaltma protokolü ile, her bölge hem yazma ve okur destekler. Çoklu master özelliği de şunları sağlar:

- Sınırsız elastik yazmak ve ölçeklenebilirlik okuyun. 
- 99.999% okumak ve tüm dünyada kullanılabilirlik yazmak.
- Garantili okuma ve yazma 99 yüzdelik olarak 10 milisaniyeden daha az bir sürede servis edilir.

Azure Cosmos DB çoklu homing API'lerini kullanarak, uygulamanız en yakın bölgenin farkındadır ve bu bölgeye istek gönderebilir. En yakın bölge herhangi bir yapılandırma değişikliği olmadan tanımlanır. Azure Cosmos hesabınıza bölgeler ekleyip kaldırdığınızda, uygulamanızın yeniden dağıtılması veya duraklatılması gerekmez, her zaman kullanılabilir olmaya devam eder.

**Son derece duyarlı uygulamalar oluşturun.** Uygulamanız, veritabanınız için seçtiğiniz tüm bölgelere karşı neredeyse gerçek zamanlı okuma ve yazma gerçekleştirebilir. Azure Cosmos DB, seçtiğiniz düzeyin tutarlılık düzeyi garantileriyle bölgeler arasındaki veri çoğaltma işlemini dahili olarak işler.

**Yüksek kullanılabilir uygulamalar oluşturun.** Dünya çapında birden çok bölgede veritabanı çalıştırmak, bir veritabanının kullanılabilirliğini artırır. Bir bölge kullanılamıyorsa, diğer bölgeler uygulama isteklerini otomatik olarak işler. Azure Cosmos DB, çok bölgeli veritabanları için %99,999 okuma ve yazma kullanılabilirliği sunar.

**Bölgesel kesintiler sırasında iş sürekliliğini koruyun.** Azure Cosmos DB, bölgesel bir kesinti sırasında [otomatik olarak başarısızlığı](how-to-manage-database-account.md#automatic-failover) destekler. Azure Cosmos DB, bölgesel bir kesinti sırasında gecikme süresi, kullanılabilirlik, tutarlılık ve iş çıkış SLA'larını korumaya devam ediyor. Cosmos DB, tüm uygulamanızın son derece kullanılabilir olduğundan emin olmak için bölgesel bir kesintiyi simüle etmek için manuel bir hata API sunar. Bu API'yi kullanarak, düzenli iş sürekliliği matkapları gerçekleştirebilirsiniz.

**Genel olarak okuma ve yazma iş lerini ölçeklendirin.** Her bölgenin yazılabilir ve elastik ölçek tüm dünyada okur ve yazıyor olmasını sağlayabilirsiniz. Uygulamanızın Bir Azure Cosmos veritabanında veya kapsayıcıda yapılandırdığı iş, Azure Cosmos hesabınızla ilişkili tüm bölgelere teslim edilmesi garanti edilir. Sağlanan iş [kolu, mali destekli SLA'lar](https://aka.ms/acdbsla)tarafından garanti edilir.

**Birkaç iyi tanımlanmış tutarlılık modelleri arasından seçim yapın.** Azure Cosmos DB çoğaltma protokolü iyi tanımlanmış, pratik ve sezgisel beş tutarlılık modeli sunar. Her modeltutarlılık ve performans arasında bir tradeoff vardır. Genel olarak dağıtılmış uygulamaları kolaylıkla oluşturmak için bu tutarlılık modellerini kullanın.

## <a name="next-steps"></a><a id="Next Steps"></a>Sonraki adımlar

Aşağıdaki makalelerde küresel dağıtım hakkında daha fazla bilgi edinin:

* [Genel dağıtım - başlık altında](global-dist-under-the-hood.md)
* [Uygulamalarınızda çoklu master nasıl yapılandırılatır](how-to-multi-master.md)
* [Çok eşleme için istemcileri yapılandırın](how-to-manage-database-account.md#configure-multiple-write-regions)
* [Azure Cosmos DB hesabınızdan bölgeler ekleme veya kaldırma](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [SQL API hesapları için özel bir çakışma çözümlemesi ilkesi oluşturma](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)
* [Cosmos DB'de programlanabilir tutarlılık modelleri](consistency-levels.md)
* [Uygulamanız için doğru tutarlılık düzeyini seçin](consistency-levels-choosing.md)
* [Azure Cosmos DB API'lerinde tutarlılık düzeyleri](consistency-levels-across-apis.md)
* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans dengeleri](consistency-levels-tradeoffs.md)

