---
title: Otomatik pilot modunda Azure Cosmos kapsayıcıları ve veritabanları oluşturun.
description: Azure Cosmos veritabanları ve kapsayıcılarının otomatik pilot modunda nasıl sağkullanılacağı, kullanım örnekleri ve nasıl sağkullanılacağı hakkında bilgi edinin.
author: kirillg
ms.author: kirillg
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 89af30788fe5129cddc6a3607b8c722549b610d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79246662"
---
# <a name="create-azure-cosmos-containers-and-databases-in-autopilot-mode-preview"></a>Otomatik pilot modunda Azure Cosmos kapsayıcıları ve veritabanları oluşturma (Önizleme)

Azure Cosmos DB kapsayıcılarınızda el ile veya otomatik pilot modunda aktarım hızı sağlamanıza olanak tanır. Bu makalede otomatik pilot modunun avantajları ve kullanım örnekleri açıklanır.

> [!NOTE]
> Otomatik pilot modu şu anda genel önizlemede kullanılabilir. Yalnızca [yeni veritabanları ve kapsayıcılar için otomatik pilotu etkinleştirebilirsiniz.](#create-a-database-or-a-container-with-autopilot-mode) Varolan kapsayıcılar ve veritabanları için kullanılamaz.

Elde iş lerinin manuel olarak sağlanmasına ek olarak, azure cosmos kaplarını otomatik pilot modunda yapılandırabilirsiniz. Otomatik pilot modunda yapılandırılan kapsayıcılar ve veritabanları, iş yükünün genel olarak **kullanılabilirliğini, gecikmesini, iş kısmını veya performansını etkilemeden, uygulama gereksinimlerinize göre sağlanan iş kısmını otomatik olarak ve anında ölçeklendirecek.**

Kapsayıcıları ve veritabanlarını otomatik pilot modunda yapılandırırken, aşılmayacak maksimum verimi `Tmax` belirtmeniz gerekir. Kapsayıcılar daha sonra kendi `0.1*Tmax < T < Tmax`iş kadar ölçeklendirebilirsiniz böylece . Başka bir deyişle, kapsayıcılar ve veritabanları, yapılandırılmış maksimum verim değerine kadar yapılandırdığınız maksimum verim değerinin %10'una kadar olan iş yükü gereksinimlerine göre anında ölçeklenir. Otomatik pilot veritabanında veya`Tmax`kapsayıcıda en yüksek verim () ayarını herhangi bir zamanda değiştirebilirsiniz. Otomatik pilot seçeneği ile, konteyner veya veritabanı başına 400 RU/s minimum iş vericisi artık geçerli değildir.

Otomatik pilotun önizlemesi sırasında, kapsayıcıda veya veritabanında belirtilen maksimum verim için, sistem hesaplanan depolama limiti içinde çalışmasını sağlar. Depolama sınırı aşılırsa, maksimum verim otomatik olarak daha yüksek bir değere ayarlanır. Otomatik pilot modu ile veritabanı düzeyi iş bürünme kullanılırken, veritabanı `0.001*TMax`içinde izin verilen kapsayıcı sayısı şu şekilde hesaplanır: . Örneğin, 20.000 otomatik pilot RU/s sağlarsanız, veritabanında 20 kapsayıcı olabilir.

## <a name="benefits-of-autopilot-mode"></a>Otomatik pilot modunun faydaları

Otomatik pilot modunda yapılandırılan Azure Cosmos kapsayıcılarının aşağıdaki avantajları vardır:

* **Basit:** Otomatik pilot modundaki kapsayıcılar, çeşitli kapsayıcılar için sağlanan iş ve kapasiteyi el ile yönetmek için karmaşıklığı kaldırır.

* **Ölçeklenebilir:** Otomatik pilot modundaki kapsayıcılar, sağlanan iş lenme kapasitesini gerektiği gibi sorunsuz bir şekilde ölçeklendirin. İstemci bağlantılarında, uygulamalarında herhangi bir aksama yoktur ve varolan SLA'ları etkilemez.

* **Uygun maliyetli:** Otomatik pilot modunda yapılandırılan kapsayıcıları kullandığınızda, yalnızca iş yüklerinizin ihtiyaç duyduğu kaynakları saat başına ödentirsiniz.

* **Yüksek kullanılabilir:** Otomatik pilot modundaki kapsayıcılar, veri dayanıklılığı ve yüksek kullanılabilirlik sağlamak için aynı küresel olarak dağıtılan, hataya dayanıklı, yüksek kullanılabilirlik arka ucunu kullanır.

## <a name="use-cases-of-autopilot-mode"></a>Otomatik pilot modu nun kılıflarını kullanma

Otomatik pilot modunda yapılandırılan Azure Cosmos kapsayıcılarının kullanım örnekleri şunlardır:

* **Değişken iş yükleri:** Her gün birkaç kez veya yılda birkaç kez 1 saat ile birkaç saat arasında en yüksek kullanımı ile hafifçe kullanılan bir uygulama çalıştırırken. Örnekler arasında insan kaynakları uygulamaları, bütçeleme ve operasyonel raporlama yer almaktadır. Bu tür senaryolar için, otomatik pilot modunda yapılandırılan kapsayıcılar kullanılabilir ve artık en yüksek veya ortalama kapasite için el ile sağlamanız gerekmez.

* **Öngörülemeyen iş yükleri:** Gün boyunca veritabanı kullanımının olduğu iş yüklerini çalıştırırken, ancak tahmin etmesi zor olan etkinlik zirveleri de çalışırsınız. Buna örnek olarak, hava tahmini değiştiğinde bir etkinlik artışı gören bir trafik sitesi de dahildir. Otomatik pilot modunda yapılandırılan kapsayıcılar, kapasiteyi uygulamanın en yüksek yükünün gereksinimlerini karşılayacak şekilde ayarlar ve aktivite dalgalanması sona erdiğinde küçültün.

* **Yeni uygulamalar:** Yeni bir uygulama dağıtıyorsanız ve ne kadar tedarik edilen iş için (yani kaç RUs' a) ihtiyacınız olduğundan emin değilseniz. Otomatik pilot modunda yapılandırılan kapsayıcılarla, otomatik olarak uygulamanızın kapasite gereksinimlerine ve gereksinimlerine göre ölçeklendirebilirsiniz.

* **Seyrek kullanılan uygulamalar:** Düşük hacimli bir uygulama/web/blog sitesi gibi günde veya haftada veya ayda yalnızca birkaç saat kullanılan bir uygulamanız varsa.

* **Geliştirme ve test veritabanları:** Çalışma saatleri içinde kapsayıcıları kullanan geliştiricilerinvarsa, ancak gece veya hafta sonları gerekmez. Otomatik pilot modunda yapılandırılan kapsayıcılarla, kullanılmadığında en aza indirgenmiş olur.

* **Zamanlanmış üretim iş yükleri/sorguları:** Tek bir kapsayıcıda bir dizi zamanlanmış istek/işlem/sorgu nuz varsa ve mutlak düşük bir iş elde etme işlemiyapmak istediğiniz boş takılan dönemler varsa, artık bunu kolayca yapabilirsiniz. Zamanlanmış bir sorgu/istek otomatik pilot modunda yapılandırılan bir kapsayıcıya gönderildiğinde, otomatik olarak gerektiği kadar ölçeklenir ve işlemi çalıştırır.

Önceki sorunlara yönelik çözümler yalnızca uygulamada çok büyük bir zaman gerektirmekle birlikte, yapılandırmada veya kodunuzda karmaşıklık da sağlar ve bunları gidermek için sık sık el ile müdahale gerektirir. Otomatik pilot modu, artık bu sorunlar hakkında endişelenmenize gerek kalmaması için yukarıdaki senaryoları kutunun dışına çıkarmanızı sağlar.

## <a name="comparison--containers-configured-in-manual-mode-vs-autopilot-mode"></a>Karşılaştırma – Manuel modda yapılandırılan kapsayıcılar ve otomatik pilot modu

|  | El modunda yapılandırılan kapsayıcılar  | Otomatik pilot modunda yapılandırılan kapsayıcılar |
|---------|---------|---------|
| **Sağlanan iş girdisi** | El ile sağlanmış. | İş yükü kullanım şekillerine göre otomatik ve anında ölçeklendirilir. |
| **İsteklerin/operasyonların hız sınırlaması (429)**  | Tüketim sağlanan kapasiteyi aşarsa, meydana gelebilir. | Tüketilen iş tükenen iş, otomatik pilot modu ile seçtiğiniz maksimum iş elde etme alanı içindeyse gerçekleşmez.   |
| **Kapasite planlaması** |  Bir başlangıç kapasite planlaması ve ihtiyacınız olan iş kadar sağlama yapmanız gerekir. |    Kapasite planlaması konusunda endişelenmenize gerek yok. Sistem otomatik olarak kapasite planlaması ve kapasite yönetimi yle ilgilenir. |
| **Fiyatlandırma** | Saatte elle sağlanan RU/s. | Tek yazma bölge hesapları için, saat başına otomatik pilot RU/s oranını kullanarak saatlik olarak kullanılan iş başına ödeme yapılır. <br/><br/>Birden çok yazma bölgesi olan hesaplar için otomatik pilot için ek ücret alınmaz. Saat başına aynı çok ana lı RU/s oranını kullanarak saatlik olarak kullanılan iş ortası için ödeme yapılır. |
| **İş yükü türleri için en uygun** |  Öngörülebilir ve kararlı iş yükleri|   Öngörülemeyen ve değişken iş yükleri  |

## <a name="create-a-database-or-a-container-with-autopilot-mode"></a>Otomatik pilot moduna sahip bir veritabanı veya kapsayıcı oluşturma

Azure portalı üzerinden oluştururken yeni veritabanları veya kapsayıcılar için otomatik pilot yapılandırabilirsiniz. Yeni bir veritabanı veya kapsayıcı oluşturmak, otomatik pilotu etkinleştirmek ve maksimum verimi (RU/s) belirtmek için aşağıdaki adımları kullanın.

1. [Azure portalında](https://portal.azure.com) veya [Azure Cosmos DB gezgininde](https://cosmos.azure.com/) oturum açın.

1. Azure Cosmos DB hesabınıza gidin ve **Veri Gezgini** sekmesini açın.

1. **Yeni Kapsayıcı'yı seçin.** Veritabanınız, kapsayıcınız ve bir bölüm anahtarınız için bir ad girin. **İş İlerletme**altında, **Otomatik Pilot** seçeneğini seçin ve otomatik pilot seçeneğini kullanırken veritabanının veya kapsayıcının geçemeyeceği maksimum verimi (RU/s) seçin.

   ![Kapsayıcı oluşturma ve Otomatik Pilot iş buzunu yapılandırma](./media/provision-throughput-autopilot/create-container-autopilot-mode.png)

1. **Tamam'ı**seçin.

**Provision veritabanı iş oluşturma** seçeneğini seçerek otomatik pilot moduna sahip paylaşılan bir iş veri tabanı oluşturabilirsiniz.

## <a name="throughput-and-storage-limits-for-autopilot"></a><a id="autopilot-limits"></a>Otomatik pilot için iş ve depolama limitleri

Aşağıdaki tablo, otomatik pilot modunda farklı seçenekler için maksimum ve depolama sınırlarını gösterir:

|Maksimum verim sınırı  |Maksimum depolama sınırı  |
|---------|---------|
|4000 RU/s  |   50 GB    |
|20.000 RU/s  |  200 GB  |
|100.000 RU/s    |  1 TB   |
|500.000 RU/s    |  5 TB  |

## <a name="next-steps"></a>Sonraki adımlar

* Otomatik [pilot SSS'sini gözden geçirin.](autopilot-faq.md)
* [Mantıksal bölümler](partition-data.md)hakkında daha fazla bilgi edinin.
* [Azure Cosmos kapsayıcısına nasıl iş veriş sağlamayı](how-to-provision-container-throughput.md)öğrenin.
* [Azure Cosmos veritabanında iş ortası sağlama](how-to-provision-database-throughput.md)yı öğrenin.
