---
title: Rol için Uzak Masaüstü'nü etkinleştirmek için PowerShell'i kullanma
titleSuffix: Azure Cloud Services
description: Uzak masaüstü bağlantılarına izin vermek için PowerShell'i kullanarak azure bulut hizmeti uygulamanızı yapılandırma
services: cloud-services
documentationcenter: ''
author: tgore03
ms.service: cloud-services
ms.topic: article
ms.date: 07/18/2017
ms.author: tagore
ms.openlocfilehash: e4e8dca6c5359e865e6a17fc47fe47802b0ee9e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75386128"
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a>PowerShell'i kullanarak Azure Bulut Hizmetlerinde Rol Için Uzak Masaüstü Bağlantısını Etkinleştirin

> [!div class="op_single_selector"]
> * [Azure portalında](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [Powershell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md)

Uzak Masaüstü, Azure'da çalışan bir rolün masaüstüne erişmenizi sağlar. Çalışırken uygulamanızla ilgili sorunları gidermek ve tanılamak için Uzak Masaüstü bağlantısını kullanabilirsiniz.

Bu makalede, PowerShell'i kullanarak Bulut Hizmeti Rollerinizde uzak masaüstünü nasıl etkinleştiriniz açıklanmaktadır. Bu makale için gereken ön koşullar için [Azure PowerShell'i nasıl yükleyip yapılandırılabildiğini](/powershell/azure/overview) öğrenin. PowerShell, uygulama dağıtıldıktan sonra Uzak Masaüstü'nü etkinleştirebilmeniz için Uzak Masaüstü Uzantısı'nı kullanır.

