---
title: Azure 'da Office 365 yönetim çözümü | Microsoft Docs
description: Bu makalede, Azure 'da Office 365 çözümünün yapılandırması ve kullanımıyla ilgili ayrıntılar sağlanmaktadır.  Azure Izleyici 'de oluşturulan Office 365 kayıtlarının ayrıntılı açıklamasını içerir.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/30/2020
ms.openlocfilehash: 2c6eb5407ec62b6e9e771ce257b66fca2a91e0a5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82023614"
---
# <a name="office-365-management-solution-in-azure-preview"></a>Azure 'da Office 365 yönetim çözümü (Önizleme)

![Office 365 logosu](media/solution-office-365/icon.png)

> [!IMPORTANT]
> ## <a name="solution-update"></a>Çözüm güncelleştirmesi
> Bu çözüm, [Azure Sentinel](../../sentinel/overview.md) 'de [Office 365](../../sentinel/connect-office-365.md) genel KULLANıLABILIRLIK çözümüyle ve [Azure AD raporlama ve izleme çözümünde](../../active-directory/reports-monitoring/plan-monitoring-and-reporting.md)değiştirilmiştir. Birlikte, gelişmiş bir yapılandırma deneyimiyle birlikte önceki Azure Izleyici Office 365 çözümünün güncelleştirilmiş bir sürümünü sağlar. Mevcut çözümü 30 Haziran 2020 ' e kadar kullanmaya devam edebilirsiniz.
> 
> Azure Sentinel, günlük kaydı yapan ve algılamalar, araştırmalar, araştırma ve makine öğrenimi odaklı Öngörüler dahil ek SıEM işlevselliği sağlayan bir bulut Yerel güvenlik bilgileri ve olay yönetimi çözümüdür. Azure Sentinel 'in kullanılması artık Office 365 SharePoint etkinliğinin ve Exchange Yönetim günlüklerinin kullanımını sağlar.
> 
> Azure AD raporlama, ortamınızda oturum açma olayları, denetim olayları ve dizininizde değişiklik de dahil olmak üzere Azure AD etkinliğinin günlüklerine yönelik daha kapsamlı bir görünümünü sağlar. Azure AD günlüklerini bağlamak için Azure [Sentinel Azure AD bağlayıcısını](../../sentinel/connect-azure-active-directory.md) kullanabilir veya Azure [IZLEYICI ile Azure AD günlükleri tümleştirmesini](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)yapılandırabilirsiniz. 
>
> Azure AD günlüğü koleksiyonu, Azure Izleyici fiyatlandırması tabi.  Daha fazla bilgi için bkz. [Azure izleyici fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/) .
>
> Azure Sentinel Office 365 çözümünü kullanmak için:
> 1. Azure Sentinel 'de Office 365 bağlayıcısını kullanmak çalışma alanınızın fiyatlandırmasını etkiler. Daha fazla bilgi için bkz. [Azure Sentinel fiyatlandırması](https://azure.microsoft.com/pricing/details/azure-sentinel/).
> 2. Azure Izleyici Office 365 çözümünü zaten kullanıyorsanız, önce [aşağıdaki kaldırma bölümünde](#uninstall)yer alarak betiği kullanarak kaldırmanız gerekir.
> 3. Çalışma alanınızda [Azure Sentinel çözümünü etkinleştirin](../../sentinel/quickstart-onboard.md) .
> 4. Azure Sentinel 'de **veri bağlayıcıları** sayfasına gidin ve **Office 365** bağlayıcısını etkinleştirin.
>
> ## <a name="frequently-asked-questions"></a>Sık sorulan sorular
> 
> ### <a name="q-is-it-possible-to-on-board-the-office-365-azure-monitor-solution-between-now-and-july-30th"></a>S: Office 365 Azure Izleyici çözümü Şu anda ve 30 Temmuz arasında yerleşik olarak yapılabilir mi?
> Hayır, Azure Izleyici Office 365 çözüm ekleme betikleri artık kullanılamıyor. Çözüm 30 Temmuz tarihinde kaldırılacak.
> 
> ### <a name="q-will-the-tables-and-schemas-be-changed"></a>S: tablolar ve şemalar değiştirilsin mi?
> **Officeactivity** tablo adı ve şeması, geçerli çözümle aynı kalacaktır. Azure AD verilerine başvuran sorguları hariç tutarak yeni çözümde aynı sorguları kullanmaya devam edebilirsiniz.
> 
> Yeni [Azure AD raporlama ve izleme çözümü](../../active-directory/reports-monitoring/plan-monitoring-and-reporting.md) günlükleri, **Officeactivity**yerine [signınlogs](../../active-directory/reports-monitoring/concept-sign-ins.md) ve [auditlogs](../../active-directory/reports-monitoring/concept-audit-logs.md) tablolarına alınacaktır. Daha fazla bilgi için bkz. Azure Sentinel ve Azure Izleyici kullanıcıları için de uygun olan [Azure AD günlüklerini çözümleme](../../active-directory/reports-monitoring/howto-analyze-activity-logs-log-analytics.md).
> 
> Aşağıda, **Officeetkinlikten** **signınlogs**'a sorguları dönüştürme örnekleri verilmiştir:
> 
> **Sorgu, oturum açma işlemlerini Kullanıcı tarafından başarısız oldu:**
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
> **Azure AD işlemlerini görüntüleme:**
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
> ### <a name="q-how-can-i-on-board-azure-sentinel"></a>S: Azure Sentinel 'i nasıl kullanabilirim?
> Azure Sentinel, yeni veya mevcut Log Analytics çalışma alanında etkinleştirebilmeniz için bir çözümdür. Daha fazla bilgi için bkz. [Azure Sentinel on-taslak belgeleri](../../sentinel/quickstart-onboard.md).
>
> ### <a name="q-do-i-need-azure-sentinel-to-connect-the-azure-ad-logs"></a>S: Azure AD günlüklerini bağlamak için Azure Sentinel 'e ihtiyacım var mı?
> Azure [ad günlüklerini Azure izleyici ile tümleştirerek](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)Azure Sentinel çözümüyle ilgili olmayan bir şekilde yapılandırabilirsiniz. Azure Sentinel, Azure AD günlükleri için yerel bir bağlayıcı ve kullanıma hazır içerik sağlar. Daha fazla bilgi için, kullanıma hazır güvenlik odaklı içerik hakkında aşağıdaki soruya bakın.
>
> ###    <a name="q-what-are-the-differences-when-connecting-azure-ad-logs-from-azure-sentinel-and-azure-monitor"></a>S: Azure AD günlüklerini Azure Sentinel ve Azure Izleyici 'den bağlarken ne fark vardır?
> Azure Sentinel ve Azure Izleyici aynı [Azure AD raporlama ve izleme çözümüne](../../active-directory/reports-monitoring/plan-monitoring-and-reporting.md)bağlı olarak Azure AD günlüklerine bağlanır. Azure Sentinel, aynı verileri bağlayan ve izleme bilgilerini sağlayan tek tıklamayla bir yerel bağlayıcı sağlar.
>
> ###    <a name="q-what-do-i-need-to-change-when-moving-to-the-new-azure-ad-reporting-and-monitoring-tables"></a>S: yeni Azure AD raporlama ve izleme tablolarına geçiş yaparken ne yapmam gerekir?
> Uyarıları, panoları ve Office 365 Azure AD verilerini kullanarak oluşturduğunuz tüm içerikleri içeren Azure AD verilerini kullanan tüm sorgular, yeni tablolar kullanılarak yeniden oluşturulmalıdır.
>
> Azure Sentinel ve Azure AD, Azure AD raporlama ve izleme çözümüne taşırken kullanabileceğiniz yerleşik içerik sağlar. Daha fazla bilgi için, kullanıma hazır güvenlik odaklı içerik ve [Azure izleyici çalışma kitaplarını Azure Active Directory raporları Için nasıl kullanacağınızı](../../active-directory/reports-monitoring/howto-use-azure-monitor-workbooks.md)gösteren bir sonraki soruya bakın. 
>
> ### <a name="q-how-i-can-use-the-azure-sentinel-out-of-the-box-security-oriented-content"></a>S: Azure Sentinel kullanıma hazır güvenlik yönelimli içeriği nasıl kullanabilirim?
> Azure Sentinel, yerleşik güvenliğe dayalı panolar, özel uyarı sorguları, arama sorguları, araştırma ve Office 365 ve Azure AD günlükleri temelinde otomatik yanıt özellikleri sağlar. Daha fazla bilgi edinmek için Azure Sentinel GitHub ve öğreticileri bulun:
>
> - [Kullanıma hazır tehditleri algılama](../../sentinel/tutorial-detect-threats-built-in.md)
> - [Şüpheli tehditleri algılamak için özel analiz kuralları oluşturma](../../sentinel/tutorial-detect-threats-custom.md)
> - [Verilerinizi izleme](../../sentinel/tutorial-monitor-your-data.md)
> - [Azure Sentinel ile olayları araştırın](../../sentinel/tutorial-investigate-cases.md)
> - [Azure Sentinel 'de otomatik tehdit yanıtlarını ayarlama](../../sentinel/tutorial-respond-threats-playbook.md)
> - [Azure Sentinel GitHub topluluğu](https://github.com/Azure/Azure-Sentinel/tree/master/Playbooks)
> 
> ### <a name="q-does-azure-sentinel-provide-additional-connectors-as-part-of-the-solution"></a>S: Azure Sentinel, çözümün bir parçası olarak ek bağlayıcılar sağlıyor mu?
> Evet, bkz. [Azure Sentinel Connect veri kaynakları](../../sentinel/connect-data-sources.md).
> 
> ###    <a name="q-what-will-happen-on-july-30-do-i-need-to-offboard-beforehand"></a>S: 30 Temmuz 'da ne olur? Önceden Pano yapmam gerekir mi?
> 
> - **Office365** çözümünden veri alamazsınız. Bu çözüm Market 'te artık kullanılamayacak
> - Azure Sentinel müşterileri için, Azure Sentinel **Securityınsights** çözümüne Log Analytics çalışma alanı çözümü **Office365** eklenecektir.
> - Çözümünüzü el ile boşaltmıyorsanız, verilerinizin 30 Temmuz tarihinde otomatik olarak bağlantısı kesilir.
> 
> ### <a name="q-will-my-data-transfer-to-the-new-solution"></a>S: veri, yeni çözüme aktarılsın mı?
> Evet. **Office 365** çözümünü çalışma alanınızdan kaldırdığınızda, şema kaldırıldığı için verileri geçici olarak kullanılamıyor olacaktır. Yeni **Office 365** bağlayıcısını Sentinel 'de etkinleştirdiğinizde, şema çalışma alanına geri yüklenir ve önceden toplanan tüm veriler kullanılabilir hale gelir. 
 

Office 365 yönetimi çözümü, Azure Izleyici 'de Office 365 ortamınızı izlemenize olanak sağlar.

- Kullanım düzenlerini çözümlemek ve davranış eğilimlerini belirlemek için Office 365 hesaplarınızdaki Kullanıcı etkinliklerini izleyin. Örneğin, kuruluşunuz dışında paylaşılan dosyalar veya en popüler SharePoint siteleri gibi belirli kullanım senaryolarını ayıklayabilirsiniz.
- Yapılandırma değişikliklerini veya yüksek ayrıcalıklı işlemleri izlemek için yönetici etkinliklerini izleyin.
- Kurumsal gereksinimleriniz için özelleştirilebilen, istenmeyen kullanıcı davranışını algılayıp araştırın.
- Denetim ve uyumluluğu gösterir. Örneğin, gizli dosyalardaki dosya erişim işlemlerini, denetim ve uyumluluk sürecinde size yardımcı olabilecek şekilde izleyebilirsiniz.
- Kuruluşunuzun Office 365 etkinlik verilerinin en üstünde bulunan [günlük sorgularını](../log-query/log-query-overview.md) kullanarak işlem sorunlarını giderme işlemi gerçekleştirin.


## <a name="uninstall"></a>Kaldır

[Yönetim çözümünü kaldırma](solutions.md#remove-a-monitoring-solution)' daki Işlemi kullanarak Office 365 Yönetim çözümünü kaldırabilirsiniz. Bu, Office 365 ' den toplanan verileri de Azure Izleyici 'ye durdurmayacak. Office 365 aboneliğinizi kaldırmak ve veri toplamayı durdurmak için aşağıdaki yordamı izleyin.

1. Aşağıdaki betiği *office365_unsubscribe. ps1*olarak kaydedin.

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

2. Betiği aşağıdaki komutla çalıştırın:

    ```
    .\office365_unsubscribe.ps1 -WorkspaceName <Log Analytics workspace name> -ResourceGroupName <Resource Group name> -SubscriptionId <Subscription ID> -OfficeTennantID <Tenant ID> 
    ```

    Örnek:

    ```powershell
    .\office365_unsubscribe.ps1 -WorkspaceName MyWorkspace -ResourceGroupName MyResourceGroup -SubscriptionId '60b79d74-f4e4-4867-b631-yyyyyyyyyyyy' -OfficeTennantID 'ce4464f8-a172-4dcf-b675-xxxxxxxxxxxx'
    ```

Sizden kimlik bilgileri istenir. Log Analytics çalışma alanınızın kimlik bilgilerini sağlayın.

## <a name="data-collection"></a>Veri toplama

Verilerin başlangıçta toplanması birkaç saat sürebilir. Toplamaya başladıktan sonra, Office 365 bir kayıt oluşturulduğunda ayrıntılı verilerle Azure Izleyici 'ye bir [Web kancası bildirimi](https://msdn.microsoft.com/office-365/office-365-management-activity-api-reference#receiving-notifications) gönderir. Bu kayıt, Azure Izleyici 'de alındıktan sonra birkaç dakika içinde kullanılabilir.

## <a name="using-the-solution"></a>Çözümü kullanma

[!INCLUDE [azure-monitor-solutions-overview-page](../../../includes/azure-monitor-solutions-overview-page.md)]

Office 365 çözümünü Log Analytics çalışma alanınıza eklediğinizde panonuza **office 365** kutucuğu eklenecektir. Bu kutucukta, ortamınızdaki bilgisayarların sayısına ve güncelleştirme uyumluluğuna ilişkin bir sayı ve grafik gösterimi görüntülenir.<br><br>
![Office 365 Özet kutucuğu](media/solution-office-365/tile.png)  

Office **365** panosunu açmak için **Office 365** kutucuğuna tıklayın.

![Office 365 panosu](media/solution-office-365/dashboard.png)  

Pano aşağıdaki tabloda gösterilen sütunları içerir. Her sütunda, belirtilen kapsam ve zaman aralığı için bu sütun ölçütlerine uyan en üstteki on uyarı, sayıma göre listelenir. Listenin en altında bulunan tümünü görüntüle ' ye tıklayarak veya sütun başlığına tıklayarak, tüm listeyi sağlayan bir günlük araması çalıştırabilirsiniz.

| Sütun | Açıklama |
|:--|:--|
| İşlemler | Tüm izlenen Office 365 aboneliklerinizden etkin kullanıcılar hakkında bilgiler sağlar. Ayrıca, zaman içinde gerçekleşen etkinlik sayısını da görebileceksiniz.
| Exchange | Posta kutusu ekleme Izni veya Set-Mailbox gibi Exchange Server etkinliklerinin dökümünü gösterir. |
| SharePoint | Kullanıcıların SharePoint belgelerinde gerçekleştirdiği en iyi etkinlikleri gösterir. Bu kutucuktan ayrıntıya indığınızda arama sayfasında, bu etkinliklerin hedef belge ve bu etkinliğin konumu gibi ayrıntıları gösterilir. Örneğin, dosya erişimli bir olay için, erişilen belgeye, ilişkili hesap adına ve IP adresine bakabilirsiniz. |
| Azure Active Directory | Kullanıcı parolası ve oturum açma girişimlerini sıfırlama gibi ilk kullanıcı etkinliklerini içerir. Ayrıntıya gitmediğiniz zaman, bu etkinliklerin sonuç durumu gibi ayrıntılarını görebileceksiniz. Bu, genellikle Azure Active Directory şüpheli etkinlikleri izlemek istediğinizde yararlı olur. |




## <a name="azure-monitor-log-records"></a>Azure Izleyici günlük kayıtları

Office 365 çözümü tarafından Azure Izleyici 'de Log Analytics çalışma alanında oluşturulan tüm kayıtlar bir **tür** **officeetkinliğine**sahiptir.  **Officeworkload** özelliği, kaydın hangi Office 365 hizmetini (Exchange, AzureActiveDirectory, SharePoint veya OneDrive) başvurduğunu belirler.  **RecordType** özelliği işlem türünü belirtir.  Özellikler her işlem türü için farklılık gösterir ve aşağıdaki tablolarda gösterilir.

### <a name="common-properties"></a>Ortak özellikler

Aşağıdaki özellikler tüm Office 365 kayıtları için ortaktır.

| Özellik | Açıklama |
|:--- |:--- |
| Tür | *Officeetkinliği* |
| ClientIP | Etkinlik günlüğe kaydedildiğinde kullanılan cihazın IP adresi. IP adresi IPv4 veya IPv6 adresi biçiminde görüntülenir. |
| Officeiş yükü | Kaydın başvurduğu Office 365 hizmeti.<br><br>AzureActiveDirectory<br>Exchange<br>SharePoint|
| İşlem | Kullanıcı veya yönetici etkinliğinin adı.  |
| OrganizationId | Kuruluşunuzun Office 365 kiracısı için GUID. Bu değer, gerçekleştiği Office 365 hizmetine bakılmaksızın kuruluşunuzun her zaman aynı olacaktır. |
| RecordType | Gerçekleştirilen işlem türü. |
| ResultStatus | Eylemin (Operation özelliğinde belirtilen) başarılı olup olmadığını belirtir. Olası değerler başarılı, PartiallySucceeded veya başarısız. Exchange yönetici etkinliği için değer true ya da false şeklindedir. |
| UserId | Günlüğe kaydedilen kayda neden olan eylemi gerçekleştiren kullanıcının UPN (Kullanıcı asıl adı); Örneğin, my_name@my_domain_name. Sistem hesapları tarafından gerçekleştirilen etkinlik kayıtlarının (örneğin, SHAREPOINT\system veya NTAUTHORITY\SYSTEM ADLı) da dahil edildiğini unutmayın. | 
| UserKey | UserID özelliğinde tanımlanan Kullanıcı için alternatif bir KIMLIK.  Örneğin, bu özellik SharePoint, OneDrive Iş ve Exchange kullanıcıları tarafından gerçekleştirilen olaylar için Passport benzersiz KIMLIĞI (PUıD) ile doldurulur. Bu özellik aynı zamanda diğer hizmetlerde gerçekleşen olaylar ve sistem hesapları tarafından gerçekleştirilen olaylar için UserID özelliği ile aynı değeri belirtebilir|
| UserType | İşlemi gerçekleştiren kullanıcının türü.<br><br>Yönetici<br>Uygulama<br>DcAdmin<br>Normal<br>Ayrıldı<br>ServicePrincipal<br>Sistem |


### <a name="azure-active-directory-base"></a>Azure Active Directory taban

Aşağıdaki özellikler tüm Azure Active Directory kayıtları için ortaktır.

| Özellik | Açıklama |
|:--- |:--- |
| Officeiş yükü | AzureActiveDirectory |
| RecordType     | AzureActiveDirectory |
| AzureActiveDirectory_EventType | Azure AD olayının türü. |
| ExtendedProperties | Azure AD olayının genişletilmiş özellikleri. |


### <a name="azure-active-directory-account-logon"></a>Hesap oturum Azure Active Directory

Bu kayıtlar Active Directory bir Kullanıcı oturum açmayı denediğinde oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| `OfficeWorkload` | AzureActiveDirectory |
| `RecordType`     | AzureActiveDirectoryAccountLogon |
| `Application` | Office 15 gibi hesap oturum açma olayını tetikleyen uygulama. |
| `Client` | Hesap oturum açma olayı için kullanılan istemci aygıtı, cihaz işletim sistemi ve cihaz tarayıcısı hakkında ayrıntılar. |
| `LoginStatus` | Bu özellik doğrudan OrgIdLogon. LoginStatus öğesinden. Çeşitli ilginç oturum açma hatalarının eşleştirmesi uyarı algoritmaları tarafından gerçekleştirilebilir. |
| `UserDomain` | Kiracı kimlik bilgileri (TIı). | 


### <a name="azure-active-directory"></a>Azure Active Directory

Azure Active Directory nesnelerinde değişiklik veya eklemeler yapıldığında bu kayıtlar oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| Officeiş yükü | AzureActiveDirectory |
| RecordType     | AzureActiveDirectory |
| AADTarget | Eylemin (Operation özelliği tarafından tanımlanan) üzerinde gerçekleştirildiği Kullanıcı. |
| Aktör | Eylemi gerçekleştiren kullanıcı veya hizmet sorumlusu. |
| Actorcontextıd | Aktörün ait olduğu kuruluşun GUID 'SI. |
| Actorpaddress | Aktör, ıPV4 veya ıPV6 adresi biçimindeki IP adresidir. |
| Intersystemsıd | Office 365 hizmeti içindeki bileşenler arasında eylemleri izleyen GUID. |
| Insystemıd |     Eylemi izlemek için Azure Active Directory tarafından oluşturulan GUID. |
| Supportticketıd | "Adına göre hareket etme" durumlarında eyleme ilişkin müşteri destek bileti KIMLIĞI. |
| Targetcontextıd | Hedeflenen kullanıcının ait olduğu kuruluşun GUID 'SI. |


### <a name="data-center-security"></a>Veri merkezi güvenliği

Bu kayıtlar, veri merkezi güvenlik denetim verilerinden oluşturulur.  

| Özellik | Açıklama |
|:--- |:--- |
| Efekt organizasyonu | Yükseltme/cmdlet 'in hedeflediği kiracının adı. |
| Yükseltme zamanı | Yükseltme onaylandığı zaman damgası. |
| Onaylayan yükseltme | Bir Microsoft yöneticisinin adı. |
| Yükseltme süresi | Yükseltme etkin olduğu süre. |
| Yükseltme RequestId |     Yükseltme isteği için benzersiz bir tanımlayıcı. |
| Yükseltme rolü | Yükseltme için istenen rol. |
| Yükseltme zamanı | Yükseltme başlangıç saati. |
| Start_Time | Cmdlet yürütmenin başlangıç saati. |


### <a name="exchange-admin"></a>Exchange Yöneticisi

Bu kayıtlar, Exchange yapılandırmasında değişiklik yapıldığında oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| Officeiş yükü | Exchange |
| RecordType     | ExchangeAdmin |
| ExternalAccess |     Cmdlet 'inin kuruluşunuzdaki bir kullanıcı tarafından, Microsoft veri merkezi personeli veya bir veri merkezi hizmet hesabı tarafından mı yoksa yetkilendirilmiş bir yönetici tarafından mı çalıştırılacağını belirtir. False değeri, cmdlet 'inin kuruluşunuzdaki bir kişi tarafından çalıştırıldığını gösterir. True değeri, cmdlet 'in veri merkezi personeli, bir veri merkezi hizmet hesabı veya yönetici temsilcisi tarafından çalıştırıldığını belirtir. |
| ModifiedObjectResolvedName |     Bu, cmdlet tarafından değiştirilen nesnenin Kullanıcı dostu adıdır. Bu, yalnızca cmdlet nesneyi değiştirdiğinde günlüğe kaydedilir. |
| OrganizationName | Kiracının adı. |
| OriginatingServer | Cmdlet 'in yürütüldüğü sunucunun adı. |
| Parametreler | Operations özelliğinde tanımlanan cmdlet ile kullanılan tüm parametrelerin adı ve değeri. |


### <a name="exchange-mailbox"></a>Exchange posta kutusu

Exchange posta kutularına değişiklik veya eklemeler yapıldığında bu kayıtlar oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| Officeiş yükü | Exchange |
| RecordType     | Exchangeıtem |
| Clientınfostring | Bir tarayıcı sürümü, Outlook sürümü ve mobil cihaz bilgileri gibi, işlemi gerçekleştirmek için kullanılan e-posta istemcisiyle ilgili bilgiler. |
| Client_IPAddress | İşlem günlüğe kaydedildiğinde kullanılan cihazın IP adresi. IP adresi IPv4 veya IPv6 adresi biçiminde görüntülenir. |
| ClientMachineName | Outlook istemcisini barındıran makine adı. |
| ClientProcessName | Posta kutusuna erişmek için kullanılan e-posta istemcisi. |
| ClientVersion | E-posta istemcisinin sürümü. |
| Internallogontype | Dahili kullanım için ayrılmıştır. |
| Logon_Type | Posta kutusuna erişen ve günlüğe kaydedilen işlemi gerçekleştiren kullanıcı türünü gösterir. |
| LogonUserDisplayName |     İşlemi gerçekleştiren kullanıcının kolay adı. |
| LogonUserSid | İşlemi gerçekleştiren kullanıcının SID 'SI. |
| MailboxGuid | Erişilen posta kutusunun Exchange GUID 'ı. |
| MailboxOwnerMasterAccountSid | Posta kutusu sahibi hesabının ana hesap SID 'SI. |
| MailboxOwnerSid | Posta kutusu sahibinin SID 'SI. |
| MailboxOwnerUPN | Erişilen posta kutusunun sahibi olan kişinin e-posta adresi. |


### <a name="exchange-mailbox-audit"></a>Exchange posta kutusu denetimi

Bu kayıtlar, bir posta kutusu denetim girişi oluşturulduğunda oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| Officeiş yükü | Exchange |
| RecordType     | Exchangeıtem |
| Öğe | İşlemin gerçekleştirildiği öğeyi temsil eder | 
| SendAsUserMailboxGuid 'Si | E-posta göndermek için erişilen posta kutusunun Exchange GUID 'ı. |
| SendAsUserSmtp | Kimliğine bürünülen kullanıcının SMTP adresi. |
| Sendonbenoktalı Ofusermailboxguid | Adına posta gönderilirken erişilen posta kutusunun Exchange GUID 'ı. |
| Sendonbenoktalı Ofusersmtp | Adına e-posta gönderilen kullanıcının SMTP adresi. |


### <a name="exchange-mailbox-audit-group"></a>Exchange posta kutusu denetim grubu

Bu kayıtlar, Exchange gruplarında değişiklik veya eklemeler yapıldığında oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| Officeiş yükü | Exchange |
| Officeiş yükü | Exchangeıtemgroup |
| AffectedItems | Gruptaki her öğe hakkında bilgi. |
| CrossMailboxOperations | İşlemin birden fazla posta kutusu ile ilişkili olup olmadığını gösterir. |
| Destmailboxıd | Yalnızca CrossMailboxOperations parametresi true ise ayarlanır. Hedef posta kutusu GUID 'sini belirtir. |
| DestMailboxOwnerMasterAccountSid | Yalnızca CrossMailboxOperations parametresi true ise ayarlanır. Hedef posta kutusu sahibinin ana hesap SID 'sinin SID 'sini belirtir. |
| DestMailboxOwnerSid | Yalnızca CrossMailboxOperations parametresi true ise ayarlanır. Hedef posta kutusunun SID 'sini belirtir. |
| DestMailboxOwnerUPN | Yalnızca CrossMailboxOperations parametresi true ise ayarlanır. Hedef posta kutusunun sahibinin UPN 'sini belirtir. |
| DestFolder | Taşıma gibi işlemler için hedef klasör. |
| Klasör | Bir öğe grubunun bulunduğu klasör. |
| Klasörler |     Bir işlemde yer alan kaynak klasörleriyle ilgili bilgiler; Örneğin, klasörler seçildiyse ve sonra silinirse. |


### <a name="sharepoint-base"></a>SharePoint temeli

Bu özellikler tüm SharePoint kayıtları için ortaktır.

| Özellik | Açıklama |
|:--- |:--- |
| Officeiş yükü | SharePoint |
| Officeiş yükü | SharePoint |
| EventSource | SharePoint 'te bir olayın oluştuğunu tanımlar. Olası değerler SharePoint veya ObjectModel. |
| ItemType | Erişilen veya değiştirilen nesnenin türü. Nesne türleriyle ilgili ayrıntılar için bkz. ItemType tablosu. |
| Machinedomainınfo | Cihaz eşitleme işlemleri hakkında bilgi. Bu bilgiler yalnızca istekte mevcutsa bildirilir. |
| MachineID |     Cihaz eşitleme işlemleri hakkında bilgi. Bu bilgiler yalnızca istekte mevcutsa bildirilir. |
| Site_ | Kullanıcının eriştiği dosya veya klasörün bulunduğu sitenin GUID 'SI. |
| Source_Name | Denetlenen işlemi tetikleyen varlık. Olası değerler SharePoint veya ObjectModel. |
| Kullanıcı | Kullanıcının istemcisi veya tarayıcısı hakkında bilgi. Bu bilgiler istemci veya tarayıcı tarafından sağlanır. |


### <a name="sharepoint-schema"></a>SharePoint şeması

Bu kayıtlar, SharePoint 'te yapılandırma değişiklikleri yapıldığında oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| Officeiş yükü | SharePoint |
| Officeiş yükü | SharePoint |
| CustomEvent | Özel olaylar için isteğe bağlı dize. |
| Event_Data |     Özel olaylar için isteğe bağlı yük. |
| ModifiedProperties | Özelliği, bir kullanıcının bir site veya site koleksiyonu yönetici grubuna üye olarak eklenmesi gibi yönetim olaylarına dahildir. Özelliği, değiştirilen özelliğin adını (örneğin, site yönetici grubu), değiştirilen özelliğin yeni değerini (site yöneticisi olarak eklenen Kullanıcı gibi) ve değiştirilen nesnenin önceki değerini içerir. |


### <a name="sharepoint-file-operations"></a>SharePoint dosya Işlemleri

Bu kayıtlar, SharePoint 'teki dosya işlemlerine yanıt olarak oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| Officeiş yükü | SharePoint |
| Officeiş yükü | SharePointFileOperation |
| Hedef dosya uzantısı | Kopyalanmış veya taşınmış bir dosyanın dosya uzantısı. Bu özellik yalnızca dosya kopyalanmış ve dosya taşınan olayları için görüntülenir. |
| Hedef dosya adı | Kopyalanmış veya taşınan dosyanın adı. Bu özellik yalnızca dosya kopyalanmış ve dosya taşınan olayları için görüntülenir. |
| DestinationRelativeUrl 'Si | Bir dosyanın kopyalandığı veya taşındığı hedef klasörün URL 'SI. SiteURL, DestinationRelativeURL ve DestinationFileName parametrelerinin değerlerinin birleşimi, kopyalanmış dosyanın tam yol adı olan ObjectID özelliğinin değeri ile aynıdır. Bu özellik yalnızca dosya kopyalanmış ve dosya taşınan olayları için görüntülenir. |
| SharingType | Kaynağın paylaşıldığı kullanıcıya atanan paylaşım izinlerinin türü. Bu Kullanıcı, UserSharedWith parametresi tarafından tanımlanır. |
| Site_Url | Kullanıcının eriştiği dosya veya klasörün bulunduğu sitenin URL 'SI. |
| SourceFileExtension | Kullanıcı tarafından erişilen dosyanın dosya uzantısı. Erişilen nesne bir klasöriyorsa, bu özellik boştur. |
| SourceFileName |     Kullanıcı tarafından erişilen dosyanın veya klasörün adı. |
| SourceRelativeUrl 'Si | Kullanıcı tarafından erişilen dosyayı içeren klasörün URL 'SI. SiteURL, SourceRelativeURL ve SourceFileName parametrelerinin değerlerinin birleşimi, Kullanıcı tarafından erişilen dosyanın tam yol adı olan ObjectID özelliğinin değeri ile aynıdır. |
| UserSharedWith |     Bir kaynağın paylaşıldığı Kullanıcı. |




## <a name="sample-log-queries"></a>Örnek günlük sorguları

Aşağıdaki tabloda, bu çözüm tarafından toplanan güncelleştirme kayıtlarına yönelik örnek günlük sorguları verilmiştir.

| Sorgu | Açıklama |
| --- | --- |
|Office 365 aboneliğinizdeki tüm işlemlerin sayısı |OfficeActivity &#124; Işleme göre Count () özetleme |
|SharePoint sitelerinin kullanımı|OfficeActivity &#124; burada OfficeWorkload = ~ "SharePoint" &#124; Count () \| değerini|
|Kullanıcı türüne göre dosya erişim işlemleri | OfficeActivity &#124; Kullanıcı türüne göre Count () özetleme |
|Exchange 'de dış eylemleri izleme|OfficeActivity &#124; burada OfficeWorkload = ~ "Exchange" ve ExternalAccess = = true|



## <a name="next-steps"></a>Sonraki adımlar

* Ayrıntılı güncelleştirme verilerini görüntülemek için [Azure izleyici 'de günlük sorguları '](../log-query/log-query-overview.md) nı kullanın.
* En sevdiğiniz Office 365 arama sorgularını göstermek için [kendi panolarınızı oluşturun](../learn/tutorial-logs-dashboards.md) .
* Önemli Office 365 etkinliklerinden haberdar olmak için [uyarılar oluşturun](../platform/alerts-overview.md) .  
