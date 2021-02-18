---
title: Belge çevirisi nedir?
description: Bulut tabanlı Batch belge çevirisi hizmeti ve işlemine genel bakış.
ms.topic: overview
manager: nitinme
ms.author: lajanuar
author: laujan
ms.date: 02/11/2021
ms.openlocfilehash: 255de7d6739691c479e1ae98e37bc6d3e01a6710
ms.sourcegitcommit: 58ff80474cd8b3b30b0e29be78b8bf559ab0caa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100643640"
---
# <a name="what-is-document-translation-preview"></a>Belge çevirisi nedir (Önizleme)?

Belge çevirisi, [Azure Translator](../translator-info-overview.md) hizmetinin bulut tabanlı bir özelliğidir ve Azure bilişsel HIZMET ailesinin REST API 'lerinin bir parçasıdır. Belge çevirisi API 'SI, belge yapısını ve veri biçimini korurken, belgeleri 70 ' den fazla dile dönüştürür.

## <a name="document-translation-key-features"></a>Belge çevirisi anahtar özellikleri

| Özellik | Açıklama |
| ---------| -------------|
| **Büyük dosyaları çevir**| Tüm belgeleri zaman uyumsuz olarak çevirin.|
|**Çok sayıda dosyayı çevir**|Birden çok dosyayı 70 ' den fazla dile veya daha fazla dilde çevirin.|
|**Kaynak dosya sunumunu koru**| Özgün düzen ve biçimi koruyarak dosyaları çevirin.|
|**Özel çeviri Uygula**| Genel ve [özel çeviri](../customization.md#custom-translator) modellerini kullanarak belgeleri çevirin.|
|**Özel ışları Uygula**|Özel ışları kullanarak belgeleri çevirin.|

## <a name="how-to-get-started"></a>Nereden başlayacaksınız?

Nasıl yapılır kılavuzumuzdan, belge çeviricisini kullanmaya hızlı bir şekilde nasıl başlayacağınızı öğreneceksiniz. Başlamak için etkin bir [Azure hesabınız](https://azure.microsoft.com/free/cognitive-services/)olması gerekir.  Bir [hesabınız yoksa ücretsiz bir hesap oluşturabilirsiniz](https://azure.microsoft.com/free).

> [!div class="nextstepaction"]
> [Başlarken](get-started-with-document-translation.md)

## <a name="supported-document-formats"></a>Desteklenen belge biçimleri

Aşağıdaki belge dosyası türleri belge çevirisi tarafından desteklenir:

| Dosya türü| Dosya uzantısı|Description|
|---|---|--|
|Adobe PDF|.pdf|Adobe Acrobat taşınabilir belge biçimi|
|HTML|.html|Hiper metin biçimlendirme dili.|
|Yerelleştirme değişim dosyası biçimi|. xlf. , XLIFF| Bir paralel belge biçimi, çeviri belleği sistemlerini dışarı aktarma. Kullanılan diller dosya içinde tanımlanmıştır.|
|Microsoft Excel|.xlsx|Veri analizi ve belgeler için bir elektronik tablo dosyası.|
|Microsoft Outlook|. msg|Microsoft Outlook içinde oluşturulan veya kaydedilen bir e-posta iletisi.|
|Microsoft PowerPoint|.pptx| Bir slayt gösterisi biçiminde içerik göstermek için kullanılan bir sunum dosyası.|
|Microsoft Word|.docx| Bir metin belgesi dosyası.|
|Sekmeyle ayrılmış değerler/sekme|. tsv/. Tab| elektronik tablo programları tarafından kullanılan sekmeyle ayrılmış ham veri dosyası.|
|Metin|.txt| Biçimlendirilmemiş metin belgesi.|
|Çeviri belleği değişimi|. tmx|Bilgisayar destekli çeviri (CAT) ve yerelleştirme uygulamaları tarafından oluşturulan çeviri belleği (TM) verilerini değiş tokuş etmek için kullanılan açık bir XML standardı.|

## <a name="supported-glossary-formats"></a>Desteklenen sözlük biçimleri

Aşağıdaki sözlük dosyası türleri belge çevirisi tarafından desteklenir:

| Dosya türü| Dosya uzantısı|Description|
|---|---|--|
|Yerelleştirme değişim dosyası biçimi|. xlf. , XLIFF| Bir paralel belge biçimi, çeviri belleği sistemlerini dışarı aktarma. Kullanılan diller dosya içinde tanımlanmıştır.|
|Sekmeyle ayrılmış değerler/sekme|. tsv/. Tab| elektronik tablo programları tarafından kullanılan sekmeyle ayrılmış ham veri dosyası.|

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Belge çevirisi ile çalışmaya başlama](get-started-with-document-translation.md)
>
>