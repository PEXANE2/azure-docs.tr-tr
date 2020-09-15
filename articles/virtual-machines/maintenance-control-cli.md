---
title: CLı kullanarak Azure sanal makineleri için bakım denetimi
description: Bakım denetimi ve CLı kullanarak Azure VM 'lerinize bakım uygulandığını nasıl denetleyeceğinizi öğrenin.
author: cynthn
ms.service: virtual-machines
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 04/20/2020
ms.author: cynthn
ms.openlocfilehash: 67e33732574d2a6c173675d5adf0a7d1c2050688
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90528185"
---
# <a name="control-updates-with-maintenance-control-and-the-azure-cli"></a>Bakım denetimi ve Azure CLı ile güncelleştirmeleri denetleme

Bakım denetimi, yalıtılmış sanal makinelerinize ve Azure adanmış ana bilgisayarlara güncelleştirmelerin ne zaman uygulanacağına karar vermenizi sağlar. Bu konu, bakım denetimi için Azure CLı seçeneklerini içerir. Bakım denetimini, sınırlamalarını ve diğer yönetim seçeneklerini kullanmanın avantajları hakkında daha fazla bilgi için bkz. [Platform güncelleştirmelerini bakım denetimiyle yönetme](maintenance-control.md).

## <a name="create-a-maintenance-configuration"></a>Bakım yapılandırması oluşturma

`az maintenance configuration create`Bakım yapılandırması oluşturmak için kullanın. Bu örnek, konakta *MyConfig* adlı bir bakım yapılandırması oluşturur. 

```azurecli-interactive
az group create \
   --location eastus \
   --name myMaintenanceRG
az maintenance configuration create \
   -g myMaintenanceRG \
   --name myConfig \
   --maintenanceScope host\
   --location eastus
```

Daha sonra kullanmak için çıktıdan yapılandırma KIMLIĞINI kopyalayın.

Kullanarak, `--maintenanceScope host` bakım yapılandırmasının konaktaki güncelleştirmeleri denetlemek için kullanılmasını sağlar.

Aynı ada sahip bir yapılandırma oluşturmaya çalışırsanız, ancak farklı bir konumda hata alırsınız. Yapılandırma adları, kaynak grubunuz için benzersiz olmalıdır.

Kullanarak kullanılabilir bakım yapılandırması için sorgulama yapabilirsiniz `az maintenance configuration list` .

```azurecli-interactive
az maintenance configuration list --query "[].{Name:name, ID:id}" -o table 
```

## <a name="assign-the-configuration"></a>Yapılandırmayı ata

`az maintenance assignment create`Yapılandırmayı YALıTıLMıŞ sanal makinenize veya Azure adanmış konağa atamak için kullanın.

### <a name="isolated-vm"></a>Yalıtılmış VM

Yapılandırma KIMLIĞINI kullanarak yapılandırmayı bir VM 'ye uygulayın. İçin `--resource-type virtualMachines` sanal makinenin adını `--resource-name` ve içindeki VM 'ye yönelik kaynak grubunu `--resource-group` ve VM 'nin konumunu belirtin ve sağlayın `--location` . 

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

Bir yapılandırmayı adanmış bir konağa uygulamak için, `--resource-type hosts` `--resource-parent-name` konak grubunun adı ve ile birlikte dahil etmeniz gerekir `--resource-parent-type hostGroups` . 

Parametresi, `--resource-id` KONAĞıN kimliğidir. Adanmış ana makinenizin KIMLIĞINI almak için [az VM Host Get-instance-View](/cli/azure/vm/host#az-vm-host-get-instance-view) ' i kullanabilirsiniz.

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

Yapılandırmanın doğru şekilde uygulandığını doğrulayabilirsiniz veya hangi yapılandırmanın geçerli olarak uygulandığını görmek için kontrol edin `az maintenance assignment list` .

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

`az maintenance update list`Bekleyen güncelleştirmeler olup olmadığını görmek için kullanın. Güncelleştirme--VM 'yi içeren aboneliğin KIMLIĞI olacak abonelik.

Güncelleştirme yoksa, komut, şu metni içeren bir hata iletisi döndürür: `Resource not found...StatusCode: 404` .

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

`az maintenance apply update`Bekleyen güncelleştirmeleri uygulamak için kullanın. Başarı durumunda bu komut, güncelleştirmenin ayrıntılarını içeren JSON döndürür.

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

' İ kullanarak güncelleştirmelerin ilerlemesini kontrol edebilirsiniz `az maintenance applyupdate get` . 

`default`Son güncelleştirmenin sonuçlarını görmek için güncelleştirme adı olarak veya öğesini `myUpdateName` çalıştırdığınızda döndürülen güncelleştirmenin adıyla değiştirin `az maintenance applyupdate create` .

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
LastUpdateTime, güncelleştirme tamamlandığında sizin tarafınızdan ya da kendi kendine bakım penceresi kullanılmadığınızda platform tarafından başlatılan zaman olacaktır. Bakım denetimi aracılığıyla uygulanan bir güncelleştirme olmamışsa, varsayılan değer gösterilir.

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

`az maintenance configuration delete`Bakım yapılandırmasını silmek için kullanın. Yapılandırma silindiğinde, bakım denetimi ilişkili kaynaklardan kaldırılır.

```azurecli-interactive
az maintenance configuration delete \
   --subscription 1111abcd-1a11-1a2b-1a12-123456789abc \
   -g myResourceGroup \
   --name myConfig
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz. [bakım ve güncelleştirmeler](maintenance-and-updates.md).
