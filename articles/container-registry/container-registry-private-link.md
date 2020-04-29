---
title: Özel bağlantıyı ayarla
description: Bir kapsayıcı kayıt defterinde özel bir uç nokta ayarlama ve yerel bir sanal ağda özel bağlantıyı etkinleştirme
ms.topic: article
ms.date: 03/10/2020
ms.openlocfilehash: de8228d84497e71f24dba3dd4e6162cb6735a8c1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79498921"
---
# <a name="configure-azure-private-link-for-an-azure-container-registry"></a>Azure Container Registry için Azure özel bağlantısını yapılandırma 

Azure sanal ağ üzerindeki istemcilerin kayıt defterine [özel bir bağlantı](../private-link/private-link-overview.md)üzerinden güvenli bir şekilde erişmesini sağlamak Için, Azure Container Registry 'niz için [özel bir uç nokta](../private-link/private-endpoint-overview.md) ayarlayabilirsiniz. Özel uç nokta, kayıt defteriniz için sanal ağ adres alanından bir IP adresi kullanır. Sanal ağ ve kayıt defteri istemcileri arasındaki ağ trafiği, sanal ağın ve Microsoft omurga ağındaki özel bir bağlantının yanı sıra, genel İnternet 'ten etkilenme olasılığını ortadan kaldırır.

