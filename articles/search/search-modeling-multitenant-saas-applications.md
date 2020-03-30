---
title: Çoklu oyunculuğu ve içerik yalıtımı
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'yı kullanırken çok kiracılı SaaS uygulamaları için yaygın tasarım desenleri hakkında bilgi edinin.
manager: nitinme
author: LiamCavanagh
ms.author: liamca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: d37abd1b5d212c3d920cb68b6236029b2112ae24
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74113264"
---
# <a name="design-patterns-for-multitenant-saas-applications-and-azure-cognitive-search"></a>Çok kiracılı SaaS uygulamaları ve Azure Bilişsel Arama için tasarım desenleri
Çok kiracılı uygulama, başka bir kiracının verilerini göremeyen veya paylaşamayan kiracı sayısına aynı hizmetleri ve yetenekleri sağlayan uygulamadır. Bu belgede, Azure Bilişsel Arama ile oluşturulmuş çok kiracılı uygulamalar için kiracı yalıtım stratejileri anlatılmaktadır.

## <a name="azure-cognitive-search-concepts"></a>Azure Bilişsel Arama kavramları
Hizmet olarak arama çözümü olarak Azure Bilişsel Arama, geliştiricilerin herhangi bir altyapıyı yönetmeden veya bilgi alma konusunda uzman olmadan uygulamalara zengin arama deneyimleri eklemelerine olanak tanır. Veriler hizmete yüklenir ve bulutta depolanır. Azure Bilişsel Arama API'sine yapılan basit istekler kullanılarak veriler değiştirilebilir ve aranabilir. Hizmete genel bir bakış [bu makalede](https://aka.ms/whatisazsearch)bulunabilir. Tasarım modellerini tartışmadan önce Azure Bilişsel Arama'da bazı kavramları anlamak önemlidir.

### <a name="search-services-indexes-fields-and-documents"></a>Arama hizmetleri, dizinler, alanlar ve belgeler
Azure Bilişsel Arama'yı kullanırken, bir *arama hizmetine*abone olunur. Veriler Azure Bilişsel Arama'ya yüklendikçe, arama hizmeti içindeki bir *dizinde* depolanır. Tek bir hizmet içinde diziler bir dizi olabilir. Veritabanlarının tanıdık kavramlarını kullanmak için, bir hizmetteki dizinler veritabanındaki tablolara benzetilebilirken, arama hizmeti veritabanına benzetilebilir.

Bir arama hizmeti içindeki her dizin, özelleştirilebilir *alanlar*bir dizi tarafından tanımlanan kendi şema vardır. Veriler, azure bilişsel arama dizinine tek tek *belgeler*biçiminde eklenir. Her belge belirli bir dizin için yüklenmiş olmalı ve bu dizin şemaya uymalıdır. Azure Bilişsel Arama'yı kullanarak veri ararken, tam metin arama sorguları belirli bir dizine karşı verilir.  Bu kavramları veritabanındakilerle karşılaştırmak için alanlar tablodaki sütunlara benzetilebilir ve belgeler satırlara benzetilebilir.

