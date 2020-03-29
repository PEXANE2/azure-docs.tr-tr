---
title: Azure sanal makine ölçek kümeleriyle özel ölçeklendirme ilkeleri kullanma
description: Örnek sayısını yönetmek için otomatik ölçek yapılandırması kullanan Azure sanal makine ölçek kümeleriyle özel ölçeklendirme ilkelerini nasıl kullanacağınızı öğrenin
services: virtual-machine-scale-sets
author: avirishuv
manager: vashan
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.topic: conceptual
ms.date: 02/26/2020
ms.author: avverma
ms.openlocfilehash: ffcdaf76bdd08ee5505ddbeff6a6698e231b6171
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77919847"
---
# <a name="use-custom-scale-in-policies-with-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümeleriyle özel ölçeklendirme ilkeleri kullanma

Sanal makine ölçeği kümesi dağıtımı, platform ve kullanıcı tanımlı özel ölçümler de dahil olmak üzere bir dizi ölçüme göre ölçeklenebilir veya ölçeklenebilir. Ölçeklendirme, ölçek kümesi modeline dayalı olarak yeni sanal makineler oluştururken, ölçeklendirme, ölçek kümesi iş yükü geliştikçe farklı yapılandırmalara ve/veya işlevlerine sahip olabilecek çalışan sanal makineleri etkiler. 

Ölçeklendirme ilkesi özelliği, kullanıcılara sanal makinelerin üç ölçekli yapılandırmayla ölçeklendirildiği sırayı yapılandırmaları için bir yol sağlar: 

1. Varsayılan
2. En YeniVM
3. En EskiVM

### <a name="default-scale-in-policy"></a>Varsayılan ölçek-in ilkesi

Varsayılan olarak, sanal makine ölçeği kümesi, hangi örnek(ler) ölçeklendirilecek belirlemek için bu ilkeyi uygular. *Varsayılan* ilke ile, VM'ler aşağıdaki sırada ölçeklendirme için seçilir:

1. Sanal makineleri kullanılabilirlik bölgeleri arasında dengeleyin (ölçek kümesi bölge yapılandırmasında dağıtılırsa)
2. Sanal makineleri hata etki alanları arasında dengeleme (en iyi çaba)
3. En yüksek örnek kimliğiyle sanal makineyi silme

Kullanıcılar, varsayılan sıralamanın izlenmesini istiyorlarsa, bir ölçeklendirme ilkesi belirtmeye gerek yoktur.

Kullanılabilirlik bölgeleri veya hata etki alanları arasında dengelemenin örnekleri kullanılabilirlik bölgeleri veya hata etki alanları arasında taşımadığını unutmayın. Dengeleme, sanal makinelerin dağıtımı dengeli hale gelene kadar sanal makinelerin dengesiz kullanılabilirlik bölgelerinden veya fay etki alanlarından silinmesi yoluyla elde edilir.

### <a name="newestvm-scale-in-policy"></a>NewestVM ölçeklendirme ilkesi

Bu ilke, kullanılabilirlik bölgeleri arasında (bölge dağıtımları için) VM'leri dengeledikten sonra ölçek kümesinde oluşturulan en yeni sanal makineyi siler. Bu ilkeyi etkinleştirmek, sanal makine ölçeği kümesi modelinde bir yapılandırma değişikliği gerektirir.

### <a name="oldestvm-scale-in-policy"></a>En EskiVM ölçeklendirme ilkesi

Bu ilke, kullanılabilirlik bölgeleri arasında (bölge dağıtımları için) VM'leri dengeledikten sonra ölçek kümesinde oluşturulan en eski sanal makineyi siler. Bu ilkeyi etkinleştirmek, sanal makine ölçeği kümesi modelinde bir yapılandırma değişikliği gerektirir.

## <a name="enabling-scale-in-policy"></a>Ölçeklendirme ilkesini etkinleştirme

Sanal makine ölçeği kümesi modelinde ölçeklendirme ilkesi tanımlanır. Yukarıdaki bölümlerde belirtildiği gibi, 'En YeniVM' ve 'OldestVM' ilkeleri kullanırken bir ölçek-in ilke tanımı gereklidir. Sanal makine ölçeği kümesi, ölçek kümesi modelinde bulunan ölçeklendirme ilkesi yoksa otomatik olarak 'Varsayılan' ölçeklendirme ilkesini kullanır. 

Sanal makine ölçeği kümesi modelinde ölçeklendirme ilkesi aşağıdaki şekillerde tanımlanabilir:

