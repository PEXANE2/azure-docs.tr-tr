---
title: 'Quickstart: Düğüm.js için REST APIs ile QnA Maker'
description: Bu hızlı başlangıç, Node.js için QnA Maker REST API'leri ile nasıl başlayacağımı gösterir. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin.  Soru-Cevap Oluşturma, SSS belgeleri, URL'ler ve ürün kılavuzları gibi yarı yapılandırılmış içeriklerinizden bir soru cevap hizmeti oluşturmanızı sağlar.
ms.date: 02/08/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27
ms.topic: conceptual
ms.openlocfilehash: ecc3fb144fb4b4e27182567925199f841b1c4357
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78851679"
---
# <a name="quickstart-qna-maker-rest-apis-for-nodejs"></a>Quickstart: Düğüm.js için QnA Maker REST APIs

Node.js için QnA Maker REST API'leri ile başlayın. Temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin.  Soru-Cevap Oluşturma, SSS belgeleri, URL'ler ve ürün kılavuzları gibi yarı yapılandırılmış içeriklerinizden bir soru cevap hizmeti oluşturmanızı sağlar.

Düğüm.js için QnA Maker REST API'lerini kullanın:

* Bilgi bankası oluşturma
* Bilgi tabanını değiştirme
* Bilgi bankası yayımlama
* Bilgi bankasını silme
* Bilgi tabanı indirin
* İşlemin durumunu alma

