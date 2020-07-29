---
title: Azure Service Bus kaynaklarını yönetmek için PowerShell 'i kullanma | Microsoft Docs
description: Bu makalede, Service Bus varlıkları (ad alanları, kuyruklar, konular, abonelikler) oluşturmak ve yönetmek için Azure PowerShell modülün nasıl kullanılacağı açıklanmaktadır.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 7ff79d8cf2aac09a0b8d2f04ade0b3d1d9b2e74e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85341745"
---
# <a name="use-powershell-to-manage-service-bus-resources"></a>Service Bus kaynaklarını yönetmek için PowerShell 'i kullanma

Microsoft Azure PowerShell, Azure hizmetlerinin dağıtımını ve yönetimini denetlemek ve otomatikleştirmek için kullanabileceğiniz bir betik ortamıdır. Bu makalede, yerel bir Azure PowerShell konsolu veya betiği kullanarak Service Bus varlıkları (ad alanları, kuyruklar, konular ve abonelikler) sağlamak ve yönetmek için [Service Bus Kaynak Yöneticisi PowerShell modülünün](/powershell/module/az.servicebus) nasıl kullanılacağı açıklanır.

Ayrıca, Azure Resource Manager şablonları kullanarak Service Bus varlıklarını yönetebilirsiniz. Daha fazla bilgi için [Azure Resource Manager şablonları kullanarak Service Bus kaynakları oluşturma](service-bus-resource-manager-overview.md)makalesine bakın.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdaki önkoşullara sahip olmanız gerekir:

* Azure aboneliği. Abonelik edinme hakkında daha fazla bilgi için bkz. [satın alma seçenekleri][purchase options], [üye teklifleri][member offers]veya [ücretsiz hesap][free account].
* Azure PowerShell olan bir bilgisayar. Yönergeler için bkz. [Azure PowerShell cmdlet 'leri ile çalışmaya başlama](/powershell/azure/get-started-azureps).
* PowerShell betikleri, NuGet paketleri ve .NET Framework genel olarak anlaşılmasıdır.

## <a name="get-started"></a>başlarken

İlk adım, PowerShell 'i kullanarak Azure hesabınızda ve Azure aboneliğinizde oturum açmak için kullanılır. Azure hesabınızda oturum açmak ve Azure aboneliğinizdeki kaynakları almak ve bu kaynaklara erişmek için [Azure PowerShell cmdlet 'leriyle çalışmaya başlama](/powershell/azure/get-started-azureps) bölümündeki yönergeleri izleyin.

## <a name="provision-a-service-bus-namespace"></a>Service Bus ad alanı sağlama

Service Bus ad alanlarıyla çalışırken, [Get-AzServiceBusNamespace](/powershell/module/az.servicebus/get-azservicebusnamespace), [New-azservicebusnamespace](/powershell/module/az.servicebus/new-azservicebusnamespace), [Remove-Azservicebusnamespace](/powershell/module/az.servicebus/remove-azservicebusnamespace)ve [set-azservicebusnamespace](/powershell/module/az.servicebus/set-azservicebusnamespace) cmdlet 'lerini kullanabilirsiniz.

Bu örnek, betikte birkaç yerel değişken oluşturur; `$Namespace`ve `$Location` .

* `$Namespace`, çalışmak istediğimiz Service Bus ad alanının adıdır.
* `$Location`ad alanını sağladığımız veri merkezini tanımlar.
* `$CurrentNamespace`elde ettiğimiz başvuru ad alanını depolar (veya oluşturur).

Gerçek bir betikte `$Namespace` ve `$Location` parametre olarak geçirilebilir.

Komut dosyasının bu bölümü şunları yapar:

1. Belirtilen ada sahip bir Service Bus ad alanı almaya çalışır.
2. Ad alanı bulunursa, nelerin bulunmuş olduğunu bildirir.
3. Ad alanı bulunamazsa, ad alanını oluşturur ve ardından yeni oluşturulan ad alanını alır.
   
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

Aşağıdaki örnek, [New-azservicebusauthorizationrule](/powershell/module/az.servicebus/new-azservicebusauthorizationrule), [Get-azservicebusauthorizationrule](/powershell/module/az.servicebus/get-azservicebusauthorizationrule), [set-Azservicebusauthorizationrule](/powershell/module/az.servicebus/set-azservicebusauthorizationrule)ve [Remove-azservicebusauthorizationrule](/powershell/module/az.servicebus/remove-azservicebusauthorizationrule) cmdlet 'lerini kullanarak ad alanı yetkilendirme kurallarının nasıl yönetileceğini gösterir.

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

Bir kuyruk veya konu oluşturmak için önceki bölümde yer alan betiği kullanarak bir ad alanı denetimi gerçekleştirin. Ardından kuyruğu oluşturun:

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

Önceki bölümde betiği yürüttükten sonra, aşağıdaki örnekte olduğu gibi, bir kuyruğun özelliklerini güncelleştirmek için [set-AzServiceBusQueue](/powershell/module/az.servicebus/set-azservicebusqueue) cmdlet 'ini kullanabilirsiniz:

```powershell
$CurrentQ.DeadLetteringOnMessageExpiration = $True
$CurrentQ.MaxDeliveryCount = 7
$CurrentQ.MaxSizeInMegabytes = 2048
$CurrentQ.EnableExpress = $True

Set-AzServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -QueueObj $CurrentQ
```

## <a name="provisioning-other-service-bus-entities"></a>Diğer Service Bus varlıklarını sağlama

[Service Bus PowerShell modülünü](/powershell/module/az.servicebus) , konular ve abonelikler gibi diğer varlıkları sağlamak için kullanabilirsiniz. Bu cmdlet 'ler, önceki bölümde gösterilen sıra oluşturma cmdlet 'lerine benzer bir şekilde benzerdir.

## <a name="next-steps"></a>Sonraki adımlar

- Service Bus Kaynak Yöneticisi PowerShell modülünün [Tüm belgelerine bakın](/powershell/module/az.servicebus). Bu sayfa tüm kullanılabilir cmdlet 'leri listeler.
- Azure Resource Manager şablonlarını kullanma hakkında daha fazla bilgi için [Azure Resource Manager şablonları kullanarak Service Bus kaynakları oluşturma](service-bus-resource-manager-overview.md)makalesine bakın.
- [Service Bus .NET Yönetim kitaplıkları](service-bus-management-libraries.md)hakkında bilgi.

Aşağıdaki blog gönderilerinde açıklandığı gibi Service Bus varlıklarını yönetmenin bazı farklı yolları vardır:

* [PowerShell betiği kullanarak Service Bus kuyrukları, konuları ve abonelikleri oluşturma](https://docs.microsoft.com/archive/blogs/paolos/how-to-create-service-bus-queues-topics-and-subscriptions-using-a-powershell-script)
* [PowerShell betiği kullanarak Service Bus ad alanı ve Olay Hub 'ı oluşturma](https://docs.microsoft.com/archive/blogs/paolos/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script)
* [Service Bus PowerShell betikleri](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[purchase options]: https://azure.microsoft.com/pricing/purchase-options/
[member offers]: https://azure.microsoft.com/pricing/member-offers/
[free account]: https://azure.microsoft.com/pricing/free-trial/
