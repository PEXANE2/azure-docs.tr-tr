---
title: Özel bağlantı ayarlama
description: Konteyner kayıt defterinde özel bir bitiş noktası ayarlama ve yerel sanal ağda özel bir bağlantı etkinleştirme
ms.topic: article
ms.date: 03/10/2020
ms.openlocfilehash: de8228d84497e71f24dba3dd4e6162cb6735a8c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79498921"
---
# <a name="configure-azure-private-link-for-an-azure-container-registry"></a>Azure kapsayıcı kayıt defteri için Azure Özel Bağlantısını yapılandırma 

Azure kapsayıcı kayıt defteriniz için özel bir [bitiş noktası](../private-link/private-endpoint-overview.md) ayarlayabilirsiniz, böylece bir Azure sanal ağındaki istemciler özel bir [bağlantı](../private-link/private-link-overview.md)üzerinden kayıt defterine güvenli bir şekilde erişebilirsiniz. Özel bitiş noktası, kayıt defteriniz için sanal ağ adres alanından bir IP adresi kullanır. Sanal ağdaki istemciler ile kayıt defteri arasındaki ağ trafiği, sanal ağa ve Microsoft omurga ağındaki özel bir bağlantıdan geçerek genel internetten açığa çıkarı ortadan kaldırır.

