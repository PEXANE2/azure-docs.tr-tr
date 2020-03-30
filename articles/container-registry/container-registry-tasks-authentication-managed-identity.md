---
title: ACR görevinde yönetilen kimlik
description: Görevin diğer özel kapsayıcı kayıt defterleri de dahil olmak üzere diğer Azure kaynaklarına erişmesine izin vermek için Azure Kapsayıcı Kayıt Defteri görevinde Azure Kaynakları için yönetilen bir kimliği etkinleştirin.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 01/14/2020
ms.author: danlep
ms.openlocfilehash: f3294698f6973437a23fab798e8daf5642cc9b49
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77111774"
---
# <a name="use-an-azure-managed-identity-in-acr-tasks"></a>ACR Görevlerinde Azure tarafından yönetilen bir kimlik kullanma 

[ACR görevinde](container-registry-tasks-overview.md) [Azure kaynakları için yönetilen](../active-directory/managed-identities-azure-resources/overview.md) bir kimliği etkinleştirin, böylece görev kimlik bilgilerini sağlamaya veya yönetmeye gerek kalmadan diğer Azure kaynaklarına erişebilir. Örneğin, kapsayıcı görüntülerini başka bir kayıt defterine çekmek veya itmek için bir görev adımı etkinleştirmek için yönetilen bir kimlik kullanın.

Bu makalede, bir ACR görevinde kullanıcı tarafından atanan veya sistem tarafından atanan yönetilen bir kimliği etkinleştirmek için Azure CLI'yi nasıl kullanacağınızı öğrenirsiniz. Azure Bulut Kabuğu'nu veya Azure CLI'nin yerel yüklemesini kullanabilirsiniz. Yerel olarak kullanmak isterseniz, sürüm 2.0.68 veya daha sonra gereklidir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

Çizim amacıyla, bu makaledeki örnek komutlar, yönetilen bir kimliğe olanak tanıyan temel bir görüntü oluşturma görevi oluşturmak için [az acr görev oluşturur.][az-acr-task-create] Örnek senaryoların yönetilen bir kimlik kullanarak Bir ACR görevinden güvenli kaynaklara erişebilmek için bkz:

