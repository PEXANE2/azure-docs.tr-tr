---
title: Özel bağlantıyı ayarla
description: Bir kapsayıcı kayıt defterinde özel bir uç nokta ayarlama ve yerel bir sanal ağdaki özel bir bağlantı üzerinden erişimi etkinleştirme
ms.topic: article
ms.date: 05/19/2020
ms.openlocfilehash: da51a35b66b793294f146c5a0a30b6a91d8aa01b
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83850054"
---
# <a name="configure-azure-private-link-for-an-azure-container-registry"></a>Azure Container Registry için Azure özel bağlantısını yapılandırma 

[Azure özel bağlantısı](../private-link/private-link-overview.md)kullanarak kayıt defteri uç noktalarına sanal ağ özel IP adresleri atayarak bir kayıt defterine erişimi sınırlayın. Sanal ağ ve kayıt defteri istemcileri arasındaki ağ trafiği, sanal ağın ve Microsoft omurga ağındaki özel bir bağlantının yanı sıra, genel İnternet 'ten etkilenme olasılığını ortadan kaldırır.

Ayarları, kayıt defterinin ayrılmış özel IP adresine çözümleyecek şekilde, Özel uç noktanız için [DNS ayarlarını yapılandırabilirsiniz](../private-link/private-endpoint-overview.md#dns-configuration) . DNS yapılandırması ile ağdaki istemciler ve hizmetler, kayıt defterine *myregistry.azurecr.io*gibi tam etki alanı adı ile erişmeye devam edebilir.

Bu özellik **Premium** kapsayıcı kayıt defteri hizmet katmanında kullanılabilir. Kayıt defteri hizmeti katmanları ve limitleri hakkında bilgi için bkz. [Azure Container Registry katmanları](container-registry-skus.md).

## <a name="things-to-know"></a>Bilinmesi gerekenler

* Şu anda, Azure Güvenlik Merkezi 'ni kullanan görüntü tarama, özel bir uç noktayla yapılandırılmış bir kayıt defterinde kullanılamaz.

## <a name="prerequisites"></a>Ön koşullar

* Bu makalede Azure CLı adımlarını kullanmak için, Azure CLı sürüm 2.6.0 veya sonraki bir sürümü önerilir. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli]. Veya [Azure Cloud Shell](../cloud-shell/quickstart.md)içinde çalıştırın.
* Zaten bir kapsayıcı kayıt defteriniz yoksa, bir tane oluşturun (Premium katman gereklidir) ve Docker Hub 'dan gibi örnek bir görüntüyü [içeri aktarın](container-registry-import-images.md) `hello-world` . Örneğin, [Azure Portal][quickstart-portal] veya [Azure CLI][quickstart-cli] kullanarak bir kayıt defteri oluşturun.
* Farklı bir Azure aboneliğinde özel bir bağlantı kullanarak kayıt defteri erişimini yapılandırmak için bu abonelikte Azure Container Registry kaynak sağlayıcısını kaydetmeniz gerekir. Örneğin:

  ```azurecli
  az account set --subscription <Name or ID of subscription of private link>

  az provider register --namespace Microsoft.ContainerRegistry
  ``` 

Bu makaledeki Azure CLı örnekleri aşağıdaki ortam değişkenlerini kullanır. Ortamınız için uygun değerleri değiştirin. Bash kabuğu için tüm örnekler biçimlendirilir:

```bash
REGISTRY_NAME=<container-registry-name>
REGISTRY_LOCATION=<container-registry-location> # Azure region such as westeurope where registry created
RESOURCE_GROUP=<resource-group-name>
VM_NAME=<virtual-machine-name>
```

[!INCLUDE [Set up Docker-enabled VM](../../includes/container-registry-docker-vm-setup.md)]

## <a name="set-up-private-link---cli"></a>Özel bağlantı ayarlama-CLı

### <a name="get-network-and-subnet-names"></a>Ağ ve alt ağ adlarını al

Zaten yoksa, bir özel bağlantı kurmak için bir sanal ağ ve alt ağ adları gerekir. Bu örnekte, VM ve kayıt defterinin özel uç noktası için aynı alt ağ kullanılır. Ancak birçok senaryoda, uç noktayı ayrı bir alt ağda ayarlarsınız. 

Bir VM oluşturduğunuzda, Azure varsayılan olarak aynı kaynak grubunda bir sanal ağ oluşturur. Sanal ağın adı, sanal makinenin adını temel alır. Örneğin, *Mydockervm*sanal makinenizi adlandırdıysanız, varsayılan sanal ağ adı Mydockervmvnet adlı bir alt ağ ile Mydockervmvnet *olur.* *myDockerVMVNET* Bu değerleri, [az Network VNET List][az-network-vnet-list] komutunu çalıştırarak ortam değişkenlerinde ayarlayın:

