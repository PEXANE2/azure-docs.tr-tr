---
title: PowerShell ile bulut hizmet konteyneri oluşturma | Microsoft Dokümanlar
description: Bu makalede, PowerShell ile bir bulut hizmet kapsayıcısı nasıl oluşturulacak açıklanmaktadır. Kapsayıcı web ve işçi rolleri barındıran.
services: cloud-services
documentationcenter: .net
author: cawaMS
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: ef9d3c7c479df9c71a855f0a243b5b9d0da947d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68359496"
---
# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Boş bir bulut hizmet kapsayıcısı oluşturmak için Azure PowerShell komutunu kullanma

Bu makalede, Azure PowerShell cmdlets kullanarak bir Bulut Hizmetleri kapsayıcısı nasıl hızlı bir şekilde oluşturulabileceğiniz açıklanmaktadır. Lütfen aşağıdaki adımları izleyin:

1. [Azure PowerShell indirme sayfasından](https://aka.ms/webpi-azps) Microsoft Azure PowerShell cmdlet'ini yükleyin.
2. PowerShell komut istemini açın.
3. Oturum açabilmek için [Azure Ekle Hesabını](/powershell/module/servicemanagement/azure/add-azureaccount?view=azuresmps-4.0.0) kullanın.

   > [!NOTE]
   > Azure PowerShell cmdlet'ini yüklemek ve Azure aboneliğinize bağlanmak hakkında daha fazla bilgi için [Azure PowerShell'i nasıl yükleyip yapılandırabilirsiniz'](/powershell/azure/overview)a bakın.
   >
   >
4. Boş bir Azure bulut hizmet kapsayıcısı oluşturmak için **Yeni AzureHizmeti** cmdlet'ini kullanın.

   ```
   New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```

5. Cmdlet çağırmak için aşağıdaki örneği izleyin:

   ```powershell
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

Azure bulut hizmeti oluşturma hakkında daha fazla bilgi için şu bilgileri çalıştırın:

```powershell
Get-help New-AzureService
```

### <a name="next-steps"></a>Sonraki adımlar

* Bulut hizmeti dağıtımını yönetmek için [Get-AzureService](/powershell/module/servicemanagement/azure/Get-AzureService?view=azuresmps-4.0.0), [Kaldır-AzureService](/powershell/module/servicemanagement/azure/Remove-AzureService?view=azuresmps-4.0.0)ve [AzureService Ayarla](/powershell/module/servicemanagement/azure/set-azureservice?view=azuresmps-4.0.0) komutlarına bakın. Ayrıca, daha fazla bilgi [için bulut hizmetlerinin nasıl yapılandırılabildiğini](cloud-services-how-to-configure-portal.md) de belirtebilirsiniz.
* Bulut hizmeti projenizi Azure'da yayımlamak için [arşivlenmiş bulut hizmetleri deposundan](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Scripts/cloud-services-continuous-delivery) **PublishCloudService.ps1** kod örneğine bakın.