---
title: Itme ve Çekme OCI artifakı
description: Azure'da özel bir konteyner kayıt defteri kullanarak Açık Konteyner Girişimi (OCI) yapıtlarını itme ve çekme
author: SteveLasker
manager: gwallace
ms.topic: article
ms.date: 03/11/2020
ms.author: stevelas
ms.openlocfilehash: 2c6b66b635a2513ccc19e0352414d18d8389fef1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371061"
---
# <a name="push-and-pull-an-oci-artifact-using-an-azure-container-registry"></a>Azure kapsayıcı kayıt defterini kullanarak bir OCI artifacını itme ve çekme

[Açık Kapsayıcı Girişimi (OCI) yapıtlarının](container-registry-image-formats.md#oci-artifacts) yanı sıra Docker ve Docker uyumlu kapsayıcı görüntülerini depolamak ve yönetmek için bir Azure kapsayıcı kayıt defteri kullanabilirsiniz.

Bu özelliği göstermek için bu makalede, oci kayıt defterinin bir metin dosyası gibi örnek bir yapıyı Azure kapsayıcı kayıt defterine itmek için [Depolama (ORAS)](https://github.com/deislabs/oras) aracı olarak nasıl kullanılacağı gösterilmektedir. Sonra, artefaktı kayıt defterinden çıkar. Her yapıya uygun farklı komut satırı araçlarını kullanarak azure kapsayıcı kayıt defterinde çeşitli OCI yapılarını yönetebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* **Azure kapsayıcısı kayıt defteri** -Azure aboneliğinizde bir kapsayıcı kayıt defteri oluşturun. Örneğin, Azure [portalını](container-registry-get-started-portal.md) veya [Azure CLI'yi](container-registry-get-started-azure-cli.md)kullanın.
* **ORAS aracı** - [GitHub repo'sundan](https://github.com/deislabs/oras/releases)işletim sisteminiz için geçerli bir ORAS sürümü indirin ve kurun. Araç sıkıştırılmış tarball (dosya)`.tar.gz` olarak serbest bırakılır. İşletim sisteminiz için standart yordamları kullanarak dosyayı ayıklayın ve yükleyin.
* **Azure Active Directory hizmet sorumlusu (isteğe bağlı)** - Doğrudan ORAS ile kimlik doğrulaması yapmak için kayıt defterinize erişmek için bir [hizmet ilkesi](container-registry-auth-service-principal.md) oluşturun. Hizmet sorumlusuna, yapıtları itme ve çekme izinleri olması için AcrPush gibi bir rol atandığından emin olun.
* **Azure CLI (isteğe bağlı)** - Tek bir kimliği kullanmak için Azure CLI'nin yerel bir yüklemesine ihtiyacınız vardır. Sürüm 2.0.71 veya sonrası önerilir. Sürümü `az --version `bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).
* **Docker (isteğe bağlı)** - Tek bir kimlik kullanmak için, kayıt defteriile kimlik doğrulamak için Docker'ı yerel olarak da yüklemeniz gerekir. Docker [macOS][docker-mac], [Windows][docker-windows] veya [Linux][docker-linux]'ta Docker'ı kolayca yapılandırmanızı sağlayan paketler sağlar.


## <a name="sign-in-to-a-registry"></a>Kayıt defterinde oturum açma

Bu bölümde, kullanılan kimliğe bağlı olarak kayıt defterinde oturum açılmak için iki önerilen işakı görülürülüyor. Ortamınız için uygun yöntemi seçin.

### <a name="sign-in-with-oras"></a>ORAS ile oturum açın

İtme haklarına sahip bir `oras login` hizmet [sorumlusu](container-registry-auth-service-principal.md) kullanarak, hizmet temel uygulama kimliği ve parolasını kullanarak kayıt defterinde oturum açma komutunu çalıştırın. Tam nitelikli kayıt defteri adını (tüm küçük harf) belirtin, bu durumda *myregistry.azurecr.io.* Hizmet temel uygulama kimliği ortam değişkeninde `$SP_APP_ID`ve parola `$SP_PASSWD`değişkeninde geçirilir.

```bash
oras login myregistry.azurecr.io --username $SP_APP_ID --password $SP_PASSWD
```

Stdin şifresini okumak için. `--password-stdin`

### <a name="sign-in-with-azure-cli"></a>Azure CLI'de oturum açma

Yapıları konteyner kayıt defterinden itmek ve çekmek için kimliğinizle Birlikte Azure CLI'de [oturum açın.](/cli/azure/authenticate-azure-cli)

Ardından, kayıt defterine erişmek için Azure CLI [komutaz acr girişini](/cli/azure/acr?view=azure-cli-latest#az-acr-login) kullanın. Örneğin, *myregistry*adlı bir kayıt defterine kimlik doğrulamak için:

```azurecli
az login
az acr login --name myregistry
```

> [!NOTE]
> `az acr login`dosyada bir Azure Etkin Dizin belirteci `docker.config` ayarlamak için Docker istemcisini kullanır. Docker istemcisi, bireysel kimlik doğrulama akışını tamamlamak için yüklü ve çalışıyor olmalıdır.

## <a name="push-an-artifact"></a>Bir artifakı itin

Bazı örnek metiniçeren yerel çalışma dizininde bir metin dosyası oluşturun. Örneğin, bir bash kabuk:

```bash
echo "Here is an artifact!" > artifact.txt
```

Bu `oras push` metin dosyasını kayıt defterinize itmek için komutu kullanın. Aşağıdaki örnek, örnek metin dosyasını `samples/artifact` repo'ya iter. Kayıt defteri, tam nitelikli kayıt defteri adı *myregistry.azurecr.io* (tüm küçük harf) ile tanımlanır. Artefakt `1.0`etiketlendi. Yapının, dosya adını `artifact.txt`izleyen ortam *türü* dizesi tarafından tanımlanan tanımlanmamış bir türü vardır. Ek türler için [OCI Yapıları'na](https://github.com/opencontainers/artifacts) bakın. 

**Linux**

```bash
oras push myregistry.azurecr.io/samples/artifact:1.0 \
    --manifest-config /dev/null:application/vnd.unknown.config.v1+json \
    ./artifact.txt:application/vnd.unknown.layer.v1+txt
```

**Windows**

```cmd
.\oras.exe push myregistry.azurecr.io/samples/artifact:1.0 ^
    --manifest-config NUL:application/vnd.unknown.config.v1+json ^
    .\artifact.txt:application/vnd.unknown.layer.v1+txt
```

Başarılı bir itme için çıktı aşağıdakilere benzer:

```console
Uploading 33998889555f artifact.txt
Pushed myregistry.azurecr.io/samples/artifact:1.0
Digest: sha256:xxxxxxbc912ef63e69136f05f1078dbf8d00960a79ee73c210eb2a5f65xxxxxx
```

Kayıt defterinizdeki yapıları yönetmek için, Azure CLI'yi kullanıyorsanız, görüntüleri yönetmek için standart `az acr` komutlar çalıştırın. Örneğin, az acr deposu gösteri komutunu kullanarak yapının [özniteliklerini][az-acr-repository-show] alın:

```azurecli
az acr repository show \
    --name myregistry \
    --image samples/artifact:1.0
```

Çıktı aşağıdakine benzer:

```json
{
  "changeableAttributes": {
    "deleteEnabled": true,
    "listEnabled": true,
    "readEnabled": true,
    "writeEnabled": true
  },
  "createdTime": "2019-08-28T20:43:31.0001687Z",
  "digest": "sha256:xxxxxxbc912ef63e69136f05f1078dbf8d00960a79ee73c210eb2a5f65xxxxxx",
  "lastUpdateTime": "2019-08-28T20:43:31.0001687Z",
  "name": "1.0",
  "signed": false
}
```

## <a name="pull-an-artifact"></a>Bir artefakt çekme

Yapıyı `oras pull` kayıt defterinizden çekmek için komutu çalıştırın.

Önce metin dosyasını yerel çalışma dizininizden kaldırın:

```bash
rm artifact.txt
```

Yapıyı çekmek için çalıştırın `oras pull` ve yapıyı itmek için kullanılan ortam türünü belirtin:

```bash
oras pull myregistry.azurecr.io/samples/artifact:1.0 \
    --media-type application/vnd.unknown.layer.v1+txt
```

Çekmenin başarılı olduğunu doğrulayın:

```bash
$ cat artifact.txt
Here is an artifact!
```

## <a name="remove-the-artifact-optional"></a>Yapıyı kaldırma (isteğe bağlı)

Yapıyı Azure kapsayıcı kayıt defterinizden kaldırmak için [az acr deposu silme][az-acr-repository-delete] komutunu kullanın. Aşağıdaki örnek, orada depolanan yapıyı kaldırır:

```azurecli
az acr repository delete \
    --name myregistry \
    --image samples/artifact:1.0
```

## <a name="next-steps"></a>Sonraki adımlar

* Bir yapı için bir manifestonun nasıl yapılandırılabildiğini de içeren [ORAS Kitaplığı](https://github.com/deislabs/oras/tree/master/docs)hakkında daha fazla bilgi edinin
* Yeni yapı türleri hakkında referans bilgileri için [OCI Artefakt](https://github.com/opencontainers/artifacts) repo'yu ziyaret edin



<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-acr-repository-show]: /cli/azure/acr/repository?#az-acr-repository-show
[az-acr-repository-delete]: /cli/azure/acr/repository#az-acr-repository-delete
