---
title: Bakım denetimi
description: Bakım denetimini kullanarak Azure VM 'lerinize bakım uygulandığını nasıl denetleyeceğinizi öğrenin.
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.workload: infrastructure-services
ms.date: 11/21/2019
ms.author: cynthn
ms.openlocfilehash: cf4c8c57121a06a607770470c5aef69c5edc0c5f
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77115645"
---
# <a name="preview-control-updates-with-maintenance-control-and-the-azure-cli"></a>Önizleme: bakım denetimi ve Azure CLı ile güncelleştirmeleri denetleme

Bakım denetimini kullanarak yeniden başlatma gerektirmeyen platform güncelleştirmelerini yönetin. Azure, güvenilirliği, performansı, güvenliği geliştirmek veya yeni özellikleri başlatmak için altyapısını sıklıkla güncelleştirir. Çoğu güncelleştirme kullanıcılara saydamdır. Oyun, medya akışı ve finans işlemleri gibi bazı hassas iş yükleri, bir VM 'nin bakım için donuyor veya bağlantısı kesilmesinin birkaç saniyesini de kabul edemez. Bakım denetimi, platform güncelleştirmelerini bekleme ve bunları 35 günlük bir sıralı pencere içinde uygulama seçeneği sunar. 

Bakım denetimi, yalıtılmış sanal makinelerinize ve Azure adanmış ana bilgisayarlara güncelleştirmelerin ne zaman uygulanacağına karar vermenizi sağlar.

Bakım denetimi ile şunları yapabilirsiniz:
- Toplu güncelleştirmeler tek bir güncelleştirme paketine sahiptir.
- Güncelleştirmelerin uygulanması için 35 güne kadar bekleyin. 
- Azure Işlevleri 'ni kullanarak bakım pencereniz için platform güncelleştirmelerini otomatikleştirin.
- Bakım yapılandırması abonelikler ve kaynak grupları arasında çalışır. 

> [!IMPORTANT]
> Bakım denetimi şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="limitations"></a>Sınırlamalar

- VM 'Lerin [ayrılmış bir konakta](./linux/dedicated-hosts.md)olması veya [yalıtılmış bir VM boyutu](./linux/isolation.md)kullanılarak oluşturulması gerekir.
- 35 gün sonra, bir güncelleştirme otomatik olarak uygulanır.
- Kullanıcının, **kaynak katılımcısı** erişimi olmalıdır.


## <a name="install-the-maintenance-extension"></a>Bakım uzantısını yükler

[Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) 'yı yerel olarak yüklemeyi tercih ederseniz, sürüm 2.0.76 veya daha yeni bir sürüme ihtiyacınız vardır.

`maintenance` Preview CLı uzantısını yerel olarak veya Cloud Shell olarak yükler. 

```azurecli-interactive
az extension add -n maintenance
```


## <a name="create-a-maintenance-configuration"></a>Bakım yapılandırması oluşturma

Bakım yapılandırması oluşturmak için `az maintenance configuration create` kullanın. Bu örnek, konakta *MyConfig* adlı bir bakım yapılandırması oluşturur. 

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

Daha sonra kullanmak için çıktıdan yapılandırma KIMLIĞINI kopyalayın.

`--maintenanceScope host` kullanmak, bakım yapılandırmasının konaktaki güncelleştirmeleri denetlemek için kullanılmasını sağlar.

Aynı ada sahip bir yapılandırma oluşturmaya çalışırsanız, ancak farklı bir konumda hata alırsınız. Yapılandırma adları, aboneliğiniz için benzersiz olmalıdır.

`az maintenance configuration list`kullanarak kullanılabilir bakım yapılandırması için sorgulama yapabilirsiniz.

```azurecli-interactive
az maintenance configuration list --query "[].{Name:name, ID:id}" -o table 
```

## <a name="assign-the-configuration"></a>Yapılandırmayı ata

Yapılandırmayı yalıtılmış sanal makinenize veya Azure adanmış ana bilgisayarınıza atamak için `az maintenance assignment create` kullanın.

### <a name="isolated-vm"></a>Yalıtılmış VM

