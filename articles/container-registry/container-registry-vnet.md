---
title: Sanal ağla erişimi kısıtlama
description: Azure kapsayıcı kayıt defterine yalnızca Bir Azure sanal ağındaki kaynaklardan veya genel IP adresi aralıklarından erişmene izin verin.
ms.topic: article
ms.date: 07/01/2019
ms.openlocfilehash: a6b89b074c25ea0948597ede7e5681b100c7f429
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74454335"
---
# <a name="restrict-access-to-an-azure-container-registry-using-an-azure-virtual-network-or-firewall-rules"></a>Azure sanal ağı veya güvenlik duvarı kurallarını kullanarak Azure kapsayıcı kayıt defterine erişimi kısıtlama

[Azure Sanal Ağı,](../virtual-network/virtual-networks-overview.md) Azure ve şirket içi kaynaklarınız için güvenli ve özel ağ sağlar. Azure sanal ağındaki özel Azure kapsayıcı kayıt defterinize erişimi sınırlandırarak, yalnızca sanal ağdaki kaynakların kayıt defterine erişmesini sağlarsınız. Binalar arası senaryolar için, yalnızca belirli IP adreslerinden kayıt defteri erişimine izin verecek şekilde güvenlik duvarı kurallarını da yapılandırabilirsiniz.

Bu makalede, gelen ağ erişim kurallarını bir kapsayıcı kayıt defterinde yapılandırmak için iki senaryo gösterilmektedir: sanal ağda dağıtılan sanal bir makineden veya Bir VM'nin genel IP adresinden.

> [!IMPORTANT]
> Bu özellik şu anda önizlemede dir ve bazı [sınırlamalar geçerlidir.](#preview-limitations) Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.
>

Bunun yerine, bir güvenlik duvarının arkasından bir kapsayıcı kayıt defterine ulaşmak için kaynakların erişim kuralları ayarlamanız gerekiyorsa, [güvenlik duvarının arkasındaki Azure konteyner kayıt defterine erişmek için Yapılandırma kurallarına](container-registry-firewall-access-rules.md)bakın.


## <a name="preview-limitations"></a>Önizleme sınırlamaları

* Yalnızca **Premium** kapsayıcı kayıt defteri ağ erişim kurallarıyla yapılandırılabilir. Kayıt defteri hizmet katmanları hakkında daha fazla bilgi için [Azure Konteyner Kayıt Defteri SK'leri'ne](container-registry-skus.md)bakın. 

* Sanal ağdaki bir kapsayıcı kayıt defterine erişmek için yalnızca bir [Azure Kubernetes Hizmet](../aks/intro-kubernetes.md) kümesi veya Azure [sanal makinesi](../virtual-machines/linux/overview.md) ana bilgisayar olarak kullanılabilir. *Azure Kapsayıcı Örnekleri de dahil olmak üzere diğer Azure hizmetleri şu anda desteklenmez.*

* [ACR Görevleri](container-registry-tasks-overview.md) işlemleri şu anda sanal ağda erişilen bir kapsayıcı kayıt defterinde desteklenmez.

* Her kayıt defteri en fazla 100 sanal ağ kuralını destekler.

## <a name="prerequisites"></a>Ön koşullar

* Bu makalede Azure CLI adımlarını kullanmak için Azure CLI sürüm 2.0.58 veya sonraki sürüm gereklidir. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli].

