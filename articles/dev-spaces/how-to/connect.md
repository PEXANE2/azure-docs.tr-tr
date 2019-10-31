---
title: Azure Dev Spaces Bağlan
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 10/24/2019
ms.topic: conceptual
description: Azure Dev Spaces Connect 'i kullanmayı öğrenin
keywords: Azure Dev Spaces, dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Connect
ms.openlocfilehash: 2e1984b838e961239e0533179c34eccb118abd8e
ms.sourcegitcommit: f7f70c9bd6c2253860e346245d6e2d8a85e8a91b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73065879"
---
# <a name="azure-dev-spaces-connect-preview"></a>Azure Dev Spaces Connect (Önizleme)

Azure Dev Spaces Connect, geliştirme makinenizde bir kapsayıcı ile veya bu kodla hata ayıklamanıza olanak sağlar. bu sayede hala uygulamanızın veya hizmetlerinizin geri kalanı ile Kubernetes kümenize bağlanırsınız. Geliştirme makinenizi kümenize bağlamak, herhangi bir Docker veya Kubernetes yapılandırması oluşturmaya gerek kalmadan uygulamanızı hızlı bir şekilde geliştirmenize ve uçtan uca test gerçekleştirmenize yardımcı olur. Ayrıca, diğer iş yüklerini veya aynı kümeyi kullanabilecek kullanıcıları etkilemeden AKS kümenize bağlanabilirsiniz.

Azure Dev Spaces Connect, bağlı AKS kümeniz ile geliştirme makineniz arasındaki trafiği yeniden yönlendirir. Bu trafik yeniden yönlendirme, AKS kümenizde çalışan geliştirme makinenizde ve hizmetinizdeki kodların aynı AKS kümesinde olduklarından farklı iletişim kurmasına olanak tanır. Kodunuz geliştirme makinenizde çalıştığından, bu kodu çalıştırmak ve hatalarını ayıklamak için kullandığınız geliştirme araçlarında esneklik de vardır. Azure Dev Spaces Connect Ayrıca, geliştirme makinenizdeki AKS kümenizdeki ortam değişkenlerini ve takılı dosyaları bir arada çoğaltmak için bir yol sağlar.

Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:

