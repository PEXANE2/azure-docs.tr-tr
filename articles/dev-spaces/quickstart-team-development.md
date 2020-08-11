---
title: Kubernetes üzerinde takım geliştirme
services: azure-dev-spaces
ms.date: 01/22/2020
ms.topic: quickstart
description: Bu hızlı başlangıçta, Azure Dev Spaces ile takım Kubernetes geliştirme işlemlerini ve mikro hizmetleri nasıl yapabileceğiniz gösterilmektedir
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s
manager: gwallace
ms.openlocfilehash: 3b4aae5c249e4dcbc4365c98791415e9fdc55c0c
ms.sourcegitcommit: 2ffa5bae1545c660d6f3b62f31c4efa69c1e957f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88080445"
---
# <a name="quickstart-team-development-on-kubernetes---azure-dev-spaces"></a>Hızlı başlangıç: Kubernetes 'de takım geliştirme-Azure Dev Spaces

Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:

- Azure 'da yönetilen bir Kubernetes kümesinde Azure Dev Spaces ayarlayın.
- Birden fazla mikro hizmet ile bir geliştirme alanına büyük bir uygulama dağıtın.
- Tek bir mikro hizmeti, tam uygulamanın bağlamı içinde yalıtılmış bir geliştirme alanında test edin.

![Azure Dev Spaces takım geliştirme](media/azure-dev-spaces/collaborate-graphic.gif)

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.
- [Yüklü Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).
- [Held 3 yüklendi][helm-installed].

## <a name="create-an-azure-kubernetes-service-cluster"></a>Azure Kubernetes Service kümesini oluşturma

[Desteklenen bir bölgede][supported-regions]aks kümesi oluşturmanız gerekir. Aşağıdaki komutlar *Myresourcegroup* adlı bir kaynak grubu ve *myaks*adlı bir aks kümesi oluşturur.

```azurecli
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --generate-ssh-keys
```

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>AKS kümenizde Azure Dev Spaces etkinleştirme

`use-dev-spaces`AKS kümenizde dev alanlarını etkinleştirmek ve istemleri izlemek için komutunu kullanın. Aşağıdaki komut *Myresourcegroup* grubundaki *myaks* kümesinde dev alanları sunar ve *dev*adlı bir dev alanı oluşturur.

> [!NOTE]
> `use-dev-spaces`Bu komut, zaten yüklenmemişse Azure dev Spaces CLI 'yi de yükleyecek. Azure Dev Spaces CLı 'yi Azure Cloud Shell yükleyemezsiniz.

```azurecli
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

`azds show-context` *Geliştirme*için hostsuffix öğesini göstermek için komutunu kullanın.

```cmd
$ azds show-context

