---
title: Azure Veri Kataloğu sık sorulan sorular
description: Veri kaynağı bulma, ek açıklama ve yönetim yetenekleri de dahil olmak üzere Azure Veri Kataloğu hakkında sık sorulan sorular.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: 7dbb1b4a8b85350b8bf8a6df0c9035a19055444c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79409028"
---
# <a name="azure-data-catalog-frequently-asked-questions"></a>Azure Veri Kataloğu sık sorulan sorular
Bu makalede, Azure Veri Kataloğu hizmetiyle ilgili sık sorulan soruların yanıtları sağlanmaktadır.

## <a name="what-is-azure-data-catalog"></a>Azure Veri Kataloğu nedir?
Veri Kataloğu, kurumsal veri kaynakları için bir kayıt ve keşif sistemi olarak hizmet veren, Microsoft Azure'da barındırılan tam olarak yönetilen bir hizmettir. Veri Kataloğu ile, analistlerden veri bilimcilerine ve geliştiricilere kadar her kullanıcı veri kaynaklarını kaydedebilir, keşfedebilir, anlayabilir ve tüketebilir.

## <a name="what-customer-challenges-does-it-solve"></a>Hangi müşteri zorluklarını çözer?
Veri Kataloğu, kullanıcıların kurumsal veri kaynaklarını keşfedip anlayabilmeleri için veri kaynağı bulma ve "karanlık veri" zorluklarına değiner.

## <a name="what-are-its-target-audiences"></a>Hedef kitleleri nelerdir?
Veri Kataloğu, teknik ve teknik olmayan kullanıcılar için tasarlanmıştır:

* Veri geliştiricileri ve BI ve analiz uzmanları: Başkalarının tükettiği veri ve analiz içeriği üretmekle sorumlu kişiler.
* Veri stewards: Veri hakkında bilgi sahibi olan kişiler, ne anlama geldiğini ve nasıl kullanılacağı.
* Veri tüketicileri: Kendi seçtikleri aracı kullanarak işlerini yapmak için ihtiyaç duydukları verileri kolayca keşfedebilme, anlama ve bunlara bağlanabilmesi gereken kişilerdir.
* Merkezi BT: Yüzlerce veri kaynağını iş kullanıcıları tarafından kullanılabilir hale getirmeleri gereken ve verilerin nasıl ve kim tarafından kullanıldığı konusunda gözetim idame ettirmesi gereken kişiler.

## <a name="what-is-its-availability-by-region"></a>Bölgeye göre kullanılabilirliği nedir?
Veri Kataloğu hizmetleri şu anda aşağıdaki veri merkezlerinde kullanılabilir:

* Batı ABD
* Doğu ABD
* Batı Avrupa
* Kuzey Avrupa
* Doğu Avustralya
* Güneydoğu Asya

## <a name="what-are-its-limits-on-the-number-of-data-assets"></a>Veri varlıklarının sayısıüzerindeki sınırları nelerdir?
Veri Kataloğu'nun Ücretsiz Sürümü 5.000 kayıtlı veri varlığıyla sınırlıdır.

Veri Kataloğu'nun Standart Sürümü, 100.000'e kadar kayıtlı veri varlığını destekler.

Tablolar, görünümler, dosyalar ve raporlar gibi Veri Kataloğu'nda kayıtlı herhangi bir nesne bir veri varlığı olarak sayılır.

## <a name="what-are-its-supported-data-source-and-asset-types"></a>Desteklenen veri kaynağı ve varlık türleri nelerdir?
Şu anda desteklenen veri kaynaklarının listesi için [Bkz. Veri Kataloğu DSR.](data-catalog-dsr.md)

