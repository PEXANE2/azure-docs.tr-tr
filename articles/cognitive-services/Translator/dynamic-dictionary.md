---
title: Dinamik sözlük-Translator
titleSuffix: Azure Cognitive Services
description: Bu makalede, Azure bilişsel hizmetler çeviricisinin dinamik Sözlük özelliğinin nasıl kullanılacağı açıklanmaktadır.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 3ef881efc417a6c3816a92d9af7d77bba74355aa
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83592551"
---
# <a name="how-to-use-a-dynamic-dictionary"></a>Dinamik sözlük kullanma

Bir sözcüğe veya ifadeye uygulamak istediğiniz çeviriyi zaten biliyorsanız istek içinde biçimlendirme olarak sağlayabilirsiniz. Dinamik sözlük, yalnızca uygun adlar ve ürün adları gibi bileşik adlarla güvende olur.

**Sözdizimi**

<mstrans: Sözlük çevirisi = "deyimin çevirisi" >tümcecik</mstrans: Sözlük>

**Gereklilik**

* `From`Ve `To` dilleri İngilizce ve desteklenen başka bir dili içermelidir. 
* `From`Otomatik Algıla özelliğini kullanmak yerıne API çeviri isteğinize parametresini eklemeniz gerekir. 

**Örnek: en-de:**

Kaynak girişi:`The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.`

Hedef çıktı:`Das Wort "wordomatic" ist ein Wörterbucheintrag.`

Bu özellik HTML modu olmadan ve ile aynı şekilde çalışmaktadır.

Özelliği gelişigüzel bir şekilde kullanın. Çeviriyi özelleştirmenin daha iyi bir yolu özel çevirici kullanmaktır. Özel çevirici, bağlam ve istatistiksel olasılıkların tam kullanımını sağlar. Çalışma veya tümceciğe bağlam içinde görüntülenen eğitim verileri varsa veya bu verilere sahipseniz, daha iyi sonuçlar elde edersiniz. İçinde özel çevirici hakkında daha fazla bilgi edinebilirsiniz [https://aka.ms/CustomTranslator](https://aka.ms/CustomTranslator) .