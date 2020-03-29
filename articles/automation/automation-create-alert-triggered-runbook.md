---
title: Azure Otomasyonu runbook'u tetiklemek için uyarı kullanma
description: Azure uyarısı yükseltildiğinde çalışacak bir runbook'u nasıl tetiklediğinizi öğrenin.
services: automation
ms.subservice: process-automation
ms.date: 04/29/2019
ms.topic: conceptual
ms.openlocfilehash: df28116c588ed77f02c78a42a85feb91ca339e7b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75366709"
---
# <a name="use-an-alert-to-trigger-an-azure-automation-runbook"></a>Azure Otomasyonu runbook'u tetiklemek için uyarı kullanma

Azure'daki çoğu hizmetiçin taban düzeyinde ölçümleri ve günlükleri izlemek için [Azure Monitor'u](../azure-monitor/overview.md?toc=%2fazure%2fautomation%2ftoc.json) kullanabilirsiniz. [Eylem gruplarını](../azure-monitor/platform/action-groups.md?toc=%2fazure%2fautomation%2ftoc.json) kullanarak veya uyarıları temel alarak görevleri otomatikleştirmek için klasik uyarıları kullanarak Azure Otomasyon runbook'larını arayabilirsiniz. Bu makalede, uyarıları kullanarak bir runbook yapılandırma ve çalıştırmak nasıl gösterir.

## <a name="alert-types"></a>Uyarı türleri

Üç uyarı türüne sahip otomasyon runbook'larını kullanabilirsiniz:

* Sık karşılaşılan uyarılar
* Etkinlik günlüğü uyarıları
* Gerçek zamanlı metrik uyarıların yakınında

> [!NOTE]
> Ortak uyarı şeması, bugün Azure'da uyarı bildirimleri için tüketim deneyimini standartlaştırır. Tarihsel olarak, bugün Azure'daki üç uyarı türü (metrik, günlük ve etkinlik günlüğü) kendi e-posta şablonları, webhook şemaları vb. vardır. Daha fazla bilgi için Ortak [uyarı şemasına](../azure-monitor/platform/alerts-common-schema.md) bakın

Bir uyarı bir runbook'u aradığında, asıl arama webhook'a bir HTTP POST isteğidir. POST isteğinin gövdesi, uyarıyla ilgili yararlı özelliklere sahip JSON biçimli bir nesne içerir. Aşağıdaki tabloda, her uyarı türü için taşıma şemasına bağlantılar listelenir:

