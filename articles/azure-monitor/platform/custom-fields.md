---
title: Azure Monitör'de özel alanlar (Önizleme) | Microsoft Dokümanlar
description: Azure Monitor'un Özel Alanlar özelliği, toplanan kaydın özelliklerine katkıda bulunan Bir Günlük Analizi çalışma alanındaki kayıtlardan kendi aranabilir alanlarınızı oluşturmanıza olanak tanır.  Bu makalede, özel bir alan oluşturmak için işlem açıklanır ve örnek bir olay ile ayrıntılı bir gözden geçirme sağlar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/23/2019
ms.openlocfilehash: bfb0a73631564c96a4af745fe9d7540a3a84f9c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77655370"
---
# <a name="create-custom-fields-in-a-log-analytics-workspace-in-azure-monitor-preview"></a>Azure Monitörü'nde Günlük Analizi çalışma alanında özel alanlar oluşturma (Önizleme)

> [!NOTE]
> Bu makalede, bir Log Analytics çalışma alanında metin verilerinin nasıl toplandığı ayrıştırılırken nasıl ayrıştırılanınca açıklanmaktadır. [Azure Monitor'daki Parse metin verilerinde](../log-query/parse-text.md)açıklanan kılavuzdan sonra toplanan bir sorgu filtresinde metin verilerini ayrıştırmanızı öneririz. Özel alanları kullanmanın üzerinde çeşitli avantajlar sağlar.

Azure Monitor'un **Özel Alanlar** özelliği, kendi aranabilir alanlarınızı ekleyerek Log Analytics çalışma alanınızdaki varolan kayıtları genişletmenize olanak tanır.  Özel alanlar, aynı kayıttaki diğer özelliklerden çıkarılan verilerden otomatik olarak doldurulur.

![Genel Bakış](media/custom-fields/overview.png)

Örneğin, aşağıdaki örnek kayıt olay açıklamasında gömülü yararlı verilere sahiptir. Bu verilerin ayrı bir özelliğe ayıklanması, bu verilerin sıralama ve filtreleme gibi eylemler için kullanılabilir olmasını sağlar.

![Örnek özü](media/custom-fields/sample-extract.png)

> [!NOTE]
> Önizleme'de, çalışma alanınızda 100 özel alanla sınırlıdırsınız.  Bu özellik genel kullanılabilirliğe ulaştığında bu sınır genişletilir.

## <a name="creating-a-custom-field"></a>Özel alan oluşturma
Özel bir alan oluşturduğunuzda, Log Analytics değerini doldurmak için hangi veriyi kullanacağınızı anlamalıdır.  Bu verileri hızlı bir şekilde tanımlamak için Microsoft Research FlashExtract adlı bir teknoloji kullanır.  Azure Monitor, açık yönergeler sağlamanızı istemek yerine, sağladığınız örneklerden ayıklamak istediğiniz veriler hakkında bilgi ediner.

Aşağıdaki bölümler, özel bir alan oluşturmak için yordamı sağlar.  Bu makalenin alt kısmında bir örnek çıkarma bir walkthrough olduğunu.

> [!NOTE]
> Belirtilen ölçütlerle eşleşen kayıtlar Log Analytics çalışma alanına eklendikçe özel alan doldurulur, bu nedenle yalnızca özel alan oluşturulduktan sonra toplanan kayıtlarda görünür.  Özel alan, oluşturulduğunda veri deposunda zaten bulunan kayıtlara eklenmez.
> 

### <a name="step-1--identify-records-that-will-have-the-custom-field"></a>Adım 1 – Özel alana sahip olacak kayıtları belirleme
İlk adım, özel alanı alacak kayıtları tanımlamaktır.  Standart bir [günlük sorgusuyla](../log-query/log-query-overview.md) başlarsınız ve ardından Azure Monitor'un öğreneceği model olarak hareket etmek için bir kayıt seçersiniz.  Verileri özel bir alana ayıklamanızı belirttiğiniz zaman, ölçütleri doğruladığınız ve iyileştirdiğiniz **Alan Ayıklama Sihirbazı** açılır.

1. **Günlükler'e** gidin ve özel alana sahip [olan kayıtları almak için](../log-query/log-query-overview.md) bir sorgu kullanın.
2. Log Analytics'in özel alanı doldurmak için veri ayıklamak için bir model olarak kullanacağı bir kayıt seçin.  Bu kayıttan ayıklamak istediğiniz verileri tanımlarsınız ve Log Analytics benzer tüm kayıtlar için özel alanı doldurmak için mantığı belirlemek için bu bilgileri kullanır.
3. Kayıt özelliklerini genişletin, kaydın en üst özelliğinin solundaki elipsi tıklatın ve **'den ayıklama alanlarını**seçin.
4. **Alan Çıkarma Sihirbazı** açılır ve seçtiğiniz kayıt **Ana Örnek** sütununda görüntülenir.  Özel alan, seçilen özelliklerde aynı değerlere sahip kayıtlar için tanımlanır.  
5. Seçim tam olarak istediğiniz gibi değilse, ölçütleri daraltmak için ek alanlar seçin.  Ölçütler için alan değerlerini değiştirmek için, iptal etmeli ve istediğiniz ölçütleri eşleşen farklı bir kayıt seçmeniz gerekir.

### <a name="step-2---perform-initial-extract"></a>Adım 2 - İlk özü gerçekleştirin.
Özel alana sahip olan kayıtları tanımladıktan sonra ayıklamak istediğiniz verileri tanımlarsınız.  Log Analytics bu bilgileri benzer kayıtlardaki benzer desenleri tanımlamak için kullanır.  Bundan sonraki adımda sonuçları doğrulayabilecek ve Log Analytics'in analizinde kullanması için daha fazla ayrıntı sağlayabilirsiniz.

1. Özel alanı doldurmak istediğiniz örnek kayıttaki metni vurgulayın.  Daha sonra alan için bir ad ve veri türü sağlamak ve ilk ayıklama gerçekleştirmek için bir iletişim kutusu ile sunulacaktır.  ** \_CF** karakterleri otomatik olarak eklenir.
2. Toplanan **Extract** kayıtların analizini gerçekleştirmek için Ayıklama'yı tıklatın.  
3. **Özet** ve **Arama Sonuçları** bölümleri ekstresonuçlarını görüntüler, böylece doğruluğunu inceleyebilirsiniz.  **Özet,** kayıtları tanımlamak için kullanılan ölçütleri ve tanımlanan veri değerlerinin her biri için bir sayım görüntüler.  **Arama Sonuçları,** ölçütleriyle eşleşen kayıtların ayrıntılı bir listesini sağlar.

### <a name="step-3--verify-accuracy-of-the-extract-and-create-custom-field"></a>Adım 3 – Ekstrenin doğruluğunu doğrulayın ve özel alan oluşturun
İlk ekstresi gerçekleştirdikten sonra, Log Analytics sonuçlarını zaten toplanmış verilere göre görüntüler.  Sonuçlar doğru görünüyorsa, daha fazla çalışma olmadan özel alanı oluşturabilirsiniz.  Değilse, Log Analytics'in mantığını geliştirebileceği sonuçları hassaslaştırabilirsiniz.

1. İlk ekstredeki herhangi bir değer doğru değilse, hatalı bir kaydın yanındaki **Düzenle** simgesini tıklatın ve seçimi değiştirmek için **bu vurguyu değiştir'i** seçin.
2. Giriş, **Ana Örnek'in**altındaki **Ek örnekler** bölümüne kopyalanır.  Log Analytics'in yapması gereken seçimi anlamasına yardımcı olmak için vurguyu burada ayarlayabilirsiniz.
3. Varolan tüm kayıtları değerlendirmek için bu yeni bilgileri kullanmak için **Ayıklama'yı** tıklatın.  Sonuçlar, bu yeni zekaya dayanarak yeni değiştirdiğiniz kayıt dışındaki kayıtlar için değiştirilebilir.
4. Ekstredeki tüm kayıtlar, yeni özel alanı doldurmak için verileri doğru bir şekilde tanımlayana kadar düzeltmeeklemeye devam edin.
5. Sonuçlardan memnun olduğunuzda **Ayıkla'yı Kaydet'i** tıklatın.  Özel alan şimdi tanımlanır, ancak henüz hiçbir kayda eklenmez.
6. Belirtilen ölçütlerin toplanmasını eşleşen yeni kayıtları bekleyin ve ardından günlük aramasını yeniden çalıştırın. Yeni kayıtlar özel alana sahip olmalıdır.
7. Diğer kayıt özelliği gibi özel alanı kullanın.  Verileri toplamak ve gruplandırmak ve hatta yeni öngörüler üretmek için kullanabilirsiniz.

## <a name="viewing-custom-fields"></a>Özel alanları görüntüleme
Azure portalındaki Log Analytics çalışma alanınızın **Gelişmiş Ayarlar** menüsünden yönetim grubunuzdaki tüm özel alanların listesini görüntüleyebilirsiniz.  Çalışma alanınızdaki tüm özel alanların listesi için **Veri** ve ardından **Özel alanları** seçin.  

![Özel alanlar](media/custom-fields/list.png)

## <a name="removing-a-custom-field"></a>Özel bir alanı kaldırma
Özel bir alanı kaldırmanın iki yolu vardır.  Bunlardan ilki, yukarıda açıklandığı gibi tam listeyi görüntülerken her alan için **Kaldır** seçeneğidir.  Diğer yöntem, bir kayıt almak ve alanın solundaki düğmeyi tıklatın.  Menüde özel alanı kaldırma seçeneği vardır.

## <a name="sample-walkthrough"></a>Örnek kılavuz
Aşağıdaki bölüm, özel bir alan oluşturmanın tam bir örneğini gözden geçirir.  Bu örnek, Windows olaylarında hizmet değiştirme durumunu gösteren hizmet adını ayıklar.  Bu, Windows bilgisayarlarında sistem başlatma sırasında Service Control Manager tarafından oluşturulan olaylara dayanır.  Bu örneği izlemek istiyorsanız, Sistem [günlüğü için Bilgi olayları topluyor](data-sources-windows-events.md)olmalısınız.

7036 Olay Kimliği olan ve hizmetin başlatılmasını veya durdurulduğu bir olayı gösteren Hizmet Kontrol Yöneticisi'nden tüm olayları döndürmek için aşağıdaki sorguyu gireriz.

![Sorgu](media/custom-fields/query.png)

Daha sonra event ID 7036 ile herhangi bir kaydı seçer ve genişletiriz.

![Kaynak kaydı](media/custom-fields/source-record.png)

En üst teki özelliğin yanındaki elipsi tıklatarak özel alanlar tanımlıyoruz.

![Ayıklama alanları](media/custom-fields/extract-fields.png)

**Alan Ayıklama Sihirbazı** açılır ve **Olay Günlüğü** ve **EventID** alanları **Ana Örnek** sütununda seçilir.  Bu, 7036 olay kimliğiile Sistem günlüğünden olaylar için özel alanın tanımlanacağını gösterir.  Bu, başka alan seçmemiz gerekmeden yeterlidir.

![Ana örnek](media/custom-fields/main-example.png)

**RenderedDescription** özelliğinde hizmetin adını vurgular ve hizmet adını tanımlamak için **Hizmeti** kullanırız.  Özel alan **Service_CF**olarak adlandırılacak. Bu durumda alan türü bir dize, bu yüzden değişmeden bırakabilirsiniz.

![Alan Adı](media/custom-fields/field-title.png)

Hizmet adının bazı kayıtlar için doğru şekilde tanımladığını, ancak diğerleri için tanımlanmadığını görüyoruz.   **Arama Sonuçları,** **WMI Performans Bağdaştırıcısı** adının bir bölümünün seçilmediğini gösterir.  **Özet,** bir kaydın **Windows Modülleri Yükleyici**yerine **Modül yükleyicisini** tanımladığını gösterir.  

![Arama sonuçları](media/custom-fields/search-results-01.png)

**WMI Performance Adaptör** kaydıyla başlıyoruz.  Onun edit simgesine tıklayın ve sonra **bu vurgulamak değiştirin.**  

![Vurgusunu değiştirme](media/custom-fields/modify-highlight.png)

**WMI** sözcükünü eklemek için vurguyu artırıyoruz ve ekstresi yeniden çalıştırıyoruz.  

![Ek örnek](media/custom-fields/additional-example-01.png)

**WMI Performans Bağdaştırıcısı** için girişlerin düzeltildiğini ve Log Analytics'in de bu bilgileri **Windows Modül Yükleyicisinin**kayıtlarını düzeltmek için kullandığını görebiliriz.

![Arama sonuçları](media/custom-fields/search-results-02.png)

Artık **Service_CF** oluşturulan ancak henüz herhangi bir kayda eklenmemiş bir sorgu çalıştırabiliriz. Bunun nedeni, özel alanın varolan kayıtlara karşı çalışmamasıdır, bu nedenle yeni kayıtların toplanmasını beklememiz gerekir.

![İlk sayım](media/custom-fields/initial-count.png)

Yeni etkinliklerin toplanması için bir süre geçtikten sonra, **Service_CF** alanının artık ölçütlerimize uygun kayıtlara eklendiğini görebiliriz.

![Kesin sonuçlar](media/custom-fields/final-results.png)

Artık diğer kayıt özelliği gibi özel alanı kullanabiliriz.  Bunu göstermek için, hangi hizmetlerin en etkin olduğunu denetlemek için yeni **Service_CF** alanına göre gruplanan bir sorgu oluştururuz.

![Sorguya göre gruplandırma](media/custom-fields/query-group.png)

## <a name="next-steps"></a>Sonraki adımlar
* Ölçütler için özel alanları kullanarak sorgu oluşturmak için [günlük sorguları](../log-query/log-query-overview.md) hakkında bilgi edinin.
* Özel alanları kullanarak ayrıştIzlediğiniz [özel günlük dosyalarını](data-sources-custom-logs.md) izleyin.

