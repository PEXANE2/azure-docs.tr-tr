---
title: Geliştirme bilgisayarınızı bir AKS kümesine bağlayın (önizleme)
services: azure-dev-spaces
ms.date: 11/04/2019
ms.topic: conceptual
description: Azure Geliştirme Alanları ile geliştirme bilgisayarınızı bir AKS kümesine nasıl bağlayabilirsiniz öğrenin
keywords: Azure Dev Alanlar, Dev Alanlar, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler
ms.openlocfilehash: 772f221a8047a71902f36fa98ded6c24b5e02d27
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235011"
---
# <a name="connect-your-development-computer-to-an-aks-cluster-preview"></a>Geliştirme bilgisayarınızı bir AKS kümesine bağlayın (önizleme)

Azure Dev Spaces, uygulamanızın veya hizmetlerinizin geri kalanıyla birlikte Kubernetes kümenize bağlı yken, geliştirme bilgisayarınızda bir kapsayıcı yla veya kapsayıcı olmadan kod çalıştırmanızı ve hata ayıklamanızı sağlar. Geliştirme bilgisayarınızı kümenize bağlamak, herhangi bir Docker veya Kubernetes yapılandırması oluşturmak zorunda kalmadan uygulamanızı hızla geliştirmenize ve uçlardan uca test gerçekleştirmenize yardımcı olur. AkS kümenize diğer iş yüklerini veya aynı kümeyi kullanan kullanıcıları etkilemeden de bağlanabilirsiniz.

Azure Dev Spaces, bağlı AKS kümeniz ile geliştirme bilgisayarınız arasındaki trafiği yeniden yönlendirir. Bu trafik yeniden yönlendirmesi, geliştirme bilgisayarınızdaki kodun ve AKS kümenizde çalışan hizmetlerin aynı AKS kümesindeler gibi iletişim kurmasına olanak tanır. Kodunuz geliştirme bilgisayarınızda çalışırken, bu kodu çalıştırmak ve hata ayıklamak için kullandığınız geliştirme araçlarında da esneklik vardır. Azure Geliştirme Alanları, geliştirme bilgisayarınızdaAKS kümenizdeki bölmelerde kullanılabilen ortam değişkenlerini ve monte edilmiş dosyaları çoğaltmanın bir yolunu da sağlar.

Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:

* Azure'da yönetilen bir Kubernetes kümesinde Azure Dev Alanları'nı ayarlayın.
* Bir dev alana birden çok mikro hizmet içeren büyük bir uygulama dağıtın.
* AKS kümeniz ile geliştirme bilgisayarınızda çalışan kod arasındaki trafiği yeniden yönlendirmek için Azure Dev Alanları'nı kullanın.

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu kılavuz, geliştirme bilgisayarınızı bir AKS kümesine bağlanınmasını göstermek için [Azure Dev Spaces Bike Paylaşımı örnek uygulamasını](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp) kullanır. Örnek uygulamayı çalıştırmak için [Azure Dev Spaces Bike Sharing örnek uygulaması README'deki](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/README.md) yönergeleri izleyin. Alternatif olarak, bir AKS kümesinde kendi uygulamanız varsa, aşağıdaki adımları izleyebilir ve kendi hizmet ve bölmelerinizin adlarını kullanabilirsiniz.

### <a name="limitations"></a>Sınırlamalar

* UDP şu anda desteklenmez.

### <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.
* [Yüklü Azure CLI][azure-cli].
* MacOS veya Windows 10'da yüklenen ve çalışan [Azure Geliştirme Alanları][azds-vs-code] uzantısı ile Visual Studio [Kodu.][vs-code]
* [Azure Dev Spaces Bike Paylaşımı örnek uygulaması](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp) veya AKS kümesiüzerinde çalışan kendi uygulamanız.

## <a name="connect-your-development-computer"></a>Geliştirme bilgisayarınızı bağlama

Visual Studio Code'ta *dev alanları/örnekleri/BikeSharingApp/Bikes'ı* açın ve geliştirme bilgisayarınızı AKS kümenize bağlamak için Azure Geliştirme Alanları uzantısını kullanın.

Azure Dev Spaces uzantısını kullanmak için Visual Studio Code'daki Komut Paleti'ni *Görünüm* sonra *Komut Paleti'ni*tıklatarak açın. Yazmaya `Azure Dev Spaces: Redirect` başlayın ve `Azure Dev Spaces: Redirect an existing Kubernetes service to my machine [Preview]`, `Azure Dev Spaces: Redirect an existing Kubernetes pod to my machine [Preview]`veya `Azure Dev Spaces: Redirect a new Kubernetes pod to my machine [Preview]`.

![Komutlar](../media/how-to-connect/connect-commands.png)

### <a name="select-a-redirection-option"></a>Yeniden yönlendirme seçeneği seçin

`Azure Dev Spaces: Redirect an existing Kubernetes service to my machine [Preview]`Çalıştırırsanız, varolan bir Kubernetes hizmetini seçmeniz istenir:

![Hizmet Seçin](../media/how-to-connect/connect-choose-service.png)

Bu seçenek, bu hizmet için AKS kümesindeki tüm trafiği geliştirme bilgisayarınızda çalışan uygulamanızın sürümüne yönlendirir. Bu hizmetin AKS kümesinde çalışan birden çok bölmesi varsa, bu hizmetin tüm trafiği yalnızca geliştirme bilgisayarınıza yönlendirilir. Azure Dev Spaces ayrıca uygulamadan gelen tüm giden trafiği AKS kümenize geri yönlendirir.

Çalıştırırsanız, `Azure Dev Spaces: Redirect an existing Kubernetes pod to my machine [Preview]`belirli bir bölme seçmeniz istenir:

![Pod'u Seçin](../media/how-to-connect/connect-choose-pod.png)

Bu seçenek belirli bir bölmeye bağlanır. Bu seçenek, trafik göndermeyen veya almayan bölmelerle etkileşim kurmak ve sonlandırılan bölmeleri çoğaltmak için yararlıdır. Bölme trafik gönderip alıyorsa, bu seçenek benzer bir `Azure Dev Spaces: Redirect an existing Kubernetes service to my machine [Preview]` şekilde çalışır ve seçilen bölmenin hizmetiyle ilgili tüm bölmeler için AKS kümesindeki tüm trafiği yeniden yönlendirir.

Çalıştırırsanız, `Azure Dev Spaces: Redirect a new Kubernetes pod to my machine [Preview]`varolan bir bölme veya hizmet seçmeniz istenmez. Bu seçenek, geliştirme bilgisayarınızda çalışan uygulamadan aks kümesine tüm giden trafiği yönlendirir.

Bu örnekiçin, `Azure Dev Spaces: Redirect an existing Kubernetes service to my machine [Preview]` *bisiklet* servisini seçin ve seçin.

### <a name="select-a-connection-mode"></a>Bağlantı modu seçin

Yeniden yönlendirme seçeneğinizi seçtikten sonra, *Değiştir* veya *Klon* bağlantı modunu seçmeniz istenir.

![Değiştir veya Klonla](../media/how-to-connect/connect-replace-clone.png)

*Değiştir* seçeneği, AKS kümesindeki geçerli bölme veya hizmetin yerini alır ve bu hizmetin tüm trafiğini geliştirme bilgisayarınıza yönlendirir. Bu seçenek, yeniden yönlendirdiğiniz hizmetle etkileşimde bulunan AKS kümenizdeki diğer hizmetler için rahatsız edici olabilir, uygulama geliştirme bilgisayarınızda başlayana kadar çalışmayabilir. *Klon* seçeneği, varolan bir alt geliştirme alanı seçmenize veya bir bölme veya hizmet in trafiğini geliştirme bilgisayarınıza yönlendirmek için yeni bir alt geliştirme alanı oluşturmanıza olanak tanır. Bu seçenek, yalnızca o çocuk geliştirme alanına gelen trafik geliştirme bilgisayarınıza yönlendirilecektir, çünkü izole çalışma nızı ve diğer hizmetleri kesintiye uğratmamanızı sağlar. *Klon* seçeneği, AKS kümenizin Azure Dev Spaces'inetkinleştirilmiş olmasını gerektirir.

Bu örnekte, *Değiştir'i*seçin.

> [!NOTE]
> Varolan hizmetbölmenizde birden çok kapsayıcı varsa, uygulamanın kapsayıcısını seçmeniz de istenir.

### <a name="select-a-port-for-your-application"></a>Uygulamanız için bir bağlantı noktası seçin

Bağlantı modunuzu seçtikten sonra, yerel uygulamanız olan TCP bağlantı noktasını girmeniz istenir. Uygulamanız birden çok bağlantı noktası açarsa, bunları örneğin *80,81*gibi virgülle ayırın. Başvurunuz herhangi bir ağ isteklerini kabul etmiyorsa, *0*girin. Bu örnekiçin, *3000*girin.

![Connect select bağlantı noktasını seç](../media/how-to-connect/connect-choose-port.png)

### <a name="confirm-you-are-connected"></a>Bağlı olduğunuzu doğrulayın

Uygulamanızın TCP bağlantı noktasını seçtikten sonra, Azure Dev Spaces AKS kümesine bir bağlantı kurar. Azure Dev Spaces, AKS kümesi ile geliştirme bilgisayarınız arasındaki trafiği yeniden yönlendirmek için AKS kümenize bir aracı enjekte eder. Bu bağlantıyı kurmak birkaç dakika sürebilir. Azure Dev Spaces, geliştirme bilgisayarınızdaki *ana bilgisayar* dosyasını değiştirmek için yönetici erişimi de talep edecektir.

> [!IMPORTANT]
> Azure Dev Spaces AKS kümenize bir bağlantı kurduktan sonra, bağlantı *değiştir* modunu seçerseniz geliştirme bilgisayarınızdaki hizmeti başlatana kadar AKS kümenizdeki diğer hizmetler düzgün çalışmayabilir. Bunun yerine, yeniden yönlendirmeniz için bir alt öğe geliştirme alanı oluşturmak ve üst alandaki herhangi bir kesintiyi önlemek için *Klon* bağlantı modunu seçebilirsiniz. Ayrıca, hizmetinizin geliştirme bilgisayarınızda bulunmayan bir bağımlılığı varsa, uygulamanızı değiştirmeniz veya [ek yapılandırma](#additional-configuration) sağlamanız gerekebilir

Azure Dev Spaces, AKS kümenize bağlantı kurduktan sonra *AZDS Connect - Bikes* başlıklı bir terminal penceresi açar. Bu terminal penceresi, AKS kümenizden yapılandırılan tüm ortam değişkenlerine ve DNS girişlerine sahiptir. Bu terminal penceresinde veya Visual Studio Code hata ayıklayıcısını kullanarak çalıştırdığınız tüm kodLAR AKS kümesine bağlıdır.

![Terminal](../media/how-to-connect/connect-terminal.png)

Ayrıca, Azure Dev Spaces, tüm çıktılarıyla *Dev Spaces Connect* başlıklı bir pencere oluşturur.

![Çıktı](../media/how-to-connect/connect-output.png)

Azure Dev Spaces bağlantı durumunu gösteren bir durum çubuğu öğesine de sahiptir.

![Durum](../media/how-to-connect/connect-status.png)

Durum çubuğunun Dev Spaces'i gösterdiğini *doğrulayın: Yerel bağlantı noktası 3000'deki dev/bike'lara bağlı.*

### <a name="configure-your-application-on-your-development-computer"></a>Uygulamanızı geliştirme bilgisayarınızda yapılandırın

*AZDS Connect - Bikes* terminal `npm install`penceresi açın ve çalıştırın:

```console
$ npm install

> fsevents@1.2.9 install ./dev-spaces/samples/BikeSharingApp/Bikes/node_modules/fsevents
> node install
...
```

*Hata Ayıklama'yı* tıklatın ve *yapılandırmaları açın.* Bir ortam seçmek *istenirse, Düğüm.js'yi*seçin. Bu bir `.vscode/launch.json` dosya oluşturur. Bu dosyanın içeriğini aşağıdakilerle değiştirin:

```json
{
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "name": "Launch via NPM",
            "runtimeExecutable": "npm",
            "runtimeArgs": [
                "run-script",
                "debug"
            ],
            "port": 9229
        }
    ]
}
```

[Open package.json](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/Bikes/package.json) ve hata ayıklama komut dosyası ekleyin:

```json
  "devDependencies": {
    "nodemon": "^1.18.10"
  },
  "scripts": {
    "debug": "node --nolazy --inspect-brk=9229 server.js"
  }
```

### <a name="start-your-application-on-your-development-computer"></a>Uygulamanızı geliştirme bilgisayarınızda başlatın

Soldaki *Hata Ayıklama* simgesine tıklayın ve üstteki *NPM üzerinden Başlat'ın* yanındaki başlat düğmesine tıklayın.

![NPM üzerinden başlat](../media/how-to-connect/launch-npm.png)

Uygulamanız başlar ve Azure Dev Spaces AKS kümeniz ile geliştirme bilgisayarınız arasındaki trafiği yeniden yönlendirir. *Hata Ayıklama Konsolunda*aşağıdakine benzer iletileri görürsünüz:

```console
/usr/local/bin/npm run-script debug 
...
Debugger attached.
Collection: bikes
MongoDB connection string: mongodb://databases-mongo
Connected to MongoDB
Listening on port 3000
```

Azure Dev Spaces durum çubuğunu tıklayarak ve uygulamanızın genel URL'sini seçerek *bikesharingweb* hizmetine gidin. Ayrıca, daha önce çalıştırdığınız `azds list-uris` komuttan ortak URL'yi de bulabilirsiniz. Kümenizde Azure Geliştirme Alanları kullanmıyorsanız, kullandığınız ad alanı için uygulamanın IP'sini veya URL'sini kullanın. Yukarıdaki örnekte, *bikesharingweb* hizmetinin genel `http://dev.bikesharingweb.fedcab0987.eus.azds.io/`URL'si . Kullanıcı olarak *Aurelia Briggs'i (müşteri)* seçin ve ardından kiralamak için bir bisiklet seçin.

### <a name="set-a-break-point"></a>Bir kesme noktası ayarlama

[Server.js'yi](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/Bikes/server.js#L233) açın ve imlecinizi oraya koymak için 233 satırında bir yere tıklayın. *F9* tuşuna basarak veya *Hata* Ayıklama'yı tıklatarak bir kesme noktası ayarlayın ve *breakpoint'i toggle.*

Genel URL'yi açarak *bikesharingweb* hizmetine gidin. Kullanıcı olarak *Aurelia Briggs'i (müşteri)* seçin ve ardından kiralamak için bir bisiklet seçin. Bisikletin yüklenmediği görüntüye dikkat edin. Visual Studio Code'a dönün ve 233 hattını izleyin vurgulanır. Ayarladığınız kesme noktası, 233 satırındaki hizmeti duraklattı. Hizmeti devam ettirmek için *F5* tuşuna basveya *Hata Ayıklama'yı* tıklatın ve *devam edin.* Tarayıcınıza dönün ve bisiklet için bir yer tutucu görüntüsü gördüğünüzden doğrulayın.

İmlecinizi 233. `server.js` *F9*

### <a name="update-your-application"></a>Uygulamanızı güncelleyin

232 ve 233 satırlarını kaldırmak için edin: `server.js`

```javascript
    // Hard code image url *FIX ME*
    theBike.imageUrl = "/static/logo.svg";
```

Bölüm şimdi şöyle görünmelidir:

```javascript
    var theBike = result;
    theBike.id = theBike._id;
    delete theBike._id;
```

Değişikliklerinizi kaydedin ve *Hata Ayıklama'yı* tıklatın ve hata *ayıklama'yı yeniden başlatın.* Tarayıcınızı yenileyin ve bisiklet için artık bir yer tutucu görüntüsü görmediğinizden doğrulayın.

Hata Ayıklama'yı durdurmak için *Hata* *Ayıklama'yı durdur'u* tıklatın. AKS kümesiyle bağlantısını kesmek için Azure Dev Spaces durum çubuğunu tıklatın.

## <a name="additional-configuration"></a>Ek yapılandırma

Azure Dev Spaces, yönlendirme trafiğini ve çoğaltma ortamı değişkenlerini ek yapılandırma olmadan işleyebilir. Aks kümenizdeki ConfigMap dosyası gibi kapsayıcıya monte edilmiş dosyaları indirmeniz gerekiyorsa, bu `azds-local.env` dosyaları geliştirme bilgisayarınıza indirmek için bir dosya oluşturabilirsiniz.

İşte bir `azds-local.env`örnek:

```
# This downloads the "whitelist" volume from the container,
# saves it to a temporary directory on your development computer,
# and sets the full path to an environment variable called WHITELIST_PATH.

WHITELIST_PATH=${volumes.whitelist}/whitelist

# This downloads a file from the container's 'default-token-<any>' mount directory 
# to /var/run/secrets/kubernetes.io/serviceaccount on your development computer.

KUBERNETES_IN_CLUSTER_CONFIG_OVERWRITE=${volumes.default-token-*|/var/run/secrets/kubernetes.io/serviceaccount}


# This makes the myapp1 service available to your development computer
# regardless of the AKS cluster you are connected to and
# sets the local IP to an environment variable called MYAPP1_SERVICE_HOST.

# If the myapp1 service is made available in this way, 
# you can also access it using "myapp1" and "myapp1.svc.cluster.local"
# in addition to the IP in the MYAPP1_SERVICE_HOST environment variable.

MYAPP1_SERVICE_HOST=${services.myapp1}

# This makes the service myapp2 in namespace mynamespace available to your 
# development computer regardless of the AKS cluster you are connected to and
# sets the local IP to an environment variable called MYAPP2_SERVICE_HOST.

MYAPP2_SERVICE_HOST=${services.mynamespace.myapp2}
```

## <a name="using-logging-and-diagnostics"></a>Günlük ve tanılama kullanma

Günlüğe kaydetme çıktısı, geliştirme bilgisayarınızı AKS kümenize bağladıktan sonra *Dev Spaces Connect* penceresine yazılır.

![Çıktı](../media/how-to-connect/connect-output.png)

Azure Geliştirme Alanları durum çubuğuna tıklayın ve *tanılama bilgilerini göster'i*seçin. Bu komut, günlüğe kaydetme çıkışında geçerli ortam değişkenlerini ve DNS tümlerini yazdırır.

![Tanılama ile çıktı](../media/how-to-connect/connect-output-diagnostics.png)

Ayrıca, [geliştirme bilgisayarınızın *TEMP* dizininde][azds-tmp-dir] `Azure Dev Spaces` tanılama günlüklerini bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Çekme isteği deponuzun ana dalı ile birleştirilmeden önce, çekme isteğindeki değişiklikleri doğrudan AKS'de test etmek için Azure Dev Spaces ve GitHub Eylemleri'ni nasıl kullanacağınızı öğrenin.

> [!div class="nextstepaction"]
> [Azure Kubernetes Hizmeti & GitHub Eylemleri][gh-actions]

[azds-tmp-dir]: ../troubleshooting.md#before-you-begin
[azds-vs-code]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[azure-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[bike-sharing-github]: https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp
[gh-actions]: github-actions.md
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[team-quickstart]: ../quickstart-team-development.md
[vs-code]: https://code.visualstudio.com/download