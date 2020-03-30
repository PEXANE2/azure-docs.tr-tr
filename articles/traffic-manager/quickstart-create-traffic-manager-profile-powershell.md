---
title: 'Hızlı başlangıç: Yüksek kullanılabilirlik için bir profil oluşturma - Azure PowerShell - Azure Trafik Yöneticisi'
description: Bu hızlı başlangıç makalesi, yüksek kullanılabilir bir web uygulaması oluşturmak için trafik yöneticisi profilinin nasıl oluşturulacağını açıklar.
services: traffic-manager
author: rohinkoul
mnager: twooley
Customer intent: As an IT admin, I want to direct user traffic to ensure high availability of web applications.
ms.service: traffic-manager
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/04/2019
ms.author: rohink
ms.openlocfilehash: 0ab7392b4fa6e248d51392706fedaed156344a99
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76934813"
---
# <a name="quickstart-create-a-traffic-manager-profile-for-a-highly-available-web-application-using-azure-powershell"></a>Hızlı başlatma: Azure PowerShell'i kullanarak yüksek kullanılabilir bir web uygulaması için Trafik Yöneticisi profili oluşturma

Bu hızlı başlatma, web uygulamanız için yüksek kullanılabilirlik sağlayan bir Trafik Yöneticisi profilinin nasıl oluşturulacağını açıklar.

