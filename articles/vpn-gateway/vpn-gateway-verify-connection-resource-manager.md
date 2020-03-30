---
title: 'Azure VPN Ağ Geçidi: Ağ geçidi bağlantısını doğrulama'
description: Bu makalede, sanal ağ VPN Ağ Geçidi bağlantısını nasıl doğrulayabileceğiniz gösterilmektedir.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 05/16/2017
ms.author: cherylmc
ms.openlocfilehash: 40417b078577424b1ba13ce60f4f773a1c8fd4dc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75780156"
---
# <a name="verify-a-vpn-gateway-connection"></a>VPN Ağ Geçidi bağlantısını doğrulama

Bu makalede, hem klasik hem de Kaynak Yöneticisi dağıtım modelleri için VPN ağ geçidi bağlantısını nasıl doğrulayabileceğiniz gösterilmektedir.

## <a name="azure-portal"></a>Azure portalında

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="powershell"></a>PowerShell

PowerShell'i kullanarak Kaynak Yöneticisi dağıtım modeli için VPN ağ geçidi bağlantısını doğrulamak için [Azure Kaynak Yöneticisi PowerShell cmdlets'in](/powershell/azure/overview)en son sürümünü yükleyin.

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="azure-cli"></a>Azure CLI

Azure CLI kullanarak Kaynak Yöneticisi dağıtım modeli için VPN ağ geçidi bağlantısını doğrulamak için [CLI komutlarının](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0 veya sonraki) en son sürümünü yükleyin.

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]


## <a name="azure-portal-classic"></a>Azure portal (klasik)

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="powershell-classic"></a>PowerShell (Klasik)

PowerShell'i kullanarak klasik dağıtım modeli için VPN ağ geçidi bağlantınızı doğrulamak için Azure PowerShell cmdlets'in en son sürümlerini yükleyin. [Hizmet Yönetimi](https://docs.microsoft.com/powershell/azure/servicemanagement/install-azure-ps?view=azuresmps-4.0.0#azure-service-management-cmdlets) modüllerini indirdiğinizden ve yüklediğinizden emin olun. Klasik dağıtım modelinde oturum açmak için 'Azure Hesabı Ekle'yi kullanın.

[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için bkz. [Sanal Makine Oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
