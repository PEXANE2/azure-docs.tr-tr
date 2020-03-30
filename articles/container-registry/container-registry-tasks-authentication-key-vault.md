---
title: ACR görevinden dış kimlik doğrulaması
description: Azure kaynakları için yönetilen bir kimlik kullanarak, Azure anahtar kasasında depolanan Docker Hub kimlik bilgilerini okumak için bir Azure Kapsayıcı Kayıt Defteri Görevi (ACR Görevi) yapılandırın.
ms.topic: article
ms.date: 01/14/2020
ms.openlocfilehash: 47d3d643ee1287ef4f444095a2c6cfe6dcab294b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76842529"
---
# <a name="external-authentication-in-an-acr-task-using-an-azure-managed-identity"></a>Azure tarafından yönetilen bir kimlik kullanarak ACR görevinde dış kimlik doğrulama 

[ACR görevinde,](container-registry-tasks-overview.md)Azure [kaynakları için yönetilen bir kimliği etkinleştirebilirsiniz.](container-registry-tasks-authentication-managed-identity.md) Görev, kimlik bilgilerini sağlamaveya yönetmeye gerek kalmadan kimliği diğer Azure kaynaklarına erişmek için kullanabilir. 

Bu makalede, Azure anahtar kasasında depolanan sırlara erişen bir görevde yönetilen bir kimliği nasıl etkinleştirdiğinizi öğrenirsiniz. 

Azure kaynaklarını oluşturmak için bu makalede, Azure CLI sürümü 2.0.68 veya daha sonra çalıştırmanız gerekmektedir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli].

## <a name="scenario-overview"></a>Senaryoya genel bakış

Örnek görev, Azure anahtar kasasında depolanan Docker Hub kimlik bilgilerini okur. Kimlik bilgileri, özel bir Docker Hub deposuna yazma (itme) izinlerine sahip bir Docker Hub hesabı içindir. Kimlik bilgilerini okumak için, görevi yönetilen bir kimlikle yapılandırın ve uygun izinler atarsınız. Kimlikle ilişkili görev bir görüntü oluşturur ve görüntüyü özel repo'ya itmek için Docker Hub'a işaret eder. 

Bu örnek, kullanıcı tarafından atanan veya sistem tarafından atanan yönetilen kimliği kullanan adımları gösterir. Kimlik seçiminiz kuruluşunuzun ihtiyaçlarına bağlıdır.

Gerçek bir senaryoda, bir şirket bir yapı sürecinin parçası olarak görüntüleri Docker Hub'daki özel bir repo'da yayımlayabilir. 

## <a name="prerequisites"></a>Ön koşullar

Görevi çalıştırdığınız bir Azure kapsayıcı kayıt defterine ihtiyacınız var. Bu makalede, bu kayıt *defteri myregistry*adlandırılır. Sonraki adımlarda kendi kayıt defteri adınızı değiştirin.

