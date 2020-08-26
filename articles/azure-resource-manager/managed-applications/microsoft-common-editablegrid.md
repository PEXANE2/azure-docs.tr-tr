---
title: EditableGrid UI öğesi
description: Azure portal için Microsoft. Common. EditableGrid UI öğesini açıklar. Kullanıcıların tablolu giriş toplamasına olanak sağlar.
author: tfitzmac
ms.topic: conceptual
ms.date: 08/24/2020
ms.author: tomfitz
ms.openlocfilehash: 04f86883a75110985d1cbe050fe3fd3e0582986a
ms.sourcegitcommit: c6b9a46404120ae44c9f3468df14403bcd6686c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88893814"
---
# <a name="microsoftcommoneditablegrid-ui-element"></a>Microsoft. Common. EditableGrid UI öğesi

Tablo girişi toplamaya yönelik bir denetim. Kılavuz içindeki tüm alanlar düzenlenebilir ve satır sayısı farklılık gösterebilir.

## <a name="ui-sample"></a>UI örneği

:::image type="content" source="./media/managed-application-elements/microsoft-common-editablegrid.png" alt-text="Microsoft. Common. EditableGrid":::

## <a name="schema"></a>Şema

```json
{
  "name": "people",
  "type": "Microsoft.Common.EditableGrid",
  "ariaLabel": "Enter information per person",
  "label": "People",
  "constraints": {
    "width": "Full",
    "rows": {
      "count": {
        "min": 1,
        "max": 10
      }
    },
    "columns": [
      {
        "id": "colName",
        "header": "Name",
        "width": "1fr",
        "element": {
          "type": "Microsoft.Common.TextBox",
          "placeholder": "Full name",
          "constraints": {
            "required": true,
            "validations": [
              {
                "isValid": "[startsWith(last(take(steps('grid').people, $rowIndex)).colName, 'contoso')]",
                "message": "Must start with 'contoso'."
              },
              {
                "regex": "^[a-z0-9A-Z]{1,30}$",
                "message": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
              }
            ]
          }
        }
      },
      {
        "id": "colGender",
        "header": "Gender",
        "width": "1fr",
        "element": {
          "name": "dropDown1",
          "type": "Microsoft.Common.DropDown",
          "placeholder": "Select a gender...",
          "constraints": {
            "allowedValues": [
              {
                "label": "Female",
                "value": "female"
              },
              {
                "label": "Male",
                "value": "male"
              },
              {
                "label": "Other",
                "value": "other"
              }
            ],
            "required": true
          }
        }
      },
      {
        "id": "colContactPreference",
        "header": "Contact preference",
        "width": "1fr",
        "element": {
          "type": "Microsoft.Common.OptionsGroup",
          "constraints": {
            "allowedValues": [
              {
                "label": "Email",
                "value": "email"
              },
              {
                "label": "Text",
                "value": "text"
              }
            ],
            "required": true
          }
        }
      }
    ]
  }
}
```

## <a name="sample-output"></a>Örnek çıktı

```json
{
  "colName": "contoso",
  "colGender": "female",
  "colContactPreference": "email"
}
```

## <a name="remarks"></a>Açıklamalar

- Columns dizisinin içindeki geçerli denetimler [TextBox](microsoft-common-textbox.md), [Options Group](microsoft-common-optionsgroup.md)ve [DropDown](microsoft-common-dropdown.md)' dir.
- `$rowIndex`Değişken yalnızca Grid 'in sütunlarının alt öğelerinde yer alan ifadelerde geçerlidir. Bu, öğenin göreli satır dizinini temsil eden bir tamsayıdır ve sayım bir ile başlar ve bir artar. Şemanın bölümünde gösterildiği gibi, `"columns":` `$rowIndex` doğrulama için kullanılır.
- Değişken kullanılarak doğrulamalar gerçekleştirildiğinde `$rowIndex` , ve komutlarını birleştirerek geçerli satırın değerini almak mümkündür `last()` `take()` .

  Örneğin:

  `last(take(<reference_to_grid>, $rowIndex))`

- `label`Özelliği denetimin parçası olarak görünmez, ancak son sekme özetinde görüntülenir.
- `ariaLabel`Özelliği, kılavuz için erişilebilirlik etikettir. Ekran okuyucuları kullanan kullanıcılar için yararlı metin belirtin.
- `constraints.width`Özelliği, kılavuzun genel genişliğini ayarlamak için kullanılır. Seçenekler _dolu_, _Orta_, _küçük_. Varsayılan değer _dolu_' dır.
- `width`Sütunların alt öğelerindeki özelliği sütun genişliğini belirler. Genişlikler, birimlere orantılı sütunlara ayrılan toplam alana sahip _3FR_gibi kesirli birimler kullanılarak belirtilir. Sütun genişliği belirtilmediyse, varsayılan değer _1FR_' dir.

## <a name="next-steps"></a>Sonraki adımlar

- UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
- UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
