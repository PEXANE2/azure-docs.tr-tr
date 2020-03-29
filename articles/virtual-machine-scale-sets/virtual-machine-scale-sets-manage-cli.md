---
title: Azure CLI ile Sanal Makine Ölçek Kümelerini Yönetme
description: Ortak Azure CLI komutları, bir örneğin nasıl başlatıp durdurulur veya ölçek kümesi kapasitesini değiştirme gibi Sanal Makine Ölçeği Kümelerini yönetmeyi emreder.
author: cynthn
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: cynthn
ms.openlocfilehash: d8f40708cf5a9989146a72e2093de7bad47ffde9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76275744"
---
# <a name="manage-a-virtual-machine-scale-set-with-the-azure-cli"></a>Azure CLI ile sanal makine ölçeği kümesini yönetme
Sanal makine ölçek kümesinin yaşam döngüsü boyunca bir veya daha fazla yönetim görevi çalıştırmanız gerekebilir. Ayrıca, çeşitli yaşam döngüsü görevlerini otomatikleştiren betikler oluşturmak isteyebilirsiniz. Bu makalede, bu görevleri gerçekleştirmenize izin veren ortak Azure CLI komutlarından bazıları ayrıntılarıyla anlatılıyor.

Bu yönetim görevlerini tamamlamak için en son Azure CLI'sine ihtiyacınız var. Daha fazla bilgi için Azure [CLI'yi yükleyin.](/cli/azure/install-azure-cli) Sanal makine ölçeği kümesi oluşturmanız gerekiyorsa, [Azure CLI ile bir ölçek kümesi oluşturabilirsiniz.](quick-create-cli.md)


