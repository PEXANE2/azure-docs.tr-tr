---
title: Erişim jetonuna erişmek için sanal bir makinede yönetilen kimlikleri kullanma - Azure AD
description: OAuth erişim jetonunu elde etmek için sanal makinelerde Azure kaynakları için yönetilen kimlikleri kullanmak için adım adım talimatlar ve örnekler.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: a58103bad3914bd0c0c6e70f8e3d2882271e1070
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80049203"
---
# <a name="how-to-use-managed-identities-for-azure-resources-on-an-azure-vm-to-acquire-an-access-token"></a>Bir erişim jetonu edinmek için Azure kaynaklarının azure kaynakları için yönetilen kimlikler nasıl kullanılır? 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  

Azure kaynakları için yönetilen kimlikler, Azure Etkin Dizini'nde otomatik olarak yönetilen bir kimlik le Azure hizmetlerine olanak sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini doğrulamak için kullanabilirsiniz. 

Bu makalede, belirteç edinimi için çeşitli kod ve komut dosyası örneklerinin yanı sıra belirteç sona erme ve HTTP hataları işleme gibi önemli konularda rehberlik sağlar. 

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Bu makalede Azure PowerShell örneklerini kullanmayı planlıyorsanız, [Azure PowerShell'in](/powershell/azure/install-az-ps)en son sürümünü yüklediğinizden emin olun.


> [!IMPORTANT]
> - Bu makaledeki tüm örnek kod/komut dosyası, istemcinin Azure kaynakları için yönetilen kimliklere sahip sanal bir makinede çalıştığını varsayar. VM'inize uzaktan bağlanmak için Azure portalındaki sanal makine "Bağlan" özelliğini kullanın. VM'de Azure kaynakları için yönetilen kimlikleri etkinleştirme yle ilgili ayrıntılar için, Azure portalını veya varyant makalelerinden birini (PowerShell, CLI, şablon veya Azure SDK kullanarak) [kullanarak VM'deki Azure kaynakları için yönetilen kimlikleri yapılandır'](qs-configure-portal-windows-vm.md)a bakın. 

> [!IMPORTANT]
> - Azure kaynakları için yönetilen kimliklerin güvenlik sınırı, üzerinde kullanıldığı kaynaktır. Sanal bir makinede çalışan tüm kodlar/komut dosyaları, üzerinde bulunan yönetilen kimlikler için belirteçleri isteyebilir ve alabilir. 

## <a name="overview"></a>Genel Bakış

İstemci uygulaması, belirli bir kaynağa erişmek için Yalnızca Uygulama [uygulamasına erişim](../develop/developer-glossary.md#access-token) için yönetilen kimlikler isteyebilir. Belirteç, [Azure kaynakları hizmeti ilkesinin yönetilen kimliklerine dayanır.](overview.md#how-does-the-managed-identities-for-azure-resources-work) Bu nedenle, istemcinin kendi hizmet ilkesi altında bir erişim belirteci elde etmek için kendini kaydetmesi gerekmez. Belirteç, [istemci kimlik bilgilerini gerektiren hizmet-servis aramalarında](../develop/v2-oauth2-client-creds-grant-flow.md)taşıyıcı belirteci olarak kullanım için uygundur.

|  |  |
| -------------- | -------------------- |
| [HTTP kullanarak bir belirteç alın](#get-a-token-using-http) | Azure kaynakları belirteç bitiş noktası için yönetilen kimlikler için protokol ayrıntıları |
| [.NET için Microsoft.Azure.Services.AppAuthentication kitaplığını kullanarak bir belirteç alın](#get-a-token-using-the-microsoftazureservicesappauthentication-library-for-net) | Microsoft.Azure.Services.AppAuthentication kitaplığını bir .NET istemcisinden kullanma örneği
| [C kullanarak belirteç alın #](#get-a-token-using-c) | Bir C# istemcisinden Azure kaynakları için yönetilen kimlikleri kullanma örneği REST bitiş noktası |
| [Java kullanarak bir belirteç alın](#get-a-token-using-java) | Bir Java istemcisinden Azure kaynakları IÇIN yönetilen kimlikleri kullanma örneği REST bitiş noktası |
| [Go'u kullanarak bir belirteç alın](#get-a-token-using-go) | Azure kaynakları için yönetilen kimlikleri kullanma örneği REST bitiş noktası bir Go istemcisinden |
| [Azure PowerShell'i kullanarak belirteç elde etme](#get-a-token-using-azure-powershell) | Bir PowerShell istemcisinden Azure kaynakları IÇIN yönetilen kimlikleri kullanma örneği REST bitiş noktası |
| [CURL kullanarak belirteç alma](#get-a-token-using-curl) | Bir Bash/CURL istemcisinden Azure kaynakları için yönetilen kimlikleri kullanma örneği REST bitiş noktası |
| Belirteç önbelleğe alma | Süresi dolmuş erişim belirteçlerini işleme kılavuzu |
| [Hata işleme](#error-handling) | Azure kaynakları belirteç uç noktası için yönetilen kimliklerden döndürülen HTTP hatalarını işleme kılavuzu |
| [Azure hizmetleri için kaynak it'leri](#resource-ids-for-azure-services) | Desteklenen Azure hizmetleri için kaynak t.c. nereden ulaşabilirsiniz? |

## <a name="get-a-token-using-http"></a>HTTP kullanarak bir belirteç alın 

Erişim jetonu edinmenin temel arabirimi REST'e dayanır ve VM'de çalışan ve HTTP REST aramaları yapabilen herhangi bir istemci uygulaması için erişilebilir hale getirir. Bu, istemcinin sanal makinede bir bitiş noktası kullanması (Azure AD bitiş noktası nı karşılaştırması) dışında Azure AD programlama modeline benzer.

Azure Örneği Meta veri hizmeti (IMDS) bitiş noktasını kullanarak örnek istek *(önerilir)*:

```
GET 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.azure.com/' HTTP/1.1 Metadata: true
```

| Öğe | Açıklama |
| ------- | ----------- |
| `GET` | Son noktadan veri almak istediğinizi belirten HTTP fiili. Bu durumda, bir OAuth erişim belirteci. | 
| `http://169.254.169.254/metadata/identity/oauth2/token` | Örnek Meta veri hizmeti için Azure kaynakları bitiş noktası için yönetilen kimlikler. |
| `api-version`  | IMDS bitiş noktası için API sürümünü gösteren bir sorgu dize parametresi. Lütfen API `2018-02-01` sürümünü veya daha büyük kullanın. |
| `resource` | Hedef kaynağın App ID URI'sini gösteren bir sorgu dize parametresi. Ayrıca verilen belirteci `aud` (hedef kitle) iddiasıgörünür. Bu örnek, Bir Uygulama Kimliği URI'si olan Azure `https://management.azure.com/`Kaynak Yöneticisi'ne erişmek için bir belirteç istiyor. |
| `Metadata` | Sunucu Tarafı İstek Sahteciliği (SSRF) saldırısına karşı bir azaltma olarak Azure kaynakları için yönetilen kimlikler tarafından gerekli bir HTTP istek üstbilgi alanı. Bu değer, tüm küçük harfle "true" olarak ayarlanmalıdır. |
| `object_id` | (İsteğe bağlı) Belirteci istediğiniz yönetilen kimliğin object_id gösteren bir sorgu dize parametresi. VM'nizde birden çok kullanıcı tarafından atanmış yönetilen kimlik varsa gereklidir.|
| `client_id` | (İsteğe bağlı) Bir sorgu dize parametresi, için belirteç istediğiniz yönetilen kimliğin client_id gösteren. VM'nizde birden çok kullanıcı tarafından atanmış yönetilen kimlik varsa gereklidir.|
| `mi_res_id` | (İsteğe bağlı) Belirteci istediğiniz yönetilen kimliğin mi_res_id (Azure Kaynak Kimliği) belirten bir sorgu dize parametresi. VM'nizde birden çok kullanıcı tarafından atanmış yönetilen kimlik varsa gereklidir. |

Azure kaynakları VM Extension Endpoint için yönetilen kimlikleri kullanarak örnek istek *(Ocak 2019'da amortisman için planlanmıştır)*:

```http
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F HTTP/1.1
Metadata: true
```

| Öğe | Açıklama |
| ------- | ----------- |
| `GET` | Son noktadan veri almak istediğinizi belirten HTTP fiili. Bu durumda, bir OAuth erişim belirteci. | 
| `http://localhost:50342/oauth2/token` | 50342 varsayılan bağlantı noktası olan ve yapılandırılabilir olan Azure kaynakları bitiş noktası için yönetilen kimlikler. |
| `resource` | Hedef kaynağın App ID URI'sini gösteren bir sorgu dize parametresi. Ayrıca verilen belirteci `aud` (hedef kitle) iddiasıgörünür. Bu örnek, Bir Uygulama Kimliği URI'si olan Azure `https://management.azure.com/`Kaynak Yöneticisi'ne erişmek için bir belirteç istiyor. |
| `Metadata` | Sunucu Tarafı İstek Sahteciliği (SSRF) saldırısına karşı bir azaltma olarak Azure kaynakları için yönetilen kimlikler tarafından gerekli bir HTTP istek üstbilgi alanı. Bu değer, tüm küçük harfle "true" olarak ayarlanmalıdır.|
| `object_id` | (İsteğe bağlı) Belirteci istediğiniz yönetilen kimliğin object_id gösteren bir sorgu dize parametresi. VM'nizde birden çok kullanıcı tarafından atanmış yönetilen kimlik varsa gereklidir.|
| `client_id` | (İsteğe bağlı) Bir sorgu dize parametresi, için belirteç istediğiniz yönetilen kimliğin client_id gösteren. VM'nizde birden çok kullanıcı tarafından atanmış yönetilen kimlik varsa gereklidir.|

Örnek yanıt:

```json
HTTP/1.1 200 OK
Content-Type: application/json
{
  "access_token": "eyJ0eXAi...",
  "refresh_token": "",
  "expires_in": "3599",
  "expires_on": "1506484173",
  "not_before": "1506480273",
  "resource": "https://management.azure.com/",
  "token_type": "Bearer"
}
```

| Öğe | Açıklama |
| ------- | ----------- |
| `access_token` | İstenen erişim jetonu. Güvenli bir REST API'yi ararken, belirteç `Authorization` istek üstbilgi alanına "taşıyıcı" belirteci olarak katıştırılmış ve API'nin arayanın kimliğini doğrulaması sağlar. | 
| `refresh_token` | Azure kaynakları için yönetilen kimlikler tarafından kullanılmaz. |
| `expires_in` | Erişim belirteci nin verildiği tarihten itibaren süresi dolmadan önce geçerli olmaya devam eden saniye sayısı. Verme zamanı belirteç `iat` iddiasında bulunabilir. |
| `expires_on` | Erişim belirteci süresi dolduğunda zaman aşımı. Tarih, "1970-01-01T0:0:0Z UTC" saniye sayısı olarak temsil edilir (belirteç `exp` talebine karşılık gelir). |
| `not_before` | Erişim belirteci etkin olduğunda zaman alanı ve kabul edilebilir. Tarih, "1970-01-01T0:0:0Z UTC" saniye sayısı olarak temsil edilir (belirteç `nbf` talebine karşılık gelir). |
| `resource` | Erişim belirteci için istenen ve isteğin `resource` sorgu dize parametresi ile eşleşen kaynak. |
| `token_type` | "Taşıyıcı" erişim belirteci olan belirteci türü, hangi kaynak bu belirteç taşıyıcısı erişim verebilir anlamına gelir. |

## <a name="get-a-token-using-the-microsoftazureservicesappauthentication-library-for-net"></a>.NET için Microsoft.Azure.Services.AppAuthentication kitaplığını kullanarak bir belirteç alın

.NET uygulamaları ve işlevleri için, Azure kaynakları için yönetilen kimliklerle çalışmanın en basit yolu Microsoft.Azure.Services.AppAuthentication paketidir. Bu kitaplık ayrıca Visual Studio, [Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest)veya Active Directory Integrated Authentication'daki kullanıcı hesabınızı kullanarak kodunuzu geliştirme makinenizde yerel olarak test etmenize olanak tanır. Bu kitaplıkla yerel geliştirme seçenekleri hakkında daha fazla bilgi için [Microsoft.Azure.Services.AppAuthentication başvurusuna](/azure/key-vault/service-to-service-authentication)bakın. Bu bölümde, kodunuzda kitaplık la nasıl başlanıncayasınız gösterilmektedir.

1. [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) ve [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) NuGet paketlerine başvurularınızı uygulamanıza ekleyin.

2.  Uygulamanıza aşağıdaki kodu ekleyin:

    ```csharp
    using Microsoft.Azure.Services.AppAuthentication;
    using Microsoft.Azure.KeyVault;
    // ...
    var azureServiceTokenProvider = new AzureServiceTokenProvider();
    string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://management.azure.com/");
    // OR
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
    ```
    
Microsoft.Azure.Services.AppAuthentication ve sunduğu işlemler hakkında daha fazla bilgi edinmek için [Microsoft.Azure.Services.AppAuthentication başvurusu](/azure/key-vault/service-to-service-authentication) ve [Azure kaynakları .NET örneği için yönetilen kimliklere sahip Uygulama Hizmeti ve KeyVault'a](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet)bakın.

## <a name="get-a-token-using-c"></a>C kullanarak belirteç alın #

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Net;
using System.Web.Script.Serialization; 

// Build request to acquire managed identities for Azure resources token
HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.azure.com/");
request.Headers["Metadata"] = "true";
request.Method = "GET";

try
{
    // Call /token endpoint
    HttpWebResponse response = (HttpWebResponse)request.GetResponse();

    // Pipe response Stream to a StreamReader, and extract access token
    StreamReader streamResponse = new StreamReader(response.GetResponseStream()); 
    string stringResponse = streamResponse.ReadToEnd();
    JavaScriptSerializer j = new JavaScriptSerializer();
    Dictionary<string, string> list = (Dictionary<string, string>) j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
    string accessToken = list["access_token"];
}
catch (Exception e)
{
    string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
}

```

## <a name="get-a-token-using-java"></a>Java kullanarak bir belirteç alın

Java kullanarak bir belirteç almak için bu [JSON kitaplığını](https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core/2.9.4) kullanın.

```Java
import java.io.*;
import java.net.*;
import com.fasterxml.jackson.core.*;
 
class GetMSIToken {
    public static void main(String[] args) throws Exception {
 
        URL msiEndpoint = new URL("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.azure.com/");
        HttpURLConnection con = (HttpURLConnection) msiEndpoint.openConnection();
        con.setRequestMethod("GET");
        con.setRequestProperty("Metadata", "true");
 
        if (con.getResponseCode()!=200) {
            throw new Exception("Error calling managed identity token endpoint.");
        }
 
        InputStream responseStream = con.getInputStream();
 
        JsonFactory factory = new JsonFactory();
        JsonParser parser = factory.createParser(responseStream);
 
        while(!parser.isClosed()){
            JsonToken jsonToken = parser.nextToken();
 
            if(JsonToken.FIELD_NAME.equals(jsonToken)){
                String fieldName = parser.getCurrentName();
                jsonToken = parser.nextToken();
 
                if("access_token".equals(fieldName)){
                    String accesstoken = parser.getValueAsString();
                    System.out.println("Access Token: " + accesstoken.substring(0,5)+ "..." + accesstoken.substring(accesstoken.length()-5));
                    return;
                }
            }
        }
    }
}
```

## <a name="get-a-token-using-go"></a>Go'u kullanarak bir belirteç alın

```
package main

import (
  "fmt"
  "io/ioutil"
  "net/http"
  "net/url"
  "encoding/json"
)

type responseJson struct {
  AccessToken string `json:"access_token"`
  RefreshToken string `json:"refresh_token"`
  ExpiresIn string `json:"expires_in"`
  ExpiresOn string `json:"expires_on"`
  NotBefore string `json:"not_before"`
  Resource string `json:"resource"`
  TokenType string `json:"token_type"`
}

func main() {
    
    // Create HTTP request for a managed services for Azure resources token to access Azure Resource Manager
    var msi_endpoint *url.URL
    msi_endpoint, err := url.Parse("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01")
    if err != nil {
      fmt.Println("Error creating URL: ", err)
      return 
    }
    msi_parameters := url.Values{}
    msi_parameters.Add("resource", "https://management.azure.com/")
    msi_endpoint.RawQuery = msi_parameters.Encode()
    req, err := http.NewRequest("GET", msi_endpoint.String(), nil)
    if err != nil {
      fmt.Println("Error creating HTTP request: ", err)
      return 
    }
    req.Header.Add("Metadata", "true")

    // Call managed services for Azure resources token endpoint
    client := &http.Client{}
    resp, err := client.Do(req) 
    if err != nil{
      fmt.Println("Error calling token endpoint: ", err)
      return
    }

    // Pull out response body
    responseBytes,err := ioutil.ReadAll(resp.Body)
    defer resp.Body.Close()
    if err != nil {
      fmt.Println("Error reading response body : ", err)
      return
    }

    // Unmarshall response body into struct
    var r responseJson
    err = json.Unmarshal(responseBytes, &r)
    if err != nil {
      fmt.Println("Error unmarshalling the response:", err)
      return
    }

    // Print HTTP response and marshalled response body elements to console
    fmt.Println("Response status:", resp.Status)
    fmt.Println("access_token: ", r.AccessToken)
    fmt.Println("refresh_token: ", r.RefreshToken)
    fmt.Println("expires_in: ", r.ExpiresIn)
    fmt.Println("expires_on: ", r.ExpiresOn)
    fmt.Println("not_before: ", r.NotBefore)
    fmt.Println("resource: ", r.Resource)
    fmt.Println("token_type: ", r.TokenType)
}
```

## <a name="get-a-token-using-azure-powershell"></a>Azure PowerShell'i kullanarak belirteç elde etme

Aşağıdaki örnek, Bir PowerShell istemcisinden Azure kaynakları REST bitiş noktası için yönetilen kimliklerin nasıl kullanılacağını gösterir:

1. Bir erişim jetonu edinin.
2. Azure Kaynak Yöneticisi REST API'yi aramak ve VM hakkında bilgi almak için erişim belirteci'ni kullanın. Abonelik kimliğinizi, kaynak grup adınızı ve sanal makine `<SUBSCRIPTION-ID>` `<RESOURCE-GROUP>`adınızı `<VM-NAME>`sırasıyla , ve , için değiştirin.

```azurepowershell
Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Headers @{Metadata="true"}
```

Erişim belirteci nin yanıttan ayrışdırılaması ile ilgili örnek:
```azurepowershell
# Get an access token for managed identities for Azure resources
$response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' `
                              -Headers @{Metadata="true"}
$content =$response.Content | ConvertFrom-Json
$access_token = $content.access_token
echo "The managed identities for Azure resources access token is $access_token"

# Use the access token to get resource information for the VM
$vmInfoRest = (Invoke-WebRequest -Uri 'https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Compute/virtualMachines/<VM-NAME>?api-version=2017-12-01' -Method GET -ContentType "application/json" -Headers @{ Authorization ="Bearer $access_token"}).content
echo "JSON returned from call to get VM info:"
echo $vmInfoRest

```

## <a name="get-a-token-using-curl"></a>CURL kullanarak belirteç alma

```bash
curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true -s
```


Erişim belirteci nin yanıttan ayrışdırılaması ile ilgili örnek:

```bash
response=$(curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true -s)
access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
echo The managed identities for Azure resources access token is $access_token
```

## <a name="token-caching"></a>Belirteç önbelleğe alma

Azure kaynakları alt sisteminin yönetilen kimlikleri (Azure kaynakları VM Uzantısı için IMDS/yönetilen kimlikler) önbellek belirteçleri olsa da, kodunuzda belirteç önbelleği uygulamanızı öneririz. Sonuç olarak, kaynağın belirteç süresinin dolduğunu gösterdiği senaryolar için hazırlanmanız gerekir. 

Azure AD'ye yapılan telefon görüşmeleri yalnızca aşağıdaki anda sonuçlanmaktadır:
- önbellek miss Azure kaynakları alt sistem önbelleği için yönetilen kimliklerde belirteç nedeniyle oluşur
- önbelleğe alınmış belirteç süresi doldu

## <a name="error-handling"></a>Hata işleme

Azure kaynakları bitiş noktası için yönetilen kimlikler, HTTP yanıt iletisi üstbilgisinin durum kodu alanı üzerinden 4xx veya 5xx hataları gibi hataları bildirir:

| Durum Kodu | Hata Nedeni | Nasıl Başa Çıkılabilen |
| ----------- | ------------ | ------------- |
| 404 Bulunamadı. | IMDS bitiş noktası güncelleniyor. | Üstel Backoff ile yeniden deneyin. Aşağıdaki kılavuza bakın. |
| 429 Çok fazla istek. |  IMDS Azaltma sınırına ulaşıldı. | Üstel Backoff ile yeniden deneyin. Aşağıdaki kılavuza bakın. |
| Istekte 4xx Hata. | İstek parametrelerinden biri veya birkaçı yanlıştı. | Tekrar denemeyin.  Daha fazla bilgi için hata ayrıntılarını inceleyin.  4xx hataları tasarım zamanı hatalarıdır.|
| Hizmetten 5xx Geçici hata. | Azure kaynakları alt sistemi veya Azure Etkin Dizin için yönetilen kimlikler geçici bir hatayı döndürür. | En az 1 saniye bekledikten sonra yeniden denemek güvenlidir.  Çok hızlı veya çok sık yeniden denerseniz, IMDS ve/veya Azure AD bir oran sınırı hatası döndürebilir (429).|
| timeout | IMDS bitiş noktası güncelleniyor. | Üstel Backoff ile yeniden deneyin. Aşağıdaki kılavuza bakın. |

Bir hata oluşursa, ilgili HTTP yanıt gövdesi hata ayrıntıları ile JSON içerir:

| Öğe | Açıklama |
| ------- | ----------- |
| error   | Hata tanımlayıcısı. |
| error_description | Hata nın ayrıntılı açıklaması. **Hata açıklamaları herhangi bir zamanda değişebilir. Hata açıklamasındaki değerlere göre dalları kod yazmayın.**|

### <a name="http-response-reference"></a>HTTP yanıt başvurusu

Bu bölüm, olası hata yanıtlarını belgeler. "200 Tamam" durumu başarılı bir yanıttır ve erişim belirteci yanıt gövdesi JSON'da, access_token öğesinde bulunur.

| Durum kodu | Hata | Hata Açıklaması | Çözüm |
| ----------- | ----- | ----------------- | -------- |
| 400 Kötü İstek | invalid_resource | AADSTS50001: * \<URI\> * adlı uygulama * \<kiracı-ID\>* adlı bulunamadı. Bu, uygulama kiracının yöneticisi tarafından yüklenmediyse veya kiracıdaki herhangi bir kullanıcı tarafından onaylanmışsa gerçekleşebilir. Kimlik doğrulama isteğinizi yanlış kiracıya göndermiş olabilirsiniz.\ | (Sadece Linux) |
| 400 Kötü İstek | bad_request_102 | Gerekli meta veri üstbilgisi belirtilmedi | `Metadata` İstek üstbilgi alanı isteğinizde eksik veya yanlış biçimlendirilmiş. `true`Değer, tüm küçük harf olarak belirtilmelidir. Bir örnek için önceki REST bölümündeki "Örnek istek"e bakın.|
| 401 Yetkisiz | unknown_source | Bilinmeyen * \<Kaynak URI\>* | HTTP GET isteğiniz IN'in doğru biçimlendirilmiş olduğunu doğrulayın. Kısım `scheme:host/resource-path` ' olarak `http://localhost:50342/oauth2/token`belirtilmelidir. Bir örnek için önceki REST bölümündeki "Örnek istek"e bakın.|
|           | invalid_request | İstek gerekli bir parametre eksik, geçersiz bir parametre değeri içerir, birden fazla kez bir parametre içerir veya başka bir şekilde yanlış biçimlendirilmiştir. |  |
|           | unauthorized_client | İstemci bu yöntemi kullanarak bir erişim belirteci istemeye yetkili değildir. | Uzantıyı aramak için yerel geri dönüş kullanmayan bir istek veya Azure kaynaklarının kimliklerini doğru yapılandırılmamış bir VM'de kaynaklanır. VM yapılandırması ile ilgili yardıma ihtiyacınız varsa, [Azure portalını kullanarak Bir VM'de Azure kaynakları için yönetilen kimlikleri yapılandırma](qs-configure-portal-windows-vm.md) yada düzenleme hakkında bilgi bulundu. |
|           | access_denied | Kaynak sahibi veya yetkilendirme sunucusu isteği reddetti. |  |
|           | unsupported_response_type | Yetkilendirme sunucusu bu yöntemi kullanarak bir erişim belirteci elde desteklemiyor. |  |
|           | invalid_scope | İstenen kapsam geçersiz, bilinmiyor veya biçimsiz. |  |
| 500 Dahili sunucu hatası | Bilinmeyen | Etkin dizinden belirteç alınamadı. Ayrıntılar için dosya * \<yolundaki günlüklere bakın\>* | VM'de Azure kaynakları için yönetilen kimliklerin etkinleştirildiğini doğrulayın. VM yapılandırması ile ilgili yardıma ihtiyacınız varsa, [Azure portalını kullanarak Bir VM'de Azure kaynakları için yönetilen kimlikleri yapılandırma](qs-configure-portal-windows-vm.md) yada düzenleme hakkında bilgi bulundu.<br><br>Ayrıca, HTTP GET istek URI'nizin özellikle sorgu dizesinde belirtilen kaynak URI'nin doğru biçimlendirilmiş olduğunu da doğrulayın. Bir örnek için önceki REST bölümündeki "Örnek istek"e veya bir hizmet listesi ve ilgili kaynak kimliklerinin [Azure AD kimlik doğrulamasını destekleyen Azure hizmetlerine](services-support-msi.md) bakın.

## <a name="retry-guidance"></a>Kılavuzu yeniden deneyin 

404, 429 veya 5xx hata kodu alırsanız yeniden denemeniz önerilir (yukarıdaki [hata işleme](#error-handling) ye bakın).

Azaltma limitleri, IMDS bitiş noktasına yapılan arama ların sayısı için geçerlidir. Azaltma eşiği aşıldığında, IMDS bitiş noktası, azaltma etkinken diğer istekleri sınırlar. Bu süre zarfında, IMDS bitiş noktası HTTP durum kodu 429'u ("Çok fazla istek") döndürecek ve istekler başarısız olur. 

Yeniden deneme için aşağıdaki stratejiyi öneririz: 

| **Yeniden deneme stratejisi** | **Ayarlar** | **Değer** | **Nasıl çalışır?** |
| --- | --- | --- | --- |
|ExponentialBackoff |Yeniden deneme sayısı<br />En düşük geri alma<br />En yüksek geri alma<br />Delta geri alma<br />İlk hızlı yeniden deneme |5<br />0 sn<br />60 sn<br />2 sn<br />yanlış |Deneme 1 - 0 sn gecikme<br />Deneme 2 - yaklaşık 2 sn gecikme<br />Deneme 3 - yaklaşık 6 sn gecikme<br />Deneme 4 - yaklaşık 14 sn gecikme<br />Deneme 5 - yaklaşık 30 sn gecikme |

## <a name="resource-ids-for-azure-services"></a>Azure hizmetleri için kaynak it'leri

Azure AD'yi destekleyen ve Azure kaynakları ve ilgili kaynak kimlikleri için yönetilen kimliklerle sınanmış kaynakların listesi için [Azure AD kimlik doğrulamasını destekleyen Azure hizmetlerine](services-support-msi.md) bakın.


## <a name="next-steps"></a>Sonraki adımlar

- Azure VM'de Azure kaynakları için yönetilen kimlikleri etkinleştirmek [için, Azure portalını kullanarak Bir VM'de Azure kaynakları için yönetilen kimlikleri yapılandırma'ya](qs-configure-portal-windows-vm.md)bakın.








