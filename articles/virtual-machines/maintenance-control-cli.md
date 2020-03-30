---
title: Bakım denetimi
description: Bakım Denetimi'ni kullanarak Azure VM'lerinize ne zaman bakım uygulandığını nasıl denetlerinizi öğrenin.
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.workload: infrastructure-services
ms.date: 11/21/2019
ms.author: cynthn
ms.openlocfilehash: 58c0964d170f49066802b955f09dab01eaf998a7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250185"
---
# <a name="preview-control-updates-with-maintenance-control-and-the-azure-cli"></a>Önizleme: Bakım Denetimi ve Azure CLI ile güncelleştirmeleri denetleme

Bakım denetimini kullanarak yeniden başlatma gerektirmeyen platform güncelleştirmelerini yönetin. Azure, güvenilirliği, performansı, güvenliği artırmak veya yeni özellikler başlatmak için altyapısını sık sık güncelleştirir. Güncelleştirmelerin çoğu kullanıcılar için saydamdır. Oyun, medya akışı ve finansal işlemler gibi bazı hassas iş yükleri, birkaç saniyelik VM donma veya bakım için bağlantının kesilmesine bile tahammül edemez. Bakım denetimi, platform güncelleştirmelerini bekleme ve bunları 35 günlük bir haddeleme süresi içinde uygulama seçeneği sunar. 

Bakım denetimi, güncelleştirmeleri yalıtılmış VM'lerinize ve Azure Özel Ana Bilgisayarlarınıza ne zaman uygulayacağınıza karar vermenizi sağlar.

Bakım kontrolü ile şunları yapabilirsiniz:
- Toplu güncelleştirmeleri tek bir güncelleştirme paketinde tamamlar.
- Güncelleştirmeleri uygulamak için 35 güne kadar bekleyin. 
- Azure İşlevlerini kullanarak bakım pencereniz için platform güncelleştirmelerini otomatikleştirin.
- Bakım yapılandırmaları abonelikler ve kaynak grupları arasında çalışır. 

> [!IMPORTANT]
> Bakım Denetimi şu anda genel önizlemede.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.
>

## <a name="limitations"></a>Sınırlamalar

- VM'ler özel bir [ana bilgisayarda](./linux/dedicated-hosts.md)olmalı veya [yalıtılmış VM boyutu](./linux/isolation.md)kullanılarak oluşturulmalıdır.
- 35 gün sonra otomatik olarak bir güncelleştirme uygulanır.
- Kullanıcı **kaynak katkıda bulunan** erişimi ne olmalıdır.


## <a name="install-the-maintenance-extension"></a>Bakım uzantısını yükleme