## <a name="view-information-about-a-scale-set"></a>Ölçek kümesi hakkındaki bilgileri görüntüleme
Bir ölçek kümesi hakkındaki genel bilgileri görüntülemek için [az vmss'i kullanın.](/cli/azure/vmss) Aşağıdaki örnek, *myResourceGroup* kaynak grubunda *myScaleSet* adlı ölçek kümesi hakkında bilgi alır. Kendi adlarınızı aşağıdaki gibi girin:

```azurecli
az vmss show --resource-group myResourceGroup --name myScaleSet
```


## <a name="view-vms-in-a-scale-set"></a>Ölçek kümesindeki VM’leri görüntüleme
Bir ölçek kümesinde VM örneğinin listesini görüntülemek için [az vmss liste örneklerini](/cli/azure/vmss)kullanın. Aşağıdaki örnekte, *myResourceGroup* kaynak grubunda *myScaleSet* adlı ölçek kümesindeki tüm VM örnekleri listelenebilmiştir. Bu adlar için kendi değerlerinizi sağlayın:

```azurecli
az vmss list-instances \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --output table
```

Belirli bir VM örneği hakkında ek `--instance-id` bilgileri görüntülemek için, [az vmss get-instance-view'a](/cli/azure/vmss) parametre ekleyin ve görüntüleyecek bir örnek belirtin. Aşağıdaki örnek, *myScaleSet* ve *myResourceGroup* kaynak grubu adlı ölçek kümesinde VM örnek *0* hakkındaki bilgileri görüntüler. Kendi adlarınızı aşağıdaki gibi girin:

```azurecli
az vmss get-instance-view \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --instance-id 0
```


## <a name="list-connection-information-for-vms"></a>VM'ler için bağlantı bilgilerini listele
Bir ölçek kümesindeki VM'lere bağlanmak için, ssh veya RDP atanmış bir genel IP adresine ve bağlantı noktası numarasına bağlanırsınız. Varsayılan olarak, her VM'ye uzaktan bağlantı trafiğini ileten Azure yük bakiyesine ağ adresi çevirisi (NAT) kuralları eklenir. VM örneklerine bir ölçek kümesinde bağlanmak için adresi ve bağlantı noktalarını listelemek için [az vmss list-instance-connection-info'yu](/cli/azure/vmss)kullanın. Aşağıdaki örnekte, *myScaleSet* adlı ölçek kümesinde ve *myResourceGroup* kaynak grubunda VM örnekleriiçin bağlantı bilgileri listelenebilmiştir. Bu adlar için kendi değerlerinizi sağlayın:

```azurecli
az vmss list-instance-connection-info \
    --resource-group myResourceGroup \
    --name myScaleSet
```


## <a name="change-the-capacity-of-a-scale-set"></a>Ölçek kümesinin kapasitesini değiştirme
Önceki komutlar ölçek kümeniz ve VM örnekleri hakkında bilgi gösterdi. Ölçek kümesindeki örnek sayısını artırmak veya azaltmak için kapasiteyi değiştirebilirsiniz. Ölçek kümesi, gerekli VM sayısını oluşturur veya kaldırır, ardından uygulama trafiğini alacak şekilde VM'leri yapılandırır.

Ölçek kümesinde şu anda yer alan örneklerin sayısını görmek için [az vmss show](/cli/azure/vmss) komutunu kullanarak *sku.capacity* üzerinde bir sorgu çalıştırın:

```azurecli
az vmss show \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

Ardından [az vmss scale](/cli/azure/vmss) ile ölçek kümesindeki sanal makinelerin sayısını elle artırabilir veya azaltabilirsiniz. Aşağıdaki örnek, ölçeğinizdeki VM sayısını *5*olarak ayarlar:

```azurecli
az vmss scale \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --new-capacity 5
```

Ölçek kümenizin kapasitesinin güncelleştirilmesi birkaç dakika sürer. Bir ölçek kümesinin kapasitesini azaltırsanız, önce en yüksek örnek lid'lere sahip VM'ler kaldırılır.


## <a name="stop-and-start-vms-in-a-scale-set"></a>Ölçek kümesindeki VM’leri durdurma ve başlatma
Bir ölçek kümesinde bir veya daha fazla VM'yi durdurmak için [az vmss stop'u](/cli/azure/vmss#az-vmss-stop)kullanın. `--instance-ids` parametresi, durdurulacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler durdurulur. Birden çok VM'yi durdurmak için, her örnek kimliğini bir boşlukla ayırın.

Aşağıdaki örnek, *myScaleSet* ve *myResourceGroup* kaynak grubu adlı ölçek kümesinde *örnek 0'ı* durdurur. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```azurecli
az vmss stop --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```

Durdurulan VM'ler ayrılmış durumda kalır ve işlem ücretleri almaya devam edilir. Bunun yerine VM'lerin ayrılmasını ve yalnızca depolama ücretlerine tabi olmasını istiyorsanız, [az vmss deallocate'ı](/cli/azure/vmss)kullanın. Birden çok VM'yi bulmak için, her örnek kimliğini bir boşlukla ayırın. Aşağıdaki örnek, *myScaleSet* ve *myResourceGroup* kaynak grubu adlı ölçek kümesindeki örnek *0'ı* durdurur ve anlaşma belirler. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```azurecli
az vmss deallocate --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


### <a name="start-vms-in-a-scale-set"></a>VM'leri ölçek kümesinde başlatma
Bir ölçek kümesinde bir veya daha fazla VM başlatmak için [az vmss başlatın.](/cli/azure/vmss) `--instance-ids` parametresi, başlatılacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler başlatılır. Birden çok VM başlatmak için, her örnek kimliğini bir boşlukla ayırın.

Aşağıdaki örnek, *myScaleSet* ve *myResourceGroup* kaynak grubu adlı ölçek kümesinde *örnek 0'ı* başlatır. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```azurecli
az vmss start --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="restart-vms-in-a-scale-set"></a>Ölçek kümesindeki VM’leri yeniden başlatma
Bir ölçek kümesinde bir veya daha fazla VM'yi yeniden başlatmak için [az vmss yeniden başlat'ı](/cli/azure/vmss)kullanın. `--instance-ids` parametresi, yeniden başlatılacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler yeniden başlatılır. Birden çok VM'yi yeniden başlatmak için, her örnek kimliğini bir boşlukla ayırın.

Aşağıdaki örnek, *myScaleSet* ve *myResourceGroup* kaynak grubu adlı ölçek kümesinde örnek *0'ı* yeniden başlatır. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```azurecli
az vmss restart --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="remove-vms-from-a-scale-set"></a>VM'leri ölçek kümesinden kaldırma
Bir ölçek kümesinde bir veya daha fazla VM kaldırmak için [az vmss delete-instances](/cli/azure/vmss)kullanın. `--instance-ids` Parametre kaldırmak için bir veya daha fazla VMs belirtmenize olanak sağlar. Örnek kimlik için * belirtirseniz, ölçek kümesindeki tüm VM'ler kaldırılır. Birden çok VM kaldırmak için, her örnek kimliğini bir boşlukla ayırın.

Aşağıdaki örnek, *myScaleSet* ve *myResourceGroup* kaynak grubu adlı ölçek kümesindeki *örnek 0'ı* kaldırır. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```azurecli
az vmss delete-instances --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="next-steps"></a>Sonraki adımlar
Ölçek kümeleri için diğer yaygın görevler, [bir uygulamanın](virtual-machine-scale-sets-deploy-app.md)nasıl dağıtılanmasını ve [VM örneklerini yükseltmeyi](virtual-machine-scale-sets-upgrade-scale-set.md)içerir. [Otomatik ölçek kurallarını yapılandırmak](virtual-machine-scale-sets-autoscale-overview.md)için Azure CLI'yi de kullanabilirsiniz.
