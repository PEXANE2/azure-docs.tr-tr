---
title: Visual Studio Code ile Python 'da Azure Işlevleri oluşturun ve dağıtın
description: Python 'da sunucusuz işlevler oluşturmak ve bunları Azure 'a dağıtmak için Azure Işlevleri Visual Studio Code uzantısını kullanma.
services: functions
author: ggailey777
manager: gwallace
ms.service: azure-functions
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: glenga
ms.openlocfilehash: 590757f78086be894cdc2384bb4a4df380e91c27
ms.sourcegitcommit: cd70273f0845cd39b435bd5978ca0df4ac4d7b2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71098595"
---
# <a name="deploy-python-to-azure-functions-with-visual-studio-code"></a>Visual Studio Code ile Azure Işlevlerine Python dağıtma

Bu öğreticide, Python ile sunucusuz bir HTTP uç noktası oluşturmak ve ayrıca depolama alanına bir bağlantı (ya da "bağlama") eklemek için Visual Studio Code ve Azure Işlevleri uzantısını kullanırsınız. Azure Işlevleri, bir sanal makine sağlamaya veya bir Web uygulamasını yayımlamaya gerek kalmadan kodunuzu sunucusuz bir ortamda çalıştırır. Visual Studio Code için Azure Işlevleri uzantısı, birçok yapılandırma sorunlarını otomatik olarak işleyerek Işlevleri kullanma sürecini büyük ölçüde basitleştirir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure Işlevleri uzantısını yükler
> * HTTP ile tetiklenen bir işlev oluşturma
> * Yerel olarak hata ayıkla
> * Uygulama ayarlarını eşitler
> * Akış günlüklerini görüntüle
> * Azure depolama 'ya bağlanma

Bu öğreticideki adımların herhangi biriyle ilgili sorunlarla karşılaşırsanız, ayrıntıları öğrenmek istiyoruz. Ayrıntılı geri bildirim göndermek için her bölümün sonundaki **bir sorunla karşılaştım** düğmesini kullanın.

## <a name="prerequisites"></a>Önkoşullar

