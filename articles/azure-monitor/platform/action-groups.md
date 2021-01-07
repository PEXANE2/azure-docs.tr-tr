---
title: Azure portalında eylem gruplarını oluşturma ve yönetme
description: Azure portal eylem grupları oluşturmayı ve yönetmeyi öğrenin.
author: dkamstra
ms.topic: conceptual
ms.date: 07/28/2020
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: 72caeb60fc058b88158979d211a0bc38985975c7
ms.sourcegitcommit: 9514d24118135b6f753d8fc312f4b702a2957780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2021
ms.locfileid: "97968866"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Azure portalında eylem gruplarını oluşturma ve yönetme
Bir eylem grubu, bir Azure aboneliğinin sahibi tarafından tanımlanan bildirim tercihleri koleksiyonudur. Azure Izleyici ve hizmet durumu uyarıları, kullanıcılara bir uyarının tetiklendiğini bildirmek için eylem gruplarını kullanır. Çeşitli uyarılar, kullanıcının gereksinimlerine bağlı olarak aynı eylem grubunu veya farklı eylem gruplarını kullanabilir. 

Bu makalede Azure portal eylem gruplarının nasıl oluşturulacağı ve yönetileceği gösterilmektedir.

Her eylem aşağıdaki özelliklerden oluşur:

* **Yazın**: bildirim veya eylem gerçekleştirildi. Bir sesli çağrı, SMS, e-posta gönderme örnekleri aşağıda verilmiştir. veya çeşitli otomatikleştirilmiş eylem türlerini tetikleyerek. Bu makalenin ilerleyen kısımlarında bulunan türlere bakın.
* **Ad**: eylem grubu içindeki benzersiz bir tanımlayıcı.
* **Ayrıntılar**: *türe* göre farklılık gösteren ilgili ayrıntılar.

