---
title: Belge biçimleri ve adlandırma kuralları - Özel Çevirmen
titleSuffix: Azure Cognitive Services
description: Bu, Özel Çevirmen'de belge biçimleri ve adlandırma kuralı hakkında bir kılavuzdur. Bu kavram, belge adlarını daha iyi abd çakışmaları adlandırma önlemek yönetmek için yardımcı olur.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 41b15cc998a7bacd033ef2fe083fc99f1bff0286
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68595858"
---
# <a name="document-formats-and-naming-convention-guidance"></a>Belge biçimleri ve adlandırma kuralı kılavuzu

Özel çeviri için kullanılan herhangi bir dosya en az **dört** karakter uzunluğunda olmalıdır.

Bu tablo, çeviri sisteminizi oluşturmak için kullanabileceğiniz tüm desteklenen dosya biçimlerini içerir:

| Biçimlendir            | Uzantılar   | Açıklama                                                                                                                                                                                                                                                                    |
|-------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| XLIFF             | . XLF, . XLIFF | Paralel bir belge biçimi, Çeviri Bellek sistemlerinin dışa aktarım. Kullanılan diller dosya içinde tanımlanır.                                                                                                                                                              |
| Tmx               | . Tmx         | Paralel bir belge biçimi, Çeviri Bellek sistemlerinin dışa aktarım. Kullanılan diller dosya içinde tanımlanır.                                                                                                                                                              |
| Zip               | . Zip         | ZIP bir arşiv dosyası biçimidir.                                                                                                                                                                                                        |
| Locstudio         | . Lcl         | Paralel belgeler için Microsoft biçimi                                                                                                                                                                                                                                      |
| Microsoft Word    | . Docx        | Microsoft Word belgesi                                                                                                                                                                                                                                                        |
| Adobe Akrobat     | . Pdf         | Adobe Acrobat taşınabilir belge                                                                                                                                                                                                                                                |
| HTML              | . Html. HTM  | HTML belgesi                                                                                                                                                                                                                                                                  |
| Metin dosyası         | . Txt         | UTF-16 veya UTF-8 kodlanmış metin dosyaları. Dosya adı Japonca karakterler içermemelidir.                                                                                                                                                                                        |
| Hizalanmış metin dosyası | . Hizalamak       | Uzantı, `.ALIGN` belge çiftindeki cümlelerin mükemmel bir şekilde hizalanmış olduğunu biliyorsanız kullanabileceğiniz özel bir uzantıdır. Bir `.ALIGN` dosya sağlarsanız, Özel Çevirmen cümleleri sizin için hizalamaz. |
| Excel dosyası        | . XLSX        | Excel dosyası (2013 veya sonrası). Elektronik tablonun ilk satırı/satırı dil kodu olmalıdır.                                                                                                                                                                                                                                                      |

## <a name="dictionary-formats"></a>Sözlük biçimleri

Sözlükler için Custom Translator, eğitim kümeleri için desteklenen tüm dosya biçimlerini destekler. Excel sözlüğü kullanıyorsanız, elektronik tablonun ilk satırı/satırı dil kodları olmalıdır.

## <a name="zip-file-formats"></a>Zip dosya biçimleri

Belgeler tek bir zip dosyasında gruplandırılabilir ve yüklenebilir. Özel Çevirmen zip dosya biçimlerini (ZIP, GZ ve TGZ) destekler.

TXT, HTML, HTM, PDF, DOCX, ALIGN uzantılı zip dosyasındaki her belge bu adlandırma kuralını izlemelidir:

{belge adı} \_{belge adı} belgenizin adı dır, {dil kodu} ISO LanguageID (iki karakter) ve belgenin o dilde cümleler içerdiğini gösterir. Dil kodundan önce bir alt skor (_) olmalıdır.

Örneğin, Bir İngilizce'den İspanyolca sisteme bir zip içinde iki paralel belge yüklemek için, dosyaların "data_en" ve "data_es" olarak adlandırılması gerekir.

Çeviri Bellek dosyaları (TMX, XLF, XLIFF, LCL, XLSX) belirli dil adlandırma kuralını izlemek için gerekli değildir.  

## <a name="next-steps"></a>Sonraki adımlar

- Bunları oluşturmak ve yönetmek için [proje](workspace-and-project.md#what-is-a-custom-translator-project) hakkında bilgi edinin.
