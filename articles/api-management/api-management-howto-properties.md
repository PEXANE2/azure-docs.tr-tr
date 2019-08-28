---
title: Azure API Management ilkelerinde adlandırılmış değerleri kullanma
description: Azure API Management ilkelerinde adlandırılmış değerleri nasıl kullanacağınızı öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/22/2019
ms.author: apimpm
ms.openlocfilehash: d71d71c4d289235e5b67a5201c1f7417274b8fca
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70072321"
---
# <a name="how-to-use-named-values-in-azure-api-management-policies"></a>Azure API Management ilkelerinde adlandırılmış değerleri kullanma

API Management ilkeleri, sistemin yapılandırma aracılığıyla API 'nin davranışını değiştirmesine Azure portal olanak tanıyan güçlü bir yetenektir. İlkeler, bir API isteği veya yanıtı üzerinde sırayla yürütülen deyimlerin bir koleksiyonudur. İlke deyimleri, sabit metin değerleri, ilke ifadeleri ve adlandırılmış değerler kullanılarak oluşturulabilir.

Her bir API Management hizmet örneği, hizmet örneği için genel olan adlandırılmış değerler olarak adlandırılan anahtar/değer çiftlerinin bir özellikler koleksiyonuna sahiptir. Koleksiyondaki öğe sayısı üzerinde hiçbir uygulanan sınır yoktur. Adlandırılmış değerler, tüm API yapılandırması ve ilkeleri genelinde sabit dize değerlerini yönetmek için kullanılabilir. Her bir adlandırılmış değer aşağıdaki özniteliklere sahip olabilir:

| Öznitelik      | Type            | Açıklama                                                                                                                         |
| -------------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `Display name` | dize          | İlkelerde özelliğe başvurmak için kullanılır. Bir ile 256 karakter arasında bir dize. Yalnızca harfler, rakamlar, nokta ve tireye izin verilir. |
| `Value`        | dize          | Gerçek değer. Boş olmamalı veya yalnızca boşluklardan oluşmalıdır. En fazla 4096 karakter uzunluğunda.                                     |
| `Secret`       | boolean         | Değerin gizli olup olmadığını ve şifrelenmesinin gerekip gerekmediğini belirler.                                                            |
| `Tags`         | dize dizisi | Özellik listesini filtrelemek için kullanılır. En fazla 32 etiket.                                                                                    |

![Adlandırılmış değerler](./media/api-management-howto-properties/named-values.png)

Adlandırılmış değerler, sabit dizeler ve [ilke ifadeleri](/azure/api-management/api-management-policy-expressions)içerebilir. Örneğin, değeri `Expression` geçerli tarih ve saati içeren bir dize döndüren bir ilke deyimidir. Adlandırılmış değer `Credential` gizli olarak işaretlenir, bu nedenle değeri varsayılan olarak görüntülenmez.

| Name       | Value                      | Secret | Tags          |
| ---------- | -------------------------- | ------ | ------------- |
| Value      | 42                         | False  | önemli sayılar |
| Kimlik bilgisi | ••••••••••••••••••••••     | Doğru   | güvenlik      |
| İfade | @ (DateTime. Now. ToString ()) | False  |               |

## <a name="to-add-and-edit-a-property"></a>Bir özellik eklemek ve düzenlemek için

![Özellik Ekle](./media/api-management-howto-properties/add-property.png)

1. **API YÖNETİMİ** bölümünden **API’ler** öğesini seçin.
2. **Adlandırılmış değerleri**seçin.
3. **+ Ekle**tuşuna basın.

    Ad ve değer gerekli değerlerdir. Bu özellik değeri bir gizli dizi ise, bu gizli bir onay kutusu olup olmadığına bakın. Adlandırılmış değerlerinizi organize etmenize yardımcı olmak için bir veya daha fazla isteğe bağlı etiket girin ve Kaydet ' e tıklayın.

4. **Oluştur**'a tıklayın.

Özellik oluşturulduktan sonra, özelliği üzerine tıklayarak düzenleyebilirsiniz. Özellik adını değiştirirseniz, bu özelliğe başvuruda bulunan tüm ilkeler, yeni adı kullanacak şekilde otomatik olarak güncelleştirilir.

REST API kullanarak bir özelliği düzenleme hakkında daha fazla bilgi için, bkz. [bir özelliği REST API kullanarak düzenleme](/rest/api/apimanagement/2019-01-01/property?patch).

## <a name="to-delete-a-property"></a>Bir özelliği silmek için

Bir özelliği silmek için, Silinecek özelliğin yanındaki **Sil** ' e tıklayın.