Name                ResourceGroup     DevSpace  HostSuffix
------------------  ----------------  --------  -----------------------
MyAKS               MyResourceGroup   dev       fedcab0987.eus.azds.io
```

## <a name="update-the-helm-chart-with-your-hostsuffix"></a>Held grafiğini HostSuffix ile güncelleştirme

[Grafikler/değerler. YAML](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/charts/values.yaml) 'yi açın ve tüm örneklerini `<REPLACE_ME_WITH_HOST_SUFFIX>` daha önce aldığınız hostsuffix değeri ile değiştirin. Değişikliklerinizi kaydedin ve dosyayı kapatın.

## <a name="run-the-sample-application-in-kubernetes"></a>Kubernetes 'te örnek uygulamayı çalıştırma

Kubernetes üzerinde örnek uygulamayı çalıştırmaya yönelik komutlar, var olan bir işlemin parçasıdır ve Azure Dev Spaces araçları üzerinde hiçbir bağımlılığı yoktur. Bu durumda, Held Bu örnek uygulamayı çalıştırmak için kullanılan araçlama, ancak diğer araçlar, tüm uygulamanızı bir küme içindeki bir ad alanında çalıştırmak için kullanılabilir. HELI komutları, daha önce oluşturduğunuz *dev* adlı dev alanını hedeflerken, ancak bu dev Space de bir Kubernetes ad alanıdır. Sonuç olarak, dev Spaces diğer diğer ad alanları ile aynı diğer araç tarafından hedeflenebilir.

Bir uygulama bir kümede çalıştıktan sonra, dağıtımı yapmak için kullanılan araç ne olursa olsun, takım geliştirmesi için Azure Dev Spaces kullanabilirsiniz.

`helm install`Örnek uygulamayı kümenize kurmak ve kurmak için komutunu kullanın.

```cmd
cd charts/
helm install bikesharingsampleappsampleapp . --dependency-update --namespace dev --atomic
```

`helm install`Komutun tamamlanması birkaç dakika sürebilir. Örnek uygulama kümenize yüklendikten sonra ve kümenizde geliştirme alanları etkinleştirildikten sonra, `azds list-uris` Şu anda seçili olan *geliştirme* ortamında örnek uygulamanın URL 'lerini göstermek için komutunu kullanın.

```cmd
$ azds list-uris
Uri                                                 Status
--------------------------------------------------  ---------
http://dev.bikesharingweb.fedcab0987.eus.azds.io/  Available
http://dev.gateway.fedcab0987.eus.azds.io/         Available
```

Komuttan ortak URL 'yi açarak *bıkesharingweb* hizmetine gidin `azds list-uris` . Yukarıdaki örnekte, *bıkesharingweb* hizmeti 'nin Genel URL 'si `http://dev.bikesharingweb.fedcab0987.eus.azds.io/` . Kullanıcı olarak *Aurelia Briggs (müşteri)* seçeneğini belirleyin. *Yüksek bir Brig metnini gördiğinizi doğrulayın | En üstte oturumu kapatın* .

![Azure Dev Spaces bisiklet paylaşımı örnek uygulaması](media/quickstart-team-development/bikeshare.png)

## <a name="create-child-dev-spaces"></a>Alt dev alanları oluşturma

`azds space select` *Geliştirme*altında iki alt boşluk oluşturmak için komutunu kullanın:

```cmd
azds space select -n dev/azureuser1 -y
azds space select -n dev/azureuser2 -y
```

Yukarıdaki komutlar *dev* adında *azureuser1* ve *azureuser2*adlı iki alt boşluk oluşturur. Bu iki alt boşluk, geliştiricilerin örnek uygulamada değişiklikler yapmak için kullanması *azureuser1* ve *azureuser2* için farklı geliştirme alanlarını temsil eder.

`azds space list`Tüm dev alanlarını listelemek için komutunu kullanın ve *dev/azureuser2* ' ın seçili olduğunu onaylayın.

```cmd
$ azds space list
   Name            DevSpacesEnabled
-  --------------  ----------------
   default         False
   dev             True
   dev/azureuser1  True
*  dev/azureuser2  True
```

`azds list-uris` *Geliştirme/azureuser2*olan şu anda seçili olan alanda örnek uygulamanın URL 'lerini göstermek için öğesini kullanın.

```cmd
$ azds list-uris
Uri                                                             Status
--------------------------------------------------              ---------
http://azureuser2.s.dev.bikesharingweb.fedcab0987.eus.azds.io/  Available
http://azureuser2.s.dev.gateway.fedcab0987.eus.azds.io/         Available
```

Komut tarafından görüntülenen URL 'Lerin `azds list-uris` *azureuser2. s. dev* ön ekine sahip olduğunu doğrulayın. Bu ön ek, seçilen geçerli alanın *dev*öğesinin bir alt öğesi olan *azureuser2*olduğunu onaylar.

Komuttan ortak URL 'yi açarak *dev/azureuser2* dev alanı için *bıkesharingweb* hizmetine gidin `azds list-uris` . Yukarıdaki örnekte, *bıkesharingweb* hizmeti 'nin Genel URL 'si `http://azureuser2.s.dev.bikesharingweb.fedcab0987.eus.azds.io/` . Kullanıcı olarak *Aurelia Briggs (müşteri)* seçeneğini belirleyin. *Yüksek bir Brig metnini gördiğinizi doğrulayın | En üstte oturumu kapatın* .

## <a name="update-code"></a>Kodu güncelleştirme

