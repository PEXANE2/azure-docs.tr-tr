---
title: Kullanılabilirlik Bölgeleri kullanan bir Azure ölçeği kümesi oluşturma
description: Kesintilere karşı daha fazla fazlalık için Kullanılabilirlik Bölgelerini kullanan Azure sanal makine ölçeği kümelerini nasıl oluşturabilirsiniz öğrenin
author: ju-shim
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.topic: conceptual
ms.date: 08/08/2018
ms.author: jushiman
ms.openlocfilehash: c8795f46e47b2ab43898f6f436b9ee6026a22fa7
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81011574"
---
# <a name="create-a-virtual-machine-scale-set-that-uses-availability-zones"></a>Kullanılabilirlik Bölgeleri kullanan sanal makine ölçeği kümesi oluşturma

Sanal makine ölçeği kümelerinizi veri merkezi düzeyindeki hatalardan korumak için Kullanılabilirlik Bölgeleri arasında bir ölçek kümesi oluşturabilirsiniz. Kullanılabilirlik Bölgelerini destekleyen Azure bölgeleri, her biri kendi bağımsız güç kaynağına, ağına ve soğutmalarına sahip en az üç ayrı bölgeye sahiptir. Daha fazla bilgi için, [Kullanılabilirlik Bölgelerine Genel Bakış](../availability-zones/az-overview.md)bölümüne bakın.

## <a name="availability-considerations"></a>Kullanılabilirlik konusunda dikkat edilmesi gerekenler

API sürüm *2017-12-01*itibariyle bir veya daha fazla bölgeye bir ölçek kümesi dağıttığınızda, "maksimum yayılma" veya "statik 5 hata etki alanı yayma" ile dağıtma seçeneğiniz vardır. Maksimum yayılma yla, ölçek kümesi VM'lerinizi her bölge içinde mümkün olduğunca çok sayıda hata etki alanına yayır. Bu yayılma, bölge başına beşten fazla veya daha az hata etki alanına yayılabilir. "Statik 5 hata etki alanı nın yayılması" ile ölçek kümesi VM'lerinizi bölge başına tam olarak beş hata etki alanına yayır. Ölçek kümesi ayırma isteğini karşılamak için bölge başına beş farklı hata etki alanı bulamazsa, istek başarısız olur.

Bu yaklaşım çoğu durumda en iyi yayılmayı sağladığından, **çoğu iş yükü için maksimum yayılma yla dağıtmanızı öneririz.** Çoğaltmaların farklı donanım yalıtım birimlerine yayılması gerekiyorsa, Kullanılabilirlik Bölgeleri arasında yayılmanızı ve her bölge içinde maksimum yayılmayı kullanmanızı öneririz.

Maksimum yayılma ile, vm'lerin kaç hata etki alanına yayılmış olduğuna bakılmaksızın ölçek kümesi VM örnek görünümünde ve örnek meta verilerde yalnızca bir hata etki alanı görürsünüz. Her bölge içinde yayılma örtülüdür.

Maksimum yayılma yıkullanmak için *PlatformFaultDomainCount'ı* *1'e*ayarlayın. Statik beş hata etki alanı yayılmasını kullanmak için *platformFaultDomainCount'ı* *5'e*ayarlayın. API *sürümünde 2017-12-01*, *platformFaultDomainCount* tek bölgeli ve çapraz bölge ölçek kümeleri için *1* varsayılan. Şu anda, bölgesel (non-zonal) ölçek kümeleri için yalnızca statik beş hata etki alanı yayılması desteklenir.

### <a name="placement-groups"></a>Yerleştirme grupları

Bir ölçek kümesi dağıttığınızda, Kullanılabilirlik Bölgesi başına tek bir [yerleşim grubuyla](./virtual-machine-scale-sets-placement-groups.md) veya bölge başına birden fazla yerleşim grubuyla dağıtma seçeneğiniz de olur. Bölgesel (non-zonal) ölçek kümeleri için, seçim bölgede tek bir yerleşim grubu veya bölgede birden fazla olması. Çoğu iş yükü için, daha fazla ölçek sağlayan birden çok yerleşim grubu öneririz. API *sürümü2017-12-01'de*ölçek, tek bölgeli ve bölgeler arası ölçek kümeleri için birden çok yerleşim grubuna varsayılan olarak ayarlar, ancak bölgesel (zonal olmayan) ölçek kümeleri için varsayılan olarak tek bir yerleşim grubu olarak kümeler.

> [!NOTE]
> Maksimum yayılma kullanıyorsanız, birden çok yerleşim grubu kullanmanız gerekir.

