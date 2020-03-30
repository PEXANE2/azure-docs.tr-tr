---
title: CLI'yi kullanarak linux vm'lerini özel ana bilgisayarlara dağıtın
description: Azure CLI'yi kullanarak özel ana bilgisayarlara VM dağıtın.
author: cynthn
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 01/09/2020
ms.author: cynthn
ms.openlocfilehash: ba40e610e31a1215ac90baf63a04b435b636d68a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127688"
---
# <a name="deploy-vms-to-dedicated-hosts-using-the-azure-cli"></a>Azure CLI'yi kullanarak özel ana bilgisayarlara VM dağıtma
 

Bu makalede, sanal makinelerinizi (VM) barındırmak için [Azure'a adanmış](dedicated-hosts.md) bir ana bilgisayar oluşturma konusunda size rehberlik eder. 

Azure CLI sürümünü 2.0.70 veya sonrası olarak yüklediğinizden ve bir `az login`Azure hesabında oturum açtığınızdan emin olun. 


## <a name="limitations"></a>Sınırlamalar

- Sanal makine ölçek kümeleri şu anda özel ana bilgisayarlarda desteklenmez.
- Özel ana bilgisayarlar için kullanılabilen boyutlar ve donanım türleri bölgeye göre değişir. Daha fazla bilgi edinmek için ana bilgisayar [fiyatlandırma sayfasına](https://aka.ms/ADHPricing) bakın.
 

## <a name="create-resource-group"></a>Kaynak grubu oluşturma 
Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. az grubu oluşturma ile kaynak grubu oluşturun. Aşağıdaki örnek, *Doğu ABD* konumunda *myDHResourceGroup* adında bir kaynak grubu oluşturur.

```bash
az group create --name myDHResourceGroup --location eastus 
```
 
## <a name="create-a-host-group"></a>Ana bilgisayar grubu oluşturma 

**Ana bilgisayar grubu,** özel ana bilgisayarkoleksiyonunu temsil eden bir kaynaktır. Bir bölgede ve kullanılabilirlik bölgesinde bir ana bilgisayar grubu oluşturur ve ona ana bilgisayarlar eklersiniz. Yüksek kullanılabilirlik için planlama yaparken, ek seçenekler vardır. İlgili ana bilgisayarlarınızla aşağıdaki seçeneklerden birini veya her ikisini de kullanabilirsiniz: 
- Birden çok kullanılabilirlik bölgesine yayılma. Bu durumda, kullanmak istediğiniz bölgelerin her birinde bir ana bilgisayar grubuna sahip olması gerekir.
- Fiziksel raflara eşlenen birden çok hata etki alanına yayılma alanı. 
 
Her iki durumda da, ana bilgisayar grubunuz için hata etki alanı sayısını sağlamanız gerekir. Grubunuzdaki hata etki alanlarını yayılımını istemiyorsanız, 1 hata etki alanı sayısı kullanın. 

Ayrıca, hem kullanılabilirlik bölgelerini hem de hata etki alanlarını kullanmaya karar verebilirsiniz. 

Bu örnekte, hem kullanılabilirlik bölgelerini hem de hata etki alanlarını kullanan bir ana bilgisayar grubu oluşturmak için [az vm ana bilgisayar grubu oluşturmayı](/cli/azure/vm/host/group#az-vm-host-group-create) kullanacağız. 

```bash
az vm host group create \
   --name myHostGroup \
   -g myDHResourceGroup \
   -z 1 \
   --platform-fault-domain-count 2 
``` 

### <a name="other-examples"></a>Diğer örnekler

Ayrıca, kullanılabilirlik bölgesi 1'de (ve hata etki alanı olmayan) bir ana bilgisayar grubu oluşturmak için [az vm ana bilgisayar grubu oluşturabilirsiniz.](/cli/azure/vm/host/group#az-vm-host-group-create)

```bash
az vm host group create \
   --name myAZHostGroup \
   -g myDHResourceGroup \
   -z 1 \
   --platform-fault-domain-count 1 
```
 
Aşağıdaki az [vm ana bilgisayar grubu yalnızca](/cli/azure/vm/host/group#az-vm-host-group-create) hata etki alanlarını kullanarak bir ana bilgisayar grubu oluşturmak için oluşturur (kullanılabilirlik bölgeleri desteklenmeyen bölgelerde kullanılacak). 

```bash
az vm host group create \
   --name myFDHostGroup \
   -g myDHResourceGroup \
   --platform-fault-domain-count 2 
```
 
## <a name="create-a-host"></a>Ana bilgisayar oluşturma 

Şimdi ev sahibi grubunda özel bir ana bilgisayar oluşturalım. Ev sahibi nin adına ek olarak, ev sahibi için SKU sağlamanız gerekir. Ana bilgisayar SKU, desteklenen VM serisinin yanı sıra özel ana bilgisayarınız için donanım üretimini de yakalar.  

Ana bilgisayar SK'leri ve fiyatlandırması hakkında daha fazla bilgi için [Azure Özel Ana Bilgisayar fiyatlandırması'na](https://aka.ms/ADHPricing)bakın.

Bir ana bilgisayar oluşturmak için [az vm ana bilgisayar oluşturma](/cli/azure/vm/host#az-vm-host-create) kullanın. Ana bilgisayar grubunuz için bir hata etki alanı sayısı ayarlarsanız, ev sahibiniz için hata etki alanını belirtmeniz istenir.  

```bash
az vm host create \
   --host-group myHostGroup \
   --name myHost \
   --sku DSv3-Type1 \
   --platform-fault-domain 1 \
   -g myDHResourceGroup
```


 
## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma 
[Az vm oluştur](/cli/azure/vm#az-vm-create)kullanarak özel bir ana bilgisayar içinde sanal bir makine oluşturun. Ana bilgisayar grubunuzu oluştururken bir kullanılabilirlik bölgesi belirttiyseniz, sanal makineoluştururken aynı bölgeyi kullanmanız gerekir.

```bash
az vm create \
   -n myVM \
   --image debian \
   --generate-ssh-keys \
   --host-group myHostGroup \
   --host myHost \
   --generate-ssh-keys \
   --size Standard_D4s_v3 \
   -g myDHResourceGroup \
   --zone 1
```
 
> [!WARNING]
> Yeterli kaynağa sahip olmayan bir ana bilgisayarda sanal bir makine oluşturursanız, sanal makine BAŞARıSıZ durumda oluşturulur. 


## <a name="check-the-status-of-the-host"></a>Ev sahibinin durumunu denetleme

Az [vm ana bilgisayar get-instance-view](/cli/azure/vm/host#az-vm-host-get-instance-view)kullanarak ana bilgisayar durumu durumunu ve kaç sanal makineyi hala ana bilgisayara dağıtabileceğinizi kontrol edebilirsiniz.

```bash
az vm host get-instance-view \
   -g myDHResourceGroup \
   --host-group myHostGroup \
   --name myHost
```
 Çıktı şuna benzer:
 
```json
{
  "autoReplaceOnFailure": true,
  "hostId": "6de80643-0f45-4e94-9a4c-c49d5c777b62",
  "id": "/subscriptions/10101010-1010-1010-1010-101010101010/resourceGroups/myDHResourceGroup/providers/Microsoft.Compute/hostGroups/myHostGroup/hosts/myHost",
  "instanceView": {
    "assetId": "12345678-1234-1234-abcd-abc123456789",
    "availableCapacity": {
      "allocatableVms": [
        {
          "count": 31.0,
          "vmSize": "Standard_D2s_v3"
        },
        {
          "count": 15.0,
          "vmSize": "Standard_D4s_v3"
        },
        {
          "count": 7.0,
          "vmSize": "Standard_D8s_v3"
        },
        {
          "count": 3.0,
          "vmSize": "Standard_D16s_v3"
        },
        {
          "count": 1.0,
          "vmSize": "Standard_D32-8s_v3"
        },
        {
          "count": 1.0,
          "vmSize": "Standard_D32-16s_v3"
        },
        {
          "count": 1.0,
          "vmSize": "Standard_D32s_v3"
        },
        {
          "count": 1.0,
          "vmSize": "Standard_D48s_v3"
        },
        {
          "count": 0.0,
          "vmSize": "Standard_D64-16s_v3"
        },
        {
          "count": 0.0,
          "vmSize": "Standard_D64-32s_v3"
        },
        {
          "count": 0.0,
          "vmSize": "Standard_D64s_v3"
        }
      ]
    },
    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "displayStatus": "Provisioning succeeded",
        "level": "Info",
        "message": null,
        "time": "2019-07-24T21:22:40.604754+00:00"
      },
      {
        "code": "HealthState/available",
        "displayStatus": "Host available",
        "level": "Info",
        "message": null,
        "time": null
      }
    ]
  },
  "licenseType": null,
  "location": "eastus2",
  "name": "myHost",
  "platformFaultDomain": 1,
  "provisioningState": "Succeeded",
  "provisioningTime": "2019-07-24T21:22:40.604754+00:00",
  "resourceGroup": "myDHResourceGroup",
  "sku": {
    "capacity": null,
    "name": "DSv3-Type1",
    "tier": null
  },
  "tags": null,
  "type": null,
  "virtualMachines": [
    {
      "id": "/subscriptions/10101010-1010-1010-1010-101010101010/resourceGroups/MYDHRESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/MYVM",
      "resourceGroup": "MYDHRESOURCEGROUP"
    }
  ]
}

```
 
## <a name="export-as-a-template"></a>Şablon olarak dışa aktarma 
Şimdi aynı parametrelere sahip ek bir geliştirme ortamı veya onunla eşleşen bir üretim ortamı oluşturmak istiyorsanız, şablon dışa aktarabilirsiniz. Kaynak Yöneticisi, ortamınız için tüm parametreleri tanımlayan JSON şablonlarını kullanır. Bu JSON şablonuna başvurarak tüm ortamları oluşturursunuz. Sizin için JSON şablonu oluşturmak için JSON şablonlarını el ile oluşturabilir veya varolan bir ortamı dışa aktarabilirsiniz. Kaynak grubunuzu dışa aktarmak için [az grubu dışa aktarmayı](/cli/azure/group#az-group-export) kullanın.

```bash
az group export --name myDHResourceGroup > myDHResourceGroup.json 
```

Bu komut, `myDHResourceGroup.json` geçerli çalışma dizininizdeki dosyayı oluşturur. Bu şablondan bir ortam oluşturduğunuzda, tüm kaynak adları için istenir. Bu adları `--include-parameter-default-value` `az group export` komuta parametre ekleyerek şablon dosyanızda doldurabilirsiniz. Kaynak adlarını belirtmek için JSON şablonunuzu edin veya kaynak adlarını belirten bir parameters.json dosyası oluşturun.
 
Şablonunuzdan bir ortam oluşturmak için [az grup dağıtımı oluşturma'yı](/cli/azure/group/deployment#az-group-deployment-create)kullanın.

```bash
az group deployment create \ 
    --resource-group myNewResourceGroup \ 
    --template-file myDHResourceGroup.json 
```


## <a name="clean-up"></a>Temizleme 

Sanal makineler dağıtılmasa bile, özel ana bilgisayarlarınız için ücretlendirilirsiniz. Maliyetleri kaydetmek için şu anda kullanmadığınız ana bilgisayarları silmeniz gerekir.  

Bir ana bilgisayarı yalnızca artık sanal makineler olmadığında silebilirsiniz. [Az vm sil](/cli/azure/vm#az-vm-delete)kullanarak VM'leri silin.

```bash
az vm delete -n myVM -g myDHResourceGroup
```

VM'leri sildikten sonra [az vm ana bilgisayar silme'yi](/cli/azure/vm/host#az-vm-host-delete)kullanarak ana bilgisayarını silebilirsiniz.

```bash
az vm host delete -g myDHResourceGroup --host-group myHostGroup --name myHost 
```
 
Tüm ana bilgisayarlarınızı sildikten sonra, [az vm ana bilgisayar grubu silmeyi](/cli/azure/vm/host/group#az-vm-host-group-delete)kullanarak ana bilgisayar grubunu silebilirsiniz.  
 
```bash
az vm host group delete -g myDHResourceGroup --host-group myHostGroup  
```
 
Ayrıca, tek bir komuttaki tüm kaynak grubunu da silebilirsiniz. Bu, tüm VM'ler, ana bilgisayarlar ve ana bilgisayar grupları da dahil olmak üzere grupta oluşturulan tüm kaynakları siler.
 
```bash
az group delete -n myDHResourceGroup 
```

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi [için, Özel ana bilgisayarlarına](dedicated-hosts.md) genel bakış alabakın.

- [Azure portalını](dedicated-hosts-portal.md)kullanarak özel ana bilgisayarlar da oluşturabilirsiniz.

- Bir bölgede maksimum esneklik için hem bölgeleri hem de fay etki alanlarını kullanan örnek şablon [burada](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md)bulunur.