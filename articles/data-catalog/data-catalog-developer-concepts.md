---
title: Azure Veri Kataloğu geliştirici kavramları
description: Azure Veri Kataloğu kavramsal modelindeki temel kavramlara giriş, Catalog REST API'de açıkolarak ortaya çıktı.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: 80adc98255cfc9145d583ac775bbc490d599234e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68976821"
---
# <a name="azure-data-catalog-developer-concepts"></a>Azure Veri Kataloğu geliştirici kavramları
Microsoft **Azure Veri Kataloğu,** veri kaynağı bulma ve kitle kaynak verisi meta verileri için yetenekler sağlayan tam olarak yönetilen bir bulut hizmetidir. Geliştiriciler hizmeti REST API'leri aracılığıyla kullanabilir. Hizmette uygulanan kavramları anlamak, geliştiricilerin Azure Veri **Kataloğu**ile başarılı bir şekilde bütünleşmesi açısından önemlidir.

## <a name="key-concepts"></a>Önemli kavramlar
**Azure Veri Kataloğu** kavramsal modeli dört temel kavrama dayanmaktadır: **Katalog,** **Kullanıcılar,** **Varlıklar**ve **Ek Açıklamalar**.

![Azure Veri Kataloğu kavramsal model illüstrasyonu](./media/data-catalog-developer-concepts/concept2.png)

*Şekil 1 - Azure Veri Kataloğu basitleştirilmiş kavramsal model*

### <a name="catalog"></a>Katalog
**Katalog,** kuruluşun depolatadaki tüm meta veriler için en üst düzey kapsayıcıdır. Azure Hesabı başına bir **Katalog'a** izin verilir. Kataloglar bir Azure aboneliğine bağlıdır, ancak bir hesabın birden çok aboneliği olsa bile, belirli bir Azure hesabı için yalnızca bir **Katalog** oluşturulabilir.

Bir **katalog, Kullanıcılar** ve **Varlıklar**içerir.

### <a name="users"></a>Kullanıcılar
Kullanıcılar, Katalog'da eylem gerçekleştirme izni (katalogda arama yapma, ekleme, kaldırma veya öğeleri kaldırma vb.) izinleri olan güvenlik ilkeleridir.

Bir kullanıcının sahip olabileceği birkaç farklı rol vardır. Roller hakkında daha fazla bilgi için Roller ve Yetkilendirme bölümüne bakın.

Tek tek kullanıcılar ve güvenlik grupları eklenebilir.

Azure Veri Kataloğu, kimlik ve erişim yönetimi için Azure Active Directory'yi kullanır. Her Katalog kullanıcısı, hesap için Active Directory üyesi olmalıdır.

### <a name="assets"></a>Varlıklar
**Katalog** veri varlıkları içerir. **Varlıklar,** katalog tarafından yönetilen taneciklilik birimidir.

Bir varlığın tanecikliliği veri kaynağına göre değişir. SQL Server veya Oracle Database için bir varlık Tablo veya Görünüm olabilir. SQL Server Analysis Services için bir varlık Ölçü, Boyut veya Anahtar Performans Göstergesi (KPI) olabilir. SQL Server Reporting Services için bir varlık Bir Rapordur.

**Varlık,** Katalog'dan eklediğiniz veya kaldırdığınız şeydir. **Bu arama**geri almak sonuç birimidir.

Bir **Varlık,** adından, konumundan ve türünden ve onu daha fazla tanımlayan ek açıklamalardan oluşur.

### <a name="annotations"></a>Ek Açıklamalar
Ek açıklamalar, Varlıklar hakkındaki meta verileri temsil eden öğelerdir.

Ek açıklamalara örnek olarak açıklama, etiketler, şema, dokümantasyon vb. verilebilir. Kıymet türleri ve ek açıklama türlerinin tam listesi Varlık Nesnesi modeli bölümünde bulunmaktadır.

## <a name="crowdsourcing-annotations-and-user-perspective-multiplicity-of-opinion"></a>Crowdsourcing ek açıklamaları ve kullanıcı perspektifi (görüş çokluğu)
Azure Veri Kataloğu'nun önemli bir yönü, sistemdeki meta verilerin kitlesel kullanımını nasıl desteklediğidir. Yalnızca bir görüşün olduğu ve son yazarın kazandığı wiki yaklaşımının aksine, Azure Veri Kataloğu modeli birden fazla görüşün sistemde yan yana yaşamasına olanak tanır.

Bu yaklaşım, farklı kullanıcıların belirli bir varlık üzerinde farklı bakış açılarına sahip olabileceği kurumsal verilerin gerçek dünyasını yansıtır:

* Bir veritabanı yöneticisi hizmet düzeyi anlaşmaları veya toplu ETL işlemleri için kullanılabilir işlem penceresi hakkında bilgi sağlayabilir
* Bir veri sorumlusu, varlığın uygulandığı iş süreçleri veya işletmenin ona uyguladığı sınıflandırmalar hakkında bilgi verebilir
* Bir finans analisti, dönem sonu raporlama görevleri sırasında verilerin nasıl kullanıldığı hakkında bilgi verebilir

Bu örneği desteklemek için, her kullanıcı (DBA, veri sorumlusu ve analist) Katalog'da kayıtlı olan tek bir tabloya bir açıklama ekleyebilir. Tüm açıklamalar sistemde tutulur ve Azure Veri Kataloğu portalında tüm açıklamalar görüntülenir.

