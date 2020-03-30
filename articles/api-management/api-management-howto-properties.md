---
title: Azure API Yönetimi ilkelerinde adlandırılmış değerler nasıl kullanılır?
description: Azure API Yönetimi ilkelerinde adlandırılmış değerleri nasıl kullanacağınızı öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/08/2020
ms.author: apimpm
ms.openlocfilehash: 7c25455e28e57ff40664a69718a2e406b52b7632
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75834307"
---
# <a name="how-to-use-named-values-in-azure-api-management-policies"></a>Azure API Yönetimi ilkelerinde adlandırılmış değerler nasıl kullanılır?

API Yönetimi ilkeleri, sistemin Azure portalının yapılandırma yoluyla API'nin davranışını değiştirmesine olanak tanıyan güçlü bir yeteneğidir. İlkeler, bir API isteği veya yanıtı üzerinde sırayla yürütülen deyimlerin bir koleksiyonudur. İlke deyimleri, gerçek metin değerleri, ilke ifadeleri ve adlandırılmış değerler kullanılarak oluşturulabilir.

Her API Yönetimi hizmeti örneğinde, hizmet örneğine genel olan adlandırılmış değerler adı verilen anahtar/değer çiftleri koleksiyonu vardır. Koleksiyondaki madde sayısında sınır uygulanmaz. Adlandırılmış değerler, tüm API yapılandırması ve ilkeleri arasında sabit dize değerlerini yönetmek için kullanılabilir. Adlandırılmış her değer aşağıdaki özniteliklere sahip olabilir:

| Öznitelik      | Tür            | Açıklama                                                                                                                            |
| -------------- | --------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `Display name` | string          | İlkelerde adlandırılmış değere başvurmak için kullanılır. 1 ila 256 karakterden oluşan bir dize. Yalnızca harflere, sayılara, noktave tireye izin verilir. |
| `Value`        | string          | Gerçek değer. Boş olmamalı veya sadece boşluktan oluşmamalıdır. En fazla 4096 karakter uzunluğunda.                                        |
| `Secret`       | boole         | Değerin gizli olup olmadığını ve şifrelenip şifrelenmemesi gerektiğini belirler.                                                               |
| `Tags`         | dize dizisi | Adlandırılmış değer listesini filtrelemek için kullanılır. En fazla 32 etiket.                                                                                    |

![Adlandırılmış değerler](./media/api-management-howto-properties/named-values.png)

Adlandırılmış değerler, edebi dizeleri ve [ilke ifadeleri](/azure/api-management/api-management-policy-expressions)içerebilir. Örneğin, değeri geçerli `Expression` tarih ve saati içeren bir dize döndüren bir ilke ifadesidir. Adlandırılmış `Credential` değer gizli olarak işaretlenir, bu nedenle değeri varsayılan olarak görüntülenmez.

| Adı       | Değer                      | Gizli dizi | Etiketler          |
| ---------- | -------------------------- | ------ | ------------- |
| Değer      | 42                         | False  | hayati sayılar |
| Kimlik Bilgisi | ••••••••••••••••••••••     | True   | güvenlik      |
| İfadeler | @(DateTime.Now.ToString()) | False  |               |

