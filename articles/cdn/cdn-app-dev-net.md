---
title: .NET için Azure CDN Kitaplığı ile çalışmaya başlama | Microsoft Docs
description: Visual Studio 'Yu kullanarak Azure CDN yönetmek için .NET uygulamaları yazmayı öğrenin.
services: cdn
documentationcenter: .net
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: 63cf4101-92e7-49dd-a155-a90e54a792ca
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 01/23/2017
ms.author: mazha
ms.custom: has-adal-ref
ms.openlocfilehash: 9f891327a60d5438369c56dd858bfcefff182cdf
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87049334"
---
# <a name="get-started-with-the-azure-cdn-library-for-net"></a>.NET için Azure CDN Kitaplığı ile çalışmaya başlama
> [!div class="op_single_selector"]
> * [Node.js](cdn-app-dev-node.md)
> * [.NET](cdn-app-dev-net.md)
>
>

CDN profillerinin ve uç noktaların oluşturulmasını ve yönetimini otomatikleştirmek için [.net Azure CDN Kitaplığı](/dotnet/api/overview/azure/cdn) 'nı kullanabilirsiniz.  Bu öğretici, kullanılabilir işlemlerin birkaçını gösteren basit bir .NET konsol uygulaması oluşturmayı gösterir.  Bu öğretici, .NET için Azure CDN kitaplığının tüm yönlerini ayrıntılı olarak tanımlamaya yönelik değildir.

Bu öğreticiyi tamamlayabilmeniz için Visual Studio 2015 gerekir.  [Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) , indirme için ücretsiz olarak kullanılabilir.

> [!TIP]
> [Bu öğreticiden tamamlanmış proje](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) MSDN 'de indirilebilir.
>
>

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a>Projenizi oluşturun ve NuGet paketleri ekleyin
CDN profillerimiz için bir kaynak grubu oluşturduğumuzdan ve bu grup içindeki CDN profillerini ve uç noktalarını yönetmek için Azure AD uygulama iznimize verildik. uygulamamızı oluşturmaya başlayabiliriz.

Visual Studio 2015 içinden yeni proje iletişim kutusunu açmak için **Dosya**, **Yeni**, **proje..** . öğesine tıklayın.  **Visual C#**' yi genişletin, ardından soldaki bölmeden **Windows** ' u seçin.  Orta bölmedeki **konsol uygulaması** ' na tıklayın.  Projenizi adlandırın ve ardından **Tamam**' a tıklayın.

![Yeni Proje](./media/cdn-app-dev-net/cdn-new-project.png)

Projemiz, NuGet paketlerinde bulunan bazı Azure kitaplıklarını kullanacak.  Projeye bunları ekleyelim.

1. **Araçlar** menüsünü, **NuGet Paket Yöneticisi**' ni ve ardından **Paket Yöneticisi konsolu**' nu tıklatın.

    ![NuGet Paketlerini Yönet](./media/cdn-app-dev-net/cdn-manage-nuget.png)
2. Paket Yöneticisi konsolunda, **Active Directory Authentication Library (ADAL)** yüklemek için aşağıdaki komutu yürütün:

    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`
3. **Azure CDN yönetim kitaplığını**yüklemek için aşağıdakileri yürütün:

    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a>Yönergeler, sabitler, Main yöntemi ve yardımcı yöntemleri
Şimdi yazılan programımızın temel yapısını inceleyelim.

1. Program.cs sekmesine döndüğünüzde, `using` üstteki yönergeleri aşağıdaki şekilde değiştirin:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Microsoft.Azure.Management.Cdn;
    using Microsoft.Azure.Management.Cdn.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```
2. Yöntemlerimizin kullanacağı bazı sabitleri tanımlamanız gerekiyor.  `Program`Sınıfında, ancak `Main` yönteminden önce aşağıdakileri ekleyin.  ** &lt; Açılı ayraçlar &gt; **dahil olmak üzere yer tutucuları, gerektiğinde kendi değerlerinizle değiştirdiğinizden emin olun.

    ```csharp
    //Tenant app constants
    private const string clientID = "<YOUR CLIENT ID>";
    private const string clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    private const string authority = "https://login.microsoftonline.com/<YOUR TENANT ID>/<YOUR TENANT DOMAIN NAME>";

    //Application constants
    private const string subscriptionId = "<YOUR SUBSCRIPTION ID>";
    private const string profileName = "CdnConsoleApp";
    private const string endpointName = "<A UNIQUE NAME FOR YOUR CDN ENDPOINT>";
    private const string resourceGroupName = "CdnConsoleTutorial";
    private const string resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```