Bu desen nesne modelindeki öğelerin çoğuna uygulanır, bu nedenle JSON yükündeki nesne türleri genellikle tek ton bekleyebileceğiniz özellikler için dizilerdir.

Örneğin, varlık kökünün altında açıklama nesneleri dizisi vardır. Dizi özelliği "açıklamalar" olarak adlandırılır. Açıklama nesnesi bir özelliği vardır - açıklama. Desen, açıklama yazan her kullanıcının, kullanıcı tarafından sağlanan değer için oluşturulmuş bir açıklama nesnesi elde edilmesidir.

UX daha sonra kombinasyonun nasıl görüntüleneceğini seçebilir. Görüntü için üç farklı desen vardır.

* En basit desen "Tümünü Göster". Bu desende, tüm nesneler liste görünümünde gösterilir. Azure Veri Kataloğu portalı UX açıklama için bu deseni kullanır.
* Başka bir desen "Birleştirme". Bu desende, farklı kullanıcılardan gelen tüm değerler birleştirilir ve yinelenen kaldırılır. Azure Veri Kataloğu portalı UX'deki bu modele örnek olarak etiketler ve uzman özellikleri verilebilir.
* Üçüncü bir desen "son yazar kazanır". Bu desende, yalnızca en son yazılan değer gösterilir. friendlyName bu modelin bir örneğidir.

## <a name="asset-object-model"></a>Varlık nesnesi modeli
Temel Kavramlar bölümünde tanıtılan **Azure Veri Kataloğu** nesne modeli, varlık veya ek açıklama olabilecek öğeleri içerir. Öğeleristeğe bağlı veya gerekli olabilir özelliklere sahiptir. Bazı özellikler tüm öğeler için geçerlidir. Bazı özellikler tüm varlıklar için geçerlidir. Bazı özellikler yalnızca belirli varlık türleri için geçerlidir.

### <a name="system-properties"></a>Sistem özellikleri
<table><tr><td><b>Özellik Adı</b></td><td><b>Veri Türü</b></td><td><b>Açıklamalar</b></td></tr><tr><td>timestamp</td><td>DateTime</td><td>Öğe son kez değiştirilmiştir. Bu alan, bir öğe eklendiğinde ve bir öğe her güncelleştirildiğinde sunucu tarafından oluşturulur. Bu özelliğin değeri, yayımlama işlemlerinin girişinde yoksayılır.</td></tr><tr><td>id</td><td>Urı</td><td>Öğenin mutlak url'si (salt okunur). Bu öğe için benzersiz adreslenebilir URI olduğunu.  Bu özelliğin değeri, yayımlama işlemlerinin girişinde yoksayılır.</td></tr><tr><td>type</td><td>Dize</td><td>Varlığın türü (salt okunur).</td></tr><tr><td>Etag</td><td>Dize</td><td>Katalogdaki öğeleri güncelleştiren işlemleri gerçekleştirirken iyimser eşzamanlılık denetimi için kullanılabilecek öğenin sürümüne karşılık gelen dize. "*" herhangi bir değerle eşleşecek şekilde kullanılabilir.</td></tr></table>

### <a name="common-properties"></a>Ortak özellikler
Bu özellikler tüm kök varlık türleri ve tüm ek açıklama türleri için geçerlidir.

<table>
<tr><td><b>Özellik Adı</b></td><td><b>Veri Türü</b></td><td><b>Açıklamalar</b></td></tr>
<tr><td>kaynak Kaynak Sistemi</td><td>Boole</td><td>Maddenin verilerinin bir kaynak sistemden (Sql Server Database, Oracle Database gibi) mı türetilmiş yoksa bir kullanıcı tarafından mı yazDığını gösterir.</td></tr>
</table>

### <a name="common-root-properties"></a>Ortak kök özellikleri
<p>
Bu özellikler tüm kök varlık türleri için geçerlidir.

<table><tr><td><b>Özellik Adı</b></td><td><b>Veri Türü</b></td><td><b>Açıklamalar</b></td></tr><tr><td>ad</td><td>Dize</td><td>Veri kaynağı konum bilgisinden türetilen bir ad</td></tr><tr><td>Dsl</td><td>DataSourceLocation</td><td>Veri kaynağını benzersiz bir şekilde açıklar ve varlığın tanımlayıcılarından biridir. (Bkz. çift kimlik bölümü).  dsl yapısı protokol ve kaynak türüne göre değişir.</td></tr><tr><td>Datasource</td><td>DataSourceInfo</td><td>Varlığın türü hakkında daha fazla ayrıntı.</td></tr><tr><td>lastRegisteredBy</td><td>Securityprincipal</td><td>Bu varlığı en son kaydeden kullanıcıyı açıklar.  Hem kullanıcı için benzersiz kimliği (upn) hem de bir görüntü adı (soyadı ve firstName) içerir.</td></tr><tr><td>containerId</td><td>Dize</td><td>Veri kaynağı için kapsayıcı kıymetinkimliği. Bu özellik Kapsayıcı türü için desteklenmez.</td></tr></table>

### <a name="common-non-singleton-annotation-properties"></a>Ortak singleton olmayan ek açıklama özellikleri
Bu özellikler, tüm singleton olmayan ek açıklama türleri (varlık başına birden fazla olması için izin verilen ek açıklamalar) için geçerlidir.

