---
title: Faturalar-form tanıyıcı
titleSuffix: Azure Cognitive Services
description: Algılayıcı API 'SI kullanımı ve limitleriyle birlikte fatura analizine ilişkin kavramları öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 11/18/2020
ms.author: pafarley
ms.openlocfilehash: 875fa2ad5a3a18c0e2582425a6528730234ac8ed
ms.sourcegitcommit: 2817d7e0ab8d9354338d860de878dd6024e93c66
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99585084"
---
# <a name="form-recognizer-prebuilt-invoice-model"></a>Form tanıyıcı önceden oluşturulmuş fatura modeli

Azure form tanıyıcı, önceden oluşturulmuş fatura modellerini kullanarak satış faturalarından bilgileri çözümleyebilir ve ayıklayabilir. Fatura API 'SI, müşterilerin çeşitli biçimlerde faturalar almasını sağlar ve fatura işlemini otomatikleştirmek için yapılandırılmış verileri döndürür. Güçlü [optik karakter tanıma (OCR)](../computer-vision/concept-recognizing-text.md) olanaklarımızı, fatura ile ilgili ayrıntılı öğrenme modellerini anlamak için, gelişmiş bir deyişle, önemli bilgileri İngilizce olarak birleştirir. Metin, tablo ve müşteri, satıcı, fatura KIMLIĞI, fatura son tarihi, toplam, fatura tutarı, ödenecek vergi tutarı, sevk edilecek, fatura vb. gibi bilgileri ayıklar. Önceden oluşturulmuş fatura API 'SI, tanıyıcı v 2.1 Preview biçiminde herkese açık bir şekilde sunulmaktadır.

## <a name="what-does-the-invoice-service-do"></a>Fatura hizmeti ne yapar?

Fatura API 'SI, ana alanları faturalardan ayıklar ve bunları düzenlenmiş yapılandırılmış bir JSON yanıtında döndürür. Faturalar, telefon ile yakalanan görüntüler, taranmış belgeler ve dijital PDF 'Ler dahil olmak üzere çeşitli biçimlerdeki ve kaliteden olabilir. Fatura API 'SI tüm bu faturalardan yapılandırılmış çıktıyı ayıklar. 

![Contoso fatura örneği](./media/invoice-example.jpg)

## <a name="try-it-out"></a>Deneyin

Form tanıyıcı fatura hizmetini denemek için çevrimiçi örnek UI aracına gidin:

