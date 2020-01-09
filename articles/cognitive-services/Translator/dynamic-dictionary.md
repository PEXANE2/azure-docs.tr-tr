---
title: Dinamik sözlük-Translator Metin Çevirisi API'si
titleSuffix: Azure Cognitive Services
description: Bu makalede, Azure bilişsel hizmetler Translator Metin Çevirisi API'si 'ın dinamik Sözlük özelliğinin nasıl kullanılacağı açıklanmaktadır.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 944bca8644da6127e73af04eb75d01697cd34399
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75446706"
---
# <a name="how-to-use-a-dynamic-dictionary"></a>Dinamik sözlük kullanma

Bir sözcüğe veya ifadeye uygulamak istediğiniz çeviriyi zaten biliyorsanız istek içinde biçimlendirme olarak sağlayabilirsiniz. Dinamik sözlük, yalnızca uygun adlar ve ürün adları gibi bileşik adlarla güvende olur.

**Sözdizimi**

< mstrans: Sözlük çevirisi = "deyimin çevirisi" > tümcecik </mstrans: sözlük >

**Gereksinimler:**

* `From` ve `To` dilleri Ingilizce ve desteklenen başka bir dili içermelidir. 
* Otomatik Algıla özelliğini kullanmak yerine API çeviri isteğinize `From` parametresini eklemeniz gerekir. 

**Örnek: en-de:**

Kaynak girişi: `The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.`

Hedef çıkış: `Das Wort "wordomatic" ist ein Wörterbucheintrag.`

Bu özellik HTML modu olmadan ve ile aynı şekilde çalışmaktadır.

Özelliği gelişigüzel bir şekilde kullanın. Çeviriyi özelleştirmenin daha iyi bir yolu özel çevirici kullanmaktır. Özel çevirici, bağlam ve istatistiksel olasılıkların tam kullanımını sağlar. Çalışma veya tümceciğe bağlam içinde görüntülenen eğitim verileri varsa veya bu verilere sahipseniz, daha iyi sonuçlar elde edersiniz. [https://aka.ms/CustomTranslator](https://aka.ms/CustomTranslator)adresinden özel çevirici hakkında daha fazla bilgi edinebilirsiniz.
