---
title: PowerShell'i kullanarak Bildirim Hub'larını dağıtma ve yönetme
description: Otomasyon için PowerShell'i kullanarak Bildirim Hub'ları oluşturma ve yönetme
services: notification-hubs
documentationcenter: ''
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: 7c58f2c8-0399-42bc-9e1e-a7f073426451
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 863fdb445cce41f0fe4cbee63a3d6198c0a79339
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76264653"
---
# <a name="deploy-and-manage-notification-hubs-using-powershell"></a>PowerShell'i kullanarak bildirim hub'larını dağıtma ve yönetme

## <a name="overview"></a>Genel Bakış

Bu makalede, PowerShell kullanarak Azure Bildirim Hub'larını oluştur ve yönet' i nasıl kullanacağınızı gösterilmektedir. Bu makalede, aşağıdaki ortak otomasyon görevleri gösterilmiştir.

- Bildirim Merkezi Oluşturma
- Kimlik Bilgilerini Ayarla

Bildirim hub'larınız için yeni bir servis veri günü adı alanı oluşturmanız gerekiyorsa, [PowerShell ile Servis Veri Veri Tobunu Yönet'e](../service-bus-messaging/service-bus-powershell-how-to-provision.md)bakın.

Bildirim Hub'larını Yönetme, Azure PowerShell ile birlikte verilen cmdlets tarafından doğrudan desteklenmez. PowerShell'in en iyi yaklaşımı Microsoft.Azure.NotificationHubs.dll derlemesine başvurmaktır. Derleme, [Microsoft Azure Bildirim Hub'ları NuGet paketiyle](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)dağıtılır.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Azure, abonelik tabanlı bir platformdur. Abonelik alma hakkında daha fazla bilgi için [Satın Alma Seçenekleri,] [Üye Teklifleri]veya [Ücretsiz Deneme'ye]bakın.
- Azure PowerShell'e sahip bir bilgisayar. Talimatlar için Azure [PowerShell'i Yükle ve yapılandırma]'ya bakın.
- PowerShell komut dosyalarının, NuGet paketlerinin ve .NET Framework'ün genel bir anlayışı.

## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Servis Veri Servisi için .NET montajına başvuru dahil

