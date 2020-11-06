---
title: Azure Güvenlik duvarı DNS ayarları (Önizleme)
description: Azure Güvenlik Duvarı 'Nı DNS sunucusu ve DNS proxy ayarları ile yapılandırabilirsiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: efe608437f350a29147cf10989cdb6c17a23196d
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93398003"
---
# <a name="azure-firewall-dns-settings-preview"></a>Azure Güvenlik duvarı DNS ayarları (Önizleme)

> [!IMPORTANT]
> Azure Güvenlik duvarı DNS ayarları şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Özel bir DNS sunucusu yapılandırabilir ve Azure Güvenlik Duvarı için DNS proxy 'yi etkinleştirebilirsiniz. Bu ayarları, **DNS ayarları** sayfasından güvenlik duvarını veya daha yenisini dağıtırken yapılandırabilirsiniz.

## <a name="dns-servers"></a>DNS sunucuları

DNS sunucusu, etki alanı adlarını IP adreslerine tutar ve çözümler. Azure Güvenlik Duvarı, varsayılan olarak ad çözümlemesi için Azure DNS kullanır. **DNS sunucusu** ayarı, kendi DNS sunucularınızı Azure Güvenlik Duvarı ad çözümlemesi için yapılandırmanızı sağlar. Tek veya birden çok sunucu yapılandırabilirsiniz.

> [!NOTE]
> Azure Güvenlik Duvarı Yöneticisi kullanılarak yönetilen Azure Güvenlik duvarları için DNS ayarları, ilişkili Azure Güvenlik Duvarı Ilkesinde yapılandırılır.

### <a name="configure-custom-dns-servers-preview---azure-portal"></a>Özel DNS sunucularını yapılandırma (Önizleme)-Azure portalı

1. Azure Güvenlik Duvarı **ayarları** altında **DNS ayarları** ' nı seçin.
2. **DNS sunucuları** altında, sanal ağınızda daha önce BELIRTILEN mevcut DNS sunucularını yazabilir veya ekleyebilirsiniz.
3. **Kaydet** ’i seçin.
4. Güvenlik duvarı artık, DNS trafiğini ad çözümlemesi için belirtilen DNS sunucusuna yönlendirir.

:::image type="content" source="media/dns-settings/dns-servers.png" alt-text="DNS sunucuları":::

### <a name="configure-custom-dns-servers-preview---azure-cli"></a>Özel DNS sunucularını yapılandırma (Önizleme)-Azure CLı

Aşağıdaki örnek Azure CLı kullanarak özel DNS sunucularıyla Azure Güvenlik Duvarı 'nı güncelleştirir.

```azurecli-interactive
az network firewall update \
    --name fwName \ 
    --resource-group fwRG \
    --dns-servers 10.1.0.4 10.1.0.5
```

> [!IMPORTANT]
> Komut `az network firewall` Için Azure CLI uzantısının `azure-firewall` yüklü olması gerekir. Komutu kullanılarak yüklenebilir `az extension add --name azure-firewall` . 

### <a name="configure-custom-dns-servers-preview---azure-powershell"></a>Özel DNS sunucularını yapılandırma (Önizleme)-Azure PowerShell

Aşağıdaki örnek, Azure PowerShell kullanarak özel DNS sunucularıyla Azure Güvenlik Duvarı 'nı güncelleştirir.

```azurepowershell
$dnsServers = @("10.1.0.4", "10.1.0.5")
$azFw = Get-AzFirewall -Name "fwName" -ResourceGroupName "fwRG"
$azFw.DNSServer = $dnsServers

$azFw | Set-AzFirewall
```

## <a name="dns-proxy-preview"></a>DNS proxy (Önizleme)

Azure Güvenlik duvarını, DNS proxy 'si olarak davranacak şekilde yapılandırabilirsiniz. DNS proxy 'si, istemci sanal makinelerinden bir DNS sunucusuna DNS istekleri için bir ara sunucu görevi görür. Özel bir DNS sunucusu yapılandırırsanız DNS çözümleme uyuşmazlığını önlemek için DNS proxy 'yi etkinleştirmeniz ve ağ kurallarında FQDN filtrelemeyi etkinleştirmeniz gerekir.

DNS proxy 'yi etkinleştirmezseniz, istemciden gelen DNS istekleri farklı bir zamanda bir DNS sunucusuna seyahat edebilir veya güvenlik duvarından göre farklı bir yanıt döndürebilir. DNS proxy, tutarsızlığın önüne geçmek için Azure Güvenlik duvarını istemci isteklerinin yoluna koyar.

DNS proxy yapılandırması üç adım gerektirir:
1. Azure Güvenlik duvarı DNS ayarları 'nda DNS proxy 'yi etkinleştirin.
2. İsteğe bağlı olarak özel DNS sunucunuzu yapılandırın veya belirtilen varsayılanı kullanın.
3. Son olarak, Azure Güvenlik duvarının özel IP adresini, sanal ağ DNS sunucusu ayarlarınızda özel bir DNS adresi olarak yapılandırmanız gerekir. Bu, DNS trafiğinin Azure Güvenlik Duvarı 'na yönlendirilmesini sağlar.

