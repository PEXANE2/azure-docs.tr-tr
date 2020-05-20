---
title: V3 makine öğrenimi varlığına geçiş
description: V3 yazma, makine öğrenme varlığına ve uygulamanın diğer varlıklarına ya da özelliklerine ilişki ekleyebilme olanağı sunan yeni bir varlık türü olan makine öğrenimi varlığı sağlar.
ms.topic: how-to
ms.date: 05/08/2020
ms.openlocfilehash: aaa5472f25a5eca5ceadf979c57a83874ce4cb6e
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684600"
---
# <a name="migrate-to-v3-authoring-entity"></a>V3 yazma varlığına geçiş

V3 yazma, makine öğrenme varlığına ve uygulamanın diğer varlıklarına ya da özelliklerine ilişki ekleyebilme olanağı sunan yeni bir varlık türü olan makine öğrenimi varlığı sağlar.

## <a name="entities-are-decomposable-in-v3"></a>Varlıklar v3 'de parçalançıkarıldı

[API 'leri](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview) ya da Portal Ile oluşturulan v3 yazma API 'leri ile oluşturulan varlıklar, bir üst ve alt öğe ile katmanlı bir varlık modeli oluşturmanıza olanak sağlar. Üst öğe makine öğrenimi varlığı olarak bilinir ve alt öğeler makine **tarafından** öğrenilen varlığın **alt varlıkları** olarak bilinir.

Her alt varlık aynı zamanda bir makine öğrenimi varlığı ve özelliklerin yapılandırma seçenekleri eklenmiştir.

* **Gerekli özellikler** , bir varlığın bir özellikle eşleştiğinde ayıklandığını garanti eden kurallardır. Kural, modele gereken özellik tarafından tanımlanır:
    * [Önceden oluşturulmuş varlık](luis-reference-prebuilt-entities.md)
    * [Normal ifade varlığı](reference-entity-regular-expression.md)
    * [Varlık listeleyin](reference-entity-list.md).

## <a name="how-do-these-new-relationships-compare-to-v2-authoring"></a>Bu yeni ilişkiler v2 yazma ile nasıl karşılaştırılır

V2 yazma, hiyerarşik ve bileşik varlıkların yanı sıra bu aynı görevi yerine getirmek için roller ve özelliklerle birlikte verilmiştir. Varlıklar, Özellikler ve roller birbirleriyle açık bir şekilde ilişkili olmadığından, YAĞMANıN tahmin sırasında ilişkilerin nasıl ima olduğunu anlamak zordur.

V3 ile, ilişki uygulama yazarları tarafından açık ve tasarlanmıştır. Bu, uygulama yazarı olarak şunları yapmanıza olanak sağlar:

* Bu ilişkilerin bu ilişkileri nasıl tahmin ediyor olduğunu görsel olarak görme
* [Etkileşimli test bölmesi](luis-interactive-test.md) veya uç noktada bu ilişkiler için test yapın
* İstemci uygulamasında, iyi yapılandırılmış, adlandırılmış, iç içe geçmiş bir [. JSON nesnesi](reference-entity-machine-learned-entity.md) aracılığıyla bu ilişkileri kullanın

## <a name="planning"></a>Planlama

' Yi geçirdiğinizde, geçiş planınızda aşağıdakileri göz önünde bulundurun:

* LUSıS uygulamanızı yedekleyin ve geçişi ayrı bir uygulamada gerçekleştirin. Bir v2 ve v3 uygulamasının aynı anda kullanılabilir olması, gerekli değişiklikleri ve tahmin sonuçları üzerindeki etkiyi doğrulamanızı sağlar.
* Geçerli tahmini başarı ölçümlerini yakala
* Geçerli Pano bilgilerini uygulama durumunun bir anlık görüntüsü olarak yakala
* Mevcut hedefleri, varlıkları, tümcecik listelerini, desenleri ve toplu testleri gözden geçirin
* Aşağıdaki öğeler **değişiklik yapılmadan**geçirilebilir:
    * Hedefler
    * Varlıklar
        * Normal ifade varlığı
        * Liste varlığı
    * Özellikler
        * Tümcecik listesi
* Aşağıdaki öğelerin **değişikliklerle**geçirilmesi gerekir:
    * Varlıklar
        * Hiyerarşik varlık
        * Bileşik varlık
    * Roller-roller yalnızca bir makine öğrenimi (üst) varlığına uygulanabilir. Roller alt varlıklara uygulanamıyor
    * Hiyerarşik ve bileşik varlıkları kullanan toplu testler ve desenler

Geçiş planınızı tasarlarken, tüm hiyerarşik ve bileşik varlıklar geçirildikten sonra son makine öğrenimi varlıklarını gözden geçirmek için zaman bırakın. Düz bir geçiş çalışmaya başladıktan sonra, değişikliği yaptıktan ve Batch test sonuçlarınızı ve tahmin JSON 'larınızı gözden geçirdikten sonra, daha fazla birleştirilmiş JSON, istemci tarafı uygulamasına teslim edilen son bilgilerin farklı şekilde organize edilmesi için sizi değişiklik yapmaya yol açabilir. Bu kod yeniden düzenlemesi ile benzerdir ve kuruluşunuzun yerinde olduğu gözden geçirme işlemiyle birlikte değerlendirilmelidir.

V2 modelinize yönelik Batch testleriniz yoksa ve geçişin bir parçası olarak Batch testlerini v3 modeline geçirirseniz, geçişin Endpoint tahmin sonuçlarını nasıl etkileyeceğini doğrulayamazsınız.

## <a name="migrating-from-v2-entities"></a>V2 varlıklarından geçiş

V3 yazma modeline taşımaya başladığınızda, makine öğrenimi varlığına ve bunların alt varlıklarını ve özelliklerine nasıl geçdiklerinizi göz önünde bulundurmanız gerekir.

Aşağıdaki tabloda, hangi varlıkların bir v2 'den v3 varlık tasarımına geçirilmesi gerektiğini not edin.

|V2 yazma varlık türü|V3 yazarlık varlık türü|Örnek|
|--|--|--|
|Bileşik varlık|Makine tarafından öğrenilen varlık|[Daha fazla bilgi edinin](#migrate-v2-composite-entity)|
|Hiyerarşik varlık|makine öğrenimi varlığının rolü|[Daha fazla bilgi edinin](#migrate-v2-hierarchical-entity)|

## <a name="migrate-v2-composite-entity"></a>V2 bileşik varlığını geçirme

V2 Composite 'un her bir alt öğesi v3 makine öğrenimi varlığının bir alt varlığıyla temsil edilmelidir. Bileşik alt öğe önceden oluşturulmuş bir, normal ifade veya bir liste varlığı ise, bu, alt varlığa gerekli bir özellik olarak uygulanmalıdır.

Bileşik bir varlığı makine öğrenimi varlığına geçirmeyi planlarken dikkat edilecek noktalar:
* Alt varlıklar desenlerinde kullanılamaz
* Alt varlıklar artık paylaşılmıyor
* Makine öğrenildikleri takdirde alt varlıkların etiketlenmesi gerekir

### <a name="existing-features"></a>Mevcut özellikler

Bileşik varlıktaki sözcükleri artırmak için kullanılan tümcecik listesi, makine öğrenimi (üst) varlığına, alt varlık (alt varlık) varlığına veya amaca (tümcecik listesi yalnızca bir amaç için geçerliyse) bir özellik olarak uygulanmalıdır. Özelliği en önemli düzeyde iyileştirmeli varlığa eklemeyi planlayın. Özelliği, bir alt varlığın (alt) tahminini önemli ölçüde iyileştirildiğinden, özelliği makine öğrenimi (üst) varlığına eklemeyin.

### <a name="new-features"></a>Yeni özellikler

V3 yazma bölümünde varlıkları tüm varlıklar ve amaçlar için olası özellikler olarak değerlendirmek üzere bir planlama adımı ekleyin.

### <a name="example-entity"></a>Örnek varlık

Bu varlık yalnızca bir örnektir. Kendi varlık geçişiniz, başka hususlar gerektirebilir.

Şunu kullanan bir pizza 'yi değiştirmek için bir v2 bileşimini düşünün `order` :
* teslim süresi için önceden oluşturulmuş datetimeV2
* Pizza, pasta, Crust ve tografik gibi belirli kelimeleri artırmak için tümcecik listesi
* mushodalar, zeytin, pepperoni gibi toppings algılamak için varlık listeleyin.

Bu varlık için bir örnek söylenişi şunlardır:

`Change the toppings on my pie to mushrooms and delivery it 30 minutes later`

Aşağıdaki tabloda geçiş gösterilmektedir:

|V2 modelleri|V3 modelleri|
|--|--|
|Adlı üst bileşen varlık`Order`|Adlı üst makine öğrenimi varlığı`Order`|
|Alt-önceden oluşturulmuş datetimeV2|* Önceden oluşturulmuş varlığı yeni uygulamaya geçirin.<br>* Önceden oluşturulmuş datetimeV2 için üst öğeye gerekli özelliği ekleyin.|
|Toppings için alt liste varlığı|* Liste varlığını yeni uygulamaya geçirin.<br>* Ardından liste varlığı için üst öğeye gerekli bir özellik ekleyin.|


## <a name="migrate-v2-hierarchical-entity"></a>V2 hiyerarşik varlığını geçirme

V2 yazma sürümünde, LUSıS 'de mevcut rollerden önce hiyerarşik bir varlık sağlandı. Her ikisi de bağlam kullanımına göre varlıkların ayıklanması için de aynı amaca hizmet. Hiyerarşik varlıklarınız varsa, bunları rollerle basit varlıklar olarak düşünebilirsiniz.

V3 yazma:
* Makine öğrenimi (üst) varlığına bir rol uygulanabilir.
* Herhangi bir alt varlığa bir rol uygulanamaz.

Bu varlık yalnızca bir örnektir. Kendi varlık geçişiniz, başka hususlar gerektirebilir.

Bir pizza değiştirmek için bir v2 hiyerarşik varlığı düşünün `order` :
* Her çocuğun orijinal veya son oluşturma

Bu varlık için bir örnek söylenişi şunlardır:

`Change the topping from mushrooms to olives`

Aşağıdaki tabloda geçiş gösterilmektedir:

|V2 modelleri|V3 modelleri|
|--|--|
|Adlı üst bileşen varlık`Order`|Adlı üst makine öğrenimi varlığı`Order`|
|Orijinal ve son pizza ile alt hiyerarşik varlık|* `Order` Her bir for için rol ekleyin.|

## <a name="api-change-constraint-replaced-with-required-feature"></a>API değişiklik kısıtlaması, gerekli özellik ile değiştirilmiş

Bu değişiklik, nbuild konferansında 2020 Mayıs 'da yapılmıştır ve yalnızca uygulamanın kısıtlanmış bir özelliği kullandığı v3 yazma API 'Leri için geçerlidir. V2 Authoring öğesinden v3 Authoring 'e geçiş yapıyorsanız veya v3 kısıtlı özellikleri kullanmadıysanız, bu bölümü atlayın.

**İşlevsellik** -mevcut bir varlığı başka bir modele özellik olarak gerektirme ve bu modeli yalnızca varlık algılanırsa ayıkla. İşlev değiştirilmedi, ancak API ve terminoloji değişmiş.

|Önceki terminoloji|Yeni terminoloji|
|--|--|
|`constrained feature`<br>`constraint`<br>`instanceOf`|`required feature`<br>`isRequired`|

#### <a name="automatic-migration"></a>Otomatik geçiş

**Haziran 19 2020**' den itibaren, bu işlevselliği kullanıma sunan ÖNCEKI yazma API 'sini kullanarak program aracılığıyla kısıtlama oluşturmanıza izin verilmez.

Tüm mevcut kısıtlama özellikleri otomatik olarak gerekli özellik bayrağına geçirilir. Tahmin API 'niz için programlı bir değişiklik yapılması ve tahmin doğruluk kalitesi üzerinde sonuç değişikliği olmaması gerekmez.

#### <a name="luis-portal-changes"></a>LUSıS portalı değişiklikleri

LUSıS önizleme portalı bu işlevselliğe bir **kısıtlama**olarak başvurdu. Geçerli Lua portalı, bu işlevselliği **gerekli bir özellik**olarak belirler.

#### <a name="previous-authoring-api"></a>Önceki yazma API 'SI

Bu işlev, bir varlığın alt öğesinin özelliğini kullanarak bir varlığın tanımının parçası olarak bir varlık tanımının **[varlık oluşturma alt API 'si](https://westus.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5d86cf3c6a25a45529767d77)** için Önizleme yazma ' ya uygulandı `instanceOf` :

```json
{
    "name" : "dayOfWeek",
    "instanceOf": "datetimeV2",
    "children": [
        {
           "name": "dayNumber",
           "instanceOf": "number",
           "children": []
        }
    ]
}
```

#### <a name="new-authoring-api"></a>Yeni yazma API 'SI

Bu işlev, ve özellikleri kullanılarak **[varlık özelliği Ilişkisi Ekle API 'si](https://westus.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5d9dc1781e38aaec1c375f26)** ile birlikte `featureName` uygulanır `isRequired` . Özelliğin değeri, `featureName` modelin adıdır.

```json
{
    "featureName": "YOUR-MODEL-NAME-HERE",
    "isRequired" : true
}
```


## <a name="next-steps"></a>Sonraki adımlar

* [Geliştirici kaynakları](developer-reference-resource.md)
