---
title: Kimlik doğrulaması için Azure ön uç API 'Leri
description: Kimlik doğrulaması için C# ön uç API 'sinin nasıl kullanılacağını açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/12/2010
ms.topic: how-to
ms.openlocfilehash: 04296a3dab61fdb569126abc1bc1f975d69e226d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80681356"
---
# <a name="use-the-azure-frontend-apis-for-authentication"></a>Kimlik doğrulaması için Azure Ön Uç API’lerini kullanma

Bu bölümde, kimlik doğrulaması için C# API 'SININ nasıl kullanılacağını anlatmaktadır.

## <a name="azurefrontendaccountinfo"></a>Azurefrontendaccountınfo

Azurefrontendaccountınfo SDK 'daki bir ```AzureFrontend``` örnek için kimlik doğrulama bilgilerini ayarlamak üzere kullanılır.

Önemli alanlar şunlardır:

```cs

    public class AzureFrontendAccountInfo
    {
        // Something akin to "<region>.mixedreality.azure.com"
        public string AccountDomain;

        // Can use one of:
        // 1) ID and Key.
        // 2) AuthenticationToken.
        // 3) AccessToken.
        public string AccountId = Guid.Empty.ToString();
        public string AccountKey = string.Empty;
        public string AuthenticationToken = string.Empty;
        public string AccessToken = string.Empty;
    }

```

Etki alanındaki _bölge_ bölümü için, [size yakın bir bölge](../reference/regions.md)kullanın.

