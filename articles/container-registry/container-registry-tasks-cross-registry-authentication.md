---
title: ACR görevinden çapraz kayıt kimlik doğrulaması
description: Azure kaynakları için yönetilen bir kimlik kullanarak başka bir özel Azure kapsayıcı kayıt defterine erişmek için bir Azure Kapsayıcı Kayıt Defteri Görevi (ACR Görevi) yapılandırın
ms.topic: article
ms.date: 01/14/2020
ms.openlocfilehash: 47b2a50784cf56b089fea0981e5a06d581b8ba3a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76842511"
---
# <a name="cross-registry-authentication-in-an-acr-task-using-an-azure-managed-identity"></a>Azure tarafından yönetilen bir kimlik kullanarak ACR görevinde çapraz kayıt kimlik doğrulaması 

[ACR görevinde,](container-registry-tasks-overview.md)Azure [kaynakları için yönetilen bir kimliği etkinleştirebilirsiniz.](container-registry-tasks-authentication-managed-identity.md) Görev, kimlik bilgilerini sağlamaveya yönetmeye gerek kalmadan kimliği diğer Azure kaynaklarına erişmek için kullanabilir. 

Bu makalede, görevin çalıştırılmasını çalıştırmak için kullanılandan farklı bir kayıt defterinden görüntü çekmek için görevde yönetilen bir kimliği etkinleştirmeyi öğrenirsiniz.

Azure kaynaklarını oluşturmak için bu makalede, Azure CLI sürümü 2.0.68 veya daha sonra çalıştırmanız gerekmektedir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli].

## <a name="scenario-overview"></a>Senaryoya genel bakış

Örnek görev, bir uygulama görüntüsü oluşturmak ve itmek için başka bir Azure kapsayıcı kayıt defterinden temel görüntü çeker. Temel görüntüyü çekmek için, görevi yönetilen bir kimlikle yapılandırın ve uygun izinler atarsınız. 

Bu örnek, kullanıcı tarafından atanan veya sistem tarafından atanan yönetilen kimliği kullanan adımları gösterir. Kimlik seçiminiz kuruluşunuzun ihtiyaçlarına bağlıdır.

Gerçek bir senaryoda, bir kuruluş tüm geliştirme ekipleri tarafından uygulamalarını oluşturmak için kullanılan temel görüntüler kümesini koruyabilir. Bu temel görüntüler, her geliştirme ekibinin yalnızca çekme haklarına sahip olduğu bir şirket kayıt defterinde depolanır. 

## <a name="prerequisites"></a>Ön koşullar

Bu makale için iki Azure kapsayıcı kayıt defterine ihtiyacınız var:

* ACR görevleri oluşturmak ve yürütmek için ilk kayıt defterini kullanırsınız. Bu makalede, bu kayıt *defteri myregistry*adlandırılır. 
* İkinci kayıt defteri, bir görüntü oluşturmak için görev için kullanılan bir temel görüntü barındıran. Bu makalede, ikinci kayıt *mybaseregistry*adlandırılır. 

Sonraki adımlarda kendi kayıt defteri adlarınızı değiştirin.

