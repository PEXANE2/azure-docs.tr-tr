---
title: Dinamik Sözlük - Çevirmen Metin API
titleSuffix: Azure Cognitive Services
description: Bu makalede, Azure Bilişsel Hizmetler Çevirmeni Metin API'sinin dinamik sözlük özelliğinin nasıl kullanılacağı açıklanmaktadır.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 944bca8644da6127e73af04eb75d01697cd34399
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75446706"
---
# <a name="how-to-use-a-dynamic-dictionary"></a>Dinamik sözlük nasıl kullanılır?

Bir sözcüğe veya tümcecikiçin uygulamak istediğiniz çeviriyi zaten biliyorsanız, istek içinde biçimlendirme olarak sağlayabilirsiniz. Dinamik sözlük yalnızca uygun adlar ve ürün adları gibi bileşik adlar için güvenlidir.

**Sözdizimi:**

<mstrans:sözlük çevirisi=">cümlesi</mstrans:sözlük>

**Gereksinim -leri:**

* `From` Ve `To` dillerİngilizce ve desteklenen başka bir dili içermelidir. 
* Otomatik algılama `From` özelliğini kullanmak yerine API çeviri isteğinize parametre eklemeniz gerekir. 

**Örnek: en-de:**

Kaynak girişi:`The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.`

Hedef çıktı:`Das Wort "wordomatic" ist ein Wörterbucheintrag.`

Bu özellik HTML modu ile ve olmadan aynı şekilde çalışır.

Bu özelliği dikkatli kullanın. Çeviriözelleştirmek için daha iyi bir yol Özel Çevirmen kullanarak. Özel Çevirmen bağlam ve istatistiksel olasılıkları tam olarak kullanır. Çalışmanızı veya ifadenizi bağlam içinde gösteren eğitim verileriniz varsa veya oluşturabiliyorsanız, çok daha iyi sonuçlar alırsınız. Özel Çevirmen hakkında daha fazla [https://aka.ms/CustomTranslator](https://aka.ms/CustomTranslator)bilgiyi .
