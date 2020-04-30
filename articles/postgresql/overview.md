---
title: PostgreSQL için Azure Veritabanı ilişkisel veritabanı hizmetine genel bakış
description: PostgreSQL için Azure Veritabanı ilişkisel veritabanı hizmetine genel bir bakış sağlar.
author: jonels-msft
ms.author: jonels
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 11/25/2019
ms.openlocfilehash: 9ea0610811f6906526afe55d577e04a8decd5f49
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "74481659"
---
# <a name="what-is-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı nedir?
PostgreSQL için Azure veritabanı, geliştiriciler için derlenmiş Microsoft bulutundaki bir ilişkisel veritabanı hizmetidir. Bu, açık kaynak [PostgreSQL](https://www.postgresql.org/) veritabanı altyapısının topluluk sürümüne dayalıdır ve iki dağıtım seçeneklerinde mevcuttur: tek sunucu ve hiper ölçek (Citus).

## <a name="azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanı - Tek Sunucu
Tek sunuculu dağıtım seçeneği şunları sağlar:

- Ek ücret ödemeden yerleşik [yüksek kullanılabilirlik](concepts-high-availability.md) (% 99,99 SLA)
- Kapsamlı kullandıkça öde fiyatlandırması kullanılarak öngörülebilir performans
- Saniyeler içinde [gereken şekilde dikey ölçek](concepts-pricing-tiers.md)
- Sunucunuzu değerlendirmek için [izleme ve uyarma](concepts-monitoring.md)
- Kurumsal düzeyde güvenlik ve uyumluluk
- REST ve hareket halindeyken hassas verileri [korumaya güvenli hale getirilmiş](concepts-security.md)
- 35 güne kadar [otomatik yedeklemeler ve zaman içinde geri yükleme](concepts-business-continuity.md)


Tüm bu özellikler neredeyse hiç yönetim gerektirmez ve tümüyle ek ücret ödemeden sağlanır. Bu kişiler, sanal makineleri ve altyapıyı yönetmek için değerli zaman ve kaynakları harcamaktansa hızlı uygulama geliştirmeye odaklanmanıza ve zamanınızı pazara sunma sürecinize olanak tanır. Yeni beceriler öğrenmeye gerek kalmadan, seçtiğiniz açık kaynaklı araçlar ve platformla uygulamanızı geliştirmeye devam edebilirsiniz.

Tek sunuculu dağıtım seçeneği üç fiyatlandırma katmanı sunar: temel, Genel Amaçlı ve bellek için Iyileştirilmiş. Her katman veritabanı iş yükünüzü desteklemek için farklı kaynak özellikleri sunar. İlk uygulamanızı aylık birkaç dolar ücretle küçük bir veritabanı üzerinde oluşturabilir ve sonra çözümünüzün gereksinimlerine göre ölçeği ayarlayabilirsiniz. Dinamik ölçeklendirebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine saydam bir şekilde yanıt verebilmesini sağlar. Yalnızca ihtiyacınız olan kaynaklar için ve yalnızca bunlara ihtiyacınız olduğunda ödeme yaparsınız. Ayrıntılar için bkz. [fiyatlandırma katmanları](concepts-pricing-tiers.md) .

## <a name="azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL için Azure veritabanı-hiper ölçek (Citus)
Hyperscale (Citus) seçeneği, parçaları kullanarak birden çok makine genelinde sorguları yatay olarak ölçeklendirir. Sorgu altyapısı, büyük veri kümelerinde daha hızlı yanıtlar için bu sunucular genelinde gelen SQL sorgularını paralelleştiriyor. Daha fazla ölçek ve performans gerektiren uygulamalara, genellikle yaklaştığı veya en fazla-100 GB veri aşmış olan iş yükleri sunar.

Hyperscale (Citus) dağıtım seçeneği şunları sağlar:

- Parçalara ayırma kullanılarak birden çok makine arasında yatay ölçeklendirme
- Büyük veri kümelerinde daha hızlı yanıtlar için bu sunucular genelinde paralelleştirme sorgula
- Çok kiracılı uygulamalar, gerçek zamanlı işlem analizi ve yüksek verimlilik işlem iş yükleri için mükemmel destek

PostgreSQL için derlenmiş uygulamalar, standart [bağlantı kitaplıkları](./concepts-connection-libraries.md) ve en az değişiklikle hiper ölçekte (Citus) dağıtılmış sorgular çalıştırabilir.

## <a name="contacts"></a>Kişiler
PostgreSQL için Azure veritabanı ile çalışmaya yönelik herhangi bir soru veya öneri için, PostgreSQL için Azure veritabanı ekibine bir e-posta gönderin ([ @Ask PostgreSQL için Azure DB](mailto:AskAzureDBforPostgreSQL@service.microsoft.com)). Bu adres destek bileti yerine genel sorulara yöneliktir.

Ayrıca, bu iletişim noktalarını uygun şekilde göz önünde bulundurun:
- Azure desteği ile iletişim kurmak veya hesabınızla ilgili bir sorunu sabitlemek için [Azure Portal bir bilet](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)gönderin.
- Görüş bildirmek veya yeni özellikler istemek için [UserVoice](https://feedback.azure.com/forums/597976-azure-database-for-postgresql) aracılığıyla bir giriş oluşturun.

## <a name="next-steps"></a>Sonraki adımlar
- Maliyet karşılaştırmaları ve hesaplayıcıları için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/postgresql/) bakın.
- İlk PostgreSQL için Azure veritabanınızı [tek sunuculu](./quickstart-create-server-database-portal.md) veya [hiper ölçekte oluşturmaya başlayın (Citus)](./quickstart-create-hyperscale-portal.md)
- Python, PHP, Ruby, C\#, Java, Node.js'de ilk uygulamanızı oluşturun: [Bağlantı kitaplıkları](./concepts-connection-libraries.md)