Eylem gruplarını yapılandırmak için Azure Resource Manager şablonlarını kullanma hakkında daha fazla bilgi için bkz. [eylem grubu Kaynak Yöneticisi şablonları](./action-groups-create-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Azure portal kullanarak bir eylem grubu oluşturun

1. [Azure Portal](https://portal.azure.com), **izleme**' yi arayıp seçin. **İzleyici** bölmesi tüm izleme ayarlarınızı ve verilerinizi tek bir görünümde birleştirir.

1. **Uyarılar**' ı seçin ve ardından **eylemleri Yönet**' i seçin.

    ![Eylemleri Yönet düğmesi](./media/action-groups/manage-action-groups.png)
    
1. **Eylem grubu Ekle**' yi seçin ve sihirbaz deneyiminde ilgili alanları girin.

    !["Eylem grubu Ekle" komutu](./media/action-groups/add-action-group.PNG)

### <a name="configure-basic-action-group-settings"></a>Temel eylem grubu ayarlarını yapılandırma

**Proje ayrıntıları**:

Eylem grubunun kaydedildiği **aboneliği** ve **kaynak grubunu** seçin.

**Örnek ayrıntıları**:

1. Bir **eylem grubu adı** girin.

1. Bir **görünen ad** girin. Görünen ad, bildirimler bu grup kullanılarak gönderildiğinde tam bir eylem grubu adı yerine kullanılır.

      ![Eylem grubu Ekle "iletişim kutusu](./media/action-groups/action-group-1-basics.png)


### <a name="configure-notifications"></a>Bildirimleri yapılandırma

1. **Bildirimler** sekmesine gitmek için **ileri: bildirimler >** düğmesine tıklayın veya ekranın üst kısmındaki **Bildirimler** sekmesini seçin.

1. Bir uyarı tetiklendiğinde gönderilecek bildirimlerin listesini tanımlayın. Her bildirim için aşağıdakileri sağlayın:

    a. **Bildirim türü**: göndermek istediğiniz bildirim türünü seçin. Şu seçenekler sağlanır:
      * E-posta Azure Resource Manager rolü-belirli bir abonelik düzeyi ARM rolüne atanan kullanıcılara bir e-posta gönderin.
      * E-posta/SMS/Push/Voice-bu bildirim türlerini belirli alıcılara gönderir.
    
    b. **Ad**: bildirim için benzersiz bir ad girin.

    c. **Ayrıntılar**: seçili bildirim türüne göre bir e-posta adresi, telefon numarası vb. girin.
    
    d. **Ortak uyarı şeması**: Azure izleyici 'deki tüm uyarı hizmetlerinde tek bir Genişletilebilir ve birleştirilmiş uyarı yüküne sahip olmanın avantajını sağlayan [ortak uyarı şemasını](./alerts-common-schema.md)etkinleştirmeyi seçebilirsiniz.

    ![Bildirimler sekmesi](./media/action-groups/action-group-2-notifications.png)
    
### <a name="configure-actions"></a>Eylemleri yapılandırma

1. **Eylemler** sekmesine gitmek için **ileri: eylemler >** düğmesine tıklayın veya ekranın üst kısmındaki **Eylemler** sekmesini seçin.

1. Bir uyarı tetiklendiğinde tetiklenecek eylemlerin listesini tanımlayın. Her eylem için aşağıdakileri sağlayın:

    a. **Eylem türü**: Otomasyon Runbook 'U, Azure IşLEVI, ITSM, mantıksal uygulama, güvenli Web kancası, Web kancası 'yi seçin.
    
    b. **Ad**: eylem için benzersiz bir ad girin.

    c. **Ayrıntılar**: eylem türüne bağlı olarak, bir Web kancası URI 'Si, Azure UYGULAMASı, ITSM bağlantısı veya Otomasyon Runbook 'u girin. ITSM eylemi için, ayrıca ıTSM araclarınızın gerektirdiği **Iş öğesini** ve diğer alanları belirtin.
    
    d. **Ortak uyarı şeması**: Azure izleyici 'deki tüm uyarı hizmetlerinde tek bir Genişletilebilir ve birleştirilmiş uyarı yüküne sahip olmanın avantajını sağlayan [ortak uyarı şemasını](./alerts-common-schema.md)etkinleştirmeyi seçebilirsiniz.
    
    ![Eylemler sekmesi](./media/action-groups/action-group-3-actions.png)

### <a name="create-the-action-group"></a>Eylem grubunu oluşturma

1. Dilerseniz **Etiketler** ayarlarını da inceleyebilirsiniz. Bu, anahtar/değer çiftlerini kategorilere ayırma için eylem grubuyla ilişkilendirmenize ve tüm Azure kaynakları için kullanılabilen bir özelliktir.

    ![Etiketler sekmesi](./media/action-groups/action-group-4-tags.png)
    
1. Ayarları gözden geçirmek için **Gözden geçir + oluştur**'a tıklayın. Bu, tüm gerekli alanların seçili olduğundan emin olmak için girişlerinizin hızlı bir şekilde doğrulanmasını sağlar. Sorun varsa burada bildirilir. Ayarları inceledikten sonra, eylem grubunu sağlamak için **Oluştur** ' a tıklayın.
    
    ![Gözden geçir + Oluştur sekmesi](./media/action-groups/action-group-5-review.png)

> [!NOTE]
> E-posta veya SMS ile bir kişiye bildirimde bulunan bir eylem yapılandırdığınızda, bunlar eylem grubuna eklendiğini belirten bir onay alırlar.

## <a name="manage-your-action-groups"></a>Eylem gruplarınızı yönetin

Bir eylem grubu oluşturduktan sonra, **izleme** bölmesinde **Uyarılar** giriş sayfasından **eylemleri Yönet** ' i seçerek **eylem gruplarını** görüntüleyebilirsiniz. Yönetmek istediğiniz eylem grubunu seçin:

* Eylemleri ekleyin, düzenleyin veya kaldırın.
* Eylem grubunu silin.

## <a name="action-specific-information"></a>Eyleme özgü bilgiler

> [!NOTE]
> Aşağıdaki öğelerin her birinde sayısal limitleri [izlemek Için abonelik hizmeti sınırlarına](../../azure-resource-manager/management/azure-subscription-service-limits.md#azure-monitor-limits) bakın.  

### <a name="automation-runbook"></a>Otomasyon Runbook'u
Runbook yüklerinin sınırları için [Azure abonelik hizmeti sınırlarına](../../azure-resource-manager/management/azure-subscription-service-limits.md) bakın.

Bir eylem grubunda sınırlı sayıda runbook eylemi olabilir. 

### <a name="azure-app-push-notifications"></a>Azure Uygulama anında Iletme bildirimleri
Bir eylem grubunda sınırlı sayıda Azure uygulama eylemi olabilir.

### <a name="email"></a>E-posta
E-postalar aşağıdaki e-posta adreslerinden gönderilir. E-posta filtrelemesinin uygun şekilde yapılandırıldığından emin olun
- azure-noreply@microsoft.com
- azureemail-noreply@microsoft.com
- alerts-noreply@mail.windowsazure.com

Bir eylem grubunda sınırlı sayıda e-posta eylemi olabilir. Bkz. [hız sınırlandırma bilgileri](./alerts-rate-limiting.md) makalesi.

### <a name="email-azure-resource-manager-role"></a>Azure Resource Manager Rolüne E-posta Gönder
Abonelik rolü üyelerine e-posta gönderin. E-posta yalnızca rolün **Azure AD Kullanıcı** üyelerine gönderilir. Azure AD gruplarına veya hizmet sorumlularına e-posta gönderilmez.

Yalnızca *birincil e-posta* adresine bir bildirim e-postası gönderilir.

*Birincil e-postanıza* bildirim aldıysanız aşağıdaki adımları deneyebilirsiniz:

1. Azure portal *Active Directory* gidin.
2. Tüm kullanıcılar ' a (sol bölmede) tıklayın, Kullanıcı listesini (sağ bölmede) görürsünüz.
3. *Birincil e-posta* bilgilerini gözden geçirmek istediğiniz kullanıcıyı seçin.

  :::image type="content" source="media/action-groups/active-directory-user-profile.png" alt-text="Kullanıcı profilini İnceleme hakkında örnek."border="true":::

4. "E-posta" sekmesi boş ise, Iletişim bilgileri altındaki Kullanıcı profili ' nde, üstteki *Düzenle* düğmesine tıklayın ve *birincil e-postanızı* ekleyin ve en üstteki *Kaydet* düğmesine basın.

  :::image type="content" source="media/action-groups/active-directory-add-primary-email.png" alt-text="Birincil e-postanın nasıl ekleneceği hakkında örnek."border="true":::

Bir eylem grubunda sınırlı sayıda e-posta eylemi olabilir. Bkz. [hız sınırlandırma bilgileri](./alerts-rate-limiting.md) makalesi.

### <a name="function"></a>İşlev
[Azure işlevlerinde](../../azure-functions/functions-create-first-azure-function.md#create-a-function-app)var olan bir http tetikleyici uç noktasını çağırır.

Bir eylem grubunda sınırlı sayıda Işlev eylemine sahip olabilirsiniz.

### <a name="itsm"></a>ITSM
ITSM eylemi bir ıTSM bağlantısı gerektiriyor. [ITSM bağlantısı](./itsmc-overview.md)oluşturmayı öğrenin.

Bir eylem grubunda sınırlı sayıda ıSM eylemi olabilir. 

### <a name="logic-app"></a>Logic App
Bir eylem grubunda sınırlı sayıda mantıksal uygulama eylemi olabilir.

### <a name="secure-webhook"></a>Güvenli Web Kancası

> [!NOTE]
> Web kancası eyleminin kullanılması için hedef Web kancası uç noktasının, uyarının ayrıntılarının başarıyla çalışması gerekmez veya POST işleminin bir parçası olarak sağlanmış olan uyarı bağlamı bilgilerini ayrıştırmaktan emin olmanız gerekir. Web kancası uç noktası, uyarı bağlamı bilgilerini kendi kendine işleyemez, Web kancasının beklenen veri biçimiyle eşleşecek şekilde uyarı bağlamı bilgilerini özel bir şekilde işlemek için bir [mantıksal uygulama eylemi](./action-groups-logic-app.md) gibi bir çözüm kullanabilirsiniz.

Eylem grupları Web kancası eylemi, eylem grubunuz ve korumalı Web API 'niz (Web kancası uç noktası) arasındaki bağlantıyı güvenli hale getirmek için Azure Active Directory avantajlarından yararlanmanızı sağlar. Bu işlevden yararlanmak için genel iş akışı aşağıda açıklanmıştır. Azure AD uygulamalarına ve hizmet sorumlularına genel bakış için bkz. [Microsoft Identity platform (v 2.0) genel bakış](../../active-directory/develop/v2-overview.md).

1. Korumalı Web API 'niz için bir Azure AD uygulaması oluşturun. Bkz. [korumalı Web API 'si: uygulama kaydı](../../active-directory/develop/scenario-protected-web-api-app-registration.md).
    - Korumalı API 'nizi [bir Daemon uygulaması tarafından çağrılacak](../../active-directory/develop/scenario-protected-web-api-app-registration.md#if-your-web-api-is-called-by-a-daemon-app)şekilde yapılandırın.
    
2. Azure AD uygulamanızı kullanmak için eylem gruplarını etkinleştirin.

    > [!NOTE]
    > Bu betiği yürütmek için [Azure AD uygulama Yöneticisi rolünün](../../active-directory/roles/permissions-reference.md#available-roles) bir üyesi olmanız gerekir.
    
    - PowerShell betiğinin Connect-AzureAD çağrısını Azure AD kiracı KIMLIĞINIZI kullanacak şekilde değiştirin.
    - PowerShell betiğinin değişken $myAzureADApplicationObjectId Azure AD uygulamanızın nesne KIMLIĞINI kullanacak şekilde değiştirin.
    - Değiştirilen betiği çalıştırın.
    
3. Eylem grubu güvenli Web kancası eylemini yapılandırın.
    - Betikten $myApp. ObjectID değerini kopyalayın ve Web kancası eylem tanımındaki uygulama nesne KIMLIĞI alanına girin.
    
    ![Güvenli Web kancası eylemi](./media/action-groups/action-groups-secure-webhook.png)

#### <a name="secure-webhook-powershell-script"></a>Güvenli Web kancası PowerShell betiği

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
Ek önemli bilgiler için bkz. bilgi ve [SMS uyarı davranışını](./alerts-sms-behavior.md) [sınırlandırma](./alerts-rate-limiting.md) . 

Bir eylem grubunda sınırlı sayıda SMS eylemi olabilir.

> [!NOTE]
> Azure portal eylem grubu Kullanıcı arabirimi ülke/bölge kodunuzu seçmenizi vermezse, ülkeniz/bölgeniz için SMS desteklenmez.  Ülke/Bölge kodunuz yoksa, ülkeniz/bölgenizin [Kullanıcı sesine](https://feedback.azure.com/forums/913690-azure-monitor/suggestions/36663181-add-more-country-codes-for-sms-alerting-and-voice)eklenmesini sağlayabilirsiniz. Bu sırada, geçici bir çözüm, eylem grubunuzun bir üçüncü taraf SMS sağlayıcısına ait bir Web kancasını ülkeniz/bölgeniz desteğiyle çağırmasını sağlar.  

Desteklenen ülkeler/bölgeler için fiyatlandırma, [Azure izleyici fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/monitor/)listelenmiştir.

**SMS bildiriminin desteklendiği ülkelerin listesi**

| Ülke Kodu | Ülke Adı |
|:---|:---|
| 61 | Avustralya |
| 43 | Avusturya |
| 32 | Belçika |
| 55 | Brezilya |
| 1 |Kanada |
| 56 | Şili |
| 86 | Çin |
| 420 | Çek Cumhuriyeti |
| 45 | Danimarka |
| 372 | Estonya |
| 358 | Finlandiya |
| 33 | Fransa |
| 49 | Almanya |
| 852 | Hong Kong |
| 91 | Hindistan |
| 353 | İrlanda |
| 972 | İsrail |
| 39 | İtalya |
| 81 | Japonya |
| 352 | Lüksemburg |
| 60 | Malezya |
| 52 | Meksika |
| 31 | Hollanda |
| 64 | Yeni Zelanda |
| 47 | Norveç |
| 351 | Portekiz |
| 1 | Porto Riko |
| 40 | Romanya |
| 65 | Singapur |
| 27 | Güney Afrika |
| 82 | Güney Kore |
| 34 | İspanya |
| 41 | İsviçre |
| 886 | Tayvan |
| 44 | Birleşik Krallık |
| 1 | Birleşik Devletler |

### <a name="voice"></a>Ses
Daha önemli davranış için bkz. [hız sınırlandırma bilgileri](./alerts-rate-limiting.md) makalesi.

Bir eylem grubunda sınırlı sayıda ses eylemi olabilir.

> [!NOTE]
> Azure portal eylem grubu Kullanıcı arabirimi ülke/bölge kodunuzu seçmenizi desteklemiyorsa, ülkeniz/bölgeniz için sesli çağrılar desteklenmez. Ülke/Bölge kodunuz yoksa, ülkeniz/bölgenizin [Kullanıcı sesine](https://feedback.azure.com/forums/913690-azure-monitor/suggestions/36663181-add-more-country-codes-for-sms-alerting-and-voice)eklenmesini sağlayabilirsiniz.  Bu sırada, geçici bir çözüm, eylem grubunuzun bir üçüncü taraf sesli çağrı sağlayıcısına ait bir Web kancasını ülke/bölgenizde destekleyecek şekilde çağırmasını sağlar.  
> Yalnızca ses bildirimi için Azure portal eylem grubunda bugün desteklenen ülke kodu + 1 ' dir (Birleşik Devletler). 

Desteklenen ülkeler/bölgeler için fiyatlandırma, [Azure izleyici fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/monitor/)listelenmiştir.

### <a name="webhook"></a>Web Kancası

> [!NOTE]
> Web kancası eyleminin kullanılması için hedef Web kancası uç noktasının, uyarının ayrıntılarının başarıyla çalışması gerekmez veya POST işleminin bir parçası olarak sağlanmış olan uyarı bağlamı bilgilerini ayrıştırmaktan emin olmanız gerekir. 

> Güvenliğin ihlal olmadığından emin olmak için kullanıcının Web kancası hizmet sorumlusu 'nın **sahibi** olması gerekir. Herhangi bir Azure müşterisi Portal aracılığıyla tüm nesne kimliklerine erişebilir, sahibi denetlemeden, herkes güvenliği ihlal eden Azure izleyici uyarı bildirimi için güvenli Web kancasını kendi eylem grubuna ekleyebilir.

> Web kancası uç noktası, uyarı bağlamı bilgilerini kendi kendine işleyemez, Web kancasının beklenen veri biçimiyle eşleşecek şekilde uyarı bağlamı bilgilerini özel bir şekilde işlemek için bir [mantıksal uygulama eylemi](./action-groups-logic-app.md) gibi bir çözüm kullanabilirsiniz.

Web kancaları aşağıdaki kurallar kullanılarak işlenir
- Bir Web kancası çağrısı en fazla 3 kez denendi.
- Zaman aşımı süresi içinde bir yanıt alınmadığında veya aşağıdaki HTTP durum kodlarından biri döndürülürse, çağrı yeniden denenir: 408, 429, 503 veya 504.
- İlk çağrı bir yanıt için 10 saniye bekler.
- İkinci ve üçüncü denemeler, yanıt için 30 saniye bekler.
- Web kancasını çağırma 3 girişimi başarısız olduysa, hiçbir eylem grubu 15 dakika boyunca bitiş noktasını çağırmaz.

Kaynak IP adresi aralıkları:

 - 13.66.60.119/32
 - 13.66.143.220/30
 - 13.66.202.14/32
 - 13.66.248.225/32
 - 13.66.249.211/32
 - 13.67.10.124/30
 - 13.69.109.132/30
 - 13.71.199.112/30
 - 13.77.53.216/30
 - 13.77.172.102/32
 - 13.77.183.209/32
 - 13.78.109.156/30
 - 13.84.49.247/32
 - 13.84.51.172/32
 - 13.84.52.58/32
 - 13.86.221.220/30
 - 13.106.38.142/32
 - 13.106.38.148/32
 - 13.106.54.3/32
 - 13.106.54.19/32
 - 13.106.57.181/32
 - 13.106.57.196/31
 - 20.38.149.132/30
 - 20.42.64.36/30
 - 20.43.121.124/30
 - 20.44.17.220/30
 - 20.45.123.236/30
 - 20.72.27.152/30
 - 20.150.172.228/30
 - 20.192.238.124/30
 - 20.193.202.4/30
 - 40.68.195.137/32
 - 40.68.201.58/32
 - 40.68.201.65/32
 - 40.68.201.206/32
 - 40.68.201.211/32
 - 40.68.204.18/32
 - 40.115.37.106/32
 - 40.121.219.215/32
 - 40.121.221.62/32
 - 40.121.222.201/32
 - 40.121.223.186/32
 - 51.104.9.100/30
 - 52.183.20.244/32
 - 52.183.31.0/32
 - 52.183.94.59/32
 - 52.184.145.166/32
 - 191.233.50.4/30
 - 191.233.207.64/26
 - 2603:1000:4:402::178/125
 - 2603:1000:104:402::178/125
 - 2603:1010:6:402::178/125
 - 2603:1010:101:402::178/125
 - 2603:1010:304:402::178/125
 - 2603:1010:404:402::178/125
 - 2603:1020:5:402::178/125
 - 2603:1020:206:402::178/125
 - 2603:1020:305:402::178/125
 - 2603:1020:405:402::178/125
 - 2603:1020:605:402::178/125
 - 2603:1020:705:402::178/125
 - 2603:1020:805:402::178/125
 - 2603:1020:905:402::178/125
 - 2603:1020: A04:402:: 178/125
 - 2603:1020: B04:402:: 178/125
 - 2603:1020: C04:402:: 178/125
 - 2603:1020: D04:402:: 178/125
 - 2603:1020: E04:402:: 178/125
 - 2603:1020: F04:402:: 178/125
 - 2603:1020:1004:800:: F8/125
 - 2603:1020:1104:400::178/125
 - 2603:1030: f:400:: 978/125
 - 2603:1030:10:402::178/125
 - 2603:1030:104:402::178/125
 - 2603:1030:107:400:: F0/125
 - 2603:1030:210:402::178/125
 - 2603:1030:40B: 400:: 978/125
 - 2603:1030:40C: 402:: 178/125
 - 2603:1030:504:802:: F8/125
 - 2603:1030:608:402::178/125
 - 2603:1030:807:402::178/125
 - 2603:1030: A07:402:: 8f8/125
 - 2603:1030: B04:402:: 178/125
 - 2603:1030: C06:400:: 978/125
 - 2603:1030: F05:402:: 178/125
 - 2603:1030:1005:402::178/125
 - 2603:1040:5:402::178/125
 - 2603:1040:207:402::178/125
 - 2603:1040:407:402::178/125
 - 2603:1040:606:402::178/125
 - 2603:1040:806:402::178/125
 - 2603:1040:904:402::178/125
 - 2603:1040: A06:402:: 178/125
 - 2603:1040: B04:402:: 178/125
 - 2603:1040: C06:402:: 178/125
 - 2603:1040: D04:800:: F8/125
 - 2603:1040: F05:402:: 178/125
 - 2603:1040:1104:400::178/125
 - 2603:1050:6:402::178/125
 - 2603:1050:403:400:: 1F8/125

Bu IP adreslerindeki değişikliklerle ilgili güncelleştirmeleri almak için, eylem grupları hizmeti hakkında bilgilendirici bildirimleri izleyen bir hizmet sistem durumu uyarısı yapılandırmanız önerilir.

Bir eylem grubunda sınırlı sayıda Web kancası eylemi olabilir.

Kaynak IP adreslerine sık yapılan güncelleştirmeler, Web kancası içinde oldukça zaman alabilir. *ActionGroup* Için **hizmet etıketı** kullanımı, IP adreslerinde sık sık güncelleştirmelerin karmaşıklığını el ile en aza indirmenize yardımcı olur. Yukarıda paylaşılan kaynak IP adresleri Aralık önekleri, Microsoft tarafından **hizmet etiketiyle** aynı şekilde otomatik olarak yönetilir.

#### <a name="service-tag"></a>Hizmet etiketi
Hizmet etiketi, belirli bir Azure hizmetinden bir IP adresi önekleri grubunu temsil eder. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir ve bir ActionGroup için sık sık güncelleştirmelerin ağ güvenlik kurallarına göre karmaşıklığını en aza indirir.

1. Azure portal ' de Azure Hizmetleri *ağ güvenlik grubu* araması ' nı arayın.
2. **Ekle** ' ye tıklayın ve ağ güvenlik grubu oluşturun.

   1. Kaynak grubu adını ekleyin ve ardından *örnek ayrıntılarını* girin.
   1. **Gözden geçir + oluştur** ' a ve ardından *Oluştur*' a tıklayın.
   
   :::image type="content" source="media/action-groups/action-group-create-security-group.png" alt-text="Ağ güvenlik grubu oluşturma hakkında örnek."border="true":::

3. Kaynak grubu ' na gidin ve ardından oluşturduğunuz *ağ güvenlik grubu* ' na tıklayın.

    1. *Gelen güvenlik kuralları*' nı seçin.
    1. **Ekle**'ye tıklayın.
    
    :::image type="content" source="media/action-groups/action-group-add-service-tag.png" alt-text="Hizmet etiketi ekleme hakkında örnek."border="true":::

4. Sağ bölmede yeni bir pencere açılır.
    1.  Kaynak seçin: **hizmet etiketi**
    1.  Kaynak hizmet etiketi: **ActionGroup**
    1.  **Ekle**'ye tıklayın.
    
    :::image type="content" source="media/action-groups/action-group-service-tag.png" alt-text="Hizmet etiketi ekleme hakkında örnek."border="true":::

## <a name="next-steps"></a>Sonraki adımlar
* [SMS uyarı davranışı](./alerts-sms-behavior.md)hakkında daha fazla bilgi edinin.  
* [Etkinlik günlüğü uyarısı Web kancası şemasının anlaşılmasını](./activity-log-alerts-webhook.md)elde edin.  
* [ITSM Bağlayıcısı](./itsmc-overview.md)hakkında daha fazla bilgi edinin.
* Uyarıların [hız sınırlaması](./alerts-rate-limiting.md) hakkında daha fazla bilgi edinin.
* [Etkinlik günlüğü uyarılarına genel bir bakış](./alerts-overview.md)elde edin ve uyarıları alma hakkında bilgi edinin.  
* [Bir hizmet durumu bildirimi gönderildiğinde uyarıların nasıl yapılandırılacağını](../../service-health/alerts-activity-log-service-notifications-portal.md)öğrenin.
