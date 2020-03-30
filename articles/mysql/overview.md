---
title: Genel Bakış - MySQL için Azure Veritabanı
description: Microsoft bulutundaki MySQL Community Edition'ı temel alan ilişkisel bir veritabanı hizmeti olan MySQL için Azure Veritabanı hakkında bilgi edinin.
author: ajlam
ms.service: mysql
ms.author: andrela
ms.custom: mvc
ms.topic: overview
ms.date: 3/18/2020
ms.openlocfilehash: 8f49811ad0d40c70933d32227cfb17a5144b857a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80067815"
---
# <a name="what-is-azure-database-for-mysql"></a>MySQL için Azure Veritabanı nedir?

MySQL için Azure Veritabanı, Microsoft bulutundaki [MySQL Community Edition](https://www.mysql.com/products/community/) (GPLv2 lisansı altında kullanılabilir) veritabanı altyapısı, 5.6, 5.7 ve 8.0 sürümlerine dayalı ilişkisel bir veritabanı hizmetidir. MySQL için Azure Veritabanı şunları sunar:

- Ek ücret ödemeden yerleşik yüksek kullanılabilirlik.
- Kapsamlı kullandıkça öde fiyatlandırması kullanılarak öngörülebilir performans.
- Saniyeler içinde gerektiği gibi ölçeklendirin.
- Bekleyen ve hareket halindeki hassas verileri korumaya yönelik güvenlik.
- Otomatik yedeklemeler ve 35 güne kadar belirli bir noktaya geri yükleme.
- Kurumsal düzeyde güvenlik ve uyumluluk.

Bu özellikler neredeyse hiç yönetim gerektirmez ve tümüyle ek ücret ödemeden sağlanır. Bunlar sayesinde değerli zamanınızı ve kaynaklarınızı sanal makine ve altyapı yönetimi yerine hızlı uygulama geliştirmeye ve piyasaya sunma sürenizi kısaltmaya ayırabilirsiniz. Buna ek olarak, kendi seçtiğiniz açık kaynak araçları ve platformuyla uygulamanızı geliştirmeye devam edebilir, böylelikle yeni beceriler edinmek zorunda kalmadan işlerinizin gerektirdiği hızla ve verimlilikle kullanıma sunabilirsiniz.

![MySQL kavramsal diyagramı için Azure Veritabanı](media/overview/1-azure-db-for-mysql-conceptual-diagram.png)

Bu makale, mysql temel kavramları ve performans, ölçeklenebilirlik ve yönetilebilirlik ile ilgili özellikler için Azure Veritabanı'na giriş tir ve ayrıntıları keşfetmek için bağlantılar içerir. Başlamanıza yardımcı olacak şu hızlı başlangıçlara bakın:

- [Azure portalını kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-portal.md)
- [Azure CLI aracını kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-cli.md)

Azure CLI örnekleri için bkz:

- [MySQL için Azure Veritabanı’na yönelik Azure CLI örnekleri](sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>Saniyeler içinde performansı ve ölçeği ayarlama
MySQL için Azure Veritabanı hizmeti, temel, genel amaç ve bellek optimize edilmiş çeşitli hizmet katmanları sunar. Her katman, hafiften ağıra kadar tüm iş yüklerini desteklemek üzere farklı performans ve özellikler getirir. İlk uygulamanızı aylık birkaç dolar ücretle küçük bir veritabanı üzerinde oluşturabilir ve sonra çözümünüzün gereksinimlerine göre ölçeği ayarlayabilirsiniz. Dinamik ölçeklendirebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine saydam bir şekilde yanıt verebilmesini sağlar. Yalnızca ihtiyacınız olan kaynaklar için ve yalnızca bunlara ihtiyacınız olduğunda ödeme yaparsınız. Ayrıntılar için [Fiyatlandırma katmanlarına](concepts-service-tiers.md) bakın.

## <a name="monitoring-and-alerting"></a>İzleme ve uyarı
Ne zaman artırılacağına ve ne zaman azaltılacağını nasıl karar verirsiniz? Yerleşik performans izleme ve uyarı özelliklerini, sanal çekirdekleri temel alan performans değerlendirmeleriyle birlikte kullanırsınız. Bu araçları kullanarak geçerli veya projeye özgü performans ihtiyaçlarınıza göre sanal çekirdeklerin ölçeğini büyütme veya küçültme işlemlerinin etkisini hızla değerlendirebilirsiniz. Ayrıntılar için bkz. [Uyarılar](howto-alert-on-metric.md).

## <a name="keep-your-app-and-business-running"></a>Uygulamanızın ve işinizin hiç kesintiye uğramamasını sağlayın
Azure'ın Microsoft yönetimindeki veri merkezlerinin küresel bir ağı tarafından desteklenen ve endüstri lideri niteliğinde %99,99'luk bir kullanılabilirlik oranı sunan hizmet düzeyi sözleşmesi (SLA), uygulamanızın 7/24 kesintiye uğramamasına yardımcı olur. MySQL sunucusu için her Azure Veritabanında, aksi takdirde satın almak veya tasarlamak, oluşturmak ve yönetmek zorunda kalacağınız yerleşik güvenlik, hata toleransı ve veri korumasından yararlanırsınız. MySQL için Azure Veritabanı ile, bir sunucunun 35 güne kadar daha eski bir durumuna kurtarılması için zaman içinde nokta geri yüklemesini kullanabilirsiniz.

## <a name="secure-your-data"></a>Verilerinizin güvenliğini sağlama
Azure veritabanı hizmetleri, erişimi sınırlayan, verileri istirahatte ve hareket halinde olan verileri koruyan ve etkinliği izlemenize yardımcı olan özelliklere sahip MySQL veritabanının onaydığı bir veri güvenliği geleneğine sahiptir. Azure'ın platform güvenliği hakkında bilgi edinmek için [Azure Güven Merkezi](https://www.microsoft.com/trustcenter/security)'ni ziyaret edin. MySQL güvenlik özellikleri için Azure Veritabanı hakkında daha fazla bilgi için [güvenlik genel bakışına](concepts-security.md)bakın.

## <a name="contacts"></a>Kişiler
MySQL için Azure Veritabanı ile çalışma nızla ilgili sorularınız veya önerileriniz için MySQL Ekibi için Azure Veritabanı'na[ @Ask (MySQL için Azure DB)](mailto:AskAzureDBforMySQL@service.microsoft.com)bir e-posta gönderin. Bu e-posta adresi teknik destek takma adı değildir.

Buna ek olarak, aşağıdaki iletişim noktalarını uygun şekilde göz önünde bulundurun:

- Azure Desteği ile iletişim kurmak için [Azure portaldan bir bilet oluşturun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Hesabınızla ilgili bir sorun gidermek için Azure portalda bir [destek isteği](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) oluşturun.
- Görüş bildirmek veya yeni özellikler istemek için [UserVoice](https://feedback.azure.com/forums/597982-azure-database-for-mysql) aracılığıyla bir giriş oluşturun.

## <a name="next-steps"></a>Sonraki adımlar
Şimdi MySQL için Azure Veritabanı'na giriş okudunuz ve "MySQL için Azure Veritabanı nedir?" sorusunu yanıtladınız. :

- Maliyet karşılaştırmaları ve hesaplayıcıları için fiyatlandırma sayfasına bakın. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/mysql/)
- İlk sunucunuzu oluşturarak başlayın. [Azure portalını kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-portal.md)
- Tercih ettiğiniz dili kullanarak ilk uygulamanızı oluşturun: [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md) | [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [Go](./connect-go.md)
