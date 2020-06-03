---
title: Visual Studio Code (Önizleme) ile Kubernetes ile yerel Işlem kullanma
services: azure-dev-spaces
ms.date: 06/02/2020
ms.topic: conceptual
description: Geliştirme bilgisayarınızı bir Kubernetes kümesine bağlamak için Kubernetes ile yerel Işlemi nasıl kullanacağınızı öğrenin Azure Dev Spaces
keywords: Kubernetes, Azure Dev Spaces, dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar ile yerel Işlem
ms.openlocfilehash: 23a94528ffa4e9e412f472349ea26d1a14003616
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84316743"
---
# <a name="use-local-process-with-kubernetes-with-visual-studio-code-preview"></a>Visual Studio Code (Önizleme) ile Kubernetes ile yerel Işlem kullanma

Kubernetes ile yerel Işlem, geliştirme bilgisayarınızda kod çalıştırmanıza ve hata ayıklamanıza karşın uygulamanızın veya hizmetlerinizin geri kalanı ile Kubernetes kümenize bağlı olmaya devam etmenize olanak tanır. Örneğin, birbirine bağlı çok sayıda hizmet ve veritabanına sahip büyük bir mikro hizmet mimariniz varsa, bu bağımlılıkların geliştirme bilgisayarınızda çoğaltılması zor olabilir. Ayrıca, iç döngü geliştirme sırasında her kod değişikliği için Kubernetes kümenize kod oluşturup dağıtmak, bir hata ayıklayıcıyla yavaş, zaman alabilir ve kullanılması zor olabilir.

Kubernetes ile yerel Işlem, doğrudan geliştirme bilgisayarınız ve kümeniz arasında bir bağlantı oluşturmak için kodunuzu kümenize derleyip dağıtmak zorunda kalmaktan kaçınır. Hata ayıklama sırasında geliştirme bilgisayarınızı kümenize bağlamak, herhangi bir Docker veya Kubernetes yapılandırması oluşturmadan hizmetinizi tam uygulama bağlamında hızlıca test etmenize ve geliştirmenize olanak sağlar.

Kubernetes ile yerel Işlem, bağlı Kubernetes kümeniz ile geliştirme bilgisayarınız arasında trafiği yeniden yönlendirir. Bu trafik yeniden yönlendirmesi, Kubernetes kümenizde çalışan geliştirme ve hizmetinizdeki kodların aynı Kubernetes kümesinde olduklarından farklı iletişim kurmasına olanak tanır. Kubernetes ile yerel Işlem, geliştirme bilgisayarınızda Kubernetes kümenizde bulunan ortam değişkenlerini ve takılı birimleri de çoğaltmak için bir yol sağlar. Geliştirme bilgisayarınızda ortam değişkenlerine ve bağlı birimlere erişim sağlamak, bu bağımlılıkları el ile çoğaltmadan kodunuzda hızla çalışmanıza olanak sağlar.

Bu kılavuzda, Kubernetes ile yerel Işlem kullanarak, geliştirme bilgisayarınızda çalışan Kubernetes kümeniz ve kodunuz arasında trafiği yeniden yönlendirme hakkında bilgi edineceksiniz. Bu kılavuz Ayrıca, bir Kubernetes kümesinde birden fazla mikro hizmet içeren büyük bir örnek uygulama dağıtmaya yönelik bir betik sağlar.

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını][preview-terms] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu kılavuz, geliştirme bilgisayarınızı bir Kubernetes kümesine bağlamayı göstermek için [Azure dev Spaces bisiklet paylaşımı örnek uygulamasını][bike-sharing-github] kullanır. Zaten bir Kubernetes kümesinde çalışan bir uygulamanız varsa, aşağıdaki adımları izleyerek kendi hizmetlerinizin adlarını da kullanabilirsiniz.

### <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.
* [Yüklü Azure CLI][azure-cli].
* MacOS veya Windows 10 ' da çalışan [Visual Studio Code][vs-code] .
* Visual Studio Code [Azure dev Spaces][azds-vs-code] uzantısı sürümü 2.0.220200601 veya üzeri yüklendi.
* [Azure dev Spaces CLI yüklendi][azds-cli].

## <a name="create-a-kubernetes-cluster"></a>Kubernetes kümesi oluşturma

[Desteklenen bir bölgede][supported-regions]aks kümesi oluşturma. Aşağıdaki komutlar *Myresourcegroup* adlı bir kaynak grubu ve *myaks*adlı bir aks kümesi oluşturur.

```azurecli-interactive
az group create \
    --name MyResourceGroup \
    --location eastus

az aks create \
    --resource-group MyResourceGroup \
    --name MyAKS \
    --location eastus \
    --node-count 3 \
    --generate-ssh-keys
```

## <a name="install-the-sample-application"></a>Örnek uygulamayı yükler

Örnek uygulamayı, belirtilen betiği kullanarak kümenize yükler. Bu betiği, geliştirme bilgisayarınızda veya [Azure Cloud Shell][azure-cloud-shell]kullanarak çalıştırabilirsiniz.

