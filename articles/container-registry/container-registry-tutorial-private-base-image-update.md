---
title: Öğretici-özel temel görüntü güncelleştirmesine göre görüntü oluşturmayı Tetikle
description: Bu öğreticide, başka bir özel Azure Container Registry 'de bir temel görüntü güncelleştirilirken buluttaki kapsayıcı görüntüsü yapılarını otomatik olarak tetiklemek üzere bir Azure Container Registry görevi yapılandırırsınız.
ms.topic: tutorial
ms.date: 01/22/2020
ms.openlocfilehash: e8aae8a91288d470c801dc4d82cfa6b44369d832
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77617705"
---
# <a name="tutorial-automate-container-image-builds-when-a-base-image-is-updated-in-another-private-container-registry"></a>Öğretici: bir temel görüntü başka bir özel kapsayıcı kayıt defterinde güncelleştirildiği zaman kapsayıcı görüntüsü derlemelerini otomatikleştirin 

ACR görevleri, bir kapsayıcının [temel görüntüsü](container-registry-tasks-base-images.md)güncelleştirilirken (örneğin, işletim sistemi veya uygulama çerçevesini temel görüntülerinizden birinde yamanız gibi) otomatik görüntü derlemelerini destekler. 

Bu öğreticide, bir kapsayıcının temel görüntüsü başka bir Azure Container Registry 'ye gönderildiğinde buluttaki bir derlemeyi tetikleyen bir ACR görevi oluşturma hakkında bilgi edineceksiniz. Ayrıca, bir temel görüntü [aynı Azure Container Registry](container-registry-tutorial-base-image-update.md)'ye gönderildiğinde görüntü derlemesini tetikleyen bir ACR görevi oluşturma öğreticisini deneyebilirsiniz.

Bu öğreticide:

> [!div class="checklist"]
> * Temel görüntüyü temel bir kayıt defterinde oluşturma
> * Temel görüntüyü izlemek için başka bir kayıt defterinde uygulama derleme görevi oluşturma 
> * Uygulama görüntüsü görevini tetiklemek için temel görüntüyü güncelleştirme
> * Tetiklenen görevi görüntüleme
> * Güncelleştirilmiş uygulama görüntüsünü doğrulama

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLı 'yı yerel olarak kullanmak istiyorsanız, Azure CLı sürüm **2.0.68** veya sonraki bir sürümün yüklü olması gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. CLı 'yi yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI 'Yı yüklemek][azure-cli].

## <a name="prerequisites"></a>Önkoşullar

### <a name="complete-the-previous-tutorials"></a>Önceki öğreticileri tamamlama

Bu öğreticide, serinin ilk iki öğreticisindeki adımları zaten tamamladığınız ve şu işlemleri yaptığınız varsayılır:

* Azure kapsayıcı kayıt defteri oluşturma
* Örnek deponun çatalını oluşturma
* Örnek depoyu kopyalama
* GitHub kişisel erişim belirteci oluşturma

Daha önce yapmadıysanız, devam etmeden önce aşağıdaki öğreticilerini doldurun:

[Azure Container Registry Görevleri ile bulutta kapsayıcı görüntüleri derleme](container-registry-tutorial-quick-task.md)

[Azure Container Registry Görevleri ile kapsayıcı görüntüsü derlemelerini otomatik hale getirme](container-registry-tutorial-build-task.md)

Önceki öğreticiler için oluşturulan kapsayıcı kayıt defterine ek olarak, temel görüntüleri depolamak için bir kayıt defteri oluşturmanız gerekir. İsterseniz, ikinci kayıt defterini özgün kayıt defterinden farklı bir konumda oluşturun.

### <a name="configure-the-environment"></a>Ortamı yapılandırma

Bu kabuk ortam değişkenlerini ortamınıza uygun değerlerle doldurun. Bu adımın yapılması kesinlikle zorunlu değildir ancak bu öğreticideki çok satırlı Azure CLI komutlarını yürütmeyi biraz daha kolaylaştırır. Bu ortam değişkenlerini doldurmazsanız, her değeri örnek komutlarda göründüğü her yerde el ile değiştirmelisiniz.

```azurecli-interactive
BASE_ACR=<base-registry-name>   # The name of your Azure container registry for base images
ACR_NAME=<registry-name>        # The name of your Azure container registry for application images
GIT_USER=<github-username>      # Your GitHub user account name
GIT_PAT=<personal-access-token> # The PAT you generated in the second tutorial
```

### <a name="base-image-update-scenario"></a>Temel görüntü güncelleştirme senaryosu

Bu öğreticide, bir temel görüntü güncelleştirme senaryosunda size yol gösterilir. Bu senaryo, diğer kayıt defterlerinde uygulama görüntülerini oluştururken ortak ve özel kapsayıcı kayıt defterindeki temel görüntüleri yönetmek için bir geliştirme iş akışını yansıtır. Temel görüntüler, bir takım veya hatta ortak hizmet bileşenleri tarafından kullanılan ortak işletim sistemlerini ve çerçeveleri belirtebilir.