### <a name="scalability"></a>Ölçeklenebilirlik
Standart [fiyatlandırma katmanındaki](https://azure.microsoft.com/pricing/details/search/) herhangi bir Azure Bilişsel Arama hizmeti iki boyutta ölçeklenebilir: depolama ve kullanılabilirlik.

* *Bir* arama hizmetinin depolanmasını artırmak için bölümler eklenebilir.
* *Yinelemeler,* bir arama hizmetinin işleyebilir isteklerinin işlenebilir büyütmek için bir hizmetekilebilir.

Bölüm ve yinelemelerin eklenmesi ve kaldırılması, arama hizmetinin kapasitesinin, uygulamanın gerektirdiği veri miktarı yla birlikte büyümesine ve trafik temesine olanak sağlar. Bir arama hizmetinin okuma [SLA'sını](https://azure.microsoft.com/support/legal/sla/search/v1_0/)elde edebilmesi için iki yineleme gerektirir. Bir hizmetin okuma yazma [SLA'sına](https://azure.microsoft.com/support/legal/sla/search/v1_0/)ulaşabilmesi için üç yineleme gerektirir.

### <a name="service-and-index-limits-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da hizmet ve dizin sınırları
Azure Bilişsel Arama'da birkaç farklı [fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/search/) vardır, katmanların her birinin farklı [sınırları ve kotaları](search-limits-quotas-capacity.md)vardır. Bu sınırlardan bazıları hizmet düzeyinde, bazıları dizin düzeyinde ve bazıları bölüm düzeyindedir.

|  | Temel | Standart1 | Standart2 | Standart3 | Standart3 HD |
| --- | --- | --- | --- | --- | --- |
| Hizmet Başına Maksimum Yinelemeler |3 |12 |12 |12 |12 |
| Hizmet Başına Maksimum Bölümler |1 |12 |12 |12 |3 |
| Hizmet Başına Maksimum Arama Birimleri (Yinelemeler*Bölümler) |3 |36 |36 |36 |36 (max 3 bölüm) |
| Hizmet Başına Maksimum Depolama |2 GB |300 GB |1,2 TB |2,4 TB |600 GB |
| Bölüm Başına Maksimum Depolama |2 GB |25 GB |100 GB |200 GB |200 GB |
| Hizmet Başına Maksimum Dizinler |5 |50 |200 |200 |3000 (max 1000 dizinler/bölümleme) |

#### <a name="s3-high-density"></a>S3 Yüksek Yoğunluklu'
Azure Bilişsel Arama'nın S3 fiyatlandırma katmanında, çok kiracılı senaryolar için özel olarak tasarlanmış Yüksek Yoğunluklu (HD) modu için bir seçenek vardır. Çoğu durumda, basitlik ve maliyet verimliliği nin avantajlarından yararlanmak için çok sayıda küçük kiracıyı tek bir hizmet altında desteklemek gerekir.

S3 HD, tek bir hizmette daha fazla dizin barındırma yeteneği için bölümleri kullanarak dizinleri ölçeklendirme yeteneği ticareti yaparak birçok küçük dizinlerin tek bir arama hizmetinin yönetimi altında paketlenmesini sağlar.

Somut olarak, bir S3 hizmetinin birlikte 1,4 milyar belgeye ev sahipliği yapabilecek 1 ila 200 dizinleri olabilir. Öte yandan bir S3 HD, tek tek dizinlerin yalnızca 1 milyon belgeye kadar gitmesini sağlar, ancak bölüm başına 1000'e kadar dizin (hizmet başına 3000'e kadar) bölüm başına toplam belge sayısı 200 milyon (hizmet başına 600 milyona kadar) işleyebilir.

## <a name="considerations-for-multitenant-applications"></a>Çok kiracılı uygulamalar için dikkat edilmesi gerekenler
Çok kiracılı uygulamalar, çeşitli kiracılar arasında gizlilik düzeyi korurken, kaynakları kiracılar arasında etkin bir şekilde dağıtmalıdır. Böyle bir uygulama için mimari tasarlarken birkaç husus vardır:

* *Kiracı yalıtımı:* Uygulama geliştiricilerin, hiçbir kiracının diğer kiracıların verilerine yetkisiz veya istenmeyen erişime sahip olmadığından emin olmak için uygun önlemleri alması gerekir. Veri gizliliği perspektifinin ötesinde, kiracı yalıtım stratejileri paylaşılan kaynakların etkin bir şekilde yönetilmesini ve gürültülü komşulara karşı koruma gerektirir.
* *Bulut kaynak maliyeti:* Diğer uygulamalarda olduğu gibi, yazılım çözümleri de çok kiracılı bir uygulamanın bir bileşeni olarak maliyet açısından rekabetçi kalmalıdır.
* *İşlem Kolaylığı:* Çok kiracılı bir mimari geliştirirken, uygulamanın işlemleri ve karmaşıklığı üzerindeki etkisi önemli bir husustur. Azure Bilişsel [Arama%99,9 SLA'ya](https://azure.microsoft.com/support/legal/sla/search/v1_0/)sahiptir.
* *Küresel ayak izi:* Çok kiracılı uygulamaların, dünya çapında dağıtılan kiracılara etkin bir şekilde hizmet etmesi gerekebilir.
* *Ölçeklenebilirlik:* Uygulama geliştiricilerin, yeterince düşük bir uygulama karmaşıklığı nı korumakla uygulamayı kiracı sayısıyla ölçeklendirecek şekilde tasarlamak ile kiracıların veri ve iş yükünün boyutu arasında nasıl uzlaştırdıklarını göz önünde bulundurmaları gerekir.

Azure Bilişsel Arama, kiracıların verilerini ve iş yükünü yalıtmak için kullanılabilecek birkaç sınır sunar.

## <a name="modeling-multitenancy-with-azure-cognitive-search"></a>Azure Bilişsel Arama ile çoklu kiralama yı modelleme
Çok kiracılı bir senaryo durumunda, uygulama geliştiricisi bir veya daha fazla arama hizmeti tüketir ve kiracılarını hizmetler, dizinler veya her ikisi arasında böler. Azure Bilişsel Arama, çok kiracılı bir senaryoyu modellerken birkaç ortak desene sahiptir:

1. *Kiracı başına dizin:* Her kiracının, diğer kiracılarla paylaşılan bir arama hizmeti içinde kendi dizini vardır.
2. *Kiracı başına hizmet:* Her kiracının, en yüksek düzeyde veri ve iş yükü ayrımı sunan kendi özel Azure Bilişsel Arama hizmeti vardır.
3. *Her ikisinin karışımı:* Daha büyük, daha etkin kiracılara özel hizmetler atanırken, daha küçük kiracılarpaylaşılan hizmetler içinde tek tek dizinler atanır.

## <a name="1-index-per-tenant"></a>1. Kiracı başına endeks
![Kiracı başına dizin modelinin bir tasviri](./media/search-modeling-multitenant-saas-applications/azure-search-index-per-tenant.png)

Kiracı başına dizin modelinde, birden çok kiracı, her kiracının kendi dizinine sahip olduğu tek bir Azure Bilişsel Arama hizmetinde yer alır.

Tüm arama istekleri ve belge işlemleri Azure Bilişsel Arama'da dizin düzeyinde yayımlandığından, kiracılar veri yalıtımı elde eder. Uygulama katmanında, çeşitli kiracıların trafiğini uygun dizinlere yönlendirirken, tüm kiracılar arasında hizmet düzeyindeki kaynakları yönetme ye ihtiyaç bilinci vardır.

Kiracı başına dizin modelinin önemli bir özelliği, uygulama geliştiricisinin uygulamanın kiracıları arasında bir arama hizmetinin kapasitesini aşmasıdır. Kiracının iş yükünün düzensiz bir dağılımı varsa, kiracıların en uygun birleşimi, aynı anda daha az uzun bir kuyruk hizmet ederken, son derece etkin, kaynak yoğun kiracı bir dizi karşılamak için bir arama hizmetinin dizinleri arasında dağıtılabilir aktif kiracılar. Takas, modelin her kiracının aynı anda son derece etkin olduğu durumları ele alamamasıdır.

Kiracı başına dizin modeli, tüm Azure Bilişsel Arama hizmetinin önceden satın alınıp daha sonra kiracılarla doldurulduğu değişken maliyet modelinin temelini oluşturur. Bu, kullanılmayan kapasitenin denemeler ve ücretsiz hesaplar için belirlenmesine olanak tanır.

Genel ayak izine sahip uygulamalariçin, kiracı başına dizin modeli en verimli olmayabilir. Bir uygulamanın kiracıları dünya çapında dağıtılırsa, her bölge için, maliyetleri her biri arasında yineleyebilecek ayrı bir hizmet gerekebilir.

Azure Bilişsel Arama, hem tek tek dizinlerin ölçeğinin hem de toplam dizin sayısının büyümesine olanak tanır. Uygun bir fiyatlandırma katmanı seçilirse, hizmet içindeki tek bir dizin depolama veya trafik açısından çok büyük büyüdüğünde, tüm arama hizmetine bölümler ve yinelemeler eklenebilir.

Dizin lerin toplam sayısı tek bir hizmet için çok büyük olursa, yeni kiracıları barındırmak için başka bir hizmetin sağlanması gerekir. Yeni hizmetler eklendikçe dizinlerin arama hizmetleri arasında taşınması gerekiyorsa, Azure Bilişsel Arama bir dizinin taşınmasına izin vermediği için dizindeki verilerin bir dizinden diğerine el ile kopyalanması gerekir.

## <a name="2-service-per-tenant"></a>2. Kiracı başına hizmet
![Kiracı başına hizmet modelinin bir tasviri](./media/search-modeling-multitenant-saas-applications/azure-search-service-per-tenant.png)

Kiracı başına hizmet mimarisinde, her kiracının kendi arama hizmeti vardır.

Bu modelde, uygulama kiracıları için maksimum yalıtım düzeyine ulaşır. Her hizmet, arama isteğini işlemenin yanı sıra ayrı API anahtarları için özel depolama ve iş metoduna sahiptir.

Her kiracının büyük bir ayak izine sahip olduğu veya iş yükünün kiracıdan kiracıya çok az değişkenliğe sahip olduğu uygulamalarda, kaynaklar çeşitli kiracıların iş yükleri arasında paylaşılmaması nedeniyle kiracı başına hizmet modeli etkili bir seçimdir.

Kiracı modeli başına bir hizmet de öngörülebilir, sabit maliyet modeli nin yararı nı sunar. Bir kiracı dolduracak bir kiracı olana kadar tüm arama hizmetinde ön yatırım yoktur, ancak kiracı başına maliyet, kiracı başına dizin modelinden daha yüksektir.

Kiracı başına hizmet modeli, küresel ayak izine sahip uygulamalar için etkili bir seçimdir. Coğrafi olarak dağıtılmış kiracılarla, her kiracının hizmetinin uygun bölgede olması kolaydır.

Bu örüntün ölçeklemede karşılaşılan zorluklar, bireysel kiracıların hizmetlerinden daha fazla büyümesi yle ortaya çıkar. Azure Bilişsel Arama şu anda bir arama hizmetinin fiyatlandırma katmanının yükseltilmesine destek sağlamaz, bu nedenle tüm verilerin yeni bir hizmete el ile kopyalanması gerekir.

## <a name="3-mixing-both-models"></a>3. Her iki modelin karıştırılması
Çoklu kirayı modellemek için başka bir model, hem kiracı başına dizin ve hizmet başına kiracı stratejilerini karıştırmaktır.

İki örüntüyu karıştırarak, bir uygulamanın en büyük kiracıları özel hizmetleri işgal ederken, daha az etkin, daha küçük kiracıların uzun kuyruğu paylaşılan bir hizmetteki dizinleri kaplayabilir. Bu model, en büyük kiracıların hizmetten sürekli olarak yüksek performans alabilmesini sağlarken, küçük kiracıların gürültülü komşulardan korunmasına yardımcı olur.

Ancak, bu stratejinin uygulanması, paylaşılan bir hizmetteki bir dizine karşı hangi kiracıların özel bir hizmete ihtiyaç duyacağını tahmin etmede öngörüye dayanır. Uygulama karmaşıklığı, bu çok amaçlı modellerin her ikisini de yönetme ihtiyacı yla artar.

## <a name="achieving-even-finer-granularity"></a>Daha da ince taneciklilik elde etmek
Azure Bilişsel Arama'da çok kiracılı senaryoları modellemek için yukarıdaki tasarım desenleri, her kiracının bir uygulamanın bütün örneği olduğu tek tip bir kapsam varsayar. Ancak, uygulamalar bazen çok daha küçük kapsamları işleyebilir.

Kiracı başına hizmet ve kiracı başına dizin modelleri yeterince küçük kapsamlar değilse, daha da ince bir ayrıntı derecesi elde etmek için bir dizin modellemek mümkündür.

Tek bir dizin farklı istemci uç noktaları için farklı davranması için, bir alan her olası istemci için belirli bir değer belirleyen bir dizin eklenebilir. İstemci bir dizini sorgulamak veya değiştirmek için Azure Bilişsel Arama'yı her çağırınca, istemci uygulamasından gelen kod, sorgu sırasında Azure Bilişsel Arama'nın [filtre](https://msdn.microsoft.com/library/azure/dn798921.aspx) yeteneğini kullanarak bu alan için uygun değeri belirtir.

Bu yöntem, ayrı kullanıcı hesaplarının işlevselliğini, ayrı izin düzeylerini ve hatta tamamen ayrı uygulamaların işlevselliğini elde etmek için kullanılabilir.

> [!NOTE]
> Birden çok kiracı hizmet etmek için tek bir dizin yapılandırmak için yukarıda açıklanan yaklaşımı kullanarak arama sonuçlarının alaka etkiler. Arama alaka düzeyi puanları kiracı düzeyinde bir kapsamda değil, dizin düzeyinde bir kapsamda hesaplanır, bu nedenle tüm kiracıların verileri alaka düzeyi puanlarının terim sıklığı gibi temel istatistiklerine dahil edilir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Azure Bilişsel Arama birçok uygulama için ilgi çekici bir seçimdir. Çok kiracılı uygulamalar için çeşitli tasarım modellerini değerlendirirken, uygulama iş yüklerine ve her boyuttaki mimariye uyacak şekilde Azure Bilişsel Arama'yı en iyi şekilde uyarlamak için [çeşitli fiyatlandırma katmanlarını](https://azure.microsoft.com/pricing/details/search/) ve ilgili [hizmet sınırlarını](search-limits-quotas-capacity.md) göz önünde bulundurun.

Azure Bilişsel Arama ve çok kiracılı senaryolarla azuresearch_contact@microsoft.comilgili tüm sorular .

