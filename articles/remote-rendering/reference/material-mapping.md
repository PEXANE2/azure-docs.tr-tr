---
title: Model biçimleri için malzeme eşleme
description: Model kaynak biçimlerinden PBR malzemesine varsayılan dönüştürmeyi açıklar
author: jakrams
ms.author: jakras
ms.date: 02/11/2020
ms.topic: reference
ms.openlocfilehash: ce287ed94066aac4b900d2ddb02579a54b8550f6
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80680394"
---
# <a name="material-mapping-for-model-formats"></a>Model biçimleri için malzeme eşleme

Bir kaynak varlık [bir model olarak dönüştürüldüğünde,](../how-tos/conversion/model-conversion.md)dönüştürücü her [kafes](../concepts/meshes.md)için [malzeme](../concepts/materials.md) oluşturur. Malzemelerin oluşturulma şekli [geçersiz](../how-tos/conversion/override-materials.md)kılınabilir. Ancak varsayılan olarak dönüştürme [PBR malzemeleri](../overview/features/pbr-materials.md)oluşturur. FBX gibi her kaynak dosya biçimi malzemeleri tanımlamak için kendi kurallarını kullandığından, bu kuralların Azure Uzaktan İşleme'nin PBR malzeme parametrelerine eşlemesi gerekir. 

Bu makalede, kaynak varlıklardan çalışma zamanı malzemelerine malzeme dönüştürmek için kullanılan tam eşlemeler listelenir.

## <a name="gltf"></a>glTF

GlTF 2.0 spec hemen hemen her şey *EmissiveFactor* ve *EmissiveTexture*dışında, Azure Uzaktan Render desteklenir.

Aşağıdaki tablo eşleme gösterir:

| glTF | Azure Remote Rendering |
|:-------------------|:--------------------------|
|   baseColorFactor   |   albedoColor              |
|   baseColorTexture  |   albedoMap                |
|   metallicFactor    |   metallik                |
|   metallicTexture   |   metalnessMap             |
|   pürüzlülükFactor   |   Pürüz                |
|   pürüzlülükTexture  |   pürüzlülükMap             |
|   oklüzyonFaktörü   |   Tıkanık -lığı                |
|   oklüzyonTexture  |   oklüzyonHaritası             |
|   normalDoku     |   normalHarita                |
|   alphaCutoff       |   alphaClipThreshold       |
|   alphaMode.OPAQUE  |   alphaClipEnabled = false, isTransparent = false |
|   alphaMode.MASK    |   alphaClipEnabled = true, isTransparent = false  |
|   alphaMode.BLEND   |   isTransparent = true     |
|   doubleSided       |   isDoubleSided            |
|   emissiveFactor    |   -                        |
|   emissiveTexture   |   -                        |

glTF'deki her dokunun `texCoord` bir değeri olabilir ve bu da Azure Uzaktan İşleme malzemelerinde desteklenir.

### <a name="embedded-textures"></a>Gömülü dokular

* \*.bin* veya * \*.glb* dosyalarına katıştılan dokular desteklenir.

### <a name="supported-gltf-extension"></a>Desteklenen glTF uzantısı

Temel özellik kümesine ek olarak, Azure Uzaktan İşleme aşağıdaki glTF uzantılarını destekler:

* [MSFT_packing_occlusionRoughnessMetallic](https://github.com/KhronosGroup/glTF/blob/master/extensions/2.0/Vendor/MSFT_packing_occlusionRoughnessMetallic/README.md)
* [MSFT_texture_dds](https://github.com/KhronosGroup/glTF/blob/master/extensions/2.0/Vendor/MSFT_texture_dds/README.md)
* [KHR_materials_unlit](https://github.com/KhronosGroup/glTF/blob/master/extensions/2.0/Khronos/KHR_materials_unlit/README.md): [Renkli malzemelere](../overview/features/color-materials.md)karşılık gelir. *Emissive* malzemeler için, bu uzantısı kullanılması tavsiye edilir.
* [KHR_materials_pbrSpecularGlossiness](https://github.com/KhronosGroup/glTF/blob/master/extensions/2.0/Khronos/KHR_materials_pbrSpecularGlossiness/README.md): Metalik pürüzlülük dokuları yerine, diffüz-aynasal-parlaklık dokuları sağlayabilirsiniz. Azure Uzaktan İşleme uygulaması, uzantıdaki dönüşüm formüllerini doğrudan izler.

## <a name="fbx"></a>Fbx

FBX formatı kapalı kaynakkodludur ve FBX malzemeleri genel olarak PBR malzemelerle uyumlu değildir. FBX, birçok benzersiz parametre ve özelliye sahip yüzeylerin karmaşık bir açıklamasını kullanır ve **bunların tümü Azure Uzaktan İşleme ardışık hattı tarafından kullanılmaz.**

> [!IMPORTANT]
> Azure Uzaktan İşleme modeli dönüştürme ardışık alanı yalnızca **FBX 2011 ve üzerini**destekler.

FBX biçimi malzemeler için muhafazakar bir yaklaşım tanımlar, resmi FBX belirtiminde sadece iki türü vardır:

* *Lambert* - Yaygın zaten oldukça uzun bir süre için kullanılan değil, ama yine de dönüşüm zamanında Phong dönüştürerek desteklenir.
* *Phong* - Hemen hemen tüm malzemeler ve en içerik araçları bu türü kullanın.

Phong modeli daha doğru ve FBX malzemeler için *tek* model olarak kullanılır. Aşağıda *FBX Malzeme*olarak anılacaktır.

> Maya, bir malzemenin PBR ve Stingray türleri için özel özellikler tanımlayarak FBX için iki özel uzantı kullanır. Bu ayrıntılar FBX belirtimine dahil edilmeyin, bu nedenle şu anda Azure Uzaktan İşleme tarafından desteklenmez.

FBX Malzemeler Diffuse-Specular-SpecularLevel kavramını kullanmak, bu yüzden bir albedo harita için yaygın bir doku dönüştürmek için biz diffüz onları çıkarmak için diğer parametreleri hesaplamak gerekir.

> FBX'teki tüm renkler ve dokular sRGB alanındadır (Gama alanı olarak da bilinir) ancak Azure Uzaktan İşleme görselleştirme sırasında doğrusal alanla çalışır ve çerçevenin sonunda her şeyi sRGB alanına dönüştürür. Azure Uzaktan İşleme varlık ardışık alanı, her şeyi doğrusal alana dönüştürerek hazır veri olarak işleyiciye gönderir.

Bu tablo, dokuların FBX Malzemelerinden Azure Uzaktan İşleme malzemelerine nasıl eşlenmediğini gösterir. Bazıları doğrudan kullanılmaz, ancak formüllere katılan diğer dokularla birlikte (örneğin dağınık doku):

| Fbx | Azure Remote Rendering |
|:-----|:----|
| Ortam Rengi | Oklüzyon Haritası   |
| DiffuseColor | *Albedo, Metalness için kullanılır* |
| Transparentcolor | *Albedo alfa kanalı için kullanılan* |
| Şeffaflık Faktörü | *Albedo alfa kanalı için kullanılan* |
| Opak -lık | *Albedo alfa kanalı için kullanılan* |
| Aynarengi | *Albedo, Metalness, Pürüzlülük için kullanılır* |
| SpecularFactor| *Albedo, Metalness, Pürüzlülük için kullanılır* |
| ShininessExponent | *Albedo, Metalness, Pürüzlülük için kullanılır* |
| NormalHarita | NormalHarita |
| Yumru | *NormalMap'e dönüştürüldü* |
| EmissiveColor | - |
| EmissiveFactor | - |
| Yansıma Rengi | - |
| Deplasman Rengi | - |

Yukarıdaki eşleme, yapılması gereken birçok varsayım nedeniyle malzeme dönüştürmenin en karmaşık parçasıdır. Bu varsayımları aşağıda tartışıyoruz.

Aşağıda kullanılan bazı tanımlar:

* `Specular` =  `SpecularColor` * `SpecularFactor`
* `SpecularIntensity` = `Specular`. Kırmızı - 0,2125 + `Specular`. Yeşil - 0,7154 + `Specular`. Mavi - 0,0721
* `DiffuseBrightness`= 0.299 `Diffuse`* . Kırmızı<sup>2</sup> + 0.587 * `Diffuse`. Yeşil<sup>2</sup> + 0.114 * `Diffuse`. Mavi<sup>2</sup>
* `SpecularBrightness`= 0.299 `Specular`* . Kırmızı<sup>2</sup> + 0.587 * `Specular`. Yeşil<sup>2</sup> + 0.114 * `Specular`. Mavi<sup>2</sup>
* `SpecularStrength`= max`Specular`( . Kırmızı, `Specular`. Yeşil, `Specular`. Mavi)

SpecularIntensity formülü [buradan](https://en.wikipedia.org/wiki/Luma_(video))elde edilir.
Parlaklık formülü bu [belirtimde](http://www.itu.int/dms_pubrec/itu-r/rec/bt/R-REC-BT.601-7-201103-I!!PDF-E.pdf)açıklanmıştır.

### <a name="roughness"></a>Pürüz

`Roughness`[bu formülden](https://www.cs.cornell.edu/~srm/publications/EGSR07-btdf.pdf) `Specular` hesaplanır. `ShininessExponent` Formül Phong specular üs pürüzlülük bir yaklaşımdır:

```Cpp
Roughness = sqrt(2 / (ShininessExponent * SpecularIntensity + 2))
```

### <a name="metalness"></a>Metallik

`Metalness``Diffuse` [glTF belirtiminden](https://github.com/bghgary/glTF/blob/gh-pages/convert-between-workflows-bjs/js/babylon.pbrUtilities.js)bu formül kullanılarak hesaplanır. `Specular`

Buradaki fikir denklemi çözmemizdir: Ax<sup>2</sup> + Bx + C = 0.
Temel olarak, dielektrik yüzeyler aynasal bir şekilde ışığın yaklaşık% 4 yansıtır, ve geri kalanı yaygındır. Metalik yüzeyler dağınık bir şekilde hiçbir ışık yansıtır, ama tüm aynalı bir şekilde.
Parlak plastik ve parlak metalik yüzeyleri ayırt etmenin bir yolu yoktur, çünkü bu formülün birkaç dezavantajı vardır. Çoğu zaman yüzeyin metalik özelliklere sahip olduğunu ve dolayısıyla parlak plastik/kauçuk yüzeylerin beklendiği gibi görünmeyebileceğini varsayıyoruz.
```cpp
dielectricSpecularReflectance = 0.04
oneMinusSpecularStrength = 1 - SpecularStrength

A = dielectricSpecularReflectance
B = (DiffuseBrightness * (oneMinusSpecularStrength / (1 - A)) + SpecularBrightness) - 2 * A
C = A - SpecularBrightness
squareRoot = sqrt(max(0.0, B * B - 4 * A * C))
value = (-B + squareRoot) / (2 * A)
Metalness = clamp(value, 0.0, 1.0);
```

### <a name="albedo"></a>Albedo

`Albedo``Diffuse`, , `Specular`ve `Metalness`.

Metalness bölümünde açıklandığı gibi, dielektrik yüzeyler ışığın yaklaşık% 4 yansıtır.  
Burada fikir doğrusal bir faktör `Dielectric` olarak `Metal` değer `Metalness` kullanarak ve renkler arasında interpolate etmektir. Metalness ise `0.0`, o zaman aynaya bağlı olarak ya koyu renk (ayna yüksek ise) ya da diffüz değişmez (hiçbir aynasal varsa). Metallik büyük bir değer ise, o zaman yaygın renk aynasal renk lehine kaybolur.

```Cpp
dielectricSpecularReflectance = 0.04
oneMinusSpecularStrength = 1 - SpecularStrength

dielectricColor = diffuseColor * (oneMinusSpecularStrength / (1.0f - dielectricSpecularReflectance) / max(1e-4, 1.0 - metalness))
metalColor = (Specular - dielectricSpecularReflectance * (1.0 - metalness)) * (1.0 / max(1e-4, metalness))
albedoRawColor = lerpColors(dielectricColor, metalColor, metalness * metalness)
AlbedoRGB = clamp(albedoRawColor, 0.0, 1.0);
```

`AlbedoRGB`yukarıdaki formül tarafından hesaplanmıştır, ancak alfa kanalı ek hesaplamalar gerektirir. FBX biçimi şeffaflık hakkında belirsiz ve tanımlamak için birçok yolu vardır. Farklı içerik araçları farklı yöntemler kullanır. Buradaki amaç onları tek bir formülde birletmek. Ortak bir şekilde oluşturulmazsa, bazı varlıkların hatalı olarak saydam olarak gösterilmesini sağlar.

Bu , `TransparentColor`, , `TransparencyFactor` `Opacity`:

`Opacity` tanımlanırsa, doğrudan kullanın: `AlbedoAlpha`  =  `Opacity` başka  
tanımlanırsa, `TransparencyColor` `AlbedoAlpha` o zaman = 1.0 - ((`TransparentColor`. Kırmızı `TransparentColor`+ . Yeşil `TransparentColor`+ . Mavi) / 3.0) başka  
if `TransparencyFactor`, `AlbedoAlpha` sonra = 1.0 -`TransparencyFactor`

Son `Albedo` renk `AlbedoRGB` ile birleştirerek, dört kanal `AlbedoAlpha`vardır.

### <a name="summary"></a>Özet

Burada özetlemek `Albedo` gerekirse, sıfıra `Diffuse`yakın `Specular` ise, orijinal çok yakın olacaktır. Aksi takdirde yüzey metalik bir yüzey gibi görünür ve dağınık renk kaybeder. Yüzey yeterince büyük ve parlak ise `ShininessExponent` daha parlak `Specular` ve yansıtıcı görünecektir. Aksi takdirde yüzey pürüzlü görünür ve çevreyi zar zor yansıtır.

### <a name="known-issues"></a>Bilinen sorunlar

* Geçerli formül basit renkli geometri için iyi çalışmaz. Yeterince `Specular` parlaksa, tüm geometriler renksiz yansıtıcı metalik yüzeylere dönüşür. Burada geçici çözüm orijinal% 30 düşürmek `Specular` veya dönüşüm ayarı [fbxAssumeMetallic](../how-tos/conversion/configure-model-conversion.md#converting-from-older-fbx-formats-with-a-phong-material-model)kullanmaktır.
* PBR malzemeleri yakın `Maya` zamanda `3DS Max` ve içerik oluşturma araçları eklendi. FBX'e aktarmak için özel kullanıcı tanımlı kara kutu özelliklerini kullanırlar. Azure Uzaktan İşleme, belgelenmedikleri ve biçim kapalı kaynak olduğu için bu ek özellikleri okumaz.

## <a name="next-steps"></a>Sonraki adımlar

* [Model dönüştürme](../how-tos/conversion/model-conversion.md)
* [Model dönüştürme sırasında malzemeleri geçersiz kılma](../how-tos/conversion/override-materials.md)
