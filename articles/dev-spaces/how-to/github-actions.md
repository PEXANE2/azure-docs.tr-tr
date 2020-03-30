---
title: Azure Kubernetes Hizmeti & GitHub Eylemleri (önizleme)
services: azure-dev-spaces
ms.date: 02/04/2020
ms.topic: conceptual
description: GitHub Eylemleri ve Azure Dev Alanları'nı kullanarak doğrudan Azure Kubernetes Hizmeti'nde çekme isteğindeki değişiklikleri gözden geçirme ve test etme
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, GitHub Eylemleri, Miğfer, servis örgü, servis örgü yönlendirme, kubectl, k8s
manager: gwallace
ms.openlocfilehash: 49715e38f36d4421b7327640ec8392a83b3c2996
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78252371"
---
# <a name="github-actions--azure-kubernetes-service-preview"></a>Azure Kubernetes Hizmeti & GitHub Eylemleri (önizleme)

Azure Geliştirme Alanları, çekme isteği deponuzun ana dalı ile birleştirilmeden önce çekme isteğindeki değişiklikleri doğrudan AKS'de test etmenizi sağlayan GitHub Eylemleri'ni kullanarak bir iş akışı sağlar. Çekme isteğideğişikliklerini gözden geçirmek için çalışan bir uygulamanın olması hem geliştiricinin hem de ekip üyelerinin güvenini artırabilir. Bu çalışan uygulama, ürün yöneticileri ve tasarımcılar gibi ekip üyelerinin geliştirmenin erken aşamalarında inceleme sürecinin bir parçası olmalarına da yardımcı olabilir.

Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:

* Azure'da yönetilen bir Kubernetes kümesinde Azure Dev Alanları'nı ayarlayın.
* Bir dev alana birden çok mikro hizmet içeren büyük bir uygulama dağıtın.
* GitHub eylemleri ile CI/CD'yi ayarlayın.
* Tam uygulama bağlamında yalıtılmış bir dev alanda tek bir microservice test edin.

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.
* [Yüklü Azure CLI][azure-cli-installed].
* [Helm 3 yüklü][helm-installed].
* [GitHub Eylemleri etkin][github-actions-beta-signup]leştirilmiş bir GitHub Hesabı.
* AKS kümesinde çalışan [Azure Dev Spaces Bike Paylaşımı örnek uygulaması.](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp/README.md)

## <a name="create-an-azure-container-registry"></a>Azure Container Registry oluşturma

Azure Kapsayıcı Kayıt Defteri (ACR):

```azurecli
az acr create --resource-group MyResourceGroup --name <acrName> --sku Basic
```

> [!IMPORTANT]
> ACR'nizin adı Azure'da benzersiz olmalı ve 5-50 alfanümerik karakter içermelidir. Kullandığınız herhangi bir harf küçük harf olmalıdır.

Daha sonraki bir adımda kullanıldığından *loginServer* değerini çıktıdan kaydedin.

## <a name="create-a-service-principal-for-authentication"></a>Kimlik doğrulama için bir hizmet ilkesi oluşturma

