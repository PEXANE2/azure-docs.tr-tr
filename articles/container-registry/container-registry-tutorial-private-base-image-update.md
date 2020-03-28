---
title: Öğretici - Özel temel görüntü güncellemesi ile görüntü oluşturmayı tetikleme
description: Bu öğreticide, başka bir özel Azure kapsayıcı kayıt defterindeki temel görüntü güncelleştirildiğinde buluttaki kapsayıcı görüntü yapılarını otomatik olarak tetiklemek için bir Azure Kapsayıcı Kayıt Defteri Görevi yapılandırın.
ms.topic: tutorial
ms.date: 01/22/2020
ms.openlocfilehash: e8aae8a91288d470c801dc4d82cfa6b44369d832
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77617705"
---
# <a name="tutorial-automate-container-image-builds-when-a-base-image-is-updated-in-another-private-container-registry"></a>Öğretici: Bir temel görüntü başka bir özel kapsayıcı kayıt defterinde güncelleştirildiğinde kapsayıcı görüntüsünü otomatikleştirin 

ACR Görevleri, bir kapsayıcının temel [görüntüsü güncelleştirildiğinde](container-registry-tasks-base-images.md)(örneğin, temel görüntülerinizden birinde işletim sistemi veya uygulama çerçevesini yamayaptığınızda) otomatik görüntü oluşturmalarını destekler. 

Bu öğreticide, bir kapsayıcının temel görüntüsü başka bir Azure kapsayıcı kayıt defterine itildiğinde bulutta bir yapıyı tetikleyen bir ACR görevinin nasıl oluşturulacağımı öğrenirsiniz. Bir temel görüntü [aynı Azure kapsayıcı kayıt defterine](container-registry-tutorial-base-image-update.md)itildiğinde görüntü oluşturmayı tetikleyen bir ACR görevi oluşturmak için bir öğretici de deneyebilirsiniz.

Bu eğitimde:

> [!div class="checklist"]
> * Temel kayıt defterinde temel görüntü oluşturma
> * Temel görüntüyü izlemek için başka bir kayıt defterinde uygulama oluşturma görevi oluşturma 
> * Uygulama görüntüsü görevini tetiklemek için temel görüntüyü güncelleştirme
> * Tetiklenen görevi görüntüleme
> * Güncelleştirilmiş uygulama görüntüsünü doğrulama

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI'yi yerel olarak kullanmak istiyorsanız, Azure CLI sürümü **2.0.68** veya daha sonra yüklü olmalıdır. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’yı yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI’yı yükleme][azure-cli].

## <a name="prerequisites"></a>Ön koşullar

### <a name="complete-the-previous-tutorials"></a>Önceki öğreticileri tamamlama

Bu öğreticide, serinin ilk iki öğreticisindeki adımları zaten tamamladığınız ve şu işlemleri yaptığınız varsayılır:

* Azure kapsayıcı kayıt defteri oluşturma
* Örnek deponun çatalını oluşturma
* Örnek depoyu kopyalama
* GitHub kişisel erişim belirteci oluşturma

Bunu daha önce yapmadıysanız, devam etmeden önce aşağıdaki öğreticileri tamamlayın:

[Azure Container Registry Görevleri ile bulutta kapsayıcı görüntüleri derleme](container-registry-tutorial-quick-task.md)

[Azure Container Registry Görevleri ile kapsayıcı görüntüsü derlemelerini otomatik hale getirme](container-registry-tutorial-build-task.md)

Önceki öğreticiler için oluşturulan kapsayıcı kayıt defterine ek olarak, temel görüntüleri depolamak için bir kayıt defteri oluşturmanız gerekir. İstersen, ikinci kayıt defterini özgün kayıt defterinden farklı bir konumda oluşturun.

### <a name="configure-the-environment"></a>Ortamı yapılandırma

Bu kabuk ortam değişkenlerini ortamınıza uygun değerlerle doldurun. Bu adımın yapılması kesinlikle zorunlu değildir ancak bu öğreticideki çok satırlı Azure CLI komutlarını yürütmeyi biraz daha kolaylaştırır. Bu ortam değişkenlerini doldurmazsanız, her değeri örnek komutlarda göründüğü her yerde el ile değiştirmeniz gerekir.

