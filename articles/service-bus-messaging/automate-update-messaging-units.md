---
title: Azure Service Bus-mesajlaşma birimlerini otomatik olarak güncelleştir
description: Bu makalede, bir Service Bus ad alanının mesajlaşma birimlerini otomatik olarak güncelleştirmek için bir Azure Otomasyonu runbook 'u nasıl kullanabileceğiniz gösterilmektedir.
ms.topic: how-to
ms.date: 06/23/2020
ms.openlocfilehash: 52f5b13b482739bfa56ff606f684fd5a9c7d3b6e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85341488"
---
# <a name="automatically-update-messaging-units-of-an-azure-service-bus-namespace"></a>Azure Service Bus bir ad alanının mesajlaşma birimlerini otomatik olarak güncelleştir 
Bu makalede, kaynak (CPU veya bellek) kullanımı temel alınarak bir Service Bus ad alanının [mesajlaşma birimlerini](service-bus-premium-messaging.md) otomatik olarak nasıl güncelleştirebilmeniz gösterilmektedir. 

Bu makaledeki örnekte, ad alanı CPU kullanımı %75 üzerinde kaldığında bir Service Bus ad alanı için mesajlaşma birimlerinin nasıl artırdığı gösterilmektedir. Üst düzey adımlar şunlardır:

1. Bir Service Bus ad alanı için mesajlaşma birimlerini ölçeklendirirken (artan) bir PowerShell betiği ile Azure Otomasyonu runbook oluşturun. 
2. Ad alanı, CPU kullanımı %75 üzerinde kaldığında PowerShell betiğini çağıran Service Bus ad alanında bir CPU Kullanım Uyarısı oluşturun. 

> [!IMPORTANT]
> Bu makale yalnızca Azure Service Bus **Premium** katmanı için geçerlidir. 