Ayarları, kayıt defterinin ayrılmış özel IP adresine çözümleyecek şekilde, Özel uç noktanız için [DNS ayarlarını yapılandırabilirsiniz](../private-link/private-endpoint-overview.md#dns-configuration) . DNS yapılandırması ile ağdaki istemciler ve hizmetler, kayıt defterine *myregistry.azurecr.io*gibi tam etki alanı adı ile erişmeye devam edebilir.

Bu özellik **Premium** kapsayıcı kayıt defteri hizmet katmanında kullanılabilir. Kayıt defteri hizmeti katmanları ve limitleri hakkında bilgi için bkz. [Azure Container Registry SKU 'lar](container-registry-skus.md).

> [!IMPORTANT]
> Bu özellik şu anda önizleme aşamasındadır ve bazı [sınırlamalar](#preview-limitations) geçerlidir. Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="preview-limitations"></a>Önizleme sınırlamaları

* Şu anda, [coğrafi olarak çoğaltılan bir kayıt defterinde](container-registry-geo-replication.md)özel bir uç nokta ile özel bir bağlantı ayarlayamazsınız. 

## <a name="prerequisites"></a>Ön koşullar

* Bu makalede Azure CLı adımlarını kullanmak için, Azure CLı sürüm 2.2.0 veya sonraki bir sürümü önerilir. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli]. Veya [Azure Cloud Shell](../cloud-shell/quickstart.md)içinde çalıştırın.
* Zaten bir kapsayıcı kayıt defteriniz yoksa, bir tane oluşturun (Premium katman gereklidir) ve Docker Hub 'dan gibi `hello-world` örnek bir görüntü gönderin. Örneğin, [Azure Portal][quickstart-portal] veya [Azure CLI][quickstart-cli] kullanarak bir kayıt defteri oluşturun.
* Farklı bir Azure aboneliğinde özel bir bağlantı kullanarak kayıt defteri erişimini yapılandırmak istiyorsanız bu abonelikte Azure Container Registry kaynak sağlayıcısını kaydetmeniz gerekir. Örneğin:

  ```azurecli
  az account set --subscription <Name or ID of subscription of private link>

  az provider register --namespace Microsoft.ContainerRegistry
  ``` 

Bu makaledeki Azure CLı örnekleri aşağıdaki ortam değişkenlerini kullanır. Ortamınız için uygun değerleri değiştirin. Bash kabuğu için tüm örnekler biçimlendirilir:

```bash
registryName=<container-registry-name>
registryLocation=<container-registry-location> # Azure region such as westeurope where registry created
resourceGroup=<resource-group-name>
vmName=<virtual-machine-name>
```

## <a name="create-a-docker-enabled-virtual-machine"></a>Docker özellikli bir sanal makine oluşturma

Test amacıyla, bir Azure Container Registry 'ye erişmek için Docker özellikli bir Ubuntu VM kullanın. Kayıt defterine Azure Active Directory kimlik doğrulaması kullanmak için [Azure CLI][azure-cli] 'yı sanal makineye de yüklersiniz. Zaten bir Azure sanal makineniz varsa, bu oluşturma adımını atlayın.

Sanal makineniz ve kapsayıcı kayıt defteriniz için aynı kaynak grubunu kullanabilirsiniz. Bu kurulum, sonda temizleme işlemini basitleştirir, ancak gerekli değildir. Sanal makine ve sanal ağ için ayrı bir kaynak grubu oluşturmayı seçerseniz, [az Group Create][az-group-create]komutunu çalıştırın:

```azurecli
az group create --name $resourceGroup --location $registryLocation
```

Şimdi [az VM Create][az-vm-create]ile varsayılan bir Ubuntu Azure sanal makinesini dağıtın. Aşağıdaki örnek, *Mydockervm*ADLı bir VM oluşturur.

```azurecli
az vm create \
  --resource-group $resourceGroup \
  --name $vmName \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

VM’nin oluşturulması birkaç dakika sürer. Komut tamamlandığında, Azure CLı tarafından `publicIpAddress` görüntülendiğine göz atın. VM 'ye SSH bağlantısı oluşturmak için bu adresi kullanın.

### <a name="install-docker-on-the-vm"></a>SANAL makineye Docker 'ı yükler

VM çalıştırıldıktan sonra VM ile bir SSH bağlantısı oluşturun. *Publicıpaddress* değerini sanal MAKINENIZIN genel IP adresiyle değiştirin.

```bash
ssh azureuser@publicIpAddress
```

Ubuntu VM 'ye Docker yüklemek için aşağıdaki komutları çalıştırın:

```bash
sudo apt-get update
sudo apt install docker.io -y
```

Yükleme sonrasında, Docker 'ın sanal makinede düzgün çalıştığını doğrulamak için aşağıdaki komutu çalıştırın:

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

Azure CLI 'yi, Ubuntu sanal makinenize yüklemek için [apt Ile Azure CLI 'Yı kurma](/cli/azure/install-azure-cli-apt?view=azure-cli-latest) bölümündeki adımları izleyin. Örneğin:

```bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

SSH bağlantısından çıkın.

## <a name="set-up-private-link---cli"></a>Özel bağlantı ayarlama-CLı

### <a name="get-network-and-subnet-names"></a>Ağ ve alt ağ adlarını al

Zaten yoksa, bir özel bağlantı kurmak için bir sanal ağ ve alt ağ adları gerekir. Bu örnekte, VM ve kayıt defterinin özel uç noktası için aynı alt ağ kullanılır. Ancak birçok senaryoda, uç noktayı ayrı bir alt ağda ayarlarsınız. 

Bir VM oluşturduğunuzda, Azure varsayılan olarak aynı kaynak grubunda bir sanal ağ oluşturur. Sanal ağın adı, sanal makinenin adını temel alır. Örneğin, *Mydockervm*sanal makinenizi adlandırdıysanız, varsayılan sanal ağ adı Mydockervmvnet adlı bir alt ağ ile Mydockervmvnet *olur.* *myDockerVMVNET* Bu değerleri, [az Network VNET List][az-network-vnet-list] komutunu çalıştırarak ortam değişkenlerinde ayarlayın:

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

### <a name="disable-network-policies-in-subnet"></a>Alt ağda ağ ilkelerini devre dışı bırak

Özel uç nokta için alt ağdaki ağ güvenlik grupları gibi [ağ Ilkelerini devre dışı bırakın](../private-link/disable-private-endpoint-network-policy.md) . Alt ağ yapılandırmanızı [az Network VNET subnet Update][az-network-vnet-subnet-update]ile güncelleştirin:

```azurecli
az network vnet subnet update \
 --name $subnetName \
 --vnet-name $networkName \
 --resource-group $resourceGroup \
 --disable-private-endpoint-network-policies
```

### <a name="configure-the-private-dns-zone"></a>Özel DNS bölgesini yapılandırma

Azure Container Registry etki alanı için özel bir DNS bölgesi oluşturun. Sonraki adımlarda, bu DNS bölgesi içindeki kayıt defteri etki alanınız için DNS kayıtları oluşturursunuz.

Azure Container Registry 'niz için varsayılan DNS çözümlemesini geçersiz kılmak üzere özel bir bölge kullanmak için, bölgenin **Privatelink.azurecr.io**olarak adlandırılması gerekir. Özel bölgeyi oluşturmak için aşağıdaki [az Network Private-DNS Zone Create][az-network-private-dns-zone-create] komutunu çalıştırın:

```azurecli
az network private-dns zone create \
  --resource-group $resourceGroup \
  --name "privatelink.azurecr.io"
```

### <a name="create-an-association-link"></a>İlişki bağlantısı oluşturma

Özel bölgenizi sanal ağla ilişkilendirmek için [az Network Private-DNS link VNET Create][az-network-private-dns-link-vnet-create] ' i çalıştırın. Bu örnek *Mydnslink*adlı bir bağlantı oluşturur.

```azurecli
az network private-dns link vnet create \
  --resource-group $resourceGroup \
  --zone-name "privatelink.azurecr.io" \
  --name MyDNSLink \
  --virtual-network $networkName \
  --registration-enabled false
```

### <a name="create-a-private-registry-endpoint"></a>Özel kayıt defteri uç noktası oluşturma

Bu bölümde, sanal ağda kayıt defterinin özel uç noktasını oluşturun. İlk olarak, kayıt defterinizin kaynak KIMLIĞINI alın:

```azurecli
registryID=$(az acr show --name $registryName \
  --query 'id' --output tsv)
```

Kayıt defterinin özel uç noktasını oluşturmak için [az Network Private-ENDPOINT Create][az-network-private-endpoint-create] komutunu çalıştırın.

Aşağıdaki örnek, *Myprivateendpoint* ve hizmet bağlantısı *myConnection*uç noktasını oluşturur. Uç nokta için bir kapsayıcı kayıt defteri kaynağı belirtmek için şu `--group-ids registry`geçişi yapın:

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

### <a name="get-private-ip-addresses"></a>Özel IP adresleri Al

Ağ arabirimi KIMLIĞI için uç noktayı sorgulamak için [az Network Private-Endpoint Show][az-network-private-endpoint-show] ' i çalıştırın:

```azurecli
networkInterfaceID=$(az network private-endpoint show \
  --name myPrivateEndpoint \
  --resource-group $resourceGroup \
  --query 'networkInterfaces[0].id' \
  --output tsv)
```

Ağ arabirimiyle ilişkili, kapsayıcı kayıt defteri için iki özel IP adresi olan bir kayıt defterinin kendisi için ve bir diğeri de kayıt defterinin veri uç noktası için. Kapsayıcı kayıt defteri ve kayıt defteri 'nin veri uç noktası için özel IP adreslerini almak üzere aşağıdaki [az Resource Show][az-resource-show] komutlarını çalıştırın:

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

Aşağıdaki komutlar, kayıt defteri uç noktası ve veri uç noktası için özel bölgede DNS kayıtları oluşturur. Örneğin, westeurope bölgesinde *myregistry* adlı bir kayıt defteriniz varsa, uç *westeurope* nokta adları ve `myregistry.azurecr.io` `myregistry.westeurope.data.azurecr.io`' dir. 

İlk çalıştırma [az Network Private-DNS Record-][az-network-private-dns-record-set-a-create] kayıt defteri uç noktası ve veri uç noktası Için boş bir kayıt kümesi oluşturmak üzere Create a Create a set:

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

Kayıt defteri uç noktası ve veri uç noktası için bir kayıt oluşturmak üzere [az Network Private-DNS Record-set a Add-Record][az-network-private-dns-record-set-a-add-record] komutunu çalıştırın:

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

Özel bağlantı artık yapılandırıldı ve kullanıma hazır.

## <a name="set-up-private-link---portal"></a>Özel bağlantı ayarlama-Portal

Aşağıdaki adımlarda, test için bir sanal ağınız ve alt ağınız zaten bir VM ile ayarlanmış olduğunu varsayar. Ayrıca, [Yeni bir sanal ağ ve alt ağ oluşturabilirsiniz](../virtual-network/quick-create-portal.md).

### <a name="create-a-private-endpoint"></a>Özel uç nokta oluşturma

1. Portalda kapsayıcı Kayıt defterinize gidin.
1. **Ayarlar**altında **Özel uç nokta bağlantıları ' nı (Önizleme)** seçin.
1. **+ Özel uç nokta**' ı seçin.
1. **Temel bilgiler** sekmesinde, aşağıdaki bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **Proje ayrıntıları** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | Mevcut bir grubun adını girin veya yeni bir grup oluşturun.|
    | **Örnek ayrıntıları** |  |
    | Adı | Benzersiz bir ad girin. |
    |Bölge|Bir bölge seçin.|
    |||
5. **Sonraki: kaynak**' ı seçin.
6. Aşağıdaki bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    |Bağlantı yöntemi  | **Dizinimde bir Azure kaynağına bağlan '** ı seçin.|
    | Abonelik| Aboneliğinizi seçin. |
    | Kaynak türü | **Microsoft. ContainerRegistry/kayıt defterleri**' ni seçin. |
    | Kaynak |Kayıt defterinizin adını seçin|
    |Hedef alt kaynak |**Kayıt defteri** seçin|
    |||
7. Ileri 'yi seçin **: yapılandırma**.
8. Bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    |**Ağ**| |
    | Sanal ağ| Sanal makinenizin dağıtıldığı sanal ağı ( *Mydockervmvnet*gibi) seçin. |
    | Alt ağ | Sanal makinenizin dağıtıldığı *Mydockervmsubnet* gibi bir alt ağ seçin. |
    |**Özel DNS tümleştirme**||
    |Özel DNS bölgesiyle tümleştirin |**Evet**' i seçin. |
    |Özel DNS bölgesi |Seç *(yeni) Privatelink.azurecr.io* |
    |||

1. **İncele ve oluştur**’u seçin. Azure 'un yapılandırmanızı doğruladığı, **gözden geçir + oluştur** sayfasına götürülürsünüz. 
2. **Doğrulama başarılı** Iletisini gördüğünüzde **Oluştur**' u seçin.

Özel uç nokta oluşturulduktan sonra, özel bölgedeki DNS ayarları uç noktanın **genel bakış** sayfasında görünür.

![Uç nokta DNS ayarları](./media/container-registry-private-link/private-endpoint-overview.png)

Özel bağlantınız artık yapılandırıldı ve kullanıma hazır.

## <a name="validate-private-link-connection"></a>Özel bağlantı bağlantısını doğrula

Özel uç noktanın alt ağı içindeki kaynakların, özel bir IP adresi üzerinden kayıt defterinize bağlanıp doğru özel DNS bölge tümleştirmesine sahip olduğunu doğrulamanız gerekir.

Özel bağlantı bağlantısını doğrulamak için sanal ağda ayarladığınız sanal makineye SSH.

Özel bağlantı `nslookup` üzerinden kayıt defterinizin IP adresini çözümlemek için komutunu çalıştırın:

```bash
nslookup $registryName.azurecr.io
```

Örnek çıkış, alt ağın adres alanındaki kayıt defterinin IP adresini gösterir:

```console
[...]
myregistry.azurecr.io       canonical name = myregistry.privatelink.azurecr.io.
Name:   myregistry.privatelink.azurecr.io
Address: 10.0.0.6
```

Bu sonucu, genel bir uç nokta üzerinden aynı `nslookup` kayıt defteri için ÇıKıŞDAKI genel IP adresiyle karşılaştırın:

```console
[...]
Non-authoritative answer:
Name:   myregistry.westeurope.cloudapp.azure.com
Address: 40.78.103.41
```

### <a name="registry-operations-over-private-link"></a>Özel bağlantı üzerinden kayıt defteri işlemleri

Ayrıca, alt ağdaki sanal makineden kayıt defteri işlemleri gerçekleştirebildiğinizi doğrulayın. Sanal makinenize bir SSH bağlantısı oluşturun ve kayıt defterinizde oturum açmak için [az ACR LOGIN][az-acr-login] ' i çalıştırın. VM yapılandırmanıza bağlı olarak, ile `sudo`aşağıdaki komutları ön eki uygulamanız gerekebilir.

```bash
az acr login --name $registryName
```

Kayıt defterinden örnek bir görüntü `docker pull` çekmek için gibi kayıt defteri işlemleri gerçekleştirin. Kayıt `hello-world:v1` defteriniz için uygun olan bir görüntüyle ve etiketle değiştirin ve kayıt defteri oturum açma sunucusu adı (tümü küçük harf) öneki:

```bash
docker pull myregistry.azurecr.io/hello-world:v1
``` 

Docker görüntüyü sanal makineye başarıyla çeker.

## <a name="manage-private-endpoint-connections"></a>Özel uç nokta bağlantılarını yönetme

Azure portal kullanarak veya [az ACR Private-Endpoint-Connection][az-acr-private-endpoint-connection] komut grubundaki komutları kullanarak bir kayıt defterinin özel uç nokta bağlantılarını yönetin. İşlemler, bir kayıt defterinin özel uç nokta bağlantılarının onaylama, silme, listeleme, reddetme veya ayrıntılarını içerir.

Örneğin, bir kayıt defterinin özel uç nokta bağlantılarını listelemek için [az ACR Private-Endpoint-Connection List][az-acr-private-endpoint-connection-list] komutunu çalıştırın. Örneğin:

```azurecli
az acr private-endpoint-connection list \
  --registry-name $registryName 
```

Bu makaledeki adımları kullanarak özel bir uç nokta bağlantısı ayarladığınızda, kayıt defteri, kayıt defterinde RBAC izinleri olan istemcilerden ve hizmetlerden gelen bağlantıları otomatik olarak kabul eder. Uç noktayı bağlantıların el ile onayını gerektirecek şekilde ayarlayabilirsiniz. Özel uç nokta bağlantılarını onaylama ve reddetme hakkında daha fazla bilgi için bkz. [Özel uç nokta bağlantısını yönetme](../private-link/manage-private-endpoint.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Tüm Azure kaynaklarını aynı kaynak grubunda oluşturduysanız ve artık gerekmiyorsa, tek bir [az Group Delete](/cli/azure/group) komutu kullanarak kaynakları isteğe bağlı olarak silebilirsiniz:

```azurecli
az group delete --name $resourceGroup
```

Portalda kaynaklarınızı temizlemek için kaynak grubunuza gidin. Kaynak grubu yüklendikten sonra kaynak grubunu **Sil** ' e tıklayarak kaynak grubunu ve burada depolanan kaynakları kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

* Özel bağlantı hakkında daha fazla bilgi edinmek için bkz. [Azure özel bağlantı](../private-link/private-link-overview.md) belgeleri.
* Özel bağlantıya alternatif olarak, kayıt defteri erişimini kısıtlamak için ağ erişim kuralları ayarlanamıyor. Daha fazla bilgi edinmek için bkz. [Azure sanal ağını veya güvenlik duvarı kurallarını kullanarak bir Azure Container Registry 'ye erişimi kısıtlama](container-registry-vnet.md).

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
