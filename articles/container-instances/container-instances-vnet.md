---
title: Kapsayıcı grubunu Azure sanal ağına dağıtma
description: Kapsayıcı gruplarını yeni veya varolan bir Azure sanal ağına nasıl dağıttığın öğren.
ms.topic: article
ms.date: 01/06/2020
ms.author: danlep
ms.openlocfilehash: 318576e9b5c5b32bbc993ea16494c938b74bd2f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77200070"
---
# <a name="deploy-container-instances-into-an-azure-virtual-network"></a>Kapsayıcı örneklerini Azure sanal ağına dağıtma

[Azure Sanal Ağı,](../virtual-network/virtual-networks-overview.md) Azure ve şirket içi kaynaklarınız için güvenli ve özel ağ sağlar. Kapsayıcı gruplarını bir Azure sanal ağına dağıtarak, kapsayıcılarınız sanal ağdaki diğer kaynaklarla güvenli bir şekilde iletişim kurabilir.

Azure sanal ağına dağıtılan kapsayıcı grupları şu senaryoları etkinleştirin:

* Aynı alt ağdaki konteyner grupları arasında doğrudan iletişim
* Kapsayıcı örneklerinden sanal ağdaki bir veritabanına [görev tabanlı](container-instances-restart-policy.md) iş yükü çıktısı gönderme
* Kapsayıcı örnekleri için içeriği sanal ağdaki bir [hizmet bitiş noktasından](../virtual-network/virtual-network-service-endpoints-overview.md) alma
* Sanal ağdaki sanal makinelerle konteyner iletişimi
* [VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md) veya ExpressRoute aracılığıyla şirket içi kaynaklarla konteyner [iletişimi](../expressroute/expressroute-introduction.md)

> [!IMPORTANT]
> Sanal ağa konteyner grubu dağıtımları genellikle yalnızca aşağıdaki bölgelerdeki üretim iş yükleri için kullanılabilir: **Doğu ABD, Güney Orta ABD ve Batı ABD 2.** Özelliğin kullanılabildiği diğer bölgelerde, sanal ağ dağıtımları şu anda önizlemede dir ve genel kullanılabilirlik yakın gelecekte planlanır. Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. 


## <a name="virtual-network-deployment-limitations"></a>Sanal ağ dağıtım sınırlamaları

Kapsayıcı gruplarını sanal ağa dağıttığınızda belirli sınırlamalar uygulanır.

* Kapsayıcı gruplarını bir alt ağa dağıtmak için alt ağ başka kaynak türleri içeremez. Kapsayıcı gruplarını dağıtmadan önce varolan tüm kaynakları varolan bir alt ağdan kaldırın veya yeni bir alt ağ oluşturun.
* Sanal ağa dağıtılan bir kapsayıcı grubunda yönetilen bir [kimlik](container-instances-managed-identity.md) kullanamazsınız.
* Sanal ağa dağıtılan bir kapsayıcı grubunda [bir canlılık sondasını](container-instances-liveness-probe.md) veya [hazır bilgi sondasını](container-instances-readiness-probe.md) etkinleştiremezsiniz.
* İlgili ek ağ kaynakları nedeniyle, bir kapsayıcı grubunu sanal ağa dağıtmak genellikle standart bir kapsayıcı örneğini dağıtmaktan daha yavaştır.

[!INCLUDE [container-instances-vnet-limits](../../includes/container-instances-vnet-limits.md)]

Kapsayıcı kaynak sınırları, bu bölgelerdeki ağ olmayan kapsayıcı örnekleri için sınırlardan farklı olabilir. Şu anda bu özellik için yalnızca Linux kapsayıcıları desteklenir. Windows desteği planlandı.

### <a name="unsupported-networking-scenarios"></a>Desteklenmeyen ağ senaryoları 