```azurecli
NETWORK_NAME=$(az network vnet list \
  --resource-group $RESOURCE_GROUP \
  --query '[].{Name: name}' --output tsv)

SUBNET_NAME=$(az network vnet list \
  --resource-group $RESOURCE_GROUP \
  --query '[].{Subnet: subnets[0].name}' --output tsv)

echo NETWORK_NAME=$NETWORK_NAME
echo SUBNET_NAME=$SUBNET_NAME
```

### <a name="disable-network-policies-in-subnet"></a>Alt ağda ağ ilkelerini devre dışı bırak

Özel uç nokta için alt ağdaki ağ güvenlik grupları gibi [ağ Ilkelerini devre dışı bırakın](../private-link/disable-private-endpoint-network-policy.md) . Alt ağ yapılandırmanızı [az Network VNET subnet Update][az-network-vnet-subnet-update]ile güncelleştirin:

```azurecli
az network vnet subnet update \
 --name $SUBNET_NAME \
 --vnet-name $NETWORK_NAME \
 --resource-group $RESOURCE_GROUP \
 --disable-private-endpoint-network-policies
```

### <a name="configure-the-private-dns-zone"></a>Özel DNS bölgesini yapılandırma

Özel Azure Container Registry etki alanı için özel bir DNS bölgesi oluşturun. Sonraki adımlarda, bu DNS bölgesindeki kayıt defteri etki alanınız için DNS kayıtları oluşturursunuz.

Azure Container Registry 'niz için varsayılan DNS çözümlemesini geçersiz kılmak üzere özel bir bölge kullanmak için, bölgenin **Privatelink.azurecr.io**olarak adlandırılması gerekir. Özel bölgeyi oluşturmak için aşağıdaki [az Network Private-DNS Zone Create][az-network-private-dns-zone-create] komutunu çalıştırın:

```azurecli
az network private-dns zone create \
  --resource-group $RESOURCE_GROUP \
  --name "privatelink.azurecr.io"
```

### <a name="create-an-association-link"></a>İlişki bağlantısı oluşturma

Özel bölgenizi sanal ağla ilişkilendirmek için [az Network Private-DNS link VNET Create][az-network-private-dns-link-vnet-create] ' i çalıştırın. Bu örnek *Mydnslink*adlı bir bağlantı oluşturur.

```azurecli
az network private-dns link vnet create \
  --resource-group $RESOURCE_GROUP \
  --zone-name "privatelink.azurecr.io" \
  --name MyDNSLink \
  --virtual-network $NETWORK_NAME \
  --registration-enabled false
```

### <a name="create-a-private-registry-endpoint"></a>Özel kayıt defteri uç noktası oluşturma

Bu bölümde, sanal ağda kayıt defterinin özel uç noktasını oluşturun. İlk olarak, kayıt defterinizin kaynak KIMLIĞINI alın:

```azurecli
REGISTRY_ID=$(az acr show --name $REGISTRY_NAME \
  --query 'id' --output tsv)
```

Kayıt defterinin özel uç noktasını oluşturmak için [az Network Private-ENDPOINT Create][az-network-private-endpoint-create] komutunu çalıştırın.

Aşağıdaki örnek, *Myprivateendpoint* ve hizmet bağlantısı *myConnection*uç noktasını oluşturur. Uç nokta için bir kapsayıcı kayıt defteri kaynağı belirtmek için şu geçişi yapın `--group-ids registry` :

```azurecli
az network private-endpoint create \
    --name myPrivateEndpoint \
    --resource-group $RESOURCE_GROUP \
    --vnet-name $NETWORK_NAME \
    --subnet $SUBNET_NAME \
    --private-connection-resource-id $REGISTRY_ID \
    --group-ids registry \
    --connection-name myConnection
```

### <a name="get-private-ip-addresses"></a>Özel IP adresleri Al

Ağ arabirimi KIMLIĞI için uç noktayı sorgulamak için [az Network Private-Endpoint Show][az-network-private-endpoint-show] ' i çalıştırın:

```azurecli
NETWORK_INTERFACE_ID=$(az network private-endpoint show \
  --name myPrivateEndpoint \
  --resource-group $RESOURCE_GROUP \
  --query 'networkInterfaces[0].id' \
  --output tsv)
```