## <a name="create-a-service-bus-namespace"></a>Service Bus ad alanı oluşturma
Premier katman Service Bus ad alanı oluşturun. Ad alanını oluşturmak için [Azure Portal makalesindeki ad alanı oluşturma bölümündeki](service-bus-quickstart-portal.md#create-a-namespace-in-the-azure-portal) adımları izleyin. 

## <a name="create-an-azure-automation-account"></a>Azure Otomasyonu hesabı oluşturma
[Azure Otomasyonu hesabı oluşturma](../automation/automation-quickstart-create-account.md) makalesindeki yönergeleri Izleyerek bir Azure Otomasyonu hesabı oluşturun. 

## <a name="import-azservice-module-from-gallery"></a>Galeriden az. Service modülünü içeri aktar
`Az.Accounts`Galeriden ve `Az.ServiceBus` modüllerden Otomasyon hesabına içeri aktarma. `Az.ServiceBus`Modül `Az.Accounts` modüle bağlıdır, bu nedenle önce yüklenmesi gerekir. 

Adım adım yönergeler için bkz. modül [galerisinden modül Içeri aktarma](../automation/automation-runbook-gallery.md#import-a-module-from-the-module-gallery-with-the-azure-portal).

## <a name="create-and-publish-a-powershell-runbook"></a>PowerShell runbook 'u oluşturma ve yayımlama

1. [PowerShell runbook 'U oluşturma](../automation/automation-quickstart-create-runbook.md) makalesindeki yönergeleri Izleyerek bir PowerShell runbook 'u oluşturun. 

    Bir Service Bus ad alanı için mesajlaşma birimlerini artırmak üzere kullanabileceğiniz örnek bir PowerShell betiği aşağıda verilmiştir. Otomasyon Runbook 'unda bu PowerShell betiği, MUs 1 ile 2 arasında, 2 ile 4 arasında veya 4 ile 8 arasında artar. Bu özellik için izin verilen değerler şunlardır: 1, 2, 4 ve 8. Mesajlaşma birimlerini azaltmak için başka bir runbook oluşturabilirsiniz.

    **NamespaceName** ve **resourcegroupname** parametreleri, komut dosyasını uyarı senaryosundan ayrı test etmek için kullanılır. 
    
    **Web kancası verileri** parametresi, çalışma zamanında kaynak grubu adı, kaynak adı vb. gibi bilgileri iletmek için uyarır. 

    ```powershell
    [OutputType("PSAzureOperationResponse")]
    param
    (
        [Parameter (Mandatory=$false)]
        [object] $WebhookData,
    
        [Parameter (Mandatory = $false)]
        [String] $namespaceName,
    
        [Parameter (Mandatory = $false)]
        [String] $resourceGroupName
    )
    
    
    if ($WebhookData)
    {
        # Get the data object from WebhookData
        $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)
    
        # Get the alert schema ID
        $schemaId = $WebhookBody.schemaId

        # If it's a metric alert
        if ($schemaId -eq "AzureMonitorMetricAlert") {

            # Get the resource group name from the alert context
            $resourceGroupName = $WebhookBody.resourceGroupName
            
            # Get the namespace name from the alert context
            $namespaceName = $WebhookBody.resourceName
        }
    }
    
    # Connect to Azure account
    $connection = Get-AutomationConnection -Name AzureRunAsConnection
    
    while(!($connectionResult) -And ($logonAttempt -le 10))
    {
        $LogonAttempt++
        # Logging in to Azure...
        $connectionResult =    Connect-AzAccount `
                                    -ServicePrincipal `
                                    -Tenant $connection.TenantID `
                                    -ApplicationId $connection.ApplicationID `
                                    -CertificateThumbprint $connection.CertificateThumbprint
    
        Start-Sleep -Seconds 30
    }
    
    # Get the current capacity (number of messaging units) of the namespace
    $sbusns=Get-AzServiceBusNamespace `
        -Name $namespaceName `
        -ResourceGroupName $resourceGroupName
    
    $currentCapacity = $sbusns.Sku.Capacity
    
    # Increase the capacity
    # Capacity can be one of these values: 1, 2, 4, 8
    if ($currentCapacity -eq 1) {
        $newMU = 2
    }
    elseif ($currentCapacity -eq 2) {
        $newMU = 4
    }
    elseif ($currentCapacity -eq 4) {
        $newMU = 8    
    }
    else {
    
    }
    
    # Update the capacity of the namespace
    Set-AzServiceBusNamespace `
            -Location eastus `
            -SkuName Premium `
            -Name $namespaceName `
            -SkuCapacity $newMU `
            -ResourceGroupName $resourceGroupName

    ```
2. **NamespaceName** ve **resourcegroupname** parametrelerinin değerlerini belirterek [çalışma kitabını test](../automation/manage-runbooks.md#test-a-runbook) edin. Ad alanındaki mesajlaşma birimlerinin güncelleştirildiğinden emin olun. 
3. Başarıyla test ettikten sonra [çalışma kitabını](..//automation/manage-runbooks.md#publish-a-runbook) , daha sonra ad alanındaki bir uyarı için eylem olarak eklemek üzere yayımlayın. 

## <a name="create-an-alert-on-the-namespace-to-trigger-the-runbook"></a>Runbook 'u tetiklemek için ad alanı üzerinde bir uyarı oluşturma
Oluşturduğunuz Otomasyon Runbook 'unu tetiklemek üzere Service Bus ad alanında bir uyarı yapılandırmak için bkz. [Azure Otomasyonu runbook 'u tetiklemek için uyarı kullanma](../automation/automation-create-alert-triggered-runbook.md) makalesi. Örneğin, ad alanı başına **CPU kullanımı** için bir uyarı veya **ad alanı ölçümü başına bellek boyutu kullanımı** üzerinde bir uyarı oluşturabilir ve oluşturduğunuz Otomasyon Runbook 'unu tetiklemek için bir eylem ekleyebilirsiniz. Bu ölçümler hakkında daha fazla bilgi için bkz. [kaynak kullanım ölçümleri](service-bus-metrics-azure-monitor.md#resource-usage-metrics).

Aşağıdaki yordamda, **CPU kullanımı ad alanı CPU kullanımı** **%75**üzerinde kaldığında Otomasyon Runbook 'unu tetikleyen bir uyarının nasıl oluşturulacağı gösterilmektedir.

1. Ad alanı için **Service Bus ad alanı** sayfasında, sol taraftaki menüden **Uyarılar** ' ı seçin ve ardından araç çubuğunda **+ Yeni uyarı kuralı** ' nı seçin. 
    
    ![Uyarılar sayfası-yeni uyarı kuralı düğmesi](./media/automate-update-messaging-units/alerts-page.png)
2. **Uyarı kuralı oluştur** sayfasında **Koşul Seç**' e tıklayın. 

    ![Uyarı kuralı oluştur sayfası-koşul Seç](./media/automate-update-messaging-units/alert-rule-select-condition.png) 
3. **Sinyal mantığını Yapılandır** sayfasında, sinyal için **CPU** ' yı seçin. 

    ![Sinyal mantığını yapılandırma-CPU seçin](./media/automate-update-messaging-units/configure-signal-logic.png)
4. Bir **eşik değeri** girin (Bu örnekte, **%75**) ve **bitti**' yi seçin. 

    ![CPU sinyalini yapılandırma](./media/automate-update-messaging-units/cpu-signal-configuration.png)
5. Şimdi **uyarı oluştur sayfasında** **eylem grubu seç**' e tıklayın.
    
    ![Eylem grubunu seçin](./media/automate-update-messaging-units/select-action-group-button.png)
6. Araç çubuğunda **eylem grubu oluştur** düğmesini seçin. 

    ![Eylem grubu oluştur düğmesi](./media/automate-update-messaging-units/create-action-group-button.png)
7. **Eylem grubu Ekle** sayfasında, aşağıdaki adımları uygulayın:
    1. Eylem grubu için bir **ad** girin. 
    2. Eylem grubu için **kısa bir ad** girin.
    3. Bu eylem grubunu oluşturmak istediğiniz **aboneliği** seçin.
    4. **Kaynak grubunu**seçin. 
    5. **Eylemler** bölümünde **eylem için bir ad**girin ve **eylem türü**için **Otomasyon Runbook** ' u seçin. 

        ![Eylem grubu Ekle sayfası](./media/automate-update-messaging-units/add-action-group-page.png)
8. **Runbook 'U Yapılandır** sayfasında, aşağıdaki adımları uygulayın:
    1. **Runbook kaynağı**için **Kullanıcı**' yı seçin. 
    2. **Abonelik**için Otomasyon hesabını içeren Azure **aboneliğinizi** seçin. 
    3. **Otomasyon hesabı**için **Otomasyon hesabınızı**seçin.
    4. **Runbook**için Runbook 'unuzu seçin. 
    5. **Runbook 'U Yapılandır** sayfasında **Tamam ' ı** seçin. 
        ![Runbook 'u yapılandırma](./media/automate-update-messaging-units/configure-runbook.png)
9. **Eylem grubu Ekle** sayfasında **Tamam ' ı** seçin. 
5. Şimdi, **Uyarı kuralı oluştur** sayfasında **kural için bir ad**girin ve ardından **Uyarı kuralı oluştur**' u seçin. 
    ![Uyarı kuralı oluşturma](./media/automate-update-messaging-units/create-alert-rule.png)

    > [!NOTE]
    > Artık, CPU kullanımı ad alanı 75 üzerinde kaldığında, uyarı Service Bus ad alanının mesajlaşma birimini artıran Otomasyon Runbook 'unu tetikler. Benzer şekilde, diğer bir Otomasyon Runbook 'u için bir uyarı oluşturabilirsiniz. Bu, ad alanı CPU kullanımı 25 ' in altında olursa mesajlaşma birimlerini azaltır. 

## <a name="next-steps"></a>Sonraki adımlar
Mesajlaşma birimleri hakkında bilgi edinmek için bkz. [Premium mesajlaşma](service-bus-premium-messaging.md)
