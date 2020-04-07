---
title: Model dönüşümyapılandırma
description: Tüm model dönüştürme parametrelerinin açıklaması
author: florianborn71
ms.author: flborn
ms.date: 03/06/2020
ms.topic: how-to
ms.openlocfilehash: eb287b812c477b2e472c48d7bd8f44574a398bac
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681577"
---
# <a name="configure-the-model-conversion"></a>Model dönüşümyapılandırma

Bu bölümde, model dönüştürme seçenekleri belgelenmiştir.

## <a name="settings-file"></a>Ayarlar dosyası

Giriş modelinin `ConversionSettings.json` yanında giriş kapsayıcısında adı verilen bir dosya bulunursa, model dönüştürme işlemi için ek yapılandırma sağlamak için kullanılır.

Dosyanın içeriği aşağıdaki json şema karşılamak gerekir:

```json
{
    "$schema" : "http://json-schema.org/schema#",
    "description" : "ARR ConversionSettings Schema",
    "type" : "object",
    "properties" :
    {
        "scaling" : { "type" : "number", "exclusiveMinimum" : 0, "default" : 1.0 },
        "recenterToOrigin" : { "type" : "boolean", "default" : false },
        "opaqueMaterialDefaultSidedness" : { "type" : "string", "enum" : [ "SingleSided", "DoubleSided" ], "default" : "DoubleSided" },
        "material-override" : { "type" : "string", "default" : "" },
        "gammaToLinearMaterial" : { "type" : "boolean", "default" : false },
        "gammaToLinearVertex" : { "type" : "boolean", "default" : false },
        "sceneGraphMode": { "type" : "string", "enum" : [ "none", "static", "dynamic" ], "default" : "dynamic" },
        "generateCollisionMesh" : { "type" : "boolean", "default" : true },
        "unlitMaterials" : { "type" : "boolean", "default" : false },
        "fbxAssumeMetallic" : { "type" : "boolean", "default" : true },
        "axis" : {
            "type" : "array",
            "items" : {
                "type" : "string",
                "enum" : ["default", "+x", "-x", "+y", "-y", "+z", "-z"]
            },
            "minItems": 3,
            "maxItems": 3
        }
    },
    "additionalProperties" : false
}
```

Örnek `ConversionSettings.json` bir dosya şu olabilir:

```json
{
    "scaling" : 0.01,
    "recenterToOrigin" : true,
    "material-override" : "box_materials_override.json"
}
```

### <a name="geometry-parameters"></a>Geometri parametreleri

* `scaling`- Bu parametre bir modeli düzgün ölçekler. Ölçekleme, örneğin bir tablo nun üzerinde bir yapı modelini görüntülemek için bir modeli büyütmek veya küçültmek için kullanılabilir. İşleyici motor uzunlukların metre cinsinden belirtilmesini beklediğinden, bir model farklı birimlerde tanımlandığında bu parametrenin bir diğer önemli kullanımı ortaya çıkar. Örneğin, bir model santimetre olarak tanımlanırsa, 0,01 ölçeğini uygulamak modeli doğru boyutta işlemelidir.
Bazı kaynak veri biçimleri (örneğin .fbx), dönüştürmenin modeli metre birimlerine dolaylı olarak ölçeklediği bir birim ölçeklendirme ipucu sağlar. Kaynak biçimi tarafından sağlanan örtük ölçekleme ölçekleme parametresinin üstüne uygulanır.
Son ölçekleme faktörü geometri tepelerine ve sahne grafiği düğümlerinin yerel dönüşümlerine uygulanır. Kök varlığın dönüşümü için ölçekleme değiştirilmemiş kalır.

* `recenterToOrigin`- Bir modelin, sınırlayıcı kutusunun kaynağında ortalanacak şekilde dönüştürülmesi gerektiğini belirtir.
Kaynak model kaynaktan çok uzaksa merkezleme önemlidir, çünkü bu durumda kayan nokta hassas sorunları yapıoluşturma eserlerine neden olabilir.

* `opaqueMaterialDefaultSidedness`- Render motor opak malzemelerin çift taraflı olduğunu varsayar.
Amaçlanan davranış bu değilse, bu parametre "SingleSided" olarak ayarlanmalıdır. Daha fazla bilgi için [tek taraflı işleme](../../overview/features/single-sided-rendering.md)ye bakın.

### <a name="material-overrides"></a>Malzeme geçersiz kılar

* `material-override`- Bu [parametre, dönüştürme sırasında](override-materials.md)malzemelerin işlenmesini sağlar.

### <a name="color-space-parameters"></a>Renk alanı parametreleri