> [!IMPORTANT]
> Özelliğe herhangi bir ilke tarafından başvuruluyorsa, özelliği onu kullanan tüm ilkelerden kaldırana kadar başarılı bir şekilde silmeniz mümkün olmayacaktır.

REST API kullanarak bir özelliği silme hakkında daha fazla bilgi için, bkz. [REST API kullanarak bir özelliği silme](/rest/api/apimanagement/2019-01-01/property/delete).

## <a name="to-search-and-filter-named-values"></a>Adlandırılmış değerleri aramak ve filtrelemek için

**Adlandırılmış değerler** sekmesi, adlandırılmış değerlerinizi yönetmenize yardımcı olmak için arama ve filtreleme özelliklerini içerir. Özellik listesini özellik adına göre filtrelemek için **arama özelliği** metin kutusuna bir arama terimi girin. Tüm adlandırılmış değerleri göstermek için **arama özelliği** metin kutusunu temizleyin ve ENTER tuşuna basın.

Özellik listesini etiket değerlerine göre filtrelemek için **etiketlere göre filtrele** metin kutusuna bir veya daha fazla etiket girin. Tüm adlandırılmış değerleri göstermek için **etiketlere göre filtrele** metin kutusunu temizleyin ve ENTER tuşuna basın.

## <a name="to-use-a-property"></a>Bir özelliği kullanmak için

Bir ilkede bir özelliği kullanmak için, aşağıdaki örnekte gösterildiği gibi, özellik adını çift küme ayraçları `{{ContosoHeader}}`içine yerleştirin:

```xml
<set-header name="{{ContosoHeader}}" exists-action="override">
  <value>{{ContosoHeaderValue}}</value>
</set-header>
```

Bu örnekte, `ContosoHeader` bir `set-header` ilkedeki üst bilgi adı olarak kullanılır ve `ContosoHeaderValue` bu üstbilginin değeri olarak kullanılır. Bu ilke bir istek veya API Management ağ geçidine `{{ContosoHeader}}` yanıt sırasında değerlendirildiğinde ve `{{ContosoHeaderValue}}` ilgili özellik değerleriyle değiştirilmiştir.

Adlandırılmış değerler, önceki örnekte gösterildiği gibi, tüm öznitelik veya öğe değerleri olarak kullanılabilir, ancak aşağıdaki örnekte gösterildiği gibi, değişmez değer metin ifadesinin bir bölümüne eklenebilir veya birleştirilebilir:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Adlandırılmış değerler, ilke ifadeleri de içerebilir. Aşağıdaki örnekte `ExpressionProperty` , kullanılır.

```xml
<set-header name="CustomHeader" exists-action="override">
    <value>{{ExpressionProperty}}</value>
</set-header>
```

Bu ilke değerlendirildiğinde, `{{ExpressionProperty}}` değeri ile değiştirilmiştir:. `@(DateTime.Now.ToString())` Değer bir ilke ifadesi olduğundan, ifade değerlendirilir ve ilke yürütmeye devam eder.

Kapsam içinde adlandırılmış değerlere sahip bir ilkeye sahip bir işlem çağırarak Geliştirici Portalında bunu test edebilirsiniz. Aşağıdaki örnekte, bir işlem, adlandırılmış değerlere sahip olan iki önceki örnek `set-header` ilkesiyle birlikte çağırılır. Yanıtın, adlandırılmış değerleri olan ilkeler kullanılarak yapılandırılmış iki özel üst bilgi içerdiğini unutmayın.

![Geliştirici portalı][api-management-send-results]

Adlandırılmış değerlere sahip iki önceki örnek ilkeyi içeren bir çağrı için [API denetçisi izlemeye](api-management-howto-api-inspector.md) bakarsanız, özellik değerleri eklenen iki `set-header` ilkeyi ve özelliği için ilke ifadesi değerlendirmesini görebilirsiniz ilke ifadesini içeriyordu.

![API denetçisi izleme][api-management-api-inspector-trace]

Özellik değerleri ilke ifadeleri içerebildiği sürece, özellik değerleri diğer adlandırılmış değerleri içeremez. Özellik başvurusu içeren metin, gibi bir özellik değeri `Property value text {{MyProperty}}`için kullanılırsa, bu özellik başvurusu değiştirilmez ve özellik değerinin bir parçası olarak dahil edilir.

## <a name="next-steps"></a>Sonraki adımlar

-   İlkelerle çalışma hakkında daha fazla bilgi edinin
    -   [API Management ilkeler](api-management-howto-policies.md)
    -   [İlke başvurusu](/azure/api-management/api-management-policies)
    -   [İlke ifadeleri](/azure/api-management/api-management-policy-expressions)

[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png