[Azure CLI'yi](https://docs.microsoft.com/cli/azure/install-azure-cli) yerel olarak yüklemeyi seçerseniz, sürüm 2.0.76 veya sonraki sürüm gerekir.

Önizleme `maintenance` CLI uzantısını yerel olarak veya Cloud Shell'e yükleyin. 

```azurecli-interactive
az extension add -n maintenance
```


## <a name="create-a-maintenance-configuration"></a>Bakım yapılandırması oluşturma

Bakım `az maintenance configuration create` yapılandırması oluşturmak için kullanın. Bu örnek, ana bilgisayara kapsamı *myConfig* adlı bir bakım yapılandırması oluşturur. 

```azurecli-interactive
az group create \
   --location eastus \
   --name myMaintenanceRG
az maintenance configuration create \
   -g myMaintenanceRG \
   --name myConfig \
   --maintenanceScope host\
   --location  eastus
```

Daha sonra kullanmak üzere çıktıdan yapılandırma kimliğini kopyalayın.

Bakım `--maintenanceScope host` config'inin ana bilgisayara güncelleştirmeleri denetlemek için kullanılmasını sağlar.

Aynı ada sahip, ancak farklı bir konumda bir yapılandırma oluşturmaya çalışırsanız, bir hata alırsınız. Yapılandırma adları aboneliğinize özgü olmalıdır.

Kullanılabilir bakım yapılandırmalarını kullanarak `az maintenance configuration list`sorgulayabilirsiniz.

```azurecli-interactive
az maintenance configuration list --query "[].{Name:name, ID:id}" -o table 
```

## <a name="assign-the-configuration"></a>Yapılandırmayı atama

Yapılandırmayı yalıtılmış VM'nize veya Azure Özel Ana Bilgisayarınıza atamak için kullanın. `az maintenance assignment create`

### <a name="isolated-vm"></a>Yalıtılmış VM

Yapılandırmanın kimliğini kullanarak yapılandırmayı bir VM'ye uygulayın. VM'nin `--resource-type virtualMachines` adını ve VM'deki `--resource-name` `--resource-group`kaynak grubunu ve VM'nin yerini belirtin `--location`ve tedarik edin. 

```azurecli-interactive
az maintenance assignment create \
   --resource-group myMaintenanceRG \
   --location eastus \
   --resource-name myVM \
   --resource-type virtualMachines \
   --provider-name Microsoft.Compute \
   --configuration-assignment-name myConfig \
   --maintenance-configuration-id "/subscriptions/1111abcd-1a11-1a2b-1a12-123456789abc/resourcegroups/myMaintenanceRG/providers/Microsoft.Maintenance/maintenanceConfigurations/myConfig"
```

### <a name="dedicated-host"></a>Ayrılmış konak

Özel bir ana bilgisayara yapılandırma uygulamak `--resource-type hosts`için, ana `--resource-parent-name` bilgisayar grubunun adını `--resource-parent-type hostGroups`ve . 

Parametre `--resource-id` ana bilgisayar kimliğidir. Özel ana bilgisayarınızın kimliğini almak için [az vm ana bilgisayar get-instance-view'ı](/cli/azure/vm/host#az-vm-host-get-instance-view) kullanabilirsiniz.

```azurecli-interactive
az maintenance assignment create \
   -g myDHResourceGroup \
   --resource-name myHost \
   --resource-type hosts \
   --provider-name Microsoft.Compute \
   --configuration-assignment-name myConfig \
   --maintenance-configuration-id "/subscriptions/1111abcd-1a11-1a2b-1a12-123456789abc/resourcegroups/myDhResourceGroup/providers/Microsoft.Maintenance/maintenanceConfigurations/myConfig" \
   -l eastus \
   --resource-parent-name myHostGroup \
   --resource-parent-type hostGroups 
```

## <a name="check-configuration"></a>Yapılandırmayı denetleyin

Yapılandırmanın doğru uygulandığını doğrulayabilir veya şu anda hangi yapılandırmanın uygulandığını `az maintenance assignment list`denetleyebilirsiniz.

### <a name="isolated-vm"></a>Yalıtılmış VM

```azurecli-interactive
az maintenance assignment list \
   --provider-name Microsoft.Compute \
   --resource-group myMaintenanceRG \
   --resource-name myVM \
   --resource-type virtualMachines \
   --query "[].{resource:resourceGroup, configName:name}" \
   --output table
```

### <a name="dedicated-host"></a>Ayrılmış konak 

```azurecli-interactive
az maintenance assignment list \
   --resource-group myDHResourceGroup \
   --resource-name myHost \
   --resource-type hosts \
   --provider-name Microsoft.Compute \
   --resource-parent-name myHostGroup \
   --resource-parent-type hostGroups 
   --query "[].{ResourceGroup:resourceGroup,configName:name}" \
   -o table
```


## <a name="check-for-pending-updates"></a>Bekleyen güncelleştirmeleri denetleme

Bekleyen `az maintenance update list` güncelleştirmeler olup olmadığını görmek için kullanın. VM içeren aboneliğin kimliği olmak üzere güncelleştirme --abonelik.

Güncelleştirme yoksa, komut metni içeren bir hata iletisi döndürecektir: `Resource not found...StatusCode: 404`.

Güncelleştirmeler varsa, bekleyen birden çok güncelleştirme olsa bile yalnızca bir tanesi döndürülür. Bu güncelleştirmenin verileri bir nesnede döndürülür:

```text
[
  {
    "impactDurationInSec": 9,
    "impactType": "Freeze",
    "maintenanceScope": "Host",
    "notBefore": "2020-03-03T07:23:04.905538+00:00",
    "resourceId": "/subscriptions/9120c5ff-e78e-4bd0-b29f-75c19cadd078/resourcegroups/DemoRG/providers/Microsoft.Compute/hostGroups/demoHostGroup/hosts/myHost",
    "status": "Pending"
  }
]
  ```

### <a name="isolated-vm"></a>Yalıtılmış VM

Yalıtılmış bir VM için bekleyen güncelleştirmeleri denetleyin. Bu örnekte, çıktı okunabilirlik için bir tablo olarak biçimlendirilir.

```azurecli-interactive
az maintenance update list \
   -g myMaintenanceRg \
   --resource-name myVM \
   --resource-type virtualMachines \
   --provider-name Microsoft.Compute \
   -o table
```

### <a name="dedicated-host"></a>Ayrılmış konak

Özel bir ana bilgisayar için bekleyen güncelleştirmeleri denetlemek için. Bu örnekte, çıktı okunabilirlik için bir tablo olarak biçimlendirilir. Kaynakların değerlerini kendi değerlerinizle değiştirin.

```azurecli-interactive
az maintenance update list \
   --subscription 1111abcd-1a11-1a2b-1a12-123456789abc \
   -g myHostResourceGroup \
   --resource-name myHost \
   --resource-type hosts \
   --provider-name Microsoft.Compute \
   --resource-parentname myHostGroup \
   --resource-parent-type hostGroups \
   -o table
```

## <a name="apply-updates"></a>Güncelleştirme uygulama

Bekleyen `az maintenance apply update` güncelleştirmeleri uygulamak için kullanın. Başarı yada, bu komut güncelleştirmenin ayrıntılarını içeren JSON'u döndürecektir.

### <a name="isolated-vm"></a>Yalıtılmış VM

Güncelleştirmeleri yalıtılmış bir VM'ye uygulamak için bir istek oluşturun.

```azurecli-interactive
az maintenance applyupdate create \
   --subscription 1111abcd-1a11-1a2b-1a12-123456789abc \
   --resource-group myMaintenanceRG \
   --resource-name myVM \
   --resource-type virtualMachines \
   --provider-name Microsoft.Compute
```


### <a name="dedicated-host"></a>Ayrılmış konak

Güncelleştirmeleri özel bir ana bilgisayara uygulayın.

```azurecli-interactive
az maintenance applyupdate create \
   --subscription 1111abcd-1a11-1a2b-1a12-123456789abc \
   --resource-group myHostResourceGroup \
   --resource-name myHost \
   --resource-type hosts \
   --provider-name Microsoft.Compute \
   --resource-parent-name myHostGroup \
   --resource-parent-type hostGroups
```

## <a name="check-the-status-of-applying-updates"></a>Güncelleştirmeleri uygulama durumunu denetleme 

Güncelleştirmelerin ilerlemesini şu şekilde `az maintenance applyupdate get`kontrol edebilirsiniz. 

Son güncelleştirmenin sonuçlarını görmek için güncelleştirme adı olarak `default` `myUpdateName` kullanabilir veya çalıştırdığınızda `az maintenance applyupdate create`döndürülen güncelleştirmenin adı ile değiştirebilirsiniz.

```text
Status         : Completed
ResourceId     : /subscriptions/12ae7457-4a34-465c-94c1-17c058c2bd25/resourcegroups/TestShantS/providers/Microsoft.Comp
ute/virtualMachines/DXT-test-04-iso
LastUpdateTime : 1/1/2020 12:00:00 AM
Id             : /subscriptions/12ae7457-4a34-465c-94c1-17c058c2bd25/resourcegroups/TestShantS/providers/Microsoft.Comp
ute/virtualMachines/DXT-test-04-iso/providers/Microsoft.Maintenance/applyUpdates/default
Name           : default
Type           : Microsoft.Maintenance/applyUpdates
```
LastUpdateTime, güncelleştirmenin tamamlandığını, sizin veya kendi bakım penceresinin kullanılmaması durumunda platform tarafından başlatıldığı zaman olacaktır. Bakım denetimi yoluyla hiç güncelleştirme uygulanmamışsa, varsayılan değeri gösterir.

### <a name="isolated-vm"></a>Yalıtılmış VM

```azurecli-interactive
az maintenance applyupdate get \
   --resource-group myMaintenanceRG \
   --resource-name myVM \
   --resource-type virtualMachines \
   --provider-name Microsoft.Compute \
   --apply-update-name default 
```

### <a name="dedicated-host"></a>Ayrılmış konak

```azurecli-interactive
az maintenance applyupdate get \
   --subscription 1111abcd-1a11-1a2b-1a12-123456789abc \ 
   --resource-group myMaintenanceRG \
   --resource-name myHost \
   --resource-type hosts \
   --provider-name Microsoft.Compute \
   --resource-parent-name myHostGroup \ 
   --resource-parent-type hostGroups \
   --apply-update-name myUpdateName \
   --query "{LastUpdate:lastUpdateTime, Name:name, ResourceGroup:resourceGroup, Status:status}" \
   --output table
```


## <a name="delete-a-maintenance-configuration"></a>Bakım yapılandırması silme

Bakım `az maintenance configuration delete` yapılandırmasını silmek için kullanın. Yapılandırmanın silmesi, ilgili kaynaklardan bakım denetimini kaldırır.

```azurecli-interactive
az maintenance configuration delete \
   --subscription 1111abcd-1a11-1a2b-1a12-123456789abc \
   -g myResourceGroup \
   --name myConfig
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için [Bkz. Bakım ve güncelleştirmeler.](maintenance-and-updates.md)
