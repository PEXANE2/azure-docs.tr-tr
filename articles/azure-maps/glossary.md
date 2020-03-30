---
title: Azure Haritalar Sözlüğü | Microsoft Dokümanlar
description: Azure Haritalar, Konum Tabanlı Hizmetler ve GIS ile ilişkili yaygın olarak kullanılan terimler sözlüğü.
author: rbrundritt
ms.author: richbrun
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: c9d5c2efc14dec5beee0eaf81a4f628421ef3d49
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77657053"
---
# <a name="glossary"></a>Sözlük

Aşağıdaki listede Azure Haritalar hizmetleriyle kullanılan yaygın sözcükler açıklanmaktadır.

## <a name="a"></a>A

<a name="address-validation"></a>**Adres doğrulama**: Bir adresin varlığını doğrulama işlemidir.

<a name="advanced-routing"></a>**Gelişmiş yönlendirme**: Yol yönlendirme verilerini kullanarak önceden işlemler gerçekleştiren hizmetler topluluğu; ulaşılabilir aralıkların (isochrones), mesafe matrislerinin ve toplu iş rota isteklerinin hesaplanması gibi.

<a name="aerial-imagery"></a>**Havadan görüntüler**: [Bkz. Uydu görüntüleri](#satellite-imagery). 

<a name="along-a-route-search"></a>**Yol araması :** Belirli bir sapma süresi içinde veya rota yolundan uzaklık içinde veri arayan uzamsal bir sorgu.

<a name="altitude"></a>**Yükseklik**: Referans yüzeyinin üzerindeki bir noktanın yüksekliği veya dikey yüksekliği. Yükseklik ölçümleri, ortalama deniz seviyesi gibi belirli bir referans datum'a dayanır. Ayrıca yükseklik bakın.

<a name="ambiguous"></a>**Belirsiz**: Belirli bir öznitelik için bir nesne uygun şekilde iki veya daha fazla değer atanabilir zaman var olan veri sınıflandırmasında belirsizlik durumu. Örneğin, "CA" coğrafi kodlama, iki belirsiz sonuçlar döndürülür: "Kanada" ve "California". "CA", sırasıyla "Kanada" ve "California" için bir ülke ve eyalet kodudur. 

<a name="annotation"></a>**Ek Açıklama**: Kullanıcıya bilgi sağlamak için haritada görüntülenen metin veya grafikler. Ek açıklama belirli bir harita varlığını tanımlayabilir veya açıklayabilir, haritadaki bir alan hakkında genel bilgi sağlayabilir veya haritanın kendisi hakkında bilgi sağlayabilir.

<a name="antimeridian"></a>**Antimeridian**: 180.<sup>th</sup> Meridian olarak da bilinir. Bu noktada -180 derece ve 180 derece boylam buluşuyor. Ki bu da dünya üzerindeki ana meridyenin tam tersi.

<a name="application-programming-interface-api"></a>**Uygulama Programlama Arabirimi (API)**: Geliştiricilerin uygulama oluşturmasına olanak tanıyan belirtim.

<a name="api-key"></a>**API tuşu**: [Bkz. Paylaşılan anahtar kimlik doğrulaması.](#shared-key-authentication)

<a name="area-of-interest-aoi"></a>**İlgi Alanı (AOI)**: Bir harita veya veritabanı üretimi için odak alanını tanımlamak için kullanılan kapsam.

<a name="asset-tracking"></a>**Varlık takibi**: Bir kişi, araç veya başka bir nesne gibi bir varlığın konumunu izleme işlemidir.

<a name="asynchronous-request"></a>**Eşsenkron isteği**: Bağlantıyı açan ve sunucuya asynchronous isteği için tanımlayıcı döndüren bir istekte bulunan ve sonra bağlantıyı kapatan bir HTTP isteği. Sunucu isteği işlemeye devam ediyor ve kullanıcı tanımlayıcıyı kullanarak durumu denetleyebilir. İstek işleme tamamlandığında, kullanıcı yanıtı karşıdan yükleyebilir. Bu istek türü, uzun süren işlemler için kullanılır.

<a name="autocomplete"></a>**Otomatik Tamamlama**: Bir uygulamada, kullanıcının yazdığı bir sözcüğün geri kalanını öngören bir özelliktir. 

<a name="autosuggest"></a>**Autosuggest**: Kullanıcının yazdıklarına dair mantıksal olasılıkları öngören bir uygulamada bir özelliktir.

<a name="azure-location-based-services-lbs"></a>**Azure Konum Tabanlı Hizmetler (LBS)**: Önizlemede yken Azure Haritalar'ın eski adı.

<a name="azure-active-directory"></a>**Azure Etkin Dizin (Azure AD)**: Azure AD, Microsoft'un bulut tabanlı kimlik ve erişim yönetimi hizmetidir. Azure Haritalar Azure AD tümleştirmesi şu anda tüm Azure Haritalar API'ları için önizlemede kullanılabilir. Azure AD, Azure Haritalar kaynaklarına ince grenli erişim sağlamak için rol tabanlı erişim denetimini (RBAC) destekler. Azure Haritalar Azure AD entegrasyonu hakkında daha fazla bilgi edinmek için [Azure Haritalar ve Azure AD](azure-maps-authentication.md) ve Azure [Haritalar'da kimlik doğrulamayı yönet'e](how-to-manage-authentication.md)bakın.

<a name="azure-maps-key"></a>**Azure Haritalar tuşu**: [Bkz. Paylaşılan anahtar kimlik doğrulaması.](#shared-key-authentication)

## <a name="b"></a>B

<a name="base-map"></a>**Temel harita**: Yol, yer işaretleri ve siyasi sınırlar gibi arka plan referans bilgilerini görüntüleyen harita uygulamasının bir bölümü.

<a name="batch-request"></a>**Toplu talep**: Birden çok isteği tek bir istekte birleştirme işlemidir.

<a name="bearing"></a>**Rulman**: Bir noktanın başka bir noktaya göre yatay yönü. Bu, saat yönünde 0 dereceden 360 dereceye kadar kuzeye göre bir açı olarak ifade edilir. 

<a name="boundary"></a>**Sınır**: Ülkeler/bölgeler, bölgeler ve mülkler gibi komşu siyasi varlıkları ayıran çizgi veya çokgen. Sınır, nehirler, dağlar veya duvarlar gibi fiziksel özellikleri takip eden veya izlemeyen bir çizgidir.

<a name="bounds"></a>**Ciltler**: [Bkz. Sınırlayıcı kutu](#bounding-box).

<a name="bounding-box"></a>**Sınırlayıcı kutu**: Haritaüzerinde dikdörtgen bir alanı temsil etmek için kullanılan koordinat lar kümesidir. 

## <a name="c"></a>C

<a name="cadastre"></a>**Kadastro**: Tescilli arsa ve gayrimenkul kaydı. Ayrıca Bakınız [Parsel](#parcel).

<a name="camera"></a>**Kamera**: Etkileşimli harita kontrolü bağlamında, bir kamera harita görüş alanını tanımlar. Kameranın görüş açısı çeşitli harita parametrelerine göre belirlenir: merkez, zum seviyesi, eğim, rulman. 

<a name="centroid"></a>**Centroid**: Bir özelliğin geometrik merkezi. Bir çizginin merkezci noktası orta noktası olurken, poligonun merkezi alanın merkezi olacaktır.

<a name="choropleth-map"></a>**Koroplet haritası**: İstatistiksel bir değişkenin ölçümü ile orantılı olarak alanların gölgelendiği tematik harita. Bu istatistiksel değişken haritada görüntülenir. Örneğin, her ABD devletinin sınırını diğer tüm devletlere göre göre göre boyama.

<a name="concave-hull"></a>**İçbükey gövde**: Belirtilen veri kümesindeki tüm şekilleri içine alan olası bir içbükey geometriyi temsil eden şekildir. Oluşturulan şekil plastik şal ile veri sarma ve daha sonra ısıtma benzer, böylece diğer veri noktalarıdoğru mağara noktaları arasında büyük açıklıklar neden.

<a name="consumption-model"></a>**Tüketim modeli**: Bir aracın yakıt veya elektrik tüketme oranını tanımlayan bilgidir. Ayrıca [tüketim modeli belgelerine](consumption-model.md)bakın.

<a name="control"></a>**Denetim**: Arabirim için bir davranış kümesini tanımlayan grafik kullanıcı arabiriminden oluşan bağımsız veya yeniden kullanılabilir bir bileşendir. Örneğin, bir harita denetimi, genellikle kullanıcı arabiriminin etkileşimli bir harita yükleyen bölümüdür.

<a name="convex-hull"></a>**Eksveks gövde**: Konveks gövde, belirtilen veri kümesindeki tüm şekilleri içine alan minimum konveks geometrisini temsil eden bir şekildir. Oluşturulan şekil, veri kümesinin etrafında elastik bir bant sarma benzer.

<a name="coordinate"></a>**Koordinat**: Haritadaki bir konumu temsil etmek için kullanılan boylam ve enlem değerlerinden oluşur.

<a name="coordinate-system"></a>**Koordinat sistemi**: Uzaydaki noktaların konumlarını iki veya üç boyutlu olarak tanımlamak için kullanılan bir referans çerçevesidir.

<a name="country-code"></a>**Ülke kodu**: ISO standardına dayalı bir ülke/bölge için benzersiz bir tanımlayıcıdır. ISO2, bir ülke için iki karakterli bir koddur (örneğin, ABD), ISO3 üç karakterli bir kodu (örneğin, ABD) temsil eder.

<a name="country-subdivision"></a>**Ülke alt bölümü**: Bir ülke/bölgenin, genellikle bir devlet veya bölge olarak bilinen birinci düzey bir alt bölümüdür.

<a name="country-secondary-subdivision"></a>**Ülke ikincil alt bölümü**: Bir ülkenin/bölgenin ikinci düzey alt bölümüdür.

<a name="country-tertiary-subdivision"></a>**Ülke üçüncül alt bölümü**: Bir ülkenin/bölgenin üçüncü düzey bir alt bölümü, genellikle koğuş gibi adlandırılmış bir alan.

<a name="cross-street"></a>**Cross street**: İki veya daha fazla caddenin kesiştiği nokta.

<a name="cylindrical-projection"></a>**Silindirik projeksiyon**: Noktaları küresel veya küreden teğet veya secant silindire dönüştüren projeksiyondur. Silindir daha sonra yukarıdan aşağıya doğru dilimlenir ve bir düzlemiçine düzleştirilmiş.

## <a name="d"></a>D

<a name="datum"></a>**Datum**: Bir ölçüm sisteminin referans özellikleri, bir yüzey üzerinde koordinat konumları sistemi (yatay bir datum) veya bir yüzeyin üstünde veya altında yükseklikler (dikey datum).

<a name="dbf-file"></a>**DBF dosyası**: Shapefiles (SHP) ile birlikte kullanılan veritabanı dosya biçimidir.

<a name="degree-minutes-seconds-dms"></a>**Derece Dakika Saniye (DMS)**: Enlem ve boylam tanımlamak için ölçü birimi. Bir derece bir dairenin<sup>th</sup> 1/360'ıdır. Bir derece daha 60 dakikaya bölünür ve bir dakika 60 saniyeye bölünür.

<a name="delaunay-triangulation"></a>**Delaunay nirengi**: Noktaların bir veri kümesinden bitişik, örtüşen üçgenlerden oluşan bir ağ oluşturmak için yapılan bir tekniktir. Her üçgenin çevreleme çemberi, iç bölümündeki veri kümesinden hiçbir nokta içermez.

<a name="demographics"></a>**Demografi**: İnsan nüfusunun istatistiksel özellikleri (yaş, doğum oranı ve gelir gibi).

<a name="destination"></a>**Hedef**: Bir kişinin seyahat ettiği bitiş noktası veya konumu.

<a name="digital-elevation-model-dem"></a>**Dijital Yükseklik Modeli (DEM)**: Bir yüzeyle ilgili yükseklik değerlerinin, ortak bir datum kullanılarak düzenli aralıklarla bir alan üzerinde yakalanan veri kümesidir. DEM'ler genellikle arazi kabartmasını temsil etmek için kullanılır.

<a name="dijkstra's-algorithm"></a>**Dijkstra algoritması**: İki nokta arasındaki en kısa yolu bulmak için bir ağın bağlantısını inceleyen algoritma.

<a name="distance-matrix"></a>**Mesafe matrisi**: Bir dizi kaynak ve hedef arasındaki seyahat süresi ve mesafe bilgilerini içeren matris. 

## <a name="e"></a>E

<a name="elevation"></a>**Yükseklik**: Bir noktanın veya bir nesnenin referans yüzeyinin veya datumun üstündeveya altında dikey uzaklığı. Genellikle, referans yüzeyi ortalama deniz seviyesidir. Yükseklik genellikle arazinin dikey yüksekliği anlamına gelir.

<a name="envelope"></a>**Zarf**: [Bkz. Sınırlayıcı kutu](#bounding-box).

<a name="extended-postal-code"></a>**Genişletilmiş posta kodu**: Ek bilgiler içerebilecek posta kodu. Örneğin, ABD'de posta kodları beş basamaklıdır. Ancak, zip+4 olarak bilinen genişletilmiş posta kodu dört ek basamak içerir. Bu ek basamaklar, şehir bloğu, bir daire grubu veya posta kutusu gibi beş basamaklı teslimat alanı içindeki bir coğrafi kesimi tanımlamak için kullanılır. Coğrafi segmenti bilmek, verimli posta sıralama ve teslimatına yardımcı olur.

<a name="extent"></a>**Kapsam**: [Bkz. Sınırlayıcı kutu](#bounding-box).

## <a name="f"></a>F

<a name="federated-authentication"></a>**Federe kimlik doğrulama**: Birden çok web ve mobil uygulamada tek bir oturum açma/kimlik doğrulama mekanizmasının kullanılmasını sağlayan bir kimlik doğrulama yöntemidir. 

<a name="feature"></a>**Özellik**: Geometriile ek meta veri bilgilerini birleştiren nesnedir. 

<a name="feature-collection"></a>**Özellik koleksiyonu**: Özellik nesnelerinin koleksiyonu.

<a name="find-along-route"></a>**Yol boyunca bul**: Belirli bir sapma süresi içinde veya rota yolundan uzak olan verileri arayan uzamsal sorgu.

<a name="find-nearby"></a>**Yakındakibul**: Bir noktadan sabit bir düz çizgi mesafesini (karga uçarken) arayan uzamsal bir sorgu.

<a name="fleet-management"></a>**Filo yönetimi**: Otomobil, kamyon, gemi ve uçak gibi ticari araçların yönetimi. Filo yönetimi, araç finansmanı, bakım, telematik (izleme ve teşhis) gibi işlevlerin yanı sıra sürücü, hız, yakıt ve sağlık ve güvenlik yönetimi gibi çeşitli işlevleri içerebilir. Filo Yönetimi, işlerinde taşımacılık konusunda güvenen şirketler tarafından kullanılan bir süreçtir. Şirketler riskleri en aza indirmek ve genel ulaşım ve personel maliyetlerini azaltırken, aynı yandan da hükümet mevzuatına uygunluğu sağlamak istiyorlar.

<a name="free-flow-speed"></a>**Serbest akış hızı**: İdeal koşullarda beklenen serbest akış hızı. Genellikle hız limiti.

<a name="free-form-address"></a>**Serbest form adresi**: Tek bir metin satırı olarak temsil edilen tam adres.

<a name="fuzzy-search"></a>**Bulanık arama**: Bir adres veya ilgi çekici olabilecek serbest biçimde metin dizesini alan arama. 

## <a name="g"></a>G

<a name="geocode"></a>**Geocode**: Bu konumu haritaüzerinde görüntülemek için kullanılabilecek bir koordinata dönüştürülmüş adres veya konum. 

<a name="geocoding"></a>**Geocoding**: İleri coğrafi kodlama olarak da bilinen, konum verilerinin adresini koordinatlara dönüştürme işlemidir. 

<a name="geodesic-path"></a>**Jeodezik yol**: Kavisli bir yüzeydeki iki nokta arasındaki en kısa yoldur. Azure Haritalar'da işlendiğinde, mercator projeksiyonu nedeniyle bu yol eğri bir çizgi olarak görünür.

<a name="geofence"></a>**Geofence**: Bir aygıt bölgeye girdiğinde veya var olduğunda olayları tetiklemek için kullanılabilecek tanımlanmış bir coğrafi bölgedir.

<a name="geojson"></a>**GeoJSON**: Noktalar, çizgiler ve çokgenler gibi coğrafi vektör verilerini depolamak için kullanılan JSON tabanlı yaygın bir dosya biçimidir. **Not**: Azure Haritalar [burada belgelenen](extend-geojson.md)GeoJSON genişletilmiş bir sürümünü kullanır.

<a name="geometry"></a>**Geometri**: Nokta, çizgi veya çokgen gibi uzamsal bir nesneyi temsil eder.

<a name="geometrycollection"></a>**GeometriKoleksiyonu**: Geometri nesneleri topluluğu.

<a name="geopol"></a>**GeoPol**: Tartışmalı sınırlar ve yer adları gibi jeopolitik açıdan hassas verileri ifade eder.

<a name="georeference"></a>**Georeference**: Coğrafi verileri veya görüntüleri bilinen bir koordinat sistemine hizalama işlemidir. Bu işlem, verilerin kayması, döndürülmesi, ölçeklenmesi veya eğriltilmesinden oluşabilir.

<a name="georss"></a>**GeoRSS**: RSS akışlarına uzamsal veri eklemek için bir XML uzantısı.

<a name="gis"></a>**GIS**: "Coğrafi Bilgi Sistemi" kısaltması. Haritalama endüstrisini tanımlamak için kullanılan yaygın bir terimdir.

<a name="gml"></a>**GML**: Coğrafya Biçimlendirme Dili olarak da bilinir. Uzamsal verileri depolamak için bir XML dosya uzantısı.

<a name="gps"></a>**GPS**: Küresel Konumlandırma Sistemi olarak da bilinen, yeryüzündeki bir cihaz konumunu belirlemek için kullanılan uydu sistemidir. Yörüngedeki uydular, dünyanın herhangi bir yerindeki bir GPS alıcısının trilaterasyon yoluyla kendi konumunu hesaplamasını sağlayan sinyaller gönderiyorlar.

<a name="gpx"></a>**GPX**: GPS eXchange formatı olarak da bilinen, GPS aygıtlarından oluşturulan bir XML dosya biçimidir.  

<a name="great-circle-distance"></a>**Büyük daire mesafesi**: Kürenin yüzeyindeki iki nokta arasındaki en kısa mesafedir.

<a name="greenwich-mean-time-gmt"></a>**Greenwich Ortalama Saati (GMT)**: Greenwich, İngiltere'deki Kraliyet Gözlemevi'nden geçen prime meridyenin zamanı.

<a name="guid"></a>**GUID**: Küresel olarak benzersiz bir tanımlayıcı. Bir arabirimi, sınıfı, tür kitaplığını, bileşen kategorisini veya kaydı benzersiz olarak tanımlamak için kullanılan dize.

## <a name="h"></a>H

<a name="haversine-formula"></a>**Haversine formülü**: Bir küre üzerindeki iki nokta arasındaki büyük daire uzaklığı hesaplamak için kullanılan ortak bir denklemdir.

<a name="hd-maps"></a>**HD haritalar**: High Definition Maps olarak da bilinen, şerit işaretleri, tabela lar ve otonom sürüş için gerekli yön ışıkları gibi yüksek doğrulukta yol ağı bilgilerinden oluşur.

<a name="heading"></a>**Başlık**: Bir şeyin işaret eden veya bakan yönü. Ayrıca bakınız [Rulman](#heading).

<a name="heatmap"></a>**Isı Haritası**: Belirli bir alandaki noktaların yoğunluğunu temsil eden bir renk aralığının bulunduğu veri görselleştirmesidir. Ayrıca bkz.

<a name="hybrid-imagery"></a>**Hibrit görüntüler**: Üzerine yol verileri ve etiketleri eklenmiştir uydu veya havadan görüntüler.

## <a name="i"></a>I

<a name="iana"></a>**IANA**: İnternet Atanan Numaralar Kurumu'nun kısaltması. Genel IP adresi tahsisini denetleyen kar amacı gütmeyen bir grup.

<a name="isochrone"></a>**Isochrone**: Bir izokrom, belirli bir konumdan herhangi bir yönde bir ulaşım modu için belirli bir süre içinde seyahat edebilirsiniz alanı tanımlar. Ayrıca bakınız [Ulaşılabilir Aralığı](#reachable-range).

<a name="isodistance"></a>**Izodistance**: Bir konum göz önüne alındığında, bir izokrom herhangi bir yönde bir ulaşım modu için belirli bir mesafe içinde seyahat edebilirsiniz alanı tanımlar. Ayrıca bakınız [Ulaşılabilir Aralığı](#reachable-range).

## <a name="k"></a>K

<a name="kml"></a>**KML**: Anahtar Deliği İşaretleme Dili olarak da bilinen, noktalar, çizgiler ve çokgenler gibi coğrafi vektör verilerini depolamak için ortak bir XML dosya biçimidir. 

## <a name="l"></a>L

<a name="landsat"></a>**Landsat**: NASA tarafından geliştirilen ve karanın görüntülerini toplayan çok spektral, dünya yörüngesi uyduları. Bu görüntüler tarım, ormancılık ve kartografi gibi birçok endüstride kullanılmaktadır.

<a name="latitude"></a>**Enlem**: Kuzey veya güney yönünde ekvatordan derece cinsinden ölçülen açısal uzaklık.

<a name="level-of-detail"></a>**Ayrıntı düzeyi**: Bkz. Yakınlaştırma düzeyi.

<a name="lidar"></a>**Lidar**: Işık algılama ve değişen kısaltma. Yansıtıcı yüzeylere mesafeleri ölçmek için lazerler kullanan uzaktan algılama tekniği.

<a name="linear-interpolation"></a>**Doğrusal enterpolasyon**: Bilinen değerler arasındaki doğrusal uzaklığı kullanarak bilinmeyen bir değerin tahminidir.

<a name="linestring"></a>**LineString**: Bir çizgiyi temsil etmek için kullanılan geometri. Polyline olarak da bilinir. 

<a name="localization"></a>**Yerelleştirme**: Farklı dil ve kültürlerin desteklenmesi.

<a name="logistics"></a>**Lojistik**: İnsanları, araçları, malzemeleri veya varlıkları koordineli bir şekilde taşıma sürecidir.

<a name="longitude"></a>**Boylam**: Doğu veya batı yönünde asal meridyen den derece cinsinden ölçülen açısal uzaklıktır.

## <a name="m"></a>M

<a name="map-tile"></a>**Harita Döşemesi**: Harita tuvalinin bir bölümlebölünmesini temsil eden dikdörtgen bir görüntüdür. Daha fazla bilgi için [Yakınlaştırma düzeyleri ve döşeme ızgarası belgelerine](zoom-levels-and-tile-grid.md)bakın.

<a name="marker"></a>**İşaretçi**: İğne veya rende olarak da bilinen simge, haritadaki nokta konumunu temsil eden bir simgedir.

<a name="mercator-projection"></a>**Mercator projeksiyonu**: Meridyenlerle açıları muhafaza eden düz segmentler olarak rhumb çizgileri olarak bilinen sabit rota çizgilerini temsil etme yeteneği nden dolayı deniz amaçları için standart harita projeksiyonu haline gelen silindirik harita projeksiyonudur. Tüm düz harita projeksiyonları, Dünya yüzeyinin gerçek düzeniyle karşılaştırıldığında haritanın şekillerini veya boyutlarını bozar. Mercator projeksiyonu ekvatordan uzak alanları abartır, diğer kutuplara yaklaştıkça daha küçük alanlar haritada daha büyük görünür. 

<a name="multilinestring"></a>**MultiLineString**: LineString nesnelerinin bir koleksiyonunu temsil eden bir geometri. 

<a name="multipoint"></a>**MultiPoint**: Nokta nesnelerinin bir koleksiyonunu temsil eden bir geometri.

<a name="multipolygon"></a>**MultiPolygon**: Çokgen nesnelerinin bir koleksiyonunu temsil eden geometri. Örneğin, Hawaii sınırını göstermek için, her ada bir çokgen ile özetlenen olacaktır. Böylece, Hawaii sınırı böylece bir MultiPolygon olacaktır.

<a name="municipality"></a>**Belediye**: Bir şehir veya kasaba. 

<a name="municipality-subdivision"></a>**Belediye alt bölümü**: Bir belediyenin mahalle veya "şehir merkezi" gibi yerel alan adı gibi bir alt bölümüdür.

## <a name="n"></a>N

<a name="navigation-bar"></a>**Gezinme çubuğu**: Yakınlaştırma düzeyini, eğimi, döndürmeyi ve taban harita katmanını değiştirmek için kullanılan harita üzerindeki kontrol kümesidir.

<a name="nearby-search"></a>**Yakındaki arama**: Bir noktadan sabit bir düz çizgi mesafesini (karga uçarken) arayan uzamsal bir sorgu.

<a name="neutral-ground-truth"></a>**Nötr Yer Gerçeği**: Etiketleri temsil ettiği bölgenin resmi dilinde ve varsa yerel komut dosyalarında işleyen bir haritadır.

## <a name="o"></a>O

<a name="origin"></a>**Başlangıç**: Kullanıcının bulunduğu başlangıç noktası veya konumu.

## <a name="p"></a>P

<a name="panning"></a>**Kaydırma**: Sabit bir yakınlaştırma düzeyini korurken haritayı herhangi bir yöne taşıma işlemidir.

<a name="parcel"></a>**Parsel**: Arsa veya mülkiyet sınırının arsası.

<a name="pitch"></a>**Pitch**: Haritanın eğim miktarı, 0'ın doğrudan haritaya baktığı dikeye göredir.

<a name="point"></a>**Nokta**: Haritada tek bir konumu temsil eden geometri. 

<a name="points-of-interest-poi"></a>**İlgi çekici noktalar (POI)**: Bir işletme, dönüm noktası veya ortak ilgi çekici yer.

<a name="polygon"></a>**Çokgen**: Haritadaki bir alanı temsil eden katı geometri. 

<a name="polyline"></a>**Polyline**: Bir çizgiyi temsil etmek için kullanılan geometri. LineString olarak da bilinir. 

<a name="position"></a>**Konum**: Bir noktanın boylam, enlem ve yükseklik (x,y,z koordinatları).

<a name="post-code"></a>**Posta kodu**: [Bkz. Posta kodu](#postal-code).

<a name="postal-code"></a>**Posta kodu**: Belirli bir biçimde harf veya sayı veya her ikisi dizisi. Posta kodu, posta teslimini kolaylaştırmak için coğrafi bölgeleri bölgelere bölmek için bir ülkenin/bölgenin posta servisi tarafından kullanılır.

<a name="primary-key"></a>**Birincil anahtar**: Azure Haritalar için sağlanan iki abonelik anahtarından ilki anahtar kimlik doğrulamasını paylaştı. Bkz. [Paylaşılan anahtar kimlik doğrulaması.](#shared-key-authentication)

<a name="prime-meridian"></a>**Asal meridyen**: 0 derece boylamını temsil eden bir boylam çizgisidir. Genellikle boylam değerleri 180 dereceye kadar batı yönünde seyahat ederken azalır ve doğu yönünde -180 dereceye doğru seyahat ederken artar. 

<a name="prj"></a>**PRJ**: Genellikle veri kümesinin içinde bulunduğu öngörülen koordinat sistemi hakkında bilgi içeren shapefile dosyasına eşlik eden bir metin dosyası.

<a name="projection"></a>**Projeksiyon**: Enine Mercator, Albers eşit alan ve Robinson gibi harita projeksiyonuna dayalı öngörülen koordinat sistemidir. Bunlar, dünya yüzeyinin haritalarını iki boyutlu kartezyen koordinat düzlemine yansıtma olanağı sağlar. Öngörülen koordinat sistemleri bazen harita projeksiyonları olarak adlandırılır.

## <a name="q"></a>Q

<a name="quadkey"></a>**Quadkey**: Dört ağaç döşeme sistemi içindeki bir döşeme için taban-4 adres dizini. Daha fazla bilgi için [yakınlaştırma düzeyleri ve](zoom-levels-and-tile-grid.md) daha fazla bilgi için döşeme ızgarası belgelerine bakın.

<a name="quadtree"></a>**Quadtree**: Her düğümün tam olarak dört alt bölümü olan bir veri yapısı. Azure Haritalar'da kullanılan döşeme sistemi, kullanıcı bir düzeyde yakınlaştırma yaptığı için, her harita döşemesinin dört altyapıya parçalanması gibi dört ağaçlı bir yapı kullanır.  Daha fazla bilgi için [yakınlaştırma düzeyleri ve](zoom-levels-and-tile-grid.md) daha fazla bilgi için döşeme ızgarası belgelerine bakın.

<a name="queries-per-second-qps"></a>**Saniye Başına Sorgular (QPS)**: Bir hizmete veya platforma bir saniye içinde yapIlebilen sorgu veya istek sayısı. 

## <a name="r"></a>R

<a name="radial-search"></a>**Radyal arama**: Bir noktadan sabit bir düz çizgi mesafesini (karga uçarken) arayan uzamsal bir sorgu. 

<a name="raster-data"></a>**Raster verileri**: Her hücrenin sıcaklık gibi bilgileri temsil eden bir değer içerdiği satır ve sütunlar (veya ızgara) halinde düzenlenmiş hücreler (veya pikseller) matrisi. Raster's dijital hava fotoğrafları, uydulardan görüntüler, dijital resimler ve taranmış haritalar içerir.

<a name="raster-layer"></a>**Raster tabakası**: Raster görüntülerinden oluşan çini tabakası.

<a name="reachable-range"></a>Ulaşılabilir aralık : Ulaşılabilir **aralık,** bir kişinin belirli bir zaman veya mesafe içinde seyahat edebileceği alanı, bir ulaşım aracının bir konumdan herhangi bir yönde seyahat edebilmesi için tanımlar. Ayrıca [Isochrone](#isochrone) ve [Isodistance](#isodistance)bakın.

<a name="remote-sensing"></a>**Uzaktan algılama**: Sensör verilerinin uzaktan toplanması ve yorumlanması işlemidir.

<a name="rest-service"></a>**REST hizmeti**: REST kısaltması Temsili Durum Transferi anlamına gelir. REST hizmeti iletişim kurmak için temel web teknolojisine dayanan, en yaygın yöntemler HTTP GET ve POST istekleri olan URL tabanlı bir web hizmetidir. Bu tür hizmetler bana çok daha hızlı ve geleneksel SOAP tabanlı hizmetlerdaha küçük eğilimindedir.

<a name="reverse-geocode"></a>**Ters geocode**: Koordinat alma ve harita üzerinde temsil edilen adresi belirleme işlemidir.

<a name="reproject"></a>**Yeniden Proje**: [Bkz. Dönüşüm](#transformation).

<a name="rest-service"></a>**REST hizmeti**: Temsili Durum Transferi için kısaltma. Ademi merkeziyetçi, dağıtılmış bir ortamda eşler arasında bilgi alışverişi için bir mimari. REST, farklı bilgisayarlardaki programların bir işletim sisteminden veya platformdan bağımsız olarak iletişim kurmasına olanak tanır. Bir hizmet, tek bir kaynak bulucuya (URL) Bir Hypertext Aktarım Protokolü (HTTP) isteği gönderebilir ve verileri geri alabilir.

<a name="route"></a>**Rota**: İki veya daha fazla konum arasındaki ve rota boyunca yol işaretleri için talimatlar gibi ek bilgiler de içerebilen bir yol.

<a name="requests-per-second-rps"></a>**Saniye Başına İstek (RPS)**: [Bkz. Sorgular/Saniye Başına (QPS)](#queries-per-second-qps). 

<a name="rss"></a>**RSS**: Gerçekten Basit Sendikasyon kısaltması, Kaynak Tanımı Çerçevesi (RDF) Site Özeti veya Kaynağa bağlı olarak Zengin Site Özeti. Farklı Web siteleri arasında içerik paylaşımı için basit, yapılandırılmış xml biçimi. RSS belgeleri yazar, tarih, başlık, kısa bir açıklama ve bir hipermetin bağlantısı gibi önemli meta veri öğelerini içerir. Bu bilgiler, bir kullanıcının (veya rss yayımcı hizmetinin) hangi materyallerin daha fazla araştırmaya değer olduğuna karar vermesine yardımcı olur.

## <a name="s"></a>S

<a name="satellite-imagery"></a>**Uydu görüntüleri**: Düzlemler ve uydular tarafından doğrudan aşağıya doğru çekilen görüntülerdir.

<a name="secondary-key"></a>**İkincil anahtar**: Azure Haritalar için sağlanan iki abonelik anahtarının ikincisi, paylaşılan anahtar kimlik doğrulaması. Bkz. [Paylaşılan anahtar kimlik doğrulaması.](#shared-key-authentication)

<a name="shapefile-shp"></a>**Shapefile (SHP)**: ESRI Shapefile olarak da bilinir, coğrafi özelliklerin konumunu, şeklini ve özniteliklerini depolamak için bir vektör veri depolama biçimidir. Shapefile, ilgili dosya kümesinde depolanır.

<a name="shared-key-authentication"></a>**Paylaşılan anahtar kimlik doğrulaması**: Paylaşılan Anahtar kimlik doğrulaması, her istekle birlikte Azure Haritalar hesabının geçtiği azure haritalar hesabının azure haritalarına aktarılmasına dayanır. Bu anahtarlar genellikle abonelik anahtarları olarak adlandırılır. Anahtarların güvenlik için düzenli olarak yeniden oluşturulması önerilir. Diğerini yenilerken bir tuşu kullanarak bağlantıları koruyabilmeniz için iki anahtar sağlanır. Anahtarlarınızı yeniden oluşturduğunuzda, yeni anahtarları kullanmak için bu hesaba erişen tüm uygulamaları güncelleştirmeniz gerekir. Azure Haritalar kimlik doğrulaması hakkında daha fazla bilgi edinmek için [Azure Haritalar ve Azure AD'ye](azure-maps-authentication.md) bakın ve Azure [Haritalar'da kimlik doğrulamasını yönetin.](how-to-manage-authentication.md)

<a name="software-development-kit-sdk"></a>**Yazılım geliştirme kiti (SDK)**: Geliştiricinin uygulama oluşturmak için API kullanmasına yardımcı olmak için belge, örnek kod ve örnek uygulamalar koleksiyonu.

<a name="spherical-mercator-projection"></a>**Küresel Mercator projeksiyon**: [Web Mercator](#web-mercator)bakın. 

<a name="spatial-query"></a>**Uzamsal sorgu : Uzamsal**bir işlem gerçekleştiren bir hizmete yapılan istek. Radyal arama veya rota araması gibi.

<a name="spatial-reference"></a>**Uzamsal referans**: Coğrafi varlıkları tam olarak bulmak için kullanılan koordinat tabanlı yerel, bölgesel veya küresel sistemdir. Harita koordinatlarını gerçek dünyadaki konumlarla ilişkilendirmek için kullanılan koordinat sistemini tanımlar. Uzamsal referanslar, farklı katmanlardan veya kaynaklardan gelen uzamsal verilerin doğru görüntüleme veya analiz için tümleştirilebilir olmasını sağlar. Azure Haritalar, giriş geometrisi verileri için [EPSG:3857](https://epsg.io/3857) koordinat başvuru sistemini ve WGS 84'ü kullanır.

<a name="sql-spatial"></a>**SQL uzamsal**: SQL Azure ve SQL Server 2008 ve üzeri yerleşik işlevselliği ifade eder. Bu uzamsal işlevsellik, SQL Server'dan bağımsız olarak kullanılabilen bir .NET kitaplığı olarak da kullanılabilir. Daha fazla bilgi için [Uzamsal Veriler (SQL Server) belgelerine](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) bakın.

<a name="subscription-key"></a>**Abonelik anahtarı**: [Bkz. Paylaşılan anahtar kimlik doğrulaması.](#shared-key-authentication)

<a name="synchronous-request"></a>**Senkron istek**: BIR HTTP isteği bir bağlantı açar ve yanıt bekler. Tarayıcılar, bir sayfadan yapIlebilen eşzamanlı HTTP isteklerinin sayısını sınırlar. Aynı anda birden çok uzun süre çalışan senkron istek yapılırsa, bu sınıra ulaşılabilir. İstekler, diğer isteklerden biri tamamlanana kadar ertelenecektir.

## <a name="t"></a>T

<a name="telematics"></a>**Telematik**: Uzak nesneler üzerinde etkili kontrol ile birlikte telekomünikasyon cihazları üzerinden bilgi gönderme, alma ve depolama. 

<a name="temporal-data"></a>**Zamansal veriler**: Özellikle zaman veya tarihlere atıfta bulunan veriler. Zamansal veriler, yıldırım düşmesi gibi ayrık olaylara atıfta bulunabilir; trenler gibi hareketli nesneler; veya trafik sensörlerinden gelen sayımlar gibi tekrarlanan gözlemler.

<a name="terrain"></a>**Arazi**: Kumlu arazi veya dağlık arazi gibi belirli bir özelliğe sahip bir arazi alanı.

<a name="thematic-maps"></a>**Tematik haritalar**: Tematik harita, bir coğrafi alan hakkında bir temayı yansıtmak için yapılmış basit bir haritadır. Bu tür bir harita için ortak bir senaryo, bazı veri metriklerine göre ülkeler/bölgeler gibi yönetim bölgelerini renklendirmektir.

<a name="tile-layer"></a>**Kiremit tabakası**: Harita döşemelerinin (dikdörtgen kesitler) sürekli bir katmana monte edilerek görüntülenen katmandır. Karolar ya raster görüntü karoları veya vektör karoları vardır. Raster döşeme katmanları genellikle önceden işlenir ve bir sunucuda görüntü olarak depolanır. Raster kiremit katmanları büyük bir depolama alanı kullanabilir. Vektör döşeme katmanları istemci uygulaması içinde gerçek zamanlı yakın işlenir. Bu nedenle, sunucu tarafındaki depolama gereksinimleri vektör döşeme katmanları için daha küçüktür.

<a name="time-zone"></a>**Saat dilimi**: Dünyanın yasal, ticari ve sosyal amaçlar için tek tip standart saatini gözlemleyen bir bölgesidir. Saat dilimleri ülkelerin/bölgelerin ve bunların alt bölümlerinin sınırlarını izleme eğilimindedir.

<a name="transaction"></a>**İşlem**: Azure Haritalar, bir işlem lisanslama modeli kullanır;

- İstenen her 15 harita veya trafik kutucukları için bir işlem oluşturulur.
- Azure Haritalar'daki hizmetlerden birine yapılan her API çağrısı için bir işlem oluşturulur. Arama ve yönlendirme, Azure Haritalar hizmetine örnektir.

<a name="transformation"></a>**Dönüşüm**: Farklı coğrafi koordinat sistemleri arasında veri dönüştürme işlemidir. Örneğin, Birleşik Krallık'ta yakalanan ve OSGB 1936 coğrafi koordinat sistemine dayalı bazı verilere sahip olabilirsiniz. Azure Haritalar, WGS84'ün [EPSG:3857](https://epsg.io/3857) koordinat başvuru sistemi varyantını kullanır. Bu nedenle, verileri doğru görüntülemek için koordinatlarının bir sistemden diğerine dönüştürülmesi gerekir.

<a name="traveling-salesmen-problem-tsp"></a>**Seyahat Satıcıları Sorunu (TSP)**: Bir satış temsilcisi durur bir dizi ziyaret etmek için en verimli yolu bulmak gerekir, sonra başlangıç konumuna dönmek bir Hamiltondevre sorunu.  

<a name="trilateration"></a>**Trilaterasyon**: Üç nokta arasındaki mesafeleri ölçerek, diğer iki noktaya göre, bir noktanın dünya yüzeyindeki konumunu belirleme işlemidir.

<a name="turn-by-turn-navigation"></a>**Adım-dönüş gezinme**: Kullanıcılar bir sonraki manevraya yaklaşırken rotanın her adımı için rota yönergeleri sağlayan uygulamadır.

## <a name="v"></a>V

<a name="vector-data"></a>**Vektör verileri**: Nokta, çizgi veya çokgen olarak temsil edilen eşgüdümlü verileri koordine etmek.

<a name="vector-tile"></a>**Vektör döşemesi**: Harita denetimi ile aynı döşeme sistemini kullanarak jeouzamsal vektör verilerini depolamak için açık veri belirtimidir. [Ayrıca](#tile-layer)bkz.

<a name="vehicle-routing-problem-vrp"></a>**Araç Yönlendirme Sorunu (VRP)**: Bir araç filosu için bir dizi sıralı güzergahın, kısıtlamalar kümesi olarak dikkate alınarak hesaplandığı bir dizi sorun. Bu kısıtlamalar teslimat süresi pencerelerini, birden çok rota kapasitelerini ve seyahat süresi kısıtlamalarını içerebilir.

<a name="voronoi-diagram"></a>**Voronoi diyagramı**: Uzayın, genellikle nokta özellikleri olan bir dizi geometrik nesneyi çevreleyen alanlara veya hücrelere bölünmesi. Bu hücreler veya çokgenler, Delaunay üçgenler için kriterleri karşılamak gerekir. Bir alan içindeki tüm konumlar, çevrede bulunduğu nesneye kümedeki diğer nesnelerden daha yakındır. Voronoi diyagramları genellikle coğrafi özellikler etrafında etki alanları nı çizmek için kullanılır. 

## <a name="w"></a>W

<a name="waypoint"></a>**Yol noktası**: Yol noktası, seyir amacıyla kullanılan boylam ve enlem le tanımlanan belirli bir coğrafi konumdur. Genellikle, birinin bir rotada gezindiği bir noktayı temsil etmek için kullanılır.

<a name="waypoint-optimization"></a>**Yol açısı optimizasyonu**: Sağlanan tüm yol işaretlerinden geçmek için gereken seyahat süresini veya mesafeyi en aza indirmek için bir dizi yol noktasını yeniden sıralama işlemidir. Optimizasyon karmaşıklığına bağlı olarak, bu optimizasyon genellikle [Seyahat Satıcılar Sorunu](#traveling-salesmen-problem-tsp) veya Araç Yönlendirme [Sorunu](#vehicle-routing-problem-vrp)olarak adlandırılır.

<a name="web-map-service-wms"></a>**Web Harita Hizmeti (WMS) : WMS,** görüntü tabanlı harita hizmetlerini tanımlayan bir Open Geographic Konsorsiyumu (OGC) standardıdır. WMS hizmetleri, isteğe bağlı bir harita içindeki belirli alanlar için harita görüntüleri sağlar. Görüntüler önceden işlenmiş simgeler içerir ve hizmet tarafından tanımlandığı takdirde birkaç adlandırılmış stilden birinde işlenebilir.

<a name="web-mercator"></a>**Web Mercator**: Küresel Mercator projeksiyonu olarak da bilinir. Mercator projeksiyonunun hafif bir çeşididir, öncelikle Web tabanlı haritalama programlarında kullanılır. Küçük ölçekli haritalarda kullanılan standart Mercator projeksiyonuyla aynı formülleri kullanır. Ancak, Web Mercator küresel formülleri her ölçekte kullanır, ancak büyük ölçekli Mercator haritaları normalde projeksiyone elipsoidal formunu kullanır. Tutarsızlık küresel ölçekte algılanamaz, ancak yerel alanların haritalarının aynı ölçekte gerçek elipsoidal Mercator haritalarından biraz sapmasına neden olur.

<a name="wgs84"></a>**WGS84**: Haritanın yüzeyindeki konumlarla mekansal koordinatları ilişkilendirmek için kullanılan bir sabit kümesi. WGS84 datum, çoğu çevrimiçi harita sağlayıcısı ve GPS cihazı tarafından kullanılan standart tır. Azure Haritalar, WGS84'ün [EPSG:3857](https://epsg.io/3857) koordinat başvuru sistemi varyantını kullanır.

## <a name="z"></a>Z

<a name="z-coordinate"></a>**Z-koordinat**: [Bkz. Yükseklik](#altitude). 

<a name="zip-code"></a>**Posta kodu**: [Bkz. Posta kodu](#postal-code).

<a name="Zoom level"></a>**Yakınlaştırma düzeyi**: Ayrıntı düzeyini ve haritanın ne kadarının görünür olduğunu belirtir. Seviye 0'a kadar tüm yakınlaştırıldığında, tüm dünya haritası genellikle görünür olacaktır. Ancak, harita ülke/bölge adları, sınırlar ve okyanus adları gibi sınırlı ayrıntıları gösterir. Seviye 17'ye yakın yakınlaştırıldığında, harita ayrıntılı yol bilgileri içeren birkaç şehir bloğundan oluşan bir alanı görüntüler. Azure haritalarda en yüksek yakınlaştırma seviyesi 22'dir. Daha fazla bilgi için [Yakınlaştırma düzeyleri ve döşeme ızgarası](zoom-levels-and-tile-grid.md) belgelerine bakın.

