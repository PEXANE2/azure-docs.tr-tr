---
title: Azure CLI'yi kullanarak Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma
description: Bu makalede, Azure CLI'yi kullanarak Azure Güvenlik Duvarı'nı nasıl dağıtabileceğinizi ve yapılandırabileceğinizi öğreneceksiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.date: 08/29/2019
ms.author: victorh
ms.topic: article
ms.openlocfilehash: e97783d1a32916cad151f1d0858a8190d0005fd0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73831977"
---
# <a name="deploy-and-configure-azure-firewall-using-azure-cli"></a>Azure CLI'yi kullanarak Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma

Giden ağ erişimini denetleme, genel ağ güvenlik planının önemli bir parçasıdır. Örneğin, web sitelerine erişimi sınırlamak isteyebilirsiniz. Veya, giden IP adreslerini ve erişilebilen bağlantı noktalarını sınırlamak isteyebilirsiniz.

Azure Güvenlik Duvarı, Azure alt ağından giden ağ erişimini denetlemenin bir yoludur. Azure Güvenlik Duvarı ile şunları yapılandırabilirsiniz:

* Bir alt ağdan erişilebilen tam etki alanı adlarını (FQDN) tanımlayan uygulama kuralları. FQDN, [SQL örneklerini](sql-fqdn-filtering.md)de içerebilir.
* Kaynak adres, protokol, hedef bağlantı noktası ve hedef adresini tanımlayan ağ kuralları.

Ağ trafiğinizi güvenlik duvarından alt ağın varsayılan ağ geçidi olarak yönlendirdiğinizde ağ trafiği yapılandırılan güvenlik duvarı kurallarına tabi tutulur.

Bu makale için, kolay dağıtım için üç alt ağ içeren basitleştirilmiş tek bir VNet oluşturursunuz. Üretim dağıtımları için hub [ve spoke modeli](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) önerilir. Güvenlik duvarı kendi VNet'indedir. İş yükü sunucuları, bir veya daha fazla alt ağla aynı bölgedeki eşlenmiş VNet'lerde bulunmaktadır.

* **AzureFirewallSubnet** - güvenlik duvarı bu alt ağdadır.
* **Workload-SN**: İş yükü sunucusu bu alt ağda yer alır. Bu alt ağın ağ trafiği güvenlik duvarından geçer.
* **Jump-SN**: "Atlama" sunucusu bu alt ağda yer alır. Atlama sunucusu, Uzak Masaüstü ile bağlanabileceğiniz genel IP adresine sahiptir. Buradan iş yükü sunucusuna bağlanabilirsiniz (başka bir Uzak Masaüstü oturumuyla).

![Öğretici ağı altyapısı](media/tutorial-firewall-rules-portal/Tutorial_network.png)

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Test amaçlı ağ ortamı oluşturma
> * Güvenlik duvarı dağıtma
> * Varsayılan rota oluşturma
> * www.google.com erişime izin verecek şekilde bir uygulama kuralını yapılandırma
> * Dış DNS sunucularına erişime izin vermek için ağ kuralı yapılandırma
> * Güvenlik duvarını test etme

