---
title: Azure portalında eylem grupları oluşturma ve yönetme
description: Azure portalında eylem gruplarıoluşturmayı ve nasıl yönetiriz öğrenin.
author: dkamstra
ms.topic: conceptual
ms.date: 2/18/2020
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: 6ba48f3c40e45afa02e03a7589e968cca723118e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249522"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Azure portalında eylem grupları oluşturma ve yönetme
Eylem grubu, Azure aboneliğinin sahibi tarafından tanımlanan bildirim tercihleri topluluğudur. Azure Monitör üve Hizmet Durumu uyarıları, kullanıcılara bir uyarının tetiklendiğini bildirmek için eylem gruplarını kullanır. Çeşitli uyarılar, kullanıcının gereksinimlerine bağlı olarak aynı eylem grubunu veya farklı eylem gruplarını kullanabilir. Bir abonelikte en fazla 2.000 eylem grubu yapılandırabilirsiniz.

Bir eylemi bir kişiyi e-posta veya SMS ile bilgilendirmek için yapılandırırsanız, eylem grubuna eklendiklerini belirten bir onay alırsınız.

Bu makalede, Azure portalında eylem gruplarının nasıl oluşturulup yönetilen gösterilmektedir.

Her eylem aşağıdaki özelliklerden oluşur:

* **Adı**: Eylem grubu içinde benzersiz bir tanımlayıcı.  
* **Eylem türü**: Gerçekleştirilen eylem. Örnekler arasında sesli arama, SMS, e-posta gönderme; veya çeşitli otomatik eylemleri tetikleme. Bu makalenin ilerleyen saatlerinde türleri görün.
* **Ayrıntılar**: *Eylem türüne*göre değişen karşılık gelen ayrıntılar.

Eylem gruplarını yapılandırmak için Azure Kaynak Yöneticisi şablonlarının nasıl kullanılacağı hakkında bilgi [için, Eylem grubu Kaynak Yöneticisi şablonlarına](../../azure-monitor/platform/action-groups-create-resource-manager-template.md)bakın.

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Azure portalını kullanarak bir eylem grubu oluşturma