<table>
<tr><td><b>Özellik Adı</b></td><td><b>Veri Türü</b></td><td><b>Açıklamalar</b></td></tr>
<tr><td>anahtar</td><td>Dize</td><td>Kullanıcı, geçerli koleksiyondaki ek açıklamayı benzersiz olarak tanımlayan bir anahtar belirtti. Anahtar uzunluğu 256 karakteri geçemez.</td></tr>
</table>

### <a name="root-asset-types"></a>Kök varlık türleri
Kök varlık türleri, katalogda kaydedilebilen çeşitli veri varlıkları türlerini temsil eden türlerdir. Her kök türü için, görünümde yer alan varlık ve ek açıklamaları açıklayan bir görünüm vardır. GÖRÜNÜM adı, REST API kullanarak bir varlığı yayınlarken ilgili {view_name} url segmentinde kullanılmalıdır.

<table><tr><td><b>Varlık Türü (Görünüm adı)</b></td><td><b>Ek Özellikler</b></td><td><b>Veri Türü</b></td><td><b>İzin Verilen Ek Açıklamalar</b></td><td><b>Açıklamalar</b></td></tr><tr><td>Tablo ("tablolar")</td><td></td><td></td><td>Açıklama<p>Friendlyname<p>Etiket<p>Şema<p>Sütun Açıklaması<p>Sütun Etiketi<p> Uzman<p>Önizleme<p>Erişim Yönergesi<p>TabloDataProfili<p>SütunVeri Profili<p>SütunVeri Sınıflandırması<p>Belgeler<p></td><td>Tablo herhangi bir tablo verilerini temsil eder.  Örneğin: SQL Tablo, SQL Görünümü, Analiz Hizmetleri Tablo tablosu, Analiz Hizmetleri Çok boyutlu boyut, Oracle Table, vb.   </td></tr><tr><td>Ölçü ("ölçü")</td><td></td><td></td><td>Açıklama<p>Friendlyname<p>Etiket<p>Uzman<p>Erişim Yönergesi<p>Belgeler<p></td><td>Bu tür bir Çözümleme Hizmetleri ölçüsünü temsil eder.</td></tr><tr><td></td><td>Ölçü</td><td>Sütun</td><td></td><td>Ölçütaçıklayan meta veriler</td></tr><tr><td></td><td>hesaplanır </td><td>Boole</td><td></td><td>Ölçünün hesaplanmış olup olmadığını belirtir.</td></tr><tr><td></td><td>Measuregroup</td><td>Dize</td><td></td><td>Ölçü için fiziksel konteyner</td></tr><td>KPI ("kpis")</td><td></td><td></td><td>Açıklama<p>Friendlyname<p>Etiket<p>Uzman<p>Erişim Yönergesi<p>Belgeler</td><td></td></tr><tr><td></td><td>Measuregroup</td><td>Dize</td><td></td><td>Ölçü için fiziksel konteyner</td></tr><tr><td></td><td>goalExpression</td><td>Dize</td><td></td><td>Bir MDX sayısal ifade veya KPI hedef değerini döndüren bir hesaplama.</td></tr><tr><td></td><td>valueExpression</td><td>Dize</td><td></td><td>KPI'nin gerçek değerini döndüren bir MDX sayısal ifadesi.</td></tr><tr><td></td><td>statusExpression</td><td>Dize</td><td></td><td>Zaman içinde belirli bir noktada KPI durumunu temsil eden bir MDX ifadesi.</td></tr><tr><td></td><td>trendExpression</td><td>Dize</td><td></td><td>Zaman içinde KPI değerini değerlendiren bir MDX ifadesi. Eğilim, belirli bir iş bağlamında yararlı olan herhangi bir zaman tabanlı ölçüt olabilir.</td>
<tr><td>Rapor ("raporlar")</td><td></td><td></td><td>Açıklama<p>Friendlyname<p>Etiket<p>Uzman<p>Erişim Yönergesi<p>Belgeler<p></td><td>Bu tür bir SQL Server Reporting Services raporunu temsil eder </td></tr><tr><td></td><td>varlıkCreatedDate</td><td>Dize</td><td></td><td></td></tr><tr><td></td><td>varlıkCreatedBy</td><td>Dize</td><td></td><td></td></tr><tr><td></td><td>varlıkModiye Tarih</td><td>Dize</td><td></td><td></td></tr><tr><td></td><td>varlıkModifiye By</td><td>Dize</td><td></td><td></td></tr><tr><td>Konteyner ("konteynerler")</td><td></td><td></td><td>Açıklama<p>Friendlyname<p>Etiket<p>Uzman<p>Erişim Yönergesi<p>Belgeler<p></td><td>Bu tür, SQL veritabanı, Azure Blobs kapsayıcısı veya Çözümleme Hizmetleri modeli gibi diğer varlıkların kapsayıcılarını temsil eder.</td></tr></table>

### <a name="annotation-types"></a>Ek Açıklama türleri
Ek açıklama türleri, katalogdaki diğer türlere atanabilecek meta veri türlerini temsil emz.

<table>
<tr><td><b>Ek Açıklama Türü (İç içe görünüm adı)</b></td><td><b>Ek Özellikler</b></td><td><b>Veri Türü</b></td><td><b>Açıklamalar</b></td></tr>

