---
title: Şablon kullanarak Azure Güvenlik Duvarı’nı dağıtma
description: Bir şablon kullanarak Azure Güvenlik Duvarı'nı dağıtın. Oluşturulan ağ, üç alt ağiçeren bir VNet'e sahiptir. İki, iki çekirdekli Windows Server sanal makineleri dağıtılır.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/19/2019
ms.author: victorh
ms.openlocfilehash: b9e82998283bf44eede88148a12d88aae521b1ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74169192"
---
# <a name="deploy-azure-firewall-using-a-template"></a>Şablon kullanarak Azure Güvenlik Duvarı’nı dağıtma

[AzureFirewall sanal kurulum şablonu oluştur,](https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-with-zones-sandbox) güvenlik duvarı içeren bir test ağı ortamı oluşturur. Ağ üç alt ağları ile bir sanal ağ (VNet) vardır: *AzureFireWallSubnet*, *ServersSubnet*, ve *JumpboxSubnet*. *ServersSubnet* ve *JumpboxSubnet* alt ağının her birinde tek, iki çekirdekli Windows Server sanal makinesi vardır.

Güvenlik duvarı *AzureFirewallSubnet* alt ağıiçindedir ve `www.microsoft.com`'ye erişim sağlayan tek bir kurala sahip bir uygulama kuralı koleksiyonu vardır.

Kullanıcı tanımlı bir rota, güvenlik duvarı kurallarının uygulandığı güvenlik duvarından *SunucularSubnet* alt ağındaki ağ trafiğini işaret ediyor.

Azure Güvenlik Duvarı hakkında daha fazla bilgi için Azure [portalını kullanarak Azure Güvenlik Duvarını Dağıt'a](tutorial-firewall-deploy-portal.md)ve yapılandırmaya bakın.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="use-the-template-to-deploy-azure-firewall"></a>Azure Güvenlik Duvarı'nı dağıtmak için şablonu kullanma

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

**Şablonu kullanarak Azure Güvenlik Duvarı'nı yüklemek ve dağıtmak için:**

1. Şablona ' [https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-with-zones-sandbox](https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-with-zones-sandbox)dan erişin.
   
1. Girişi okuyun ve dağıtmaya hazır olduğunuzda **Azure'a Dağıt'ı**seçin.
   
1. Gerekirse Azure portalında oturum açın. 

1. Portalda, **AzureFirewall sayfasının sanal kutu oluştur kurulumunda** aşağıdaki değerleri yazın veya seçin:
   
   - **Kaynak grubu**: Kaynak grubu için **yeni**, ad yazın ve **Tamam'ı**seçin. 
   - **Sanal Ağ Adı**: Yeni VNet için bir ad yazın. 
   - **Yönetici Kullanıcı Adı**: Yönetici kullanıcı hesabı için bir kullanıcı adı yazın.
   - **Yönetici Şifresi**: Yönetici parolası yazın. 
   
1. Hüküm ve koşulları okuyun ve daha sonra **yukarıda belirtilen hüküm ve koşulları kabul seçin.**
   
1. **Satın al**'ı seçin.
   
   Kaynakları oluşturmak birkaç dakika sürer. 
   
1. Güvenlik duvarıyla oluşturulan kaynakları keşfedin. 

Şablondaki bir güvenlik duvarının JSON sözdizimi ve özellikleri hakkında bilgi edinmek için [Microsoft.Network/azureFirewalls'a](/azure/templates/microsoft.network/azurefirewalls)bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık bunlara ihtiyacınız olmadığında, [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) PowerShell komutunu çalıştırarak kaynak grubunu, güvenlik duvarını ve ilgili tüm kaynakları kaldırabilirsiniz. *MyResourceGroup*adlı bir kaynak grubunu kaldırmak için çalıştırın: 

```azurepowershell-interactive
Remove-AzResourceGroup -Name MyResourceGroup
```
Güvenlik duvarı izleme öğreticisine devam etmeyi planlıyorsanız, kaynak grubunu ve güvenlik duvarını henüz kaldırmayın. 

## <a name="next-steps"></a>Sonraki adımlar

Ardından, Azure Güvenlik Duvarı günlüklerini izleyebilirsiniz:

> [!div class="nextstepaction"]
> [Öğretici: Azure Güvenlik Duvarı günlüklerini izleme](./tutorial-diagnostics.md)