İsterseniz, Azure [portalını](tutorial-firewall-deploy-portal.md) veya [Azure PowerShell'i](deploy-ps.md)kullanarak bu yordamı tamamlayabilirsiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Ön koşullar

### <a name="azure-cli"></a>Azure CLI

CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, Azure CLI sürüm 2.0.4 veya sonrası sürümü çalıştırın. Sürümü bulmak **için, az --sürümünü çalıştırın.** Yükleme veya yükseltme hakkında bilgi için Azure [CLI'yi yükle'ye]( /cli/azure/install-azure-cli)bakın.

Azure Güvenlik Duvarı uzantısını yükleyin:

```azurecli-interactive
az extension add -n azure-firewall
```


## <a name="set-up-the-network"></a>Ağı ayarlama

İlk olarak güvenlik duvarını dağıtmak için gerekli olan kaynakları içerecek bir kaynak grubu oluşturun. Ardından sanal ağı, alt ağları ve test sunucularını oluşturun.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu dağıtım için tüm kaynakları içerir.

```azurecli-interactive
az group create --name Test-FW-RG --location eastus
```

### <a name="create-a-vnet"></a>Sanal ağ oluşturma

Bu sanal ağ üç alt ağa sahiptir.

> [!NOTE]
> AzureFirewallSubnet alt ağının boyutu /26'dır. Alt ağ boyutu hakkında daha fazla bilgi için [Azure Güvenlik Duvarı SSS'si'ne](firewall-faq.md#why-does-azure-firewall-need-a-26-subnet-size)bakın.

```azurecli-interactive
az network vnet create \
  --name Test-FW-VN \
  --resource-group Test-FW-RG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name AzureFirewallSubnet \
  --subnet-prefix 10.0.1.0/26
az network vnet subnet create \
  --name Workload-SN \
  --resource-group Test-FW-RG \
  --vnet-name Test-FW-VN   \
  --address-prefix 10.0.2.0/24
az network vnet subnet create \
  --name Jump-SN \
  --resource-group Test-FW-RG \
  --vnet-name Test-FW-VN   \
  --address-prefix 10.0.3.0/24
```

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Şimdi atlama ve iş yükü sanal makinelerini oluşturup uygun alt ağlara yerleştirin.
İstendiğinde, sanal makine için bir parola yazın.

Srv-Jump sanal makineoluşturun.

```azurecli-interactive
az vm create \
    --resource-group Test-FW-RG \
    --name Srv-Jump \
    --location eastus \
    --image win2016datacenter \
    --vnet-name Test-FW-VN \
    --subnet Jump-SN \
    --admin-username azureadmin
az vm open-port --port 3389 --resource-group Test-FW-RG --name Srv-Jump
```



Belirli DNS sunucu IP adresleriyle Srv-Work için bir NIC oluşturun ve test etmek için ortak IP adresi oluşturmayın.

```azurecli-interactive
az network nic create \
    -g Test-FW-RG \
    -n Srv-Work-NIC \
   --vnet-name Test-FW-VN \
   --subnet Workload-SN \
   --public-ip-address "" \
   --dns-servers 209.244.0.3 209.244.0.4
```

Şimdi iş yükü sanal makine oluşturun.
İstendiğinde, sanal makine için bir parola yazın.

```azurecli-interactive
az vm create \
    --resource-group Test-FW-RG \
    --name Srv-Work \
    --location eastus \
    --image win2016datacenter \
    --nics Srv-Work-NIC \
    --admin-username azureadmin
```

## <a name="deploy-the-firewall"></a>Güvenlik duvarını dağıtma

Şimdi güvenlik duvarını sanal ağa dağıtın.

```azurecli-interactive
az network firewall create \
    --name Test-FW01 \
    --resource-group Test-FW-RG \
    --location eastus
az network public-ip create \
    --name fw-pip \
    --resource-group Test-FW-RG \
    --location eastus \
    --allocation-method static \
    --sku standard
az network firewall ip-config create \
    --firewall-name Test-FW01 \
    --name FW-config \
    --public-ip-address fw-pip \
    --resource-group Test-FW-RG \
    --vnet-name Test-FW-VN
az network firewall update \
    --name Test-FW01 \
    --resource-group Test-FW-RG 
az network public-ip show \
    --name fw-pip \
    --resource-group Test-FW-RG
fwprivaddr="$(az network firewall ip-config list -g Test-FW-RG -f Test-FW01 --query "[?name=='FW-config'].privateIpAddress" --output tsv)"
```

Özel IP adresini not edin. Varsayılan rotayı oluştururken bu adresi kullanacaksınız.

## <a name="create-a-default-route"></a>Varsayılan rota oluşturma

BGP rota yayılma devre dışı bırakılmış bir tablo oluşturma

```azurecli-interactive
az network route-table create \
    --name Firewall-rt-table \
    --resource-group Test-FW-RG \
    --location eastus \
    --disable-bgp-route-propagation true
```

Rotayı oluşturun.

```azurecli-interactive
az network route-table route create \
  --resource-group Test-FW-RG \
  --name DG-Route \
  --route-table-name Firewall-rt-table \
  --address-prefix 0.0.0.0/0 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address $fwprivaddr
```

Rota tablosunu alt ağla ilişkilendirme

```azurecli-interactive
az network vnet subnet update \
    -n Workload-SN \
    -g Test-FW-RG \
    --vnet-name Test-FW-VN \
    --address-prefixes 10.0.2.0/24 \
    --route-table Firewall-rt-table
```

## <a name="configure-an-application-rule"></a>Uygulama kuralı yapılandırma

Uygulama kuralı, www.google.com giden erişim sağlar.

```azurecli-interactive
az network firewall application-rule create \
   --collection-name App-Coll01 \
   --firewall-name Test-FW01 \
   --name Allow-Google \
   --protocols Http=80 Https=443 \
   --resource-group Test-FW-RG \
   --target-fqdns www.google.com \
   --source-addresses 10.0.2.0/24 \
   --priority 200 \
   --action Allow
```

Azure Güvenlik Duvarı'nda varsayılan olarak izin verilen altyapı FQDN'leri için yerleşik bir kural koleksiyonu bulunur. Bu FQDN'ler platforma özgüdür ve başka amaçlarla kullanılamaz. Daha fazla bilgi için bkz. [Altyapı FQDN'leri](infrastructure-fqdns.md).

