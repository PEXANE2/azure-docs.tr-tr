---
title: Dil Anlama için önceden oluşturulmuş modeller
titleSuffix: Azure Cognitive Services
description: LUIS, hızlı bir şekilde ortak, konuşmalı kullanıcı senaryoları eklemek için önceden oluşturulmuş modeller kümesi içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: diberry
ms.openlocfilehash: 0a733aff14b71c6378971c0b72a9ca1f3cd4cfc3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74013545"
---
# <a name="add-prebuilt-models-for-common-usage-scenarios"></a>Yaygın kullanım senaryoları için önceden oluşturulmuş modeller ekleme 

LUIS, hızlı bir şekilde ortak, konuşmalı kullanıcı senaryoları eklemek için önceden oluşturulmuş modeller kümesi içerir. Bu, bu yetenekler için modelleri tasarlamak zorunda kalmadan konuşma istemcisi uygulamasına yeteneklerini eklemek için hızlı ve kolay bir yoldur. 

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

## <a name="add-a-prebuilt-domain"></a>Önceden oluşturulmuş bir alan ekleme

1. **Uygulamalarım** sayfasında uygulamanızı seçin. Bu, uygulamanızı uygulamanın **Yapı** bölümüne açar. 

1. Sol araç çubuğundan **Önceden Oluşturulmuş etki alanlarını** seçin. 

1. Uygulamaya eklenmesini istediğiniz etki alanını bulun ve ardından **etki alanı ekle** düğmesini seçin.

    > [!div class="mx-imgBorder"]
    > ![Takvim önceden oluşturulmuş etki alanı ekleme](./media/luis-prebuilt-domains/add-prebuilt-domain.png)

## <a name="add-a-prebuilt-intent"></a>Önceden oluşturulmuş bir niyet ekleme

1. **Uygulamalarım** sayfasında uygulamanızı seçin. Bu, uygulamanızı uygulamanın **Yapı** bölümüne açar. 

1. **Niyetler** sayfasında, niyet listesinin üzerindeki araç çubuğundan **önceden oluşturulmuş etki alanı amacı ekle'yi** seçin. 

1. Açılan iletişim kutusundan **Yardımcı Programlar.İptal** niyetini seçin. 

    > [!div class="mx-imgBorder"]
    > ![Önceden oluşturulmuş niyet ekleme](./media/luis-prebuilt-domains/add-prebuilt-domain-intents.png)

1. **Bitti** düğmesini seçin.

## <a name="add-a-prebuilt-entity"></a>Önceden oluşturulmuş bir varlık ekleme

1. **Uygulamalarım** sayfasında adını tıklayarak uygulamanızı açın ve ardından sol taraftaki **Varlıklar'ı** tıklatın. 

1. **Varlıklar** sayfasında, önceden **oluşturulmuş varlık ekle'yi**tıklatın.

1. **Önceden oluşturulmuş varlıklar** iletişim kutusunu ekle'de, önceden oluşturulmuş varlığı seçin. 

    > [!div class="mx-imgBorder"]
    > ![Önceden oluşturulmuş varlık iletişim kutusu ekleme](./media/luis-prebuilt-domains/add-prebuilt-entity.png)

1. **Done** (Bitti) öğesini seçin. Varlık eklendikten sonra uygulamayı eğitmene gerek yoktur. 

## <a name="publish-to-view-prebuilt-model-from-prediction-endpoint"></a>Tahmin bitiş noktasından önceden oluşturulmuş modeli görüntülemek için yayımlama

Önceden oluşturulmuş bir modelin değerini görüntülemenin en kolay yolu, yayımlanmış bitiş noktasından sorgu yapmaktır. 

## <a name="entities-containing-a-prebuilt-entity-token"></a>Önceden oluşturulmuş varlık belirteci içeren varlıklar
 
Önceden oluşturulmuş bir varlık tarafından kısıtlanmış bir makine öğrenilen varlığa sahipseniz, makineden öğrenilen varlığa bir alt bileşen ekleyin, ardından önceden oluşturulmuş bir varlığın kısıtlamasını ekleyin.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [REST API'leri ile .csv'den model oluşturma](./luis-tutorial-node-import-utterances-csv.md)
