---
title: Docker görüntüsünü & çekin
description: Docker CLI’yı kullanarak Azure’da özel bir kapsayıcı kayıt defterine Docker görüntüleri itme ve kapsayıcıdan görüntü çekme
ms.topic: article
ms.date: 01/23/2019
ms.custom: seodec18, H1Hack27Feb2017
ms.openlocfilehash: 6751a04c3c1bfe826334161704c20c1ba2e5a6d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74456363"
---
# <a name="push-your-first-image-to-a-private-docker-container-registry-using-the-docker-cli"></a>Docker CLI’yı kullanarak özel bir Dockler kapsayıcı kayıt defterine ilk görüntünüzü itme

Azure kapsayıcısı kayıt defteri, [Docker Hub](https://hub.docker.com/)’ın genel Docker görüntülerini depolama yöntemine benzer şekilde özel [Docker](https://hub.docker.com) kapsayıcı görüntülerini depolar ve yönetir. Giriş, [itme,](https://docs.docker.com/engine/reference/commandline/push/) [çekme](https://docs.docker.com/engine/reference/commandline/pull/)ve konteyner kayıt defterinizdeki diğer işlemler [için](https://docs.docker.com/engine/reference/commandline/login/)Docker komut satırı [arabirimini](https://docs.docker.com/engine/reference/commandline/cli/) (Docker CLI) kullanabilirsiniz.

Aşağıdaki adımlarda, resmi bir [Nginx görüntüsünü](https://store.docker.com/images/nginx) ortak Docker Hub kayıt defterinden indirir, özel Azure konteyner kayıt defteriniz için etiketler, kayıt defterinize iter ve ardından kayıt defterinden çekersiniz.

## <a name="prerequisites"></a>Ön koşullar

* **Azure kapsayıcısı kayıt defteri** -Azure aboneliğinizde bir kapsayıcı kayıt defteri oluşturun. Örneğin, Azure [portalını](container-registry-get-started-portal.md) veya [Azure CLI'yi](container-registry-get-started-azure-cli.md)kullanın.
* **Docker CLI** - Docker'ı yerel olarak da yüklemelisiniz. Docker [macOS][docker-mac], [Windows][docker-windows] veya [Linux][docker-linux]'ta Docker'ı kolayca yapılandırmanızı sağlayan paketler sağlar.

## <a name="log-in-to-a-registry"></a>Kayıt defterinde oturum açma

Özel konteyner kayıt defterinize [kimlik doğrulamanın birkaç yolu](container-registry-authentication.md) vardır. Komut satırında çalışırken önerilen yöntem Azure CLI komut [az acr giriş](/cli/azure/acr?view=azure-cli-latest#az-acr-login)ile . Örneğin, *myregistry*adlı bir kayıt defterine giriş yapmak için:

```azurecli
az acr login --name myregistry
```

Ayrıca [docker giriş](https://docs.docker.com/engine/reference/commandline/login/)ile giriş yapabilirsiniz. Örneğin, bir otomasyon senaryosu için kayıt defterinize [bir hizmet sorumlusu atamış](container-registry-authentication.md#service-principal) olabilirsiniz. Aşağıdaki komutu çalıştırdığınızda, istendiğinde hizmet inanca appID (kullanıcı adı) ve parolayı etkileşimli olarak sağlayın. Giriş kimlik bilgilerini yönetmek için en iyi uygulamalar için [docker giriş](https://docs.docker.com/engine/reference/commandline/login/) komutu başvurusuna bakın:

```
docker login myregistry.azurecr.io
```

Her iki `Login Succeeded` komut da tamamlandıktan sonra döner.

> [!TIP]
> Kullandığınızda `docker login` ve resimleri kayıt defterinize itmek için etiketlediğinizde her zaman tam nitelikli kayıt defteri adını (tüm küçük harf) belirtin. Bu makaledeki örneklerde, tam nitelikli ad *myregistry.azurecr.io.*

## <a name="pull-the-official-nginx-image"></a>Resmi Nginx görüntüsünü çekin

İlk olarak, ortak Nginx görüntüsünü yerel bilgisayarınıza çekin.

```
docker pull nginx
```

## <a name="run-the-container-locally"></a>Kapsayıcıyı yerel olarak çalıştırma

8080 [portundaki](https://docs.docker.com/engine/reference/run/) Nginx kapsayıcısının yerel bir`-it`örneğini etkileşimli olarak başlatmak için docker run komutunu uygulayın. Bağımsız `--rm` değişken, kapsayıcıyı durdurunca kaldırılması gerektiğini belirtir.

```
docker run -it --rm -p 8080:80 nginx
```

Nginx `http://localhost:8080` tarafından çalışan kapsayıcıda sunulan varsayılan web sayfasını görüntülemek için göz atın. Aşağıdakilere benzer bir sayfa görmeniz gerekir:

![Yerel bilgisayarda Nginx](./media/container-registry-get-started-docker-cli/nginx.png)

Kapsayıcıyı etkileşimli olarak `-it`başlattığınız için, tarayıcınızda gezinmeden sonra Nginx sunucusunun çıkışını komut satırında görebilirsiniz.

Konteyneri `Control` + `C`durdurmak ve çıkarmak için.

## <a name="create-an-alias-of-the-image"></a>Görüntünün diğer adını oluşturma

Resmin diğer adını, kayıt defterinize tam nitelikli bir şekilde oluşturmak için [docker etiketini](https://docs.docker.com/engine/reference/commandline/tag/) kullanın. Bu örnek, kayıt defterinin kökünde dağınıklığı önlemek için `samples` ad alanını belirtir.

```
docker tag nginx myregistry.azurecr.io/samples/nginx
```

Ad alanlarıyla etiketleme hakkında daha fazla bilgi için Azure Kapsayıcı Kayıt Defteri için En İyi Uygulamalar'ın [Depo ad alanları](container-registry-best-practices.md#repository-namespaces) [bölümüne](container-registry-best-practices.md)bakın.

## <a name="push-the-image-to-your-registry"></a>Görüntüyü kayıt defterinize itme

Artık resmi özel kayıt defterinize tam nitelikli bir yol ile etiketlediğinize göre, [docker itme](https://docs.docker.com/engine/reference/commandline/push/)ile kayıt defterine itebilirsiniz:

```
docker push myregistry.azurecr.io/samples/nginx
```

## <a name="pull-the-image-from-your-registry"></a>Görüntüyü kayıt defterinizden çekme

Resmi kayıt defterinizden çekmek için [docker çekme](https://docs.docker.com/engine/reference/commandline/pull/) komutunu kullanın:

```
docker pull myregistry.azurecr.io/samples/nginx
```

## <a name="start-the-nginx-container"></a>Nginx kapsayıcısını başlatma

Kayıt defterinizden çektiğiniz görüntüyü çalıştırmak için [docker run](https://docs.docker.com/engine/reference/run/) komutunu kullanın:

```
docker run -it --rm -p 8080:80 myregistry.azurecr.io/samples/nginx
```

Çalışan kapsayıcıyı görüntülemek için `http://localhost:8080` göz atın.

Konteyneri `Control` + `C`durdurmak ve çıkarmak için.

## <a name="remove-the-image-optional"></a>Görüntüyü kaldırma (isteğe bağlı)

Nginx resmine artık ihtiyacınız yoksa docker [rmi](https://docs.docker.com/engine/reference/commandline/rmi/) komutuyla yerel olarak silebilirsiniz.

```
docker rmi myregistry.azurecr.io/samples/nginx
```

Azure kapsayıcı kayıt defterinizden görüntüleri kaldırmak için Azure CLI komutu [az acr deposu silme'yi](/cli/azure/acr/repository#az-acr-repository-delete)kullanabilirsiniz. Örneğin, aşağıdaki `samples/nginx:latest` komut, etikettarafından başvurulan bildirimi, benzersiz katman verilerini ve bildirime başvuran diğer tüm etiketleri siler.

```azurecli
az acr repository delete --name myregistry --image samples/nginx:latest
```

## <a name="next-steps"></a>Sonraki adımlar

Artık temel bilgileri bildiğinize göre, kayıt defterinizi kullanmaya başlamaya hazırsınız! Örneğin, kayıt defterinizdeki kapsayıcı görüntülerini şu şekilde dağıtın:

* [Azure Kubernetes Hizmeti (AKS)](../aks/tutorial-kubernetes-prepare-app.md)
* [Azure Container Instances](../container-instances/container-instances-tutorial-prepare-app.md)
* [Service Fabric](../service-fabric/service-fabric-tutorial-create-container-images.md)

Azure konteyner kayıtlarınızla çalışmak için Visual [Studio Kodu için Docker Uzantısı'nı](https://code.visualstudio.com/docs/azure/docker) ve Azure [Hesabı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) uzantısını isteğe bağlı olarak yükleyin. Görüntüleri bir Azure kapsayıcı kayıt defterine çekin ve itin veya Tümü Visual Studio Kodu'nda bulunan ACR Görevleri'ni çalıştırın.


<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-windows]: https://docs.docker.com/docker-for-windows/
