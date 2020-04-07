---
title: PBR malzemeleri
description: PBR malzeme türünü açıklar.
author: jakrams
ms.author: jakras
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: 64553506f75451c50a87932904f00a7275ea9286
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80680264"
---
# <a name="pbr-materials"></a>PBR malzemeleri

*PBR malzemeleri,* Azure Uzaktan İşleme'de desteklenen [malzeme türlerinden](../../concepts/materials.md) biridir. Onlar gerçekçi aydınlatma almalısınız [meshes](../../concepts/meshes.md) için kullanılır.

PBR **P**hysically **B**ased **R**endering anlamına gelir ve malzeme fiziksel olarak makul bir şekilde bir yüzeyin görsel özelliklerini açıklar anlamına gelir, gerçekçi sonuçlar tüm Aydınlatma koşulları altında mümkün olduğu gibi. Çoğu modern oyun motorları ve içerik oluşturma araçları PBR malzemelerini destekler, çünkü gerçek zamanlı görüntüleme için gerçek dünya senaryolarının en iyi yaklaşımları olarak kabul edilirler.

![ARR tarafından işlenen kask glTF örnek modeli](media/helmet.png)

Ancak PBR malzemeleri evrensel bir çözüm değildir. Görüntüleme açısına bağlı olarak renkleri farklı yansıtan malzemeler vardır. Örneğin, bazı kumaşlar veya araba boyaları. Bu tür malzemeler standart PBR modeli tarafından işlenmez ve şu anda Azure Uzaktan İşlemtarafından desteklenmez. Buna *İnce Film* (çok katmanlı yüzeyler) ve *Clear-Coat* (otomobil boyaları için) gibi PBR uzantıları dahildir.

## <a name="common-material-properties"></a>Ortak malzeme özellikleri

Bu özellikler tüm malzemeler için ortaktur:

* **albedoColor:** Bu renk *albedoMap* veya *vertex renkler*gibi diğer renkler ile çarpılır. Bir malzemeüzerinde *saydamlık* etkinse, alfa kanalı opaklığı `1` ayarlamak için kullanılır, `0` anlamı tamamen opak ve anlamı tamamen saydamdır. Varsayılan değer beyazdır.

  > [!NOTE]
  > Bir PBR malzemesi tamamen saydam olduğunda, mükemmel temiz bir cam parçası gibi, hala çevreyi yansıtır. Güneş gibi parlak noktalar yansımada hala görülebilir. Bu renk [malzemeleri](color-materials.md)için farklıdır.

* **albedoMap:** Piksel başına albedo değerleri için [2B doku.](../../concepts/textures.md)

* **alphaClipEnabled** ve **alphaClipThreshold:** *alphaClipEnabled* doğruysa, albedo alfa değerinin *alphaClipThreshold'den* daha düşük olduğu tüm pikseller çizilmez. Alfa kırpma bile saydamlık etkinleştirmeden kullanılabilir ve işlemek için çok daha hızlıdır. Alfa kırpılmış malzemeler hala tam opak malzemeler daha işlemek için daha yavaş olsa da. Varsayılan olarak alfa kırpma devre dışı bırakılır.

* **textureCoordinateScale** ve **textureCoordinateOffset:** Ölçek UV doku koordinatları içine çarpılır, ofset eklenir. Dokuları germek ve kaydırmak için kullanılabilir. Varsayılan ölçek (1, 1) ve ofset (0, 0) 'dir.

* **useVertexColor:** Kafes tepe noktası renkleri içeriyorsa ve bu seçenek etkinse, kafeslerin tepe noktası renkleri *albedoColor* ve *albedoMap*ile çarpılır. Varsayılan olarak vertex renkleri devre dışı bırakılır.

* **isDoubleSided:** Çift taraflılık gerçek olarak ayarlanmışsa, kamera arka yüzlerine baksa bile bu malzemeye sahip üçgenler işlenir. PBR malzemeleri için aydınlatma da arka yüzler için düzgün bir şekilde hesaplanır. Varsayılan olarak bu seçenek devre dışı bırakılır. Ayrıca bakınız [Tek taraflı görüntüleme.](single-sided-rendering.md)

## <a name="pbr-material-properties"></a>PBR malzeme özellikleri

Fiziksel tabanlı render temel fikir Gerçek dünya malzemelerin geniş bir yelpazede taklit etmek için *BaseColor*, *Metalness*ve *Pürüzlülük* özellikleri kullanmaktır. PBR'nin ayrıntılı bir açıklaması bu makalenin kapsamı dışındadır. PBR hakkında daha fazla bilgi için [diğer kaynaklara](http://www.pbr-book.org)bakın. Aşağıdaki özellikler PBR malzemelerine özgüdir:

* **baseColor:** PBR malzemelerde *albedo rengi* *temel renk*olarak adlandırılır. Azure Uzaktan İşleme'de *albedo renk* özelliği ortak malzeme özellikleri aracılığıyla zaten mevcut olduğundan, ek temel renk özelliği yoktur.

* **pürüzlülük** ve **pürüzlülükHarita:** Pürüzlülük yüzeyinne ne kadar pürüzlü veya pürüzsüz olduğunu tanımlar. Pürüzlü yüzeyler ışığı pürüzsüz yüzeylere göre daha fazla yöne dağıtır ve bu da yansımaları keskin den ziyade bulanık laştırır. Değer aralığı `0.0` `1.0`' ndan . Eşit `roughness` olduğunda, `0.0`yansımalar keskin olacaktır. Eşit `roughness` olduğunda, `0.5`yansımalar bulanık laşır.

  Hem pürüz değeri hem de pürüzlülük haritası sağlanırsa, son değer bu ikisinin ürünü olur.

