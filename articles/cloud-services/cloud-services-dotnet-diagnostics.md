---
title: Bulut Hizmetleri ile Azure tanılama (.NET) nasıl kullanılır | Microsoft Dokümanlar
description: Hata ayıklama, performans ölçme, izleme, trafik analizi ve daha fazlası için Azure bulut Hizmetleri'nden veri toplamak için Azure tanılama yöntemini kullanma.
services: cloud-services
documentationcenter: .net
author: tgore03
manager: carmonm
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.date: 05/22/2017
ms.author: tagore
ms.openlocfilehash: 1e49a0935a70a2470267e5458fa1f55e3059e965
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77469774"
---
# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a>Azure Bulut Hizmetlerinde Azure Tanılama'yı Etkinleştirme
Azure Tanılama ile ilgili arka plan için [Azure Tanılama](../azure-diagnostics.md) Genel Bakış'a bakın.

## <a name="how-to-enable-diagnostics-in-a-worker-role"></a>İşçi Rolünde Tanılama Nasıl Etkinleştirilir?
Bu izbis, .NET EventSource sınıfını kullanarak telemetri verilerini yakan bir Azure çalışanı rolünün nasıl uygulanacağını açıklar. Azure Tanılama, telemetri verilerini toplamak ve bir Azure depolama hesabında depolamak için kullanılır. Bir çalışan rolü oluştururken, Visual Studio ,NET 2.4 ve daha önceki Azure SDK'larında çözümün bir parçası olarak Tanılama 1.0'ı otomatik olarak etkinleştirirken. Aşağıdaki yönergeler, alt rol oluşturma, Çözümden Tanılama 1.0'ı devre dışı bırakma ve Tanılama 1.2 veya 1.3'u çalışan rolünüze dağıtma işlemini açıklar.

### <a name="prerequisites"></a>Ön koşullar
Bu makalede, bir Azure aboneliğiniz olduğu ve Azure SDK ile Visual Studio'yu kullandığınız varsayar. Azure aboneliğiniz yoksa Ücretsiz [Deneme][Free Trial]sürümüne kaydolabilirsiniz. [Azure PowerShell sürümünü 0.8.7 veya sonraki sürümlere yüklediğinizden ve yapılandırdığından][Install and configure Azure PowerShell version 0.8.7 or later]emin olun.

### <a name="step-1-create-a-worker-role"></a>Adım 1: Çalışan Rolü Oluşturma
1. **Visual Studio**’yu başlatın.
2. .NET Framework 4.5'i hedefleyen **Bulut** şablonundan bir **Azure Bulut Hizmeti** projesi oluşturun.  Projeyi "WadExample" olarak adlandırın ve Tamam'ı tıklatın.
3. **İşçi Rolü'nü** seçin ve Tamam'ı tıklatın. Proje oluşturulacak.
4. **Çözüm Gezgini'nde,** **WorkerRole1** özellikleri dosyasını çift tıklatın.
5. **Yapılandırma** sekmesinde, Tanılama 1.0'ı (Azure SDK 2.4 ve daha önce) devre dışı bırakıp **Tanılamayı Etkinleştir'i** denetlemeyi kaldırın.
6. Herhangi bir hatanız olmadığını doğrulamak için çözümünüzü oluşturun.

### <a name="step-2-instrument-your-code"></a>Adım 2: Kodunuzu enstrüman
WorkerRole.cs içeriğini aşağıdaki kodla değiştirin. Class SampleEventSourceWriter, [EventSource Sınıfından][EventSource Class]devralınan, dört günlük yöntemleri uygular: **SendEnums**, **MessageMethod**, **SetOther** ve **HighFreq**. **WriteEvent** yönteminin ilk parametresi, ilgili olayın kimliğini tanımlar. Çalıştır yöntemi, **SampleEventSourceWriter** sınıfında her 10 saniyede bir uygulanan günlük yöntemlerinin her birini çağıran sonsuz bir döngü uygular.

```csharp
using Microsoft.WindowsAzure.ServiceRuntime;
using System;
using System.Diagnostics;
using System.Diagnostics.Tracing;
using System.Net;
using System.Threading;

namespace WorkerRole1
{
    sealed class SampleEventSourceWriter : EventSource
    {
        public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
        public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
        public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
        public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
        public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

    }

    enum MyColor
    {
        Red,
        Blue,
        Green
    }

    [Flags]
    enum MyFlags
    {
        Flag1 = 1,
        Flag2 = 2,
        Flag3 = 4
    }

    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
            // This is a sample worker implementation. Replace with your logic.
            Trace.TraceInformation("WorkerRole1 entry point called");

            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through the loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }
        }

        public override bool OnStart()
        {
            // Set the maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see the MSDN topic at https://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
}
```


### <a name="step-3-deploy-your-worker-role"></a>Adım 3: Çalışan Rolünüzü Dağıtın

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