<tr><td>Açıklama ("açıklamalar")</td><td></td><td></td><td>Bu özellik, bir varlığın açıklamasını içerir. Sistemin her kullanıcısı kendi açıklamasını ekleyebilir.  Açıklama nesnesini yalnızca bu kullanıcı edinebilir.  (Yöneticiler ve Varlık sahipleri Açıklama nesnesini silebilir, ancak düzenleyebilirler). Sistem, kullanıcıların açıklamalarını ayrı ayrı tutar.  Böylece her varlık üzerinde açıklamalar bir dizi (bir varlık hakkında bilgi katkıda bulunmuştur her kullanıcı için, muhtemelen bir veri kaynağından türetilen bilgi içeren ek olarak) vardır.</td></tr>
<tr><td></td><td>açıklama</td><td>string</td><td>Varlığın kısa bir açıklaması (2-3 satır)</td></tr>

<tr><td>Etiket ("etiketler")</td><td></td><td></td><td>Bu özellik, bir varlığın etiketini tanımlar. Sistemin her kullanıcısı bir varlık için birden çok etiket ekleyebilir.  Yalnızca Etiket nesnelerini oluşturan kullanıcı bunları dedebilir.  (Yöneticiler ve Varlık sahipleri Etiket nesnesini silebilir, ancak düzenleyebilir). Sistem, kullanıcıların etiketlerini ayrı ayrı tutar.  Böylece her varlık üzerinde Etiket nesneleri bir dizi vardır.</td></tr>
<tr><td></td><td>etiket</td><td>string</td><td>Varlığı açıklayan bir etiket.</td></tr>

<tr><td>FriendlyName ("friendlyName")</td><td></td><td></td><td>Bu özellik, bir varlığın dostu bir ad içerir. FriendlyName tek tonlu bir ek açıklamadır - bir varlığa yalnızca bir FriendlyName eklenebilir.  Yalnızca FriendlyName nesnesini oluşturan kullanıcı bu nesneyi dbulunabilir. (Yöneticiler ve Varlık sahipleri FriendlyName nesnesini silebilir, ancak düzenleyemezler). Sistem, kullanıcıların arkadaş çaları ayrı ayrı tutar.</td></tr>
<tr><td></td><td>Friendlyname</td><td>string</td><td>Varlığın dostça bir adı.</td></tr>

<tr><td>Şema ("şema")</td><td></td><td></td><td>Şema verilerin yapısını açıklar.  Öznitelik (sütun, öznitelik, alan, vb.) adlarını, türleri ve diğer meta verileri listeler.  Bu bilgilerin tümü veri kaynağından türetilmiştir.  Şema tek tonlu bir ek açıklamadır - bir varlık için yalnızca bir Şema eklenebilir.</td></tr>
<tr><td></td><td>sütunlar</td><td>Sütun[]</td><td>Sütun nesneleri dizisi. Sütunu veri kaynağından türetilen bilgilerle açıklarlar.</td></tr>

<tr><td>Sütun Açıklaması ("sütunAçıklamaları")</td><td></td><td></td><td>Bu özellik, bir sütun için bir açıklama içerir.  Sistemin her kullanıcısı birden çok sütun için kendi açıklamalarını ekleyebilir (sütun başına en fazla bir tane). Yalnızca ColumnDescription nesnelerini oluşturan kullanıcı bunları dedebilir.  (Yöneticiler ve Varlık sahipleri ColumnDescription nesnesini silebilir, ancak düzenleemezler). Sistem bu kullanıcının sütun açıklamalarını ayrı olarak korur.  Böylece her varlık üzerinde bir dizi Sütun Açıklaması nesnesi vardır (veri kaynağından türetilen bilgileri içeren bir ek olarak sütun hakkındaki bilgilerini katkıda bulunan her kullanıcı için her bir sütun başına bir).  ColumnDescription, senkronize edilebilsin diye Şemaya gevşek bir şekilde bağlıdır. Sütun Açıklaması şemada artık var olmayan bir sütunu açıklayabilir.  Bu eşitli lik ve şema tutmak için yazara kalmış.  Veri kaynağında sütun açıklama bilgileri de olabilir ve bunlar aracı çalıştırırken oluşturulacak ek Sütun Açıklaması nesneleridir.</td></tr>
<tr><td></td><td>columnName</td><td>Dize</td><td>Bu açıklamanın atıfta bulunduğu sütunun adı.</td></tr>
<tr><td></td><td>açıklama</td><td>Dize</td><td>sütunun kısa bir açıklaması (2-3 satır).</td></tr>

<tr><td>ColumnTag ("sütunEtiketler")</td><td></td><td></td><td>Bu özellik, bir sütun için bir etiket içerir. Sistemin her kullanıcısı belirli bir sütun için birden çok etiket ekleyebilir ve birden çok sütun için etiket ekleyebilir. Yalnızca ColumnTag nesnelerini oluşturan kullanıcı bunları dedebilir. (Yöneticiler ve Varlık sahipleri ColumnTag nesnesini silebilir, ancak düzenleemezler). Sistem bu kullanıcıların sütun etiketlerini ayrı ayrı tutar.  Böylece her varlık üzerinde ColumnTag nesnelerin bir dizi vardır.  ColumnTag, senkronize edilebilsin diye şemaya gevşek bir şekilde bağlıdır. ColumnTag, şemada artık var olmayan bir sütunu açıklayabilir.  Sütun etiketini ve şemasını senkronize tutmak yazara kılır.</td></tr>
<tr><td></td><td>columnName</td><td>Dize</td><td>Bu etiketin atıfta bulunduğu sütunun adı.</td></tr>
<tr><td></td><td>etiket</td><td>Dize</td><td>Sütunu açıklayan bir etiket.</td></tr>

