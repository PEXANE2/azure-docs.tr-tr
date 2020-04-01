---
title: Azure sanal ağında Bir IPv6 çift yığın uygulaması dağıtma - Kaynak Yöneticisi şablonu
titlesuffix: Azure Virtual Network
description: Bu makalede, Azure Kaynak Yöneticisi VM şablonlarını kullanarak Azure sanal ağında Standart Yük Dengeleyicisi ile bir IPv6 çift yığın uygulamasının nasıl dağıtılancaya kadar dağıtılanınca gösterilmektedir.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.workload: infrastructure-services
ms.date: 03/31/2020
ms.author: kumud
ms.openlocfilehash: 1f9531b5d1decfd462a82b9d389c5af519591c83
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80420639"
---
# <a name="deploy-an-ipv6-dual-stack-application-in-azure-virtual-network---template"></a>Azure sanal ağında Bir IPv6 çift yığın uygulaması dağıtma - Şablon

Bu makalede, Azure Kaynak Yöneticisi VM şablonunun geçerli olan bölümüyle birlikte IPv6 yapılandırma görevlerinin bir listesi sağlanmaktadır. IPv4 ve IPv6 alt ağlarına sahip bir çift yığın sanal ağı, çift (IPv4 + IPv6) ön uç yapılandırmasına sahip bir Standart Yük Dengeleyicisi, çift IP yapılandırmasına sahip NIC'lere sahip Sanal Bilgisayar'ları, çift IP yapılandırmasına sahip VM'leri ve genel IP'leri içeren Azure'da Standart Yük Dengeleyicisini kullanarak bu makalede açıklanan şablonu kullanın. 

## <a name="required-configurations"></a>Gerekli yapılandırmalar

Nerede oluşacaklarını görmek için şablondaki şablon bölümlerini arayın.

### <a name="ipv6-addressspace-for-the-virtual-network"></a>Sanal ağ için IPv6 adresiSpace

Eklenecek şablon bölümü:

```JSON
        "addressSpace": {
          "addressPrefixes": [
            "[variables('vnetv4AddressRange')]",
            "[variables('vnetv6AddressRange')]"    
```

### <a name="ipv6-subnet-within-the-ipv6-virtual-network-addressspace"></a>IPv6 sanal ağ adresiSpace içinde IPv6 alt

Eklenecek şablon bölümü:
```JSON
          {
            "name": "V6Subnet",
            "properties": {
              "addressPrefix": "[variables('subnetv6AddressRange')]"
            }

```

### <a name="ipv6-configuration-for-the-nic"></a>NIC için IPv6 yapılandırması

Eklenecek şablon bölümü:
```JSON
          {
            "name": "ipconfig-v6",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
          "privateIPAddressVersion":"IPv6",
              "subnet": {
                "id": "[variables('v6-subnet-id')]"
              },
              "loadBalancerBackendAddressPools": [
                {
                  "id": "[concat(resourceId('Microsoft.Network/loadBalancers','loadBalancer'),'/backendAddressPools/LBBAP-v6')]"
                }
```

### <a name="ipv6-network-security-group-nsg-rules"></a>IPv6 ağ güvenlik grubu (NSG) kuralları

```JSON
          {
            "name": "default-allow-rdp",
            "properties": {
              "description": "Allow RDP",
              "protocol": "Tcp",
              "sourcePortRange": "33819-33829",
              "destinationPortRange": "5000-6000",
              "sourceAddressPrefix": "ace:cab:deca:deed::/64",
              "destinationAddressPrefix": "cab:ace:deca:deed::/64",
              "access": "Allow",
              "priority": 1003,
              "direction": "Inbound"
            }
```

## <a name="conditional-configuration"></a>Koşullu yapılandırma

Bir ağ sanal cihazı kullanıyorsanız, Rota Tablosuna IPv6 rotalarını ekleyin. Aksi takdirde, bu yapılandırma isteğe bağlıdır.

```JSON
    {
      "type": "Microsoft.Network/routeTables",
      "name": "v6route",
      "apiVersion": "[variables('ApiVersion')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "routes": [
          {
            "name": "v6route",
            "properties": {
              "addressPrefix": "ace:cab:deca:deed::/64",
              "nextHopType": "VirtualAppliance",
              "nextHopIpAddress": "deca:cab:ace:f00d::1"
            }
```

## <a name="optional-configuration"></a>İsteğe bağlı yapılandırma

### <a name="ipv6-internet-access-for-the-virtual-network"></a>Sanal ağ için IPv6 Internet erişimi

```JSON
{
            "name": "LBFE-v6",
            "properties": {
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses','lbpublicip-v6')]"
              }
```

### <a name="ipv6-public-ip-addresses"></a>IPv6 Genel IP adresleri

```JSON
    {
      "apiVersion": "[variables('ApiVersion')]",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "lbpublicip-v6",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard"
      },
      "properties": {
        "publicIPAllocationMethod": "Static",
        "publicIPAddressVersion": "IPv6"
      }
```

### <a name="ipv6-front-end-for-load-balancer"></a>Yük Dengeleyicisi için IPv6 Ön uç

```JSON
          {
            "name": "LBFE-v6",
            "properties": {
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses','lbpublicip-v6')]"
              }
```

### <a name="ipv6-back-end-address-pool-for-load-balancer"></a>Yük Dengeleyicisi için IPv6 Arka uç adres havuzu

```JSON
              "backendAddressPool": {
                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', 'loadBalancer'), '/backendAddressPools/LBBAP-v6')]"
              },
              "protocol": "Tcp",
              "frontendPort": 8080,
              "backendPort": 8080
            },
            "name": "lbrule-v6"
```

### <a name="ipv6-load-balancer-rules-to-associate-incoming-and-outgoing-ports"></a>Gelen ve giden bağlantı noktalarını ilişkilendirmek için IPv6 yük dengeleyici kuralları

```JSON
          {
            "name": "ipconfig-v6",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
          "privateIPAddressVersion":"IPv6",
              "subnet": {
                "id": "[variables('v6-subnet-id')]"
              },
              "loadBalancerBackendAddressPools": [
                {
                  "id": "[concat(resourceId('Microsoft.Network/loadBalancers','loadBalancer'),'/backendAddressPools/LBBAP-v6')]"
                }
```

## <a name="sample-vm-template-json"></a>Örnek VM şablonu JSON
Azure Kaynak Yöneticisi şablonu kullanarak Azure sanal ağında bir IPv6 çift yığın uygulaması dağıtmak için örnek [şablonu buradan](https://azure.microsoft.com/resources/templates/ipv6-in-vnet-stdlb/)görüntüleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Genel IP adresleri,](https://azure.microsoft.com/pricing/details/ip-addresses/) [ağ bant genişliği](https://azure.microsoft.com/pricing/details/bandwidth/)veya Yük [Dengeleyicisi](https://azure.microsoft.com/pricing/details/load-balancer/)fiyatlandırması hakkında ayrıntılı bilgi bulabilirsiniz.