Bir hizmet ilkesi oluşturmak [için az reklam sp create-for-rbac'ı][az-ad-sp-create-for-rbac] kullanın. Örnek:

```azurecli
az ad sp create-for-rbac --sdk-auth --skip-assignment
```

Daha sonraki bir adımda kullanıldığından JSON çıktısını kaydedin.

AKS kümenizin *kimliğini* görüntülemek için [az aks göster'i][az-aks-show] kullanın:

```azurecli
az aks show -g MyResourceGroup -n MyAKS  --query id
```

ACR *kimliğini* görüntülemek için [az acr göster'i][az-acr-show] kullanın:

```azurecli
az acr show --name <acrName> --query id
```

*Katılımcıya* AKS kümenize ve *ACR'nize AcrPush* erişimine erişim sağlamak için [az rol ataması oluşturun.][az-role-assignment-create]

```azurecli
az role assignment create --assignee <ClientId> --scope <AKSId> --role Contributor
az role assignment create --assignee <ClientId>  --scope <ACRId> --role AcrPush
```

> [!IMPORTANT]
> Hizmetinizin bu kaynaklara temel erişimini sağlamak için hem AKS kümenizin hem de ACR'nizin sahibi olmalısınız.

## <a name="configure-your-github-action"></a>GitHub eyleminizi yapılandırın

> [!IMPORTANT]
> Deponuz için GitHub Eylemleri etkinleştirilmelidir. Deponuz için GitHub Eylemleri'ni etkinleştirmek için GitHub'daki deponuza gidin, Eylemler sekmesini tıklayın ve bu depo için eylemleri etkinleştirmeyi seçin.

Çatallı deponuza gidin ve *Ayarlar'ı*tıklatın. Sol kenar çubuğundaki *Sırlar'a* tıklayın. Aşağıdaki her yeni sırrı eklemek için *yeni bir sır ekle'yi* tıklatın:

1. *AZURE_CREDENTIALS*: hizmet ana kuruluşundan çıktının tamamı.
1. *RESOURCE_GROUP*: BU örnekte *MyResourceGroup*olan AKS kümenizin kaynak grubu.
1. *CLUSTER_NAME*: Bu örnekte *MyAKS*olan AKS kümenizin adı.
1. *CONTAINER_REGISTRY*: ACR için *loginServer.*
1. *HOST*: bu örnekte *dev.bikesharingweb.fedcab0987.eus.azds.io*olan *>.<HOST_SUFFIX>APP_NAME MASTER_SPACE>.<*<formu alan Dev Space'inizin ev sahibi.
1. *IMAGE_PULL_SECRET*: kullanmak istediğiniz sırrın adı, örneğin *demo-secret*.
1. *MASTER_SPACE*: Bu örnekte dev olan ebeveyninizin *Dev*Space'in adı.
1. *REGISTRY_USERNAME*: hizmet ana kuruluşundan JSON çıktısından *clientId.*
1. *REGISTRY_PASSWORD*: hizmet ana oluşturma JSON *çıktıistemciSecret.*

> [!NOTE]
> Tüm bu sırlar GitHub eylem tarafından kullanılan ve [.github/iş akışları/bikes.yml][github-action-yaml]yapılandırılır.

İsteğe bağlı olarak, PR birleştirdikten sonra ana alanı güncelleştirmek istiyorsanız, bu örnekte *dev.gateway.fedcab0987.eus.azds.io*olan *>.gateway.<HOST_SUFFIX>MASTER_SPACE<* formu alır *GATEWAY_HOST* sırrı ekleyin. Değişikliklerinizi çatalınızdaki ana dalda birleştirdikten sonra, ana dev alanında tüm uygulamanızı yeniden oluşturmak ve çalıştırmak için başka bir eylem çalışacaktır. Bu örnekte, ana alan *dev.* Bu eylem [.github/iş akışları/bikesharing.yml][github-action-bikesharing-yaml]olarak yapılandırılır.

## <a name="create-a-new-branch-for-code-changes"></a>Kod değişiklikleri için yeni bir dal oluşturma

Gezinmeve `BikeSharingApp/` *bisiklet görüntüleri*adı verilen yeni bir dal oluşturun.

```cmd
cd dev-spaces/samples/BikeSharingApp/
git checkout -b bike-images
```

232 ve 233 satırlarını kaldırmak için [Bikes/server.js'yi][bikes-server-js] düzenleme:

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

Dosyayı daha `git add` sonra `git commit` kullanın ve değişikliklerinizi sahnelemek için kaydedin.

```cmd
git add Bikes/server.js 
git commit -m "Removing hard coded imageUrl from /bikes/:id route"
```

## <a name="push-your-changes"></a>Değişikliklerinizi itin

Yeni `git push` şubenizi çatallı deponuza itmek için kullanın:

```cmd
git push origin bike-images
```

Itme tamamlandıktan sonra, *bisiklet görüntüleri* dalıile karşılaştırıldığında temel dal olarak çatallı deponuzdaki *ana* şubeyle çekme isteği oluşturmak için GitHub'daki çatallı deponuza gidin.

Çekme isteğiniz açıldıktan *sonra, Eylemler* sekmesine gidin. Yeni bir eylemin başladığını ve *Bisikletler* hizmetini oluşturmasını doğrulayın.

## <a name="view-the-child-space-with-your-changes"></a>Değişikliklerinizle alt alanı görüntüleme

Eylem tamamlandıktan sonra, çekme isteğindeki değişiklikleri temel alan yeni alt alanınızın URL'sini içeren bir yorum görürsünüz.

> [!div class="mx-imgBorder"]
> ![GitHub Eylem Url](../media/github-actions/github-action-url.png)

URL'yi yorumdan açarak *bikesharingweb* hizmetine gidin. Kullanıcı olarak *Aurelia Briggs'i (müşteri)* seçin ve ardından kiralamak için bir bisiklet seçin. Bisikletin yer tutucu görüntüsünü artık görmediğinizdoğrulayın.

Değişikliklerinizi çatalınızdaki *ana* dalda birleştirirseniz, tüm uygulamanızı ana geliştirme alanında yeniden oluşturmak ve çalıştırmak için başka bir eylem çalışır. Bu örnekte, üst alan *dev*. Bu eylem [.github/iş akışları/bikesharing.yml][github-action-bikesharing-yaml]olarak yapılandırılır.

## <a name="clean-up-your-azure-resources"></a>Azure kaynaklarınızı temizleme

```azurecli
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Geliştirme Alanları'nın birden çok kapsayıcıda daha karmaşık uygulamalar geliştirmenize nasıl yardımcı olduğunu ve farklı alanlarda farklı sürümlerle veya kod dallarıyla çalışarak ortak geliştirmeyi nasıl basitleştirebileceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Azure Geliştirme Alanlarında ekip geliştirme][team-quickstart]

[azure-cli-installed]: /cli/azure/install-azure-cli?view=azure-cli-latest
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-aks-show]: /cli/azure/aks?view=azure-cli-latest#az-aks-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[bikes-server-js]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/Bikes/server.js#L232-L233
[bike-sharing-gh]: https://github.com/Azure/dev-spaces/
[bike-sharing-values-yaml]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/charts/values.yaml
[github-actions-beta-signup]: https://github.com/features/actions
[github-action-yaml]: https://github.com/Azure/dev-spaces/blob/master/.github/workflows/bikes.yml
[github-action-bikesharing-yaml]: https://github.com/Azure/dev-spaces/blob/master/.github/workflows/bikesharing.yml
[helm-installed]: https://helm.sh/docs/intro/install/
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[sp-acr]: ../../container-registry/container-registry-auth-service-principal.md
[sp-aks]: ../../aks/kubernetes-service-principal.md
[team-quickstart]: ../quickstart-team-development.md
