---
title: Azure Monitor ölçümleri veritabanına klasik Bulut Hizmetleri ölçümleri gönderme
description: Azure klasik Bulut Hizmetleri için Konuk İşletim Sistemi performans ölçümlerini Azure Monitor metrik deposuna gönderme işlemini açıklar.
author: anirudhcavale
services: azure-monitor
ms.topic: conceptual
ms.date: 09/09/2019
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: 3b390ffa20cf3cf79b8fb6311ad05b2978bd5d24
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77655814"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-classic-cloud-services"></a>Konuk İşletim Sistemi ölçümlerini Azure Monitor metrik deposu klasik Bulut Hizmetleri'ne gönderin 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Monitör [Tanılama uzantısı](diagnostics-extension-overview.md)ile sanal makine, bulut hizmeti veya Hizmet Kumaşı kümesinin bir parçası olarak çalışan konuk işletim sisteminden (Konuk İşletim Sistemi) ölçümlerve günlükler toplayabilirsiniz. Uzantı, telemetriyi [birçok farklı konuma gönderebilir.](https://docs.microsoft.com/azure/monitoring/monitoring-data-collection?toc=/azure/azure-monitor/toc.json)

Bu makalede, Azure klasik Bulut Hizmetleri için Konuk İşletim Sistemi performans ölçümlerini Azure Monitor metrik deposuna gönderme işlemi açıklanmaktadır. Diagnostics sürüm 1.11'den başlayarak, ölçümleri standart platform ölçümlerinin zaten toplandığı Azure Monitor ölçüm mağazasına doğrudan yazabilirsiniz. 

Bunları bu konumda depolamak, platform ölçümleri için kullanabileceğiniz eylemlere erişmenize olanak tanır. Eylemler neredeyse gerçek zamanlı uyarı, grafik, yönlendirme, REST API'den erişim ve daha fazlasını içerir.  Geçmişte, Tanılama uzantısı Azure Depolama'ya yazdı, ancak Azure Monitor veri deposuna yazmadı.  

Bu makalede özetlenen işlem yalnızca Azure Bulut Hizmetleri'ndeki performans sayaçları için çalışır. Diğer özel ölçümler için çalışmaz. 

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğinizde [bir hizmet yöneticisi veya yardımcı yönetici](../../cost-management-billing/manage/add-change-subscription-administrator.md) olmalısınız. 

