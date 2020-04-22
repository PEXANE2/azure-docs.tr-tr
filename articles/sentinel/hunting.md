---
title: Azure Sentinel'de avlanma özellikleri| Microsoft Dokümanlar
description: Bu makalede, Azure Sentinel avlanma yeteneklerinin nasıl kullanılacağı açıklanmaktadır.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 6aa9dd27-6506-49c5-8e97-cc1aebecee87
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/10/2019
ms.author: yelevin
ms.openlocfilehash: 52af688917aa531d125f83844df29a988ed7cb7e
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81686625"
---
# <a name="hunt-for-threats-with-azure-sentinel"></a>Azure Sentinel ile tehdit avı

Güvenlik tehditleri arama konusunda proaktif olmak isteyen bir araştırmacıysanız, Azure Sentinel kuruluşunuzun veri kaynaklarında güvenlik tehditlerini avlamak için güçlü avlanma arama ve sorgu araçları. Ancak sistemleriniz ve güvenlik cihazlarınız, anlamlı olaylara ayrıştırılması ve filtreleilmesi zor olabilecek dağlarda veri üretir. Güvenlik analistlerinin güvenlik uygulamalarınız tarafından algılanmayan yeni anormallikleri proaktif olarak aramalarına yardımcı olmak için, Azure Sentinel'in yerleşik avlanma sorguları, ağınızda zaten bulunan verilerdeki sorunları bulmak için doğru soruları sormanız için size yol gösteriyor. 

Örneğin, yerleşik bir sorgu altyapınızda çalışan en yaygın işlemler le ilgili veriler sağlar - çalıştırılan her işlem hakkında bir uyarı istemezsiniz, bunlar tamamen masum olabilir, ancak olağandışı bir şey olup olmadığını görmek için zaman zaman sorguya bir göz atmak isteyebilirsiniz. 



Azure Sentinel avı ile aşağıdaki özelliklerden yararlanabilirsiniz:

- Yerleşik sorgular: Başlangıç sayfası, başlangıç sayfanızı başlatmak ve tabloları ve sorgu dilini tanımak için önceden yüklenmiş sorgu örnekleri sağlar. Bu yerleşik avcılık sorguları, Microsoft güvenlik araştırmacıları tarafından sürekli olarak geliştirilir, yeni sorgular ekler ve yeni algılamalar aramak ve yeni saldırıların başlangıcını nereden avlayabileceğinizi bulmak için bir giriş noktası sağlamak için varolan sorguları ince ayarlayayım. 

- IntelliSense ile güçlü sorgu dili: Bir sonraki seviyeye avcılık almak için gereken esneklik sağlayan bir sorgu dili üstüne inşa edilmiştir.

- Kendi yer imlerinizi oluşturun: Avlanma işlemi sırasında, alışılmadık veya şüpheli görünen eşleşmeler veya bulgular, panolar veya etkinliklerle karşılaşabilirsiniz. Gelecekte onlara geri dönebilmeniz için bu öğeleri işaretlemek için yer imi işlevini kullanın. Yer imleri, soruşturma için bir olay oluşturmak için kullanılacak öğeleri daha sonraya kaydetmenize izin verir. Yer imleri hakkında daha fazla bilgi için [bkz.](hunting.md)
- Araştırmayı otomatikleştirmek için not defterlerini kullanın: Not defterleri, bir araştırmanın ve avlanmanın adım adım adım yürümek için oluşturabileceğiniz adım adım oyun kitapları gibidir.  Not defterleri, kuruluşunuzdaki diğer kişilerle paylaşılabilen yeniden kullanılabilir bir oyun kitabındaki tüm avlanma adımlarını kapsüller. 
- Depolanan verileri sorgula: Veriler, sorgulayabilmek için tablolarda erişilebilir. Örneğin, işlem oluşturma, DNS olayları ve diğer birçok olay türünü sorgulayabilirsiniz.

- Topluluğa bağlantılar: Ek sorgular ve veri kaynakları bulmak için topluluğun gücünden yararlanın.
 
## <a name="get-started-hunting"></a>Avlanmaya başlayın

1. Azure Sentinel portalında **Avcılık'ı**tıklatın.
  ![Azure Sentinel avlanmaya başlıyor](media/tutorial-hunting/hunting-start.png)

2. **Avcılık** sayfasını açtığınızda, tüm av sorguları tek bir tabloda görüntülenir. Tablo, Microsoft'un güvenlik analistleri ekibi tarafından yazılan tüm sorguların yanı sıra oluşturduğunuz veya değiştirdiğiniz ek sorguları listeler. Her sorgu, ne için avlanır ve ne tür verilerle çalıştığının açıklamasını sağlar. Bu şablonlar çeşitli taktiklerine göre gruplandırılır - sağdaki simgeler ilk erişim, kalıcılık ve sızma gibi tehdit türünü kategorilere ayırın. Bu avlanma sorgusu şablonlarını alanlardan herhangi birini kullanarak filtreleyebilirsiniz. Herhangi bir sorgunuzu sık kullanılanlarınıza kaydedebilirsiniz. Bir sorguyu sık kullanılanlarınıza kaydederek, **Avcılık** sayfasına her erişiğinde sorgu otomatik olarak çalışır. Kendi avcılık sorgunuzu veya klonunuzu oluşturabilir ve varolan bir av sorgusu şablonunu özelleştirebilirsiniz. 
 
2. Avcılık sayfasından çıkmadan herhangi bir sorguyu çalıştırmak için avcılık sorgusu ayrıntıları sayfasında **sorguyu çalıştır'ı** tıklatın.  Eşleşme sayısı tablo içinde görüntülenir. Avcılık sorguları ve bunların eşleşmeleri listesini gözden geçirin. Öldürme zincirinin hangi aşamada ile ilişkili olduğunu kontrol edin.

