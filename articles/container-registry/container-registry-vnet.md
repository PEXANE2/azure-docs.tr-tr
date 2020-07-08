---
title: Hizmet uç noktası kullanarak erişimi kısıtlama
description: Azure sanal ağındaki bir hizmet uç noktasını kullanarak bir Azure Container Registry 'ye erişimi kısıtlayın. Hizmet uç noktası erişimi, Premium hizmet katmanının bir özelliğidir.
ms.topic: article
ms.date: 05/04/2020
ms.openlocfilehash: 0f320bb86549c801711cafdbce4500ff7737cb89
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84509296"
---
# <a name="restrict-access-to-a-container-registry-using-a-service-endpoint-in-an-azure-virtual-network"></a>Bir Azure sanal ağında hizmet uç noktası kullanarak bir kapsayıcı kayıt defterine erişimi kısıtlama

[Azure sanal ağ](../virtual-network/virtual-networks-overview.md) , Azure ve şirket içi kaynaklarınız için güvenli, özel ağ sağlar. [Hizmet uç noktası](../virtual-network/virtual-network-service-endpoints-overview.md) , kapsayıcı kayıt DEFTERINIZIN genel IP adresini yalnızca sanal ağınıza güvenli hale bırakmanıza olanak tanır. Bu uç nokta, trafiğe Azure omurga ağı üzerinden en uygun yolu verir. Sanal ağın ve alt ağın kimlikleri de her istekle birlikte iletilir.

Bu makalede bir sanal ağda kapsayıcı kayıt defteri hizmeti uç noktasının (Önizleme) nasıl yapılandırılacağı gösterilmektedir. 

> [!IMPORTANT]
> Azure Container Registry artık [Azure özel bağlantısını](container-registry-private-link.md)desteklediğinden, bir sanal ağdan bir kayıt defterine yerleştirilmesi için özel uç noktalar etkinleştiriliyor. Özel uç noktalara, özel IP adresleri kullanılarak sanal ağ içinden erişilebilir. Çoğu ağ senaryosunda hizmet uç noktaları yerine özel uç noktalar kullanmanızı öneririz.

Bir kayıt defteri hizmet uç noktası yapılandırmak **Premium** kapsayıcı kayıt defteri hizmet katmanında kullanılabilir. Kayıt defteri hizmeti katmanları ve limitleri hakkında bilgi için bkz. [Azure Container Registry hizmet katmanları](container-registry-skus.md).

## <a name="preview-limitations"></a>Önizleme sınırlamaları

* Azure Container Registry için hizmet uç noktalarının gelecekte geliştirilmesi planlanmamaktadır. Bunun yerine [Özel uç noktalar](container-registry-private-link.md) kullanmanızı öneririz.
* Kayıt defterindeki hizmet uç noktalarını yapılandırmak için Azure portal kullanamazsınız.
* Bir hizmet uç noktası kullanılarak kapsayıcı kayıt defterine erişmek için bir konak olarak yalnızca bir [Azure Kubernetes hizmet](../aks/intro-kubernetes.md) kümesi veya Azure [sanal makinesi](../virtual-machines/linux/overview.md) kullanılabilir. *Azure Container Instances dahil diğer Azure hizmetleri desteklenmez.*
* Her kayıt defteri en fazla 100 ağ erişim kuralını destekler.

## <a name="prerequisites"></a>Ön koşullar

* Bu makalede Azure CLı adımlarını kullanmak için Azure CLı sürüm 2.0.58 veya üzeri gereklidir. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli].

* Zaten bir kapsayıcı kayıt defteriniz yoksa, bir tane oluşturun (Premium katman gereklidir) ve Docker Hub 'dan gibi örnek bir görüntü gönderin `hello-world` . Örneğin, [Azure Portal][quickstart-portal] veya [Azure CLI][quickstart-cli] kullanarak bir kayıt defteri oluşturun. 

