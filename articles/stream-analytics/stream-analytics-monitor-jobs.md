---
title: Azure Akış Analizi işlerini programlı olarak izleyin ve yönetin
description: Bu makalede, REST API'leri, Azure SDK veya PowerShell aracılığıyla oluşturulan Akış Analizi işlerinin nasıl programlanabilir bir şekilde izlendiği açıklanmaktadır.
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/20/2017
ms.openlocfilehash: 23c0cc0d0e4a007fdf46021f857b559266f6a193
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75431663"
---
# <a name="programmatically-create-a-stream-analytics-job-monitor"></a>Programlı bir Akış Analizi iş monitörü oluşturun

Bu makalede, Bir Stream Analytics işi için izlemenin nasıl etkinleştirilen gösteriş. REST API'leri, Azure SDK veya PowerShell aracılığıyla oluşturulan Akış Analizi işleri varsayılan olarak izlemeyi etkinleştirmez. İşi Izleyenler sayfasına giderek ve Etkinleştir düğmesini tıklatarak Azure portalında el ile etkinleştirebilir veya bu makaledeki adımları izleyerek bu işlemi otomatikleştirebilirsiniz. İzleme verileri, Akış Analizi işiniz için Azure portalının Ölçümler alanında gösterecektir.

## <a name="prerequisites"></a>Ön koşullar

Bu işleme başlamadan önce aşağıdaki ön koşullara sahip olmalısınız:

