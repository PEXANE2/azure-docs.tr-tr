---
title: Uzun Süren İşlemleri Yoklama | Microsoft Dokümanlar
description: Azure Medya Hizmetleri, işlemleri başlatmak için Medya Hizmetleri'ne istek gönderen API'ler sunar (örneğin, bir kanal oluşturmak, başlatmak, durdurmak veya silmek), bu işlemler uzun süredir çalışır. Bu konu, uzun süren işlemlerin nasıl yoklanılandığını gösterir.
services: media-services
documentationcenter: ''
author: Juliako
writer: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 43d9a6adc935010eab6e5e52d73f2019c8afcf5f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74887191"
---
# <a name="delivering-live-streaming-with-azure-media-services"></a>Azure Medya Hizmetleri ile Canlı Akış Sağlama

## <a name="overview"></a>Genel Bakış

Microsoft Azure Medya Hizmetleri, işlemleri başlatmak için Medya Hizmetleri'ne istek gönderen API'ler sunar (örneğin: bir kanal oluşturma, başlatma, durdurma veya silme). Bu operasyonlar uzun süredir devam ediyor.

Medya Hizmetleri .NET SDK, isteği gönderen ve işlemin tamamlanmasını bekleyen API'ler sağlar (dahili olarak, API'ler bazı aralıklarla operasyon ilerlemesi için yoklama yapmaktadır). Örneğin, kanalı aradiğinizde. Başlat(), kanal başladıktan sonra yöntem döndürür. Ayrıca asynchronous sürümünü kullanabilirsiniz: kanal bekliyor. StartAsync() (Görev tabanlı Asynchronous Deseni hakkında bilgi için [TAP'a](https://msdn.microsoft.com/library/hh873175\(v=vs.110\).aspx)bakın). İşlem isteği gönderen ve ardından işlem tamamlanana kadar durum için anket yapan API'ler "yoklama yöntemleri" olarak adlandırılır. Bu yöntemler (özellikle Async sürümü) zengin istemci uygulamaları ve / veya stateful hizmetler için önerilir.

Bir uygulamauzun devam eden http isteğini bekleyemediği ve işlem ilerlemesini el ile yoklamak istediği senaryolar vardır. Tipik bir örnek, devletsiz bir web hizmetiyle etkileşimde olan bir tarayıcıdır: tarayıcı bir kanal oluşturmak istediğinde, web hizmeti uzun süren bir işlem başlatır ve işlem kimliğini tarayıcıya döndürür. Tarayıcı daha sonra web hizmetinden kimlik durumuna göre işlem durumunu almasını isteyebilir. Medya Hizmetleri .NET SDK, bu senaryo için yararlı api'ler sağlar. Bu API'ler "yoklama yöntemleri" olarak adlandırılır.
"Yoklama olmayan yöntemler" aşağıdaki adlandırma deseni vardır:*OperationName*İşlemi Gönder (örneğin, SendCreateOperation). *OperationName*İşlem yöntemleri gönder **IOperation** nesnesini döndürer; döndürülen nesne, işlemi izlemek için kullanılabilecek bilgiler içerir. *OperationName'i*GönderAsync yöntemleri **Görev\<IOperation>** döndürün.

Şu anda, aşağıdaki sınıflar yoklama dışı yöntemleri destekler: **Kanal**, **StreamingEndpoint**ve **Program**.

İşlem durumunu yoklamak için **OperationBaseCollection** sınıfındaki **GetOperation** yöntemini kullanın. İşlem durumunu kontrol etmek için aşağıdaki aralıkları kullanın: **Kanal** ve **StreamingEndpoint** işlemleri için 30 saniye kullanın; **Program** işlemleri için 10 saniye kullanın.

## <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun.

## <a name="example"></a>Örnek

Aşağıdaki örnekte **Kanal İşlemleri**adlı bir sınıf tanımlanır. Bu sınıf tanımı, web hizmeti sınıf tanımınız için bir başlangıç noktası olabilir. Basitlik için aşağıdaki örnekler, yöntemlerin eşitolmayan sürümlerini kullanır.

Örnek, istemcinin bu sınıfı nasıl kullanabileceğini de gösterir.

### <a name="channeloperations-class-definition"></a>Kanalİşlemleri sınıf tanımı

```csharp
using Microsoft.WindowsAzure.MediaServices.Client;
using System;
using System.Collections.Generic;
using System.Configuration;
using System.Net;

/// <summary> 
/// The ChannelOperations class only implements 
/// the Channel’s creation operation. 
/// </summary> 
public class ChannelOperations
{
    // Read values from the App.config file.
    private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AMSAADTenantDomain"];
    private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];
    private static readonly string _AMSClientId =
        ConfigurationManager.AppSettings["AMSClientId"];
    private static readonly string _AMSClientSecret =
        ConfigurationManager.AppSettings["AMSClientSecret"];

    // Field for service context.
    private static CloudMediaContext _context = null;

    public ChannelOperations()
    {
        AzureAdTokenCredentials tokenCredentials = 
            new AzureAdTokenCredentials(_AADTenantDomain,
                new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                AzureEnvironments.AzureCloudEnvironment);

        var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

        _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);
    }

    /// <summary>  
    /// Initiates the creation of a new channel.  
    /// </summary>  
    /// <param name="channelName">Name to be given to the new channel</param>  
    /// <returns>  
    /// Operation Id for the long running operation being executed by Media Services. 
    /// Use this operation Id to poll for the channel creation status. 
    /// </returns> 
    public string StartChannelCreation(string channelName)
    {
        var operation = _context.Channels.SendCreateOperation(
            new ChannelCreationOptions
            {
                Name = channelName,
                Input = CreateChannelInput(),
                Preview = CreateChannelPreview(),
                Output = CreateChannelOutput()
            });

        return operation.Id;
    }

    /// <summary> 
    /// Checks if the operation has been completed. 
    /// If the operation succeeded, the created channel Id is returned in the out parameter.
    /// </summary> 
    /// <param name="operationId">The operation Id.</param> 
    /// <param name="channel">
    /// If the operation succeeded, 
    /// the created channel Id is returned in the out parameter.</param>
    /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
    public bool IsCompleted(string operationId, out string channelId)
    {
        IOperation operation = _context.Operations.GetOperation(operationId);
        bool completed = false;

        channelId = null;

        switch (operation.State)
        {
            case OperationState.Failed:
                // Handle the failure. 
                // For example, throw an exception. 
                // Use the following information in the exception: operationId, operation.ErrorMessage.
                break;
            case OperationState.Succeeded:
                completed = true;
                channelId = operation.TargetEntityId;
                break;
            case OperationState.InProgress:
                completed = false;
                break;
        }
        return completed;
    }

    private static ChannelInput CreateChannelInput()
    {
        return new ChannelInput
        {
            StreamingProtocol = StreamingProtocol.RTMP,
            AccessControl = new ChannelAccessControl
            {
                IPAllowList = new List<IPRange>
                    {
                        new IPRange
                        {
                            Name = "TestChannelInput001",
                            Address = IPAddress.Parse("0.0.0.0"),
                            SubnetPrefixLength = 0
                        }
                    }
            }
        };
    }

    private static ChannelPreview CreateChannelPreview()
    {
        return new ChannelPreview
        {
            AccessControl = new ChannelAccessControl
            {
                IPAllowList = new List<IPRange>
                    {
                        new IPRange
                        {
                            Name = "TestChannelPreview001",
                            Address = IPAddress.Parse("0.0.0.0"),
                            SubnetPrefixLength = 0
                        }
                    }
            }
        };
    }

    private static ChannelOutput CreateChannelOutput()
    {
        return new ChannelOutput
        {
            Hls = new ChannelOutputHls { FragmentsPerSegment = 1 }
        };
    }
}
```

### <a name="the-client-code"></a>İstemci kodu

```csharp
ChannelOperations channelOperations = new ChannelOperations();
string opId = channelOperations.StartChannelCreation("MyChannel001");

string channelId = null;
bool isCompleted = false;

while (isCompleted == false)
{
    System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
    isCompleted = channelOperations.IsCompleted(opId, out channelId);
}

// If we got here, we should have the newly created channel id.
Console.WriteLine(channelId);
```

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