> [!IMPORTANT]
> Betiği çalıştırmak için kümenize *sahip* veya *katkıda bulunan* erişiminizin olması gerekir.

```azurecli-interactive
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/
chmod +x ./local-process-quickstart.sh
./local-process-quickstart.sh -g MyResourceGroup -n MyAKS
```

Yükleme komut dosyasının çıktısında görüntülenecek olan genel URL 'sini açarak kümenizi çalıştıran örnek uygulamaya gidin.

```console
$ ./local-process-quickstart.sh -g MyResourceGroup -n MyAKS
Defaulting Dev spaces repository root to current directory : ~/dev-spaces
Setting the Kube context
...
To try out the app, open the url:
dev.bikesharingweb.EXTERNAL_IP.nip.io
```

Yukarıdaki örnekte, genel URL olur `dev.bikesharingweb.EXTERNAL_IP.nip.io` .

## <a name="connect-to-your-cluster-and-debug-a-service"></a>Kümenize bağlanın ve bir hizmette hata ayıklayın

Geliştirme bilgisayarınızda, [az aks Get-Credentials][az-aks-get-credentials]komutunu kullanarak Kubernetes ' e bağlanmak Için KUBERNETES CLI indirin ve yapılandırın.

```azurecli
az aks get-credentials --resource-group MyResourceGroup --name MyAKS
```

Visual Studio Code ' de [Azure dev Spaces bisiklet paylaşımı örnek uygulamasından][bike-sharing-github] *dev-Spaces/Samples/Bıkesharingapp/Bisiklet* ' i açın. Azure Kubernetes hizmeti uzantısını açın ve *Myaks* kümesinde *dev* ad alanını seçin.

![Ad alanı seç](../media/local-process-kubernetes-vs-code/select-namespace.png)

`npm install`Uygulamanın bağımlılıklarını yüklemek için komutunu kullanın.

```console
npm install
```

Sol taraftaki *hata ayıklama* simgesini seçin ve en üstteki *Kubernetes (Önizleme) ile yerel işlem* ' ı seçin.

![Kubernetes ile yerel Işlem seçin](../media/local-process-kubernetes-vs-code/choose-local-process.png)

*Kubernetes (Önizleme) Ile yerel işlem*' in yanındaki Başlat düğmesine tıklayın. Bu başlatma yapılandırmasını ilk kez çalıştırdığınızda, değiştirmek istediğiniz hizmeti, geliştirme bilgisayarınızdan iletmek için kullanılacak bağlantı noktasını ve kullanılacak başlatma görevini yapılandırmanız istenir.

*Bisiklet* hizmetini seçin.

![Hizmet seçin](../media/local-process-kubernetes-vs-code/choose-service.png)

Kubernetes kümesindeki tüm trafik, *Bisiklet* servisi için geliştirme bilgisayarınızda çalışan uygulamanızın sürümüne yeniden yönlendirilir. Kubernetes ile yerel Işlem, uygulamanın tüm giden trafiğini Kubernetes kümenize geri yönlendirir.

> [!IMPORTANT]
> Yalnızca tek bir pod içeren hizmetleri yeniden yönlendirebilirsiniz.

Hizmetinizi seçtikten sonra, yerel uygulamanız için TCP bağlantı noktasını girmeniz istenir. Bu örnek için *3000*girin.

![Bağlan bağlantı noktası seç](../media/local-process-kubernetes-vs-code/choose-port.png)

Başlatma görevi olarak *NPM aracılığıyla Başlat* ' ı seçin.

![Bağlan başlatma görevi seçin](../media/local-process-kubernetes-vs-code/choose-launch.png)

> [!NOTE]
> *Kubernetesdnsmanager* 'ın yükseltilmiş çalışmasına ve hosts dosyanızı değiştirmesine izin vermeniz istenir.

Geliştirme Bilgisayarınız, durum çubuğu turuncu kapandığında ve dev Spaces uzantısının bağlı olduğunu gösterdiği zaman bağlanır.

![Geliştirme bilgisayarı bağlı](../media/local-process-kubernetes-vs-code/development-computer-connected.png)

> [!NOTE]
> Alt görevler başlatıldığında hizmet adı, bağlantı noktası veya başlatma görevi istenmez. Bu değerler *. vscode/Tasks. JSON*içine kaydedilir.

Geliştirme Bilgisayarınız bağlandıktan sonra, değiştirmek istediğiniz hizmet için trafik geliştirme bilgisayarınıza yeniden yönlendirmeye başlar.

## <a name="set-a-break-point"></a>Kesme noktası ayarlama

[Sunucu. js][server-js-breakpoint] ' yi açın ve imlecinizi buraya yerleştirmek için satır 233 ' de bir yere tıklayın. *F9* tuşuna basarak bir kesme noktası ayarlayın veya *Çalıştır* ' a tıkladıktan sonra *kesme noktasını değiştirin*.