1. Çözüm Gezgini'ndeki **WadExample** projesini seçerek, ardından **Yapı** menüsünden **Yayımla'yı** seçerek, çalışan rolünüzü Visual Studio'dan Azure'a dağıtın.
2. Aboneliğinizi seçin.
3. Microsoft **Azure Yayımlama Ayarları** iletişim kutusunda **Yeni Oluştur...** seçeneğini belirleyin.
4. Bulut **Hizmeti ve Depolama Hesabı Oluştur** iletişim kutusunda bir **Ad** girin (örneğin, "WadExample") girin ve bir bölge veya yakınlık grubu seçin.
5. **Ortamı** **Evreleme'ye**ayarlayın.
6. Diğer **Ayarları** uygun şekilde değiştirin ve **Yayımla'yı**tıklatın.
7. Dağıtım tamamlandıktan sonra, Azure portalında bulut hizmetinizin **Bir Çalışan** durumunda olduğunu doğrulayın.

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-the-extension"></a>Adım 4: Tanılama yapılandırma dosyanızı oluşturun ve uzantıyı yükleyin
1. Aşağıdaki PowerShell komutunu çalıştırarak ortak yapılandırma dosyası şema tanımını indirin:

    ```powershell
    (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'
    ```
2. **WorkerRole1** projesine sağ tıklayarak **İşçiRolü1** projenize bir XML dosyası ekleyin ve Yeni Öğe **Ekle'yi** -> **seçin...** -> **Görsel C# öğeleri** -> **Veri** -> **XML Dosya**. "WadExample.xml" dosyasını adlandırın.

   ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)
3. WadConfig.xsd'yi yapılandırma dosyasıyla ilişkilendirin. WadExample.xml düzenleyici penceresietkin pencere olduğundan emin olun. **Özellikler** penceresini açmak için **F4** tuşuna basın. **Özellikler** penceresindeki **Şema** özelliğini tıklatın. Tıklayın **...** **Şema'ların** mülkünde. **Ekle'yi tıklatın...** düğmesine basın ve XSD dosyasını kaydettiğiniz konuma gidin ve WadConfig.xsd dosyasını seçin. **Tamam**'a tıklayın.

4. WadExample.xml yapılandırma dosyasının içeriğini aşağıdaki XML ile değiştirin ve dosyayı kaydedin. Bu yapılandırma dosyası toplamak için bir çift performans sayaçları tanımlar: cpu kullanımı için bir ve bellek kullanımı için bir. Daha sonra yapılandırma, SampleEventSourceWriter sınıfındaki yöntemlere karşılık gelen dört olayı tanımlar.

```xml
<?xml version="1.0" encoding="utf-8"?>
<PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <WadCfg>
    <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
      <PerformanceCounters scheduledTransferPeriod="PT1M">
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
        <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
      </PerformanceCounters>
      <EtwProviders>
        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
          <Event id="1" eventDestination="EnumsTable"/>
          <Event id="2" eventDestination="MessageTable"/>
          <Event id="3" eventDestination="SetOtherTable"/>
          <Event id="4" eventDestination="HighFreqTable"/>
          <DefaultEvents eventDestination="DefaultTable" />
        </EtwEventSourceProviderConfiguration>
      </EtwProviders>
    </DiagnosticMonitorConfiguration>
  </WadCfg>
</PublicConfig>
```

### <a name="step-5-install-diagnostics-on-your-worker-role"></a>Adım 5: İşciyiz rolüne tanılama yı yükleme
Bir web veya çalışan rolünde Tanılama'yı yönetmek için PowerShell cmdlets şunlardır: Set-AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension ve Remove-AzureServiceDiagnosticsExtension.

1. Azure PowerShell'i açın.
2. İşcünüzün rolüne Tanılama'yı yüklemek için komut dosyasını çalıştırın *(StorageAccountKey'i* wadexample depolama hesabınız için depolama hesabı anahtarıyla değiştirin ve *WadExample.xml* dosyasına giden yolla *config_path)*

```powershell
$storage_name = "wadexample"
$key = "<StorageAccountKey>"
$config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
$service_name="wadexample"
$storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a>Adım 6: Telemetri verilerinize bakın
Visual Studio **Server Explorer'da**wadexample depolama hesabına gidin. Bulut hizmeti yaklaşık beş (5) dakika çalıştırdıktan sonra, tablolar **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** ve **WADSetOtherTable**görmelisiniz. Toplanan telemetriyi görüntülemek için tablolardan birini çift tıklatın.

![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)

## <a name="configuration-file-schema"></a>Yapılandırma Dosyası Şeması
Tanılama yapılandırma dosyası, tanılama aracısı başlatıldığında tanılama yapılandırma ayarlarını başlatmada kullanılan değerleri tanımlar. Geçerli değerler ve örnekler için [en son şema başvurusuna](/azure/azure-monitor/platform/diagnostics-extension-schema) bakın.

## <a name="troubleshooting"></a>Sorun giderme
Sorun yaşıyorsanız, sık karşılaşılan sorunlarla ilgili yardım için [Sorun Giderme Azure Tanılama'ya](../azure-diagnostics-troubleshooting.md) bakın.

## <a name="next-steps"></a>Sonraki Adımlar
Topladığınız verileri değiştirmek, sorunları gidermek veya genel olarak tanılama hakkında daha fazla bilgi edinmek için [ilgili Azure sanal makine tanılama makalelerinin listesine bakın.](../azure-monitor/platform/diagnostics-extension-overview.md)

[EventSource Class]: https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: https://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: https://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: https://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: https://azure.microsoft.com/documentation/articles/install-configure-powershell/



