---
title: Yönetilen kimlikle kimlik doğrulaması
description: Kullanıcı tarafından atanan veya sistem tarafından atanan yönetilen Azure kimliğini kullanarak özel konteyner kayıt defterinizdeki resimlere erişim sağlayın.
ms.topic: article
ms.date: 01/16/2019
ms.openlocfilehash: 9b8bed78629d3a9739ec00772ad5c8216a04c122
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74456499"
---
# <a name="use-an-azure-managed-identity-to-authenticate-to-an-azure-container-registry"></a>Azure kapsayıcı kayıt defterinde kimlik doğrulama yapmak için Azure yönetilen bir kimlik kullanma 

Kayıt defteri kimlik bilgilerini sağlamaveya yönetmeye gerek kalmadan, azure [kaynaklarının](../active-directory/managed-identities-azure-resources/overview.md) başka bir Azure kapsayıcı kayıt defterine kimlik doğrulaması için yönetilen bir kimlik kullanın. Örneğin, bir Linux VM'de, konteyner kayıt defterinizdeki kapsayıcı görüntülerine erişmek için, genel kayıt defterini kullandığınız kadar kolay bir şekilde kullanıcı tarafından atanmış veya sistem tarafından atanmış bir yönetilen kimlik ayarlayın.

Bu makale için, yönetilen kimlikler ve nasıl yönetilebilirsiniz hakkında daha fazla bilgi edinebilirsiniz:

> [!div class="checklist"]
> * Azure VM'de kullanıcı tarafından atanmış veya sistem tarafından atanmış bir kimliği etkinleştirme
> * Azure kapsayıcı kayıt defterine kimlik erişimi verme
> * Kayıt defterine erişmek ve kapsayıcı görüntüsünü çekmek için yönetilen kimliği kullanma 

Azure kaynaklarını oluşturmak için bu makalede, Azure CLI sürümü 2.0.55 veya daha sonra çalıştırmanız gerekmektedir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli].

Bir kapsayıcı kayıt defteri ayarlamak ve bir kapsayıcı görüntüsünü ona itmek için Docker'ın yerel olarak yüklenmesi de gerekir. Docker [macOS][docker-mac], [Windows][docker-windows] veya [Linux][docker-linux]'ta Docker'ı kolayca yapılandırmanızı sağlayan paketler sağlar.

## <a name="why-use-a-managed-identity"></a>Neden yönetilen bir kimlik kullanıyorsun?

Azure kaynakları için yönetilen bir kimlik, Azure hizmetlerine Azure Etkin Dizin'de (Azure AD) otomatik olarak yönetilen bir kimlik sağlar. Sanal makineler de dahil olmak üzere [belirli Azure kaynaklarını](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)yönetilen bir kimlikle yapılandırabilirsiniz. Ardından, kimlik bilgilerini kod veya komut dosyasında geçirmeden diğer Azure kaynaklarına erişmek için kimliği kullanın.

Yönetilen kimlikler iki türden gelir:

* Birden çok kaynağa atayabileceğiniz ve istediğiniz kadar süreyle devam edebileceğiniz *kullanıcı tarafından atanan kimlikler.* Kullanıcı tarafından atanan kimlikler şu anda önizlemededir.

* Tek bir sanal makine gibi belirli bir kaynağa özgü olan ve bu kaynağın ömrü boyunca süren sistem tarafından yönetilen bir *kimlik.*

Yönetilen bir kimliğe sahip bir Azure kaynağı ayarladıktan sonra, kimlik istediğiniz erişimi herhangi bir güvenlik ilkesi gibi başka bir kaynağa verin. Örneğin, yönetilen bir kimliği, Azure'daki özel bir kayıt defterine çekme, itme ve çekme veya diğer izinlerle bir rol atayın. (Kayıt defteri rollerinin tam listesi için [Azure Kapsayıcı Kayıt Defteri rolleri ve izinlerine](container-registry-roles.md)bakın.) Bir veya daha fazla kaynağa kimlik erişimi verebilirsiniz.