## <a name="configure-a-network-rule"></a>Ağ kuralını yapılandırma

Ağ kuralı, bağlantı noktası 53'teki (DNS) iki IP adresine giden erişime izin verir.

```azurecli-interactive
az network firewall network-rule create \
   --collection-name Net-Coll01 \
   --destination-addresses 209.244.0.3 209.244.0.4 \
   --destination-ports 53 \
   --firewall-name Test-FW01 \
   --name Allow-DNS \
   --protocols UDP \
   --resource-group Test-FW-RG \
   --priority 200 \
   --source-addresses 10.0.2.0/24 \
   --action Allow
```

## <a name="test-the-firewall"></a>Güvenlik duvarını test etme

Beklendiği gibi çalıştığını doğrulamak için güvenlik duvarını test edin.

1. **Srv-Work** sanal makinesinin özel IP adresine dikkat edin:

   ```azurecli-interactive
   az vm list-ip-addresses \
   -g Test-FW-RG \
   -n Srv-Work
   ```

1. Uzak bir masaüstünü **Srv-Jump** sanal makinesine bağlayın ve oturum açın. Buradan, **Srv-Work** özel IP adresine uzak bir masaüstü bağlantısı açın ve oturum açın.

3. **SRV-Work'te**PowerShell penceresini açın ve aşağıdaki komutları çalıştırın:

   ```
   nslookup www.google.com
   nslookup www.microsoft.com
   ```

   Her iki komut da, DNS sorgularınızın güvenlik duvarından geçtiğini göstererek yanıtları döndürmelidir.

1. Aşağıdaki komutları çalıştırın:

   ```
   Invoke-WebRequest -Uri https://www.google.com
   Invoke-WebRequest -Uri https://www.google.com

   Invoke-WebRequest -Uri https://www.microsoft.com
   Invoke-WebRequest -Uri https://www.microsoft.com
   ```

   İstekler `www.google.com` başarılı olmalı `www.microsoft.com` ve istekler başarısız olmalıdır. Bu, güvenlik duvarı kurallarınızın beklendiği gibi çalıştığını gösterir.

Şimdi güvenlik duvarı kurallarının işe yaradığını doğruladınız:

* Yapılandırılmış dış DNS sunucusunu kullanarak DNS adlarını çözümleyebilirsiniz.
* İzin verilen bir FQDN'ye göz atabilir ancak diğerlerine göz atamazsınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güvenlik duvarı kaynaklarınızı bir sonraki öğretici için saklayabilir veya artık gerekyoksa güvenlik duvarıyla ilgili tüm kaynakları silmek için **Test-FW-RG** kaynak grubunu silebilirsiniz:

```azurecli-interactive
az group delete \
  -n Test-FW-RG
```

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Azure Güvenlik Duvarı günlüklerini izleme](./tutorial-diagnostics.md)
