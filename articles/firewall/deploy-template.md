---
title: 'Hızlı başlangıç: Kullanılabilirlik Alanları Kaynak Yöneticisi şablonuyla Azure Güvenlik Duvarı oluşturma'
description: Bir şablon kullanarak Azure Güvenlik Duvarı dağıtın. Sanal ağın üç alt ağı olan bir VNet 'i vardır. İki Windows Server sanal makinesi dağıtılır; bir sıçrama kutusu ve sunucu.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: quickstart
ms.date: 04/30/2020
ms.author: victorh
ms.openlocfilehash: 17ab693033b61c25ba2f5b5bd588ef52caf8c046
ms.sourcegitcommit: acc558d79d665c8d6a5f9e1689211da623ded90a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82597714"
---
# <a name="quickstart-deploy-azure-firewall-with-availability-zones---resource-manager-template"></a>Hızlı başlangıç: Kullanılabilirlik Alanları Kaynak Yöneticisi şablonuyla Azure Güvenlik duvarını dağıtma

Bu hızlı başlangıçta, Azure Güvenlik duvarını üç Kullanılabilirlik Alanları dağıtmak için bir Kaynak Yöneticisi şablonu kullanırsınız. 

Şablon, güvenlik duvarıyla bir test ağı ortamı oluşturur. Ağda üç alt ağa sahip bir sanal ağ (VNet) vardır: *AzureFirewallSubnet*, *Serverssubnet*ve *jumpboxsubnet*. *Serverssubnet* ve *jumpboxsubnet* alt ağı her birinin tek, Iki çekirdekli bir Windows Server sanal makinesi vardır.

Güvenlik Duvarı *AzureFirewallSubnet* alt ağıdır ve erişim izni veren tek bir kuralla bir uygulama kuralı koleksiyonuna sahiptir `www.microsoft.com`.

Kullanıcı tanımlı bir yol, Güvenlik Duvarı kurallarının uygulandığı güvenlik duvarı aracılığıyla *Serverssubnet* alt ağından gelen ağ trafiğini gösterir.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Azure Güvenlik Duvarı hakkında daha fazla bilgi için bkz. [Azure Güvenlik duvarını Azure Portal kullanarak dağıtma ve yapılandırma](tutorial-firewall-deploy-portal.md).

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-an-azure-firewall-with-availability-zones"></a>Kullanılabilirlik Alanları ile Azure Güvenlik Duvarı oluşturma

Bu şablon, Azure Güvenlik duvarını desteklemek için gerekli kaynaklarla birlikte Kullanılabilirlik Alanları bir Azure Güvenlik duvarı oluşturur.

### <a name="review-the-template"></a>Şablonu gözden geçirin

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://github.com/Azure/azure-quickstart-templates/blob/master/101-azurefirewall-with-zones-sandbox/azuredeploy.json).

:::code language="json" source="~/quickstart-templates/101-azurefirewall-with-zones-sandbox/azuredeploy.json" range="001-444" highlight="369-442":::

Şablonda birden çok Azure kaynağı tanımlanmış:

- [**Microsoft. Network/Publicıpaddresses**](/azure/templates/microsoft.network/publicipaddresses)
- [**Microsoft. Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft. Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines)
- [**Microsoft. Network/NetworkInterfaces**](/azure/templates/microsoft.network/networkinterfaces)
- [**Microsoft. Storage/storageAccounts**](/azure/templates/microsoft.storage/storageAccounts)
- [**Microsoft. Network/azureFirewalls**](/azure/templates/microsoft.network/azureFirewalls)
- [**Microsoft. Network/routeTables**](/azure/templates/microsoft.network/routeTables)

### <a name="deploy-the-template"></a>Şablonu dağıtma

Kaynak Yöneticisi şablonunu Azure 'a dağıtın:

1. Azure 'da oturum açmak için **Azure 'A dağıt** ' ı seçin ve şablonu açın. Şablon bir Azure Güvenlik Duvarı, ağ altyapısı ve iki sanal makine oluşturur.

   [![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-azurefirewall-with-zones-sandbox%2Fazuredeploy.json)

2. Portalda, **bölgelere sahip Azure Güvenlik Duvarı 'nın korumalı alan kurulumunu oluşturma** sayfasında, aşağıdaki değerleri yazın veya seçin:
   - **Kaynak grubu**: **Yeni oluştur**' u seçin, kaynak grubu için bir ad yazın ve **Tamam**' ı seçin. 
   - **Sanal ağ adı**: yeni VNET için bir ad yazın. 
   - **Yönetici Kullanıcı adı**: yönetici kullanıcı hesabı için bir Kullanıcı adı yazın.
   - **Yönetici parolası**: bir yönetici parolası yazın. 

3. Hüküm ve koşulları okuyun ve ardından **yukarıda belirtilen hüküm ve koşulları kabul ediyorum** ' u ve ardından **satın al**' ı seçin. Dağıtımın tamamlanması 10 dakika veya daha uzun sürebilir.

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Güvenlik duvarıyla oluşturulan kaynakları keşfedebilir.

Bir şablondaki güvenlik duvarının JSON sözdizimi ve özellikleri hakkında bilgi edinmek için bkz. [Microsoft. Network/azureFirewalls](/azure/templates/microsoft.network/azurefirewalls).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız kalmadığında, `Remove-AzResourceGroup` PowerShell komutunu çalıştırarak kaynak grubunu, güvenlik duvarını ve ilgili tüm kaynakları kaldırabilirsiniz. *Myresourcegroup*adlı bir kaynak grubunu kaldırmak için şunu çalıştırın: 

```azurepowershell-interactive
Remove-AzResourceGroup -Name MyResourceGroup
```
Güvenlik Duvarı izleme öğreticisine devam etmeyi planlıyorsanız, kaynak grubunu ve güvenlik duvarını kaldırmayın. 

## <a name="next-steps"></a>Sonraki adımlar

Ardından, Azure Güvenlik Duvarı günlüklerini izleyebilirsiniz:

[Öğretici: Azure Güvenlik Duvarı günlüklerini izleme](./tutorial-diagnostics.md)