* **metalness** ve **metalnessMap:** Fizikte, bu özellik bir yüzeyin iletken veya dielektrik olup olmadığına karşılık gelir. İletken malzemeler farklı yansıtıcı özelliklere sahiptir ve albedo rengi olmayan yansıtıcı olma eğilimindedirler. PBR malzemelerinde bu özellik, bir yüzeyin çevreye ne kadar yansıdığını etkiler. Değerler ' `0.0` `1.0`den ' e kadar Metallik olduğunda, `0.0`albedo rengi tamamen görünür ve malzeme plastik veya seramik gibi görünüyor. Metallik olduğunda, `0.5`boyalı metal gibi görünür. Metallik olduğunda, `1.0`yüzey neredeyse tamamen albedo rengini kaybeder ve sadece çevreyi yansıtır. Örneğin, eğer `metalness` `1.0` ve `roughness` `0.0` sonra bir yüzey gerçek dünya aynası gibi görünüyor.

  Hem metallik değeri hem de metallik haritası sağlanırsa, nihai değer bu ikisinin ürünü olacaktır.

  ![metallik ve pürüzlülük](./media/metalness-roughness.png)

  Yukarıdaki resimde, sağ alt köşedeki küre gerçek bir metal malzemeye, sol alttaki ise seramik veya plastik gibi görünüyor. Albedo rengi de fiziksel özelliklerine göre değişiyor. Artan pürüzlülükle, malzeme yansıma keskinliğini kaybeder.

* **normalHarita:** İnce taneli ayrıntıları simüle etmek için normal bir [harita](https://en.wikipedia.org/wiki/Normal_mapping) sağlanabilir.

* **oklüzyonMap** ve **aoScale:** [Ortam tıkanıklığı](https://en.wikipedia.org/wiki/Ambient_occlusion) yarıklı nesneleri tıkanmış alanlara gölgeler ekleyerek daha gerçekçi görünmesini sağlar. Oklüzyon değeri, `1.0`karanlık `0.0` (tıkanmış) anlamına `1.0` gelir ve hiçbir tıkanıklık anlamına gelir , arasında değişir. `0.0` 2B doku oklüzyon eşlemi olarak sağlanırsa, efekt etkin dir ve *aoScale* çarpan olarak hareket eder.

  ![Oklüzyon Haritası](./media/boom-box-ao2.gif)

* **saydam:** PBR malzemeleri için yalnızca bir saydamlık ayarı vardır: etkin olup olmadığı. Opaklık albedo renginin alfa kanalı ile tanımlanır. Etkinleştirildiğinde, yarı saydam yüzeyler çizmek için daha karmaşık bir işleme ardışık hattı çağrılır. Azure Uzaktan İşleme, gerçek [sipariş bağımsız saydamlığı](https://en.wikipedia.org/wiki/Order-independent_transparency) (OIT) uygular.

  Saydam geometriişlemek pahalıdır. Yalnızca bir yüzeyde, örneğin bir ağacın yaprakları için deliklere ihtiyacınız varsa, bunun yerine alfa kırpma kullanmak daha iyidir.

  ![Yukarıdaki](./media/transparency.png) resimde Şeffaflık Bildirimi, nasıl sağ-en küre tamamen saydam, ama yansıma hala görülebilir.

  > [!IMPORTANT]
  > Herhangi bir malzemenin çalışma zamanında opaktan saydama geçmesi gerekiyorsa, işleyici *TileBasedComposition* [oluşturma modunu](../../concepts/rendering-modes.md)kullanmalıdır. Bu sınırlama, başlangıçta saydam malzeme olarak dönüştürülen malzemeler için geçerli değildir.

## <a name="technical-details"></a>Teknik ayrıntılar

Azure Uzaktan İşleme, GGX NDF, Schlick Fresnel ve GGX Smith ile bağlantılı görünürlük terimini Lambert yaygın terimiyle birlikte Cook-Torrance mikro-yönlü BRDF kullanır. Bu model şu anda fiili endüstri standardıdır. Daha ayrıntılı ayrıntılar için, bu makaleye bakın: [Fiziksel tabanlı Rendering - Cook Torrance](http://www.codinglabs.net/article_physically_based_rendering_cook_torrance.aspx)

 Azure Uzaktan İşleme'de kullanılan *Metalness-Pürüzlülük* PBR modeline alternatif olarak *Specular-Glossiness* PBR modeli dir. Bu model daha geniş bir malzeme yelpazesini temsil edebilir. Ancak, daha pahalı ve genellikle gerçek zamanlı durumlarda iyi çalışmıyor.
*(BaseColor, Metalness)* dönüştürülemeyen *(Diffuse, Specular)* değer çiftleri olduğu gibi *Specular-Glossiness'ten* *Metalness-Pürüzlülüğe* dönüştürmek her zaman mümkün değildir. Tüm *(BaseColor, Metalness)* çiftleri iyi tanımlanmış *(Diffuse, Specular)* çiftleri karşılık beri diğer yönde dönüşüm, daha basit ve daha kesindir.

## <a name="next-steps"></a>Sonraki adımlar

* [Renkli malzemeler](color-materials.md)
* [Doku](../../concepts/textures.md)
* [Kafes](../../concepts/meshes.md)