İşleme motoru renk değerlerinin doğrusal uzayda olmasını bekler.
Bir model gama alanı kullanılarak tanımlanırsa, bu seçenekler doğru ayarlanmalıdır.

* `gammaToLinearMaterial`- Malzeme renklerini gama uzayından doğrusal uzaya dönüştürün
* `gammaToLinearVertex`- Vertex renklerini gama uzayından doğrusal uzaya dönüştürün

> [!NOTE]
> FBX dosyaları için bu `true` ayarlar varsayılan olarak ayarlanır. Diğer tüm dosya türleri için `false`varsayılan değer.

### <a name="scene-parameters"></a>Sahne parametreleri

* `sceneGraphMode`- Kaynak dosyadaki sahne grafiğinin nasıl dönüştürüldüğünü tanımlar:
  * `dynamic`(varsayılan): Dosyadaki tüm nesneler API'de [varlık](../../concepts/entities.md) olarak açıklanır ve bağımsız olarak dönüştürülebilir. Çalışma zamanındaki düğüm hiyerarşisi kaynak dosyadaki yapıyla aynıdır.
  * `static`: Tüm nesneler API'de açıktadır, ancak bağımsız olarak dönüştürülemez.
  * `none`: Sahne grafiği tek bir nesneye çökertilir.

Her mod farklı çalışma zamanı performansına sahiptir. Modda, `dynamic` performans maliyeti, hiçbir parça taşınmasa bile grafikteki [varlık](../../concepts/entities.md) sayısıyla doğrusal ölçeklenir. Yalnızca, örneğin bir 'patlama görünümü' animasyonu için, uygulama için parçaları tek tek hareket ettirdiğinizde kullanılmalıdır.

`static` Mod tam sahne grafiğini dışa aktarır, ancak bu grafiğin içindeki parçalar kök parçasına göre sabit bir dönüşüme sahiptir. Ancak nesnenin kök düğümü önemli bir performans maliyeti olmadan yine de taşınabilir, döndürülebilir veya ölçeklenebilir. Ayrıca, [uzamsal sorgular](../../overview/features/spatial-queries.md) tek tek parçaları döndürecek ve her parça [durum geçersiz kılmaları](../../overview/features/override-hierarchical-state.md)ile değiştirilebilir. Bu modda, nesne başına çalışma süresi yükü ihmal edilebilir. Nesne başına denetime hala ihtiyaç duyduğunuz ancak nesne başına dönüşüm değişikliklerinin olmadığı büyük sahneler için idealdir.

Mod, `none` en az çalışma süresi ne yüküne ve aynı zamanda biraz daha iyi yükleme süreye sahiptir. Bu modda tek nesnelerin incelenmesi veya dönüştürülmesi mümkün değildir. Kullanım örnekleri, örneğin, ilk etapta anlamlı bir sahne grafiği olmayan fotogrametri modelleridir.

> [!TIP]
> Birçok uygulama birden çok model yükler. Nasıl kullanılacağına bağlı olarak her model için dönüşüm parametrelerini optimize etmeniz gerekir. Örneğin, kullanıcının parçalayıp ayrıntılı olarak incelemesi için bir otomobilin modelini görüntülemek istiyorsanız, `dynamic` onu modile dönüştürmeniz gerekir. Ancak, ayrıca bir gösteri odası ortamında araba yerleştirmek istiyorsanız, bu model `sceneGraphMode` ayarlanmış `static` veya `none`hatta dönüştürülebilir.

### <a name="physics-parameters"></a>Fizik parametreleri

* `generateCollisionMesh`- Bir modelde [uzamsal sorgular](../../overview/features/spatial-queries.md) için desteğe ihtiyacınız varsa, bu seçeneğin etkinleştirilmesi gerekir. En kötü durumda, bir çakışan kafes oluşturulması dönüşüm süresini iki katına çıkarabilir. Çakışma meshes ile modeller yüklemek `dynamic` için daha uzun sürer ve bir sahne grafiği kullanırken, onlar da daha yüksek bir çalışma zamanı performansı yükü var. Genel en iyi performans için, uzamsal sorgulara gerek duymadığınız tüm modellerde bu seçeneği devre dışı bmelisiniz.

### <a name="unlit-materials"></a>Yanmamış malzemeler

* `unlitMaterials`- Varsayılan olarak dönüştürme [PBR malzemeleri](../../overview/features/pbr-materials.md)oluşturmak için tercih edecektir. Bu seçenek, dönüştürücüye tüm malzemeleri [renk malzemesi](../../overview/features/color-materials.md) olarak ele alamasını söyler. Fotogrametri yoluyla oluşturulan modeller gibi aydınlatmayı zaten içeren verileriniz varsa, bu [seçenek, her malzemeyi](override-materials.md) ayrı ayrı geçersiz kılmaya gerek kalmadan tüm malzemeler için doğru dönüştürmeyi hızlı bir şekilde zorlamanızı sağlar.