Hesap bilgileri, [Hesap bilgilerini alma](create-an-account.md#retrieve-the-account-information) paragrafında açıklandığı gibi portaldan elde edilebilir.

## <a name="azure-frontend"></a>Azure ön ucu

İlgili sınıflar ve ' ```AzureFrontend``` ```AzureSession```dir. ```AzureFrontend```, aşağıdakiler dahil olmak üzere hesap yönetimi ve hesap düzeyi işlevsellik için kullanılır: varlık dönüştürme ve işleme oturumu oluşturma. ```AzureSession```, oturum düzeyi işlevselliği için kullanılır ve şunları içerir: oturum güncelleştirme, sorgular, yenileme ve kullanımdan kaldırma.

Her açılan/oluşturulan ```AzureSession``` , oluşturulduğu ön uç için bir başvuru tutacaktır. Temiz bir şekilde kapatmak için, ön uç serbest bırakılmadan önce tüm oturumların serbest bırakılmalıdır.

Bir oturumun ayırmayı kaldırma işlemi, Azure 'daki VM 'yi durdurmaz, `AzureSession.StopAsync` açıkça çağrılmalıdır.

Bir oturum oluşturulduktan ve durumu hazırlanıyor olarak işaretlendiğinde, ile `AzureSession.ConnectToRuntime`uzaktan işleme çalışma zamanına bağlanabilir.

### <a name="threading"></a>İş Parçacığı Oluşturma

Tüm AzureSession ve AzureFrontend zaman uyumsuz çağrıları, ana uygulama iş parçacığında değil, bir arka plan iş parçacığında tamamlanır.

### <a name="conversion-apis"></a>Dönüştürme API 'Leri

Dönüştürme hizmeti hakkında daha fazla bilgi için bkz. [model dönüştürme REST API](conversion/conversion-rest-api.md).

#### <a name="start-asset-conversion"></a>Varlık dönüştürmeyi Başlat

``` cs
private StartConversionAsync _pendingAsync = null;

void StartAssetConversion(AzureFrontend frontend, string modelName, string modelUrl, string assetContainerUrl)
{
    _pendingAsync = frontend.StartConversionAsync(
        new AssetConversionParams(modelName, modelUrl, assetContainerUrl));
    _pendingAsync.Completed +=
        (StartConversionAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                //use res.Result
            }
            else
            {
                Console.WriteLine("Failed to start asset conversion!");
            }
        };

        _pendingAsync = null;
}
```

#### <a name="get-conversion-status"></a>Dönüştürme durumunu al

``` cs
private ConversionStatusAsync _pendingAsync = null
void GetConversionStatus(AzureFrontend frontend, string assetId)
{
    _pendingAsync = frontend.GetConversionStatusAsync(assetId);
    _pendingAsync.Completed +=
        (ConversionStatusAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                //use res.Result
            }
            else
            {
                Console.WriteLine("Failed to get status of asset conversion!");
            }

            _pendingAsync = null;
        };
}
```

### <a name="rendering-apis"></a>Oluşturma API 'Leri

Oturum yönetimi hakkındaki ayrıntılar için [oturum yönetimi REST API](session-rest-api.md) bakın.

Bir işleme oturumu, hizmette dinamik olarak oluşturulabilir veya zaten mevcut bir oturum KIMLIĞI, bir AzureSession nesnesi içinde ' açılabilir ' olabilir.

#### <a name="create-rendering-session"></a>İşleme oturumu oluştur

``` cs
private CreateSessionAsync _pendingAsync = null;
void CreateRenderingSession(AzureFrontend frontend, RenderingSessionVmSize vmSize, ARRTimeSpan maxLease)
{
    _pendingAsync = frontend.CreateNewRenderingSessionAsync(
        new RenderingSessionCreationParams(vmSize, maxLease));

    _pendingAsync.Completed +=
        (CreateSessionAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                //use res.Result
            }
            else
            {
                Console.WriteLine("Failed to create session!");
            }
            _pendingAsync = null;
        };
}
```

#### <a name="open-an-existing-rendering-session"></a>Mevcut bir işleme oturumunu açın

Mevcut bir oturumu açmak zaman uyumlu bir çağrıdır.

``` cs
void CreateRenderingSession(AzureFrontend frontend, string sessionId)
{
    AzureSession session = frontend.OpenRenderingSession(sessionId);
    // Query session status, etc.
}
```

#### <a name="get-current-rendering-sessions"></a>Geçerli işleme oturumlarını al

``` cs
private SessionPropertiesArrayAsync _pendingAsync = null;
void GetCurrentRenderingSessions(AzureFrontend frontend)
{
    _pendingAsync = frontend.GetCurrentRenderingSessionsAsync();
    _pendingAsync.Completed +=
        (SessionPropertiesArrayAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                //use res.Result
            }
            else
            {
                Console.WriteLine("Failed to get current rendering sessions!");
            }
            _pendingAsync = null;
        };
}
```

### <a name="session-apis"></a>Oturum API 'Leri

#### <a name="get-rendering-session-properties"></a>İşleme oturumu özelliklerini al

``` cs
private SessionPropertiesAsync _pendingAsync = null;
void GetRenderingSessionProperties(AzureSession session)
{
    _pendingAsync = session.GetPropertiesAsync();
    _pendingAsync.Completed +=
        (SessionPropertiesAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                //use res.Result
            }
            else
            {
                Console.WriteLine("Failed to get properties of session!");
            }
            _pendingAsync = null;
        };
}
```

#### <a name="update-rendering-session"></a>İşleme oturumunu Güncelleştir

``` cs
private SessionAsync _pendingAsync;
void UpdateRenderingSession(AzureSession session, ARRTimeSpan updatedLease)
{
    _pendingAsync = session.RenewAsync(
        new RenderingSessionUpdateParams(updatedLease));
    _pendingAsync.Completed +=
        (SessionAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                Console.WriteLine("Rendering session renewed succeeded!");
            }
            else
            {
                Console.WriteLine("Failed to renew rendering session!");
            }
            _pendingAsync = null;
        };
}
```

#### <a name="stop-rendering-session"></a>İşleme oturumunu durdur

``` cs
private SessionAsync _pendingAsync;
void StopRenderingSession(AzureSession session)
{
    _pendingAsync = session.StopAsync();
    _pendingAsync.Completed +=
        (SessionAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                Console.WriteLine("Rendering session stopped successfully!");
            }
            else
            {
                Console.WriteLine("Failed to stop rendering session!");
            }
            _pendingAsync = null;
        };
}
```

#### <a name="connect-to-arr-inspector"></a>ARR denetçisine bağlanma

``` cs
private ArrInspectorAsync _pendingAsync = null;
void ConnectToArrInspector(AzureSession session, string hostname)
{
    _pendingAsync = session.ConnectToArrInspectorAsync(hostname);
    _pendingAsync.Completed +=
        (ArrInspectorAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                // Launch the html file with default browser
                string htmlPath = res.Result;
#if WINDOWS_UWP
                UnityEngine.WSA.Application.InvokeOnUIThread(async () =>
                {
                    var file = await Windows.Storage.StorageFile.GetFileFromPathAsync(htmlPath);
                    await Windows.System.Launcher.LaunchFileAsync(file);
                }, true);
#else
                InvokeOnAppThreadAsync(() =>
                {
                    System.Diagnostics.Process.Start("file:///" + htmlPath);
                });
#endif
            }
            else
            {
                Console.WriteLine("Failed to connect to ARR inspector!");
            }
        };
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Hesap oluşturma](create-an-account.md)
* [Örnek PowerShell betikleri](../samples/powershell-example-scripts.md)
