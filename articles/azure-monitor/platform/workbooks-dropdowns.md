---
title: Azure Monitor Çalışma Kitabı açılır parametreleri
description: Açılır parametreiçeren önceden oluşturulmuş ve özel parametreli çalışma kitaplarıyla karmaşık raporlamayı basitleştirin
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: f3220a363025d80fd7636dbfc3af3d2d9d7bc040
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658294"
---
# <a name="workbook-drop-down-parameters"></a>Çalışma kitabı açılır parametreleri

Açılan düşüşler, kullanıcının bilinen bir kümeden bir veya daha fazla giriş değeri toplamasına olanak sağlar (örneğin, uygulamanızın isteklerinden birini seçin). Açılan çıkışlar, kullanıcılardan rasgele girdiler toplamak için kullanıcı dostu bir yol sağlar. Açılan düşüşler, özellikle etkileşimli raporlarınızda filtreleme yi etkinleştirmede yararlıdır. 

Açılır bırakma yı belirtmenin en kolay yolu, parametre ayarında statik bir liste sağlamaktır. Daha ilginç bir yolu bir KQL sorgusu ile dinamik olarak liste almaktır. Parametre ayarları, tek veya çok seçimli olup olmadığını ve çok seçmeli olup olmadığını, sonuç kümesinin nasıl biçimlendirilmesi gerektiğini (delimiter, tırnak, vb.) belirtmenize de olanak tanır.

## <a name="creating-a-static-drop-down-parameter"></a>Statik açılır parametre oluşturma

1. Edit modunda boş bir çalışma kitabıyla başlayın.
2. Çalışma kitabındaki bağlantılardan _parametre ekle'yi_ seçin.
3. Mavi _Parametre Ekle_ düğmesine tıklayın.
4. Açılan yeni parametre bölmesine girin:
    1. Parametre adı:`Environment`
    2. Parametre türü:`Drop down`
    3. Gerekli:`checked`
    4. İzin `multiple selection`ver:`unchecked`
    5. Veri alın:`JSON`
5. JSON Giriş metin bloğuna bu json parçacığını ekleyin:
    ```json
    [
        { "value":"dev", "label":"Development" },
        { "value":"ppe", "label":"Pre-production" },
        { "value":"prod", "label":"Production", "selected":true }
    ]
    ```
6. Mavi `Update` düğmeye bas.
7. Parametreyi oluşturmak için araç çubuğundan 'Kaydet'i seçin.
8. Çevre parametresi üç değerle açılır bir parametre olacaktır.

    ![Statik boğulmanın yaratılışını gösteren görüntü](./media/workbook-dropdowns/dropdown-create.png)

## <a name="creating-a-static-dropdown-with-groups-of-items"></a>Öğe gruplarıyla statik açılır bırakma oluşturma
Sorgu sonucunuz/json'unuzun bir "grup" alanı varsa, açılır açılır değerler grupları görüntülenir. Yukarıdaki örneği izleyin, ancak bunun yerine aşağıdaki json kullanın:
```json
[
    { "value":"dev", "label":"Development", "group":"Development" },
    { "value":"dev-cloud", "label":"Development (Cloud)", "group":"Development" },
    { "value":"ppe", "label":"Pre-production", "group":"Test" },
    { "value":"ppe-test", "label":"Pre-production (Test)", "group":"Test" },
    { "value":"prod1", "label":"Prod 1", "selected":true, "group":"Production" },
    { "value":"prod2", "label":"Prod 2", "group":"Production" }
]
```
    ![Image showing an example of a grouped dropdown](./media/workbook-dropdowns/grouped-dropDown.png)


## <a name="creating-a-dynamic-drop-down-parameter"></a>Dinamik bir açılır parametre oluşturma
1. Edit modunda boş bir çalışma kitabıyla başlayın.
2. Çalışma kitabındaki bağlantılardan _parametre ekle'yi_ seçin.
3. Mavi _Parametre Ekle_ düğmesine tıklayın.
4. Açılan yeni parametre bölmesine girin:
    1. Parametre adı:`RequestName`
    2. Parametre türü:`Drop down`
    3. Gerekli:`checked`
    4. İzin `multiple selection`ver:`unchecked`
    5. Veri alın:`Query`
5. JSON Giriş metin bloğuna bu json parçacığını ekleyin:

    ```kusto
        requests
        | summarize by name
        | order by name asc
    ```