* [Kayıt defterleri arasında kimlik doğrulaması](container-registry-tasks-cross-registry-authentication.md)
* [Azure Anahtar Kasası'nda depolanan sırlar ile harici kaynaklara erişin](container-registry-tasks-authentication-key-vault.md)

## <a name="why-use-a-managed-identity"></a>Neden yönetilen bir kimlik kullanıyorsun?

Azure kaynakları için yönetilen bir kimlik, Azure Etkin Dizin'de otomatik olarak yönetilen bir kimliğe sahip seçili Azure hizmetleri sağlar. Görevin görev adımlarında kimlik bilgilerini geçirmeden diğer güvenli Azure kaynaklarına erişebilmeleri için bir ACR görevini yönetilen bir kimlikle yapılandırabilirsiniz.

Yönetilen kimlikler iki türden gelir:

* Birden çok kaynağa atayabileceğiniz ve istediğiniz kadar süreyle devam edebileceğiniz *kullanıcı tarafından atanan kimlikler.* Kullanıcı tarafından atanan kimlikler şu anda önizlemededir.

* Sistem *le atanmış*kimlik, ACR görevi gibi belirli bir kaynağa özgüdür ve bu kaynağın ömrü boyunca sürer.

ACR görevinde kimlik türlerini veya her ikisini etkinleştirebilirsiniz. Herhangi bir güvenlik ilkesi gibi başka bir kaynağa kimlik erişimi izni ver. Görev çalıştığında, erişim gerektiren herhangi bir görev adımlarında kaynağa erişmek için kimliği kullanır.

## <a name="steps-to-use-a-managed-identity"></a>Yönetilen bir kimliği kullanma adımları

ACR görevi olan yönetilen bir kimliği kullanmak için bu üst düzey adımları izleyin.

### <a name="1-optional-create-a-user-assigned-identity"></a>1. (İsteğe bağlı) Kullanıcı tarafından atanan bir kimlik oluşturma

Kullanıcı tarafından atanmış bir kimlik kullanmayı planlıyorsanız, varolan bir kimliği kullanın veya Kimliği Azure CLI veya diğer Azure araçlarını kullanarak oluşturun. Örneğin, az [kimlik oluşturma][az-identity-create] komutunu kullanın. 

Yalnızca sistemtarafından atanmış bir kimlik kullanmayı planlıyorsanız, bu adımı atlayın. ACR görevini oluştururken sistemtarafından atanmış bir kimlik oluşturursunuz.

### <a name="2-enable-identity-on-an-acr-task"></a>2. ACR görevinde kimliği etkinleştirme

Bir ACR görevi oluşturduğunuzda, isteğe bağlı olarak kullanıcı tarafından atanan bir kimliği, sistem tarafından atanmış bir kimliği veya her ikisini de etkinleştirin. Örneğin, Azure `--assign-identity` CLI'de [az acr görev oluşturma][az-acr-task-create] komutunu çalıştırdığınızda parametreyi geçirin.

Sistem tarafından atanmış bir kimliği `--assign-identity` etkinleştirmek `assign-identity [system]`için, hiçbir değer veya . Aşağıdaki örnek komut, `hello-world` görüntüyü oluşturan ve sistem tarafından atanmış yönetilen bir kimliğe olanak tanıyan ortak bir GitHub deposundan bir Linux görevi oluşturur:

```azurecli
az acr task create \
    --image hello-world:{{.Run.ID}} \
    --name hello-world --registry MyRegistry \
    --context https://github.com/Azure-Samples/acr-build-helloworld-node.git \
    --file Dockerfile \
    --commit-trigger-enabled false \
    --assign-identity
```

Kullanıcı tarafından atanan bir kimliği `--assign-identity` etkinleştirmek için, kimliğin *kaynak kimliği* değeriyle geçirin. Aşağıdaki örnek komut, `hello-world` görüntüyü oluşturan ve kullanıcı tarafından atanan yönetilen bir kimliği etkinleştiren ortak bir GitHub deposundan bir Linux görevi oluşturur:

```azurecli
az acr task create \
    --image hello-world:{{.Run.ID}} \
    --name hello-world --registry MyRegistry \
    --context https://github.com/Azure-Samples/acr-build-helloworld-node.git \
    --file Dockerfile \
    --commit-trigger-enabled false
    --assign-identity <resourceID>
```

[Az identity show][az-identity-show] komutunu çalıştırarak kimliğin kaynak kimliğini alabilirsiniz. Kaynak grubu *myResourceGroup'taki* kimlik *myUserAssignedIdentity'in* kaynak kimliği formdadır: 

```
"/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myUserAssignedIdentity"
```

### <a name="3-grant-the-identity-permissions-to-access-other-azure-resources"></a>3. Diğer Azure kaynaklarına erişmek için kimlik izinleri verme

Görevinizin gereksinimlerine bağlı olarak, diğer Azure kaynaklarına erişmek için kimlik izinleri ver. Örneklere şunlar dahildir:

* Yönetilen kimliği, Azure'daki hedef kapsayıcı kayıt defterine çekme, itme ve çekme veya diğer izinlerle bir rol atayın. Kayıt defteri rollerinin tam listesi için [Azure Kapsayıcı Kayıt Defteri rolleri ve izinlerine](container-registry-roles.md)bakın. 
* Yönetilen kimliği Azure anahtar kasasında sırları okumak için bir rol atayın.

Kaynaklara rol tabanlı erişimi yönetmek için [Azure CLI](../role-based-access-control/role-assignments-cli.md) veya diğer Azure araçlarını kullanın. Örneğin, az [rol atamasını][az-role-assignment-create] çalıştırın ve kimliği kaynağa bir rol atamak için komut oluşturun. 

Aşağıdaki örnek, bir kapsayıcı kayıt defterinden çekme izinleri yönetilen bir kimlik atar. Komut, görev *kimliğinin temel kimliğini* ve hedef kayıt defterinin *kaynak kimliğini* belirtir.


```azurecli
az role assignment create \
  --assignee <principalID> \
  --scope <registryID> \
  --role acrpull
```

### <a name="4-optional-add-credentials-to-the-task"></a>4. (İsteğe bağlı) Göreve kimlik bilgileri ekleme

Görevinizin görüntüleri başka bir özel kayıt defterine çekmek veya itmek veya diğer kaynaklara erişmek için kimlik bilgilerine ihtiyacı varsa, göreve kimlik bilgileri ekleyin. Kimlik bilgileri eklemek için [az acr görev kimlik bilgilerini çalıştırın][az-acr-task-credential-add] ve kimliğin kimlik bilgilerine erişebileceğini belirtmek için `--use-identity` parametreyi geçirin. 

Örneğin, Azure kapsayıcı kayıt *defteri hedef kayıt defteriile*kimlik doğrulaması için sistem `use-identity [system]`tarafından atanmış bir kimlik bilgileri eklemek için :

```azurecli
az acr task credential add \
    --name helloworld \
    --registry myregistry \
    --login-server targetregistry.azurecr.io \
    --use-identity [system]
```

Kayıt *defteri hedef kayıt defteri*ile kimlik doğrulamak için kullanıcı tarafından `use-identity` atanan bir kimlik bilgileri eklemek için, kimlik *istemci kimliği* değeri ile geçmek. Örnek:

```azurecli
az acr task credential add \
    --name helloworld \
    --registry myregistry \
    --login-server targetregistry.azurecr.io \
    --use-identity <clientID>
```

[Az identity show][az-identity-show] komutunu çalıştırarak kimliğin istemci kimliğini alabilirsiniz. İstemci kimliği formun `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`bir GUID'sidir.

### <a name="5-run-the-task"></a>5. Görevi çalıştırın

Bir görevi yönetilen bir kimlikle yapılandırdıktan sonra, görevi çalıştırın. Örneğin, bu makalede oluşturulan görevlerden birini sınamak için [az acr görev çalıştırma][az-acr-task-run] komutunu kullanarak el ile tetikle. Ek, otomatik görev tetikleyicilerini yapılandırırsanız, görev otomatik olarak tetiklendiğinde çalışır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir ACR görevinde kullanıcı tarafından atanan veya sistem tarafından atanan yönetilen bir kimliği etkinleştirmeyi ve kullanmayı öğrendiniz. Yönetilen bir kimlik kullanarak bir ACR görevinden güvenli kaynaklara erişmek için senaryolar için bkz:

* [Kayıt defterleri arasında kimlik doğrulaması](container-registry-tasks-cross-registry-authentication.md)
* [Azure Anahtar Kasası'nda depolanan sırlar ile harici kaynaklara erişin](container-registry-tasks-authentication-key-vault.md)


<!-- LINKS - Internal -->
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-identity-create]: /cli/azure/identity#az-identity-create
[az-identity-show]: /cli/azure/identity#az-identity-show
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task-credential-add]: /cli/azure/acr/task/credential#az-acr-task-credential-add
[azure-cli-install]: /cli/azure/install-azure-cli
