---
title: Azure Monitor ölçümleri veritabanına klasik Windows VM ölçümleri gönderme
description: Konuk İşletim Sistemi ölçümlerini Bir Windows sanal makinesi için Azure Monitor veri deposuna gönderin (klasik)
author: anirudhcavale
services: azure-monitor
ms.topic: conceptual
ms.date: 09/09/2019
ms.author: ancav
ms.subservice: ''
ms.openlocfilehash: 65bb1a3915ece384974da12b4e7a1ad0c1e08133
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77655829"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metrics-database-for-a-windows-virtual-machine-classic"></a>Konuk İşletim Sistemi ölçümlerini Bir Windows sanal makinesi için Azure Monitor ölçümleri veritabanına gönderin (klasik)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Monitör [Tanılama uzantısı](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) ("WAD" veya "Tanılama" olarak bilinir), sanal makine, bulut hizmeti veya Service Fabric kümesinin bir parçası olarak çalışan konuk işletim sisteminden (Konuk İşletim Sistemi) ölçümleri ve günlükleri toplamanızı sağlar. Uzantı, telemetriyi [birçok farklı konuma gönderebilir.](https://docs.microsoft.com/azure/monitoring/monitoring-data-collection?toc=/azure/azure-monitor/toc.json)

Bu makalede, Bir Windows sanal makine (klasik) için Konuk İşletim Sistemi performans ölçümlerini Azure Monitor metrik veritabanına gönderme işlemi açıklanmaktadır. Diagnostics sürüm 1.11'den başlayarak, ölçümleri standart platform ölçümlerinin zaten toplandığı Azure Monitor ölçüm mağazasına doğrudan yazabilirsiniz. 

Bunları bu konumda depolamak, platform ölçümleri için yaptığınız eylemlerle aynı eylemlere erişmenizi sağlar. Eylemler neredeyse gerçek zamanlı uyarı, grafik, yönlendirme, REST API'den erişim ve daha fazlasını içerir. Geçmişte, Tanılama uzantısı Azure Depolama'ya yazdı, ancak Azure Monitor veri deposuna yazmadı. 

Bu makalede özetlenen işlem yalnızca Windows işletim sistemini çalıştıran klasik sanal makinelerde çalışır.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğinizde [bir hizmet yöneticisi veya yardımcı yönetici](../../cost-management-billing/manage/add-change-subscription-administrator.md) olmalısınız. 

- Aboneliğiniz [Microsoft.Insights'a](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services)kayıtlı olmalıdır. 

- [Azure PowerShell](/powershell/azure) veya Azure [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) yüklü olması gerekir.

- VM kaynağınız özel [ölçümleri destekleyen](metrics-custom-overview.md#supported-regions)bir bölgede olmalıdır.

## <a name="create-a-classic-virtual-machine-and-storage-account"></a>Klasik bir sanal makine ve depolama hesabı oluşturma

1. Azure portalını kullanarak klasik bir VM oluşturun.
   ![Klasik VM Oluştur](./media/collect-custom-metrics-guestos-vm-classic/create-classic-vm.png)

1. Bu VM'yi oluştururken, yeni bir klasik depolama hesabı oluşturma seçeneğini seçin. Bu depolama hesabını daha sonraki adımlarda kullanırız.

1. Azure portalında, Depolama **hesapları** kaynak bıçak larına gidin. **Keys'i**seçin ve depolama hesabı adı ve depolama hesabı anahtarını not alın. Daha sonraki adımlarda bu bilgilere ihtiyacınız var.
   ![Depolama erişim anahtarları](./media/collect-custom-metrics-guestos-vm-classic/storage-access-keys.png)

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

[Hizmet anapara oluştur'daki](../../active-directory/develop/howto-create-service-principal-portal.md)yönergeleri kullanarak Azure Etkin Dizin kiracınızda bir hizmet ilkesi oluşturun. Bu süreçten geçerken aşağıdakileri not edin: 
- Bu uygulama için yeni istemci sırrı oluşturun.
- Anahtarı ve istemci kimliğini sonraki adımlarda kullanmak üzere kaydedin.

Bu uygulamaya karşı ölçümler yaymak istediğiniz kaynağa "İzleme Ölçümleri Yayıncısı" izinleri verin. Bir kaynak grubu veya aboneliğin tamamını kullanabilirsiniz.  

> [!NOTE]
> Tanılama uzantısı, Azure Monitör'e karşı kimlik doğrulamak ve klasik VM'niz için ölçümler oluşturmak için hizmet ilkesini kullanır.

## <a name="author-diagnostics-extension-configuration"></a>Yazar Tanılama uzantısı yapılandırması

1. Diagnostics uzantılı yapılandırma dosyanızı hazırlayın. Bu dosya, tanılama uzantısının klasik VM'iniz için hangi günlükleri ve performans sayaçlarını toplaması gerektiğini belirler. Aşağıda bir örnek verilmiştir:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
        <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
            <Directories scheduledTransferPeriod="PT1M">
                <IISLogs containerName="wad-iis-logfiles" />
                <FailedRequestLogs containerName="wad-failedrequestlogs" />
            </Directories>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
                <PerformanceCounterConfiguration counterSpecifier="\Processor(*)\% Processor Time" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(*)\Disk Read Bytes/sec" sampleRate="PT15S" />
            </PerformanceCounters>
            <WindowsEventLog scheduledTransferPeriod="PT1M">
                <DataSource name="Application!*[System[(Level=1 or Level=2 or Level=3)]]" />
                <DataSource name="Windows Azure!*[System[(Level=1 or Level=2 or Level=3 or Level=4)]]" />
            </WindowsEventLog>
            <CrashDumps>
                <CrashDumpConfiguration processName="WaIISHost.exe" />
                <CrashDumpConfiguration processName="WaWorkerHost.exe" />
                <CrashDumpConfiguration processName="w3wp.exe" />
            </CrashDumps>
            <Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Error" />
            <Metrics resourceId="/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicCompute/virtualMachines/MyClassicVM">
                <MetricAggregation scheduledTransferPeriod="PT1M" />
                <MetricAggregation scheduledTransferPeriod="PT1H" />
            </Metrics>
        </DiagnosticMonitorConfiguration>
        <SinksConfig>
        </SinksConfig>
        </WadCfg>
        <StorageAccount />
    </PublicConfig>
    <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <StorageAccount name="" endpoint="" />
    </PrivateConfig>
    <IsEnabled>true</IsEnabled>
    </DiagnosticsConfiguration>
    ```
1. Tanılama dosyanızın "SinksConfig" bölümünde, aşağıdaki gibi yeni bir Azure Monitor lavabosu tanımlayın:

    ```xml
    <SinksConfig>
        <Sink name="AzMonSink">
            <AzureMonitor>
                <ResourceId>Provide the resource ID of your classic VM </ResourceId>
                <Region>The region your VM is deployed in</Region>
            </AzureMonitor>
        </Sink>
    </SinksConfig>
    ```

1. Yapılandırma dosyanızın toplanacak performans sayaçları listesinin listelendiği bölümünde, performans sayaçlarını Azure Monitörü "AzMonSink" lavabosuna yönlendirin.

    ```xml
    <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="AzMonSink">
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" />
    ...
    </PerformanceCounters>
    ```

1. Özel yapılandırmada Azure Monitor hesabını tanımlayın. Ardından, ölçümleri yatmak için kullanılacak hizmet temel bilgilerini ekleyin.

    ```xml
    <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="" endpoint="" />
        <AzureMonitorAccount>
            <ServicePrincipalMeta>
                <PrincipalId>clientId for your service principal</PrincipalId>
                <Secret>client secret of your service principal</Secret>
            </ServicePrincipalMeta>
        </AzureMonitorAccount>
    </PrivateConfig>
    ```

1. Bu dosyayı yerel olarak kaydedin.

## <a name="deploy-the-diagnostics-extension-to-your-cloud-service"></a>Tanılama uzantısını bulut hizmetinize dağıtma

1. PowerShell'i başlatın ve oturum açın.

    ```powershell
    Login-AzAccount
    ```

1. Klasik VM'nizin bağlamını ayarlayarak başlayın.

    ```powershell
    $VM = Get-AzureVM -ServiceName <VM’s Service_Name> -Name <VM Name>
    ```

1. VM ile oluşturulan klasik depolama hesabının bağlamını ayarlayın.

    ```powershell
    $StorageContext = New-AzStorageContext -StorageAccountName <name of your storage account from earlier steps> -storageaccountkey "<storage account key from earlier steps>"
    ```

1.  Tanılama dosya yolunu aşağıdaki komutu kullanarak bir değişkene ayarlayın:

    ```powershell
    $diagconfig = “<path of the diagnostics configuration file with the Azure Monitor sink configured>”
    ```

1.  Azure Monitor lavabosu yapılandırılan tanılama dosyasıyla klasik VM'iniz için güncelleştirmeyi hazırlayın.

    ```powershell
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $diagconfig -VM $VM -StorageContext $Storage_Context
    ```

1.  Aşağıdaki komutu çalıştırarak güncelleştirmeyi VM'nize dağıtın:

    ```powershell
    Update-AzureVM -ServiceName "ClassicVMWAD7216" -Name "ClassicVMWAD" -VM $VM_Update.VM
    ```

> [!NOTE]
> Tanılama uzantısı yüklemesinin bir parçası olarak bir depolama hesabı sağlamak yine de zorunludur. Diagnostics config dosyasında belirtilen günlükler veya performans sayaçları belirtilen depolama hesabına yazılır.

## <a name="plot-the-metrics-in-the-azure-portal"></a>Azure portalındaki ölçümleri çizin

1.  Azure portalına gidin. 

1.  Sol menüde **Monitör'ü seçin.**

1.  **Monitör** bıçağında **Ölçümler'i**seçin.

    ![Ölçümlerde gezinme](./media/collect-custom-metrics-guestos-vm-classic/navigate-metrics.png)

1. Kaynak açılır menüsünde klasik VM'nizi seçin.

1. Ad boşlukları açılır menüsünde **azure.vm.windows.guest'i**seçin.

1. Ölçümler açılır menüsünde, **Kullanımda Bellek\Taahhüt Lütmet Baytlarını**seçin.
   ![Çizim ölçümleri](./media/collect-custom-metrics-guestos-vm-classic/plot-metrics.png)


## <a name="next-steps"></a>Sonraki adımlar
- [Özel ölçümler](metrics-custom-overview.md)hakkında daha fazla bilgi edinin.