Örneğin, uygulama görüntülerini kendi kayıt defterlerine geliştiren geliştiriciler ortak temel kayıt defterinde tutulan bir dizi temel görüntü erişimine erişebilir. Temel kayıt defteri başka bir bölgede veya hatta coğrafi olarak çoğaltılan olabilir.

[Kod örneği][code-sample] , Iki Dockerfiles içerir: bir uygulama görüntüsü ve temeli olarak belirttiği bir görüntü. Aşağıdaki bölümlerde, temel görüntünün yeni bir sürümü farklı bir Azure Container Registry 'ye gönderildiğinde, uygulama görüntüsünün derlemesini otomatik olarak tetikleyen bir ACR görevi oluşturacaksınız.

* [Dockerfile-App][dockerfile-app]: temel aldığı Node. js sürümünü görüntüleyen statik bir Web sayfası işleyen küçük bir Node. js web uygulaması. Sürüm dizesinin simülasyonu yapılır ve bu, temel görüntüde tanımlanan `NODE_VERSION` ortam değişkeninin içeriğini görüntüler.

* [Dockerfile-Base][dockerfile-base]: `Dockerfile-app` temel olarak belirttiği görüntü. Kendisi bir [düğüm][base-node] görüntüsünü temel alır ve `NODE_VERSION` ortam değişkenini içerir.

Aşağıdaki bölümlerde bir görev oluşturacak, temel görüntü Dockerfile içinde `NODE_VERSION` değerini güncelleştirecek ve sonra da ACR Görevlerini kullanarak temel görüntü oluşturacaksınız. ACR görevi yeni temel görüntüyü kayıt defterinize gönderdikten sonra, uygulama görüntüsünün derlemesini otomatik olarak tetikler. İsteğe bağlı olarak, derleme görüntülerinde farklı sürüm dizeleri görmek için uygulama kapsayıcısı görüntüsünü yerel olarak çalıştırırsınız.

Bu öğreticide, ACR göreviniz bir Dockerfile dosyasında belirtilen bir uygulama kapsayıcısı görüntüsünü oluşturur ve gönderir. ACR görevleri, birden çok kapsayıcıyı oluşturma, gönderme ve isteğe bağlı olarak test etme adımlarını tanımlamak için bir YAML dosyası kullanarak [çok adımlı görevler](container-registry-tasks-multi-step.md)de çalıştırabilir.

## <a name="build-the-base-image"></a>Temel görüntü oluşturma

[Az ACR Build][az-acr-build]kullanarak bir ACR görevler *hızlı göreviyle*temel görüntü oluşturarak başlayın. Serinin [ilk öğreticisinde](container-registry-tutorial-quick-task.md) açıklandığı gibi, bu işlem yalnızca görüntüyü oluşturmakla kalmaz, oluşturma başarılı olduysa bunu kapsayıcınızın kayıt defterine de gönderir. Bu örnekte görüntü, temel görüntü kayıt defterine gönderilir.

```azurecli-interactive
az acr build --registry $BASE_ACR --image baseimages/node:9-alpine --file Dockerfile-base .
```

## <a name="create-a-task-to-track-the-private-base-image"></a>Özel temel görüntüyü izlemek için bir görev oluşturma

Daha sonra, bir [yönetilen kimlik](container-registry-tasks-authentication-managed-identity.md)etkinleştirerek, [az ACR görev Create][az-acr-task-create]ile uygulama görüntüsü kayıt defterinde bir görev oluşturun. Yönetilen kimlik, görevin temel görüntü kayıt defteriyle kimlik doğrulamasını yapmak için sonraki adımlarda kullanılır. 

Bu örnek, sistem tarafından atanan bir kimlik kullanır, ancak belirli senaryolar için Kullanıcı tarafından atanan bir yönetilen kimlik oluşturabilir ve etkinleştirebilirsiniz. Ayrıntılar için bkz. [Azure tarafından yönetilen kimlik kullanarak ACR görevinde çapraz kayıt defteri kimlik doğrulaması](container-registry-tasks-cross-registry-authentication.md).

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


Bu görev, [önceki öğreticide](container-registry-tutorial-build-task.md)oluşturulan görevle benzerdir. ACR Görevlerine, işlemeler `--context` tarafından belirtilen depoya gönderildiğinde bir görüntü derlemesi tetiklemesini bildirir. Önceki öğreticide görüntüyü oluşturmak için kullanılan Dockerfile, genel bir temel görüntü (`FROM node:9-alpine`), bu görevde Dockerfile, [dockerfile-App][dockerfile-app], temel görüntü kayıt defterinde bir temel görüntü belirtir:

```Dockerfile
FROM ${REGISTRY_NAME}/baseimages/node:9-alpine
```

Bu yapılandırma, bu öğreticide daha sonra temel görüntüde bir çerçeve düzeltme ekinin benzetimini yapmayı kolaylaştırır.

## <a name="give-identity-pull-permissions-to-base-registry"></a>Temel kayıt defterine kimlik çekme izinleri verme

Temel görüntü kayıt defterinden görüntüleri çekmek için görevin yönetilen kimlik izinlerini vermek üzere, kimliğin hizmet sorumlusu KIMLIĞINI almak için [az ACR görev göster][az-acr-task-show] ' i çalıştırın. Ardından, temel kayıt defterinin kaynak KIMLIĞINI almak için [az ACR Show][az-acr-show] ' ı çalıştırın:

