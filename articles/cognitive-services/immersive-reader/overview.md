---
title: Sürükleyici Okuyucu API nedir?
titleSuffix: Azure Cognitive Services
description: Immersive Reader API öğrenme farklılıkları olan kişileri barındırmak veya yeni okuyuculara ve dil öğrenenlere yardımcı olmak için kullanılabilecek bir araçtır.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: overview
ms.date: 01/4/2020
ms.author: metan
ms.openlocfilehash: 2eab7ab62f6a7a105dab33aa889af9f2be3a3fa3
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80052376"
---
# <a name="what-is-immersive-reader"></a>Tam Ekran Okuyucu nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

[Immersive Reader,](https://www.onenote.com/learningtools) gelişmekte olan okuyucular, dil öğrenenler ve disleksi gibi öğrenme farklılıkları olan kişiler için okuma anlama geliştirmek için kanıtlanmış teknikleri uygulayan kapsayıcı bir şekilde tasarlanmış bir araçtır.

Tam Ekran Okuyucu SDK’sını kullanarak web uygulamanızda Tam Ekran Okuyucu’yu kullanabilirsiniz.

## <a name="what-does-immersive-reader-do"></a>Immersive Reader ne yapar?

Immersive Reader, okumayı herkes için daha erişilebilir hale getirmek için tasarlanmıştır.

* İçeriği en az okuma görünümünde gösterir

  ![Tam Ekran Okuyucu](./media/immersive-reader.png)

* Yaygın olarak kullanılan sözcüklerin resimlerini görüntüler

  ![Resimli Sözlük](./media/picture-dictionary.png)

* İsimleri, fiilleri, sıfatları ve zarfları vurgular

  ![Konuşma Bölümleri](./media/parts-of-speech.png)

* İçeriğinizi size yüksek sesle okur

  ![Yüksek Sesle Oku](./media/read-aloud.png)

* İçeriğinizi başka bir dile çevirir

  ![Çeviri](./media/translation.png)

* Kelimeleri heceye ayırır

  ![Syllabification](./media/syllabification.png)

## <a name="how-does-immersive-reader-work"></a>Immersive Reader nasıl çalışır?

Immersive Reader, Immersive Reader JavaScript SDK kullanılarak çağrıldığında, mevcut web uygulamanızın üstünde bir `iframe`. Sürükleyici Okuyucu'yu başlatmak için API'yi aradiğinizde, Sürükleyici Okuyucu'da göstermek istediğiniz içeriği belirtirsiniz. SDK'mız, konuşma, metinden `iframe` konuşmaya, çeviri ve benzeri bölümlerin içeriğini işleyen Sürükleyici Reader arka uç hizmetiile iletişimin oluşturulmasını ve şekillendirilmesini yönetir.

## <a name="next-steps"></a>Sonraki adımlar

Tam Ekran Okuyucu’yu kullanmaya başlama:

* [Hızlı başlatmaya atla](./quickstart.md)
* [GitHub'da Sürükleyici Reader SDK'yı](https://github.com/microsoft/immersive-reader-sdk) keşfedin
* Sürükleyici [Okuyucu SDK Referans](./reference.md) okuyun