### <a name="configure-dns-proxy-preview---azure-portal"></a>DNS proxy 'yi yapılandırma (Önizleme)-Azure portalı

DNS proxy 'yi yapılandırmak için, sanal ağ DNS sunucularınız ayarınızı güvenlik duvarı özel IP adresini kullanacak şekilde yapılandırmanız gerekir. Ardından, Azure Güvenlik Duvarı **DNS ayarları** 'Nda DNS proxy 'yi etkinleştirin.

#### <a name="configure-virtual-network-dns-servers"></a>Sanal ağ DNS sunucularını yapılandırma 

1. DNS trafiğinin Azure Güvenlik Duvarı üzerinden yönlendirileceği sanal ağı seçin.
2. **Ayarlar** altında, **DNS sunucuları** ' nı seçin.
3. **DNS sunucuları** altında **özel** ' i seçin.
4. Güvenlik duvarının özel IP adresini girin.
5. **Kaydet** ’i seçin.
6. Sanal ağa bağlı olan VM 'Leri yeniden başlatarak yeni DNS sunucusu ayarları atanır. VM 'Ler yeniden başlatılana kadar geçerli DNS ayarlarını kullanmaya devam eder.

#### <a name="enable-dns-proxy-preview"></a>DNS proxy 'yi etkinleştir (Önizleme)

1. Azure Güvenlik duvarınızı seçin.
2. **Ayarlar** altında **DNS ayarları** ' nı seçin.
3. **DNS proxy** varsayılan olarak devre dışıdır. Etkinleştirildiğinde güvenlik duvarı, 53 numaralı bağlantı noktasını dinler ve DNS isteklerini yapılandırılmış DNS sunucularına iletir.
4. Ayarların ortamınıza uygun olduğundan emin olmak için **DNS sunucularının** yapılandırmasını gözden geçirin.
5. **Kaydet** ’i seçin.

:::image type="content" source="media/dns-settings/dns-proxy.png" alt-text="DNS proxy 'si":::

### <a name="configure-dns-proxy-preview---azure-cli"></a>DNS proxy 'yi yapılandırma (Önizleme)-Azure CLı

Azure Güvenlik duvarında DNS proxy ayarlarını yapılandırma ve sanal ağları Azure Güvenlik Duvarı kullanacak şekilde güncelleştirme, DNS sunucusu Azure CLı kullanılarak yapılabilir.

#### <a name="configure-virtual-network-dns-servers"></a>Sanal ağ DNS sunucularını yapılandırma

Bu örnek, VNet 'i Azure Güvenlik Duvarı 'nı DNS sunucusu olarak kullanacak şekilde yapılandırır.
 
```azurecli-interactive
az network vnet update \
    --name VNetName \ 
    --resource-group VNetRG \
    --dns-servers <firewall-private-IP>
```

#### <a name="enable-dns-proxy-preview"></a>DNS proxy 'yi etkinleştir (Önizleme)

Bu örnek, Azure Güvenlik duvarında DNS proxy özelliğini sunar.

```azurecli-interactive
az network firewall update \
    --name fwName \ 
    --resource-group fwRG \
    --enable-dns-proxy true
```

### <a name="configure-dns-proxy-preview---azure-powershell"></a>DNS proxy 'yi (Önizleme) Yapılandırma-Azure PowerShell

DNS sunucusu, Azure PowerShell kullanılarak Azure Güvenlik Duvarı 'nı kullanmak için DNS proxy ayarlarını yapılandırma ve sanal ağları güncelleştirme.

#### <a name="configure-virtual-network-dns-servers"></a>Sanal ağ DNS sunucularını yapılandırma

 Bu örnek, VNet 'i Azure Güvenlik Duvarı 'nı DNS sunucusu olarak kullanacak şekilde yapılandırır.

```azurepowershell
$dnsServers = @("<firewall-private-IP>")
$VNet = Get-AzVirtualNetwork -Name "VNetName" -ResourceGroupName "VNetRG"
$VNet.DhcpOptions.DnsServers = $dnsServers

$VNet | Set-AzVirtualNetwork
```

#### <a name="enable-dns-proxy-preview"></a>DNS proxy 'yi etkinleştir (Önizleme)

Bu örnek, Azure Güvenlik duvarında DNS proxy özelliğini sunar.

```azurepowershell
$azFw = Get-AzFirewall -Name "fwName" -ResourceGroupName "fwRG"
$azFw.DNSEnableProxy = $true

$azFw | Set-AzFirewall
```

## <a name="next-steps"></a>Sonraki adımlar

[Ağ kurallarında FQDN filtrelemesi](fqdn-filtering-network-rules.md)