```azurecli-interactive
BASE_ACR=<base-registry-name>   # The name of your Azure container registry for base images
ACR_NAME=<registry-name>        # The name of your Azure container registry for application images
GIT_USER=<github-username>      # Your GitHub user account name
GIT_PAT=<personal-access-token> # The PAT you generated in the second tutorial
```

### <a name="base-image-update-scenario"></a>Temel görüntü güncelleştirme senaryosu

Bu öğreticide, bir temel görüntü güncelleştirme senaryosunda size yol gösterilir. Bu senaryo, diğer kayıt defterlerinde uygulama görüntüleri oluştururken ortak, özel kapsayıcı kayıt defterinde temel görüntüleri yönetmek için bir geliştirme iş akışını yansıtır. Temel görüntüler, bir ekip tarafından kullanılan ortak işletim sistemlerini ve çerçevelerini ve hatta ortak hizmet bileşenlerini belirtebilir.

Örneğin, kendi kayıt defterlerinde uygulama görüntüleri geliştiren geliştiriciler, ortak temel kayıt defterinde tutulan bir dizi temel resime erişebilir. Temel kayıt başka bir bölgede olabilir, hatta coğrafi olarak çoğaltılabilir.

[Kod örneği][code-sample] iki Dockerfile: bir uygulama görüntüsü ve bunun temel olarak belirttiği bir görüntü. Aşağıdaki bölümlerde, temel görüntünün yeni bir sürümü farklı bir Azure kapsayıcı kayıt defterine itildiğinde uygulama görüntüsünün oluşturulmasını otomatik olarak tetikleyen bir ACR görevi oluşturursunuz.

* [Dockerfile-app][dockerfile-app]: Temel alınan Node.js sürümünün görüntülendiği bir statik web sayfası işleyen küçük bir Node.js web uygulaması. Sürüm dizesinin simülasyonu yapılır ve bu, temel görüntüde tanımlanan `NODE_VERSION` ortam değişkeninin içeriğini görüntüler.

* [Dockerfile-base][dockerfile-base]: `Dockerfile-app` tarafından kendi temeli olarak belirtilen görüntü. Bunun kendisi de [Node][base-node] görüntüsünü temel alır ve `NODE_VERSION` ortam değişkenini içerir.

Aşağıdaki bölümlerde bir görev oluşturacak, temel görüntü Dockerfile içinde `NODE_VERSION` değerini güncelleştirecek ve sonra da ACR Görevlerini kullanarak temel görüntü oluşturacaksınız. ACR görevi yeni temel görüntüyü kayıt defterinize gönderdikten sonra, uygulama görüntüsünün derlemesini otomatik olarak tetikler. İsteğe bağlı olarak, derleme görüntülerinde farklı sürüm dizeleri görmek için uygulama kapsayıcısı görüntüsünü yerel olarak çalıştırırsınız.

Bu öğreticide, ACR göreviniz Dockerfile'da belirtilen bir uygulama kapsayıcısı görüntüsünü oluşturur ve iter. ACR Görevleri, birden çok kapsayıcıyı oluşturmak, itmek ve isteğe bağlı olarak sınamak için adımları tanımlamak için bir YAML dosyasını kullanarak [çok aşamalı görevleri](container-registry-tasks-multi-step.md)de çalıştırabilir.

## <a name="build-the-base-image"></a>Temel görüntü oluşturma

[Az acr yapı][az-acr-build]kullanarak, bir ACR Görevleri *hızlı görev*ile temel görüntü oluşturarak başlayın. Serinin [ilk öğreticisinde](container-registry-tutorial-quick-task.md) açıklandığı gibi, bu işlem yalnızca görüntüyü oluşturmakla kalmaz, oluşturma başarılı olduysa bunu kapsayıcınızın kayıt defterine de gönderir. Bu örnekte, görüntü temel görüntü kayıt defterine itilir.

```azurecli-interactive
az acr build --registry $BASE_ACR --image baseimages/node:9-alpine --file Dockerfile-base .
```

## <a name="create-a-task-to-track-the-private-base-image"></a>Özel temel görüntüyü izlemek için görev oluşturma

Ardından, [az acr görev oluşturmak][az-acr-task-create]ile uygulama görüntü kayıt defterinde bir görev oluşturmak , yönetilen bir [kimlik](container-registry-tasks-authentication-managed-identity.md)etkinleştirme . Yönetilen kimlik, görevin temel görüntü kayıt defteriyle doğrulaması için sonraki adımlarda kullanılır. 