> [!NOTE]
> API Yönetimi hizmetinde depolanan adlandırılmış değerler yerine, bu [örnekte](https://github.com/Azure/api-management-policy-snippets/blob/master/examples/Look%20up%20Key%20Vault%20secret%20using%20Managed%20Service%20Identity.policy.xml)gösterildiği gibi [Azure Anahtar Kasası](https://azure.microsoft.com/services/key-vault/) hizmetinde depolanan değerleri kullanabilirsiniz.

## <a name="to-add-and-edit-a-named-value"></a>Adlandırılmış bir değer eklemek ve onu disetmek için

![Adlandırılmış bir değer ekleme](./media/api-management-howto-properties/add-property.png)

1. **API YÖNETİMİ** bölümünden **API’ler** öğesini seçin.
2. **Adlandırılmış değerleri**seçin.
3. **+Ekle**tuşuna basın.

    Ad ve Değer gerekli değerlerdir. Değer bir sırsa, _Bu gizli bir_ onay kutusu olduğunu kontrol edin. Adlandırılmış değerlerinizi düzenlemeye yardımcı olmak için bir veya daha fazla isteğe bağlı etiket girin ve Kaydet'i tıklatın.

4. **Oluştur'u**tıklatın.

Adlandırılmış değer oluşturulduktan sonra, üzerine tıklayarak onu düzenleme yapabilirsiniz. Adlandırılmış değer adını değiştirirseniz, adı geçen değere başvuran tüm ilkeler yeni adı kullanmak üzere otomatik olarak güncelleştirilir.

REST API'sini kullanarak adlandırılmış bir değeri düzenleme hakkında bilgi için, [REST API'sini kullanarak adlandırılmış bir değer düzenle'ye](/rest/api/apimanagement/2019-01-01/property?patch)bakın.

## <a name="to-delete-a-named-value"></a>Adlandırılmış bir değeri silmek için

Adlandırılmış bir değeri silmek için, silmek için adlandırılmış değerin yanında **Sil'i** tıklatın.

> [!IMPORTANT]
> Adlandırılmış değer herhangi bir ilke tarafından başvurulursa, adı geçen değeri kullanan tüm ilkelerden kaldırana kadar başarıyla silemezsiniz.

REST API'yi kullanarak adlandırılmış bir değeri silme hakkında bilgi için [bkz.](/rest/api/apimanagement/2019-01-01/property/delete)

## <a name="to-search-and-filter-named-values"></a>Adlandırılmış değerleri aramak ve filtrelemek için

**Adlandırılmış değerler** sekmesi, adlandırılmış değerlerinizi yönetmenize yardımcı olacak arama ve filtreleme özelliklerini içerir. Adlandırılmış değerler listesini ada göre filtrelemek için **Arama özelliği** metin kutusuna bir arama terimi girin. Adlandırılmış tüm değerleri görüntülemek için **Arama özelliği** textbox'ı temizleyin ve enter tuşuna basın.

Listeye etikete göre filtrelemek **için,** etiketlertextbox'a filtre ye bir veya daha fazla etiket girin. Adlandırılmış tüm değerleri görüntülemek için Filtreyi **etiketleri** textbox'a göre temizleyin ve enter tuşuna basın.

## <a name="to-use-a-named-value"></a>Adlandırılmış bir değer kullanmak için

Bir ilkede adlandırılmış bir değer kullanmak için, adını `{{ContosoHeader}}`aşağıdaki örnekte gösterildiği gibi çift parantez içine yerleştirin:

```xml
<set-header name="{{ContosoHeader}}" exists-action="override">
  <value>{{ContosoHeaderValue}}</value>
</set-header>
```

Bu örnekte, `ContosoHeader` bir `set-header` üstbilgi nin adı olarak bir `ContosoHeaderValue` üstbilgi kullanılır ve bu üstbilginin değeri olarak kullanılır. Bu ilke, BIR istek veya API Yönetimi ağ `{{ContosoHeader}}` `{{ContosoHeaderValue}}` geçidine yanıt sırasında değerlendirildiğinde ve kendi değerleriyle değiştirildiğinde.

Adlandırılmış değerler önceki örnekte gösterildiği gibi tam öznitelik veya öğe değerleri olarak kullanılabilir, ancak aşağıdaki örnekte gösterildiği gibi bir edebi metin ifadesinin bir bölümüne eklenebilir veya bunlarla birleştirilebilir:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Adlandırılmış değerler ilke ifadeleri de içerebilir. Aşağıdaki örnekte kullanılır. `ExpressionProperty`

```xml
<set-header name="CustomHeader" exists-action="override">
    <value>{{ExpressionProperty}}</value>
</set-header>
```

Bu ilke değerlendirildiğinde, `{{ExpressionProperty}}` değeri ile değiştirilir: `@(DateTime.Now.ToString())`. Değer bir ilke ifadesi olduğundan, ifade değerlendirilir ve ilke yürütmeile devam eder.

Bunu geliştirici portalında, kapsamı içinde adlandırılmış değerlere sahip bir ilke olan bir işlemi çağırarak sınayabilirsiniz. Aşağıdaki örnekte, adlandırılmış değerlere sahip `set-header` önceki iki örnek ilkeyle bir işlem çağrılır. Yanıtın, adlandırılmış değerlerle ilkeler kullanılarak yapılandırılan iki özel üstbilgi içerdiğini unutmayın.

![Geliştirici portalı][api-management-send-results]

ADlandırılmış değerlere sahip önceki iki örnek ilkeyi içeren bir çağrı için [API Denetçisi izleme izine](api-management-howto-api-inspector.md) bakarsanız, adlandırılmış değerlerek eklenen iki `set-header` ilkenin yanı sıra ilke ifadesini içeren adlandırılmış değer için ilke ifadesi değerlendirmesini görebilirsiniz.

![API Denetçisi izleme][api-management-api-inspector-trace]

Adlandırılmış değerler ilke ifadeleri içerebilir, ancak diğer adlandırılmış değerleri içeremez. Adlandırılmış bir değer başvurusu içeren metin, `Text: {{MyProperty}}`bu başvuru çözülmez ve değiştirilmez gibi bir değer için kullanılırsa.

## <a name="next-steps"></a>Sonraki adımlar

-   İlkelerle çalışma hakkında daha fazla bilgi edinin
    -   [API Yönetiminde İlkeler](api-management-howto-policies.md)
    -   [İlke başvurusu](/azure/api-management/api-management-policies)
    -   [İlke ifadeleri](/azure/api-management/api-management-policy-expressions)

[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png