<tr><td>Uzman ("uzmanlar")</td><td></td><td></td><td>Bu özellik, veri kümesinde uzman olarak kabul edilen bir kullanıcı içerir. Uzmanların açıklamaları listelerken UX'nin tepesine kadar olan görüşleri (açıklamaları) baloncuk. Her kullanıcı kendi uzmanlarını belirtebilir. Yalnızca bu kullanıcı uzmanlar nesnesini edinebilir. (Yöneticiler ve Varlık sahipleri Uzman nesneleri silebilir, ancak düzenleyebilir).</td></tr>
<tr><td></td><td>Uzman</td><td>Securityprincipal</td><td></td></tr>

<tr><td>Önizleme ("önizlemeler")</td><td></td><td></td><td>Önizleme, varlık için en iyi 20 veri satırının anlık görüntüsünü içerir. Önizleme yalnızca bazı varlık türleri için anlamlıdır (Tablo için anlamlıdır, ancak Ölçü için anlamlı değildir).</td></tr>
<tr><td></td><td>Önizleme</td><td>nesne[]</td><td>Bir sütunu temsil eden nesne dizisi.  Her nesnenin, satır için bu sütun için değeri olan bir sütuna özellik eşlemi vardır.</td></tr>

<tr><td>AccessInstruction ("accessInstructions")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Mimetype</td><td>string</td><td>İçeriğin pandomim türü.</td></tr>
<tr><td></td><td>content</td><td>string</td><td>Bu veri varlığına nasıl erişilene yönelik talimatlar. İçerik bir URL, bir e-posta adresi veya bir dizi yönerge olabilir.</td></tr>

<tr><td>TableDataProfile ("tableDataProfiles")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>numberOfRows</td></td><td>int</td><td>Veri kümesindeki satır sayısı</td></tr>
<tr><td></td><td>size</td><td>long</td><td>Veri kümesinin baytboyutu.  </td></tr>
<tr><td></td><td>şemaModizaman</td><td>string</td><td>Şema en son değiştirildiğinde</td></tr>
<tr><td></td><td>dataModifiedTime</td><td>string</td><td>Veri kümesi en son değiştirildiğinde (veriler eklendi, değiştirildi veya silindi)</td></tr>

<tr><td>SütunlarDataProfile ("sütundataProfiller")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>sütunlar</td></td><td>SütunDataProfili[]</td><td>Bir dizi sütun veri profili.</td></tr>

<tr><td>ColumnDataClassification ("columnDataClassifications")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>columnName</td><td>Dize</td><td>Bu sınıflandırmanın atıfta bulunduğu sütunun adı.</td></tr>
<tr><td></td><td>sınıflandırma</td><td>Dize</td><td>Bu sütundaki verilerin sınıflandırılması.</td></tr>

<tr><td>Dokümantasyon ("dokümantasyon")</td><td></td><td></td><td>Belirli bir varlığın bununla ilişkili yalnızca bir belgesi olabilir.</td></tr>
<tr><td></td><td>Mimetype</td><td>string</td><td>İçeriğin pandomim türü.</td></tr>
<tr><td></td><td>content</td><td>string</td><td>Dokümantasyon içeriği.</td></tr>

</table>

### <a name="common-types"></a>Yaygın türleri
Ortak türler özellikleri türleri olarak kullanılabilir, ancak Öğeler değildir.

<table>
<tr><td><b>Ortak Tür</b></td><td><b>Özellikler</b></td><td><b>Veri Türü</b></td><td><b>Açıklamalar</b></td></tr>
<tr><td>DataSourceInfo</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Sourcetype</td><td>string</td><td>Veri kaynağının türünü açıklar.  Örneğin: SQL Server, Oracle Database, vb.  </td></tr>
<tr><td></td><td>Nesnetürü</td><td>string</td><td>Veri kaynağındaki nesne türünü açıklar. Örneğin: Tablo, SQL Server için Görünüm.</td></tr>

<tr><td>DataSourceLocation</td><td></td><td></td><td></td></tr>
<tr><td></td><td>protokol</td><td>string</td><td>Gereklidir. Veri kaynağıyla iletişim kurmak için kullanılan bir protokolü açıklar. Örneğin: SQl Server için "tds", Oracle için "oracle" vb. Şu anda desteklenen protokollerin listesi için Veri kaynağı başvuru belirtimine - <a href="https://docs.microsoft.com/azure/data-catalog/data-catalog-dsr">DSL Yapısı'na</a> bakın.</td></tr>
<tr><td></td><td>adres</td><td>Sözlük&lt;dizesi, nesne&gt;</td><td>Gereklidir. Adres, başvurulan veri kaynağını tanımlamak için kullanılan protokole özgü bir veri kümesidir. Adres verileri belirli bir protokole yöneliktir, yani protokolü bilmeden anlamsızdır.</td></tr>
<tr><td></td><td>kimlik doğrulaması</td><td>string</td><td>İsteğe bağlı. Veri kaynağıyla iletişim kurmak için kullanılan kimlik doğrulama düzeni. Örneğin: pencereler, oauth, vb.</td></tr>
<tr><td></td><td>Connectionproperties</td><td>Sözlük&lt;dizesi, nesne&gt;</td><td>İsteğe bağlı. Bir veri kaynağına nasıl bağlanılacak hakkında ek bilgi.</td></tr>