|Uyarı  |Açıklama|Yük şeması  |
|---------|---------|---------|
|[Sık uyarı](../azure-monitor/platform/alerts-common-schema.md?toc=%2fazure%2fautomation%2ftoc.json)|Bugün Azure'da uyarı bildirimleri için tüketim deneyimini standartlaştıran ortak uyarı şeması.|Ortak uyarı yük şeması|
|[Etkinlik günlüğü uyarısı](../azure-monitor/platform/activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Azure etkinlik günlüğündeki yeni bir olay belirli koşullarla eşleştiğinde bildirim gönderir. Örneğin, benim `Delete VM` **ÜretimKaynak Grubu'nda** bir işlem oluştuğunda veya **Etkin** durumu olan yeni bir Azure Hizmet Durumu olayı görüntülendiğinde.| [Etkinlik günlüğü uyarı yük şeması](../azure-monitor/platform/activity-log-alerts-webhook.md)        |
|[Gerçek zamanlı metrik uyarıya yakın](../azure-monitor/platform/alerts-metric-near-real-time.md?toc=%2fazure%2fautomation%2ftoc.json)    |Bir veya daha fazla platform düzeyinde ölçüm belirtilen koşulları karşıladığında, bir bildirimi metrik uyarılardan daha hızlı gönderir. Örneğin, bir VM'deki **CPU %** değeri **90'dan**büyükse ve **Network In** değeri son 5 dakika için **500 MB'tan** büyükse.| [Gerçek zamanlı metrik uyarı yük şemasına yakın](../azure-monitor/platform/alerts-webhooks.md#payload-schema)          |

Her uyarı türü tarafından sağlanan veriler farklı olduğundan, her uyarı türü farklı şekilde işlenir. Sonraki bölümde, farklı türde uyarıları işlemek için bir runbook oluşturmayı öğrenirsiniz.

## <a name="create-a-runbook-to-handle-alerts"></a>Uyarıları işlemek için bir runbook oluşturma

Uyarıları ile Otomasyon kullanmak için, runbook geçirilen uyarı JSON yük yöneten mantığı olan bir runbook gerekir. Aşağıdaki örnek runbook bir Azure uyarısı çağrılmalıdır.

Önceki bölümde açıklandığı gibi, her uyarı türünün farklı bir şeması vardır. Komut dosyası, `WebhookData` runbook giriş parametresindeki webhook verilerini bir uyarıdan alır. Daha sonra, komut dosyası hangi uyarı türünün kullanıldığını belirlemek için JSON yükünü değerlendirir.

Bu örnekte bir VM'den gelen bir uyarı kullanır. VM verilerini yükten alır ve vm'yi durdurmak için bu bilgileri kullanır. Bağlantı, runbook'un çalıştırıldığı Otomasyon hesabında ayarlanmalıdır. Runbook'ları tetiklemek için uyarıları kullanırken, tetiklenen runbook'taki uyarının durumunu denetlemek önemlidir. Runbook, uyarı nın durumu her değiştirdiğinde tetikler. Uyarılar birden çok durumu vardır, en `Activated` `Resolved`yaygın iki durum ve . Runbook'unuzun birden fazla kez çalışmadığından emin olmak için runbook mantığınızda bu durumu denetleyin. Bu makaledeki örnek, yalnızca `Activated` uyarıların nasıl arayacağını gösterir.

Runbook, VM'ye karşı yönetim eylemini gerçekleştirmek için Azure ile kimlik doğrulaması yapmak için **AzureRunAsConnection** [Run As hesabını](automation-create-runas-account.md) kullanır.

**Stop-AzureVmInResponsetoVMAlert**adlı bir runbook oluşturmak için bu örneği kullanın. PowerShell komut dosyasını değiştirebilir ve birçok farklı kaynakla kullanabilirsiniz.

1. Azure Otomasyon hesabınıza gidin.
2. **Proses Otomasyonu**altında **Runbook'ları**seçin.
3. Runbook listesinin en üstünde + **Runbook oluştur'u**seçin.
4. **Runbook Ekle** sayfasında, runbook adı için **Stop-AzureVmInResponsetoVMAlert'i** girin. Runbook türü için **PowerShell'i**seçin. Ardından **Oluştur'u**seçin.  
5. Aşağıdaki PowerShell örneğini **Edit** sayfasına kopyalayın.

    ```powershell-interactive
    [OutputType("PSAzureOperationResponse")]
    param
    (
        [Parameter (Mandatory=$false)]
        [object] $WebhookData
    )
    $ErrorActionPreference = "stop"

    if ($WebhookData)
    {
        # Get the data object from WebhookData
        $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

        # Get the info needed to identify the VM (depends on the payload schema)
        $schemaId = $WebhookBody.schemaId
        Write-Verbose "schemaId: $schemaId" -Verbose
        if ($schemaId -eq "azureMonitorCommonAlertSchema") {
            # This is the common Metric Alert schema (released March 2019)
            $Essentials = [object] ($WebhookBody.data).essentials
            # Get the first target only as this script doesn't handle multiple
            $alertTargetIdArray = (($Essentials.alertTargetIds)[0]).Split("/")
            $SubId = ($alertTargetIdArray)[2]
            $ResourceGroupName = ($alertTargetIdArray)[4]
            $ResourceType = ($alertTargetIdArray)[6] + "/" + ($alertTargetIdArray)[7]
            $ResourceName = ($alertTargetIdArray)[-1]
            $status = $Essentials.monitorCondition
        }
        elseif ($schemaId -eq "AzureMonitorMetricAlert") {
            # This is the near-real-time Metric Alert schema
            $AlertContext = [object] ($WebhookBody.data).context
            $SubId = $AlertContext.subscriptionId
            $ResourceGroupName = $AlertContext.resourceGroupName
            $ResourceType = $AlertContext.resourceType
            $ResourceName = $AlertContext.resourceName
            $status = ($WebhookBody.data).status
        }
        elseif ($schemaId -eq "Microsoft.Insights/activityLogs") {
            # This is the Activity Log Alert schema
            $AlertContext = [object] (($WebhookBody.data).context).activityLog
            $SubId = $AlertContext.subscriptionId
            $ResourceGroupName = $AlertContext.resourceGroupName
            $ResourceType = $AlertContext.resourceType
            $ResourceName = (($AlertContext.resourceId).Split("/"))[-1]
            $status = ($WebhookBody.data).status
        }
        elseif ($schemaId -eq $null) {
            # This is the original Metric Alert schema
            $AlertContext = [object] $WebhookBody.context
            $SubId = $AlertContext.subscriptionId
            $ResourceGroupName = $AlertContext.resourceGroupName
            $ResourceType = $AlertContext.resourceType
            $ResourceName = $AlertContext.resourceName
            $status = $WebhookBody.status
        }
        else {
            # Schema not supported
            Write-Error "The alert data schema - $schemaId - is not supported."
        }

        Write-Verbose "status: $status" -Verbose
        if (($status -eq "Activated") -or ($status -eq "Fired"))
        {
            Write-Verbose "resourceType: $ResourceType" -Verbose
            Write-Verbose "resourceName: $ResourceName" -Verbose
            Write-Verbose "resourceGroupName: $ResourceGroupName" -Verbose
            Write-Verbose "subscriptionId: $SubId" -Verbose

            # Determine code path depending on the resourceType
            if ($ResourceType -eq "Microsoft.Compute/virtualMachines")
            {
                # This is an Resource Manager VM
                Write-Verbose "This is an Resource Manager VM." -Verbose

                # Authenticate to Azure with service principal and certificate and set subscription
                Write-Verbose "Authenticating to Azure with service principal and certificate" -Verbose
                $ConnectionAssetName = "AzureRunAsConnection"
                Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
                $Conn = Get-AutomationConnection -Name $ConnectionAssetName
                if ($Conn -eq $null)
                {
                    throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
                }
                Write-Verbose "Authenticating to Azure with service principal." -Verbose
                Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
                Write-Verbose "Setting subscription to work against: $SubId" -Verbose
                Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

                # Stop the Resource Manager VM
                Write-Verbose "Stopping the VM - $ResourceName - in resource group - $ResourceGroupName -" -Verbose
                Stop-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName -Force
                # [OutputType(PSAzureOperationResponse")]
            }
            else {
                # ResourceType not supported
                Write-Error "$ResourceType is not a supported resource type for this runbook."
            }
        }
        else {
            # The alert status was not 'Activated' or 'Fired' so no action taken
            Write-Verbose ("No action taken. Alert status: " + $status) -Verbose
        }
    }
    else {
        # Error
        Write-Error "This runbook is meant to be started from an Azure alert webhook only."
    }
    ```

6. Runbook'u kaydetmek ve yayımlamak için **Yayımla'yı** seçin.

## <a name="create-the-alert"></a>Uyarıyı oluşturma

Uyarılar, uyarı tarafından tetiklenen eylem koleksiyonları olan eylem gruplarını kullanır. Runbook'lar, eylem gruplarıyla kullanabileceğiniz birçok eylemden sadece biridir.

1. Otomasyon Hesabınızda İzleme **altında** **Uyarılar'ı** seçin.
1. **+ Yeni uyarı kuralı**'nı seçin.
1. **Kaynak**altında **Seç'i** tıklatın. **Kaynak** seç sayfasında uyarı için VM'nizi seçin ve **Bitti'yi**tıklatın.
1. **Koşul**altında **koşul ekle'yi** tıklatın. Kullanmak istediğiniz sinyali seçin, örneğin **Yüzde CPU** ve **Bitti'yi**tıklatın.
1. Sinyali **yapılandırma mantığı** sayfasında, **Uyarı mantığı**altında Eşik **değerinizi** girin ve **Bitti'yi**tıklatın.
1. **Eylem grupları altında,** **Yeni Oluştur'u**seçin.
1. Eylem **grubu ekle** sayfasında, eylem grubunuza bir ad ve kısa bir ad verin.
1. Eyleme bir ad verin. Eylem türü için **Otomasyon Runbook'u'nu**seçin.
1. **Ayrıntıları Edit'i**seçin. **Runbook'u Yapılandır** sayfasında, **Runbook kaynağı**altında **Kullanıcı'yı**seçin.  
1. **Abonelik** ve **Otomasyon hesabınızı**seçin ve ardından **Stop-AzureVmInResponsetoVMAlert** runbook'unu seçin.  
1. **Ortak uyarı şemasını etkinleştirmek için Evet'i**seçin. **Yes**
1. Eylem grubu oluşturmak için **Tamam'ı**seçin.

    ![Eylem grubu sayfası ekleme](./media/automation-create-alert-triggered-runbook/add-action-group.png)

    Bu eylem grubunu etkinlik [günlüğü uyarılarında](../azure-monitor/platform/activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json) ve oluşturduğunuz [gerçek zamanlı uyarıların yakınında](../azure-monitor/platform/alerts-overview.md?toc=%2fazure%2fautomation%2ftoc.json) kullanabilirsiniz.

1. **Uyarı Ayrıntıları**altında, bir uyarı kuralı adı ve açıklama ekleyin ve uyarı **kuralı oluştur'u**tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

* Bir webhook kullanarak Bir Otomasyon runbook başlatma hakkında daha fazla bilgi için [bkz.](automation-webhooks.md)
* Runbook'u başlatmanın farklı yolları hakkında ayrıntılı bilgi [için](automation-starting-a-runbook.md)bkz.
* Etkinlik günlüğü uyarısı nasıl oluşturulacak öğrenmek için [bkz.](../azure-monitor/platform/activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)
* Neredeyse gerçek zamanlı bir uyarı oluşturmayı öğrenmek için azure [portalında bir uyarı kuralı oluşturma'ya](../azure-monitor/platform/alerts-metric.md?toc=/azure/azure-monitor/toc.json)bakın.