Azure Bildirim Hub'larını yönetmek henüz Azure PowerShell'deki PowerShell cmdlets'e dahil edilmedi. Bildirim hub'larını sağlamak için Microsoft Azure Bildirim [Hub'ları NuGet paketinde](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)sağlanan .NET istemcisini kullanabilirsiniz.

İlk olarak, komut dosyanızın Visual Studio projesinde NuGet paketi olarak yüklenen **Microsoft.Azure.NotificationHubs.dll** derlemesini bulabileceğinden emin olun. Komut dosyası esnek olmak için şu adımları gerçekleştirir:

1. Çağrıldığı yolu belirler.
2. Adlı `packages`bir klasör bulana kadar yolu geçer. Bu klasör, Visual Studio projeleri için NuGet paketlerini yüklediğinizde oluşturulur.
3. 'li `Microsoft.Azure.NotificationHubs.dll`bir derleme için `packages` klasörü özyinelemeli olarak arar.
4. Türlerin daha sonra kullanAbilmeleri için derlemeye başvurur.

PowerShell komut dosyasında bu adımların nasıl uygulandığı aşağıda açıklanmıştır:

``` powershell
try
{
    # WARNING: Make sure to reference the latest version of Microsoft.Azure.NotificationHubs.dll
    Write-Output "Adding the [Microsoft.Azure.NotificationHubs.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.Azure.NotificationHubs.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.Azure.NotificationHubs.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.Azure.NotificationHubs.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}
```

## <a name="create-the-namespacemanager-class"></a>Sınıfı `NamespaceManager` oluşturma

Bildirim Hub'larını sağlamak için SDK'dan [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager?view=azure-dotnet) sınıfının bir örneğini oluşturun.

Bağlantı dizesini sağlamak için kullanılan bir yetkilendirme kuralını almak için Azure PowerShell ile birlikte bulunan [Get-AzureSBAuthorizationRule] cmdlet'ini kullanabilirsiniz. `NamespaceManager` Örneğine yapılan bir başvuru `$NamespaceManager` değişkende depolanır. `$NamespaceManager`bir bildirim merkezi sağlamak için kullanılır.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```

## <a name="provisioning-a-new-notification-hub"></a>Yeni bir Bildirim Hub'ı sağlama

Yeni bir bildirim hub'ı sağlamak [için Bildirim Hub'ları için .NET API'sini]kullanın.

Komut dosyasının bu bölümünde, dört yerel değişken ayarlayın.

1. `$Namespace`: Bunu, bildirim merkezi oluşturmak istediğiniz ad alanının adına ayarlayın.
2. `$Path`: Bu yolu yeni bildirim merkezinin adına ayarlayın.  Örneğin, "MyHub".
3. `$WnsPackageSid`: Bunu [Windows Geliştirme Merkezi'nden](https://developer.microsoft.com/en-us/windows)Windows Uygulamanız için SID paketine ayarlayın.
4. `$WnsSecretkey`: Bunu [Windows Geliştirme Merkezi'nden](https://developer.microsoft.com/en-us/windows)Windows Uygulamanız için gizli anahtara ayarlayın.

Bu değişkenler, ad alanınıza bağlanmak ve Windows Bildirim Hizmetleri (WNS) bildirimlerini bir Windows Uygulaması için WNS kimlik bilgileriyle işlemek üzere yapılandırılmış yeni bir Bildirim Hub'ı oluşturmak için kullanılır. Paket SID ve gizli anahtar elde etme [Getting Started with Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) hakkında bilgi için bkz.

- Komut dosyası snippet `NamespaceManager` tarafından `$Path` tanımlanan Bildirim Hub olup olmadığını görmek için nesneyi kullanır.
- Yoksa, komut dosyası WNS `NotificationHubDescription` kimlik bilgileriyle birlikte oluşturur `NamespaceManager` ve `CreateNotificationHub` sınıf metoduna geçirir.

``` powershell
$Namespace = "<Enter your namespace>"
$Path  = "<Enter a name for your notification hub>"
$WnsPackageSid = "<your package sid>"
$WnsSecretkey = "<enter your secret key>"

$WnsCredential = New-Object -TypeName Microsoft.Azure.NotificationHubs.WnsCredential -ArgumentList $WnsPackageSid,$WnsSecretkey

# Query the namespace
$CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

# Check if the namespace already exists
if ($CurrentNamespace)
{
    Write-Output "The namespace [$Namespace] in the [$($CurrentNamespace.Region)] region was found."

    # Create the NamespaceManager object used to create a new notification hub
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."

    # Check to see if the Notification Hub already exists
    if ($NamespaceManager.NotificationHubExists($Path))
    {
        Write-Output "The [$Path] notification hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] notification hub in the [$Namespace] namespace."
        $NHDescription = New-Object -TypeName Microsoft.Azure.NotificationHubs.NotificationHubDescription -ArgumentList $Path;
        $NHDescription.WnsCredential = $WnsCredential;
        $NamespaceManager.CreateNotificationHub($NHDescription);
        Write-Output "The [$Path] notification hub was created in the [$Namespace] namespace."
    }
}
else
{
    Write-Host "The [$Namespace] namespace does not exist."
}
```

## <a name="additional-resources"></a>Ek Kaynaklar

- [Service Bus’ı PowerShell ile yönetme](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
- [PowerShell komut dosyası kullanarak Servis Veri Servisi kuyrukları, konular ve abonelikler nasıl oluşturulur?](https://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [PowerShell komut dosyası kullanarak Hizmet Veri Merkezi Ad Alanı ve Etkinlik Hub'ı oluşturma](https://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Bazı hazır komut dosyaları da indirilebilir:

- [Servis Otobüsü PowerShell Scripts](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)

[Satın Alma Seçenekleri]: https://azure.microsoft.com/pricing/purchase-options/
[Üye Teklifleri]: https://azure.microsoft.com/pricing/member-offers/
[Ücretsiz Deneme]: https://azure.microsoft.com/pricing/free-trial/
[Azure PowerShell'i yükleme ve yapılandırma]: /powershell/azureps-cmdlets-docs
[.NET API Bildirim Hub'ları için]: https://docs.microsoft.com/dotnet/api/overview/azure/notification-hubs?view=azure-dotnet
[Get-AzureSBNamespace]: https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azuresbnamespace
[New-AzureSBNamespace]: https://docs.microsoft.com/powershell/module/servicemanagement/azure/new-azuresbnamespace
[Get-AzureSBAuthorizationRule]: https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azuresbauthorizationrule