Bir metin düzenleyici ile *Bıkesharingweb/Components/Header.js* açın ve [span öğesindeki metni `userSignOut` ClassName ile](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/components/Header.js#L16)değiştirin.

```html
<span className="userSignOut">
    <Link href="/devsignin"><span tabIndex="0">Welcome {props.userName} | Sign out</span></Link>
</span>
```

Değişikliklerinizi kaydedin ve dosyayı kapatın.

## <a name="build-and-run-the-updated-bikesharingweb-service-in-the-devazureuser2-dev-space"></a>Güncelleştirilmiş bıkesharingweb hizmetini *geliştirme/azureuser2* dev alanında derleyin ve çalıştırın

*Bıkesharingweb/* dizinine gidin ve `azds up` komutunu çalıştırın.

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

Bu komut, *bıkesharingweb* hizmetini *geliştirme/azureuser2* dev alanında oluşturur ve çalıştırır. Bu hizmet *dev* 'da çalışan *bıkesharingweb* hizmetine ek olarak çalışır ve yalnızca *azureuser2. s* URL ön ekine sahip istekler için kullanılır. Yönlendirmenin üst ve alt dev alanları arasında nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure dev Spaces nasıl çalışıyor ve yapılandırılır](how-dev-spaces-works.md).

Komutun çıktısında görüntülenecek genel URL 'YI açarak *dev/azureuser2* dev alanı için *bıkesharingweb* hizmetine gidin `azds up` . Kullanıcı olarak *Aurelia Briggs (müşteri)* seçeneğini belirleyin. Sağ üst köşedeki güncelleştirilmiş metni gördiğinizi doğrulayın. Bu değişikliği hemen görmüyorsanız sayfayı yenilemeniz veya tarayıcınızın önbelleğini temizlemeniz gerekebilir.

![Azure Dev Spaces bisiklet paylaşımı örnek uygulaması güncelleştirildi](media/quickstart-team-development/bikeshare-update.png)

> [!NOTE]
> Çalışırken hizmetinize gittiğinizde `azds up` , http isteği izlemeleri komutun çıktısında de görüntülenir `azds up` . Bu izlemeler, hizmetinizde sorun gidermenize ve hata ayıklamanıza yardımcı olabilir. Çalıştırırken kullanarak bu izlemeleri devre dışı bırakabilirsiniz `--disable-http-traces` `azds up` .

## <a name="verify-other-dev-spaces-are-unchanged"></a>Diğer geliştirme alanlarının değiştirilmediğinden emin olun

`azds up`Komut hala çalışıyorsa, *Ctrl + c*tuşlarına basın.

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

Tarayıcınızda *bıkesharingweb* 'in *dev* sürümüne gidin, Kullanıcı olarak *Aurelia Briggs (müşteri)* seçeneğini belirleyin ve sağ üst köşedeki Orijinal metni gördiğinizi doğrulayın. Bu adımları *dev/azureuser1* URL 'si ile tekrarlayın. Değişikliklerin yalnızca *bıkesharingweb*'in *dev/azureuser2* sürümüne uygulandığını unutmayın. *Geliştirme/azureuser2* üzerinde yapılan değişikliklerin yalıtımı, *azureuser2* 'in *azureuser1*'yi etkilemeden değişiklikler yapmasına izin verir.

Bu değişikliklerin *dev* ve *dev/azureuser1*içinde yansıtılmasını sağlamak için, takımınızın mevcut iş AKıŞıNı veya CI/CD işlem hattını izlemelisiniz. Örneğin, bu iş akışı, sürüm denetim sisteminize değişiklerinizi uygulamayı ve bir CI/CD işlem hattı veya Held gibi araçları kullanarak güncelleştirmeyi dağıtmanızı içerebilir.

## <a name="clean-up-your-azure-resources"></a>Azure kaynaklarınızı Temizleme

```azurecli
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Dev Spaces nasıl çalıştığı hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure Dev Spaces nasıl çalışır?](how-dev-spaces-works.md)

[helm-installed]: https://helm.sh/docs/intro/install/
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
