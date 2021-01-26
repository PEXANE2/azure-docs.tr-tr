---
title: Sanal Makine Ölçek Kümesi tarafından kullanılan mevcut Azure Load Balancer’ı güncelleştirme veya silme
titleSuffix: Update or delete existing Azure Load Balancer used by Virtual Machine Scale Set
description: Bu nasıl yapılır makalesi sayesinde Azure Standart Load Balancer ve sanal makine ölçek kümelerini kullanmaya başlayın.
services: load-balancer
documentationcenter: na
author: irenehua
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/30/2020
ms.author: irenehua
ms.openlocfilehash: d5614490bfd2cfb67b6b7afd7b7b8643bbf754bd
ms.sourcegitcommit: a055089dd6195fde2555b27a84ae052b668a18c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98790098"
---
# <a name="how-to-updatedelete-azure-load-balancer-used-by-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri tarafından kullanılan Azure Load Balancer güncelleştirme/silme

## <a name="how-to-set-up-azure-load-balancer-for-scaling-out-virtual-machine-scale-sets"></a>Sanal makine ölçek kümelerinin ölçeğini genişletmek için Azure Load Balancer ayarlama
  * Load Balancer [gelen NAT havuzu](/cli/azure/network/lb/inbound-nat-pool?view=azure-cli-latest) ayarlanmış olduğundan ve sanal makine ölçek kümesinin Load Balancer arka uç havuzunda bulunduğundan emin olun. Azure Load Balancer, sanal makine ölçek kümesine yeni sanal makine örnekleri eklendiğinde, gelen NAT havuzunda otomatik olarak yeni gelen NAT kuralları oluşturur. 
  * Gelen NAT havuzunun düzgün ayarlanıp ayarlanmadığını denetlemek için 
  1. https://portal.azure.com adresinden Azure portalında oturum açın.
  
  1. Sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından kaynak listesinden **myloadbalancer** ' yi seçin.
  
  1. **Ayarlar** altında **gelen NAT kuralları**' nı seçin.
Sağ bölmede, sanal makine ölçek kümesindeki her bir örnek için oluşturulan kuralların bir listesi olan bir liste varsa, hepsi ölçeği istediğiniz zaman yukarı doğru olacak şekilde ayarlamış olursunuz.

