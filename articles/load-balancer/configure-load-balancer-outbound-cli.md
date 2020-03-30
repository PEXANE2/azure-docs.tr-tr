---
title: Azure CLI kullanarak yük dengeleme ve giden kuralları yapılandırma
titleSuffix: Azure Load Balancer
description: Bu makalede, Azure CLI'yi kullanarak standart yük dengeleyicisinde yük dengeleme ve giden kurallarınasıl yapılandırılanın gösterilmektedir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74225480"
---
# <a name="configure-load-balancing-and-outbound-rules-in-standard-load-balancer-using-azure-cli"></a>Azure CLI’yi kullanarak Standart Load Balancer’da yük dengelemeyi ve giden kurallarını yapılandırma

Bu hızlı başlangıç, Azure CLI'yi kullanarak Standart Yük Dengeleyici'nde giden kuralları nasıl yapılandırabileceğinizi gösterir.  

İşbittiğinde, Yük Dengeleyici sağlık gütücü iki frontends ve bunlarla ilişkilerle ilişkin kurallar içerir: biri gelen, diğeri de giden için.  Her ön uç, genel bir IP adresine başvurur ve bu senaryo, gelen ekarşı giden için farklı bir genel IP adresi kullanır.   Yük dengeleme kuralı yalnızca gelen yük dengelemesağlar ve giden kural VM için sağlanan giden NAT'yi kontrol eder.  Bu hızlı başlatma, yeteneği göstermek ve bu senaryo için esneklik sağlamak için biri gelen, diğeri giden ler için olmak üzere iki ayrı arka uç havuzu kullanır.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.28 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[az group create](https://docs.microsoft.com/cli/azure/group) ile bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek, *eastus2* konumunda *myresourcegroupoutbound* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
  az group create \
    --name myresourcegroupoutbound \
    --location eastus2
```
## <a name="create-virtual-network"></a>Sanal ağ oluşturma
Az network [vnet create](https://docs.microsoft.com/cli/azure/network/vnet)kullanarak *myresourcegroupoutbound* *mysubnetoutbound* adlı bir alt ağ ile *myvnetoutbound* adlı bir sanal ağ oluşturun.

```azurecli-interactive
  az network vnet create \
    --resource-group myresourcegroupoutbound \
    --name myvnetoutbound \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mysubnetoutbound \
    --subnet-prefix 192.168.0.0/24
```

## <a name="create-inbound-public-ip-address"></a>Gelen Genel IP adresi oluşturma 

Web uygulamanıza İnternet’ten erişmek için yük dengeleyicinin genel IP adresi gereklidir. Standart Yük Dengeleyici yalnızca Standart Genel IP adreslerini destekler. *Myresourcegroupoutbound'da* *mypublicipinbound* adlı standart bir Genel IP adresi oluşturmak için [az network public-ip oluşturmayı](https://docs.microsoft.com/cli/azure/network/public-ip) kullanın.

```azurecli-interactive
  az network public-ip create --resource-group myresourcegroupoutbound --name mypublicipinbound --sku standard
```

## <a name="create-outbound-public-ip-address"></a>Giden genel IP adresi oluşturma 

[Az Network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip)kullanarak Yük Dengeleyici'nin ön uç giden yapılandırması için standart bir IP adresi oluşturun.

```azurecli-interactive
  az network public-ip create --resource-group myresourcegroupoutbound --name mypublicipoutbound --sku standard
```

## <a name="create-azure-load-balancer"></a>Azure Load Balancer oluşturma

Bu bölümde yük dengeleyicinin aşağıdaki bileşenlerini nasıl oluşturabileceğiniz ve yapılandırabileceğiniz açıklanmaktadır:
  - Yük dengeleyicisi üzerinde gelen ağ trafiğini alan bir ön uç IP'si.
  - Ön uç IP'sinin yük dengeli ağ trafiğini gönderdiği bir arka uç havuzu.
  - Giden bağlantı için bir arka uç havuzu. 
  - Arka uç VM örneklerinin durumunu belirleyen bir sağlık sondası.
  - Trafiğin VM'lere nasıl dağıtıldığını tanımlayan bir yük dengeleyici gelen kuralı.
  - VM'lerden trafiğin nasıl dağıtıldığını tanımlayan bir yük dengeleyici giden kuralı.

### <a name="create-load-balancer"></a>Yük Dengeleyicisi Oluştur

Bir gelen ip bakiyesi oluşturmak [az ağ lb](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest) kullanarak gelen IP adresi ile gelen bir ön uç IP yapılandırması ve bir önceki adımda oluşturduğunuz genel IP adresi *mypublicipinbound* ile ilişkili bir arka uç havuzu *bepoolinbound* içeren *lb* adlı oluşturun.

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

### <a name="create-outbound-pool"></a>Giden havuz oluşturma

[Az ağ lb adresi havuzu](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest) *bepooloutbound*adı ile oluşturmak az ağ lb adresi havuzu ile VMs bir havuz için giden bağlantı tanımlamak için ek bir arka uç adresi havuzu oluşturun.  Ayrı bir giden havuz oluşturmak maksimum esneklik sağlar, ancak bu adımı atlayabilir ve yalnızca gelen *havuzlu* havuzu da kullanabilirsiniz.

```azurecli-interactive
  az network lb address-pool create \
    --resource-group myresourcegroupoutbound \
    --lb-name lb \
    --name bepooloutbound