Ortak URL 'YI açarak örnek uygulamaya gidin. Kullanıcı olarak *Aurelia Briggs (müşteri)* öğesini seçin ve ardından kiralamak istediğiniz bir bisiklet seçin. Bisiklet görüntüsünün yüklenmediğine dikkat edin. Visual Studio Code dön ve 233 satırını gözlemle vurgulanır. Ayarladığınız kesme noktası 233 satırındaki hizmeti duraklattı. Hizmeti sürdürmek için *F5* tuşuna basın veya *Çalıştır* ' a tıkladıktan sonra *devam edin*. Tarayıcınıza dönün ve bisiklet için bir yer tutucu görüntüsü gördiğinizi doğrulayın.

İmlecinizi 233. satıra yerleştirerek `server.js` ve *F9*tuşuna basarak kesme noktasını kaldırın.

### <a name="update-your-application"></a>Uygulamanızı güncelleştirme

`server.js`234 ve 235 satırlarını kaldırmak için Düzenle:

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

Değişikliklerinizi kaydedin ve *Çalıştır* ' a tıklayın ve ardından *hata ayıklamayı yeniden başlatın*. Yeniden bağlandıktan sonra, tarayıcınızı yenileyin ve bisiklet için bir yer tutucu görüntüsü görmediğinizi doğrulayın.

Hata ayıklayıcıyı durdurmak için *Çalıştır* ' a tıklayın ve *hata ayıklamayı durdurun* .

> [!NOTE]
> Varsayılan olarak, hata ayıklama görevinin durdurulması geliştirme bilgisayarınızı Kubernetes kümenizdeki bağlantısını da keser. Bu davranışı, Visual Studio Code ayarlarında *hata ayıkladıktan sonra, Kubernetes: Disconnect Ile yerel işlemi* arayarak ve *hata ayıklama sona erdiğinde bağlantıyı otomatik olarak kes* seçeneğinin yanındaki denetimi kaldırarak değiştirebilirsiniz. Bu ayar güncelleştirildikten sonra, hata ayıklamayı durdurup başlattığınızda geliştirme Bilgisayarınız bağlı kalır. Geliştirme bilgisayarınızın kümenizdeki bağlantısını kesmek için durum çubuğundaki Azure Dev Spaces uzantısına tıklayın ve *geçerli oturumun bağlantısını kes*' i seçin.
>
> Visual Studio Code, kümeye olan bağlantıyı aniden sonlandırıyorsa veya sonlandırıyorsa, Kubernetes ile yerel Işleme bağlanmadan önce yeniden yönlendiriyorsunuz hizmeti özgün durumuna geri yüklenemez. Bu sorunu gidermek için [sorun giderme kılavuzu][troubleshooting]'na bakın.

## <a name="using-logging-and-diagnostics"></a>Günlüğe kaydetme ve tanılama kullanma

Kayıt çıktısı, geliştirme Bilgisayarınız Kubernetes kümenize bağlandıktan sonra *dev Spaces* penceresine yazılır.

![Çıktı](../media/local-process-kubernetes-vs-code/output.png)

Azure Dev Spaces durum çubuğuna tıklayın ve *bağlantı tanılama bilgilerini göster*' i seçin. Bu komut, günlük çıktısında geçerli ortam değişkenlerini ve DNS değişkenlerini yazdırır.

![Tanılama ile çıkış](../media/local-process-kubernetes-vs-code/output-diagnostics.png)

Ayrıca, tanılama günlüklerini `Azure Dev Spaces` [geliştirme bilgisayarınızın *geçici* dizinindeki][azds-tmp-dir]dizinde bulabilirsiniz.

## <a name="remove-the-sample-application-from-your-cluster"></a>Örnek uygulamayı kümeinizden kaldırma

Örnek uygulamayı kümeinizden kaldırmak için, belirtilen betiği kullanın.

```azurecli-interactive
./local-process-quickstart.sh -c -g MyResourceGroup -n MyAKS
```

## <a name="next-steps"></a>Sonraki adımlar

Çekme isteği deponuzun ana dalında birleştirilmeden önce doğrudan AKS 'teki bir çekme isteğinden yapılan değişiklikleri test etmek için Azure Dev Spaces ve GitHub eylemlerini nasıl kullanacağınızı öğrenin.

> [!div class="nextstepaction"]
> [Azure Kubernetes hizmeti & GitHub eylemleri][gh-actions]

[azds-cli]: install-dev-spaces.md#install-the-client-side-tools
[azds-tmp-dir]: ../troubleshooting.md#before-you-begin
[azds-vs-code]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[azure-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[azure-cloud-shell]: ../../cloud-shell/overview.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-vs-code]: https://marketplace.visualstudio.com/items?itemName=ms-kubernetes-tools.vscode-aks-tools
[bike-sharing-github]: https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp
[gh-actions]: github-actions.md
[preview-terms]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[server-js-breakpoint]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/Bikes/server.js#L233
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[troubleshooting]: ../troubleshooting.md#fail-to-restore-original-configuration-of-deployment-on-cluster
[vs-code]: https://code.visualstudio.com/download