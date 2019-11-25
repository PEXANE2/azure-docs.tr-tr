---
title: Tutorial - Trigger image build on base image update
description: In this tutorial, you learn how to configure an Azure Container Registry Task to automatically trigger container image builds in the cloud when a base image is updated.
ms.topic: tutorial
ms.date: 08/12/2019
ms.custom: seodec18, mvc
ms.openlocfilehash: b89bf0364165822368647b4c5b773bf422902aec
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74456134"
---
# <a name="tutorial-automate-container-image-builds-when-a-base-image-is-updated-in-an-azure-container-registry"></a>Tutorial: Automate container image builds when a base image is updated in an Azure container registry 

ACR Görevleri, kapsayıcının temel görüntüsü güncelleştirildiğinde örneğin temel görüntülerinizden birinde işletim sistemine ve uygulama çerçevesine yama uyguladığınızda otomatik derleme yürütmeyi destekler. Bu öğreticide, ACR Görevlerinde bir kapsayıcı temel görüntüsü kayıt defterinize gönderildiğinde bulutta bir görev tetikleyen derleme görevini oluşturmayı öğreneceksiniz.

Bu öğreticide, serinin son kısmı:

> [!div class="checklist"]
> * Temel görüntü oluşturma
> * Uygulama görüntüsü derleme görevi oluşturma
> * Uygulama görüntüsü görevini tetiklemek için temel görüntüyü güncelleştirme
> * Tetiklenen görevi görüntüleme
> * Güncelleştirilmiş uygulama görüntüsünü doğrulama

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI’yı yerel olarak kullanmak istiyorsanız Azure CLI **2.0.46** veya sonraki bir sürüm yüklü olmalıdır. Sürümü bulmak için `az --version` komutunu çalıştırın. If you need to install or upgrade the CLI, see [Install Azure CLI][azure-cli].

## <a name="prerequisites"></a>Önkoşullar

### <a name="complete-the-previous-tutorials"></a>Önceki öğreticileri tamamlama

Bu öğreticide, serinin ilk iki öğreticisindeki adımları zaten tamamladığınız ve şu işlemleri yaptığınız varsayılır:

* Azure kapsayıcı kayıt defteri oluşturma
* Örnek deponun çatalını oluşturma
* Örnek depoyu kopyalama
* GitHub kişisel erişim belirteci oluşturma

Henüz yapmadıysanız, devam etmeden önce ilk iki öğreticiyi tamamlayın:

[Azure Container Registry Görevleri ile bulutta kapsayıcı görüntüleri derleme](container-registry-tutorial-quick-task.md)

[Azure Container Registry Görevleri ile kapsayıcı görüntüsü derlemelerini otomatik hale getirme](container-registry-tutorial-build-task.md)

### <a name="configure-the-environment"></a>Ortamı yapılandırma

Bu kabuk ortam değişkenlerini ortamınıza uygun değerlerle doldurun. Bu adımın yapılması kesinlikle zorunlu değildir ancak bu öğreticideki çok satırlı Azure CLI komutlarını yürütmeyi biraz daha kolaylaştırır. Bu ortam değişkenlerini doldurmazsanız her değeri, örnek komutlarda her göründükleri durumda el ile değiştirmeniz gerekir.

```azurecli-interactive
ACR_NAME=<registry-name>        # The name of your Azure container registry
GIT_USER=<github-username>      # Your GitHub user account name
GIT_PAT=<personal-access-token> # The PAT you generated in the second tutorial
```

## <a name="base-images"></a>Temel görüntüler

Çoğu kapsayıcı görüntüsünü tanımlayan Dockerfile'lar, temel alınan ve çoğunlukla *temel görüntü* olarak adlandırılan bir üst görüntü belirtir. Base images typically contain the operating system, for example [Alpine Linux][base-alpine] or [Windows Nano Server][base-windows], on which the rest of the container's layers are applied. They might also include application frameworks such as [Node.js][base-node] or [.NET Core][base-dotnet].

### <a name="base-image-updates"></a>Temel görüntü güncelleştirmeleri

Temel görüntü çoğunlukla, görüntüdeki işletim sistemi veya çerçevenin yeni özelliklerini veya geliştirmelerini içermesi için görüntü bakımcısı tarafından güncelleştirilir. Temel görüntüyü güncelleştirmenin bir diğer yaygın nedeni de güvenlik yamalarıdır.

