---
title: Language Understanding için önceden oluşturulmuş modeller
titleSuffix: Azure Cognitive Services
description: LUO, yaygın olarak kullanılan, konuşma Kullanıcı senaryolarını hızlıca eklemek için önceden oluşturulmuş bir modeller kümesi içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/07/2020
ms.author: diberry
ms.openlocfilehash: 653882db4b62d7731123faf7b177da44dbd74e3f
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83585003"
---
# <a name="add-prebuilt-models-for-common-usage-scenarios"></a>Yaygın kullanım senaryoları için önceden oluşturulmuş modeller ekleyin

LUO, yaygın olarak kullanılan, konuşma Kullanıcı senaryolarını hızlıca eklemek için önceden oluşturulmuş bir modeller kümesi içerir. Bu yetenekler için modeller tasarlamaya gerek kalmadan, konuşma istemci uygulamanıza yetenekler eklemenin hızlı ve kolay bir yoludur.

## <a name="add-a-prebuilt-domain"></a>Önceden oluşturulmuş bir alan ekleme

1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.

1. Sol araç çubuğundan **önceden oluşturulmuş etki alanlarını** seçin.

1. Uygulamaya eklenmesini istediğiniz etki alanını bulun ve ardından **etki alanı Ekle** düğmesini seçin.

    > [!div class="mx-imgBorder"]
    > ![Takvim önceden oluşturulmuş etki alanı Ekle](./media/luis-prebuilt-domains/add-prebuilt-domain.png)

## <a name="add-a-prebuilt-intent"></a>Önceden oluşturulmuş bir amaç ekleyin

1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.

1. **Amaçlar** sayfasında, amaçlar listesinin üzerindeki araç çubuğundan **önceden oluşturulmuş etki alanı hedefi Ekle** ' yi seçin.

1. Açılan iletişim kutusundan **yardımcı programlar. Cancel** hedefini seçin.

    > [!div class="mx-imgBorder"]
    > ![Önceden oluşturulmuş amaç Ekle](./media/luis-prebuilt-domains/add-prebuilt-domain-intents.png)

1. **Bitti** düğmesini seçin.

## <a name="add-a-prebuilt-entity"></a>Önceden oluşturulmuş bir varlık ekleyin
1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.
1. Sol taraftaki **varlıkları** seçin.

1. **Varlıklar** sayfasında, **önceden oluşturulmuş varlık Ekle**' yi seçin.

1. **Önceden oluşturulmuş varlıklar Ekle** iletişim kutusunda, önceden oluşturulmuş varlığı seçin.

    > [!div class="mx-imgBorder"]
    > ![Önceden oluşturulmuş varlık Ekle iletişim kutusu](./media/luis-prebuilt-domains/add-prebuilt-entity.png)

1. **Done** (Bitti) öğesini seçin. Varlık eklendikten sonra, uygulamayı eğmenize gerek kalmaz.

## <a name="publish-to-view-prebuilt-model-from-prediction-endpoint"></a>Ön derlenmiş modeli tahmin uç noktasından görüntülemek için Yayımla

Önceden oluşturulmuş bir modelin değerini görüntülemenin en kolay yolu, yayımlanan uç noktadan sorgu kullanmaktır.

## <a name="entities-containing-a-prebuilt-entity-token"></a>Önceden oluşturulmuş bir varlık belirteci içeren varlıklar

Önceden oluşturulmuş bir varlık için gerekli bir özelliğe ihtiyacı olan makine tarafından öğrenilen bir varlığınız varsa, makine tarafından öğrenilen varlığa bir alt bileşen ekleyin ve ardından önceden oluşturulmuş bir varlığın _gerekli_ bir özelliğini ekleyin.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [REST API 'lerle. csv 'den model oluşturma](./luis-tutorial-node-import-utterances-csv.md)
