---
title: Azure Hizmet Veri Servisi kaynaklarını yönetmek için PowerShell'i kullanın | Microsoft Dokümanlar
description: Bu makalede, Hizmet Veri Hizmeti kuruluşları (ad alanları, kuyruklar, konular, abonelikler) oluşturmak ve yönetmek için Azure PowerShell modülünasıl kullanılacağı açıklanmaktadır.
services: service-bus-messaging
documentationcenter: .NET
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2020
ms.author: aschhab
ms.openlocfilehash: e333dfb109840538fd5dec8110e1c32adedce989
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76759270"
---
# <a name="use-powershell-to-manage-service-bus-resources"></a>Servis Veri Hizmeti veri veri veri salı kaynaklarını yönetmek için PowerShell'i kullanın

Microsoft Azure PowerShell, Azure hizmetlerinin dağıtımını ve yönetimini denetlemek ve otomatikleştirmek için kullanabileceğiniz bir komut dosyası ortamıdır. Bu makalede, yerel bir Azure PowerShell konsolu veya komut dosyası kullanarak Hizmet Veri Servisi Veri Servisi varlıklarını (ad alanları, kuyruklar, konular ve abonelikler) sağlamak ve yönetmek için [Service Bus Resource Manager PowerShell modülünün](/powershell/module/az.servicebus) nasıl kullanılacağı açıklanmaktadır.

Azure Kaynak Yöneticisi şablonlarını kullanarak Hizmet Veri Mes'leri varlıklarını da yönetebilirsiniz. Daha fazla bilgi için Azure [Kaynak Yöneticisi şablonlarını kullanarak Hizmet Veri Meskeni Kaynakları Oluştur makalesine](service-bus-resource-manager-overview.md)bakın.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdaki ön koşullara ihtiyacınız vardır:

* Azure aboneliği. Abonelik alma hakkında daha fazla bilgi için [satın alma seçeneklerine,][purchase options] [üye tekliflerine][member offers]veya [ücretsiz hesaba][free account]bakın.
* Azure PowerShell'e sahip bir bilgisayar. Talimatlar için bkz: [Azure PowerShell cmdlets ile başlayın.](/powershell/azure/get-started-azureps)
* PowerShell komut dosyalarının, NuGet paketlerinin ve .NET Framework'ün genel bir anlayışı.

## <a name="get-started"></a>Kullanmaya başlayın

İlk adım, Azure hesabınızda ve Azure aboneliğinizde oturum açmak için PowerShell'i kullanmaktır. Azure hesabınızda oturum açmak ve Azure aboneliğinizdeki kaynakları almak ve erişmek için [Azure PowerShell cmdlets ile başlayın'daki](/powershell/azure/get-started-azureps) yönergeleri izleyin.

## <a name="provision-a-service-bus-namespace"></a>Hizmet Veri Günü ad alanı sağlama

Service Bus ad alanları yla çalışırken [Get-AzServiceBusNamespace,](/powershell/module/az.servicebus/get-azservicebusnamespace) [New-AzServiceBusNamespace,](/powershell/module/az.servicebus/new-azservicebusnamespace) [Remove-AzServiceBusNamespace](/powershell/module/az.servicebus/remove-azservicebusnamespace)ve [Set-AzServiceBusNamespace](/powershell/module/az.servicebus/set-azservicebusnamespace) cmdlets'i kullanabilirsiniz.

Bu örnek, komut dosyasında birkaç yerel değişken oluşturur; `$Namespace` ve `$Location`.

* `$Namespace`çalışmak istediğimiz Servis Veri Günü ad alanının adıdır.
* `$Location`ad alanını sağladığımız veri merkezini tanımlar.
* `$CurrentNamespace`aldığımız (veya oluşturduğumuz referans ad alanını) depolar.

Gerçek bir komut `$Namespace` `$Location` dosyasında ve parametreler olarak geçirilebilir.

Komut dosyasının bu bölümü aşağıdakileri yapar:

1. Belirtilen ada sahip bir Hizmet Veri Günü ad alanı almaya çalışır.
2. Ad alanı bulunursa, ne bulunduğu bildirilir.
3. Ad alanı bulunamazsa, ad alanı oluşturur ve sonra yeni oluşturulan ad alanını alır.
   
    ``` powershell
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
   
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Host "The namespace $Namespace already exists in the $Location region:"
        # Report what was found
        Get-AzServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "The $Namespace namespace does not exist."
        Write-Host "Creating the $Namespace namespace in the $Location region..."
        New-AzServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
        Write-Host "The $Namespace namespace in Resource Group $ResGrpName in the $Location region has been successfully created."
                
    }
    ```

### <a name="create-a-namespace-authorization-rule"></a>Ad alanı yetkilendirme kuralı oluşturma