3. Ayrıca, sınıf düzeyinde, bu iki değişkeni tanımlayın.  Profil ve uç noktanızın zaten var olup olmadığını öğrenmek için bunları daha sonra kullanacağız.

    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```
4. `Main`Yöntemini aşağıdaki gibi değiştirin:

   ```csharp
   static void Main(string[] args)
   {
       //Get a token
       AuthenticationResult authResult = GetAccessToken();

       // Create CDN client
       CdnManagementClient cdn = new CdnManagementClient(new TokenCredentials(authResult.AccessToken))
           { SubscriptionId = subscriptionId };

       ListProfilesAndEndpoints(cdn);

       // Create CDN Profile
       CreateCdnProfile(cdn);

       // Create CDN Endpoint
       CreateCdnEndpoint(cdn);

       Console.WriteLine();

       // Purge CDN Endpoint
       PromptPurgeCdnEndpoint(cdn);

       // Delete CDN Endpoint
       PromptDeleteCdnEndpoint(cdn);

       // Delete CDN Profile
       PromptDeleteCdnProfile(cdn);

       Console.WriteLine("Press Enter to end program.");
       Console.ReadLine();
   }
   ```
5. Diğer yöntemlerden bazıları kullanıcıdan "Evet/Hayır" soruları isteyecek.  Biraz daha kolay hale getirmek için aşağıdaki yöntemi ekleyin:

    ```csharp
    private static bool PromptUser(string Question)
    {
        Console.Write(Question + " (Y/N): ");
        var response = Console.ReadKey();
        Console.WriteLine();
        if (response.Key == ConsoleKey.Y)
        {
            return true;
        }
        else if (response.Key == ConsoleKey.N)
        {
            return false;
        }
        else
        {
            // They pressed something other than Y or N.  Let's ask them again.
            return PromptUser(Question);
        }
    }
    ```

Artık programımızın temel yapısı yazıldığına göre, yöntemi tarafından çağrılan yöntemleri oluşturacağız `Main` .

## <a name="authentication"></a>Kimlik doğrulaması
Azure CDN yönetim kitaplığını kullanabilmeniz için, hizmet Sorumlumuza kimlik doğrulaması yapmanız ve bir kimlik doğrulama belirteci edinmeniz gerekir.  Bu yöntem, belirteci almak için ADAL kullanır.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority);
    ClientCredential credential = new ClientCredential(clientID, clientSecret);
    AuthenticationResult authResult =
        authContext.AcquireTokenAsync("https://management.core.windows.net/", credential).Result;

    return authResult;
}
```

Tek bir kullanıcı kimlik doğrulaması kullanıyorsanız, `GetAccessToken` Yöntem biraz farklı görünecektir.