Bu örnekteki ağ arabirimiyle ilişkili, kapsayıcı kayıt defteri için iki özel IP adresi ve kayıt defterinin kendisi için bir diğeri ise kayıt defterinin veri uç noktası için. Aşağıdaki [az Resource Show][az-resource-show] komutları, kapsayıcı kayıt defteri ve kayıt defterinin veri uç noktası IÇIN özel IP adreslerini alır:

```azurecli
PRIVATE_IP=$(az resource show \
  --ids $NETWORK_INTERFACE_ID \
  --api-version 2019-04-01 \
  --query 'properties.ipConfigurations[1].properties.privateIPAddress' \
  --output tsv)

DATA_ENDPOINT_PRIVATE_IP=$(az resource show \
  --ids $NETWORK_INTERFACE_ID \
  --api-version 2019-04-01 \
  --query 'properties.ipConfigurations[0].properties.privateIPAddress' \
  --output tsv)
```

> [!NOTE]
> Kayıt defteriniz coğrafi olarak [çoğaltılırsa](container-registry-geo-replication.md), her kayıt defteri çoğaltması için ek veri uç noktası için sorgu sorgulayın.

### <a name="create-dns-records-in-the-private-zone"></a>Özel bölgede DNS kayıtları oluşturma

Aşağıdaki komutlar, kayıt defteri uç noktası ve veri uç noktası için özel bölgede DNS kayıtları oluşturur. Örneğin, *westeurope* bölgesinde *myregistry* adlı bir kayıt defteriniz varsa, uç nokta adları ve ' dir `myregistry.azurecr.io` `myregistry.westeurope.data.azurecr.io` . 

> [!NOTE]
> Kayıt defteriniz coğrafi olarak [çoğaltılırsa](container-registry-geo-replication.md), her bir çoğaltmanın veri uç noktası IP 'si için ek ton DNS kayıtları oluşturun.

İlk çalıştırma [az Network Private-DNS Record-][az-network-private-dns-record-set-a-create] kayıt defteri uç noktası ve veri uç noktası Için boş bir kayıt kümesi oluşturmak üzere Create a Create a set:

```azurecli
az network private-dns record-set a create \
  --name $REGISTRY_NAME \
  --zone-name privatelink.azurecr.io \
  --resource-group $RESOURCE_GROUP

# Specify registry region in data endpoint name
az network private-dns record-set a create \
  --name ${REGISTRY_NAME}.${REGISTRY_LOCATION}.data \
  --zone-name privatelink.azurecr.io \
  --resource-group $RESOURCE_GROUP
```

Kayıt defteri uç noktası ve veri uç noktası için bir kayıt oluşturmak üzere [az Network Private-DNS Record-set a Add-Record][az-network-private-dns-record-set-a-add-record] komutunu çalıştırın:

```azurecli
az network private-dns record-set a add-record \
  --record-set-name $REGISTRY_NAME \
  --zone-name privatelink.azurecr.io \
  --resource-group $RESOURCE_GROUP \
  --ipv4-address $PRIVATE_IP

# Specify registry region in data endpoint name
az network private-dns record-set a add-record \
  --record-set-name ${REGISTRY_NAME}.${REGISTRY_LOCATION}.data \
  --zone-name privatelink.azurecr.io \
  --resource-group $RESOURCE_GROUP \
  --ipv4-address $DATA_ENDPOINT_PRIVATE_IP
```

Özel bağlantı artık yapılandırıldı ve kullanıma hazır.

## <a name="set-up-private-link---portal"></a>Özel bağlantı ayarlama-Portal

Bir kayıt defteri oluşturduğunuzda özel bir bağlantı kurun veya var olan bir kayıt defterine özel bir bağlantı ekleyin. Aşağıdaki adımlarda, test için bir sanal ağınız ve alt ağınız zaten bir VM ile ayarlanmış olduğunu varsayar. Ayrıca, [Yeni bir sanal ağ ve alt ağ oluşturabilirsiniz](../virtual-network/quick-create-portal.md).

### <a name="create-a-private-endpoint---new-registry"></a>Özel uç nokta oluşturma-yeni kayıt defteri

1. Portalda bir kayıt defteri oluştururken, **temel kavramlar** sekmesinde, **SKU**' da **Premium**' u seçin.
1. **Ağ** sekmesini seçin.
1. **Ağ bağlantısı**' nda **Özel uç nokta**  >  **+ Ekle**' yi seçin.
1. Aşağıdaki bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | Mevcut bir grubun adını girin veya yeni bir grup oluşturun.|
    | Name | Benzersiz bir ad girin. |
    | Alt kaynak |**Kayıt defteri** seçin|
    | **Ağ** | |
    | Sanal ağ| Sanal makinenizin dağıtıldığı sanal ağı ( *Mydockervmvnet*gibi) seçin. |
    | Alt ağ | Sanal makinenizin dağıtıldığı *Mydockervmsubnet* gibi bir alt ağ seçin. |
    |**Özel DNS tümleştirme**||
    |Özel DNS bölgesiyle tümleştirin |**Evet**' i seçin. |
    |Özel DNS bölgesi |Seç *(yeni) Privatelink.azurecr.io* |
    |||