* Zaten bir konteyner kayıt defteriniz yoksa, bir tane oluşturun (Premium SKU gereklidir) ve Docker Hub gibi `hello-world` örnek bir görüntü itin. Örneğin, bir kayıt defteri oluşturmak için [Azure portalını][quickstart-portal] veya [Azure CLI'yi][quickstart-cli] kullanın. 

* Farklı bir Azure aboneliğinde sanal ağ kullanarak kayıt defteri erişimini kısıtlamak istiyorsanız, bu aboneliğe Azure Kapsayıcı Kayıt Defteri için kaynak sağlayıcısını kaydetmeniz gerekir. Örnek:

  ```azurecli
  az account set --subscription <Name or ID of subscription of virtual network>

  az provider register --namespace Microsoft.ContainerRegistry
  ``` 

## <a name="about-network-rules-for-a-container-registry"></a>Kapsayıcı kayıt defteri için ağ kuralları hakkında

Azure kapsayıcı kayıt defteri varsayılan olarak herhangi bir ağdaki ana bilgisayarlardaki internet üzerinden bağlantıları kabul eder. Sanal ağ da, yalnızca AKS kümesi veya Azure VM gibi Azure kaynaklarının bir ağ sınırını aşmadan kayıt defterine güvenli bir şekilde erişmesine izin verebilirsiniz. Ağ güvenlik duvarı kurallarını yalnızca belirli genel internet IP adresi aralıklarına izin verecek şekilde yapılandırabilirsiniz. 

Bir kayıt defterine erişimi sınırlamak için, önce kayıt defterinin varsayılan eylemini tüm ağ bağlantılarını reddetmesi için değiştirin. Ardından, ağ erişim kuralları ekleyin. Ağ kuralları üzerinden erişim izni verilen [istemciler, konteyner kayıt defterine kimlik doğrulamaya](https://docs.microsoft.com/azure/container-registry/container-registry-authentication) devam etmeli ve verilere erişmeye yetkili olmalıdır.

### <a name="service-endpoint-for-subnets"></a>Alt ağlar için servis bitiş noktası

Sanal ağdaki bir alt ağdan erişime izin vermek için Azure Kapsayıcı Kayıt Defteri hizmeti için bir [hizmet bitiş noktası](../virtual-network/virtual-network-service-endpoints-overview.md) eklemeniz gerekir. 

Azure Kapsayıcı Kayıt Defteri gibi çok kiracılı hizmetler, tüm müşteriler için tek bir IP adresi kümesi kullanır. Hizmet bitiş noktası, kayıt defterine erişmek için bir bitiş noktası atar. Bu bitiş noktası, trafiğe Azure omurga ağı üzerinden kaynağa en uygun rotayı sağlar. Sanal ağın ve alt ağın kimlikleri de her istekle birlikte iletilir.

### <a name="firewall-rules"></a>Güvenlik duvarı kuralları

IP ağ kuralları için, *16.17.18.0/24* gibi CIDR gösterimini veya *16.17.18.19*gibi tek bir IP adreslerini kullanarak izin verilen internet adresi aralıkları sağlayın. IP ağ kurallarına yalnızca *genel* internet IP adresleri için izin verilir. Özel ağlar için ayrılmış IP adresi aralıklarına (RFC 1918'de tanımlandığı şekilde) IP kurallarında izin verilmez.

## <a name="create-a-docker-enabled-virtual-machine"></a>Docker özellikli sanal makine oluşturma

Bu makale için, Bir Azure kapsayıcı kayıt defterine erişmek için Docker özellikli bir Ubuntu VM kullanın. Kayıt defterinde Azure Active Directory kimlik doğrulamasını kullanmak için, Azure [CLI'yi][azure-cli] VM'ye de yükleyin. Zaten bir Azure sanal makineniz varsa, bu oluşturma adımını atlayın.

Sanal makineniz ve konteyner kayıt defteriniz için aynı kaynak grubunu kullanabilirsiniz. Bu kurulum, sonunda temizleme basitleştirir, ancak gerekli değildir. Sanal makine ve sanal ağ için ayrı bir kaynak grubu oluşturmayı seçerseniz, [az grubu oluşturmayı çalıştırın.][az-group-create] Aşağıdaki örnek, *westcentralus* konumunda *myResourceGroup* adında bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location westus
```

Şimdi [az vm oluşturmak][az-vm-create]ile varsayılan bir Ubuntu Azure sanal makine dağıtmak. Aşağıdaki örnek *myDockerVM*adlı bir VM oluşturur:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

VM’nin oluşturulması birkaç dakika sürer. Komut tamamlandığında, Azure CLI `publicIpAddress` tarafından görüntülenenlere dikkat edin. VM'ye SSH bağlantıları yapmak ve isteğe bağlı olarak daha sonra güvenlik duvarı kurallarının kurulumu için bu adresi kullanın.

### <a name="install-docker-on-the-vm"></a>Docker'ı VM'ye yükleyin

VM çalışmaya devam ettikten sonra, VM'ye Bir SSH bağlantısı kurun. *PublicIpAddress'i* VM'nizin genel IP adresiyle değiştirin.

```bash
ssh azureuser@publicIpAddress
```

Docker'ı Ubuntu VM'ye yüklemek için aşağıdaki komutu çalıştırın:

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

Azure CLI'yi Ubuntu sanal makinenize yüklemek için [Azure CLI'yi](/cli/azure/install-azure-cli-apt?view=azure-cli-latest) yükleyin' i yükleyin adımlarını izleyin. Bu makale için sürüm 2.0.58 veya sonraki sürümyüklediğinizden emin olun.

SSH bağlantısından çıkın.

## <a name="allow-access-from-a-virtual-network"></a>Sanal ağdan erişime izin verme

Bu bölümde, bir Azure sanal ağındaki bir alt ağdan erişime izin verecek şekilde konteyner kayıt defterinizi yapılandırın. Azure CLI ve Azure portalı kullanılarak eşdeğer adımlar sağlanır.

### <a name="allow-access-from-a-virtual-network---cli"></a>Sanal ağdan erişime izin ver - CLI

#### <a name="add-a-service-endpoint-to-a-subnet"></a>Alt ağa hizmet bitiş noktası ekleme

Bir VM oluşturduğunuzda, Azure varsayılan olarak aynı kaynak grubunda sanal bir ağ oluşturur. Sanal ağın adı sanal makinenin adını temel alın. Örneğin, sanal makine *myDockerVM*adını alırsanız, varsayılan sanal ağ adı *myDockerVMVNET*, *myDockerVMSubnet*adlı bir alt ağ ile . Bunu Azure portalında veya [az network vnet liste][az-network-vnet-list] komutunu kullanarak doğrulayın:

```azurecli
az network vnet list --resource-group myResourceGroup --query "[].{Name: name, Subnet: subnets[0].name}"
```

Çıktı:

```console
[
  {
    "Name": "myDockerVMVNET",
    "Subnet": "myDockerVMSubnet"
  }
]
```

Alt ağınıza bir **Microsoft.ContainerRegistry** hizmet bitiş noktası eklemek için [az network vnet subnet update][az-network-vnet-subnet-update] komutunu kullanın. Aşağıdaki komutta sanal ağınızın ve alt ağınızın adlarını değiştirin:

```azurecli
az network vnet subnet update \
  --name myDockerVMSubnet \
  --vnet-name myDockerVMVNET \
  --resource-group myResourceGroup \
  --service-endpoints Microsoft.ContainerRegistry
```

Alt ağın kaynak kimliğini almak için [az network vnet subnet show][az-network-vnet-subnet-show] komutunu kullanın. Ağ erişim kuralını yapılandırmak için daha sonraki bir adımda buna ihtiyacınız var.

```azurecli
az network vnet subnet show \
  --name myDockerVMSubnet \
  --vnet-name myDockerVMVNET \
  --resource-group myResourceGroup \
  --query "id"
  --output tsv
``` 

Çıktı:

```
/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myDockerVMVNET/subnets/myDockerVMSubnet
```

#### <a name="change-default-network-access-to-registry"></a>Varsayılan ağ erişimini kayıt defterine değiştirme

Varsayılan olarak, Bir Azure kapsayıcı kayıt defteri herhangi bir ağdaki ana bilgisayarlardan bağlantılara izin verir. Seçili bir ağa erişimi sınırlamak için, erişimi reddetmek için varsayılan eylemi değiştirin. Aşağıdaki [az acr güncelleme][az-acr-update] komutunda kayıt defterinizin adını değiştirin:

```azurecli
az acr update --name myContainerRegistry --default-action Deny
```

#### <a name="add-network-rule-to-registry"></a>Kayıt defterine ağ kuralı ekleme

Kayıt defterinize VM'nin alt ağından erişime izin veren bir ağ kuralı eklemek için [az acr ağ kuralı ekle][az-acr-network-rule-add] komutunu kullanın. Konteyner kayıt defterinin adını ve alt ağının kaynak kimliğini aşağıdaki komutta değiştirin: 

 ```azurecli
az acr network-rule add --name mycontainerregistry --subnet <subnet-resource-id>
```

Kayıt [defterine erişimi doğrulamaya devam edin.](#verify-access-to-the-registry)

### <a name="allow-access-from-a-virtual-network---portal"></a>Sanal ağdan erişime izin ver - portal

#### <a name="add-service-endpoint-to-subnet"></a>Alt ağa hizmet bitiş noktası ekleme

Bir VM oluşturduğunuzda, Azure varsayılan olarak aynı kaynak grubunda sanal bir ağ oluşturur. Sanal ağın adı sanal makinenin adını temel alın. Örneğin, sanal makine *myDockerVM*adını alırsanız, varsayılan sanal ağ adı *myDockerVMVNET*, *myDockerVMSubnet*adlı bir alt ağ ile .

Azure Kapsayıcı Kayıt Defteri için bir hizmet bitiş noktası nı bir alt ağa eklemek için:

1. [Azure portalının][azure-portal]üst kısmındaki arama kutusuna *sanal ağları*girin. **Sanal ağlar** arama sonuçlarında göründüğünde, bu ağları seçin.
1. Sanal ağlar listesinden, *myDockerVMVNET*gibi sanal makinenizin dağıtıldığı sanal ağı seçin.
1. **Ayarlar** **altında, Alt Ağlar'ı**seçin.
1. *MyDockerVMSubnet*gibi sanal makinenizin dağıtıldığı alt ağı seçin.
1. **Hizmet bitiş noktaları**altında, **Microsoft.ContainerRegistry'yi**seçin.
1. **Kaydet'i**seçin.

![Alt ağa hizmet bitiş noktası ekleme][acr-subnet-service-endpoint] 

#### <a name="configure-network-access-for-registry"></a>Kayıt defteri için ağ erişimini yapılandırma

Varsayılan olarak, Bir Azure kapsayıcı kayıt defteri herhangi bir ağdaki ana bilgisayarlardan bağlantılara izin verir. Sanal ağa erişimi sınırlamak için:

1. Portalda, konteyner kayıt defterinize gidin.
1. **Ayarlar** **altında, Güvenlik Duvarı ve sanal ağları**seçin.
1. Varsayılan olarak erişimi reddetmek için, **Seçili ağlardan**erişime izin vermeyi seçin. 
1. **Varolan sanal ağ ekle'yi**seçin ve bir hizmet bitiş noktasıyla yapılandırdığınız sanal ağı ve alt ağı seçin. **Ekle'yi**seçin.
1. **Kaydet'i**seçin.

![Konteyner kayıt defteri için sanal ağı yapılandırma][acr-vnet-portal]

Kayıt [defterine erişimi doğrulamaya devam edin.](#verify-access-to-the-registry)

## <a name="allow-access-from-an-ip-address"></a>IP adresinden erişime izin verme

Bu bölümde, belirli bir IP adresinden veya aralığından erişime izin verecek şekilde konteyner kayıt defterinizi yapılandırın. Azure CLI ve Azure portalı kullanılarak eşdeğer adımlar sağlanır.

### <a name="allow-access-from-an-ip-address---cli"></a>IP adresinden erişime izin ver - CLI

#### <a name="change-default-network-access-to-registry"></a>Varsayılan ağ erişimini kayıt defterine değiştirme

Bunu daha önce yapmadıysanız, varsayılan olarak erişimi reddetmek için kayıt defteri yapılandırmasını güncelleştirin. Aşağıdaki [az acr güncelleme][az-acr-update] komutunda kayıt defterinizin adını değiştirin:

```azurecli
az acr update --name myContainerRegistry --default-action Deny
```

#### <a name="remove-network-rule-from-registry"></a>Ağ kuralını kayıt defterinden kaldırma

VM'nin alt ağından erişime izin vermek için daha önce bir ağ kuralı eklediyseniz, alt netin hizmet bitiş noktasını ve ağ kuralını kaldırın. [Az acr ağ kuralı kaldırma][az-acr-network-rule-remove] komutunda önceki bir adımda aldığınız alt ağın kapsayıcı kayıt defterinin adını ve kaynak kimliğini değiştirin: 

```azurecli
# Remove service endpoint

az network vnet subnet update \
  --name myDockerVMSubnet \
  --vnet-name myDockerVMVNET \
  --resource-group myResourceGroup \
  --service-endpoints ""

# Remove network rule

az acr network-rule remove --name mycontainerregistry --subnet <subnet-resource-id>
```

#### <a name="add-network-rule-to-registry"></a>Kayıt defterine ağ kuralı ekleme

Kayıt defterinize VM'nin IP adresinden erişime izin veren bir ağ kuralı eklemek için [az acr ağ kuralı ekleme][az-acr-network-rule-add] komutunu kullanın. Aşağıdaki komutta konteyner kayıt defterinin adını ve VM'nin genel IP adresini değiştirin.

```azurecli
az acr network-rule add --name mycontainerregistry --ip-address <public-IP-address>
```

Kayıt [defterine erişimi doğrulamaya devam edin.](#verify-access-to-the-registry)

### <a name="allow-access-from-an-ip-address---portal"></a>IP adresinden erişime izin ver - portal

#### <a name="remove-existing-network-rule-from-registry"></a>Varolan ağ kuralını kayıt defterinden kaldırma

VM'nin alt ağından erişime izin vermek için daha önce bir ağ kuralı eklediyseniz, varolan kuralı kaldırın. Kayıt defterine farklı bir VM'den erişmek istiyorsanız bu bölümü atlayın.

* Azure Kapsayıcı Kayıt Defteri için alt netin hizmet bitiş noktasını kaldırmak için alt ağ ayarlarını güncelleştirin. 

  1. Azure [portalında,][azure-portal]sanal makinenizin dağıtıldığı sanal ağa gidin.
  1. **Ayarlar** **altında, Alt Ağlar'ı**seçin.
  1. Sanal makinenizin dağıtıldığı alt ağı seçin.
  1. **Hizmet bitiş noktaları**altında, **Microsoft.ContainerRegistry**için onay kutusunu kaldırın. 
  1. **Kaydet'i**seçin.

* Alt ağın kayıt defterine erişmesine izin veren ağ kuralını kaldırın.

  1. Portalda, konteyner kayıt defterinize gidin.
  1. **Ayarlar** **altında, Güvenlik Duvarı ve sanal ağları**seçin.
  1. **Sanal ağlar**altında, sanal ağın adını seçin ve sonra **Kaldır'ı**seçin.
  1. **Kaydet'i**seçin.

#### <a name="add-network-rule-to-registry"></a>Kayıt defterine ağ kuralı ekleme

1. Portalda, konteyner kayıt defterinize gidin.
1. **Ayarlar** **altında, Güvenlik Duvarı ve sanal ağları**seçin.
1. Bunu daha önce yapmadıysanız, **Seçili ağlardan**erişime izin vermeyi seçin. 
1. **Sanal ağlar**altında, hiçbir ağın seçilmediğinden emin olun.
1. **Güvenlik Duvarı**altında, bir VM'nin genel IP adresini girin. Veya, VM'nin IP adresini içeren CIDR gösteriminde bir adres aralığı girin.
1. **Kaydet'i**seçin.

![Konteyner kayıt defteri için güvenlik duvarı kuralını yapılandırma][acr-vnet-firewall-portal]

Kayıt [defterine erişimi doğrulamaya devam edin.](#verify-access-to-the-registry)

## <a name="verify-access-to-the-registry"></a>Kayıt defterine erişimi doğrulama

Yapılandırmanın güncellendirilen birkaç dakika bekledikten sonra, VM'nin konteyner kayıt defterine erişebiliyor olduğundan doğrulayın. VM'nize bir SSH bağlantısı yapın ve kayıt defterinize giriş yapmak için [az acr giriş][az-acr-login] komutunu çalıştırın. 

```bash
az acr login --name mycontainerregistry
```

Kayıt defterinden örnek bir `docker pull` görüntü çekmek için çalıştır gibi kayıt defteri işlemleri gerçekleştirebilirsiniz. Kayıt defteriniz için uygun, kayıt defteri giriş sunucusu adı (tüm küçük harf) ile önceden belirlenmiş bir resim ve etiket değeri değiştirin:

```bash
docker pull mycontainerregistry.azurecr.io/hello-world:v1
``` 

Docker görüntüyü VM'ye başarıyla çeker.

Bu örnek, ağ erişim kuralı üzerinden özel kapsayıcı kayıt defterine erişebileceğinizi göstermektedir. Ancak, kayıt defterine ağ erişim kuralı yapılandırılan farklı bir giriş ana bilgisayarından erişilemez. `az acr login` Komut veya `docker login` komutu kullanarak başka bir ana bilgisayardan oturum açmaya çalışırsanız, çıktı aşağıdakilere benzer:

```Console
Error response from daemon: login attempt to https://xxxxxxx.azurecr.io/v2/ failed with status: 403 Forbidden
```

## <a name="restore-default-registry-access"></a>Varsayılan kayıt defteri erişimini geri yükleme

Varsayılan olarak erişime izin vermek için kayıt defterini geri yüklemek için yapılandırılan ağ kurallarını kaldırın. Ardından erişime izin vermek için varsayılan eylemi ayarlayın. Azure CLI ve Azure portalı kullanılarak eşdeğer adımlar sağlanır.

### <a name="restore-default-registry-access---cli"></a>Varsayılan kayıt defteri erişimini geri yükleme - CLI

#### <a name="remove-network-rules"></a>Ağ kurallarını kaldırma

Kayıt defteriniz için yapılandırılan ağ kurallarının listesini görmek için aşağıdaki [az acr ağ kuralı listesi][az-acr-network-rule-list] komutunu çalıştırın:

```azurecli
az acr network-rule list--name mycontainerregistry 
```

Yapılandırılan her kural için, kaldırmak için [az acr ağ kuralı kaldır][az-acr-network-rule-remove] komutunu çalıştırın. Örnek:

```azurecli
# Remove a rule that allows access for a subnet. Substitute the subnet resource ID.

az acr network-rule remove \
  --name mycontainerregistry \
  --subnet /subscriptions/ \
  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myDockerVMVNET/subnets/myDockerVMSubnet

# Remove a rule that allows access for an IP address or CIDR range such as 23.45.1.0/24.

az acr network-rule remove \
  --name mycontainerregistry \
  --ip-address 23.45.1.0/24
```

#### <a name="allow-access"></a>Erişime izin ver

Aşağıdaki [az acr güncelleme][az-acr-update] komutunda kayıt defterinizin adını değiştirin:
```azurecli
az acr update --name myContainerRegistry --default-action Allow
```

### <a name="restore-default-registry-access---portal"></a>Varsayılan kayıt defteri erişimini geri yükleme - portal


1. Portalda, konteyner kayıt defterinize gidin ve **Güvenlik Duvarı ve sanal ağları**seçin.
1. **Sanal ağlar**altında, her sanal ağı seçin ve sonra **Kaldır'ı**seçin.
1. **Güvenlik Duvarı'nın**altında, her adres aralığını seçin ve sonra Sil simgesini seçin.
1. **Erişime izin ver**altında, **Tüm ağları**seçin. 
1. **Kaydet'i**seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Tüm Azure kaynaklarını aynı kaynak grubunda oluşturduysanız ve artık bunlara gerek yoksa, tek bir [az grubu silme](/cli/azure/group) komutunu kullanarak kaynakları isteğe bağlı olarak silebilirsiniz:

```azurecli
az group delete --name myResourceGroup
```

Portaldaki kaynaklarınızı temizlemek için myResourceGroup kaynak grubuna gidin. Kaynak grubu yüklendikten sonra, kaynak grubunu ve depolanan kaynakları kaldırmak için **Kaynak Grubunu Sil'i** tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kısa bir süre için de çeşitli sanal ağ kaynakları ve özellikleri ele alınmıştır. Azure Sanal Ağ belgeleri bu konuları kapsamlı bir şekilde kapsamaktadır:

* [Sanal ağ](https://docs.microsoft.com/azure/virtual-network/manage-virtual-network)
* [Alt ağ](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet)
* [Hizmet uç noktaları](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview)

<!-- IMAGES -->

[acr-subnet-service-endpoint]: ./media/container-registry-vnet/acr-subnet-service-endpoint.png
[acr-vnet-portal]: ./media/container-registry-vnet/acr-vnet-portal.png
[acr-vnet-firewall-portal]: ./media/container-registry-vnet/acr-vnet-firewall-portal.png

<!-- LINKS - External -->
[aci-helloworld]: https://hub.docker.com/r/microsoft/aci-helloworld/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-acr-repository-show]: /cli/azure/acr/repository#az-acr-repository-show
[az-acr-repository-list]: /cli/azure/acr/repository#az-acr-repository-list
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-network-rule-add]: /cli/azure/acr/network-rule/#az-acr-network-rule-add
[az-acr-network-rule-remove]: /cli/azure/acr/network-rule/#az-acr-network-rule-remove
[az-acr-network-rule-list]: /cli/azure/acr/network-rule/#az-acr-network-rule-list
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-acr-update]: /cli/azure/acr#az-acr-update
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-group-create]: /cli/azure/group
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-vm-create]: /cli/azure/vm#az-vm-create
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-show
[az-network-vnet-subnet-update]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-update
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-show
[az-network-vnet-list]: /cli/azure/network/vnet/#az-network-vnet-list
[quickstart-portal]: container-registry-get-started-portal.md
[quickstart-cli]: container-registry-get-started-azure-cli.md
[azure-portal]: https://portal.azure.com