- Bir [Azure aboneliği](#azure-subscription).
- [Azure işlevleri uzantısıyla Visual Studio Code](#visual-studio-code-python-and-the-azure-functions-extension) .
- [Azure Functions Core Tools](#azure-functions-core-tools).

### <a name="azure-subscription"></a>Azure aboneliği

Azure aboneliğiniz yoksa, herhangi bir hizmet birleşimini denemek için Azure kredileri $200 olan ücretsiz 30 günlük bir hesap için [hemen kaydolun](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension) .

### <a name="visual-studio-code-python-and-the-azure-functions-extension"></a>Visual Studio Code, Python ve Azure Işlevleri uzantısı

Aşağıdaki yazılımı yükler:

- Azure Işlevleri için gereken Python 3.6. x. [Python 3.6.9](https://www.python.org/downloads/release/python-369/) , en son 3.6. x sürümüdür.
- [Visual Studio Code](https://code.visualstudio.com/).
- [Visual Studio Code Python öğreticisi-Önkoşullar](https://code.visualstudio.com/docs/python/python-tutorial)bölümünde açıklandığı şekilde [Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python) .
- [Azure işlevleri uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions). Genel bilgiler için [vscode-azurefunctions GitHub deposunu](https://github.com/Microsoft/vscode-azurefunctions)ziyaret edin.

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

[Azure Functions Core Tools çalışma](functions-run-local.md#v2)sırasında işletim sisteminiz için yönergeleri izleyin. Araçların kendisi .NET Core 'da yazılmıştır ve temel araçlar paketi, Python kodu için de .NET Core ve Node. js ' yi yüklemeniz gereken NPM olan Node. js Paket Yöneticisi, NPM kullanılarak en iyi şekilde yüklenir. Ancak, yukarıda bahsedilen belgelerde açıklandığı gibi "uzantı paketleri" ni kullanarak .NET Core gereksinimini atlayabilirsiniz. Her ne olursa olsun, bu bileşenleri yalnızca bir kez yüklemeniz gerekir, bu, Visual Studio Code otomatik olarak herhangi bir güncelleştirme yüklemenizi ister.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

İşlevler uzantısı yüklendikten sonra **Azure 'a giderek Azure hesabınızda oturum açın: İşlevler** Gezgini, **Azure 'da oturum aç**' ı seçin ve yönergeleri izleyin.

![Visual Studio Code aracılığıyla Azure 'da oturum açın](media/tutorial-vs-code-serverless-python/azure-sign-in.png)

Oturum açtıktan sonra, Azure aboneliğinizin e-posta hesabının durum çubuğunda göründüğünü doğrulayın:

![Azure hesabını gösteren Visual Studio Code durum çubuğu](media/tutorial-vs-code-serverless-python/azure-account-status-bar.png)

Aboneliğinize atadığınız ad da **Azure 'da görünür: İşlevler** Gezgini (aşağıdaki görüntüde "birincil"):

![Abonelikleri gösteren Azure App Service Gezgini Visual Studio Code](media/tutorial-vs-code-serverless-python/azure-subscription-view.png)

> [!NOTE]
> **"Adı [ABONELIK kimliği] olan abonelik bulunamıyor"** hatasıyla karşılaşırsanız, bunun nedeni bir proxy 'nin arkasında olmanız ve Azure API 'sine ulaşamamanız olabilir. Terminalinizdeki `HTTPS_PROXY` proxy bilgilerinizi kullanarak değişkenleri yapılandırın `HTTP_PROXY` ve ortam değişkenlerini yapın:
>
> ```bash
> # macOS/Linux
> export HTTPS_PROXY=https://username:password@proxy:8080
> export HTTP_PROXY=http://username:password@proxy:8080
> ```
>
> ```ps
> # Windows
> set HTTPS_PROXY=https://username:password@proxy:8080
> set HTTP_PROXY=http://username:password@proxy:8080
> ```

### <a name="verify-prerequisites"></a>Önkoşulları doğrulama

Tüm Azure işlevleri araçlarının yüklendiğini doğrulamak için Visual Studio Code komut paleti ' ni (F1) açın, **terminali seçin: Yeni tümleşik Terminal** komutu oluşturun ve Terminal açıldıktan sonra şu komutu `func`çalıştırın:

![Azure Işlevleri çekirdek araçları önkoşulları denetleniyor](media/tutorial-vs-code-serverless-python/check-prereqs.png)

Azure Işlevleri logosu ile başlayan çıkış (çıktıyı yukarı kaydırmanız gerekir) Azure Functions Core Tools olduğunu gösterir.

`func` Komut tanınmazsa, Azure Functions Core Tools yüklediğiniz klasörün PATH ortam değişkenine dahil edildiğini doğrulayın.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=01-verify-prerequisites)

## <a name="create-the-function"></a>İşlevi oluşturma

1. Azure Işlevleri için kod, kodu oluşturmadan önce oluşturduğunuz bir Işlevler _projesi_içinde yönetilir. Azure **'da: İşlevler** Gezgini (sol taraftaki Azure simgesi kullanılarak açılır), **Yeni proje** komut simgesini seçin veya komut paletini (F1) açın ve Azure işlevleri ' ni seçin **: Yeni proje**oluştur.

    ![Işlevler Gezgininde yeni proje oluştur düğmesi](media/tutorial-vs-code-serverless-python/project-create-new.png)

1. Aşağıdaki istemler:

    | İstem | Value | Açıklama | 
    | --- | --- | --- |
    | Proje için bir klasör belirtin | Geçerli açık klasör | Projenin oluşturulacağı klasör. Projeyi bir alt klasörde oluşturmak isteyebilirsiniz. |
    | İşlev uygulaması projeniz için bir dil seçin | **Python** | Kod için kullanılan şablonu belirleyen, işlevi için kullanılacak dil. |
    | Projenizin ilk işlevi için bir şablon seçin | **HTTP tetikleyicisi** | Bir HTTP tetikleyicisi kullanan bir işlev, işlevin uç noktasına yapılan bir HTTP isteği olduğunda çalıştırılır. (Azure Işlevleri için çeşitli diğer Tetikleyiciler vardır. Daha fazla bilgi edinmek için bkz. [Işlevlerle neler yapabilirim?](functions-overview.md#what-can-i-do-with-functions).) |
    | Bir işlev adı girin | HttpExample | Ad, işlev kodunu içeren bir alt klasör için yapılandırma verileriyle birlikte kullanılır ve ayrıca HTTP uç noktasının adını tanımlar. İşlevin kendisini tetikleyiciden ayırt etmek için varsayılan "HTTPTrigger" kabul etmek yerine "HttpExample" kullanın. |
    | Yetkilendirme düzeyi | **Çalışmayacaktır** | İşlev uç noktasına yapılan çağrılar bir [işlev anahtarı](functions-bindings-http-webhook.md#authorization-keys)gerektirir. |
    | Projenizi nasıl açmak istediğinizi seçin | **Geçerli pencerede aç** | Projeyi geçerli Visual Studio Code penceresinde açar. |

1. Kısa bir süre sonra, yeni projenin oluşturulduğunu belirten bir ileti. **Gezgin**'de, işlev için oluşturulan alt klasör vardır. 

1. Zaten açık değilse, varsayılan işlev kodunu içeren  *\_ \_\_init\_. Kopyala* dosyasını açın:

    [![Yeni bir Python işlevleri projesi oluşturma sonucu](media/tutorial-vs-code-serverless-python/project-create-results.png)](media/tutorial-vs-code-serverless-python/project-create-results.png)

    > [!NOTE]
    > Visual Studio Code,  *\_init \_.Kopyala'yı\_açtığınızda bir Python yorumlayıcı seçili olmadığını söyledikçe, komut paletini (F1) açın, Python 'u seçin:\_*  **Yorumlayıcı** komutunu seçin ve ardından yerel `.env` klasörde (projenin bir parçası olarak oluşturulan) sanal ortamı seçin. Bu ortam, [önkoşulların](#prerequisites)altında daha önce belirtildiği gibi Python 3.6 x 'i temel almalıdır.
    >
    > ![Projeyle oluşturulan sanal ortamı seçme](media/tutorial-vs-code-serverless-python/select-venv-interpreter.png)

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=02-create-function)

## <a name="examine-the-code-files"></a>Kod dosyalarını inceleyin

Yeni oluşturulan _httpexample_ işlevi alt klasöründe üç dosya bulunur:  *\_ \_\_init\_. Kopyala* işlevin kodunu içerir, *function. JSON* , işlevi Azure 'a tanımlar İşlevler ve *Sample. dat* örnek bir veri dosyasıdır. İsterseniz *Sample. dat* ' ı silebilirsiniz, çünkü yalnızca alt klasöre başka dosyalar ekleyebilmesinin görünmesini sağlayabilirsiniz.

Önce *function. JSON* ' a, sonra da  *\_init\_\_. Kopyala \_* ' da koda göz atalım.

### <a name="functionjson"></a>function. JSON

Function. JSON dosyası, Azure Işlevleri uç noktası için gerekli yapılandırma bilgilerini sağlar:

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
}
```

Özelliği, kod için başlangıç dosyasını tanımlar ve bu kodun adlı `main`bir Python işlevi içermesi gerekir. `scriptFile` Burada belirtilen dosya bir `main` işlev içerdiği sürece kodunuzu birden çok dosyaya çarpanlara ekleyebilirsiniz.

`bindings` Öğesi iki nesne içerir, biri gelen istekleri ve diğeri ise HTTP yanıtını açıklama. Gelen istekler için (`"direction": "in"`), işlev http get veya post isteklerine yanıt verir ve işlev anahtarını sağlamanız gerekir. Response (`"direction": "out"`), `main` Python işlevinden döndürülen değeri döndüren bir http yanıtdır.

### <a name="__initpy__"></a>\_\_init.py\_\_

Yeni bir işlev oluşturduğunuzda, Azure işlevleri  *\_init\_\_. Kopyala \_* içinde varsayılan Python kodu sağlar:

```python
import logging

import azure.functions as func


def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             status_code=400
        )
```

Kodun önemli bölümleri aşağıdaki gibidir:

- İçeri aktarmanız `func` `azure.functions`gerekir; günlük modülünün içeri aktarılması isteğe bağlıdır, ancak önerilir.
- Gerekli `main` Python işlevi adlı `func.request` `req`bir nesne alır ve türünde `func.HttpResponse`bir değer döndürür. Func içindeki bu nesnelerin özellikleri hakkında daha fazla bilgi edinebilirsiniz [. HttpRequest](/python/api/azure-functions/azure.functions.httprequest?view=azure-python) ve [Func. HttpResponse](/python/api/azure-functions/azure.functions.httpresponse?view=azure-python) başvuruları.
- `main` Sonra gövdesi, isteği işler ve bir yanıt üretir. Bu durumda, kod URL 'de bir `name` parametre arar. Bunun başarısız olması durumunda, istek gövdesinin JSON içerip `func.HttpRequest.get_json`içermediğini (kullanarak) ve JSON 'ın bir `name` değer içerip içermediğini (tarafından `get_json`döndürülen JSON `get` nesnesinin yöntemini kullanarak) denetler.
- Bir ad bulunursa kod, "Hello" dizesinin eklendiği adı döndürür; Aksi takdirde bir hata iletisi döndürür.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=03-examine-code-files)

## <a name="debug-locally"></a>Yerel olarak hata ayıkla

1. İşlevler projesini oluşturduğunuzda Visual Studio Code uzantısı, ' de **Python işlevlerine Ekle**adlı tek bir yapılandırma içeren `.vscode/launch.json` ' de bir başlatma yapılandırması oluşturur. Bu yapılandırma, projeyi başlatmak için yalnızca **F5** 'i seçebileceğiniz veya hata ayıklama Gezginini kullanabileceğiniz anlamına gelir:

    ![Işlev başlatma yapılandırmasını gösteren gezgin hata ayıkla](media/tutorial-vs-code-serverless-python/launch-configuration.png)

1. Hata ayıklayıcıyı başlattığınızda, kullanılabilir uç noktaların Özeti dahil olmak üzere Azure Işlevlerinden çıktıyı gösteren bir Terminal açılır. "HttpExample" dışında bir ad kullandıysanız URL 'niz farklı olabilir:

    ```output
    Http Functions:

            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    ```

1. Bu adrese bir tarayıcı açmak ya da bir tarayıcı başlatmak ve aynı URL 'ye yapıştırmak için Visual Studio Code **Çıkış** penceresindeki URL 'ye **CTRL + tıklama** ya da **cmd + ' ye tıklayın** . Her iki durumda da, uç nokta `api/<function_name>`bu durumda. `api/HttpExample` Bununla birlikte, URL bir ad parametresi içermediğinden, tarayıcı penceresi yalnızca, koddaki bu yola uygun olarak "Lütfen sorgu dizesinde veya istek gövdesinde bir ad geçirin".

1. Şimdi, gibi bir ad parametresi `http://localhost:7071/api/HttpExample?name=VS%20Code`eklemeyi deneyin, örneğin, bu kod yolunu çalıştırıp çalıştırdığınızı gösteren tarayıcı penceresinde "Merhaba Visual Studio Code!" iletisi görüntülenmelidir.

1. Bir JSON istek gövdesinde ad değerini geçirmek için JSON satır içi ile kıvrımlı gibi bir araç kullanabilirsiniz:

    ```bash
    # Mac OS/Linux: modify the URL if you're using a different function name
    curl --header "Content-Type: application/json" --request POST \
        --data {"name":"Visual Studio Code"} http://localhost:7071/api/HttpExample
    ```

    ```ps
    # Windows (escaping on the quotes is necessary; also modify the URL
    # if you're using a different function name)
    curl --header "Content-Type: application/json" --request POST \
        --data {"""name""":"""Visual Studio Code"""} http://localhost:7071/api/HttpExample
    ```

    Alternatif olarak, komutunu `curl --header "Content-Type: application/json" --request POST --data @data.json http://localhost:7071/api/HttpExample`içeren `{"name":"Visual Studio Code"}` ve kullanan *Data. JSON* gibi bir dosya oluşturun.

1. İşlevde hata ayıklamak için, URL 'ye bir istek okuyan `name = req.params.get('name')` ve bir istekte bulunan bir kesme noktası ayarlayın. Visual Studio Code hata ayıklayıcı bu satırda durarak değişkenleri incelemenizi ve kodda adım adım ilerlemenize olanak tanır. (Temel hata ayıklama hakkında kısa bir anlatım için bkz. [Visual Studio Code öğreticisi-hata ayıklayıcıyı yapılandırma ve çalıştırma](https://code.visualstudio.com/docs/python/python-tutorial.md#configure-and-run-the-debugger).)

1. İşlevi yerel olarak test ettiğiniz için, hata ayıklayıcıyı **(**  > hata**ayıklamayı Durdur** menü komutuyla veya hata ayıklama araç çubuğundaki **bağlantıyı kes** komutuyla) durdurun.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=04-test-debug)

## <a name="deploy-to-azure-functions"></a>Azure İşlevlerine dağıtma

Bu adımlarda, Azure 'da bir işlev uygulaması oluşturmak için Işlevler uzantısını, diğer gerekli Azure kaynaklarıyla birlikte kullanırsınız. İşlev uygulaması, kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır. Ayrıca veri ve [barındırma planı](functions-scale.md#hosting-plan-support)Için bir Azure depolama hesabı gerektirir. Bu kaynakların tümü tek bir kaynak grubu içinde düzenlenir.

1. **Azure 'da: İşlevler** Gezgini, **işlev uygulaması dağıt** komutunu seçin veya komut paleti ' ni (F1 **) açın ve Azure işlevlerini seçin: İşlev uygulaması** komutuna dağıtın. Bu durumda, işlev uygulaması Python projenizin Azure 'da çalıştığı yerdir.

    ![İşlev Uygulaması komutuna dağıt](media/tutorial-vs-code-serverless-python/deploy-command.png)

1. İstendiğinde, **Azure 'Da yeni işlev uygulaması oluştur**' u seçin ve Azure genelinde benzersiz bir ad sağlayın (genellikle kişisel veya şirket adınızı diğer benzersiz tanımlayıcılarla birlikte kullanarak), harfler, rakamlar ve kısa çizgiler kullanabilirsiniz. Daha önce bir İşlev Uygulaması oluşturduysanız, adı bu seçenek listesinde görünür.

1. Uzantı, Visual Studio Code açılan iletilerde ve **Çıkış** penceresinde (işlem birkaç dakika sürer) gözlemleyebileceğiniz aşağıdaki eylemleri gerçekleştirir:

    - Verdiğiniz adı kullanarak bir kaynak grubu oluşturun (kısa çizgileri kaldırma).
    - Bu kaynak grubunda depolama hesabı, barındırma planı ve işlev uygulaması oluşturun. Varsayılan olarak, bir [Tüketim planı](functions-scale.md#consumption-plan) oluşturulur. İşlevlerinizi adanmış bir planda çalıştırmak için, [Gelişmiş oluşturma seçenekleriyle yayımlamayı etkinleştirmeniz](functions-develop-vs-code.md)gerekir.
    - Kodunuzu işlev uygulamasına dağıtın.

    **Azure: İşlevler** Gezgini de ilerleme durumunu gösterir:

    ![Azure 'da dağıtım ilerleme durumu göstergesi: İşlevler Gezgini](media/tutorial-vs-code-serverless-python/deploy-progress.png)

1. Dağıtım tamamlandıktan sonra, Azure Işlevleri uzantısı üç ek eylem için düğmeler içeren bir ileti görüntüler:

    ![Ek eylemlerle başarılı dağıtımı belirten ileti](media/tutorial-vs-code-serverless-python/deployment-popup.png)

    **Akış günlükleri** ve **karşıya yükleme ayarları**için sonraki bölümlere bakın. **Çıktıyı görüntüle**için aşağıdaki 5. adıma bakın.

1. Dağıtımdan sonra **Çıkış** penceresinde Azure 'daki ortak uç nokta da gösterilmektedir:

    ```output
    HTTP Trigger Urls:
      HttpExample: https://vscode-azure-functions.azurewebsites.net/api/HttpExample
    ```

    Bu uç noktayı, yerel olarak yaptığınız testleri, URL parametrelerini ve/veya istek gövdesinde JSON verisi olan istekleri kullanarak çalıştırmak için kullanın. Genel uç noktanın sonuçları, daha önce test ettiğiniz Yerel uç noktanın sonuçlarıyla eşleşmelidir.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=05-deploy)

### <a name="stream-logs"></a>Akış günlükleri

Azure Işlevleri uzantısı için [sorun 589](https://github.com/microsoft/vscode-azurefunctions/issues/589) ' de açıklandığı gibi, günlük akışı desteği şu anda geliştirme aşamasındadır. Dağıtım iletisi açılan penceresinde **akış günlükleri** düğmesi, son olarak Azure 'daki günlük çıktısını Visual Studio Code 'e bağlayacaktır. Ayrıca, Işlevler projesine sağ tıklayıp **akış günlüklerini Başlat** veya **akış günlüklerini durdur**' u seçerek **Azure işlevleri** Gezgininde günlük akışını başlatabilir ve durdurabilirsiniz.

Ancak, bu komutlar henüz işlemsel değildir. Günlük akışı, Azure 'daki işlevler uygulamanızın adıyla değiştirerek `<app_name>` aşağıdaki komutu çalıştırarak bir tarayıcıda kullanılabilir:

```bash
# Replace <app_name> with the name of your Functions app on Azure
func azure functionapp logstream <app_name> --browser
```

### <a name="sync-local-settings-to-azure"></a>Yerel ayarları Azure 'a Eşitle

Dağıtım iletisi açılan penceresinde **ayarları karşıya yükle** düğmesi, *yerel. Settings. JSON* dosyanızda Azure 'a yaptığınız tüm değişiklikleri uygular. Ayrıca, Işlevler proje düğümünü genişleterek, **uygulama ayarları**' na sağ tıklayıp ve **yerel ayarları yükle...** ' yi seçerek **Azure işlevleri** Gezgininde komutunu çağırabilirsiniz. **Azure işlevleri 'ni seçmek için komut paletini de kullanabilirsiniz: Yerel ayarları** karşıya yükle komutu.

Ayarları karşıya yüklemek, var olan tüm ayarları güncelleştirir ve *yerel. Settings. JSON*içinde tanımlanan yeni ayarları ekler. Karşıya yükleme, Azure 'dan yerel dosyada listelenmeyen ayarları kaldırmaz. Bu ayarları kaldırmak için **Azure işlevleri** Gezgininde **uygulama ayarları** düğümünü genişletin, ayara sağ tıklayın ve **ayarı Sil...** seçeneğini belirleyin. Ayarları doğrudan Azure portal de düzenleyebilirsiniz.

Portalda yaptığınız herhangi bir değişikliği veya **Azure Gezgini** 'ni *yerel. Settings. JSON* dosyasına uygulamak için, **uygulama ayarları** düğümüne sağ tıklayıp **Uzaktan ayarları indir...** komutunu seçin. **Azure işlevleri 'ni seçmek için komut paletini de kullanabilirsiniz: Uzaktan ayarları** indir komutu.

## <a name="add-a-second-function"></a>İkinci bir Işlev ekleyin

İlk dağıtımdan sonra, kodunuzda ek işlevler ekleme ve aynı Işlevler uygulamasına yeniden dağıtma gibi değişiklikler yapabilirsiniz.

1. **Azure 'da: İşlevler** Gezgini, **işlev oluştur** komutunu seçin veya Azure işlevleri **'ni kullanın: Komut paletinden** işlev oluştur. İşlevi için aşağıdaki ayrıntıları belirtin:

    - Şablon: HTTP tetikleyicisi
    - Ad: "Digitsofpı"
    - Yetkilendirme düzeyi: Adsız

1. Visual Studio Code dosya Gezgini ' nde, işlev adınızın  *\_init\_\_. Kopyala \_* , *function. JSON*ve *Sample. dat*adlı dosyaları içeren bir alt klasörü vardır.

1. *\_ \_İnit.\_Kopyala'nın içeriğini, URL 'de belirtilen sayıda basamağa (Bu kod yalnızca bir URL parametresi kullanır), PI değerini içeren bir dize oluşturan aşağıdaki kodla eşleşecek şekilde değiştirin\_*

    ```python
    import logging

    import azure.functions as func

    """ Adapted from the second, shorter solution at http://www.codecodex.com/wiki/Calculate_digits_of_pi#Python
    """

    def pi_digits_Python(digits):
        scale = 10000
        maxarr = int((digits / 4) * 14)
        arrinit = 2000
        carry = 0
        arr = [arrinit] * (maxarr + 1)
        output = ""

        for i in range(maxarr, 1, -14):
            total = 0
            for j in range(i, 0, -1):
                total = (total * j) + (scale * arr[j])
                arr[j] = total % ((j * 2) - 1)
                total = total / ((j * 2) - 1)

            output += "%04d" % (carry + (total / scale))
            carry = total % scale

        return output;

    def main(req: func.HttpRequest) -> func.HttpResponse:
        logging.info('DigitsOfPi HTTP trigger function processed a request.')

        digits_param = req.params.get('digits')

        if digits_param is not None:
            try:
                digits = int(digits_param)
            except ValueError:
                digits = 10   # A default

            if digits > 0:
                digit_string = pi_digits_Python(digits)

                # Insert a decimal point in the return value
                return func.HttpResponse(digit_string[:1] + '.' + digit_string[1:])

        return func.HttpResponse(
             "Please pass the URL parameter ?digits= to specify a positive number of digits.",
             status_code=400
        )
    ```

1. Kod yalnızca http get 'i desteklediğinden, koleksiyon yalnızca `"get"` (yani, Kaldır `"post"`) içermesi `"methods"` için *function. JSON* ' ı değiştirin. Tüm dosya aşağıdaki gibi görünmelidir:

    ```json
    {
      "scriptFile": "__init__.py",
      "bindings": [
        {
          "authLevel": "anonymous",
          "type": "httpTrigger",
          "direction": "in",
          "name": "req",
          "methods": [
            "get"
          ]
        },
        {
          "type": "http",
          "direction": "out",
          "name": "$return"
        }
      ]
    }
    ```

1. **F5** ' i seçerek **veya hata** > **ayıklamayı Başlat** menü komutunu seçerek hata ayıklayıcıyı başlatın. **Çıkış** penceresinde artık her iki uç nokta de projenizde gösterilmektedir:

    ```output
    Http Functions:

            DigitsOfPi: [GET] http://localhost:7071/api/DigitsOfPi

            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    ```

1. Tarayıcıda veya bir tarayıcıda, çıktıyı bir istek `http://localhost:7071/api/DigitsOfPi?digits=125` haline getirin ve gözlemleyin. (Kod algoritmasının tamamen doğru olmadığını fark edebilirsiniz, ancak bu geliştirmeleri size yönelik geliştirmeleri bırakacağız!) İşiniz bittiğinde hata ayıklayıcıyı durdurun.

1. **Azure 'daki **işlev uygulaması dağıtım** kullanarak kodu yeniden dağıtın: İşlevler** Gezgini. İstenirse, daha önce oluşturulan İşlev Uygulaması seçin.

1. Dağıtım bittikten sonra (birkaç dakika sürer!), **Çıkış** penceresinde testlerinizi tekrarlamanız için kullanabileceğiniz genel uç noktalar gösterilir.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=06-second-function)

## <a name="add-a-binding-to-write-messages-to-azure-storage"></a>Azure depolama 'ya ileti yazmak için bir bağlama ekleme

Bir _bağlama_ , işlev kodunuzu herhangi bir veri erişim kodu yazmadan Azure depolama gibi kaynaklara bağlamanıza olanak tanır. Bir bağlama *function. JSON* dosyasında tanımlanır ve hem giriş hem de çıktıyı temsil edebilir. Bir işlev birden çok giriş ve çıkış bağlaması kullanabilir, ancak yalnızca bir tetikleyici olabilir. Daha fazla bilgi için bkz. [Azure işlevleri Tetikleyicileri ve bağlamaları kavramları](functions-triggers-bindings.md).

Bu bölümde, bu öğreticide daha önce oluşturulan HttpExample işlevine bir depolama bağlaması eklersiniz. İşlevi bu bağlamayı, her istekle birlikte depolamaya ileti yazmak için kullanır. Söz konusu depolama, işlev uygulaması tarafından kullanılan varsayılan depolama hesabını kullanır. Ancak depolama alanının yoğun kullanımını planlıyorsanız, ayrı bir hesap oluşturmayı düşünmek isteyebilirsiniz.

1. Azure işlevleri projeniz için uzak ayarları, komut paletini açıp Azure işlevleri ' ni seçerek  ***yerel. Settings. JSON* dosyanız ile eşitleyin: Uzak ayarları**indirin. *Local. Settings. JSON* ' i açın ve için `AzureWebJobsStorage`bir değer içerip içermediğinden emin olun. Bu değer, depolama hesabının bağlantı dizesidir.

1. Klasöründe, *. JSON işlevine*sağ tıklayın, **bağlama Ekle**' yi seçin: `HttpExample`

    ![Visual Studio Code Gezgininde bağlama komutu ekleme](media/tutorial-vs-code-serverless-python/add-binding-command.png)

1. Visual Studio Code takip eden istemler bölümünde aşağıdaki değerleri seçin veya sağlayın:

    | İstem | Sağlanacak değer |
    | --- | --- |
    | Bağlama yönünü ayarla | dışı |
    | Yönü giden bağlamayı seçin | Azure Kuyruk Depolama |
    | Kodunuzda bu bağlamayı tanımlamak için kullanılan ad | msg |
    | İletinin gönderileceği kuyruk | outqueue |
    | *Local. Settings. JSON* (depolama bağlantısı isteyen) ayarını seçin | AzureWebJobsStorage |

1. Bu seçimleri yaptıktan sonra, aşağıdaki bağlamanın *function. JSON* dosyanıza eklendiğini doğrulayın:

    ```json
        {
          "type": "queue",
          "direction": "out",
          "name": "msg",
          "queueName": "outqueue",
          "connection": "AzureWebJobsStorage"
        }
    ```

1. *Host. JSON* içeriğini, [uzantı demeti başvurusunu](functions-bindings-register.md#extension-bundles)ekleyerek aşağıdaki ile değiştirin.

    ```json
    {
        "version": "2.0",
        "extensionBundle": {
            "id": "Microsoft.Azure.Functions.ExtensionBundle",
            "version": "[1.*, 2.0.0)"
        }
    }
    ```

1. Bağlamayı yapılandırdığınıza göre, bunu işlev kodunuzda kullanabilirsiniz. Daha sonra, yeni tanımlanan `main` bağlama kodunuzda  *\_init \_\_.\_Kopyala*içindeki işleve bağımsız değişken olarak görünür. Örneğin ,`msg`  *\_\_httpexample\_içindeki init. Kopyala dosyasını, bir zaman damgamış bir ileti yazmak için bağımsız değişkenini kullanarak aşağıdaki \_* isteyen. Yorumlar, belirli değişiklikleri açıklar:

    ```python
    import logging
    import datetime  # MODIFICATION: added import
    import azure.functions as func

    # MODIFICATION: the added binding appears as an argument; func.Out[func.QueueMessage]
    # is the appropriate type for an output binding with "type": "queue" (in function.json).
    def main(req: func.HttpRequest, msg: func.Out[func.QueueMessage]) -> func.HttpResponse:
        logging.info('Python HTTP trigger function processed a request.')

        name = req.params.get('name')
        if not name:
            try:
                req_body = req.get_json()
            except ValueError:
                pass
            else:
                name = req_body.get('name')

        if name:
            # MODIFICATION: write the a message to the message queue, using msg.set
            msg.set(f"Request made for {name} at {datetime.datetime.now()}")

            return func.HttpResponse(f"Hello {name}!")
        else:
            return func.HttpResponse(
                 "Please pass a name on the query string or in the request body",
                 status_code=400
            )
    ```

1. Bu değişiklikleri yerel olarak test etmek için **F5** ' i seçerek **veya hata** > **ayıklamayı Başlat** menü komutunu seçerek Visual Studio Code hata ayıklayıcıyı yeniden başlatın. **Çıkış** penceresinde olduğu gibi, projenizdeki uç noktaları göstermemelidir.

1. Bir tarayıcıda, httpexample uç noktasına `http://localhost:7071/api/HttpExample?name=VS%20Code` bir istek oluşturmak için URL 'yi ziyaret ederek sıraya bir ileti de yazmanız gerekir.

1. İletinin "outqueue" kuyruğuna yazıldığını doğrulamak için (bağlamada adlandırılmış olarak), üç yöntemden birini kullanabilirsiniz:

    1. [Azure Portal](https://portal.azure.com)oturum açın ve işlevler projenizi içeren kaynak grubuna gidin. Bu kaynak grubunda, projenin depolama hesabını bulup açın ve **Kuyruklar**' a gidin. Bu sayfada, tüm günlüğe kaydedilen iletileri görüntülemesi gereken "outqueue" seçeneğine gidin.

    1. Bu sırayı, Visual Studio ile tümleşen Azure Depolama Gezgini, [Visual Studio Code kullanarak Işlevleri Azure depolama 'Ya bağlama](functions-add-output-binding-storage-queue-vs-code.md)bölümünde açıklandığı gibi, özellikle de [çıkış sırasını İnceleme](functions-add-output-binding-storage-queue-vs-code.md#examine-the-output-queue) bölümünü kullanarak açın ve inceleyin.

    1. Depolama kuyruğunu [sorgulama](functions-add-output-binding-storage-queue-python.md#query-the-storage-queue)bölümünde açıklandığı gibi, depolama kuyruğunu sorgulamak IÇIN Azure CLI 'yi kullanın.
    
1. Bulutta test etmek için, **Azure 'daki **işlev uygulaması dağıtma** ' yı kullanarak kodu yeniden dağıtın: İşlevler** Gezgini. İstenirse, daha önce oluşturulan İşlev Uygulaması seçin. Dağıtım bittikten sonra (birkaç dakika sürer!), **Çıkış** penceresi yeniden testlerinizi yinelemek için kullanabileceğiniz genel uç noktaları gösterir.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=07-storage-binding)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz İşlev Uygulaması, en düşük maliyette oluşturulabilecek kaynakları içerir (bkz. [Işlevler fiyatlandırması](https://azure.microsoft.com/pricing/details/functions/)). Kaynakları temizlemek için **Azure 'da işlev uygulaması sağ tıklayın: İşlevler** Gezgini ve **işlev uygulaması Sil**' i seçin. Ayrıca, [Azure Portal](https://portal.azure.com)ziyaret edebilir, sol taraftaki gezinti bölmesinden **kaynak grupları** ' nı seçin, Bu öğreticinin işleminde oluşturulan kaynak grubunu seçip **kaynak grubunu sil** komutunu kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Işlevlerine Python kodu dağıtmaya yönelik Bu izlenecek yolu kutlarız! Artık daha az sayıda sunucusuz işlev oluşturmaya hazırsınız.

Daha önce belirtildiği gibi, GitHub deposunu ziyaret ederek ( [vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions)) işlevler uzantısı hakkında daha fazla bilgi edinebilirsiniz. Sorunlar ve katkılar da hoş geldiniz.

Kullanabileceğiniz farklı Tetikleyicileri araştırmak için [Azure Işlevlerine genel bakış](functions-overview.md) konusunu okuyun.

Veri Machine Learning depolama da dahil olmak üzere Python 'dan kullanabileceğiniz Azure hizmetleri hakkında daha fazla bilgi edinmek için [Azure Python Geliştirici Merkezi](/azure/python/?view=azure-python)' ni ziyaret edin.

Ayrıca, yararlı bulabileceğiniz Visual Studio Code için başka Azure uzantıları da vardır. Yalnızca uzantılar Gezgininde "Azure" üzerinde arama yapın:

![Visual Studio Code için Azure uzantıları](media/tutorial-vs-code-serverless-python/azure-extensions.png)

Bazı popüler uzantılar şunlardır:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure uygulama hizmeti](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)
- [Azure CLı araçları](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Azure Resource Manager araçları](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
