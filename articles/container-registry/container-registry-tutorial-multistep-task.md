---
title: Öğretici - Çok aşamalı ACR görevi
description: Bu öğreticide, kaynak kodunu git deposuna işlediğinizde bulutta kapsayıcı görüntüleri oluşturmak, çalıştırmak ve itmek için çok aşamalı bir iş akışını otomatik olarak tetikleyecek bir Azure Kapsayıcı Kayıt Defteri Görevi'ni nasıl yapılandırabileceğinizi öğrenirsiniz.
ms.topic: tutorial
ms.date: 05/09/2019
ms.custom: seodec18, mvc
ms.openlocfilehash: ff32b3095638af6b2b246b99a5dc9219e0020782
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78402309"
---
# <a name="tutorial-run-a-multi-step-container-workflow-in-the-cloud-when-you-commit-source-code"></a>Öğretici: Kaynak kodu işlerken bulutta çok adımlı kapsayıcı iş akışı çalıştırma

Hızlı bir [göreve](container-registry-tutorial-quick-task.md)ek olarak, ACR Görevleri, kaynak kodunu git deposuna işlediğinizde otomatik olarak tetiklenebilen çok adımlı, çok kapsayıcı tabanlı iş akışlarını destekler. 

Bu öğreticide, kaynak kodu işlediğizde bir veya daha fazla kapsayıcı görüntüsünü oluşturan, çalıştıran ve bir kayıt defterine iten çok aşamalı görevleri tanımlamak için örnek YAML dosyalarını nasıl kullanacağınızı öğrenirsiniz. Yalnızca kod işleme üzerine inşa edilen tek bir görüntüyü otomatikleştiren bir görev oluşturmak için [bkz.](container-registry-tutorial-build-task.md) ACR Görevleri'ne genel bir bakış için, [ACR Görevleri ile Işletim Sistemi ve çerçeve düzeltme bölümüne](container-registry-tasks-overview.md)bakın,

Bu eğitimde:

> [!div class="checklist"]
> * YAML dosyalarını kullanarak çok aşamalı bir görev tanımlama
> * Görev oluşturma
> * Başka bir kayıt defterine erişimi etkinleştirmek için isteğe bağlı olarak göreve kimlik bilgileri ekleyin
> * Görevi test etme
> * Görev durumunu görüntüleme
> * Kod işlemesi ile görevi tetikleme

