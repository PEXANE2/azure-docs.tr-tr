---
title: PBR malzemeleri
description: PBR malzeme türünü açıklar.
author: jakrams
ms.author: jakras
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: 64553506f75451c50a87932904f00a7275ea9286
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80680264"
---
# <a name="pbr-materials"></a>PBR malzemeleri

*PBR malzemeleri* , Azure uzaktan işlemede desteklenen [malzeme türlerinden](../../concepts/materials.md) biridir. Gerçekçi aydınlatma alması gereken [kafesler](../../concepts/meshes.md) için kullanılırlar.

PBR, **P**HYS, **B**ased **R**endering için temsil eder ve malzemenin, bir yüzeyin görsel özelliklerini fiziksel bir şekilde, her aydınlatma koşullarında gerçekçi sonuçlar olabilmesinden kaynaklanan bir şekilde açıkladığı anlamına gelir. Çoğu modern oyun altyapısı ve içerik oluşturma araçları, gerçek zamanlı işleme için gerçek dünya senaryolarının en iyi şekilde değerlendirildikleri için PBR malzemelerini destekler.

![ARR tarafından oluşturulan Helmet glTF örnek modeli](media/helmet.png)

PBR malzemeleri evrensel bir çözüm değildir, ancak. Görüntüleme açısına göre rengi farklı şekilde yansıtan malzemeler vardır. Örneğin, bazı yapılar veya otomobil boyar. Bu tür malzemeler standart PBR modeli tarafından işlenmez ve şu anda Azure uzaktan Işleme tarafından desteklenmemektedir. Bu, *Ince Film* (çok katmanlı yüzeyler) ve *clear-Coat* (otomobil BOYAYMALARı) gibi PBR uzantıları içerir.

## <a name="common-material-properties"></a>Ortak malzeme özellikleri

Bu özellikler tüm malzemelerde ortaktır:

* **Albedocolor:** Bu renk, *Albedomap* veya *köşe renkleri*gibi diğer renklerle çarpılır. Bir malzemeden *Saydamlık* etkinse alfa kanalı, tam donuk ve `1` `0` anlamı tamamen saydam olan opaklığı ayarlamak için kullanılır. Varsayılan değer beyazdır.

  > [!NOTE]
  > Bir PBR malzemesi tamamen saydam olduğunda, mükemmel bir cam taş gibi, ortamı yine de yansıtır. Güneş gibi parlak noktalar hala yansıma içinde görünür. Bu [renk malzemeleri](color-materials.md)için farklıdır.

* **Albedomap:** Piksel başına Albedo değerleri için [2B doku](../../concepts/textures.md) .

* **alphaclipenabled** ve **alphaclipthreshold:** *alphaclipenabled* değeri true ise, Albedo Alpha değerinin *alphaclipthreshold* değerinden düşük olduğu tüm pikseller çizilmez. Alfa kırpması, saydamlığı etkinleştirmeden ve işlemek için çok daha hızlı bir şekilde kullanılabilir. Alfa kırpılan malzemeler, ancak tamamen opak malzemelerden daha yavaş işleme almaya devam etmektedir. Varsayılan olarak Alfa kırpması devre dışıdır.

* **textureCoordinateScale** ve **TEXTURECOORDINATEOFFSET:** ölçek UV doku koordinatlarına çarpılır, bu da ona eklenir. Dokuları uzatmak ve kaydırmak için kullanılabilir. Varsayılan ölçek (1, 1) ve konum (0, 0).

* **Usevertexcolor:** Kafes köşe renkleri içeriyorsa ve bu seçenek etkinleştirilirse, kafeslerin köşe renkleri *Albedocolor* ve *Albedomap*ile çarpılır. Varsayılan olarak köşe renkleri devre dışıdır.

* **ıdoubleyüzlü:** Çift sidebir değer true olarak ayarlanırsa, kamera arka yüzlerine baksa bile bu malzemeden üçgenler işlenir. PBR malzemeleri için aydınlatma, arka yüzlere uygun olarak da hesaplanır. Varsayılan olarak bu seçenek devre dışıdır. Ayrıca bkz. [tek taraflı işleme](single-sided-rendering.md).

## <a name="pbr-material-properties"></a>PBR malzeme özellikleri

Fiziksel olarak temel işlemenin temel fikri, çok çeşitli gerçek dünyada malzemelere öykünmek için *Basecolor*, *Metalness*ve *kablık* özelliklerini kullanmaktır. PBR 'nin ayrıntılı bir açıklaması Bu makalenin kapsamı dışındadır. PBR hakkında daha fazla bilgi için bkz. [diğer kaynaklar](http://www.pbr-book.org). Aşağıdaki özellikler, PBR malzemelere özgüdür:

* **Basecolor:** PBR malzemeleri ' nde, *Albedo rengi* *temel renk*olarak adlandırılır. Azure uzaktan Işlemede *Albedo Color* özelliği ortak malzeme özellikleri aracılığıyla zaten mevcut olduğundan, ek bir temel renk özelliği yoktur.

* **kabalık** ve **kabonlik** , yüzlü, yüzeyin ne kadar kaba veya düzgün olduğunu tanımlar. Kaba yüzeyler, ışığın keskin yüzeylere göre daha fazla yönde dağılımı ve bu da yansımaları keskin hale getirir. Değer aralığı `0.0` - `1.0`' dır. Eşit `roughness` `0.0`olduğunda, yansımalar keskin olur. `roughness` Eşitse `0.5`, yansıtımları bulanık hale gelir.

  Hem bir kabın değeri hem de bir kabonu eşleme sağlanırsa, son değer iki ürünün çarpımı olur.