Gerekli Azure kapsayıcı kayıt defterlerine zaten sahip değilseniz, Bkz. [Quickstart: Azure CLI'yi kullanarak özel bir kapsayıcı kayıt defteri oluşturun.](container-registry-get-started-azure-cli.md) Görüntüleri henüz kayıt defterine itmeniz gerekmez.

## <a name="prepare-base-registry"></a>Temel kayıt defterini hazırlama

İlk olarak, çalışan bir dizini oluşturun ve ardından aşağıdaki içeriği içeren Dockerfile adlı bir dosya oluşturun. Bu basit örnek, Docker Hub'daki genel bir görüntüden bir Düğüm.js temel görüntüsü oluşturur.
    
```bash
echo FROM node:9-alpine > Dockerfile
```
Geçerli dizinde, temel görüntüyü oluşturmak ve temel kayıt defterine itmek için [az acr yapı][az-acr-build] komutunu çalıştırın. Uygulamada, kuruluştaki başka bir takım veya işlem temel kayıt defterini koruyabilir.
    
```azurecli
az acr build --image baseimages/node:9-alpine --registry mybaseregistry --file Dockerfile .
```

## <a name="define-task-steps-in-yaml-file"></a>YAML dosyasındaki görev adımlarını tanımlama

Bu örnek [çok adımlı görev](container-registry-tasks-multi-step.md) için adımlar bir [YAML dosyasında](container-registry-tasks-reference-yaml.md)tanımlanır. Yerel çalışma `helloworldtask.yaml` dizininde adı geçen bir dosya oluşturun ve aşağıdaki içeriği yapıştırın. Yapı `REGISTRY_NAME` adımının değerini temel kayıt defterinizin sunucu adı ile güncelleştirin.

```yml
version: v1.1.0
steps:
# Replace mybaseregistry with the name of your registry containing the base image
  - build: -t $Registry/hello-world:$ID  https://github.com/Azure-Samples/acr-build-helloworld-node.git -f Dockerfile-app --build-arg REGISTRY_NAME=mybaseregistry.azurecr.io
  - push: ["$Registry/hello-world:$ID"]
```

Yapı adımı, `Dockerfile-app` bir görüntü oluşturmak için [Dosyayı Azure Örnekleri/acr-build-helloworld düğüm](https://github.com/Azure-Samples/acr-build-helloworld-node.git) repo'sunda kullanır. Temel `--build-arg` görüntüyü çekmek için temel kayıt defterine başvurur. Başarıyla inşa edildiğinde, görüntü görevi çalıştırmak için kullanılan kayıt defterine itilir.

## <a name="option-1-create-task-with-user-assigned-identity"></a>Seçenek 1: Kullanıcı tarafından atanan kimlikle görev oluşturma

Bu bölümdeki adımlar bir görev oluşturur ve kullanıcı tarafından atanan bir kimliği etkinleştirin. Bunun yerine sistem tarafından atanmış bir kimliği etkinleştirmek istiyorsanız, [bkz.](#option-2-create-task-with-system-assigned-identity) 

[!INCLUDE [container-registry-tasks-user-assigned-id](../../includes/container-registry-tasks-user-assigned-id.md)]

### <a name="create-task"></a>Görev oluşturma

Aşağıdaki [az acr görev oluşturma][az-acr-task-create] komutunu uygulayarak görev *helloworldtask* oluşturun. Görev kaynak kodu bağlamı olmadan çalışır ve `helloworldtask.yaml` komut dosyaya çalışma dizininde başvurur. Parametre, `--assign-identity` kullanıcı tarafından atanan kimliğin kaynak kimliğini geçer. 

```azurecli
az acr task create \
  --registry myregistry \
  --name helloworldtask \
  --context /dev/null \
  --file helloworldtask.yaml \
  --assign-identity $resourceID
```

[!INCLUDE [container-registry-tasks-user-id-properties](../../includes/container-registry-tasks-user-id-properties.md)]

## <a name="option-2-create-task-with-system-assigned-identity"></a>Seçenek 2: Sistem tarafından atanan kimlikle görev oluşturma

Bu bölümdeki adımlar bir görev oluşturur ve sistem tarafından atanmış bir kimliği etkinleştirin. Bunun yerine kullanıcı tarafından atanan bir kimliği etkinleştirmek istiyorsanız, [bkz.](#option-1-create-task-with-user-assigned-identity) 

### <a name="create-task"></a>Görev oluşturma

Aşağıdaki [az acr görev oluşturma][az-acr-task-create] komutunu uygulayarak görev *helloworldtask* oluşturun. Görev kaynak kodu bağlamı olmadan çalışır ve `helloworldtask.yaml` komut dosyaya çalışma dizininde başvurur. Değeri `--assign-identity` olmayan parametre, görevde sistem tarafından atanan kimliği etkinleştiriyor. 

```azurecli
az acr task create \
  --registry myregistry \
  --name helloworldtask \
  --context /dev/null \
  --file helloworldtask.yaml \
  --assign-identity 
```
[!INCLUDE [container-registry-tasks-system-id-properties](../../includes/container-registry-tasks-system-id-properties.md)]

## <a name="give-identity-pull-permissions-to-the-base-registry"></a>Temel kayıt defterine kimlik çekme izinleri verme

Bu bölümde, yönetilen kimlik izinleri taban kayıt, *mybaseregistry*çekmek için vermek.

Temel kayıt defterinin kaynak kimliğini almak ve bir değişkende depolamak için [az acr göster][az-acr-show] komutunu kullanın:

```azurecli
baseregID=$(az acr show --name mybaseregistry --query id --output tsv)
```

Kimliği `acrpull` temel kayıt defterine atamak için [az rol ataması oluşturma][az-role-assignment-create] komutunu kullanın. Bu rolün yalnızca kayıt defterinden görüntü çekme izinleri vardır.

```azurecli
az role assignment create \
  --assignee $principalID \
  --scope $baseregID \
  --role acrpull
```

## <a name="add-target-registry-credentials-to-task"></a>Göreve hedef kayıt defteri kimlik bilgilerini ekleme

Şimdi, görevin kimlik bilgilerini kullanarak temel kayıt defteriyle kimlik doğrulamasını sağlamak için [az acr görev kimlik bilgilerini ekleyin][az-acr-task-credential-add] komutunu kullanın. Görevde etkinleştirdiğiniz yönetilen kimlik türüne karşılık gelen komutu çalıştırın. Kullanıcı tarafından atanmış bir kimliği `--use-identity` etkinleştirlediyseniz, kimliğin istemci kimliğiyle birlikte geçiş yaptınız. Sistem tarafından atanmış bir kimliği `--use-identity [system]`etkinleştirdseniz, 'yi geçirin.

```azurecli
# Add credentials for user-assigned identity to the task
az acr task credential add \
  --name helloworldtask \
  --registry myregistry \
  --login-server mybaseregistry.azurecr.io \
  --use-identity $clientID

# Add credentials for system-assigned identity to the task
az acr task credential add \
  --name helloworldtask \
  --registry myregistry \
  --login-server mybaseregistry.azurecr.io \
  --use-identity [system]
```

## <a name="manually-run-the-task"></a>Görevi el ile çalıştırın

Yönetilen bir kimliği etkinleştirdiğiniz görevin başarılı bir şekilde çalıştığını doğrulamak için, [az acr görev çalıştırma][az-acr-task-run] komutuyla görevi el ile tetikle. 

```azurecli
az acr task run \
  --name helloworldtask \
  --registry myregistry
```

Görev başarıyla çalışırsa, çıktı aşağıdakilere benzer:

```
Queued a run with ID: cf10
Waiting for an agent...
2019/06/14 22:47:32 Using acb_vol_dbfbe232-fd76-4ca3-bd4a-687e84cb4ce2 as the home volume
2019/06/14 22:47:39 Creating Docker network: acb_default_network, driver: 'bridge'
2019/06/14 22:47:40 Successfully set up Docker network: acb_default_network
2019/06/14 22:47:40 Setting up Docker configuration...
2019/06/14 22:47:41 Successfully set up Docker configuration
2019/06/14 22:47:41 Logging in to registry: myregistry.azurecr.io
2019/06/14 22:47:42 Successfully logged into myregistry.azurecr.io
2019/06/14 22:47:42 Logging in to registry: mybaseregistry.azurecr.io
2019/06/14 22:47:43 Successfully logged into mybaseregistry.azurecr.io
2019/06/14 22:47:43 Executing step ID: acb_step_0. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/14 22:47:43 Scanning for dependencies...
2019/06/14 22:47:45 Successfully scanned dependencies
2019/06/14 22:47:45 Launching container with name: acb_step_0
Sending build context to Docker daemon   25.6kB
Step 1/6 : ARG REGISTRY_NAME
Step 2/6 : FROM ${REGISTRY_NAME}/baseimages/node:9-alpine
9-alpine: Pulling from baseimages/node
[...]
Successfully built 41b49a112663
Successfully tagged myregistry.azurecr.io/hello-world:cf10
2019/06/14 22:47:56 Successfully executed container: acb_step_0
2019/06/14 22:47:56 Executing step ID: acb_step_1. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/14 22:47:56 Pushing image: myregistry.azurecr.io/hello-world:cf10, attempt 1
The push refers to repository [myregistry.azurecr.io/hello-world]
[...]
2019/06/14 22:48:00 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 2.517011)
2019/06/14 22:48:00 The following dependencies were found:
2019/06/14 22:48:00
- image:
    registry: myregistry.azurecr.io
    repository: hello-world
    tag: cf10
    digest: sha256:611cf6e3ae3cb99b23fadcd89fa144e18aa1b1c9171ad4a0da4b62b31b4e38d1
  runtime-dependency:
    registry: mybaseregistry.azurecr.io
    repository: baseimages/node
    tag: 9-alpine
    digest: sha256:e8e92cffd464fce3be9a3eefd1b65dc9cbe2484da31c11e813a4effc6105c00f
  git:
    git-head-revision: 0f988779c97fe0bfc7f2f74b88531617f4421643

Run ID: cf10 was successful after 32s
```

Görüntünün yerleşik olduğunu ve başarıyla *myregistry*itildi doğrulamak için [az acr depo show-tags][az-acr-repository-show-tags] komutunu çalıştırın:

```azurecli
az acr repository show-tags --name myregistry --repository hello-world --output tsv
```

Örnek çıktı:

```console
cf10
```

## <a name="next-steps"></a>Sonraki adımlar

* [ACR görevinde yönetilen bir kimliği etkinleştirme](container-registry-tasks-authentication-managed-identity.md)hakkında daha fazla bilgi edinin.
* [ACR Görevleri YAML referansına](container-registry-tasks-reference-yaml.md) bakın

<!-- LINKS - Internal -->
[az-login]: /cli/azure/reference-index#az-login
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-repository-show-tags]: /cli/azure/acr/repository#az-acr-repository-show-tags
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task-list-runs]: /cli/azure/acr/task#az-acr-task-list-runs
[az-acr-task-credential-add]: /cli/azure/acr/task/credential#az-acr-task-credential-add
[az-group-create]: /cli/azure/group?#az-group-create
