---
title: 'Öğretici: Azure statik Web Apps üzerinde sunucu tarafından işlenen Next.js Web siteleri dağıtma'
description: Azure statik Web Apps ile Next.js dinamik siteler oluşturun ve dağıtın.
services: static-web-apps
author: christiannwamba
ms.service: static-web-apps
ms.topic: tutorial
ms.date: 05/08/2020
ms.author: chnwamba
ms.custom: devx-track-javascript
ms.openlocfilehash: 80a38c069f937783b60ede46bc4319253798ff44
ms.sourcegitcommit: 0b8320ae0d3455344ec8855b5c2d0ab3faa974a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87431645"
---
# <a name="deploy-server-rendered-nextjs-websites-on-azure-static-web-apps-preview"></a>Azure statik Web Apps önizlemede sunucu tarafından işlenen Next.js Web sitelerini dağıtma

Bu öğreticide, [Next.js](https://nextjs.org) üretilmiş bir statik Web sitesini [Azure statik Web Apps](overview.md)dağıtmayı öğreneceksiniz. Başlamak için Next.js uygulamasını ayarlamayı, yapılandırmayı ve dağıtmayı öğreneceksiniz. Bu işlem sırasında, genellikle Next.js ile statik sayfalar oluştururken ortaya çıkacak yaygın güçlüklerle uğraşmak üzere de öğrenirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/).
- GitHub hesabı. [Ücretsiz hesap oluşturun](https://github.com/join).
- [Node.js](https://nodejs.org) yüklendi.

## <a name="set-up-a-nextjs-app"></a>Next.js uygulaması ayarlama

Bir uygulama oluşturmak için Next.js CLı kullanmak yerine, var olan bir Next.js uygulamasını içeren bir başlangıç deposu kullanabilirsiniz. Bu depo, ortak bir dağıtım sorununu vurgulayan dinamik yollarla Next.js bir uygulama sunar. Dinamik yollara, bir süre içinde daha fazla bilgi edineceğinizi sağlayan ek bir dağıtım yapılandırması gerekir.

Başlamak için, bir şablon deposundan GitHub hesabınızda yeni bir depo oluşturun. 

1. <http://github.com/staticwebdev/nextjs-starter/generate> sayfasına gidin
1. Depoyu **nextjs-Starter** olarak adlandırın
1. Sonra, yeni depoyu makinenize kopyalayın. Hesap adınızla değiştirdiğinizden emin olun `<YOUR_GITHUB_ACCOUNT_NAME>` .

    ```bash
    git clone http://github.com/<YOUR_GITHUB_ACCOUNT_NAME>/nextjs-starter
    ```

1. Yeni kopyalanmış Next.js uygulamasına gidin:

   ```bash
   cd nextjs-starter
   ```

1. Bağımlılıkları yükler:

    ```bash
    npm install
    ```

1. Geliştirme aşamasında Next.js uygulamayı Başlat:

    ```bash
    npm run dev
    ```

<http://localhost:3000>Aşağıdaki Web sitesini tercih ettiğiniz tarayıcınızda açık olarak görmeniz gereken uygulamayı açmak için şuraya gidin:

:::image type="content" source="media/deploy-nextjs/start-nextjs-app.png" alt-text="Next.js uygulamayı Başlat":::

Bir çerçeveye/kitaplığa tıkladığınızda, seçili öğe hakkında bir Ayrıntılar sayfası görmeniz gerekir:

:::image type="content" source="media/deploy-nextjs/start-nextjs-details.png" alt-text="Ayrıntılar sayfası":::

## <a name="generate-a-static-website-from-nextjs-build"></a>Next.js derlemeden statik bir Web sitesi oluşturma

Kullanarak bir Next.js sitesi oluşturduğunuzda `npm run build` , uygulama statik bir site değil, geleneksel bir Web uygulaması olarak oluşturulur. Statik bir site oluşturmak için aşağıdaki uygulama yapılandırmasını kullanın.

1. Statik yolları yapılandırmak için, projenizin kökünde _next.config.js_ adlı dosya oluşturun ve aşağıdaki kodu ekleyin...

    ```javascript
    module.exports = {
      exportTrailingSlash: true,
      exportPathMap: function() {
        return {
          '/': { page: '/' }
        };
      }
    };
    ```
    
      Bu yapılandırma `/` `/` , yol için sunulan ve sayfa _/index.js_ sayfa dosyası olan Next.js sayfasıyla eşlenir.

1. Derleme betiği _üzerindepackage.js_, komutunu kullanarak derlemeden sonra da statik bir site oluşturmak için güncelleştirin `next export` . `export`Komut bir statik site oluşturur.

    ```json
    "scripts": {
      "dev": "next dev",
      "build": "next build && next export",
    },
    ```

    Artık bu komutla birlikte, statik Web Apps, `build` her bir yürütmeyi her gönderişinizde betiği çalıştıracaktır.

1. Statik site oluştur:

    ```bash
    npm run build
    ```

    Statik site oluşturulup çalışma dizininizin kökündeki bir _Out_ klasörüne kopyalanır.

    > [!NOTE]
    > Dağıtım sırasında CI/CD tarafından oluşturulması gerektiğinden, bu klasör _. gitignore_ dosyasında listelenir.

## <a name="push-your-static-website-to-github"></a>Statik Web sitenizi GitHub 'a gönderme

Azure statik Web Apps, uygulamanızı bir GitHub deposundan dağıtır ve belirli bir dala gönderilen her bir yürütmeye devam eder. Aşağıdaki komutları kullanarak yaptığınız değişiklikleri GitHub 'a eşitleyin.

1. Değiştirilen tüm dosyaları hazırlama

    ```bash
    git add .
    ```

1. Tüm Değişiklikleri Kaydet

    ```bash
    git commit -m "Update build config"
    ```

1. Değişikliklerinizi GitHub 'a gönderin.

    ```bash
    git push origin master
    ```

## <a name="deploy-your-static-website"></a>Statik Web sitenizi dağıtın

Aşağıdaki adımlarda, az önce GitHub 'a gönderdiğiniz uygulamayı Azure statik Web Apps bağlama gösterilmektedir. Azure 'da bir kez uygulamayı bir üretim ortamına dağıtabilirsiniz.

### <a name="create-a-static-app"></a>Statik uygulama oluşturma

1. [Azure Portal](https://portal.azure.com) gidin
1. **Kaynak oluştur** seçeneğine tıklayın
1. **Statik Web Uygulamaları** için arama yapın
1. **Statik Web Uygulamaları (Önizleme)** seçeneğine tıklayın
1. **Oluştur** seçeneğine tıklayın

1. *Abonelik* açılan listesinden bir abonelik seçin veya varsayılan değeri kullanın.
1. *Kaynak grubu açılır grubunun* altındaki **Yeni** bağlantıya tıklayın. *Yeni kaynak grubu adı*' nda **mystaticsite** yazın ve **Tamam** ' a tıklayın.
1. **Ad** metin kutusuna uygulamanız için genel olarak benzersiz bir ad sağlayın. Geçerli karakterler, `a-z` , `A-Z` `0-9` ve içerir `-` . Bu değer, statik uygulamanız için biçiminde URL öneki olarak kullanılır `https://<APP_NAME>.azurestaticapps.net` .
1. *Bölge* açılır penceresinde, size en yakın bölgeyi seçin.
1. SKU açılır listesinden **ücretsiz** ' ı seçin.

   :::image type="content" source="media/deploy-nextjs/create-static-web-app.png" alt-text="Statik Web uygulaması oluşturma":::

### <a name="add-a-github-repository"></a>GitHub deposu ekleme

Yeni statik Web Apps hesabının, işleme otomatik olarak dağıtabilmesi için Next.js uygulamanızla depoya erişmesi gerekir.

1. **GitHub Ile oturum aç düğmesine** tıklayın
1. GitHub Kullanıcı adınız olabilecek Next.js projeniz için depoyu oluşturduğunuz **kuruluşu** seçin.
1. Daha önce oluşturduğunuz deponun adını bulun ve seçin.
1. *Dal açılır listesinden* dal olarak **ana** öğesini seçin.

   :::image type="content" source="media/deploy-nextjs/connect-github.png" alt-text="GitHub'ı bağlama":::

### <a name="configure-the-build-process"></a>Yapı işlemini yapılandırma

Azure statik Web Apps, NPM modüllerini yükleme ve her dağıtım sırasında çalıştırma gibi genel görevleri otomatik olarak yürütmek üzere oluşturulmuştur `npm run build` . Bununla birlikte, el ile yapılandırmanız gereken uygulama kaynak klasörü ve derleme hedef klasörü gibi birkaç ayar vardır.

1. Statik çıkış klasörünü yapılandırmak için **derleme** sekmesine tıklayın.

   :::image type="content" source="media/deploy-nextjs/build-tab.png" alt-text="Derleme sekmesi":::

2. *Uygulama yapıtı konumu* metin **kutusuna yazın.**

### <a name="review-and-create"></a>Gözden geçirme ve oluşturma

1. Ayrıntıların tümünün doğru olduğunu doğrulamak için **gözden geçir + oluştur** düğmesine tıklayın.
1. Kaynak oluşturma Işlemini başlatmak için **Oluştur** ' a tıklayın ve ayrıca dağıtım Için bir GitHub eylemi sağlayın.
1. Dağıtım tamamlandıktan sonra **Kaynağa Git ' e** tıklayın.
1. _Genel bakış_ penceresinde, dağıtılan uygulamanızı açmak için *URL* bağlantısına tıklayın. 

Web sitesi hemen yük alıyorsa, arka plan GitHub eylemleri iş akışı çalışmaya devam eder. İş akışı tamamlandıktan sonra Web uygulamanızı görüntülemek için tarayıcıyı Yenile ' ye tıklayabilirsiniz.
Web sitesi hemen yük alıyorsa, arka plan GitHub eylemleri iş akışı çalışmaya devam eder. İş akışı tamamlandıktan sonra Web uygulamanızı görüntülemek için tarayıcıyı Yenile ' ye tıklayabilirsiniz.

Deponuzdaki eylemlere giderek eylemler iş akışlarının durumunu kontrol edebilirsiniz:

```url
https://github.com/<YOUR_GITHUB_USERNAME>/nextjs-starter/actions
```

### <a name="sync-changes"></a>Değişiklikleri Eşitle

Uygulamayı oluştururken Azure static Web Apps, deponuzda bir GitHub eylemleri iş akışı dosyası oluşturdu. Git geçmişinizin eşitlenmesi için bu dosyayı yerel deponuza getirmeniz gerekir.

Terminale dönün ve aşağıdaki komutu çalıştırın `git pull origin master` .

## <a name="configure-dynamic-routes"></a>Dinamik yolları yapılandırma

Yeni dağıtılan siteye gidin ve Framework veya kitaplık logolarının birine tıklayın. Ayrıntılar sayfası almak yerine 404 hata sayfasına sahip olursunuz.

:::image type="content" source="media/deploy-nextjs/404-in-production.png" alt-text="Dinamik yollarla 404":::

Bu hatanın nedeni, Next.js yalnızca uygulama yapılandırmasına bağlı olarak giriş sayfasını ürettiğinden oluşur.

## <a name="generate-static-pages-from-dynamic-routes"></a>Dinamik yollardan statik sayfalar oluşturma

1. _next.config.js_ dosyasını, Next.js her bir çerçeve/kitaplık için statik sayfalar oluşturmak üzere kullanılabilir tüm verilerin bir listesini kullanacak şekilde güncelleştirin:

   ```javascript
   const data = require('./utils/projectsData');

   module.exports = {
     exportTrailingSlash: true,
     exportPathMap: async function () {
       const { projects } = data;
       const paths = {
         '/': { page: '/' },
       };
  
       projects.forEach((project) => {
         paths[`/project/${project.slug}`] = {
           page: '/project/[path]',
           query: { path: project.slug },
         };
       });
  
       return paths;
     },
   };
   ```

   > [!NOTE]
   > `exportPathMap`İşlevi zaman uyumsuz bir işlevdir, bu nedenle bu işlevdeki BIR API 'ye istek yapabilir ve yollar oluşturmak için döndürülen listeyi kullanabilirsiniz.

2. Yeni değişiklikleri GitHub deponuza gönderin ve GitHub eylemleri sitenizi yeniden oluşturup birkaç dakika bekleyin. Derleme tamamlandıktan sonra 404 hatası kaybolur.

   :::image type="content" source="media/deploy-nextjs/404-in-production-fixed.png" alt-text="Dinamik yollar üzerinde 404 düzeltildi":::

> [!div class="nextstepaction"]
> [Özel etki alanı ayarlama](custom-domain.md)
