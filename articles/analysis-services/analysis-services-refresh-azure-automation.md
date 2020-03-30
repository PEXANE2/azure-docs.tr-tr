---
title: Azure Otomasyonu ile Azure Analiz Hizmetleri modellerini yenileyin | Microsoft Dokümanlar
description: Bu makalede, Azure Çözümleme Hizmetleri için Azure Otomasyonu'nun nasıl kodlanması açıklanmaktadır.
author: chrislound
ms.service: analysis-services
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: chlound
ms.openlocfilehash: a79123d57f80474e1871ef68f9a92ea9417089ac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73572363"
---
# <a name="refresh-with-azure-automation"></a>Azure Otomasyonu ile yenileme

Azure Otomasyonu ve PowerShell Runbook'ları kullanarak Azure Analizi tabular modellerinizde otomatik veri yenileme işlemleri gerçekleştirebilirsiniz.  

Bu makaledeki [örnekpowershell SqlServer modüllerini](https://docs.microsoft.com/powershell/module/sqlserver/?view=sqlserver-ps)kullanır.

Bir modeli yenilemeyi gösteren örnek bir PowerShell Runbook bu makalede daha sonra sağlanır.  

## <a name="authentication"></a>Kimlik doğrulaması

Tüm aramaların geçerli bir Azure Etkin Dizini (OAuth 2) belirteciyle kimlik doğrulaması yapılmalıdır.  Bu makaledeki örnek, Azure Çözümleme Hizmetleri'nin kimliğini doğrulamak için bir Hizmet Sorumlusu (SPN) kullanır.

Hizmet Sorumlusu oluşturma hakkında daha fazla bilgi edinmek için Azure [portalını kullanarak bir hizmet sorumlusu oluşturma](../active-directory/develop/howto-create-service-principal-portal.md)'na bakın.

## <a name="prerequisites"></a>Ön koşullar

> [!IMPORTANT]
> Aşağıdaki örnek, Azure Çözümleme Hizmetleri güvenlik duvarının devre dışı bırakıldığını varsayar. Güvenlik duvarı etkinse, istek başlatıcısının genel IP adresinin güvenlik duvarında beyaz listeye alınması gerekir.

### <a name="install-sqlserver-modules-from-powershell-gallery"></a>PowerShell galerisinden SqlServer modüllerini yükleyin.

1. Azure Otomasyon Hesabınızda **Modülleri**tıklatın ve **ardından galeriye göz atın.**

2. Arama **çubuğunda, SqlServer'ı**arayın.

    ![Arama Modülleri](./media/analysis-services-refresh-azure-automation/1.png)

3. SqlServer'ı seçin ve sonra **Aktar'ı**tıklatın.
 
    ![İthalat Modülü](./media/analysis-services-refresh-azure-automation/2.png)

4. **Tamam**'a tıklayın.
 
### <a name="create-a-service-principal-spn"></a>Hizmet Sorumlusu Oluşturma (SPN)

Hizmet Sorumlusu oluşturma hakkında bilgi edinmek için Azure [portalını kullanarak bir hizmet sorumlusu oluşturma](../active-directory/develop/howto-create-service-principal-portal.md)'na bakın.

### <a name="configure-permissions-in-azure-analysis-services"></a>Azure Çözümleme Hizmetlerinde izinleri yapılandırma
 
Oluşturduğunuz Hizmet Yöneticisi'nin sunucuda sunucu yöneticisi izinleri olmalıdır. Daha fazla bilgi için bkz. [sunucu yöneticisi rolüne bir hizmet sorumlusu ekle.](analysis-services-addservprinc-admins.md)

## <a name="design-the-azure-automation-runbook"></a>Azure Otomasyon Runbook'u tasarla

1. Otomasyon Hesabında, Hizmet Anapara'sını güvenli bir şekilde depolamak için kullanılacak bir **Kimlik Bilgileri** kaynağı oluşturun.

    ![Kimlik bilgisi oluşturma](./media/analysis-services-refresh-azure-automation/6.png)

2. Kimlik bilgisi için ayrıntıları girin.  Kullanıcı **adı**için, **SPN ClientId**girin , **Şifre**için , **SPN Secret**girin.

    ![Kimlik bilgisi oluşturma](./media/analysis-services-refresh-azure-automation/7.png)

3. Otomasyon Runbook'u Alma

    ![Runbook'u Içe Aktar](./media/analysis-services-refresh-azure-automation/8.png)

4. **Refresh-Model.ps1** dosyasına göz atın, bir **Ad** ve **açıklama**sağlayın ve ardından **Oluştur'u**tıklatın.

    ![Runbook'u Içe Aktar](./media/analysis-services-refresh-azure-automation/9.png)

5. Runbook oluşturulduğunda, otomatik olarak edit moduna geçer.  **Yayımla**’yı seçin.

    ![Runbook'u Yayımla](./media/analysis-services-refresh-azure-automation/10.png)

    > [!NOTE]
    > Daha önce oluşturulan kimlik bilgisi **kaynağı, Get-AutomationPSCredential** komutu kullanılarak runbook tarafından alınır.  Bu komut, azure çözümleme hizmetlerine kimlik doğrulamaişlemi gerçekleştirmek için **Invoke-ProcessASADatabase** PowerShell komutuna geçirilir.

6. Başlat'ı tıklatarak runbook'u test **edin.**

    ![Runbook'u başlat](./media/analysis-services-refresh-azure-automation/11.png)

7. **DATABASENAME,** **ANALYSISSERVER**ve **REFRESHTYPE** parametrelerini doldurun ve **Tamam'ı**tıklatın. Runbook el ile çalıştırıldığında **WEBHOOKDATA** parametresi gerekli değildir.

    ![Runbook'u başlat](./media/analysis-services-refresh-azure-automation/12.png)

Runbook başarıyla yürütülürse, aşağıdaki gibi bir çıktı alırsınız:

![Başarılı Çalıştırma](./media/analysis-services-refresh-azure-automation/13.png)

## <a name="use-a-self-contained-azure-automation-runbook"></a>Bağımsız Azure Otomasyon Runbook'u kullanma

Runbook, Azure Analiz Hizmetleri modelinin zamanlanmış olarak yenilenmesini tetiklemek üzere yapılandırılabilir.

Bu aşağıdaki gibi yapılandırılabilir:

1. Otomasyon Runbook'unda **Zamanlamaları**tıklatın ve ardından **Zamanlama Ekleyin.**
 
    ![Zamanlama oluşturma](./media/analysis-services-refresh-azure-automation/14.png)

2. **Zamanlama'yı** > tıklatın**Yeni bir zamanlama oluşturun**ve ardından ayrıntıları doldurun.

    ![Zamanlamayı yapılandırma](./media/analysis-services-refresh-azure-automation/15.png)

3. **Oluştur'u**tıklatın.

4. Zamanlama nın parametrelerini doldurun. Bunlar, Runbook her tetikleninher tetikleninde kullanılır. **WEBHOOKDATA** parametresi bir zamanlama ile çalışırken boş bırakılmalıdır.

    ![Parametreleri yapılandırma](./media/analysis-services-refresh-azure-automation/16.png)

5. **Tamam**'a tıklayın.

## <a name="consume-with-data-factory"></a>Veri Fabrikası ile Tüketin

Azure Veri Fabrikası'nı kullanarak runbook'u kullanmak için önce runbook için bir **Webhook** oluşturun. **Webhook,** Azure Veri Fabrikası web etkinliği aracılığıyla çağrılabilen bir URL sağlar.

> [!IMPORTANT]
> **Bir Webhook**oluşturmak için Runbook'un durumunun **yayımlanmış**olması gerekir.

1. Otomasyon Runbook'unuzda **Webhooks'u**tıklatın ve ardından **Webhook Ekle'yi**tıklatın.

   ![Webhook Ekle](./media/analysis-services-refresh-azure-automation/17.png)

2. Webhook'a bir ad ve bir son kullanma süresi verin.  Ad yalnızca Otomasyon Runbook içinde Webhook tanımlar, url bir parçası nı oluşturmaz.

   >[!CAUTION]
   >Sihirbazı kapatmadan önce URL'yi kopyaladığınızdan emin olun, çünkü bir kez kapatıldıktan sonra geri alamıyorsunuz.
    
   ![Webhook'u yapılandırma](./media/analysis-services-refresh-azure-automation/18.png)

    Webhook için parametreler boş kalabilir.  Azure Veri Fabrikası web etkinliğini yapılandırırken, parametreler web aramasının gövdesine aktarılabilir.

3. Veri Fabrikası'nda, bir **web etkinliğini** yapılandırma

### <a name="example"></a>Örnek

   ![Örnek Web Etkinliği](./media/analysis-services-refresh-azure-automation/19.png)

**URL,** Webhook'tan oluşturulan URL'dir.

**Gövde,** aşağıdaki özellikleri içermesi gereken bir JSON belgesidir:


|Özellik  |Değer  |
|---------|---------|
|**AnalysisServicesDatabase**     |Azure Analiz Hizmetleri veritabanının adı <br/> Örnek: AdventureWorksDB         |
|**AnalysisServicesServer**     |Azure Çözümleme Hizmetleri sunucu adı. <br/> Örnek: https:\//westus.asazure.windows.net/servers/myserver/models/AdventureWorks/         |
|**VeritabanıYenileme Türü**     |Gerçekleştirecek yenileme türü. <br/> Örnek: Tam         |

Örnek JSON gövdesi:

```json
{
    "AnalysisServicesDatabaseName": "AdventureWorksDB",
    "AnalysisServicesServer": "asazure://westeurope.asazure.windows.net/MyAnalysisServer",
    "DatabaseRefreshType": "Full"
}
```

Bu parametreler powershell komut dosyasında tanımlanır.  Web etkinliği yürütüldüğünde, geçirilen JSON yükü WEBHOOKDATA'dır.

Bu deserialized ve PowerShell parametreleri olarak depolanır, daha sonra Invoke-ProcesASDatabase PowerShell komutu tarafından kullanılır.

![Deserialized Webhook](./media/analysis-services-refresh-azure-automation/20.png)

## <a name="use-a-hybrid-worker-with-azure-analysis-services"></a>Azure Çözümleme Hizmetleri ile Karma Çalışan Kullanma

Statik genel IP adresine sahip bir Azure Sanal Makinesi, Azure Otomasyon Karma İşçisi olarak kullanılabilir.  Bu genel IP adresi daha sonra Azure Çözümleme Hizmetleri güvenlik duvarına eklenebilir.

> [!IMPORTANT]
> Sanal Makine genel IP adresinin statik olarak yapılandırıldığından emin olun.
>
>Azure OtomasyonKarma Çalışanları yapılandırma hakkında daha fazla bilgi edinmek [için, Karma Runbook Çalışanı'nı kullanarak veri merkezinizdeki veya bulutunuzdaki kaynakları Otomatikleştir'e](../automation/automation-hybrid-runbook-worker.md#install-a-hybrid-runbook-worker)bakın.

Bir Karma İşçi yapılandırıldıktan sonra, Veri Fabrikası [ile Tüket](#consume-with-data-factory)bölümünde açıklandığı gibi bir Webhook oluşturun.  Buradaki tek fark, Webhook'u yapılandırırken > **Karma** **Çalışan'da Çalıştır**seçeneğini seçmektir.

Hybrid Worker kullanarak örnek webhook:

![Örnek Hibrit İşçi Webhook](./media/analysis-services-refresh-azure-automation/21.png)

## <a name="sample-powershell-runbook"></a>Örnek PowerShell Runbook

Aşağıdaki kod parçacığı, PowerShell Runbook kullanarak Azure Analiz Hizmetleri modeli yenilemesinin nasıl gerçekleştirilebildiğini gösterir.

```powershell
param
(
    [Parameter (Mandatory = $false)]
    [object] $WebhookData,

    [Parameter (Mandatory = $false)]
    [String] $DatabaseName,
    [Parameter (Mandatory = $false)]
    [String] $AnalysisServer,
    [Parameter (Mandatory = $false)]
    [String] $RefreshType
)

$_Credential = Get-AutomationPSCredential -Name "ServicePrincipal"

# If runbook was called from Webhook, WebhookData will not be null.
if ($WebhookData)
{ 
    # Retrieve AAS details from Webhook request body
    $atmParameters = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)
    Write-Output "CredentialName: $($atmParameters.CredentialName)"
    Write-Output "AnalysisServicesDatabaseName: $($atmParameters.AnalysisServicesDatabaseName)"
    Write-Output "AnalysisServicesServer: $($atmParameters.AnalysisServicesServer)"
    Write-Output "DatabaseRefreshType: $($atmParameters.DatabaseRefreshType)"
    
    $_databaseName = $atmParameters.AnalysisServicesDatabaseName
    $_analysisServer = $atmParameters.AnalysisServicesServer
    $_refreshType = $atmParameters.DatabaseRefreshType
 
    Invoke-ProcessASDatabase -DatabaseName $_databaseName -RefreshType $_refreshType -Server $_analysisServer -ServicePrincipal -Credential $_credential
}
else 
{
    Invoke-ProcessASDatabase -DatabaseName $DatabaseName -RefreshType $RefreshType -Server $AnalysisServer -ServicePrincipal -Credential $_Credential
}
```


## <a name="next-steps"></a>Sonraki adımlar

[Örnekler](analysis-services-samples.md)  
[REST API](https://docs.microsoft.com/rest/api/analysisservices/servers)