- Azure 'da yönetilen bir Kubernetes kümesinde Azure Dev Spaces ayarlayın.
- Birden fazla mikro hizmet ile bir geliştirme alanına büyük bir uygulama dağıtın.
- Geliştirme makinenizde çalışan AKS kümeniz ve kodunuz arasında trafiği yeniden yönlendirmek için Azure Dev Spaces Connect ' i kullanın.

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu kılavuz, [Azure dev Spaces bisiklet paylaşımı örnek uygulamasını](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp) kullanarak Azure dev Spaces bir bağlantı gösterir. Yerine kullanmak istediğiniz AKS kümesinde kendi uygulamanız varsa [buradan](#use-azure-dev-spaces-connect)başlayabilirsiniz.

### <a name="limitations"></a>Sınırlamalar

* UDP, Azure Dev Spaces Connect ile Şu anda desteklenmiyor.

### <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.
* [Yüklü Azure CLI][azure-cli].
* [Held 2,13 veya üzeri yüklü][helm-installed].
* MacOS veya Windows 10 ' da yüklü ve çalışan [Azure dev Spaces][azds-vs-code] uzantısı ile [Visual Studio Code][vs-code] .

## <a name="create-an-azure-kubernetes-service-cluster"></a>Azure Kubernetes hizmet kümesi oluşturma

[Desteklenen bir bölgede][supported-regions]aks kümesi oluşturmanız gerekir. Aşağıdaki komutlar *Myresourcegroup* adlı bir kaynak grubu ve *myaks*adlı bir aks kümesi oluşturur.

```cmd
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --disable-rbac --generate-ssh-keys
```

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>AKS kümenizde Azure Dev Spaces etkinleştirme

AKS kümenizde dev alanlarını etkinleştirmek ve istemleri izlemek için `use-dev-spaces` komutunu kullanın. Aşağıdaki komut *Myresourcegroup* grubundaki *myaks* kümesinde dev alanları sunar ve *dev*adlı bir dev alanı oluşturur.

> [!NOTE]
> `use-dev-spaces` komutu, zaten yüklenmemişse Azure Dev Spaces CLı 'yi de yükler. Azure Dev Spaces CLı 'yi Azure Cloud Shell yükleyemezsiniz.

```cmd
az aks use-dev-spaces -g MyResourceGroup -n MyAKS --space dev --yes
```

## <a name="get-sample-application-code"></a>Örnek uygulama kodu al

Bu makalede, Azure Dev Spaces kullanmayı göstermek için [Azure dev Spaces bisiklet paylaşımı örnek uygulamasını](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp) kullanacaksınız.

Uygulamayı GitHub 'dan kopyalayıp dizinine gidin:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/BikeSharingApp/
```

## <a name="retrieve-the-hostsuffix-for-dev"></a>*Geliştirme* Için hostsuffix alma

*Dev*Için hostsuffix öğesini göstermek için `azds show-context` komutunu kullanın.

```cmd
$ azds show-context

Name                ResourceGroup     DevSpace  HostSuffix
------------------  ----------------  --------  -----------------------
MyAKS               MyResourceGroup   dev       fedcab0987.eus.azds.io
```

## <a name="update-the-helm-chart-with-your-hostsuffix"></a>Held grafiğini HostSuffix ile güncelleştirme

[Grafikler/değerler. YAML](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/charts/values.yaml) 'yi açın ve tüm `<REPLACE_ME_WITH_HOST_SUFFIX>` örneklerini daha önce aldığınız hostsuffix değeri ile değiştirin. Değişikliklerinizi kaydedin ve dosyayı kapatın.

## <a name="run-the-sample-application-in-kubernetes"></a>Kubernetes 'te örnek uygulamayı çalıştırma

Kubernetes üzerinde örnek uygulamayı çalıştırmaya yönelik komutlar, var olan bir işlemin parçasıdır ve Azure Dev Spaces araçları üzerinde hiçbir bağımlılığı yoktur. Bu durumda, Held Bu örnek uygulamayı çalıştırmak için kullanılan araçlama, ancak diğer araçlar, tüm uygulamanızı bir küme içindeki bir ad alanında çalıştırmak için kullanılabilir. HELI komutları, daha önce oluşturduğunuz *dev* adlı dev alanını hedeflerken, ancak bu dev Space de bir Kubernetes ad alanıdır. Sonuç olarak, dev Spaces diğer diğer ad alanları ile aynı diğer araç tarafından hedeflenebilir.

Uygulamayı dağıtmak için kullanılan araçları ne olursa olsun, bir kümede çalışmaya başladıktan sonra geliştirme için Azure Dev Spaces kullanabilirsiniz.

### <a name="use-helm-to-install-the-sample-application"></a>Örnek uygulamayı yüklemek için Held kullanma

Kümenize örnek uygulamayı ayarlamak ve yüklemek için `helm init` ve `helm install` komutlarını kullanın.

```cmd
cd charts/
helm init --wait
helm install -n bikesharing . --dep-up --namespace dev --atomic 
```

> [!Note]
> **RBAC özellikli bir küme kullanıyorsanız**, [Tiller için bir hizmet hesabı](https://helm.sh/docs/using_helm/#role-based-access-control)yapılandırmadığınızdan emin olun. Aksi takdirde, `helm` komutları başarısız olur.

`helm install` komutun tamamlanması birkaç dakika sürebilir. Komutun çıktısı tamamlandığında kümeye dağıtıldığı tüm hizmetlerin durumunu gösterir:

```cmd
$ cd charts/
$ helm init --wait
...
Happy Helming!

$ helm install -n bikesharing . --dep-up --namespace dev --atomic

Hang tight while we grab the latest from your chart repositories...
...
NAME               READY  UP-TO-DATE  AVAILABLE  AGE
bikes              1/1    1           1          4m32s
bikesharingweb     1/1    1           1          4m32s
billing            1/1    1           1          4m32s
gateway            1/1    1           1          4m32s
reservation        1/1    1           1          4m32s
reservationengine  1/1    1           1          4m32s
users              1/1    1           1          4m32s
```

### <a name="navigate-to-your-sample-application"></a>Örnek uygulamanıza gidin

Örnek uygulama kümenize yüklendikten sonra ve kümenizde geliştirme alanları etkin olduktan sonra, şu anda seçili olan *dev* 'de örnek uygulamanın URL 'lerini göstermek için `azds list-uris` komutunu kullanın.

```cmd
$ azds list-uris
Uri                                                 Status
--------------------------------------------------  ---------
http://dev.bikesharingweb.fedcab0987.eus.azds.io/  Available
http://dev.gateway.fedcab0987.eus.azds.io/         Available
```

`azds list-uris` komutundan ortak URL 'yi açarak *bıkesharingweb* hizmetine gidin. Yukarıdaki örnekte, *bıkesharingweb* hizmeti IÇIN genel URL `http://dev.bikesharingweb.fedcab0987.eus.azds.io/` ' dir. Kullanıcı olarak *Aurelia Briggs (müşteri)* öğesini seçin ve ardından kiralamak istediğiniz bir bisiklet seçin. Bisiklet için bir yer tutucu görüntüsü gördiğinizi doğrulayın.

## <a name="use-azure-dev-spaces-connect"></a>Azure Dev Spaces Bağlan 'ı kullanma

Visual Studio Code 'de *dev-Spaces/Samples/Bıkesharingapp/Bisiklet* açın ve Azure dev Spaces Connect uzantısını kullanarak geliştirme makinenizi aks kümenize bağlayın.

Azure Dev Spaces Connect uzantısını kullanmak için, *görüntüle* ' ye ve ardından *komut paleti*' ne tıklayarak Visual Studio Code komut paleti ' ni açın. `Azure Dev Spaces: Redirect` yazmaya başlayın ve `Azure Dev Spaces: Redirect an existing Kubernetes service to my machine [Preview]`, `Azure Dev Spaces: Redirect an existing Kubernetes pod to my machine [Preview]`ya da `Azure Dev Spaces: Redirect a new Kubernetes pod to my machine [Preview]`seçeneğine tıklayın.

![Bağlanma komutları](../media/how-to-connect/connect-commands.png)

### <a name="select-a-redirection-option"></a>Yeniden yönlendirme seçeneği seçin

`Azure Dev Spaces: Redirect an existing Kubernetes service to my machine [Preview]`çalıştırırsanız, var olan bir Kubernetes hizmeti seçmeniz istenir:

![Connect hizmet Seç](../media/how-to-connect/connect-choose-service.png)

Bu seçenek, bu hizmet için AKS kümesindeki tüm trafiği, geliştirme makinenizde çalışan uygulamanızın sürümüne yönlendirir. Bu hizmette AKS kümesinde çalışan birden fazla sayıda yer varsa, bu hizmetin tüm trafiği yalnızca geliştirme makinenize yönlendirilir. Azure Dev Spaces Connect Ayrıca, uygulamanın tüm giden trafiğini AKS kümenize geri yönlendirir.

`Azure Dev Spaces: Redirect an existing Kubernetes pod to my machine [Preview]`çalıştırırsanız, belirli bir pod seçmeniz istenir:

![Bağlan Pod seçin](../media/how-to-connect/connect-choose-pod.png)

Bu seçenek, belirli bir pod 'a bağlanır. Bu seçenek, trafik gönderme veya alma ve sonlandırılan Pod 'yi çoğaltma gibi Pod ile etkileşim kurmak için yararlıdır. Pod, trafik gönderiyor ve alıyorsa, bu seçenek `Azure Dev Spaces: Redirect an existing Kubernetes service to my machine [Preview]` benzer bir şekilde davranır ve seçilen Pod hizmeti ile ilgili tüm FID 'ler için AKS kümesindeki tüm trafiği yönlendirecektir.

`Azure Dev Spaces: Redirect a new Kubernetes pod to my machine [Preview]`çalıştırırsanız, var olan bir pod veya hizmet seçmeniz istenmez. Bu seçenek, geliştirme makinenizde çalışan uygulamadaki tüm giden trafiği AKS kümesine yönlendirir.

Bu örnek için `Azure Dev Spaces: Redirect an existing Kubernetes service to my machine [Preview]` seçin ve *Bisiklet* hizmetini seçin.

### <a name="select-a-connection-mode"></a>Bir bağlantı modu seçin

Yeniden yönlendirme seçeneğinizi belirledikten sonra, bağlantıyı *Değiştir* veya *Kopyala* modunu seçmeniz istenir.

![Değiştirme veya kopyalama ile bağlantı](../media/how-to-connect/connect-replace-clone.png)

*Replace* seçeneği, aks kümesindeki geçerli Pod veya hizmetin yerini alır ve bu hizmet için tüm trafiği geliştirme makinenize yönlendirir. Bu seçenek, AKS kümenizde, uygulamayı geliştirme makinenizde başlatana kadar yönlendirireceğiniz hizmetle etkileşimde bulunan diğer hizmetlere yönelik karışıklığa neden olabilir. *Kopyalama* seçeneği, var olan bir alt geliştirme alanını seçmenizi veya bir pod veya hizmet için trafiği geliştirme makinenize yönlendirmek üzere yeni bir alt geliştirme alanı oluşturmanızı sağlar. Bu seçenek, yalnızca söz konusu alt dev alanının trafiği geliştirme makinenize yönlendirilirken, yalıtımta çalışmanıza ve diğer hizmetleri kesintiye uğramayacak şekilde izin verir. *Kopyalama* seçeneği, aks kümenizin Azure dev Spaces etkinleştirilmesini gerektirir.

Bu örnek için *Değiştir*' i seçin.

> [!NOTE]
> Var olan hizmetinizin Pod 'ınızda birden çok kapsayıcı varsa, uygulamanın kapsayıcısını de seçmeniz istenir.

### <a name="select-a-port-for-your-application"></a>Uygulamanız için bir bağlantı noktası seçin

Bağlantı kipini seçtikten sonra, yerel uygulamanızın TCP bağlantı noktasını girmeniz istenir. Uygulamanız birden çok bağlantı noktası açarsa, bunları virgülle ayırın, örneğin *80, 81*. Uygulamanız herhangi bir ağ isteğini kabul etmezse *0*girin. Bu örnek için *8080*girin.

![Bağlan bağlantı noktası seç](../media/how-to-connect/connect-choose-port.png)

### <a name="confirm-you-are-connected"></a>Bağlı olduğunu onaylayın

Uygulamanızın TCP bağlantı noktasını seçtikten sonra, Azure Dev Spaces Connect AKS kümesiyle bağlantı kurar. Azure Dev Spaces Connect, AKS kümesi ile geliştirme makineniz arasında trafiği yeniden yönlendirmek için bir aracıyı AKS kümenize çıkarır. Bu bağlantının oluşturulması birkaç dakika sürebilir. Azure Dev Spaces Connect, geliştirme makinenizdeki *Hosts* dosyasını değiştirmek için yönetici erişimi de ister.

> [!IMPORTANT]
> Azure Dev Spaces Connect, AKS kümenize bir bağlantı kurduğunda, geliştirme makinenizde hizmeti başlatana kadar AKS kümenizdeki diğer hizmetler düzgün çalışmayabilir. Ayrıca, hizmetinizin geliştirme makinenizde kullanılamayan bir bağımlılığı varsa, uygulamanızı değiştirmeniz veya [ek yapılandırma](#additional-configuration) sağlamanız gerekebilir

Azure Dev Spaces Connect, AKS kümenize bir bağlantı kurduktan sonra *azds Connect-Bisiklet* adlı bir Terminal penceresi açar. Bu Terminal penceresinde, AKS kümenizde yapılandırılan tüm ortam değişkenleri ve DNS girişleri bulunur. Bu Terminal penceresinde çalıştırdığınız veya Visual Studio Code hata ayıklayıcıyı kullanan herhangi bir kod AKS kümesine bağlıdır.

![Terminal Connect](../media/how-to-connect/connect-terminal.png)

Ayrıca, Azure Dev Spaces Connect, tüm çıktılarına göre *dev Spaces Connect* adlı bir pencere oluşturur.

![Bağlantı çıkışı](../media/how-to-connect/connect-output.png)

Azure Dev Spaces Connect 'in ayrıca bağlantı durumunu gösteren bir durum çubuğu öğesi vardır.

![Bağlantı durumu](../media/how-to-connect/connect-status.png)

Durum çubuğunun *dev alanlarını gösterdiğini doğrulayın: yerel bağlantı noktası 8080 üzerinde geliştirme/Bisiklet 'e bağlanıldı*.

### <a name="configure-your-application-on-your-development-machine"></a>Uygulamanızı geliştirme makinenizde yapılandırma

*Azds Connect-Bisiklet* Terminal penceresini açın ve `npm install`çalıştırın:

```console
$ npm install

> fsevents@1.2.9 install ./dev-spaces/samples/BikeSharingApp/Bikes/node_modules/fsevents
> node install
...
```

[Server. js](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/Bikes/server.js#L352) ve Line 352 güncelleştirme *bağlantı noktasını* *8080*' de açın:

```javascript
var port = 8080;
var server = null;
```

*Hata Ayıkla* ' ya tıkladıktan sonra yapılandırma ' yı *açın* Bu, bir `.vscode/launch.json` dosyası oluşturur. Bu dosyanın içeriğini aşağıdaki kodla değiştirin:

```json
{
    "configurations": [
        ...
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

[Package. JSON](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/Bikes/package.json) dosyasını açın ve bir hata ayıklama betiği ekleyin:

```json
  "devDependencies": {
    "nodemon": "^1.18.10"
  },
  "scripts": {
    "debug": "node --nolazy --inspect-brk=9229 server.js"
  }
```

### <a name="start-your-application-on-your-development-machine"></a>Uygulamanızı geliştirme makinenizde başlatın

Sol taraftaki *hata ayıklama* simgesine tıklayın ve en üstte *NPM aracılığıyla* Başlat ' ın yanındaki Başlat düğmesine tıklayın.

![NPM aracılığıyla Başlat](../media/how-to-connect/launch-npm.png)

Uygulamanız başlar ve Azure Dev Spaces Connect, AKS kümeniz ile geliştirme makineniz arasındaki trafiği yeniden yönlendirir. *Hata ayıklama konsolunda*aşağıdakine benzer iletiler görürsünüz:

```console
/usr/local/bin/npm run-script debug 
...
Debugger attached.
Collection: bikes
MongoDB connection string: mongodb://databases-mongo
Connected to MongoDB
Listening on port 8080
```

Azure Dev Spaces Connect durum çubuğuna tıklayıp uygulamanızın genel URL 'sini seçerek *bıkesharingweb* hizmetine gidin. Ayrıca, daha önce çalıştırdığınız `azds list-uris` komutundan genel URL 'YI bulabilirsiniz. Kümenizde Azure Dev Spaces kullanmıyorsanız, kullandığınız ad alanı için IP veya uygulamanın URL 'sini kullanın. Yukarıdaki örnekte, *bıkesharingweb* hizmeti IÇIN genel URL `http://dev.bikesharingweb.fedcab0987.eus.azds.io/` ' dir. Kullanıcı olarak *Aurelia Briggs (müşteri)* öğesini seçin ve ardından kiralamak istediğiniz bir bisiklet seçin.

### <a name="set-a-break-point"></a>Kesme noktası ayarlama

[Sunucu. js](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/Bikes/server.js#L233) ' yi açın ve imlecinizi buraya yerleştirmek için satır 233 ' de bir yere tıklayın. *F9* tuşuna basarak bir kesme noktası ayarlayın veya *Hata Ayıkla* 'Yı tıklattıktan sonra *kesme noktasını değiştirin*.

Genel URL 'YI açarak *bıkesharingweb* hizmetine gidin. Kullanıcı olarak *Aurelia Briggs (müşteri)* öğesini seçin ve ardından kiralamak istediğiniz bir bisiklet seçin. Bisiklet görüntüsünün yüklenmediğine dikkat edin. Visual Studio Code dön ve 233 satırını gözlemle vurgulanır. Ayarladığınız kesme noktası 233 satırındaki hizmeti duraklattı. Hizmeti sürdürmek için *F5* 'e basın veya *Hata Ayıkla* 'Ya tıkladıktan sonra *devam edin*. Tarayıcınıza dönün ve bisiklet için bir yer tutucu görüntüsü gördiğinizi doğrulayın.

İmleci `server.js` satır 233 ' ye yerleştirerek ve *F9*tuşuna basarak kesme noktasını kaldırın.

### <a name="update-your-application"></a>Uygulamanızı güncelleştirme

232 ve 233 satırlarını kaldırmak için `server.js` düzenleyin:

```javascript
    // Hard code image url *FIX ME*
    theBike.imageUrl = "/static/logo.svg";
```

Bölüm şu şekilde görünmelidir:

```javascript
    var theBike = result;
    theBike.id = theBike._id;
    delete theBike._id;
```

Değişikliklerinizi kaydedin ve *Hata Ayıkla* ' ya tıkladıktan sonra *hata ayıklamayı yeniden başlatın*. Tarayıcınızı yenileyin ve bisiklet için artık bir yer tutucu görüntüsü görmediğinizi doğrulayın.

Hata ayıklayıcıyı durdurmak için hata *Ayıkla* ve hata *ayıklamayı Durdur* ' a tıklayın. AKS kümesi bağlantısını kesmek için Azure Dev Spaces Connect durum çubuğuna tıklayın.

## <a name="additional-configuration"></a>Ek yapılandırma

Azure Dev Spaces Connect, yönlendirme trafiğini işleyebilir ve ek yapılandırma olmadan ortam değişkenlerini çoğaledebilir. Bir ConfigMap dosyası gibi AKS kümenizdeki kapsayıcıya bağlanmış herhangi bir dosyayı indirmeniz gerekiyorsa, bu dosyaları geliştirme makinenize indirmek için bir `azds-local.env` oluşturabilirsiniz.

Örnek `azds-local.env`aşağıda verilmiştir:

```
# This downloads the "whitelist" volume from the container,
# saves it to a temporary directory on your development machine,
# and sets the full path to an environment variable called WHITELIST_PATH.

WHITELIST_PATH=${volumes.whitelist}/whitelist

# This downloads a file from the container's 'default-token-<any>' mount directory 
# to /var/run/secrets/kubernetes.io/serviceaccount on your development machine.

KUBERNETES_IN_CLUSTER_CONFIG_OVERWRITE=${volumes.default-token-*|/var/run/secrets/kubernetes.io/serviceaccount}


# This makes the myapp1 service available to your development machine
# regardless of the AKS cluster you are connected to and
# sets the local IP to an environment variable called MYAPP1_SERVICE_HOST.

MYAPP1_SERVICE_HOST=${services.myapp1}

# If the myapp1 service is made available in this way, 
# you can also access it using "myapp1" and "myapp1.svc.cluster.local"
# in addition to the IP in the MYAPP1_SERVICE_HOST environment variable.
```

## <a name="next-steps"></a>Sonraki adımlar

Çekme isteği deponuzun ana dalında birleştirilmeden önce doğrudan AKS 'teki bir çekme isteğinden yapılan değişiklikleri test etmek için Azure Dev Spaces ve GitHub eylemlerini nasıl kullanacağınızı öğrenin.

> [!div class="nextstepaction"]
> [Azure Kubernetes hizmeti & GitHub eylemleri][gh-actions]

[azds-vs-code]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[azure-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[bike-sharing-github]: https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp
[gh-actions]: github-actions.md
[helm-installed]: https://helm.sh/docs/using_helm/#installing-helm
[supported-regions]: ../about.md#supported-regions-and-configurations
[team-quickstart]: ../quickstart-team-development.md
[vs-code]: https://code.visualstudio.com/download