---
title: Azure'da Office 365 yönetim çözümü | Microsoft Dokümanlar
description: Bu makalede, Azure'da Office 365 çözümünün yapılandırması ve kullanımı hakkında ayrıntılı bilgi verilmektedir.  Azure Monitor'da oluşturulan Office 365 kayıtlarının ayrıntılı açıklamasını içerir.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/08/2019
ms.openlocfilehash: 0018ae55ab74e691577a34a397c15355587e0fac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77663275"
---
# <a name="office-365-management-solution-in-azure-preview"></a>Azure'da Office 365 yönetim çözümü (Önizleme)

![Office 365 logosu](media/solution-office-365/icon.png)


> [!IMPORTANT]
> ## <a name="solution-update"></a>Çözüm güncelleştirmesi
> Bu çözüm, [Azure Sentinel'deki](../../sentinel/overview.md) [Office 365](../../sentinel/connect-office-365.md) Genel Kullanılabilirlik çözümü ve [Azure AD raporlama ve izleme çözümü yle](../../active-directory/reports-monitoring/plan-monitoring-and-reporting.md)değiştirilmiştir. Birlikte, gelişmiş bir yapılandırma deneyimiyle önceki Azure Monitor Office 365 çözümünün güncelleştirilmiş bir sürümünü sağlarlar. Mevcut çözümü 30 Nisan 2020 tarihine kadar kullanmaya devam edebilirsiniz.
> 
> Azure Sentinel, günlükleri alan ve algılamalar, araştırmalar, avcılık ve makine öğrenimi odaklı öngörüler dahil olmak üzere ek SIEM işlevselliği sağlayan bulut yerel güvenlik bilgileri ve Olay Yönetimi çözümüdür. Azure Sentinel'i kullanmak artık size Office 365 SharePoint etkinliğini ve Exchange yönetim günlüklerini almanızı sağlayacaktır.
> 
> Azure REKLAM raporlaması, etkinliklerde oturum açma, denetim etkinlikleri ve dizininizdeki değişiklikler de dahil olmak üzere ortamınızdaki Azure AD etkinliğindeki günlüklerin daha kapsamlı bir görünümünü sağlar. Azure AD günlüklerini bağlamak için [Azure Sentinel Azure AD konektörünü](../../sentinel/connect-azure-active-directory.md) kullanabilir veya Azure AD [günlükleri tümleştirmesini Azure Monitor ile](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)yapılandırabilirsiniz. 
>
> Azure REKLAM günlüğü koleksiyonu Azure Monitor fiyatlandırmasına tabi tutulur.  Daha fazla bilgi için [Azure Monitor fiyatlandırması'na](https://azure.microsoft.com/pricing/details/monitor/) bakın.
>
> Azure Sentinel Office 365 çözümlerini kullanmak için:
> 1. Azure Sentinel'de Office 365 bağlayıcısını kullanmak çalışma alanınızın fiyatlandırmasını etkiler. Daha fazla bilgi için Azure [Sentinel fiyatlandırması'na](https://azure.microsoft.com/pricing/details/azure-sentinel/)bakın.
> 2. Azure Monitor Office 365 çözümünü zaten kullanıyorsanız, önce aşağıdaki Kaldır [bölümündeki](#uninstall)komut dosyasını kullanarak kaldırmanız gerekir.
> 3. Çalışma alanınızda [Azure Sentinel çözümünüzü etkinleştirin.](../../sentinel/quickstart-onboard.md)
> 4. Azure Sentinel'deki **Veri bağlayıcıları** sayfasına gidin ve **Office 365** bağlayıcısını etkinleştirin.
>
> ## <a name="frequently-asked-questions"></a>Sık sorulan sorular
> 
> ### <a name="q-is-it-possible-to-on-board-the-office-365-azure-monitor-solution-between-now-and-april-30th"></a>S: Şu andan 30 Nisan'a kadar Office 365 Azure Monitör çözümüne binme mümkün mü?
> Hayır, Azure Monitor Office 365 çözümü artık kullanılabilir komut dosyaları yok. Çarşünün çözüm çözümü
> 
> ### <a name="q-will-the-tables-and-schemas-be-changed"></a>S: Tablolar ve şemalar değişecek mi?
> **OfficeActivity** tablo adı ve şema geçerli çözümde olduğu gibi kalır. Azure AD verilerine başvuran sorgular hariç olmak üzere, yeni çözümde aynı sorguları kullanmaya devam edebilirsiniz.
> 
> Yeni [Azure AD raporlama ve izleme çözüm](../../active-directory/reports-monitoring/plan-monitoring-and-reporting.md) günlükleri **OfficeEtkinliği**yerine Oturum Açma ve [Denetleme Günlükleri](../../active-directory/reports-monitoring/concept-audit-logs.md) tablolarına yutulacaktır. [SigninLogs](../../active-directory/reports-monitoring/concept-sign-ins.md) Daha fazla bilgi için, Azure Sentinel ve Azure Monitor kullanıcıları için de alakalı olan [Azure AD günlüklerini nasıl analiz edin;](../../active-directory/reports-monitoring/howto-analyze-activity-logs-log-analytics.md)
> 
> **OfficeActivity'den** **SigninLogs'a**sorguları dönüştürmek için aşağıdaki örnekler vereme örnekleri şunlardır:
> 
> **Kullanıcıya göre sorgu başarısız oturum açma:**
> 
> ```Kusto
> OfficeActivity
> | where TimeGenerated >= ago(1d) 
> | where OfficeWorkload == "AzureActiveDirectory"                      
> | where Operation == 'UserLoginFailed'
> | summarize count() by UserId 
> ```
> 
> ```Kusto
> SigninLogs
> | where ConditionalAccessStatus == "failure" or ConditionalAccessStatus == "notApplied"
> | summarize count() by UserDisplayName
> ```
> 
> **Azure AD işlemlerini görüntüleyin:**
> 
> ```Kusto
> OfficeActivity
> | where OfficeWorkload =~ "AzureActiveDirectory"
> | sort by TimeGenerated desc
> | summarize AggregatedValue = count() by Operation
> ```
> 
> ```Kusto
> AuditLogs
> | summarize count() by OperationName
> ```
> 
> ### <a name="q-how-can-i-on-board-azure-sentinel"></a>S: Azure Sentinel'de nasıl olabilirim?
> Azure Sentinel, yeni veya mevcut Log Analytics çalışma alanında etkinleştirebileceğiniz bir çözümdür. Daha fazla bilgi için [Azure Sentinel biniş belgelerine](../../sentinel/quickstart-onboard.md)bakın.
>
> ### <a name="q-do-i-need-azure-sentinel-to-connect-the-azure-ad-logs"></a>S: Azure REKLAM günlüklerini bağlamak için Azure Sentinel'e ihtiyacım var mı?
> Azure Sentinel çözümüyle ilgili olmayan [Azure AD günlükleri tümleştirmesini Azure Monitor ile](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)yapılandırabilirsiniz. Azure Sentinel, Azure REKLAM günlükleri için yerel bir bağlayıcı ve kutu dışı içerik sağlar. Daha fazla bilgi için, kutunun dışında güvenlik odaklı içerik le ilgili aşağıdaki soruya bakın.
>
> ###   <a name="q-what-are-the-differences-when-connecting-azure-ad-logs-from-azure-sentinel-and-azure-monitor"></a>S: Azure Sentinel ve Azure Monitor'dan Azure AD günlüklerini bağlarken ne gibi farklar nelerdir?
> Azure Sentinel ve Azure Monitor, aynı Azure [REKLAM raporlama ve izleme çözümüne](../../active-directory/reports-monitoring/plan-monitoring-and-reporting.md)dayalı olarak Azure AD günlüklerine bağlanır. Azure Sentinel, aynı verileri bağlayan ve izleme bilgileri sağlayan tek tıklamayla yerel bir bağlayıcı sağlar.
>
> ###   <a name="q-what-do-i-need-to-change-when-moving-to-the-new-azure-ad-reporting-and-monitoring-tables"></a>S: Yeni Azure AD raporlama ve izleme tablolarına taşınırken neyi değiştirmem gerekir?
> Uyarılardaki sorgular, panolar ve Office 365 Azure AD verilerini kullanarak oluşturduğunuz tüm içerikler de dahil olmak üzere Azure REKLAM verilerini kullanan tüm sorguların yeni tablolar kullanılarak yeniden oluşturulması gerekir.
>
> Azure Sentinel ve Azure AD, Azure AD raporlama ve izleme çözümüne taşınırken kullanabileceğiniz yerleşik içerik sağlar. Daha fazla bilgi için, kullanıma hazır güvenlik odaklı içerik ve [Azure Etkin Dizin raporları için Azure Monitor çalışma kitaplarını nasıl kullanacağıyla](../../active-directory/reports-monitoring/howto-use-azure-monitor-workbooks.md)ilgili bir sonraki soruya bakın. 
>
> ### <a name="q-how-i-can-use-the-azure-sentinel-out-of-the-box-security-oriented-content"></a>S: Azure Sentinel'i kullanıma açık güvenlik odaklı içeriği nasıl kullanabilirim?
> Azure Sentinel, Office 365 ve Azure AD günlüklerini temel alarak kutudan uzak güvenlik panelleri, özel uyarı sorguları, av sorguları, araştırma ve otomatik yanıt özellikleri sağlar. Daha fazla bilgi edinmek için Azure Sentinel GitHub'ı ve öğreticileri keşfedin:
>
> - [Tehditleri kutudan çıkarmadan algılama](../../sentinel/tutorial-detect-threats-built-in.md)
> - [Şüpheli tehditleri algılamak için özel analiz kuralları oluşturma](../../sentinel/tutorial-detect-threats-custom.md)
> - [Verilerinizi izleme](../../sentinel/tutorial-monitor-your-data.md)
> - [Azure Sentinel ile ilgili olayları araştırma](../../sentinel/tutorial-investigate-cases.md)
> - [Azure Sentinel'de otomatik tehdit yanıtları ayarlama](../../sentinel/tutorial-respond-threats-playbook.md)
> - [Azure Sentinel GitHub topluluğu](https://github.com/Azure/Azure-Sentinel/tree/master/Playbooks)
> 
> ### <a name="q-does-azure-sentinel-provide-additional-connectors-as-part-of-the-solution"></a>S: Azure Sentinel çözümün bir parçası olarak ek bağlayıcılar sağlıyor mu?
> Evet, bkz. [Azure Sentinel veri kaynaklarını bağlayın.](../../sentinel/connect-data-sources.md)
> 
> ###   <a name="q-what-will-happen-on-april-30-do-i-need-to-offboard-beforehand"></a>S: 30 Nisan'da ne olacak? Önceden izinden ayrılmam gerekiyor mu?
> 
> - **Office365** çözümünden veri alamazsınız. Çözüm artık Market'te kullanılmayacak
> - Azure Sentinel müşterileri için, Log Analytics çalışma alanı çözümü **Office365,** Azure Sentinel **SecurityInsights** çözümüne dahil edilecektir.
> - Çözümünüzü el ile kapalı kullanmazsanız, 30 Nisan'da verilerinizin bağlantısı otomatik olarak kesilir.
> 
> ### <a name="q-will-my-data-transfer-to-the-new-solution"></a>S: Verilerim yeni çözüme aktayacak mı?
> Evet. **Office 365** çözümünü çalışma alanınızdan kaldırdığınızda, şema kaldırıldığıiçin verileri geçici olarak kullanılamaz hale gelir. Sentinel'deki yeni **Office 365** bağlayıcısını etkinleştirdiğinizde, şema çalışma alanına geri yüklenir ve zaten toplanan veriler kullanılabilir hale gelir. 
 

Office 365 yönetim çözümü, Azure Monitor'da Office 365 ortamınızı izlemenize olanak tanır.

- Kullanım modellerini analiz etmek ve davranış eğilimlerini belirlemek için Office 365 hesaplarınızdaki kullanıcı etkinliklerini izleyin. Örneğin, kuruluşunuz dışında paylaşılan dosyalar veya en popüler SharePoint siteleri gibi belirli kullanım senaryolarını ayıklayabilirsiniz.
- Yapılandırma değişikliklerini veya yüksek ayrıcalık işlemlerini izlemek için yönetici etkinliklerini izleyin.
- Kuruluş gereksinimlerinize göre özelleştirilebilen istenmeyen kullanıcı davranışlarını algılayıp araştırın.
- Denetim ve uyumluluk gösterin. Örneğin, denetim ve uyumluluk sürecinde size yardımcı olabilecek gizli dosyalardaki dosya erişim işlemlerini izleyebilirsiniz.
- Kuruluşunuzun Office 365 etkinlik verilerinin üstünde [günlük sorgularını](../log-query/log-query-overview.md) kullanarak operasyonel sorun giderme gerçekleştirin.


## <a name="uninstall"></a>Kaldırma

Bir [yönetim çözümleçle](solutions.md#remove-a-monitoring-solution)kaldırma işlemini kullanarak Office 365 yönetim çözümlerini kaldırabilirsiniz. Bu, Office 365'ten Azure Monitor'a toplanan verileri durdurmaz. Office 365 aboneliğini iptal etmek ve veri toplamayı durdurmak için aşağıdaki yordamı izleyin.

1. Aşağıdaki komut dosyasını *office365_unsubscribe.ps1*olarak kaydedin.

    ```powershell
    param (
        [Parameter(Mandatory=$True)][string]$WorkspaceName,
        [Parameter(Mandatory=$True)][string]$ResourceGroupName,
        [Parameter(Mandatory=$True)][string]$SubscriptionId,
        [Parameter(Mandatory=$True)][string]$OfficeTennantId
    )
    $line='#-------------------------------------------------------------------------------------------------------------------------------------------------------------------------'
    
    $line
    IF ($Subscription -eq $null)
        {Login-AzAccount -ErrorAction Stop}
    $Subscription = (Select-AzSubscription -SubscriptionId $($SubscriptionId) -ErrorAction Stop)
    $Subscription
    $option = [System.StringSplitOptions]::RemoveEmptyEntries 
    $Workspace = (Set-AzOperationalInsightsWorkspace -Name $($WorkspaceName) -ResourceGroupName $($ResourceGroupName) -ErrorAction Stop)
    $Workspace
    $WorkspaceLocation= $Workspace.Location
    
    # Client ID for Azure PowerShell
    $clientId = "1950a258-227b-4e31-a9cf-717495945fc2"
    # Set redirect URI for Azure PowerShell
    $redirectUri = "urn:ietf:wg:oauth:2.0:oob"
    $domain='login.microsoftonline.com'
    $adTenant =  $Subscription[0].Tenant.Id
    $authority = "https://login.windows.net/$adTenant";
    $ARMResource ="https://management.azure.com/";
    $xms_client_tenant_Id ='55b65fb5-b825-43b5-8972-c8b6875867c1'
    
    switch ($WorkspaceLocation) {
           "USGov Virginia" { 
                             $domain='login.microsoftonline.us';
                              $authority = "https://login.microsoftonline.us/$adTenant";
                              $ARMResource ="https://management.usgovcloudapi.net/"; break} # US Gov Virginia
           default {
                    $domain='login.microsoftonline.com'; 
                    $authority = "https://login.windows.net/$adTenant";
                    $ARMResource ="https://management.azure.com/";break} 
                    }
    
    Function RESTAPI-Auth { 
    
    $global:SubscriptionID = $Subscription.SubscriptionId
    # Set Resource URI to Azure Service Management API
    $resourceAppIdURIARM=$ARMResource;
    # Authenticate and Acquire Token 
    # Create Authentication Context tied to Azure AD Tenant
    $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList $authority
    # Acquire token
    $platformParameters = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.PlatformParameters" -ArgumentList "Auto"
    $global:authResultARM = $authContext.AcquireTokenAsync($resourceAppIdURIARM, $clientId, $redirectUri, $platformParameters)
    $global:authResultARM.Wait()
    $authHeader = $global:authResultARM.Result.CreateAuthorizationHeader()
    $authHeader
    }
    
    Function Office-UnSubscribe-Call{
    
    #----------------------------------------------------------------------------------------------------------------------------------------------
    $authHeader = $global:authResultARM.Result.CreateAuthorizationHeader()
    $ResourceName = "https://manage.office.com"
    $SubscriptionId   = $Subscription[0].Subscription.Id
    $OfficeAPIUrl = $ARMResource + 'subscriptions/' + $SubscriptionId + '/resourceGroups/' + $ResourceGroupName + '/providers/Microsoft.OperationalInsights/workspaces/' + $WorkspaceName + '/datasources/office365datasources_'  + $SubscriptionId + $OfficeTennantId + '?api-version=2015-11-01-preview'
    
    $Officeparams = @{
        ContentType = 'application/json'
        Headers = @{
        'Authorization'="$($authHeader)"
        'x-ms-client-tenant-id'=$xms_client_tenant_Id
        'Content-Type' = 'application/json'
        }
        Method = 'Delete'
        URI = $OfficeAPIUrl
      }
    
    $officeresponse = Invoke-WebRequest @Officeparams 
    $officeresponse
    
    }
    
    #GetDetails 
    RESTAPI-Auth -ErrorAction Stop
    Office-UnSubscribe-Call -ErrorAction Stop
    ```

2. Komut dosyasını aşağıdaki komutla çalıştırın:

    ```
    .\office365_unsubscribe.ps1 -WorkspaceName <Log Analytics workspace name> -ResourceGroupName <Resource Group name> -SubscriptionId <Subscription ID> -OfficeTennantID <Tenant ID> 
    ```

    Örnek:

    ```powershell
    .\office365_unsubscribe.ps1 -WorkspaceName MyWorkspace -ResourceGroupName MyResourceGroup -SubscriptionId '60b79d74-f4e4-4867-b631-yyyyyyyyyyyy' -OfficeTennantID 'ce4464f8-a172-4dcf-b675-xxxxxxxxxxxx'
    ```

Kimlik bilgileri için istenir. Log Analytics çalışma alanınız için kimlik bilgilerini sağlayın.

## <a name="data-collection"></a>Veri toplama

Verilerin başlangıçta toplanması birkaç saat sürebilir. Toplamaya başladıktan sonra, Office 365 her kayıt oluşturulduğunda Azure Monitor'a ayrıntılı verileriçeren bir [webhook bildirimi](https://msdn.microsoft.com/office-365/office-365-management-activity-api-reference#receiving-notifications) gönderir. Bu kayıt alındıktan sonra birkaç dakika içinde Azure Monitor'da kullanılabilir.

## <a name="using-the-solution"></a>Çözümü kullanma

[!INCLUDE [azure-monitor-solutions-overview-page](../../../includes/azure-monitor-solutions-overview-page.md)]

Günlük Analizi çalışma alanınıza Office 365 çözümünüzü eklediğinizde, **Office 365** döşemesi panonuza eklenir. Bu kutucukta, ortamınızdaki bilgisayarların sayısına ve güncelleştirme uyumluluğuna ilişkin bir sayı ve grafik gösterimi görüntülenir.<br><br>
![Office 365 Özet Karo](media/solution-office-365/tile.png)  

**Office** 365 panosunu açmak için **Office 365** döşemesini tıklatın.

![Office 365 Pano](media/solution-office-365/dashboard.png)  

Pano aşağıdaki tabloda gösterilen sütunları içerir. Her sütun, belirtilen kapsam ve zaman aralığı için bu sütunun ölçütleriyle eşleşen sayıya göre en üstteki uyarıları listeler. Sütunun altındaki tümleri gör'e tıklayarak veya sütun üstbilgisini tıklatarak tüm listeyi sağlayan bir günlük araması çalıştırabilirsiniz.

| Sütun | Açıklama |
|:--|:--|
| İşlemler | Tüm izlenen Office 365 aboneliklerinizden etkin kullanıcılar hakkında bilgi sağlar. Ayrıca zaman içinde meydana gelen etkinlik sayısını da görebilirsiniz.
| Exchange | Exchange Server'da Add-Mailbox İzni veya Set-Posta Kutusu gibi etkinliklerin dökümünü gösterir. |
| SharePoint | Kullanıcıların SharePoint belgelerinde gerçekleştirdiği en iyi etkinlikleri gösterir. Bu döşemeden ayrıntıya girdiğiniz zaman, arama sayfası hedef belge ve bu etkinliğin konumu gibi bu etkinliklerin ayrıntılarını gösterir. Örneğin, Dosyaya Erişilen bir olay için erişilen belgeyi, ilişkili hesap adını ve IP adresini görebilirsiniz. |
| Azure Active Directory | Kullanıcı Parolası Sıfırlama ve Giriş Denemelerini Sıfırlama gibi en iyi kullanıcı etkinliklerini içerir. Detaya indiğinizde, Sonuç Durumu gibi bu etkinliklerin ayrıntılarını görebilirsiniz. Azure Etkin Dizininizde şüpheli etkinlikleri izlemek istiyorsanız, bu çoğunlukla yararlıdır. |




## <a name="azure-monitor-log-records"></a>Azure Monitör günlük kayıtları

Office 365 çözümü tarafından Azure Monitor'daki Log Analytics çalışma alanında oluşturulan tüm kayıtların bir **Office Etkinliği** **Türü** vardır.  **Officeİş Yükü** özelliği, kaydın hangi Office 365 hizmetinin hangi hizmete atıfta bulunduğunu belirler - Exchange, AzureActiveDirectory, SharePoint veya OneDrive.  **RecordType** özelliği işlem türünü belirtir.  Özellikler her işlem türüiçin değişir ve aşağıdaki tablolarda gösterilir.

### <a name="common-properties"></a>Ortak özellikler

Aşağıdaki özellikler tüm Office 365 kayıtları için ortaktür.

| Özellik | Açıklama |
|:--- |:--- |
| Tür | *Ofis Etkinliği* |
| Müşteri IP | Etkinlik günlüğe kaydedildiğinde kullanılan cihazın IP adresi. IP adresi IPv4 veya IPv6 adresi biçiminde görüntülenir. |
| Ofis İş Yükü | Kaydın atıfta bulunduğu Office 365 hizmeti.<br><br>AzureActiveDirectory<br>Exchange<br>SharePoint|
| İşlem | Kullanıcı veya yönetici etkinliğinin adı.  |
| OrganizationId | Kuruluşunuzun Office 365 kiracısının GUID'i. Bu değer, oluştuğu Office 365 hizmetinden bağımsız olarak kuruluşunuz için her zaman aynı olacaktır. |
| RecordType | Gerçekleştirilen işlem türü. |
| ResultStatus | Eylemin (Operation özelliğinde belirtilen) başarılı olup olmadığını belirtir. Olası değerler Başarılı, Kısmen Başarılı veya Başarısız oldu. Exchange yöneticisi etkinliği için değer Doğru veya Yanlış'tır. |
| UserId | Kaydın günlüğe kaydedilmesiyle sonuçlanan eylemi gerçekleştiren kullanıcının UPN 'si (Kullanıcı Adı); örneğin, my_name@my_domain_name. Sistem hesapları (SHAREPOINT\system veya NTAUTHORITY\SYSTEM gibi) tarafından gerçekleştirilen etkinlik kayıtlarının da dahil olduğunu unutmayın. | 
| UserKey | UserId özelliğinde tanımlanan kullanıcı için alternatif bir kimlik.  Örneğin, bu özellik, SharePoint, OneDrive for Business ve Exchange'deki kullanıcılar tarafından gerçekleştirilen etkinlikler için pasaportbenzersiz kimliği (PUID) ile doldurulur. Bu özellik, sistem hesapları tarafından gerçekleştirilen diğer hizmetler de ve olaylarda meydana gelen olaylar için UserID özelliğiyle aynı değeri de belirtebilir|
| UserType | İşlemi gerçekleştiren kullanıcının türü.<br><br>Yönetici<br>Uygulama<br>DcAdmin<br>Normal<br>Ayrıldı<br>ServicePrincipal<br>Sistem |


### <a name="azure-active-directory-base"></a>Azure Etkin Dizin tabanı

Aşağıdaki özellikler tüm Azure Etkin Dizin kayıtlarında yaygındır.

| Özellik | Açıklama |
|:--- |:--- |
| Ofis İş Yükü | AzureActiveDirectory |
| RecordType     | AzureActiveDirectory |
| AzureActiveDirectory_EventType | Azure AD etkinliğinin türü. |
| Genişletilmiş Özellikler | Azure AD etkinliğinin genişletilmiş özellikleri. |


### <a name="azure-active-directory-account-logon"></a>Azure Active Directory Hesabı girişi

Bu kayıtlar, Etkin Dizin kullanıcısı oturum açmaya çalıştığında oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| `OfficeWorkload` | AzureActiveDirectory |
| `RecordType`     | AzureActiveDirectoryAccountLogon |
| `Application` | Office 15 gibi hesap oturum açma olayını tetikleyen uygulama. |
| `Client` | Hesap giriş olayı için kullanılan istemci aygıtı, aygıt işletim sistemi ve aygıt tarayıcısı hakkındaki ayrıntılar. |
| `LoginStatus` | Bu özellik doğrudan OrgIdLogon.LoginStatus adresinden dir. Çeşitli ilginç oturum açma hatalarının eşlemesi algoritmaları uyararak yapılabilir. |
| `UserDomain` | Kiracı Kimlik Bilgileri (TII). | 


### <a name="azure-active-directory"></a>Azure Active Directory

Bu kayıtlar, Azure Etkin Dizin nesnelerinde değişiklik veya eklemeyapıldığında oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| Ofis İş Yükü | AzureActiveDirectory |
| RecordType     | AzureActiveDirectory |
| AADTarget | Eylemin (Operasyon özelliği tarafından tanımlanan) gerçekleştirildiği kullanıcı. |
| Aktör | Eylemi gerçekleştiren kullanıcı veya hizmet sorumlusu. |
| AktörContextId | Aktörün ait olduğu kuruluşun GUID'i. |
| AktörIpAddress | Aktörün IPV4 veya IPV6 adres biçimindeki IP adresi. |
| InterSystemsId | Office 365 hizmetindeki bileşenler arasındaki eylemleri izleyen GUID. |
| IntraSystemid |   Eylemi izlemek için Azure Active Directory tarafından oluşturulan GUID. |
| SupportTicketId | "Adına hareket et" durumlarında eylem için müşteri destek bilet kimliği. |
| HedefBağlamid | Hedeflenen kullanıcının ait olduğu kuruluşun GUID'i. |


### <a name="data-center-security"></a>Veri Merkezi Güvenliği

Bu kayıtlar Veri Merkezi Güvenlik denetim verilerinden oluşturulur.  

| Özellik | Açıklama |
|:--- |:--- |
| Etkili Organizasyon | Yükseklik/cmdlet'in hedef aldığı kiracının adı. |
| YükseklikOnay Süresi | Yükseklik onaylandığı zaman için zaman damgası. |
| YükseklikOnaylayıcı | Bir Microsoft yöneticisinin adı. |
| YükseklikSüresi | Yükselmenin etkin olduğu süre. |
| YükseklikRequestId |  Yükseklik isteği için benzersiz bir tanımlayıcı. |
| YükseklikRolü | Yükseklik için istenen rol. |
| Yükseklik Zamanı | Yükselmenin başlangıç saati. |
| Start_time | Cmdlet yürütmenin başlangıç saati. |


### <a name="exchange-admin"></a>Exchange Yöneticisi

Bu kayıtlar Exchange yapılandırmasına değişiklikler yapıldığında oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| Ofis İş Yükü | Exchange |
| RecordType     | ExchangeAdmin |
| Harici Erişim |  Cmdlet'in kuruluşunuzdaki bir kullanıcı, Microsoft veri merkezi personeli veya veri merkezi hizmet hesabı veya temsilci bir yönetici tarafından çalıştırılıp çalıştırılmayacağını belirtir. False değeri, cmdlet'in kuruluşunuzdaki biri tarafından çalıştırıldığını gösterir. True değeri, cmdlet'in veri merkezi personeli, veri merkezi hizmet hesabı veya temsilci yöneticisi tarafından çalıştırıldığını gösterir. |
| ModifiyeObjectResolvedName |  Bu cmdlet tarafından değiştirilen nesnenin kullanıcı dostu adıdır. Bu yalnızca cmdlet nesneyi değiştirirse günlüğe kaydedilir. |
| OrganizationName | Kiracının adı. |
| OriginatingServer | Cmdlet'in yürütüldürün sunucunun adı. |
| Parametreler | İşlemler özelliğinde tanımlanan cmdlet ile kullanılan tüm parametrelerin adı ve değeri. |


### <a name="exchange-mailbox"></a>Exchange Posta Kutusu

Bu kayıtlar, Exchange posta kutularında değişiklikler veya eklemeler yapıldığında oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| Ofis İş Yükü | Exchange |
| RecordType     | ExchangeItem |
| ClientInfoString | Tarayıcı sürümü, Outlook sürümü ve mobil cihaz bilgileri gibi işlemi gerçekleştirmek için kullanılan e-posta istemcisi hakkındaki bilgiler. |
| Client_IPAddress | İşlem günlüğe kaydedilirken kullanılan aygıtın IP adresi. IP adresi IPv4 veya IPv6 adresi biçiminde görüntülenir. |
| Müşteri MachineName | Outlook istemcisini barındıran makine adı. |
| Müşteri İşlem Adı | Posta kutusuna erişmek için kullanılan e-posta istemcisi. |
| Müşteri Sürümü | E-posta istemcisinin sürümü. |
| InternalLogonType | Dahili kullanım için ayrılmıştır. |
| Logon_Type | Posta kutusuna erişen ve günlüğe kaydedilen işlemi gerçekleştiren kullanıcı türünü gösterir. |
| LogonUserDisplayName |    İşlemi gerçekleştiren kullanıcının kullanıcı dostu adı. |
| LogonUserSid | İşlemi gerçekleştiren kullanıcının SID'si. |
| Posta KutusuGuid | Erişilen posta kutusunun Exchange GUID'i. |
| MailboxOwnerMasterAccountSid | Posta kutusu sahibi hesabının ana hesabı SID. |
| Posta KutusuSahibiSid | Posta kutusu sahibinin SID'si. |
| Posta KutusuSahibiMUPN | Erişilen posta kutusunun sahibinin e-posta adresi. |


### <a name="exchange-mailbox-audit"></a>Exchange Posta Kutusu Denetimi

Bu kayıtlar, posta kutusu denetim girişi oluşturulduğunda oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| Ofis İş Yükü | Exchange |
| RecordType     | ExchangeItem |
| Öğe | İşlemin gerçekleştirildiği öğeyi temsil eder | 
| SendAsUserMailboxGuid | Olarak e-posta göndermek için erişilen posta kutusunun Exchange GUID. |
| SendAsUserSmtp | Kimliğe bürünmekte olan kullanıcının SMTP adresi. |
| SendonBehalfOfUserMailboxGuid | Adına posta göndermek için erişilen posta kutusunun Exchange GUID. |
| SendOnBehalfOfUserSmtp | E-postanın gönderildiği kullanıcıadına SMTP adresi. |


### <a name="exchange-mailbox-audit-group"></a>Exchange Mailbox Denetim Grubu

Bu kayıtlar, Exchange gruplarına değişiklikler veya eklemeler yapıldığında oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| Ofis İş Yükü | Exchange |
| Ofis İş Yükü | ExchangeItemGroup |
| Etkilenen Öğeler | Gruptaki her öğe hakkında bilgi. |
| CrossMailboxİşlemleri | İşlemin birden fazla posta kutusu olup olmadığını gösterir. |
| DestMailboxId | Yalnızca CrossMailboxOperations parametresi Doğruysa ayarlayın. Hedef posta kutusu GUID'i belirtir. |
| DestMailboxOwnerMasterAccountSid | Yalnızca CrossMailboxOperations parametresi Doğruysa ayarlayın. Hedef posta kutusu sahibinin ana hesap SID'si için SID'yi belirtir. |
| DestMailboxOwnerSid | Yalnızca CrossMailboxOperations parametresi Doğruysa ayarlayın. Hedef posta kutusunun SID'sini belirtir. |
| DestMailboxOwnerUPN | Yalnızca CrossMailboxOperations parametresi Doğruysa ayarlayın. Hedef posta kutusunun sahibinin UPN'sini belirtir. |
| DestFolder | Taşı gibi işlemler için hedef klasör. |
| Klasör | Bir grup öğenin bulunduğu klasör. |
| Klasörler |     Bir işlemle ilgili kaynak klasörler hakkında bilgi; örneğin, klasörler seçilip sonra silinirse. |


### <a name="sharepoint-base"></a>SharePoint Taban

Bu özellikler tüm SharePoint kayıtları için ortaktadır.

| Özellik | Açıklama |
|:--- |:--- |
| Ofis İş Yükü | SharePoint |
| Ofis İş Yükü | SharePoint |
| EventSource | SharePoint'te bir olayın oluştuğunu tanımlar. Olası değerler SharePoint veya ObjectModel'dir. |
| ItemType | Erişilen veya değiştirilen nesne türü. Nesne türleri hakkında ayrıntılar için ItemType tablosuna bakın. |
| MakineDomainInfo | Aygıt eşitleme işlemleri hakkında bilgi. Bu bilgiler yalnızca istekte varsa bildirilir. |
| MachineId |   Aygıt eşitleme işlemleri hakkında bilgi. Bu bilgiler yalnızca istekte varsa bildirilir. |
| Site_ | Kullanıcı tarafından erişilen dosya veya klasörün bulunduğu sitenin GUID'i. |
| Source_name | Denetlenen işlemi tetikleyen varlık. Olası değerler SharePoint veya ObjectModel'dir. |
| Useragent | Kullanıcının istemcisi veya tarayıcısı hakkında bilgi. Bu bilgiler istemci veya tarayıcı tarafından sağlanır. |


### <a name="sharepoint-schema"></a>SharePoint Şeması

Bu kayıtlar, SharePoint'te yapılandırma değişiklikleri yapıldığında oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| Ofis İş Yükü | SharePoint |
| Ofis İş Yükü | SharePoint |
| Özel Etkinlik | Özel olaylar için isteğe bağlı dize. |
| Event_Data |  Özel olaylar için isteğe bağlı yük. |
| Modifiye Özellikler | Özellik, bir sitenin veya site koleksiyonu yönetici grubunun üyesi olarak kullanıcı eklemek gibi yönetici etkinlikleri için dahildir. Özellik, değiştirilen özelliğin adını (örneğin, Site Yöneticisi grubu), değiştirilen özelliğin yeni değerini (site yöneticisi olarak eklenen kullanıcı gibi) ve değiştirilen nesnenin önceki değerini içerir. |


### <a name="sharepoint-file-operations"></a>SharePoint Dosya İşlemleri

Bu kayıtlar SharePoint'teki dosya işlemlerine yanıt olarak oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| Ofis İş Yükü | SharePoint |
| Ofis İş Yükü | SharePointFileİşlem |
| HedefDosya Uzantısı | Kopyalanan veya taşınan bir dosyanın dosya uzantısı. Bu özellik yalnızca FileCopyd ve FileMoved olayları için görüntülenir. |
| Destinationfilename | Kopyalanan veya taşınan dosyanın adı. Bu özellik yalnızca FileCopyd ve FileMoved olayları için görüntülenir. |
| Gidilecek YerlerGöreceli Url | Dosyanın kopyalandığı veya taşındığı hedef klasörün URL'si. SiteURL, DestinationRelativeURL ve DestinationFileName parametreleri için değerlerin birleşimi, kopyalanan dosyanın tam yol adı olan ObjectID özelliğinin değeriyle aynıdır. Bu özellik yalnızca FileCopyd ve FileMoved olayları için görüntülenir. |
| PaylaşımTürü | Kaynağın paylaşıldığı kullanıcıya atanan paylaşım izinlerinin türü. Bu kullanıcı UserSharedWith parametresi tarafından tanımlanır. |
| Site_Url | Kullanıcı tarafından erişilen dosya veya klasörün bulunduğu sitenin URL'si. |
| SourceFileExtension | Kullanıcı tarafından erişilen dosya uzantısı. Erişilen nesne bir klasörse, bu özellik boştur. |
| Sourcefilename |  Kullanıcı tarafından erişilen dosya veya klasörün adı. |
| KaynakRelativeUrl | Kullanıcı tarafından erişilen dosyayı içeren klasörün URL'si. SiteURL, SourceRelativeURL ve SourceFileName parametrelerinin değerlerinin birleşimi, kullanıcı tarafından erişilen dosyanın tam yol adı olan ObjectID özelliğinin değeriyle aynıdır. |
| Kullanıcı Paylaşılan |  Kaynağın paylaşılabilen kullanıcı. |




## <a name="sample-log-queries"></a>Örnek günlük sorguları

Aşağıdaki tablo, bu çözüm tarafından toplanan güncelleştirme kayıtları için örnek günlük sorguları sağlar.

| Sorgu | Açıklama |
| --- | --- |
|Office 365 aboneliğinizdeki tüm işlemleri sayma |OfficeActivity&#124; Özetle sayım() |
|SharePoint sitelerinin kullanımı|OfficeEtkinlik &#124; nerede Officeİş =~ "sharepoint" &#124; \| SiteUrl sıralama tarafından sayma özetlemek asc|
|Kullanıcı türüne göre dosya erişim işlemleri | OfficeActivity&#124;userType'a göre sayım() özetle |
|Exchange'deki dış eylemleri izleme|OfficeActivity &#124; nerede Officeİş =~ "exchange" ve ExternalAccess == true|



## <a name="next-steps"></a>Sonraki adımlar

* Ayrıntılı güncelleştirme verilerini görüntülemek için [Azure Monitor'daki günlük sorgularını](../log-query/log-query-overview.md) kullanın.
* En sevdiğiniz Office 365 arama sorgularını görüntülemek için [kendi panolarınızı oluşturun.](../learn/tutorial-logs-dashboards.md)
* Önemli Office 365 etkinliklerinden proaktif olarak haberdar olmak için [uyarılar oluşturun.](../platform/alerts-overview.md)  
