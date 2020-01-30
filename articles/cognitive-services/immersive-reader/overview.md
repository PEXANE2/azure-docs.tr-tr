---
title: Tam ekran okuyucu API nedir?
titleSuffix: Azure Cognitive Services
description: Modern okuyucu API 'SI, kişileri öğrenme farklılığı veya yeni okuyucular ve dil öğrenimine yardımcı olmak için kullanılabilen bir araçtır.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: overview
ms.date: 01/4/2020
ms.author: metan
ms.openlocfilehash: 5acd6e4be178c7e645eb6855a79cd0f58303ba9a
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76846428"
---
# <a name="what-is-immersive-reader"></a>Tam Ekran Okuyucu nedir?

[Modern okuyucu](https://www.onenote.com/learningtools) , gelişmekte olan okuyucular, dil öğrenimi ve öğrenimi gibi öğrenme farklılığı olan kişiler için okuma kavramasını geliştiren, benzer şekilde tasarlanmış bir araçtır.

Tam Ekran Okuyucu SDK’sını kullanarak web uygulamanızda Tam Ekran Okuyucu’yu kullanabilirsiniz.

## <a name="what-does-immersive-reader-do"></a>Modern okuyucu ne yapar?

Derinlikli okuyucu herkes için daha fazla erişilebilir hale getirmek üzere tasarlanmıştır.

* En küçük okuma görünümündeki içeriği gösterir

  ![Tam Ekran Okuyucu](./media/immersive-reader.png)

* Yaygın olarak kullanılan sözcüklerin resimlerini görüntüler

  ![Resim sözlüğü](./media/picture-dictionary.png)

* İsimler, fiiller, sıfatlar ve zarflara önemli noktalar

  ![Konuşma bölümleri](./media/parts-of-speech.png)

* İçeriğinizi sizin için sesli okur

  ![Sesli oku](./media/read-aloud.png)

* İçeriğinizi başka bir dile çevirir

  ![Çeviri](./media/translation.png)

* Sözcükleri hecelere ayırır

  ![Heceye birleşme](./media/syllabification.png)

## <a name="how-does-immersive-reader-work"></a>Modern okuyucu nasıl çalışır?

Tam ekran okuyucu, modern okuyucu JavaScript SDK 'Sı kullanılarak çağrıldığında, mevcut Web uygulamanızın en üstünde bir `iframe`ile görüntülendiğinde tek başına bir Web uygulamasıdır. Modern okuyucuyu başlatmak için API 'YI çağırdığınızda, tam ekran okuyucusunda göstermek istediğiniz içeriği belirtin. SDK, `iframe` oluşturma ve stillendirme, okuma ve metin okuma, çeviri vb. için içeriği işleyen, derinlikli okuyucu arka uç hizmetiyle iletişim kurmasından de ilgilenir.

## <a name="next-steps"></a>Sonraki adımlar

Tam Ekran Okuyucu’yu kullanmaya başlama:

* [Hızlı](./quickstart.md) başlangıca atlayın
* [GitHub 'Da modern Okuyucu SDK 'sını](https://github.com/microsoft/immersive-reader-sdk) keşfet
* [Tam ekran okuyucu SDK başvurusunu](./reference.md) okuyun