Bu öğreticide, [önceki öğreticide](container-registry-tutorial-quick-task.md) yer alan adımları zaten tamamladığınız varsayılır. Henüz yapmadıysanız, devam etmeden önce önceki öğreticinin [Önkoşullar](container-registry-tutorial-quick-task.md#prerequisites) bölümündeki adımları tamamlayın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI'yi yerel olarak kullanmak istiyorsanız, Azure CLI sürüm **2.0.62** veya daha sonra az [girişi][az-login]ile yüklenmiş ve oturum açmış olmalıdır. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’yı yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI’yı yükleme][azure-cli].

[!INCLUDE [container-registry-task-tutorial-prereq.md](../../includes/container-registry-task-tutorial-prereq.md)]

## <a name="create-a-multi-step-task"></a>Çok adımlı bir görev oluşturma

Artık ACR Görevlerinin commit status'u okumasını ve bir depoda web hooks oluşturmasına olanak sağlamak için gereken adımları tamamladığınızda, kapsayıcı görüntüsünü oluşturmayı, çalıştırmayı ve itmenizi tetikleyen çok aşamalı bir görev oluşturun.

### <a name="yaml-file"></a>YAML dosyası

Yaml [dosyasında](container-registry-tasks-reference-yaml.md)çok adımlı bir görevin adımlarını tanımlarsınız. Bu öğretici için ilk örnek çok adımlı `taskmulti.yaml`görev, klonladığınız GitHub repo'nun kökünde bulunan dosyada tanımlanır:

```yml
version: v1.0.0
steps:
# Build target image
- build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} -f Dockerfile .
# Run image 
- cmd: -t {{.Run.Registry}}/hello-world:{{.Run.ID}}
  id: test
  detach: true
  ports: ["8080:80"]
- cmd: docker stop test
# Push image
- push:
  - {{.Run.Registry}}/hello-world:{{.Run.ID}}
```

Bu çok adımlı görev aşağıdakileri yapar:

1. Çalışma `build` dizinindeki Dockerfile'den bir görüntü oluşturmak için bir adım çalıştırın. `Run.Registry`Görüntü, görevin çalıştırıldığı kayıt defterini hedefler ve benzersiz bir ACR Görevleri çalıştırılaç kimliğiyle etiketlenir. 
1. Görüntüyü `cmd` geçici bir kapsayıcıda çalıştırmak için bir adım çalıştırın. Bu örnek, arka planda uzun süren bir kapsayıcı yı başlatır ve kapsayıcı kimliğini döndürür, ardından kapsayıcıyı durdurur. Gerçek bir senaryoda, doğru çalıştığından emin olmak için çalışan kapsayıcıyı sınamak için adımlar ekleyebilirsiniz.
1. Bir `push` adımda, yapıldaki görüntüyü çalıştır kayıt defterine iter.

### <a name="task-command"></a>Görev komutu

İlk olarak, bu kabuk ortam değişkenlerini ortamınıza uygun değerlerle doldurun. Bu adımın yapılması kesinlikle zorunlu değildir ancak bu öğreticideki çok satırlı Azure CLI komutlarını yürütmeyi biraz daha kolaylaştırır. Bu ortam değişkenlerini doldurmazsanız, her değeri örnek komutlarda göründüğü her yerde el ile değiştirmeniz gerekir.

[![Gömme başlatma](https://shell.azure.com/images/launchcloudshell.png "Azure Cloud Shell'i başlatma")](https://shell.azure.com)

```console
ACR_NAME=<registry-name>        # The name of your Azure container registry
GIT_USER=<github-username>      # Your GitHub user account name
GIT_PAT=<personal-access-token> # The PAT you generated in the previous section
```

Şimdi, aşağıdaki [az acr görev oluşturma][az-acr-task-create] komutunu çalıştırarak görev oluşturun:

```azurecli-interactive
az acr task create \
    --registry $ACR_NAME \
    --name example1 \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node.git \
    --file taskmulti.yaml \
    --git-access-token $GIT_PAT
```

Bu görev, herhangi bir zaman kodunun, `--context`acr Görevler tarafından belirtilen depodaki *ana* dala adandığını belirtir, ACR Görevleri bu daldaki koddan çok aşamalı görevi çalıştıracaktır. Depo `--file` kökünden belirtilen YAML dosyası adımları tanımlar. 

Başarılı bir [az acr task create][az-acr-task-create] komutundaki çıktı aşağıdakilere benzer:

```output
{
  "agentConfiguration": {
    "cpu": 2
  },
  "creationDate": "2019-05-03T03:14:31.763887+00:00",
  "credentials": null,
  "id": "/subscriptions/<Subscription ID>/resourceGroups/myregistry/providers/Microsoft.ContainerRegistry/registries/myregistry/tasks/taskmulti",
  "location": "westus",
  "name": "example1",
  "platform": {
    "architecture": "amd64",
    "os": "linux",
    "variant": null
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "status": "Enabled",
  "step": {
    "baseImageDependencies": null,
    "contextAccessToken": null,
    "contextPath": "https://github.com/gituser/acr-build-helloworld-node.git",
    "taskFilePath": "taskmulti.yaml",
    "type": "FileTask",
    "values": [],
    "valuesFilePath": null
  },
  "tags": null,
  "timeout": 3600,
  "trigger": {
    "baseImageTrigger": {
      "baseImageTriggerType": "Runtime",
      "name": "defaultBaseimageTriggerName",
      "status": "Enabled"
    },
    "sourceTriggers": [
      {
        "name": "defaultSourceTriggerName",
        "sourceRepository": {
          "branch": "master",
          "repositoryUrl": "https://github.com/gituser/acr-build-helloworld-node.git",
          "sourceControlAuthProperties": null,
          "sourceControlType": "Github"
        },
        "sourceTriggerEvents": [
          "commit"
        ],
        "status": "Enabled"
      }
    ]
  },
  "type": "Microsoft.ContainerRegistry/registries/tasks"
}
```

## <a name="test-the-multi-step-workflow"></a>Çok aşamalı iş akışını test edin

Çok adımlı görevi sınamak için [az acr görev çalıştırma][az-acr-task-run] komutunu çalıştırarak el ile tetikle:

```azurecli-interactive
az acr task run --registry $ACR_NAME --name example1
```

Varsayılan olarak, `az acr task run` komutunu yürüttüğünüzde komut, günlük çıktısını konsolunuza akışla aktarır. Çıktı, görev adımlarının her birinin çalıştırıldının ilerlemesini gösterir. Aşağıdaki çıktı önemli adımları göstermek için yoğuşturulur.

```output
Queued a run with ID: cf19
Waiting for an agent...
2019/05/03 03:03:31 Downloading source code...
2019/05/03 03:03:33 Finished downloading source code
2019/05/03 03:03:33 Using acb_vol_cfe6bd55-3076-4215-8091-6a81aec3d1b1 as the home volume
2019/05/03 03:03:33 Creating Docker network: acb_default_network, driver: 'bridge'
2019/05/03 03:03:34 Successfully set up Docker network: acb_default_network
2019/05/03 03:03:34 Setting up Docker configuration...
2019/05/03 03:03:34 Successfully set up Docker configuration
2019/05/03 03:03:34 Logging in to registry: myregistry.azurecr.io
2019/05/03 03:03:35 Successfully logged into myregistry.azurecr.io
2019/05/03 03:03:35 Executing step ID: acb_step_0. Working directory: '', Network: 'acb_default_network'
2019/05/03 03:03:35 Scanning for dependencies...
2019/05/03 03:03:36 Successfully scanned dependencies
2019/05/03 03:03:36 Launching container with name: acb_step_0
Sending build context to Docker daemon  24.06kB

[...]

Successfully built f669bfd170af
Successfully tagged myregistry.azurecr.io/hello-world:cf19
2019/05/03 03:03:43 Successfully executed container: acb_step_0
2019/05/03 03:03:43 Executing step ID: acb_step_1. Working directory: '', Network: 'acb_default_network'
2019/05/03 03:03:43 Launching container with name: acb_step_1
279b1cb6e092b64c8517c5506fcb45494cd5a0bd10a6beca3ba97f25c5d940cd
2019/05/03 03:03:44 Successfully executed container: acb_step_1
2019/05/03 03:03:44 Executing step ID: acb_step_2. Working directory: '', Network: 'acb_default_network'
2019/05/03 03:03:44 Pushing image: myregistry.azurecr.io/hello-world:cf19, attempt 1

[...]

2019/05/03 03:03:46 Successfully pushed image: myregistry.azurecr.io/hello-world:cf19
2019/05/03 03:03:46 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 7.425169)
2019/05/03 03:03:46 Populating digests for step ID: acb_step_0...
2019/05/03 03:03:47 Successfully populated digests for step ID: acb_step_0
2019/05/03 03:03:47 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 0.827129)
2019/05/03 03:03:47 Step ID: acb_step_2 marked as successful (elapsed time in seconds: 2.112113)
2019/05/03 03:03:47 The following dependencies were found:
2019/05/03 03:03:47
- image:
    registry: myregistry.azurecr.io
    repository: hello-world
    tag: cf19
    digest: sha256:6b981a8ca8596e840228c974c929db05c0727d8630465de536be74104693467a
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 9-alpine
    digest: sha256:8dafc0968fb4d62834d9b826d85a8feecc69bd72cd51723c62c7db67c6dec6fa
  git:
    git-head-revision: 1a3065388a0238e52865db1c8f3e97492a43444c

Run ID: cf19 was successful after 18s
```

## <a name="trigger-a-build-with-a-commit"></a>İşleme ile derleme tetikleme

Görevi el ile çalıştırarak test ettikten sonra, bir kaynak kodu değişikliği ile otomatik olarak tetikleyin.

İlk olarak, [depo][sample-repo] yerel kopyanızı içeren dizinde olduğunuzdan emin olun:

```console
cd acr-build-helloworld-node
```

Ardından, yeni bir dosya oluşturmak, işlemek ve GitHub üzerindeki depo çatalınıza göndermek için aşağıdaki komutları yürütün:

```console
echo "Hello World!" > hello.txt
git add hello.txt
git commit -m "Testing ACR Tasks"
git push origin master
```

`git push` komutunu yürüttüğünüzde GitHub kimlik bilgilerinizi sağlamanız istenebilir. GitHub kullanıcı adınızı sağlayın ve parola için daha önce oluşturduğunuz kişisel erişim belirtecini (PAT) girin.

```azurecli-interactive
Username for 'https://github.com': <github-username>
Password for 'https://githubuser@github.com': <personal-access-token>
```

Deponuza bir taahhütte bulunundıktan sonra, ACR Görevler tarafından oluşturulan web hook, Azure Konteyner Kayıt Defteri'nde devreyi kapatır ve başlatAr. Derlemenin ilerleme durumunu doğrulamak ve izlemek için o anda devam eden görevin günlüklerini görüntüleyin:

```azurecli-interactive
az acr task logs --registry $ACR_NAME
```

Çıktı aşağıdakine benzer ve o anda yürütülen (veya son yürütülen) görevi gösterir:

```output
Showing logs of the last created run.
Run ID: cf1d

[...]

Run ID: cf1d was successful after 37s
```

## <a name="list-builds"></a>Derlemeleri listeleme

[az acr task list-runs][az-acr-task-list-runs] komutunu çalıştırarak ACR Görevlerinin kayıt defteriniz için tamamladığı çalıştırmaları listeleyebilirsiniz:

```azurecli-interactive
az acr task list-runs --registry $ACR_NAME --output table
```

Komut çıktısı aşağıdakine benzer şekilde görünmelidir. ACR Görevlerinin yürüttüğü çalıştırmalar gösterilir ve en son görev için TRIGGER sütununda "Git İşleme" ifadesi görünür:

```output
RUN ID    TASK       PLATFORM    STATUS     TRIGGER    STARTED               DURATION
--------  ---------  ----------  ---------  ---------  --------------------  ----------
cf1d      example1   linux       Succeeded  Commit     2019-05-03T04:16:44Z  00:00:37
cf1c      example1   linux       Succeeded  Commit     2019-05-03T04:16:44Z  00:00:39
cf1b      example1   linux       Succeeded  Manual     2019-05-03T03:10:30Z  00:00:31
cf1a      example1   linux       Succeeded  Commit     2019-05-03T03:09:32Z  00:00:31
cf19      example1   linux       Succeeded  Manual     2019-05-03T03:03:30Z  00:00:21
```

## <a name="create-a-multi-registry-multi-step-task"></a>Çok kayıt defteri çok adımlı görev oluşturma

ACR Görevleri varsayılan olarak, görevin çalıştığı kayıt defterinden görüntüleri itme veya çekme izinlerine sahiptir. Run kayıt defterine ek olarak bir veya daha fazla kayıt defterini hedefleyen çok aşamalı bir görev çalıştırmak isteyebilirsiniz. Örneğin, bir kayıt defterinde görüntüler oluşturmanız ve görüntüleri üretim sistemi tarafından erişilen ikinci bir kayıt defterinde farklı etiketlerle depolamanız gerekebilir. Bu örnek, böyle bir görevoluşturmak ve başka bir kayıt için kimlik bilgileri sağlamak nasıl gösterir.

Zaten ikinci bir kayıt defteriniz yoksa, bu örnek için bir tane oluşturun. Bir kayıt defterine ihtiyacınız varsa, [önceki öğreticiye](container-registry-tutorial-quick-task.md) veya [Hızlı Başlangıç: Azure CLI kullanarak kapsayıcı kayıt defteri oluşturma](container-registry-get-started-azure-cli.md) bölümüne bakın.

Görevi oluşturmak için, *mycontainerregistrydate.azurecr.io* formu (tüm küçük harf) olan kayıt defteri giriş sunucusunun adını almanız gerekir. Bu örnekte, yapı tarihine göre etiketlenmiş görüntüleri depolamak için ikinci kayıt defterini kullanırsınız.

### <a name="yaml-file"></a>YAML dosyası

Bu öğretici için ikinci örnek çok adımlı `taskmulti-multiregistry.yaml`görev, klonladığınız GitHub repo'nun kökünde bulunan dosyada tanımlanır:

```yml
version: v1.0.0
steps:
# Build target images
- build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} -f Dockerfile .
- build: -t {{.Values.regDate}}/hello-world:{{.Run.Date}} -f Dockerfile .
# Run image 
- cmd: -t {{.Run.Registry}}/hello-world:{{.Run.ID}}
  id: test
  detach: true
  ports: ["8080:80"]
- cmd: docker stop test
# Push images
- push:
  - {{.Run.Registry}}/hello-world:{{.Run.ID}}
  - {{.Values.regDate}}/hello-world:{{.Run.Date}}
```

Bu çok adımlı görev aşağıdakileri yapar:

1. Çalışma `build` dizininde Dockerfile'den görüntüler oluşturmak için iki adım çalışır:
    * İlk hedef `Run.Registry`, görevin çalıştırıldığı kayıt defteri ve ACR Görevler çalıştırıçalıştıran kimliği ile etiketlenir. 
    * İkinci hedef, görevi oluştururken belirlediğiniz (veya geçirilen harici `regDate` `values.yaml` bir dosya aracılığıyla `az acr task create`sağladığınız) değeriyle tanımlanan kayıt defterini hedefler. Bu resim çalışma tarihi ile etiketlenir.
1. Yapılı `cmd` kapsayıcılardan birini çalıştırmak için bir adım çalıştırın. Bu örnek, arka planda uzun süren bir kapsayıcı yı başlatır ve kapsayıcı kimliğini döndürür, ardından kapsayıcıyı durdurur. Gerçek bir senaryoda, çalışan bir kapsayıcının doğru çalıştığından emin olmak için test edebilirsiniz.
1. Bir `push` adımda, inşa edilen görüntüleri iter, ilk çalıştırKayıt defterine, ikinci tarafından `regDate`tanımlanan kayıt defterine .

### <a name="task-command"></a>Görev komutu

Daha önce tanımlanan kabuk ortamı değişkenlerini kullanarak, aşağıdaki [az acr görev oluşturma][az-acr-task-create] komutunu çalıştırarak görevi oluşturun. Kayıt defterinizin adını *mycontainer registrydate*için değiştirin.

```azurecli-interactive
az acr task create \
    --registry $ACR_NAME \
    --name example2 \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node.git \
    --file taskmulti-multiregistry.yaml \
    --git-access-token $GIT_PAT \
    --set regDate=mycontainerregistrydate.azurecr.io
```

### <a name="add-task-credential"></a>Görev kimlik bilgisi ekleme

Değeri ile tanımlanan kayıt defterine görüntüleri `regDate`itmek için, göreve bu kayıt defteri için giriş kimlik bilgilerini eklemek için [az acr görev kimlik bilgilerini ekleyin][az-acr-task-credential-add] komutunu kullanın.

Bu örnekte, *AcrPush* rolüne yönelik kayıt defterine erişimi olan bir [hizmet ilkesi](container-registry-auth-service-principal.md) oluşturmanızı öneririz. Hizmet ilkesini oluşturmak için bu [Azure CLI komut dosyasına](https://github.com/Azure-Samples/azure-cli-samples/blob/master/container-registry/service-principal-create/service-principal-create.sh)bakın.

Hizmet temel başvuru kimliği ve parolasını aşağıdaki `az acr task credential add` komutla geçirin:

```azurecli-interactive
az acr task credential add --name example2 \
    --registry $ACR_NAME \
    --login-server mycontainerregistrydate.azurecr.io \
    --username <service-principal-application-id> \
    --password <service-principal-password>
```

CLI, eklediğiniz kayıt defteri giriş sunucusunun adını döndürür.

### <a name="test-the-multi-step-workflow"></a>Çok aşamalı iş akışını test edin

Önceki örnekte olduğu gibi, çok adımlı görevi sınamak için [az acr görev çalıştırma][az-acr-task-run] komutunu çalıştırarak el ile tetikle. Git deposuna bir taahhütile görevi tetiklemek için, commit [with a commit ile bir yapıyı tetikleyen](#trigger-a-build-with-a-commit)bölümüne bakın.

```azurecli-interactive
az acr task run --registry $ACR_NAME --name example2
```

Varsayılan olarak, `az acr task run` komutunu yürüttüğünüzde komut, günlük çıktısını konsolunuza akışla aktarır. Daha önce olduğu gibi, çıktı görev adımlarının her birini çalıştırmanın ilerlemesini gösterir. Çıktı, önemli adımları göstermek için yoğunlaştırılmıştır.

Çıktı:

```output
Queued a run with ID: cf1g
Waiting for an agent...
2019/05/03 04:33:39 Downloading source code...
2019/05/03 04:33:41 Finished downloading source code
2019/05/03 04:33:42 Using acb_vol_4569b017-29fe-42bd-83b2-25c45a8ac807 as the home volume
2019/05/03 04:33:42 Creating Docker network: acb_default_network, driver: 'bridge'
2019/05/03 04:33:43 Successfully set up Docker network: acb_default_network
2019/05/03 04:33:43 Setting up Docker configuration...
2019/05/03 04:33:44 Successfully set up Docker configuration
2019/05/03 04:33:44 Logging in to registry: mycontainerregistry.azurecr.io
2019/05/03 04:33:45 Successfully logged into mycontainerregistry.azurecr.io
2019/05/03 04:33:45 Logging in to registry: mycontainerregistrydate.azurecr.io
2019/05/03 04:33:47 Successfully logged into mycontainerregistrydate.azurecr.io
2019/05/03 04:33:47 Executing step ID: acb_step_0. Working directory: '', Network: 'acb_default_network'
2019/05/03 04:33:47 Scanning for dependencies...
2019/05/03 04:33:47 Successfully scanned dependencies
2019/05/03 04:33:47 Launching container with name: acb_step_0
Sending build context to Docker daemon  25.09kB

[...]

Successfully tagged mycontainerregistry.azurecr.io/hello-world:cf1g
2019/05/03 04:33:55 Successfully executed container: acb_step_0
2019/05/03 04:33:55 Executing step ID: acb_step_1. Working directory: '', Network: 'acb_default_network'
2019/05/03 04:33:55 Scanning for dependencies...
2019/05/03 04:33:56 Successfully scanned dependencies
2019/05/03 04:33:56 Launching container with name: acb_step_1
Sending build context to Docker daemon  25.09kB

[...]

Successfully tagged mycontainerregistrydate.azurecr.io/hello-world:20190503-043342z
2019/05/03 04:33:57 Successfully executed container: acb_step_1
2019/05/03 04:33:57 Executing step ID: acb_step_2. Working directory: '', Network: 'acb_default_network'
2019/05/03 04:33:57 Launching container with name: acb_step_2
721437ff674051b6be63cbcd2fa8eb085eacbf38d7d632f1a079320133182101
2019/05/03 04:33:58 Successfully executed container: acb_step_2
2019/05/03 04:33:58 Executing step ID: acb_step_3. Working directory: '', Network: 'acb_default_network'
2019/05/03 04:33:58 Launching container with name: acb_step_3
test
2019/05/03 04:34:09 Successfully executed container: acb_step_3
2019/05/03 04:34:09 Executing step ID: acb_step_4. Working directory: '', Network: 'acb_default_network'
2019/05/03 04:34:09 Pushing image: mycontainerregistry.azurecr.io/hello-world:cf1g, attempt 1
The push refers to repository [mycontainerregistry.azurecr.io/hello-world]

[...]

2019/05/03 04:34:12 Successfully pushed image: mycontainerregistry.azurecr.io/hello-world:cf1g
2019/05/03 04:34:12 Pushing image: mycontainerregistrydate.azurecr.io/hello-world:20190503-043342z, attempt 1
The push refers to repository [mycontainerregistrydate.azurecr.io/hello-world]

[...]

2019/05/03 04:34:19 Successfully pushed image: mycontainerregistrydate.azurecr.io/hello-world:20190503-043342z
2019/05/03 04:34:19 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 8.125744)
2019/05/03 04:34:19 Populating digests for step ID: acb_step_0...
2019/05/03 04:34:21 Successfully populated digests for step ID: acb_step_0
2019/05/03 04:34:21 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 2.009281)
2019/05/03 04:34:21 Populating digests for step ID: acb_step_1...
2019/05/03 04:34:23 Successfully populated digests for step ID: acb_step_1
2019/05/03 04:34:23 Step ID: acb_step_2 marked as successful (elapsed time in seconds: 0.795440)
2019/05/03 04:34:23 Step ID: acb_step_3 marked as successful (elapsed time in seconds: 11.446775)
2019/05/03 04:34:23 Step ID: acb_step_4 marked as successful (elapsed time in seconds: 9.734973)
2019/05/03 04:34:23 The following dependencies were found:
2019/05/03 04:34:23
- image:
    registry: mycontainerregistry.azurecr.io
    repository: hello-world
    tag: cf1g
    digest: sha256:75354e9edb995e8661438bad9913deed87a185fddd0193811f916d684b71a5d2
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 9-alpine
    digest: sha256:8dafc0968fb4d62834d9b826d85a8feecc69bd72cd51723c62c7db67c6dec6fa
  git:
    git-head-revision: 9d9023473c46a5e2c315681b11eb4552ef0faccc
- image:
    registry: mycontainerregistrydate.azurecr.io
    repository: hello-world
    tag: 20190503-043342z
    digest: sha256:75354e9edb995e8661438bad9913deed87a185fddd0193811f916d684b71a5d2
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 9-alpine
    digest: sha256:8dafc0968fb4d62834d9b826d85a8feecc69bd72cd51723c62c7db67c6dec6fa
  git:
    git-head-revision: 9d9023473c46a5e2c315681b11eb4552ef0faccc

Run ID: cf1g was successful after 46s
```

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, kaynak kodunu git deposuna adadığınızda otomatik olarak tetikleyen çok aşamalı, çok kapsayıcı tabanlı görevlerin nasıl oluşturulacağımı öğrendiniz. Paralel ve bağımlı adım yürütme de dahil olmak üzere çok adımlı görevlerin gelişmiş özellikleri için [ACR Görevleri YAML başvurusuna](container-registry-tasks-reference-yaml.md)bakın. Bir kapsayıcı görüntüsünün temel görüntüsü güncelleştirildiğinde derlemeleri tetikleyen görevler oluşturmak için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Temel görüntü güncelleştirmesi ile derlemeleri otomatikleştirme](container-registry-tutorial-base-image-update.md)

<!-- LINKS - External -->
[sample-repo]: https://github.com/Azure-Samples/acr-build-helloworld-node

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-task]: /cli/azure/acr/task
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task-list-runs]: /cli/azure/acr/task#az-acr-task-list-runs
[az-acr-task-credential-add]: /cli/azure/acr/task/credential#az-acr-task-credential-add    
[az-login]: /cli/azure/reference-index#az-login

<!-- IMAGES -->
[build-task-01-new-token]: ./media/container-registry-tutorial-build-tasks/build-task-01-new-token.png
[build-task-02-generated-token]: ./media/container-registry-tutorial-build-tasks/build-task-02-generated-token.png