1. Kalan kayıt defteri ayarlarını yapılandırın ve ardından **gözden geçir + oluştur**' u seçin.

  ![Özel uç nokta ile kayıt defteri oluşturma](./media/container-registry-private-link/private-link-create-portal.png)

### <a name="create-a-private-endpoint---existing-registry"></a>Özel uç nokta oluşturma-mevcut kayıt defteri

1. Portalda kapsayıcı Kayıt defterinize gidin.
1. **Ayarlar**altında **ağ**' ı seçin.
1. **Özel uç noktalar** sekmesinde **+ Özel uç nokta**' ı seçin.
1. **Temel bilgiler** sekmesinde, aşağıdaki bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **Proje ayrıntıları** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | Mevcut bir grubun adını girin veya yeni bir grup oluşturun.|
    | **Örnek ayrıntıları** |  |
    | Name | Bir ad girin. |
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

Özel uç nokta oluşturulduktan sonra, özel bölgedeki DNS ayarları portaldaki **Özel uç noktalar** sayfasında görünür:

1. Portalda kapsayıcı Kayıt defterinize gidin ve **ayarlar > ağ**' ı seçin.
1. **Özel uç noktalar** sekmesinde, oluşturduğunuz özel uç noktayı seçin.
1. **Genel bakış** sayfasında bağlantı ayarlarını ve özel DNS ayarlarını gözden geçirin.

  ![Uç nokta DNS ayarları](./media/container-registry-private-link/private-endpoint-overview.png)

Özel bağlantınız artık yapılandırıldı ve kullanıma hazır.

## <a name="disable-public-access"></a>Genel erişimi devre dışı bırak

Birçok senaryo için, Ortak ağlardan kayıt defteri erişimini devre dışı bırakın. Bu yapılandırma, sanal ağın dışındaki istemcilerin kayıt defteri uç noktalarına ulaşmasını engeller. 

### <a name="disable-public-access---cli"></a>Genel erişimi devre dışı bırak-CLı

Azure CLı kullanarak genel erişimi devre dışı bırakmak için [az ACR Update][az-acr-update] çalıştırın ve `--public-network-enabled` olarak ayarlayın `false` . 

> [!NOTE]
> `public-network-enabled`Bağımsız değişken Azure CLI 2.6.0 veya üstünü gerektirir. 

```azurecli
az acr update --name $REGISTRY_NAME --public-network-enabled false
```


### <a name="disable-public-access---portal"></a>Genel erişimi devre dışı bırak-Portal

1. Portalda kapsayıcı Kayıt defterinize gidin ve **ayarlar > ağ**' ı seçin.
1. **Genel erişim** sekmesinde, **genel ağ erişimine izin ver**' in altında **devre dışı**' yı seçin. Sonra **Kaydet**' i seçin.

## <a name="validate-private-link-connection"></a>Özel bağlantı bağlantısını doğrula

Özel uç noktanın alt ağı içindeki kaynakların, özel bir IP adresi üzerinden kayıt defterinize bağlanıp doğru özel DNS bölge tümleştirmesine sahip olduğunu doğrulamanız gerekir.

Özel bağlantı bağlantısını doğrulamak için sanal ağda ayarladığınız sanal makineye SSH.

`nslookup`Özel bağlantı üzerinden kayıt DEFTERINIZIN IP adresini çözümlemek için komutunu çalıştırın:

```bash
nslookup $REGISTRY_NAME.azurecr.io
```

Örnek çıkış, alt ağın adres alanındaki kayıt defterinin IP adresini gösterir:

```console
[...]
myregistry.azurecr.io       canonical name = myregistry.privatelink.azurecr.io.
Name:   myregistry.privatelink.azurecr.io
Address: 10.0.0.6
```

Bu sonucu `nslookup` , genel bir uç nokta üzerinden aynı kayıt defteri için çıkışdaki genel IP adresiyle karşılaştırın:

```console
[...]
Non-authoritative answer:
Name:   myregistry.westeurope.cloudapp.azure.com
Address: 40.78.103.41
```

