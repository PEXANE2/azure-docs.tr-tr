---
title: Kubernetes'te takım geliştirme
services: azure-dev-spaces
ms.date: 01/22/2020
ms.topic: quickstart
description: Bu hızlı başlangıç, Azure Dev Spaces ile kapsayıcılar ve mikro hizmetlerle Kubernetes'in takım geliştirmesini nasıl yapacağınızı gösterir
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, Miğfer, servis kafesi, servis örgü yönlendirme, kubectl, k8s
manager: gwallace
ms.openlocfilehash: 0fe177db420913e5d68807dd803df791653c0914
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78244951"
---
# <a name="quickstart-team-development-on-kubernetes---azure-dev-spaces"></a>Quickstart: Kubernetes'te takım geliştirme - Azure Dev Spaces

Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:

- Azure'da yönetilen bir Kubernetes kümesinde Azure Dev Alanları'nı ayarlayın.
- Bir dev alana birden çok mikro hizmet içeren büyük bir uygulama dağıtın.
- Tam uygulama bağlamında yalıtılmış bir dev alanda tek bir microservice test edin.

![Azure Dev Spaces takım geliştirme](media/azure-dev-spaces/collaborate-graphic.gif)

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.
- [Yüklü Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).
- [Helm 3 yüklü][helm-installed].

## <a name="create-an-azure-kubernetes-service-cluster"></a>Azure Kubernetes Hizmet kümesi oluşturma

Desteklenen bir [bölgede][supported-regions]bir AKS kümesi oluşturmanız gerekir. Aşağıdaki komutlar *MyResourceGroup* adında bir kaynak grubu ve *MyAKS*adlı bir AKS kümesi oluşturur.

```azurecli
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --generate-ssh-keys
```

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>AKS kümenizde Azure Dev Alanları'nı etkinleştirme

AKS `use-dev-spaces` kümenizde Dev Spaces'i etkinleştirmek ve istemleri izlemek için komutu kullanın. Aşağıdaki *komut, MyResourceGroup* grubunda *MyAKS* kümesindeki Dev Spaces'i etkinleştiriyor ve dev adı verilen bir *dev*alanı oluşturuyor.

> [!NOTE]
> Komut, `use-dev-spaces` zaten yüklü değilse Azure Dev Spaces CLI'yi de yükler. Azure Geliştirme Alanları CLI'sini Azure Bulut Kabuğu'na yükleyemezsiniz.

```azurecli
az aks use-dev-spaces -g MyResourceGroup -n MyAKS --space dev --yes
```

## <a name="get-sample-application-code"></a>Örnek uygulama kodu alın

Bu makalede, Azure [Dev](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp) Spaces kullanarak göstermek için Azure Dev Spaces Bike Paylaşımı örnek uygulamasını kullanırsınız.

Uygulamayı GitHub'dan klonla ve dizinine gidin:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/BikeSharingApp/
```

## <a name="retrieve-the-hostsuffix-for-dev"></a>*Dev* için HostSuffix'i alın

Dev `azds show-context` için HostSuffix göstermek *dev*için komutu kullanın.

```cmd
$ azds show-context

Name                ResourceGroup     DevSpace  HostSuffix
------------------  ----------------  --------  -----------------------
MyAKS               MyResourceGroup   dev       fedcab0987.eus.azds.io
```

## <a name="update-the-helm-chart-with-your-hostsuffix"></a>HostSuffix'inizle Miğfer grafiğini güncelleştirin

[Grafikleri/values.yaml'ı](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/charts/values.yaml) açın ve `<REPLACE_ME_WITH_HOST_SUFFIX>` daha önce aldığınız HostSuffix değeriyle tüm örnekleri değiştirin. Değişikliklerinizi kaydedin ve dosyayı kapatın.

## <a name="run-the-sample-application-in-kubernetes"></a>Örnek uygulamayı Kubernetes'te çalıştırın

Örnek uygulamayı Kubernetes'te çalıştırmak için komutlar varolan bir işlemin parçasıdır ve Azure Dev Spaces aracına bağımlı değildir. Bu durumda, Helm bu örnek uygulamayı çalıştırmak için kullanılan araçtır, ancak diğer araçlar tüm uygulamanızı bir küme içinde bir ad alanında çalıştırmak için kullanılabilir. Helm komutları daha önce oluşturduğunuz dev adlı *dev* alanını hedeflemektedir, ancak bu dev alanı aynı zamanda bir Kubernetes ad alanıdır. Sonuç olarak, dev boşluklar diğer ad alanlarıyla aynı diğer araçlar tarafından hedeflenebilir.

Bir uygulama bir kümede çalıştırıladıktan sonra, onu dağıtmak için kullanılan araçtan bağımsız olarak, ekip geliştirme için Azure Dev Spaces'i kullanabilirsiniz.

Örnek `helm install` uygulamayı kümenize kurmak ve yüklemek için komutu kullanın.

```cmd
cd charts/
helm install bikesharingsampleappsampleapp . --dependency-update --namespace dev --atomic
```

Komutun `helm install` tamamlanması birkaç dakika sürebilir. Örnek uygulama kümenize yüklendikten ve kümenizde Dev Spaces etkinleştirildiğinden, `azds list-uris` şu anda seçili olan *örnek* uygulamanın URL'lerini göstermek için komutu kullanın.

```cmd
$ azds list-uris
Uri                                                 Status
--------------------------------------------------  ---------
http://dev.bikesharingweb.fedcab0987.eus.azds.io/  Available
http://dev.gateway.fedcab0987.eus.azds.io/         Available
```

Genel URL'yi komuttan açarak *bikesharingweb* hizmetine `azds list-uris` gidin. Yukarıdaki örnekte, *bikesharingweb* hizmetinin genel `http://dev.bikesharingweb.fedcab0987.eus.azds.io/`URL'si . Kullanıcı olarak *Aurelia Briggs'i (müşteri)* seçin. Merhaba Aurelia Briggs metnini gördüğünüzü doğrulayın *| *En üstte çıkış ol.

