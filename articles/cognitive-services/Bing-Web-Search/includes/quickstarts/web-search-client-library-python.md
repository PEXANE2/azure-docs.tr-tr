---
title: Bing Web Arama Python istemci kitaplığı hızlı başlat
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/05/2020
ms.author: aahi
ms.openlocfilehash: 019abc79091527b105238ef83bfca1913fae600b
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78925946"
---
Bing Web Arama istemci kitaplığı, Bing Web Arama'yı Python uygulamanıza entegre etmeyi kolaylaştırır. Bu hızlı başlangıçta istek göndermeyi, JSON yanıtı almayı, sonuçları filtrelemeyi ve ayrıştırmayı öğreneceksiniz.

Kodu hemen görmek istiyor musunuz? [Python için Bing Arama istemci kitaplıkları için](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/tree/master/samples/search) örnekler GitHub'da mevcuttur.


## <a name="prerequisites"></a>Ön koşullar
Bing Web Araması SDK'sı Python 2.7, 3.3, 3.4, 3.5 ve 3.6 ile uyumludur. Bu hızlı başlangıç için sanal ortam kullanmanızı öneririz.

* Python 2.7, 3.3, 3.4, 3.5 veya 3.6
* Python 2.7 için [virtualenv](https://docs.python.org/3/tutorial/venv.html)
* Python 3.x için [venv](https://pypi.python.org/pypi/virtualenv)

[!INCLUDE [bing-web-search-quickstart-signup](~/includes/bing-web-search-quickstart-signup.md)]

## <a name="create-and-configure-your-virtual-environment"></a>Sanal ortamınızı oluşturma ve yapılandırma

Sanal ortamınızı ayarlama ve yapılandırma talimatları Python 2.x ve Python 3.x sürümleri için değişiklik gösterecektir. Sanal ortamınızı oluşturmak ve başlatmak için aşağıdaki adımları izleyin.

### <a name="python-2x"></a>Python 2.x

Python 2.7 için `virtualenv` ile sanal ortam oluşturun:

```console
virtualenv mytestenv
```

Ortamınızı etkinleştirme:

```console
cd mytestenv
source bin/activate
```

Bing Web Araması SDK'sı bağımlılıklarını yükleyin:

```console
python -m pip install azure-cognitiveservices-search-websearch
```

### <a name="python-3x"></a>Python 3.x

Python 3.x için `venv` ile sanal ortam oluşturun:

```console
python -m venv mytestenv
```

Ortamınızı etkinleştirme:

```console
mytestenv\Scripts\activate.bat
```

Bing Web Araması SDK'sı bağımlılıklarını yükleyin:

```console
cd mytestenv
python -m pip install azure-cognitiveservices-search-websearch
```

## <a name="create-a-client-and-print-your-first-results"></a>İstemci oluşturma ve ilk sonuçlarınızı yazdırma

Sanal ortamınızı ayarladığınıza ve bağımlılıkları yüklediğinizde göre bir istemci oluşturabilirsiniz. İstemci, Bing Web Araması API'sinden gelen istekleri ve yanıtları işler.

Yanıtta web sayfaları, görüntüler, haberler veya videolar varsa hepsinin ilk sonuçları yazdırılır.

1. Favori IDE ortamınızda veya düzenleyicide yeni bir Python projesi oluşturun.

1. Bu örnek kodu projenize kopyalayın. `endpoint`aşağıdaki genel bitiş noktası veya kaynağınız için Azure portalında görüntülenen [özel alt etki alanı](~/articles/cognitive-services/cognitive-services-custom-subdomains.md) bitiş noktası olabilir.:  

    ```python
    # Import required modules.
    from azure.cognitiveservices.search.websearch import WebSearchClient
    from azure.cognitiveservices.search.websearch.models import SafeSearch
    from msrest.authentication import CognitiveServicesCredentials

    # Replace with your subscription key.
    subscription_key = "YOUR_SUBSCRIPTION_KEY"

    # Instantiate the client and replace with your endpoint.
    client = WebSearchClient(endpoint="YOUR_ENDPOINT", CognitiveServicesCredentials(subscription_key))

    # Make a request. Replace Yosemite if you'd like.
    web_data = client.web.search(query="Yosemite")
    print("\r\nSearched for Query# \" Yosemite \"")

    '''
    Web pages
    If the search response contains web pages, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.web_pages, 'value'):

        print("\r\nWebpage Results#{}".format(len(web_data.web_pages.value)))

        first_web_page = web_data.web_pages.value[0]
        print("First web page name: {} ".format(first_web_page.name))
        print("First web page URL: {} ".format(first_web_page.url))

    else:
        print("Didn't find any web pages...")

    '''
    Images
    If the search response contains images, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.images, 'value'):

        print("\r\nImage Results#{}".format(len(web_data.images.value)))

        first_image = web_data.images.value[0]
        print("First Image name: {} ".format(first_image.name))
        print("First Image URL: {} ".format(first_image.url))

    else:
        print("Didn't find any images...")

    '''
    News
    If the search response contains news, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.news, 'value'):

        print("\r\nNews Results#{}".format(len(web_data.news.value)))

        first_news = web_data.news.value[0]
        print("First News name: {} ".format(first_news.name))
        print("First News URL: {} ".format(first_news.url))

    else:
        print("Didn't find any news...")

    '''
    If the search response contains videos, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.videos, 'value'):

        print("\r\nVideos Results#{}".format(len(web_data.videos.value)))

        first_video = web_data.videos.value[0]
        print("First Videos name: {} ".format(first_video.name))
        print("First Videos URL: {} ".format(first_video.url))

    else:
        print("Didn't find any videos...")
    ```

1. `SUBSCRIPTION_KEY` değerini geçerli bir abonelik anahtarıyla değiştirin.

1. Portaldaki uç nokta url'nizle değiştirin. `YOUR_ENDPOINT`

1. Programı çalıştırın. Örneğin: `python your_program.py`.

## <a name="define-functions-and-filter-results"></a>İşlevleri tanımlama ve sonuçları filtreleme

Bing Web Arama API'sine ilk aramanızı yaptığınıza göre, birkaç işleve bakalım. Aşağıdaki bölümlerde sorguları rafine etmek ve sonuçları filtreleme için SDK işlevselliği vurgulanır. Her işlev, önceki bölümde oluşturduğunuz Python programına eklenebilir.

### <a name="limit-the-number-of-results-returned-by-bing"></a>Bing tarafından döndürülen sonuç sayısını sınırlama

Bu örnek, `count` `offset` SDK [ `search` yöntemi](https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python)kullanılarak döndürülen sonuç sayısını sınırlamak için parametreleri ve parametreleri kullanır. İlk sonucun `name` ve `url` değerleri yazdırılır.

1. Python projenize şu kodu ekleyin:

   ```python
    # Declare the function.
    def web_results_with_count_and_offset(subscription_key):
        client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))

        try:
            '''
            Set the query, offset, and count using the SDK's search method. See:
            https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python.
            '''
            web_data = client.web.search(query="Best restaurants in Seattle", offset=10, count=20)
            print("\r\nSearching for \"Best restaurants in Seattle\"")

            if web_data.web_pages.value:
                '''
                If web pages are available, print the # of responses, and the first and second
                web pages returned.
                '''
                print("Webpage Results#{}".format(len(web_data.web_pages.value)))

                first_web_page = web_data.web_pages.value[0]
                print("First web page name: {} ".format(first_web_page.name))
                print("First web page URL: {} ".format(first_web_page.url))

            else:
                print("Didn't find any web pages...")

        except Exception as err:
            print("Encountered exception. {}".format(err))
    ```

1. Programı çalıştırın.

### <a name="filter-for-news-and-freshness"></a>Haberler ve güncellik filtresi

Bu örnek, `response_filter` `freshness` [ `search` SDK'nın yöntemini](/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations)kullanarak arama sonuçlarını filtrelemek için parametreleri ve parametreleri kullanır. Döndürülen arama sonuçları Bing'in son 24 saat içinde keşfettiği haberler ve sayfalarla sınırlıdır. İlk sonucun `name` ve `url` değerleri yazdırılır.

1. Python projenize şu kodu ekleyin:

    ```python
    # Declare the function.
    def web_search_with_response_filter(subscription_key):
        client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))
        try:
            '''
            Set the query, response_filter, and freshness using the SDK's search method. See:
            https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python.
            '''
            web_data = client.web.search(query="xbox",
                response_filter=["News"],
                freshness="Day")
            print("\r\nSearching for \"xbox\" with the response filter set to \"News\" and freshness filter set to \"Day\".")

            '''
            If news articles are available, print the # of responses, and the first and second
            articles returned.
            '''
            if web_data.news.value:

                print("# of news results: {}".format(len(web_data.news.value)))

                first_web_page = web_data.news.value[0]
                print("First article name: {} ".format(first_web_page.name))
                print("First article URL: {} ".format(first_web_page.url))

                print("")

                second_web_page = web_data.news.value[1]
                print("\nSecond article name: {} ".format(second_web_page.name))
                print("Second article URL: {} ".format(second_web_page.url))

            else:
                print("Didn't find any news articles...")

        except Exception as err:
            print("Encountered exception. {}".format(err))

    # Call the function.
    web_search_with_response_filter(subscription_key)
    ```

1. Programı çalıştırın.

### <a name="use-safe-search-answer-count-and-the-promote-filter"></a>Güvenli arama, yanıt sayısı ve yükseltme filtresini kullanma

Bu örnek, `answer_count` `promote`SDK `safe_search` [ `search` yöntemini](https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python)kullanarak arama sonuçlarını filtrelemek için , ve parametreleri kullanır. İlk sonucun `name` ve `url` değerleri görüntülenir.

1. Python projenize şu kodu ekleyin:

    ```python
    # Declare the function.
    def web_search_with_answer_count_promote_and_safe_search(subscription_key):

        client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))

        try:
            '''
            Set the query, answer_count, promote, and safe_search parameters using the SDK's search method. See:
            https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python.
            '''
            web_data = client.web.search(
                query="Niagara Falls",
                answer_count=2,
                promote=["videos"],
                safe_search=SafeSearch.strict  # or directly "Strict"
            )
            print("\r\nSearching for \"Niagara Falls\"")

            '''
            If results are available, print the # of responses, and the first result returned.
            '''
            if web_data.web_pages.value:

                print("Webpage Results#{}".format(len(web_data.web_pages.value)))

                first_web_page = web_data.web_pages.value[0]
                print("First web page name: {} ".format(first_web_page.name))
                print("First web page URL: {} ".format(first_web_page.url))

            else:
                print("Didn't see any Web data..")

        except Exception as err:
            print("Encountered exception. {}".format(err))
    ```

1. Programı çalıştırın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu projeyi tamamladıktan sonra abonelik anahtarınızı program kodundan kaldırmayı ve sanal ortamınızı devre dışı bırakmayı unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilişsel Hizmetler Python SDK'sı örnekleri](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Python SDK'sı başvurusu](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/websearch)