Bu hızlı başlangıçta, bir web uygulamasının iki örneğini oluşturursunuz. Her biri farklı bir Azure bölgesinde çalışıyor. [Bitiş noktası önceliğine](traffic-manager-routing-methods.md#priority-traffic-routing-method)dayalı bir Trafik Yöneticisi profili oluşturursunuz. Profil, kullanıcı trafiğini web uygulamasını çalıştıran birincil siteye yönlendirir. Trafik Yöneticisi web uygulamasını sürekli olarak izler. Birincil site kullanılamıyorsa, yedekleme sitesine otomatik olarak başarısız olur.

Azure aboneliğiniz yoksa şimdi [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure PowerShell modülü 5.4.1 veya sonraki bir sürümünü gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak Grubu oluşturma
[Yeni-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)kullanarak bir kaynak grubu oluşturun.

```azurepowershell-interactive


# Variables
$Location1="WestUS"

# Create a Resource Group
New-AzResourceGroup -Name MyResourceGroup -Location $Location1
```

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

Uç nokta önceliğine göre kullanıcı trafiğini yönlendiren [New-AzTrafficManagerProfile'ı](/powershell/module/az.trafficmanager/new-aztrafficmanagerprofile) kullanarak bir Trafik Yöneticisi profili oluşturun.

```azurepowershell-interactive

# Generates a random value
$Random=(New-Guid).ToString().Substring(0,8)
$mytrafficmanagerprofile="mytrafficmanagerprofile$Random"

New-AzTrafficManagerProfile `
-Name $mytrafficmanagerprofile `
-ResourceGroupName MyResourceGroup `
-TrafficRoutingMethod Priority `
-MonitorPath '/' `
-MonitorProtocol "HTTP" `
-RelativeDnsName $mytrafficmanagerprofile `
-Ttl 30 `
-MonitorPort 80
```

## <a name="create-web-apps"></a>Web Uygulamaları Oluştur

Bu hızlı başlangıç için, iki farklı Azure bölgesinde *(Batı ABD* ve *Doğu ABD)* dağıtılan bir web uygulamasının iki örneğine ihtiyacınız vardır. Her biri Trafik Yöneticisi için birincil ve başarısız uç nokta olarak hizmet verecektir.

### <a name="create-web-app-service-plans"></a>Web App Service planları oluşturma
İki farklı Azure bölgesinde dağıtacağınız web uygulamasının iki örneği için [Yeni AzAppServicePlan](/powershell/module/az.websites/new-azappserviceplan) kullanarak Web Uygulaması hizmet planları oluşturun.

```azurepowershell-interactive

# Variables
$App1Name="AppServiceTM1$Random"
$App2Name="AppServiceTM2$Random"
$Location1="WestUS"
$Location2="EastUS"

# Create an App service plan
New-AzAppservicePlan -Name "$App1Name-Plan" -ResourceGroupName MyResourceGroup -Location $Location1 -Tier Standard
New-AzAppservicePlan -Name "$App2Name-Plan" -ResourceGroupName MyResourceGroup -Location $Location2 -Tier Standard

```
### <a name="create-a-web-app-in-the-app-service-plan"></a>Uygulama Hizmet Planında Bir Web Uygulaması Oluşturma
*Batı ABD* ve Doğu *ABD* Azure bölgelerindeki Uygulama Hizmeti planlarında [New-AzWebApp'ı](/powershell/module/az.websites/new-azwebapp) kullanarak web uygulamasının iki örneği oluşturun.

```azurepowershell-interactive
$App1ResourceId=(New-AzWebApp -Name $App1Name -ResourceGroupName MyResourceGroup -Location $Location1 -AppServicePlan "$App1Name-Plan").Id
$App2ResourceId=(New-AzWebApp -Name $App2Name -ResourceGroupName MyResourceGroup -Location $Location2 -AppServicePlan "$App2Name-Plan").Id

```

## <a name="add-traffic-manager-endpoints"></a>Traffic Manager uç noktalarını ekleme
[Yeni-AzTrafficManagerEndpoint](/powershell/module/az.trafficmanager/new-aztrafficmanagerendpoint) kullanarak Trafik Yöneticisi bitiş noktaları olarak iki Web Apps ekleyin Trafik Yöneticisi profiline aşağıdaki gibi:
- Tüm kullanıcı trafiğini yönlendirmek için birincil bitiş noktası olarak *Batı ABD* Azure bölgesinde bulunan Web Uygulamasını ekleyin. 
- *Doğu ABD* Azure bölgesinde bulunan Web Uygulamasını bitiş noktası olarak ekleyin. Birincil bitiş noktası kullanılamıyorsa, trafik otomatik olarak başarısız bitiş noktasına yönlendirir.

```azurepowershell-interactive
New-AzTrafficManagerEndpoint -Name "$App1Name-$Location1" `
-ResourceGroupName MyResourceGroup `
-ProfileName "$mytrafficmanagerprofile" `
-Type AzureEndpoints `
-TargetResourceId $App1ResourceId `
-EndpointStatus "Enabled"

New-AzTrafficManagerEndpoint -Name "$App2Name-$Location2" `
-ResourceGroupName MyResourceGroup `
-ProfileName "$mytrafficmanagerprofile" `
-Type AzureEndpoints `
-TargetResourceId $App2ResourceId `
-EndpointStatus "Enabled"
```

## <a name="test-traffic-manager-profile"></a>Traffic Manager profilini test etme

Bu bölümde, Trafik Yöneticisi profilinizin alan adını kontrol eedeceksiniz. Ayrıca, birincil bitiş noktasını kullanılamaz olarak da yapılandıracaksınız. Son olarak, web uygulamasının hala kullanılabilir olduğunu görebilirsiniz. Çünkü Trafik Yöneticisi trafiği başarısız bitiş noktasına gönderir.

### <a name="determine-the-dns-name"></a>DNS adını belirleme

[Get-AzTrafficManagerProfile'ı](/powershell/module/az.trafficmanager/get-aztrafficmanagerprofile)kullanarak Trafik Yöneticisi profilinin DNS adını belirleyin.

```azurepowershell-interactive
Get-AzTrafficManagerProfile -Name $mytrafficmanagerprofile `
-ResourceGroupName MyResourceGroup
```

**RelativeDnsName** değerini kopyalayın. Trafik Yöneticisi profilinin DNS adı *http://<* relativednsname *>.trafficmanager.net*. 

### <a name="view-traffic-manager-in-action"></a>Traffic Manager'ın nasıl çalıştığını görün
1. Web Uygulamanızın varsayılan web sitesini görüntülemek için trafik yöneticisi profilinizin DNS adını *(http://<* relativednsname *>.trafficmanager.net)* girin.

    > [!NOTE]
    > Bu hızlı başlangıç senaryosunda, tüm istekler birincil bitiş noktasına yönlendirin. **Öncelik 1**olarak ayarlanır.
2. Trafik Yöneticisi'nin başarısız olduğunu görüntülemek için, [Devre Dışı-AzTrafficManagerEndpoint'i](/powershell/module/az.trafficmanager/disable-aztrafficmanagerendpoint)kullanarak birincil sitenizi devre dışı edin.

   ```azurepowershell-interactive
    Disable-AzTrafficManagerEndpoint -Name $App1Name-$Location1 `
    -Type AzureEndpoints `
    -ProfileName $mytrafficmanagerprofile `
    -ResourceGroupName MyResourceGroup `
    -Force
   ```
3. Web sitesini yeni bir web tarayıcısı oturumunda görüntülemek için Trafik Yöneticisi profilinizin DNS adını *(http://<* relativednsname *>.trafficmanager.net)* kopyalayın.
4. Web uygulamasının hala kullanılabilir olduğunu doğrulayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

İşi bittiğinde, [Remove-AzResourceGroup'u](/powershell/module/az.resources/remove-azresourcegroup)kullanarak kaynak gruplarını, web uygulamalarını ve ilgili tüm kaynakları silin.

```azurepowershell-interactive
Remove-AzResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, web uygulamanız için yüksek kullanılabilirlik sağlayan bir Trafik Yöneticisi profili oluşturdunuz. Trafiği yönlendirme hakkında daha fazla bilgi edinmek için Trafik Yöneticisi öğreticilerine devam edin.

> [!div class="nextstepaction"]
> [Traffic Manager öğreticileri](tutorial-traffic-manager-improve-website-response.md)
