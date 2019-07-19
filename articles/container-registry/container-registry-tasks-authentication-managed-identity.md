---
title: Azure Container Registry görevlerle yönetilen bir kimlik kullanma
description: Azure kaynakları için yönetilen bir kimlik atayarak diğer özel kapsayıcı kayıt defterleri dahil olmak üzere Azure kaynaklarına bir Azure Container Registry görevi erişimi sağlayın.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 06/12/2019
ms.author: danlep
ms.openlocfilehash: 46351af375ab4c6e59a3ddfba3c05c1e517fab0d
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68311528"
---
# <a name="use-an-azure-managed-identity-in-acr-tasks"></a>ACR görevlerinde Azure yönetilen kimliği kullanma 

Bir Azure kapsayıcısı kayıt defterine veya diğer Azure kaynaklarına kimlik doğrulaması gerçekleştirmek için [Azure kaynakları için yönetilen bir kimlik](../active-directory/managed-identities-azure-resources/overview.md) , kodda kimlik bilgileri sağlamaya veya yönetmeye gerek kalmadan kullanın. Örneğin, bir görevde bir adım olarak kapsayıcı görüntülerini başka bir kayıt defterine çekmek veya göndermek için yönetilen bir kimlik kullanın.

Bu makalede, Yönetilen kimlikler ve nasıl yapılır hakkında daha fazla bilgi edinebilirsiniz:

> [!div class="checklist"]
> * ACR görevinde sistem tarafından atanan bir kimlik veya Kullanıcı tarafından atanan bir kimlik etkinleştirme
> * Diğer Azure Container kayıt defterleri gibi Azure kaynaklarına kimlik erişimi verme
> * Bir görevden kaynaklara erişmek için yönetilen kimliği kullanın 

Azure kaynaklarını oluşturmak için bu makale, Azure CLı sürüm 2.0.66 veya üstünü çalıştırmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][azure-cli].

## <a name="why-use-a-managed-identity"></a>Yönetilen kimlik neden kullanılmalıdır?

Azure kaynakları için yönetilen bir kimlik, Azure hizmetlerini Azure Active Directory (Azure AD) içinde otomatik olarak yönetilen kimlik sağlar. Bir yönetilen kimlikle ACR görevleri dahil olmak üzere [belirli Azure kaynaklarını](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)yapılandırabilirsiniz. Daha sonra, kodu veya betiklerdeki kimlik bilgilerini geçirmeden diğer Azure kaynaklarına erişmek için bu kimliği kullanın.

Yönetilen kimlikler iki türtür:

* Birden fazla kaynağa atayabileceğiniz ve istediğiniz sürece devam eden *Kullanıcı tarafından atanan kimlikler*. Kullanıcı tarafından atanan kimlikler Şu anda önizlemededir.

* ACR görevi gibi belirli bir kaynak için benzersiz olan ve söz konusu kaynağın kullanım ömrü boyunca kullanılan *sistem tarafından yönetilen bir kimlik*.

Yönetilen kimliğe sahip bir Azure kaynağı ayarladıktan sonra, her türlü güvenlik sorumlusu gibi başka bir kaynağa kimlik erişimi verin. Örneğin, Azure 'da özel bir kapsayıcı kayıt defterine çekme, gönderme ve çekme veya diğer izinlerle yönetilen bir kimlik atayın. (Kayıt defteri rollerinin tüm listesi için bkz. [Azure Container Registry roller ve izinler](container-registry-roles.md).) Bir veya daha fazla kaynağa bir kimlik erişimi verebilirsiniz.

## <a name="create-container-registries"></a>Kapsayıcı kayıt defterleri oluşturma

Bu öğretici için üç kapsayıcı kayıt defterlerine ihtiyacınız vardır:

* ACR görevlerini oluşturmak ve yürütmek için ilk kayıt defterini kullanın. Bu makalede, bu kaynak kayıt defteri *myregistry*olarak adlandırılmıştır. 
* İkinci ve üçüncü kayıt defterleri, oluşturduğu bir görüntüyü göndermek için ilk örnek görevin hedef kayıt defterlerine yöneliktir. Bu makalede, hedef kayıt defterleri *customregistry1* ve *customregistry2*olarak adlandırılır.