## <a name="configure-remote-desktop-from-powershell"></a>PowerShell'den Uzak Masaüstünü Yapılandırma
[Set-AzureServiceRemoteDesktopExtension](/powershell/module/servicemanagement/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet, Uzak Masaüstü'nü belirtilen rollerde veya bulut hizmeti dağıtımınızın tüm rollerinde etkinleştirmenizi sağlar. Cmdlet, pscredential nesnesi kabul eden *Kimlik Bilgisi* parametresi aracılığıyla uzak masaüstü kullanıcısının Kullanıcı Adı ve Parolasını belirtmenizi sağlar.

PowerShell'i etkileşimli olarak kullanıyorsanız, [Kimlik Bilgileri Al](https://technet.microsoft.com/library/hh849815.aspx) cmdlet'i arayarak PSCredential nesnesini kolayca ayarlayabilirsiniz.

```powershell
$remoteusercredentials = Get-Credential
```

Bu komut, uzak kullanıcının kullanıcı adını ve parolasını güvenli bir şekilde girmenize olanak tanıyan bir iletişim kutusu görüntüler.

PowerShell otomasyon senaryolarında yardımcı olduğundan, **PSCredential** nesnesini kullanıcı etkileşimi gerektirmeyeceğini zedelemis bir şekilde de ayarlayabilirsiniz. İlk olarak, güvenli bir parola ayarlamanız gerekir. Düz metin parolası belirterek başlayın [convertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx)kullanarak güvenli bir dize dönüştürün. Daha sonra [convertFrom-SecureString](https://technet.microsoft.com/library/hh849814.aspx)kullanarak bu güvenli dizeyi şifreli bir standart dizeye dönüştürmeniz gerekir. Artık bu şifreli standart dizeyi [Set-İçerik'i](https://technet.microsoft.com/library/ee176959.aspx)kullanarak bir dosyaya kaydedebilirsiniz.

Ayrıca, her seferinde parola yazmak zorunda kalmamak için güvenli bir parola dosyası da oluşturabilirsiniz. Ayrıca, güvenli bir parola dosyası düz metin dosyasından daha iyidir. Güvenli bir parola dosyası oluşturmak için aşağıdaki PowerShell'i kullanın:

```powershell
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

> [!IMPORTANT]
> Parolayı ayarlarken, [karmaşıklık gereksinimlerini](https://technet.microsoft.com/library/cc786468.aspx)karşıladığınızdan emin olun.

Güvenli parola dosyasından kimlik bilgisi nesnesini oluşturmak için, dosya içeriğini okumanız ve [ConvertTo-SecureString'i](https://technet.microsoft.com/library/hh849818.aspx)kullanarak güvenli bir dize dönüştürmeniz gerekir.

[Set-AzureServiceRemoteDesktopExtension](/powershell/module/servicemanagement/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet, kullanıcı hesabının süresinin dolduğu bir **DateTime** belirten bir *Son Kullanma* parametresi de kabul eder. Örneğin, hesabı geçerli tarih ve saatten birkaç gün sonra sona erecek şekilde ayarlayabilirsiniz.

Bu PowerShell örneği, Uzak Masaüstü Uzantısı'nı bir bulut hizmetinde nasıl ayarlayabileceğinizi gösterir:

```powershell
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
Ayrıca, uzak masaüstünde etkinleştirmek istediğiniz dağıtım yuvasını ve rolleri isteğe bağlı olarak belirtebilirsiniz. Bu parametreler belirtilmemişse, cmdlet **Üretim** dağıtım yuvasındaki tüm rollerde uzak masaüstüne olanak tanır.

Uzak Masaüstü uzantısı bir dağıtım ile ilişkilidir. Hizmet için yeni bir dağıtım oluşturursanız, bu dağıtımda uzak masaüstünü etkinleştirmeniz gerekir. Her zaman uzak masaüstünün etkin olmasını istiyorsanız, PowerShell komut dosyalarını dağıtım iş akışınıza entegre etmeyi düşünmelisiniz.

## <a name="remote-desktop-into-a-role-instance"></a>Rol örneğine Uzak Masaüstü

[AzureRemoteDesktopFile](/powershell/module/servicemanagement/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet, masaüstünü bulut hizmetinizin belirli bir rol örneğine uzak bir şekilde uzaklamak için kullanılır. RDP dosyasını yerel olarak indirmek için *LocalPath* parametresini kullanabilirsiniz. Veya bulut hizmeti rolü örneğine erişmek için Uzak Masaüstü Bağlantısı iletişim kutusunu doğrudan başlatmak için *Başlat* parametresini kullanabilirsiniz.

```powershell
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```

## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a>Bir hizmette Uzak Masaüstü uzantısı etkin olup olmadığını denetleme

[Get-AzureServiceRemoteDesktopExtension](/powershell/module/servicemanagement/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet, bir hizmet dağıtımında uzak masaüstünün etkin veya devre dışı bırakıldığını görüntüler. Cmdlet, uzak masaüstü kullanıcısının kullanıcı adını ve uzak masaüstü uzantısının etkinleştirilen rolleri döndürür. Varsayılan olarak, bu dağıtım yuvasında olur ve bunun yerine hazırlama yuvasını kullanmayı seçebilirsiniz.

```powershell
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a>Uzak Masaüstü uzantısını bir hizmetten kaldırma

Dağıtımda uzak masaüstü uzantısını zaten etkinleştirdiyseniz ve uzak masaüstü ayarlarını güncelleştirmeniz gerekiyorsa, önce uzantıyı kaldırın. Ve yeni ayarlarla yeniden etkinleştirin. Örneğin, uzak kullanıcı hesabı için yeni bir parola ayarlamak istiyorsanız veya hesabın süresi doldu. Bunu yapmak, uzak masaüstü uzantısı etkinleştirilmiş varolan dağıtımlarda gereklidir. Yeni dağıtımlar için uzantıyı doğrudan uygulayabilirsiniz.

Uzak masaüstü uzantısını dağıtımdan kaldırmak için [Kaldır-AzureServiceRemoteDesktopExtension](/powershell/module/servicemanagement/azure/remove-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet'ini kullanabilirsiniz. Ayrıca, uzak masaüstü uzantısını kaldırmak istediğiniz dağıtım yuvasını ve rolünü isteğe bağlı olarak belirtebilirsiniz.

```powershell
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

> [!NOTE]
> Uzantı yapılandırmasını tamamen kaldırmak **için, Kaldırma Yapılandırması** parametresi ile cmdlet *kaldır'ı* aramalısınız.
>
> **UninstallConfiguration** parametresi hizmete uygulanan herhangi bir uzantı yapılandırmasını kaldırMaktadır. Her uzantı yapılandırması hizmet yapılandırmasıyla ilişkilidir. **Kaldırma Yapılandırması** olmadan *cmdlet kaldırma'yı* <mark>çağırmak, dağıtımı</mark> uzantı yapılandırmasından çıkarır ve böylece uzantıyı etkin bir şekilde kaldırır. Ancak, uzantı yapılandırması hizmetle ilişkili kalır.

## <a name="additional-resources"></a>Ek kaynaklar

[Cloud Services’ı Yapılandırma](cloud-services-how-to-configure-portal.md)


