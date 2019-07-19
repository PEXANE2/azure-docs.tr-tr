---
title: Bulut hizmeti için birden çok VIP
titlesuffix: Azure Load Balancer
description: Çoklu VIP 'ye genel bakış ve bir bulut hizmetinde birden çok VIP ayarlama
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: allensu
ms.openlocfilehash: 3e97bea85d4d97b159168b21b4a6e932e655ccfb
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68274699"
---
# <a name="configure-multiple-vips-for-a-cloud-service"></a>Bulut hizmeti için birden çok VIP yapılandırma

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Azure bulut hizmetlerine, Azure tarafından sunulan bir IP adresi kullanarak genel Internet üzerinden erişebilirsiniz. Bu genel IP adresi, bulut hizmetindeki sanal makine (VM) örnekleri değil, Azure yük dengeleyiciye bağlı olduğundan VIP (sanal IP) olarak adlandırılır. Bir bulut hizmeti içindeki herhangi bir sanal makine örneğine tek bir VIP kullanarak erişebilirsiniz.

Ancak, aynı bulut hizmetine giriş noktası olarak birden fazla VIP gereksinimi olabilecek senaryolar vardır. Örneğin, bulut hizmetiniz, her site farklı bir müşteri veya kiracı için barındırıldığından, varsayılan 443 bağlantı noktasını kullanarak SSL bağlantısı gerektiren birden çok Web sitesini barındırabilir. Bu senaryoda, her Web sitesi için bir genel kullanıma yönelik IP adresine sahip olmanız gerekir. Aşağıdaki diyagramda, aynı genel bağlantı noktasında birden çok SSL sertifikası için ihtiyacı olan tipik bir çok kiracılı web barındırma görülmektedir.

![Çoklu VIP SSL senaryosu](./media/load-balancer-multivip/Figure1.png)

Yukarıdaki örnekte, tüm VIP 'Ler aynı ortak bağlantı noktasını (443) kullanır ve trafik, tüm Web sitelerini barındıran bulut hizmetinin iç IP adresi için benzersiz bir özel bağlantı noktasındaki bir veya daha fazla yük dengeli VM 'ye yönlendirilir.

> [!NOTE]
> Birden çok VIP kullanılması gereken başka bir durum da aynı sanal makine kümesinde birden çok SQL AlwaysOn kullanılabilirlik grubu dinleyicisi barındırıyor.

VIP 'ler varsayılan olarak dinamiktir. Bu, bulut hizmetine atanan gerçek IP adresinin zamanla değiştirebileceği anlamına gelir. Bunun gerçekleşmesini engellemek için hizmetiniz için bir VIP ayırabilirsiniz. Ayrılmış VIP 'ler hakkında daha fazla bilgi için bkz. [ayrılmış genel IP](../virtual-network/virtual-networks-reserved-public-ip.md).