## <a name="how-do-i-request-support-for-another-data-source"></a>Başka bir veri kaynağı için nasıl destek isteyebilirim?
Özellik istekleri ni ve diğer geri bildirimleri göndermek için [Azure Geri Bildirim Forumlarında Veri Kataloğu'na](https://feedback.azure.com/forums/906052-data-catalog/category/320788-data-sources)gidin.

## <a name="why-do-i-get-an-error-catalog-already-exists-when-i-try-to-create-a-new-catalog"></a>Yeni bir katalog oluşturmaya çalıştığımda *katalog zaten var* olan bir hata neden alabilirim?

Power BI Pro Lisansı ile Office 365 E5 satın aldığınızda, Microsoft aboneliğin bölgesinde otomatik olarak varsayılan bir katalog oluşturur. Bu katalog ücretsiz SKU kullanır. Office 365 / Power BI kullanıcı lisansı Office 365 Yönetim sayfasında yönetilir. 

Ancak, bu tür veri kataloğunun **Yönetici Seçeneği** yoktur ve Azure **portalında**görünmez. Bu tür veri kataloğunu silemezsiniz. Benzer şekilde, veri kataloğunu yeniden adlandırmanız yasaktır ve başka bir bölgeye taşıyamazsınız. 

Power BI Pro lisansı otomatik olarak atanan kullanıcı hesapları, Power BI Pro Lisansı ile Office 365 E5'e kaydolduklarında Lisans Sözleşmesi nedeniyle veri kataloğuna erişebilir. Bu tür bir kullanıcı, yönetim ayrıcalıkları olmadan veri kataloğu varlıklarına tam erişime sahiptir. Bu tür bir kullanıcı, Azure Veri Kataloğu'ndaki **Katalog Kullanıcı** rolünün bir parçası *değildir.*


## <a name="how-do-i-get-started-with-data-catalog"></a>Veri Kataloğu'na nasıl başlarım?
Başlamak için en iyi yolu [Veri Kataloğu ile Başlarken](data-catalog-get-started.md)giderek. Bu makalede, hizmetteki yeteneklerin uçtan uca genel bakışı dır.

## <a name="how-do-i-register-my-data"></a>Verilerimi nasıl kaydedebilirim?
Verilerinizi Veri Kataloğu'na kaydetmek için:
1. Azure Veri Kataloğu portalında, **Yayımla** alanında, Azure Veri Kataloğu kayıt aracını başlatın. 
2. Veri Kataloğu veri kaynağı kayıt aracında, Veri Kataloğu portalına erişmek için kullandığınız kimlik bilgileriyle oturum açın.
3. Veri kaynağını ve kaydetmek istediğiniz belirli varlıkları seçin.

## <a name="what-properties-does-it-extract-for-data-assets-that-are-registered"></a>Kayıtlı veri varlıkları için hangi özellikleri ayıklar?
Belirli özellikler veri kaynağından veri kaynağına farklılık gösterir, ancak genel olarak Veri Kataloğu yayımlama hizmeti aşağıdaki bilgileri ayıklar:

* Varlık Adı
* Varlık Türü
* Varlık Açıklaması
* Öznitelik/Sütun Adları
* Öznitelik/Sütun Veri Türleri
* Öznitelik/Sütun Açıklaması

> [!IMPORTANT]
> Veri varlıklarını Veri Kataloğu'na kaydetmek verilerinizi buluta taşımaz veya kopyalamaz. Varlıkları bir veri kaynağından kaydetmek, varlıkların meta verilerini Azure'a kopyalar, ancak veriler varolan veri kaynağı konumunda kalır. Bu kuralın istisnası, varlıkları kaydederken önizleme kayıtlarını veya veri profilini yüklemeyi seçmenizdir. Bir önizleme eklediğinizde, her varlıktan en fazla 20 kayıt kopyalanır ve Veri Kataloğu'nda anlık görüntü olarak depolanır. Bir veri profili eklediğinizde, toplam bilgiler hesaplanır ve katalogda depolanan meta verilere dahil edilir. Toplu bilgiler tabloların boyutunu, sütun başına null değerlerin in yüzdesini veya sütunlar için en az, en büyük ve ortalama değerleri içerebilir. 
>
>

> [!NOTE]
> Birinci sınıf **Açıklama** özelliğine sahip SQL Server Analysis Services gibi veri kaynakları için, Veri Kataloğu veri kaynağı kayıt aracı bu özellik değerini ayıklar. Birinci sınıf **Açıklama** özelliği olmayan *şirket içi* SQL Server ilişkisel veritabanları için, Veri Kataloğu veri kaynağı kayıt aracı, nesneler ve sütunlar için genişletilmiş **ms_description** özelliğinden değeri ayıklar. Bu özellik SQL Azure için desteklenmez. Daha fazla bilgi için veritabanı [nesnelerinde Genişletilmiş Özellikleri Kullanma](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx)konusuna bakın.
>
>

## <a name="how-long-should-it-take-for-newly-registered-assets-to-appear-in-the-catalog"></a>Yeni kaydedilmiş varlıkların katalogda görünmesi ne kadar sürer?
Varlıkları Veri Kataloğu'na kaydettirdikten sonra, Veri Kataloğu portalında görünmeleri için 5 ila 10 saniye lik bir süre olabilir.

## <a name="how-do-i-annotate-and-enrich-the-metadata-for-my-registered-data-assets"></a>Kayıtlı veri varlıklarım için meta verilere nasıl açıklama ek olabilirim ve zenginleştirebilirim?
Kayıtlı varlıklar için meta veri sağlamanın en basit yolu, Veri Kataloğu portalındaki varlığı seçmek ve ardından seçili nesne için özellikler bölmesine veya şema bölmesine değerleri girmektir.

Kayıt işlemi sırasında uzmanlar ve etiketler gibi bazı meta verileri de sağlayabilirsiniz. Veri Kataloğu yayımlama hizmetinde sağladığınız değerler, o anda kayıtlı olan tüm varlıklar için geçerlidir. Ek ek açıklama için portalda en son kaydedilmiş nesneleri görüntülemek için Veri Kataloğu veri kaynağı kayıt aracının son ekranındaki **Portalı Görüntüle** düğmesini seçin.

## <a name="how-do-i-delete-my-registered-data-objects"></a>Kayıtlı veri nesnelerimi nasıl silerim?
Portaldaki nesneyi seçip **Sil** düğmesini tıklatarak veri kataloğundan bir nesneyi silebilirsiniz. Nesneyi kaldırmak meta verilerini Veri Kataloğu'ndan kaldırır, ancak temel veri kaynağını etkilemez.

## <a name="what-is-an-expert"></a>Uzman nedir?
Uzman, veri nesnesi hakkında bilinçli bir bakış açısına sahip olan kişidir. Bir nesnenin birden çok uzmanı olabilir. Bir uzman bir nesne için "sahibi" olmak gerekmez, ama sadece nasıl veri ve kullanılması gerektiğini bilen birisidir.

## <a name="how-do-i-share-information-with-the-data-catalog-team-if-i-encounter-problems"></a>Sorunlarla karşılaştığımda Veri Kataloğu ekibiyle nasıl bilgi paylaşebilirim?
Sorunları bildirmek, bilgi paylaşmak ve soru sormak için [Azure Veri Kataloğu forumuna](https://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409)gidin.

## <a name="does-the-catalog-work-with-another-data-source-that-im-interested-in"></a>Katalog, ilgilendiğim başka bir veri kaynağıyla mı çalışıyor?
Veri Kataloğu'na daha fazla veri kaynağı ekleme üzerinde aktif olarak çalışıyoruz. Belirli bir veri kaynağının desteklendiğini görmek istiyorsanız, [Azure Geri Bildirim Forumlarında Veri Kataloğu'na](https://feedback.azure.com/forums/906052-data-catalog)giderek bunu önerin (veya zaten önerilmişse desteğinizi dile verin).

## <a name="what-permissions-do-i-need-to-register-assets-with-data-catalog"></a>Varlıkları Veri Kataloğu'na kaydetmek için hangi izinlere ihtiyacım var?
Veri Kataloğu kayıt aracını çalıştırmak için, veri kaynağında meta verileri kaynaktan okumanıza olanak tanıyan izinlere ihtiyacınız vardır. Bir önizleme eklemek için, kayıtlı nesnelerden gelen verileri okumanızı sağlayan izinlere sahip olmalısınız.

Veri Kataloğu ayrıca katalog yöneticilerinin hangi kullanıcıların ve grupların kataloğa meta veri ekleyebileceğini kısıtlamasına da olanak tanır. Daha fazla bilgi için [veri kataloğuna ve veri varlıklarına erişimin nasıl güvenli hale girilebildiğini](data-catalog-how-to-secure-catalog.md)öğrenin.

## <a name="will-data-catalog-be-made-available-for-on-premises-deployment-as-well"></a>Veri Kataloğu şirket içi dağıtım için de kullanılabilir olacak mı?
Veri Kataloğu, karma veri kaynağı bulma çözümü sunmak için hem bulut hem de şirket içi veri kaynaklarıyla çalışabilen bir bulut hizmetidir. Şu anda şirket içinde çalışan Veri Kataloğu hizmetinin bir sürümü için plan bulunmamaktadır.

## <a name="can-i-extract-more-or-richer-metadata-from-the-data-sources-i-register"></a>Kaydettiğim veri kaynaklarından daha fazla veya daha zengin meta veri ayıklayabilir miyim?
Veri Kataloğu'nun yeteneklerini genişletmek için aktif olarak çalışıyoruz. Kayıt sırasında veri kaynağından ek meta verilerin çıkarılmasını istiyorsanız, [Azure Geri Bildirim Forumları'ndaki Veri Kataloğu'nda](https://feedback.azure.com/forums/906052-data-catalog)bunu önerin (veya önceden önerilmişse ona oy verin). 

Bu meta verilerin veri kaynağı kayıt aracı tarafından ayıklanmadığı veri kaynakları için sütun/şema meta verileri, önizlemeleri veya veri profilleri eklemek isterseniz, bu meta verileri eklemek için Veri Kataloğu API'sini kullanabilirsiniz. Daha fazla bilgi için Azure [Veri Kataloğu REST API'ye](https://docs.microsoft.com/rest/api/datacatalog/)bakın.

## <a name="how-do-i-restrict-the-visibility-of-registered-data-assets-so-that-only-certain-people-can-discover-them"></a>Yalnızca belirli kişilerin bunları keşfedebilmeleri için kayıtlı veri varlıklarının görünürlüğünü nasıl kısıtlayabilirim?
Veri Kataloğu'ndaki veri varlıklarını seçin ve ardından **Sahipliği Al** düğmesini tıklatın. Veri Kataloğu'ndaki veri varlıklarının sahipleri, tüm kullanıcıların sahip olunan varlıkları keşfetmesine veya belirli kullanıcılarla görünürlüğü kısıtlamasına izin verecek şekilde görünürlük ayarlarını değiştirebilir. Daha fazla bilgi için bkz. Azure [Veri Kataloğu'ndaki veri varlıklarını yönet.](data-catalog-how-to-manage.md)

## <a name="how-do-i-update-the-registration-for-a-data-asset-so-that-changes-in-the-data-source-are-reflected-in-the-catalog"></a>Veri kaynağındaki değişikliklerin kataloga yansıtılabilmesi için bir veri varlığının kaydını nasıl güncellerim?
Katalogda zaten kayıtlı olan veri varlıklarının meta verilerini güncelleştirmek için varlıkları içeren veri kaynağını yeniden kaydetmemiz yeterlidir. Tablolar veya görünümlerden eklenen veya kaldırılan sütunlar gibi veri kaynağındaki tüm değişiklikler katalogda güncelleştirilir, ancak kullanıcılar tarafından sağlanan ek açıklamalar korunur.

## <a name="my-question-isnt-answered-here-where-can-i-go-for-answers"></a>Sorum burada cevaplanmadı. Cevaplar için nereye gidebilirim?
[Azure Veri Kataloğu forumuna](https://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409)gidin. Orada sorulan sorular buraya gelecektir.
