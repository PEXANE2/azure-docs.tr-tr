---
title: Düzenler-form tanıyıcı
titleSuffix: Azure Cognitive Services
description: Tanıyıcı API kullanımı ve limitleriyle düzen analizine ilişkin kavramları öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 11/18/2020
ms.author: pafarley
ms.openlocfilehash: b489964e5ef8ea483cbe203be1ff665078a6d66a
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95741738"
---
# <a name="form-recognizer-layout-service"></a>Form tanıyıcı düzen hizmeti

Azure form tanıyıcı, düzen hizmetini kullanarak belgelerden metin, tablo, seçim işareti ve yapı bilgilerini ayıklayabilir. Düzen API 'SI, müşterilerin belgeleri çeşitli biçimlerde almasını ve yapılandırılmış verileri ve belgenin temsilini döndürmesini sağlar. Güçlü [optik karakter tanıma (OCR)](https://docs.microsoft.com/azure/cognitive-services/computer-vision/concept-recognizing-text) olanaklarımızın metin, tablo, seçim işaretlerini ve belge yapısını ayıklamak için derin öğrenme modellerini anlama başlıklı belge ile birleştirir. 

## <a name="what-does-the-layout-service-do"></a>Düzen hizmeti ne yapar?

Düzen API 'SI, önemli doğruluk içeren belgelerden metin, tablo, seçim işaretleri ve yapı bilgilerini ayıklar ve bunları düzenlenmiş yapılandırılmış bir JSON yanıtında döndürür. Belgeler, telefon ile yakalanan görüntüler, taranmış belgeler ve dijital PDF 'Ler dahil olmak üzere çeşitli biçimlerdeki ve kaliteden olabilir. Düzen API 'SI tüm bu belgelerden yapılandırılmış çıktıyı ayıklar.

![Düzen örneği](./media/layout-tool-example.JPG)

## <a name="try-it-out"></a>Deneyin

Form tanıyıcı düzen hizmetini denemek için çevrimiçi örnek UI aracına gidin:

> [!div class="nextstepaction"]
> [Örnek Kullanıcı Arabirimi](https://fott-preview.azurewebsites.net/)

Form tanıyıcı düzeni API 'sini denemek için bir Azure aboneliğine ([ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)) ve bir [form tanıyıcı kaynak](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer) uç noktası ve anahtarına ihtiyacınız olacaktır. 

![Örnek UI ekran görüntüsü; bir belgenin metni, tabloları ve seçim işaretleri çözümlenmekte](./media/analyze-layout.png)

### <a name="input-requirements"></a>Giriş gereksinimleri 

[!INCLUDE [input requirements](./includes/input-requirements-receipts.md)]

## <a name="the-analyze-layout-operation"></a>Düzen çözümleme işlemi

[Düzen çözümleme](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-2/operations/AnalyzeLayoutAsync) işlemi, giriş olarak bir belge (görüntü, TIFF veya PDF dosyası) alır ve metin, tablolar, seçim işaretleri ve belgenin yapısını ayıklar. Çağrı, adlı bir yanıt üst bilgisi alanı döndürüyor `Operation-Location` . `Operation-Location`Değer, bir sonraki adımda kullanılacak sonuç kimliğini içeren BIR URL 'dir.

|Yanıt üst bilgisi| Sonuç URL 'SI |
|:-----|:----|
|Operation-Location | `https://cognitiveservice/formrecognizer/v2.1-preview.2/prebuilt/layout/analyzeResults/44a436324-fc4b-4387-aa06-090cfbf0064f` |

## <a name="the-get-analyze-layout-result-operation"></a>Düzen çözümleme sonucunu al işlemi

İkinci adım, [Çözümleme düzenleme sonucunu al](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-2/operations/GetAnalyzeLayoutResult) işlemini çağıracaktır. Bu işlem, düzen çözümleme işlemi tarafından oluşturulan sonuç KIMLIĞINI giriş olarak alır. Aşağıdaki olası değerlere sahip bir **durum** alanı IÇEREN bir JSON yanıtı döndürür. 

|Alan| Tür | Olası değerler |
|:-----|:----:|:----|
|durum | string | `notStarted`: Analiz işlemi başlatılmamış.<br /><br />`running`: Analiz işlemi devam ediyor.<br /><br />`failed`: Çözümleme işlemi başarısız oldu.<br /><br />`succeeded`: Çözümleme işlemi başarılı oldu.|

Bu işlemi, değere dönene kadar tekrarlayarak çağırın `succeeded` . Saniye başına istek (RPS) oranını aşmamak için 3 ile 5 saniye arasında bir Aralık kullanın.

**Durum** alanı `succeeded` değeri olduğunda JSON yanıtı, düzen ayıklama sonuçları, metin, tablolar ve seçim işaretlerini ayıkladıktan sonra dahil edilir. Ayıklanan veriler, ayıklanan metin satırları ve kelimeleri, sınırlayıcı kutusu, metin görünümü el yazısı belirtme, tablolar ve seçim işaretlerini seçili/seçilmemiş durumuyla birlikte içerir. 

### <a name="sample-json-output"></a>Örnek JSON çıkışı

Düzen çözümleme sonucunu al işleminin yanıtı, ayıklanan tüm bilgileri içeren belgenin yapısal gösterimi olur. [Örnek belge dosyası](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/curl/form-recognizer/sample-layout.pdf) ve yapılandırılmış çıkış [örneği düzen çıktısı](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/curl/form-recognizer/sample-layout-output.json)için buraya bakın.

JSON çıktısı iki bölümden oluşur: 
* `"readResults"` düğüm, tüm tanınan metin ve seçim işaretlerini içerir. Metin sayfaya, sonra satıra, sonra da tek sözcüklere göre düzenlenir. 
* `"pageResults"` düğüm, sınırlayıcı kutuları, güvenle ayıklanan tabloları ve hücreleri ve "readResults" içindeki satır ve sözcüklere yönelik bir başvuruyu içerir.

## <a name="example-output"></a>Örnek çıkış

### <a name="text"></a>Metin

Düzen, belgelerden (PDF, TIFF) ve görüntülerden (jpg, PNG, BMP) metin metinlerini, farklı metin açılarına, renklerle, açılarına, belge fotoğraflarını, faksları, yazdırılmış, el ile (Yalnızca Ingilizce) ve karışık modlarına sahip metinleri ayıklar. Metin, çizgiler, kelimeler, sınırlama kutuları, güven puanları ve stil (el ile veya diğer) hakkındaki bilgilerle ayıklanır. Tüm metin bilgileri `"readResults"` JSON çıktısının bölümüne dahil edilir. 

### <a name="tables"></a>Tablolar

Düzen belgelerden (PDF, TIFF) ve görüntülerden (jpg, PNG, BMP) tabloları ayıklar. Belgeler taranabilir, fotokopi veya dijital olabilir. Tablolar, birleştirilmiş hücreleri veya sütunları olan, kenarlıklı veya olmayan ve tek açılarla karmaşık tablolar olabilir. Ayıklanan tablolar, sütun ve satır sayısını, satır yayılımını ve sütun yayılımını içerir. Her bir hücre, sınırlayıcı kutusuyla ayıklanır ve bölümünde ayıklanan metne başvurur `"readResults"` . Tablo bilgileri, `"pageResults"` JSON çıktısının bölümünde bulunur. 

![Tablolar örneği](./media/tables-example.jpg)

### <a name="selection-marks"></a>Seçim işaretleri

Düzen Ayrıca belgelerden seçim işaretlerini de ayıklar. Ayıklanan seçim işaretleri sınırlayıcı kutusu, güvenirlik ve durumu (seçili/seçilmemiş) içerir. Seçim işareti bilgileri `"readResults"` JSON çıktısının bölümünde ayıklanır. 

## <a name="next-steps"></a>Sonraki adımlar

- Bir [Biçim tanıyıcı örnek kullanıcı arabirimini kullanarak kendi düzen ayıklayıcınızı](https://fott-preview.azurewebsites.net/) deneyin
- Seçtiğiniz dilde düzenleri çıkarmaya başlamak için [form tanıyıcı istemci kitaplığı hızlı](quickstarts/client-library.md) başlangıcını doldurun.
- Ya da Python ve REST API kullanarak düzen verilerinin ayıklanmasını uygulamak için [düzen verilerini](./QuickStarts/python-layout.md) ayıklama hızlı başlangıcını izleyin.

## <a name="see-also"></a>Ayrıca bkz.

* [Form Tanıma nedir?](./overview.md)
* [REST API başvuru belgeleri](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-2/operations/AnalyzeLayoutAsync)




