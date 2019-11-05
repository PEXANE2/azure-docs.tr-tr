---
title: Autopilot modunda işleme ile Azure Cosmos kapsayıcıları ve veritabanları oluşturun.
description: Avantajlar, kullanım durumları ve Autopilot modunda Azure Cosmos veritabanları ve kapsayıcıları sağlama hakkında bilgi edinin.
author: kirillg
ms.author: kirillg
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 598dc6394e8be8b3372f4ed61a522454830a22d6
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73512281"
---
# <a name="create-azure-cosmos-containers-and-databases-with-provisioned-throughput-in-autopilot-mode-preview"></a>Autopilot modunda (Önizleme) sağlanan aktarım hızına sahip Azure Cosmos kapsayıcıları ve veritabanları oluşturun

Azure Cosmos DB, kapsayıcılarınızdaki iş üretimini el ile veya Autopilot modunda sağlamanıza olanak tanır. Bu makalede, Autopilot modunun avantajları ve kullanım durumları açıklanmaktadır.

> [!NOTE]
> Autopilot modu şu anda genel önizlemede kullanılabilir.

Aktarım hızını el ile sağlamaya ek olarak, artık Autopilot modundaki Azure Cosmos kapsayıcıları ' nı yapılandırabilirsiniz. Autopilot modunda yapılandırılan Azure Cosmos kapsayıcıları ve veritabanları, **SLA 'lara ödün vermeden uygulama gereksinimlerinize göre sağlanan üretilen işi otomatik olarak ve anında ölçeklendirecektir.**

Artık sağlanan üretilen işi el ile yönetmeniz veya hız sınırlandırma sorunlarını ele almanız gerekmez. Autopilot modunda yapılandırılan Azure Cosmos kapsayıcıları, iş yükünün kullanılabilirliğini, gecikmesini, verimini veya küresel olarak performansını etkilemeden iş yüküne anında ölçeklendirilebilir. Yüksek kullanım altında, Autopilot modunda yapılandırılan Azure Cosmos kapsayıcıları, devam eden işlemleri etkilemeden ölçeği artırılabilecek veya azaltılabilir.

Autopilot modunda kapsayıcılar ve veritabanları yapılandırılırken, aşılmayacak `Tmax` en yüksek aktarım hızını belirtmeniz gerekir. Kapsayıcılar daha sonra `0.1*Tmax < T < Tmax` aralığında iş yükü ihtiyaçlarına göre anında ölçeklendirebilir. Diğer bir deyişle, kapsayıcılar ve veritabanları, yapılandırılan Aktarım hızı değerinin %10 ' u kadar, belirtilen maksimum yapılandırılmış değere kadar iş yükü ihtiyaçlarına göre anında ölçeklenir. Autopilot veritabanı veya kapsayıcısındaki en fazla üretilen iş (Tmax) ayarını dilediğiniz zaman değiştirebilirsiniz.

## <a name="benefits-of-autopilot-mode"></a>Autopilot modunun avantajları

Autopilot modunda yapılandırılan Azure Cosmos kapsayıcıları aşağıdaki avantajlara sahiptir:

* **Basit:** Autopilot modundaki kapsayıcılar, çeşitli kapsayıcılar için sağlanan üretilen işi (ru) ve kapasitesini el ile yönetme karmaşıklığını ortadan kaldırır.

* **Ölçeklenebilir:** Autopilot modundaki kapsayıcılar, sağlanan verimlilik kapasitesini gerektiği şekilde sorunsuz bir şekilde ölçeklendirin. İstemci bağlantıları, uygulamalar ve mevcut SLA 'Ları etkilemez.

* Uygun **maliyetli:** Autopilot modunda yapılandırılmış Azure Cosmos kapsayıcıları kullandığınızda, yalnızca iş yüklerinizin saat başına ihtiyaç duyduğu kaynaklar için ödeme yaparsınız.

* **Yüksek oranda kullanılabilir:** Autopilot modundaki Azure Cosmos kapsayıcıları, veri dayanıklılığı ve her zaman yüksek kullanılabilirlik sağlamak için aynı küresel olarak dağıtılmış, hataya dayanıklı, yüksek oranda kullanılabilir arka uca sahiptir.

## <a name="use-cases-of-autopilot-mode"></a>Autopilot modunun kullanım durumları

Autopilot modunda yapılandırılan Azure Cosmos kapsayıcıları için kullanım örnekleri şunları içerir:

* **Değişken iş yükleri:** En yüksek kullanımı 1 saat ile her gün birkaç saat arasında veya yılda birkaç kez kullanıldığında, hafif olarak kullanılan bir uygulama çalıştırırken. Örnek olarak insan kaynakları, bütçeleme ve operasyonel raporlama için uygulamalar sayılabilir. Bu tür senaryolar için Autopilot modunda yapılandırılan kapsayıcılar kullanılabilir, artık yoğun veya ortalama kapasite için el ile sağlamanız gerekmez.

* **Öngörülemeyen iş yükleri:** Gün boyunca veritabanı kullanımı olan iş yüklerini çalıştırırken, aynı zamanda tahmin edilmesi zor olan etkinlik de çok fazla olur. Örnek, hava durumu tahmini değiştiğinde etkinliğin ani bir durumunu gösteren bir trafik sitesi içerir. Autopilot modunda yapılandırılan kapsayıcılar, uygulamanın en yoğun yük ihtiyaçlarını karşılamak için kapasiteyi ayarlayın ve etkinliğin aşırı dönmesi üzerindeyken ölçeği yeniden azaltın.

