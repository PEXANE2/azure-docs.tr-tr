---
title: Azure Cosmos DB tüketim tabanlı sunucusuz teklif
description: Azure Cosmos DB tüketim tabanlı sunucusuz teklif hakkında daha fazla bilgi edinin.
author: ThomasWeiss
ms.author: thweiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/23/2020
ms.openlocfilehash: c5086eee805ffbcdf0741eae4db405b1bcbe8692
ms.sourcegitcommit: 6cca6698e98e61c1eea2afea681442bd306487a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/24/2020
ms.locfileid: "97760387"
---
# <a name="azure-cosmos-db-serverless-preview"></a>Azure Cosmos DB sunucusuz (Önizleme)
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

> [!IMPORTANT]
> Azure Cosmos DB sunucusuz Şu anda önizleme aşamasındadır. Bu önizleme sürümü bir Hizmet Düzeyi Sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Cosmos DB sunucusuz, Azure Cosmos hesabınızı, yalnızca veritabanı işlemleriniz tarafından tüketilen Istek birimleri ve verileriniz tarafından tüketilen depolama alanı için ücretlendirilebilen tüketim tabanlı bir biçimde kullanmanıza olanak sağlar. Sunucusuz kapsayıcılar, saniyede minimum ücret ödemeden ve kapasite planlaması gerekmeden binlerce istek sunabilir.

> [!IMPORTANT] 
> Sunucusuz hakkında geri bildiriminiz var mı? Duymak istiyoruz! Azure Cosmos DB sunucusuz ekibe bir ileti bırakmayı ücretsiz olarak hissetmekten çekinmeyin: [azurecosmosdbserverless@service.microsoft.com](mailto:azurecosmosdbserverless@service.microsoft.com) .

Azure Cosmos DB kullanırken, her veritabanı işleminin, [Istek birimlerinde](request-units.md)belirtilen bir maliyeti vardır. Bu maliyet için nasıl ücretlendirildiğiniz, kullanmakta olduğunuz Azure Cosmos hesabının türüne bağlıdır:

- [Sağlanan aktarım hızı](set-throughput.md) modunda, veritabanlarınızda ve kapsayıcılarınızda sağlanan belirli bir aktarım hızına (saniye başına istek birimi cinsinden ifade edilir) uygulamanız gerekir. Daha sonra veritabanı operasyonlarınızın maliyeti, her saniye kullanılabilir Istek birimi sayısından düşülür. Faturalama döneminizin sonunda, sağladığınız üretilen iş miktarı için faturalandırılırsınız.
- Sunucusuz modda, Azure Cosmos hesabınızda kapsayıcı oluştururken herhangi bir aktarım hızı sağlamanız gerekmez. Faturalama döneminizin sonunda, veritabanı işlemleriniz tarafından tüketilen Istek birimi sayısı için faturalandırılırsınız.

## <a name="use-cases"></a>Kullanım örnekleri

İstediğiniz yerde sunucusuz en iyi uyan senaryolar Azure Cosmos DB:

