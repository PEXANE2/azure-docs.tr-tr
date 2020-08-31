---
title: dosya dahil etme
description: dosya dahil etme
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: include
ms.custom: include file
ms.date: 08/25/2020
ms.openlocfilehash: 4eacc1c4e863ad1a278a4974bb0f6c101aafe7e0
ms.sourcegitcommit: 420c30c760caf5742ba2e71f18cfd7649d1ead8a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89055420"
---
### <a name="change-the-model-update-frequency"></a>Model güncelleştirme sıklığını değiştirme

Azure portal, **yapılandırma** sayfasındaki kişiselleştirici kaynağında, **model güncelleştirme sıklığını** 10 saniye olarak değiştirin. Bu kısa süre, hizmeti hızlı bir şekilde eğitecektir ve her yineleme için en iyi eylem değişikliğini görmenizi sağlar.

![Model güncelleştirme sıklığını Değiştir](../media/settings/configure-model-update-frequency-settings.png)

Bir kişiselleştirici döngüsü ilk kez oluşturulduğunda, üzerinden eğitelenecek bir API çağrısı olmadığından model yoktur. Sıralama çağrıları her öğe için eşit olasılıklara dönüşe sahip olur. Uygulamanız hala Rewarterctionıd çıkışını kullanarak içeriği her zaman derecelendirmelidir.