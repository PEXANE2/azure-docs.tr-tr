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
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74481659"
---
# <a name="what-is-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı nedir?
PostgreSQL için Azure Veritabanı, geliştiriciler için oluşturulmuş Microsoft bulutundaki ilişkisel bir veritabanı hizmetidir. Açık kaynak [postgreSQL](https://www.postgresql.org/) veritabanı altyapısının topluluk sürümüne dayanır ve iki dağıtım seçeneğinde kullanılabilir: Tek Sunucu ve Hyperscale (Citus).

## <a name="azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanı - Tek Sunucu
Tek Sunucu dağıtım seçeneği sunar:

- Ek ücret ödemeden yerleşik [yüksek kullanılabilirlik](concepts-high-availability.md) (%99,99 SLA)
- Kapsamlı kullandıkça öde fiyatlandırması kullanılarak öngörülebilir performans
- Saniyeler içinde [gerektiği gibi dikey ölçek](concepts-pricing-tiers.md)
- Sunucunuzu değerlendirmek için [izleme ve uyarı](concepts-monitoring.md)
- Kurumsal düzeyde güvenlik ve uyumluluk
- Hassas verileri istirahat ve hareket halinde [korumak için güvenli](concepts-security.md)
- [Otomatik yedeklemeler ve](concepts-business-continuity.md) 35 güne kadar zaman içinde geri yükleme


Tüm bu özellikler neredeyse hiç yönetim gerektirmez ve tümüyle ek ücret ödemeden sağlanır. Sanal makineleri ve altyapıyı yönetmek için değerli zaman ve kaynaklar harcamak yerine hızlı uygulama geliştirmeye ve pazara zaman ayırmaya odaklanmanızı sağlarlar. Yeni beceriler öğrenmek zorunda kalmadan, açık kaynak araçları ve seçtiğiniz platform ile uygulamanızı geliştirmeye devam edebilirsiniz.

Tek Sunucu dağıtım seçeneği üç fiyatlandırma katmanı sunar: Temel, Genel Amaç ve Bellek Optimize Edilmiştir. Her katman veritabanı iş yükünüzü desteklemek için farklı kaynak özellikleri sunar. İlk uygulamanızı aylık birkaç dolar ücretle küçük bir veritabanı üzerinde oluşturabilir ve sonra çözümünüzün gereksinimlerine göre ölçeği ayarlayabilirsiniz. Dinamik ölçeklendirebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine saydam bir şekilde yanıt verebilmesini sağlar. Yalnızca ihtiyacınız olan kaynaklar için ve yalnızca bunlara ihtiyacınız olduğunda ödeme yaparsınız. Ayrıntılar için [Fiyatlandırma katmanlarına](concepts-pricing-tiers.md) bakın.

## <a name="azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL için Azure Veritabanı - Hyperscale (Citus)
Hyperscale (Citus) seçeneği, parçalama özelliğini kullanarak sorguları birden çok makinede yatay olarak ölçeklendiriyor. Sorgu altyapısı, büyük veri kümelerinde daha hızlı yanıtlar için bu sunucular arasında gelen SQL sorgularını paralelleştirir. Daha fazla ölçek ve performans gerektiren uygulamalara, genellikle 100 GB veriye yaklaşan veya zaten aşan iş yüklerine hizmet eder.

Hyperscale (Citus) dağıtım seçeneği şunları sağlar:

- Parçalama kullanarak birden fazla makinede yatay ölçekleme
- Büyük veri kümelerinde daha hızlı yanıtlar için bu sunucular arasında paralelleştirme sorgusu
- Çok kiracılı uygulamalar, gerçek zamanlı operasyonel analitik ve yüksek iş gücü işlem iş yükleri için mükemmel destek

PostgreSQL için oluşturulmuş uygulamalar, standart [bağlantı kitaplıkları](./concepts-connection-libraries.md) ve en az değişikliklerle Hyperscale'de (Citus) dağıtılmış sorgular çalıştırabilir.

## <a name="contacts"></a>Kişiler
PostgreSQL için Azure Veritabanı ile çalışma yla ilgili soru ve önerileriniz için PostgreSQL Ekibi için Azure Veritabanı'na[ @Ask (PostgreSQL için Azure DB)](mailto:AskAzureDBforPostgreSQL@service.microsoft.com)bir e-posta gönderin. Bu adres destek biletleri yerine genel sorular içindir.

Buna ek olarak, bu temas noktalarını uygun olarak düşünün:
- Azure Destek'e başvurmak veya hesabınızla ilgili bir sorunu gidermek için [Azure portalından bir bilet gönderin.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
- Görüş bildirmek veya yeni özellikler istemek için [UserVoice](https://feedback.azure.com/forums/597976-azure-database-for-postgresql) aracılığıyla bir giriş oluşturun.

## <a name="next-steps"></a>Sonraki adımlar
- Maliyet karşılaştırmaları ve hesaplayıcıları için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/postgresql/) bakın.
- PostgreSQL [Tek Sunucu](./quickstart-create-server-database-portal.md) veya [Hyperscale (Citus)](./quickstart-create-hyperscale-portal.md) için ilk Azure Veritabanınızı oluşturarak başlayın
- Python, PHP, Ruby, C\#, Java, Node.js'de ilk uygulamanızı oluşturun: [Bağlantı kitaplıkları](./concepts-connection-libraries.md)