### <a name="zone-balancing"></a>Bölge dengeleme

Son olarak, birden çok bölgede dağıtılan ölçek kümeleri için "en iyi çaba bölgesi dengesi" veya "sıkı bölge dengesi" seçeneğini zedeleme seçeneğiniz de vardır. Ölçek kümesi için diğer tüm bölgelerde her bölge aynı sayıda\\VM veya + - 1 VM ise bir ölçek kümesi "dengeli" olarak kabul edilir. Örneğin:

- Bölge 1'de 2 VM, bölge 2'de 3 VM ve bölge 3'teki 3 VM'den bir ölçek dengeli olarak kabul edilir. Farklı bir VM sayısına sahip tek bir bölge vardır ve diğer bölgelere göre yalnızca 1 bölge daha azdır. 
- Bölge 1'de 1 VM, bölge 2'de 3 VM ve bölge 3'teki 3 VM'li bir ölçek dengesiz olarak kabul edilir. Bölge 1'de 2 ve 3.

Ölçek kümesindeki VM'ler başarıyla oluşturulmuş olabilir, ancak bu VM'lerin uzantıları dağıtılamaz. Uzantılı bu VM'ler, ölçek kümesinin dengeli olup olmadığını belirlerken yine de sayılır. Örneğin, bölge 1'de 3 VM, bölge 2'de 3 VM ve bölge 3'teki 3 VM'den alınan bir ölçek, tüm uzantılar bölge 1'de başarısız olsa ve tüm uzantılar 2 ve 3 bölgesinde başarılı olsa bile dengeli olarak kabul edilir.

En iyi çaba alan bölge dengesi ile, ölçek seti dengeyi korurken içeri ve dışa ölçeklendirme girişimleri nde dir. Ancak, bu herhangi bir nedenle mümkün değilse (örneğin, bir bölge çökerse, ölçek kümesi o bölgede yeni bir VM oluşturamazsa), ölçek kümesi geçici dengesizlik içinde veya dışında başarıyla ölçeklendirmesine olanak tanır. Sonraki ölçek-out denemelerinde, ölçek kümesi dengelenecek şekilde ayarlanmış ölçek için daha fazla VM gerektiren bölgelere VM ekler. Benzer şekilde, denemelerde sonraki ölçekte, ölçek kümesi dengelenmesi için kümenin daha az VM'ye ihtiyaç duyduğu bölgelerden VM'leri kaldırır. "Sıkı bölge dengesi" ile, ölçek kümesi, dengesizlik neden olur eğer ölçeklendirmek için herhangi bir girişim başarısız olur.

En iyi çaba bölge dengesini kullanmak için *zoneBalance'ı* *false'a*ayarlayın. Bu ayar, *2017-12-01*API sürümünde varsayılandır. Sıkı bölge dengesini kullanmak için *zoneBalance'ı* *doğru*ayarlayın.

## <a name="single-zone-and-zone-redundant-scale-sets"></a>Tek bölgeli ve bölge yedekli ölçek kümeleri

Sanal makine ölçeği kümesi dağıttığınızda, bir bölgede veya birden çok bölgede tek bir Kullanılabilirlik Bölgesi kullanmayı seçebilirsiniz.

Tek bir bölgede bir ölçek kümesi oluşturduğunuzda, tüm bu VM örneklerinin hangi bölgede koşturduğuna ve ölçek kümesinin yönetildiğini ve yalnızca bu bölge içinde otomatik ölçeklendirildiğini denetlersiniz. Bölge yedekli ölçek kümesi, birden çok bölgeye yayılan tek bir ölçek kümesi oluşturmanıza olanak tanır. VM örnekleri oluşturulduğundan, varsayılan olarak bölgeler arasında eşit olarak dengelenirler. Bölgelerden birinde bir kesinti oluşursa, bir ölçek kümesi otomatik olarak kapasiteyi artırmak için ölçeklendirmez. En iyi yöntem, CPU veya bellek kullanımına dayalı otomatik ölçeklendirme kurallarını yapılandırmak olacaktır. Otomatik ölçeklendirme kuralları, ölçek kümesinin, kalan işlem bölgelerindeki yeni örnekleri ölçeklendirerek o bölgede vm örneklerinin kaybına yanıt vermesine olanak sağlar.

