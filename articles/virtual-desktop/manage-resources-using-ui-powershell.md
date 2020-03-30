---
title: Hizmet üstatlarını kullanarak Windows Sanal Masaüstü için bir yönetim aracı dağıtma - Azure
description: PowerShell'i kullanarak Windows Sanal Masaüstü için yönetim aracı nasıl dağıtılır?
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 01/10/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 0838edb03c4868548f3d09f14d71ec7016e670a4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127803"
---
# <a name="deploy-a-management-tool-with-powershell"></a>PowerShell ile bir yönetim aracı dağıtma

Bu makalede, PowerShell kullanarak yönetim aracınasıl dağıtılacağınızda gösterecektir.

## <a name="important-considerations"></a>Önemli noktalar

Her Azure Etkin Dizin (Azure AD) kiracısının aboneliğinin yönetim aracının kendi ayrı dağıtımına ihtiyacı vardır. Bu araç, Azure İş-İş (B2B) senaryolarını desteklemez. 

Bu yönetim aracı bir örnektir. Microsoft önemli güvenlik ve kalite güncelleştirmeleri sağlayacaktır. [Kaynak kodu GitHub'da kullanılabilir.](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/wvd-management-ux/deploy) İster müşteri ister iş ortağı olun, iş ihtiyaçlarınızı karşılamak için aracı özelleştirmenizi öneririz.

Aşağıdaki tarayıcılar yönetim aracıyla uyumludur:

- Google Chrome 68 veya sonrası
- Microsoft Edge 40.15063 veya sonrası
- Mozilla Firefox 52.0 veya sonrası
- Safari 10 veya sonraki (yalnızca macOS)

## <a name="what-you-need-to-deploy-the-management-tool"></a>Yönetim aracını dağıtmak için gerekenler

Yönetim aracını dağıtmadan önce, bir uygulama kaydı oluşturmak ve yönetim kullanıcısını dağıtmak için bir Azure Etkin Dizin (Azure AD) kullanıcısına ihtiyacınız vardır. Bu kullanıcı şunları yapmalı:

- Azure aboneliğinizde kaynak oluşturma iznine sahip olun
- Azure AD uygulaması oluşturmak için izniniz var. Kullanıcınızın [Gerekli Izinler'deki](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)yönergeleri izleyerek gerekli izinlere sahip olup olmadığını kontrol etmek için aşağıdaki adımları izleyin.

Yönetim aracını başarıyla dağıtmak ve yapılandırmak için, öncelikle [RDS-Templates GitHub repo'sundan](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/wvd-management-ux/deploy/scripts) aşağıdaki PowerShell komut dosyalarını indirmeniz ve bunları yerel makinenizdeki aynı klasöre kaydetmeniz gerekir.

  - oluşturmakWvdMgmtUxAppRegistration.ps1
  - güncellemeWvdMgmtUxApiUrl.ps1

Yönetim aracını dağıttıktan ve yapılandırdıktan sonra, her şeyin çalıştığından emin olmak için bir kullanıcıdan yönetim kullanıcıara'sını başlatmasını istemenizi öneririz. Yönetim Kullanıcı Yı'sını başlatan kullanıcının, Windows Sanal Masaüstü kiracısını görüntülemelerine veya yönetmelerine olanak tanıyan bir rol ataması olmalıdır.

## <a name="set-up-powershell"></a>PowerShell'i ayarlama

Az ve Azure AD PowerShell modüllerine oturum açarak başlayın. Şu şekilde oturum açabilirsiniz:

1. PowerShell'i Yönetici olarak açın ve PowerShell komut dosyalarını kaydettiğiniz dizine gidin.
2. Aşağıdaki cmdlet'i çalıştırarak yönetim aracını oluşturmak için kullanmayı planladığınız Azure aboneliğinde Kullanım Alanı veya Katılımcı izinlerine sahip bir hesapla Azure'da oturum açın:

    ```powershell
    Login-AzAccount
    ```

3. Az PowerShell modülü için kullandığınız hesapla Azure AD'de oturum açabilmek için aşağıdaki cmdlet'i çalıştırın:

    ```powershell
    Connect-AzureAD
    ```

4. Bundan sonra, iki PowerShell komut dosyasını RDS-Templates GitHub repo'sundan kaydettiğiniz klasöre gidin.

Oturum açarken ek PowerShell cmdletleri çalıştırmak için oturum açmak için kullandığınız PowerShell penceresini açık tutun.

## <a name="create-an-azure-active-directory-app-registration"></a>Azure Active Directory uygulama kaydı oluşturma

Gerekli API izinleri ile uygulama kaydı oluşturmak için aşağıdaki komutları çalıştırın:

```powershell
$appName = Read-Host -Prompt "Enter a unique name for the management tool's app registration. The name can't contain spaces or special characters."
$subscriptionId = Read-Host -Prompt "Enter the Azure subscription ID where you will be deploying the management tool."

.\createWvdMgmtUxAppRegistration.ps1 -AppName $appName -SubscriptionId $subscriptionId
```

Azure AD uygulama kaydını tamamladığınızda, yönetim aracını dağıtabilirsiniz.

## <a name="deploy-the-management-tool"></a>Yönetim aracını dağıtma

Yönetim aracını dağıtmak ve yeni oluşturduğunuz hizmet müdürüyle ilişkilendirmek için aşağıdaki PowerShell komutlarını çalıştırın:
     
```powershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$templateParameters = @{}
$templateParameters.Add('isServicePrincipal', $true)
$templateParameters.Add('azureAdminUserPrincipalNameOrApplicationId', $ServicePrincipalCredentials.UserName)
$templateParameters.Add('azureAdminPassword', $servicePrincipalCredentials.Password)
$templateParameters.Add('applicationName', $appName)

Get-AzSubscription -SubscriptionId $subscriptionId | Select-AzSubscription
New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/RDS-Templates/master/wvd-templates/wvd-management-ux/deploy/mainTemplate.json" `
    -TemplateParameterObject $templateParameters `
    -Verbose
```

Web uygulamasını oluşturduktan sonra, kullanıcıları başarılı bir şekilde oturum alabilmek için Azure REKLAM uygulamasına yeniden yönlendirme URI eklemeniz gerekir.

## <a name="set-the-redirect-uri"></a>Uri'yi Yönlendirmeyi Ayarlama

Web uygulaması URL'sini almak için aşağıdaki PowerShell komutlarını çalıştırın ve uri'yi (yanıt URL'si olarak da adlandırılır) kimlik doğrulama yönlendirmesi olarak ayarlayın:

```powershell
$webApp = Get-AzWebApp -ResourceGroupName $resourceGroupName -Name $appName
$redirectUri = "https://" + $webApp.DefaultHostName + "/"
Get-AzureADApplication -All $true | where { $_.AppId -match $servicePrincipalCredentials.UserName } | Set-AzureADApplication -ReplyUrls $redirectUri  
```

Artık bir yeniden yönlendirme URI eklediyseniz, yönetim aracının API arka uç hizmetiyle etkileşim kurabilmesi için bir sonraki güncelleştirmeniz GEREKIR.

## <a name="update-the-api-url-for-the-web-application"></a>Web uygulaması için API URL'sini güncelleştirme

Web uygulamasıön ucundaki API URL yapılandırmasını güncelleştirmek için aşağıdaki komut dosyasını çalıştırın:

```powershell
.\updateWvdMgmtUxApiUrl.ps1 -AppName $appName -SubscriptionId $subscriptionId
```

Yönetim aracı web uygulamasını tamamen yapılandırdığınıza göre, Azure AD uygulamasını doğrulama ve onay verme zamanı geldi.

## <a name="verify-the-azure-ad-application-and-provide-consent"></a>Azure AD uygulamasını doğrulayın ve onay sağlayın

Azure AD uygulama yapılandırmasını doğrulamak ve onay sağlamak için:

1. İnternet tarayıcınızı açın ve yönetim hesabınızla [Azure portalında](https://portal.azure.com/) oturum açın.
2. Azure portalının üst kısmındaki arama çubuğundan **Uygulama kayıtlarını** arayın ve **Hizmetler**altındaki öğeyi seçin.
3. **Tüm uygulamaları** seçin ve Azure [Active Directory uygulama kaydı oluşturma'nda](#create-an-azure-active-directory-app-registration)PowerShell komut dosyası için sağladığınız benzersiz uygulama adını arayın.
4. Tarayıcının sol tarafındaki panelde Kimlik **Doğrulama'yı** seçin ve yeniden yönlendirme URI'sinin aşağıdaki resimde gösterildiği gibi yönetim aracının web uygulaması URL'si ile aynı olduğundan emin olun.
   
   [![Girilen yeniden yönlendirme URI](media/management-ui-redirect-uri-inline.png) ile kimlik doğrulama sayfası](media/management-ui-redirect-uri-expanded.png#lightbox)

5. Sol panelde, izinlerin eklandığını onaylamak için **API izinlerini** seçin. Genel bir yöneticiyseniz, düğme **için `tenantname` Grant yöneticisi onayını** seçin ve kuruluşunuz için yönetici onayı sağlamak için iletişim istemlerini izleyin.
    
    [![API izinleri](media/management-ui-permissions-inline.png) sayfası](media/management-ui-permissions-expanded.png#lightbox)

Artık yönetim aracını kullanmaya başlayabilirsiniz.

## <a name="use-the-management-tool"></a>Yönetim aracını kullanma

Artık yönetim aracını istediğiniz zaman kurduğunuzdan, istediğiniz zaman, istediğiniz yerde başlatabilirsiniz. Aracı şu şekilde başlatabilirsiniz:

1. Web uygulamasının URL'sini bir web tarayıcısında açın. URL'yi hatırlamıyorsanız, Azure'da oturum açabilir, yönetim aracı için dağıttığınız uygulama hizmetini bulabilir ve ardından URL'yi seçebilirsiniz.
2. Windows Sanal Masaüstü kimlik bilgilerinizi kullanarak oturum açın.
   
   > [!NOTE]
   > Yönetim aracını yapılandırırken yönetici onayı vermediyseniz, sözleşme imzalayan her kullanıcının aracı kullanabilmesi için kendi kullanıcı onayını sağlaması gerekir.

3. Bir kiracı grubu seçmesi istendiğinde, açılan listedevarsayılan **kiracı grubu'nu** seçin.
4. **Varsayılan Kiracı Grubu'nu**seçtiğinizde, pencerenizin sol tarafında bir menü görünmelidir. Bu menüde, kiracı grubunuzun adını bulun ve seçin.
   
   > [!NOTE]
   > Özel bir kiracı grubunuz varsa, açılan listeden seçim yapmak yerine adı el ile girin.

## <a name="report-issues"></a>Sorun bildirme

Yönetim aracı veya diğer Windows Sanal Masaüstü araçlarıyla ilgili herhangi bir sorunla karşılaşırsanız, Bunları GitHub'da bildirmek [için Uzak Masaüstü Hizmetleri için Azure Kaynak Yöneticisi şablonlarındaki](https://github.com/Azure/RDS-Templates/blob/master/README.md) yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Yönetim aracını dağıtmayı ve bunlara bağlanmayı öğrendiğiniz için, hizmet sorunlarını ve sistem durumu danışma belgelerini izlemek için Azure Hizmet Durumu'nu nasıl kullanacağınızı öğrenebilirsiniz. Daha fazla bilgi edinmek için, [servis uyarıları hazırla öğreticimize](./set-up-service-alerts.md)bakın.
