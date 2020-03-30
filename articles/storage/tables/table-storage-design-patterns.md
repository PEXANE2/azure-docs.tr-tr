---
title: Azure depolama tablosu tasarım desenleri | Microsoft Dokümanlar
description: Azure tablo hizmeti çözümleri için desenleri kullanın.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/08/2019
ms.author: tamram
ms.subservice: tables
ms.openlocfilehash: 5478163a6103bcc84b4f3608d7513c6e7cb11c01
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79529348"
---
# <a name="table-design-patterns"></a>Tablo tasarımı desenleri
Bu makalede, Tablo hizmeti çözümleri ile kullanıma uygun bazı desenler açıklanmaktadır. Ayrıca, diğer Tablo depolama tasarım makalelerinde tartışılan bazı sorunları ve dengeleri pratik olarak nasıl ele alabileceğinizi göreceksiniz. Aşağıdaki diyagram, farklı desenler arasındaki ilişkileri özetler:  

![ilgili verilere bakmak için](media/storage-table-design-guide/storage-table-design-IMAGE05.png)


Yukarıdaki desen eşlemi, bu kılavuzda belgelenen desenler (mavi) ve anti-desenler (turuncu) arasındaki bazı ilişkileri vurgular. Dikkate değer birçok diğer desenler vardır. Örneğin, Tablo Hizmeti için önemli senaryolardan biri Komut Sorgusu Sorumluluk [Ayrımı (CQRS)](https://msdn.microsoft.com/library/azure/jj554200.aspx) deseninden [Maddeleştirilmiş Görünüm Deseni'ni](https://msdn.microsoft.com/library/azure/dn589782.aspx) kullanmaktır.  

## <a name="intra-partition-secondary-index-pattern"></a>Bölüm içi ikincil dizin deseni
Hızlı ve verimli aramalar ve farklı **RowKey** değerlerini kullanarak alternatif sıralama siparişleri etkinleştirmek için her varlığın birden çok kopyasını farklı **RowKey** değerlerini (aynı bölümde) kullanarak depolayın. Kopyalar arasındaki güncellemeler EGT'ler kullanılarak tutarlı tutulabilir.  

### <a name="context-and-problem"></a>Bağlam ve sorun
Tablo hizmeti, **PartitionKey** ve **RowKey** değerlerini kullanarak varlıkları otomatik olarak dizinler. Bu, istemci uygulamasının bu değerleri kullanarak etkin bir varlık almasına olanak tanır. Örneğin, aşağıda gösterilen tablo yapısını kullanarak, istemci uygulaması departman adını ve çalışan kimliğini **(PartitionKey** ve **RowKey** değerleri) kullanarak tek bir çalışan varlığını almak için bir nokta sorgusu kullanabilir. İstemci, her departmanda çalışan kimliğine göre sıralanmış varlıkları da alabilir.

![Resim06](media/storage-table-design-guide/storage-table-design-IMAGE06.png)

Ayrıca, e-posta adresi gibi başka bir özelliğin değerine dayalı bir çalışan varlığı bulmak istiyorsanız, eşleşme bulmak için daha az verimli bir bölüm taramayı kullanmanız gerekir. Bunun nedeni, tablo hizmetinin ikincil dizinler sağlamamasıdır. Ayrıca, **RowKey** siparişinden farklı bir sırada sıralanmış çalışanların listesini isteme seçeneği de yoktur.  

### <a name="solution"></a>Çözüm
İkincil dizinlerin eksikliğini aşmak için, her bir varlığın farklı bir **RowKey** değerini kullanarak her bir kopyayla birden çok kopyasını depolayabilirsiniz. Aşağıda gösterilen yapıları içeren bir varlığı depolarsanız, e-posta adresine veya çalışan kimliğine göre çalışan varlıklarını verimli bir şekilde alabilirsiniz. **RowKey**, "empid_" ve "email_" için önek değerleri, bir dizi e-posta adresi veya çalışan teşekkülskullanarak tek bir çalışan veya çeşitli çalışan için sorgu yapmanızı sağlar.  

![Çalışan varlıkları](media/storage-table-design-guide/storage-table-design-IMAGE07.png)

Aşağıdaki iki filtre ölçütü (biri çalışan kimliğine, biri e-posta adresine bakarak) nokta sorgularını belirt  

* $filter=(PartitionKey eq 'Satış') ve (RowKey eq 'empid_000223')  
* $filter=(PartitionKey eq 'Satış') ve (RowKey eq 'email_jonesj@contoso.com')  

Bir dizi çalışan varlığı için sorgu larsanız, Çalışan Kimliği siparişinde sıralanmış bir aralık veya **RowKey'de**uygun öneki olan varlıkları sorgulayarak e-posta adresi siparişinde sıralanmış bir aralık belirtebilirsiniz.  

* Satış departmanında 000100 ile 000199 arasında çalışan kimliği olan tüm çalışanları bulmak için: $filter=(PartitionKey eq 'Sales') ve (RowKey ge 'empid_000100') ve (RowKey le 'empid_000199')  
* 'a' harfi ile başlayan bir e-posta adresi ile Satış departmanında tüm çalışanları bulmak için: $filter =(PartitionKey eq 'Satış') ve (RowKey ge 'email_a') ve (RowKey lt 'email_b')  
  
  Yukarıdaki örneklerde kullanılan filtre sözdizimi Tablo hizmeti REST API'den, daha fazla bilgi için [sorgu varlıklarına](https://msdn.microsoft.com/library/azure/dd179421.aspx)bakın.  

### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Tablo depolama kullanımı nispeten ucuz olduğundan, yinelenen verileri depolama maliyeti yükü önemli bir sorun olmamalıdır. Ancak, her zaman beklenen depolama gereksinimlerinize göre tasarımınızın maliyetini değerlendirmeli ve yalnızca istemci uygulamanızın yürüteceği sorguları desteklemek için yinelenen varlıklar eklemeniz gerekir.  
* İkincil dizin varlıkları özgün varlıklarla aynı bölümde depolandığı için, tek bir bölüm için ölçeklenebilirlik hedeflerini aşmadığınızdan emin olmalısınız.  
* Varlığın iki kopyasını atomik olarak güncelleştirmek için EGT'leri kullanarak yinelenen varlıklarınızı birbiriyle tutarlı tutabilirsiniz. Bu, bir varlığın tüm kopyalarını aynı bölümde depolamanız gerektiği anlamına gelir. Daha fazla bilgi için [Varlık Grubu Hareketlerini Kullanma](table-storage-design.md#entity-group-transactions)bölümüne bakın.  
* **RowKey** için kullanılan değer her varlık için benzersiz olmalıdır. Bileşik anahtar değerlerini kullanmayı düşünün.  
* **RowKey'deki** sayısal değerleri (örneğin, çalışan kimliği 000223) doldurma, üst ve alt sınırlara göre doğru sıralama ve filtreleme sağlar.  
* Varlığınızın tüm özelliklerini yinelemeniz gerekmez. Örneğin, **RowKey'deki** e-posta adresini kullanarak varlıkları arayan sorguların çalışanın yaşına gerek yoksa, bu varlıklar aşağıdaki yapıya sahip olabilir:

   ![Çalışan varlık yapısı](media/storage-table-design-guide/storage-table-design-IMAGE08.png)


* Yinelenen verileri depolamak ve gerekli verileri aramak için bir sorgu ve başka bir sorgu kullanmak yerine, tek bir sorgu ile ihtiyacınız olan tüm verileri alabilirsiniz sağlamak için genellikle daha iyidir.  

### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
İstemci uygulamanızın çeşitli farklı anahtarları kullanarak varlıkları alması gerektiğinde, istemcinizin farklı sıralama siparişlerinde varlıkları alması gerektiğinde ve her varlığı çeşitli benzersiz değerler kullanarak tanımlayabileceğiniz durumlarda bu deseni kullanın. Ancak, farklı **RowKey** değerlerini kullanarak varlık aramaları gerçekleştirirken bölüm ölçeklenebilirlik sınırlarını aşmadığınızdan emin olmalısınız.  

### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Bölümler arası ikincil dizin deseni](#inter-partition-secondary-index-pattern)
* [Bileşik anahtar deseni](#compound-key-pattern)
* Varlık Grubu İşlemleri
* [Heterojen varlık tipleri ile çalışma](#working-with-heterogeneous-entity-types)

## <a name="inter-partition-secondary-index-pattern"></a>Bölümler arası ikincil dizin deseni
Farklı RowKey **değerlerini** kullanarak hızlı ve verimli aramalar ve alternatif sıralama siparişleri sağlamak için her varlığın farklı **RowKey** değerlerini kullanarak farklı bölümlerde veya ayrı tablolarda birden fazla kopyasını depolayın.  

### <a name="context-and-problem"></a>Bağlam ve sorun
Tablo hizmeti, **PartitionKey** ve **RowKey** değerlerini kullanarak varlıkları otomatik olarak dizinler. Bu, istemci uygulamasının bu değerleri kullanarak etkin bir varlık almasına olanak tanır. Örneğin, aşağıda gösterilen tablo yapısını kullanarak, istemci uygulaması departman adını ve çalışan kimliğini **(PartitionKey** ve **RowKey** değerleri) kullanarak tek bir çalışan varlığını almak için bir nokta sorgusu kullanabilir. İstemci, her departmanda çalışan kimliğine göre sıralanmış varlıkları da alabilir.  

![Çalışan Kimlik Numarası](media/storage-table-design-guide/storage-table-design-IMAGE09.png)

Ayrıca, e-posta adresi gibi başka bir özelliğin değerine dayalı bir çalışan varlığı bulmak istiyorsanız, eşleşme bulmak için daha az verimli bir bölüm taramayı kullanmanız gerekir. Bunun nedeni, tablo hizmetinin ikincil dizinler sağlamamasıdır. Ayrıca, **RowKey** siparişinden farklı bir sırada sıralanmış çalışanların listesini isteme seçeneği de yoktur.  

Bu varlıklara karşı yüksek hacimli bir işlem bekliyorsunuz ve Tablo hizmetinin müşterinizi azaltma riskini en aza indirmek istiyorsunuz.  

### <a name="solution"></a>Çözüm
İkincil dizinlerin eksikliğini aşmak için, farklı **PartitionKey** ve **RowKey** değerlerini kullanarak her bir tüzel kişiliğin birden çok kopyasını her kopyayla depolayabilirsiniz. Aşağıda gösterilen yapıları içeren bir varlığı depolarsanız, e-posta adresine veya çalışan kimliğine göre çalışan varlıklarını verimli bir şekilde alabilirsiniz. **PartitionKey**, "empid_" ve "email_" için önek değerleri, bir sorgu için hangi dizini kullanmak istediğinizi belirlemenize olanak tanır.  

![Birincil indeks ve ikincil indeks](media/storage-table-design-guide/storage-table-design-IMAGE10.png)


Aşağıdaki iki filtre ölçütü (biri çalışan kimliğine, biri e-posta adresine bakarak) nokta sorgularını belirt  

* $filter=(PartitionKey eq 'empid_Sales') ve (RowKey eq '000223')
* $filter=(PartitionKey eq 'email_Sales') ve (RowKey eq 'jonesj@contoso.com')  

Bir dizi çalışan varlığı için sorgu larsanız, Çalışan Kimliği siparişinde sıralanmış bir aralık veya **RowKey'de**uygun öneki olan varlıkları sorgulayarak e-posta adresi siparişinde sıralanmış bir aralık belirtebilirsiniz.  

* **000100** ile **000199** aralığında çalışan kimliği ne kadar olan satış departmanındaki tüm çalışanları bulmak için çalışan kimliği kullanımında sıralanır: $filter=(PartitionKey eq 'empid_Sales') ve (RowKey ge '000100') ve (RowKey le '000199')  
* Satış departmanındaki tüm çalışanları e-posta adresi siparişi kullanımında 'a' ile başlayan bir e-posta adresiyle bulmak için: $filter=(PartitionKey eq 'email_Sales') ve (RowKey ge 'a') ve (RowKey lt 'b')  

Yukarıdaki örneklerde kullanılan filtre sözdizimi Tablo hizmeti REST API'den, daha fazla bilgi için [sorgu varlıklarına](https://msdn.microsoft.com/library/azure/dd179421.aspx)bakın.  

### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Birincil ve ikincil dizin varlıklarını korumak için [Son olarak tutarlı hareketler modelini](#eventually-consistent-transactions-pattern) kullanarak yinelenen varlıklarınızı sonunda birbiriyle tutarlı tutabilirsiniz.  
* Tablo depolama kullanımı nispeten ucuz olduğundan, yinelenen verileri depolama maliyeti yükü önemli bir sorun olmamalıdır. Ancak, her zaman beklenen depolama gereksinimlerinize göre tasarımınızın maliyetini değerlendirmeli ve yalnızca istemci uygulamanızın yürüteceği sorguları desteklemek için yinelenen varlıklar eklemeniz gerekir.  
* **RowKey** için kullanılan değer her varlık için benzersiz olmalıdır. Bileşik anahtar değerlerini kullanmayı düşünün.  
* **RowKey'deki** sayısal değerleri (örneğin, çalışan kimliği 000223) doldurma, üst ve alt sınırlara göre doğru sıralama ve filtreleme sağlar.  
* Varlığınızın tüm özelliklerini yinelemeniz gerekmez. Örneğin, **RowKey'deki** e-posta adresini kullanarak varlıkları arayan sorguların çalışanın yaşına gerek yoksa, bu varlıklar aşağıdaki yapıya sahip olabilir:
  
   ![Çalışan varlığı (ikincil dizin)](media/storage-table-design-guide/storage-table-design-IMAGE11.png)

* Yinelenen verileri depolamak ve tek bir sorguyla ihtiyacınız olan tüm verileri alabilmeniz, ikincil dizini kullanan bir varlığı bulmak için bir sorgu ve birincil dizinde gerekli verileri aramak için başka bir sorgu kullanmaktan daha iyidir.  

### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
İstemci uygulamanızın çeşitli farklı anahtarları kullanarak varlıkları alması gerektiğinde, istemcinizin farklı sıralama siparişlerinde varlıkları alması gerektiğinde ve her varlığı çeşitli benzersiz değerler kullanarak tanımlayabileceğiniz durumlarda bu deseni kullanın. Farklı **RowKey** değerlerini kullanarak varlık aramaları gerçekleştirirken bölüm ölçeklenebilirlik sınırlarını aşmaktan kaçınmak istediğinizde bu deseni kullanın.  

### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Sonunda tutarlı hareketler deseni](#eventually-consistent-transactions-pattern)  
* [Bölüm içi ikincil dizin deseni](#intra-partition-secondary-index-pattern)  
* [Bileşik anahtar deseni](#compound-key-pattern)  
* Varlık Grubu İşlemleri  
* [Heterojen varlık tipleri ile çalışma](#working-with-heterogeneous-entity-types)  

## <a name="eventually-consistent-transactions-pattern"></a>Sonunda tutarlı hareketler deseni
Azure kuyruklarını kullanarak bölüm sınırları veya depolama sistemi sınırları arasında en geç tutarlı davranışı etkinleştirin.  

### <a name="context-and-problem"></a>Bağlam ve sorun
EGT'ler, aynı bölüm anahtarını paylaşan birden çok kuruluş arasında atomik işlemlere olanak tanır. Performans ve ölçeklenebilirlik nedenleriyle, tutarlılık gereksinimleri olan varlıkları ayrı bölümlerde veya ayrı bir depolama sisteminde depolamaya karar verebilirsiniz: böyle bir senaryoda, tutarlılığı korumak için EGT'leri kullanamazsınız. Örneğin, arasında nihai tutarlılığı korumak için bir gereksinim olabilir:  

* Aynı tabloda, farklı tablolarda veya farklı depolama hesaplarında iki farklı bölümde depolanan varlıklar.  
* Tablo hizmetinde depolanan bir varlık ve Blob hizmetinde depolanan bir blob.  
* Tablo hizmetinde depolanan bir varlık ve dosya sistemindeki bir dosya.  
* Azure Bilişsel Arama hizmetini kullanarak tablo hizmetinde depolanan bir varlık.  

### <a name="solution"></a>Çözüm
Azure kuyruklarını kullanarak, iki veya daha fazla bölüm veya depolama sisteminde nihai tutarlılık sağlayan bir çözüm uygulayabilirsiniz.
Bu yaklaşımı göstermek için, eski çalışan varlıklarını arşivleme gereksiniminiz olduğunu varsayalım. Eski çalışan varlıkları nadiren sorgulanır ve mevcut çalışanlarla ilgili tüm etkinliklerin dışında tutulmalıdır. Bu gereksinimi uygulamak için, etkin çalışanları **Geçerli** tabloda ve eski çalışanları **Arşiv** tablosunda saklarsınız. Bir çalışanın arşivleme geçerli **tablodan** varlığı silmesini ve varlığı **Arşiv** tablosuna eklemenizi gerektirir, ancak bu iki işlemi gerçekleştirmek için bir EGT kullanamazsınız. Bir hatanın bir varlığın her iki tabloda veya her iki tabloda da görünmesine neden olma riskini önlemek için, arşiv işlemi sonunda tutarlı olmalıdır. Aşağıdaki sıralı diyagram, bu işlemdeki adımları özetleyin. Aşağıdaki metinde özel durum yolları için daha fazla ayrıntı sağlanır.  

![Azure kuyrukları çözümü](media/storage-table-design-guide/storage-table-design-IMAGE12.png)

İstemci, bu örnekte çalışan #456 arşivlemek için bir iletiyi Azure kuyruğuna yerleştirerek arşiv işlemini başlatır. Bir işçi rolü yeni iletiler için sırayı yoklar; bir tane bulduğunda, iletiyi okur ve kuyrukta gizli bir kopyasını bırakır. Alt rol sonraki **geçerli** tablodan varlığın bir kopyasını getirir, **Arşiv** tablosuna bir kopyasını ekler ve ardından **Özgün** tablodan siler. Son olarak, önceki adımlardan hata yoksa, alt rol gizli iletiyi kuyruktan siler.  

Bu örnekte, adım 4 çalışanı **Arşiv** tablosuna ekler. Bu Blob hizmeti veya bir dosya sisteminde bir dosya bir blob için çalışan ekleyebilirsiniz.  

### <a name="recovering-from-failures"></a>Hatalardan kurtarma
**4** ve **5.** *idempotent* Tablo hizmetini kullanıyorsanız, adım **4** için bir "ekle veya değiştir" işlemi kullanmalısınız; adım **5** için kullandığınız istemci kitaplığında bir "varsa sil" işlemini kullanmalısınız. Başka bir depolama sistemi kullanıyorsanız, uygun bir idempotent işlem kullanmanız gerekir.  

Alt rol adım **6'yı**asla tamamlarsa, bir zaman aladıktan sonra ileti, işçi rolünün yeniden işlemeye çalışması için hazır sırada yeniden görünür. İşçi rolü, kuyruktaki bir iletinin kaç kez okunduğunu denetleyebilir ve gerekirse ayrı bir kuyruğa göndererek soruşturma için bir "zehir" iletisi olduğunu işaretleyebilir. Sıra iletilerini okuma ve sıra silme sayısını denetleme hakkında daha fazla bilgi için [iletileri al](https://msdn.microsoft.com/library/azure/dd179474.aspx)bölümüne bakın.  

Tablo ve Sıra hizmetlerinden gelen bazı hatalar geçici hatalardır ve istemci uygulamanız bunları işlemek için uygun yeniden deneme mantığını içermelidir.  

### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Bu çözüm işlem yalıtımı sağlamaz. Örneğin, bir istemci, alt rol **4** ve **5**adımları arasındayken **Geçerli** ve **Arşiv** tablolarını okuyabilir ve verilerin tutarsız bir görünümünü görebilir. Veriler eninde sonunda tutarlı olacaktır.  
* Nihai tutarlılığı sağlamak için 4 ve 5 adımlarının iktidara olduğundan emin olmalısınız.  
* Birden çok kuyruk ve alt rol örnekleri kullanarak çözümü ölçeklendirebilirsiniz.  

### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Farklı bölümlerde veya tablolarda bulunan varlıklar arasında nihai tutarlılığı garanti etmek istediğinizde bu deseni kullanın. Tablo hizmeti ve Blob hizmeti ve veritabanı veya dosya sistemi gibi diğer Azure olmayan Depolama veri kaynakları genelinde işlemler için nihai tutarlılık sağlamak için bu deseni genişletebilirsiniz.  

### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* Varlık Grubu İşlemleri  
* [Birleştirme veya değiştirme](#merge-or-replace)  

> [!NOTE]
> İşlem yalıtımı çözümünüz için önemliyse, EGT'leri kullanabilmeniz için tablolarınızı yeniden tasarlamayı düşünmelisiniz.  
> 
> 

## <a name="index-entities-pattern"></a>Dizin varlıkları deseni
Varlıkların listelerini döndüren verimli aramaları etkinleştirmek için dizin varlıklarını koruyun.  

### <a name="context-and-problem"></a>Bağlam ve sorun
Tablo hizmeti, **PartitionKey** ve **RowKey** değerlerini kullanarak varlıkları otomatik olarak dizinler. Bu, istemci uygulamasının bir nokta sorgusu kullanarak etkin bir varlık almasına olanak tanır. Örneğin, aşağıda gösterilen tablo yapısını kullanarak, istemci uygulaması departman adını ve çalışan kimliğini **(PartitionKey** ve **RowKey)** kullanarak tek bir çalışan varlığını verimli bir şekilde alabilir.  

![Çalışan varlığı](media/storage-table-design-guide/storage-table-design-IMAGE13.png)

Ayrıca, soyadları gibi benzersiz olmayan başka bir özelliğin değerine dayalı olarak çalışan varlıklarının listesini almak istiyorsanız, bunları doğrudan aramak için bir dizin kullanmak yerine eşleşmeleri bulmak için daha az verimli bir bölüm taramayı kullanmanız gerekir. Bunun nedeni, tablo hizmetinin ikincil dizinler sağlamamasıdır.  

### <a name="solution"></a>Çözüm
Yukarıda gösterilen varlık yapısıyla soyadına göre aramayı etkinleştirmek için çalışan adlarının listelerini tutmanız gerekir. Jones gibi belirli bir soyadı olan çalışan varlıklarını almak istiyorsanız, önce Jones'un soyadı olan çalışanların çalışan kimliklerini bulmanız ve ardından bu çalışan varlıklarını almanız gerekir. Çalışan iT'lerinin listelerini depolamak için üç ana seçenek vardır:  

* Blob depolama kullanın.  
* Dizin varlıklarını çalışan varlıklarla aynı bölümde oluşturun.  
* Dizin varlıklarını ayrı bir bölüm veya tabloda oluşturun.  

<u>Seçenek #1: Blob depolama kullanın</u>  

İlk seçenek için, her benzersiz soyadı için bir blob oluşturursunuz ve her blob'da bu soyadı olan çalışanlar için **PartitionKey** (departman) ve **RowKey** (çalışan kimliği) değerlerinin bir listesini saklarsınız. Bir çalışan eklediğinizde veya sildiğinizde, ilgili blob içeriğinin sonunda çalışan varlıklarıyla tutarlı olduğundan emin olmalısınız.  

<u>Seçenek #2:</u> Aynı bölümdeki dizin varlıkları oluşturma  

İkinci seçenek için, aşağıdaki verileri depolayan dizin varlıklarını kullanın:  

![Çalışan diziliş varlığı](media/storage-table-design-guide/storage-table-design-IMAGE14.png)

**EmployeeIDs** **özelliği, RowKey'de**depolanan soyadıolan çalışanlar için çalışan ların listesini içerir.  

İkinci seçeneği kullanıyorsanız, yeni bir çalışan eklerken izlemeniz gereken işlemi aşağıdaki adımlar alısatır. Bu örnekte, Satış departmanına kimliği 000152 olan bir çalışan ve soyadı Jones ekliyoruz:  

1. **PartitionKey** değeri "Satışlar" ve **RowKey** değeri "Jones" olan dizin varlığını alın. Adım 2'de kullanmak üzere bu varlığın ETag kaydedin.  
2. Yeni çalışan varlığını **(PartitionKey** value "Sales" ve **RowKey** değeri "000152") ekleyen ve index varlığını **(PartitionKey** değeri "Satışlar" ve **Satır Anahtar** değeri "Jones") çalışanlar alanında listeye yeni çalışan kimliğini ekleyerek güncelleştiren bir varlık grubu hareketi (yani toplu işlem) oluşturun. Varlık grubu hareketleri hakkında daha fazla bilgi için Entity Group Hareketleri'ne bakın.  
3. Varlık grubu hareketi iyimser bir eşzamanlılık hatası nedeniyle başarısız olursa (başka biri dizin varlığını yeni değiştirmişse), 1.  

İkinci seçeneği kullanıyorsanız, bir çalışanı silerken benzer bir yaklaşım kullanabilirsiniz. Çalışan varlık, eski soyadı için dizin varlığı ve yeni soyadı için dizin varlığı: üç varlık güncelleştirmeleri bir varlık grubu hareketi yürütmek gerekir, çünkü bir çalışanın soyadını değiştirme biraz daha karmaşıktır. Daha sonra iyimser eşzamanlılık kullanarak güncelleştirmeleri gerçekleştirmek için kullanabileceğiniz ETag değerlerini almak için herhangi bir değişiklik yapmadan önce her varlığı almak gerekir.  

Aşağıdaki adımlar, ikinci seçeneği kullanıyorsanız, bir departmanda belirli bir soyadı olan tüm çalışanları aramanız gerektiğinde izlemeniz gereken işlemi ana hatlar. Bu örnekte, Satış departmanında soyadı Jones olan tüm çalışanları arıyoruz:  

1. **PartitionKey** değeri "Satışlar" ve **RowKey** değeri "Jones" olan dizin varlığını alın.  
2. Çalışanlar alanında çalışan kimlikleri listesini ayrıştırın.  
3. Bu çalışanların her biri hakkında ek bilgilere (e-posta adresleri gibi) ihtiyacınız varsa, bölüm 2'de elde ettiğiniz çalışanlar listesinden **PartitionKey** değeri "Satışlar" ve **RowKey** değerlerini kullanarak çalışan varlıklarının her birini alın.  

<u>Seçenek #3:</u> Ayrı bir bölüm veya tabloda dizin varlıkları oluşturma  

Üçüncü seçenek için, aşağıdaki verileri depolayan dizin varlıklarını kullanın:  

![Ayrı bir bölümdeki çalışan dizin varlığı](media/storage-table-design-guide/storage-table-design-IMAGE15.png)


**EmployeeIDs** **özelliği, RowKey'de**depolanan soyadıolan çalışanlar için çalışan ların listesini içerir.  

Üçüncü seçenekte, dizin varlıkları çalışan varlıklarından ayrı bir bölüm de olduğundan tutarlılığı korumak için EGT'leri kullanamazsınız. Dizin varlıklarının sonunda çalışan varlıklarla tutarlı olduğundan emin olun.  

### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Bu çözüm, eşleşen varlıkları almak için en az iki sorgu gerektirir: biri **Satır Anahtar** değerleri listesini almak için dizin varlıklarını sorgulamak ve ardından listedeki her varlığı almak için sorgular.  
* Tek bir varlığın maksimum 1 MB boyutuna sahip olduğu göz önüne alındığında, çözümdeki seçenek #2 ve seçenek #3, belirli bir soyadıiçin çalışan adlarının listesinin hiçbir zaman 1 MB'dan büyük olmadığını varsayalım. Çalışan iAd'leri listesinin boyutu 1 MB'dan büyükse, #1 seçeneğini kullanın ve dizin verilerini blob depolamada saklayın.  
* #2 seçeneğini kullanıyorsanız (çalışan ekleme ve silme işlemlerini işlemek için EGT'leri kullanıyorsanız ve bir çalışanın soyadını değiştirmek) hareket hacminin belirli bir bölümdeki ölçeklenebilirlik sınırlarına yaklaşıp yaklaşacağını değerlendirmeniz gerekir. Bu durumda, güncelleştirme isteklerini işlemek için kuyrukları kullanan ve dizin varlıklarınızı çalışan varlıklarından ayrı bir bölümde depolamanızı sağlayan, sonunda tutarlı bir çözüm (seçenek #1 veya seçenek #3) düşünmelisiniz.  
* Bu çözümdeki #2 seçeneği, bir departmandaki soyadınızı aramak istediğinizi varsayar: örneğin, Satış departmanında Jones soyadı olan çalışanların listesini almak istiyorsunuz. Tüm kuruluş genelinde Jones soyadıolan tüm çalışanları aramak istiyorsanız, #1 seçeneğini veya #3 seçeneğini kullanın.
* Nihai tutarlılık sağlayan sıra tabanlı bir çözüm uygulayabilirsiniz (daha fazla ayrıntı için [Eventually tutarlı hareketler desenine](#eventually-consistent-transactions-pattern) bakın).  

### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Jones soyadına sahip tüm çalışanlar gibi ortak bir özellik değerini paylaşan bir varlık kümesini aramak istediğinizde bu deseni kullanın.  

### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Bileşik anahtar deseni](#compound-key-pattern)  
* [Sonunda tutarlı hareketler deseni](#eventually-consistent-transactions-pattern)  
* Varlık Grubu İşlemleri  
* [Heterojen varlık tipleri ile çalışma](#working-with-heterogeneous-entity-types)  

## <a name="denormalization-pattern"></a>Denormalizasyon deseni
Tek bir nokta sorgusuyla ihtiyacınız olan tüm verileri alabilmeniz için ilgili verileri tek bir varlıkta birleştirin.  

### <a name="context-and-problem"></a>Bağlam ve sorun
İlişkisel bir veritabanında, yinelemeyi kaldırmak için verileri normalleştirirsiniz ve bu da birden çok tablodan veri alan sorgularla sonuçlanır. Verilerinizi Azure tablolarında normalleştirirseniz, ilgili verilerinizi almak için istemciden sunucuya birden çok gidiş dönüş seyahati yapmalısınız. Örneğin, aşağıda gösterilen tablo yapısıyla, bir bölümün ayrıntılarını almak için iki gidiş-dönüş geziye ihtiyacınız vardır: biri yöneticinin kimliğini içeren departman varlığını almak ve ardından yöneticinin ayrıntılarını çalışan bir varlıkta almak için başka bir istek.  

![Bölüm tüzel kişiliği ve Çalışan tüzel kişisi](media/storage-table-design-guide/storage-table-design-IMAGE16.png)

### <a name="solution"></a>Çözüm
Verileri iki ayrı varlıkta depolamak yerine, verileri normalden arındırın ve yöneticinin ayrıntılarının bir kopyasını departman varlığında tutun. Örnek:  

![Bölüm tüzel kişiliği](media/storage-table-design-guide/storage-table-design-IMAGE17.png)

Bu özelliklerle depolanan departman varlıklarıyla, artık nokta sorgusunu kullanarak bir departman la ilgili ihtiyacınız olan tüm ayrıntıları alabilirsiniz.  

### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Bazı verilerin iki kez depolanmasıyla ilişkili bazı maliyet yükü vardır. Performans avantajı (depolama hizmetine daha az istekten kaynaklanan) genellikle depolama maliyetlerindeki marjinal artıştan daha ağır basar (ve bu maliyet kısmen bir departmanın ayrıntılarını almak için gereken işlem sayısındaki azalmayla dengelenir ).  
* Yöneticiler hakkında bilgi depolayan iki varlığın tutarlılığını korumanız gerekir. Tek bir atomik işlemde birden çok varlığı güncelleştirmek için EGT'leri kullanarak tutarlılık sorununu işleyebilirsiniz: bu durumda, departman varlığı ve departman yöneticisinin çalışan varlığı aynı bölümde depolanır.  

### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
İlgili bilgileri sık sık aramanız gerektiğinde bu deseni kullanın. Bu desen, istemcinizin gerektirdiği verileri almak için yapması gereken sorgu sayısını azaltır.  

### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Bileşik anahtar deseni](#compound-key-pattern)  
* Varlık Grubu İşlemleri  
* [Heterojen varlık tipleri ile çalışma](#working-with-heterogeneous-entity-types)

## <a name="compound-key-pattern"></a>Bileşik anahtar deseni
İstemcinin ilgili verileri tek bir nokta sorgusuyla aramasını sağlamak için bileşik **RowKey** değerlerini kullanın.  

### <a name="context-and-problem"></a>Bağlam ve sorun
İlişkisel bir veritabanında, ilgili veri parçalarını istemciye tek bir sorguda döndürmek için sorgularda birleştirmeler kullanmak doğaldır. Örneğin, çalışan kimliğini, söz sahibi çalışanın performans ve inceleme verilerini içeren ilgili varlıkların listesini aramak için kullanabilirsiniz.  

Aşağıdaki yapıyı kullanarak Tablo hizmetinde çalışan varlıkları depoladığınızı varsayalım:  

![Çalışan varlık yapısı](media/storage-table-design-guide/storage-table-design-IMAGE18.png)

Ayrıca, çalışanın kuruluşunuz için çalıştığı her yıl için incelemeler ve performansla ilgili geçmiş verileri depolamanız ve bu bilgilere her yıl erişebilmeniz gerekir. Seçeneklerden biri, varlıkları aşağıdaki yapıya sahip depolayan başka bir tablo oluşturmaktır:  

![Alternatif çalışan varlık yapısı](media/storage-table-design-guide/storage-table-design-IMAGE19.png)

Bu yaklaşımla, verilerinizi tek bir istekle almanıza olanak sağlamak için yeni varlıktaki bazı bilgileri (ad ve soyadı gibi) çoğaltmaya karar verebilirsiniz. Ancak, iki varlığı atomik olarak güncelleştirmek için bir EGT kullanamadığınız için güçlü tutarlılık sağlayamazsınız.  

### <a name="solution"></a>Çözüm
Aşağıdaki yapıya sahip varlıkları kullanarak özgün tablonuzda yeni bir varlık türünü depolayın:  

![Çalışan varlık yapısı için çözüm](media/storage-table-design-guide/storage-table-design-IMAGE20.png)

**RowKey'nin** artık çalışan kimliğinden ve çalışanın performansını almanızı ve tek bir varlık için tek bir istekle verileri gözden geçirmenize olanak tanıyan inceleme verilerinden oluşan bileşik anahtarına dikkat edin.  

Aşağıdaki örnekte, belirli bir çalışanın (Satış departmanındaki çalışan 000123 gibi) tüm gözden geçirme verilerini nasıl alabileceğiniz özetlenebilir:  

$filter=(PartitionKey eq 'Satış') ve (RowKey ge 'empid_000123') ve (RowKey lt 'empid_000124')&$select=RowKey,Yönetici Derecelendirmesi,Eş Değerlendirme,Yorumlar  

### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* **RowKey** değerini ayrıştırmayı kolaylaştıran uygun bir ayırıcı karakter kullanmalısınız: örneğin, **000123_2012.**  
* Ayrıca, bu varlığı aynı çalışan için ilgili verileri içeren diğer varlıklarla aynı bölümde de depolayasınız, bu da güçlü tutarlılık sağlamak için EGT'leri kullanabileceğiniz anlamına gelir.
* Bu desenin uygun olup olmadığını belirlemek için verileri ne sıklıkta sorgulayacağınızı düşünmelisiniz.  Örneğin, inceleme verilerine seyrek olarak erişecekseniz ve ana çalışan verilerine genellikle bunları ayrı varlıklar olarak tutmanız gerekir.  

### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Sık sorguladığınız bir veya daha fazla ilgili varlığı depolamanız gerektiğinde bu deseni kullanın.  

### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* Varlık Grubu İşlemleri  
* [Heterojen varlık tipleri ile çalışma](#working-with-heterogeneous-entity-types)  
* [Sonunda tutarlı hareketler deseni](#eventually-consistent-transactions-pattern)  

## <a name="log-tail-pattern"></a>Kütük kuyruk deseni
En son bir bölüme eklenen *n* varlıklarını, ters tarih ve saat sırasına göre sıralayan bir **RowKey** değeri kullanarak alın.  

### <a name="context-and-problem"></a>Bağlam ve sorun
Ortak bir gereksinim, örneğin bir çalışan tarafından gönderilen en son 10 gider talebi gibi en son oluşturulan varlıkları geri alabilir. Tablo sorguları, ilk *n* varlıklarını bir kümeden döndürmek için **$top** sorgu işlemini destekler: bir kümedeki son n varlıklarını döndürmek için eşdeğer bir sorgu işlemi yoktur.  

### <a name="solution"></a>Çözüm
Varlıkları, en son girişin her zaman tablodaki ilk giriş olması için kullanarak ters tarih/saat sırasına doğal olarak sıralayan bir **RowKey** kullanarak depolayın.  

Örneğin, bir çalışan tarafından gönderilen en son 10 gider talebinin alınabilmesi için geçerli tarihten/saatten türetilen bir ters onay değeri kullanabilirsiniz. Aşağıdaki C# kodu örneği, **RowKey** için en sondan en eskiye doğru sıralayan uygun bir "ters kene" değeri oluşturmanın bir yolunu gösterir:  

`string invertedTicks = string.Format("{0:D19}", DateTime.MaxValue.Ticks - DateTime.UtcNow.Ticks);`  

Aşağıdaki kodu kullanarak tarih saati değerine geri dönebilirsiniz:  

`DateTime dt = new DateTime(DateTime.MaxValue.Ticks - Int64.Parse(invertedTicks));`  

Tablo sorgusu aşağıdaki gibi görünür:  

`https://myaccount.table.core.windows.net/EmployeeExpense(PartitionKey='empid')?$top=10`  

### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Dize değeri sıralamalarını beklendiği gibi sağlamak için, satır aralığı sıfırlarıyla ters kene değerini pad gerekir.  
* Bir bölüm düzeyinde ölçeklenebilirlik hedefleri farkında olmalıdır. Sıcak nokta bölümleri oluşturmamaya dikkat edin.  

### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Bu deseni, ters tarih/saat sırasına göre varlıklara erişmeniz gerektiğinde veya en son eklenen varlıklara erişmeniz gerektiğinde kullanın.  

### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Prepend / ek anti-desen](#prepend-append-anti-pattern)  
* [Varlıkların alınması](#retrieving-entities)  

## <a name="high-volume-delete-pattern"></a>Yüksek hacimli silme deseni
Tüm varlıkları kendi ayrı tablolarında eşzamanlı silme için depolayarak yüksek hacimli varlıkların silinmesini etkinleştirin; tabloyu silerek varlıkları silersiniz.  

### <a name="context-and-problem"></a>Bağlam ve sorun
Birçok uygulama, istemci uygulaması için artık kullanılabilir olması gerekmeyen veya uygulamanın başka bir depolama ortamına arşivlenmiş eski verileri siler. Genellikle bu tür verileri bir tarihe göre tanımlarsınız: örneğin, 60 günden eski olan tüm oturum açma isteklerinin kayıtlarını silme niz gerekir.  

Olası bir tasarım **RowKey**giriş isteği tarih ve saat kullanmaktır:  

![Oturum açma girişiminin tarihi ve saati](media/storage-table-design-guide/storage-table-design-IMAGE21.png)

Uygulama ayrı bir bölüme her kullanıcı için giriş varlıkları ekleyebilir ve silmek, bu yaklaşım bölüm etkin noktaları önler. Ancak, bu yaklaşım, çok sayıda varlığınız varsa maliyetli ve zaman alıcı olabilir, çünkü önce silecek tüm varlıkları tanımlamak için bir tablo tarası gerçekleştirmeniz ve ardından her eski varlığı silmeniz gerekir. EGT'lere birden çok silme isteği ekleyerek eski varlıkları silmek için gereken sunucuya yapılan gidiş dönüş sayısını azaltabilirsiniz.  

### <a name="solution"></a>Çözüm
Giriş denemelerinin her günü için ayrı bir tablo kullanın. Varlıkları eklerken etkin noktalardan kaçınmak için yukarıdaki varlık tasarımını kullanabilirsiniz ve eski varlıkları silerken artık yüzlerce ve binlerce tek tek bulmak ve salmak yerine her gün bir tabloyu (tek bir depolama işlemi) silme sorunu her gün giriş varlıklar.  

### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Tasarımınız, uygulamanızın belirli varlıkları arama, diğer verilerle bağlantı kurma veya toplu bilgi oluşturma gibi verileri kullanma nın diğer yollarını destekliyor mu?  
* Yeni varlıklar eklerken tasarımınız etkin noktalardan kaçınıyor mu?  
* Silen sonra aynı tablo adını yeniden kullanmak istiyorsanız bir gecikme bekleyin. Her zaman benzersiz tablo adları kullanmak daha iyidir.  
* Tablo hizmeti erişim desenlerini öğrenirken ve bölümleri düğümlere dağıtırken yeni bir tabloyu ilk kullandığınızda biraz azaltma bekleyin. Yeni tablolar oluşturmanız için ne sıklıkta yapmanız gerektiğini göz önünde bulundurmalısınız.  

### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Aynı anda silmeniz gereken yüksek hacimli varlıklar varsa bu deseni kullanın.  

### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* Varlık Grubu İşlemleri
* [Varlıkları değiştirme](#modifying-entities)  

## <a name="data-series-pattern"></a>Veri serisi deseni
Yaptığınız istek sayısını en aza indirmek için tüm veri serilerini tek bir varlıkta depolayın.  

### <a name="context-and-problem"></a>Bağlam ve sorun
Yaygın bir senaryo, bir uygulamanın genellikle aynı anda alınması gereken bir dizi veriyi depolamasıdır. Örneğin, uygulamanız her çalışanın her saat kaç IM iletisi gönderdiğini kaydedebilir ve daha sonra bu bilgileri, her kullanıcının önceki 24 saat içinde kaç ileti gönderdiğini çizmek için kullanabilir. Bir tasarım her çalışan için 24 varlıkları depolamak için olabilir:  

![Her çalışan için 24 varlık depolama](media/storage-table-design-guide/storage-table-design-IMAGE22.png)

Bu tasarımla, uygulamanın ileti sayısı değerini güncelleştirmesi gerektiğinde varlığı her çalışan için güncelleştirmek üzere kolayca bulabilir ve güncelleştirebilirsiniz. Ancak, önceki 24 saat için etkinliğin bir grafik çizmek için bilgi almak için, 24 varlıklar almanız gerekir.  

### <a name="solution"></a>Çözüm
İleti sayısını her saat için depolamak için aşağıdaki tasarımı ayrı bir özellik ile kullanın:  

![İleti istatistikleri varlık](media/storage-table-design-guide/storage-table-design-IMAGE23.png)

Bu tasarımla, bir çalışanın ileti sayısını belirli bir saat için güncelleştirmek için birleştirme işlemini kullanabilirsiniz. Şimdi, tek bir varlık için bir istek kullanarak grafiği çizmek için gereken tüm bilgileri alabilirsiniz.  

### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Tam veri seriniz tek bir varlığa sığmazsa (bir varlığın en fazla 252 özelliği olabilir), blob gibi alternatif bir veri deposu kullanın.  
* Bir varlığı aynı anda güncelleyen birden çok istemciniz varsa, iyimser eşzamanlılık uygulamak için **ETag'ı** kullanmanız gerekir. Eğer çok sayıda müşteriniz varsa, yüksek çekişme yaşayabilirsiniz.  

### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Tek bir varlıkla ilişkili bir veri serisini güncelleştirmeniz ve almanız gerektiğinde bu deseni kullanın.  

### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Büyük varlıklar deseni](#large-entities-pattern)  
* [Birleştirme veya değiştirme](#merge-or-replace)  
* [Sonunda tutarlı işlemler deseni](#eventually-consistent-transactions-pattern) (veri serisini bir blob'da depolıyorsanız)  

## <a name="wide-entities-pattern"></a>Geniş varlıklar deseni
252'den fazla özelliyi olan mantıksal varlıkları depolamak için birden çok fiziksel varlık kullanın.  

### <a name="context-and-problem"></a>Bağlam ve sorun
Tek bir varlık en fazla 252 özelliye (zorunlu sistem özellikleri hariç) sahip olabilir ve toplamda 1 MB'dan fazla veri depolayamaz. İlişkisel bir veritabanında, genellikle yeni bir tablo ekleyerek ve aralarında bire bir ilişki uygulayarak bir satırBoyutu üzerinde herhangi bir sınır yuvarlak alırsınız.  

### <a name="solution"></a>Çözüm
Tablo hizmetini kullanarak, 252'den fazla özelliği olan tek bir büyük iş nesnesini temsil edecek şekilde birden çok varlığı depolayabilirsiniz. Örneğin, son 365 gün içinde her çalışan tarafından gönderilen IM iletilerinin sayısını depolamak istiyorsanız, farklı şemalara sahip iki varlığı kullanan aşağıdaki tasarımı kullanabilirsiniz:  

![Birden çok varlık](media/storage-table-design-guide/storage-table-design-IMAGE24.png)

Birbirleri ile eşitleme lerini sağlamak için her iki varlığı da güncelleştirmeyi gerektiren bir değişiklik yapmanız gerekiyorsa, EGT kullanabilirsiniz. Aksi takdirde, ileti sayısını belirli bir gün için güncelleştirmek için tek bir birleştirme işlemi kullanabilirsiniz. Tek bir çalışanın tüm verilerini almak için, hem **PartitionKey** hem de **RowKey** değerini kullanan iki verimli istekle yapabileceğiniz her iki varlığı da almanız gerekir.  

### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Tam bir mantıksal varlık alma en az iki depolama hareketleri içerir: her fiziksel varlık almak için bir.  

### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Tablo hizmetinde tek bir varlığın sınırlarını aşan özellikleri veya sayısı olan varlıkları depolamak gerektiğinde bu deseni kullanın.  

### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* Varlık Grubu İşlemleri
* [Birleştirme veya değiştirme](#merge-or-replace)

## <a name="large-entities-pattern"></a>Büyük varlıklar deseni
Büyük özellik değerlerini depolamak için blob depolamasını kullanın.  

### <a name="context-and-problem"></a>Bağlam ve sorun
Tek bir varlık toplamda 1 MB'dan fazla veri depolayamaz. Özelliklerinizden biri veya birkaçı, varlığınızın toplam boyutunun bu değeri aşması için değer depolarsa, tüm varlığı Tablo hizmetinde saklayamazsınız.  

### <a name="solution"></a>Çözüm
Bir veya daha fazla özellik büyük miktarda veri içerdiğinden, varlığınız boyutu 1 MB'ı aşarsa, verileri Blob hizmetinde depolayabilir ve ardından blob adresini varlıktaki bir özellikte depolayabilirsiniz. Örneğin, bir çalışanın fotoğrafını blob depolama alanında saklayabilir ve çalışan varlığınızın **Fotoğraf** özelliğinde fotoğrafın bağlantısını depolayabilirsiniz:  

![Fotoğraf özelliği](media/storage-table-design-guide/storage-table-design-IMAGE25.png)

### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Tablo hizmetindeki varlık la Blob hizmetindeki veriler arasında nihai tutarlılığı korumak için, varlıklarınızı korumak için [Son olarak tutarlı hareketler modelini](#eventually-consistent-transactions-pattern) kullanın.
* Tam bir varlığın alınması en az iki depolama işlemi içerir: biri varlığı almak için, diğeri blob verilerini almak için.  

### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Boyutu Tablo hizmetinde tek bir varlığın sınırlarını aşan varlıkları depolamanız gerektiğinde bu deseni kullanın.  

### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Sonunda tutarlı hareketler deseni](#eventually-consistent-transactions-pattern)  
* [Geniş varlıklar deseni](#wide-entities-pattern)

<a name="prepend-append-anti-pattern"></a>

## <a name="prependappend-anti-pattern"></a>Prepend/ek anti-desen
Eklerin birden çok bölüme yayılmasını sağlayarak yüksek hacimli kesici uçlara sahip olduğunuzda ölçeklenebilirliği artırın.  

### <a name="context-and-problem"></a>Bağlam ve sorun
Depolanan varlıklarınıza ön bekleyen veya eklenen varlıklar genellikle uygulamanın bir dizi bölümün ilk veya son bölümüne yeni varlıklar eklemesine neden olur. Bu durumda, belirli bir zamanda ki eklerin tümü aynı bölümde yer alıyor, tablo hizmetinin birden çok düğüm üzerinde uçları dengelemesini engelleyen bir etkin nokta oluşturuyor ve uygulamanızın ölçeklenebilirlik hedeflerini vurmasına neden olabilir Bölüm. Örneğin, çalışanlar tarafından ağ ve kaynak erişimini günlüğe kaydeden bir uygulamanız varsa, aşağıda gösterildiği gibi bir varlık yapısı, hareketlerin hacmi bir uygulamanın ölçeklenebilirlik hedefine ulaşırsa geçerli saatin bölünmesinin etkin nokta haline gelmesine neden olabilir. bireysel bölüm:  

![Varlık yapısı](media/storage-table-design-guide/storage-table-design-IMAGE26.png)

### <a name="solution"></a>Çözüm
Aşağıdaki alternatif varlık yapısı, uygulama olayları günlüğe kaydederken belirli bir bölüm üzerinde etkin bir nokta önler:  

![Alternatif varlık yapısı](media/storage-table-design-guide/storage-table-design-IMAGE27.png)

Bu örnekte, PartitionKey ve **RowKey'in** bileşik anahtarları olduğuna dikkat edin. **RowKey** **PartitionKey,** günlüğe kaydetmeyi birden çok bölüme dağıtmak için hem departman hem de çalışan kimliğini kullanır.  

### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Eklerde sıcak bölümler oluşturmayı önleyen alternatif anahtar yapısı istemci uygulamanızın yaptığı sorguları verimli bir şekilde destekliyor mu?  
* Beklenen işlem hacminiz, tek bir bölüm için ölçeklenebilirlik hedeflerine ulaşma ve depolama hizmeti tarafından daraltılmış olma olasılığınız anlamına mı geliyor?  

### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Sıcak bir bölüme erişdiğinizde, işlem hacminiz depolama hizmeti tarafından daralmaya neden olacaksa, hazırlık/ek anti-desenkaçının.  

### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Bileşik anahtar deseni](#compound-key-pattern)  
* [Kütük kuyruk deseni](#log-tail-pattern)  
* [Varlıkları değiştirme](#modifying-entities)  

## <a name="log-data-anti-pattern"></a>Günlük veri anti-desen
Genellikle, günlük verilerini depolamak için Tablo hizmeti yerine Blob hizmetini kullanmanız gerekir.  

### <a name="context-and-problem"></a>Bağlam ve sorun
Günlük verileri için yaygın bir kullanım örneği, belirli bir tarih/saat aralığı için günlük girişleri seçimini almaktır: örneğin, uygulamanızın belirli bir tarihte 15:04 ile 15:06 arasında günlüğe kaydettiği tüm hataları ve kritik iletileri bulmak istersiniz. Günlük varlıklarını kaydettiğiniz bölümü belirlemek için günlük iletisinin tarih ve saatini kullanmak istemezsiniz: bu, herhangi bir zamanda tüm günlük varlıkları aynı **PartitionKey** değerini paylaşacağı için sıcak bir bölümle sonuçlanır (bkz. bölüm [Prepend/ek anti-desen).](#prepend-append-anti-pattern) Örneğin, günlük iletisi için aşağıdaki varlık şeması, uygulama tüm günlük iletilerini geçerli tarih ve saat için bölüme yazdığından, sıcak bir bölümle sonuçlanır:  

![Günlük ileti varlığı](media/storage-table-design-guide/storage-table-design-IMAGE28.png)

Bu örnekte **RowKey,** günlük iletilerinin tarih/saat sırasına göre sıralandığından emin olmak için günlük iletisinin tarih ve saatini içerir ve birden çok günlük iletisinin aynı tarih ve saati paylaşması durumunda bir ileti kimliği içerir.  

Başka bir yaklaşım, uygulamanın çeşitli bölümler arasında ileti ler yazmasını sağlayan bir **PartitionKey** kullanmaktır. Örneğin, günlük iletisinin kaynağı birçok bölüm arasında iletidağıtmak için bir yol sağlıyorsa, aşağıdaki varlık şemasını kullanabilirsiniz:  

![Alternatif günlük ileti varlığı](media/storage-table-design-guide/storage-table-design-IMAGE29.png)

Ancak, bu şema ile ilgili sorun, belirli bir zaman aralığı için tüm günlük iletileri almak için tablodaki her bölümü aramak gerekir.

### <a name="solution"></a>Çözüm
Önceki bölümde, günlük girişlerini depolamak için Tablo hizmetini kullanmaya çalışma sorunu vurgulanmış ve iki, tatmin edici olmayan tasarımlar önerilmiştir. Bir çözüm kötü performans günlük mesajları yazma riski ile sıcak bir bölümyol açtı; diğer çözüm, belirli bir zaman aralığı için günlük iletileri almak için tablodaki her bölümü tarama gereksinimi nedeniyle kötü sorgu performansı ile sonuçlandı. Blob depolama, bu tür senaryolar için daha iyi bir çözüm sunar ve Azure Depolama Analitiği topladığı günlük verilerini bu şekilde depolar.  

Bu bölümde, Depolama Analitiği'nin genellikle aralıklara göre sorguladığınız verileri depolamaya ilişkin bu yaklaşımın bir örneği olarak verileri blob depolamaalanında nasıl depoladığını özetler.  

Depolama Analizi, günlük iletilerini birden çok blob'da sınırlı bir biçimde saklar. Sınırlı biçim, istemci uygulamasının günlük iletisindeki verileri ayrışdırmasını kolaylaştırır.  

Depolama Analizi, arama yaptığınız günlük iletilerini içeren blob 'u (veya blobs)'ı bulmanızı sağlayan blob'lar için bir adlandırma kuralı kullanır. Örneğin, "queue/2014/07/31/1800/000001.log" adlı bir blob, 31 Temmuz 2014 tarihinde saat 18:00'den itibaren sıra hizmetiyle ilgili günlük iletileri içerir. "000001" bu dönemin ilk günlük dosyası olduğunu gösterir. Depolama Analizi ayrıca, blob'un meta verilerinin bir parçası olarak dosyada depolanan ilk ve son günlük iletilerinin zaman damgalarını da kaydeder. Blob depolama için API bir ad öneki dayalı bir kapsayıcıda lekeler bulmanızı sağlar: saat 18:00'den itibaren saat için sıra günlüğü verileri içeren tüm lekeleri bulmak için" önekini kullanabilirsiniz "queue/2014/07/31/1800."  

Depolama Analitiği arabellekleri iletileri dahili olarak günlüğe kaydeder ve ardından uygun blob'u düzenli aralıklarla güncelleştirir veya en son günlük girişleri yle yeni bir tane oluşturur. Bu, blob hizmetine gerçekleştirmesi gereken yazma sayısını azaltır.  

Kendi uygulamanızda da benzer bir çözüm uyguluyorsanız, güvenilirlik (blob depolamasına her günlük girişini olduğu gibi yazma) ve maliyet ve ölçeklenebilirlik (uygulamanızdaki ve yazınızdaki güncelleştirmeleri arabelleğe alma) arasındaki dengeyi nasıl yönetacağınızı göz önünde bulundurmalısınız. toplu olarak depolama blob için).  

### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Günlük verilerinin nasıl depolanmasına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Olası sıcak bölümleri önleyen bir tablo tasarımı oluşturursanız, günlük verilerinize verimli bir şekilde erişemediğinizi görebilirsiniz.  
* Günlük verilerini işlemek için istemcinin genellikle çok sayıda kayıt yüklemesi gerekir.  
* Günlük verileri genellikle yapılandırılmış olmasına rağmen, blob depolama daha iyi bir çözüm olabilir.  

## <a name="implementation-considerations"></a>Uygulama konuları
Bu bölümde, önceki bölümlerde açıklanan desenleri uygularken göz önünde bulundurulması gereken bazı hususlar açıklanmaktadır. Bu bölümün çoğu, Depolama İstemci Kitaplığı'nı kullanan C# ile yazılmış örnekleri kullanır (yazım sırasında sürüm 4.3.0).  

## <a name="retrieving-entities"></a>Varlıkların alınması
Sorgulama için Tasarım bölümünde anlatıldığı gibi, en verimli sorgu bir nokta sorgusudur. Ancak, bazı senaryolarda birden çok varlık almanız gerekebilir. Bu bölümde, Depolama İstemci Kitaplığı'nı kullanarak varlıkları almak için bazı yaygın yaklaşımlar açıklanmaktadır.  

### <a name="executing-a-point-query-using-the-storage-client-library"></a>Depolama İstemci Kitaplığı'nı kullanarak nokta sorgusu yürütme
Bir nokta sorgusunu yürütmenin en kolay yolu, "Satışlar" değerinde **Bir PartitionKey** değeri ne var bir varlık ve "212" değerinin **RowKey'i** olan bir varlığı alan aşağıdaki C# kodu snippet'inde gösterildiği gibi Tablo Yufkadı'nı kullanmaktır: **Retrieve**  

```csharp
TableOperation retrieveOperation = TableOperation.Retrieve<EmployeeEntity>("Sales", "212");
var retrieveResult = employeeTable.Execute(retrieveOperation);
if (retrieveResult.Result != null)
{
    EmployeeEntity employee = (EmployeeEntity)retrieveResult.Result;
    ...
}  
```

Bu örnek, alıyan varlığın **EmployeeEntity**türünden olmasını nasıl beklediğine dikkat edin.  

### <a name="retrieving-multiple-entities-using-linq"></a>LINQ kullanarak birden çok varlığı alma
Microsoft Azure Cosmos Tablo Standart Kitaplığı ile çalışırken Tablo hizmetinden birden çok varlık almak için LINQ'yi kullanabilirsiniz. 

```azurecli
dotnet add package Microsoft.Azure.Cosmos.Table
```

Aşağıdaki örneklerin çalışması için ad boşluklarını eklemeniz gerekir:

```csharp
using System.Linq;
using Microsoft.Azure.Cosmos.Table;
using Microsoft.Azure.Cosmos.Table.Queryable;
```

EmployeeTable, TableQuery\<\<ITableEntity> döndüren createquery itableentity>() yöntemini uygulayan bir CloudTable nesnesidir. Bu tür nesneler bir IQueryable uygular ve hem LINQ Sorgu İfadeleri ve nokta gösterimsözdizimini kullanarak izin verir.

Birden çok varlığı alma ve bir **where** yan tümcesi ile bir sorgu belirterek elde edilebilir. Tablo taramasından kaçınmak için, tablo ve bölüm taramalarından kaçınmak için her zaman **BölümKey** değerini where yan tümcesine ve mümkünse **RowKey** değerini eklemeniz gerekir. Tablo hizmeti, where yan tümcesinde kullanmak üzere sınırlı bir karşılaştırma işleci kümesini (büyük, büyük veya eşit, daha az, daha az veya eşit, eşit ve eşit olmayan) destekler. 

Aşağıdaki C# kodu snippet, soyadı "B" ile başlayan **(RowKey'nin** soyadını depoladığını varsayarsak) satış departmanında **(PartitionKey'nin** departman adını depoladığını varsayarak) tüm çalışanları bulur:  

```csharp
TableQuery<EmployeeEntity> employeeQuery = employeeTable.CreateQuery<EmployeeEntity>();
var query = (from employee in employeeQuery
            where employee.PartitionKey == "Sales" &&
            employee.RowKey.CompareTo("B") >= 0 &&
            employee.RowKey.CompareTo("C") < 0
            select employee).AsTableQuery();
            
var employees = query.Execute();  
```

Sorgunun daha iyi performans sağlamak için hem **RowKey** hem de **PartitionKey'i** nasıl belirlediğine dikkat edin.  

Aşağıdaki kod örneği LINQ sözdizimini kullanmadan eşdeğer işlevselliği gösterir:  

```csharp
TableQuery<EmployeeEntity> employeeQuery = 
    new TableQuery<EmployeeEntity>().Where(
        TableQuery.CombineFilters(
            TableQuery.CombineFilters(
                TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Sales"),
                TableOperators.And,
                TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThanOrEqual, "B")),
            TableOperators.And,
            TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "C")));
            
var employees = employeeTable.ExecuteQuery(employeeQuery);  
```

> [!NOTE]
> Örnek, üç filtre koşullarını içerecek şekilde birden çok **CombineFilters** yöntemini birleştirir.  
> 
> 

### <a name="retrieving-large-numbers-of-entities-from-a-query"></a>Bir sorgudan çok sayıda varlık alma
En iyi sorgu, **PartitionKey** değeri ve **RowKey** değerini temel alan tek bir varlığı döndürür. Ancak, bazı senaryolarda aynı bölümden veya hatta birçok bölümden birçok varlığı döndürme gereksiniminiz olabilir.  

Bu tür senaryolarda uygulamanızın performansını her zaman tam olarak test etmelidir.  

Tablo hizmetine karşı bir sorgu aynı anda en fazla 1.000 varlık döndürebilir ve en fazla beş saniye süreyle yürütülebilir. Sonuç kümesi 1.000'den fazla varlık içeriyorsa, sorgu beş saniye içinde tamamlanmadıysa veya sorgu bölüm sınırını geçiyorsa, Istemci uygulamasının bir sonraki varlık kümesini istemesini sağlamak için Tablo hizmeti bir devam belirteci döndürür. Devam belirteçlerinin nasıl çalıştığı hakkında daha fazla bilgi için [Sorgu Zaman Aşım ve Pagination'a](https://msdn.microsoft.com/library/azure/dd135718.aspx)bakın.  

Depolama İstemci Kitaplığı kullanıyorsanız, Tablo hizmetinden varlıkları döndürür gibi sizin için devam belirteçleri otomatik olarak işleyebilir. Depolama İstemci Kitaplığı'nı kullanan aşağıdaki C# kod örneği, tablo hizmeti yanıt olarak iade ederse, devam belirteçlerini otomatik olarak işler:  

```csharp
string filter = TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<EmployeeEntity> employeeQuery = new TableQuery<EmployeeEntity>().Where(filter);

var employees = employeeTable.ExecuteQuery(employeeQuery);
foreach (var emp in employees)
{
    // ...
}  
```

Aşağıdaki C# kodu, devam belirteçlerini açıkça işler:  

```csharp
string filter = TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<EmployeeEntity> employeeQuery = new TableQuery<EmployeeEntity>().Where(filter);

TableContinuationToken continuationToken = null;
do
{
    var employees = employeeTable.ExecuteQuerySegmented(employeeQuery, continuationToken);
    foreach (var emp in employees)
    {
        // ...
    }
    
    continuationToken = employees.ContinuationToken;
} while (continuationToken != null);  
```

Devam belirteçlerini açıkça kullanarak, uygulamanızın bir sonraki veri kesimini ne zaman aldığını denetleyebilirsiniz. Örneğin, istemci uygulamanız kullanıcıların bir tabloda depolanan varlıklar aracılığıyla sayfa uygulamasına izin veriyorsa, kullanıcı sorgu tarafından alınan tüm varlıklar arasında sayfa kullanmamaya karar verebilir, böylece uygulamanız yalnızca bir sonraki kesimi almak için bir devam belirteci kullanır. kullanıcı, geçerli segmentteki tüm varlıklar arasında sayfalamayı tamamlamıştı. Bu yaklaşımın birkaç faydası vardır:  

* Tablo hizmetinden alınacak veri miktarını sınırlamanızı ve ağ üzerinde hareket etmenizi sağlar.  
* .NET'te eşzamanlı IO gerçekleştirmenizi sağlar.  
* Bir uygulama nın çatlaması durumunda devam edebilmeniz için kalıcı depolama ya da kalıcı depolama için devamı belirteci serihale sağlar.  

> [!NOTE]
> Devamı belirteci genellikle 1.000 varlık içeren bir kesim döndürür, ancak daha az olabilir. Arama ölçütlerinizle eşleşen ilk n varlıklarını döndürmek için **Bir** sorgunun geri dönen giriş sayısını sınırlarsanız da durum böyledir: tablo hizmeti, kalan varlıkları almanızı sağlamak için bir devamı belirteciyle birlikte n'den az varlık içeren bir segment döndürebilir.  
> 
> 

Aşağıdaki C# kodu, bir segment içinde döndürülen varlıkların sayısını nasıl değiştirebilirsiniz gösterir:  

```csharp
employeeQuery.TakeCount = 50;  
```

### <a name="server-side-projection"></a>Sunucu tarafı projeksiyonu
Tek bir varlık en fazla 255 özellüğe sahip olabilir ve 1 MB boyutuna kadar olabilir. Tabloyu sorgulayıp varlıkları geri aldığınızda, tüm özelliklere ihtiyaç duymayabilir ve gereksiz yere veri aktarımından kaçınabilirsiniz (gecikme süresini ve maliyeti azaltmaya yardımcı olmak için). İhtiyacınız olan özellikleri aktarmak için sunucu tarafı projeksiyonu kullanabilirsiniz. Aşağıdaki örnek, sorgu tarafından seçilen varlıklardan yalnızca **E-posta** özelliğini **(PartitionKey,** **RowKey,** **Timestamp**ve **ETag**ile birlikte) alır.  

```csharp
string filter = TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Sales");
List<string> columns = new List<string>() { "Email" };
TableQuery<EmployeeEntity> employeeQuery =
    new TableQuery<EmployeeEntity>().Where(filter).Select(columns);

var entities = employeeTable.ExecuteQuery(employeeQuery);
foreach (var e in entities)
{
    Console.WriteLine("RowKey: {0}, EmployeeEmail: {1}", e.RowKey, e.Email);
}  
```

**RowKey** değerinin, alınacak özellikler listesine dahil edilmemesine rağmen nasıl kullanılabilir olduğuna dikkat edin.  

## <a name="modifying-entities"></a>Varlıkları değiştirme
Depolama İstemci Kitaplığı, tablo hizmetinde depolanan varlıklarınızı, varlıkları ekleyerek, silerek ve güncelleştirerek değiştirmenize olanak tanır. EGT'leri, gerekli gidiş-dönüş sayısını azaltmak ve çözümünüzünüzün performansını artırmak için birden fazla eklemeyi toplu olarak kullanmak, işlemleri güncellemek ve silmek için kullanabilirsiniz.  

Depolama İstemci Kitaplığı bir EGT yürüdüğünde atılan özel durumlar genellikle toplu işlemin başarısız olmasıiçin neden olan varlığın dizinini içerir. Bu, EGT'leri kullanan kod hata ayıklama yaparken yararlıdır.  

Ayrıca, tasarımınızın istemci uygulamanızın eşzamanlılık ve güncelleştirme işlemlerini nasıl ele adadığını da göz önünde bulundurmalısınız.  

### <a name="managing-concurrency"></a>Eşzamanlılığı yönetme
Varsayılan olarak, tablo hizmeti, bir istemcinin tablo hizmetini bu denetimleri atlayarak zorlaması mümkün olsa da, **Insert**, **Birleştirme**ve **Silme** işlemleri için tek tek varlıklar düzeyinde iyimser eşzamanlılık denetimleri uygular. Tablo hizmetinin eşzamanlılığı nasıl yönettiği hakkında daha fazla bilgi için [bkz.](../../storage/common/storage-concurrency.md)  

### <a name="merge-or-replace"></a>Birleştirme veya değiştirme
**TableOperation** sınıfının **Değiştir** yöntemi her zaman Tablo hizmetindeki tüm varlığın yerini alır. Bu özellik depolanan varlıkta bulunduğunda isteğe bir özellik eklemezseniz, istek bu özelliği depolanan varlıktan kaldırır. Bir özelliği depolanmış bir varlıktan açıkça kaldırmak istemiyorsanız, istekteki tüm mülkleri eklemeniz gerekir.  

Bir varlığı güncelleştirmek istediğinizde Tablo hizmetine gönderdiğiniz veri miktarını azaltmak için **TableOperation** sınıfının **Birleştirme** yöntemini kullanabilirsiniz. **Birleştirme** yöntemi, depolanan varlıktaki tüm özellikleri isteğe dahil olan varlıktan gelen özellik değerleriyle değiştirir, ancak depolanan varlıkta isteğe dahil olmayan tüm özellikleri sağlam bırakır. Bu, büyük varlıklarınız varsa ve yalnızca bir istekteki az sayıda özelliği güncelleştirmeniz gerekiyorsa yararlıdır.  

> [!NOTE]
> Varlık yoksa **Değiştir** ve **Birleştir** yöntemleri başarısız olur. Alternatif olarak, yeni bir varlık oluşturan **InsertOrReplace** ve **InsertOrMerge** yöntemlerini kullanabilirsiniz.  
> 
> 

## <a name="working-with-heterogeneous-entity-types"></a>Heterojen varlık tipleri ile çalışma
Tablo hizmeti, tek bir tablonun tasarımınızda büyük esneklik sağlayan birden çok türdeki varlıkları depolayabildiği anlamına gelen *şemasız* bir tablo deposudur. Aşağıdaki örnekte, hem çalışan hem de departman varlıklarını depolayan bir tablo gösterilebilir:  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Zaman damgası</th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>DepartmentName</th>
<th>Çalışan Sayısı</th>
</tr>
<tr>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

Her varlığın **yine de PartitionKey,** **RowKey**ve **Timestamp** değerleri olması gerekir, ancak herhangi bir özellik kümesi olabilir. Ayrıca, bu bilgileri bir yerde depolamayı seçmediğiniz sürece varlığın türünü gösteren hiçbir şey yoktur. Varlık türünü tanımlamak için iki seçenek vardır:  

* Varlık türünü **RowKey'e** (veya büyük olasılıkla **PartitionKey'e)** hazırlayın. Örneğin, **EMPLOYEE_000123** EMPLOYEE_000123 veya **rowkey** değerleri olarak **DEPARTMENT_SALES.**  
* Aşağıdaki tabloda gösterildiği gibi varlık türünü kaydetmek için ayrı bir özellik kullanın.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Zaman damgası</th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Employee (Çalışan)</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Employee (Çalışan)</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>DepartmentName</th>
<th>Çalışan Sayısı</th>
</tr>
<tr>
<td>Bölüm</td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Employee (Çalışan)</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

Varlık türünü **RowKey'e**önceden bekleyen ilk seçenek, farklı türde iki varlığın aynı anahtar değerine sahip olma olasılığı varsa yararlıdır. Ayrıca, aynı türdeki varlıkları bölümiçinde gruplayır.  

Bu bölümde tartışılan teknikler özellikle makale [modelleme ilişkileri](table-storage-design-modeling.md)bu kılavuzda daha önce tartışma [Kalıtım ilişkileri](table-storage-design-modeling.md#inheritance-relationships) ile ilgilidir.  

> [!NOTE]
> İstemci uygulamalarının POCO nesnelerini geliştirmesini ve farklı sürümlerle çalışmasını sağlamak için varlık türü değerine bir sürüm numarası dahil etmeyi düşünmelisiniz.  
> 
> 

Bu bölümün geri kalanı, Depolama İstemci Kitaplığı'nda aynı tablodaki birden çok varlık türüyle çalışmayı kolaylaştıran bazı özellikleri açıklar.  

### <a name="retrieving-heterogeneous-entity-types"></a>Heterojen varlık türlerinin alınması
Depolama İstemci Kitaplığı kullanıyorsanız, birden çok varlık türüyle çalışmak için üç seçeneğiniz vardır.  

Belirli bir **RowKey** ve **PartitionKey** değerleriyle depolanan varlığın türünü biliyorsanız, **önceki**iki örnekte gösterildiği gibi varlığı aldığınızda varlık türünü belirtebilirsiniz: [Depolama İstemci Kitaplığı'nı kullanarak bir nokta sorgusu yürütme](#executing-a-point-query-using-the-storage-client-library) ve [LINQ kullanarak birden çok varlığı alma](#retrieving-multiple-entities-using-linq).  

İkinci seçenek, somut bir POCO varlık türü yerine **DynamicTableEntity** türünü (özellik torbası) kullanmaktır (bu seçenek, varlığı .NET türlerine serihale ve deserialize etmeye gerek olmadığından performansı da artırabilir). Aşağıdaki C# kodu, tablodan farklı türlerde birden çok varlık alabilir, ancak tüm varlıkları **DynamicTableEntity** örnekleri olarak döndürür. Daha sonra her varlığın türünü belirlemek için **EntityType** özelliğini kullanır:  

```csharp
string filter =
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Sales"),
        TableOperators.And,
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThanOrEqual, "B"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "F")));
        
TableQuery<DynamicTableEntity> entityQuery =
    new TableQuery<DynamicTableEntity>().Where(filter);
    
var employees = employeeTable.ExecuteQuery(entityQuery);

IEnumerable<DynamicTableEntity> entities = employeeTable.ExecuteQuery(entityQuery);
foreach (var e in entities)
{
    EntityProperty entityTypeProperty;
    if (e.Properties.TryGetValue("EntityType", out entityTypeProperty))
    {
        if (entityTypeProperty.StringValue == "Employee")
        {
            // use entityTypeProperty, RowKey, PartitionKey, Etag, and Timestamp
        }
    }
}  
```

Diğer özellikleri almak için **DynamicTableEntity** sınıfının **Özellikler** özelliğinde **TryGetValue** yöntemini kullanmanız gerekir.  

Üçüncü bir seçenek **DynamicTableEntity** türü ve **EntityResolver** örneğini kullanarak birleştirmektir. Bu, aynı sorguda birden çok POCO türüne çözüm sağlamanızı sağlar. Bu örnekte, **EntityResolver** temsilcisi, sorgunun döndürdeceği iki varlık türünü birbirinden ayırmak için **EntityType** özelliğini kullanır. **Çözümle** yöntemi, **Dinamik TabloVarlık** örneklerini **TableEntity** örnekleriyle çözmek için **çözümleyici** temsilcini kullanır.  

```csharp
EntityResolver<TableEntity> resolver = (pk, rk, ts, props, etag) =>
{
    TableEntity resolvedEntity = null;
    if (props["EntityType"].StringValue == "Department")
    {
        resolvedEntity = new DepartmentEntity();
    }
    else if (props["EntityType"].StringValue == "Employee")
    {
        resolvedEntity = new EmployeeEntity();
    }
    else 
    {
        throw new ArgumentException("Unrecognized entity", "props");
    }

    resolvedEntity.PartitionKey = pk;
    resolvedEntity.RowKey = rk;
    resolvedEntity.Timestamp = ts;
    resolvedEntity.ETag = etag;
    resolvedEntity.ReadEntity(props, null);
    return resolvedEntity;
};

string filter = TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Sales");
        
TableQuery<DynamicTableEntity> entityQuery = new TableQuery<DynamicTableEntity>().Where(filter);

var entities = employeeTable.ExecuteQuery(entityQuery, resolver);
foreach (var e in entities)
{
    if (e is DepartmentEntity)
    {
        // ...
    }
    else if (e is EmployeeEntity)
    {
        // ...
    }
}  
```

### <a name="modifying-heterogeneous-entity-types"></a>Heterojen varlık türlerini değiştirme
Silmek için varlığın türünü bilmeniz gerekmez ve eklediğinizde varlığın türünü her zaman bilirsiniz. Ancak, bir varlığı türünü bilmeden ve BIR POCO varlık sınıfı kullanmadan güncelleştirmek için **DynamicTableEntity** türünü kullanabilirsiniz. Aşağıdaki kod örneği tek bir varlığı alır ve güncelleştirilmeden önce **EmployeeCount** özelliğinin var olduğunu denetler.  

```csharp
TableResult result = employeeTable.Execute(TableOperation.Retrieve(partitionKey, rowKey));
DynamicTableEntity department = (DynamicTableEntity)result.Result;

EntityProperty countProperty;
if (!department.Properties.TryGetValue("EmployeeCount", out countProperty))
{
    throw new InvalidOperationException("Invalid entity, EmployeeCount property not found.");
}

countProperty.Int32Value += 1;
employeeTable.Execute(TableOperation.Merge(department));
```

## <a name="controlling-access-with-shared-access-signatures"></a>Paylaşılan Erişim İmzaları ile erişimi denetleme
Paylaşılan Erişim İmzası (SAS) belirteçlerini, istemci uygulamalarının depolama hesabı anahtarınızı kodunuza eklemenize gerek kalmadan tablo varlıklarını değiştirmesine (ve sorgulayabilmesini) etkinleştirmek için kullanabilirsiniz. Genellikle, uygulamanızda SAS kullanmanın üç ana avantajı vardır:  

* Bu aygıtın Tablo hizmetindeki varlıklara erişmesine ve varlıkları değiştirmesine izin vermek için depolama hesabı anahtarınızı güvenli olmayan bir platforma (mobil aygıt gibi) dağıtmanız gerekmez.  
* Web ve çalışan rollerinin varlıklarınızı yönetmede gerçekleştirdiği çalışmaların bir kısmını son kullanıcı bilgisayarları ve mobil aygıtlar gibi istemci aygıtlarına boşaltabilirsiniz.  
* Bir istemciye kısıtlı ve sınırlı izinler atayabilirsiniz (örneğin, belirli kaynaklara salt okunur erişime izin vermek gibi).  

Tablo hizmetinde SAS belirteçlerini kullanma hakkında daha fazla bilgi için [bkz.](../../storage/common/storage-sas-overview.md)  

Ancak, yine de tablo hizmetindeki varlıklara istemci uygulaması veren SAS belirteçleri oluşturmanız gerekir: bunu depolama hesabı anahtarlarınıza güvenli erişimi olan bir ortamda yapmalısınız. Genellikle, SAS belirteçlerini oluşturmak ve bunları varlıklarınıza erişması gereken istemci uygulamalarına teslim etmek için bir web veya çalışan rolü kullanırsınız. Müşterilere SAS belirteçleri oluşturma ve sunma da dahil olmak üzere hala bir ek yükü olduğundan, özellikle yüksek hacimli senaryolarda bu yükü en iyi nasıl azaltabileceğinizi düşünmelisiniz.  

Tablodaki varlıkların bir alt kümesine erişim sağlayan bir SAS belirteci oluşturmak mümkündür. Varsayılan olarak, tüm tablo için bir SAS belirteci oluşturursunuz, ancak SAS belirteci nin bir dizi **PartitionKey** değerine veya **PartitionKey** ve **RowKey** değerlerine erişim izni verdiğini de belirtmek mümkündür. Sisteminizin tek tek kullanıcıları için, her kullanıcının SAS belirteci yalnızca tablo hizmetinde kendi varlıklarına erişmelerine izin veren SAS belirteçleri oluşturmayı seçebilirsiniz.  

## <a name="asynchronous-and-parallel-operations"></a>Asynchronous ve paralel işlemler
İsteklerinizi birden çok bölüme yayıyorsanız, eşzamanlı veya paralel sorgular kullanarak iş ve istemci yanıt verme yeteneğini artırabilirsiniz.
Örneğin, tablolarınıza paralel olarak erişen iki veya daha fazla alt rol örneğiniz olabilir. Belirli bölüm kümelerinden sorumlu tek tek çalışan rolleri olabilir veya her biri bir tablodaki tüm bölümlere erişebilen birden çok alt rol örneğine sahip olabilirsiniz.  

İstemci örneğinde, depolama işlemlerini eşit bir şekilde yürüterek iş elde etme işlemini artırabilirsiniz. Depolama İstemci Kitaplığı, eşzamanlı sorgular ve değişiklikler yazmayı kolaylaştırır. Örneğin, aşağıdaki C# kodunda gösterildiği gibi bir bölümdeki tüm varlıkları alan senkron yöntemiyle başlayabilirsiniz:  

```csharp
private static void ManyEntitiesQuery(CloudTable employeeTable, string department)
{
    string filter = TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, department);
    TableQuery<EmployeeEntity> employeeQuery = new TableQuery<EmployeeEntity>().Where(filter);

    TableContinuationToken continuationToken = null;
    do
    {
        var employees = employeeTable.ExecuteQuerySegmented(employeeQuery, continuationToken);
        foreach (var emp in employees)
        {
            // ...
        }
        
        continuationToken = employees.ContinuationToken;
    } while (continuationToken != null);
}  
```

Sorgunun eş senkronize olarak aşağıdaki gibi çalıştırılabilmeleri için bu kodu kolayca değiştirebilirsiniz:  

```csharp
private static async Task ManyEntitiesQueryAsync(CloudTable employeeTable, string department)
{
    string filter = TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, department);
    TableQuery<EmployeeEntity> employeeQuery = new TableQuery<EmployeeEntity>().Where(filter);
    
    TableContinuationToken continuationToken = null;
    do
    {
        var employees = await employeeTable.ExecuteQuerySegmentedAsync(employeeQuery, continuationToken);
        foreach (var emp in employees)
        {
            // ...
        }
    
        continuationToken = employees.ContinuationToken;
    } while (continuationToken != null);
}  
```

Bu eşzamanlı örnekte, senkron sürümden aşağıdaki değişiklikleri görebilirsiniz:  

* Yöntem imzası şimdi **async** değiştirici içerir ve bir **Görev** örneği döndürür.  
* Sonuçları almak için **ExecuteSegmented** yöntemini çağırmak yerine, yöntem şimdi **ExecuteSegmentedAsync** yöntemini çağırır ve sonuçları eşzamanlı olarak almak için **bekleyen** değiştiricisini kullanır.  

İstemci uygulaması bu yöntemi birden çok kez **(departman** parametresi için farklı değerlerle) arayabilir ve her sorgu ayrı bir iş parçacığı üzerinde çalışır.  

**TabloSorgusu** sınıfında **Yürüt** metodunun eşzamanlı sürümü yoktur, çünkü **Ayrılmaz** arabirim asynchronous numaralandırmayı desteklemez.  

Ayrıca varlıkları eşit bir şekilde ekleyebilir, güncelleyebilir ve silebilirsiniz. Aşağıdaki C# örneği, çalışan varlığını eklemek veya değiştirmek için basit, eşzamanlı bir yöntem gösterir:  

```csharp
private static void SimpleEmployeeUpsert(
    CloudTable employeeTable,
    EmployeeEntity employee)
{
    TableResult result = employeeTable.Execute(TableOperation.InsertOrReplace(employee));
    Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

Güncelleştirmenin aşağıdaki gibi eşit bir şekilde çalıştırılabilmeleri için bu kodu kolayca değiştirebilirsiniz:  

```csharp
private static async Task SimpleEmployeeUpsertAsync(
    CloudTable employeeTable,
    EmployeeEntity employee)
{
    TableResult result = await employeeTable.ExecuteAsync(TableOperation.InsertOrReplace(employee));
    Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

Bu eşzamanlı örnekte, senkron sürümden aşağıdaki değişiklikleri görebilirsiniz:  

* Yöntem imzası şimdi **async** değiştirici içerir ve bir **Görev** örneği döndürür.  
* Varlığı güncelleştirmek için **Yürüt** metodunu çağırmak yerine, yöntem şimdi **ExecuteAsync** yöntemini çağırır ve sonuçları eşit bir şekilde almak için **bekleyen** değiştiricisini kullanır.  

İstemci uygulaması bunun gibi birden çok eşzamanlı yöntem çağırabilir ve her yöntem çağırma ayrı bir iş parçacığı üzerinde çalışacaktır.  

## <a name="next-steps"></a>Sonraki adımlar

- [İlişkileri modelleme](table-storage-design-modeling.md)
- [Sorgulama için tasarım](table-storage-design-for-query.md)
- [Tablo verilerini şifreleme](table-storage-design-encrypt-data.md)
- [Veri değişikliği için tasarım](table-storage-design-for-modification.md)