### <a name="registry-operations-over-private-link"></a>Özel bağlantı üzerinden kayıt defteri işlemleri

Ayrıca, alt ağdaki sanal makineden kayıt defteri işlemleri gerçekleştirebildiğinizi doğrulayın. Sanal makinenize bir SSH bağlantısı oluşturun ve kayıt defterinizde oturum açmak için [az ACR LOGIN][az-acr-login] ' i çalıştırın. VM yapılandırmanıza bağlı olarak, ile aşağıdaki komutları ön eki uygulamanız gerekebilir `sudo` .

```bash
az acr login --name $REGISTRY_NAME
```

`docker pull`Kayıt defterinden örnek bir görüntü çekmek için gibi kayıt defteri işlemleri gerçekleştirin. Kayıt defteriniz `hello-world:v1` için uygun olan bir görüntüyle ve etiketle değiştirin ve kayıt defteri oturum açma sunucusu adı (tümü küçük harf) öneki:

```bash
docker pull myregistry.azurecr.io/hello-world:v1
``` 

Docker görüntüyü sanal makineye başarıyla çeker.

## <a name="manage-private-endpoint-connections"></a>Özel uç nokta bağlantılarını yönetme

Azure portal kullanarak veya [az ACR Private-Endpoint-Connection][az-acr-private-endpoint-connection] komut grubundaki komutları kullanarak bir kayıt defterinin özel uç nokta bağlantılarını yönetin. İşlemler, bir kayıt defterinin özel uç nokta bağlantılarının onaylama, silme, listeleme, reddetme veya ayrıntılarını içerir.

Örneğin, bir kayıt defterinin özel uç nokta bağlantılarını listelemek için [az ACR Private-Endpoint-Connection List][az-acr-private-endpoint-connection-list] komutunu çalıştırın. Örneğin:

```azurecli
az acr private-endpoint-connection list \
  --registry-name $REGISTRY_NAME 
```

Bu makaledeki adımları kullanarak özel bir uç nokta bağlantısı ayarladığınızda, kayıt defteri, kayıt defterinde RBAC izinleri olan istemcilerden ve hizmetlerden gelen bağlantıları otomatik olarak kabul eder. Uç noktayı bağlantıların el ile onayını gerektirecek şekilde ayarlayabilirsiniz. Özel uç nokta bağlantılarını onaylama ve reddetme hakkında daha fazla bilgi için bkz. [Özel uç nokta bağlantısını yönetme](../private-link/manage-private-endpoint.md).

## <a name="add-zone-records-for-replicas"></a>Çoğaltmalar için bölge kayıtları ekleme

Bu makalede gösterildiği gibi, bir kayıt defterine özel bir uç nokta bağlantısı eklediğinizde, bölgedeki DNS kayıtları kayıt defteri `privatelink.azurecr.io` ve kayıt defteri 'nin [çoğaltılacağı](container-registry-geo-replication.md)bölgelerdeki veri uç noktaları için oluşturulur. 

Daha sonra yeni bir çoğaltma eklerseniz, bu bölgedeki veri uç noktası için el ile yeni bir bölge kaydı eklemeniz gerekir. Örneğin, *northeurope* konumunda *myregistry* 'nin bir çoğaltmasını oluşturursanız, için bir bölge kaydı ekleyin `myregistry.northeurope.data.azurecr.io` . Adımlar için, bu makaledeki [özel bölgede DNS kayıtları oluşturma](#create-dns-records-in-the-private-zone) bölümüne bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Tüm Azure kaynaklarını aynı kaynak grubunda oluşturduysanız ve artık gerekmiyorsa, tek bir [az Group Delete](/cli/azure/group) komutu kullanarak kaynakları isteğe bağlı olarak silebilirsiniz:

```azurecli
az group delete --name $RESOURCE_GROUP
```

Portalda kaynaklarınızı temizlemek için kaynak grubunuza gidin. Kaynak grubu yüklendikten sonra kaynak grubunu **Sil** ' e tıklayarak kaynak grubunu ve burada depolanan kaynakları kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

* Özel bağlantı hakkında daha fazla bilgi edinmek için bkz. [Azure özel bağlantı](../private-link/private-link-overview.md) belgeleri.
* Bir istemci güvenlik duvarının ardında kayıt defteri erişim kuralları ayarlamanız gerekiyorsa, bkz. [güvenlik duvarı arkasındaki bir Azure Container Registry 'ye erişmek için kuralları yapılandırma](container-registry-firewall-access-rules.md).

<!-- LINKS - external -->
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
[az-acr-update]: /cli/azure/acr#az-acr-update
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
