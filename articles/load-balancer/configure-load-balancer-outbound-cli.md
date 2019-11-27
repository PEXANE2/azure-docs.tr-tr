---
title: Azure CLı kullanarak yük dengelemeyi ve giden kuralları yapılandırma
titleSuffix: Azure Load Balancer
description: Bu makalede, Azure CLI kullanarak nasıl standart yük dengeleyici Yük Dengeleme veya giden kuralları yapılandırılacağı gösterilmektedir.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/01/2019
ms.author: allensu
ms.openlocfilehash: 7230b0c2b80137b068bbeacf43ab2133491a69b0
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74225480"
---
# <a name="configure-load-balancing-and-outbound-rules-in-standard-load-balancer-using-azure-cli"></a>Standart yük Azure CLI kullanarak dengeleyici Yük Dengeleme ve giden kuralları yapılandırma

Bu hızlı başlangıçta Azure CLI kullanarak nasıl standart yük dengeleyici giden kuralları yapılandırılacağını gösterir.  

İşiniz bittiğinde, yük dengeleyici kaynağı iki ön uçlar ve bunlarla ilişkili kurallar içeriyor: için bir gelen ve giden.  Her ön uç genel IP adresi ve bu senaryo kullanır, farklı bir genel IP adresi için gelen ve giden bir başvuru içeriyor.   Yük Dengeleme kuralını yalnızca gelen Yük Dengeleme sağlar ve giden NAT VM için sağlanan giden kuralı denetler.  Bu hızlı başlangıç, bir diğeri gelen ve giden için bir tane olmak üzere iki ayrı arka uç havuzu kullanır, bu senaryo için yetenek ve esneklik sağlar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.28 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[az group create](https://docs.microsoft.com/cli/azure/group) ile bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek *eastus2* konumunda *myresourcegroupoutbağlanmadı* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
  az group create \
    --name myresourcegroupoutbound \
    --location eastus2
```
## <a name="create-virtual-network"></a>Sanal ağ oluşturma
[Az Network VNET Create](https://docs.microsoft.com/cli/azure/network/vnet)kullanılarak *myresourcegroupoutbağlanmadı* adlı alt ağ ile *myvnetoutbağlanmadı* *adlı bir* sanal ağ oluşturun.

```azurecli-interactive
  az network vnet create \
    --resource-group myresourcegroupoutbound \
    --name myvnetoutbound \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mysubnetoutbound \
    --subnet-prefix 192.168.0.0/24
```

## <a name="create-inbound-public-ip-address"></a>Gelen genel IP adresi oluşturma 

Web uygulamanıza İnternet’ten erişmek için yük dengeleyicinin genel IP adresi gereklidir. Standart Yük Dengeleyici yalnızca Standart Genel IP adreslerini destekler. *Myresourcegroupoutbound*içinde *mypublicipınbound* ADLı standart bir genel IP adresi oluşturmak için [az Network public-ip Create](https://docs.microsoft.com/cli/azure/network/public-ip) komutunu kullanın.

```azurecli-interactive
  az network public-ip create --resource-group myresourcegroupoutbound --name mypublicipinbound --sku standard
```

## <a name="create-outbound-public-ip-address"></a>Giden genel IP adresi oluşturma 

[Az Network public-IP Create](https://docs.microsoft.com/cli/azure/network/public-ip)kullanılarak Load Balancer ön uç giden yapılandırması için standart bir IP adresi oluşturun.

```azurecli-interactive
  az network public-ip create --resource-group myresourcegroupoutbound --name mypublicipoutbound --sku standard
```

## <a name="create-azure-load-balancer"></a>Azure yük dengeleyici oluşturma

Bu bölümde yük dengeleyicinin aşağıdaki bileşenlerini nasıl oluşturabileceğiniz ve yapılandırabileceğiniz açıklanmaktadır:
  - Yük dengeleyicideki gelen ağ trafiğini alan bir ön uç IP.
  - Ön uç IP 'nin yük dengeli ağ trafiğini gönderdiği bir arka uç Havuzu.
  - Giden bağlantı için bir arka uç Havuzu. 
  - arka uç sanal makine örneklerinin durumunu belirleyen bir durum araştırması.
  - Trafiğin sanal makinelere dağıtımını tanımlayan bir yük dengeleyici gelen kuralı.
  - Trafiğin sanal makinelerinden dağıtımını tanımlayan bir yük dengeleyici giden kuralı.

### <a name="create-load-balancer"></a>Yük Dengeleyici oluşturma

Bir gelen ön uç IP yapılandırması ve önceki adımda oluşturduğunuz *mypublicipınbound* genel IP adresiyle ilişkili bir arka *uç havuzu olan* [az Network lb Create](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest) adlı *lb* öğesini kullanarak gelen IP adresine sahip bir Load Balancer oluşturun.

```azurecli-interactive
  az network lb create \
    --resource-group myresourcegroupoutbound \
    --name lb \
    --sku standard \
    --backend-pool-name bepoolinbound \
    --frontend-ip-name myfrontendinbound \
    --location eastus2 \
    --public-ip-address mypublicipinbound   
  ```

### <a name="create-outbound-pool"></a>Giden Havuz oluştur

[Az Network lb Address-Pool Create](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest) , *Bepooloutbound*adıyla bir VM havuzu için giden bağlantıyı tanımlamak üzere ek bir arka uç adres havuzu oluşturun.  Ayrı bir giden havuzun oluşturulması en yüksek esnekliği sağlar, ancak bu adımı atlayabilir ve yalnızca gelen *bepoolınas* 'yı kullanabilirsiniz.

```azurecli-interactive
  az network lb address-pool create \
    --resource-group myresourcegroupoutbound \
    --lb-name lb \
    --name bepooloutbound
```

### <a name="create-outbound-frontend-ip"></a>Giden ön uç IP oluşturma
Load Balancer için giden ön uç IP yapılandırmasını oluşturun [az Network lb ön uç-IP Create](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest) The myfrontendoutbound ADLı genel IP adresi *mypublicıpoıb* ile ilişkili olan adlı giden ön uç IP yapılandırması.

```azurecli-interactive
  az network lb frontend-ip create \
    --resource-group myresourcegroupoutbound \
    --name myfrontendoutbound \
    --lb-name lb \
    --public-ip-address mypublicipoutbound 
  ```

### <a name="create-health-probe"></a>Durum araştırması oluşturma

Sistem durumu araştırması tüm sanal makine örneklerini denetleyerek ağ trafiği gönderdiklerinden emin olur. Sistem durumu denetimi başarısız olan sanal makine örnekleri tekrar çevrimiçi olana ve sistem durumu denetimi iyi olduğuna karar verene kadar yük dengeleyiciden kaldırılır. Sanal makinelerin durumunu izlemek için [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe?view=azure-cli-latest) ile bir durum araştırması oluşturun. 

```azurecli-interactive
  az network lb probe create \
    --resource-group myresourcegroupoutbound \
    --lb-name lb \
    --name http \
    --protocol http \
    --port 80 \
    --path /  
```

### <a name="create-load-balancing-rule"></a>Yük Dengeleme kuralı oluşturma

Yük Dengeleyici kuralı, gelen trafik ve gerekli kaynak ve hedef bağlantı noktalarının yanı sıra trafiği almak için arka uç havuzu için ön uç IP yapılandırması tanımlar. Ön *uç havuzundaki 80* numaralı bağlantı noktasını dinlemek için [az Network lb Rule Create](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest) *ile bir* yük dengeleyici kuralı oluşturun ve arka uç adres havuzu *beyana* yük dengeli ağ trafiği gönderir 80 numaralı bağlantı noktasını kullanma. 

>[!NOTE]
>Devre dışı bırakma giden-snat parametresiyle birlikte, bu kural nedeniyle otomatik giden (S) NAT bu Yük Dengeleme kuralı devre dışı bırakır. Giden NAT, yalnızca giden kuralı tarafından sağlanır.

```azurecli-interactive
az network lb rule create \
--resource-group myresourcegroupoutbound \
--lb-name lb \
--name inboundlbrule \
--protocol tcp \
--frontend-port 80 \
--backend-port 80 \
--probe http \
--frontend-ip-name myfrontendinbound \
--backend-pool-name bepoolinbound \
--disable-outbound-snat
```

### <a name="create-outbound-rule"></a>Giden kuralı oluşturma

Giden bir kural, tüm giden NAT trafiği ve bu kuralın uygulandığı arka uç havuzu için kullanılacak ön uç *myfrontendoutbound*tarafından temsil edilen ön uç genel IP 'yi tanımlar.  *Bepool* arka uç havuzundaki tüm sanal MAKINELERIN (NIC IP yapılandırması) giden ağ çevirisi için *mdeni boundrule* çıkış kuralını oluşturun.  Aşağıdaki komutta giden boşta kalma zaman aşımı 4'ten 15 dakika olarak değiştirir ve 10000 SNAT 1024 yerine bağlantı noktası ayırır.  Daha fazla ayrıntı için [giden kurallarını](https://aka.ms/lboutboundrules) gözden geçirin.

```azurecli-interactive
az network lb outbound-rule create \
 --resource-group myresourcegroupoutbound \
 --lb-name lb \
 --name outboundrule \
 --frontend-ip-configs myfrontendoutbound \
 --protocol All \
 --idle-timeout 15 \
 --outbound-ports 10000 \
 --address-pool bepooloutbound
```

Ayrı bir giden havuz kullanmak istemiyorsanız, bunun yerine *bepoolınbound* öğesini belirtmek için önceki komutta adres havuzu bağımsız değişkenini değiştirebilirsiniz.  Elde edilen yapılandırmanın esneklik ve okunabilirlik için ayrı havuzlar kullanmanız önerilir.

Bu noktada, [az Network NIC IP-Config Address-Pool Add](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest)komutunu kullanarak ilgili NIC kaynaklarının IP YAPıLANDıRMASıNı güncelleştirerek sanal makinenizin *bpoolınbound* __ve__ *bepooloutbound* arka uç havuzuna eklenmesine devam edebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az group delete](/cli/azure/group#az-group-delete) komutunu kullanarak kaynak grubunu, yük dengeleyiciyi ve tüm ilgili kaynakları kaldırabilirsiniz.

```azurecli-interactive 
  az group delete --name myresourcegroupoutbound
```

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, standart yük dengeleyici oluşturdunuz, hem gelen yük dengeleyici trafik kuralları, yapılandırılmış ve durum yoklaması, arka uç havuzundaki sanal makineleri için yapılandırılmış. Azure Load Balancer hakkında daha fazla bilgi almak için Azure Load Balancer öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Azure Load Balancer öğreticileri](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