Yapılandırma KIMLIĞINI kullanarak yapılandırmayı bir VM 'ye uygulayın. `--resource-type virtualMachines` belirtin ve `--resource-name`sanal makinenin adını ve `--resource-group`içindeki VM 'ye yönelik kaynak grubunu ve `--location`için sanal makinenin konumunu belirtin. 

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

Ayrılmış bir konağa yapılandırma uygulamak için `--resource-type hosts`, konak grubunun adı ile `--resource-parent-name` ve `--resource-parent-type hostGroups`dahil etmeniz gerekir. 

`--resource-id` parametresi konağın KIMLIĞIDIR. Adanmış ana makinenizin KIMLIĞINI almak için [az VM Host Get-instance-View](/cli/azure/vm/host#az-vm-host-get-instance-view) ' i kullanabilirsiniz.

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

## <a name="check-configuration"></a>Yapılandırmayı denetle

Yapılandırmanın doğru şekilde uygulandığını doğrulayabilirsiniz veya `az maintenance assignment list`kullanarak şu anda uygulanan yapılandırmayı görmek için kontrol edebilirsiniz.

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


## <a name="check-for-pending-updates"></a>Bekleyen güncelleştirmeleri denetle

Bekleyen güncelleştirmeler olup olmadığını görmek için `az maintenance update list` kullanın. Güncelleştirme--VM 'yi içeren aboneliğin KIMLIĞI olacak abonelik.

Güncelleştirme yoksa, komut, şu metni içeren bir hata iletisi döndürür: `Resource not found...StatusCode: 404`.

Güncelleştirmeler varsa, bekleyen birden çok güncelleştirme olsa bile yalnızca bir tane döndürülür. Bu güncelleştirmenin verileri bir nesnesinde döndürülür:

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

Yalıtılmış bir VM için bekleyen güncelleştirmeleri denetleyin. Bu örnekte, çıkış okunabilirlik için bir tablo olarak biçimlendirilir.

```azurecli-interactive
az maintenance update list \
   -g myMaintenanceRg \
   --resource-name myVM \
   --resource-type virtualMachines \
   --provider-name Microsoft.Compute \
   -o table
```

### <a name="dedicated-host"></a>Ayrılmış konak

Adanmış bir ana bilgisayar için bekleyen güncelleştirmeleri denetlemek için. Bu örnekte, çıkış okunabilirlik için bir tablo olarak biçimlendirilir. Kaynakların değerlerini kendi değerlerinizle değiştirin.

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

Bekleyen güncelleştirmeleri uygulamak için `az maintenance apply update` kullanın. Başarı durumunda bu komut, güncelleştirmenin ayrıntılarını içeren JSON döndürür.

### <a name="isolated-vm"></a>Yalıtılmış VM

Yalıtılmış bir VM 'ye güncelleştirme uygulamak için bir istek oluşturun.

```azurecli-interactive
az maintenance applyupdate create \
   --subscription 1111abcd-1a11-1a2b-1a12-123456789abc \
   --resource-group myMaintenanceRG \
   --resource-name myVM \
   --resource-type virtualMachines \
   --provider-name Microsoft.Compute
```


### <a name="dedicated-host"></a>Ayrılmış konak

Güncelleştirmeleri adanmış bir konağa uygulayın.

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

## <a name="check-the-status-of-applying-updates"></a>Güncelleştirmelerin uygulama durumunu denetleyin 

`az maintenance applyupdate get`kullanarak güncelleştirmelerin ilerlemesini kontrol edebilirsiniz. 

Son güncelleştirmenin sonuçlarını görmek için `default` güncelleştirme adı olarak kullanabilir veya `myUpdateName` `az maintenance applyupdate create`çalıştırdığınızda döndürülen güncelleştirmenin adıyla değiştirebilirsiniz.

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


## <a name="delete-a-maintenance-configuration"></a>Bakım yapılandırmasını silme

Bakım yapılandırmasını silmek için `az maintenance configuration delete` kullanın. Yapılandırma silindiğinde, bakım denetimi ilişkili kaynaklardan kaldırılır.

```azurecli-interactive
az maintenance configuration delete \
   --subscription 1111abcd-1a11-1a2b-1a12-123456789abc \
   -g myResourceGroup \
   --name myConfig
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz. [bakım ve güncelleştirmeler](maintenance-and-updates.md).
