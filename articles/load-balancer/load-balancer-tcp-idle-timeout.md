---
title: Yük dengeleyici TCP sıfırlaması ve boşta kalma zaman aşımını yapılandırma
titleSuffix: Azure Load Balancer
description: Bu makalede, Azure Load Balancer TCP boşta kalma zaman aşımını yapılandırmayı ve sıfırlamayı öğrenin.
services: load-balancer
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/26/2020
ms.author: allensu
ms.openlocfilehash: 8a6be588544883b77c3ff115c9dba5e6ecd5fbd7
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92747223"
---
# <a name="configure-tcp-reset-and-idle-timeout-for-azure-load-balancer"></a>Azure Load Balancer için TCP sıfırlamayı ve boşta kalma zaman aşımını yapılandırma

Azure Load Balancer şu boşta zaman aşımı aralığına sahiptir:

* Giden kuralları için 4 dakika ila 100 dakika
* Load Balancer kuralları ve gelen NAT kuralları için 4 dakika ila 30 dakika

Varsayılan olarak 4 dakikaya ayarlanır. İşlem yapılmayan bir süre, zaman aşımı değerinden uzunsa, TCP veya HTTP oturumunun istemci ile hizmetiniz arasında korunduğundan emin olmaz. 

Aşağıdaki bölümlerde, yük dengeleyici kaynakları için boşta kalma zaman aşımı ve TCP sıfırlama ayarlarının nasıl değiştirileceği açıklanır.

## <a name="set-tcp-reset-and-idle-timeout"></a>TCP sıfırlama ve boşta kalma zaman aşımını ayarla
---
# <a name="portal"></a>[**Portal**](#tab/tcp-reset-idle-portal)

Boşta kalma zaman aşımını ve bir yük dengeleyici için TCP sıfırlamayı ayarlamak için, yük dengeli kuralı düzenleyin. 

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.

2. Sol taraftaki menüde **kaynak grupları** ' nı seçin.

3. Yük dengeleyiciniz için kaynak grubunu seçin. Bu örnekte kaynak grubu **Myresourcegroup** olarak adlandırılmıştır.

4. Yük dengeleyicinizi seçin. Bu örnekte, yük dengeleyici **Myloadbalancer** olarak adlandırılmıştır.

5. **Ayarlar** bölümünde **Yük Dengeleme kuralları** ' nı seçin.

     :::image type="content" source="./media/load-balancer-tcp-idle-timeout/portal-lb-rules.png" alt-text="Yük dengeleyici kurallarını düzenleyin." border="true":::

6. Yük Dengeleme kuralınızı seçin. Bu örnekte, Yük Dengeleme kuralı **Mylbrule** olarak adlandırılmıştır.

7. Yük Dengeleme kuralında, kaydırıcıyı **boşta zaman aşımı (dakika)** cinsinden zaman aşımı değerine taşıyın.  

8. **TCP sıfırlaması** altında **etkin** ' i seçin.

   :::image type="content" source="./media/load-balancer-tcp-idle-timeout/portal-lb-rules-tcp-reset.png" alt-text="Yük dengeleyici kurallarını düzenleyin." border="true":::

9. **Kaydet** ’i seçin.

# <a name="powershell"></a>[**PowerShell**](#tab/tcp-reset-idle-powershell)

Boşta kalma zaman aşımını ve TCP sıfırlamayı ayarlamak için, aşağıdaki yük dengeleme kuralı parametrelerinin değerlerini [set-AzLoadBalancer](/powershell/module/az.network/set-azloadbalancer)ile ayarlayın:

* **Idletimeoutınminutes**
* **Enabletcönayar**

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure PowerShell modülü 5.4.1 veya sonraki bir sürümünü gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell 'i yerel olarak çalıştırıyorsanız `Connect-AzAccount` Azure ile bir bağlantı oluşturmak için öğesini de çalıştırmanız gerekir.

Aşağıdaki örnekleri kaynaklarınızın değerleriyle değiştirin:

* **myResourceGroup**
* **myLoadBalancer**

```azurepowershell
$lb = Get-AzLoadBalancer -Name "myLoadBalancer" -ResourceGroup "myResourceGroup"
$lb.LoadBalancingRules[0].IdleTimeoutInMinutes = '15'
$lb.LoadBalancingRules[0].EnableTcpReset = 'true'
Set-AzLoadBalancer -LoadBalancer $lb
```

# <a name="azure-cli"></a>[**Azure CLI**](#tab/tcp-reset-idle-cli)

Boşta kalma zaman aşımını ve TCP sıfırlamayı ayarlamak için [az Network lb Rule Update](/cli/azure/network/lb/rule?az_network_lb_rule_update)için şu parametreleri kullanın:

* **--boşta-zaman aşımı**
* **--Enable-TCP-Reset**

Başlamadan önce ortamınızı doğrulayın:

* Azure portal oturum açın ve ' i çalıştırarak aboneliğinizin etkin olduğunu denetleyin `az login` .
* Çalıştıran bir Terminal veya komut penceresinde Azure CLı sürümünüzü denetleyin `az --version` . En son sürüm için bkz. [en son sürüm notları](/cli/azure/release-notes-azure-cli?tabs=azure-cli).
  * En son sürüme sahip değilseniz, [işletim sisteminiz veya platformunuzun yükleme kılavuzunu](/cli/azure/install-azure-cli)izleyerek yüklemenizi güncelleştirin.

Aşağıdaki örnekleri kaynaklarınızın değerleriyle değiştirin:

* **myResourceGroup**
* **myLoadBalancer**
* **myLBrule**


```azurecli
az network lb rule update \
    --resource-group myResourceGroup \
    --name myLBrule \
    --lb-name myLoadBalancer \
    --idle-timeout 15 \
    --enable-tcp-reset true
```
---
## <a name="next-steps"></a>Sonraki adımlar

TCP boşta kalma zaman aşımı ve sıfırlama hakkında daha fazla bilgi için bkz. [Load Balancer TCP sıfırlaması ve boşta zaman aşımı](load-balancer-tcp-reset.md)

Yük dengeleyici Dağıtım modunu yapılandırma hakkında daha fazla bilgi için bkz. [yük dengeleyici Dağıtım modunu yapılandırma](load-balancer-distribution-mode.md).