* Visual Studio 2019 veya 2015
* [Azure .NET SDK](https://azure.microsoft.com/downloads/) indirildi ve yüklendi
* İzlemenin etkinleştirilmesi gereken varolan bir Stream Analytics işi

## <a name="create-a-project"></a>Proje oluşturma

1. Visual Studio C# .NET konsol uygulaması oluşturun.
2. Paket Yöneticisi Konsolunda, NuGet paketlerini yüklemek için aşağıdaki komutları çalıştırın. Bunlardan ilki Azure Akış Analizi Yönetimi .NET SDK'dır. İkincisi, izlemeyi etkinleştirmek için kullanılacak Azure Monitor SDK'dır. Sonuncusu, kimlik doğrulaması için kullanılacak Azure Etkin Dizin istemcisidir.
   
   ```powershell
   Install-Package Microsoft.Azure.Management.StreamAnalytics
   Install-Package Microsoft.Azure.Insights -Pre
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```
3. App.config dosyasına aşağıdaki appAyarlar bölümünü ekleyin.
   
   ```csharp
   <appSettings>
     <!--CSM Prod related values-->
     <add key="ResourceGroupName" value="RESOURCE GROUP NAME" />
     <add key="JobName" value="YOUR JOB NAME" />
     <add key="StorageAccountName" value="YOUR STORAGE ACCOUNT"/>
     <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
     <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
     <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
     <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
     <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
     <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION ID" />
     <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
   </appSettings>
   ```
   *SubscriptionId* ve *ActiveDirectoryTenantId* değerlerini Azure aboneliğiniz ve kiracı kimlikleriniz ile değiştirin. Aşağıdaki PowerShell cmdlet çalıştırarak bu değerleri elde edebilirsiniz:
   
   ```powershell
   Get-AzureAccount
   ```
4. Aşağıdaki using bildirimlerini projedeki kaynak dosyasına (Program.cs) ekleyin.
   
   ```csharp
     using System;
     using System.Configuration;
     using System.Threading;
     using Microsoft.Azure;
     using Microsoft.Azure.Management.Insights;
     using Microsoft.Azure.Management.Insights.Models;
     using Microsoft.Azure.Management.StreamAnalytics;
     using Microsoft.Azure.Management.StreamAnalytics.Models;
     using Microsoft.IdentityModel.Clients.ActiveDirectory;
   ```
5. Kimlik doğrulama yardımcı yöntemi ekleyin.

   ```csharp   
   public static string GetAuthorizationHeader()
   {
      AuthenticationResult result = null;
      var thread = new Thread(() =>
      {
         try
         {
             var context = new AuthenticationContext(
                ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
             result = context.AcquireToken(
                 resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                 clientId: ConfigurationManager.AppSettings["AsaClientId"],
                 redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                 promptBehavior: PromptBehavior.Always);
         }
         catch (Exception threadEx)
         {
             Console.WriteLine(threadEx.Message);
         }
     });

     thread.SetApartmentState(ApartmentState.STA);
     thread.Name = "AcquireTokenThread";
     thread.Start();
     thread.Join();
   
     if (result != null)
     {
         return result.AccessToken;
     }
         throw new InvalidOperationException("Failed to acquire token");
   }
   ```

## <a name="create-management-clients"></a>Yönetim istemcileri oluşturma

Aşağıdaki kod gerekli değişkenleri ve yönetim istemcilerini ayarlar.

   ```csharp
    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    Uri resourceManagerUri = new
    Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    // Create Stream Analytics and Insights management client
    StreamAnalyticsManagementClient streamAnalyticsClient = new
    StreamAnalyticsManagementClient(aadTokenCredentials, resourceManagerUri);
    InsightsManagementClient insightsClient = new
    InsightsManagementClient(aadTokenCredentials, resourceManagerUri);
   ```

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a>Varolan bir Stream Analytics işi için izlemeyi etkinleştirin

Aşağıdaki kod, **varolan** bir Akış Analizi işinin izlenmesini sağlar. Kodun ilk bölümü, belirli Stream Analytics işi hakkında bilgi almak için Stream Analytics hizmetine karşı bir GET isteği gerçekleştirir. Kodun ikinci yarısında Put yöntemi için bir parametre olarak *kimlik* özelliğini (GET isteğinden alınan) kullanır ve Bu da Akış Analizi işinin izlenmesini etkinleştirmek için Insights hizmetine bir PUT isteği gönderir.

> [!WARNING]
> Azure portalı üzerinden veya aşağıdaki kod aracılığıyla farklı bir Akış Analizi işi için izlemeyi daha **önce etkinleştirdiyseniz, daha önce izlemeyi etkinleştirdiğinizde kullandığınız depolama hesabı adını sağlamanızı öneririz.**
> 
> Depolama hesabı, Akış Analizi işinizi oluşturduğunuz bölgeye bağlıdır, özellikle işin kendisine değil.
> 
> Aynı bölgedeki tüm Akış Analizi işleri (ve diğer tüm Azure kaynakları) izleme verilerini depolamak için bu depolama hesabını paylaşır. Farklı bir depolama hesabı sağlarsanız, diğer Akış Analizi işlerinizi veya diğer Azure kaynaklarının izlenmesinde istenmeyen yan etkilere neden olabilir.
> 
> Aşağıdaki kodda değiştirmek `<YOUR STORAGE ACCOUNT NAME>` için kullandığınız depolama hesabı adı, izlemeyi etkinleştirdiğiniz Akış Analizi işiyle aynı abonelikte bulunan bir depolama hesabı olmalıdır.
> 
> 
>    ```csharp
>    // Get an existing Stream Analytics job
>     JobGetParameters jobGetParameters = new JobGetParameters()
>     {
>         PropertiesToExpand = "inputs,transformation,outputs"
>     };
>     JobGetResponse jobGetResponse = streamAnalyticsClient.StreamingJobs.Get(resourceGroupName, streamAnalyticsJobName, jobGetParameters);
>
>    // Enable monitoring
>    ServiceDiagnosticSettingsPutParameters insightPutParameters = new ServiceDiagnosticSettingsPutParameters()
>    {
>            Properties = new ServiceDiagnosticSettings()
>           {
>               StorageAccountName = "<YOUR STORAGE ACCOUNT NAME>"
>           }
>    };
>   insightsClient.ServiceDiagnosticSettingsOperations.Put(jobGetResponse.Job.Id, insightPutParameters);
>   ```


## <a name="get-support"></a>Destek alın

Daha fazla yardım için [Azure Akışı Analizi forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Akış Analizine Giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
