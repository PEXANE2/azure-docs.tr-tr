---
title: .NET | Microsoft Dokümanlar
description: Visual Studio'yu kullanarak Azure CDN'yi yönetmek için .NET uygulamalarını nasıl yazarak yazacağız öğrenin.
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
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 7e3ad3a5928b36c221bb83b1c4012c3c9e14f35d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67594173"
---
# <a name="get-started-with-azure-cdn-development"></a>Azure CDN ile geliştirmeye başlama
> [!div class="op_single_selector"]
> * [Node.js](cdn-app-dev-node.md)
> * [.NET](cdn-app-dev-net.md)
> 
> 

CDN profillerinin ve uç noktalarının oluşturulmasını ve yönetimini otomatikleştirmek [için .NET için Azure CDN Kitaplığını](/dotnet/api/overview/azure/cdn) kullanabilirsiniz.  Bu öğretici, kullanılabilir işlemlerden birkaçını gösteren basit bir .NET konsol uygulaması nın oluşturulmasında yürür.  Bu öğretici, .NET için Azure CDN Kitaplığı'nın tüm yönlerini ayrıntılı olarak açıklamak için tasarlanmamıştır.

Bu eğitimi tamamlamak için Visual Studio 2015'e ihtiyacınız var.  [Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) ücretsiz olarak indirilebilir.

> [!TIP]
> [Bu öğretici tamamlanan proje](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) MSDN indirmek için kullanılabilir.
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a>Projenizi oluşturun ve Nuget paketleri ekleyin
CDN profillerimiz için bir kaynak grubu oluşturduğumuza ve Azure AD uygulamamıza bu grup içindeki CDN profillerini ve uç noktalarını yönetme izni verdiğimize göre, uygulamamızı oluşturmaya başlayabiliriz.

Visual Studio 2015 içinden, yeni proje iletişim kutusunu açmak için **Dosya**, **Yeni**, **Proje...** seçeneğini tıklayın.  **Visual C#** seçeneğini genişletin, ardından soldaki bölmede **Windows'u** seçin.  Orta bölmede **Konsol Uygulaması'nı** tıklatın.  Projenizi adlandırın, ardından **Tamam'ı**tıklatın.  

![Yeni Proje](./media/cdn-app-dev-net/cdn-new-project.png)

Projemiz Nuget paketlerinde bulunan bazı Azure kitaplıklarını kullanacak.  Bunları projeye ekleyelim.

1. **Araçlar** menüsüne tıklayın, **Nuget Paket Yöneticisi,** sonra **Paket Yöneticisi Konsolu**.
   
    ![Nuget Paketlerini Yönet](./media/cdn-app-dev-net/cdn-manage-nuget.png)
2. Paket Yöneticisi Konsolunda, **Active Directory Authentication Library'yi (ADAL)** yüklemek için aşağıdaki komutu uygulayın:
   
    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`
3. **Azure CDN Yönetim Kitaplığını**yüklemek için aşağıdakileri uygulayın:
   
    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a>Direktifler, sabitler, ana yöntem ve yardımcı yöntemler
Programımızın temel yapısını yazalım.

1. Program.cs sekmesinde, üstteki `using` yönergeleri aşağıdakilerle değiştirin:
   
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
2. Yöntemlerimizin kullanacağı bazı sabitleri tanımlamalıyız.  `Program` Sınıfta, ancak yöntemden `Main` önce aşağıdakileri ekleyin.  Açı braketleri de dahil olmak üzere yer **tutucuları&gt;gerektiğinde kendi değerlerinizle değiştirdiğinizden emin olun. &lt;**
   
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
3. Ayrıca sınıf düzeyinde, bu iki değişkentanımlayın.  Bunları daha sonra profilimizin ve bitiş noktamızın var olup olmadığını belirlemek için kullanırız.
   
    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```
4. Yöntemi `Main` aşağıdaki gibi değiştirin:
   
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
5. Diğer yöntemlerimizden bazıları kullanıcıya "Evet/Hayır" soruları yöneltecek.  Bunu biraz daha kolaylaştırmak için aşağıdaki yöntemi ekleyin:
   
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

Artık programımızın temel yapısı yazıldığına göre, `Main` yöntemle adlandırılan yöntemleri oluşturmalıyız.

