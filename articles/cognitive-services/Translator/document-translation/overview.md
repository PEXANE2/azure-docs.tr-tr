---
title: Belge çevirisi nedir?
description: Bulut tabanlı Batch belge çevirisi hizmeti ve işlemine genel bakış.
ms.topic: overview
manager: nitinme
ms.author: lajanuar
author: laujan
ms.date: 02/11/2021
ms.openlocfilehash: f84dc66828f2ebf13316b3348fd93933a2223b5a
ms.sourcegitcommit: c94e282a08fcaa36c4e498771b6004f0bfe8fb70
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2021
ms.locfileid: "105612288"
---
# <a name="what-is-document-translation-preview"></a>Belge çevirisi nedir (Önizleme)?

Belge çevirisi, [Azure Translator](../translator-info-overview.md) hizmetinin bulut tabanlı bir özelliğidir ve Azure bilişsel HIZMET ailesinin REST API 'lerinin bir parçasıdır. Belge çevirisi API 'si, belge yapısını ve veri biçimini koruyarak 90 dil ve diyalektler belgelerini dönüştürür.

Bu belge aşağıdaki makale türlerini içerir:  

* [**Hızlı**](get-started-with-document-translation.md) başlangıçlarda, hizmette istek yapma konusunda size kılavuzluk eden başlangıç yönergeleri bulunur.
* [**Nasıl yapılır kılavuzlarında**](create-sas-tokens.md) , özelliği daha belirgin veya özelleştirilmiş yollarla kullanmaya yönelik yönergeler bulunur.  

## <a name="document-translation-key-features"></a>Belge çevirisi anahtar özellikleri

| Özellik | Açıklama |
| ---------| -------------|
| **Büyük dosyaları çevir**| Tüm belgeleri zaman uyumsuz olarak çevirin.|
|**Çok sayıda dosyayı çevir**|Birden çok dosyayı 90 dil ve diatlerin içine ve içine çevirin.|
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
|Sekmeyle ayrılmış değerler/sekme|. tsv/. Tab| Elektronik tablo programları tarafından kullanılan sekmeyle ayrılmış ham veri dosyası.|
|Metin|.txt| Biçimlendirilmemiş metin belgesi.|
|Çeviri belleği değişimi|. tmx|Bilgisayar destekli çeviri (CAT) ve yerelleştirme uygulamaları tarafından oluşturulan çeviri belleği (TM) verilerini değiş tokuş etmek için kullanılan açık bir XML standardı.|

## <a name="supported-glossary-formats"></a>Desteklenen sözlük biçimleri

Aşağıdaki sözlük dosyası türleri belge çevirisi tarafından desteklenir:

| Dosya türü| Dosya uzantısı|Description|
|---|---|--|
|Yerelleştirme değişim dosyası biçimi|. xlf. , XLIFF| Bir paralel belge biçimi, çeviri belleği sistemlerini dışarı aktarma. Kullanılan diller dosya içinde tanımlanmıştır.|
|Sekmeyle ayrılmış değerler/sekme|. tsv/. Tab| Elektronik tablo programları tarafından kullanılan sekmeyle ayrılmış ham veri dosyası.|

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Belge çevirisi ile çalışmaya başlama](get-started-with-document-translation.md)