- **Düşük, aralıklı ve öngörülemeyen trafik**: Bu gibi durumlarda sağlama kapasitesi gerekli değildir ve maliyet probisel olabilir
- **Orta performans**: sunucusuz kapsayıcılar [belirli performans özelliklerine](#performance) sahip olduğundan

Bu nedenlerden dolayı sunucusuz Azure Cosmos DB aşağıdaki durumlarda göz önünde bulundurulmalıdır:

- Azure Cosmos DB kullanmaya başlama
- Geliştirme, test etme ve yeni uygulamaların prototipini oluşturma
- Tahmin etmek zor aralıklı trafik sayesinde küçük ve orta ölçekli uygulamalar çalıştırma
- [Azure işlevleri](../azure-functions/functions-overview.md) gibi sunucusuz işlem hizmetleriyle tümleştirme

Kullanım örneğine en uygun teklifi seçme hakkında daha fazla bilgi için bkz. [sağlanan verimlilik ve sunucusuz](throughput-serverless.md) makale arasından seçim yapma.

## <a name="using-serverless-resources"></a>Sunucusuz kaynakları kullanma

Sunucusuz yeni bir Azure Cosmos hesap türüdür. Bu, yeni bir hesap oluştururken **sağlanan aktarım hızı** ve **sunucusuz** arasında seçim yapmanız gerektiği anlamına gelir. Sunucusuz kullanmaya başlamak için yeni bir sunucusuz hesap oluşturmanız gerekir. Önizleme sürümü sırasında, yeni bir sunucusuz hesap oluşturmak için desteklenen tek yol [Azure Portal kullanmaktır](create-cosmosdb-resources-portal.md). Var olan hesapların sunucusuz moda geçirilmesi Şu anda desteklenmiyor.

Sunucusuz hesapta oluşturulan herhangi bir kapsayıcı sunucusuz bir kapsayıcıdır. Sunucusuz kapsayıcılar, sağlanan aktarım hızı modunda oluşturulan kapsayıcılarla aynı özellikleri sunar, böylece verilerinizi tam olarak aynı şekilde okuyup sorgular. Ancak sunucusuz hesaplar ve kapsayıcılar de belirli özelliklere sahiptir:

> [!IMPORTANT]
> Sunucusuz genel kullanıma sunulduğunda ve **geri bildiriminiz** karar vermemize yardımcı olacağı için bu sınırlamaların bazıları kaldırılabilir veya kaldırılabilir Bize ulaşın ve sunucusuz deneyiminiz hakkında daha fazla bilgi verin: [azurecosmosdbserverless@service.microsoft.com](mailto:azurecosmosdbserverless@service.microsoft.com) .

- Sunucusuz bir hesap yalnızca tek bir Azure bölgesinde çalıştırılabilir. Bir sunucusuz hesaba oluşturduktan sonra ek Azure bölgeleri eklemek mümkün değildir.
- [SYNAPSE link Preview özelliğinin](synapse-link.md) sunucusuz bir hesapta etkinleştirilmesi mümkün değildir.
- Sağlama verimi sunucusuz kapsayıcılar üzerinde gerekli değildir, bu nedenle aşağıdaki deyimler geçerlidir:
    - Sunucusuz kapsayıcı oluştururken bir işleme geçirilemez ve bunu yapmak bir hata döndürür.
    - Sunucusuz bir kapsayıcıda üretilen işi okuyamıyorum ve güncelleştiremezsiniz, bu nedenle bir hata döndürür.
    - Sunucusuz bir hesapta paylaşılan bir üretilen iş veritabanı oluşturamazsınız ve bunu yapmak bir hata döndürür.
- Sunucusuz kapsayıcılar en fazla 50 GB veri ve Dizin saklayabilir.

## <a name="monitoring-your-consumption"></a>Tüketiminizi izleme

Daha önce sağlanan aktarım hızı modunda Azure Cosmos DB kullandıysanız, trafiğiniz sağlanan kapasiteyi içermiyorsa sunucusuz 'ın daha düşük maliyetli olduğunu fark edersiniz. Kullanım ücreti, veritabanınızın işlendiği istek sayısına göre faturalandırılacağından maliyetlerinizin daha öngörülebilir hale gelebilmesini sağlayacak. Bu nedenle, geçerli tüketiminize göz önünde tutmanız önemlidir.

Hesabınızın **ölçümler** bölmesine gözatarken, **genel bakış** sekmesi altında **tüketilen istek birimleri** adlı bir grafik bulacaksınız. Bu grafik, hesabınızın kaç Istek birimi kullandığını gösterir:

:::image type="content" source="./media/serverless/request-units-consumed.png" alt-text="Tüketilen Istek birimlerini gösteren grafik" border="false":::

[Burada](monitor-request-unit-usage.md)açıklandığı gibi, Azure izleyici kullanırken aynı grafiği bulabilirsiniz. Azure Izleyici, Istek birimi tüketiminiz belirli bir eşiği geçtiğinde size bildirimde bulunan [uyarıları](../azure-monitor/platform/alerts-metric-overview.md)ayarlamanıza izin verir.

## <a name="performance"></a><a id="performance"></a>Performans

Sunucusuz kaynaklar, sağlanan aktarım hızı kaynaklarından farklı olarak belirli performans özellikleri sunar:

- **Kullanılabilirlik**: sunucusuz teklif genel kullanıma sunulduktan sonra, kullanılabilirlik alanları (bölge yedekliliği) kullanılmazsa sunucusuz kapsayıcıların kullanılabilirliği% 99,9 HIZMET DÜZEYI SÖZLEŞMESI (SLA) kapsamında olur. Kullanılabilirlik Alanları kullanıldığında SLA% 99,99 ' dir.
- **Gecikme süresi**: sunucusuz teklif genel kullanıma hazır hale geldikten sonra sunucusuz kapsayıcıların gecikmesi, bir hizmet düzeyi hedefı (SLO) için 10 milisaniye veya nokta okuma ve 30 milisaniye ya da daha az yazma için daha az olabilir. Bir nokta okuma işlemi, KIMLIĞINE ve bölüm anahtarı değerine göre tek bir öğe getirilirken oluşur.
- **Buristimsuz**: sunucusuz bir teklif genel kullanıma sunulduğunda, sunucusuz kapsayıcıların maistikliğine yönelik bir hizmet düzeyi hedefı (SLO) %95 olarak ele alınacaktır. Bu, en fazla patlama süresinin en az %95 ' i kadar erişilebilir olması anlamına gelir.

> [!NOTE]
> Azure önizlemesi olarak, Azure Cosmos DB sunucusuz hizmet düzeyi sözleşmelerinden (SLA) hariç tutulur. Yukarıda belirtilen performans özellikleri, genel kullanıma sunulduğunda bu teklifin ne kadar teslim edileceği hakkında bir önizleme olarak sağlanır.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerle sunucusuz ile çalışmaya başlayın:

- [Azure Cosmos DB'de İstek birimleri](request-units.md)
- [Sağlanan aktarım hızı ile sunucusuz sistem arasında tercih yapın](throughput-serverless.md)
- [Azure Cosmos DB’de modeli fiyatlandırma](how-pricing-works.md)