## <a name="authentication"></a>Kimlik doğrulaması
Azure CDN Yönetim Kitaplığını kullanabilmek için önce hizmet yöneticiliğimizin kimliğini doğrulamamız ve kimlik doğrulama belirteci edinmemiz gerekir.  Bu yöntem, belirteci almak için ADAL kullanır.

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

Tek tek kullanıcı kimlik doğrulaması `GetAccessToken` kullanıyorsanız, yöntem biraz farklı görünecektir.

> [!IMPORTANT]
> Bu kod örneğini yalnızca hizmet sorumlusu yerine tek tek kullanıcı kimlik doğrulaması yapmayı tercih ediyorsanız kullanın.
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

Uygulamayı Azure `<redirect URI>` AD'ye kaydettirirken girdiğiniz yeniden yönlendirme URI ile değiştirdiğinden emin olun.

## <a name="list-cdn-profiles-and-endpoints"></a>CDN profillerini ve uç noktalarını listele
Artık CDN işlemlerini gerçekleştirmeye hazırız.  Yöntemimizin yaptığı ilk şey, kaynak grubumuzdaki tüm profilleri ve uç noktaları listelemektir ve sabitlerimizde belirtilen profil ve uç nokta adları için bir eşleşme bulursa, daha sonra için buna not verir, böylece yinelenenler oluşturmaya çalışmayız.

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

## <a name="create-cdn-profiles-and-endpoints"></a>CDN profilleri ve uç noktalar oluşturma
Sonra, bir profil oluşturacağız.

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

Profil oluşturulduktan sonra bir bitiş noktası oluşturacağız.

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
> Yukarıdaki örnek, bitiş noktasına bir ana bilgisayar adı `www.contoso.com`ile *Contoso* adlı bir başlangıç atar.  Bunu kendi kaynağınızın ana bilgisayar adını işaret edecek şekilde değiştirmelisiniz.
> 
> 

## <a name="purge-an-endpoint"></a>Bir bitiş noktasını temizleme
Bitiş noktasının oluşturulduğunu varsayarsak, programımızda gerçekleştirmek isteyebileceğimiz ortak bir görev, bitiş noktamızdaki içeriği temizlemektir.

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
> Yukarıdaki örnekte, dize, `/*` bitiş noktası yolunun kökündeki her şeyi temizlemek istediğimi gösterir.  Bu, Azure portalının "temizleme" iletişim kutusunda **Tümtemizleme'yi** denetlemeye eşdeğerdir. Yöntemde, `CreateCdnProfile` kodu `Sku = new Sku(SkuName.StandardVerizon)`kullanarak Verizon **profilinden azure CDN** olarak profilimizi oluşturdum, böylece bu başarılı olacaktır.  Ancak, **Akamai profillerinden Azure CDN** **Tüm Temizleme**desteklemiyor , bu nedenle bu öğretici için bir Akamai profili kullanıyorsanız, ben temizlemek için belirli yollar eklemek gerekir.
> 
> 

## <a name="delete-cdn-profiles-and-endpoints"></a>CDN profillerini ve uç noktalarını silme
Son yöntemler bitiş noktamızı ve profilimizi siler.

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
Visual Studio'da **Başlat** butonuna tıklayarak programı derleyip çalıştırabiliriz.

![Program çalışıyor](./media/cdn-app-dev-net/cdn-program-running-1.png)

Program yukarıdaki istemi ulaştığında, Azure portalındaki kaynak grubunuza dönebilmeli ve profilin oluşturulduğunu görebilmelisiniz.

![Başarılı!](./media/cdn-app-dev-net/cdn-success.png)

Daha sonra programın geri kalanını çalıştırmak için istemleri doğrulayabilir.

![Program tamamlama](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a>Sonraki Adımlar
Tamamlanan projeyi bu gözden geçirme den görmek [için, örneği indirin.](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c)

.NET için Azure CDN Yönetim Kitaplığı'nda ek belgeler bulmak için [BAŞVURUYU MSDN'de](/dotnet/api/overview/azure/cdn)görüntüleyin.

[PowerShell](cdn-manage-powershell.md)ile CDN kaynaklarınızı yönetin.

