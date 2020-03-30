---
title: Azure Monitör çalışma kitapları kaynak parametreleri
description: Önceden oluşturulmuş ve özel parametreli çalışma kitaplarıyla karmaşık raporlamayı basitleştirin
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: cc2cde7932f783f63ee2783f0589ce4f88f248a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658111"
---
# <a name="workbook-resource-parameters"></a>Çalışma kitabı kaynak parametreleri

Kaynak parametreleri çalışma kitaplarındaki kaynakların seçilebilmesine olanak sağlar. Bu, verileri almak için kapsamı ayarlamada yararlıdır. Bir örnek, kullanıcıların, grafiklerin verileri sunarken daha sonra kullanacağı VM kümesini seçmelerine izin vermektir.

Kaynak toplayıcılarının değerleri çalışma kitabı bağlamından, statik listeden veya Azure Kaynak Grafiği sorgularından gelebilir.

## <a name="creating-a-resource-parameter-workbook-resources"></a>Kaynak parametresi oluşturma (çalışma kitabı kaynakları)
1. Edit modunda boş bir çalışma kitabıyla başlayın.
2. Çalışma kitabındaki bağlantılardan _parametre ekle'yi_ seçin.
3. Mavi _Parametre Ekle_ düğmesine tıklayın.
4. Açılan yeni parametre bölmesine girin:
    1. Parametre adı:`Applications`
    2. Parametre türü:`Resource picker`
    3. Gerekli:`checked`
    4. Birden çok seçime izin ver:`checked`
5. Veri alın:`Workbook Resources`
6. Yalnızca kaynak türlerini ekleyin:`Application Insights`
7. Parametreyi oluşturmak için araç çubuğundan 'Kaydet'i seçin.

![Çalışma kitabı kaynaklarını kullanarak kaynak parametresi oluşturulmasını gösteren resim](./media/workbooks-resources/resource-create.png)

## <a name="creating-a-resource-parameter-azure-resource-graph"></a>Kaynak parametresi oluşturma (Azure Kaynak Grafiği)
1. Edit modunda boş bir çalışma kitabıyla başlayın.
2. Çalışma kitabındaki bağlantılardan _parametre ekle'yi_ seçin.
3. Mavi _Parametre Ekle_ düğmesine tıklayın.
4. Açılan yeni parametre bölmesine girin:
    1. Parametre adı:`Applications`
    2. Parametre türü:`Resource picker`
    3. Gerekli:`checked`
    4. Birden çok seçime izin ver:`checked`
5. Veri alın:`Query`
    1. Sorgu Türü:`Azure Resource Graph`
    2. Abonelik:`Use default subscriptions`
    3. Sorgu denetiminde, bu parçacık ekleyin
    ```kusto
    where type == 'microsoft.insights/components'
    | project value = id, label = name, selected = false, group = resourceGroup
    ```
7. Parametreyi oluşturmak için araç çubuğundan 'Kaydet'i seçin.

![Azure Kaynak Grafiği'ni kullanarak kaynak parametresi oluşturulmasını gösteren resim](./media/workbooks-resources/resource-query.png)

> [!NOTE]
> Azure Kaynak Grafiği henüz tüm bulutlarda kullanılamıyor. Bu yaklaşımı seçerseniz hedef bulutunuzda desteklendirildiğından emin olun.

[Azure Kaynak Grafiği belgeleri](https://docs.microsoft.com/azure/governance/resource-graph/overview)

## <a name="creating-a-resource-parameter--json-list"></a>Kaynak parametresi oluşturma (JSON listesi)
1. Edit modunda boş bir çalışma kitabıyla başlayın.
2. Çalışma kitabındaki bağlantılardan _parametre ekle'yi_ seçin.
3. Mavi _Parametre Ekle_ düğmesine tıklayın.
4. Açılan yeni parametre bölmesine girin:
    1. Parametre adı:`Applications`
    2. Parametre türü:`Resource picker`
    3. Gerekli:`checked`
    4. Birden çok seçime izin ver:`checked`
5. Veri alın:`JSON`
    1. İçerik denetiminde, bu json snippet ekleyin
    ```json
    [
        { "value":"/subscriptions/<sub-id>/resourceGroups/<resource-group>/providers/<resource-type>/acmeauthentication", "label": "acmeauthentication", "selected":true, "group":"Acme Backend" },
        { "value":"/subscriptions/<sub-id>/resourceGroups/<resource-group>/providers/<resource-type>/acmeweb", "label": "acmeweb", "selected":false, "group":"Acme Frontend" }
    ]
    ```
    2. Mavi _Güncelleme_ düğmesine basın.
6. İsteğe bağlı `Include only resource types` olarak _Uygulama Öngörüleri'ni_ ayarlayın
7. Parametreyi oluşturmak için araç çubuğundan 'Kaydet'i seçin.

## <a name="referencing-a-resource-parameter"></a>Kaynak parametreye başvurma
1. Çalışma kitabına bir sorgu denetimi ekleyin ve bir Uygulama Öngörüleri kaynağını seçin.
2. Parametreyi denetime bağlamak için _Uygulama Öngörüleri_ düşüşünü kullanın. Bunu yapmak, sorgunun kapsamını çalışma zamanında parametre tarafından döndürülen kaynaklara ayarlar.
4. KQL denetiminde, bu parçacık ekleyin
    ```kusto
    requests
    | summarize Requests = count() by appName, name
    | order by Requests desc
    ```
5. Sonuçları görmek için sorguyu çalıştırın. 

![Sorgu denetiminde başvurulan kaynak parametreyi gösteren resim](./media/workbooks-resources/resource-reference.png)

> Bu yaklaşım, kaynakları ölçümler gibi diğer denetimlere bağlamak için kullanılabilir.

## <a name="resource-parameter-options"></a>Kaynak parametre seçenekleri
| Parametre | Açıklama | Örnek |
| ------------- |:-------------|:-------------|
| `{Applications}` | Seçili kaynak kimliği | _/subscriptions/<alt id>/kaynakGruplar/<kaynak grubu>/sağlayıcıları/<kaynak türü>/acmeauthentication_ |
| `{Applications:label}` | Seçili kaynağın etiketi | `acmefrontend` |
| `{Applications:value}` | Seçili kaynağın değeri | _'/subscriptions/<alt id>/kaynakGruplar/<kaynak grubu>/sağlayıcılar/<kaynak türü>/acmeauthentication'_ |
| `{Applications:name}` | Seçili kaynağın adı | `acmefrontend` |
| `{Applications:resourceGroup}` | Seçili kaynağın kaynak grubu | `acmegroup` |
| `{Applications:resourceType}` | Seçili kaynağın türü | _microsoft.insights/components_ |
| `{Applications:subscription}` | Seçili kaynağın aboneliği |  |
| `{Applications:grid}` | Kaynak özelliklerini gösteren bir ızgara. Hata ayıklama sırasında metin bloğunda işlemek için yararlıdır  |  |

## <a name="next-steps"></a>Sonraki adımlar

* Çalışma kitapları hakkında daha fazla bilgi [edinmeye başlayın](workbooks-visualizations.md) birçok zengin görselleştirme seçeneği.
* Çalışma kitabı kaynaklarınıza erişimi [kontrol](workbooks-access-control.md) edin ve paylaşın.
