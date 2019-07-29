---
title: Dinamik sözlük-Translator Metin Çevirisi API'si
titleSuffix: Azure Cognitive Services
description: Translator Metin Çevirisi API'si dinamik sözlük özelliğini kullanma.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: f586702350b38c6d546767f64a8e2f6115b59817
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68595295"
---
# <a name="how-to-use-a-dynamic-dictionary"></a>Dinamik sözlük kullanma

Bir sözcüğe veya ifadeye uygulamak istediğiniz çeviriyi zaten biliyorsanız istek içinde biçimlendirme olarak sağlayabilirsiniz. Dinamik sözlük, yalnızca uygun adlar ve ürün adları gibi bileşik adlarla güvende olur.

**Sözdizimi**

< mstrans: Sözlük çevirisi = "deyimin çevirisi" > tümcecik </mstrans: sözlük >

**Gereklilik**

* `From` Ve`To` dilleri farklı olmalıdır. 
* Otomatik Algıla özelliğini kullanmak `From` yerine API çeviri isteğinize parametresini eklemeniz gerekir. 

**Örnek: en-de:**

Kaynak girişi: < Mstrans: Sözlük çevirisi =\"wordomatik\"> sözcük veya tümcecik </mstrans: sözlük > sözlük girişi.

Hedef çıktı: "Wordomatik" ist Ein Wörterbucheintrag.

Bu özellik HTML modu olmadan ve ile aynı şekilde çalışmaktadır.

Özelliğin gelişigüzel kullanılması gerekir. Çeviriyi özelleştirmenin uygun ve en iyi yolu özel çeviriciyi kullanmaktır. Özel çevirici, bağlam ve istatistiksel olasılıkların tam kullanımını sağlar. Çalışma veya tümceciğe bağlam içinde görüntülenen eğitim verileri varsa veya bu verilere sahipseniz, daha iyi sonuçlar elde edersiniz. İçinde özel çevirici [https://aka.ms/CustomTranslator](https://aka.ms/CustomTranslator)hakkında daha fazla bilgi edinebilirsiniz.
