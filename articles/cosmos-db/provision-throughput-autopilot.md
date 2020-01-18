---
title: Autopilot modunda Azure Cosmos kapsayıcıları ve veritabanları oluşturun.
description: Avantajlar, kullanım durumları ve Autopilot modunda Azure Cosmos veritabanları ve kapsayıcıları sağlama hakkında bilgi edinin.
author: kirillg
ms.author: kirillg
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 89af30788fe5129cddc6a3607b8c722549b610d1
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76264059"
---
# <a name="create-azure-cosmos-containers-and-databases-in-autopilot-mode-preview"></a>Autopilot modunda Azure Cosmos kapsayıcıları ve veritabanları oluşturma (Önizleme)

Azure Cosmos DB kapsayıcılarınızda el ile veya otomatik pilot modunda aktarım hızı sağlamanıza olanak tanır. Bu makalede otomatik pilot modunun avantajları ve kullanım örnekleri açıklanır.

> [!NOTE]
> Autopilot modu şu anda genel önizlemede kullanılabilir. Yalnızca [yeni veritabanları ve kapsayıcılar için Autopilot etkinleştirebilirsiniz](#create-a-database-or-a-container-with-autopilot-mode) . Mevcut kapsayıcılar ve veritabanları için kullanılamaz.

Aktarım hızını el ile sağlamaya ek olarak, artık Autopilot modundaki Azure Cosmos kapsayıcıları ' nı yapılandırabilirsiniz. Autopilot modunda yapılandırılan kapsayıcılar ve veritabanları, **iş yükünün kullanılabilirliğini, gecikmesini, verimini veya küresel olarak performansını etkilemeden uygulamanızın gereksinimlerine göre sağlanan aktarım hızını otomatik olarak ve anında ölçeklendirecektir.**

Autopilot modunda kapsayıcılar ve veritabanları yapılandırılırken, aşılmayacak `Tmax` en yüksek aktarım hızını belirtmeniz gerekir. Kapsayıcılar daha sonra `0.1*Tmax < T < Tmax`aktarım hızını ölçeklendirebilir. Diğer bir deyişle, kapsayıcılar ve veritabanları, en yüksek aktarım hızı değeri olarak yapılandırdığınız maksimum üretilen iş değeri en fazla %10 ' dan düşük olan iş yükü ihtiyaçlarına göre anında ölçeklendirilir. Bir Autopilot veritabanı veya kapsayıcısındaki en fazla üretilen iş (`Tmax`) ayarını dilediğiniz zaman değiştirebilirsiniz. Autopilot seçeneğiyle, kapsayıcı veya veritabanı başına 400 RU/sn en düşük aktarım hızı artık geçerli değildir.

Autopilot önizlemesi sırasında, kapsayıcıda veya veritabanında bulunan belirtilen en yüksek aktarım hızı için sistem, hesaplanan depolama sınırı içinde çalışmasına izin verir. Depolama sınırı aşılırsa, en yüksek aktarım hızı otomatik olarak daha yüksek bir değere ayarlanır. Autopilot modu ile veritabanı düzeyinde aktarım hızı kullanılırken, bir veritabanı içinde izin verilen kapsayıcıların sayısı şu şekilde hesaplanır: `0.001*TMax`. Örneğin, 20.000 Autopilot RU/s temin ediyorsanız, veritabanında 20 kapsayıcı olabilir.

## <a name="benefits-of-autopilot-mode"></a>Autopilot modunun avantajları

Autopilot modunda yapılandırılan Azure Cosmos kapsayıcıları aşağıdaki avantajlara sahiptir:

* **Basit:** Autopilot modundaki kapsayıcılar, çeşitli kapsayıcılar için sağlanan üretilen işi (ru) ve kapasitesini el ile yönetme karmaşıklığını ortadan kaldırır.

* **Ölçeklenebilir:** Autopilot modundaki kapsayıcılar, sağlanan verimlilik kapasitesini gerektiği şekilde sorunsuz bir şekilde ölçeklendirin. İstemci bağlantıları, uygulamalar ve mevcut SLA 'Ları etkilemez.

* Uygun **maliyetli:** Autopilot modunda yapılandırılmış kapsayıcıları kullandığınızda, yalnızca iş yüklerinizin saat başına ihtiyaç duyduğu kaynaklar için ödeme yaparsınız.

* **Yüksek oranda kullanılabilir:** Autopilot modundaki kapsayıcılar, veri dayanıklılığı ve yüksek kullanılabilirlik sağlamak için aynı küresel olarak dağıtılmış, hataya dayanıklı, yüksek oranda kullanılabilir arka uca kullanır.

## <a name="use-cases-of-autopilot-mode"></a>Autopilot modunun kullanım durumları

Autopilot modunda yapılandırılan Azure Cosmos kapsayıcıları için kullanım örnekleri şunları içerir:

* **Değişken iş yükleri:** En yüksek düzeyde kullanılan bir uygulamayı, her gün ya da yılda birkaç kez bir kez 1 saat boyunca birkaç saat olacak şekilde çalıştırıyorsanız. Örnek olarak insan kaynakları, bütçeleme ve operasyonel raporlama için uygulamalar sayılabilir. Bu tür senaryolar için Autopilot modunda yapılandırılan kapsayıcılar kullanılabilir ve artık tepe ya da ortalama kapasite için el ile sağlamanız gerekmez.

* **Öngörülemeyen iş yükleri:** Gün boyunca veritabanı kullanımı olan iş yüklerini çalıştırırken, aynı zamanda tahmin edilmesi zor olan etkinlik de çok fazla olur. Örnek, hava durumu tahmini değiştiğinde etkinliğin ani bir durumunu gösteren bir trafik sitesi içerir. Autopilot modunda yapılandırılan kapsayıcılar, uygulamanın en yoğun yük ihtiyaçlarını karşılamak için kapasiteyi ayarlayın ve etkinliğin aşırı dönmesi üzerindeyken ölçeği yeniden azaltın.

* **Yeni uygulamalar:** Yeni bir uygulama dağıtıyorsanız ve ne kadar sağlanan aktarım hızı (yani, ne kadar ru) gerektiği konusunda emin değilseniz. Autopilot modunda yapılandırılmış kapsayıcılar ile uygulamanızın kapasite ihtiyaçlarına ve gereksinimlerine otomatik olarak ölçeklendirebilirsiniz.

* **Sık kullanılmayan uygulamalar:** Yalnızca birkaç saat için günde bir veya haftada bir veya ayda birkaç kez kullanılan bir uygulamanız varsa (düşük hacimli bir uygulama/web/blog sitesi gibi).

* **Geliştirme ve test veritabanları:** İş saatlerinde kapsayıcılar kullanan geliştiriciler varsa, ancak onlara gece veya hafta sonları üzerinde gerek yoktur. Autopilot modunda yapılandırılmış kapsayıcılar, kullanımda olmadığında en az bir alt sınır olacak şekilde ölçeklendirilir.

* **Zamanlanan üretim iş yükleri/sorguları:** Tek bir kapsayıcıda bir dizi zamanlanmış istek/işlem/sorgu varsa ve mutlak düşük bir verimlilik üzerinde çalıştırmak istediğiniz boşta dönemler varsa, artık bu işlemi kolayca yapabilirsiniz. Zamanlanan bir sorgu/istek Autopilot modunda yapılandırılmış bir kapsayıcıya gönderildiğinde, otomatik olarak gerektiği kadar ölçeklendirecektir ve işlemi çalıştırır.

Önceki sorunlara yönelik çözümler uygulamada yalnızca çok büyük bir süre gerektirmez, ancak aynı zamanda yapılandırmada veya kodunuzda karmaşıklık sağlar ve genellikle el ile müdahale gerektirir. Autopilot modu, yukarıdaki senaryolara izin vermez, böylece artık bu sorunlar hakkında endişelenmenize gerek kalmaz.

## <a name="comparison--containers-configured-in-manual-mode-vs-autopilot-mode"></a>Karşılaştırma – manuel modda ve Autopilot modunda yapılandırılmış kapsayıcılar

|  | El ile modda yapılandırılmış kapsayıcılar  | Autopilot modunda yapılandırılmış kapsayıcılar |
|---------|---------|---------|
| **Sağlanan aktarım hızı** | El ile sağlandı. | Otomatik olarak ve anında iş yükü kullanım desenlerine göre ölçeklendirildi. |
| **İsteklerin/işlemlerin hız sınırlaması (429)**  | Tüketim sağlanan kapasiteyi aşarsa meydana gelebilir. | Tüketilen verimlilik, Autopilot modu ile seçtiğiniz en fazla aktarım hızı içindeyse gerçekleşmeyecektir.   |
| **Kapasite planlaması** |  Bir ilk kapasite planlaması ve ihtiyacınız olan aktarım hızını temin etmeniz gerekir. |    Kapasite planlaması konusunda endişelenmeniz gerekmez. Sistem, kapasite planlama ve kapasite yönetimini otomatik olarak gerçekleştirir. |
| **Fiyatlandırma** | Saat başına el ile sağlanan RU/s. | Tek bir yazma bölgesi hesabında, saat başına Autopilot RU/s 'yi kullanarak saatlik olarak kullanılan aktarım hızı için ödeme yaparsınız. <br/><br/>Birden fazla yazma bölgesi olan hesaplar için Autopilot için ek ücret alınmaz. Saat başına aynı çok yöneticili RU/sn 'yi kullanarak saatlik olarak kullanılan aktarım hızı için ödeme yaparsınız. |
| **İş yükü türleri için en uygun** |  Öngörülebilir ve kararlı iş yükleri|   Tahmin edilemeyen ve değişken iş yükleri  |

## <a name="create-a-database-or-a-container-with-autopilot-mode"></a>Autopilot modu ile bir veritabanı veya kapsayıcı oluşturma

Azure portal aracılığıyla oluştururken yeni veritabanları veya kapsayıcılar için Autopilot yapılandırabilirsiniz. Yeni bir veritabanı veya kapsayıcı oluşturmak, Autopilot etkinleştirmek ve en yüksek aktarım hızını (RU/s) belirtmek için aşağıdaki adımları kullanın.

1. [Azure Portal](https://portal.azure.com) veya [Azure Cosmos DB Gezgininde](https://cosmos.azure.com/) oturum açın.

1. Azure Cosmos DB hesabınıza gidin ve **Veri Gezgini** sekmesini açın.

1. **Yeni kapsayıcı** ' yı seçin. Veritabanınız, Kapsayıcınız ve bölüm anahtarınız için bir ad girin. **Verimlilik**altında **Autopilot** seçeneğini belirleyin ve Autopilot seçeneğini kullanırken veritabanının veya kapsayıcının aşmadığı en yüksek aktarım hızını (ru/sn) seçin.

   ![Kapsayıcı oluşturma ve Autopilot verimini yapılandırma](./media/provision-throughput-autopilot/create-container-autopilot-mode.png)

1. **Tamam**’ı seçin.

**Veritabanı Işleme sağlama** seçeneğini belirleyerek Autopilot modu ile paylaşılan bir üretilen iş veritabanı oluşturabilirsiniz.

## <a id="autopilot-limits"></a>Autopilot için üretilen iş ve depolama sınırları

Aşağıdaki tabloda, Autopilot modundaki farklı seçenekler için en fazla ve depolama sınırları gösterilmektedir:

|En yüksek aktarım hızı sınırı  |En fazla depolama sınırı  |
|---------|---------|
|4000 RU/sn  |   50 GB    |
|20.000 RU/sn  |  200 GB  |
|100.000 RU/sn    |  1 TB   |
|500.000 RU/sn    |  5 TB  |

## <a name="next-steps"></a>Sonraki adımlar

* [AUTOPILOT SSS](autopilot-faq.md)makalesini inceleyin.
* [Mantıksal bölümler](partition-data.md)hakkında daha fazla bilgi edinin.
* [Azure Cosmos kapsayıcısında üretilen iş sağlama](how-to-provision-container-throughput.md)hakkında bilgi edinin.
* [Azure Cosmos veritabanında üretilen iş sağlama](how-to-provision-database-throughput.md)hakkında bilgi edinin.