> [!NOTE]
> VIP 'Ler ve ayrılmış IP 'lerde fiyatlandırma hakkında bilgi için lütfen [IP adresi fiyatlandırmasına](https://azure.microsoft.com/pricing/details/ip-addresses/) bakın.

Cloud Services tarafından kullanılan VIP 'leri doğrulamak, VIP 'leri eklemek ve kaldırmak, VIP 'yi bir uç noktayla ilişkilendirmek ve belirli bir VIP üzerinde yük dengelemeyi yapılandırmak için PowerShell 'i kullanabilirsiniz.

## <a name="limitations"></a>Sınırlamalar

Şu anda, çoklu VIP işlevselliği aşağıdaki senaryolarla sınırlandırılmıştır:

* **Yalnızca IaaS**. VM 'Leri içeren bulut hizmetleri için yalnızca çoklu VIP 'yi etkinleştirebilirsiniz. PaaS senaryolarında rol örnekleriyle çoklu VIP kullanamazsınız.
* **Yalnızca PowerShell**. Yalnızca PowerShell kullanarak birden çok VIP yönetebilirsiniz.

Bu sınırlamalar geçicidir ve herhangi bir zamanda değişebilir. Gelecekteki değişiklikleri doğrulamak için bu sayfayı yeniden ziyaret ettiğinizden emin olun.

## <a name="how-to-add-a-vip-to-a-cloud-service"></a>Bulut hizmetine VIP ekleme
Hizmetinize bir VIP eklemek için aşağıdaki PowerShell komutunu çalıştırın:

```powershell
Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

Bu komut, aşağıdaki örneğe benzer bir sonuç görüntüler:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-to-remove-a-vip-from-a-cloud-service"></a>Bulut hizmetinden VIP kaldırma
Yukarıdaki örnekte hizmetinize eklenen VIP 'yi kaldırmak için aşağıdaki PowerShell komutunu çalıştırın:

```powershell
Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

> [!IMPORTANT]
> Bir VIP 'yi yalnızca kendisiyle ilişkili bir uç nokta yoksa kaldırabilirsiniz.


## <a name="how-to-retrieve-vip-information-from-a-cloud-service"></a>Bulut hizmetinden VIP bilgileri alma
Bir bulut hizmeti ile ilişkili VIP 'leri almak için aşağıdaki PowerShell betiğini çalıştırın:

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

Betik, aşağıdaki örneğe benzer bir sonuç görüntüler:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

Bu örnekte, bulut hizmetinde 3 VIP vardır:

* Varsayılan VIP olan **VIP1** , ıdnsprogrammedname değeri true olarak ayarlandığından emin olmalısınız.
* **Vip2** ve **VIP3** , hiçbir IP adresi olmadığı için kullanılmaz. Yalnızca bir uç noktayı VIP ile ilişkilendirdiğinizde bu bunlar kullanılacaktır.

> [!NOTE]
> Aboneliğiniz, bir uç nokta ile ilişkilendirildikten sonra yalnızca ek VIP 'ler için ücretlendirilir. Fiyatlandırma hakkında daha fazla bilgi için bkz. [IP adresi fiyatlandırması](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="how-to-associate-a-vip-to-an-endpoint"></a>Bir VIP 'yi bir uç noktayla ilişkilendirme

Bulut hizmetindeki bir VIP 'yi bir uç noktayla ilişkilendirmek için aşağıdaki PowerShell komutunu çalıştırın:

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 |
    Update-AzureVM
```

Bu komut, *80*numaralı bağlantı noktasında *Vip2* adlı VIP 'ye bağlı bir uç nokta oluşturur ve bu bağlantıyı, numaralı bağlantı *8080*noktasında *TCP* kullanarak *hizmetim* adlı bir bulut hizmetinde *myVM1* adlı VM 'ye bağlar.

Yapılandırmayı doğrulamak için aşağıdaki PowerShell komutunu çalıştırın:

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

Çıktı aşağıdaki örneğe benzer şekilde görünür:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         : 191.238.74.13
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

## <a name="how-to-enable-load-balancing-on-a-specific-vip"></a>Belirli bir VIP üzerinde yük dengelemeyi etkinleştirme

Yük Dengeleme amacıyla, birden çok sanal makineyle tek bir VIP 'yi ilişkilendirebilirsiniz. Örneğin, *hizmetim*adlı bir bulut hizmetiniz ve *myVM1* ve *myVM2*adlı iki sanal makine vardır. Bulut hizmetinizde, biri *Vip2*adlı birden çok VIP vardır. *Vip2* üzerinde bağlantı noktası *81* ' e giden tüm trafiğin, bağlantı noktası *8181*' de *myVM1* ve *myVM2* arasında dengelenmesi sağlamak istiyorsanız, aşağıdaki PowerShell betiğini çalıştırın:

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2 -DefaultProbe |
    Update-AzureVM

Get-AzureVM -ServiceName myService -Name myVM2 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe |
    Update-AzureVM
```

Ayrıca, yük dengeleyiciyi farklı bir VIP kullanmak üzere güncelleştirebilirsiniz. Örneğin, aşağıdaki PowerShell komutunu çalıştırırsanız, yük dengeleme kümesini Vıp1 adlı bir VIP kullanacak şekilde değiştirirsiniz:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1
```

## <a name="next-steps"></a>Sonraki Adımlar

[Azure Yük Dengelemesi için Azure Izleyici günlükleri](load-balancer-monitor-log.md)

[Internet 'e yönelik yük dengeleyiciye genel bakış](load-balancer-internet-overview.md)

[Internet 'e yönelik yük dengeleyiciye başlayın](load-balancer-get-started-internet-arm-ps.md)

[Sanal Ağ’a Genel Bakış](../virtual-network/virtual-networks-overview.md)

[REST API 'Leri Ayrılmış IP](https://msdn.microsoft.com/library/azure/dn722420.aspx)