Temel görüntü güncelleştirildiğinde, yeni özellik ve düzeltmelerin eklenmesi için kayıt defterinizde bulunan ve bu temel görüntüye dayanan tüm kapsayıcı görüntülerini yeniden oluşturmanız gerektiği belirtilir. ACR Görevleri, kapsayıcının temel görüntüsü güncelleştirildiğinde sizin için görüntüleri otomatik olarak oluşturma özelliğine sahiptir.

### <a name="tasks-triggered-by-a-base-image-update"></a>Tasks triggered by a base image update

* For image builds from a Dockerfile, an ACR task detects dependencies on base images in the following locations:

  * The same Azure container registry where the task runs
  * Another Azure container registry in the same region 
  * A public repo in Docker Hub 
  * A public repo in Microsoft Container Registry

   If the base image specified in the `FROM` statement resides in one of these locations, the ACR task adds a hook to ensure the image is rebuilt any time its base is updated.

* Currently, an ACR tasks only tracks base image updates for application (*runtime*) images. It doesn't track base image updates for intermediate (*buildtime*) images used in multi-stage Dockerfiles.  

* When you create an ACR task with the [az acr task create][az-acr-task-create] command, by default the task is *enabled* for trigger by a base image update. That is, the `base-image-trigger-enabled` property is set to True. If you want to disable this behavior in a task, update the property to False. For example, run the following [az acr task update][az-acr-task-update] command:

  ```azurecli
  az acr task update --myregistry --name mytask --base-image-trigger-enabled False
  ```

* To enable an ACR task to determine and track a container image's dependencies -- which include its base image -- you must first trigger the task **at least once**. For example, trigger the task manually using the [az acr task run][az-acr-task-run] command.

* To trigger a task on base image update, the base image must have a *stable* tag, such as `node:9-alpine`. This tagging is typical for a base image that is updated with OS and framework patches to a latest stable release. If the base image is updated with a new version tag, it does not trigger a task. For more information about image tagging, see the [best practices guidance](container-registry-image-tag-version.md). 

### <a name="base-image-update-scenario"></a>Temel görüntü güncelleştirme senaryosu

Bu öğreticide, bir temel görüntü güncelleştirme senaryosunda size yol gösterilir. The [code sample][code-sample] includes two Dockerfiles: an application image, and an image it specifies as its base. In the following sections, you create an ACR task that automatically triggers a build of the application image when a new version of the base image is pushed to the same container registry.

[Dockerfile-app][dockerfile-app]: A small Node.js web application that renders a static web page displaying the Node.js version on which it's based. Sürüm dizesinin simülasyonu yapılır ve bu, temel görüntüde tanımlanan `NODE_VERSION` ortam değişkeninin içeriğini görüntüler.

[Dockerfile-base][dockerfile-base]: The image that `Dockerfile-app` specifies as its base. It is itself based on a [Node][base-node] image, and includes the `NODE_VERSION` environment variable.

Aşağıdaki bölümlerde bir görev oluşturacak, temel görüntü Dockerfile içinde `NODE_VERSION` değerini güncelleştirecek ve sonra da ACR Görevlerini kullanarak temel görüntü oluşturacaksınız. ACR görevi yeni temel görüntüyü kayıt defterinize gönderdikten sonra, uygulama görüntüsünün derlemesini otomatik olarak tetikler. İsteğe bağlı olarak, derleme görüntülerinde farklı sürüm dizeleri görmek için uygulama kapsayıcısı görüntüsünü yerel olarak çalıştırırsınız.

In this tutorial, your ACR task builds and pushes an application container image specified in a Dockerfile. ACR Tasks can also run [multi-step tasks](container-registry-tasks-multi-step.md), using a YAML file to define steps to build, push, and optionally test multiple containers.

## <a name="build-the-base-image"></a>Temel görüntü oluşturma

Başlangıç olarak ACR Görevleri *hızlı görevi* ile temel görüntüyü oluşturun. Serinin [ilk öğreticisinde](container-registry-tutorial-quick-task.md) açıklandığı gibi, bu işlem yalnızca görüntüyü oluşturmakla kalmaz, oluşturma başarılı olduysa bunu kapsayıcınızın kayıt defterine de gönderir.

```azurecli-interactive
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
```

## <a name="create-a-task"></a>Görev oluşturma

Next, create a task with [az acr task create][az-acr-task-create]:

```azurecli-interactive
az acr task create \
    --registry $ACR_NAME \
    --name taskhelloworld \
    --image helloworld:{{.Run.ID}} \
    --arg REGISTRY_NAME=$ACR_NAME.azurecr.io \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node.git \
    --file Dockerfile-app \
    --git-access-token $GIT_PAT
```