1. Azure [portalında,](https://portal.azure.com) **Monitör'ü**arayın ve seçin. **Monitör** bölmesi, tüm izleme ayarlarınızı ve verilerinizi tek bir görünümde birleştirir.

1. **Uyarılar**'ı ve ardından **Eylemleri yönet**'i seçin.

    ![Eylemleri Yönet düğmesi](./media/action-groups/manage-action-groups.png)
    
1. **Eylem grubu ekle'yi**seçin ve alanları doldurun.

    !["Eylem grubu ekle" komutu](./media/action-groups/add-action-group.png)
    
1. **Eylem grubu adı** kutusuna bir ad girin ve Kısa ad kutusuna bir **ad** girin. Bu eylem grubu kullanılarak bildirim gönderildiğinde tam grup adı yerine kısa ad kullanılır.

      ![Eylem grubu ekle" iletişim kutusu](./media/action-groups/action-group-define.png)

1. **Abonelik** kutusu geçerli aboneliğinizi otomatik olarak doldurur. Bu abonelik, eylem grubunun kaydedildiği aboneliktir.

1. Eylem **grubunun** kaydedildiği Kaynak grubunu seçin.

1. Eylemlerin listesini tanımlayın. Her eylem için aşağıdakileri sağlayın:

    1. **Adı**: Bu eylem için benzersiz bir tanımlayıcı girin.

    1. **Eylem Türü**: E-posta/SMS/Push/Voice, Logic App, Webhook, ITSM veya Automation Runbook'u seçin.

    1. **Ayrıntılar**: Eylem türüne bağlı olarak, bir telefon numarası, e-posta adresi, webhook URI, Azure uygulaması, ITSM bağlantısı veya Otomasyon runbook girin. ITSM Eylemi **için, ITSM** aracınızın gerektirdiği İş Öğesi ve diğer alanları ayrıca belirtin.
    
    1. **Ortak uyarı şeması**: Azure Monitor'daki tüm uyarı hizmetlerinde tek bir genişletilebilir ve birleşik uyarı yüküne sahip olmanın avantajını sağlayan [ortak uyarı şemasını](https://aka.ms/commonAlertSchemaDocs)etkinleştirmeyi seçebilirsiniz.

1. Eylem grubunu oluşturmak için **Tamam'ı** seçin.

## <a name="manage-your-action-groups"></a>Eylem gruplarınızı yönetme

Bir eylem grubu oluşturduktan sonra, **İzle bölmesi'ndeki** **Uyarılar** açılış sayfasından eylemleri **yönet'i** seçerek Eylem **gruplarını** görüntüleyebilirsiniz. Yönetmek istediğiniz eylem grubunu seçin:

* Eylemlerekleme, kaldırma veya kaldırma.
* Eylem grubunu silin.

## <a name="action-specific-information"></a>Eyleme özgü bilgiler

> [!NOTE]
> Aşağıdaki öğelerin her birinin sayısal sınırları için [İzleme için Abonelik Hizmet Sınırları'na](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#azure-monitor-limits) bakın.  

### <a name="automation-runbook"></a>Otomasyon Runbook
Runbook yüklerinde sınırlamalar için [Azure abonelik hizmet sınırlarına](../../azure-resource-manager/management/azure-subscription-service-limits.md) bakın.

Bir Eylem Grubunda sınırlı sayıda Runbook eylemine sahip olabilirsiniz. 

### <a name="azure-app-push-notifications"></a>Azure uygulaması Anında Iletme Bildirimleri
Bir Eylem Grubunda sınırlı sayıda Azure uygulama eylemine sahip olabilirsiniz.

### <a name="email"></a>Email
E-postalar aşağıdaki e-posta adreslerinden gönderilecektir. E-posta filtrelemenizin uygun şekilde yapılandırıldığından emin olun
- azure-noreply@microsoft.com
- azureemail-noreply@microsoft.com
- alerts-noreply@mail.windowsazure.com

Bir Eylem Grubunda sınırlı sayıda e-posta eylemine sahip olabilirsiniz. Bilgi makalesini [sınırlayan orana](./../../azure-monitor/platform/alerts-rate-limiting.md) bakın.

### <a name="email-azure-resource-manager-role"></a>Azure Kaynak Yöneticisi Rolünü E-postayla Gönder
Aboneliğin rolü üyelerine e-posta gönderin. E-postalar yalnızca **rolün Azure AD kullanıcı** üyelerine gönderilir. E-posta, Azure REKLAM gruplarına veya hizmet ilkelerine gönderilmez.

Bir Eylem Grubunda sınırlı sayıda e-posta eylemine sahip olabilirsiniz. Bilgi makalesini [sınırlayan orana](./../../azure-monitor/platform/alerts-rate-limiting.md) bakın.

### <a name="function"></a>İşlev
[Azure İşlevlerinde](../../azure-functions/functions-create-first-azure-function.md#create-a-function-app)varolan bir HTTP tetikleyici uç noktasını çağırır.

Bir Eylem Grubunda sınırlı sayıda İşlev eylemine sahip olabilirsiniz.

### <a name="itsm"></a>ITSM
ITSM Action bir ITSM Bağlantısı gerektirir. [ITSM Bağlantısı](../../azure-monitor/platform/itsmc-overview.md)nasıl oluşturulacak öğrenin.

Bir Eylem Grubunda sınırlı sayıda ITSM eylemine sahip olabilirsiniz. 

### <a name="logic-app"></a>Logic App
Bir Eylem Grubunda sınırlı sayıda Mantık Uygulaması eylemine sahip olabilirsiniz.

### <a name="secure-webhook"></a>Güvenli Webhook
Eylem Grupları Webhook eylemi, eylem grubunuzla korumalı web API'nız (webhook bitiş noktası) arasındaki bağlantıyı güvence altına almak için Azure Etkin Dizini'nden yararlanmanızı sağlar. Bu işlevselliğin yararlanılanması için genel işakışı aşağıda açıklanMıştır. Azure AD Uygulamalarına ve hizmet ilkelerine genel bakış için [Microsoft kimlik platformuna (v2.0) genel bakış](https://docs.microsoft.com/azure/active-directory/develop/v2-overview)bölümüne bakın.

1. Korumalı web API'nız için bir Azure REKLAM Uygulaması oluşturun. Bkz. https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview.
    - Korumalı API'nizi bir daemon uygulaması tarafından çağrılacak şekilde yapılandırın.
    
1. Eylem Gruplarının Azure REKLAM Uygulamanızı kullanmasını etkinleştirin.

    > [!NOTE]
    > Bu komut dosyasını yürütmek için [Azure AD Uygulama Yöneticisi rolünün](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#available-roles) bir üyesi olmalısınız.
    
    - Azure AD Kiracı Kimliğinizi kullanmak için PowerShell komut dosyasının Connect-AzureAD çağrısını değiştirin.
    - Azure AD Uygulamanızın Nesne Kimliğini kullanmak için PowerShell komut dosyasının değişken $myAzureADApplicationObjectId değiştirme
    - Değiştirilmiş komut dosyasını çalıştırın.
    
1. Eylem Grubu Güvenli Webhook eylemini yapılandırın.
    - Komut dosyasından $myApp.ObjectId değerini kopyalayın ve Webhook eylem tanımında Uygulama Nesnesi Kimliği alanına girin.
    
    ![Güvenli Webhook eylemi](./media/action-groups/action-groups-secure-webhook.png)

#### <a name="secure-webhook-powershell-script"></a>Güvenli Webhook PowerShell Komut Dosyası

```PowerShell
Connect-AzureAD -TenantId "<provide your Azure AD tenant ID here>"
    
# This is your Azure AD Application's ObjectId. 
$myAzureADApplicationObjectId = "<the Object Id of your Azure AD Application>"
    
# This is the Action Groups Azure AD AppId
$actionGroupsAppId = "461e8683-5575-4561-ac7f-899cc907d62a"
    
# This is the name of the new role we will add to your Azure AD Application
$actionGroupRoleName = "ActionGroupsSecureWebhook"
    
# Create an application role of given name and description
Function CreateAppRole([string] $Name, [string] $Description)
{
    $appRole = New-Object Microsoft.Open.AzureAD.Model.AppRole
    $appRole.AllowedMemberTypes = New-Object System.Collections.Generic.List[string]
    $appRole.AllowedMemberTypes.Add("Application");
    $appRole.DisplayName = $Name
    $appRole.Id = New-Guid
    $appRole.IsEnabled = $true
    $appRole.Description = $Description
    $appRole.Value = $Name;
    return $appRole
}
    
# Get my Azure AD Application, it's roles and service principal
$myApp = Get-AzureADApplication -ObjectId $myAzureADApplicationObjectId
$myAppRoles = $myApp.AppRoles
$actionGroupsSP = Get-AzureADServicePrincipal -Filter ("appId eq '" + $actionGroupsAppId + "'")

Write-Host "App Roles before addition of new role.."
Write-Host $myAppRoles
    
# Create the role if it doesn't exist
if ($myAppRoles -match "ActionGroupsSecureWebhook")
{
    Write-Host "The Action Groups role is already defined.`n"
}
else
{
    $myServicePrincipal = Get-AzureADServicePrincipal -Filter ("appId eq '" + $myApp.AppId + "'")
    
    # Add our new role to the Azure AD Application
    $newRole = CreateAppRole -Name $actionGroupRoleName -Description "This is a role for Action Groups to join"
    $myAppRoles.Add($newRole)
    Set-AzureADApplication -ObjectId $myApp.ObjectId -AppRoles $myAppRoles
}
    
# Create the service principal if it doesn't exist
if ($actionGroupsSP -match "AzNS AAD Webhook")
{
    Write-Host "The Service principal is already defined.`n"
}
else
{
    # Create a service principal for the Action Groups Azure AD Application and add it to the role
    $actionGroupsSP = New-AzureADServicePrincipal -AppId $actionGroupsAppId
}
    
New-AzureADServiceAppRoleAssignment -Id $myApp.AppRoles[0].Id -ResourceId $myServicePrincipal.ObjectId -ObjectId $actionGroupsSP.ObjectId -PrincipalId $actionGroupsSP.ObjectId
    
Write-Host "My Azure AD Application ($myApp.ObjectId): " + $myApp.ObjectId
Write-Host "My Azure AD Application's Roles"
Write-Host $myApp.AppRoles
```

### <a name="sms"></a>SMS
Ek önemli bilgiler için bilgileri ve [SMS uyarı davranışını](../../azure-monitor/platform/alerts-sms-behavior.md) [sınırlayan hıza](./../../azure-monitor/platform/alerts-rate-limiting.md) bakın.

Bir Eylem Grubunda sınırlı sayıda SMS eyleminiz olabilir.  

### <a name="voice"></a>Ses
Bilgi makalesini [sınırlayan orana](./../../azure-monitor/platform/alerts-rate-limiting.md) bakın.

Bir Eylem Grubunda sınırlı sayıda Ses eylemine sahip olabilirsiniz.

### <a name="webhook"></a>Web Kancası
Webhooks aşağıdaki kurallar kullanılarak yeniden denendi. Webhook araması, aşağıdaki HTTP durum kodları döndürüldüğünde en fazla 2 kez yeniden denenmektedir: 408, 429, 503, 504 veya HTTP bitiş noktası yanıt vermez. İlk yeniden deneme 10 saniye sonra yapılır. İkinci tekrar 100 saniye sonra gerçekleşir. İki hatadan sonra, hiçbir eylem grubu 30 dakika boyunca bitiş noktasını aramaz. 

Kaynak IP adresi aralıkları
 - 13.72.19.232
 - 13.106.57.181
 - 13.106.54.3
 - 13.106.54.19
 - 13.106.38.142
 - 13.106.38.148
 - 13.106.57.196
 - 13.106.57.197
 - 52.244.68.117
 - 52.244.65.137
 - 52.183.31.0
 - 52.184.145.166
 - 51.4.138.199
 - 51.5.148.86
 - 51.5.149.19

Bu IP adreslerindeki değişikliklerle ilgili güncelleştirmeleri almak için, Eylem Grupları hizmeti yle ilgili Bilgilendirme bildirimlerini izleyen bir Hizmet Durumu uyarısı yapılandırmanızı öneririz.

Bir Eylem Grubunda sınırlı sayıda Webhook eylemine sahip olabilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar
* [SMS uyarısı davranışı](../../azure-monitor/platform/alerts-sms-behavior.md)hakkında daha fazla bilgi edinin.  
* Etkinlik [günlüğü uyarısı webhook şema](../../azure-monitor/platform/activity-log-alerts-webhook.md)bir anlayış kazanın.  
* [ITSM Konektörü](../../azure-monitor/platform/itsmc-overview.md) hakkında daha fazla bilgi edinin
* Uyarılarda [hız sınırlaması](../../azure-monitor/platform/alerts-rate-limiting.md) hakkında daha fazla bilgi edinin.
* Etkinlik [günlüğü uyarılarına genel](../../azure-monitor/platform/alerts-overview.md)bir bakış alın ve uyarıları nasıl alacağınızı öğrenin.  
* Bir hizmet durumu bildirimi yayınlandığında uyarıları nasıl [yapılandırıştırılaca](../../azure-monitor/platform/alerts-activity-log-service-notifications.md)öğrenin.