<tr><td>Securityprincipal</td><td></td><td></td><td>Arka uç, yayımlama sırasında AAD'ye karşı sağlanan özelliklerin herhangi bir doğrulanmasını gerçekleştirmez.</td></tr>
<tr><td></td><td>Upn</td><td>string</td><td>Kullanıcının benzersiz e-posta adresi. ObjectId sağlanmazsa veya "lastRegisteredBy" özelliği bağlamında belirtilmelidir, aksi takdirde isteğe bağlıdır.</td></tr>
<tr><td></td><td>Objectıd</td><td>Guid</td><td>Kullanıcı veya güvenlik grubu AAD kimliği. İsteğe bağlı. Upn sağlanmamışsa belirtilmelidir, aksi takdirde isteğe bağlıdır.</td></tr>
<tr><td></td><td>firstName</td><td>string</td><td>Kullanıcının adı (görüntü amaçlı). İsteğe bağlı. Yalnızca "lastRegisteredBy" özelliği bağlamında geçerlidir. "Roller", "izinler" ve "uzmanlar" için güvenlik ilkesi sağlarken belirtilemez.</td></tr>
<tr><td></td><td>lastName</td><td>string</td><td>Kullanıcının soyadı (görüntüleme amacıyla). İsteğe bağlı. Yalnızca "lastRegisteredBy" özelliği bağlamında geçerlidir. "Roller", "izinler" ve "uzmanlar" için güvenlik ilkesi sağlarken belirtilemez.</td></tr>

<tr><td>Sütun</td><td></td><td></td><td></td></tr>
<tr><td></td><td>ad</td><td>string</td><td>Sütunun veya öznitelik adı.</td></tr>
<tr><td></td><td>type</td><td>string</td><td>sütunun veya öznitelik veri türü. İzin verilebilir türler varlığın veri kaynağıtürüne bağlıdır.  Yalnızca bir tür alt kümesi desteklenir.</td></tr>
<tr><td></td><td>Maxlength</td><td>int</td><td>Sütun veya öznitelik için izin verilen maksimum uzunluk. Veri kaynağından türetilmiştir. Yalnızca bazı kaynak türleri için geçerlidir.</td></tr>
<tr><td></td><td>Hassas</td><td>byte</td><td>Sütun veya öznitelik için kesinlik. Veri kaynağından türetilmiştir. Yalnızca bazı kaynak türleri için geçerlidir.</td></tr>
<tr><td></td><td>ısnullable</td><td>Boole</td><td>Sütunun null değeri olup olmadığı. Veri kaynağından türetilmiştir. Yalnızca bazı kaynak türleri için geçerlidir.</td></tr>
<tr><td></td><td>ifade</td><td>string</td><td>Değer hesaplanan bir sütunsa, bu alan değeri ifade eden ifadeyi içerir. Veri kaynağından türetilmiştir. Yalnızca bazı kaynak türleri için geçerlidir.</td></tr>

<tr><td>SütunVeri Profili</td><td></td><td></td><td></td></tr>
<tr><td></td><td>columnName </td><td>string</td><td>Sütunun adı</td></tr>
<tr><td></td><td>type </td><td>string</td><td>Sütunun türü</td></tr>
<tr><td></td><td>min </td><td>string</td><td>Veri kümesindeki minimum değer</td></tr>
<tr><td></td><td>max </td><td>string</td><td>Veri kümesindeki maksimum değer</td></tr>
<tr><td></td><td>Avg </td><td>double</td><td>Veri kümesindeki ortalama değer</td></tr>
<tr><td></td><td>Stdev </td><td>double</td><td>Veri kümesi için standart sapma</td></tr>
<tr><td></td><td>nullCount </td><td>int</td><td>Veri kümesindeki null değerlerin sayısı</td></tr>
<tr><td></td><td>Distinctcount  </td><td>int</td><td>Veri kümesindeki farklı değerlerin sayısı</td></tr>


</table>

## <a name="asset-identity"></a>Varlık kimliği
Azure Veri Kataloğu, Katalog içindeki varlığı adreslemek için kullanılan varlığın kimliğini oluşturmak için DataSourceLocation "dsl" özelliğinin "adres" özelliğinden "protokol" ve kimlik özelliklerini kullanır.
Örneğin, "tds" protokolü "sunucu", "veritabanı", "şema" ve "nesne" kimlik özelliklerine sahiptir. SQL Server Table Asset kimliğini oluşturmak için protokol ve kimlik özelliklerinin birleşimleri kullanılır.
Azure Veri Kataloğu, Veri kaynağı başvuru belirtiminde listelenen birkaç yerleşik veri kaynağı protokolü sağlar [- DSL Yapısı.](data-catalog-dsr.md)
Desteklenen protokoller kümesi programlı olarak genişletilebilir (Veri Kataloğu REST API başvurusuna bakın). Kataloğun yöneticileri özel veri kaynağı protokollerini kaydedebilir. Aşağıdaki tabloda özel bir protokol kaydetmek için gereken özellikleri açıklanmaktadır.