> [!div class="nextstepaction"]
> [Önceden oluşturulmuş modelleri deneyin](https://fott-preview.azurewebsites.net/)

Form tanıyıcı fatura hizmetini denemek için bir Azure aboneliğine ([ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)) ve bir [form tanıyıcı kaynak](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer) uç noktası ve anahtarına ihtiyacınız olacak. 

![Çözümlenen fatura örneği](./media/analyze-invoice.png)


### <a name="input-requirements"></a>Giriş gereksinimleri 

[!INCLUDE [input reqs](./includes/input-requirements-receipts.md)]

## <a name="the-analyze-invoice-operation"></a>Fatura çözümleme işlemi

[Faturayı çözümle](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-2/operations/5ed8c9843c2794cbb1a96291) işlemi, giriş olarak bir faturanın GÖRÜNTÜSÜNÜ veya PDF 'sini alır ve ilgilendiğiniz değerleri ayıklar. Çağrı, adlı bir yanıt üst bilgisi alanı döndürüyor `Operation-Location` . `Operation-Location`Değer, bir sonraki adımda kullanılacak sonuç kimliğini içeren BIR URL 'dir.

|Yanıt üst bilgisi| Sonuç URL 'SI |
|:-----|:----|
|Operation-Location | `https://cognitiveservice/formrecognizer/v2.1-preview.2/prebuilt/invoice/analyzeResults/49a36324-fc4b-4387-aa06-090cfbf0064f` |

## <a name="the-get-analyze-invoice-result-operation"></a>Fatura sonuçlarını çözümlemeyi al işlemi

İkinci adım, [Fatura sonuçlarını al işlemini çağıracaktır](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-2/operations/5ed8c9acb78c40a2533aee83) . Bu işlem, faturayı çözümle işlemi tarafından oluşturulan sonuç KIMLIĞINI giriş olarak alır. Aşağıdaki olası değerlere sahip bir **durum** alanı IÇEREN bir JSON yanıtı döndürür. **Başarılı** değerle döndürülünceye kadar bu işlemi tekrarlayarak çağırın. Saniye başına istek (RPS) oranını aşmamak için 3 ile 5 saniye arasında bir Aralık kullanın.

|Alan| Tür | Olası değerler |
|:-----|:----:|:----|
|durum | string | notStarted: analiz işlemi başlatılmamış.<br /><br />çalışıyor: analiz işlemi devam ediyor.<br /><br />başarısız: çözümleme işlemi başarısız oldu.<br /><br />başarılı: çözümleme işlemi başarılı oldu.|

**Durum** alanı **başarılı** değerine sahip olduğunda JSON yanıtı, istenirse sonuçları, ayıklanan tabloları ve isteğe bağlı metin tanıma sonuçlarını içerir. Fatura anlama sonucu, her değerin ayıklanan metin, normalleştirilmiş değer, sınırlama kutusu, güvenirlik ve karşılık gelen Word öğelerini içerdiği adlandırılmış alan değerlerinin bir sözlüğü olarak düzenlenmiştir. Metin tanıma sonucu, metin, sınırlayıcı kutusu ve güvenle bilgi içeren bir satır ve sözcük hiyerarşisi olarak düzenlenir.

### <a name="sample-json-output"></a>Örnek JSON çıkışı

Fatura sonuçlarını al işleminin yanıtı, ayıklanan tüm bilgiler ile faturaya ait yapısal bir temsil olacaktır. [Örnek bir fatura dosyası](./media/sample-invoice.jpg) ve yapılandırılmış çıkış [örneği fatura çıktısı](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/curl/form-recognizer/sample-invoice-output.json)için buraya bakın.

JSON çıktısı 3 bölümden oluşur: 
* `"readResults"` düğüm, tüm tanınan metin ve seçim işaretlerini içerir. Metin sayfaya, sonra satıra, sonra da tek sözcüklere göre düzenlenir. 
* `"pageResults"` düğüm, sınırlayıcı kutuları, güvenle ayıklanan tabloları ve hücreleri ve "readResults" içindeki satır ve sözcüklerin başvurusunu içerir.
* `"documentResults"` düğüm, modelin bulduğu faturaya özgü değerleri içerir. Fatura KIMLIĞI, sevk yeri, fatura, müşteri, toplam ve çok daha fazlası gibi faturadaki tüm alanları burada bulabilirsiniz.

## <a name="example-output"></a>Örnek çıkış

Fatura hizmeti metin, tablolar ve 26 fatura alanlarını ayıklar. JSON çıkış yanıtında bir faturadan ayıklanan alanlar aşağıda verilmiştir (aşağıdaki çıktı bu [örnek faturayı](./media/sample-invoice.jpg)kullanır)  

|Ad| Tür | Description | Metin | Değer (standartlaştırılmış çıkış) |
|:-----|:----|:----|:----| :----|
| CustomerName | string | Faturalandırılmakta olan müşteri | Microsoft Corp |  |
| CustomerId | string | Müşterinin başvuru KIMLIĞI | CıD-12345 |  |
| PurchaseOrder | string | Bir satınalma siparişi başvuru numarası | PO-3333 | |  |
| InvoiceId | string | Bu belirli faturaya ait KIMLIK (genellikle "fatura numarası") | INV-100 | |  |
| InvoiceDate | date | Faturanın verildiği tarih | 11/15/2019 | 
| DueDate | date | Bu faturaya ait tarih ödemesi vadesi | 12/15/2019 | 2019-12-15 | 2019-11-15 |
| SatıcıAdı | string | Bu faturayı oluşturan satıcı | CONTOSO LTD. | |
| VendorAddress | string | Satıcının posta adresi | 123 456th St New York, NY, 10001 | |
| VendorAddressRecipient | string | VendorAddress ile ilişkili ad | Contoso yönetim merkezleri | |
| CustomerAddress | string | Müşterinin posta adresi | 123 diğer St, Redmond WA, 98052 | |
| CustomerAddressRecipient | string | CustomerAddress ile ilişkili ad | Microsoft Corp | |
| BillingAddress | string | Müşterinin açık fatura adresi | 123 Bill St, Redmond WA, 98052 | |
| BillingAddressRecipient | string | BillingAddress ile ilişkili ad | Microsoft Hizmetleri | |
| ShippingAddress | string | Müşterinin açık teslimat adresi | 123 geme, Redmond WA, 98052 | |
| ShippingAddressRecipient | string | ShippingAddress ile ilişkili ad | Microsoft teslimi | |
| Dikçe | sayı | Bu faturada tanımlanan alt toplam alanı | $100,00 | 100 | 
| Toplam vergi | sayı | Bu faturada tanımlanan toplam vergi alanı | $10,00 | 10 |
| Invoicetotal | sayı | Bu faturayla ilişkilendirilen toplam yeni ücret | $110,00 | 110 |
| AmountDue |  sayı | Satıcıdan kaynaklanan toplam tutar | $610,00 | 610 |
| Hizmet adresi | string | Müşterinin açık hizmet adresi veya özellik adresi | 123 Service St, Redmond WA, 98052 | |
| ServiceAddressRecipient | string | ServiceAddress ile ilişkili ad | Microsoft Hizmetleri | |
| Havaletanceaddress | string | Müşteri için açık havale veya ödeme adresi | 123 New York, NY, 10001 havale |  |
| Havaletanceaddressrecipient | string | Havaletanceaddress ile ilişkili ad | Contoso faturalandırma |  |
| ServiceStartDate | date | Hizmet döneminin ilk tarihi (örneğin, bir yardımcı program fatura hizmeti dönemi) | 14.10.2019 | 2019-10-14 |
| ServiceEndDate | date | Hizmet döneminin bitiş tarihi (örneğin, bir yardımcı program fatura hizmeti dönemi) | 11/14/2019 | 2019-11-14 |
| Previousunpaıdbakiye | sayı | Açık önceden Ödenmemiş bakiye | $500,00 | 500 |


## <a name="next-steps"></a>Sonraki adımlar

- Kendi faturanız ve örneklerinizi [form tanıyıcı örnek Kullanıcı arabiriminde](https://fott-preview.azurewebsites.net/)deneyin.
- Form tanıyıcı ile seçtiğiniz geliştirme dilinde bir fatura işleme uygulaması yazmaya başlamak için [form tanıyıcı hızlı](quickstarts/client-library.md) başlangıcını doldurun.

## <a name="see-also"></a>Ayrıca bkz.

* [Form Tanıma nedir?](./overview.md)
* [REST API başvuru belgeleri](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-2/operations/5ed8c9843c2794cbb1a96291)