Aşağıdaki örnek, [New-AzServiceBusAuthorizationRule](/powershell/module/az.servicebus/new-azservicebusauthorizationrule), [Get-AzServiceBusAuthorizationRule , Set-AzServiceBusAuthorizationRule](/powershell/module/az.servicebus/get-azservicebusauthorizationrule)ve [Remove-AzServiceBusAuthorizationRule](/powershell/module/az.servicebus/remove-azservicebusauthorizationrule) cmdlets kullanarak ad alanı yetkilendirme kurallarının nasıl yönetilen gösterir. [Set-AzServiceBusAuthorizationRule](/powershell/module/az.servicebus/set-azservicebusauthorizationrule)

```powershell
# Query to see if rule exists
$CurrentRule = Get-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

# Check if the rule already exists or needs to be created
if ($CurrentRule)
{
    Write-Host "The $AuthRule rule already exists for the namespace $Namespace."
}
else
{
    Write-Host "The $AuthRule rule does not exist."
    Write-Host "Creating the $AuthRule rule for the $Namespace namespace..."
    New-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule -Rights @("Listen","Send")
    $CurrentRule = Get-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
    Write-Host "The $AuthRule rule for the $Namespace namespace has been successfully created."

    Write-Host "Setting rights on the namespace"
    $authRuleObj = Get-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

    Write-Host "Remove Send rights"
    $authRuleObj.Rights.Remove("Send")
    Set-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Add Send and Manage rights to the namespace"
    $authRuleObj.Rights.Add("Send")
    Set-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj
    $authRuleObj.Rights.Add("Manage")
    Set-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Show value of primary key"
    $CurrentKey = Get-AzServiceBusKey -ResourceGroup $ResGrpName -NamespaceName $Namespace -Name $AuthRule
        
    Write-Host "Remove this authorization rule"
    Remove-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -Name $AuthRule
}
```

## <a name="create-a-queue"></a>Bir kuyruk oluşturma

Bir sıra veya konu oluşturmak için, önceki bölümdeki komut dosyasını kullanarak bir ad alanı denetimi gerçekleştirin. Ardından sırayı oluşturun:

```powershell
# Check if queue already exists
$CurrentQ = Get-AzServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName

if($CurrentQ)
{
    Write-Host "The queue $QueueName already exists in the $Location region:"
}
else
{
    Write-Host "The $QueueName queue does not exist."
    Write-Host "Creating the $QueueName queue in the $Location region..."
    New-AzServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -EnablePartitioning $True
    $CurrentQ = Get-AzServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName
    Write-Host "The $QueueName queue in Resource Group $ResGrpName in the $Location region has been successfully created."
}
```

### <a name="modify-queue-properties"></a>Sıra özelliklerini değiştirme

Önceki bölümde komut dosyası çalıştırdıktan sonra, aşağıdaki örnekte olduğu gibi, bir sıranın özelliklerini güncelleştirmek için [Set-AzServiceBusQueue](/powershell/module/az.servicebus/set-azservicebusqueue) cmdlet kullanabilirsiniz:

```powershell
$CurrentQ.DeadLetteringOnMessageExpiration = $True
$CurrentQ.MaxDeliveryCount = 7
$CurrentQ.MaxSizeInMegabytes = 2048
$CurrentQ.EnableExpress = $True

Set-AzServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -QueueObj $CurrentQ
```

## <a name="provisioning-other-service-bus-entities"></a>Diğer Servis Veri Günü kuruluşlarının sağlanması

Hizmet Veri [Servisi PowerShell modüllerini,](/powershell/module/az.servicebus) konular ve abonelikler gibi diğer varlıkları sağlamak için kullanabilirsiniz. Bu cmdletler bir önceki bölümde gösterilen kuyruk oluşturma cmdletlerine sözdizimolarak benzerdir.

## <a name="next-steps"></a>Sonraki adımlar

- Servis Veri Kurumu Kaynak Yöneticisi PowerShell modül belgelerinin [tamamına buradan bakınız.](/powershell/module/az.servicebus) Bu sayfa, mevcut tüm cmdletleri listeler.
- Azure Kaynak Yöneticisi şablonlarını kullanma hakkında bilgi için, [Azure Kaynak Yöneticisi şablonlarını kullanarak Hizmet Veri Servisi kaynakları oluştur makalesine](service-bus-resource-manager-overview.md)bakın.
- Servis [Veri Kurumu .NET yönetim kitaplıkları](service-bus-management-libraries.md)hakkında bilgi.

Bu blog gönderilerinde açıklandığı gibi, Hizmet Veri Yolu varlıklarını yönetmenin bazı alternatif yolları vardır:

* [PowerShell komut dosyası kullanarak Servis Veri Servisi kuyrukları, konular ve abonelikler nasıl oluşturulur?](https://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [PowerShell komut dosyası kullanarak Hizmet Veri Merkezi Ad Alanı ve Etkinlik Hub'ı oluşturma](https://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)
* [Servis Otobüsü PowerShell Scripts](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[purchase options]: https://azure.microsoft.com/pricing/purchase-options/
[member offers]: https://azure.microsoft.com/pricing/member-offers/
[free account]: https://azure.microsoft.com/pricing/free-trial/