### <a name="azure-portal"></a>Azure portalında
 
Aşağıdaki adımlar, yeni bir ölçek kümesi oluştururken ölçek-in ilkesini tanımlar. 
 
1. Sanal **makine ölçek kümelerine**gidin.
1. Yeni bir ölçek kümesi oluşturmak için **+ Ekle'yi** seçin.
1. **Ölçekleme** sekmesine gidin. 
1. **Ölçek-in ilkesi** bölümünü bulun.
1. Açılan politikadan bir ölçeklendirme ilkesi seçin.
1. Yeni ölçek kümesini oluşturmayı **bitirdiğinizde, Gözden Geçir + oluştur** düğmesini seçin.

### <a name="using-api"></a>API'yi kullanma

API 2019-03-01 kullanarak sanal makine ölçeğinde bir PUT çalıştırın:

```
PUT
https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<myRG>/providers/Microsoft.Compute/virtualMachineScaleSets/<myVMSS>?api-version=2019-03-01

{ 
"location": "<VMSS location>", 
    "properties": { 
        "scaleInPolicy": {  
            "rules": ["OldestVM"]  
        } 
    }    
} 
```
### <a name="azure-powershell"></a>Azure PowerShell

Bir kaynak grubu oluşturun, ardından en *eski VM*olarak ölçeklendirme ilkesi yle yeni bir ölçek kümesi oluşturun.

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName "myResourceGroup" -Location "<VMSS location>"
New-AzVmss `
  -ResourceGroupName "myResourceGroup" `
  -Location "<VMSS location>" `
  -VMScaleSetName "myScaleSet" `
  -ScaleInPolicy “OldestVM”
```

### <a name="azure-cli-20"></a>Azure CLI 2.0

Aşağıdaki örnek, yeni bir ölçek kümesi oluştururken bir ölçek-in ilkesi ekler. Önce bir kaynak grubu oluşturun, sonra ölçeklendirme ilkesine sahip yeni bir ölçek kümesi *oluşturun.* 

```azurecli-interactive
az group create --name <myResourceGroup> --location <VMSSLocation>
az vmss create \
  --resource-group <myResourceGroup> \
  --name <myVMScaleSet> \
  --image UbuntuLTS \
  --admin-username <azureuser> \
  --generate-ssh-keys \
  --scale-in-policy OldestVM
```

### <a name="using-template"></a>Şablonu Kullanma

Şablonunuzda, "özellikler" altında aşağıdakileri ekleyin:

```json
"scaleInPolicy": {  
      "rules": ["OldestVM"]  
}
```

Yukarıdaki bloklar, sanal makine ölçeği kümesinin, bir ölçek-in tetiklendiğinde (Otomatik ölçekveya manuel silme yoluyla) bölge dengeli ölçek kümesindeki En Eski VM'yi siler.

Sanal makine ölçeği kümesi bölge dengelenmiyorsa, ölçek kümesi önce dengesiz bölge(ler) boyunca VM'leri siler. Dengesiz bölgeler içinde, ölçek kümesi hangi VM'de ölçeklendirileceklerini belirlemek için yukarıda belirtilen ölçeklendirme ilkesini kullanır. Bu durumda, dengesiz bir bölge içinde, ölçek kümesi silinecek o bölgede en eski VM'yi seçer.

Zonal olmayan sanal makine ölçeği kümesi için, ilke silme için ayarlanan ölçek teki en eski VM'yi seçer.

Yukarıdaki ölçeklendirme ilkesinde 'NewestVM' kullanılarak da aynı işlem uygulanır.

## <a name="modifying-scale-in-policies"></a>Ölçeklendirme ilkelerini değiştirme

Ölçek-in ilkesini değiştirmek, ölçeklendirme ilkesini uygulamakla aynı işlemi izler. Örneğin, yukarıdaki örnekte, ilkeyi 'OldestVM'den 'NewestVM' olarak değiştirmek istiyorsanız, bunu şunları yapabilirsiniz:

### <a name="azure-portal"></a>Azure portalında

Azure portalı üzerinden ayarlanmış varolan bir ölçek kümesinin ölçeklendirme ilkesini değiştirebilirsiniz. 
 
1. Varolan sanal makine ölçeği kümesinde, soldaki menüden **Ölçekleme'yi** seçin.
1. **Ölçek-In İlkesi** sekmesini seçin.
1. Açılan politikadan bir ölçeklendirme ilkesi seçin.
1. İşiniz bittiğinde **Kaydet**‘i seçin. 

### <a name="using-api"></a>API'yi kullanma

API 2019-03-01 kullanarak sanal makine ölçeğinde bir PUT çalıştırın:

```
PUT
https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<myRG>/providers/Microsoft.Compute/virtualMachineScaleSets/<myVMSS>?api-version=2019-03-01 

