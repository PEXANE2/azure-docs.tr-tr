---
title: 'Hızlı başlangıç: ilk .NET Core sorgunuz'
description: Bu hızlı başlangıçta, .NET Core için kaynak grafiği NuGet paketlerini etkinleştirmek ve ilk sorgunuzu çalıştırmak için adımları izleyin.
ms.date: 06/29/2020
ms.topic: quickstart
ms.custom: devx-track-csharp
ms.openlocfilehash: 0470f100b6323f760bc48af70e8a6c11b94dfb3c
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89005879"
---
# <a name="quickstart-run-your-first-resource-graph-query-using-net-core"></a>Hızlı başlangıç: .NET Core kullanarak ilk kaynak grafik sorgunuzu çalıştırma

Azure Kaynak Grafiği 'ni kullanmanın ilk adımı, .NET Core için gereken paketlerin yüklü olduğunu denetlerdir. Bu hızlı başlangıç, paketleri .NET Core yüklemenize ekleme sürecinde size yol gösterir.

Bu işlemin sonunda, paketleri .NET Core yüklemenize eklemiş ve ilk kaynak Graf sorgunuzu çalıştıracaksınız.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.
- _ClientID_ ve _ClientSecret_dahil olmak üzere bir Azure hizmet sorumlusu. Kaynak Graph ile kullanım için bir hizmet sorumlusu yoksa veya yeni bir tane oluşturmak istiyorsanız bkz. [.net Için Azure Yönetim kitaplıkları kimlik doğrulaması](/dotnet/azure/sdk/authentication#mgmt-auth).
  Sonraki adımlarda bunu yapacağımız için .NET Core paketlerini yüklemek için adımı atlayın.

## <a name="create-the-resource-graph-project"></a>Kaynak Grafiği projesi oluşturma

.NET Core 'u Azure Kaynak grafiğini sorgulamak üzere etkinleştirmek için yeni bir konsol uygulaması oluşturun ve gerekli paketleri yükler.

1. En son .NET Core 'un yüklü olduğundan emin olun (en azından **3.1.5**). Henüz yüklenmemişse, [DotNet.Microsoft.com](https://dotnet.microsoft.com/download/dotnet-core)adresinden indirin.

1. "ArgQuery" adlı yeni bir .NET Core konsol uygulaması başlatın:

   ```dotnetcli
   dotnet new console --name "argQuery"
   ```

1. Dizinleri yeni proje klasörüne değiştirin ve gerekli Azure Kaynak Grafiği paketlerini yükler:

   ```dotnetcli
   # Add the Resource Graph package for .NET Core
   dotnet add package Microsoft.Azure.Management.ResourceGraph --version 2.0.0

   # Add the Azure app auth package for .NET Core
   dotnet add package Microsoft.Azure.Services.AppAuthentication --version 1.5.0
   ```

1. Varsayılan değerini `program.cs` aşağıdaki kodla değiştirin ve güncelleştirilmiş dosyayı kaydedin:

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Threading.Tasks;
   using Microsoft.IdentityModel.Clients.ActiveDirectory;
   using Microsoft.Rest;
   using Microsoft.Azure.Management.ResourceGraph;
   using Microsoft.Azure.Management.ResourceGraph.Models;

   namespace argQuery
   {
       class Program
       {
           static async Task Main(string[] args)
           {
               string strTenant = args[0];
               string strClientId = args[1];
               string strClientSecret = args[2];
               string strSubscriptionId = args[3];
               string strQuery = args[4];

               AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/" + strTenant);
               AuthenticationResult authResult = await authContext.AcquireTokenAsync("https://management.core.windows.net", new ClientCredential(strClientId, strClientSecret));
               ServiceClientCredentials serviceClientCreds = new TokenCredentials(authResult.AccessToken);

               ResourceGraphClient argClient = new ResourceGraphClient(serviceClientCreds);
               QueryRequest request = new QueryRequest();
               request.Subscriptions = new List<string>(){ strSubscriptionId };
               request.Query = strQuery;

               QueryResponse response = argClient.Resources(request);
               Console.WriteLine("Records: " + response.Count);
               Console.WriteLine("Data:\n" + response.Data);
           }
       }
   }
   ```

1. Konsol uygulaması oluşturun ve yayımlayın `argQuery` :

   ```dotnetcli
   dotnet build
   dotnet publish -o {run-folder}
   ```

## <a name="run-your-first-resource-graph-query"></a>İlk Kaynak Grafiği sorgunuzu çalıştırma

.NET Core konsol uygulaması oluşturulup yayımlandıysa, basit bir kaynak grafik sorgusunu denemekle ilgili bir zaman vardır. Sorgu, ilk beş Azure kaynağını her bir kaynağın **adı** ve **kaynak türü** ile döndürür.

Her çağrıda `argQuery` , kendi değerlerinizle değiştirmeniz gereken kullanılan değişkenler vardır:

- `{tenantId}` -Kiracı KIMLIĞINIZLE değiştirin
- `{clientId}` -Hizmet sorumlunun istemci KIMLIĞIYLE değiştirin
- `{clientSecret}` -Hizmet sorumlunun istemci gizli anahtarı ile değiştirin
- `{subscriptionId}` - Abonelik kimliğinizle değiştirin

1. `{run-folder}`Önceki komutla tanımladığınız dizinleri değiştirin `dotnet publish` .

1. Derlenmiş .NET Core konsol uygulamasını kullanarak ilk Azure Kaynak Grafiği sorgunuzu çalıştırın:

   ```bash
   argQuery "{tenantId}" "{clientId}" "{clientSecret}" "{subscriptionId}" "Resources | project name, type | limit 5"
   ```

   > [!NOTE]
   > Bu sorgu örneği, `order by` gibi bir sıralama değiştirici sağlamadığı için, bu sorgunun birden çok kez çalıştırılması muhtemelen istek başına farklı bir kaynak kümesi sunacaktır.

1. Son parametreyi olarak değiştirin `argQuery.exe` ve sorguyu Name özelliği olarak değiştirin `order by` : **Name**

   ```bash
   argQuery "{tenantId}" "{clientId}" "{clientSecret}" "{subscriptionId}" "Resources | project name, type | limit 5 | order by name asc"
   ```

   > [!NOTE]
   > İlk sorguda olduğu gibi, bu sorguyu birden çok kez çalıştırmak, muhtemelen istek başına farklı bir kaynak kümesi sunacaktır. Sorgu komutlarının düzeni önemlidir. Bu örnekte `order by`, `limit`’den sonra gelmektedir. Bu komut sırası ilk olarak sorgu sonuçlarını kısıtlar ve sonra sıralar.

1. Son parametreyi olarak değiştirin `argQuery.exe` ve sorguyu ilk olarak `order by` **ad** özelliğine, sonra da `limit` en üstteki beş sonuca değiştirin:

   ```bash
   argQuery "{tenantId}" "{clientId}" "{clientSecret}" "{subscriptionId}" "Resources | project name, type | order by name asc | limit 5"
   ```

Son sorgu birkaç kez çalıştırıldığında, ortamınızdaki hiçbir şeyin değişmediği kabul edildiğinde döndürülen sonuçlar tutarlı ve **ad** özelliğine göre sıralanır, ancak yine de en üstteki beş sonuçla sınırlıdır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

.NET Core konsol uygulamasını ve yüklü paketleri kaldırmak istiyorsanız proje klasörünü silerek bunu yapabilirsiniz `argQuery` .

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, gerekli kaynak grafik paketleriyle bir .NET Core konsol uygulaması oluşturdunuz ve ilk sorgunuzu çalıştırdık. Kaynak grafik dili hakkında daha fazla bilgi edinmek için sorgu dili ayrıntıları sayfasına ilerleyin.

> [!div class="nextstepaction"]
> [Sorgu dili hakkında daha fazla bilgi alın](./concepts/query-language.md)