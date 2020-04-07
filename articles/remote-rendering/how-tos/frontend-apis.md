---
title: Kimlik doğrulaması için Azure Frontend API'leri
description: Kimlik doğrulaması için C# frontend API'nin nasıl kullanılacağını açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/12/2010
ms.topic: how-to
ms.openlocfilehash: 04296a3dab61fdb569126abc1bc1f975d69e226d
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681356"
---
# <a name="use-the-azure-frontend-apis-for-authentication"></a>Kimlik doğrulama için Azure Frontend API'lerini kullanma

Bu bölümde, kimlik doğrulaması için C# API'nin nasıl kullanılacağıaçıklanacaktır.

## <a name="azurefrontendaccountinfo"></a>AzureFrontendAccountInfo

AzureFrontendAccountInfo, SDK'daki bir ```AzureFrontend``` örnek için kimlik doğrulama bilgilerini ayarlamak için kullanılır.

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

Etki alanında _bölge_ bölümü [için, size yakın](../reference/regions.md)bir bölge kullanın.

Hesap [bilgileri, hesap bilgileri al](create-an-account.md#retrieve-the-account-information) paragrafında açıklandığı şekilde portaldan elde edilebilir.

## <a name="azure-frontend"></a>Azure Ön Uç

İlgili sınıflar ```AzureFrontend``` ve ```AzureSession```. ```AzureFrontend```varlık dönüştürme ve oturum oluşturma oluşturma yı içeren hesap yönetimi ve hesap düzeyi işlevselliği için kullanılır. ```AzureSession```oturum düzeyi işlevselliği için kullanılır ve şunları içerir: oturum güncelleştirme, sorgular, yenileme ve devre dışı bırakma.

Açılan/oluşturulan ```AzureSession``` her biri, onu oluşturan ön uça bir başvuru tutar. Temiz bir şekilde kapanmak için, ön uç ayrılmadan önce tüm oturumların ayrılması gerekir.

Oturumun ayrılması Azure'daki VM'yi `AzureSession.StopAsync` durdurmaz, açıkça çağrılmalıdır.

Bir oturum oluşturulduktan ve durumu hazır olarak işaretlendikten sonra, uzaktan işleme `AzureSession.ConnectToRuntime`çalışma süresine ' le bağlanabilir.

### <a name="threading"></a>İş Parçacığı Oluşturma

Tüm AzureSession ve AzureFrontend async çağrıları ana uygulama iş parçacığında değil, arka plan iş parçacığında tamamlanır.

### <a name="conversion-apis"></a>Dönüşüm API'leri

Dönüşüm hizmeti hakkında daha fazla bilgi [için, model dönüşümü REST API'ye](conversion/conversion-rest-api.md)bakın.

#### <a name="start-asset-conversion"></a>Varlık dönüştürmeyi başlat

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

#### <a name="get-conversion-status"></a>Dönüşüm durumu alma

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

### <a name="rendering-apis"></a>Görüntüleme API'leri

Oturum yönetimi hakkında ayrıntılı bilgi için [oturum yönetimi REST API'ye](session-rest-api.md) bakın.

Bir işleme oturumu hizmette dinamik olarak oluşturulabilir veya zaten varolan bir oturum kimliği AzureSession nesnesine 'açılabilir'.

#### <a name="create-rendering-session"></a>Oluşturma oturumu oluşturma

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

#### <a name="open-an-existing-rendering-session"></a>Varolan bir işleme oturumunu açma

Varolan bir oturumu açmak eşzamanlı bir çağrıdır.

``` cs
void CreateRenderingSession(AzureFrontend frontend, string sessionId)
{
    AzureSession session = frontend.OpenRenderingSession(sessionId);
    // Query session status, etc.
}
```

#### <a name="get-current-rendering-sessions"></a>Geçerli görüntüleme oturumları alma

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

### <a name="session-apis"></a>Oturum API'ları

#### <a name="get-rendering-session-properties"></a>Oluşturma oturumu özelliklerini alma

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

#### <a name="update-rendering-session"></a>Oluşturma oturumunu güncelleştirme

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

#### <a name="stop-rendering-session"></a>Oturum oluşturmayı durdurma

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

#### <a name="connect-to-arr-inspector"></a>ARR müfettişine bağlanın

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
* [Örnek PowerShell komut dosyaları](../samples/powershell-example-scripts.md)