> [!IMPORTANT]
> Bu kod örneğini yalnızca bir hizmet sorumlusu yerine bireysel kullanıcı kimlik doğrulamasının olmasını tercih ediyorsanız kullanın.
>
>

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority);
    AuthenticationResult authResult = authContext.AcquireTokenAsync("https://management.core.windows.net/",
        clientID, new Uri("http://<redirect URI>"), new PlatformParameters(PromptBehavior.RefreshSession)).Result;

    return authResult;
}
```

`<redirect URI>`Uygulamayı Azure AD 'ye kaydettiğinizde girdiğiniz yeniden yönlendirme URI 'si ile değiştirdiğinizden emin olun.

## <a name="list-cdn-profiles-and-endpoints"></a>CDN profillerini ve uç noktalarını listeleme
Artık CDN işlemleri gerçekleştirmeye hazır hale getiriyoruz.  İlk yöntem, kaynak grubumuzdaki tüm profilleri ve uç noktaları listeliyor ve sabitlerimizde belirtilen profil ve uç nokta adları için bir eşleşme bulduğunda, daha sonra yinelenenleri oluşturmaya çalışmayın.

```csharp
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
    // List all the CDN profiles in this resource group
    var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
    foreach (Profile p in profileList)
    {
        Console.WriteLine("CDN profile {0}", p.Name);
        if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
        {
            // Hey, that's the name of the CDN profile we want to create!
            profileAlreadyExists = true;
        }

        //List all the CDN endpoints on this CDN profile
        Console.WriteLine("Endpoints:");
        var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
        foreach (Endpoint e in endpointList)
        {
            Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
            if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
            {
                // The unique endpoint name already exists.
                endpointAlreadyExists = true;
            }
        }
        Console.WriteLine();
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>CDN profilleri ve uç noktaları oluşturma
Daha sonra bir profil oluşturacağız.

```csharp
private static void CreateCdnProfile(CdnManagementClient cdn)
{
    if (profileAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating profile {0}.", profileName);
        ProfileCreateParameters profileParms =
            new ProfileCreateParameters() { Location = resourceLocation, Sku = new Sku(SkuName.StandardVerizon) };
        cdn.Profiles.Create(profileName, profileParms, resourceGroupName);
    }
}
```

Profil oluşturulduktan sonra bir uç nokta oluşturacağız.

```csharp
private static void CreateCdnEndpoint(CdnManagementClient cdn)
{
    if (endpointAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating endpoint {0} on profile {1}.", endpointName, profileName);
        EndpointCreateParameters endpointParms =
            new EndpointCreateParameters()
            {
                Origins = new List<DeepCreatedOrigin>() { new DeepCreatedOrigin("Contoso", "www.contoso.com") },
                IsHttpAllowed = true,
                IsHttpsAllowed = true,
                Location = resourceLocation
            };
        cdn.Endpoints.Create(endpointName, endpointParms, profileName, resourceGroupName);
    }
}
```

> [!NOTE]
> Yukarıdaki örnek, uç noktayı *contoso* adlı bir kaynak ana bilgisayar adı ile atar `www.contoso.com` .  Bunu kendi başlangıcının ana bilgisayar adının işaret etmesi için değiştirmelisiniz.
>
>

## <a name="purge-an-endpoint"></a>Bir uç noktayı Temizleme
Uç noktanın oluşturulduğu varsayılarak, programımızda gerçekleştirmek isteyebileceğiniz bir ortak görev, uç noktanızda içeriği temizliyoruz.

```csharp
private static void PromptPurgeCdnEndpoint(CdnManagementClient cdn)
{
    if (PromptUser(String.Format("Purge CDN endpoint {0}?", endpointName)))
    {
        Console.WriteLine("Purging endpoint. Please wait...");
        cdn.Endpoints.PurgeContent(endpointName, profileName, resourceGroupName, new List<string>() { "/*" });
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

> [!NOTE]
> Yukarıdaki örnekte dize, `/*` uç nokta yolunun kökündeki her şeyi temizlemek istediğimizi gösterir.  Bu, tüm Azure portal "Temizleme" iletişim kutusunda **Temizleme** işlemi denetimi ile eşdeğerdir. Yönteminde, `CreateCdnProfile` Verizon profilinden kodu kullanarak profilimizi bir **Azure CDN** olarak oluşturdum `Sku = new Sku(SkuName.StandardVerizon)` , bu nedenle bu işlem başarılı olur.  Ancak, **Akamai profillerden Azure CDN** **Tümünü temizlemeyi**desteklemez, bu nedenle, bu öğretici için bir Akamai profili kullanırsam, temizlemek için belirli yollar eklemesi gerekir.
>
>

## <a name="delete-cdn-profiles-and-endpoints"></a>CDN profillerini ve uç noktalarını silme
Son Yöntemler bitiş noktasıdır ve profilimizi silecektir.

```csharp
private static void PromptDeleteCdnEndpoint(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN endpoint {0} on profile {1}?", endpointName, profileName)))
    {
        Console.WriteLine("Deleting endpoint. Please wait...");
        cdn.Endpoints.DeleteIfExists(endpointName, profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}

private static void PromptDeleteCdnProfile(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN profile {0}?", profileName)))
    {
        Console.WriteLine("Deleting profile. Please wait...");
        cdn.Profiles.DeleteIfExists(profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

## <a name="running-the-program"></a>Programı çalıştırma
Şimdi Visual Studio 'daki **Başlat** düğmesine tıklayarak programı derleyip çalıştırabiliriz.

![Program çalışıyor](./media/cdn-app-dev-net/cdn-program-running-1.png)

Program yukarıdaki istemine ulaştığında, Azure portal kaynak grubunuza dönebilmeniz ve profilin oluşturulduğunu görmeniz gerekir.

![Başarılı!](./media/cdn-app-dev-net/cdn-success.png)

Daha sonra programın geri kalanını çalıştırmak için istemleri doğrulayabiliriz.

![Program Tamamlanıyor](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a>Sonraki Adımlar
Bu izlenecek yolda tamamlanmış projeyi görmek için [örneği indirin](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).

.NET için Azure CDN Yönetim Kitaplığı hakkında ek belgeler bulmak için [MSDN 'deki başvuruyu](/dotnet/api/overview/azure/cdn)görüntüleyin.

CDN kaynaklarınızı [PowerShell](cdn-manage-powershell.md)ile yönetin.
