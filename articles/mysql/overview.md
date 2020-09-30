---
title: Genel Bakış-MySQL için Azure veritabanı
description: Microsoft bulutundaki MySQL Community sürümünü temel alan ilişkisel bir veritabanı hizmeti olan MySQL için Azure veritabanı hizmeti hakkında bilgi edinin.
author: savjani
ms.service: mysql
ms.author: pariks
ms.custom: mvc
ms.topic: overview
ms.date: 3/18/2020
ms.openlocfilehash: 51c194ca9b091bc685f293320750da55925ad49d
ms.sourcegitcommit: f796e1b7b46eb9a9b5c104348a673ad41422ea97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2020
ms.locfileid: "91565624"
---
# <a name="what-is-azure-database-for-mysql"></a>MySQL için Azure Veritabanı nedir?

MySQL için Azure veritabanı, [MySQL Community Edition](https://www.mysql.com/products/community/) 'Da (GPLv2 License) veritabanı altyapısı, sürüm 5,6, 5,7 ve 8,0 temel alınarak Microsoft bulutundaki bir ilişkisel veritabanı hizmetidir. MySQL için Azure Veritabanı şu özellikleri sunar:

- Yerleşik yüksek kullanılabilirlik.
- Otomatik yedeklemeler ve en çok 35 güne kadar bir noktadan sonra geri yükleme kullanarak veri koruma.
- Hizmeti güvenli ve güncel tutmak için temel alınan donanım, işletim sistemi ve veritabanı altyapısı için otomatik bakım.
- Kullandıkça öde fiyatlandırması ile tahmin edilebilir performans.
- Saniyeler içinde elastik ölçekleme.
- Sunucuyu durdurma/başlatma yeteneğine sahip maliyet iyileştirme denetimleri. 
- Önemli verileri REST ve hareket halindeyken korumak için kurumsal düzeyde güvenlik ve sektör lideri uyumluluk.
- Büyük ölçekli dağıtımlar için yönetimi ve izlemeyi basitleştirmek üzere izleme ve otomatikleştirme.
- Sektör lideri destek deneyimi.

Bu özellikler neredeyse hiç yönetim gerektirmez ve tümüyle ek ücret ödemeden sağlanır. Bunlar sayesinde değerli zamanınızı ve kaynaklarınızı sanal makine ve altyapı yönetimi yerine hızlı uygulama geliştirmeye ve piyasaya sunma sürenizi kısaltmaya ayırabilirsiniz. Buna ek olarak, kendi seçtiğiniz açık kaynak araçları ve platformuyla uygulamanızı geliştirmeye devam edebilir, böylelikle yeni beceriler edinmek zorunda kalmadan işlerinizin gerektirdiği hızla ve verimlilikle kullanıma sunabilirsiniz.

:::image type="content" source="media/overview/1-azure-db-for-mysql-conceptual-diagram.png" alt-text="MySQL için Azure veritabanı kavramsal diyagramı":::

## <a name="deployment-models"></a>Dağıtım modelleri

MySQL Community Edition tarafından desteklenen MySQL için Azure veritabanı, iki dağıtım modunda sunulmaktadır:
- Tek sunucu 
- Esnek Sunucu (Önizleme)
  
### <a name="azure-database-for-mysql---single-server"></a>MySQL için Azure veritabanı-tek sunucu

MySQL için Azure veritabanı tek sunucu, veritabanı özelleştirmeleri için en az gereksinimle tam olarak yönetilen bir veritabanı hizmetidir. Tek sunucu platformu, düzeltme eki uygulama, yedekleme, yüksek kullanılabilirlik, en az Kullanıcı Yapılandırması ve denetimiyle güvenlik gibi veritabanı yönetim işlevlerinin çoğunu işleyecek şekilde tasarlanmıştır. Mimari, tek kullanılabilirlik bölgesinde% 99,99 kullanılabilirlik ile yerleşik yüksek kullanılabilirlik için iyileştirilmiştir. MySQL 5,6, 5,7 ve 8,0 topluluk sürümünü destekler. Hizmet, günümüzde çok çeşitli [Azure bölgelerinde](https://azure.microsoft.com/global-infrastructure/services/)kullanılabilir.

Tek Sunucu dağıtım seçeneği şu üç fiyatlandırma katmanına sahiptir: Temel, Genel Amaçlı ve Bellek İçin İyileştirilmiş. Her katman veritabanı iş yükünüzü desteklemek için farklı kaynak özellikleri sunar. İlk uygulamanızı aylık birkaç dolar ücretle küçük bir veritabanı üzerinde oluşturabilir ve sonra çözümünüzün gereksinimlerine göre ölçeği ayarlayabilirsiniz. Dinamik ölçeklendirebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine saydam bir şekilde yanıt verebilmesini sağlar. Yalnızca ihtiyacınız olan kaynaklar için ve yalnızca bunlara ihtiyacınız olduğunda ödeme yaparsınız. Ayrıntılar için bkz. [Fiyatlandırma katmanları](concepts-pricing-tiers.md).

Tek sunucular, düzeltme eki uygulama ve özel MySQL yapılandırma ayarları üzerinde ayrıntılı denetim gereksinimi olmadan otomatik düzeltme eki uygulamayı işleyecek şekilde tasarlanan bulut Yerel uygulamaları için idealdir. 

Tek sunuculu dağıtım moduna yönelik ayrıntılı genel bakış için [tek sunucuya genel bakış](single-server-overview.md)konusuna bakın.

### <a name="azure-database-for-mysql---flexible-server-preview"></a>MySQL için Azure veritabanı-esnek sunucu (Önizleme)

MySQL için Azure veritabanı esnek sunucu, veritabanı yönetim işlevleri ve yapılandırma ayarları üzerinde daha ayrıntılı denetim ve esneklik sağlamak için tasarlanmış, tam olarak yönetilen bir veritabanı hizmetidir. Genel olarak, hizmet kullanıcı gereksinimlerine göre daha fazla esneklik ve özelleştirmeler sağlar. Esnek sunucu mimarisi, kullanıcıların tek kullanılabilirlik bölgesinde ve birden çok kullanılabilirlik alanında yüksek kullanılabilirliği kabul etmesine olanak tanır. Esnek sunucular, sürekli olarak tam işlem kapasitesi gerektirmeyen iş yükleri için ideal olan sunucu ve Burstable işlem katmanını durdurma/başlatma yeteneğine sahip daha iyi maliyet iyileştirme denetimleri sağlar. Hizmet şu anda MySQL 5,7 'in topluluk sürümünü yakında daha yeni sürümler eklemeye yönelik planlar ile desteklemektedir. Hizmet şu anda genel önizleme aşamasındadır ve çok çeşitli [Azure bölgelerinde](https://azure.microsoft.com/global-infrastructure/services/)kullanılabilir.

Esnek sunucular için en uygun 
- Daha iyi denetim ve özelleştirmeler gerektiren uygulama geliştirmeleri.
- Bölge yedekli yüksek kullanılabilirlik
- Yönetilen bakım pencereleri

Esnek sunucu dağıtım moduna ayrıntılı genel bakış için bkz. [esnek sunucuya genel bakış](flexible-server/overview.md).

## <a name="contacts"></a>Kişiler
MySQL için Azure veritabanı ile çalışmaya yönelik herhangi bir soru veya öneri için, MySQL için Azure veritabanı ekibine bir e-posta gönderin ([ @Ask MySQL IÇIN Azure DB](mailto:AskAzureDBforMySQL@service.microsoft.com)). Bu e-posta adresi bir teknik destek diğer adı değil.

Buna ek olarak, aşağıdaki iletişim noktalarını uygun şekilde göz önünde bulundurun:

- Azure Desteği ile iletişim kurmak için [Azure portaldan bir bilet oluşturun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Hesabınızla ilgili bir sorun gidermek için Azure portalda bir [destek isteği](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) oluşturun.
- Görüş bildirmek veya yeni özellikler istemek için [UserVoice](https://feedback.azure.com/forums/597982-azure-database-for-mysql) aracılığıyla bir giriş oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

MySQL için Azure veritabanı 'nın iki dağıtım modu hakkında daha fazla bilgi edinin ve gereksinimlerinize göre doğru seçenekleri seçin.

- [Tek sunucu](single-server/index.yml)
- [Esnek sunucu](flexible-server/index.yml)
- [İş yükünüz için doğru MySQL dağıtım seçeneğini belirleyin](select-right-deployment-type.md)
