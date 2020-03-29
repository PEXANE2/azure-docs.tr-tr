---
title: Azure PowerShell kullanarak Azure Trafik Yöneticisi alt net geçersiz kılma | Microsoft Dokümanlar
description: Bu makale, Trafik Yöneticisi alt net geçersiz kılmanın, Trafiği Yöneticisi profilinin yönlendirme yöntemini geçersiz kılmak için, trafiği, önceden tanımlanmış IP aralığı üzerinden son kullanıcı IP adresine dayalı olarak son kullanıcı IP adresine yönlendiren bir uç noktaya, Azure'u kullanarak uç nokta eşlemelerine nasıl yönlendireceğini anlamanıza yardımcı olacaktır Powershell.
services: traffic-manager
documentationcenter: ''
author: rohinkoul
manager: twooley
ms.topic: article
ms.service: traffic-manager
ms.date: 09/18/2019
ms.author: rohink
ms.openlocfilehash: 323093ec78a9486d19496b0ee90e37cb42eea341
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76938414"
---
# <a name="traffic-manager-subnet-override-using-azure-powershell"></a>Azure Powershell kullanarak Trafik Yöneticisi alt net geçersiz kılma

Trafik Yöneticisi alt net geçersiz kılma, bir profilin yönlendirme yöntemini değiştirmenize olanak tanır.  Geçersiz kılmanın eklenmesi, son kullanıcının IP adresine bağlı olarak trafiği, önceden tanımlanmış bir IP aralığıyla uç nokta eşlemesi ile yönlendirir. 

## <a name="how-subnet-override-works"></a>Alt ağ geçersiz kılma nasıl çalışır?

Alt ağ geçersiz kılmaları bir trafik yöneticisi profiline eklendiğinde, Trafik Yöneticisi önce son kullanıcının IP adresi için bir alt net geçersiz kılma olup olmadığını denetler. Biri bulunursa, kullanıcının DNS sorgusu ilgili bitiş noktasına yönlendirilir.  Eşleme bulunmazsa, Trafik Yöneticisi profilin özgün yönlendirme yöntemine geri döner. 

IP adresi aralıkları CIDR aralıkları (örneğin, 1.2.3.0/24) veya adres aralıkları (örneğin, 1.2.3.4-5.6.7.8) olarak belirtilebilir. Her uç noktayla ilişkili IP aralıkları bu uç noktaya özgü olmalıdır. IP aralıklarının farklı uç noktalar arasında çakışması, profilin Trafik Yöneticisi tarafından reddedilmesine neden olur.

Alt ağı geçersiz kılan iki tür yönlendirme profili vardır:

* **Coğrafi** - Trafik Yöneticisi DNS sorgusunun IP adresi için bir alt ağ geçersiz kılma bulursa, son noktanın durumu ne olursa olsun sorguyu bitiş noktasına yönlendirir.
* **Performans** - Trafik Yöneticisi DNS sorgusunun IP adresi için bir alt ağ geçersiz kılma bulursa, trafiği yalnızca sağlıklıysa bitiş noktasına yönlendirir.  Alt ağ bitiş ucunu geçersiz kılma sağlıklı değilse Trafik Yöneticisi performans yönlendirme sezgisel geri düşecek.

## <a name="create-a-traffic-manager-subnet-override"></a>Trafik Yöneticisi alt ağ geçersiz kılma oluşturma