{ 
"location": "<VMSS location>", 
    "properties": { 
        "scaleInPolicy": {  
            "rules": ["NewestVM"]  
        } 
    }    
}
```
### <a name="azure-powershell"></a>Azure PowerShell

Varolan bir ölçek kümesinin ölçeklendirme ilkesini güncelleştirme:

```azurepowershell-interactive
Update-AzVmss `
 -ResourceGroupName "myResourceGroup" `
 -VMScaleSetName "myScaleSet" `
 -ScaleInPolicy “OldestVM”
```

### <a name="azure-cli-20"></a>Azure CLI 2.0

Varolan bir ölçek kümesinin ölçeklendirme ilkesini güncelleştirmek için aşağıdaki örnek: 

```azurecli-interactive
az vmss update \  
  --resource-group <myResourceGroup> \
  --name <myVMScaleSet> \
  --scale-in-policy OldestVM
```

### <a name="using-template"></a>Şablonu Kullanma

Şablonunuzda, "özellikler" altında şablonu aşağıdaki gibi değiştirin ve yeniden dağıtın: 

```json
"scaleInPolicy": {  
      "rules": ["NewestVM"]  
} 
```

'En YeniVM'i 'Varsayılan' veya 'OldestVM' olarak değiştirmeye karar verirseniz aynı işlem geçerli olacaktır.

## <a name="instance-protection-and-scale-in-policy"></a>Örnek koruma ve ölçeklendirme ilkesi