```

### <a name="create-outbound-frontend-ip"></a>Giden frontend IP oluşturma
Az [ağ lb frontend-ip içeren ve](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest) giden frontend IP yapılandırması *myfrontendoutbound* adlı dahil olan yük dengeleyici için giden frontend IP yapılandırmaoluşturun kamu IP adresi *mypublicipoutbound* ilişkili

```azurecli-interactive
  az network lb frontend-ip create \
    --resource-group myresourcegroupoutbound \
    --name myfrontendoutbound \
    --lb-name lb \
    --public-ip-address mypublicipoutbound 
  ```

### <a name="create-health-probe"></a>Sistem durumu sondası oluşturma

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

### <a name="create-load-balancing-rule"></a>Yük dengeleme kuralı oluşturma

Yük dengeleyici kuralı, gelen trafik için ön uç IP yapılandırmasını ve trafiği almak için arka uç havuzunu, gerekli kaynak ve hedef bağlantı noktasıyla birlikte tanımlar. Az ağ lb kuralı ile bir yük dengeleyici *kural myinboundlbrule* oluşturun ön uç havuzu *myfrontendinbound* bağlantı da bağlantı noktası *bepool* 80 kullanarak yük dengeli ağ trafiği göndermek için port 80 dinlemek için [oluşturun.](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest) 

>[!NOTE]
>Bu yük dengeleme kuralı otomatik giden (S)NAT'ı bu kuralın bir sonucu olarak devre dışı bırakarak --devre dışı bırak-giden-snat parametresi. Giden NAT yalnızca giden kural tarafından sağlanır.

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

Giden kural, ön uç *myfrontendoutbound*tarafından temsil edilen ön uç genel IP'yi tanımlar, bu kural geçerli dir.  *Bepool* arka uç havuzundaki tüm sanal makinelerin (NIC IP yapılandırmaları) giden ağ çevirisi için giden kural *myoutboundrule* oluşturun.  Aşağıdaki komut ayrıca giden boşta zaman ödeme süresini 4 dakikadan 15 dakikaya değiştirir ve 1024 yerine 10000 SNAT bağlantı noktası ayırır.  Daha fazla ayrıntı için [giden kuralları](https://aka.ms/lboutboundrules) gözden geçirin.

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

Ayrı bir giden havuz kullanmak istemiyorsanız, önceki komuttaki adres havuzu bağımsız değişkenini değiştirerek bunun yerine bir havuz *alabilen* bir havuz belirtebilirsiniz.  Ortaya çıkan yapılandırmanın esnekliği ve okunabilirliği için ayrı havuzlar kullanmanızı öneririz.

Bu noktada, [az network nic ip-config adres havuzu ekle](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest)kullanarak ilgili NIC kaynaklarının IP yapılandırmasını güncelleyerek vm'lerinizi arka uç havuzuna ekleyerek *bepoolinbound* __devam__ *bepooloutbound* edebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az group delete](/cli/azure/group#az-group-delete) komutunu kullanarak kaynak grubunu, yük dengeleyiciyi ve tüm ilgili kaynakları kaldırabilirsiniz.

```azurecli-interactive 
  az group delete --name myresourcegroupoutbound
```

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Standart Yük Dengeleyicisi oluşturdunuz, hem gelen yük dengeleyici trafik kurallarını yapılandırıldınız, arka uç havuzundaki VM'ler için yapılandırıldınız ve sistem durumu sondası. Azure Load Balancer hakkında daha fazla bilgi almak için Azure Load Balancer öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Azure Load Balancer öğreticileri](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
