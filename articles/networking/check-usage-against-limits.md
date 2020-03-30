---
title: Azure kaynak kullanımını sınırlamayla karşıdan kontrol edin | Microsoft Dokümanlar
description: Azure kaynak kullanımınızı Azure abonelik sınırlarıyla karşı nasıl denetlersiniz öğrenin.
services: networking
documentationcenter: na
author: KumudD
ms.author: kumud
tags: azure-resource-manager
ms.service: azure
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/05/2018
ms.openlocfilehash: cffa5677c5531f3887639c049998523d7d07586a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79455571"
---
# <a name="check-resource-usage-against-limits"></a>Kaynak kullanımını sınırlara göre denetleme

Bu makalede, aboneliğinizde dağıttığınız her ağ kaynağı türünün sayısını ve [abonelik sınırlarınızın](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fnetworking%2ftoc.json#networking-limits) ne olduğunu nasıl göreceğinizi öğrenirsiniz. Kaynak kullanımını sınırlara göre görüntüleme yeteneği, geçerli kullanımı izlemek ve gelecekteki kullanım için plan yapmak için yararlıdır. Kullanımı izlemek için [Azure Portalını,](#azure-portal) [PowerShell'i](#powershell)veya [Azure CLI'yi](#azure-cli) kullanabilirsiniz.

## <a name="azure-portal"></a>Azure Portalı

1. Azure [portalına](https://portal.azure.com)giriş yapın.
2. Azure portalının üst, sol köşesinde **Tüm hizmetler'i**seçin.
3. *Abonelikleri* **Filtre** kutusuna girin. Arama sonuçlarında **Abonelik** ifadesi göründüğünde bunu seçin.
4. Kullanım bilgilerini görüntülemek istediğiniz aboneliğin adını seçin.
5. **AYARLAR** **altında, Kullanım + kotayı**seçin.
6. Şu seçenekleri seçebilirsiniz:
   - **Kaynak türleri**: Tüm kaynak türlerini seçebilir veya görüntülemek istediğiniz belirli kaynak türlerini seçebilirsiniz.
   - **Sağlayıcılar**: Tüm kaynak sağlayıcılarını seçebilir veya **Bilgi İşlem,** **Ağ**veya **Depolama'yı**seçebilirsiniz.
   - **Konumlar**: Tüm Azure konumlarını seçebilir veya belirli konumları seçebilirsiniz.
   - Tüm kaynakları veya yalnızca en az birinin dağıtıldığı kaynakları göstermeyi seçebilirsiniz.

     Aşağıdaki resimdeki örnek, Doğu ABD'de dağıtılan en az bir kaynağı olan tüm ağ kaynaklarını gösterir:

       ![Kullanım verilerini görüntüleme](./media/check-usage-against-limits/view-usage.png)

     Sütun başlığını seçerek sütunları sıralayabilirsiniz. Gösterilen sınırlar aboneliğinizin sınırlarıdır. Varsayılan sınırı artırmanız gerekiyorsa, **İstek Artışı'nı**seçin ve ardından destek isteğini tamamlayın ve gönderin. Tüm kaynakların Azure [sınırlarında](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fnetworking%2ftoc.json#networking-limits)listelenen maksimum sınırı vardır. Geçerli sınırınız zaten en yüksek sayıdaysa, sınır artırılamaz.

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[Azure Bulut Kabuğu'nda](https://shell.azure.com/powershell)veya bilgisayarınızdan PowerShell çalıştırarak takip eden komutları çalıştırabilirsiniz. Azure Bulut Kabuğu ücretsiz bir etkileşimli kabuktır. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. PowerShell'i bilgisayarınızdan çalıştırAcaksanız, Azure PowerShell modülüne, sürüm 1.0.0 veya daha sonrasına ihtiyacınız vardır. Yüklü `Get-Module -ListAvailable Az` sürümü bulmak için bilgisayarınızda çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız, Azure'da oturum açmak için de çalışmanız `Login-AzAccount` gerekir.

[Get-AzNetworkUsage](https://docs.microsoft.com/powershell/module/az.network/get-aznetworkusage)ile kullanımınızı sınırlara göre görüntüleyin. Aşağıdaki örnek, Doğu ABD konumunda en az bir kaynağın dağıtıldığı kaynakların kullanımını alır:

```azurepowershell-interactive
Get-AzNetworkUsage `
  -Location eastus `
  | Where-Object {$_.CurrentValue -gt 0} `
  | Format-Table ResourceType, CurrentValue, Limit
```

Aşağıdaki örnek çıktıyla aynı biçimlendirilmiş çıktı alırsınız:

```output
ResourceType            CurrentValue Limit
------------            ------------ -----
Virtual Networks                   1    50
Network Security Groups            2   100
Public IP Addresses                1    60
Network Interfaces                 1 24000
Network Watchers                   1     1
```

## <a name="azure-cli"></a>Azure CLI

Bu makaledeki görevleri tamamlamak için Azure Komut satırı arabirimi (CLI) komutlarını kullanıyorsanız, [komutları Azure Bulut Kabuğu'ndaki](https://shell.azure.com/bash)komutları çalıştırın veya CLI'yi bilgisayarınızdan çalıştırın. Bu makale, Azure CLI sürümü 2.0.32 veya daha sonra gerektirir. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, Azure'da oturum açmak için de çalışmanız `az login` gerekir.

az ağ listesi [kullanımları](/cli/azure/network?view=azure-cli-latest#az-network-list-usages)ile kullanımınızı sınırlara göre görüntüleyin. Aşağıdaki örnek, Doğu ABD'deki kaynakların kullanımını alır:

```azurecli-interactive
az network list-usages \
  --location eastus \
  --out table
```

Aşağıdaki örnek çıktıyla aynı biçimlendirilmiş çıktı alırsınız:

```output
Name                    CurrentValue Limit
------------            ------------ -----
Virtual Networks                   1    50
Network Security Groups            2   100
Public IP Addresses                1    60
Network Interfaces                 1 24000
Network Watchers                   1     1
Load Balancers                     0   100
Application Gateways               0    50
```