1. Mavi `Run Query` düğmeye bas.
2. Parametreyi oluşturmak için araç çubuğundan 'Kaydet'i seçin.
3. RequestName parametresi, uygulamadaki tüm isteklerin adlarını açılır olacaktır.

    ![Dinamik bir açılır bırakma oluşturmayı gösteren görüntü](./media/workbook-dropdowns/dropdown-dynamic.png)

## <a name="referencing-drop-down-parameter"></a>Açılan parametreye başvurma
### <a name="in-kql"></a>KQL içinde
1. Çalışma kitabına bir sorgu denetimi ekleyin ve bir Uygulama Öngörüleri kaynağını seçin.
2. KQL düzenleyicisinde, bu snippet girin

    ```kusto
        requests
        | where name == '{RequestName}'
        | summarize Requests = count() by bin(timestamp, 1h)

    ```
3. Bu, sorgu değerlendirme süresini şu şekilde genişletir:

    ```kusto
        requests
        | where name == 'GET Home/Index'
        | summarize Requests = count() by bin(timestamp, 1h)
    ```

4. Sonuçları görmek için sorguyu çalıştırın. İsteğe bağlı olarak, bir grafik olarak işledin.

    ![KQL'de başvurulan bir açılır düşüşü gösteren resim](./media/workbook-dropdowns/dropdown-reference.png)


## <a name="parameter-value-label-selection-and-group"></a>Parametre değeri, etiket, seçim ve grup
Yukarıdaki dinamik açılır parametrede kullanılan sorgu, açılan yolda sadakatle işlenen değerlerin listesini döndürür. Ama ya farklı bir görüntü adı veya bunlardan birinin seçilmesini istiyorsanız? Düşürme parametreleri değer, etiket, seçim ve grup sütunları aracılığıyla buna izin verir.

Aşağıdaki örnekte, ekran adları bir emoji ile stile sahip, ilki seçilen ve işlem adlarına göre gruplanmış olan Uygulama Öngörüleri bağımlılıklarının bir listesini nasıl elde edilir gösterilmektedir.

```kusto
dependencies
| summarize by operation_Name, name
| where name !contains ('.')
| order by name asc
| serialize Rank = row_number()
| project value = name, label = strcat('🌐 ', name), selected = iff(Rank == 1, true, false), group = operation_Name
```
    ![Image showing a drop-down parameter using value, label, selection and group options](./media/workbook-dropdowns/dropdown-more-options.png)


## <a name="drop-down-parameter-options"></a>Parametre seçeneklerini aşağı bırakma
| Parametre | Açıklama | Örnek |
| ------------- |:-------------|:-------------|
| `{DependencyName}` | Seçili değer | Fabrikamaccount AL |
| `{DependencyName:label}` | Seçili etiket | 🌐 GET fabrikamaccount |
| `{DependencyName:value}` | Seçili değer | Fabrikamaccount AL |

## <a name="multiple-selection"></a>Çoklu seçim
Şimdiye kadarki örnekler, parametreyi açılır yolda yalnızca bir değer seçecek şekilde açıkça ayarlar. Parametreleri de `multiple selection` destek açılır - bu `Allow multiple selection` seçeneği kontrol etmek kadar basittir etkinleştirme. 

Kullanıcı ayrıca, ve `delimiter` `quote with` ayarlar üzerinden belirlenen sonucun biçimini belirtme seçeneğine de sahiptir. Varsayılan değer, bu formdaki bir koleksiyon olarak değerleri döndürür: 'a', 'b', 'c'. Ayrıca seçim sayısını sınırlama seçeneğine de sahiptir.

Parametreye başvuran KQL'nin sonucun biçimiyle çalışmak için değişmesi gerekir. Bunu etkinleştirmek için en yaygın `in` yolu operatör aracılığıyla.

```kusto
dependencies
| where name in ({DependencyName})
| summarize Requests = count() by bin(timestamp, 1h), name
```

İşte iş yerinde çok seçimli açılır bırakma için bir örnek:

![Çok seçimli açılır parametreyi gösteren resim](./media/workbook-dropdowns/dropdown-multiselect.png)

## <a name="next-steps"></a>Sonraki adımlar

* Çalışma kitapları hakkında daha fazla bilgi [edinmeye başlayın](workbooks-visualizations.md) birçok zengin görselleştirme seçeneği.
* Çalışma kitabı kaynaklarınıza erişimi [kontrol](workbooks-access-control.md) edin ve paylaşın.