```azurecli-interactive
# Get service principal ID of the task
principalID=$(az acr task show --name taskhelloworld --registry $ACR_NAME --query identity.principalId --output tsv) 

# Get resource ID of the base registry
baseregID=$(az acr show --name $BASE_ACR --query id --output tsv) 
```
 
Yönetilen kimlik çekme izinlerini kayıt defterine atamak için [az role atama Create][az-role-assignment-create]: ' i çalıştırın.

```azurecli-interactive
az role assignment create \
  --assignee $principalID \
  --scope $baseregID --role acrpull 
```

## <a name="add-target-registry-credentials-to-the-task"></a>Göreve hedef kayıt defteri kimlik bilgilerini ekleyin

Göreve kimlik bilgileri eklemek için [az ACR Task Credential Add][az-acr-task-credential-add] ' i çalıştırın. Görevin sistem tarafından atanan yönetilen kimliğin kimlik bilgilerine erişemeyeceğini belirtmek için `--use-identity [system]` parametresini geçirin.

```azurecli-interactive
az acr task credential add \
  --name taskhelloworld \
  --registry $ACR_NAME \
  --login-server $BASE_ACR.azurecr.io \
  --use-identity [system] 
```

## <a name="manually-run-the-task"></a>Görevi el ile çalıştırın

Görevi el ile tetiklemek ve uygulama görüntüsünü derlemek için [az ACR görev çalıştırmasını][az-acr-task-run] kullanın. Görevin, temel görüntüde uygulama görüntüsünün bağımlılığını izlemesi için bu adım gereklidir.

```azurecli-interactive
az acr task run --registry $ACR_NAME --name taskhelloworld
```

Görev tamamlandıktan sonra, aşağıdaki isteğe bağlı adımı tamamlamak istiyorsanız **Run ID** (örneğin, "da6") değerini not alın.

### <a name="optional-run-application-container-locally"></a>İsteğe bağlı: Uygulama kapsayıcısını yerel olarak çalıştırma

Cloud Shell'de değil de yerel olarak çalışıyorsanız ve Docker'ı yüklediyseniz, temel görüntüsünü oluşturmadan önce web tarayıcısında işlenen uygulamayı görmek için kapsayıcıyı çalıştırın. Cloud Shell kullanıyorsanız bu bölümü atlayın (Cloud Shell `az acr login` veya `docker run` komutunu desteklemez).

İlk olarak, [az ACR oturum açma][az-acr-login]ile kapsayıcı kayıt defterinizde kimlik doğrulaması yapın:

```azurecli
az acr login --name $ACR_NAME
```

Şimdi `docker run` ile kapsayıcıyı yerel olarak çalıştırın. **\<run-id\>** değerini önceki adımdaki çıktıda bulunan Run ID değeriyle (örneğin, "da6") değiştirin. Bu örnek, kapsayıcıyı `myapp` adlandırır ve `--rm` parametresini, durdurduğunuzda kapsayıcıyı kaldırmak için içerir.

```bash
docker run -d -p 8080:80 --name myapp --rm $ACR_NAME.azurecr.io/helloworld:<run-id>
```

Tarayıcınızda `http://localhost:8080` adresine gidin; aşağıdakine benzer biçimde web sayfasında işlenmiş Node.js sürüm numarasını görüyor olmalısınız. Sonraki adımlardan birinde, sürüm dizesine bir "a" ekleyerek sürümü yükseltirsiniz.

![Tarayıcıda işlenen örnek uygulamanın ekran görüntüsü][base-update-01]

Kapsayıcıyı durdurmak ve kaldırmak için aşağıdaki komutu çalıştırın:

```bash
docker stop myapp
```

## <a name="list-the-builds"></a>Derlemeleri listeleme

Ardından, [az ACR Task List-çalıştırmaları][az-acr-task-list-runs] komutunu kullanarak kayıt defteriniz için ACR görevlerinin tamamladığı görevi listeleyin:

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

![Tarayıcıda işlenen örnek uygulamanın ekran görüntüsü][base-update-02]

**Temel** görüntünüzü yeni sürüm numarasıyla güncelleştirdiğinize ama son oluşturulan **uygulama** görüntüsünde yeni sürümün görüntülendiğine dikkat etmelisiniz. ACR Görevler temel görüntüde yaptığınız değişikliği almış ve uygulama görüntünüzü otomatik olarak yeniden oluşturmuştur.

Kapsayıcıyı durdurmak ve kaldırmak için aşağıdaki komutu çalıştırın:

```bash
docker stop updatedapp
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, temel görüntü güncelleştirildiğinde kapsayıcı görüntü derlemelerini otomatik olarak tetiklemek üzere bir görevi kullanmayı öğrendiniz. Şimdi, tanımlı bir zamanlamaya göre görevleri nasıl tetikleyeceğinizi öğrenmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Bir görevi zamanlamaya göre çalıştırma](container-registry-tasks-scheduled.md)

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