* **metalness** ve **metalnessMap:** bu özellik, bir yüzeyin iletken ya da dielektrik olup olmadığına karşılık gelir. İletken malzemeler farklı yansıtmalı özelliklere sahiptir ve Albedo rengi olmadan yansıtıcı olarak eğilimlidir. PBR malzemelerde, bu özellik, çevreleyen ortamı ne kadar bir yüzey yansıttığını etkiler. Değerler ile `1.0`arasında `0.0` değişir. Metalness olduğunda `0.0`, Albedo rengi tamamen görünür olur ve malzemeler plastik ya da ceramikler gibi görünür. Metalness olduğunda `0.5`, boyanmış metal gibi görünür. Metalness `1.0`olduğunda yüzey, Albedo rengini neredeyse kaybeder ve yalnızca surlaları yansıtır. `metalness` Örneğin, `1.0` ve `roughness` `0.0` ise, bir yüzey gerçek dünya yansıtmaya benzer şekilde görünür.

  Hem bir metalness değeri hem de bir metalness eşlemesi sağlanırsa, son değer iki ürünün çarpımı olur.

  ![metalness ve kablık](./media/metalness-roughness.png)

  Yukarıdaki resimde, sağ alt köşedeki Sphere gerçek metal malzemeler gibi görünür ve sol alt, Ceramik veya plastik gibi görünür. Albedo rengi Ayrıca fiziksel özelliklere göre değişiyor. Büyük bir artış sayesinde, malzeme yansıma keskinliğini kaybeder.

* **NormalMap:** İnce ayrıntıların benzetimini yapmak için, [normal bir eşleme](https://en.wikipedia.org/wiki/Normal_mapping) sağlanverilebilir.

* **Occlusionmap** ve **aoscale:** [ortam occlusiyon](https://en.wikipedia.org/wiki/Ambient_occlusion) , occluya ve daha gerçekçi alanlara gölge ekleyerek nesnelerin daha gerçekçi görünmesini sağlar. `0.0` Occlusiyon değeri `1.0`, değerinden (occluded) `0.0` anlamına gelir ve `1.0` hiçbir anlamı yoktur. Bir 2B doku bir occlusiyon eşlemesi olarak sağlanıyorsa, efekt etkinleştirilir ve *AOCE ölçeği* bir çarpan gibi davranır.

  ![Occlusiyon eşleme](./media/boom-box-ao2.gif)

* **saydam:** PBR malzemeleri için yalnızca bir saydamlık ayarı vardır: etkin veya değildir. Opaklık, Albedo renginin alfa kanalı tarafından tanımlanır. Etkinleştirildiğinde yarı saydam yüzeyler çizmek için daha karmaşık bir işleme işlem hattı çağrılır. Azure uzaktan Işleme, gerçek [sıra bağımsız saydamlığı](https://en.wikipedia.org/wiki/Order-independent_transparency) (OIT) uygular.

  Saydam geometri işleme pahalıdır. Yalnızca bir yüzeydeki deliklere ihtiyacınız varsa (örneğin, bir ağacın yaprakları için), bunun yerine Alfa kırpması kullanmak daha iyidir.

  ![Yukarıdaki](./media/transparency.png) görüntüde, en sağdaki Sphere 'in tamamen saydam olduğu, ancak yansıma hala görünür durumda olan saydamlık bildirimi.

  > [!IMPORTANT]
  > Çalışma zamanında, herhangi bir malzemenin donuk ' den saydam 'e geçiş olması gerekiyorsa, oluşturucunun *Tilebasedcomposition* [işleme modunu](../../concepts/rendering-modes.md)kullanması gerekir. Bu sınırlama, ile başlamak için saydam malzemeler olarak dönüştürülmüş malzemeler için geçerlidir.

## <a name="technical-details"></a>Teknik ayrıntılar

Azure uzaktan Işleme, GGX NDF, Schlick Fresnel düşüşünü ve bir Lambx Smith bağıntılı görünürlük terimi olan Cook-Torrance Micro-model BRDF 'ı ve Lambert dağıtma terimiyle birlikte kullanır. Bu model, şu anda endüstri standardıdır. Daha ayrıntılı bilgi için şu makaleye bakın: [fiziksel tabanlı işleme-Cook Torrance](http://www.codinglabs.net/article_physically_based_rendering_cook_torrance.aspx)

 Azure uzaktan Işlemede kullanılan *Metalness-Kabghize* yönelik PBR modelinin bir alternatifi, *Yansımalı* bir ve daha fazla kullanım PBR modelidir. Bu model, daha geniş bir malzeme aralığını temsil edebilir. Ancak, daha pahalıdır ve genellikle gerçek zamanlı durumlarda iyi çalışmaz.
( *Basecolor, Metalness)* öğesine dönüştürülemeyen *(dağıtılmış, yansımalı)* değer çiftleri olduğu Için, *Yansımalı bir ışıldan* *Metalness-kabıya* dönüştürme her zaman mümkün değildir. Tüm *(Basecolor, Metalness)* çiftleri iyi tanımlanmış *(dağıtılmış, yansımalı)* çiftlerine karşılık geldiğinden, diğer yönde dönüştürme daha basit ve daha kesin bir hale gelir.

## <a name="next-steps"></a>Sonraki adımlar

* [Renk malzemeleri](color-materials.md)
* [Dokular](../../concepts/textures.md)
* [Ağ yapıları](../../concepts/meshes.md)