- Aboneliğiniz [Microsoft.Insights'a](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services)kayıtlı olmalıdır. 

- [Azure PowerShell](/powershell/azure) veya Azure [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) yüklü olması gerekir.

- Bulut Hizmetiniz özel [ölçümleri destekleyen](metrics-custom-overview.md#supported-regions)bir bölgede olmalıdır.

## <a name="provision-a-cloud-service-and-storage-account"></a>Bulut hizmeti ve depolama hesabı sağlama 

1. Klasik bir bulut hizmeti oluşturun ve dağıtın. Azure Bulut Hizmetleri ve ASP.NET ile [başlatılan](../../cloud-services/cloud-services-dotnet-get-started.md)bir örnek klasik Bulut Hizmetleri uygulaması ve dağıtımı bulabilirsiniz. 

2. Varolan bir depolama hesabı kullanabilir veya yeni bir depolama hesabı dağıtabilirsiniz. Depolama hesabının oluşturduğunuz klasik bulut hizmetiyle aynı bölgede olması en iyisidir. Azure portalında, Depolama **hesapları** kaynak bıçağına gidin ve ardından **Keys'i**seçin. Depolama hesabı adını ve depolama hesabı anahtarını not alın. Daha sonraki adımlarda bu bilgilere ihtiyacınız olacak.

   ![Depolama hesabı anahtarları](./media/collect-custom-metrics-guestos-vm-cloud-service-classic/storage-keys.png)

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma 

[Kaynaklara erişebilen bir Azure Active Directory uygulaması ve hizmet ilkesi oluşturmak için Kullanım portalındaki](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)yönergeleri kullanarak Azure Etkin Dizin kiracınızda bir hizmet ilkesi oluşturun. Bu süreçten geçerken aşağıdakileri not edin: 

- Oturum açma URL'si için herhangi bir URL koyabilirsiniz.  
- Bu uygulama için yeni istemci sırrı oluşturun.  
- Anahtarı ve istemci kimliğini sonraki adımlarda kullanmak üzere kaydedin.  

Önceki adımda oluşturulan uygulamaya *İzleme Ölçümleri Yayıncıizinlerini* karşı ölçümler ibareleri vermek istediğiniz kaynağa verin. Uygulamayı birçok kaynağa karşı özel ölçümler yaramak için kullanmayı planlıyorsanız, bu izinleri kaynak grubu veya abonelik düzeyinde verebilirsiniz.  

> [!NOTE]
> Tanılama uzantısı, Azure Monitörü'ne karşı kimlik doğrulamak ve bulut hizmetiniz için ölçümler oluşturmak için hizmet ilkesini kullanır.

## <a name="author-diagnostics-extension-configuration"></a>Yazar Tanılama uzantısı yapılandırması 

Diagnostics uzantılı yapılandırma dosyanızı hazırlayın. Bu dosya, Tanılama uzantısının bulut hizmetiniz için toplaması gereken günlükleri ve performans sayaçlarını belirler. Aşağıda örnek bir Tanılama yapılandırma dosyası verem:  

```XML
<?xml version="1.0" encoding="utf-8"?> 
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"> 
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"> 
    <WadCfg> 
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096"> 
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" /> 
        <Directories scheduledTransferPeriod="PT1M"> 
          <IISLogs containerName="wad-iis-logfiles" /> 
          <FailedRequestLogs containerName="wad-failedrequestlogs" /> 
        </Directories> 
        <PerformanceCounters scheduledTransferPeriod="PT1M"> 
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" /> 
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT15S" /> 
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" /> 
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Page Faults/sec" sampleRate="PT15S" /> 
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

Tanılama dosyanızın "SinksConfig" bölümünde yeni bir Azure Monitor lavabosu tanımlayın: 

```XML
  <SinksConfig> 
    <Sink name="AzMonSink"> 
    <AzureMonitor> 
      <ResourceId>-Provide ClassicCloudService’s Resource ID-</ResourceId> 
      <Region>-Azure Region your Cloud Service is deployed in-</Region> 
    </AzureMonitor> 
    </Sink> 
  </SinksConfig> 
```

Yapılandırma dosyanızın toplanacak performans sayaçlarını listelediğiniz bölümüne Azure Monitör lavabosu ekleyin. Bu giriş, belirttiğiniz tüm performans sayaçlarının metrik olarak Azure Monitor'a yönlendirilmesini sağlar. İhtiyaçlarınıza göre performans sayaçları ekleyebilir veya kaldırabilirsiniz. 

```xml
    <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="AzMonSink">
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" />
    ...
    </PerformanceCounters>
```

Son olarak, özel yapılandırmada bir *Azure Monitör Hesabı* bölümü ekleyin. Daha önce oluşturduğunuz hizmet inanın istemci kimliğini ve sırrını girin. 

```XML
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"> 
  <StorageAccount name="" endpoint="" /> 
    <AzureMonitorAccount> 
      <ServicePrincipalMeta> 
        <PrincipalId>clientId from step 3</PrincipalId> 
        <Secret>client secret from step 3</Secret> 
      </ServicePrincipalMeta> 
    </AzureMonitorAccount> 
</PrivateConfig> 
```

Bu tanılama dosyanı yerel olarak kaydedin.  

## <a name="deploy-the-diagnostics-extension-to-your-cloud-service"></a>Tanılama uzantısını bulut hizmetinize dağıtma 

PowerShell'i başlatın ve Azure'da oturum açın. 

```powershell
Login-AzAccount 
```

Daha önce oluşturduğunuz depolama hesabının ayrıntılarını depolamak için aşağıdaki komutları kullanın. 

```powershell
$storage_account = <name of your storage account from step 3> 
$storage_keys = <storage account key from step 3> 
```

Benzer şekilde, tanılama dosya yolunu aşağıdaki komutu kullanarak bir değişkene ayarlayın:

```powershell
$diagconfig = “<path of the Diagnostics configuration file with the Azure Monitor sink configured>” 
```

Aşağıdaki komut kullanılarak yapılandırılan Azure Monitor lavabosuyla tanılama dosyasıyla Tanılama uzantısını bulut hizmetinize dağıtın:  

```powershell
Set-AzureServiceDiagnosticsExtension -ServiceName <classicCloudServiceName> -StorageAccountName $storage_account -StorageAccountKey $storage_keys -DiagnosticsConfigurationPath $diagconfig 
```

> [!NOTE] 
> Tanılama uzantısıyüklemesinin bir parçası olarak bir depolama hesabı sağlamak yine de zorunludur. Tanılama config dosyasında belirtilen tüm günlükler veya performans sayaçları belirtilen depolama hesabına yazılır.  

## <a name="plot-metrics-in-the-azure-portal"></a>Azure portalında çizim ölçümleri 

1. Azure portalına gidin. 

   ![Ölçümler Azure portalı](./media/collect-custom-metrics-guestos-vm-cloud-service-classic/navigate-metrics.png)

2. Sol menüde **Monitör'ü seçin.**

3. **Monitör** **bıçağında, Ölçümler Önizleme** sekmesini seçin.

4. Kaynaklar açılır menüsünde, klasik bulut hizmetinizi seçin.

5. Ad boşlukları açılır menüsünde **azure.vm.windows.guest'i**seçin. 

6. Ölçümler açılır menüsünde, **Kullanımda Bellek\Taahhüt Lütmet Baytlarını**seçin. 

Belirli bir rol veya rol örneği tarafından kullanılan toplam belleği görüntülemek için boyut filtreleme ve bölme özelliklerini kullanırsınız. 

 ![Ölçümler Azure portalı](./media/collect-custom-metrics-guestos-vm-cloud-service-classic/metrics-graph.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Özel ölçümler](metrics-custom-overview.md)hakkında daha fazla bilgi edinin.

