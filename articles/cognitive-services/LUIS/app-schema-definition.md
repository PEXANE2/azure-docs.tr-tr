---
title: Uygulama şeması tanımı
description: LUO uygulaması veya ' de temsil edilir `.json` `.lu` ve tüm amaçları, varlıkları, örnek utbotları, özellikleri ve ayarları içerir.
ms.topic: reference
ms.date: 05/05/2020
ms.openlocfilehash: 21b58f79ffd2baf553c6f8b07daa84473e620f77
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83599375"
---
# <a name="app-schema-definition"></a>Uygulama şeması tanımı

LUO uygulaması veya ' de temsil edilir `.json` `.lu` ve tüm amaçları, varlıkları, örnek utbotları, özellikleri ve ayarları içerir.

## <a name="format"></a>Biçimlendir

Uygulamayı içeri ve dışarı aktardığınızda, veya seçeneklerinden birini belirleyin `.json` `.lu` .

|Biçimlendir|Bilgi|
|--|--|
|`.json`| Standart programlama biçimi|
|`.lu`|Bot Framework 'Ün [bot Builder araçları](https://github.com/microsoft/botbuilder-tools/blob/master/packages/Ludown/docs/lu-file-format.md)tarafından desteklenir.|

## <a name="version-7x"></a>Sürüm 7. x

* 7. x sürümüne geçiş yapmak için varlıklar, iç içe yerleştirilmiş makine tarafından öğrenilen varlıklar olarak temsil edilir.
* Aşağıdaki yazma API 'Lerinde özelliği olan iç içe makine ile öğrenilen varlıkları yazma desteği `enableNestedChildren` :
    * [Etiket ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c08)
    * [Batch etiketi ekle](https://westus.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c09)
    * [Etiketleri gözden geçir](https://westus.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c0a)
    * [Varlıklar için uç nokta sorguları öner](https://westus.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c2e)
    * [Amaçlar için uç nokta sorguları önerin](https://westus.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c2d)

```json
{
  "luis_schema_version": "7.0.0",
  "intents": [
    {
      "name": "None",
      "features": []
    }
  ],
  "entities": [],
  "hierarchicals": [],
  "composites": [],
  "closedLists": [],
  "prebuiltEntities": [],
  "utterances": [],
  "versionId": "0.1",
  "name": "example-app",
  "desc": "",
  "culture": "en-us",
  "tokenizerVersion": "1.0.0",
  "patternAnyEntities": [],
  "regex_entities": [],
  "phraselists": [],
  "regex_features": [],
  "patterns": [],
  "settings": []
}
```

## <a name="version-6x"></a>Sürüm 6. x

* Sürüm 6. x ' e taşınıyor, varlıklarınızı temsil etmek için [makine tarafından öğrenilen yeni varlığı](reference-entity-machine-learned-entity.md) kullanın.

```json
{
  "luis_schema_version": "6.0.0",
  "intents": [
    {
      "name": "None",
      "features": []
    }
  ],
  "entities": [],
  "hierarchicals": [],
  "composites": [],
  "closedLists": [],
  "prebuiltEntities": [],
  "utterances": [],
  "versionId": "0.1",
  "name": "example-app",
  "desc": "",
  "culture": "en-us",
  "tokenizerVersion": "1.0.0",
  "patternAnyEntities": [],
  "regex_entities": [],
  "phraselists": [],
  "regex_features": [],
  "patterns": [],
  "settings": []
}
```

## <a name="version-4x"></a>Sürüm 4. x

```json
{
  "luis_schema_version": "4.0.0",
  "versionId": "0.1",
  "name": "example-app",
  "desc": "",
  "culture": "en-us",
  "tokenizerVersion": "1.0.0",
  "intents": [
    {
      "name": "None"
    }
  ],
  "entities": [],
  "composites": [],
  "closedLists": [],
  "patternAnyEntities": [],
  "regex_entities": [],
  "prebuiltEntities": [],
  "model_features": [],
  "regex_features": [],
  "patterns": [],
  "utterances": [],
  "settings": []
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [V3 yazma API 'lerine](luis-migration-authoring-entities.md) geçiş