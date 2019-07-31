---
title: Tam ekran okuyucu API nedir?
titleSuffix: Azure Cognitive Services
description: Modern okuyucu API 'SI hakkında bilgi edinin.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: overview
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: f84804fbad07357706e786fc449fce9e42a5ec4a
ms.sourcegitcommit: d585cdda2afcf729ed943cfd170b0b361e615fae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68688342"
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

Modern okuyucu, modern okuyucu JavaScript SDK 'Sı kullanılarak çağrıldığında, mevcut Web uygulamanızın en üstünde bir `iframe`ile görüntülenen tek başına bir Web uygulamasıdır. Modern okuyucuyu başlatmak için API 'YI çağırdığınızda, tam ekran okuyucusunda göstermek istediğiniz içeriği belirtin. SDK 'mız, okuma ve metin okuma, `iframe` çeviri vb. için içeriği işleyen, tam ekran okuyucu arka uç hizmeti ile iletişim oluşturma ve stillendirmeyi idare eder.

## <a name="next-steps"></a>Sonraki adımlar

Tam Ekran Okuyucu’yu kullanmaya başlama:

* [Hızlı](./quickstart.md) başlangıca atlayın
* [GitHub 'Da modern Okuyucu SDK 'sını](https://github.com/microsoft/immersive-reader-sdk) keşfet
* [Tam ekran okuyucu SDK başvurusunu](./reference.md) okuyun