### <a name="converting-from-older-fbx-formats-with-a-phong-material-model"></a>Phong malzeme modeliyle eski FBX biçimlerinden dönüştürme

* `fbxAssumeMetallic`- FBX formatının eski sürümleri bir Phong malzeme modeli kullanarak malzemelerini tanımlar. Dönüştürme işlemi, bu malzemelerin renderörün [PBR modeline](../../overview/features/pbr-materials.md)nasıl eşlenebildiğini ortaya çıkarmıştır. Genellikle bu iyi çalışır, ancak bir malzeme hiçbir dokular, yüksek aynasal değerler ve gri olmayan albedo renk olduğunda bir belirsizlik ortaya çıkabilir. Bu durumda, dönüşüm yüksek aynasal değerleri öncelik arasında seçim yapmak zorunda, albedo renk çözülür son derece yansıtıcı, metalik malzeme tanımlayan, ya da albedo renk öncelik, parlak renkli plastik gibi bir şey tanımlayan. Varsayılan olarak, dönüştürme işlemi, belirsizliğin geçerli olduğu durumlarda yüksek aynasal değerlerin metalik bir malzeme anlamına geldiğini varsayar. Bu parametre tam `false` tersi geçmek için ayarlanabilir.

### <a name="coordinate-system-overriding"></a>Koordinat sistemi geçersiz kılma

* `axis`- Koordinat sistemi birim vektörlerini geçersiz kılmak için. Varsayılan değerler. `["+x", "+y", "+z"]` Teorik olarak, FBX biçimi, bu vektörlerin tanımlandığı ve dönüştürmenin sahneyi dönüştürmek için bu bilgileri kullandığı bir üstbilgiye sahiptir. glTF biçimi de sabit bir koordinat sistemi tanımlar. Uygulamada, bazı varlıkların üstbilgilerinde yanlış bilgiler vardır veya farklı bir koordinat sistemi kuralıyla kaydedilir. Bu seçenek, telafi etmek için koordinat sistemini geçersiz kılmanızı sağlar. Örneğin: `"axis" : ["+x", "+z", "-y"]` Z ekseni ve Y eksenini değiştirir ve Y ekseni yönünü ters çevirerek koordinat sistemini tutar.

### <a name="vertex-format"></a>Tepe noktası biçimi

Bellek tasarrufu için hassas ticaret, bir kafes için vertex biçimini ayarlamak mümkündür. Daha düşük bellek ayak izi, daha büyük modelleri yüklemenize veya daha iyi performans elde etmenizi sağlar. Ancak, verilerinize bağlı olarak, yanlış biçim işleme kalitesini önemli ölçüde etkileyebilir.

> [!CAUTION]
> Vertex biçimini değiştirmek, modeller artık belleğe sığmıyorsa veya mümkün olan en iyi performansı optimize ederken son çare olmalıdır. Değişiklikler kolayca render eserler, hem bariz olanlar ve ince olanları tanıtmak olabilir. Nelere dikkat edeceğinibilmiyorsanız, varsayılanı değiştirmemelisiniz.

Bu ayarlamalar mümkündür:

* Belirli veri akışları açıkça dahil edilebilir veya dışlanabilir.
* Bellek ayak izini azaltmak için veri akışlarının doğruluğu azaltılabilir.

Dosyadaki `vertex` `.json` aşağıdaki bölüm isteğe bağlıdır. Açıkça belirtilmeyen her bölüm için dönüştürme hizmeti varsayılan ayarına geri döner.

```json
{
    ...
    "vertex" : {
        "position"  : "32_32_32_FLOAT",
        "color0"    : "NONE",
        "color1"    : "NONE",
        "normal"    : "NONE",
        "tangent"   : "NONE",
        "binormal"  : "NONE",
        "texcoord0" : "32_32_FLOAT",
        "texcoord1" : "NONE"
    },
    ...
```

Bir bileşeni `NONE`zorlayarak, çıktı kafesinin ilgili akışa sahip olmadığı garanti edilir.

#### <a name="component-formats-per-vertex-stream"></a>Vertex akışı başına bileşen biçimleri

Bu biçimlere ilgili bileşenler için izin verilir:

| Vertex bileşeni | Desteklenen biçimler (bold = varsayılan) |
|:-----------------|:------------------|
|konum| **32_32_32_FLOAT**, 16_16_16_16_FLOAT |
|renkli 0| **8_8_8_8_UNSIGNED_NORMALIZED**, YOK |
|renk1| 8_8_8_8_UNSIGNED_NORMALIZED, **YOK**|
|Normal| **8_8_8_8_SIGNED_NORMALIZED**, 16_16_16_16_FLOAT, YOK |
|Teğet| **8_8_8_8_SIGNED_NORMALIZED**, 16_16_16_16_FLOAT, YOK |
|binormal| **8_8_8_8_SIGNED_NORMALIZED**, 16_16_16_16_FLOAT, YOK |
|texcoord0| **32_32_FLOAT**, 16_16_FLOAT, YOK |
|texcoord1| **32_32_FLOAT**, 16_16_FLOAT, YOK |

#### <a name="supported-component-formats"></a>Desteklenen bileşen biçimleri

Biçimlerin bellek ayak izleri aşağıdaki gibidir:

| Biçimlendir | Açıklama | Tepe noktası başına bayt |
|:-------|:------------|:---------------|
|32_32_FLOAT|iki bileşenli tam kayan nokta hassasiyeti|8
|16_16_FLOAT|iki bileşenli yarım kayan nokta hassasiyeti|4
|32_32_32_FLOAT|üç bileşenli tam yüzer nokta hassasiyeti|12
|16_16_16_16_FLOAT|dört bileşenli yarım kayan nokta hassasiyeti|8
|8_8_8_8_UNSIGNED_NORMALIZED|dört bileşenli bayt, `[0; 1]` aralık normalleştirilmiş|4
|8_8_8_8_SIGNED_NORMALIZED|dört bileşenli bayt, `[-1; 1]` aralık normalleştirilmiş|4

#### <a name="best-practices-for-component-format-changes"></a>Bileşen biçimi değişiklikleri için en iyi uygulamalar

* `position`: Azaltılmış doğruluğun yeterli olması nadirdir. **16_16_16_16_FLOAT** küçük modeller için bile, fark niceliksel eserler tanıttı.
* `normal`, `tangent` `binormal`, : Genellikle bu değerler birlikte değiştirilir. Normal niceleme sonucu fark aydınlatma yapıları olmadığı sürece, bunların doğruluğunu artırmak için bir neden yoktur. Ancak bazı durumlarda, bu bileşenler **NONE**olarak ayarlanabilir:
  * `normal`, `tangent`ve `binormal` yalnızca modeldeki en az bir malzeme yakılması gerektiğinde gereklidir. ARR'da bu durum, modelde herhangi bir zamanda bir [PBR malzemesi](../../overview/features/pbr-materials.md) kullanıldığında geçerlidir.
  * `tangent`ve `binormal` yalnızca aydınlatılmış malzemelerden herhangi biri normal bir harita dokusu kullandığında gereklidir.
* `texcoord0`, `texcoord1` : Doku koordinatları, değerleri `[0; 1]` aralıkta kaldığında ve adreslenen dokuların maksimum boyutu 2048 x 2048 piksel olduğunda azaltılmış doğruluk **(16_16_FLOAT)** kullanabilir. Bu sınırlar aşılırsa, doku eşleme kalitesi zarar görecektir.

#### <a name="example"></a>Örnek

Eğer dokular içine pişmiş aydınlatma olan bir fotogrametri modeli varyalım. Modeli işlemek için gereken tek şey tepe noktası konumları ve doku koordinatlarıdır.

Varsayılan olarak dönüştürücü bir zaman bir model üzerinde PBR malzemeleri kullanmak isteyebilirsiniz varsaymak zorundadır, bu yüzden sizin için , `normal` `tangent`ve `binormal` veri üretecek. Sonuç olarak, tepe noktası başına `position` bellek kullanımı (12 `texcoord0` bayt) + `normal` (8 bayt) `tangent` + (4 bayt) + (4 bayt) + `binormal` (4 bayt) = 32 bayt' tir. Bu tür büyük modeller kolayca bellek birden fazla gigabayt sürebilir modelleri sonuçlanan vertices milyonlarca olabilir. Bu kadar büyük miktarda veri performansı etkiler ve hatta bellek tükenebilir.

Modelde dinamik aydınlatmaya asla ihtiyacınız olmadığını bilmek ve tüm `[0; 1]` doku koordinatlarının `normal`aralıkta olduğunu `texcoord0` bilerek, tepe`16_16_FLOAT`noktası başına sadece 16 bayt la sonuçlanan , `tangent`ve `binormal` `NONE` yarı hassasiyete () ayarlayabilirsiniz. Kafes verilerinin yarıya inmesini sağlamak, daha büyük modeller yüklemenize olanak tanır ve performansı potansiyel olarak artırır.

