---
title: Bir uygulamayla yönetilen kimliği kullanma
description: Azure Hizmetleri'ne erişmek için Azure Hizmet Dokusu uygulama kodunda yönetilen kimlikler nasıl kullanılır? Bu özellik genel önizleme aşamasındadır.
ms.topic: article
ms.date: 10/09/2019
ms.openlocfilehash: 59680ec7911f55c3dc49d8834b410a039aa435dc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75610327"
---
# <a name="how-to-leverage-a-service-fabric-applications-managed-identity-to-access-azure-services-preview"></a>Azure hizmetlerine erişmek için Service Fabric uygulamasının yönetilen kimliğinasıl kullanılır (önizleme)

Service Fabric uygulamaları, Azure Etkin Dizin tabanlı kimlik doğrulamasını destekleyen diğer Azure kaynaklarına erişmek için yönetilen kimliklerden yararlanabilir. Bir uygulama, sistem tarafından atanmış veya kullanıcı tarafından atanmış olabilecek kimliğini temsil eden bir [erişim belirteci](../active-directory/develop/developer-glossary.md#access-token) alabilir ve kendisini başka bir hizmete doğrulamak için 'taşıyıcı' belirteci olarak kullanabilir - [korumalı kaynak sunucusu](../active-directory/develop/developer-glossary.md#resource-server)olarak da bilinir. Belirteç, Hizmet Dokusu uygulamasına atanan kimliği temsil eder ve yalnızca bu kimliği paylaşan Azure kaynaklarına (SF uygulamaları dahil) verilir. Yönetilen kimliklerin ayrıntılı bir açıklamasının yanı sıra sistem tarafından atanan ve kullanıcı tarafından atanan kimlikler arasındaki ayrım için [yönetilen kimliğe genel bakış](../active-directory/managed-identities-azure-resources/overview.md) belgelerine bakın. Bu makale boyunca yönetilen kimlik etkinleştirilmiş Hizmet Dokusu uygulamasını [istemci uygulaması](../active-directory/develop/developer-glossary.md#client-application) olarak adlandıracağız.

> [!IMPORTANT]
> Yönetilen kimlik, kaynağı içeren abonelikle ilişkili ilgili Azure AD kiracısında bir Azure kaynağı ile hizmet sorumlusu arasındaki ilişkiyi temsil eder. Bu nedenle, Hizmet Dokusu bağlamında yönetilen kimlikler yalnızca Azure kaynağı olarak dağıtılan uygulamalar için desteklenir. 

> [!IMPORTANT]
> Hizmet Kumaşı uygulamasının yönetilen kimliğini kullanmadan önce, istemci uygulamasına korumalı kaynağa erişim izni verilmelidir. Destek için denetim yapmak için [Azure AD kimlik doğrulamasını destekleyen Azure hizmetleri](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-managed-identities-for-azure-resources) listesine ve ilgi çekici kaynaklara kimlik erişimi sağlamak için belirli adımlar için ilgili hizmetin belgelerine bakın. 

## <a name="acquiring-an-access-token-using-rest-api"></a>REST API'yi kullanarak erişim belirteci edinme
Yönetilen kimlik için etkinleştirilen kümelerde, Hizmet Dokusu çalışma süresi, uygulamaların erişim belirteçleri elde etmek için kullanabileceği yerel bir ana bilgisayar bitiş noktasını ortaya çıkarır. Bitiş noktası kümenin her düğümünde kullanılabilir ve bu düğümdeki tüm varlıklar tarafından erişilebilir. Yetkili arayanlar bu bitiş noktasını arayarak ve bir kimlik doğrulama kodu sunarak erişim belirteçleri edinebilirler; kod, her farklı hizmet kodu paketi etkinleştirme için Hizmet Dokusu tarafından oluşturulur ve bu hizmet kodu paketini barındıran işlemin ömrüne bağlıdır.

Özellikle, yönetilen kimlik etkin Hizmet Kumaşı hizmetinin ortamı aşağıdaki değişkenlerle tohumlanacaktır:
- 'MSI_ENDPOINT': yol, API sürümü ve bu hizmetin yönetilen kimliğine karşılık gelen parametrelerle birlikte localhost bitiş noktası
- 'MSI_SECRET': opak bir dize olan ve geçerli düğümdeki hizmeti benzersiz bir şekilde temsil eden bir kimlik doğrulama kodu

> [!NOTE]
> 'MSI_ENDPOINT' ve 'MSI_SECRET' adları, yönetilen kimliklerin önceki atamasını ("Yönetilen Hizmet Kimliği") ifade eder ve şimdi de amortismana uyrabilir. Adlar, yönetilen kimlikleri destekleyen diğer Azure hizmetleri tarafından kullanılan eşdeğer ortam değişken adları ile de tutarlıdır.

> [!IMPORTANT]
> Uygulama kodu 'MSI_SECRET' ortam değişkeninin değerini hassas veriler olarak dikkate almalıdır - günlüğe kaydedilmemeli veya başka bir şekilde dağıtılmamalıdır. Kimlik doğrulama kodunun yerel düğüm dışında veya hizmeti barındıran işlem sona erdikten sonra bir değeri yoktur, ancak Hizmet Kumaşı hizmetinin kimliğini temsil eder ve bu nedenle erişim belirteciyle aynı önlemlerle tedavi edilmelidir.

Bir belirteç elde etmek için istemci aşağıdaki adımları gerçekleştirir:
- yönetilen kimlik bitiş noktasını (MSI_ENDPOINT değeri) API sürümü ve belirteç için gerekli olan kaynak (hedef kitle) ile bir araya getirmek tarafından bir URI oluşturur
- belirtilen URI için get http isteği oluşturur
- istek için üstbilgi olarak kimlik doğrulama kodunu (MSI_SECRET değeri) ekler
- isteği gönderir

Başarılı bir yanıt, ortaya çıkan erişim belirteci temsil eden bir JSON yükü yanı sıra onu açıklayan meta veri içerir. Başarısız bir yanıt da hata bir açıklama içerecektir. Hata işleme ile ilgili ek ayrıntılar için aşağıya bakın.

Erişim belirteçleri Çeşitli düzeylerde (düğüm, küme, kaynak sağlayıcı hizmeti) Service Fabric tarafından önbelleğe alınacaktır, bu nedenle başarılı bir yanıt, belirteci kullanıcı uygulamasının isteğine yanıt olarak doğrudan verildiği anlamına gelmez. Belirteçler kullanım ömürlerinden daha kısa bir süre için önbelleğe alınacaktır ve böylece geçerli bir jeton alacağı garanti edilir. Uygulama kodunun kendisine edindığı herhangi bir erişim belirteçlerini önbelleğe alması önerilir; önbelleğe alma anahtarı izleyiciyi içermelidir (bir türetme). 


Örnek istek:
```http
GET 'http://localhost:2377/metadata/identity/oauth2/token?api-version=2019-07-01-preview&resource=https://keyvault.azure.com/' HTTP/1.1 Secret: 912e4af7-77ba-4fa5-a737-56c8e3ace132
```
burada:

| Öğe | Açıklama |
| ------- | ----------- |
| `GET` | Son noktadan veri almak istediğinizi belirten HTTP fiili. Bu durumda, bir OAuth erişim belirteci. | 
| `http://localhost:2377/metadata/identity/oauth2/token` | MSI_ENDPOINT ortam değişkeni üzerinden sağlanan Service Fabric uygulamaları için yönetilen kimlik bitiş noktası. |
| `api-version` | Yönetilen Kimlik Belirteç Hizmeti'nin API sürümünü belirten bir sorgu dize parametresi; şu anda kabul `2019-07-01-preview`edilen tek değerdir ve değiştirilebilir. |
| `resource` | Hedef kaynağın App ID URI'sini gösteren bir sorgu dize parametresi. Bu, verilen belirteci `aud` (hedef kitle) iddiası olarak yansıtılacaktır. Bu örnek, App ID URI https olan Azure Key Vault'a erişmek için bir belirteç talep ediyor:\//keyvault.azure.com/. |
| `Secret` | Arayan kişinin kimliğini doğrulamak için Hizmet Kumaşı Yönetilen Kimlik Belirteci Hizmeti tarafından gerekli olan BIR HTTP istek üstbilgi alanı. Bu değer, sf çalışma süresi tarafından MSI_SECRET ortam değişkeni üzerinden sağlanır. |


Örnek yanıt:
```json
HTTP/1.1 200 OK
Content-Type: application/json
{
    "token_type":  "Bearer",
    "access_token":  "eyJ0eXAiO...",
    "expires_on":  1565244611,
    "resource":  "https://keyvault.azure.com/"
}
```
burada:

| Öğe | Açıklama |
| ------- | ----------- |
| `token_type` | Belirteç türü; Bu durumda, bu belirteç sunucu ('taşıyıcı') anlamına gelir bir "Taşıyıcı" erişim belirteci, belirteci amaçlanan konudur. |
| `access_token` | İstenen erişim jetonu. Güvenli bir REST API'yi ararken, belirteç `Authorization` istek üstbilgi alanına "taşıyıcı" belirteci olarak katıştırılmış ve API'nin arayanın kimliğini doğrulaması sağlar. | 
| `expires_on` | Erişim belirteci sona erdirme zaman damgası; "1970-01-01T0:0:0Z UTC" saniye sayısı olarak temsil edilir ve belirteç `exp` iddiasına karşılık gelir. Bu durumda belirteç 2019-08-08T06:10:11+00:00 tarihinde sona erer (RFC 3339' da)|
| `resource` | Erişim belirteci verilen kaynak, isteğin `resource` sorgu dize parametresi ile belirtilir; belirteci 'aud' iddiasına karşılık gelir. |


## <a name="acquiring-an-access-token-using-c"></a>C kullanarak bir erişim belirteci edinme #
Yukarıdaki olur, C#:

```C#
namespace Azure.ServiceFabric.ManagedIdentity.Samples
{
    using System;
    using System.Net.Http;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Web;
    using Newtonsoft.Json;

    /// <summary>
    /// Type representing the response of the SF Managed Identity endpoint for token acquisition requests.
    /// </summary>
    [JsonObject]
    public sealed class ManagedIdentityTokenResponse
    {
        [JsonProperty(Required = Required.Always, PropertyName = "token_type")]
        public string TokenType { get; set; }

        [JsonProperty(Required = Required.Always, PropertyName = "access_token")]
        public string AccessToken { get; set; }

        [JsonProperty(PropertyName = "expires_on")]
        public string ExpiresOn { get; set; }

        [JsonProperty(PropertyName = "resource")]
        public string Resource { get; set; }
    }

    /// <summary>
    /// Sample class demonstrating access token acquisition using Managed Identity.
    /// </summary>
    public sealed class AccessTokenAcquirer
    {
        /// <summary>
        /// Acquire an access token.
        /// </summary>
        /// <returns>Access token</returns>
        public static async Task<string> AcquireAccessTokenAsync()
        {
            var managedIdentityEndpoint = Environment.GetEnvironmentVariable("MSI_ENDPOINT");
            var managedIdentityAuthenticationCode = Environment.GetEnvironmentVariable("MSI_SECRET");
            var managedIdentityAuthenticationHeader = "secret";
            var managedIdentityApiVersion = "2019-07-01-preview";
            var resource = "https://management.azure.com/";

            var requestUri = $"{managedIdentityEndpoint}?api-version={managedIdentityApiVersion}&resource={HttpUtility.UrlEncode(resource)}";

            var requestMessage = new HttpRequestMessage(HttpMethod.Get, requestUri);
            requestMessage.Headers.Add(managedIdentityAuthenticationHeader, managedIdentityAuthenticationCode);

            try
            {
                var response = await new HttpClient().SendAsync(requestMessage)
                    .ConfigureAwait(false);

                response.EnsureSuccessStatusCode();

                var tokenResponseString = await response.Content.ReadAsStringAsync()
                    .ConfigureAwait(false);

                var tokenResponseObject = JsonConvert.DeserializeObject<ManagedIdentityTokenResponse>(tokenResponseString);

                return tokenResponseObject.AccessToken;
            }
            catch (Exception ex)
            {
                string errorText = String.Format("{0} \n\n{1}", ex.Message, ex.InnerException != null ? ex.InnerException.Message : "Acquire token failed");

                Console.WriteLine(errorText);
            }

            return String.Empty;
        }
    } // class AccessTokenAcquirer
} // namespace Azure.ServiceFabric.ManagedIdentity.Samples
```
## <a name="accessing-key-vault-from-a-service-fabric-application-using-managed-identity"></a>Yönetilen Kimlik Kullanarak Servis Kumaşı uygulamasından Anahtar Kasasına Erişim
Bu örnek, yönetilen kimlik kullanarak Bir Key Vault'ta depolanan bir gizliye erişimi göstermek için yukarıdaki örnek üzerine inşa edin.

```C#
        /// <summary>
        /// Probe the specified secret, displaying metadata on success.  
        /// </summary>
        /// <param name="vault">vault name</param>
        /// <param name="secret">secret name</param>
        /// <param name="version">secret version id</param>
        /// <returns></returns>
        public async Task<string> ProbeSecretAsync(string vault, string secret, string version)
        {
            // initialize a KeyVault client with a managed identity-based authentication callback
            var kvClient = new Microsoft.Azure.KeyVault.KeyVaultClient(new Microsoft.Azure.KeyVault.KeyVaultClient.AuthenticationCallback((a, r, s) => { return AuthenticationCallbackAsync(a, r, s); }));

            Log(LogLevel.Info, $"\nRunning with configuration: \n\tobserved vault: {config.VaultName}\n\tobserved secret: {config.SecretName}\n\tMI endpoint: {config.ManagedIdentityEndpoint}\n\tMI auth code: {config.ManagedIdentityAuthenticationCode}\n\tMI auth header: {config.ManagedIdentityAuthenticationHeader}");
            string response = String.Empty;

            Log(LogLevel.Info, "\n== {DateTime.UtcNow.ToString()}: Probing secret...");
            try
            {
                var secretResponse = await kvClient.GetSecretWithHttpMessagesAsync(vault, secret, version)
                    .ConfigureAwait(false);

                if (secretResponse.Response.IsSuccessStatusCode)
                {
                    // use the secret: secretValue.Body.Value;
                    response = String.Format($"Successfully probed secret '{secret}' in vault '{vault}': {PrintSecretBundleMetadata(secretResponse.Body)}");
                }
                else
                {
                    response = String.Format($"Non-critical error encountered retrieving secret '{secret}' in vault '{vault}': {secretResponse.Response.ReasonPhrase} ({secretResponse.Response.StatusCode})");
                }
            }
            catch (Microsoft.Rest.ValidationException ve)
            {
                response = String.Format($"encountered REST validation exception 0x{ve.HResult.ToString("X")} trying to access '{secret}' in vault '{vault}' from {ve.Source}: {ve.Message}");
            }
            catch (KeyVaultErrorException kvee)
            {
                response = String.Format($"encountered KeyVault exception 0x{kvee.HResult.ToString("X")} trying to access '{secret}' in vault '{vault}': {kvee.Response.ReasonPhrase} ({kvee.Response.StatusCode})");
            }
            catch (Exception ex)
            {
                // handle generic errors here
                response = String.Format($"encountered exception 0x{ex.HResult.ToString("X")} trying to access '{secret}' in vault '{vault}': {ex.Message}");
            }

            Log(LogLevel.Info, response);

            return response;
        }

        /// <summary>
        /// KV authentication callback, using the application's managed identity.
        /// </summary>
        /// <param name="authority">The expected issuer of the access token, from the KV authorization challenge.</param>
        /// <param name="resource">The expected audience of the access token, from the KV authorization challenge.</param>
        /// <param name="scope">The expected scope of the access token; not currently used.</param>
        /// <returns>Access token</returns>
        public async Task<string> AuthenticationCallbackAsync(string authority, string resource, string scope)
        {
            Log(LogLevel.Verbose, $"authentication callback invoked with: auth: {authority}, resource: {resource}, scope: {scope}");
            var encodedResource = HttpUtility.UrlEncode(resource);

            // This sample does not illustrate the caching of the access token, which the user application is expected to do.
            // For a given service, the caching key should be the (encoded) resource uri. The token should be cached for a period
            // of time at most equal to its remaining validity. The 'expires_on' field of the token response object represents
            // the number of seconds from Unix time when the token will expire. You may cache the token if it will be valid for at
            // least another short interval (1-10s). If its expiration will occur shortly, don't cache but still return it to the 
            // caller. The MI endpoint will not return an expired token.
            // Sample caching code:
            //
            // ManagedIdentityTokenResponse tokenResponse;
            // if (responseCache.TryGetCachedItem(encodedResource, out tokenResponse))
            // {
            //     Log(LogLevel.Verbose, $"cache hit for key '{encodedResource}'");
            //
            //     return tokenResponse.AccessToken;
            // }
            //
            // Log(LogLevel.Verbose, $"cache miss for key '{encodedResource}'");
            //
            // where the response cache is left as an exercise for the reader. MemoryCache is a good option, albeit not yet available on .net core.

            var requestUri = $"{config.ManagedIdentityEndpoint}?api-version={config.ManagedIdentityApiVersion}&resource={encodedResource}";
            Log(LogLevel.Verbose, $"request uri: {requestUri}");

            var requestMessage = new HttpRequestMessage(HttpMethod.Get, requestUri);
            requestMessage.Headers.Add(config.ManagedIdentityAuthenticationHeader, config.ManagedIdentityAuthenticationCode);
            Log(LogLevel.Verbose, $"added header '{config.ManagedIdentityAuthenticationHeader}': '{config.ManagedIdentityAuthenticationCode}'");

            var response = await httpClient.SendAsync(requestMessage)
                .ConfigureAwait(false);
            Log(LogLevel.Verbose, $"response status: success: {response.IsSuccessStatusCode}, status: {response.StatusCode}");

            response.EnsureSuccessStatusCode();

            var tokenResponseString = await response.Content.ReadAsStringAsync()
                .ConfigureAwait(false);

            var tokenResponse = JsonConvert.DeserializeObject<ManagedIdentityTokenResponse>(tokenResponseString);
            Log(LogLevel.Verbose, "deserialized token response; returning access code..");

            // Sample caching code (continuation):
            // var expiration = DateTimeOffset.FromUnixTimeSeconds(Int32.Parse(tokenResponse.ExpiresOn));
            // if (expiration > DateTimeOffset.UtcNow.AddSeconds(5.0))
            //    responseCache.AddOrUpdate(encodedResource, tokenResponse, expiration);

            return tokenResponse.AccessToken;
        }

        private string PrintSecretBundleMetadata(SecretBundle bundle)
        {
            StringBuilder strBuilder = new StringBuilder();

            strBuilder.AppendFormat($"\n\tid: {bundle.Id}\n");
            strBuilder.AppendFormat($"\tcontent type: {bundle.ContentType}\n");
            strBuilder.AppendFormat($"\tmanaged: {bundle.Managed}\n");
            strBuilder.AppendFormat($"\tattributes:\n");
            strBuilder.AppendFormat($"\t\tenabled: {bundle.Attributes.Enabled}\n");
            strBuilder.AppendFormat($"\t\tnbf: {bundle.Attributes.NotBefore}\n");
            strBuilder.AppendFormat($"\t\texp: {bundle.Attributes.Expires}\n");
            strBuilder.AppendFormat($"\t\tcreated: {bundle.Attributes.Created}\n");
            strBuilder.AppendFormat($"\t\tupdated: {bundle.Attributes.Updated}\n");
            strBuilder.AppendFormat($"\t\trecoveryLevel: {bundle.Attributes.RecoveryLevel}\n");

            return strBuilder.ToString();
        }

        private enum LogLevel
        {
            Info,
            Verbose
        };

        private void Log(LogLevel level, string message)
        {
            if (level != LogLevel.Verbose
                || config.DoVerboseLogging)
            {
                Console.WriteLine(message);
            }
        }

```

## <a name="error-handling"></a>Hata işleme
HTTP yanıt üstbilgisinin 'durum kodu' alanı isteğin başarı durumunu gösterir; '200 Tamam' durumu başarıyı gösterir ve yanıt yukarıda açıklandığı gibi erişim belirteci içerir. Aşağıda olası hata yanıtlarının kısa bir numaralandırma vardır.

| Durum Kodu | Hata Nedeni | Nasıl Başa Çıkılabilen |
| ----------- | ------------ | ------------- |
| 404 Bulunamadı. | Bilinmeyen kimlik doğrulama kodu veya uygulama yönetilen bir kimlik atanmadı. | Uygulama kurulumveya belirteç edinme kodunu düzeltin. |
| 429 Çok fazla istek. |  AAD veya SF tarafından uygulanan gaz sınırına ulaşıldı. | Üstel Backoff ile yeniden deneyin. Aşağıdaki kılavuza bakın. |
| Istekte 4xx Hata. | İstek parametrelerinden biri veya birkaçı yanlıştı. | Tekrar denemeyin.  Daha fazla bilgi için hata ayrıntılarını inceleyin.  4xx hataları tasarım zamanı hatalarıdır.|
| Hizmetten 5xx Hata. | Yönetilen kimlik alt sistemi veya Azure Etkin Dizin geçici bir hata döndürdü. | Kısa bir süre sonra yeniden denemek güvenlidir. Yeniden denedikten sonra bir daralma durumuna (429) çarpabilirsiniz.|

Bir hata oluşursa, ilgili HTTP yanıt gövdesi hata ayrıntıları içeren bir JSON nesnesi içerir:

| Öğe | Açıklama |
| ------- | ----------- |
| kod | Hata kodu. |
| correlationId | Hata ayıklama için kullanılabilecek bir korelasyon kimliği. |
| message | Hata nın ayrıntılı açıklaması. **Hata açıklamaları herhangi bir zamanda değişebilir. Hata iletisinin kendisine bağlı değil.**|

Örnek hata:
```json
{"error":{"correlationId":"7f30f4d3-0f3a-41e0-a417-527f21b3848f","code":"SecretHeaderNotFound","message":"Secret is not found in the request headers."}}
```

Yönetilen kimliklere özgü tipik Hizmet Dokusu hatalarının bir listesi aşağıda verilmiştir:

| Kod | İleti | Açıklama | 
| ----------- | ----- | ----------------- |
| SecretHeaderNotFound | Gizli istek üstbilgisinde bulunmaz. | Kimlik doğrulama kodu istekle birlikte sağlanmadı. | 
| Yönetilen IdentityNotFound | Belirtilen uygulama ana bilgisayarı için yönetilen kimlik bulunamadı. | Uygulamanın kimliği yoktur veya kimlik doğrulama kodu bilinmiyor. |
| ArgumentNullOrEmpty | Parametre 'kaynak' null veya boş dize olmamalıdır. | Kaynak (hedef kitle) istekte sağlanmadı. |
| GeçersizApiVersion | Api sürümü '' desteklenmez. Desteklenen sürüm '2019-07-01-önizleme'. | İstek URI'de belirtilen eksik veya desteklenmeyen API sürümü. |
| InternalServerError | Bir hata oluşmuştur. | Yönetilen kimlik alt sisteminde, büyük olasılıkla Hizmet Kumaşı yığınının dışında bir hatayla karşılaşıldı. Büyük olasılıkla kaynak için belirtilen yanlış bir değerdir ('/'??) | 

## <a name="retry-guidance"></a>Kılavuzu yeniden deneyin 

Genellikle yeniden deneyilebilir tek hata kodu 429 (Çok Fazla İstek); iç sunucu hataları/5xx hata kodları yeniden denilebilir, ancak nedeni kalıcı olabilir. 

Azaltma sınırları, yönetilen kimlik alt sistemine yapılan arama ların sayısına (özellikle 'yukarı akış' bağımlılıkları (Yönetilen Kimlik Azure hizmeti veya güvenli belirteç hizmeti) için geçerlidir. Service Fabric, söz konusu bileşenlerin çeşitli düzeylerde belirteçleri önbelleğe alır, ancak ilgili bileşenlerin dağıtılmış yapısı göz önüne alındığında, arayan tutarsız azaltma yanıtları yaşayabilir (yani bir uygulamanın bir düğüm/örneğinde daraltılabilir, ancak aynı kimlik için bir belirteç talep ederken farklı bir düğümüzerinde değil.) Azaltma koşulu ayarlandığında, koşul temizlenene kadar aynı uygulamadan sonraki istekler HTTP durum kodu 429 (Çok Fazla İstek) ile başarısız olabilir.  

Azaltma nedeniyle başarısız olan isteklerin üstel bir geri leme ile yeniden denenmesi önerilir: 

| Çağrı dizini | 429 alma eylemi | 
| --- | --- | 
| 1 | 1 saniye bekleyin ve yeniden deneyin |
| 2 | 2 saniye bekleyin ve yeniden deneyin |
| 3 | 4 saniye bekleyin ve yeniden deneyin |
| 4 | 8 saniye bekleyin ve yeniden deneyin |
| 4 | 8 saniye bekleyin ve yeniden deneyin |
| 5 | 16 saniye bekleyin ve yeniden deneyin |

## <a name="resource-ids-for-azure-services"></a>Azure hizmetleri için kaynak it'leri
Azure AD'yi ve ilgili kaynak kimliklerini destekleyen kaynakların listesi için [Azure AD kimlik doğrulamasını destekleyen Azure hizmetlerine](../active-directory/managed-identities-azure-resources/services-support-msi.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar
* [Sistem tarafından atanmış yönetilen bir kimliğe sahip bir Azure Hizmet Kumaşı uygulamasını dağıtma](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)
* [Kullanıcı tarafından atanan yönetilen bir kimliğe sahip bir Azure Hizmet Kumaşı uygulaması dağıtma](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)
* [Azure Hizmet Kumaşı uygulamasına diğer Azure kaynaklarına erişim hakkı verme](./how-to-grant-access-other-resources.md)