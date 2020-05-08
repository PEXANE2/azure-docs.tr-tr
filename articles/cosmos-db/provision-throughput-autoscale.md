---
title: Otomatik ölçeklendirme sağlanan aktarım hızı içinde Azure Cosmos kapsayıcıları ve veritabanları oluşturun.
description: Avantajlar, kullanım örnekleri ve Azure Cosmos veritabanları ile kapsayıcıları için otomatik ölçeklendirme sağlanan aktarım hızı hakkında bilgi edinin.
author: kirillg
ms.author: kirillg
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/28/2020
ms.openlocfilehash: 81a13dcb7955a7d46f485416bf9b7e4e7be4d9ac
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82791723"
---
# <a name="create-azure-cosmos-containers-and-databases-with-autoscale-provisioned-throughput"></a>Otomatik ölçeklendirme sağlanan aktarım hızı ile Azure Cosmos kapsayıcıları ve veritabanları oluşturma

Azure Cosmos DB, Kapsayıcılarınızı standart (el ile) sağlanan aktarım hızı veya otomatik ölçeklendirme sağlanan aktarım hızı ile yapılandırmanıza olanak tanır. Bu makalede otomatik ölçeklendirme avantajları ve kullanım durumları açıklanmaktadır.

> [!NOTE]
> [Otomatik ölçeklendirmeyi yalnızca yeni veritabanları ve kapsayıcılar için etkinleştirebilirsiniz](#create-db-container-autoscale) . Mevcut kapsayıcılar ve veritabanları için kullanılamaz.

Standart üretilen iş sağlamaya ek olarak Azure Cosmos kapsayıcılarını artık otomatik ölçeklendirme sağlanan aktarım hızı ile yapılandırabilirsiniz. Otomatik ölçeklendirme sağlanmış üretilen iş için yapılandırılan kapsayıcılar ve veritabanları, **iş yükünün kullanılabilirliğini, gecikmesini, verimini veya küresel olarak performansını etkilemeden uygulamanızın gereksinimlerine göre sağlanan aktarım hızını otomatik olarak ve anında ölçeklendirecektir.**

Otomatik ölçeklendirme sırasında kapsayıcılar ve veritabanları yapılandırılırken, aşılmayacak olan en yüksek aktarım hızını `Tmax` belirtmeniz gerekir. Kapsayıcılar daha sonra üretilen iş verimini ölçeklendirebilir `0.1*Tmax < T < Tmax`. Diğer bir deyişle, kapsayıcılar ve veritabanları, en yüksek aktarım hızı değeri olarak yapılandırdığınız maksimum üretilen iş değeri en fazla %10 ' dan düşük olan iş yükü ihtiyaçlarına göre anında ölçeklendirilir. Otomatik ölçeklendirmeyi yapılandırdıktan sonra herhangi bir zamanda bir veritabanı veya kapsayıcıda`Tmax`maksimum üretilen iş () ayarını değiştirebilirsiniz. Otomatik ölçeklendirme seçeneği ile, kapsayıcı veya veritabanı başına en düşük 400 RU/sn, artık geçerli değildir.

Kapsayıcıda veya veritabanında belirtilen en yüksek aktarım hızı için sistem, hesaplanan depolama sınırı içinde çalışmasına izin verir. Depolama sınırı aşılırsa, en yüksek aktarım hızı otomatik olarak daha yüksek bir değere ayarlanır. Veritabanı düzeyinde üretilen iş boyutunu otomatik ölçeklendirme ile kullanırken, bir veritabanı içinde izin verilen kapsayıcıların sayısı şu şekilde hesaplanır `0.001*TMax`:. Örneğin, 20.000 otomatik ölçeklendirme RU/s sağlarsanız, veritabanında 20 kapsayıcı olabilir.

## <a name="benefits-of-autoscale-provisioned-throughput"></a><a id="autoscale-benefits"></a>Otomatik ölçeklendirme sağlanan aktarım hızı avantajları

Otomatik ölçeklendirme ile yapılandırılan Azure Cosmos kapsayıcıları aşağıdaki avantajlara sahiptir:

* **Basit:** Otomatik ölçeklendirme içeren kapsayıcılar, sağlanan üretilen işi (ru) ve kapasitesini çeşitli kapsayıcılar için el ile yönetmek üzere karmaşıklığı ortadan kaldırır.

* **Ölçeklenebilir:** Otomatik ölçeklendirme içeren kapsayıcılar, sağlanan verimlilik kapasitesini gerektiği gibi sorunsuzca ölçeklendirin. İstemci bağlantıları, uygulamalar ve mevcut SLA 'Ları etkilemez.

* Uygun **maliyetli:** Otomatik ölçeklendirme ile yapılandırılmış kapsayıcıları kullandığınızda, yalnızca iş yüklerinizin saatlik olarak ihtiyaç duyduğu kaynaklar için ödeme yaparsınız.

* **Yüksek oranda kullanılabilir:** Otomatik ölçeklendirme içeren kapsayıcılar, veri dayanıklılığı ve yüksek kullanılabilirlik sağlamak için aynı küresel olarak dağıtılmış, hataya dayanıklı, yüksek oranda kullanılabilir arka uca kullanır.

## <a name="use-cases-of-autoscale-provisioned-throughput"></a><a id="autoscale-usecases"></a>Otomatik ölçeklendirme sağlanan aktarım hızı kullanım örnekleri

Otomatik ölçeklendirme ile yapılandırılan Azure Cosmos kapsayıcıları için kullanım örnekleri şunlardır:

* **Değişken iş yükleri:** En yüksek düzeyde kullanılan bir uygulamayı, her gün ya da yılda birkaç kez bir kez 1 saat boyunca birkaç saat olacak şekilde çalıştırıyorsanız. Örnek olarak insan kaynakları, bütçeleme ve operasyonel raporlama için uygulamalar sayılabilir. Bu tür senaryolarda, otomatik ölçeklendirme ile yapılandırılan kapsayıcılar kullanılabilir ve artık yoğun veya ortalama kapasite için el ile sağlamanız gerekmez.

* **Öngörülemeyen iş yükleri:** Gün boyunca veritabanı kullanımı olan iş yüklerini çalıştırırken, aynı zamanda tahmin edilmesi zor olan etkinlik de çok fazla olur. Örnek, hava durumu tahmini değiştiğinde etkinliğin ani bir durumunu gösteren bir trafik sitesi içerir. Otomatik ölçeklendirme ile yapılandırılan kapsayıcılar, uygulamanın en yoğun yük ihtiyaçlarını karşılamak için kapasiteyi ayarlayın ve etkinliğin aşırı dönmesi üzerindeyken ölçeği yeniden azaltın.

* **Yeni uygulamalar:** Yeni bir uygulama dağıtıyorsanız ve ne kadar sağlanan aktarım hızı (yani, ne kadar ru) gerektiği konusunda emin değilseniz. Otomatik ölçeklendirme ile yapılandırılmış kapsayıcılar sayesinde, uygulamanızın kapasite ihtiyaçlarına ve gereksinimlerine otomatik olarak ölçeklendirebilirsiniz.

* **Sık kullanılmayan uygulamalar:** Yalnızca birkaç saat için günde bir veya haftada bir veya ayda birkaç kez kullanılan bir uygulamanız varsa (düşük hacimli bir uygulama/web/blog sitesi gibi).

* **Geliştirme ve test veritabanları:** İş saatlerinde kapsayıcılar kullanan geliştiriciler varsa, ancak onlara gece veya hafta sonları üzerinde gerek yoktur. Otomatik ölçeklendirme ile yapılandırılmış kapsayıcılar, kullanımda olmadığında en az bir değer olacak şekilde ölçeklendirilir.

* **Zamanlanan üretim iş yükleri/sorguları:** Tek bir kapsayıcıda bir dizi zamanlanmış istek/işlem/sorgu varsa ve mutlak düşük bir verimlilik üzerinde çalıştırmak istediğiniz boşta dönemler varsa, artık bu işlemi kolayca yapabilirsiniz. Zamanlanan bir sorgu/istek otomatik ölçeklendirme ile yapılandırılmış bir kapsayıcıya gönderildiğinde, otomatik olarak gerektiği kadar ölçeklendirecektir ve işlemi çalıştırır.

Önceki sorunlara yönelik çözümler uygulamada yalnızca çok büyük bir süre gerektirmez, ancak aynı zamanda yapılandırmada veya kodunuzda karmaşıklık sağlar ve genellikle el ile müdahale gerektirir. Otomatik ölçeklendirme, yukarıdaki senaryolara izin vermez, böylece artık bu sorunlar hakkında endişelenmenize gerek kalmaz.

## <a name="comparison--standard-manual-vs-autoscale-provisioned-throughput"></a>Karşılaştırma – standart (el ile)-otomatik ölçeklendirme sağlanan aktarım hızı

|  | Standart sağlanan aktarım hızı ile yapılandırılmış kapsayıcılar  | Otomatik ölçeklendirme sağlanan aktarım hızı ile yapılandırılmış kapsayıcılar |
|---------|---------|---------|
| **Sağlanan aktarım hızı** | El ile sağlandı. | Otomatik olarak ve anında iş yükü kullanım desenlerine göre ölçeklendirildi. |
| **İsteklerin/işlemlerin hız sınırlaması (429)**  | Tüketim sağlanan kapasiteyi aşarsa meydana gelebilir. | Tüketilen verimlilik, otomatik ölçeklendirme ile seçtiğiniz en fazla aktarım hızı içindeyse gerçekleşmeyecektir.   |
| **Kapasite planlaması** |  Bir ilk kapasite planlaması ve ihtiyacınız olan aktarım hızını temin etmeniz gerekir. |    Kapasite planlaması konusunda endişelenmeniz gerekmez. Sistem, kapasite planlama ve kapasite yönetimini otomatik olarak gerçekleştirir. |
| **Fiyatlandırma** | Saat başına el ile sağlanan RU/s. | Tek bir yazma bölgesi hesabında, saat başına otomatik ölçeklendirme RU/sn kullanarak saatlik olarak kullanılan aktarım hızı için ödeme yaparsınız. <br/><br/>Birden çok yazma bölgesi olan hesaplar için otomatik ölçeklendirme için ek ücret alınmaz. Saat başına aynı çok yöneticili RU/sn 'yi kullanarak saatlik olarak kullanılan aktarım hızı için ödeme yaparsınız. |
| **İş yükü türleri için en uygun** |  Öngörülebilir ve kararlı iş yükleri|   Tahmin edilemeyen ve değişken iş yükleri  |

## <a name="create-a-database-or-a-container-with-autoscale"></a><a id="create-db-container-autoscale"></a>Otomatik ölçeklendirme ile veritabanı veya kapsayıcı oluşturma

Azure portal aracılığıyla oluştururken yeni veritabanları veya kapsayıcılar için otomatik ölçeklendirmeyi yapılandırabilirsiniz. Yeni bir veritabanı veya kapsayıcı oluşturmak, otomatik ölçeklendirmeyi etkinleştirmek ve en yüksek aktarım hızını (RU/s) belirtmek için aşağıdaki adımları kullanın.

1. [Azure Portal](https://portal.azure.com) veya [Azure Cosmos DB Gezgininde](https://cosmos.azure.com/) oturum açın.

1. Azure Cosmos DB hesabınıza gidin ve **Veri Gezgini** sekmesini açın.

1. **Yeni kapsayıcı** ' yı seçin. Veritabanınız, Kapsayıcınız ve bölüm anahtarınız için bir ad girin. **Verimlilik**altında, **Otomatik ölçeklendirme** seçeneğini belirleyin ve veritabanının veya kapsayıcının otomatik ölçeklendirme seçeneğini kullanırken aşmadığı en yüksek aktarım hızını (ru/sn) seçin.

   ![Kapsayıcı oluşturma ve otomatik ölçeklendirme verimini yapılandırma](./media/provision-throughput-autoscale/create-container-autoscale-mode.png)

1. **Tamam**’ı seçin.

**Veritabanı Işleme sağlama** seçeneğini belirleyerek, otomatik ölçeklendirme ile paylaşılan bir üretilen iş veritabanı oluşturabilirsiniz.

## <a name="throughput-and-storage-limits-for-autoscale"></a><a id="autoscale-limits"></a>Otomatik ölçeklendirme için üretilen iş ve depolama sınırları

Aşağıdaki tabloda, otomatik ölçeklendirme içindeki farklı seçenekler için en fazla ve depolama sınırları gösterilmektedir:

|En yüksek aktarım hızı sınırı  |En fazla depolama sınırı  |
|---------|---------|
|4000 RU/sn  |   50 GB    |
|20.000 RU/sn  |  200 GB  |
|100.000 RU/sn    |  1 TB   |
|500.000 RU/sn    |  5 TB  |

## <a name="next-steps"></a>Sonraki adımlar

* [Otomatik ÖLÇEKLENDIRME SSS](autoscale-faq.md)'sini inceleyin.
* [Mantıksal bölümler](partition-data.md)hakkında daha fazla bilgi edinin.
* [Azure Cosmos kapsayıcısında üretilen iş sağlama](how-to-provision-container-throughput.md)hakkında bilgi edinin.
* [Azure Cosmos veritabanında üretilen iş sağlama](how-to-provision-database-throughput.md)hakkında bilgi edinin.