![Azure Dev Spaces Bisiklet Paylaşımı örnek uygulaması](media/quickstart-team-development/bikeshare.png)

## <a name="create-child-dev-spaces"></a>Alt geliştirme alanları oluşturma

Dev `azds space select` altında iki alt boşluk *dev*oluşturmak için komutu kullanın:

```cmd
azds space select -n dev/azureuser1 -y
azds space select -n dev/azureuser2 -y
```

Yukarıdaki *komutlar, azureuser1* ve *azureuser2* adlı dev altında iki alt boşluk oluşturur. *azureuser2* Bu iki alt alan, geliştiriciler için *azureuser1* ve *azureuser2* için örnek uygulamada değişiklik yapmak için kullanılacak farklı geliştirme alanlarını temsil etmektedir.

Tüm `azds space list` dev alanlarını listelemek ve *dev/azureuser2'nin* seçildiğini onaylamak için komutu kullanın.

```cmd
$ azds space list
   Name            DevSpacesEnabled
-  --------------  ----------------
   default         False
   dev             True
   dev/azureuser1  True
*  dev/azureuser2  True
```

`azds list-uris` Örnek uygulamanın URL'lerini *dev/azureuser2*olarak seçili alanda görüntülemek için kullanın.

```cmd
$ azds list-uris
Uri                                                             Status
--------------------------------------------------              ---------
http://azureuser2.s.dev.bikesharingweb.fedcab0987.eus.azds.io/  Available
http://azureuser2.s.dev.gateway.fedcab0987.eus.azds.io/         Available
```

`azds list-uris` Komut tarafından görüntülenen URL'lerin *azureuser2.s.dev* önekine sahip olduğunu doğrulayın. Bu önek, seçilen geçerli alanın *dev'in*bir alt nedeni olan *azureuser2*olduğunu doğrular.

Komutu genel URL'yi açarak *dev/azureuser2* dev alanı için `azds list-uris` *bikesharingweb* hizmetine gidin. Yukarıdaki örnekte, *bikesharingweb* hizmetinin genel `http://azureuser2.s.dev.bikesharingweb.fedcab0987.eus.azds.io/`URL'si . Kullanıcı olarak *Aurelia Briggs'i (müşteri)* seçin. Merhaba Aurelia Briggs metnini gördüğünüzü doğrulayın *| *En üstte oturum aç.

## <a name="update-code"></a>Kodu güncelleştirme

Bir metin düzenleyicisi ile *BikeSharingWeb/components/Header.js'i* açın ve [ `userSignOut` sınıf Adı ile yayılma öğesindeki metni değiştirin.](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/components/Header.js#L16)

```html
<span className="userSignOut">
    <Link href="/devsignin"><span tabIndex="0">Welcome {props.userName} | Sign out</span></Link>
</span>
```

Değişikliklerinizi kaydedin ve dosyayı kapatın.

## <a name="build-and-run-the-updated-bikesharingweb-service-in-the-devazureuser2-dev-space"></a>*Geliştirme/azureuser2 dev* alanında güncelleştirilmiş bikesharingweb hizmetini oluşturun ve çalıştırın

