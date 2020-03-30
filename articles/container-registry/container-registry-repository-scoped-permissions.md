---
title: Azure Kapsayıcı Kayıt Defteri'nde depo kullanma izinleri
description: Görüntüleri çekmek veya itmek veya başka eylemler gerçekleştirmek için kayıt defterindeki belirli depolara yönelik izinleri içeren bir belirteç oluşturma
ms.topic: article
ms.date: 02/13/2020
ms.openlocfilehash: 7d390bf4d97561e374c70f184534ac4f98a40611
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77444340"
---
# <a name="create-a-token-with-repository-scoped-permissions"></a>Depo kapsamı yla ilgili izinleri içeren bir belirteç oluşturma

Bu makalede, konteyner kayıt defterinizde depo kapsamı yla ilgili izinleri yönetmek için belirteçler ve kapsam eşlemleri nasıl oluşturulacak açıklanmaktadır. Kayıt defteri sahibi, belirteçler oluşturarak, görüntüleri çekmek veya itmek veya başka eylemleri gerçekleştirmek için depolara kapsamlı, zaman sınırlı erişim sağlayan kullanıcılara veya hizmetler sağlayabilir. Belirteç, tüm kayıt defterine izin veren diğer kayıt defteri [kimlik doğrulama seçeneklerinden](container-registry-authentication.md)daha fazla ince taneli izin sağlar. 

Belirteç oluşturmak için senaryolar şunlardır:

* Tek tek belirteçleri olan IoT aygıtlarının bir depodan görüntü çekmesine izin ver
* Belirli bir depoya izinveren harici bir kuruluş sağlama 
* Kuruluşunuzdaki farklı kullanıcı gruplarına depo erişimini sınırlandırın. Örneğin, belirli depoları hedefleyen görüntüler oluşturan geliştiricilere yazma ve okuma erişimi sağlayın ve bu depolardan dağıtan takımlara erişimi okuyun.