Ardından, kodunuzda kimlik belgesi olmadan [Azure AD kimlik doğrulamasını destekleyen](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)herhangi bir hizmetin kimliğini kullanın. Sanal bir makineden bir Azure kapsayıcı kayıt defterine erişmek için kimliği kullanmak için Azure Kaynak Yöneticisi ile kimlik doğrulaması yaptığınızda, kimliğinizi kullanırsınız. Senaryonuza bağlı olarak yönetilen kimliği kullanarak kimlik doğrulamayı seçin:

* HTTP veya REST çağrılarını kullanarak azure [AD erişimine](../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md) programlı olarak bir Azure REKLAM erişimi edinin

* Azure [SDK'larını](../active-directory/managed-identities-azure-resources/how-to-use-vm-sdk.md) kullanma

* Kimlikle [Azure CLI veya PowerShell'de oturum açın.](../active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in.md) 

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Azure kapsayıcı kayıt defteriniz yoksa, bir kayıt defteri oluşturun ve örnek bir kapsayıcı görüntüsünü ona itin. Adımlar için [Bkz. Hızlı Başlangıç: Azure CLI'yi kullanarak özel bir kapsayıcı kayıt defteri oluşturun.](container-registry-get-started-azure-cli.md)

Bu makalede, kayıt `aci-helloworld:v1` defterinizde depolanan kapsayıcı görüntüsü olduğunu varsayar. Örnekler *myContainerRegistry*bir kayıt defteri adı kullanın. Sonraki adımlarda kendi kayıt defteri ve resim adlarınızı değiştirin.

## <a name="create-a-docker-enabled-vm"></a>Docker özellikli vm oluşturma

Docker özellikli bir Ubuntu sanal makinesi oluşturun. [Azure CLI'yi](/cli/azure/install-azure-cli?view=azure-cli-latest) sanal makineye yüklemeniz de gerekir. Zaten bir Azure sanal makineniz varsa, sanal makineyi oluşturmak için bu adımı atlayın.

Az vm oluştur ile varsayılan bir Ubuntu Azure sanal makine [dağıtın.][az-vm-create] Aşağıdaki örnek, *myResourceGroup*adlı varolan bir kaynak grubunda *myDockerVM* adında bir VM oluşturur:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

VM’nin oluşturulması birkaç dakika sürer. Komut tamamlandığında, Azure CLI `publicIpAddress` tarafından görüntülenenlere dikkat edin. VM'ye SSH bağlantıları yapmak için bu adresi kullanın.

### <a name="install-docker-on-the-vm"></a>Docker'ı VM'ye yükleyin

VM çalışmaya devam ettikten sonra, VM'ye Bir SSH bağlantısı kurun. *PublicIpAddress'i* VM'nizin genel IP adresiyle değiştirin.

```bash
ssh azureuser@publicIpAddress
```

Docker'ı VM'ye yüklemek için aşağıdaki komutu çalıştırın:

```bash
sudo apt install docker.io -y
```

Yüklemeden sonra, Docker'ın VM üzerinde düzgün çalıştığını doğrulamak için aşağıdaki komutu çalıştırın:

```bash
sudo docker run -it hello-world
```

Çıktı:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
[...]
```

### <a name="install-the-azure-cli"></a>Azure CLI'yı yükleme

Azure CLI'yi Ubuntu sanal makinenize yüklemek için [Azure CLI'yi](/cli/azure/install-azure-cli-apt?view=azure-cli-latest) yükleyin' i yükleyin adımlarını izleyin. Bu makale için sürüm 2.0.55 veya sonraki sürümyüklediğinizden emin olun.

SSH oturumundan çıkın.

## <a name="example-1-access-with-a-user-assigned-identity"></a>Örnek 1: Kullanıcı tarafından atanan bir kimlikle erişim

### <a name="create-an-identity"></a>Kimlik oluşturma

Az kimlik oluşturma komutunu kullanarak aboneliğinizde bir [kimlik oluşturun.](/cli/azure/identity?view=azure-cli-latest#az-identity-create) Konteyner kayıt defterini veya sanal makineyi veya farklı bir tane oluşturmak için daha önce kullandığınız kaynak grubunu kullanabilirsiniz.

```azurecli-interactive
az identity create --resource-group myResourceGroup --name myACRId
```

Aşağıdaki adımlarda kimliği yapılandırmak için, kimliğin kaynak kimliğini ve hizmet asıl kimliğini değişkenlerde depolamak için [az identity show][az-identity-show] komutunu kullanın.

```azurecli
# Get resource ID of the user-assigned identity
userID=$(az identity show --resource-group myResourceGroup --name myACRId --query id --output tsv)

# Get service principal ID of the user-assigned identity
spID=$(az identity show --resource-group myResourceGroup --name myACRId --query principalId --output tsv)
```

Sanal makinenizden CLI'de oturum açışı yaptığınızda daha sonraki bir adımda kimliğin kimliğine ihtiyacınız olduğundan, değeri gösterin:

```bash
echo $userID
```

Kimlik formu:

```
/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myACRId
```

### <a name="configure-the-vm-with-the-identity"></a>VM'yi kimlikle yapılandırma

Aşağıdaki [az vm kimlik atama][az-vm-identity-assign] komutu Docker VM'nizi kullanıcı tarafından atanan kimlikle yapılandırır:

```azurecli
az vm identity assign --resource-group myResourceGroup --name myDockerVM --identities $userID
```

### <a name="grant-identity-access-to-the-container-registry"></a>Konteyner kayıt defterine kimlik erişimi verme

Şimdi kapsayıcı kayıt defterinize erişmek için kimliği yapılandırın. Önce kayıt defterinin kaynak kimliğini almak için [az acr göster][az-acr-show] komutunu kullanın:

```azurecli
resourceID=$(az acr show --resource-group myResourceGroup --name myContainerRegistry --query id --output tsv)
```

AcrPull rolünü kayıt defterine atamak için [az rol ataması oluşturma][az-role-assignment-create] komutunu kullanın. Bu rol kayıt defterine [çekme izinleri](container-registry-roles.md) sağlar. Hem çekme hem de itme izinleri sağlamak için ACRPush rolünü atayın.

```azurecli
az role assignment create --assignee $spID --scope $resourceID --role acrpull
```

### <a name="use-the-identity-to-access-the-registry"></a>Kayıt defterine erişmek için kimliği kullanma

Kimlikle yapılandırılan Docker sanal makinesine SSH. VM'de yüklü olan Azure CLI'yi kullanarak aşağıdaki Azure CLI komutlarını çalıştırın.

İlk olarak, VM'de yapılandırdığınız kimliği kullanarak [az giriş][az-login]le Azure CLI'ye kimlik doğrulaması yapın. Bunun `<userID>`için, önceki adımda aldığınız kimliğin yerine geçin. 

```azurecli
az login --identity --username <userID>
```

Daha sonra, [az acr giriş][az-acr-login]ile kayıt defterine doğrulayın. Bu komutu kullandığınızda, CLI, oturumunuzu kapsayıcı kayıt `az login` defteriyle sorunsuz bir şekilde doğrulamak için çalıştırdığınızda oluşturulan Etkin Dizin belirteci'ni kullanır. (VM kurulumuna bağlı olarak, bu komutu ve docker komutlarını `sudo`.)

```azurecli
az acr login --name myContainerRegistry
```

Bir `Login succeeded` mesaj görmelisin. Daha sonra `docker` kimlik bilgileri sağlamadan komutları çalıştırabilirsiniz. Örneğin, kayıt [docker pull][docker-pull] defterinizin `aci-helloworld:v1` giriş sunucusu adını belirterek görüntüyü çekmek için docker çekin çalıştırın. Giriş sunucusu adı kapsayıcı kayıt defteri adınızı (tüm küçük `.azurecr.io` harf) ve `mycontainerregistry.azurecr.io`ardından oluşur - örneğin, .

```
docker pull mycontainerregistry.azurecr.io/aci-helloworld:v1
```

## <a name="example-2-access-with-a-system-assigned-identity"></a>Örnek 2: Sistemle atanmış bir kimlikle erişim

### <a name="configure-the-vm-with-a-system-managed-identity"></a>VM'yi sistem tarafından yönetilen bir kimlikle yapılandırma

Aşağıdaki [az vm kimlik atama][az-vm-identity-assign] komutu Docker VM'nizi sistem tarafından atanmış bir kimlikle yapılandırır:

```azurecli
az vm identity assign --resource-group myResourceGroup --name myDockerVM 
```

Bir değişkeni VM kimliğinin `principalId` değerine (hizmet ana kimliği) ayarlamak için az [vm show][az-vm-show] komutunu kullanın ve sonraki adımlarda kullanın.

```azurecli-interactive
spID=$(az vm show --resource-group myResourceGroup --name myDockerVM --query identity.principalId --out tsv)
```

### <a name="grant-identity-access-to-the-container-registry"></a>Konteyner kayıt defterine kimlik erişimi verme

Şimdi kapsayıcı kayıt defterinize erişmek için kimliği yapılandırın. Önce kayıt defterinin kaynak kimliğini almak için [az acr göster][az-acr-show] komutunu kullanın:

```azurecli
resourceID=$(az acr show --resource-group myResourceGroup --name myContainerRegistry --query id --output tsv)
```

AcrPull rolünü kimliğe atamak için [az rol ataması oluşturma][az-role-assignment-create] komutunu kullanın. Bu rol kayıt defterine [çekme izinleri](container-registry-roles.md) sağlar. Hem çekme hem de itme izinleri sağlamak için ACRPush rolünü atayın.

```azurecli
az role assignment create --assignee $spID --scope $resourceID --role acrpull
```

### <a name="use-the-identity-to-access-the-registry"></a>Kayıt defterine erişmek için kimliği kullanma

Kimlikle yapılandırılan Docker sanal makinesine SSH. VM'de yüklü olan Azure CLI'yi kullanarak aşağıdaki Azure CLI komutlarını çalıştırın.

İlk olarak, VM'de sistem tarafından atanan kimliği kullanarak Azure CLI'yi [az girişle][az-login]doğrulayın.

```azurecli
az login --identity
```

Daha sonra, [az acr giriş][az-acr-login]ile kayıt defterine doğrulayın. Bu komutu kullandığınızda, CLI, oturumunuzu kapsayıcı kayıt `az login` defteriyle sorunsuz bir şekilde doğrulamak için çalıştırdığınızda oluşturulan Etkin Dizin belirteci'ni kullanır. (VM kurulumuna bağlı olarak, bu komutu ve docker komutlarını `sudo`.)

```azurecli
az acr login --name myContainerRegistry
```

Bir `Login succeeded` mesaj görmelisin. Daha sonra `docker` kimlik bilgileri sağlamadan komutları çalıştırabilirsiniz. Örneğin, kayıt [docker pull][docker-pull] defterinizin `aci-helloworld:v1` giriş sunucusu adını belirterek görüntüyü çekmek için docker çekin çalıştırın. Giriş sunucusu adı kapsayıcı kayıt defteri adınızı (tüm küçük `.azurecr.io` harf) ve `mycontainerregistry.azurecr.io`ardından oluşur - örneğin, .

```
docker pull mycontainerregistry.azurecr.io/aci-helloworld:v1
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Kapsayıcı Kayıt Defteri ile yönetilen kimlikleri kullanmayı ve nasıl şunları öğrendiniz:

> [!div class="checklist"]
> * Azure VM'de kullanıcı tarafından atanmış veya sistemtarafından atanmış bir kimliği etkinleştirme
> * Azure kapsayıcı kayıt defterine kimlik erişimi verme
> * Kayıt defterine erişmek ve kapsayıcı görüntüsünü çekmek için yönetilen kimliği kullanma

* [Azure kaynakları için yönetilen kimlikler](/azure/active-directory/managed-identities-azure-resources/)hakkında daha fazla bilgi edinin.


<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - Internal -->
[az-login]: /cli/azure/reference-index#az-login
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-vm-create]: /cli/azure/vm#az-vm-create
[az-vm-show]: /cli/azure/vm#az-vm-show
[az-vm-identity-assign]: /cli/azure/vm/identity#az-vm-identity-assign
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-identity-show]: /cli/azure/identity#az-identity-show
[azure-cli]: /cli/azure/install-azure-cli