Sanal makine ölçek kümeleri iki tür [örnek koruma](./virtual-machine-scale-sets-instance-protection.md#types-of-instance-protection)sağlar:

1. Ölçeklendirmeden koruyun
2. Ölçek ayarlı eylemlerden koruyun

Korumalı sanal makine, uygulanan ölçeklendirme ilkesinden bağımsız olarak, ölçeklendirme eylemi yle silinmez. Örneğin, VM_0 (ölçek kümesindeki en eski VM) ölçek-in'e karşı korunuyorsa ve ölçek kümesi 'OldestVM' ölçeklendirme ilkesi etkinse, VM_0 ölçek kümesindeki en eski VM olmasına rağmen ölçeklendirildik olarak kabul edilmez. 

Korumalı bir sanal makine, ölçek kümesinde etkinleştirilen ölçeklendirme ilkesine bakılmaksızın, kullanıcı tarafından herhangi bir zamanda el ile silinebilir. 

## <a name="usage-examples"></a>Kullanım örnekleri 

Aşağıdaki örnekler, sanal makine ölçeği kümesinin bir ölçekolayı tetiklendiğinde silinecek VM'leri nasıl seçeceğini gösterir. En yüksek örnek iHd'lere sahip sanal makinelerin ölçek kümesindeki en yeni VM'ler olduğu varsayılır ve en küçük örnek iHd'lere sahip Sanal Ayarlar ölçek kümesindeki en eski VM'ler olarak kabul edilir. 

### <a name="oldestvm-scale-in-policy"></a>En EskiVM ölçeklendirme ilkesi

| Olay                 | Bölge1'deki Örnek Adlar  | Bölge2'deki Örnek Adlar  | Bölge3'teki Örnek Adlar  | Ölçeklendirme Seçimi                                                                                                               |
|-----------------------|------------------------|------------------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| Başlangıç               | 3, 4, 5, 10            | 2, 6, 9, 11            | 1, 7, 8                |                                                                                                                                  |
| Ölçek-in              | 3, 4, 5, 10            | ***2***, 6, 9, 11      | 1, 7, 8                | Bölge 3 en eski VM'ye sahip olsa bile, Bölge 1 ve 2 arasında seçim yapın. O bölgede ki en eski VM olduğu için Bölge 2'den VM2'yi silin.   |
| Ölçek-in              | ***3***, 4, 5, 10      | 6, 9, 11               | 1, 7, 8                | Bölge 3 en eski VM'ye sahip olsa bile Bölge 1'i seçin. O bölgede en eski VM olduğu için Bölge 1'den VM3'ü silin.                  |
| Ölçek-in              | 4, 5, 10               | 6, 9, 11               | ***1***, 7, 8          | Bölgeler dengelenir. Ölçek kümesindeki en eski VM olduğu için Bölge 3'teki VM1'i silin.                                               |
| Ölçek-in              | ***4***, 5, 10         | 6, 9, 11               | 7, 8                   | Bölge 1 ve Bölge 2 arasında seçim yapın. İki Bölge'deki en eski VM olduğu için Bölge 1'deki VM4'ü silin.                              |
| Ölçek-in              | 5, 10                  | ***6***, 9, 11         | 7, 8                   | Bölge 1 en eski VM'ye sahip olsa bile Bölge 2'yi seçin. Bölge 1'deki VM6'yı, o bölgenin en eski VM'si olduğu için silin.                    |
| Ölçek-in              | ***5***, 10            | 9, 11                  | 7, 8                   | Bölgeler dengelenir. Ölçek kümesindeki en eski VM olduğu için Bölge 1'deki VM5'i silin.                                                |

Zonal olmayan sanal makine ölçek kümeleri için, ilke silme için ayarlanan ölçek boyunca en eski VM'yi seçer. Herhangi bir "korumalı" VM silme için atlanır.

### <a name="newestvm-scale-in-policy"></a>NewestVM ölçeklendirme ilkesi

| Olay                 | Bölge1'deki Örnek Adlar  | Bölge2'deki Örnek Adlar  | Bölge3'teki Örnek Adlar  | Ölçeklendirme Seçimi                                                                                                               |
|-----------------------|------------------------|------------------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| Başlangıç               | 3, 4, 5, 10            | 2, 6, 9, 11            | 1, 7, 8                |                                                                                                                                  |
| Ölçek-in              | 3, 4, 5, 10            | 2, 6, 9, ***11***      | 1, 7, 8                | Bölge 1 ve 2 arasında seçim yapın. İki bölge deki en yeni VM olduğu için VM11'i Bölge 2'den silin.                                |
| Ölçek-in              | 3, 4, 5, ***10***      | 2, 6, 9                | 1, 7, 8                | Diğer iki bölgeden daha fazla VM'se sahip olduğu için Bölge 1'i seçin. O Bölge'deki en yeni VM olduğu için Bölge 1'den VM10'u silin.          |
| Ölçek-in              | 3, 4, 5                | 2, 6, ***9***          | 1, 7, 8                | Bölgeler dengelenir. Ölçek kümesindeki en yeni VM olduğu için Bölge 2'deki VM9'u silin.                                                |
| Ölçek-in              | 3, 4, 5                | 2, 6                   | 1, 7, ***8***          | Bölge 1 ve Bölge 3 arasında seçim yapın. O Bölge'deki en yeni VM olduğu için Bölge 3'teki VM8'i silin.                                      |
| Ölçek-in              | 3, 4, ***5***          | 2, 6                   | 1, 7                   | Bölge 3 en yeni VM'ye sahip olsa bile Bölge 1'i seçin. Bölge 1'deki VM5'i o Bölge'deki en yeni VM olduğu için silin.                    |
| Ölçek-in              | 3, 4                   | 2, 6                   | 1, ***7***             | Bölgeler dengelenir. Ölçek kümesindeki en yeni VM olduğu için Bölge 3'teki VM7'yi silin.                                                |

Zonal olmayan sanal makine ölçek kümeleri için, ilke silme için ayarlanan ölçek boyunca en yeni VM'yi seçer. Herhangi bir "korumalı" VM silme için atlanır. 

## <a name="troubleshoot"></a>Sorun giderme

1. Ölçek etkinleştirilmemesiInPolicy "'özellikler' türünesnesinde üye 'scaleInPolicy' bulamadım" şeklinde bir hata iletisi ile 'BadRequest' hatası alırsanız, sanal makine ölçeği kümesi için kullanılan API sürümünü kontrol edin. BU özellik için API sürümü 2019-03-01 veya daha yüksek olması gerekir.

2. Ölçek için Yanlış VM seçimi Yukarıdaki örneklere bakın. Sanal makine ölçek kümeniz bir Zonal dağıtımise, ölçeklendirme ilkesi önce dengesiz Bölgeler'e, sonra da bölge dengeli olduğunda ölçek kümesiboyunca uygulanır. Ölçeklendirme sırası yukarıdaki örneklerle tutarlı değilse, sorun giderme için sanal makine ölçeği kümesi ekibiyle bir sorgu yükseltin.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızı sanal makine ölçeği kümelerinde nasıl [dağıtılayacaklarınıöğrenin.](virtual-machine-scale-sets-deploy-app.md)