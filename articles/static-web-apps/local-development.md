---
title: Azure statik Web Apps için yerel geliştirmeyi ayarlama
description: Azure statik Web Apps için yerel geliştirme ortamınızı ayarlamayı öğrenin
services: static-web-apps
author: burkeholland
ms.service: static-web-apps
ms.topic: how-to
ms.date: 05/08/2020
ms.author: buhollan
ms.openlocfilehash: 36d580b7659325d4bf5f13889f774ddaa2ab0702
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83597131"
---
# <a name="set-up-local-development-for-azure-static-web-apps-preview"></a>Azure statik Web Apps önizlemesi için yerel geliştirmeyi ayarlama

Azure statik Web Apps örneği, iki farklı türde uygulamalardan oluşur. Birincisi, statik içeriğiniz için bir Web uygulamasıdır. Web Apps genellikle ön uç çerçeveleri ve kitaplıkları ile veya statik site oluşturucuları ile oluşturulur. İkinci boyut, zengin bir arka uç geliştirme ortamı sağlayan bir Azure Işlevleri uygulaması olan API 'dir.

Azure statik Web Apps, bulutta çalışırken, istekleri `api` Web uygulamasından Azure işlevleri uygulamasına, CORS yapılandırmasına gerek kalmadan sorunsuz bir şekilde eşler. Yerel olarak, uygulamanızı bu davranışı taklit etmek için yapılandırmanız gerekir.

Bu makalede, aşağıdaki kavramlar dahil olmak üzere yerel geliştirme için önerilen en iyi uygulamalar gösterilmektedir:

- Web uygulamasını statik içerik için ayarlama
- Uygulamanızın API 'SI için Azure Işlevleri uygulamasını yapılandırma
- Hata ayıklama ve uygulamayı çalıştırma
- Uygulamanızın dosya ve klasör yapısına yönelik en iyi uygulamalar

## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio Code](https://code.visualstudio.com/)
- Visual Studio Code için [Azure işlevleri uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
- Visual Studio Code için [canlı sunucu uzantısı](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)
  - Yalnızca ön uç JavaScript çerçevesi veya statik site oluşturucunun CLı kullanmıyorsanız gereklidir

## <a name="run-projects-locally"></a>Projeleri yerel olarak çalıştır

Azure statik Web uygulamasını yerel olarak çalıştırmak, projenizin bir API içerip içermediğini bağlı olarak üç işlem içerir.

- Yerel bir Web sunucusu çalıştırma
- API 'YI çalıştırma
- Web projesini API 'ye bağlama

Bir Web sitesinin nasıl oluşturulduğuna bağlı olarak, uygulamayı tarayıcıda çalıştırmak için yerel bir Web sunucusu gerekli olmayabilir veya olmayabilir. Ön uç JavaScript çerçeveleri ve statik site oluşturucuları kullanılırken, bu işlev ilgili uçlarında (komut satırı arabirimleri) yerleşik olarak bulunur. Aşağıdaki bağlantılar, bir çerçeve, kitaplık ve üreteçilerin seçimi için CLı başvurusunu işaret noktasıdır.

### <a name="javascript-frameworks-and-libraries"></a>JavaScript çerçeveleri ve kitaplıkları

- [Angular CLı](https://angular.io/cli)
- [Vue CLı](https://cli.vuejs.org/guide/creating-a-project.html)
- [CLı 'ye tepki verme](https://create-react-app.dev/)

### <a name="static-site-generators"></a>Statik site oluşturucuları

- [CLı tarafından Gatsby](https://www.gatsbyjs.org/docs/gatsby-cli/)
- [Kugo](https://gohugo.io/getting-started/quick-start/)
- [Jekyıll](https://jekyllrb.com/docs/usage/)

Sitenize hizmeti sağlamak için bir CLı aracı kullanıyorsanız, [API 'Yi çalıştıran](#run-api-locally) bölüme atlayabilirsiniz.

### <a name="running-a-local-web-server-with-live-server"></a>Canlı sunucu ile yerel bir Web sunucusu çalıştırma

Visual Studio Code için canlı sunucu uzantısı, statik içerik sunan yerel bir geliştirme Web sunucusu sağlar.

#### <a name="create-a-repository"></a>Depo oluştur

1. [https://github.com/staticwebdev/vanilla-api/generate](https://github.com/staticwebdev/vanilla-api/generate)Bu şablonu kullanarak **Vanilla-API**adlı yeni bir GitHub projesine gidin ve oluşturun.

    :::image type="content" source="media/local-development/vanilla-api.png" alt-text="GitHub yeni depo penceresi":::

1. Visual Studio Code'u açın.

1. Komut paletini açmak için **F1** tuşuna basın.

1. Arama kutusuna **kopya** yazın ve **Git: Kopyala**' yı seçin.

    :::image type="content" source="media/local-development/command-palette-git-clone.png" alt-text="Visual Studio Code git kopyalama seçeneği":::

1. **Depo URL 'si**için aşağıdaki değeri girin.

   ```http
   git@github.com:<YOUR_GITHUB_ACCOUNT>/vanilla-api.git
   ```

1. Yeni proje için bir klasör konumu seçin.

1. Kopyalanmış depoyu açmak isteyip istemediğiniz sorulduğunda **Aç**' ı seçin.

    :::image type="content" source="media/local-development/open-new-window.png" alt-text="Yeni pencerede aç":::

Visual Studio Code klonlanan projeyi düzenleyicide açar.

### <a name="run-the-website-locally-with-live-server"></a>Web sitesini Live Server ile yerel olarak çalıştırın

1. Komut paletini açmak için **F1** tuşuna basın.

1. Arama kutusuna **canlı sunucu** yazın ve **canlı sunucu: Live Server ile aç** ' ı seçin

    Uygulamayı göstermek için bir tarayıcı sekmesi açılır.

    :::image type="content" source="media/local-development/vanilla-api-site.png" alt-text="Tarayıcıda çalışan basit statik site":::

    Bu uygulama uç noktaya bir HTTP isteği oluşturur `api/message` . Şu anda, bu uygulamanın API bölümünün başlatılması gerektiğinden bu istek başarısız oluyor.

### <a name="run-api-locally"></a>API 'YI yerel olarak çalıştır

Azure statik Web Apps API 'Leri Azure Işlevleri tarafından desteklenmektedir. Azure statik Web Apps projesine API ekleme ile ilgili ayrıntılar için bkz. Azure [işlevleri Ile Azure statik Web Apps API ekleme](add-api.md) .

API oluşturma sürecinin bir parçası olarak Visual Studio Code için bir başlatma yapılandırması oluşturulur. Bu yapılandırma _. vscode_ klasöründe bulunur. Bu klasör, API 'YI yerel olarak oluşturmak ve çalıştırmak için gerekli tüm ayarları içerir.

1. Visual Studio Code içinde, API 'yi başlatmak için **F5** ' e basın.

1. API derleme işlemindeki çıktıyı gösteren yeni bir Terminal örneği açılır.

    :::image type="content" source="media/local-development/terminal-api-debug.png" alt-text="Visual Studio Code terminalde çalışan API":::

   Visual Studio Code durum çubuğu artık turuncu. Bu renk, API 'nin artık çalıştığını ve hata ayıklayıcının ekli olduğunu gösterir.

1. Ardından, **Ctrl/Cmd** tuşlarına basın ve, API 'yi çağıran bir tarayıcı penceresi açmak için TERMINALDEKI URL 'ye tıklayın.

    :::image type="content" source="media/local-development/hello-from-api-endpoint.png" alt-text="Tarayıcı API çağrısının sonucunu görüntüleme":::

### <a name="debugging-the-api"></a>API 'de hata ayıklama

1. Visual Studio Code ' de _API/GetMessage/index. js_ dosyasını açın.

1. Kesme noktası ayarlamak için 2. satırdaki sol kenar boşluğuna tıklayın. Kesme noktasının ayarlandığını belirten kırmızı bir nokta görünür.

    :::image type="content" source="media/local-development/breakpoint-set.png" alt-text="Visual Studio Code kesme noktası":::

1. Tarayıcıda, konumundaki çalıştıran sayfayı yenileyin <http://127.0.0.1:7071/api/message> .

1. Kesme noktası Visual Studio Code, program yürütme duraklatıldı.

   :::image type="content" source="media/local-development/breakpoint-hit.png" alt-text="Kesme noktası isabet Visual Studio Code":::

   API 'niz için [Visual Studio Code, tüm hata ayıklama deneyimi mevcuttur](https://code.visualstudio.com/Docs/editor/debugging) .

1. Yürütmeye devam etmek için hata ayıklama çubuğundaki **devam** düğmesine basın.

    :::image type="content" source="media/local-development/continue-button.png" alt-text="Visual Studio Code 'de devam düğmesi":::

### <a name="calling-the-api-from-the-application"></a>Uygulamadan API çağırma

Dağıtıldığında, Azure statik Web Apps, bu istekleri _API_ klasöründeki uç noktalarla otomatik olarak eşler. Bu eşleme, uygulamadan API 'ye yapılan isteklerin aşağıdaki örneğe benzer şekilde görünmesini sağlar.

```javascript
let response = await fetch("/api/message");
```

Uygulamanızın JavaScript Framework CLı ile oluşturulmuş olmasına bağlı olarak, `api` uygulamanızı yerel olarak çalıştırırken yolun yolunu yapılandırmanın iki yolu vardır.

- Ortam yapılandırma dosyaları (JavaScript çerçeveleri ve kitaplıkları için önerilir)
- Yerel ara sunucu

### <a name="environment-configuration-files"></a>Ortam yapılandırma dosyaları

Uygulamanızı CLı içeren ön uç çerçeveleri ile oluşturuyorsanız, ortam yapılandırma dosyalarını kullanmanız gerekir. Her çerçeve veya kitaplık, bu ortam yapılandırma dosyalarını işlemek için farklı bir yönteme sahiptir. Uygulamanız yerel olarak çalışırken kullanılan geliştirme için bir yapılandırma dosyası ve uygulamanız üretimde çalışırken kullanılan üretime yönelik bir yapılandırma dosyası olması yaygındır. Kullandığınız JavaScript çerçevesi veya statik site Oluşturucu CLı, uygulamanız Azure statik Web Apps tarafından oluşturulduğunda, geliştirme dosyasını yerel olarak ve üretim dosyasını kullanacak şekilde otomatik olarak bilir.

Geliştirme yapılandırma dosyasında, API 'nin yolunu belirtebilirsiniz. Bu, `http:127.0.0.1:7071` sitenizin API 'sinin yerel olarak çalıştığı yerel konumunu işaret eder.

```
API=http:127.0.0.1:7071/api
```

Üretim yapılandırma dosyasında, API 'nin yolunu belirtin `api` . Bu şekilde, uygulamanız üretimde çalışırken "yoursite.com/api" aracılığıyla API 'yi çağıracaktır.

```
API=api
```

Bu yapılandırma değerlerine, Web uygulamasının JavaScript 'teki düğüm ortam değişkenleri olarak başvurulabilir.

```js
let response = await fetch(`${process.env.API}/message`);
```

CLı, sitenizi geliştirme modunda çalıştırmak veya üretim için siteyi oluşturmak üzere kullanıldığında, `process.env.API` değer uygun yapılandırma dosyasındaki değerle değiştirilmiştir.

Ön uç JavaScript çerçeveleri ve kitaplıkları için ortam dosyalarını yapılandırma hakkında daha fazla bilgi için şu makalelere bakın:

- [Angular ortam değişkenleri](https://angular.io/guide/build#configuring-application-environments)
- [Tepki verme-özel ortam değişkenleri ekleme](https://create-react-app.dev/docs/adding-custom-environment-variables/)
- [Vue-modlar ve ortam değişkenleri](https://cli.vuejs.org/guide/mode-and-env.html)

[!INCLUDE [static-web-apps-local-proxy](../../includes/static-web-apps-local-proxy.md)]

##### <a name="restart-live-server"></a>Canlı sunucuyu yeniden Başlat

1. Visual Studio Code içinde komut paletini açmak için **F1** tuşuna basın.

1. **Canlı sunucu** yazın ve **canlı sunucu: canlı sunucuyu durdur**' u seçin.

    :::image type="content" source="media/local-development/stop-live-server.png" alt-text="Visual Studio komut paletinde canlı sunucu komutunu durdur":::

1. Komut paletini açmak için **F1** tuşuna basın.

1. **Canlı** sunucu yazın ve canlı sunucu **: açık canlı sunucu**' yı seçin.

1. Üzerinde çalışan uygulamayı yenileyin `http://locahost:3000` . Tarayıcı artık API 'den döndürülen iletiyi görüntüler.

    :::image type="content" source="media/local-development/hello-from-api.png" alt-text="Tarayıcıda görünen API 'den Merhaba":::

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama ayarlarını yapılandırma](application-settings.md)
