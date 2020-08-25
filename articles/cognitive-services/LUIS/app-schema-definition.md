---
title: Uygulama şeması tanımı
description: LUO uygulaması veya ' de temsil edilir `.json` `.lu` ve tüm amaçları, varlıkları, örnek utbotları, özellikleri ve ayarları içerir.
ms.topic: reference
ms.date: 08/22/2020
ms.openlocfilehash: 816a6c50129f37a55ab3dba72319358e832a6b8b
ms.sourcegitcommit: afa1411c3fb2084cccc4262860aab4f0b5c994ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2020
ms.locfileid: "88756797"
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

* 7. x sürümüne geçiş yapmak için varlıklar, iç içe makine öğrenimi varlıkları olarak temsil edilir.
* Aşağıdaki yazma API 'Lerinde özelliği ile iç içe makine öğrenimi varlıkları yazma desteği `enableNestedChildren` :
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
  "phraselists": [
  ],
  "regex_features": [],
  "patterns": [],
  "settings": []
}
```

| öğesi                  | Yorum                              |
|--------------------------|--------------------------------------|
| "hiyerarşik Öğeler": [],     | Kullanım dışı, [makine öğrenimi varlıklarını](luis-concept-entity-types.md)kullanın.   |
| "bileşik siteler": [],        | Kullanım dışı, [makine öğrenimi varlıklarını](luis-concept-entity-types.md)kullanın. [Bileşik varlık](reference-entity-composite.md) başvurusu. |
| "closedLists": [],       | Birincil olarak varlıklara özellikler olarak kullanılan [varlık başvurusunu listeleyin](reference-entity-list.md) .    |
| "VersionId": "0,1",      | LUSıS uygulamasının sürümü.|
| "ad": "örnek-uygulama",   | LUSıS uygulamasının adı. |
| "desc": "",              | LUIS uygulamasının isteğe bağlı açıklaması.  |
| "kültür": "en-US",      | Uygulamanın [dili](luis-language-support.md) , önceden oluşturulmuş varlıklar, makine öğrenimi ve Simgeleştirici gibi temel özellikleri etkiler.  |
| "tokenizerVersion": "1.0.0", | [Belirteç ayırıcı](luis-language-support.md#tokenization)  |
| "patternAnyEntities": [],   | [Pattern.any varlığı](reference-entity-pattern-any.md)    |
| "regex_entities": [],    |  [Normal ifade varlığı](reference-entity-regular-expression.md)   |
| "phraselists": [],       |  [Tümcecik listeleri (özellik)](luis-concept-feature.md#create-a-phrase-list-for-a-concept)   |
| "regex_features": [],    |  Kullanım dışı, [makine öğrenimi varlıklarını](luis-concept-entity-types.md)kullanın. |
| "desenler": [],          |  Desenler, [desen sözdizimi](reference-pattern-syntax.md) ile [tahmin doğruluğunu geliştirir](luis-concept-patterns.md)   |
| "Ayarlar": []           | [Uygulama ayarları](luis-reference-application-settings.md)|

## <a name="version-6x"></a>Sürüm 6. x

* Sürüm 6. x ' e taşınıyor, varlıklarınızı temsil etmek için yeni [makine öğrenimi varlığını](reference-entity-machine-learned-entity.md) kullanın.

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