## <a name="typical-use-cases"></a>Tipik kullanım örnekleri

Belirli bir kullanım örneği için iyi alma ayarları bulmak sıkıcı bir işlem olabilir. Diğer taraftan, dönüşüm ayarlarının çalışma zamanı performansı üzerinde önemli bir etkisi olabilir.

Belirli optimizasyonlar için uygun kullanım örnekleri belirli sınıflar vardır. Bazı örnekler aşağıda verilmiştir.

### <a name="use-case-architectural-visualization--large-outdoor-maps"></a>Kullanım örneği: Mimari görselleştirme / büyük dış mekan haritaları

* Bu tür sahneler statik olma eğilimindedir, yani hareketli parçalara ihtiyaç duymaz. Buna göre, `sceneGraphMode` çalışma zamanı `static` performansını `none`artıran, hatta ayarlanabilir. Mod `static` ile, sahnenin kök düğümü yine de taşınabilir, döndürülebilir ve ölçeklenebilir, örneğin 1:1 ölçeği (birinci kişi görünümü için) ve tablo üstü görünümü arasında dinamik olarak geçiş yapmak için.

* Parçaları hareket ettirmeniz gerektiğinde, bu genellikle ray yayınları veya diğer [uzamsal sorgular](../../overview/features/spatial-queries.md)için desteğe ihtiyacınız olduğu anlamına gelir, böylece bu parçaları ilk etapta seçebilirsiniz. Öte yandan, bir şeyi hareket ettirmeyi düşünmüyorsanız, uzamsal sorgulara katılmak için buna da gerek duymama nız `generateCollisionMesh` ve bu nedenle bayrağı kapatabilmeniz olasılığı yüksektir. Bu anahtarın dönüşüm süreleri, yükleme süreleri ve ayrıca çerçeve başına çalışma süresi güncelleştirme maliyetleri üzerinde önemli bir etkisi vardır.

* Uygulama [kesme düzlemleri](../../overview/features/cut-planes.md)kullanmıyorsa, `opaqueMaterialDefaultSidedness` bayrak kapatılmalıdır. Performans artışı genellikle %20-%30'dur. Kesme düzlemleri hala kullanılabilir, ancak nesnelerin iç kısımlarına bakarken arka yüzler olmaz, bu da sezgisel olmayan bir görünüme sahip olur. Daha fazla bilgi için [tek taraflı işleme](../../overview/features/single-sided-rendering.md)ye bakın.

### <a name="use-case-photogrammetry-models"></a>Kullanım örneği: Fotogrametri modelleri

Fotogrametri modellerini işlerken genellikle bir sahne grafiğine gerek yoktur, `sceneGraphMode` `none`böylece . Bu modeller nadiren başlangıçta karmaşık bir sahne grafiği içerdiğinden, bu seçeneğin etkisi önemsiz olmalıdır.

Aydınlatma zaten dokular içine pişmiş olduğundan, hiçbir dinamik aydınlatma gereklidir. Bu nedenle:

* Tüm `unlitMaterials` malzemeleri `true` ışıksız [renkli malzemelere](../../overview/features/color-materials.md)dönüştürmek için bayrağı ayarlayın.
* Vertex biçiminden gereksiz verileri kaldırın. Yukarıdaki [örneğe](#example) bakın.

### <a name="use-case-visualization-of-compact-machines-etc"></a>Kullanım örneği: Kompakt makinelerin görselleştirilmesi, vb.

Bu kullanım durumlarında, modeller genellikle küçük bir hacim içinde çok yüksek ayrıntıya sahiptir. İşleyici, bu gibi durumları iyi işlemek için yoğun olarak optimize edicisidir. Ancak, önceki kullanım örneğinde belirtilen optimizasyonların çoğu burada geçerli değildir:

* Tek tek parçalar seçilebilir ve taşınabilir `sceneGraphMode` olmalıdır, bu `dynamic`nedenle .
* Işın dökümleri genellikle uygulamanın ayrılmaz bir parçasıdır, bu nedenle çarpışma meshes oluşturulmalıdır.
* Kesme düzlemleri `opaqueMaterialDefaultSidedness` bayrak etkinken daha iyi görünür.

## <a name="next-steps"></a>Sonraki adımlar

* [Model dönüştürme](model-conversion.md)
* [Renkli malzemeler](../../overview/features/color-materials.md)
* [PBR malzemeleri](../../overview/features/pbr-materials.md)
* [Model dönüştürme sırasında malzemeleri geçersiz kılma](override-materials.md)