## <a name="how-to-add-inbound-nat-rules"></a>Gelen NAT kuralları nasıl eklenir? 
  * Tek bir gelen NAT kuralı eklenemez. Ancak, sanal makine ölçek kümesindeki tüm örnekler için tanımlı ön uç bağlantı noktası aralığı ve arka uç bağlantı noktası içeren bir dizi gelen NAT kuralı ekleyebilirsiniz.
  * Sanal Makine Ölçek Kümeleri için bir bütün gelen NAT kuralı kümesini eklemek için, önce Load Balancer bir gelen NAT havuzu oluşturmanız ve ardından sanal makine ölçek kümesinin ağ profilinden gelen NAT havuzuna başvurmanız gerekir. CLı kullanan tam bir örnek aşağıda gösterilmiştir.
  * Yeni gelen NAT havuzu, mevcut gelen NAT havuzlarıyla çakışan ön uç bağlantı noktası aralığına sahip olmamalıdır. Ayarlanmış olan gelen NAT havuzlarını görüntülemek için bu [CLI komutunu](/cli/azure/network/lb/inbound-nat-pool?view=azure-cli-latest#az_network_lb_inbound_nat_pool_list) kullanabilirsiniz
```azurecli-interactive
az network lb inbound-nat-pool create 
        -g MyResourceGroup 
        --lb-name MyLb
        -n MyNatPool 
        --protocol Tcp 
        --frontend-port-range-start 80 
        --frontend-port-range-end 89 
        --backend-port 80 
        --frontend-ip-name MyFrontendIp
az vmss update 
        -g MyResourceGroup 
        -n myVMSS 
        --add virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerInboundNatPools "{'id':'/subscriptions/mySubscriptionId/resourceGroups/MyResourceGroup/providers/Microsoft.Network/loadBalancers/MyLb/inboundNatPools/MyNatPool'}"
        
az vmss update-instances
        -–instance-ids *
        --resource-group MyResourceGroup
        --name MyVMSS
```
## <a name="how-to-update-inbound-nat-rules"></a>Gelen NAT kuralları nasıl güncelleştirilecek? 
  * Tek bir gelen NAT kuralı güncelleştirilemez. Ancak, sanal makine ölçek kümesindeki tüm örnekler için tanımlı ön uç bağlantı noktası aralığı ve arka uç bağlantı noktası içeren bir gelen NAT kuralları kümesini güncelleştirebilirsiniz.
  * Sanal Makine Ölçek Kümeleri için bir dizi gelen NAT kuralını güncelleştirmek üzere Load Balancer gelen NAT havuzunu güncelleştirmeniz gerekir. 
```azurecli-interactive
az network lb inbound-nat-pool update 
        -g MyResourceGroup 
        --lb-name MyLb 
        -n MyNatPool
        --protocol Tcp 
        --backend-port 8080
```

## <a name="how-to-delete-inbound-nat-rules"></a>Gelen NAT kuralları nasıl silinir? 
* Tek bir gelen NAT kuralı silinemez. Ancak, gelen NAT kuralları kümesinin tamamını silebilirsiniz.
* Ölçek kümesi tarafından kullanılan tüm gelen NAT kuralları kümesini silmek için önce ölçek kümesinden NAT havuzunu kaldırmanız gerekir. CLı kullanan tam bir örnek aşağıda gösterilmiştir:
```azurecli-interactive
  az vmss update
     --resource-group MyResourceGroup
     --name MyVMSS
   az vmss update-instances 
     --instance-ids "*" 
     --resource-group MyResourceGroup
     --name MyVMSS
  az network lb inbound-nat-pool delete
     --resource-group MyResourceGroup
     -–lb-name MyLoadBalancer
     --name MyNatPool
```

## <a name="how-to-add-multiple-ip-configurations"></a>Birden çok IP yapılandırması ekleme:
1. Sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından kaynak listesinden **myloadbalancer** ' yi seçin.
   
1. **Ayarlar** altında **ön uç IP yapılandırması**' nı seçin ve ardından **Ekle**' yi seçin.
   
1. **Ön uç IP adresi ekle** sayfasında, değerleri yazın ve **Tamam** ' ı seçin.

1. Yeni Yük Dengeleme kuralları gerekiyorsa, bu öğreticide [5](./load-balancer-multiple-ip.md#step-5-configure-the-health-probe) . ve [6](./load-balancer-multiple-ip.md#step-5-configure-the-health-probe) . adım ' a uyun

1. Gerekirse yeni oluşturulan ön uç IP yapılandırmasını kullanarak yeni bir gelen NAT kuralları kümesi oluşturun. Örnek burada [önceki bölümde] bulunabilir.

## <a name="how-to-delete-frontend-ip-configuration-used-by-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi tarafından kullanılan ön uç IP yapılandırması nasıl silinir: 
 1. Ölçek kümesi tarafından kullanılan ön uç IP yapılandırmasını silmek için, önce ön uç IP yapılandırmasına başvuran gelen NAT havuzunu (gelen NAT kuralları kümesi) silmeniz gerekir. Gelen kuralların nasıl silineceği hakkındaki yönergeler, önceki bölümde bulunabilir.
 1. Ön uç IP yapılandırmasına başvuran Yük Dengeleme kuralını silin. 
 1. Ön uç IP yapılandırmasını silin.
 

## <a name="how-to-delete-azure-load-balancer-used-by-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi tarafından kullanılan Azure Load Balancer silme: 
 1. Ölçek kümesi tarafından kullanılan ön uç IP yapılandırmasını silmek için, önce ön uç IP yapılandırmasına başvuran gelen NAT havuzunu (gelen NAT kuralları kümesi) silmeniz gerekir. Gelen kuralların nasıl silineceği hakkındaki yönergeler, önceki bölümde bulunabilir.
 
 1. Sanal makine ölçek kümesini içeren arka uç havuzuna başvuran Yük Dengeleme kuralını silin.
 
 1. Loadbalancerbackendadddresspool başvurusunu, sanal makine ölçek kümesinin ağ profilinden kaldırın. CLı kullanan tam bir örnek aşağıda gösterilmiştir:
 ```azurecli-interactive
  az vmss update
     --resource-group MyResourceGroup
     --name MyVMSS
     --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerBackendAddressPools
  az vmss update-instances 
     --instance-ids "*" 
     --resource-group MyResourceGroup
     --name MyVMSS
```
Son olarak Load Balancer kaynağını silin.
 
## <a name="next-steps"></a>Sonraki adımlar

Azure Load Balancer ve sanal makine ölçek kümesi hakkında daha fazla bilgi edinmek için kavramlar hakkında daha fazla bilgi edinin.

> [Azure sanal makine ölçek kümeleri ile Azure Load Balancer](load-balancer-standard-virtual-machine-scale-sets.md)