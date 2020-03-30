---
title: Windows PowerShell'de Azure bulut hizmetini ölçeklendirin | Microsoft Dokümanlar
description: (klasik) Azure'da veya Azure'da bir web rolünü veya çalışan rolünü ölçeklendirmek için PowerShell'i nasıl kullanacağınızı öğrenin.
services: cloud-services
author: mmccrory
ms.service: cloud-services
ms.topic: article
ms.date: 12/01/2016
ms.author: memccror
ms.openlocfilehash: a1945aad12eb34bad1b593878779e1ceb0dae686
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68359044"
---
# <a name="how-to-scale-a-cloud-service-in-powershell"></a>PowerShell'de bulut hizmeti nasıl ölçeklendirilir?

Örnekleri ekleyerek veya kaldırarak bir web rolünü veya çalışan rolünü ölçeklendirmek için Windows PowerShell'i kullanabilirsiniz.  

## <a name="log-in-to-azure"></a>Azure'da oturum açma

PowerShell üzerinden aboneliğinizde herhangi bir işlem gerçekleştirebilmeniz için oturum açmanız gerekir:

```powershell
Add-AzureAccount
```

Hesabınızla ilişkili birden çok aboneliğiniz varsa, bulut hizmetinizin nerede bulunduğuna bağlı olarak geçerli aboneliği değiştirmeniz gerekebilir. Geçerli aboneliği denetlemek için çalıştırın:

```powershell
Get-AzureSubscription -Current
```

Geçerli aboneliği değiştirmeniz gerekiyorsa, çalıştırın:

```powershell
Set-AzureSubscription -SubscriptionId <subscription_id>
```

## <a name="check-the-current-instance-count-for-your-role"></a>Rolünüz için geçerli örnek sayısını denetleme

Rolünüzün geçerli durumunu denetlemek için çalıştırın:

```powershell
Get-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>'
```

Geçerli işletim sistemi sürümü ve örnek sayısı da dahil olmak üzere rol hakkında bilgi almalısınız. Bu durumda, rolün tek bir örneği vardır.

![Rol hakkında bilgi](./media/cloud-services-how-to-scale-powershell/get-azure-role.png)

## <a name="scale-out-the-role-by-adding-more-instances"></a>Daha fazla örnek ekleyerek rolü ölçeklendirin

Rolünüzü ölçeklendirmek için, Istediğiniz sayıda örneği **Set-AzureRole** cmdlet'e **Sayma** parametresi olarak geçirin:

```powershell
Set-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>' -Slot <target_slot> -Count <desired_instances>
```

Cmdlet, yeni örnekler verilirken ve başlatılırken bir an için bloke olur. Bu süre zarfında, yeni bir PowerShell penceresi açar ve daha önce gösterildiği gibi **AzureRol Al'ı** ararsanız, yeni hedef örnek sayısını görürsünüz. Portaldaki rol durumunu incelerseniz, yeni örneğin başlatılabaşladığını görmeniz gerekir:

![Portaldan başlayan VM örneği](./media/cloud-services-how-to-scale-powershell/role-instance-starting.png)

Yeni örnekler başladıktan sonra cmdlet başarıyla geri dönecektir:

![Rol örneği başarıyı artırır](./media/cloud-services-how-to-scale-powershell/set-azure-role-success.png)

## <a name="scale-in-the-role-by-removing-instances"></a>Örnekleri kaldırarak rolü ölçeklendirin

Örnekleri aynı şekilde kaldırarak bir rolü ölçeklendirebilirsiniz. **Ayar-AzureRole'deki** **Kont** parametresini, çalışmadaki ölçek tamamlandıktan sonra sahip olmak istediğiniz örnek sayısına ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar

PowerShell'den bulut hizmetleri için otomatik ölçeklendirme yapmak mümkün değildir. Bunu yapmak için bulut [hizmetini otomatik olarak nasıl ölçeklendirin.](cloud-services-how-to-scale-portal.md)