Sonraki adımlarda kendi kayıt defteri adlarınızla değiştirin.

Gerekli Azure Container Registry 'ye zaten sahip değilseniz, bkz [. hızlı başlangıç: Azure CLı](container-registry-get-started-azure-cli.md)kullanarak özel bir kapsayıcı kayıt defteri oluşturun. Görüntüleri kayıt defterine henüz göndermeniz gerekmez.

## <a name="example-task-with-a-system-assigned-identity"></a>Örnek: Sistem tarafından atanan kimliğe sahip görev

Bu örnekte, sistem tarafından atanan kimlik ile [çok adımlı bir görevin](container-registry-tasks-multi-step.md) nasıl oluşturulacağı gösterilmektedir. Görev bir görüntü oluşturur ve sonra görüntüyü göndermek için iki hedef kayıt defterlerinden kimlik doğrulaması yapmak üzere kimliğini kullanır.

Bu örnek görevin adımları adlı `testtask.yaml`bir [YAML dosyasında](container-registry-tasks-reference-yaml.md) tanımlanmıştır. Dosya, [ACR-Tasks](https://github.com/Azure-Samples/acr-tasks) örnek deposunun Multipleregisdenemeler dizininde bulunur. Dosya şurada yeniden oluşturulur:

```yml
version: v1.0.0
steps:
  - build: -t {{.Values.REGISTRY1}}/hello-world:{{.Run.ID}} . -f hello-world.dockerfile
  - push: ["{{.Values.REGISTRY1}}/hello-world:{{.Run.ID}}"]
  - build: -t {{.Values.REGISTRY2}}/hello-world:{{.Run.ID}} . -f hello-world.dockerfile
  - push: ["{{.Values.REGISTRY2}}/hello-world:{{.Run.ID}}"]
```

### <a name="create-task-with-system-assigned-identity"></a>Sistem tarafından atanan kimlikle görev oluştur

Aşağıdaki [az ACR Task Create][az-acr-task-create] komutunu yürüterek görevi *birden çok reg* oluşturun. Görev bağlamı, örnek deposunun multipleregisdenemeler klasörüdür ve komut depodaki dosyaya `testtask.yaml` başvurur. Ek değer içermeyen parametre, görev için sistem tarafından atanan bir kimlik oluşturur. `--assign-identity` Bu görev, el ile tetiklemeniz için ayarlandı, ancak depoyu depoya gönderildiğinde veya bir çekme isteği yapıldığında çalışacak şekilde ayarlayabilirsiniz. 

```azurecli
az acr task create \
  --registry myregistry \
  --name multiple-reg \
  --context https://github.com/Azure-Samples/acr-tasks.git#:multipleRegistries \
  --file testtask.yaml \
  --commit-trigger-enabled false \
  --pull-request-trigger-enabled false \
  --assign-identity
```

Komut çıkışında, `identity` bölümünde türünde `SystemAssigned` bir kimlik ayarlanır. , `principalId` Kimliğin hizmet sorumlusu kimliğidir:

```console
[...]
  "identity": {
    "principalId": "xxxxxxxx-2703-42f9-97d0-xxxxxxxxxxxx",
    "tenantId": "xxxxxxxx-86f1-41af-91ab-xxxxxxxxxxxx",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
  },
  "location": "eastus",
[...]
``` 

Daha sonraki komutlarda kullanmak için bir değişkende depolamak `principalId` üzere [az ACR Task Show][az-acr-task-show] komutunu kullanın:

```azurecli
principalID=$(az acr task show --name multiple-reg --registry myregistry --query identity.principalId --output tsv)
```

### <a name="give-identity-push-permissions-to-two-target-container-registries"></a>İki hedef kapsayıcı kayıt defterlerine kimlik gönderme izinleri verin

Bu bölümde, *customregistry1* ve *customregistry2*adlı iki hedef kayıt defterlerine göndermek için sistem tarafından atanan kimlik izinlerini verin.

İlk olarak, her bir kayıt defterinin kaynak KIMLIĞINI almak için [az ACR Show][az-acr-show] komutunu kullanın ve kimlikleri değişkenlerde depolayın:

```azurecli
reg1_id=$(az acr show --name customregistry1 --query id --output tsv)
reg2_id=$(az acr show --name customregistry2 --query id --output tsv)
```

`acrpush` Rolü her kayıt defterine atamak için [az role atama Create][az-role-assignment-create] komutunu kullanın. Bu rol, bir kapsayıcı kayıt defterine görüntü çekme ve gönderme izinlerine sahiptir.

```azurecli
az role assignment create --assignee $principalID --scope $reg1_id --role acrpush
az role assignment create --assignee $principalID --scope $reg2_id --role acrpush
```

### <a name="add-target-registry-credentials-to-task"></a>Göreve hedef kayıt defteri kimlik bilgilerini ekle

Şimdi, kimliğin kimlik bilgilerini, hedef kayıt defterlerine göre kimlik doğrulayabilmesi için göreve eklemek üzere [az ACR Task Credential Add][az-acr-task-credential-add] komutunu kullanın.

```azurecli
az acr task credential add \
  --name multiple-reg \
  --registry myregistry \
  --login-server customregistry1.azurecr.io \
  --use-identity [system]

az acr task credential add \
  --name multiple-reg \
  --registry myregistry \
  --login-server customregistry2.azurecr.io \
  --use-identity [system]
```

### <a name="manually-run-the-task"></a>Görevi el ile çalıştırın

Görevi el ile tetiklemek için [az ACR Task Run][az-acr-task-run] komutunu kullanın. Parametresi, iki hedef kayıt defterlerinin oturum açma sunucusu adlarını görev `REGISTRY2`değişkenleri `REGISTRY1` için değer olarak geçirmek için kullanılır. `--set`

```azurecli
az acr task run \
  --name multiple-reg \
  --registry myregistry \
  --set REGISTRY1=customregistry1.azurecr.io \
  --set REGISTRY2=customregistry2.azurecr.io
```

Çıkış şuna benzer olacaktır:

```console
Sending build context to Docker daemon  4.096kB
Step 1/1 : FROM hello-world
 ---> fce289e99eb9
Successfully built fce289e99eb9
Successfully tagged customregistry2.azurecr.io/hello-world:cf31
2019/05/31 22:16:02 Successfully executed container: acb_step_0
2019/05/31 22:16:02 Executing step ID: acb_step_1. Timeout(sec): 600, Working directory: 'multipleRegistries', Network: 'acb_default_network'
2019/05/31 22:16:02 Pushing image: customregistry2.azurecr.io/hello-world:cf31, attempt 1
The push refers to repository [customregistry2.azurecr.io/hello-world]
af0b15c8625b: Preparing
af0b15c8625b: Pushed
cf31: digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a size: 524
2019/05/31 22:16:08 Successfully pushed image: customregistry2.azurecr.io/hello-world:cf31
2019/05/31 22:16:08 Executing step ID: acb_step_2. Timeout(sec): 600, Working directory: 'multipleRegistries', Network: 'acb_default_network'
2019/05/31 22:16:08 Scanning for dependencies...
2019/05/31 22:16:08 Successfully scanned dependencies
2019/05/31 22:16:08 Launching container with name: acb_step_2
Sending build context to Docker daemon  4.096kB
Step 1/1 : FROM hello-world
 ---> fce289e99eb9
Successfully built fce289e99eb9
Successfully tagged customregistry1.azurecr.io/hello-world:cf31
2019/05/31 22:16:09 Successfully executed container: acb_step_2
2019/05/31 22:16:09 Executing step ID: acb_step_3. Timeout(sec): 600, Working directory: 'multipleRegistries', Network: 'acb_default_network'
2019/05/31 22:16:09 Pushing image: customregistry1.azurecr.io/hello-world:cf31, attempt 1
The push refers to repository [customregistry1.azurecr.io/hello-world]
af0b15c8625b: Preparing
af0b15c8625b: Pushed
cf31: digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a size: 524
2019/05/31 22:16:21 Successfully pushed image: customregistry1.azurecr.io/hello-world:cf31
2019/05/31 22:16:21 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 1.985291)
2019/05/31 22:16:21 Populating digests for step ID: acb_step_0...
2019/05/31 22:16:22 Successfully populated digests for step ID: acb_step_0
2019/05/31 22:16:22 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 5.743225)
2019/05/31 22:16:22 Step ID: acb_step_2 marked as successful (elapsed time in seconds: 1.925959)
2019/05/31 22:16:22 Populating digests for step ID: acb_step_2...
2019/05/31 22:16:24 Successfully populated digests for step ID: acb_step_2
2019/05/31 22:16:24 Step ID: acb_step_3 marked as successful (elapsed time in seconds: 11.061057)
2019/05/31 22:16:24 The following dependencies were found:
2019/05/31 22:16:24
- image:
    registry: customregistry2.azurecr.io
    repository: hello-world
    tag: cf31
    digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: latest
    digest: sha256:6f744a2005b12a704d2608d8070a494ad1145636eeb74a570c56b94d94ccdbfc
  git:
    git-head-revision: 05275dca2bc61f584085ca913c39d509236f576b
- image:
    registry: customregistry1.azurecr.io
    repository: hello-world
    tag: cf31
    digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: latest
    digest: sha256:6f744a2005b12a704d2608d8070a494ad1145636eeb74a570c56b94d94ccdbfc
  git:
    git-head-revision: 05275dca2bc61f584085ca913c39d509236f576b

Run ID: cf31 was successful after 35s
```

## <a name="example-task-with-a-user-assigned-identity"></a>Örnek: Kullanıcı tarafından atanan kimliği olan görev

Bu örnekte, bir Azure anahtar kasasından gizli dizileri okumak için izinleri olan kullanıcı tarafından atanan bir kimlik oluşturursunuz. Bu kimliği, gizli dizi okur, bir görüntü oluşturur ve resim etiketini okumak için Azure CLı 'de oturum açar.

### <a name="create-a-key-vault-and-store-a-secret"></a>Anahtar Kasası oluşturma ve gizli dizi depolama

İlk olarak, gerekirse, *eastus* konumunda *myresourcegroup* adlı bir kaynak grubunu aşağıdaki [az Group Create][az-group-create] komutuyla oluşturun:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Bir Anahtar Kasası oluşturmak için [az keykasacreate][az-keyvault-create] komutunu kullanın. Benzersiz bir Anahtar Kasası adı belirttiğinizden emin olun. 

```azurecli-interactive
az keyvault create --name mykeyvault --resource-group myResourceGroup --location eastus
```

[Az keykasasecret set][az-keyvault-secret-set] komutunu kullanarak anahtar kasasında örnek bir gizli dizi depolayın:

```azurecli
az keyvault secret set \
  --name SampleSecret \
  --value "Hello ACR Tasks!" \
  --description ACRTasksecret  \
  --vault-name mykeyvault
```

Örneğin, özel bir Docker kayıt defteri ile kimlik doğrulaması yapmak için kimlik bilgilerini depolamak isteyebilirsiniz, böylece özel bir görüntü çekebilirsiniz.

### <a name="create-an-identity"></a>Kimlik oluşturma

[Az Identity Create][az-identity-create] komutunu kullanarak aboneliğinizde *Myacrtasksıd* adlı bir kimlik oluşturun. Daha önce bir kapsayıcı kayıt defteri veya Anahtar Kasası oluşturmak için kullandığınız kaynak grubunu kullanabilir veya farklı bir tane oluşturabilirsiniz.

```azurecli-interactive
az identity create --resource-group myResourceGroup --name myACRTasksId
```

Aşağıdaki adımlarda kimliği yapılandırmak için [az Identity Show][az-identity-show] komutunu kullanarak KIMLIğIN kaynak kimliğini ve hızmet sorumlusu kimliğini değişkenlere depolayın.

```azurecli
# Get resource ID of the user-assigned identity
resourceID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query id --output tsv)

# Get service principal ID of the user-assigned identity
principalID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query principalId --output tsv)
```

### <a name="grant-identity-access-to-keyvault-to-read-secret"></a>Gizli dizi okumak için anahtar kasasına kimlik erişimi verme

Anahtar kasasında bir erişim ilkesi ayarlamak için aşağıdaki [az keykasası Set-Policy][az-keyvault-set-policy] komutunu çalıştırın. Aşağıdaki örnek, Kullanıcı tarafından atanan kimliğin anahtar kasasından gizli dizileri almasına izin verir. Bu erişim daha sonra çok adımlı bir görevi başarıyla çalıştırmak için gereklidir.

```azurecli-interactive
 az keyvault set-policy --name mykeyvault --resource-group myResourceGroup --object-id $principalID --secret-permissions get
```

### <a name="grant-identity-reader-access-to-the-resource-group-for-registry"></a>Kayıt defteri için kaynak grubuna kimlik okuyucusu erişimi verme

Bir okuyucu rolü kimliğini bu durumda kaynak kayıt defteri 'ni içeren kaynak grubuna atamak için aşağıdaki [az role atama Create][az-role-assignment-create] komutunu çalıştırın. Bu rol, daha sonra çok adımlı bir görevi başarıyla çalıştırmak için gereklidir.

```azurecli
az role assignment create --role reader --resource-group myResourceGroup --assignee $principalID
```

### <a name="create-task-with-user-assigned-identity"></a>Kullanıcı tarafından atanan kimlikle görev oluştur

Şimdi [çok adımlı bir görev](container-registry-tasks-multi-step.md) oluşturun ve Kullanıcı tarafından atanan kimliği atayın. Bu örnek görev için, yerel çalışma dizininde adlı `managed-identities.yaml` bir [YAML dosyası](container-registry-tasks-reference-yaml.md) oluşturun ve aşağıdaki içeriği yapıştırın. Dosyadaki Anahtar Kasası adını anahtar kasanızın adıyla değiştirdiğinizden emin olun

```yml
version: v1.0.0
# Replace mykeyvault with the name of your key vault
secrets:
  - id: name
    keyvault: https://mykeyvault.vault.azure.net/secrets/SampleSecret
steps:
  # Verify that the task can access the secret in the key vault
  - cmd: bash -c 'if [ -z "$MY_SECRET" ]; then echo "Secret not resolved"; else echo "Secret resolved"; fi'
    env: 
      - MY_SECRET='{{.Secrets.name}}' 

  # Build/push the website image to source registry, using dockerfile in the Azure-Samples repo
  - cmd: docker build -t {{.Run.Registry}}/my-website:{{.Run.ID}} https://github.com/Azure-Samples/aci-helloworld.git
  - push: 
    - "{{.Run.Registry}}/my-website:{{.Run.ID}}"
  
  # Login to Azure CLI with identity and list the tags to verify that the image is in the registry
  - cmd: microsoft/azure-cli az login --identity
  - cmd: microsoft/azure-cli az acr repository show-tags -n {{.Values.registryName}} --repository my-website
```

Bu görev şunları yapar:

* Anahtar kasasındaki gizli dizi erişimine erişebileceğini doğrular. Bu adım tanıtım amaçlıdır. Gerçek dünyada bir senaryoda, özel bir Docker Hub deposuna erişmek için kimlik bilgilerini almak üzere bir görev adımı gerekebilir.
* `mywebsite` Görüntüyü kaynak kayıt defterine oluşturur ve gönderir.
* Kaynak kayıt defterindeki `my-website` resim etiketlerini listelemek için Azure CLI 'da oturum açar.

*Msıtask* adlı bir görev oluşturun ve daha önce oluşturduğunuz Kullanıcı tarafından atanan KIMLIğIN kaynak kimliği ' ni geçirin. Bu örnek görev, yerel çalışma dizininizde `managed-identities.yaml` kaydettiğiniz dosyadan oluşturulur, bu nedenle el ile tetiklemeniz gerekir.

```azurecli
az acr task create \
  --name msitask \
  --registry myregistry \
  --context /dev/null \
  --file managed-identities.yaml \
  --pull-request-trigger-enabled false \
  --commit-trigger-enabled false \
  --assign-identity $resourceID
```

Komut çıkışında, `identity` bölümünde türünde `UserAssigned` bir kimlik ayarlanır. , `principalId` Kimliğin hizmet sorumlusu kimliğidir:

```console
[...]
"identity": {
    "principalId": null,
    "tenantId": null,
    "type": "UserAssigned",
    "userAssignedIdentities": {
      "/subscriptions/xxxxxxxx-d12e-4760-9ab6-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myACRTasksId": {
        "clientId": "xxxxxxxx-f17e-4768-bb4e-xxxxxxxxxxxx",
        "principalId": "xxxxxxxx-1335-433d-bb6c-xxxxxxxxxxxx"
      }
[...]
```

### <a name="manually-run-the-task"></a>Görevi el ile çalıştırın

Görevi el ile tetiklemek için [az ACR Task Run][az-acr-task-run] komutunu kullanın. `--set` Parametresi, kaynak kayıt defteri adını göreve geçirmek için kullanılır:

```azurecli
az acr task run --name msitask --registry myregistry --set registryName=myregistry  
```

Çıktıda gizli dizi çözümlendiğini, görüntünün başarıyla oluşturulup gönderildiği ve görevin, kaynak kayıt defterinden görüntü etiketini okumak üzere kimlik ile Azure CLı 'da oturum açtığı gösterilmektedir:

```console
Queued a run with ID: cf32
Waiting for an agent...
2019/06/10 23:25:37 Downloading source code...
2019/06/10 23:25:38 Finished downloading source code
2019/06/10 23:25:39 Using acb_vol_4e4a0a7c-b6ef-4ec5-b40f-3436fc5eb0f5 as the home volume
2019/06/10 23:25:41 Creating Docker network: acb_default_network, driver: 'bridge'
2019/06/10 23:25:41 Successfully set up Docker network: acb_default_network
2019/06/10 23:25:41 Setting up Docker configuration...
2019/06/10 23:25:42 Successfully set up Docker configuration
2019/06/10 23:25:42 Logging in to registry: myregistry.azurecr.io
2019/06/10 23:25:43 Successfully logged into myregistry.azurecr.io
2019/06/10 23:25:43 Executing step ID: acb_step_0. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:25:43 Launching container with name: acb_step_0
Secret resolved
2019/06/10 23:25:44 Successfully executed container: acb_step_0
2019/06/10 23:25:44 Executing step ID: acb_step_1. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:25:44 Launching container with name: acb_step_1
Sending build context to Docker daemon  74.75kB

[...]

cf32: digest: sha256:cbb4aa83b33f6959d83e84bfd43ca901084966a9f91c42f111766473dc977f36 size: 1577
2019/06/10 23:26:05 Successfully pushed image: myregistry.azurecr.io/my-website:cf32
2019/06/10 23:26:05 Executing step ID: acb_step_3. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:26:05 Launching container with name: acb_step_3
[
  {
    "environmentName": "AzureCloud",
    "id": "xxxxxxxx-d12e-4760-9ab6-xxxxxxxxxxxx",
    "isDefault": true,
    "name": "DanLep Internal Consumption",
    "state": "Enabled",
    "tenantId": "xxxxxxxx-86f1-41af-91ab-xxxxxxxxxxxx",
      "user": {
      "assignedIdentityInfo": "MSI",
      "name": "systemAssignedIdentity",
      "type": "servicePrincipal"
    }
  }
]
2019/06/10 23:26:09 Successfully executed container: acb_step_3
2019/06/10 23:26:09 Executing step ID: acb_step_4. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:26:09 Launching container with name: acb_step_4
[
  "cf32"
]
2019/06/10 23:26:14 Successfully executed container: acb_step_4
2019/06/10 23:26:14 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 1.025312)
2019/06/10 23:26:14 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 13.703823)
2019/06/10 23:26:14 Step ID: acb_step_2 marked as successful (elapsed time in seconds: 6.791506)
2019/06/10 23:26:14 Step ID: acb_step_3 marked as successful (elapsed time in seconds: 3.852972)
2019/06/10 23:26:14 Step ID: acb_step_4 marked as successful (elapsed time in seconds: 5.079079)
```


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Container Registry görevlerle Yönetilen kimlikler kullanma hakkında bilgi edindiniz ve şunları yapabilirsiniz:

> [!div class="checklist"]
> * Bir ACR görevinde sistem tarafından atanan bir kimlik veya kullanıcı atama özelliğini etkinleştirme
> * Diğer Azure Container kayıt defterleri gibi Azure kaynaklarına kimlik erişimi verme
> * Bir görevden kaynaklara erişmek için yönetilen kimliği kullanın  

* [Azure kaynakları için Yönetilen kimlikler](/azure/active-directory/managed-identities-azure-resources/)hakkında daha fazla bilgi edinin.

<!-- LINKS - Internal -->
[az-login]: /cli/azure/reference-index#az-login
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-identity-create]: /cli/azure/identity?view=azure-cli-latest#az-identity-create
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