* Farklı bir Azure aboneliğindeki bir hizmet uç noktası kullanarak kayıt defteri erişimini kısıtlamak istiyorsanız, bu abonelikte Azure Container Registry kaynak sağlayıcısını kaydedin. Örneğin:

  ```azurecli
  az account set --subscription <Name or ID of subscription of virtual network>

  az provider register --namespace Microsoft.ContainerRegistry
  ``` 

[!INCLUDE [Set up Docker-enabled VM](../../includes/container-registry-docker-vm-setup.md)]

## <a name="configure-network-access-for-registry"></a>Kayıt defteri için ağ erişimini yapılandırma

Bu bölümde, kapsayıcı kayıt defterinizi Azure sanal ağındaki bir alt ağdan erişime izin verecek şekilde yapılandırın. Azure CLı ve Azure portal ile eşdeğer adımlar sağlanır.

### <a name="allow-access-from-a-virtual-network---cli"></a>Bir sanal ağ üzerinden erişime izin ver-CLı

#### <a name="add-a-service-endpoint-to-a-subnet"></a>Bir alt ağa hizmet uç noktası ekleme

Bir VM oluşturduğunuzda, Azure varsayılan olarak aynı kaynak grubunda bir sanal ağ oluşturur. Sanal ağın adı, sanal makinenin adını temel alır. Örneğin, *Mydockervm*sanal makinenizi adlandırdıysanız, varsayılan sanal ağ adı Mydockervmvnet adlı bir alt ağ ile Mydockervmvnet *olur.* *myDockerVMVNET* Bunu Azure portal veya [az Network VNET List][az-network-vnet-list] komutunu kullanarak doğrulayın:

```azurecli
az network vnet list \
  --resource-group myResourceGroup \
  --query "[].{Name: name, Subnet: subnets[0].name}"
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

Alt ağınıza bir **Microsoft. ContainerRegistry** hizmeti uç noktası eklemek için [az Network VNET subnet Update][az-network-vnet-subnet-update] komutunu kullanın. Aşağıdaki komutta, sanal ağınızın ve alt ağınızın adlarını değiştirin:

```azurecli
az network vnet subnet update \
  --name myDockerVMSubnet \
  --vnet-name myDockerVMVNET \
  --resource-group myResourceGroup \
  --service-endpoints Microsoft.ContainerRegistry
```

Alt ağın kaynak KIMLIĞINI almak için [az Network VNET subnet Show][az-network-vnet-subnet-show] komutunu kullanın. Bu, bir ağ erişim kuralını yapılandırmak için sonraki bir adımda gerekli olacaktır.

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

#### <a name="change-default-network-access-to-registry"></a>Kayıt defteri için varsayılan ağ erişimini değiştirme

Varsayılan olarak, bir Azure Container Registry, herhangi bir ağdaki konaklardan gelen bağlantılara izin verir. Seçilen bir ağa erişimi sınırlandırmak için, varsayılan eylemi erişimi Reddet olarak değiştirin. Kayıt defterinizin adını şu [az ACR Update][az-acr-update] komutunda değiştirin:

```azurecli
az acr update --name myContainerRegistry --default-action Deny
```

#### <a name="add-network-rule-to-registry"></a>Kayıt defterine ağ kuralı ekle

Kayıt defterinize VM 'nin alt ağından erişime izin veren bir ağ kuralı eklemek için [az ACR Network-Rule Add][az-acr-network-rule-add] komutunu kullanın. Aşağıdaki komutta, kapsayıcı kayıt defterinin adını ve alt ağın kaynak KIMLIĞINI değiştirin: 

 ```azurecli
az acr network-rule add \
  --name mycontainerregistry \
  --subnet <subnet-resource-id>
