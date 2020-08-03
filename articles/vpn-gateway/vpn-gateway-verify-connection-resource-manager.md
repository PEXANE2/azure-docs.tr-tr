---
title: 'Azure VPN Gateway: ağ geçidi bağlantısını doğrulama'
description: Bu makalede, bir sanal ağ VPN Gateway bağlantısının nasıl doğrulanyapılacağı gösterilir.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 05/16/2017
ms.author: cherylmc
ms.custom: devx-track-azurecli
ms.openlocfilehash: 01e84ab1d67206d1d0eb54b0813756a29fea4685
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87496738"
---
# <a name="verify-a-vpn-gateway-connection"></a>VPN Gateway bağlantısını doğrulama

Bu makalede, hem klasik hem de Kaynak Yöneticisi dağıtım modelleriyle ilgili bir VPN ağ geçidi bağlantısını nasıl doğrulayabildiğinizi gösterilmektedir.

## <a name="azure-portal"></a>Azure portal

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="powershell"></a>PowerShell

PowerShell kullanarak Kaynak Yöneticisi dağıtım modeli için bir VPN ağ geçidi bağlantısını doğrulamak için, [Azure Resource Manager PowerShell cmdlet](/powershell/azure/)'lerinin en son sürümünü yükler.

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="azure-cli"></a>Azure CLI’si

Azure CLı kullanarak Kaynak Yöneticisi dağıtım modeli için bir VPN ağ geçidi bağlantısını doğrulamak için, [CLI komutlarının](https://docs.microsoft.com/cli/azure/install-azure-cli) en son sürümünü (2,0 veya üzeri) yüklemelisiniz.

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]


## <a name="azure-portal-classic"></a>Azure portal (klasik)

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="powershell-classic"></a>PowerShell (Klasik)

PowerShell kullanarak klasik dağıtım modeliyle ilgili VPN Gateway bağlantınızı doğrulamak için Azure PowerShell cmdlet 'lerinin en son sürümlerini yükler. [Hizmet yönetimi](https://docs.microsoft.com/powershell/azure/servicemanagement/install-azure-ps?view=azuresmps-4.0.0#azure-service-management-cmdlets) modülünü indirip yüklediğinizden emin olun. Klasik dağıtım modelinde oturum açmak için ' Add-AzureAccount ' kullanın.

[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için bkz. [Sanal Makine Oluşturma](../virtual-machines/windows/quick-create-portal.md).
