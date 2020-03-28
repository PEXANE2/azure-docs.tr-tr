---
title: 'Quickstart: REST API ve C# kullanarak görüntü öngörüleri alın - Bing Görsel Arama'
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama API'sine nasıl görüntü yükleyip bu konuda bilgi edineceklerini öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 12/17/2019
ms.author: scottwhi
ms.openlocfilehash: 07ecac46ab13058d308c17c5747701ee5ed577fc
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75446679"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-c"></a>Hızlı başlatma: Bing Visual Search REST API ve C'yi kullanarak görüntü öngörüleri alın #

Bu hızlı başlangıç, bir resmin Bing Görsel Arama API'sine nasıl yüklenerek nasıl yüklenilebildiğini ve döndürdüğü kavraıları gÜ

## <a name="prerequisites"></a>Ön koşullar

* [Visual Studio 2019](https://www.visualstudio.com/downloads/)herhangi bir baskı .
* [Json.NET çerçevesi,](https://www.newtonsoft.com/json)NuGet paketi olarak mevcuttur.
* Linux/MacOS kullanıyorsanız, bu uygulamayı [Mono](https://www.mono-project.com/)kullanarak çalıştırabilirsiniz.

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. Visual Studio'da BingSearchApisQuickStart adında yeni bir konsol çözümü oluşturun. Ana kod dosyasına aşağıdaki ad alanlarını ekleyin:

    ```csharp
    using System;
    using System.Text;
    using System.Net;
    using System.IO;
    using System.Collections.Generic;
    ```

2. Yüklemek istediğiniz resme abonelik anahtarınız, bitiş noktanız ve yolunuz için değişkenler ekleyin. `uriBase`aşağıdaki genel bitiş noktası veya kaynağınız için Azure portalında görüntülenen [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) bitiş noktası olabilir:

    ```csharp
        const string accessKey = "<my_subscription_key>";
        const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
        static string imagePath = @"<path_to_image>";
    ```

3. Görüntünüz için `GetImageFileName()` yol almak için adlı bir yöntem oluşturun:
    
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

Yerel bir resim yüklemek için, önce API'ye göndermek üzere form verilerini oluşturursunuz. Form verileri üstbilgi `Content-Disposition` içermelidir, `name` parametresi "görüntü" olarak ayarlanmalıdır ve `filename` parametre herhangi bir dize olarak ayarlanabilir. Formun içeriği görüntünün ikili verilerini içerir. Yükleyebileceğiniz maksimum görüntü boyutu 1 MB'dır.

    ```
    --boundary_1234-abcd
    Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
    
    ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...
    
    --boundary_1234-abcd--
    ```

1. POST form verilerini biçimlendirmek için sınır dizeleri ekleyin. Sınır dizeleri, verilerin başlangıç, bitiş ve yeni çizgi karakterlerini belirler:

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

3. Sınır dizelerini ve görüntü yolunu kullanarak form verilerinin başlangıcını oluşturmak için adlı `BuildFormDataStart()` bir işlev oluşturun:
    
    ```csharp
        static string BuildFormDataStart(string boundary, string filename)
        {
            var startBoundary = string.Format(StartBoundaryTemplate, boundary);

            var requestBody = startBoundary + CRLF;
            requestBody += string.Format(POST_BODY_DISPOSITION_HEADER, filename);

            return requestBody;
        }
    ```

4. Sınır dizelerini kullanarak form verilerinin sonunu oluşturmak için adlandırılmış `BuildFormDataEnd()` bir işlev oluşturun:
    
    ```csharp
        static string BuildFormDataEnd(string boundary)
        {
            return CRLF + CRLF + string.Format(EndBoundaryTemplate, boundary) + CRLF;
        }
    ```

## <a name="call-the-bing-visual-search-api"></a>Bing Görsel Arama API'sini arayın

1. Bing Görsel Arama bitiş noktasını aramak ve JSON yanıtını döndürmek için bir işlev oluşturun. İşlev, form verilerinin, görüntü verilerini içeren bir bayt dizisini `contentType` ve bir değeri alır.

2. Uri, `WebRequest` contentType değeri nizi ve üstbilgilerinizi depolamak için a kullanın.  

3. Form `request.GetRequestStream()` unuzu ve resim verilerinizi yazmak için kullanın, ardından yanıt alın. Işleviniz aşağıdakine benzer olmalıdır:
        
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

## <a name="create-the-main-method"></a>Ana yöntemi oluşturma

1. Uygulama `Main` yönteminizde, görüntünüzün dosya adını ve ikili verilerini alın:

    ```csharp
    var filename = GetImageFileName(imagePath);
    var imageBinary = GetImageBinary(imagePath);
    ```

2. Bunun için sınır biçimlendirerek POST gövdesini ayarlayın. Daha `startFormData()` sonra `endFormData` arama ve form verileri oluşturmak için:

    ```csharp
    // Set up POST body.
    var boundary = string.Format(BoundaryTemplate, Guid.NewGuid());
    var startFormData = BuildFormDataStart(boundary, filename);
    var endFormData = BuildFormDataEnd(boundary);
    ```

3. Biçimlendirerek `ContentType` `CONTENT_TYPE_HEADER_PARAMS` değeri ve form veri sınırını oluşturun:

    ```csharp
    var contentTypeHdrValue = string.Format(CONTENT_TYPE_HEADER_PARAMS, boundary);
    ```

4. Arayarak `BingImageSearch()` API yanıtını alın ve yanıtı yazdırın:

    ```csharp
    var json = BingImageSearch(startFormData, endFormData, imageBinary, contentTypeHdrValue);
    Console.WriteLine(json);
    Console.WriteLine("enter any key to continue");
    Console.readKey();
    ```

## <a name="using-httpclient"></a>HttpClient kullanma

Kullanıyorsanız, `HttpClient`form verilerini `MultipartFormDataContent` oluşturmak için sınıfı kullanabilirsiniz. Önceki örnekte karşılık gelen yöntemleri değiştirmek için aşağıdaki kod bölümlerini kullanmanız gereken bir yöntemdir.

Yöntemi `Main` bu kodla değiştirin:

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

Yöntemi `BingImageSearch` bu kodla değiştirin:

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
> [Görsel Arama tek sayfalık web uygulaması oluşturma](../tutorial-bing-visual-search-single-page-app.md)