Bu örnek, sistem tarafından atanan bir kimlik kullanır, ancak belirli senaryolar için kullanıcı tarafından atanan yönetilen bir kimlik oluşturabilir ve etkinleştirebilirsiniz. Ayrıntılar için, [Azure tarafından yönetilen bir kimlik kullanarak ACR görevinde çapraz kayıt defteri kimlik doğrulaması'na](container-registry-tasks-cross-registry-authentication.md)bakın.

```azurecli-interactive
az acr task create \
    --registry $ACR_NAME \
    --name taskhelloworld \
    --image helloworld:{{.Run.ID}} \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node.git \
    --file Dockerfile-app \
    --git-access-token $GIT_PAT \
    --arg REGISTRY_NAME=$BASE_ACR.azurecr.io \
    --assign-identity
```


Bu görev, [önceki öğreticide](container-registry-tutorial-build-task.md)oluşturulan göreve benzer. ACR Görevlerine, işlemeler `--context` tarafından belirtilen depoya gönderildiğinde bir görüntü derlemesi tetiklemesini bildirir. Dockerfile önceki öğreticide görüntü oluşturmak için kullanılan bir ortak temel`FROM node:9-alpine`görüntü belirtir iken ( ), Bu görevde Dockerfile, [Dockerfile-app][dockerfile-app], temel görüntü kayıt defterinde bir temel görüntü belirtir:

```Dockerfile
FROM ${REGISTRY_NAME}/baseimages/node:9-alpine
```

Bu yapılandırma, daha sonra bu öğreticide temel görüntüdeki bir çerçeve yamasını simüle etmeyi kolaylaştırır.

## <a name="give-identity-pull-permissions-to-base-registry"></a>Temel kayıt defterine kimlik çekme izinleri verme

Görevin yönetilen kimlik izinlerini temel görüntü kayıt defterinden görüntü çekmek için vermek için, ilk olarak kimliğin hizmet temel kimliğini almak için [az acr görev gösteriyi][az-acr-task-show] çalıştırın. Sonra temel kayıt defterinin kaynak kimliğini almak için [az acr göster][az-acr-show] çalıştırın:

```azurecli-interactive
# Get service principal ID of the task
principalID=$(az acr task show --name taskhelloworld --registry $ACR_NAME --query identity.principalId --output tsv) 

# Get resource ID of the base registry
baseregID=$(az acr show --name $BASE_ACR --query id --output tsv) 
```
 
Yönetilen kimlik çekme izinlerini [az rol atamasını][az-role-assignment-create]çalıştırarak kayıt defterine atama:

```azurecli-interactive
az role assignment create \
  --assignee $principalID \
  --scope $baseregID --role acrpull 
```

## <a name="add-target-registry-credentials-to-the-task"></a>Göreve hedef kayıt defteri kimlik bilgilerini ekleme

Göreve kimlik bilgileri eklemek için [az acr görev kimlik bilgilerini][az-acr-task-credential-add] çalıştırın. Görevin `--use-identity [system]` sistem tarafından atanan yönetilen kimliğinin kimlik bilgilerine erişebileceğini belirtmek için parametreyi geçirin.

```azurecli-interactive
az acr task credential add \
  --name taskhelloworld \
  --registry $ACR_NAME \
  --login-server $BASE_ACR.azurecr.io \
  --use-identity [system] 
```

## <a name="manually-run-the-task"></a>Görevi el ile çalıştırın

Görevi el ile tetiklemek ve uygulama görüntüsünü oluşturmak için [az acr görev çalıştır'ı][az-acr-task-run] kullanın. Görevin uygulama görüntüsünün temel görüntüye olan bağımlılığını İzleyebilmek için bu adım gereklidir.

```azurecli-interactive
az acr task run --registry $ACR_NAME --name taskhelloworld
```

Görev tamamlandıktan sonra, aşağıdaki isteğe bağlı adımı tamamlamak istiyorsanız **Run ID** (örneğin, "da6") değerini not alın.

### <a name="optional-run-application-container-locally"></a>İsteğe bağlı: Uygulama kapsayıcısını yerel olarak çalıştırma

Cloud Shell'de değil de yerel olarak çalışıyorsanız ve Docker'ı yüklediyseniz, temel görüntüsünü oluşturmadan önce web tarayıcısında işlenen uygulamayı görmek için kapsayıcıyı çalıştırın. Cloud Shell kullanıyorsanız bu bölümü atlayın (Cloud Shell `az acr login` veya `docker run` komutunu desteklemez).

İlk olarak, [az acr giriş][az-acr-login]ile konteyner kayıt defterine doğrulayın:

```azurecli
az acr login --name $ACR_NAME
```

Şimdi `docker run` ile kapsayıcıyı yerel olarak çalıştırın. ** \<\> Run-id'yi** önceki adımdan çıktıda bulunan Run ID ile değiştirin (örneğin, "da6"). Bu örnek kapsayıcıyı `myapp` adlandırır ve bunu durdurunca kapsayıcıyı kaldırmak için `--rm` parametreiçerir.

```bash
docker run -d -p 8080:80 --name myapp --rm $ACR_NAME.azurecr.io/helloworld:<run-id>
```

Tarayıcınızda `http://localhost:8080` adresine gidin; aşağıdakine benzer biçimde web sayfasında işlenmiş Node.js sürüm numarasını görüyor olmalısınız. Sonraki adımlardan birinde, sürüm dizesine bir "a" ekleyerek sürümü yükseltirsiniz.

![Tarayıcıda oluşturulan örnek uygulamanın ekran görüntüsü][base-update-01]

Kapsayıcıyı durdurmak ve kaldırmak için aşağıdaki komutu çalıştırın:

```bash
docker stop myapp
```

## <a name="list-the-builds"></a>Derlemeleri listeleme

Bu adımda [az acr task list-runs][az-acr-task-list-runs] komutunu kullanarak ACR Görevlerinin kayıt defteriniz için tamamladığı çalıştırmaları listeleyin:

```azurecli-interactive
az acr task list-runs --registry $ACR_NAME --output table
```

Önceki öğreticiyi tamamladıysanız (ve kayıt defterini silmediyseniz), aşağıdakine benzer bir çıkış görüyor olmalısınız. Sonraki bölümde temel görüntüyü güncelleştirdikten sonra çıkışı karşılaştırabilmek için görev sayısını ve en son RUN ID değerini not alın.

```console
$ az acr task list-runs --registry $ACR_NAME --output table

RUN ID    TASK            PLATFORM    STATUS     TRIGGER     STARTED               DURATION
--------  --------------  ----------  ---------  ----------  --------------------  ----------
da6       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T23:07:22Z  00:00:38
da5                       Linux       Succeeded  Manual      2018-09-17T23:06:33Z  00:00:31
da4       taskhelloworld  Linux       Succeeded  Git Commit  2018-09-17T23:03:45Z  00:00:44
da3       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T22:55:35Z  00:00:35
da2       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T22:50:59Z  00:00:32
da1                       Linux       Succeeded  Manual      2018-09-17T22:29:59Z  00:00:57
```

## <a name="update-the-base-image"></a>Temel görüntüyü güncelleştirme

Burada, temel görüntüde bir çerçeve yamasının simülasyonunu yaparsınız. **Dockerfile-base** öğesini düzenleyin ve `NODE_VERSION` içinde tanımlanan sürüm numarasından sonra bir "a" ekleyin:

```Dockerfile
ENV NODE_VERSION 9.11.2a
```

Değiştirilmiş temel görüntüyü derlemek için bir hızlı görev çalıştırın. Çıkıştaki **Run ID** değerini not alın.

```azurecli-interactive
az acr build --registry $BASE_ACR --image baseimages/node:9-alpine --file Dockerfile-base .
```

Derleme tamamlandıktan ve ACR görevi yeni temel görüntüyü kayıt defterinize gönderdikten sonra, uygulama görüntüsünün derlemesini tetikler. Daha önce oluşturduğunuz görevin uygulama görüntüsü derlemeyi tetiklemesi birkaç dakika sürebilir çünkü yeni derlenen ve gönderilen temel görüntüyü algılaması gerekir.

## <a name="list-updated-build"></a>Güncelleştirilmiş derlemeyi listeleme

Artık temel görüntüyü güncelleştirdiğinize göre, görev çalıştırmalarınızı bir kez daha listeleyip önceki listeyle bunu karşılaştırın. İlk seferinde çıkış farklı görünmüyorsa, yeni görev çalıştırmasının listede yer aldığını görmek için komutu düzenli aralıklarla çalıştırın.

```azurecli-interactive
az acr task list-runs --registry $ACR_NAME --output table
```

Çıktı aşağıdakine benzer olacaktır. Son yürütülen derlemenin TRIGGER değeri "Image Update" olmalıdır; bu değer, görevin temel görüntünüzün hızlı görevi ile başlatıldığını gösterir.

```console
$ az acr task list-runs --registry $ACR_NAME --output table

Run ID    TASK            PLATFORM    STATUS     TRIGGER       STARTED               DURATION
--------  --------------  ----------  ---------  ------------  --------------------  ----------
da8       taskhelloworld  Linux       Succeeded  Image Update  2018-09-17T23:11:50Z  00:00:33
da7                       Linux       Succeeded  Manual        2018-09-17T23:11:27Z  00:00:35
da6       taskhelloworld  Linux       Succeeded  Manual        2018-09-17T23:07:22Z  00:00:38
da5                       Linux       Succeeded  Manual        2018-09-17T23:06:33Z  00:00:31
da4       taskhelloworld  Linux       Succeeded  Git Commit    2018-09-17T23:03:45Z  00:00:44
da3       taskhelloworld  Linux       Succeeded  Manual        2018-09-17T22:55:35Z  00:00:35
da2       taskhelloworld  Linux       Succeeded  Manual        2018-09-17T22:50:59Z  00:00:32
da1                       Linux       Succeeded  Manual        2018-09-17T22:29:59Z  00:00:57
```

Yeni oluşturulan kapsayıcıyı çalıştırıp güncelleştirilmiş sürüm numarasını görmek için aşağıdaki isteğe bağlı adımı uygulamak isterseniz, Görüntü Güncelleştirmesi ile tetiklenen derlemenin **RUN ID** değerini (önceki çıktıda "da8") not alın.

### <a name="optional-run-newly-built-image"></a>İsteğe bağlı: Yeni oluşturulan görüntüyü çalıştırma

Cloud Shell'de değil de yerel olarak çalışıyorsanız ve Docker'ı yüklediyseniz, derlemesi tamamlandıktan sonra yeni uygulama görüntüsünü çalıştırın. `<run-id>` değerinin yerine önceki adımda aldığınız RUN ID değerini koyun. Cloud Shell kullanıyorsanız bu bölümü atlayın (Cloud Shell `docker run` komutunu desteklemez).

```bash
docker run -d -p 8081:80 --name updatedapp --rm $ACR_NAME.azurecr.io/helloworld:<run-id>
```

Tarayıcınızda http://localhost:8081 adresine gidin; web sayfasında güncelleştirilmiş Node.js sürüm numarasını ("a" ile) görüyor olmalısınız:

![Tarayıcıda oluşturulan örnek uygulamanın ekran görüntüsü][base-update-02]

**Temel** görüntünüzü yeni sürüm numarasıyla güncelleştirdiğinize ama son oluşturulan **uygulama** görüntüsünde yeni sürümün görüntülendiğine dikkat etmelisiniz. ACR Görevler temel görüntüde yaptığınız değişikliği almış ve uygulama görüntünüzü otomatik olarak yeniden oluşturmuştur.

Kapsayıcıyı durdurmak ve kaldırmak için aşağıdaki komutu çalıştırın:

```bash
docker stop updatedapp
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, temel görüntü güncelleştirildiğinde kapsayıcı görüntü derlemelerini otomatik olarak tetiklemek üzere bir görevi kullanmayı öğrendiniz. Şimdi, görevleri tanımlı bir zamanlamada nasıl tetikleyecek öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Görev zamanlamada çalıştır](container-registry-tasks-scheduled.md)

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[code-sample]: https://github.com/Azure-Samples/acr-build-helloworld-node
[dockerfile-app]: https://github.com/Azure-Samples/acr-build-helloworld-node/blob/master/Dockerfile-app
[dockerfile-base]: https://github.com/Azure-Samples/acr-build-helloworld-node/blob/master/Dockerfile-base

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-update]: /cli/azure/acr/task#az-acr-task-update
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show
[az-acr-task-credential-add]: /cli/azure/acr/task/credential#az-acr-task-credential-add
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-task-list-runs]: /cli/azure/acr/task#az-acr-task-list-runs
[az-acr-task]: /cli/azure/acr#az-acr-task
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create

<!-- IMAGES -->
[base-update-01]: ./media/container-registry-tutorial-base-image-update/base-update-01.png
[base-update-02]: ./media/container-registry-tutorial-base-image-update/base-update-02.png
