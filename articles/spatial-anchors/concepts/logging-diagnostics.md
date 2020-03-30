---
title: Günlüğe kaydetme ve tanılama
description: Azure Uzamsal Çapalar'da günlüğe kaydetme ve tanılamanın nasıl oluşturacağı ve alınacağı hakkında ayrıntılı açıklama.
author: ramonarguelles
manager: vriveras
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/22/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: f4359db1deda2295a66bcb97cf374d0fe9bc3ef7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74270122"
---
# <a name="logging-and-diagnostics-in-azure-spatial-anchors"></a>Azure Uzamsal Çapalarda günlüğe kaydetme ve tanılama

Azure Uzamsal Çapalar, uygulama geliştirme için yararlı olan standart bir günlük mekanizması sağlar. Uzamsal Çapalar tanılama günlüğü modu, hata ayıklama için daha fazla bilgiye ihtiyaç duyduğunuzda yararlıdır. Tanılama günlüğü, ortamın görüntülerini depolar.

## <a name="standard-logging"></a>Standart günlüğe kaydetme
Uzamsal ÇapaLAR API'sinde, uygulama geliştirme ve hata ayıklama için yararlı günlükleri almak için günlük mekanizmasına abone olabilirsiniz. Standart günlük API'leri aygıt diskinde ortamın resimlerini depolamaz. SDK bu günlükleri olay geri çağırmaları olarak sağlar. Bu günlükleri uygulamanın günlük mekanizmasına entegre etmek size kalmış.

### <a name="configuration-of-log-messages"></a>Günlük iletilerinin yapılandırması
Kullanıcı için ilgi çekici iki geri arama vardır. Aşağıdaki örnek, oturumun nasıl yapılandırılabildiğini gösterir.

```csharp
    cloudSpatialAnchorSession = new CloudSpatialAnchorSession();
    . . .
    // set up the log level for the runtime session
    cloudSpatialAnchorSession.LogLevel = SessionLogLevel.Information;

    // configure the callback for the debug log
    cloudSpatialAnchorSession.OnLogDebug += CloudSpatialAnchorSession_OnLogDebug;

    // configure the callback for the error log
    cloudSpatialAnchorSession.Error += CloudSpatialAnchorSession_Error;
```

### <a name="events-and-properties"></a>Etkinlikler ve özellikler

Bu olay geri aramaları, oturumgünlüklerini ve hataları işlemek için sağlanır:

- [LogLevel](https://docs.microsoft.com/dotnet/api/microsoft.azure.spatialanchors.cloudspatialanchorsession.loglevel): Çalışma zamanından itibaren alacak olaylar için ayrıntı düzeyini belirtir.
- [OnLogDebug](https://docs.microsoft.com/dotnet/api/microsoft.azure.spatialanchors.cloudspatialanchorsession.onlogdebug): Standart hata ayıklama günlüğü olayları sağlar.
- [Hata](https://docs.microsoft.com/dotnet/api/microsoft.azure.spatialanchors.cloudspatialanchorsession.error): Çalışma zamanının hata olarak gördüğü günlük olayları sağlar.

## <a name="diagnostics-logging"></a>Tanılama günlüğü

Günlük için standart çalışma moduna ek olarak, Uzamsal Çapalar da bir tanılama modu vardır. Tanılama modu ortamın görüntülerini yakalar ve diske kaydeder. Bu modu, bir çapayı tahmin edilebileceği gibi belirli türdeki sorunları hata ayıklamak için kullanabilirsiniz. Yalnızca belirli bir sorunu çoğaltmak için tanılama günlüğe kaydetmeyi etkinleştirin. Sonra devre dışı k. Uygulamalarınızı normal bir şekilde çalıştırırken tanılamayı etkinleştirmeyin.

Microsoft ile bir destek etkileşimi sırasında, bir Microsoft temsilcisi daha fazla araştırma için bir tanılama paketi göndermek isteyip istemediğinizi sorabilir. Bu durumda, tanılamayı etkinleştirmeye ve tanılama paketini gönderebilmeniz için sorunu yeniden oluşturmaya karar verebilirsiniz.

Bir Microsoft temsilcisinin önceden onayını almadan Microsoft'a bir tanılama günlüğü gönderirseniz, gönderim yanıtsız kalır.

Aşağıdaki bölümlerde tanılama modunun nasıl etkinleştirilen ve ayrıca tanılama günlüklerinin Microsoft'a nasıl gönderilen gösterin.

### <a name="enable-diagnostics-logging"></a>Tanılama günlüğünü etkinleştirme

Tanılama günlüğe kaydetme için bir oturumu etkinleştirdiğinizde, oturumdaki tüm işlemler, yerel dosya sisteminde karşılık gelen tanılama günlüğe kaydetmeye sahiptir. Günlüğe kaydetme sırasında, ortam görüntüleri diske kaydedilir.

```csharp
private void ConfigureSession()
{
    cloudSpatialAnchorSession = new CloudSpatialAnchorSession();
    . . .

    // set up the log level for the runtime session
    cloudSpatialAnchorSession.LogLevel = SessionLogLevel.Information;

    // configure the callbacks for logging and errors
    cloudSpatialAnchorSession.OnLogDebug += CloudSpatialAnchorSession_OnLogDebug;
    cloudSpatialAnchorSession.Error += CloudSpatialAnchorSession_Error;

    // opt in to diagnostics logging of environment images
    // if this is enabled, the diagnostics bundle includes images of the environment captured by the session
    cloudSpatialAnchorSession.Diagnostics.ImagesEnabled = true;

    // set the level of detail to be collected in the diagnostics log by the session
    cloudSpatialAnchorSession.Diagnostics.LogLevel = SessionLogLevel.All;

    // set the max bundle size to capture the bug information
    cloudSpatialAnchorSession.Diagnostics.MaxDiskSizeInMB = 200;
    . . .
}
```

### <a name="submit-the-diagnostics-bundle"></a>Tanılama paketini gönderme

Aşağıdaki kod parçacığı, tanılama paketinin Microsoft'a nasıl gönderilebildiğini gösterir. Bu paket, tanılamayı etkinleştirdikten sonra oturum tarafından yakalanan ortamın görüntülerini içerir.

```csharp
// method to handle the diagnostics bundle submission
private async Task CreateAndSubmitBundle()
{
    // create the diagnostics bundle manifest to collect the session information
    string path = await cloudSpatialAnchorSession
                              .Diagnostics
                              .CreateManifestAsync("Description of the issue");

    // submit the manifest and data to send feedback to Microsoft
    await cloudSpatialAnchorSession.Diagnostics.SubmitManifestAsync(path);
}
```

### <a name="parts-of-a-diagnostics-bundle"></a>Tanılama paketinin parçaları
Tanılama paketi aşağıdaki bilgileri içerebilir:

- **Anahtar kare görüntüleri**: Tanılama etkinken oturum sırasında yakalanan ortamın görüntüleri.
- **Günlükler**: Çalışma süresine göre kaydedilen olayları günlüğe kaydedin.
- **Oturum meta verileri**: Oturumu tanımlayan meta veriler.