> [!IMPORTANT]
> If you previously created tasks during the preview with the `az acr build-task` command, those tasks need to be re-created using the [az acr task][az-acr-task] command.

Bu görev, [önceki öğreticide](container-registry-tutorial-build-task.md) oluşturulan hızlı göreve benzer. ACR Görevlerine, işlemeler `--context` tarafından belirtilen depoya gönderildiğinde bir görüntü derlemesi tetiklemesini bildirir. While the Dockerfile used to build the image in the previous tutorial specifies a public base image (`FROM node:9-alpine`), the Dockerfile in this task, [Dockerfile-app][dockerfile-app], specifies a base image in the same registry:

```Dockerfile
FROM ${REGISTRY_NAME}/baseimages/node:9-alpine
```

This configuration makes it easy to simulate a framework patch in the base image later in this tutorial.

## <a name="build-the-application-container"></a>Uygulama kapsayıcısını oluşturma

Use [az acr task run][az-acr-task-run] to manually trigger the task and build the application image. This step ensures that the task tracks the application image's dependency on the base image.

```azurecli-interactive
az acr task run --registry $ACR_NAME --name taskhelloworld
```

Görev tamamlandıktan sonra, aşağıdaki isteğe bağlı adımı tamamlamak istiyorsanız **Run ID** (örneğin, "da6") değerini not alın.

### <a name="optional-run-application-container-locally"></a>İsteğe bağlı: Uygulama kapsayıcısını yerel olarak çalıştırma

Cloud Shell'de değil de yerel olarak çalışıyorsanız ve Docker'ı yüklediyseniz, temel görüntüsünü oluşturmadan önce web tarayıcısında işlenen uygulamayı görmek için kapsayıcıyı çalıştırın. Cloud Shell kullanıyorsanız bu bölümü atlayın (Cloud Shell `az acr login` veya `docker run` komutunu desteklemez).

First, authenticate to your container registry with [az acr login][az-acr-login]:

```azurecli
az acr login --name $ACR_NAME
```

Şimdi `docker run` ile kapsayıcıyı yerel olarak çalıştırın. **\<run-id\>** değerini önceki adımdaki çıktıda bulunan Run ID değeriyle (örneğin, "da6") değiştirin. This example names the container `myapp` and includes the `--rm` parameter to remove the container when you stop it.

```bash
docker run -d -p 8080:80 --name myapp --rm $ACR_NAME.azurecr.io/helloworld:<run-id>
```

Tarayıcınızda `http://localhost:8080` adresine gidin; aşağıdakine benzer biçimde web sayfasında işlenmiş Node.js sürüm numarasını görüyor olmalısınız. Sonraki adımlardan birinde, sürüm dizesine bir "a" ekleyerek sürümü yükseltirsiniz.

![Tarayıcıda işlenen örnek uygulamanın ekran görüntüsü][base-update-01]

To stop and remove the container, run the following command:

```bash
docker stop myapp
```

## <a name="list-the-builds"></a>Derlemeleri listeleme

Next, list the task runs that ACR Tasks has completed for your registry using the [az acr task list-runs][az-acr-task-list-runs] command:

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
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
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

To stop and remove the container, run the following command:

```bash
docker stop updatedapp
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kapsayıcı kayıt defteri, kapsayıcı örneğin, anahtar kasası ve hizmet sorumlusu dahil olmak üzere bu öğretici serisinde oluşturduğunuz tüm kaynakları kaldırmak için aşağıdaki komutları gönderin:

```azurecli-interactive
az group delete --resource-group $RES_GROUP
az ad sp delete --id http://$ACR_NAME-pull
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, temel görüntü güncelleştirildiğinde kapsayıcı görüntü derlemelerini otomatik olarak tetiklemek üzere bir görevi kullanmayı öğrendiniz. Now, move on to the next tutorial to learn how to trigger tasks on a defined schedule.

> [!div class="nextstepaction"]
> [Run a task on a schedule](container-registry-tasks-scheduled.md)

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
[az-acr-build]: /cli/azure/acr#az-acr-build-run
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-update]: /cli/azure/acr/task#az-acr-task-update
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-task-list-runs]: /cli/azure/acr
[az-acr-task]: /cli/azure/acr

<!-- IMAGES -->
[base-update-01]: ./media/container-registry-tutorial-base-image-update/base-update-01.png
[base-update-02]: ./media/container-registry-tutorial-base-image-update/base-update-02.png