* **Azure Yük Dengeleyicisi** - Ağ üzerinde bulunan bir kapsayıcı grubuna bir Azure Yük Dengeleyicisinin konteyner örneklerinin önüne yerleştirilmesi desteklenmez
* **Sanal ağ eşleme**
  * ACI VNet'in bakmakta olduğu ağ ortak bir IP alanı kullanıyorsa VNet eşleme, ACI için çalışmaz. VNet'in çalışması için eşlenen ağın bir RFC 1918 özel IP alanına ihtiyacı vardır. 
  * VNet'inizi yalnızca bir diğer VNet'e görebilirsiniz
* **Sanal ağ trafiği yönlendirmesi** - Özel yollar genel IP'lerin etrafında kurulamaz. Rotalar, ACI kaynaklarının dağıtıldığı temsilci alt ağının özel IP alanı içinde ayarlanabilir 
* **Ağ güvenlik grupları** - Azure Kapsayıcı Örneklerine devredilen bir alt ağa uygulanan NSG'lerde giden güvenlik kuralları şu anda uygulanmıyor 
* **Genel IP veya DNS etiketi** - Sanal ağa dağıtılan kapsayıcı grupları şu anda kapsayıcıları genel bir IP adresi veya tam nitelikli bir etki alanı adı ile doğrudan internete maruz bırakmayı desteklemiyor
* **Dahili ad çözümü** - Dahili Azure DNS aracılığıyla sanal ağdaki Azure kaynakları için ad çözümü desteklenmez

