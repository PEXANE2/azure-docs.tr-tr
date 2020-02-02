---
title: -Azure Time Series Insights kullanarak C# GA ortamlarında başvuru verilerini yönetme | Microsoft Docs
description: İçinde C#yazılmış özel bir uygulama oluşturarak GA ortamınız için başvuru verilerini yönetmeyi öğrenin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 01/31/2020
ms.custom: seodec18
ms.openlocfilehash: 079cfcee543cf1ce36c4a1394479a622b3658789
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76935119"
---
# <a name="manage-ga-reference-data-for-an-azure-time-series-insights-environment-using-c"></a>Kullanarak bir Azure Time Series Insights ortamı için GA başvuru verilerini yönetmeC#

Bu makalede, Azure Time Series Insights GA C# [Reference veri yönetimi API](https://docs.microsoft.com/rest/api/time-series-insights/ga-reference-data-api)'sine programlı API istekleri yapmak için nasıl birleştirileceğini, [msal.net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)ve Azure Active Directory gösterilmektedir.

## <a name="summary"></a>Özet

Aşağıdaki örnek kod aşağıdaki özellikleri göstermektedir:

* [Msal.net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) **publicclientapplication**kullanarak bir erişim belirteci alınıyor.
* GA [başvuru veri yönetimi API 'sine](https://docs.microsoft.com/rest/api/time-series-insights/ga-reference-data-api)KARŞı sıralı oluşturma, okuma, GÜNCELLEŞTIRME ve silme işlemleri.
* [Ortak hata kodları](https://docs.microsoft.com/rest/api/time-series-insights/ga-reference-data-api#validation-and-error-handling)dahil ortak yanıt kodları.
    
    Başvuru Veri Yönetimi API 'SI her öğeyi ayrı ayrı işler ve bir öğe ile bir hata, diğerlerinin başarıyla tamamlanmasını engellemez. Örneğin, isteğiniz 100 öğe içeriyorsa ve bir öğede hata varsa, 99 öğe yazılır ve bir öğe reddedilir.

## <a name="prerequisites-and-setup"></a>Önkoşullar ve kurulum

Örnek kodu derleyip çalıştırmadan önce aşağıdaki adımları gerçekleştirin:

1. [BIR GA Azure Time Series Insights ortamı sağlayın](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started
) .

1. Ortamınızda [bir başvuru veri kümesi oluşturun](time-series-insights-add-reference-data-set.md) . Aşağıdaki başvuru veri şemasını kullanın:

   | Anahtar adı | Tür |
   | --- | --- |
   | uuid | Dize | 

1. [Kimlik doğrulama ve yetkilendirme](time-series-insights-authentication-and-authorization.md)bölümünde açıklandığı gibi, Azure Active Directory için Azure Time Series Insights ortamınızı yapılandırın. **Yeniden yönlendirme URI 'si**olarak `http://localhost:8080/` kullanın.

1. Gerekli proje bağımlılıklarını yükler.

1. Her **#PLACEHOLDER #** öğesini uygun ortam tanımlayıcısı ile değiştirerek aşağıdaki örnek kodu düzenleyin.

1. `dotnet run`, projenizin kök dizini içinde çalıştırın. İstendiğinde, Azure 'da oturum açmak için Kullanıcı profilinizi kullanın. 

> [!TIP]
> * https://github.com/Azure-Samples/Azure-Time-Series-Insights diğer GA C# kod örneklerini görüntüleyin [ ](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-ga-sample).

## <a name="project-dependencies"></a>Proje bağımlılıkları

Visual Studio 'nun en yeni sürümünü ve **Netcore. app**' i kullanmanız önerilir:

* [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) -sürüm 16.4.2 +
* [Netcore. app](https://www.nuget.org/packages/Microsoft.NETCore.App/2.2.8) -Version 2.2.8

Örnek kodun iki zorunlu bağımlılığı vardır:

* MSAL.NET [Microsoft. Identity. Client](https://www.nuget.org/packages/Microsoft.Identity.Client/) -4.7.1 Package.
* [Newtonsoft. JSON](https://www.nuget.org/packages/Newtonsoft.Json) -12.0.3 paketi.

[NuGet 2.12 +](https://www.nuget.org/)kullanarak paketleri ekleyin:

* `dotnet add package Newtonsoft.Json --version 12.0.3`
* `dotnet add package Microsoft.Identity.Client --version 4.7.1`

Veya

1. Bir `csharp-tsi-msal-ga-sample.csproj` dosyası bildirin:

    ```XML
    <Project Sdk="Microsoft.NET.Sdk">
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <LangVersion>latest</LangVersion>
        <RootNamespace>csharp-tsi-msal-ga-sample</RootNamespace>
        <RunWorkingDirectory>$(MSBuildThisFileDirectory)</RunWorkingDirectory>
      </PropertyGroup>
      <ItemGroup>
        <PackageReference Include="Microsoft.Identity.Client" Version="4.7.1.0" Culture="neutral" PublicKeyToken="0a613f4dd989e8ae" />
        <PackageReference Include="Newtonsoft.Json" Version="12.0.3" />
      </ItemGroup>
    </Project>
    ```
1. Ardından `dotnet restore` komutunu çalıştırın.

## <a name="c-sample-code"></a>C#örnek kod

```csharp
// Copyright (c) Microsoft Corporation.  All rights reserved.

namespace CsharpTsiMsalGaSample
{
    using Microsoft.Identity.Client;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    using System;
    using System.IO;
    using System.Net.Http;
    using System.Text;
    using System.Threading.Tasks;

    public static class Program
    {
        /**
         * Review the product documentation for detailed configuration steps or skip ahead and configure your environment settings.
         * 
         * https://docs.microsoft.com/azure/time-series-insights/time-series-insights-authentication-and-authorization
         */

        // Azure Time Series Insights environment configuration
        internal static string EnvironmentFqdn = "#PLACEHOLDER#.env.timeseries.azure.com";
        internal static string EnvironmentReferenceDataSetName = "#PLACEHOLDER#";

        // Azure Active Directory application configuration
        internal static string AadClientApplicationId = "#PLACEHOLDER#";
        internal static string[] AadScopes = new string[] { "https://api.timeseries.azure.com//user_impersonation" };
        internal static string AadRedirectUri = "http://localhost:8080/";
        internal static string AadTenantName = "#PLACEHOLDER#";
        internal static string AadAuthenticationAuthority = "https://login.microsoftonline.com/" + AadTenantName + ".onmicrosoft.com/oauth2/authorize?resource=https://api.timeseries.azure.com/";

        private static async Task<string> AcquireAccessTokenAsync()
        {
            if (AadClientApplicationId == "#PLACEHOLDER#" || AadScopes.Length == 0 || AadRedirectUri == "#PLACEHOLDER#" || AadTenantName.StartsWith("#PLACEHOLDER#"))
            {
                throw new Exception($"Use the link {"https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started"} to update the values of 'AadClientApplicationId', 'AadScopes', 'AadRedirectUri', and 'AadAuthenticationAuthority'.");
            }

            /**
             * MSAL.NET configuration. Review the product documentation for more information about MSAL.NET authentication options.
             * 
             * https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/
             */

            IPublicClientApplication app = PublicClientApplicationBuilder
                .Create(AadClientApplicationId)
                .WithRedirectUri(AadRedirectUri)
                .WithAuthority(AadAuthenticationAuthority)
                .Build();

            AuthenticationResult result = await app
                .AcquireTokenInteractive(AadScopes)
                .ExecuteAsync();

            Console.WriteLine("MSAL Authentication Token Acquired: {0}", result.AccessToken);
            Console.WriteLine("");
            return result.AccessToken;
        }

        // System.Net.HttpClient helper to wrap HTTP POST made to the GA Reference Data API
        private static async Task<HttpResponseMessage> AsyncHttpPostRequestHelper(HttpClient httpClient, string input)
        {
             if (EnvironmentFqdn.StartsWith("#PLACEHOLDER#") || EnvironmentReferenceDataSetName == "#PLACEHOLDER#")
             {
                throw new Exception($"Use the link {"https://docs.microsoft.com/azure/time-series-insights/time-series-insights-authentication-and-authorization"} to update the values of 'EnvironmentFqdn' and 'EnvironmentReferenceDataSetName'.");
             }

             Console.WriteLine("HTTP JSON Request Body: {0}", input);
             Console.WriteLine("");
             HttpContent requestBody = new StringContent(input, Encoding.UTF8, "application/json");

             Uri uri = new UriBuilder("https", EnvironmentFqdn)
             {
                Path = $"referencedatasets/{EnvironmentReferenceDataSetName}/$batch",
                Query = "api-version=2016-12-12"
             }.Uri;
                
             Console.WriteLine("Making HTTP POST to URI: {0}", uri);
             Console.WriteLine("");

             HttpResponseMessage response = await httpClient.PostAsync(uri, requestBody);
             if (response.IsSuccessStatusCode)
             {
                var jsonString = await response.Content.ReadAsStringAsync();
                var jsonStringTransferObject = JsonConvert.DeserializeObject<object>(jsonString);
                Console.WriteLine("HTTP JSON Response Body: {0}", jsonStringTransferObject);
                Console.WriteLine("");
                return response;
             }

             return null;
        }

        private static async Task TsiMsalGaSample()
        {
            Console.WriteLine("Beginning demo...");
            Console.WriteLine("");
            Console.WriteLine("The following samples assume a single Key Name (uuid) with String type...");
            Console.WriteLine("");

            string accessToken = await AcquireAccessTokenAsync();
            var httpClient = new HttpClient();
            httpClient.DefaultRequestHeaders.Add("Authorization", "Bearer " + accessToken);

            {   
                // CREATE reference data
                Console.WriteLine("CREATE reference data example...");
                Console.WriteLine("");
                string createInput = @"
                    {
                        ""put"": [
                            {
                                ""uuid"": ""Fan1""
                            },
                            {
                                ""uuid"": ""Fan2"",
                                ""color"": ""White"",
                                ""floor"": 2
                            },
                            {
                                ""uuid"": ""Fan3"",
                                ""color"": ""Red"",
                                ""maxSpeed"": 5
                            }
                        ]
                    }";


                var createResponse = await AsyncHttpPostRequestHelper(httpClient, createInput);

                // READ reference data
                Console.WriteLine("READ reference data example...");
                Console.WriteLine("");
                string readInput = @"
                    {
                        ""get"": [
                            {
                                ""uuid"": ""Fan1""
                            },
                            {
                                ""uuid"": ""Fan2"",
                                ""color"": ""White"",
                                ""desc"": ""example""
                            },
                            {
                                ""uuid"": ""Fan3"",
                                ""color"": ""Red"",
                                ""maxSpeed"": 5
                            }
                        ]
                    }";

                var readResponse = await AsyncHttpPostRequestHelper(httpClient, readInput);

                // UPDATE reference data
                Console.WriteLine("UPDATE reference data example...");
                Console.WriteLine("");
                string updateInput = @"
                    {
                        ""patch"": [
                            {
                                ""uuid"": ""Fan1"",
                                ""color"": ""Red""
                            },
                            {
                                ""uuid"": ""Fan2"",
                                ""color"": ""Orange""
                            },
                            {
                                ""uuid"": ""Fan3"",
                                ""desc"": ""Example""
                            }
                        ]
                    }";

                var inputResponse = await AsyncHttpPostRequestHelper(httpClient, updateInput);

                // DELETE reference data
                Console.WriteLine("DELETE reference data example...");
                Console.WriteLine("");
                string deleteInput = @"
                    {
                        ""delete"": [
                            {
                                ""uuid"": ""Fan1""
                            },
                            {
                                ""uuid"": ""Fan2"",
                                ""color"": ""Orange""
                            },
                            {
                                ""uuid"": ""Fan2"",
                                ""desc"": ""Blue""
                            }
                        ]
                    }";

                var deleteResponse = await AsyncHttpPostRequestHelper(httpClient, deleteInput);
            }
        }

        internal static void Main(string[] args)
        {
            Task.Run(async () => await TsiMsalGaSample()).Wait();
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- GA [başvurusu VERI YÖNETIMI API](https://docs.microsoft.com/rest/api/time-series-insights/ga-reference-data-api) başvurusu belgelerini okuyun.
