---
title: PowerShell ile uygulama kopyalama
description: PowerShell'i kullanarak Uygulama Hizmeti uygulamanızı yeni bir uygulamaya nasıl klonlayınızı öğrenin. Trafik Yöneticisi tümleştirmesi de dahil olmak üzere çeşitli klonlama senaryoları kapsanmaktadır.
ms.assetid: f9a5cfa1-fbb0-41e6-95d1-75d457347a35
ms.topic: article
ms.date: 01/14/2016
ms.custom: seodec18
ms.openlocfilehash: e7ad45ea4cb1049ed7eeb454162e23e81ed35019
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78255197"
---
# <a name="azure-app-service-app-cloning-using-powershell"></a>PowerShell kullanarak Azure Uygulama Hizmeti Uygulaması Klonlama

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Microsoft Azure PowerShell sürüm 1.1.0 sürümüyle, varolan `New-AzWebApp` bir Uygulama Hizmeti uygulamasını farklı bir bölgede veya aynı bölgede yeni oluşturulan bir uygulamaya klonlamanızı sağlayan yeni bir seçenek eklendi. Bu seçenek, müşterilerin farklı bölgelerde ki uygulamaları hızlı ve kolay bir şekilde dağıtmasına olanak tanır.

Uygulama klonlama, Standart, Premium, Premium V2 ve İzole uygulama hizmet planları için desteklenir. Yeni özellik, App Service Backup özelliğiyle aynı sınırlamaları kullanır, bkz. [Azure Uygulama Hizmeti'nde bir uygulamayı yedekle.](manage-backup.md)

## <a name="cloning-an-existing-app"></a>Varolan bir uygulamayı klonlama
Senaryo: Güney Orta ABD bölgesinde varolan bir uygulama ve Kuzey Orta ABD bölgesinde yeni bir uygulama için içeriğini klonlamak istiyorum. PowerShell cmdlet'in Azure Kaynak Yöneticisi sürümünü kullanarak `-SourceWebApp` seçeneği olan yeni bir uygulama oluşturmak için gerçekleştirilebilir.

Kaynak uygulamayı içeren kaynak grup adını bilerek, kaynak uygulamanın bilgilerini almak için aşağıdaki PowerShell komutunu kullanabilirsiniz (bu durumda adlandırılmış): `source-webapp`

```powershell
$srcapp = Get-AzWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

Yeni bir Uygulama Hizmet Planı oluşturmak `New-AzAppServicePlan` için aşağıdaki örnekte olduğu gibi komutu kullanabilirsiniz

```powershell
New-AzAppServicePlan -Location "North Central US" -ResourceGroupName DestinationAzureResourceGroup -Name DestinationAppServicePlan -Tier Standard
```

Komutu `New-AzWebApp` kullanarak, Kuzey Orta ABD bölgesinde yeni bir uygulama oluşturabilir ve mevcut bir Uygulama Hizmet Planı'na bağlayabilirsiniz. Ayrıca, kaynak uygulamayla aynı kaynak grubunu kullanabilir veya aşağıdaki komutta gösterildiği gibi yeni bir kaynak grubu tanımlayabilirsiniz:

```powershell
$destapp = New-AzWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp
```

İlişkili tüm dağıtım yuvalarını içeren varolan bir uygulamayı `IncludeSourceWebAppSlots` klonlamak için parametreyi kullanmanız gerekir.  Parametrenin `IncludeSourceWebAppSlots` yalnızca tüm yuvaları da dahil olmak üzere tüm uygulamayı klonlamak için desteklendirdiğini unutmayın. Aşağıdaki PowerShell komutu bu parametrenin `New-AzWebApp` komutla kullanımını gösterir:

```powershell
$destapp = New-AzWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots
```

Aynı bölgede varolan bir uygulamayı klonlamak için, aynı bölgede yeni bir kaynak grubu ve yeni bir uygulama hizmet planı oluşturmanız ve ardından uygulamayı klonlamak için aşağıdaki PowerShell komutunu kullanmanız gerekir:

```powershell
$destapp = New-AzWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcapp
```

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Varolan bir Uygulamayı Uygulama Hizmet Ortamına Klonlama
Senaryo: Güney Orta ABD bölgesinde varolan bir uygulama ve içeriği yeni bir uygulamaya mevcut bir Uygulama Hizmet Ortamına (ASE) klonlamak istiyorsunuz.

Kaynak uygulamayı içeren kaynak grup adını bilerek, kaynak uygulamanın bilgilerini almak için aşağıdaki PowerShell komutunu kullanabilirsiniz (bu durumda adlandırılmış): `source-webapp`

```powershell
$srcapp = Get-AzWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

ASE'nin adını ve ASE'nin ait olduğu kaynak grubu adını bilerek, aşağıdaki komutta gösterildiği gibi mevcut ASE'de yeni uygulamayı oluşturabilirsiniz:

```powershell
$destapp = New-AzWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp
```

Parametre `Location` eski nedenden dolayı gereklidir, ancak uygulamayı bir ASE'de oluşturduğunuzda yoksayılır. 

## <a name="cloning-an-existing-app-slot"></a>Varolan bir Uygulama Yuvasını Klonlama
Senaryo: Bir uygulamanın varolan dağıtım yuvasını yeni bir uygulama veya yeni bir yuvayla klonlamak istiyorsunuz. Yeni uygulama, orijinal uygulama yuvasıyla aynı bölgede veya farklı bir bölgede olabilir.

Kaynak uygulamayı içeren kaynak grup adını bilerek, kaynak uygulama yuvasının bilgilerini almak için aşağıdaki PowerShell komutunu kullanabilirsiniz (bu durumda adlandırılmış) `source-appslot` `source-app`aşağıdaki:

```powershell
$srcappslot = Get-AzWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-app -Slot source-appslot
```

Aşağıdaki komut, yeni bir uygulamaya kaynak uygulamanın bir klonu oluşturmayı gösterir:

```powershell
$destapp = New-AzWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-app -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot
```

## <a name="configuring-traffic-manager-while-cloning-an-app"></a>Bir uygulamayı klonlarken Trafik Yöneticisini yapılandırma
Çok bölgeli uygulamalar oluşturmak ve Azure Trafik Yöneticisi'ni trafiği tüm bu uygulamalara yönlendirecek şekilde yapılandırmak, müşterilerin uygulamalarının yüksek oranda kullanılabilir olmasını sağlamak için önemli bir senaryodur. Varolan bir uygulamayı klonlarken, her iki uygulamayı da yeni bir trafik yöneticisi profiline veya varolan bir uygulamaya bağlama seçeneğiniz var. Trafik Yöneticisi'nin yalnızca Azure Kaynak Yöneticisi sürümü desteklenir.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-an-app"></a>Uygulamayı klonlarken yeni bir Trafik Yöneticisi profili oluşturma
Senaryo: Bir uygulamayı başka bir bölgeye klonlarken, her iki uygulamayı da içeren bir Azure Kaynak Yöneticisi trafik yöneticisi profilini yapılandırmak istiyorsunuz. Aşağıdaki komut, yeni bir Trafik Yöneticisi profilini yapılandırırken yeni bir uygulamaya kaynak uygulamanın bir klonunu oluşturmayı gösterir:

```powershell
$destapp = New-AzWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile
```

### <a name="adding-new-cloned-app-to-an-existing-traffic-manager-profile"></a>Varolan bir Trafik Yöneticisi profiline yeni klonlanmış uygulama ekleme
Senaryo: Zaten bir Azure Kaynak Yöneticisi trafik yöneticisi profiliniz var ve her iki uygulamayı da bitiş noktası olarak eklemek istiyorsunuz. Bunu yapmak için öncelikle varolan trafik yöneticisi profil kimliğini bir araya getirmeniz gerekir. Abonelik kimliğine, kaynak grubu adına ve varolan trafik yöneticisi profil adına ihtiyacınız var.

```powershell
$TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"
```

Trafik yöneticisi kimliğine sahip olduktan sonra, aşağıdaki komut, kaynak uygulamanın bir klonunu yeni bir uygulamaya oluştururken, bunları mevcut bir Trafik Yöneticisi profiline ekler:

```powershell
$destapp = New-AzWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID
```

## <a name="current-restrictions"></a>Güncel Kısıtlamalar
Uygulama klonlamanın bilinen kısıtlamaları şunlardır:

* Otomatik ölçek ayarları klonlanmaz
* Yedekleme zamanlama ayarları klonlanmış değil
* VNET ayarları klonlanmaz
* App Insights hedef uygulamada otomatik olarak ayarlanmaz
* Kolay Auth ayarları klonlanmış değil
* Kudu Uzantısı klonlanmış değildir
* TiP kuralları klonlanmaz
* Veritabanı içeriği klonlanmaz
* Farklı bir ölçek birimine klonlama varsa Giden IP Adresleri değişiklikleri
* Linux Uygulamaları için kullanılamaz

### <a name="references"></a>Başvurular
* [Uygulama Hizmeti Klonlama](app-service-web-app-cloning.md)
* [Azure Uygulama Hizmeti'nde bir uygulamayı yedekleme](manage-backup.md)
* [Azure Trafik Yöneticisi Önizlemeiçin Azure Kaynak Yöneticisi desteği](../traffic-manager/traffic-manager-powershell-arm.md)
* [App Service Ortamı’na giriş](environment/intro.md)
* [Azure PowerShell’i Azure Resource Manager ile kullanma](../azure-resource-manager/management/manage-resources-powershell.md)