Özel bitiş noktanız için [DNS ayarlarını yapılandırabilirsiniz,](../private-link/private-endpoint-overview.md#dns-configuration) böylece ayarlar kayıt defterinin ayrılan özel IP adresine gider. DNS yapılandırması ile, istemciler ve ağdaki hizmetler, kayıt defterine *myregistry.azurecr.io*gibi tam nitelikli alan adı olan kayıt defterine erişmeye devam edebilir.

Bu özellik **Premium** konteyner kayıt hizmeti katmanında kullanılabilir. Kayıt defteri hizmeti katmanları ve sınırları hakkında daha fazla bilgi için [Azure Konteyner Kayıt Defteri SK'leri'ne](container-registry-skus.md)bakın.

> [!IMPORTANT]
> Bu özellik şu anda önizlemededir ve bazı [sınırlamalar](#preview-limitations) geçerlidir. Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="preview-limitations"></a>Önizleme sınırlamaları

* Şu anda, [coğrafi olarak çoğaltılan](container-registry-geo-replication.md)bir kayıt defterinde özel bir bitiş noktası olan özel bir bağlantı ayarlayamadığınız. 

## <a name="prerequisites"></a>Ön koşullar

* Bu makalede Azure CLI adımlarını kullanmak için Azure CLI sürüm 2.2.0 veya sonrası önerilir. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli]. Veya [Azure Bulut Shell'de](../cloud-shell/quickstart.md)çalıştırın.
* Zaten bir konteyner kayıt defteriniz yoksa, bir tane (Premium katmanı gereklidir) `hello-world` oluşturun ve Docker Hub gibi örnek bir görüntü itin. Örneğin, bir kayıt defteri oluşturmak için [Azure portalını][quickstart-portal] veya [Azure CLI'yi][quickstart-cli] kullanın.
* Farklı bir Azure aboneliğinde özel bir bağlantı kullanarak kayıt defteri erişimini yapılandırmak istiyorsanız, bu aboneliğe Azure Kapsayıcı Kayıt Defteri için kaynak sağlayıcısını kaydetmeniz gerekir. Örnek:

  ```azurecli
  az account set --subscription <Name or ID of subscription of private link>

  az provider register --namespace Microsoft.ContainerRegistry
  ``` 

Bu makaledeki Azure CLI örnekleri aşağıdaki ortam değişkenlerini kullanır. Ortamınız için uygun değerleri değiştirin. Tüm örnekler Bash kabuğu için biçimlendirilir:

```bash
registryName=<container-registry-name>
registryLocation=<container-registry-location> # Azure region such as westeurope where registry created
resourceGroup=<resource-group-name>
vmName=<virtual-machine-name>
```

## <a name="create-a-docker-enabled-virtual-machine"></a>Docker özellikli sanal makine oluşturma

Test amacıyla, Bir Azure kapsayıcı kayıt defterine erişmek için Docker özellikli bir Ubuntu VM kullanın. Kayıt defterinde Azure Active Directory kimlik doğrulamasını kullanmak için, Azure [CLI'yi][azure-cli] VM'ye de yükleyin. Zaten bir Azure sanal makineniz varsa, bu oluşturma adımını atlayın.

Sanal makineniz ve konteyner kayıt defteriniz için aynı kaynak grubunu kullanabilirsiniz. Bu kurulum, sonunda temizleme basitleştirir, ancak gerekli değildir. Sanal makine ve sanal ağ için ayrı bir kaynak grubu oluşturmayı seçerseniz, [az grubu oluşturmayı][az-group-create]çalıştırın:

```azurecli
az group create --name $resourceGroup --location $registryLocation
```

Şimdi [az vm oluşturmak][az-vm-create]ile varsayılan bir Ubuntu Azure sanal makine dağıtmak. Aşağıdaki örnek, *myDockerVM*adında bir VM oluşturur.

```azurecli
az vm create \
  --resource-group $resourceGroup \
  --name $vmName \
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

Docker'ı Ubuntu VM'ye yüklemek için aşağıdaki komutları çalıştırın:

```bash
sudo apt-get update
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

Azure CLI'yi Ubuntu sanal makinenize yüklemek için [Azure CLI'yi](/cli/azure/install-azure-cli-apt?view=azure-cli-latest) yükleyin' i yükleyin adımlarını izleyin. Örnek:

```bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

SSH bağlantısından çıkın.

## <a name="set-up-private-link---cli"></a>Özel bağlantı kurun - CLI

### <a name="get-network-and-subnet-names"></a>Ağ ve alt ağ adlarını alma

Bunları zaten yoksa, özel bir bağlantı kurmak için sanal bir ağ ve alt ağ adlarına ihtiyacınız vardır. Bu örnekte, VM ve kayıt defterinin özel bitiş noktası için aynı alt ağı kullanırsınız. Ancak, birçok senaryoda bitiş noktasını ayrı bir alt ağda ayarlarsınız. 

Bir VM oluşturduğunuzda, Azure varsayılan olarak aynı kaynak grubunda sanal bir ağ oluşturur. Sanal ağın adı sanal makinenin adını temel alın. Örneğin, sanal makine *myDockerVM*adını alırsanız, varsayılan sanal ağ adı *myDockerVMVNET*, *myDockerVMSubnet*adlı bir alt ağ ile . [Az network vnet list][az-network-vnet-list] komutunu çalıştırarak ortam değişkenlerinde bu değerleri ayarlayın:

```azurecli
networkName=$(az network vnet list \
  --resource-group $resourceGroup \
  --query '[].{Name: name}' --output tsv)

subnetName=$(az network vnet list \
  --resource-group $resourceGroup \
  --query '[].{Subnet: subnets[0].name}' --output tsv)

echo networkName=$networkName
echo subnetName=$subnetName
```

### <a name="disable-network-policies-in-subnet"></a>Alt ağdaki ağ ilkelerini devre dışı

Alt ağdaki ağ güvenlik grupları gibi [ağ ilkelerini](../private-link/disable-private-endpoint-network-policy.md) özel bitiş noktası için devre dışı kalım. [Az network vnet subnet güncellemesi][az-network-vnet-subnet-update]ile subnet yapılandırmanızı güncelleyin:

```azurecli
az network vnet subnet update \
 --name $subnetName \
 --vnet-name $networkName \
 --resource-group $resourceGroup \
 --disable-private-endpoint-network-policies
```

### <a name="configure-the-private-dns-zone"></a>Özel DNS bölgesini yapılandırma

Azure kapsayıcı kayıt alanının özelleştirilmesi için özel bir DNS bölgesi oluşturun. Daha sonraki adımlarda, bu DNS bölgesi içindeki kayıt alanı etki alanınız için DNS kayıtları oluşturursunuz.

Azure kapsayıcı kayıt defteriniz için varsayılan DNS çözünürlüğünü geçersiz kılmak için özel bir bölge kullanmak için, bölge **privatelink.azurecr.io.** Özel bölge oluşturmak için aşağıdaki [az ağ özel-dns bölge oluşturma][az-network-private-dns-zone-create] komutunu çalıştırın:

```azurecli
az network private-dns zone create \
  --resource-group $resourceGroup \
  --name "privatelink.azurecr.io"
```

### <a name="create-an-association-link"></a>İlişkilendirme bağlantısı oluşturma

Özel bölgenizi sanal ağla ilişkilendirmek için [az network private-dns linkvnet oluşturun.][az-network-private-dns-link-vnet-create] Bu örnek *myDNSLink*adlı bir bağlantı oluşturur.

```azurecli
az network private-dns link vnet create \
  --resource-group $resourceGroup \
  --zone-name "privatelink.azurecr.io" \
  --name MyDNSLink \
  --virtual-network $networkName \
  --registration-enabled false
```

### <a name="create-a-private-registry-endpoint"></a>Özel kayıt defteri bitiş noktası oluşturma

Bu bölümde, sanal ağda kayıt defterinin özel bitiş noktasını oluşturun. İlk olarak, kayıt defterinizin kaynak kimliğini alın:

```azurecli
registryID=$(az acr show --name $registryName \
  --query 'id' --output tsv)
```

Kayıt defterinin özel bitiş noktasını oluşturmak için [az ağ özel uç noktası oluşturma][az-network-private-endpoint-create] komutunu çalıştırın.

Aşağıdaki örnek, *myPrivateEndpoint* ve hizmet bağlantısı *myConnection*bitiş noktası oluşturur. Bitiş noktası için bir kapsayıcı kayıt defteri `--group-ids registry`kaynağı belirtmek için:

```azurecli
az network private-endpoint create \
    --name myPrivateEndpoint \
    --resource-group $resourceGroup \
    --vnet-name $networkName \
    --subnet $subnetName \
    --private-connection-resource-id $registryID \
    --group-ids registry \
    --connection-name myConnection
```

### <a name="get-private-ip-addresses"></a>Özel IP adresleri alın

Ağ arabirimi kimliği için bitiş noktasını sorgulamak için [az ağ özel bitiş noktası gösterisini][az-network-private-endpoint-show] çalıştırın:

```azurecli
networkInterfaceID=$(az network private-endpoint show \
  --name myPrivateEndpoint \
  --resource-group $resourceGroup \
  --query 'networkInterfaces[0].id' \
  --output tsv)
```

Ağ arabirimi yle ilişkili olarak, kapsayıcı kayıt defteri için iki özel IP adresi bulunur: biri kayıt defterinin kendisi için, diğeri de kayıt defterinin veri bitiş noktası için. Kapsayıcı kayıt defteri ve kayıt defterinin veri bitiş noktası için özel IP adreslerini almak için aşağıdaki [az kaynak gösteri][az-resource-show] komutlarını çalıştırın:

```azurecli
privateIP=$(az resource show \
  --ids $networkInterfaceID \
  --api-version 2019-04-01 --query 'properties.ipConfigurations[1].properties.privateIPAddress' \
  --output tsv)

dataEndpointPrivateIP=$(az resource show \
  --ids $networkInterfaceID \
  --api-version 2019-04-01 \
  --query 'properties.ipConfigurations[0].properties.privateIPAddress' \
  --output tsv)
```

### <a name="create-dns-records-in-the-private-zone"></a>Özel bölgede DNS kayıtları oluşturma

Aşağıdaki komutlar, kayıt defteri bitiş noktası ve veri bitiş noktası için özel bölgede DNS kayıtları oluşturur. Örneğin, *westeurope* bölgesinde *myregistry* adlı bir kayıt defteri varsa, bitiş `myregistry.azurecr.io` `myregistry.westeurope.data.azurecr.io`noktası adları ve . 

İlk çalıştırma [az ağ özel-dns kayıt-kayıt defteri][az-network-private-dns-record-set-a-create] bitiş noktası ve veri bitiş noktası için boş A kayıt kümeleri oluşturmak için bir oluşturma ayarlayın:

```azurecli
az network private-dns record-set a create \
  --name $registryName \
  --zone-name privatelink.azurecr.io \
  --resource-group $resourceGroup

# Specify registry region in data endpoint name
az network private-dns record-set a create \
  --name ${registryName}.${registryLocation}.data \
  --zone-name privatelink.azurecr.io \
  --resource-group $resourceGroup
```

Kayıt defteri bitiş noktası ve veri bitiş noktası için A kayıtları oluşturmak için [az ağ private-dns kayıt-set bir add-record][az-network-private-dns-record-set-a-add-record] komutu çalıştırın:

```azurecli
az network private-dns record-set a add-record \
  --record-set-name $registryName \
  --zone-name privatelink.azurecr.io \
  --resource-group $resourceGroup \
  --ipv4-address $privateIP

# Specify registry region in data endpoint name
az network private-dns record-set a add-record \
  --record-set-name ${registryName}.${registryLocation}.data \
  --zone-name privatelink.azurecr.io \
  --resource-group $resourceGroup \
  --ipv4-address $dataEndpointPrivateIP
```

Özel bağlantı şimdi yapılandırılmış ve kullanıma hazır.

## <a name="set-up-private-link---portal"></a>Özel bağlantı kurun - portal

Aşağıdaki adımlar, zaten bir sanal ağ ve alt ağ sınama için bir VM ile ayarlanmış olduğunu varsayalım. Ayrıca [yeni bir sanal ağ ve alt ağ oluşturabilirsiniz.](../virtual-network/quick-create-portal.md)

### <a name="create-a-private-endpoint"></a>Özel uç nokta oluşturma

1. Portalda, konteyner kayıt defterinize gidin.
1. **Ayarlar**altında, **Özel bitiş noktası bağlantılarını (Önizleme)** seçin.
1. + **Özel bitiş noktası seçin.**
1. Temel **Bilgiler** sekmesine aşağıdaki bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **Proje ayrıntıları** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | Varolan bir grubun adını girin veya yeni bir grup oluşturun.|
    | **Örnek ayrıntıları** |  |
    | Adı | Benzersiz bir ad girin. |
    |Bölge|Bir bölge seçin.|
    |||
5. **Sonraki seçin: Kaynak**.
6. Aşağıdaki bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    |Bağlantı yöntemi  | **Dizinimdeki Bir Azure kaynağına Bağlan'ı**seçin.|
    | Abonelik| Aboneliğinizi seçin. |
    | Kaynak türü | **Microsoft.ContainerRegistry/registries'i**seçin. |
    | Kaynak |Kayıt defterinizin adını seçin|
    |Hedef alt kaynak |**Kayıt defterini** seçin|
    |||
7. **Sonraki'ni seçin: Yapılandırma.**
8. Bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    |**Ağ Oluşturma**| |
    | Sanal ağ| *MyDockerVMVNET*gibi sanal makinenizin dağıtıldığı sanal ağı seçin. |
    | Alt ağ | Sanal makinenizin dağıtıldığı *myDockerVMSubnet* gibi bir alt ağ seçin. |
    |**Özel DNS Entegrasyonu**||
    |Özel DNS bölgesiyle tümleştirme |**Evet'i**seçin. |
    |Özel DNS Bölgesi |Seç *(Yeni) privatelink.azurecr.io* |
    |||

1. **İncele ve oluştur**’u seçin. Azure'un yapılandırmanızı doğruladığı **Gözden Geçirme + oluşturma** sayfasına götürülürnüz. 
2. **Validation geçirilen** iletiyi gördüğünüzde **Oluştur'u**seçin.

Özel bitiş noktası oluşturulduktan sonra, özel bölgedeki DNS ayarları bitiş noktasının **Genel Bakış** sayfasında görünür.

![Bitiş Noktası DNS ayarları](./media/container-registry-private-link/private-endpoint-overview.png)

Özel bağlantınız artık yapılandırıldı ve kullanıma hazır.

## <a name="validate-private-link-connection"></a>Özel bağlantı bağlantısını doğrulama

Özel bitiş noktasının alt netindeki kaynakların özel bir IP adresi üzerinden kayıt defterinize bağladığını ve doğru özel DNS bölge tümleştirmesine sahip olduğunu doğrulamanız gerekir.

Özel bağlantı bağlantısını doğrulamak için, Sanal ağda kurduğunuz sanal makineye SSH.

Kayıt `nslookup` defterinizin IP adresini özel bağlantı üzerinden çözmek için komutu çalıştırın:

```bash
nslookup $registryName.azurecr.io
```

Örnek çıktı, alt ağın adres alanında kayıt defterinin IP adresini gösterir:

```console
[...]
myregistry.azurecr.io       canonical name = myregistry.privatelink.azurecr.io.
Name:   myregistry.privatelink.azurecr.io
Address: 10.0.0.6
```

Bu sonucu, ortak bir `nslookup` bitiş noktası üzerinden aynı kayıt defteri için çıktıdaki ortak IP adresiyle karşılaştırın:

```console
[...]
Non-authoritative answer:
Name:   myregistry.westeurope.cloudapp.azure.com
Address: 40.78.103.41
```

### <a name="registry-operations-over-private-link"></a>Özel bağlantı üzerinden kayıt defteri işlemleri

Ayrıca, alt ağdaki sanal makineden kayıt defteri işlemleri gerçekleştirebileceğinizi de doğrulayın. Sanal makinenize bir SSH bağlantısı yapın ve kayıt defterinize giriş yapmak için [az acr oturum][az-acr-login] açın. VM yapılandırmanıza bağlı olarak, aşağıdaki komutları `sudo`.

```bash
az acr login --name $registryName
```

Kayıt defterinden örnek `docker pull` bir görüntü çekmek gibi kayıt defteri işlemleri gerçekleştirin. Kayıt `hello-world:v1` defteriniz için uygun bir resim ve etiketle değiştirin, kayıt defteri giriş sunucusu adı (tüm küçük harf):

```bash
docker pull myregistry.azurecr.io/hello-world:v1
``` 

Docker görüntüyü VM'ye başarıyla çeker.

## <a name="manage-private-endpoint-connections"></a>Özel uç nokta bağlantılarını yönetme

Azure portalını kullanarak veya [az acr private-endpoint-connection][az-acr-private-endpoint-connection] komut grubundaki komutları kullanarak kayıt defterinin özel uç nokta bağlantılarını yönetin. İşlemler, kayıt defterinin özel bitiş noktası bağlantılarının ayrıntılarını onaylamayı, silmeyi, listelemeyi, reddetmeyi veya göstermeyi içerir.

Örneğin, bir kayıt defterinin özel bitiş noktası bağlantılarını listelemek için [az acr private-endpoint-connection list][az-acr-private-endpoint-connection-list] komutunu çalıştırın. Örnek:

```azurecli
az acr private-endpoint-connection list \
  --registry-name $registryName 
```

Bu makaledeki adımları kullanarak özel bir uç nokta bağlantısı ayarladığınızda, kayıt defteri, kayıt defterinde RBAC izinleri olan istemcive hizmetlerden gelen bağlantıları otomatik olarak kabul eder. Bağlantıların el ile onayını gerektirecek şekilde bitiş noktasını ayarlayabilirsiniz. Özel uç nokta bağlantılarını onaylama ve reddetme hakkında bilgi [için](../private-link/manage-private-endpoint.md)bkz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Tüm Azure kaynaklarını aynı kaynak grubunda oluşturduysanız ve artık bunlara gerek yoksa, tek bir [az grubu silme](/cli/azure/group) komutunu kullanarak kaynakları isteğe bağlı olarak silebilirsiniz:

```azurecli
az group delete --name $resourceGroup
```

Portaldaki kaynaklarınızı temizlemek için kaynak grubunuza gidin. Kaynak grubu yüklendikten sonra, kaynak grubunu ve depolanan kaynakları kaldırmak için **Kaynak Grubunu Sil'i** tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

* Private Link hakkında daha fazla bilgi edinmek için [Azure Özel Bağlantı](../private-link/private-link-overview.md) belgelerine bakın.
* Özel bağlantıya alternatif olarak, kayıt defteri erişimini kısıtlamak için ağ erişim kuralları ayarlamak tır. Daha fazla bilgi edinmek için bkz. Azure [sanal ağı veya güvenlik duvarı kurallarını kullanarak Azure kapsayıcı kayıt defterine erişimi kısıtlayın.](container-registry-vnet.md)

<!-- LINKS - external -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms
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
[az-acr-private-endpoint-connection]: /cli/azure/acr/private-endpoint-connection
[az-acr-private-endpoint-connection-list]: /cli/azure/acr/private-endpoint-connection#az-acr-private-endpoint-connection-list
[az-acr-private-endpoint-connection-approve]: /cli/azure/acr/private-endpoint-connection#az-acr-private-endpoint-connection-approve
[az-group-create]: /cli/azure/group
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-vm-create]: /cli/azure/vm#az-vm-create
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-show
[az-network-vnet-subnet-update]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-update
[az-network-vnet-list]: /cli/azure/network/vnet/#az-network-vnet-list
[az-network-private-endpoint-create]: /cli/azure/network/private-endpoint#az-network-private-endpoint-create
[az-network-private-endpoint-show]: /cli/azure/network/private-endpoint#az-network-private-endpoint-show
[az-network-private-dns-zone-create]: /cli/azure/network/private-dns/zone#az-network-private-dns-zone-create
[az-network-private-dns-link-vnet-create]: /cli/azure/network/private-dns/link/vnet#az-network-private-dns-link-vnet-create
[az-network-private-dns-record-set-a-create]: /cli/azure/network/private-dns/record-set/a#az-network-private-dns-record-set-a-create
[az-network-private-dns-record-set-a-add-record]: /cli/azure/network/private-dns/record-set/a#az-network-private-dns-record-set-a-add-record
[az-resource-show]: /cli/azure/resource#az-resource-show
[quickstart-portal]: container-registry-get-started-portal.md
[quickstart-cli]: container-registry-get-started-azure-cli.md
[azure-portal]: https://portal.azure.com