Trafik Yöneticisi alt ağ geçersiz kılma oluşturmak için, geçersiz kılmanın alt ağlarını Trafik Yöneticisi bitiş noktasına eklemek için Azure PowerShell'i kullanabilirsiniz.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[Azure Bulut Kabuğu'nda](https://shell.azure.com/powershell)veya bilgisayarınızdan PowerShell çalıştırarak takip eden komutları çalıştırabilirsiniz. Azure Bulut Kabuğu ücretsiz bir etkileşimli kabuktır. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. PowerShell'i bilgisayarınızdan çalıştırAcaksanız, 1.0.0 veya sonrası Azure PowerShell modülüne ihtiyacınız vardır. Yüklü sürümü `Get-Module -ListAvailable Az` bulmak için çalıştırabilirsiniz. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız, `Login-AzAccount` Azure'da oturum açabilmek için de çalışmanız gerekir.


1. **Trafik Yöneticisi bitiş noktasını alın:**

    Alt ağ geçersiz kılmayı etkinleştirmek için, geçersiz kılmayı eklemek istediğiniz bitiş noktasını alın ve [Get-AzTrafficManagerEndpoint'i](https://docs.microsoft.com/powershell/module/az.trafficmanager/get-aztrafficmanagerendpoint?view=azps-2.5.0)kullanarak bir değişkende saklayın.

    Ad, ProfileName ve ResourceGroupName'yi değiştirdiğiniz bitiş noktası değerleriyle değiştirin.

    ```powershell

    $TrafficManagerEndpoint = Get-AzTrafficManagerEndpoint -Name "contoso" -ProfileName "ContosoProfile" -ResourceGroupName "ResourceGroup" -Type AzureEndpoints

    ```
2. **IP adresi aralığını bitiş noktasına ekleyin:**
    
    IP adresi aralığını bitiş noktasına eklemek için, aralığı eklemek için [Add-AzTrafficManagerIpAddressRange'i](https://docs.microsoft.com/powershell/module/az.trafficmanager/add-aztrafficmanageripaddressrange?view=azps-2.5.0&viewFallbackFrom=azps-2.4.0) kullanırsınız.

    ```powershell

    ### Add a range of IPs ###
    Add-AzTrafficManagerIPAddressRange -TrafficManagerEndpoint $TrafficManagerEndpoint -First "1.2.3.4" -Last "5.6.7.8"

    ### Add a subnet ###
    Add-AzTrafficManagerIPAddressRange -TrafficManagerEndpoint $TrafficManagerEndpoint -First "9.10.11.0" -Scope 24

    ### Add a range of IPs with a subnet ###
    Add-AzTrafficManagerIPAddressRange -TrafficManagerEndpoint $TrafficManagerEndpoint -First "12.13.14.0" -Last "12.13.14.31" -Scope 27
 
    ```
    Aralıklar eklendikten sonra, bitiş noktasını güncelleştirmek için [Set-AzTrafficManagerEndpoint'i](https://docs.microsoft.com/powershell/module/az.trafficmanager/set-aztrafficmanagerendpoint?view=azps-2.5.0) kullanın.

    ```powershell

    Set-AzTrafficManagerEndpoint -TrafficManagerEndpoint $TrafficManagerEndpoint

    ```
IP adres aralığının kaldırılması [Remove-AzTrafficManagerIpAddressRange](https://docs.microsoft.com/powershell/module/az.trafficmanager/remove-aztrafficmanageripaddressrange?view=azps-2.5.0)kullanılarak tamamlanabilir.

1.  **Trafik Yöneticisi bitiş noktasını alın:**

    Alt ağ geçersiz kılmayı etkinleştirmek için, geçersiz kılmayı eklemek istediğiniz bitiş noktasını alın ve [Get-AzTrafficManagerEndpoint'i](https://docs.microsoft.com/powershell/module/az.trafficmanager/get-aztrafficmanagerendpoint?view=azps-2.5.0)kullanarak bir değişkende saklayın.

    Ad, ProfileName ve ResourceGroupName'yi değiştirdiğiniz bitiş noktası değerleriyle değiştirin.

    ```powershell

    $TrafficManagerEndpoint = Get-AzTrafficManagerEndpoint -Name "contoso" -ProfileName "ContosoProfile" -ResourceGroupName "ResourceGroup" -Type AzureEndpoints

    ```
2. **IP adresi aralığını uç noktadan kaldırın:**

    ```powershell
    
    ### Remove a range of IPs ###
    Remove-AzTrafficManagerIpAddressRange -TrafficManagerEndpoint $TrafficManagerEndpoint -First "1.2.3.4" -Last "5.6.7.8"

    ### Remove a subnet ###
    Remove-AzTrafficManagerIpAddressRange -TrafficManagerEndpoint $TrafficManagerEndpoint -First "9.10.11.0" -Scope 24

    ### Remove a range of IPs with a subnet ###
    Remove-AzTrafficManagerIpAddressRange -TrafficManagerEndpoint $TrafficManagerEndpoint -First "12.13.14.0" -Last "12.13.14.31" -Scope 27

    ```
     Aralıklar kaldırıldıktan sonra bitiş noktasını güncelleştirmek için [Set-AzTrafficManagerEndpoint'i](https://docs.microsoft.com/powershell/module/az.trafficmanager/set-aztrafficmanagerendpoint?view=azps-2.5.0) kullanın.

    ```powershell

    Set-AzTrafficManagerEndpoint -TrafficManagerEndpoint $TrafficManagerEndpoint

    ```

## <a name="next-steps"></a>Sonraki adımlar
Trafik Yöneticisi [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md)hakkında daha fazla bilgi edinin.

[Subnet trafik yönlendirme yöntemi](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods#subnet-traffic-routing-method) hakkında bilgi edinin