3. Sorgu ayrıntıları bölmesinde temel sorgunun hızlı bir incelemesini gerçekleştirin veya Sorguyu Log Analytics'te açmak için **sorgu sonucunu görüntüle'yi** tıklatın. Altta, sorgu için eşleşmeleri gözden geçirin.

4.    Satıra tıklayın ve araştırılacak satırları eklemek için **yer imi** ekle'yi seçin - bunu şüpheli görünen her şey için yapabilirsiniz. 

5. Ardından, ana **Avcılık** sayfasına geri dön ve tüm şüpheli etkinlikleri görmek için **Yer İşaretleri** sekmesini tıklatın. 

6. Bir yer imi seçin ve ardından araştırma deneyimini açmak için **Araştır'ı** tıklatın. Yer imlerini filtreleyebilirsiniz. Örneğin, bir kampanyayı araştırıyorsanız, kampanya için bir etiket oluşturabilir ve ardından kampanyaya göre tüm yer imlerini filtreleyebilirsiniz.

1. Hangi av sorgusunun olası saldırılara ilişkin yüksek değerli bilgiler sağladığını keşfettikten sonra, sorgunuza dayalı özel algılama kuralları oluşturabilir ve bu öngörüleri güvenlik olayı yanıtlayıcılarınıza uyarı olarak sunabilirsiniz.

 

## <a name="query-language"></a>Sorgu dili 

Azure Sentinel'de avlanmak Kusto sorgu diline dayanır. Sorgu dili ve desteklenen işleçler hakkında daha fazla bilgi için Sorgu [Dili Başvurusu'na](/azure/azure-monitor/log-query/get-started-queries)bakın.

## <a name="public-hunting-query-github-repository"></a>Genel avcılık sorgusu GitHub deposu

[Avcılık sorgu deposuna](https://github.com/Azure/Orion)göz atın. Müşterilerimiz tarafından paylaşılan örnek sorgulara katkıda bulunun ve bunları kullanın.

 

## <a name="sample-query"></a>Örnek sorgu

Tipik bir sorgu, tablo adı ile başlar ve \|ardından bir dizi işleç tarafından ayrılmıştır.

Yukarıdaki örnekte, Tablo adı SecurityEvent ile başlayın ve gerektiğinde borulu öğeler ekleyin.

1. Yalnızca önceki yedi güne ait kayıtları gözden geçirmek için bir zaman filtresi tanımlayın.

2. Yalnızca olay kimliği 4688'i göstermek için sorguya bir filtre ekleyin.

3. Yalnızca cscript.exe örneklerini içerecek şekilde CommandLine'daki sorguya bir filtre ekleyin.

4. Yalnızca keşfetmek istediğiniz sütunları projeleyin ve sonuçları 1000 ile sınırlayın ve **Sorguyu Çalıştır'ı**tıklatın.
5. Yeşil üçgeni tıklatın ve sorguyu çalıştırın. Sorguyu sınayabilir ve anormal davranışları aramak için çalıştırabilirsiniz.

## <a name="useful-operators"></a>Yararlı işleçler

Sorgu dili güçlüdür ve birçok kullanılabilir işleçleri vardır, bazı yararlı işleçler burada listelenir:

**nerede** - Tabloyu bir yüklemi karşılayan satır alt kümesine filtreleyin.

**özetlemek** - Giriş tablosunun içeriğini toplayan bir tablo üretin.

**join** - Her tablodan belirtilen sütun(lar) değerlerini eşleştirerek yeni bir tablo oluşturmak için iki tablonun satırlarını birleştirin.

**count** - Giriş kayıt kümesindeki kayıt sayısını döndürün.

**üst** - Belirtilen sütunlara göre sıralanmış ilk N kayıtlarını döndürün.

**limit** - Belirtilen satır sayısına kadar dön.

**proje** - Eklemek, yeniden adlandırmak veya bırakmak için sütunları seçin ve yeni açılan sütunlar ekleyin.

**genişletmek** - Hesaplanan sütunlar oluşturun ve bunları sonuç kümesine ekleyin.

**makeset** - Expr'ın grupta aldığı farklı değerler kümesinin dinamik (JSON) dizisini döndürme

**bul** - Bir tablo kümesi boyunca yüklemle eşleşen satırları bulun.

## <a name="save-a-query"></a>Sorgukaydetme

Bir sorgu oluşturabilir veya değiştirebilir ve kendi sorgunuz olarak kaydedebilir veya aynı kiracıdaki kullanıcılarla paylaşabilirsiniz.

   ![Sorguyı kaydet](./media/tutorial-hunting/save-query.png)

Yeni bir avcılık sorgusu oluşturun:

1. **Yeni sorgu'ya** tıklayın ve **Kaydet'i**seçin.
2. Tüm boş alanları doldurun ve **Kaydet'i**seçin.

   ![Yeni sorgu](./media/tutorial-hunting/new-query.png)

Varolan bir avı sorgusunun klonlanmave değiştirilmesi:

1. Değiştirmek istediğiniz tablodaki av sorgusunu seçin.
2. Değiştirmek istediğiniz sorgu satırındaki elipsleri (...) seçin ve **Klon sorgusunu**seçin.

   ![klon sorgusu](./media/tutorial-hunting/clone-query.png)
 

3. Sorguyu değiştirin ve **Oluştur'u**seçin.

   ![özel sorgu](./media/tutorial-hunting/custom-query.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Sentinel ile bir av araştırması nın nasıl yürütüldgerektiğini öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:


- [Otomatik avlanma kampanyaları yürütmek için dizüstü bilgisayarları kullanın](notebooks.md)
- [Avlanırken ilginç bilgileri kaydetmek için yer imlerini kullanın](bookmarks.md)