```

## <a name="verify-access-to-the-registry"></a>Kayıt defterine erişimi doğrulama

Yapılandırmanın güncelleştirilmesi birkaç dakika bekledikten sonra, sanal makinenin kapsayıcı kayıt defterine erişebildiğini doğrulayın. Sanal makinenize bir SSH bağlantısı oluşturun ve [az ACR Login][az-acr-login] komutunu çalıştırarak kayıt defterinizde oturum açın. 

```bash
az acr login --name mycontainerregistry
```

`docker pull`Kayıt defterinden örnek bir görüntü çekmek için çalıştırma gibi kayıt defteri işlemleri gerçekleştirebilirsiniz. Kayıt defteri oturum açma sunucusu adı (tümü küçük harf) önekini kullanarak kayıt defteriniz için uygun bir görüntü ve etiket değeri koyun:

```bash
docker pull mycontainerregistry.azurecr.io/hello-world:v1
``` 

Docker görüntüyü sanal makineye başarıyla çeker.

Bu örnekte, özel kapsayıcı kayıt defterine ağ erişim kuralıyla erişebileceğiniz gösterilmektedir. Bununla birlikte, kayıt defterine, yapılandırılmış bir ağ erişim kuralına sahip olmayan bir oturum açma konağından erişilemez. Komutunu veya komutunu kullanarak başka bir ana bilgisayardan oturum açmaya çalışırsanız `az acr login` `docker login` , çıkış aşağıdakine benzer:

```Console
Error response from daemon: login attempt to https://xxxxxxx.azurecr.io/v2/ failed with status: 403 Forbidden
```

## <a name="restore-default-registry-access"></a>Varsayılan kayıt defteri erişimini geri yükle

Kayıt defterini varsayılan olarak erişime izin verecek şekilde geri yüklemek için, yapılandırılmış tüm ağ kurallarını kaldırın. Ardından, varsayılan eylemi erişime izin verecek şekilde ayarlayın. Azure CLı ve Azure portal ile eşdeğer adımlar sağlanır.

### <a name="restore-default-registry-access---cli"></a>Varsayılan kayıt defteri erişimini geri yükleme-CLı

#### <a name="remove-network-rules"></a>Ağ kurallarını kaldır

Kayıt defteriniz için yapılandırılmış ağ kurallarının bir listesini görmek için şu [az ACR Network-Rule List][az-acr-network-rule-list] komutunu çalıştırın:

```azurecli
az acr network-rule list --name mycontainerregistry 
```

Yapılandırılan her kural için [az ACR Network-Rule Remove][az-acr-network-rule-remove] komutunu çalıştırarak kaldırın. Örneğin:

```azurecli
# Remove a rule that allows access for a subnet. Substitute the subnet resource ID.

az acr network-rule remove \
  --name mycontainerregistry \
  --subnet /subscriptions/ \
  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myDockerVMVNET/subnets/myDockerVMSubnet
```

#### <a name="allow-access"></a>Erişime izin ver

Kayıt defterinizin adını şu [az ACR Update][az-acr-update] komutunda değiştirin:
```azurecli
az acr update --name myContainerRegistry --default-action Allow
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Tüm Azure kaynaklarını aynı kaynak grubunda oluşturduysanız ve artık gerekmiyorsa, tek bir [az Group Delete](/cli/azure/group) komutu kullanarak kaynakları isteğe bağlı olarak silebilirsiniz:

```azurecli
az group delete --name myResourceGroup
```

Portalda kaynaklarınızı temizlemek için myResourceGroup kaynak grubuna gidin. Kaynak grubu yüklendikten sonra kaynak grubunu **Sil** ' e tıklayarak kaynak grubunu ve burada depolanan kaynakları kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

* Bir sanal ağda özel bir uç nokta kullanarak bir kayıt defterine erişimi kısıtlamak için bkz. Azure [Container kayıt defteri Için Azure özel bağlantısını yapılandırma](container-registry-private-link.md).
* Bir istemci güvenlik duvarının ardında kayıt defteri erişim kuralları ayarlamanız gerekiyorsa, bkz. [güvenlik duvarı arkasındaki bir Azure Container Registry 'ye erişmek için kuralları yapılandırma](container-registry-firewall-access-rules.md).


<!-- IMAGES -->

[acr-subnet-service-endpoint]: ./media/container-registry-vnet/acr-subnet-service-endpoint.png


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
