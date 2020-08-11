---
title: 'Terrayform: VNet tümleştirmesi ve özel uç noktayla güvenli şekilde bağlı ön uç Web uygulaması ve arka uç Web uygulaması dağıtma'
description: Özel uç nokta ve VNet tümleştirmesiyle güvenli bir şekilde bağlanan iki Web uygulaması dağıtmak üzere App Service için teraform sağlayıcısını kullanmayı öğrenin
author: ericgre
ms.assetid: 3e5d1bbd-5581-40cc-8f65-bc74f1802156
ms.topic: sample
ms.date: 08/10/2020
ms.author: ericg
ms.service: app-service
ms.workload: web
ms.openlocfilehash: 238cd5e79fe749052206cfdf25d576f88f1020e8
ms.sourcegitcommit: d8b8768d62672e9c287a04f2578383d0eb857950
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88065446"
---
# <a name="create-two-web-apps-connected-securely-with-private-endpoint-and-vnet-integration"></a>Özel uç nokta ve VNet tümleştirmesiyle güvenli bir şekilde bağlanan iki Web uygulaması oluşturma

Bu makalede, aşağıdaki adımları izleyerek iki Web uygulaması (ön uç ve arka uç) güvenli bir şekilde bağlamak için [Özel uç nokta](https://docs.microsoft.com/azure/app-service/networking/private-endpoint) ve bölgesel [VNET tümleştirmesinin](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet) örnek kullanımı gösterilmektedir:
- VNet dağıtma
- Tümleştirme için ilk alt ağ oluşturma
- Özel uç nokta için ikinci alt ağ oluşturun, ağ ilkelerini devre dışı bırakmak için belirli bir parametre ayarlamanız gerekir
- PremiumV2 türünde bir App Service planı dağıtın, Özel uç nokta özelliği için gereken en düşük SKU
- Özel DNS bölgesini kullanmak için belirli uygulama ayarlarıyla ön uç Web uygulamasını oluşturun, [daha fazla ayrıntı](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet#azure-dns-private-zones)
- Ön uç Web uygulamasını tümleştirme alt ağına bağlama
- Arka uç Web uygulaması oluşturma
- Web uygulaması için özel bağlantı bölgesinin adı ile DNS özel bölgesini oluşturma privatelink.azurewebsites.net
- Bu bölgeyi VNet 'e bağla
- Uç nokta alt ağında arka uç Web uygulaması için özel uç nokta oluşturun ve DNS adlarını (Web sitesi ve SCM) önceden oluşturulan DNS özel bölgesine kaydedin

## <a name="how-to-use-terraform-in-azure"></a>Azure 'da terrayform kullanma

Azure ile terrayform kullanmayı öğrenmek için [Azure belgelerine](https://docs.microsoft.com/azure/developer/terraform/) gidin.

## <a name="the-complete-terraform-file"></a>Tüm terrayform dosyası

Bu dosyayı kullanmak için, frontwebapp ve backwebapp kaynakları için Name özelliğini değiştirmeniz gerekir (WebApp adı Dünya genelinde benzersiz bir DNS adı olmalıdır). 

```hcl
provider "azurerm" {
  version = "~>2.0"
  features {}
}

resource "azurerm_resource_group" "rg" {
  name     = "appservice-rg"
  location = "francecentral"
}

resource "azurerm_virtual_network" "vnet" {
  name                = "vnet"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  address_space       = ["10.0.0.0/16"]
}

resource "azurerm_subnet" "integrationsubnet" {
  name                 = "integrationsubnet"
  resource_group_name  = azurerm_resource_group.rg.name
  virtual_network_name = azurerm_virtual_network.vnet.name
  address_prefixes     = ["10.0.1.0/24"]
  delegation {
    name = "delegation"
    service_delegation {
      name = "Microsoft.Web/serverFarms"
    }
  }
}

resource "azurerm_subnet" "endpointsubnet" {
  name                 = "endpointsubnet"
  resource_group_name  = azurerm_resource_group.rg.name
  virtual_network_name = azurerm_virtual_network.vnet.name
  address_prefixes     = ["10.0.2.0/24"]
  enforce_private_link_endpoint_network_policies = true
}

resource "azurerm_app_service_plan" "appserviceplan" {
  name                = "appserviceplan"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name

  sku {
    tier = "Premiumv2"
    size = "P1v2"
  }
}

resource "azurerm_app_service" "frontwebapp" {
  name                = "frontwebapp20200810"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  app_service_plan_id = azurerm_app_service_plan.appserviceplan.id

  app_settings = {
    "WEBSITE_DNS_SERVER": "168.63.129.16",
    "WEBSITE_VNET_ROUTE_ALL": "1"
  }
}

resource "azurerm_app_service_virtual_network_swift_connection" "vnetintegrationconnection" {
  app_service_id  = azurerm_app_service.frontwebapp.id
  subnet_id       = azurerm_subnet.integrationsubnet.id
}

resource "azurerm_app_service" "backwebapp" {
  name                = "backwebapp20200810"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  app_service_plan_id = azurerm_app_service_plan.appserviceplan.id
}

resource "azurerm_private_dns_zone" "dnsprivatezone" {
  name                = "privatelink.azurewebsites.net"
  resource_group_name = azurerm_resource_group.rg.name
}

resource "azurerm_private_dns_zone_virtual_network_link" "dnszonelink" {
  name = "dnszonelink"
  resource_group_name = azurerm_resource_group.rg.name
  private_dns_zone_name = azurerm_private_dns_zone.dnsprivatezone.name
  virtual_network_id = azurerm_virtual_network.vnet.id
}

resource "azurerm_private_endpoint" "privateendpoint" {
  name                = "backwebappprivateendpoint"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  subnet_id           = azurerm_subnet.endpointsubnet.id

  private_dns_zone_group {
    name = "privatednszonegroup"
    private_dns_zone_ids = [azurerm_private_dns_zone.dnsprivatezone.id]
  }

  private_service_connection {
    name = "privateendpointconnection"
    private_connection_resource_id = azurerm_app_service.backwebapp.id
    subresource_names = ["sites"]
    is_manual_connection = false
  }
}
```




## <a name="next-steps"></a>Sonraki adımlar


> [Azure 'da Terrayform kullanma hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/developer/terraform/)
