---
title: Tam ekran okuyucu nedir?
titleSuffix: Azure Cognitive Services
description: Modern okuyucu, öğrenme farkları olan kişilerin öğrenimine ve yeni okuyucular ve dil öğrenimine yardımcı olmak için tasarlanan bir araçtır.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: overview
ms.date: 01/4/2020
ms.author: metang
ms.custom: cog-serv-seo-aug-2020
keywords: okuyucular, dil öğrenenler, resim görüntüleme, okuma, okuma içeriği, çeviri
ms.openlocfilehash: 6affc0a2b94c7ea950a8995981d63470722d981f
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104599536"
---
# <a name="what-is-immersive-reader"></a>Tam Ekran Okuyucu nedir?

[Modern okuyucu](https://www.onenote.com/learningtools) , yeni okuyucular, dil öğrenimi ve öğrenimi gibi öğrenme farklılığı olan kişiler için okuma kavramasını geliştirmek üzere kanıtlanmış teknikler uygulayan, benzer şekilde tasarlanmış bir araçtır. Modern okuyucu istemci kitaplığı ile, Web uygulamalarınızı geliştirmek için Microsoft Word ve Microsoft One notunda kullanılan teknolojinin aynısını kullanabilirsiniz. 

Bu belge aşağıdaki makale türlerini içerir:  

* **[Hızlı başlangıç](quickstarts/client-libraries.md)** , hizmete çağrı yapıp sonuçları elde etmeniz için adım adım yönergeler sağlar.
* **[Nasıl yapılır kılavuzlarında](how-to-create-immersive-reader.md)** , hizmetin daha belirli veya özelleştirilmiş yollarla kullanılmasına ilişkin yönergeler bulunur.  

## <a name="use-immersive-reader-to-improve-reading-accessibility"></a>Modern okuyucu kullanarak okuma erişilebilirliği geliştirme 

Derinlikli okuyucu, okuma daha kolay ve herkes için daha erişilebilir hale getirmek için tasarlanmıştır. Daha basit bir okuyucu 'nın temel özelliklerine göz atalım.

### <a name="isolate-content-for-improved-readability"></a>İyileştirilmiş okunabilirlik için içerik yalıtma

Modern okuyucu, okunabilirliği artırmak için içeriği yalıtır. 

  ![Modern okuyucuyla gelişmiş okunabilirlik için içerik yalıtma](./media/immersive-reader.png)

### <a name="display-pictures-for-common-words"></a>Ortak sözcüklerin resimlerini görüntüleme

Yaygın olarak kullanılan şartlar için, derinlikli okuyucu bir resim görüntüler.

  ![Modern okuyucu ile resim sözlüğü](./media/picture-dictionary.png)

### <a name="highlight-parts-of-speech"></a>Konuşma parçalarını Vurgula

Tam ekran okuyucu, fiilleri, isimleri, zamirler ve daha fazlasını vurgulayarak konuşma ve dilbilgisini anlamak için kullanılabilir.

  ![Tam ekran okuyucu ile konuşma parçalarını göster](./media/parts-of-speech.png)

### <a name="read-content-aloud"></a>İçeriği sesli oku

Konuşma senmetini (veya metinden konuşmaya), okuyucularınızın sesli okunacak metin seçmesini sağlayan, derinlikli okuyucu hizmetine bakmış olur. 

  ![Tam ekran okuyucu ile metni sesli oku](./media/read-aloud.png)

### <a name="translate-content-in-real-time"></a>İçeriği gerçek zamanlı olarak çevir

Modern okuyucu, metni gerçek zamanlı olarak birçok dile çevirebilir. Bu, okuyucuların yeni bir dil öğreniminde daha fazla bilgi edinilmesine yardımcı olur.

  ![Tam ekran okuyucu ile metin çevirme](./media/translation.png)

### <a name="split-words-into-syllables"></a>Sözcükleri hecelere Böl

Modern okuyucu sayesinde, okunabilirliği artırmak veya yeni sözcükleri kesmek için kelimeleri hecelere kesebilirsiniz.

  ![Derinlikli okuyucu ile sözcükleri hecelere bölün](./media/syllabification.png)

## <a name="how-does-immersive-reader-work"></a>Modern okuyucu nasıl çalışır?

Modern okuyucu, tek başına bir Web uygulamasıdır. Modern okuyucu istemci kitaplığı kullanılarak çağrıldığında, içindeki mevcut Web uygulamanızın üstünde görüntülenir `iframe` . WEP uygulamanız, derinlikli okuyucu hizmetini çağırdığında, okuyucunun gösterileceği içeriği belirlersiniz. Tam ekran okuyucusu istemci kitaplığı, `iframe` tam ekran okuyucusu arka uç hizmetiyle ve iletişimini oluşturma ve stillendirilmesini işler. Modern okuyucu hizmeti, konuşma parçaları, metin okuma, çeviri ve daha fazlasına ait içeriği işler.

## <a name="get-started-with-immersive-reader"></a>Modern okuyucuyla çalışmaya başlama

Tam ekran okuyucu istemci kitaplığı C#, JavaScript, Java (Android), Kotlin (Android) ve Swift (iOS) ile kullanılabilir. Kullanmaya başlayın:

* [Hızlı başlangıç: tam ekran okuyucusu istemci kitaplığını kullanma](quickstarts/client-libraries.md)