### <a name="custom-data-source-protocol-specification"></a>Özel veri kaynağı protokolü belirtimi
<table>
<tr><td><b>Tür</b></td><td><b>Özellikler</b></td><td><b>Veri Türü</b></td><td><b>Açıklamalar</b></td></tr>

<tr><td>DataSourceProtocol</td><td></td><td></td><td></td></tr>
<tr><td></td><td>ad alanı</td><td>string</td><td>Protokolün ad alanı. Ad alanı 1 ile 255 karakter uzunluğunda olmalı, noktayla ayrılmış bir veya daha fazla boş olmayan parça içermelidir (.). Her bölüm 1 ila 255 karakter uzunluğunda olmalı, bir harfle başlamalı ve yalnızca harf ve sayılar içermelidir.</td></tr>
<tr><td></td><td>ad</td><td>string</td><td>Protokolün adı. Ad 1 ile 255 karakter uzunluğunda olmalı, harfle başlamalı ve yalnızca harfler, sayılar ve tire (-) karakterini içermelidir.</td></tr>
<tr><td></td><td>identityProperties</td><td>DataSourceProtocolIdentityProperty[]</td><td>Kimlik özellikleri listesi, en az bir, ancak en fazla 20 özellikleri içermelidir. Örneğin: "sunucu", "veritabanı", "şema", "nesne" "tds" protokolünün kimlik özellikleridir.</td></tr>
<tr><td></td><td>identitySets</td><td>DataSourceProtocolIdentitySet[]</td><td>Kimlik kümelerinin listesi. Geçerli varlığın kimliğini temsil eden kimlik özellikleri kümelerini tanımlar. En az bir tane içermelidir, ancak en fazla 20 set içermelidir. Örneğin: {"server", "database", "schema" ve "object"} Sql Server Table varlığının kimliğini tanımlayan "tds" protokolü için ayarlanmış bir kimlik kümesidir.</td></tr>

<tr><td>DataSourceProtocolIdentityProperty</td><td></td><td></td><td></td></tr>
<tr><td></td><td>ad</td><td>string</td><td>Özelliğin adı. Ad 1 ila 100 karakter uzunluğunda olmalı, bir harfle başlamalı ve yalnızca harf ve sayılar içermelidir.</td></tr>
<tr><td></td><td>type</td><td>string</td><td>Özelliğin türü. Desteklenen değerler: "bool", boolean", "bayt", "guid", "int", "integer", "long", "string", "url"</td></tr>
<tr><td></td><td>Ignorecase</td><td>bool</td><td>Özellik değerini kullanırken servis talebinin göz ardı edilip edilmemesi gerektiğini gösterir. Yalnızca "string" türüne sahip özellikler için belirtilebilir. Varsayılan değer yanlıştır.</td></tr>
<tr><td></td><td>urlPathSegmentsIgnoreCase</td><td>bool[]</td><td>Url'nin yolunun her kesimi için servis talebinin yoksayılması gerekip gerekmediğini gösterir. Yalnızca "url" türüne sahip özellikler için belirtilebilir. Varsayılan değer [false]'dur.</td></tr>

<tr><td>DataSourceProtocolIdentitySet</td><td></td><td></td><td></td></tr>
<tr><td></td><td>ad</td><td>string</td><td>Kimlik kümesinin adı.</td></tr>
<tr><td></td><td>properties</td><td>dize[]</td><td>Bu kimlik kümesine dahil edilen kimlik özellikleri listesi. Yinelenenleri içeremez. Kimlik kümesi tarafından başvurulan her özellik protokolün "identityProperties" listesinde tanımlanmalıdır.</td></tr>

</table>

## <a name="roles-and-authorization"></a>Roller ve yetkilendirme
Microsoft Azure Veri Kataloğu, varlıklar ve ek açıklamalar la ilgili CRUD işlemleri için yetkilendirme özellikleri sağlar.

## <a name="key-concepts"></a>Önemli kavramlar
Azure Veri Kataloğu iki yetkilendirme mekanizması kullanır:

* Rol tabanlı yetkilendirme
* İzin tabanlı yetkilendirme

### <a name="roles"></a>Roller
Üç rol vardır: **Yönetici**, **Sahibi**ve **Katkıda Bulunan**.  Her rolün kapsamı ve hakları aşağıdaki tabloda özetlenmiştir.

<table><tr><td><b>Rol</b></td><td><b>Kapsam</b></td><td><b>Haklar</b></td></tr><tr><td>Yönetici</td><td>Katalog (Katalogdaki tüm varlıklar/ek açıklamalar)</td><td>Oku Sil Görünüm Rolleri

ChangeOwnership ChangeGörünüry ViewPermissions</td></tr><tr><td>Sahip</td><td>Her varlık (kök madde)</td><td>Oku Sil Görünüm Rolleri

ChangeOwnership ChangeGörünüry ViewPermissions</td></tr><tr><td>Katılımcı</td><td>Her bir varlık ve ek açıklama</td><td>Oku Update ViewRoles Notunu Sil: Öğedeki Okuma hakkı Katılımcı'dan iptal edilirse tüm haklar iptal edilir</td></tr></table>