*BikeSharingWeb/* dizinine gidin ve `azds up` komutu çalıştırın.

```cmd
$ cd ../BikeSharingWeb/
$ azds up

Using dev space 'dev/azureuser2' with target 'MyAKS'
Synchronizing files...2s
...
Service 'bikesharingweb' port 'http' is available at http://azureuser2.s.dev.bikesharingweb.fedcab0987.eus.azds.io/
Service 'bikesharingweb' port 80 (http) is available at http://localhost:54256
...
```

Bu *komut, dev/azureuser2 dev* alanında *bikesharingweb* hizmetini oluşturur ve çalıştırUr. Bu *hizmet, dev'de* çalışan *bikesharingweb* hizmetine ek olarak çalışır ve yalnızca *azureuser2.s* URL önekiyle gelen istekler için kullanılır. Yönlendirmenin üst ve alt geliştirme alanları arasında nasıl çalıştığı hakkında daha fazla bilgi için Azure [Dev Alanları'nın nasıl çalıştığı ve nasıl yapılandırıldığı](how-dev-spaces-works.md)nabakını öğrenin.

`azds up` Komutun çıktısında görüntülenen genel URL'yi açarak *dev/azureuser2* dev alanı için *bikesharingweb* hizmetine gidin. Kullanıcı olarak *Aurelia Briggs'i (müşteri)* seçin. Sağ üst köşede güncelleştirilmiş metni gördüğünüzü doğrulayın. Bu değişikliği hemen göremiyorsanız sayfayı yenilemeniz veya tarayıcınızın önbelleğini temizlemeniz gerekebilir.

![Azure Dev Spaces Bisiklet Paylaşımı örnek uygulaması güncellendi](media/quickstart-team-development/bikeshare-update.png)

> [!NOTE]
> Çalışırken `azds up`hizmetinize gidince, HTTP istek izleri de `azds up` komutun çıktısında görüntülenir. Bu izlemeler, hizmetinizi sorun gidermenize ve hata ayıklamanıza yardımcı olabilir. Çalışırken bu izleri kullanarak `--disable-http-traces` devre dışı `azds up`kullanabilirsiniz.

## <a name="verify-other-dev-spaces-are-unchanged"></a>Diğer Dev Boşlukların değişmediğini doğrulayın

Komut `azds up` hala çalışıyorsa *Ctrl+c*tuşuna basın.

```cmd
$ azds list-uris --all
Uri                                                             Status
--------------------------------------------------              ---------
http://azureuser1.s.dev.bikesharingweb.fedcab0987.eus.azds.io/  Available
http://azureuser1.s.dev.gateway.fedcab0987.eus.azds.io/         Available
http://azureuser2.s.dev.bikesharingweb.fedcab0987.eus.azds.io/  Available
http://azureuser2.s.dev.gateway.fedcab0987.eus.azds.io/         Available
http://dev.bikesharingweb.fedcab0987.eus.azds.io/               Available
http://dev.gateway.fedcab0987.eus.azds.io/                      Available
```

Tarayıcınızda *bikesharingweb'in* *dev* sürümüne gidin, kullanıcı olarak *Aurelia Briggs'i (müşteri)* seçin ve sağ üst köşedeki orijinal metni gördüğünüzü doğrulayın. Bu adımları *dev/azureuser1* URL'si ile tekrarlayın. Değişikliklerin yalnızca *bikesharingweb'in* *dev/azureuser2* sürümüne uygulandığına dikkat edin. *Dev/azureuser2'deki* değişikliklerin bu yalıtımı *azureuser2'nin* *azureuser1'i*etkilemeden değişiklik yapmasına olanak tanır.

Bu değişikliklerin *dev* ve *dev/azureuser1'e*yansıtılmış olması için, ekibinizin varolan iş akışını veya CI/CD ardışık hattını izlemeniz gerekir. Örneğin, bu iş akışı, sürüm denetim sisteminizde değişiklik yapmayı ve güncelleştirmeyi bir CI/CD ardışık hattını veya Helm gibi takım ları kullanarak dağıtmayı içerebilir.

## <a name="clean-up-your-azure-resources"></a>Azure kaynaklarınızı temizleme

```azurecli
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Dev Spaces uygulamasının birden fazla kapsayıcı arasında daha karmaşık uygulamalar geliştirmenize nasıl yardımcı olduğunu ve farklı alanlarda kodunuzun farklı sürümleri veya dallarıyla çalışarak işbirliğine dayalı geliştirme deneyimini nasıl kolaylaştırabildiğinizi öğrenin.

> [!div class="nextstepaction"]
> [Birden çok kapsayıcı ve takım geliştirme ile çalışma](multi-service-nodejs.md)

[helm-installed]: https://helm.sh/docs/intro/install/
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
