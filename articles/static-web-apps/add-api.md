---
title: Azure Işlevleri ile Azure statik Web Apps bir API ekleme
description: Azure Işlevleri 'ni kullanarak statik Web uygulamanıza sunucusuz bir API ekleyerek Azure statik Web Apps kullanmaya başlayın.
services: static-web-apps
author: manekinekko
ms.service: static-web-apps
ms.topic: how-to
ms.date: 05/08/2020
ms.author: wachegha
ms.openlocfilehash: a6aee5c8049e03a43c547f419f6c6646617e651c
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83598461"
---
# <a name="add-an-api-to-azure-static-web-apps-preview-with-azure-functions"></a>Azure Işlevleri ile Azure statik Web Apps önizlemesine API ekleme

Azure Işlevleri ile tümleştirme yoluyla, Azure statik Web Apps sunucusuz API 'Ler ekleyebilirsiniz. Bu makalede bir Azure statik Web Apps sitesine API ekleme ve dağıtma işlemlerinin nasıl yapılacağı gösterilir.

API yollarının güvenliğini sağlama hakkında daha fazla bilgi için [yönlendirme kılavuzuna](routes.md)bakın.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free).
- [Visual Studio Code](https://code.visualstudio.com/)
- Visual Studio Code için [Azure işlevleri uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
- [Canlı sunucu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) uzantısı.

## <a name="create-a-git-repository"></a>Git deposu oluşturma 

Aşağıdaki adımlarda, yeni bir depoyu oluşturma ve dosyaları bilgisayarınıza kopyalama işlemleri gösterilmektedir.

1. https://github.com/staticwebdev/vanilla-basic/generateYeni bir depo oluşturmak için ' a gidin.
1. _Depo adı_ kutusuna **My-Vanilla-API**yazın.
1. **Şablondan depo oluştur**' a tıklayın.

   :::image type="content" source="media/add-api/create-repository.png" alt-text="Vanilla 'dan yeni bir depo oluşturma-temel":::

Projeniz oluşturulduktan sonra Git deposunu kopyalamak için Visual Studio Code kullanabilirsiniz.

1. Komut paletinde komutunu açmak için **F1** tuşuna basın.
1. URL 'YI _Git: kopyalama_ istemine yapıştırın ve **ENTER**tuşuna basın.

   :::image type="content" source="media/add-api/vscode-git-0.png" alt-text="Visual Studio Code kullanarak bir GitHub projesini kopyalama":::

   :::image type="content" source="media/add-api/github-clone-url.png" alt-text="Visual Studio Code kullanarak bir GitHub projesini kopyalama":::


## <a name="create-your-local-project"></a>Yerel projenizi oluşturma

Bu bölümde, yerel bir Azure Işlevleri projesi oluşturmak için Visual Studio Code kullanırsınız. Daha sonra, Işlevler uygulamasını Azure 'da yayımlarsınız.

1. _My-Vanilla-API_ projesinde, **API**adlı bir alt klasör oluşturun.

   > [!NOTE]
   > Bu klasöre herhangi bir ad verebilirsiniz. Bu örnekte, kullanılır `api` . 

2. Komut paletini açmak için **F1** tuşuna basın
3. **Azure işlevleri yazın: yeni proje oluştur...**
4. **ENTER** tuşuna basın
5. **Gözatmayı** seçin
6. Proje çalışma alanınızın dizini olarak **API** klasörünü seçin
7. **Seç ' i** seçin

   :::image type="content" source="media/add-api/create-azure-functions-vscode-1.png" alt-text="Visual Studio Code kullanarak yeni bir Azure Işlevi oluşturma":::

8. İstemlere aşağıdaki bilgileri sağlayın:

    - _İşlev projeniz için bir dil seçin_: **JavaScript** seçin
    - _Projenizin ilk işlevi için bir şablon seçin_: **http tetikleyicisi** seçin
    - _Bir işlev adı girin_: tür **GetMessage**
    - _Yetkilendirme düzeyi_: herkesin işlev uç noktanızı çağırmasını sağlayan **anonim**' i seçin.
        - Yetkilendirme düzeyleri hakkında bilgi edinmek için bkz. [Yetkilendirme anahtarları](../azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys).

9. Bu bilgileri kullanarak, Visual Studio Code HTTP tetikleyicisiyle bir Azure Işlevleri projesi oluşturur.
    - Yerel proje dosyalarını Visual Studio Code Gezgin penceresinde görüntüleyebilirsiniz.
    - Oluşturulan dosyalar hakkında daha fazla bilgi edinmek için bkz. [oluşturulan proje dosyaları](https://docs.microsoft.com/azure/azure-functions/functions-develop-vs-code#generated-project-files).

10. Uygulamanız artık bu örneğe benzer bir proje yapısına sahip olmalıdır.

    ```files
    ├── api
    │   ├── GetMessage
    │   │   ├── function.json
    │   │   ├── index.js
    │   │   └── sample.dat
    │   ├── host.json
    │   ├── local.settings.json
    │   ├── package.json
    │   └── proxies.json
    ├── index.html
    ├── readme.md
    └── styles.css
    ```

11. Sonra, `GetMessage` aşağıdaki kodla _API/GetMessage/index. js_ altındaki işlevi güncelleştirin.

    ```JavaScript
    module.exports = async function (context, req) {
      context.res = {
        body: { 
          text: "Hello from the API" 
        }
      };
    };
    ```

12. `GetMessage`Aşağıdaki ayarlarla yapılandırmayı güncelleştirin `api/GetMessage/function.json` .

    ```json
    {
      "bindings": [
        {
          "authLevel": "anonymous",
          "type": "httpTrigger",
          "direction": "in",
          "name": "req",
          "methods": [
            "get"
          ],
          "route": "message"
        },
        {
          "type": "http",
          "direction": "out",
          "name": "res"
        }
      ]
    }
    ```
    
Yukarıdaki ayarlarla API uç noktası şu şekilde olur:

- İşleve bir HTTP isteğiyle tetiklenen
- Kimlik doğrulama durumundan bağımsız olarak tüm istekler için kullanılabilir
- _/Api/Message_ yolu aracılığıyla sunulur

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Visual Studio Code, Azure 'da yayımlamadan önce bu projeyi yerel geliştirme bilgisayarınızda çalıştırmanıza olanak sağlamak için [Azure Functions Core Tools](https://docs.microsoft.com/azure/azure-functions/functions-run-local) tümleştirilir.

1. Işlevler uygulamasını başlatmak için **F5** 'e basarak işlevi çalıştırın ve ana araçlar çıkışı, _Terminal_ panelinde görüntülenir.

2. Azure Functions Core Tools önceden yüklenmemişse, istemde **yükleme** ' yi seçin.

    Temel araçlar yüklendiğinde, uygulamanız _Terminal_ panelinden başlatılır. Çıktının bir parçası olarak, HTTP ile tetiklenen işlevinizin URL uç noktasını yerel olarak çalıştıran bir şekilde görebilirsiniz.

    :::image type="content" source="media/add-api/create-azure-functions-vscode-2.png" alt-text="Visual Studio Code kullanarak yeni bir Azure Işlevi oluşturma":::

3. Çalıştıran temel araçlar ile bir istek yürütmek için aşağıdaki URL 'ye gidin `GET` .

   <http://localhost:7071/api/message>

   Tarayıcıda aşağıdakine benzer bir yanıt döndürülür:

   :::image type="content" source="media/add-api/create-azure-functions-vscode-3.png" alt-text="Visual Studio Code kullanarak yeni bir Azure Işlevi oluşturma":::

İşlevin düzgün çalıştığını doğruladıktan sonra JavaScript uygulamasından API çağırabilirsiniz.

### <a name="call-the-api-from-the-application"></a>Uygulamadan API 'YI çağırma

[!INCLUDE [static-web-apps-local-proxy](../../includes/static-web-apps-local-proxy.md)]


#### <a name="update-files-to-access-the-api"></a>API 'ye erişmek için dosyaları güncelleştirme

1. Daha sonra, aşağıdaki kodla _Dizin. html_ dosyası içeriğini güncelleştirin ve metni API işlevinden getirip ekranda görüntüleyin:

   ```html
   <!DOCTYPE html>
   <html lang="en">

   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <link rel="stylesheet" href="styles.css">
     <title>Vanilla JavaScript App</title>
   </head>

   <body>
     <main>
       <h1>Vanilla JavaScript App</h1>
       <p>Loading message from the API: <b id="name">...</b></p>
     </main>

     <script>
       (async function() {
         let { text } = await( await fetch(`/api/message`)).json();
         document.querySelector('#name').textContent = text;
       }())
     </script>
   </body>

   </html>
   ```

   Temel araçlar çalışırken, _index. html_ dosyasını kullanmak ve bir tarayıcıda açmak Için [canlı sunucu](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) Visual Studio Code uzantısını kullanın. 

2. **F1** tuşuna basın ve **canlı sunucu: Live Server ile aç**' ı seçin.

   :::image type="content" source="media/add-api/create-azure-functions-vscode-4.png" alt-text="Visual Studio Code kullanarak yeni bir Azure Işlevi oluşturma":::

   > [!NOTE]
   > Dosyayı çalıştırmak için diğer HTTP sunucularını veya proxy 'leri kullanabilirsiniz `index.html` . Kaynağından erişim `index.html` `file:///` çalışmayacak.

### <a name="commit-and-push-your-changes-to-github"></a>Değişikliklerinizi yürütün ve GitHub 'a gönderin

Visual Studio Code kullanarak değişikliklerinizi uzak git deposuna yürütün ve gönderin.

1. **F1** tuşuna basarak komut paletini açın
1. **Git yazın: Tümünü Kaydet**
1. Bir COMMIT iletisi ekleyin
1. **Git: Push** yazın

## <a name="create-a-static-web-app"></a>Statik Web uygulaması oluşturma

:::image type="content" source="media/add-api/create-static-app-on-azure-portal-1.png" alt-text="Azure portal ekran 1 ' de statik uygulama oluşturma":::

1. [Azure Portal](https://portal.azure.com) gidin
1. **Kaynak oluştur ' a** tıklayın
1. **Statik Web Apps** ara
1. **Statik Web Apps (Önizleme)** seçeneğine tıklayın
1. **Oluştur** 'a tıklayın
1. _Azure aboneliğinizi_ seçin
1. Yeni bir _kaynak grubu_ seçin veya oluşturun
1. Uygulamayı **My-Vanilla-API**olarak adlandırın.
1. Size en yakın _bölgeyi_ seçin
1. **Ücretsiz** _SKU 'yu_ seçin
1. GitHub ile **oturum açma** düğmesine tıklayın ve GitHub ile kimlik doğrulaması yapın
1. Tercih ettiğiniz _kuruluşu_ seçin
1. _Depo_ açılır listesinden **My-Vanilla-API** ' yi seçin
1. _Dal_ açılan listesinden **ana öğe** seçin
1. Derleme yapılandırmasını düzenlemek için **İleri: oluştur >** düğmesine tıklayın

:::image type="content" source="media/add-api/create-static-app-on-azure-portal-2.png" alt-text="Azure portal-ekran 2 ' de statik uygulama oluşturma":::

Ardından, aşağıdaki derleme ayrıntılarını ekleyin.

1. _Uygulama konumu_için **./** girin.

1. API _konumu_ kutusuna **API** girin.

   Bu, önceki adımda oluşturulan API klasörünün adıdır.
   
1. Varsayılan değeri, _uygulama yapıtı konumundan_kaldırın ve kutuyu boş bırakın.

1. **Gözden geçir ve oluştur**’a tıklayın.

| Ayar | Açıklama             | Gerekli |
| -------- | ----------------------- |
|  Uygulama konumu | Statik uygulama kaynak kodunun konumu | Yes |
|  API konumu | API arka ucunun konumu. Bu, Azure Işlevleri uygulama projesinin kök klasörüne işaret eder | Hayır |
|  Uygulama yapıtı konumu | Yapı çıkış klasörünün konumu. Bazı ön uç JavaScript çerçeveleri, üretim dosyalarını bir klasöre yerleştiren bir yapı adımına sahiptir. Bu ayar, derleme çıkış klasörünü işaret eder. | Hayır |

:::image type="content" source="media/add-api/create-static-app-on-azure-portal-3.png" alt-text="Azure portal için statik uygulama oluşturma-ekran 3":::

1. **Oluştur** 'a tıklayın
1. Dağıtımın bitmesini bekle (Bu işlem birkaç dakika sürebilir)
1. Şuraya gidin`https://github.com/<YOUR_GITHUB_ACCOUNT>/my-vanilla-api/actions?query=workflow%3A"Azure+Pages+CI%2FCD"`
1. Yapılandırmanın başarılı olduğundan emin olun

:::image type="content" source="media/add-api/github-workflow-0.png" alt-text="GitHub Iş akışı":::

Dağıtılan API ' de kullanılabilir olacaktır `https://<STATIC_APP_NAME>.azurestaticapps.net/api/<FUNCTION_OR_ROUTE_NAME>` .

:::image type="content" source="media/add-api/github-workflow-1.png" alt-text="GitHub Iş akışı":::

Uygulama URL 'sini Azure portal de bulabilirsiniz:

:::image type="content" source="media/add-api/static-app-url-from-portal.png" alt-text="Azure portal statik uygulama URL 'sine erişme":::

Alternatif olarak, konumundaki Azure statik Web uygulamanıza doğrudan erişebilirsiniz `https://<STATIC_APP_NAME>.azurestaticapps.net` ve tarayıcıdaki sonucu kontrol edebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı daha fazla kullanım için tutmak istemiyorsanız, Azure statik Web uygulamasını ve ilgili kaynaklarını silmek için aşağıdaki adımları kullanabilirsiniz.

1. [Azure Portal](https://portal.azure.com) gidin
1. En üstteki arama çubuğunda **kaynak grupları** yazın
1. **Kaynak grupları** ' na tıklayın 
1. **Myresourcegroup** seçin
1. _Myresourcegroup_ sayfasında, listelenen kaynakların silmek istedikleriniz olduğundan emin olun.
1. **Sil** ' i seçin
1. Metin kutusuna **Myresourcegroup** yazın
1. **Sil**’i seçin.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama ayarlarını yapılandırma](./application-settings.md)