* **Yeni uygulamalar:** Yeni bir uygulama dağıtıyorsanız ve ne kadar sağlanan aktarım hızı (yani, ne kadar ru) gerektiği konusunda emin değilseniz. Autopilot modunda yapılandırılmış kapsayıcılar ile uygulamanızın kapasite ihtiyaçlarına ve gereksinimlerine otomatik olarak ölçeklendirebilirsiniz.

* **Sık kullanılmayan uygulamalar:** Yalnızca birkaç saat için günde bir veya haftada bir veya ayda birkaç kez kullanılan bir uygulamanız varsa (düşük hacimli bir uygulama/web/blog sitesi gibi).

* **Geliştirme ve test veritabanları:** Geliştiriciler iş saatlerinde Azure Cosmos hesaplarını kullanır, ancak bunları gece veya hafta sonları üzerinde gerektirmez. Autopilot modunda yapılandırılmış kapsayıcılar ile, kullanımda olmadığında en düşük olarak ölçeklenir.

* **Zamanlanan üretim iş yükleri/sorguları:** Tek bir kapsayıcıda bir dizi zamanlanmış istek/işlem/sorgu varsa ve mutlak düşük bir verimlilik üzerinde çalıştırmak istediğiniz boşta dönemler varsa, artık bu işlemi kolayca yapabilirsiniz. Zamanlanan bir sorgu/istek Autopilot modunda yapılandırılmış bir kapsayıcıya gönderildiğinde, otomatik olarak gerektiği kadar ölçeklendirecektir ve işlemi çalıştırır.

Önceki sorunlara yönelik çözümler uygulamada yalnızca çok büyük bir süre gerektirmez, ancak aynı zamanda yapılandırmada veya kodunuzda karmaşıklık sağlar ve genellikle el ile müdahale gerektirir. Autopilot modu, yukarıdaki senaryolara izin vermez, böylece artık bu sorunlar hakkında endişelenmenize gerek kalmaz.

## <a name="comparison--containers-configured-in-manual-mode-vs-autopilot-mode"></a>Karşılaştırma – manuel modda ve Autopilot modunda yapılandırılmış kapsayıcılar

|  | El ile modda yapılandırılmış kapsayıcılar  | Autopilot modunda yapılandırılmış kapsayıcılar |
|---------|---------|---------|
| **Sağlanan aktarım hızı** | El ile sağlanmış | İş yükü kullanım düzenlerine göre proaktif ve yeniden etkin şekilde ölçeklendirildi. |
| **İsteklerin/işlemlerin hız sınırlaması (429)**  | Tüketim sağlanan kapasiteyi aşarsa meydana gelebilir. | Gerçekleşmeyecektir.  |
| **Kapasite planlaması** |  Bir ilk kapasite planlaması ve ihtiyacınız olan aktarım hızını temin etmeniz gerekir. |    Kapasite planlaması konusunda endişelenmeniz gerekmez. Sistem, kapasite planlama ve kapasite yönetimini otomatik olarak gerçekleştirir. |
| **Fiyatlandırma** | Saat başına el ile sağlanan RU/s. | Tek bir yazma bölgesi hesabında, saat başına Autopilot RU/s 'yi kullanarak saatlik olarak kullanılan aktarım hızı için ödeme yaparsınız. <br/><br/>Birden fazla yazma bölgesi olan hesaplar için Autopilot için ek ücret alınmaz. Saat başına aynı çok yöneticili RU/sn 'yi kullanarak saatlik olarak kullanılan aktarım hızı için ödeme yaparsınız. |
| **İş yükü türleri için en uygun** |  Öngörülebilir ve kararlı iş yükleri|   Tahmin edilemeyen ve değişken iş yükleri  |

## <a name="create-a-database-or-a-container-with-autopilot-mode"></a>Autopilot modu ile bir veritabanı veya kapsayıcı oluşturma

Veritabanları veya kapsayıcılar oluşturma sırasında Autopilot yapılandırabilirsiniz. Aşağıdaki adımları yeni bir veritabanı veya kapsayıcıya kullanın, Autopilot etkinleştirin ve en yüksek aktarım hızını belirtin.

1. [Azure Portal](https://portal.azure.com) veya [Azure Cosmos Gezgini](https://cosmos.azure.com/) ' nde oturum açın.

1. Azure Cosmos hesabınıza gidin ve **Veri Gezgini** sekmesini açın.

1. **Yeni veritabanı**' nı seçin, veritabanınız için bir ad girin. **Autopilot** seçeneği Için, **etkin** ' i seçin ve Autopilot seçeneğini kullanırken veritabanının aşmadığı en yüksek aktarım hızını belirtin.

   ![Autopilot modunda veritabanı oluşturma](./media/provision-throughput-autopilot/create-database-autopilot-mode.png)

1. **Tamam**’ı seçin

Benzer adımlarla, Autopilot modunda sağlanan aktarım hızına sahip bir kapsayıcı da oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal bölümler](partition-data.md)hakkında daha fazla bilgi edinin.
* [Azure Cosmos kapsayıcısında üretilen iş sağlama](how-to-provision-container-throughput.md)hakkında bilgi edinin.
* [Azure Cosmos veritabanında üretilen iş sağlama](how-to-provision-database-throughput.md)hakkında bilgi edinin.