**Kapsayıcı gruplarını** sanal ağa dağıttıktan sonra ağ kaynağı silme işlemi [için ek adımlar](#delete-network-resources) gerekir.

## <a name="required-network-resources"></a>Gerekli ağ kaynakları

Kapsayıcı grupları sanal ağa dağıtmak için gereken üç Azure Sanal Ağ kaynağı vardır: [sanal ağın](#virtual-network) kendisi, sanal ağ içindeki [bir alt ağ](#subnet-delegated) ve ağ [profili.](#network-profile) 

### <a name="virtual-network"></a>Sanal ağ

Sanal ağ, bir veya daha fazla alt ağ oluşturduğunuz adres alanını tanımlar. Daha sonra Azure kaynaklarını (kapsayıcı grupları gibi) sanal ağınızdaki alt ağlara dağıtırsınız.

### <a name="subnet-delegated"></a>Alt ağ (delege)

Alt ağlar, sanal ağı, içine yerleştirdiğiniz Azure kaynakları tarafından kullanılabilir ayrı adres alanlarına böler. Sanal ağ içinde bir veya birkaç alt ağ oluşturursunuz.

Kapsayıcı grupları için kullandığınız alt ağ yalnızca kapsayıcı grupları içerebilir. Bir kapsayıcı grubunu bir alt ağa ilk dağıttığınızda, Azure, Azure Kapsayıcı Örnekleri'ne alt ağ oluşturan temsilci verir. Bir kez devredildikten sonra, alt ağ yalnızca kapsayıcı grupları için kullanılabilir. Kapsayıcı grupları dışındaki kaynakları devralınan bir alt ağa dağıtmaya çalışırsanız, işlem başarısız olur.

### <a name="network-profile"></a>Ağ profili

Ağ profili, Azure kaynakları için bir ağ yapılandırma şablonudur. Kaynak için belirli ağ özelliklerini belirtir, örneğin, dağıtılması gereken alt ağ. Bir kapsayıcı grubunu bir alt ağa (ve dolayısıyla sanal ağa) dağıtmak için [az kapsayıcı oluşturma][az-container-create] komutunu ilk kullandığınızda, Azure sizin için bir ağ profili oluşturur. Daha sonra bu ağ profilini alt ağa gelecekteki dağıtımlar için kullanabilirsiniz. 

Bir alt ağa kapsayıcı grubu dağıtmak için Kaynak Yöneticisi şablonu, YAML dosyası veya programlı bir yöntem kullanmak için, bir ağ profilinin tam Kaynak Yöneticisi kaynak kimliğini sağlamanız gerekir. [Az kapsayıcı oluşturarak][az-container-create]daha önce oluşturulmuş bir profil kullanabilir veya Kaynak Yöneticisi şablonu kullanarak profil oluşturabilirsiniz [(bkz. şablon örneği](https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-vnet) ve [başvuru).](https://docs.microsoft.com/azure/templates/microsoft.network/networkprofiles) Daha önce oluşturulmuş bir profilin kimliğini almak için [az ağ profil listesi][az-network-profile-list] komutunu kullanın. 

Aşağıdaki diyagramda, Azure Kapsayıcı Örnekleri'ne devredilen bir alt ağa birkaç kapsayıcı grubu dağıtılmıştır. Bir alt ağa bir kapsayıcı grubu dağıttıktan sonra, aynı ağ profilini belirterek bu gruba ek kapsayıcı grupları dağıtabilirsiniz.

![Sanal ağ içindeki kapsayıcı grupları][aci-vnet-01]

## <a name="deployment-scenarios"></a>Dağıtım senaryoları

Kapsayıcı gruplarını yeni bir sanal ağa dağıtmak ve Azure'un sizin için gerekli ağ kaynaklarını oluşturmasına veya varolan bir sanal ağa dağıtım almasına izin vermek için [az kapsayıcı oluşturmayı][az-container-create] kullanabilirsiniz. 

### <a name="new-virtual-network"></a>Yeni sanal ağ

Yeni bir sanal ağa dağıtmak ve Azure'un sizin için ağ kaynaklarını otomatik olarak oluşturmasını sağlamak için, [az kapsayıcı oluşturma][az-container-create]işlemini yürütürken aşağıdakileri belirtin:

* Sanal ağın adı
* CIDR formatında sanal ağ adresi öneki
* Alt ağ adı
* CIDR formatında alt net adresi öneki

Sanal ağ ve alt ağ adresi önekleri, sırasıyla sanal ağ ve alt ağ için adres alanlarını belirtir. Bu değerler, örneğin `10.0.0.0/16`Sınıfsız Etki Alanları Arası Yönlendirme (CIDR) gösteriminde temsil edilir. Alt ağlarla çalışma hakkında daha fazla bilgi için [bkz.](../virtual-network/virtual-network-manage-subnet.md)

İlk kapsayıcı grubunuzu bu yöntemle dağıttıktan sonra, sanal ağ ve alt ağ adlarını veya Azure'un sizin için otomatik olarak oluşturduğu ağ profilini belirterek aynı alt ağa dağıtabilirsiniz. Azure alt ağı Azure Kapsayıcı Örnekleri'ne devrettiği için, *yalnızca* kapsayıcı gruplarını alt ağa dağıtabilirsiniz.

### <a name="existing-virtual-network"></a>Mevcut sanal ağ

Bir kapsayıcı grubunu varolan bir sanal ağa dağıtmak için:

1. Varolan sanal ağınızda bir alt ağ oluşturun, kapsayıcı grubunun zaten dağıtıldığı varolan bir alt ağ kullanın veya diğer *tüm* kaynaklardan boşaltılan varolan bir alt ağ kullanın
1. [Az kapsayıcı oluşturmak][az-container-create] ve aşağıdakilerden birini belirterek bir kapsayıcı grubu dağıtmak:
   * Sanal ağ adı ve alt net adı
   * Farklı bir kaynak grubundan sanal ağ kullanmanızı sağlayan sanal ağ kaynağı kimliği ve alt ağ kaynak kimliği
   * [Az ağ profil listesini][az-network-profile-list] kullanarak elde edebilirsiniz ağ profil adı veya kimliği,

İlk kapsayıcı grubunuzu varolan bir alt ağa dağıttıktan sonra, Azure, Azure Kapsayıcı Örnekleri'ne alt ağ oluşturan temsilci verir. Artık bu alt ağa kapsayıcı grupları dışındaki kaynakları dağıtamazsınız.

## <a name="deployment-examples"></a>Dağıtım örnekleri

Aşağıdaki bölümlerde, Azure CLI ile kapsayıcı gruplarının sanal ağa nasıl dağıtılanılabildiğini açıklanmaktadır. Komut örnekleri **Bash** kabuğu için biçimlendirilir. PowerShell veya Command Prompt gibi başka bir kabuk tercih ederseniz, satır devam karakterlerini buna göre ayarlayın.

### <a name="deploy-to-a-new-virtual-network"></a>Yeni bir sanal ağa dağıtma

İlk olarak, bir kapsayıcı grubu dağıtın ve yeni bir sanal ağ ve alt ağ için parametreleri belirtin. Bu parametreleri belirttiğiniz zaman, Azure sanal ağ ve alt ağ oluşturur, alt ağı Azure Kapsayıcı örneklerine devreder ve bir ağ profili de oluşturur. Bu kaynaklar oluşturulduktan sonra, kapsayıcı grubunuz alt ağa dağıtılır.

Yeni bir sanal ağ ve alt ağ için ayarları belirten aşağıdaki [az kapsayıcı oluşturma][az-container-create] komutunu çalıştırın. Sanal ağdaki kapsayıcı grup dağıtımlarının [kullanılabildiği](#virtual-network-deployment-limitations)bir bölgede oluşturulan bir kaynak grubunun adını sağlamanız gerekir. Bu komut, statik bir web sayfasına hizmet veren küçük bir Node.js web sunucusu çalıştıran ortak Microsoft [aci-helloworld][aci-helloworld] kapsayıcısını dağıtıyor. Sonraki bölümde, aynı alt ağa ikinci bir kapsayıcı grubu dağıtacak ve iki kapsayıcı örneği arasındaki iletişimi sınayacaksınız.

```azurecli
az container create \
    --name appcontainer \
    --resource-group myResourceGroup \
    --image mcr.microsoft.com/azuredocs/aci-helloworld \
    --vnet aci-vnet \
    --vnet-address-prefix 10.0.0.0/16 \
    --subnet aci-subnet \
    --subnet-address-prefix 10.0.0.0/24
```

Bu yöntemi kullanarak yeni bir sanal ağa dağıttığınızda, ağ kaynakları oluşturulurken dağıtım birkaç dakika sürebilir. İlk dağıtımdan sonra, ek kapsayıcı grubu dağıtımları daha hızlı tamamlar.

### <a name="deploy-to-existing-virtual-network"></a>Varolan sanal ağa dağıtma

Artık yeni bir sanal ağa bir kapsayıcı grubu dağıttığınız için, aynı alt ağa ikinci bir kapsayıcı grubu dağıtın ve iki kapsayıcı örneği arasındaki iletişimi doğrulayın.

İlk olarak, dağıttığınız ilk konteyner grubunun IP adresini, *ek kapsayıcıyı*alın:

```azurecli
az container show --resource-group myResourceGroup --name appcontainer --query ipAddress.ip --output tsv
```

Çıktı, konteyner grubunun IP adresini özel alt ağda görüntülemelidir:

```console
$ az container show --resource-group myResourceGroup --name appcontainer --query ipAddress.ip --output tsv
10.0.0.4
```

Şimdi, `CONTAINER_GROUP_IP` `az container show` komutile aldığınız IP'ye ayarlayın ve aşağıdaki `az container create` komutu uygulayın. Bu ikinci kapsayıcı, *commchecker,* bir Alp Linux `wget` tabanlı görüntü çalışır ve ilk konteyner grubunun özel alt ağ IP adresi karşı yürütür.

```azurecli
CONTAINER_GROUP_IP=<container-group-IP-here>

az container create \
    --resource-group myResourceGroup \
    --name commchecker \
    --image alpine:3.5 \
    --command-line "wget $CONTAINER_GROUP_IP" \
    --restart-policy never \
    --vnet aci-vnet \
    --subnet aci-subnet
```

Bu ikinci kapsayıcı dağıtımı tamamlandıktan sonra, yürütüldüğü `wget` komutun çıktısını görebilmeniz için günlüklerini çekin:

```azurecli
az container logs --resource-group myResourceGroup --name commchecker
```

İkinci kapsayıcı ilki ile başarıyla iletişim kuruyorsa, çıktı aşağıdakilere benzer olmalıdır:

```console
$ az container logs --resource-group myResourceGroup --name commchecker
Connecting to 10.0.0.4 (10.0.0.4:80)
index.html           100% |*******************************|  1663   0:00:00 ETA
```

Günlük çıktısı, `wget` yerel alt ağdaki özel IP adresini kullanarak dizin dosyasını ilk kapsayıcıdan bağlayıp indirebildiğini göstermelidir. İki kapsayıcı grubu arasındaki ağ trafiği sanal ağ içinde kaldı.

### <a name="deploy-to-existing-virtual-network---yaml"></a>Mevcut sanal ağa dağıtın - YAML

Yaml dosyası, Kaynak Yöneticisi şablonu veya Python SDK gibi başka bir programyöntemi kullanarak bir kapsayıcı grubunu varolan bir sanal ağa da dağıtabilirsiniz. Sanal ağdaki bir alt ağa dağıtmak için YAML'de birkaç ek özellik belirtirsiniz:

* `ipAddress`: Konteyner grubunun IP adresi ayarları.
  * `ports`: Varsa açılacak bağlantı noktaları.
  * `protocol`: Açılan bağlantı noktası için protokol (TCP veya UDP).
* `networkProfile`: Azure kaynağı için sanal ağ ve alt ağ gibi ağ ayarlarını belirtir.
  * `id`: Tam Kaynak Yöneticisi kaynak `networkProfile`kimliği.

Bir kapsayıcı grubunu YAML dosyası olan sanal ağa dağıtmak için öncelikle ağ profilinin kimliğini almanız gerekir. Sanal [ağınızı][az-network-profile-list] ve temsilci alt ağınızı içeren kaynak grubunun adını belirterek az ağ profil listesi komutunu çalıştırın.

``` azurecli
az network profile list --resource-group myResourceGroup --query [0].id --output tsv
```

Komutun çıktısı ağ profili için tam kaynak kimliğini görüntüler:

```console
$ az network profile list --resource-group myResourceGroup --query [0].id --output tsv
/subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkProfiles/aci-network-profile-aci-vnet-aci-subnet
```

Ağ profil kimliğini aldıktan sonra, aşağıdaki YAML'yi *vnet-deploy-aci.yaml*adlı yeni bir dosyaya kopyalayın. Altında, `networkProfile` `id` değeri yeni aldığınız kimlikle değiştirin ve dosyayı kaydedin. Bu YAML, sanal ağınızda *appcontaineryaml* adında bir kapsayıcı grubu oluşturur.

```YAML
apiVersion: '2018-09-01'
location: westus
name: appcontaineryaml
properties:
  containers:
  - name: appcontaineryaml
    properties:
      image: mcr.microsoft.com/azuredocs/aci-helloworld
      ports:
      - port: 80
        protocol: TCP
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  ipAddress:
    type: Private
    ports:
    - protocol: tcp
      port: '80'
  networkProfile:
    id: /subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkProfiles/aci-network-profile-aci-vnet-subnet
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Az kapsayıcı oluşturma komutu ile kapsayıcı grubunu dağıtın ve parametreiçin YAML dosya adını belirtin: [az container create][az-container-create] `--file`

```azurecli
az container create --resource-group myResourceGroup --file vnet-deploy-aci.yaml
```

Dağıtım tamamlandıktan sonra, durumunu görüntülemek için [az kapsayıcı göster][az-container-show] komutunu çalıştırın:

```console
$ az container show --resource-group myResourceGroup --name appcontaineryaml --output table
Name              ResourceGroup    Status    Image                                       IP:ports     Network    CPU/Memory       OsType    Location
----------------  ---------------  --------  ------------------------------------------  -----------  ---------  ---------------  --------  ----------
appcontaineryaml  myResourceGroup  Running   mcr.microsoft.com/azuredocs/aci-helloworld  10.0.0.5:80  Private    1.0 core/1.5 gb  Linux     westus
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

### <a name="delete-container-instances"></a>Kapsayıcı örneklerini silme

Oluşturduğunuz kapsayıcı örnekleriyle çalışmayı bitirdiğinizde, bunları aşağıdaki komutlarla silin:

```azurecli
az container delete --resource-group myResourceGroup --name appcontainer -y
az container delete --resource-group myResourceGroup --name commchecker -y
az container delete --resource-group myResourceGroup --name appcontaineryaml -y
```

### <a name="delete-network-resources"></a>Ağ kaynaklarını silme

Bu özellik şu anda daha önce oluşturduğunuz ağ kaynaklarını silmek için birkaç ek komut gerektirir. Sanal ağınızı ve alt ağınızı oluşturmak için bu makalenin önceki bölümlerindeki örnek komutları kullandıysanız, bu ağ kaynaklarını silmek için aşağıdaki komut dosyasını kullanabilirsiniz. Komut dosyası, kaynak grubunuzun tek bir ağ profiline sahip tek bir sanal ağ içerdiğini varsayar.

Komut dosyasını yürütmeden `RES_GROUP` önce, değişkeni silinmesi gereken sanal ağ ve alt ağı içeren kaynak grubunun adına ayarlayın. Daha önce önerilen `aci-vnet` adı kullanmadıysanız sanal ağın adını güncelleştirin. Komut dosyası Bash kabuğu için biçimlendirilmiştir. PowerShell veya Command Prompt gibi başka bir kabuk tercih ederseniz, değişken atamave erişime girenleri buna göre ayarlamanız gerekir.

> [!WARNING]
> Bu komut dosyası kaynakları siler! Sanal ağı ve içerdiği tüm alt ağları siler. Bu komut dosyasını çalıştırmadan önce içerdiği alt ağlar da dahil olmak üzere sanal ağdaki kaynaklardan *hiçbirine* artık ihtiyacınız olmadığından emin olun. Silindikten sonra, **bu kaynaklar kurtarılamaz.**

```azurecli
# Replace <my-resource-group> with the name of your resource group
# Assumes one virtual network in resource group
RES_GROUP=<my-resource-group>

# Get network profile ID
# Assumes one profile in virtual network
NETWORK_PROFILE_ID=$(az network profile list --resource-group $RES_GROUP --query [0].id --output tsv)

# Delete the network profile
az network profile delete --id $NETWORK_PROFILE_ID -y

# Delete virtual network
az network vnet delete --resource-group $RES_GROUP --name aci-vnet
```

## <a name="next-steps"></a>Sonraki adımlar

Kaynak Yöneticisi şablonu kullanarak yeni bir sanal ağ, alt ağ, ağ profili ve kapsayıcı grubu dağıtmak için [bkz.](https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-vnet
)

Bu makalede, kısa bir süre için de çeşitli sanal ağ kaynakları ve özellikleri ele alınmıştır. Azure Sanal Ağ belgeleri bu konuları kapsamlı bir şekilde kapsamaktadır:

* [Sanal ağ](../virtual-network/manage-virtual-network.md)
* [Alt ağ](../virtual-network/virtual-network-manage-subnet.md)
* [Hizmet uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md)
* [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [Ekspres Rota](../expressroute/expressroute-introduction.md)

<!-- IMAGES -->
[aci-vnet-01]: ./media/container-instances-vnet/aci-vnet-01.png

<!-- LINKS - External -->
[aci-helloworld]: https://hub.docker.com/_/microsoft-azuredocs-aci-helloworld
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show
[az-network-vnet-create]: /cli/azure/network/vnet#az-network-vnet-create
[az-network-profile-list]: /cli/azure/network/profile#az-network-profile-list
