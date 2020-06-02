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
ms.openlocfilehash: b9efe70e8658e25d61decffbe44dec776890b17b
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84267282"
---
# <a name="what-is-immersive-reader"></a>Tam Ekran Okuyucu nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

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

Modern okuyucu, modern okuyucu JavaScript SDK 'Sı kullanılarak çağrıldığında, mevcut Web uygulamanızın en üstünde bir ile görüntülenen tek başına bir Web uygulamasıdır `iframe` . Modern okuyucuyu başlatmak için API 'YI çağırdığınızda, tam ekran okuyucusunda göstermek istediğiniz içeriği belirtin. SDK 'mız, `iframe` okuma ve metin okuma, çeviri vb. için içeriği işleyen, tam ekran okuyucu arka uç hizmeti ile iletişim oluşturma ve stillendirmeyi idare eder.

## <a name="next-steps"></a>Sonraki adımlar

Tam Ekran Okuyucu’yu kullanmaya başlama:

* [Hızlı](./quickstarts/client-libraries.md?pivots=programming-language-csharp) başlangıçlara atlayın
* [GitHub 'Da modern Okuyucu SDK 'sını](https://github.com/microsoft/immersive-reader-sdk) keşfet
* [Tam ekran okuyucu SDK başvurusunu](./reference.md) okuyun