> [!NOTE]
> **Oku**, **Güncelleştir**, **Sil**, **ViewRoles** hakları herhangi bir öğe (varlık veya ek açıklama) için geçerliyken **TakeOwnership**, **ChangeOwnership**, **ChangeVisibility**, **ViewPermissions** yalnızca kök varlık için geçerlidir.
> 
> **Silme** hakkı bir öğe ve altındaki herhangi bir alt öğe veya tek öğe için geçerlidir. Örneğin, bir kıymeti silmek de o varlık için tüm ek açıklamaları siler.
> 
> 

### <a name="permissions"></a>İzinler
İzin, erişim denetimi girişlerinin listesi dir. Her erişim denetimi girişi bir güvenlik ilkesine bir hak kümesi atar. İzinler yalnızca bir varlıkta (yani kök madde) belirtilebilir ve kıymet ve herhangi bir alt öğe için geçerli olabilir.

Azure **Veri Kataloğu** önizlemesi sırasında, bir varlığın görünürlüğünü kısıtlayan senaryoyu etkinleştirmek için izinler listesinde yalnızca **Okuma** hakkı desteklenir.

Görünürlük izinlerde ilkeler kümesiyle sınırlı olmadığı sürece, varsayılan olarak kimlik doğrulaması yapılan herhangi bir kullanıcı katalogdaki herhangi bir öğe için **Okuma** hakkına sahiptir.

## <a name="rest-api"></a>REST API
**PUT** ve **POST** görünüm öğesi istekleri rolleri ve izinleri denetlemek için kullanılabilir: madde yüküne ek olarak, iki sistem özelliği belirtilen **roller** ve **izinler**olabilir.

> [!NOTE]
> yalnızca bir kök öğe için geçerli **olan izinler.**
> 
> **Yalnızca** bir kök öğe için geçerli olan sahip rolü.
> 
> Varsayılan olarak, bir öğe katalogda **oluşturulduğunda, Katılımcısı** şu anda kimlik doğrulaması yapılan kullanıcıya ayarlanır. Öğe herkes tarafından güncellenebilir olması gerekiyorsa, **Katılımcı,** öğe ilk yayımlandığında &lt; **roller** özelliğindeki Herkes&gt; özel güvenlik ilkesine ayarlanmalıdır (aşağıdaki örneğe bakın). **Katılımcı** değiştirilemez ve bir öğenin ömrü boyunca aynı kalır **(Yönetici** veya **Sahip** bile **Katılımcıyı**değiştirme hakkına sahip değildir). **Katılımcının** açık ayarı için desteklenen tek &lt;&gt;değer Herkes'tir : **Katılımcı** yalnızca &lt;bir&gt;öğeyi veya Herkes'i oluşturan bir kullanıcı olabilir.
> 
> 

### <a name="examples"></a>Örnekler
**Bir &lt;öğeyi&gt; yayınlarken Herkese Katkıda Bulunanı Ayarlayın.**
Özel güvenlik &lt;&gt; müdürü Herkes "00000000-0000-0000-0000-0000000000201" objectId vardır.
  **POST** https:\//api.azuredatacatalog.com/catalogs/default/views/tables/?api-version=2016-03-30

> [!NOTE]
> Bazı HTTP istemci uygulamaları, sunucudan gelen 302'ye yanıt olarak istekleri otomatik olarak yeniden yayımlayabilir, ancak genellikle Yetkilendirme üstbilgilerini istekten ayırabilir. Yetkilendirme üstbilgisinin Azure Veri Kataloğu'na istekte bulunmak için gerekli olduğundan, bir isteği Azure Veri Kataloğu tarafından belirtilen yeniden yönlendirme konumuna yeniden aktarırken Yetkilendirme üstbilgisinin hala sağlandığından emin olmalısınız. Aşağıdaki örnek kod .NET HttpWebRequest nesnesini kullanarak bunu gösterir.
> 
> 

**Gövde**
```json
    {
        "roles": [
            {
                "role": "Contributor",
                "members": [
                    {
                        "objectId": "00000000-0000-0000-0000-000000000201"
                    }
                ]
            }
        ]
    }
```

  **Sahipleri atama ve varolan bir kök öğeiçin görünürlüğü kısıtlamak**: **PUT** https:\//api.azuredatacatalog.com/catalogs/default/views/tables/042297b0...1be45ecd462a?api-version=2016-03-30

```json
    {
        "roles": [
            {
                "role": "Owner",
                "members": [
                    {
                        "objectId": "c4159539-846a-45af-bdfb-58efd3772b43",
                        "upn": "user1@contoso.com"
                    },
                    {
                        "objectId": "fdabd95b-7c56-47d6-a6ba-a7c5f264533f",
                        "upn": "user2@contoso.com"
                    }
                ]
            }
        ],
        "permissions": [
            {
                "principal": {
                    "objectId": "27b9a0eb-bb71-4297-9f1f-c462dab7192a",
                    "upn": "user3@contoso.com"
                },
                "rights": [
                    {
                        "right": "Read"
                    }
                ]
            },
            {
                "principal": {
                    "objectId": "4c8bc8ce-225c-4fcf-b09a-047030baab31",
                    "upn": "user4@contoso.com"
                },
                "rights": [
                    {
                        "right": "Read"
                    }
                ]
            }
        ]
    }
```

> [!NOTE]
> PUT'ta gövdede bir madde yükü belirtmek gerekmez: PUT yalnızca rolleri ve/veya izinleri güncelleştirmek için kullanılabilir.
> 