[Referans belgeleme](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase) | [Düğüm.js Örnekleri](https://github.com/Azure-Samples/cognitive-services-qnamaker-nodejs/tree/master/documentation-samples/quickstarts/rest-api)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Düğüm.js](https://nodejs.org)geçerli sürümü .
* [Bir QnA Maker hizmetiniz](../How-To/set-up-qnamaker-service-azure.md)olmalı. Anahtar ve bitiş noktanızı (kaynak adını içerir) almak için Azure portalındaki kaynağınız için **Hızlı Başlat'ı** seçin.

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-qna-maker-azure-resource"></a>QnA Maker Azure kaynağı oluşturma

Azure Bilişsel Hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure portalını](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI'yi](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak QnA Maker için bir kaynak oluşturun.

Kaynağınızdan bir anahtar aldıktan sonra, kaynak için ortam `QNAMAKER_RESOURCE_KEY` `QNAMAKER_AUTHORING_ENDPOINT` [değişkenleri oluşturun,](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) adlı ve . Azure portalında Kaynağın **Hızlı Başlangıç** sayfasında bulunan anahtar ve uç nokta değerlerini kullanın.

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu pencereye gidin.

```console
mkdir myapp && cd myapp
```

Düğüm `npm init -y` `package.json` dosyası oluşturmak için komutu çalıştırın.

```console
npm init -y
```

`reqeuestretry` NPM paketlerini `request` ekleyin:

```console
npm install requestretry request --save
```

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Node.js için QnA Maker REST API'leri ile aşağıdakileri nasıl yapacağınızı gösterir:

* [Bilgi bankası oluşturma](#create-a-knowledge-base)
* [Bilgi tabanını değiştirme](#replace-a-knowledge-base)
* [Bilgi bankası yayımlama](#publish-a-knowledge-base)
* [Bilgi bankasını silme](#delete-a-knowledge-base)
* [Bilgi tabanı indirin](#download-the-knowledge-base)
* [İşlemin durumunu alma](#get-status-of-an-operation)

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

Adlandırılmış `rest-apis.js` bir dosya oluşturun ve _http_ isteklerini yapmak için aşağıdaki ifadeleri ekleyin.

```javascript
const request = require("requestretry");
```

## <a name="add-azure-resource-information"></a>Azure kaynak bilgileri ekleme

Kaynağınızın Azure bitiş noktası ve anahtarı için değişkenler oluşturun. Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için düzenleyiciyi, IDE'yi veya bu uygulamayı çalıştıran kabuğu kapatmanız ve yeniden açmanız gerekir.

Aşağıdaki ortam değerlerini ayarlayın:

* `QNAMAKER_RESOURCE_KEY`- **Anahtar** 32 karakterli bir dizedir ve Azure portalında, QnA Maker kaynağında, **Hızlı başlangıç** sayfasında mevcuttur. Bu, tahmin bitiş noktası anahtarıyla aynı değildir.
* `QNAMAKER_AUTHORING_ENDPOINT`- Sizin yazma bitiş noktası, `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com`biçiminde , **kaynak adınızı**içerir. Bu, tahmin bitiş noktasını sorgulamak için kullanılan URL ile aynı url değildir.

[!code-javascript[Add Azure resources from environment variables](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/rest-api/rest-api.js?name=authorization)]

## <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma

Bir bilgi bankası, JSON nesnesinden oluşturulan soru ve yanıt çiftlerini depolar:

* **Editoryal içerik**.
* **Dosyalar** - herhangi bir izin gerektirmeyen yerel dosyalar.
* **URL'ler** - herkese açık URL'ler.

Bir [bilgi tabanı oluşturmak için REST API'yi](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create)kullanın.

[!code-javascript[Add Azure resources from environment variables](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/rest-api/rest-api.js?name=createKb)]

## <a name="replace-a-knowledge-base"></a>Bilgi tabanını değiştirme

Bir [bilgi tabanını değiştirmek için REST API'sini](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/replace)kullanın.

[!code-javascript[Add Azure resources from environment variables](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/rest-api/rest-api.js?name=replaceKb)]

## <a name="publish-a-knowledge-base"></a>Bilgi bankası yayımlama

Bilgi tabanını yayınlayın. Bu işlem, bilgi tabanını bir HTTP sorgusu tahmin bitiş noktasından kullanılabilir hale getirir.

Bir [bilgi tabanı yayınlamak için REST API'yi](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish)kullanın.


[!code-javascript[Add Azure resources from environment variables](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/rest-api/rest-api.js?name=publish)]

## <a name="download-the-knowledge-base"></a>Bilgi tabanını indirin

Bir [bilgi tabanı indirmek için REST API'yi](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/download)kullanın.

[!code-javascript[Add Azure resources from environment variables](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/rest-api/rest-api.js?name=download)]

## <a name="delete-a-knowledge-base"></a>Bilgi bankasını silme

Bilgi tabanını kullanarak bitirdiğinizde, silin.

Bir [bilgi tabanını silmek için REST API'sini](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/delete)kullanın.

[!code-javascript[Add Azure resources from environment variables](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/rest-api/rest-api.js?name=deleteKb)]

## <a name="get-status-of-an-operation"></a>İşlemin durumunu alma

Oluşturma işlemi gibi uzun süren işlemler, ayrı bir REST API çağrısıyla denetlenmesi gereken bir işlem kimliği döndürür. Bu işlev oluşturma yanıtının gövdesini alır. Önemli `operationState`anahtar, yoklama devam etmek gerekip gerekip gerekmeden belirler , olduğunu.

Bir [bilgi tabanındaki işlemleri izlemek için REST API'sini](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/operations/getdetails)kullanın.


[!code-javascript[Add Azure resources from environment variables](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/rest-api/rest-api.js?name=operationDetails)]


## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı uygulama `node rest-apis.js` dizininizin komutuyla çalıştırın.

```console
node rest-apis.js
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>[Öğretici: Bir KB oluşturma ve yanıtlama](../tutorials/create-publish-query-in-portal.md)

* [QnA Maker API nedir?](../Overview/overview.md)
* [Bilgi bankası düzenleme](../how-to/edit-knowledge-base.md)
* [Kullanım analitiği alın](../how-to/get-analytics-knowledge-base.md)
* Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-qnamaker-nodejs/blob/master/documentation-samples/quickstarts/rest-api/rest-api.js)bulunabilir.