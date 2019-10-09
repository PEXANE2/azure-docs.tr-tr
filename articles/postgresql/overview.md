---
title: PostgreSQL için Azure veritabanı ilişkisel veritabanı hizmetine genel bakış
description: PostgreSQL için Azure veritabanı ilişkisel veritabanı hizmetine genel bir bakış sağlar.
author: jonels-msft
ms.author: jonels
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 05/06/2019
ms.openlocfilehash: 5194cf51fd7f1debeba76edb48e8377919ae448a
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72177961"
---
# <a name="what-is-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı nedir?
PostgreSQL için Azure veritabanı, geliştiriciler için derlenmiş Microsoft bulutundaki bir ilişkisel veritabanı hizmetidir. Bu, açık kaynak [PostgreSQL](https://www.postgresql.org/) veritabanı altyapısının topluluk sürümüne dayalıdır ve iki dağıtım seçeneklerinde mevcuttur: tek sunucu ve hiper ölçek (Citus) (Önizleme).

## <a name="azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı-tek sunucu
Tek sunuculu dağıtım seçeneği şunları sağlar:

- Ek ücret ödemeden yerleşik yüksek kullanılabilirlik (% 99,99 SLA)
- Kapsamlı Kullandıkça Öde fiyatlandırması kullanılarak öngörülebilir performans
- Saniyeler içinde gereken şekilde dikey ölçek
- Ölçeklendirmenin etkilerini hızlı bir şekilde değerlendirmek için izleme ve uyarma
- REST ve hareket halindeyken hassas verileri korumaya güvenli hale getirilmiş
- 35 güne kadar otomatik yedeklemeler ve zaman içinde geri yükleme
- Kurumsal düzeyde güvenlik ve uyumluluk

Tüm bu yetenekler neredeyse yönetim gerektirmez ve hepsi hiçbir ek ücret ödemeden sağlanır. Bu kişiler, sanal makineleri ve altyapıyı yönetmek için değerli zaman ve kaynakları harcamaktansa hızlı uygulama geliştirmeye odaklanmanıza ve zamanınızı pazara sunma sürecinize olanak tanır. Yeni beceriler öğrenmeye gerek kalmadan, seçtiğiniz açık kaynaklı araçlar ve platformla uygulamanızı geliştirmeye devam edebilirsiniz.

Tek sunuculu dağıtım seçeneği üç fiyatlandırma katmanı sunar: temel, Genel Amaçlı ve bellek için Iyileştirilmiş. Her katman, veritabanı iş yüklerinizi destekleyecek farklı kaynak özellikleri sunar. İlk uygulamanızı bir ay birkaç ABD Doları için küçük bir veritabanında oluşturabilir ve sonra ölçeklendirmeyi çözümünüzün ihtiyaçlarını karşılayacak şekilde ayarlayabilirsiniz. Dinamik ölçeklenebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine şeffaf bir şekilde yanıt vermesini sağlar. Yalnızca ihtiyacınız olan kaynaklar için ve yalnızca ihtiyacınız olduğunda ödeme yaparsınız. Ayrıntılar için bkz. [fiyatlandırma katmanları](concepts-pricing-tiers.md) .

## <a name="azure-database-for-postgresql---hyperscale-citus-preview"></a>PostgreSQL için Azure veritabanı-hiper ölçek (Citus) (Önizleme)
Hyperscale (Citus) seçeneği, parçaları kullanarak birden çok makine genelinde sorguları yatay olarak ölçeklendirir. Sorgu altyapısı, büyük veri kümelerinde daha hızlı yanıtlar için bu sunucular genelinde gelen SQL sorgularını paralelleştiriyor. Daha fazla ölçek ve performans gerektiren uygulamalara, genellikle yaklaştığı veya en fazla-100 GB veri aşmış olan iş yükleri sunar.

Hyperscale (Citus) dağıtım seçeneği şunları sağlar:

- Parçalara ayırma kullanılarak birden çok makine arasında yatay ölçeklendirme
- Büyük veri kümelerinde daha hızlı yanıtlar için bu sunucular genelinde paralelleştirme sorgula
- Çok kiracılı uygulamalar, gerçek zamanlı işlem analizi ve yüksek verimlilik işlem iş yükleri için mükemmel destek

PostgreSQL için derlenmiş uygulamalar, standart [bağlantı kitaplıkları](./concepts-connection-libraries.md) ve en az değişiklikle hiper ölçekte (Citus) dağıtılmış sorgular çalıştırabilir.

Hiper ölçeğin (Citus) genel önizlemede olduğunu ve bu nedenle henüz bir SLA sunmadığını unutmayın.

## <a name="data-security"></a>Veri güvenliği
PostgreSQL için Azure veritabanı, Azure veritabanı Hizmetleri ' gelenek of Data Security 'yi içerir. Erişimi sınırlayan, verileri REST ve hareket halindeyken koruyan ve etkinliği izlemenize yardımcı olan özelliklere sahiptir. Azure 'un Platform güvenliği hakkında bilgi edinmek için [Azure Güven Merkezi](https://azure.microsoft.com/overview/trusted-cloud/) ziyaret edin.

PostgreSQL için Azure veritabanı hizmeti, bekleyen verilerin depolama şifrelemesi için FIPS 140-2 tarafından doğrulanan şifreleme modülünü kullanır. Yedeklemeler de dahil olmak üzere veriler, sorgular çalıştırılırken oluşturulan geçici dosyalar hariç olmak üzere diskte şifrelenir. Hizmet, Azure depolama şifrelemesi 'ne dahil olan AES 256 bitlik şifrelemeyi kullanır ve anahtarlar sistem tarafından yönetilir. Depolama şifrelemesi her zaman açıktır ve devre dışı bırakılamaz. Varsayılan olarak, PostgreSQL için Azure veritabanı hizmeti, hem ağ hem de veritabanı ile istemci uygulaması arasında hareket halindeki veriler için güvenli bağlantılar gerektirir.

## <a name="contacts"></a>Kişiler
PostgreSQL için Azure veritabanı ile çalışma hakkında sorularınız veya öneriler için PostgreSQL için Azure veritabanı ekibine bir e-posta gönderin ([@Ask. PostgreSQL Için Azure DB](mailto:AskAzureDBforPostgreSQL@service.microsoft.com)). Bu adres destek bileti yerine genel sorulara yöneliktir.

Ayrıca, bu iletişim noktalarını uygun şekilde göz önünde bulundurun:
- Azure desteği ile iletişim kurmak veya hesabınızla ilgili bir sorunu sabitlemek için [Azure Portal bir bilet](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)gönderin.
- Geri bildirim sağlamak veya yeni özellikler istemek için [UserVoice](https://feedback.azure.com/forums/597976-azure-database-for-postgresql)aracılığıyla bir giriş oluşturun.

## <a name="next-steps"></a>Sonraki adımlar
- Maliyet karşılaştırmaları ve hesaplayıcıları için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/postgresql/) bakın.
- İlk PostgreSQL için Azure veritabanınızı [tek sunucu](./quickstart-create-server-database-portal.md) veya [hiper ölçek (Citus) oluşturarak başlayın (Önizleme)](./quickstart-create-hyperscale-portal.md)
- Python, PHP, Ruby, C @ no__t-0, Java, Node. js ' de ilk uygulamanızı oluşturun: [bağlantı kitaplıkları](./concepts-connection-libraries.md)
