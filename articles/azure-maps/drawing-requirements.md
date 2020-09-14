---
title: Azure haritalar Creator 'da paket gereksinimlerini çizme
description: Tesis tasarım dosyalarınızı eşleme verilerine dönüştürmek için çizim paketi gereksinimleri hakkında bilgi edinin
author: anastasia-ms
ms.author: v-stharr
ms.date: 6/12/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philMea
ms.openlocfilehash: 1f25aadf716b7768b6122a4fb165466aef7f8a16
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2020
ms.locfileid: "90053401"
---
# <a name="drawing-package-requirements"></a>Çizim paketi gereksinimleri

Karşıya yüklenen çizim paketlerini [Azure Maps dönüştürme hizmetini](https://docs.microsoft.com/rest/api/maps/conversion)kullanarak harita verilerine dönüştürebilirsiniz. Bu makalede, dönüştürme API 'SI için çizim paketi gereksinimleri açıklanmaktadır. Örnek bir paket görüntülemek için örnek [Çizim paketini](https://github.com/Azure-Samples/am-creator-indoor-data-examples)indirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Çizim paketi, Autodesk 'nin AutoCAD® yazılımının yerel dosya biçimi olan, DWG biçiminde kaydedilen çizimleri içerir.

Çizim paketindeki çizimleri oluşturmak için herhangi bir CAD yazılımını seçebilirsiniz.  

[Azure haritalar dönüştürme hizmeti](https://docs.microsoft.com/rest/api/maps/conversion) , çizim paketini harita verilerine dönüştürür. Dönüştürme hizmeti, AutoCAD DWG dosya biçimiyle birlikte kullanılabilir. `AC1032` , DWG dosyalarının iç biçim sürümüdür ve `AC1032` Iç DWG dosya biçimi sürümünü seçmek iyi bir fikirdir.  

## <a name="glossary-of-terms"></a>Terimler sözlüğü

Kolay başvuru için, bu makaleyi okurken önemli olan bazı hüküm ve tanımlar aşağıda verilmiştir.

| Süre  | Tanım |
|:-------|:------------|
| Katman | Bir AutoCAD DWG katmanı.|
| Düzey | Bir küme yükseltmesinde bina alanı. Örneğin, bir bina tabanı. |
| XREF  |AutoCAD DWG dosya biçimindeki (. dwg), dış başvuru olarak birincil Çizime eklenen bir dosya.  |
| Öne çıkan özelliği | Ek meta veri bilgileriyle geometriyi birleştiren bir nesne. |
| Özellik sınıfları | Özellikler için ortak bir şema. Örneğin, bir *birim* bir özellik sınıfıdır ve *Office* bir özelliktir. |

## <a name="drawing-package-structure"></a>Çizim paketi yapısı

Bir çizim paketi, aşağıdaki dosyaları içeren bir. zip arşividir:

* AutoCAD DWG dosya biçimindeki DWG dosyaları.
* Tek bir tesis için dosyada _manifest.js_ .

DWG dosyalarını, klasörü içinde herhangi bir şekilde düzenleyebilirsiniz, ancak bildirim dosyası klasörün kök dizininde canlı olmalıdır. Tek bir arşiv dosyasındaki klasörü. zip uzantısına sahip olarak zip etmeniz gerekir. Sonraki bölümlerde, DWG dosyaları, bildirim dosyası ve bu dosyaların içeriği için gereksinimler ayrıntılandırır.  

## <a name="dwg-files-requirements"></a>DWG dosyaları gereksinimleri

Her tesis düzeyi için tek bir DWG dosyası gereklidir. Düzeyin verileri tek bir DWG dosyasında bulunmalıdır. Tüm dış başvurular (_xrefs_) üst çizime bağlanmalıdır. Ayrıca, her bir DWG dosyası:

* _Dış_ ve _birim_ katmanları tanımlanmalıdır. İsteğe bağlı olarak, aşağıdaki isteğe bağlı katmanları tanımlayabilir: _duvar_, _kapı_, _Unitlabel_, _Zone_ve _zonelabel_.
* Birden çok düzeyden Özellikler içermemelidir.
* Birden çok tesisten Özellikler içermemelidir.

[Azure haritalar dönüştürme hizmeti](https://docs.microsoft.com/rest/api/maps/conversion) , aşağıdaki özellik SıNıFLARıNı bir DWG dosyasından ayıklayabilir:

* Düzeyler
* Birimler
* Bölgeler
* Larını
* Ların
* Dikey penoylamalar

Tüm dönüştürme işleri en az bir varsayılan kategori kümesine neden olacak: Oda, yapı. duvar, açılış. kapı, bölge ve tesis. Ek kategoriler, nesneler tarafından başvurulan her kategori adına yöneliktir.  

Bir DWG katmanının tek bir sınıfın özellikleri içermesi gerekir. Sınıflar bir katman paylaşmamalıdır. Örneğin, birimler ve duvarlar bir katmanı paylaşamaz.

DWG katmanları da aşağıdaki ölçütlere uymalıdır:

* Tüm DWG dosyaları için çizimlerin kaynakları, aynı Enlem ve Boylam ile hizalanmalıdır.
* Her düzey, diğer düzeyler ile aynı yönde olmalıdır.
* Otomatik olarak kesişen çokgenler otomatik olarak onarılır ve [Azure Maps dönüştürme hizmeti](https://docs.microsoft.com/rest/api/maps/conversion) bir uyarı oluşturur. Beklenen sonuçlarla eşleşeceğinden, onarılan sonuçları el ile incelemeniz gerekir.

Tüm katman varlıkları şu türlerden biri olmalıdır: çizgi, çoklu çizgi, Çokgen, dairesel yay, daire veya metin (tek satırlı). Diğer herhangi bir varlık türü yok sayılır.

Aşağıdaki tabloda, desteklenen varlık türleri ve her katman için desteklenen özellikler özetlenmektedir. Bir katman Desteklenmeyen varlık türleri içeriyorsa, [Azure Maps dönüştürme hizmeti](https://docs.microsoft.com/rest/api/maps/conversion) bu varlıkları yoksayar.  

| Katman | Varlık türleri | Özellikler |
| :----- | :-------------------| :-------
| [Dış](#exterior-layer) | Çokgen, çoklu çizgi (kapalı), daire | Düzeyler
| [Birim](#unit-layer) |  Çokgen, çoklu çizgi (kapalı), daire | Dikey penetler, birimler
| [Dolg](#wall-layer)  | Çokgen, çoklu çizgi (kapalı), daire | Geçerli değildir. Daha fazla bilgi için bkz. [duvar katmanı](#wall-layer).
| [Kapı](#door-layer) | Çokgen, çoklu çizgi, çizgi, devre bir yay, daire | Larını
| [Bölge](#zone-layer) | Çokgen, çoklu çizgi (kapalı), daire | Bölge
| [UnitLabel](#unitlabel-layer) | Metin (tek satırlı) | Geçerli değildir. Bu katman yalnızca birimler katmanından birim özelliklerine özellikler ekleyebilir. Daha fazla bilgi için bkz. [Unitlabel katmanı](#unitlabel-layer).
| [Bölge etiketi](#zonelabel-layer) | Metin (tek satırlı) | Geçerli değildir. Bu katman, bölge özelliklerine yalnızca bölgelere Eslayer öğesinden özellikler ekleyebilir. Daha fazla bilgi için bkz. [bölge etiketi katmanı](#zonelabel-layer).

Sonraki bölümlerde her bir katmanın gereksinimleri ayrıntılandırır.

### <a name="exterior-layer"></a>Dış katman

Her düzeyin DWG dosyası, bu düzeyin çevre birimi tanımlamak için bir katman içermelidir. Bu katman, *dış* katman olarak adlandırılır. Örneğin, bir tesis iki düzey içeriyorsa, her dosya için dış katmanla birlikte iki DWG dosyası olması gerekir.

Dış katmanda kaç varlık çizimleri olduğuna bakılmaksızın, [sonuçta elde edilen tesis veri kümesi](tutorial-creator-indoor-maps.md#create-a-feature-stateset) her bir DWG dosyası için yalnızca bir düzey özellik içerir. Ek olarak:

* Dış örler, Çokgen, çoklu çizgi (kapalı) veya daire olarak çizmelidir.
* Dış dosyalar örtüşüyor, ancak tek bir geometride çözülür.

Katman birden çok çakışan PolyLines içeriyorsa, PolyLines tek düzeyli bir özellik olarak çözülür. Alternatif olarak, katman birden çok çakışmayan PolyLines içeriyorsa, sonuçta elde edilen düzey özelliğin çok Çokgen bir temsili vardır.

[Örnek çizim paketindeki](https://github.com/Azure-Samples/am-creator-indoor-data-examples)ana hat katmanı olarak dış katmana bir örnek görebilirsiniz.

### <a name="unit-layer"></a>Birim katmanı

Her düzey için DWG dosyası birimleri içeren bir katman tanımlar. Birimler, binadaki ofisler, hallyollar, STAIRS ve yükseltme gibi gezinilebilir alanlar. Birimler katmanının aşağıdaki gereksinimlere uyması gerekir:

* Birimler Çokgen, çoklu çizgi (kapalı) veya daire olarak çizilmelidir.
* Birimler, Tesis dış çevre sınırları içine düşmelidir.
* Birimler kısmen örtüşmemelidir.
* Birimler, kendisiyle kesişen bir geometri içermemelidir.

UnitLabel katmanında bir metin nesnesi oluşturarak bir birimi adlandırın ve ardından nesneyi birimin sınırları içine yerleştirin. Daha fazla bilgi için bkz. [Unitlabel katmanı](#unitlabel-layer).

[Örnek çizim paketindeki](https://github.com/Azure-Samples/am-creator-indoor-data-examples)birimler katmanının bir örneğini görebilirsiniz.

### <a name="wall-layer"></a>Duvar katmanı

Her düzeyin DWG dosyası, duvarların, sütunların ve diğer yapı yapısının fiziksel kapsamlarını tanımlayan bir katman içerebilir.

* Duvarlar Çokgen, çoklu çizgi (kapalı) veya daire olarak çizmelidir.
* Duvar katmanı veya Katmanlar yalnızca yapı yapısı olarak yorumlanan geometriyi içermelidir.

[Örnek çizim paketindeki](https://github.com/Azure-Samples/am-creator-indoor-data-examples)duvarlar katmanının bir örneğini görebilirsiniz.

### <a name="door-layer"></a>Kapı katmanı

Kapıların bulunduğu bir DWG katmanını dahil edebilirsiniz. Her kapıdaki birim katmanından bir birimin kenarıyla örtüşmelidir.

Azure haritalar veri kümesindeki kapı kapalerinin birden çok birim sınırı ile örtüşen tek satırlık bir kesim olarak temsil edilir. Aşağıdaki resimlerde, bir veri kümesindeki özellikleri açmak için kapı katmanındaki geometriyi dönüştürme gösterilmektedir.

![Açılışlarını oluşturma adımlarını gösteren dört grafik](./media/drawing-requirements/opening-steps.png)

### <a name="zone-layer"></a>Bölge katmanı

Her düzeyin DWG dosyasında, bölgelerin fiziksel kapsamlarını tanımlayan bir bölge katmanı bulunabilir. Bir bölge, boş bir alan veya bir arka bahçe olabilir.

* Bölgeler Çokgen, çoklu çizgi (kapalı) veya daire olarak çizmelidir.
* Bölgeler örtüşebilir.
* Bölgeler, Tesis dış çevre birimi içinde veya dışında olabilir.

Bölge etiketi katmanında bir metin nesnesi oluşturarak ve metin nesnesini bölgenin sınırları içine yerleştirerek bir bölgeyi adlandırın. Daha fazla bilgi için bkz. [bölge etiketi katmanı](#zonelabel-layer).

[Örnek çizim paketindeki](https://github.com/Azure-Samples/am-creator-indoor-data-examples)bölge katmanının bir örneğini görebilirsiniz.

### <a name="unitlabel-layer"></a>UnitLabel katmanı

Her düzeyin DWG dosyası bir UnitLabel katmanı içerebilir. UnitLabel katmanı, birim katmanından ayıklanan birimlere bir Name özelliği ekler. Name özelliğine sahip birimler, bildirim dosyasında belirtilen ek ayrıntılara sahip olabilir.

* Birim etiketleri tek satırlık metin varlıkları olmalıdır.
* Birim etiketleri, birimlerinin sınırlarının içine düşmelidir.
* Birimler UnitLabel katmanında birden çok metin varlığı içermemelidir.

[Örnek çizim paketindeki](https://github.com/Azure-Samples/am-creator-indoor-data-examples)unitlabel katmanının bir örneğini görebilirsiniz.

### <a name="zonelabel-layer"></a>Bölge etiketi katmanı

Her düzeyin DWG dosyası bir bölge etiketi katmanı içerebilir. Bu katman, bölge katmanından ayıklanan bölgelere bir ad özelliği ekler. Name özelliğine sahip bölgeler, bildirim dosyasında belirtilen ek ayrıntılara sahip olabilir.

* Bölgeler etiketleri tek satırlık metin varlıkları olmalıdır.
* Bölgeler etiketleri kendi bölgelerinin sınırları içine düşmelidir.
* Bölgeler, bölge etiketi katmanında birden çok metin varlığı içermemelidir.

[Örnek çizim paketindeki](https://github.com/Azure-Samples/am-creator-indoor-data-examples)bölge etiketi katmanının bir örneğini görebilirsiniz.

## <a name="manifest-file-requirements"></a>Bildirim dosyası gereksinimleri

ZIP klasörü, dizinin kök düzeyinde bir bildirim dosyası içermeli ve dosyanın **manifest.jsolarak**adlandırılması gerekir. [Azure haritalar dönüştürme hizmeti](https://docs.microsoft.com/rest/api/maps/conversion) 'nin içeriklerini ayrıştırmasına izin vermek için DWG dosyalarını açıklar. Yalnızca bildirimle tanımlanan dosyalar alınır. ZIP klasöründeki, ancak bildirimde düzgün listelenmeyen dosyalar yok sayılır.

`buildingLevels`Bildirim dosyasının nesnesindeki dosya yolları ZIP klasörünün köküne göreli olmalıdır. DWG dosya adı, Tesis düzeyinin adıyla tam olarak eşleşmelidir. Örneğin, "Basement" düzeyi için bir DWG dosyası "Basement. dwg" dir. Düzey 2 için bir DWG dosyası "level_2. dwg" olarak adlandırılır. Düzey adınızın bir alanı varsa alt çizgi kullanın.

Bildirim nesnelerini kullandığınızda gereksinimler olsa da, tüm nesneler gerekli değildir. Aşağıdaki tabloda, [Azure Maps dönüştürme hizmeti](https://docs.microsoft.com/rest/api/maps/conversion)'nin 1,1 sürümü için gerekli ve isteğe bağlı nesneler gösterilmektedir.

| Nesne | Gerekli | Açıklama |
| :----- | :------- | :------- |
| `version` | true |Bildirim şeması sürümü. Şu anda yalnızca sürüm 1,1 destekleniyor.|
| `directoryInfo` | true | Tesis coğrafi ve iletişim bilgilerini özetler. Ayrıca, bir açık coğrafi ve iletişim bilgilerini ana hatlarıyla yapmak için de kullanılabilir. |
| `buildingLevels` | true | Bina düzeylerini ve düzeylerin tasarımını içeren dosyaları belirtir. |
| `georeference` | true | Tesis çizimi için sayısal coğrafi bilgiler içerir. |
| `dwgLayers` | true | Katmanların adlarını listeler ve her bir katman kendi özelliklerinin adlarını listeler. |
| `unitProperties` | yanlış | Birim özelliklerine ek meta veri eklemek için kullanılabilir. |
| `zoneProperties` | yanlış | , Bölge özellikleri için ek meta veri eklemek için kullanılabilir. |

Sonraki bölümlerde her bir nesne için gereksinimler ayrıntılandırır.

### `directoryInfo`

| Özellik  | Tür | Gerekli | Description |
|-----------|------|----------|-------------|
| `name`      | dize | true   |  Oluşturma adı. |
| `streetAddress`|    string |    yanlış    | Bina adresi. |
|`unit`     | string    |  yanlış    |  Bina birimi. |
| `locality` |    string |    yanlış |    Bir alanın, semtin veya bölgenin adı. Örneğin, "Overlake" veya "Merkezi bölge". Konum, posta adresinin bir parçası değil. |
| `adminDivisions` |    Dizelerin JSON dizisi |    yanlış     | Adres göstergeleri (ülke, eyalet, şehir) veya (ülke, tercih Ecture, şehir, Town) içeren bir dizi. ISO 3166 ülke kodlarını ve ISO 3166-2 Eyalet/bölge kodlarını kullanın. |
| `postalCode` |    string    | yanlış    | Posta sıralama kodu. |
| `hoursOfOperation` |    string |     yanlış | [OSD açılış saati](https://wiki.openstreetmap.org/wiki/Key:opening_hours/specification) biçimine uyar. |
| `phone`    | string |    yanlış |    Binanın ilişkilendirildiği telefon numarası. Ülke kodunu içermelidir. |
| `website`    | string |    yanlış    | Binanın ilişkilendirildiği Web sitesi. Http veya https ile başlamalıdır. |
| `nonPublic` |    bool    | yanlış | Binanın herkese açık olup olmadığını belirten bayrak. |
| `anchorLatitude` | sayısal |    yanlış | Bir tesis çıpası (raptiye). |
| `anchorLongitude` | sayısal |    yanlış | Tesis bağlayıcının Boylam (raptiye). |
| `anchorHeightAboveSeaLevel`  | sayısal | yanlış | Tesislerin, metre cinsinden deniz düzeyindeki zemin tabanı genişliği. |
| `defaultLevelVerticalExtent` | sayısal | yanlış | Bir düzeyin tanımsız olduğu durumlarda kullanılacak bu tesis düzeyinin varsayılan yüksekliği (kalınlığı) `verticalExtent` . |

### `buildingLevels`

`buildingLevels`Nesne, bir bina seviyelerinin JSON dizisini içeriyor.

| Özellik  | Tür | Gerekli | Description |
|-----------|------|----------|-------------|
|`levelName`    |dize    |true |    Açıklayıcı düzey adı. Örneğin: kat 1, giriş, mavi park veya taban.|
|`ordinal` | integer |    true | Düzeylerin dikey sırasını belirler. Her tesis 0 sıralı bir düzeye sahip olmalıdır. |
|`heightAboveFacilityAnchor` | sayısal | yanlış |    Metredeki bağlayıcının üzerindeki düzey yüksekliği. |
| `verticalExtent` | sayısal | yanlış | Ölçü cinsinden düzeyin zemin-üst yüksekliği (kalınlığı). |
|`filename` |    string |    true |    Bir bina düzeyi için CAD çiziminin dosya sistemi yolu. Bu, binanın ZIP dosyasının köküne göreli olmalıdır. |

### `georeference`

| Özellik  | Tür | Gerekli | Açıklama |
|-----------|------|----------|-------------|
|`lat`    | sayısal |    true |    Tesis çiziminin başlangıç aşamasında Enlem derecenin ondalık temsili. Kaynak koordinatları WGS84 Web Mercator () içinde olmalıdır `EPSG:3857` .|
|`lon`    |sayısal|    true|    Tesis çiziminin başlangıç noktasında derece boylam özelliğinin ondalık temsili. Kaynak koordinatları WGS84 Web Mercator () içinde olmalıdır `EPSG:3857` . |
|`angle`|    sayısal|    true|   Doğru Kuzey ile çizimin düşey (Y) ekseni arasındaki saat yönünde açı.   |

### `dwgLayers`

| Özellik  | Tür | Gerekli | Açıklama |
|-----------|------|----------|-------------|
|`exterior`    |dize dizisi|    true|    Dış yapı profilini tanımlayan katmanların adları.|
|`unit`|    dize dizisi|    true|    Birimleri tanımlayan katmanların adları.|
|`wall`|    dize dizisi    |yanlış|    Duvarları tanımlayan katmanların adları.|
|`door`    |dize dizisi|    yanlış   | Kapaların tanımlayan katmanların adları.|
|`unitLabel`    |dize dizisi|    yanlış    |Birimlerin adlarını tanımlayan katmanların adları.|
|`zone` | dize dizisi    | yanlış    | Bölgeleri tanımlayan katmanların adları.|
|`zoneLabel` | dize dizisi |     yanlış |    Bölgelerin adlarını tanımlayan katmanların adları.|

### `unitProperties`

`unitProperties`Nesne, birim ÖZELLIKLERININ JSON dizisini içerir.

| Özellik  | Tür | Gerekli | Description |
|-----------|------|----------|-------------|
|`unitName`    |dize    |true    |Bu kayıtla ilişkilendirilecek birimin adı `unitProperty` . Bu kayıt yalnızca katmanlarda eşleşen bir etiket bulunduğunda geçerlidir `unitName` `unitLabel` . |
|`categoryName`|    string|    yanlış    |Kategori adı. Kategorilerin tüm listesi için [Kategoriler](https://aka.ms/pa-indoor-spacecategories)' e bakın. |
|`navigableBy`| dize dizisi |    yanlış    |Birimi geçebileceğini gezinme aracılarının türlerini gösterir. Bu özellik, waybulma yeteneklerini bilgilendirir. İzin verilen değerler: `pedestrian` , `wheelchair` ,, `machine` `bicycle` , `automobile` , `hiredAuto` , `bus` , `railcar` , `emergency` , `ferry` , `boat` , ve `disallowed` .|
|`routeThroughBehavior`|    string|    yanlış    |Birim için yönlendirme davranışı. İzin verilen değerler, `disallowed` , `allowed` ve `preferred` . Varsayılan değer: `allowed`.|
|`occupants`    |DirectoryInfo nesneleri dizisi |yanlış    |Birim için işanlar listesi. |
|`nameAlt`|    string|    yanlış|    Birimin alternatif adı. |
|`nameSubtitle`|    string    |yanlış|    Birimin alt başlığı. |
|`addressRoomNumber`|    string|    yanlış|    Birimin Oda, birim, Grup veya paket numarası.|
|`verticalPenetrationCategory`|    string|    yanlış| Bu özellik tanımlandığında, elde edilen özellik bir birim yerine dikey bir sızma (VRT) olur. Sanal ağları yukarıdaki veya altındaki düzeylerdeki diğer VRT özelliklerine gitmek için de kullanabilirsiniz. Dikey penetasyon [Kategori](https://aka.ms/pa-indoor-spacecategories) adıdır. Bu özellik tanımlanmışsa, `categoryName` özelliği ile geçersiz kılınır `verticalPenetrationCategory` . |
|`verticalPenetrationDirection`|    string|    yanlış    |`verticalPenetrationCategory`Tanımlanmışsa, isteğe bağlı olarak geçerli seyahat yönünü tanımlayın. İzin verilen değerler: `lowToHigh` , `highToLow` , `both` , ve `closed` . Varsayılan değer: `both`.|
| `nonPublic` | bool | yanlış | Birimin herkese açık olup olmadığını gösterir. |
| `isRoutable` | bool | yanlış | Bu özellik olarak ayarlandığında `false` , birime veya birime gidemezsiniz. Varsayılan değer: `true`. |
| `isOpenArea` | bool | yanlış | Gezinti aracının, birime eklenmiş bir açmaya gerek kalmadan birimi girmesine izin verir. Varsayılan olarak, bu değer, yer `true` içermeyen birimler için ve yer imiyle birimler için olarak ayarlanır `false` . `isOpenArea` `false` Açık olmayan bir birimde el ile olarak ayarlanması bir uyarı ile sonuçlanır. Bunun nedeni, elde edilen birime gezinme Aracısı tarafından erişilememesi olabilir.|

### `zoneProperties`

`zoneProperties`Nesne, bölge ÖZELLIKLERININ JSON dizisini içerir.

| Özellik  | Tür | Gerekli | Açıklama |
|-----------|------|----------|-------------|
|ZoneName & lt        |string    |true    |Kayıtla ilişkilendirilecek bölgenin adı `zoneProperty` . Bu kayıt yalnızca bölgenin katmanında eşleşen bir etiket bulunduğunda geçerlidir `zoneName` `zoneLabel` .  |
|categoryName|    string|    yanlış    |Kategori adı. Kategorilerin tüm listesi için [Kategoriler](https://aka.ms/pa-indoor-spacecategories)' e bakın. |
|Bölge \/alt|    string|    yanlış    |Bölgenin alternatif adı.  |
|Bölgesi \ Amesubtitle|    string |    yanlış    |Bölgenin alt başlığı. |
|Bölge SetID|    string |    yanlış    | Bir grup olarak sorgulanabilmeleri veya seçilebilmeleri için birden çok bölge arasında ilişki kurmak üzere KIMLIĞI ayarlayın. Örneğin, birden çok düzeye yayılan bölgeler. |

### <a name="sample-drawing-package-manifest"></a>Örnek çizim paketi bildirimi

Örnek çizim paketine yönelik örnek bir bildirim dosyası aşağıda verilmiştir. Tüm paketi indirmek için bkz. [örnek çizim paketi](https://github.com/Azure-Samples/am-creator-indoor-data-examples).

#### <a name="manifest-file"></a>Bildirim dosyası

```JSON
{
    "version": "1.1", 
    "directoryInfo": { 
        "name": "Contoso Building", 
        "streetAddress": "Contoso Way", 
        "unit": "1", 
        "locality": "Contoso eastside", 
        "postalCode": "98052", 
        "adminDivisions": [ 
            "Contoso city", 
            "Contoso state", 
            "Contoso country" 
        ], 
        "hoursOfOperation": "Mo-Fr 08:00-17:00 open", 
        "phone": "1 (425) 555-1234", 
        "website": "www.contoso.com", 
        "nonPublic": false, 
        "anchorLatitude": 47.636152, 
        "anchorLongitude": -122.132600, 
        "anchorHeightAboveSeaLevel": 1000, 
        "defaultLevelVerticalExtent": 3  
    }, 
    "buildingLevels": { 
        "levels": [ 
            { 
                "levelName": "Basement", 
                "ordinal": -1, 
                "filename": "./Basement.dwg" 
            }, { 
                "levelName": "Ground", 
                "ordinal": 0, 
                "verticalExtent": 5, 
                "filename": "./Ground.dwg" 
            }, { 
                "levelName": "Level 2", 
                "ordinal": 1, 
                "heightAboveFacilityAnchor": 3.5, 
                "filename": "./Level_2.dwg" 
            } 
        ] 
    }, 
    "georeference": { 
        "lat": 47.636152, 
        "lon": -122.132600, 
        "angle": 0 
    }, 
    "dwgLayers": { 
        "exterior": [ 
            "OUTLINE", "WINDOWS" 
        ], 
        "unit": [ 
            "UNITS" 
        ], 
        "wall": [ 
            "WALLS" 
        ], 
        "door": [ 
            "DOORS" 
        ], 
        "unitLabel": [ 
            "UNITLABELS" 
        ], 
        "zone": [ 
            "ZONES" 
        ], 
        "zoneLabel": [ 
            "ZONELABELS" 
        ] 
    }, 
    "unitProperties": [ 
        { 
            "unitName": "B01", 
            "categoryName": "room.office", 
            "navigableBy": ["pedestrian", "wheelchair", "machine"], 
            "routeThroughBehavior": "disallowed", 
            "occupants": [ 
                { 
                    "name": "Joe's Office", 
                    "phone": "1 (425) 555-1234" 
                } 
            ], 
            "nameAlt": "Basement01", 
            "nameSubtitle": "01", 
            "addressRoomNumber": "B01", 
            "nonPublic": true, 
            "isRoutable": true, 
            "isOpenArea": true 
        }, 
        { 
            "unitName": "B02" 
        }, 
        { 
            "unitName": "B05", 
            "categoryName": "room.office" 
        }, 
        { 
            "unitName": "STRB01", 
            "verticalPenetrationCategory": "verticalPenetration.stairs", 
            "verticalPenetrationDirection": "both" 
        }, 
        { 
            "unitName": "ELVB01", 
            "verticalPenetrationCategory": "verticalPenetration.elevator", 
            "verticalPenetrationDirection": "high_to_low" 
        } 
    ], 
    "zoneProperties": 
    [ 
        { 
            "zoneName": "WifiB01", 
            "categoryName": "Zone", 
            "zoneNameAlt": "MyZone", 
            "zoneNameSubtitle": "Wifi", 
            "zoneSetId": "1234" 
        }, 
        { 
            "zoneName": "Wifi101",
            "categoryName": "Zone",
            "zoneNameAlt": "MyZone",
            "zoneNameSubtitle": "Wifi",
            "zoneSetId": "1234"
        }
    ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Çizim paketiniz gereksinimleri karşıladığında, paketi bir harita veri kümesine dönüştürmek için [Azure Maps dönüştürme hizmetini](https://docs.microsoft.com/rest/api/maps/conversion) kullanabilirsiniz. Daha sonra, ınkapımaps modülünü kullanarak bir ınkapısı eşlemesi oluşturmak için veri kümesini kullanabilirsiniz.

> [!div class="nextstepaction"]
>[Inkapı haritaları için Oluşturucu](creator-indoor-maps.md)

> [!div class="nextstepaction"]
> [Öğretici: bir Oluşturucu iç eşleme oluşturma](tutorial-creator-indoor-maps.md)

> [!div class="nextstepaction"]
> [Inkapı haritaları dinamik stil oluşturma](indoor-map-dynamic-styling.md)