Zaten bir Azure kapsayıcı kayıt defteriniz yoksa, Bkz. [Hızlı Başlangıç: Azure CLI'yi kullanarak özel bir kapsayıcı kayıt defteri oluşturun.](container-registry-get-started-azure-cli.md) Görüntüleri henüz kayıt defterine itmeniz gerekmez.

Ayrıca Docker Hub'da özel bir depoya ve repo'ya yazma izinleri olan bir Docker Hub hesabına da ihtiyacınız vardır. Bu örnekte, bu repo *hubuser/hubrepo*olarak adlandırılır. 

## <a name="create-a-key-vault-and-store-secrets"></a>Önemli bir kasa oluşturun ve sırları saklayın

İlk olarak, gerekirse, aşağıdaki az grup oluşturma komutu ile *eastus* yerde *myResourceGroup* adlı bir kaynak [grubu oluşturun:][az-group-create]

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Bir [anahtar tonoz][az-keyvault-create] oluşturmak için az keyvault oluşturma komutunu kullanın. Benzersiz bir anahtar kasa adı belirttiğinden emin olun. 

```azurecli-interactive
az keyvault create --name mykeyvault --resource-group myResourceGroup --location eastus
```

Az [keyvault gizli ayar][az-keyvault-secret-set] komutunu kullanarak gerekli Docker Hub kimlik bilgilerini anahtar kasasında saklayın. Bu komutlarda, değerler çevre değişkenlerinde geçirilir:

```azurecli
# Store Docker Hub user name
az keyvault secret set \
  --name UserName \
  --value $USERNAME \
  --vault-name mykeyvault

# Store Docker Hub password
az keyvault secret set \
  --name Password \
  --value $PASSWORD \
  --vault-name mykeyvault
```

Gerçek bir senaryoda, sırlar büyük olasılıkla ayrı bir süreçte ayarlanır ve korunur.

## <a name="define-task-steps-in-yaml-file"></a>YAML dosyasındaki görev adımlarını tanımlama

Bu örnek görev için adımlar bir [YAML dosyasında](container-registry-tasks-reference-yaml.md)tanımlanır. Yerel çalışma `dockerhubtask.yaml` dizininde adı geçen bir dosya oluşturun ve aşağıdaki içeriği yapıştırın. Dosyadaki anahtar kasa adını anahtar kasanızın adı ile değiştirdiğinizden emin olun.

```yml
version: v1.1.0
# Replace mykeyvault with the name of your key vault
secrets:
  - id: username
    keyvault: https://mykeyvault.vault.azure.net/secrets/UserName
  - id: password
    keyvault: https://mykeyvault.vault.azure.net/secrets/Password
steps:
# Log in to Docker Hub
  - cmd: bash echo '{{.Secrets.password}}' | docker login --username '{{.Secrets.username}}' --password-stdin 
# Build image
  - build: -t {{.Values.PrivateRepo}}:$ID https://github.com/Azure-Samples/acr-tasks.git -f hello-world.dockerfile
# Push image to private repo in Docker Hub
  - push:
    - {{.Values.PrivateRepo}}:$ID
```

Görev adımları aşağıdakileri yapar:

* Docker Hub ile kimlik doğrulaması yapmak için gizli kimlik bilgilerini yönetin.
* Sırları `docker login` komutana ileterek Docker Hub ile kimlik doğrula.
* [Azure Örnekleri/acr-tasks](https://github.com/Azure-Samples/acr-tasks.git) repo'sunda örnek bir Dockerfile kullanarak bir resim oluşturun.
* Görüntüyü özel Docker Hub deposuna itin.


## <a name="option-1-create-task-with-user-assigned-identity"></a>Seçenek 1: Kullanıcı tarafından atanan kimlikle görev oluşturma

Bu bölümdeki adımlar bir görev oluşturur ve kullanıcı tarafından atanan bir kimliği etkinleştirin. Bunun yerine sistem tarafından atanmış bir kimliği etkinleştirmek istiyorsanız, [bkz.](#option-2-create-task-with-system-assigned-identity) 

[!INCLUDE [container-registry-tasks-user-assigned-id](../../includes/container-registry-tasks-user-assigned-id.md)]

### <a name="create-task"></a>Görev oluşturma

Aşağıdaki [az acr görev oluşturma][az-acr-task-create] komutunu çalıştırarak görev *dockerhubtask* oluşturun. Görev kaynak kodu bağlamı olmadan çalışır ve `dockerhubtask.yaml` komut dosyaya çalışma dizininde başvurur. Parametre, `--assign-identity` kullanıcı tarafından atanan kimliğin kaynak kimliğini geçer. 

```azurecli
az acr task create \
  --name dockerhubtask \
  --registry myregistry \
  --context /dev/null \
  --file dockerhubtask.yaml \
  --assign-identity $resourceID
```

[!INCLUDE [container-registry-tasks-user-id-properties](../../includes/container-registry-tasks-user-id-properties.md)]

## <a name="option-2-create-task-with-system-assigned-identity"></a>Seçenek 2: Sistem tarafından atanan kimlikle görev oluşturma

Bu bölümdeki adımlar bir görev oluşturur ve sistem tarafından atanmış bir kimliği etkinleştirin. Bunun yerine kullanıcı tarafından atanan bir kimliği etkinleştirmek istiyorsanız, [bkz.](#option-1-create-task-with-user-assigned-identity) 

### <a name="create-task"></a>Görev oluşturma

Aşağıdaki [az acr görev oluşturma][az-acr-task-create] komutunu çalıştırarak görev *dockerhubtask* oluşturun. Görev kaynak kodu bağlamı olmadan çalışır ve `dockerhubtask.yaml` komut dosyaya çalışma dizininde başvurur. Değeri `--assign-identity` olmayan parametre, görevde sistem tarafından atanan kimliği etkinleştiriyor.  

```azurecli
az acr task create \
  --name dockerhubtask \
  --registry myregistry \
  --context /dev/null \
  --file dockerhubtask.yaml \
  --assign-identity 
```

[!INCLUDE [container-registry-tasks-system-id-properties](../../includes/container-registry-tasks-system-id-properties.md)]

## <a name="grant-identity-access-to-key-vault"></a>Anahtar kasasına kimlik erişimi verme

Anahtar kasasında bir erişim ilkesi ayarlamak için aşağıdaki [az keyvault ayar ilkesi][az-keyvault-set-policy] komutunu çalıştırın. Aşağıdaki örnek, kimliğin anahtar kasasından sırları okumasına olanak tanır. 

```azurecli
az keyvault set-policy --name mykeyvault \
  --resource-group myResourceGroup \
  --object-id $principalID \
  --secret-permissions get
```

## <a name="manually-run-the-task"></a>Görevi el ile çalıştırın

Yönetilen bir kimliği etkinleştirdiğiniz görevin başarılı bir şekilde çalıştığını doğrulamak için, [az acr görev çalıştırma][az-acr-task-run] komutuyla görevi el ile tetikle. Parametre, `--set` özel repo adını göreve geçirmek için kullanılır. Bu örnekte, yer tutucu repo adı *hubuser/hubrepo'dur.*

```azurecli
az acr task run --name dockerhubtask --registry myregistry --set PrivateRepo=hubuser/hubrepo
```

Görev başarılı bir şekilde çalıştığında, çıktı Docker Hub'a başarılı bir kimlik doğrulaması gösterir ve görüntü başarıyla oluşturulur ve özel repo'ya itilir:

```console
Queued a run with ID: cf24
Waiting for an agent...
2019/06/20 18:05:55 Using acb_vol_b1edae11-30de-4f2b-a9c7-7d743e811101 as the home volume
2019/06/20 18:05:58 Creating Docker network: acb_default_network, driver: 'bridge'
2019/06/20 18:05:58 Successfully set up Docker network: acb_default_network
2019/06/20 18:05:58 Setting up Docker configuration...
2019/06/20 18:05:59 Successfully set up Docker configuration
2019/06/20 18:05:59 Logging in to registry: myregistry.azurecr.io
2019/06/20 18:06:00 Successfully logged into myregistry.azurecr.io
2019/06/20 18:06:00 Executing step ID: acb_step_0. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/20 18:06:00 Launching container with name: acb_step_0
[...]
Login Succeeded
2019/06/20 18:06:02 Successfully executed container: acb_step_0
2019/06/20 18:06:02 Executing step ID: acb_step_1. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/20 18:06:02 Scanning for dependencies...
2019/06/20 18:06:04 Successfully scanned dependencies
2019/06/20 18:06:04 Launching container with name: acb_step_1
Sending build context to Docker daemon    129kB
[...]
2019/06/20 18:06:07 Successfully pushed image: hubuser/hubrepo:cf24
2019/06/20 18:06:07 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 2.064353)
2019/06/20 18:06:07 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 2.594061)
2019/06/20 18:06:07 Populating digests for step ID: acb_step_1...
2019/06/20 18:06:09 Successfully populated digests for step ID: acb_step_1
2019/06/20 18:06:09 Step ID: acb_step_2 marked as successful (elapsed time in seconds: 2.743923)
2019/06/20 18:06:09 The following dependencies were found:
2019/06/20 18:06:09
- image:
    registry: registry.hub.docker.com
    repository: hubuser/hubrepo
    tag: cf24
    digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: latest
    digest: sha256:0e11c388b664df8a27a901dce21eb89f11d8292f7fca1b3e3c4321bf7897bffe
  git:
    git-head-revision: b0ffa6043dd893a4c75644c5fed384c82ebb5f9e

Run ID: cf24 was successful after 15s
```

Görüntünün itildiğini doğrulamak için, özel`cf24` Docker Hub repo'sundaki etiketi (bu örnekte) denetleyin.

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
[az-identity-create]: /cli/azure/identity#az-identity-create
[az-identity-show]: /cli/azure/identity#az-identity-show
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task-list-runs]: /cli/azure/acr/task#az-acr-task-list-runs
[az-acr-task-credential-add]: /cli/azure/acr/task/credential#az-acr-task-credential-add
[az-group-create]: /cli/azure/group?#az-group-create
[az-keyvault-create]: /cli/azure/keyvault?#az-keyvault-create
[az-keyvault-secret-set]: /cli/azure/keyvault/secret#az-keyvault-secret-set
[az-keyvault-set-policy]: /cli/azure/keyvault#az-keyvault-set-policy
