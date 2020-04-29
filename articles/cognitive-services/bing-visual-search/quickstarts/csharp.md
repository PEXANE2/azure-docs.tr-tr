---
title: 'Hızlı başlangıç: REST API ve C# kullanarak görüntü öngörülerini alın Bing Görsel Arama'
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama API'si bir görüntüyü karşıya yüklemeyi ve ilgili öngörüleri nasıl alabileceğinizi öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 12/17/2019
ms.author: scottwhi
ms.openlocfilehash: 07ecac46ab13058d308c17c5747701ee5ed577fc
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75446679"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-c"></a>Hızlı başlangıç: Bing Görsel Arama REST API ve C kullanarak görüntü öngörülerini alın #

Bu hızlı başlangıçta bir görüntünün Bing Görsel Arama API'si nasıl yükleneceği ve döndürdüğü öngörülerin nasıl görüntüleneceği gösterilmiştir.

## <a name="prerequisites"></a>Ön koşullar

* Herhangi bir [Visual Studio 2019](https://www.visualstudio.com/downloads/)sürümü.
* NuGet paketi olarak kullanılabilen [JSON.NET Framework](https://www.newtonsoft.com/json).
* Linux/MacOS kullanıyorsanız, [mono](https://www.mono-project.com/)kullanarak bu uygulamayı çalıştırabilirsiniz.

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. Visual Studio 'da BingSearchApisQuickStart adlı yeni bir konsol çözümü oluşturun. Aşağıdaki ad alanlarını ana kod dosyasına ekleyin:

    ```csharp
    using System;
    using System.Text;
    using System.Net;
    using System.IO;
    using System.Collections.Generic;
    ```

2. Karşıya yüklemek istediğiniz görüntünün abonelik anahtarınız, uç noktanız ve yolu için değişkenler ekleyin. `uriBase`Aşağıdaki genel uç nokta veya kaynağınız için Azure portal görüntülenmiş [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktası olabilir:

    ```csharp
        const string accessKey = "<my_subscription_key>";
        const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
        static string imagePath = @"<path_to_image>";
    ```

3. Görüntünüzün yolunu almak için `GetImageFileName()` adlı bir yöntem oluşturun:
    
    ```csharp
    static string GetImageFileName(string path)
            {
                return new FileInfo(path).Name;
            }
    ```

4. Görüntünün ikili verilerini almak için bir yöntem oluşturun:

    ```csharp
    static byte[] GetImageBinary(string path)
    {
        return File.ReadAllBytes(path);
    }
    ```

## <a name="build-the-form-data"></a>Form verilerini oluşturma

Yerel bir görüntüyü karşıya yüklemek için önce API 'ye göndermek üzere form verileri oluşturun. Form verileri `Content-Disposition` üst bilgiyi içermelidir, `name` parametresinin parametresi "image" olarak ayarlanmalıdır ve `filename` parametresi herhangi bir dizeye ayarlanabilir. Formun içeriği görüntünün ikili verilerini içerir. Karşıya yükleyebileceğiniz en büyük görüntü boyutu 1 MB 'tır.

    ```
    --boundary_1234-abcd
    Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
    
    ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...
    
    --boundary_1234-abcd--
    ```

1. GÖNDERI formu verilerini biçimlendirmek için sınır dizeleri ekleyin. Sınır dizeleri veriler için başlangıç, bitiş ve yeni satır karakterlerini belirlenir:

    ```csharp
    // Boundary strings for form data in body of POST.
    const string CRLF = "\r\n";
    static string BoundaryTemplate = "batch_{0}";
    static string StartBoundaryTemplate = "--{0}";
    static string EndBoundaryTemplate = "--{0}--";
    ```

2. Form verilerine parametreler eklemek için aşağıdaki değişkenleri kullanın:

    ```csharp
    const string CONTENT_TYPE_HEADER_PARAMS = "multipart/form-data; boundary={0}";
    const string POST_BODY_DISPOSITION_HEADER = "Content-Disposition: form-data; name=\"image\"; filename=\"{0}\"" + CRLF +CRLF;
    ```

3. Sınır dizelerini ve görüntü `BuildFormDataStart()` yolunu kullanarak form verilerinin başlangıcını oluşturmak için adlı bir işlev oluşturun:
    
    ```csharp
        static string BuildFormDataStart(string boundary, string filename)
        {
            var startBoundary = string.Format(StartBoundaryTemplate, boundary);

            var requestBody = startBoundary + CRLF;
            requestBody += string.Format(POST_BODY_DISPOSITION_HEADER, filename);

            return requestBody;
        }
    ```

4. Sınır dizelerini kullanarak form `BuildFormDataEnd()` verilerinin sonunu oluşturmak için adlı bir işlev oluşturun:
    
    ```csharp
        static string BuildFormDataEnd(string boundary)
        {
            return CRLF + CRLF + string.Format(EndBoundaryTemplate, boundary) + CRLF;
        }
    ```

## <a name="call-the-bing-visual-search-api"></a>Bing Görsel Arama API'si çağırın

1. Bing Görsel Arama uç noktasını çağırmak ve JSON yanıtını döndürmek için bir işlev oluşturun. İşlev, form verilerinin başlangıç ve bitişini, görüntü verilerini içeren bir bayt dizisi ve bir `contentType` değer alır.

2. URI, `WebRequest` ContentType değer ve üst bilgileri depolamak için bir kullanın.  

3. Formunuzu `request.GetRequestStream()` ve resim verilerinizi yazmak için kullanın, ardından yanıtı alın. İşleviniz aşağıdakine benzer olmalıdır:
        
    ```csharp
        static string BingImageSearch(string startFormData, string endFormData, byte[] image, string contentTypeValue)
        {
            WebRequest request = HttpWebRequest.Create(uriBase);
            request.ContentType = contentTypeValue;
            request.Headers["Ocp-Apim-Subscription-Key"] = accessKey;
            request.Method = "POST";

            // Writes the boundary and Content-Disposition header, then writes
            // the image binary, and finishes by writing the closing boundary.
            using (Stream requestStream = request.GetRequestStream())
            {
                StreamWriter writer = new StreamWriter(requestStream);
                writer.Write(startFormData);
                writer.Flush();
                requestStream.Write(image, 0, image.Length);
                writer.Write(endFormData);
                writer.Flush();
                writer.Close();
            }

            HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
            string json = new StreamReader(response.GetResponseStream()).ReadToEnd();

            return json;
        }
    ```

## <a name="create-the-main-method"></a>Main metodunu oluşturma

1. Uygulamanızın `Main` yönteminde, görüntünüzün dosya adını ve ikili verilerini alın:

    ```csharp
    var filename = GetImageFileName(imagePath);
    var imageBinary = GetImageBinary(imagePath);
    ```

2. Sınır biçimlendirmesini düzenleyerek GÖNDERI gövdesini ayarlayın. Ardından form `startFormData()` verilerini `endFormData` oluşturmak için ve öğesini çağırın:

    ```csharp
    // Set up POST body.
    var boundary = string.Format(BoundaryTemplate, Guid.NewGuid());
    var startFormData = BuildFormDataStart(boundary, filename);
    var endFormData = BuildFormDataEnd(boundary);
    ```

3. `ContentType` Değeri biçimlendirmeye `CONTENT_TYPE_HEADER_PARAMS` ve form veri sınırına göre oluşturun:

    ```csharp
    var contentTypeHdrValue = string.Format(CONTENT_TYPE_HEADER_PARAMS, boundary);
    ```

4. Yanıtı çağırarak `BingImageSearch()` API yanıtını alın ve yazdırın:

    ```csharp
    var json = BingImageSearch(startFormData, endFormData, imageBinary, contentTypeHdrValue);
    Console.WriteLine(json);
    Console.WriteLine("enter any key to continue");
    Console.readKey();
    ```

## <a name="using-httpclient"></a>HttpClient kullanma

Kullanıyorsanız `HttpClient`, form verilerini oluşturmak için `MultipartFormDataContent` sınıfını kullanabilirsiniz. Önceki örnekteki karşılık gelen yöntemleri değiştirmek için aşağıdaki kod bölümlerini kullanmanız yeterlidir.

`Main` Yöntemi şu kodla değiştirin:

```csharp
        static void Main()
        {
            try
            {
                Console.OutputEncoding = System.Text.Encoding.UTF8;

                if (accessKey.Length == 32)
                {
                    if (IsImagePathSet(imagePath))
                    {
                        var filename = GetImageFileName(imagePath);
                        Console.WriteLine("Getting image insights for image: " + filename);
                        var imageBinary = GetImageBinary(imagePath);

                        var boundary = string.Format(BoundaryTemplate, Guid.NewGuid());
                        var json = BingImageSearch(imageBinary, boundary, uriBase, accessKey);

                        Console.WriteLine("\nJSON Response:\n");
                        Console.WriteLine(JsonPrettyPrint(json));
                    }
                }
                else
                {
                    Console.WriteLine("Invalid Bing Visual Search API subscription key!");
                    Console.WriteLine("Please paste yours into the source code.");
                }

                Console.Write("\nPress Enter to exit ");
                Console.ReadLine();
            }
            catch (Exception e)
            {
                Console.WriteLine(e.Message);
            }
        }
```

`BingImageSearch` Yöntemi şu kodla değiştirin:

```csharp
        /// <summary>
        /// Calls the Bing visual search endpoint and returns the JSON response.
        /// </summary>
        static string BingImageSearch(byte[] image, string boundary, string uri, string subscriptionKey)
        {
            var requestMessage = new HttpRequestMessage(HttpMethod.Post, uri);
            requestMessage.Headers.Add("Ocp-Apim-Subscription-Key", accessKey);

            var content = new MultipartFormDataContent(boundary);
            content.Add(new ByteArrayContent(image), "image", "myimage");
            requestMessage.Content = content;

            var httpClient = new HttpClient();

            Task<HttpResponseMessage> httpRequest = httpClient.SendAsync(requestMessage, HttpCompletionOption.ResponseContentRead, CancellationToken.None);
            HttpResponseMessage httpResponse = httpRequest.Result;
            HttpStatusCode statusCode = httpResponse.StatusCode;
            HttpContent responseContent = httpResponse.Content;

            string json = null;

            if (responseContent != null)
            {
                Task<String> stringContentsTask = responseContent.ReadAsStringAsync();
                json = stringContentsTask.Result;
            }

            return json;
        }
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Görsel Arama tek sayfalı Web uygulaması oluşturma](../tutorial-bing-visual-search-single-page-app.md)
