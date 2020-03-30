---
title: Quickstart - PowerShell Azure Analiz Hizmetlerini Kullanarak Azure Analiz Hizmetleri Oluşturun | Microsoft Dokümanlar
description: PowerShell kullanarak bir Azure Analysis Services sunucusu oluşturma hakkında bilgi edinin.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 07/29/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 071c44136d917874843bdf080de775ec6739937a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75442865"
---
# <a name="quickstart-create-a-server---powershell"></a>Hızlı başlangıç: Sunucu oluşturma - PowerShell

Bu hızlı başlangıç, Azure aboneliğinizde bir Azure Analysis Services sunucusu oluşturmak için komut satırından PowerShell kullanmayı anlatmaktadır.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure aboneliği**: Hesap oluşturmak için [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/offers/ms-azr-0044p/)’nü ziyaret edin.
- **Azure Active Directory**: Aboneliğinizin bir Azure Active Directory Kiracısı ile ilişkilendirilmiş olması ve ilgili dizinde bir hesabınızın olması gerekir. Daha fazla bilgi edinmek için bkz. [Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md).
- **Azure PowerShell**. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yüklemek veya yükseltmek için bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-Az-ps).

## <a name="import-azanalysisservices-module"></a>İthalat Az.AnalysisServices modülü

Aboneliğinizde bir sunucu oluşturmak için [Az.AnalysisServices](/powershell/module/az.analysisservices) modüllerini kullanırsınız. Az.AnalysisServices modüllerini PowerShell oturumunuza yükleyin.

```powershell
Import-Module Az.AnalysisServices
```

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) komutunu kullanarak Azure aboneliğinizde oturum açın. Ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[Azure kaynak grubu](../azure-resource-manager/management/overview.md), Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Sunucunuzu oluşturduğunuzda, aboneliğinizde bir kaynak grubu belirtmeniz gerekir. Zaten bir kaynak grubunuz yoksa, [Yeni Kaynak Grubu](/powershell/module/az.resources/new-azresourcegroup) komutunu kullanarak yeni bir kaynak oluşturabilirsiniz. Aşağıdaki örnekte Batı ABD bölgesinde `myResourceGroup` adında bir kaynak grubu oluşturulmaktadır.

```powershell
New-AzResourceGroup -Name "myResourceGroup" -Location "WestUS"
```

## <a name="create-a-server"></a>Sunucu oluşturma

[New-AzAnalysisServicesServer](/powershell/module/az.analysisservices/new-azanalysisservicesserver) komutunu kullanarak yeni bir sunucu oluşturun. Aşağıdaki örnek, Batı ABD bölgesinde D1 (ücretsiz) katmanında myResourceGroup’ta myServer adlı bir sunucu oluşturur ve sunucu yöneticisi olarak philipc@adventureworks.com adresini belirtir.

```powershell
New-AzAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myserver" -Location WestUS -Sku D1 -Administrator "philipc@adventure-works.com"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

[Remove-AzAnalysisServicesServer](/powershell/module/az.analysisservices/new-azanalysisservicesserver) komutunu kullanarak sunucuyu aboneliğinizden kaldırabilirsiniz. Bu koleksiyondaki diğer hızlı başlangıçları ve öğreticileri kullanacaksanız sunucunuzu kaldırmayın. Aşağıdaki örnekte önceki adımda oluşturduğunuz sunucu kaldırılır.


```powershell
Remove-AzAnalysisServicesServer -Name "myserver" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta PowerShell kullanarak Azure aboneliğinizde sunucu oluşturmayı öğrendiniz. Sunucuyu oluşturduğunuza göre bir sunucu güvenlik duvarı yapılandırarak (isteğe bağlı) güvenliğinin sağlanmasına yardımcı olacaksınız. Sunucunuza doğrudan portaldan örnek bir veri modeli de ekleyebilirsiniz. Örnek modelin olması, model veritabanı rollerini yapılandırma ve istemci bağlantılarını test etme işlemlerini öğrenme konusunda yardımcı olur. Daha fazla bilgi edinmek için, örnek model ekleme öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Quickstart: Sunucu güvenlik duvarLarını yapılandırma - Portal](analysis-services-qs-firewall.md)      
> [!div class="nextstepaction"]
> [Öğretici: Sunucunuza örnek model ekleme](analysis-services-create-sample-model.md)