> [!IMPORTANT]
> Bu özellik şu anda önizlemede dir ve bazı [sınırlamalar geçerlidir.](#preview-limitations) Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="preview-limitations"></a>Önizleme sınırlamaları

* Bu özellik yalnızca **Premium** konteyner kayıt defterinde kullanılabilir. Kayıt defteri hizmeti katmanları ve sınırları hakkında daha fazla bilgi için [Azure Konteyner Kayıt Defteri SK'leri'ne](container-registry-skus.md)bakın.
* Şu anda bir hizmet yöneticisi veya yönetilen kimlik gibi bir Azure Etkin Dizin kimliğine depo kapsamı yla ilgili izinler atayamazsınız.

## <a name="concepts"></a>Kavramlar

Depo kapsamı yla ilgili izinleri yapılandırmak için ilişkili *kapsam eşlemi*içeren bir *belirteç* oluşturursunuz. 

* Oluşturulan parolayla birlikte bir **belirteç,** kullanıcının kayıt defteriyle birlikte kimlik doğrulamasını sağlar. Belirteç parolası için son kullanma tarihi ayarlayabilir veya istediğiniz zaman bir belirteci devre dışı bırakabilirsiniz.  

  Kullanıcı veya hizmet, bir belirteçle kimlik doğrulaması yaptıktan sonra, bir veya daha fazla depoya yönelik bir veya daha fazla *eylem* gerçekleştirebilir.

  |Eylem  |Açıklama  | Örnek |
  |---------|---------|--------|
  |`content/delete`    | Depodan verileri kaldırma  | Bir depoveya manifestosilme |
  |`content/read`     |  Depodaki verileri okuma |  Bir artefakt çekme |
  |`content/write`     |  Depoya veri yazma     | Bir `content/read` artefakt itmek için kullanın |
  |`metadata/read`    | Depodaki meta verileri okuma   | Etiketleri veya bildirimleri listele |
  |`metadata/write`     |  Depoya meta veri yazma  | Okuma, yazma veya silme işlemlerini etkinleştirme veya devre dışı |

* **Kapsam eşlemi,** bir belirteç için uyguladığınız depo izinlerini gruplatır ve diğer belirteçlere yeniden başvurabilir. Her belirteç tek bir kapsam eşlemi ile ilişkilidir. 

   Kapsam haritası ile:

    * Aynı izinlere sahip birden çok belirteçleri bir dizi depoya yapılandırma
    * Kapsam haritasına depo eylemleri eklediğinizde veya kaldırdığınızda veya farklı bir kapsam eşlemi uyguladığınızda belirteç izinlerini güncelleştirme 

  Azure Kapsayıcı Kayıt Defteri, tüm depolarda sabit izinlerle uygulayabileceğiniz sistem tanımlı kapsam eşlemleri de sağlar.

Aşağıdaki resimde belirteçler ve kapsam eşlemleri arasındaki ilişki gösterilmektedir. 

![Kayıt defteri belirteçleri ve kapsam haritaları](media/container-registry-repository-scoped-permissions/token-scope-map-concepts.png)

## <a name="prerequisites"></a>Ön koşullar

* **Azure CLI** - Azure CLI komutları oluşturma ve yönetme belirteçleri Azure CLI sürüm 2.0.76 veya sonraki sürümlerde kullanılabilir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).
* **Docker** - Görüntüleri çekmek veya itmek için kayıt defteriyle kimlik doğrulaması yapmak için yerel bir Docker yüklemesine ihtiyacınız vardır. Docker, [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms) sistemleri için yükleme yönergeleri sağlar.
* **Konteyner kayıt defteri** - Hesabınız yoksa, Azure aboneliğinizde bir Premium kapsayıcı kayıt defteri oluşturun veya varolan bir kayıt defterini yükseltin. Örneğin, Azure [portalını](container-registry-get-started-portal.md) veya [Azure CLI'yi](container-registry-get-started-azure-cli.md)kullanın. 

## <a name="create-token---cli"></a>Belirteç oluşturma - CLI

### <a name="create-token-and-specify-repositories"></a>Belirteç oluşturma ve depoları belirtin

[Az acr belirteç oluşturma][az-acr-token-create] komutunu kullanarak bir belirteç oluşturun. Bir belirteç oluştururken, her depoda bir veya daha fazla depo ve ilişkili eylem belirtebilirsiniz. Depoların henüz kayıt defterinde olması gerekmiyor. Varolan bir kapsam eşlemi belirterek bir belirteç oluşturmak için bir sonraki bölüme bakın.

Aşağıdaki örnek, `samples/hello-world` kayıt *defteri defterinde* repo'da aşağıdaki izinleri içeren bir `content/write` `content/read`belirteç oluşturur: ve . Varsayılan olarak, komut varsayılan belirteç `enabled`durumunu , ancak durumu `disabled` istediğiniz zaman olarak güncelleştirebilirsiniz olarak ayarlar.

```azurecli
az acr token create --name MyToken --registry myregistry \
  --repository samples/hello-world \
  content/write content/read
```

Çıktı, oluşturulan iki parola da dahil olmak üzere belirteç hakkındaki ayrıntıları gösterir. Parolaları daha sonra kimlik doğrulama için kullanmak üzere güvenli bir yere kaydetmeniz önerilir. Parolalar yeniden alınamaz, ancak yenileri oluşturulabilir.

```console
{
  "creationDate": "2020-01-18T00:15:34.066221+00:00",
  "credentials": {
    "certificates": [],
    "passwords": [
      {
        "creationTime": "2020-01-18T00:15:52.837651+00:00",
        "expiry": null,
        "name": "password1",
        "value": "uH54BxxxxK7KOxxxxRbr26dAs8JXxxxx"
      },
      {
        "creationTime": "2020-01-18T00:15:52.837651+00:00",
        "expiry": null,
        "name": "password2",
        "value": "kPX6Or/xxxxLXpqowxxxxkA0idwLtmxxxx"
      }
    ],
    "username": "MyToken"
  },
  "id": "/subscriptions/xxxxxxxx-adbd-4cb4-c864-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry/tokens/MyToken",
  "name": "MyToken",
  "objectId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "scopeMapId": "/subscriptions/xxxxxxxx-adbd-4cb4-c864-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry/scopeMaps/MyToken-scope-map",
  "status": "enabled",
  "type": "Microsoft.ContainerRegistry/registries/tokens"
```

Çıktı, komutun oluşturduğu kapsam eşlemi yle ilgili ayrıntıları içerir. Aynı depo hareketlerini diğer belirteçlere uygulamak için burada adlandırılmış `MyToken-scope-map`kapsam eşlemini kullanabilirsiniz. Veya, ilişkili belirteçlerin izinlerini değiştirmek için kapsam eşlesini daha sonra güncelleştirin.

### <a name="create-token-and-specify-scope-map"></a>Belirteci oluşturma ve kapsam eşlemi belirt

Belirteç oluşturmanın alternatif bir yolu, varolan bir kapsam eşlemi belirtmektir. Zaten bir kapsam eşleminiz yoksa, önce depoları ve ilişkili eylemleri belirterek bir tane oluşturun. Ardından, bir belirteç oluştururken kapsam eşlemi belirtin. 

Kapsam eşlemi oluşturmak için [az acr kapsam eşlemi oluşturma][az-acr-scope-map-create] komutunu kullanın. Aşağıdaki komut, daha önce kullanılan `samples/hello-world` depoda aynı izinlere sahip bir kapsam eşlemi oluşturur. 

```azurecli
az acr scope-map create --name MyScopeMap --registry myregistry \
  --repository samples/hello-world \
  content/write content/read \
  --description "Sample scope map"
```

*MyScopeMap* kapsam eşlemi belirten bir belirteç oluşturmak için [az acr belirteci oluşturun.][az-acr-token-create] Önceki örnekte olduğu gibi, komut varsayılan belirteç durumunu `enabled`.

```azurecli
az acr token create --name MyToken \
  --registry myregistry \
  --scope-map MyScopeMap
```

Çıktı, oluşturulan iki parola da dahil olmak üzere belirteç hakkındaki ayrıntıları gösterir. Parolaları daha sonra kimlik doğrulama için kullanmak üzere güvenli bir yere kaydetmeniz önerilir. Parolalar yeniden alınamaz, ancak yenileri oluşturulabilir.

## <a name="create-token---portal"></a>Belirteç oluşturma - portal

Belirteçler ve kapsam haritaları oluşturmak için Azure portalını kullanabilirsiniz. CLI `az acr token create` komutunda olduğu gibi, varolan bir kapsam eşlemi uygulayabilir veya bir veya daha fazla depo ve ilişkili eylem belirterek bir belirteç oluşturduğunuzda bir kapsam eşlemi oluşturabilirsiniz. Depoların henüz kayıt defterinde olması gerekmiyor. 

Aşağıdaki örnek bir belirteç oluşturur ve `samples/hello-world` depoda aşağıdaki izinleri içeren bir kapsam `content/write` `content/read`eşlemi oluşturur: ve .

1. Portalda, konteyner kayıt defterinize gidin.
1. **Hizmetler**altında, **Jeton (Önizleme) > +Ekle'yi**seçin.
  ![Portalda belirteç oluşturma](media/container-registry-repository-scoped-permissions/portal-token-add.png)
1. Bir belirteç adı girin.
1. **Kapsam haritası**altında, **yeni oluştur'u**seçin.
1. Kapsam haritasını yapılandırın:
    1. Kapsam haritası için bir ad ve açıklama girin. 
    1. **Depolar**altında , `samples/hello-world`girin ve **İzinler** `content/read` altında `content/write`, seçin ve . Sonra **+Ekle'yi**seçin.  
    ![Portalda kapsam haritası oluşturma](media/container-registry-repository-scoped-permissions/portal-scope-map-add.png)

    1. Depolar ve izinler ekledikten sonra kapsam eşlemi eklemek için **Ekle'yi** seçin.
1. **Etkin** varsayılan belirteç **Durumunu** kabul edin ve ardından **Oluştur'u**seçin.

Belirteç doğrulandıktan ve oluşturulduktan sonra belirteç ayrıntıları **Belirteçler** ekranında görünür.

### <a name="add-token-password"></a>Belirteç parolası ekleme

Bir belirteç oluşturduktan sonra parola oluşturun. Kayıt defteriyle kimlik doğrulaması yapmak için belirteci etkinleştirilmeli ve geçerli bir parolaya sahip olmalıdır.

Bir veya iki parola oluşturabilir ve her biri için bir son kullanma tarihi ayarlayabilirsiniz. 

1. Portalda, konteyner kayıt defterinize gidin.
1. **Hizmetler** **altında, Belirteçler (Önizleme) seçeneğini**belirleyin ve bir belirteç seçin.
1. Belirteç ayrıntılarında **parola1** veya **password2'yi**seçin ve Oluştur simgesini seçin.
1. Parola ekranında isteğe bağlı olarak parola için bir son kullanma tarihi ayarlayın ve **Oluştur'u**seçin.
1. Parola oluşturup kopyaladıktan sonra kopyalayın ve güvenli bir konuma kaydedin. Ekranı kapattıktan sonra oluşturulan parolayı alamazsınız, ancak yeni bir parola oluşturabilirsiniz.

    ![Portalda belirteç parolası oluşturma](media/container-registry-repository-scoped-permissions/portal-token-password.png)

## <a name="authenticate-with-token"></a>Belirteçle kimlik doğrulaması

Bir kullanıcı veya hizmet, hedef kayıt defteriyle kimlik doğrulaması yapmak için bir belirteç kullandığında, belirteç adını bir kullanıcı adı ve oluşturulan parolalardan biri olarak sağlar. Kimlik doğrulama yöntemi, belirteçle ilişkili yapılandırılan eylem veya eylemlere bağlıdır.

|Eylem  |Kimlik doğrulaması nasıl  |
  |---------|---------|
  |`content/delete`    | `az acr repository delete`azure cli içinde |
  |`content/read`     |  `docker login`<br/><br/>`az acr login`azure cli içinde  |
  |`content/write`     |  `docker login`<br/><br/>`az acr login`azure cli içinde     |
  |`metadata/read`    | `az acr repository show`<br/><br/>`az acr repository show-tags`<br/><br/>`az acr repository show-manifests`azure cli içinde   |
  |`metadata/write`     |  `az acr repository untag`<br/><br/>`az acr repository update`azure cli içinde |

## <a name="examples-use-token"></a>Örnekler: Belirteç kullanın

Aşağıdaki örnekler, bir depo üzerinde ortak işlemleri gerçekleştirmek için bu makalede daha önce oluşturulan belirteci kullanır: itme ve çekme görüntüleri, görüntüleri silmek ve liste deposu etiketleri. Belirteç, başlangıçta depodaki`content/write` `content/read` `samples/hello-world` itme izinleri (ve eylemleri) ile ayarlanmıştır.

### <a name="pull-and-tag-test-images"></a>Test görüntülerini çekme ve etiketleme

Aşağıdaki örnekler için Docker `hello-world` `alpine` Hub'dan ve görüntüleri çekin ve bunları kayıt defteriniz ve deponuz için etiketleyin.

```bash
docker pull hello-world
docker pull alpine
docker tag hello-world myregistry.azurecr.io/samples/hello-world:v1
docker tag hello-world myregistry.azurecr.io/samples/alpine:v1
```

### <a name="authenticate-using-token"></a>Belirteç kullanarak kimlik doğrulaması

Kayıt `docker login` defteriyle kimlik doğrulaması yapmak için çalıştırın, belirteç adını kullanıcı adı olarak sağlayın ve parolalarından birini sağlayın. Belirteç `Enabled` durumu olmalıdır.

Aşağıdaki örnek bash kabuğu için biçimlendirilir ve çevre değişkenlerini kullanarak değerleri sağlar.

```bash
TOKEN_NAME=MyToken
TOKEN_PWD=<token password>

echo $TOKEN_PWD | docker login --username $TOKEN_NAME --password-stdin myregistry.azurecr.io
```

Çıktı başarılı kimlik doğrulaması göstermelidir:

```console
Login Succeeded
```

### <a name="push-images-to-registry"></a>Kayıt defterine görüntü gönderme

Başarılı oturum açtıktan sonra, etiketlenmiş resimleri kayıt defterine itmeye çalış. Belirteç, görüntüleri depoya itmek `samples/hello-world` için izinlere sahip olduğundan, aşağıdaki itme başarılı oldu:

```bash
docker push myregistry.azurecr.io/samples/hello-world:v1
```

Belirteç `samples/alpine` repo için izinleri yoktur, bu nedenle aşağıdaki itme girişimi `requested access to the resource is denied`benzer bir hata ile başarısız olur:

```bash
docker push myregistry.azurecr.io/samples/alpine:v1
```

### <a name="change-pushpull-permissions"></a>İtme/çekme izinlerini değiştirme

Bir belirteç izinlerini güncelleştirmek için, ilişkili kapsam haritasındaki izinleri güncelleştirin. Güncelleştirilmiş kapsam eşlemi, ilişkili tüm belirteçlere hemen uygulanır. 

`MyToken-scope-map` Örneğin, depodaki `content/write` `content/read` `samples/alpine` eylemleri ve ilgili eylemleri güncelleştirin `content/write` ve `samples/hello-world` depodaki eylemi kaldırın.  

Azure CLI'yi kullanmak için kapsam eşlemi güncelleştirmek için [az acr kapsam eşlemi güncelleştirmesini][az-acr-scope-map-update] çalıştırın:

```azurecli
az acr scope-map update \
  --name MyScopeMap \
  --registry myregistry \
  --add samples/alpine content/write content/read \
  --remove samples/hello-world content/write 
```

Azure portalında:

1. Konteyner kayıt defterinize gidin.
1. **Hizmetler**altında **Kapsam eşlemlerini (Önizleme)** seçin ve güncelleştirilecek kapsam eşlemi'ni seçin.
1. **Depolar**altında , `samples/alpine`girin ve **İzinler** `content/read` altında `content/write`, seçin ve . Sonra **+Ekle'yi**seçin.
1. **Depolar**altında, seçin `samples/hello-world` ve **İzinler**altında `content/write`, seçin. Ardından **Kaydet'i**seçin.

Kapsam eşlemi güncelleştirdikten sonra, aşağıdaki itme başarılı oldu:

```bash
docker push myregistry.azurecr.io/samples/alpine:v1
```

Kapsam eşlemi yalnızca `content/read` `samples/hello-world` depoda izin olduğundan, `samples/hello-world` repo'ya yönelik bir itme girişimi başarısız olur:
 
```bash
docker push myregistry.azurecr.io/samples/hello-world:v1
```

Kapsam eşlemi her iki depoda da `content/read` izinler sağladığından, her iki depodan da görüntü çekmek başarılı dır:

```bash
docker pull myregistry.azurecr.io/samples/alpine:v1
docker pull myregistry.azurecr.io/samples/hello-world:v1
```
### <a name="delete-images"></a>Görüntüleri silme

Eylemi depoya ekleyerek `content/delete` kapsam eşlemi güncelleştirin. `alpine` Bu eylem, depodaki görüntülerin silinmesine veya deponun tamamının silinmesine olanak tanır.

Kısalık için, kapsam eşlemi güncelleştirmek için yalnızca [az acr kapsam eşlemi güncelleştirme][az-acr-scope-map-update] komutunu gösteririz:

```azurecli
az acr scope-map update \
  --name MyScopeMap \
  --registry myregistry \
  --add samples/alpine content/delete
``` 

Portalı kullanarak kapsam eşlesini güncelleştirmek için önceki bölüme bakın.

Depoyu silmek için aşağıdaki [az acr deposu silme][az-acr-repository-delete] komutunu `samples/alpine` kullanın. Görüntüleri veya depoları silmek için belirteç üzerinden kimlik `docker login`doğrulamaz. Bunun yerine, belirteç adını ve parolasını komuta geçirin. Aşağıdaki örnek, makalede daha önce oluşturulan ortam değişkenlerini kullanır:

```azurecli
az acr repository delete \
  --name myregistry --repository samples/alpine \
  --username $TOKEN_NAME --password $TOKEN_PWD
```

### <a name="show-repo-tags"></a>Repo etiketlerini göster 

Eylemi depoya ekleyerek `metadata/read` kapsam eşlemi güncelleştirin. `hello-world` Bu eylem, depodaki manifesto ve etiket verilerinin okunmasına olanak tanır.

Kısalık için, kapsam eşlemi güncelleştirmek için yalnızca [az acr kapsam eşlemi güncelleştirme][az-acr-scope-map-update] komutunu gösteririz:

```azurecli
az acr scope-map update \
  --name MyScopeMap \
  --registry myregistry \
  --add samples/hello-world metadata/read 
```  

Portalı kullanarak kapsam eşlesini güncelleştirmek için önceki bölüme bakın.

Depodaki `samples/hello-world` meta verileri okumak için az [acr deposu show-manifests][az-acr-repository-show-manifests] veya [az acr depo show-tags][az-acr-repository-show-tags] komutunu çalıştırın. 

Meta verileri okumak için belirteç üzerinden doğrudoğrulama `docker login`yapmaz. Bunun yerine, belirteç adını ve parolasını her iki komuta geçirin. Aşağıdaki örnek, makalede daha önce oluşturulan ortam değişkenlerini kullanır:

```azurecli
az acr repository show-tags \
  --name myregistry --repository samples/hello-world \
  --username $TOKEN_NAME --password $TOKEN_PWD
```

Örnek çıktı:

```console
[
  "v1"
]
```
## <a name="manage-tokens-and-scope-maps"></a>Jetonları ve kapsam eşlemlerini yönetme

### <a name="list-scope-maps"></a>Kapsam eşlemlerini listele

Bir kayıt defterinde yapılandırılan tüm kapsam eşlemlerini listelemek için [az acr kapsam haritası listesi][az-acr-scope-map-list] komutunu veya portaldaki Kapsam **eşlemleri (Önizleme)** ekranını kullanın. Örnek:

```azurecli
az acr scope-map list \
  --registry myregistry --output table
```

Çıktı, tanımladığınız kapsam eşlemlerini ve belirteçleri yapılandırmak için kullanabileceğiniz sistem tanımlı kapsam eşlerini gösterir:

```
NAME                 TYPE           CREATION DATE         DESCRIPTION
-------------------  -------------  --------------------  ------------------------------------------------------------
_repositories_admin  SystemDefined  2020-01-20T09:44:24Z  Can perform all read, write and delete operations on the ...
_repositories_pull   SystemDefined  2020-01-20T09:44:24Z  Can pull any repository of the registry
_repositories_push   SystemDefined  2020-01-20T09:44:24Z  Can push to any repository of the registry
MyScopeMap           UserDefined    2019-11-15T21:17:34Z  Sample scope map
```

### <a name="show-token-details"></a>Belirteç ayrıntılarını göster

Durum ve parola son kullanma tarihleri gibi bir belirteç ayrıntılarını görüntülemek için [az acr belirteç göster][az-acr-token-show] komutunu çalıştırın veya portaldaki **Belirteçler (Önizleme)** ekranındaki belirteçleri seçin. Örnek:

```azurecli
az acr scope-map show \
  --name MyScopeMap --registry myregistry
```

Bir kayıt defterinde yapılandırılan tüm belirteçleri listelemek için [az acr belirteç listesi][az-acr-token-list] komutunu veya portaldaki **belirteçler (Önizleme)** ekranını kullanın. Örnek:

```azurecli
az acr token list --registry myregistry --output table
```

### <a name="generate-passwords-for-token"></a>Belirteç için parola oluşturma

Belirteç parolanız yoksa veya yeni parolalar oluşturmak istiyorsanız, [az kısaltma kimlik bilgisi oluşturma][az-acr-token-credential-generate] komutunu çalıştırın. 

Aşağıdaki örnek, *MyToken* belirteci için 30 günlük bir son kullanma süresiyle parola1 için yeni bir değer oluşturur. Parolayı ortam değişkeninde `TOKEN_PWD`depolar. Bu örnek bash kabuğu için biçimlendirilir.

```azurecli
TOKEN_PWD=$(az acr token credential generate \
  --name MyToken --registry myregistry --days 30 \
  --password1 --query 'passwords[0].value' --output tsv)
```

Belirteç parolası oluşturmak için Azure portalını kullanmak için, bu makalenin başlarında [belirteç oluştur - portalındaki](#create-token---portal) adımlara bakın.

### <a name="update-token-with-new-scope-map"></a>Yeni kapsam haritasıyla belirteç güncelleştirme

Belirteci farklı bir kapsam eşlemi yle güncelleştirmek istiyorsanız, [az acr belirteci güncelleştirmesini][az-acr-token-update] çalıştırın ve yeni kapsam eşlemi belirtin. Örnek:

```azurecli
az acr token update --name MyToken --registry myregistry \
  --scope-map MyNewScopeMap
```

Portalda, **Jetonlar (önizleme)** ekranında, belirteci seçin ve **Kapsam haritası**altında, farklı bir kapsam eşlemi seçin.

> [!TIP]
> Bir belirteci yeni bir kapsam eşlemi yle güncelledikten sonra, yeni belirteç parolaları oluşturmak isteyebilirsiniz. Az [acr belirteç kimlik bilgisi oluşturma][az-acr-token-credential-generate] komutunu kullanın veya Azure portalında bir belirteç parolasını yeniden oluşturun.

## <a name="disable-or-delete-token"></a>Belirteci devre dışı veya silme

Bir kullanıcı veya hizmet için belirteç kimlik bilgilerinin kullanımını geçici olarak devre dışı bmalısınız. 

Azure CLI'yi kullanarak, [az acr belirteç güncelleştirme][az-acr-token-update] komutunu çalıştırarak `status` aşağıdakileri `disabled`ayarlayın:

```azurecli
az acr token update --name MyToken --registry myregistry \
  --status disabled
```

Portalda, **Belirteçler (Önizleme)** ekranındaki belirteci seçin ve **Durum**altında **Devre Dışı'yı** seçin.

Kimlik bilgilerini kullanan herkesin erişimini kalıcı olarak geçersiz kılarak bir belirteci silmek için [az acr belirteç silme][az-acr-token-delete] komutunu çalıştırın. 

```azurecli
az acr token delete --name MyToken --registry myregistry
```

Portalda, **Belirteçler (Önizleme)** ekranındaki belirteçleri seçin ve **At'ı**seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Kapsam eşlemlerini ve belirteçlerini yönetmek için [az acr kapsam eşlem][az-acr-scope-map] ve [az acr belirteç][az-acr-token] komut gruplarından ek komutlar kullanın.
* Azure Etkin Dizin kimliği, hizmet yöneticisi veya yönetici hesabı kullanma da dahil olmak üzere, azure kapsayıcı kayıt defteriyle kimlik doğrulaması yapmak için diğer seçenekleriçin [kimlik doğrulama genel ine](container-registry-authentication.md) bakın.


<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-repository]: /cli/azure/acr/repository/
[az-acr-repository-show-tags]: /cli/azure/acr/repository/#az-acr-repository-show-tags
[az-acr-repository-show-manifests]: /cli/azure/acr/repository/#az-acr-repository-show-manifests
[az-acr-repository-delete]: /cli/azure/acr/repository/#az-acr-repository-delete
[az-acr-scope-map]: /cli/azure/acr/scope-map/
[az-acr-scope-map-create]: /cli/azure/acr/scope-map/#az-acr-scope-map-create
[az-acr-scope-map-list]: /cli/azure/acr/scope-map/#az-acr-scope-map-show
[az-acr-scope-map-show]: /cli/azure/acr/scope-map/#az-acr-scope-map-list
[az-acr-scope-map-update]: /cli/azure/acr/scope-map/#az-acr-scope-map-update
[az-acr-scope-map-list]: /cli/azure/acr/scope-map/#az-acr-scope-map-list
[az-acr-token]: /cli/azure/acr/token/
[az-acr-token-show]: /cli/azure/acr/token/#az-acr-token-show
[az-acr-token-list]: /cli/azure/acr/token/#az-acr-token-list
[az-acr-token-delete]: /cli/azure/acr/token/#az-acr-token-delete
[az-acr-token-create]: /cli/azure/acr/token/#az-acr-token-create
[az-acr-token-update]: /cli/azure/acr/token/#az-acr-token-update
[az-acr-token-credential-generate]: /cli/azure/acr/token/credential/#az-acr-token-credential-generate