Kullanılabilirlik Bölgelerini kullanmak için ölçek kümenizin desteklenen bir [Azure bölgesinde](../availability-zones/az-overview.md#services-support-by-region)oluşturulması gerekir. Aşağıdaki yöntemlerden biriyle Kullanılabilirlik Bölgeleri kullanan bir ölçek kümesi oluşturabilirsiniz:

- [Azure portal](#use-the-azure-portal)
- Azure CLI
- [Azure PowerShell](#use-azure-powershell)
- [Azure Resource Manager şablonları](#use-azure-resource-manager-templates)

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Kullanılabilirlik Bölgesi kullanan bir ölçek kümesi oluşturma [işlemi, başlangıç makalesinde](quick-create-portal.md)ayrıntılı olarak belirtildiği gibi. Desteklenen bir Azure bölgesini seçtiğinizde, aşağıdaki örnekte gösterildiği gibi, bir veya daha fazla kullanılabilir bölgede bir ölçek kümesi oluşturabilirsiniz:

![Tek bir Kullanılabilirlik Bölgesinde ölçek kümesi oluşturma](media/virtual-machine-scale-sets-use-availability-zones/vmss-az-portal.png)

Azure yük dengeleyicisi ve genel IP adresi gibi ölçek kümesi ve destekleyici kaynaklar, belirttiğiniz tek bölgede oluşturulur.

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

Kullanılabilirlik Bölgesi kullanan bir ölçek kümesi oluşturma [işlemi, başlangıç makalesinde](quick-create-cli.md)ayrıntılı olarak belirtildiği gibi. Kullanılabilirlik Bölgelerini kullanmak için, ölçek kümenizi desteklenen bir Azure bölgesinde oluşturmanız gerekir.

`--zones` [Az vmss'e](/cli/azure/vmss) parametre ekleyin komut u oluşturun ve hangi bölgenin kullanılacağını belirtin (bölge *1,* *2*veya *3*gibi). Aşağıdaki örnek, bölge *1'de* *myScaleSet* adlı tek bölgeli bir ölçek kümesi oluşturur:

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --zones 1
```

Tek bölgeli ölçek kümesi ve ağ kaynaklarının tam bir örneği için [bu örnek CLI komut dosyasına](https://github.com/Azure/azure-docs-cli-python-samples/blob/master/virtual-machine-scale-sets/create-single-availability-zone/create-single-availability-zone.sh) bakın

### <a name="zone-redundant-scale-set"></a>Bölge yedekli ölçek kümesi

Bölge yedekli ölçek kümesi oluşturmak için *Standart* SKU genel IP adresi ve yük dengeleyicisi kullanırsınız. Gelişmiş artıklık *için, Standart* SKU bölge artıklı ağ kaynakları oluşturur. Daha fazla bilgi için Azure [Yük Dengeleyici Standardıgenel bakışı](../load-balancer/load-balancer-standard-overview.md) ve [Standart Yük Dengeleyicisi ve Kullanılabilirlik Bölgeleri'ne](../load-balancer/load-balancer-standard-availability-zones.md)bakın.

Bölge yedekli ölçek kümesi oluşturmak için parametreli `--zones` birden çok bölge belirtin. Aşağıdaki örnek, *1,2,3*bölgeleri arasında *myScaleSet* adlı bir bölge yedekli ölçek kümesi oluşturur:

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --zones 1 2 3
```

Belirttiğiniz bölge(ler)deki tüm ölçek kümesi kaynaklarını ve VM'leri oluşturmak ve yapılandırmak birkaç dakika sürer. Bölge yedekli ölçek kümesi nin ve ağ kaynaklarının tam bir örneği için [bu örnek CLI komut dosyasına](https://github.com/Azure/azure-docs-cli-python-samples/blob/master/virtual-machine-scale-sets/create-zone-redundant-scale-set/create-zone-redundant-scale-set.sh) bakın

## <a name="use-azure-powershell"></a>Azure PowerShell kullanma

Kullanılabilirlik Bölgelerini kullanmak için, ölçek kümenizi desteklenen bir Azure bölgesinde oluşturmanız gerekir. `-Zone` [Yeni-AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig) komutuna parametre ekleyin ve hangi bölgenin kullanılacağını belirtin (bölge *1*, *2*veya *3*gibi).

Aşağıdaki örnek, *Doğu ABD 2* bölge *1* *myScaleSet* adlı tek bölgeli bir ölçek kümesi oluşturur. Sanal ağ, genel IP adresi ve yük dengeleyici için Azure ağ kaynakları otomatik olarak oluşturulur. İstendiğinde, ölçek kümesindeki sanal makine örnekleri için kendi istediğiniz yönetici kimlik bilgilerini sağlayın:

```powershell
New-AzVmss `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS2" `
  -VMScaleSetName "myScaleSet" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicy "Automatic" `
  -Zone "1"
```

### <a name="zone-redundant-scale-set"></a>Bölge yedekli ölçek kümesi

Bölge yedekli ölçek kümesi oluşturmak için parametreli `-Zone` birden çok bölge belirtin. Aşağıdaki örnek, *Doğu ABD 2* bölgeleri *1, 2, 3*arasında *myScaleSet* adlı bir bölge yedekli ölçek kümesi oluşturur. Sanal ağ, genel IP adresi ve yük dengeleyicisi için bölge yedekli Azure ağ kaynakları otomatik olarak oluşturulur. İstendiğinde, ölçek kümesindeki sanal makine örnekleri için kendi istediğiniz yönetici kimlik bilgilerini sağlayın:

```powershell
New-AzVmss `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS2" `
  -VMScaleSetName "myScaleSet" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicy "Automatic" `
  -Zone "1", "2", "3"
```

## <a name="use-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanma

Kullanılabilirlik Bölgesi kullanan bir ölçek kümesi oluşturma işlemi, [Linux](quick-create-template-linux.md) veya [Windows](quick-create-template-windows.md)için başlangıç makalesinde ayrıntılı olarak belirtildiği gibi. Kullanılabilirlik Bölgelerini kullanmak için, ölçek kümenizi desteklenen bir Azure bölgesinde oluşturmanız gerekir. `zones` Özelliği *şablonunuzdaMicrosoft.Compute/virtualMachineScaleSets* kaynak türüne ekleyin ve hangi bölgenin kullanılacağını belirtin (bölge *1*, *2*veya *3*gibi).

Aşağıdaki örnek, *Doğu ABD 2* bölge *1* *myScaleSet* adlı bir Linux tek bölgeli ölçek kümesi oluşturur:

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US 2",
  "apiVersion": "2017-12-01",
  "zones": ["1"],
  "sku": {
    "name": "Standard_A1",
    "capacity": "2"
  },
  "properties": {
    "upgradePolicy": {
      "mode": "Automatic"
    },
    "virtualMachineProfile": {
      "storageProfile": {
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage"
        },
        "imageReference":  {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "16.04-LTS",
          "version": "latest"
        }
      },
      "osProfile": {
        "computerNamePrefix": "myvmss",
        "adminUsername": "azureuser",
        "adminPassword": "P@ssw0rd!"
      }
    }
  }
}
```

Tek bölgeli ölçek kümesi ve ağ kaynaklarının tam bir örneği için [bu örnek Kaynak Yöneticisi şablonuna](https://github.com/Azure/vm-scale-sets/blob/master/preview/zones/singlezone.json) bakın

### <a name="zone-redundant-scale-set"></a>Bölge yedekli ölçek kümesi

Bölge gereksiz ölçek kümesi oluşturmak için, `zones` *Microsoft.Compute/virtualMachineScaleSets* kaynak türü için özellikte birden çok değer belirtin. Aşağıdaki örnek, *Doğu ABD 2* bölgeleri *1,2,3*arasında *myScaleSet* adlı bir bölge yedekli ölçek kümesi oluşturur:

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US 2",
  "apiVersion": "2017-12-01",
  "zones": [
        "1",
        "2",
        "3"
      ]
}
```

Genel bir IP adresi veya yük dengeleyicisi oluşturursanız, *"sku": { "ad": "Standart" }"* özelliğini bölge yedekli ağ kaynakları oluşturmak için belirtin. Ayrıca, herhangi bir trafiğe izin vermek için bir Ağ Güvenlik Grubu ve kuralları oluşturmanız gerekir. Daha fazla bilgi için Azure [Yük Dengeleyici Standardıgenel bakışı](../load-balancer/load-balancer-standard-overview.md) ve [Standart Yük Dengeleyicisi ve Kullanılabilirlik Bölgeleri'ne](../load-balancer/load-balancer-standard-availability-zones.md)bakın.

Bölge yedekli ölçek kümesi ve ağ kaynaklarının tam bir örneği için [bu örnek Kaynak Yöneticisi şablonuna](https://github.com/Azure/vm-scale-sets/blob/master/preview/zones/multizone.json) bakın

## <a name="next-steps"></a>Sonraki adımlar

Artık Bir Kullanılabilirlik Bölgesi'nde bir ölçek kümesi oluşturduğunuza göre, [uygulamaları sanal makine ölçeği kümelerinde](tutorial-install-apps-cli.md) dağıtmayı veya sanal makine ölçek [kümeleriyle otomatik ölçek](tutorial-autoscale-cli.md)kullanmayı öğrenebilirsiniz.
