---
title: Öğretici-uygulama olarak Microsoft Graph Web uygulaması erişim | Mavisi
description: Bu öğreticide, yönetilen kimlikleri kullanarak Microsoft Graph verilere nasıl erişebileceğinizi öğreneceksiniz.
services: microsoft-graph, app-service-web
author: rwike77
manager: CelesteDG
ms.service: app-service-web
ms.topic: tutorial
ms.workload: identity
ms.date: 11/09/2020
ms.author: ryanwi
ms.reviewer: stsoneff
ms.openlocfilehash: 70b180efa35d6310735f045a85103719b17c8555
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94428959"
---
# <a name="tutorial-access-microsoft-graph-from-a-secured-app-as-the-app"></a>Öğretici: uygulama olarak güvenli bir uygulamadan Microsoft Graph erişin

Azure App Service çalıştıran bir Web uygulamasından Microsoft Graph erişme hakkında bilgi edinin.

:::image type="content" alt-text="Erişim Microsoft Graph" source="./media/scenario-secure-app-access-microsoft-graph/web-app-access-graph.svg" border="false":::

Web uygulaması adına Microsoft Graph çağırmak istiyorsunuz.  Web uygulamanıza verilere erişim sağlamanın güvenli bir yolu, [sistem tarafından atanmış bir yönetilen kimlik](/azure/active-directory/managed-identities-azure-resources/overview)kullanmaktır. Azure AD 'den yönetilen bir kimlik, uygulama hizmetlerinin uygulama kimlik bilgileri gerekmeden Role-Based Access Control (RBAC) aracılığıyla kaynaklara erişmesine olanak sağlar. Web uygulamanıza yönetilen bir kimlik atadıktan sonra, Azure bir sertifikanın oluşturulmasını ve dağıtımını gerçekleştirir.  Gizli dizileri veya uygulama kimlik bilgilerini yönetme konusunda endişelenmeniz gerekmez.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Bir Web uygulamasında bir sistem tarafından atanmış yönetilen kimlik oluşturma
> * Yönetilen bir kimliğe Microsoft Graph API izinleri ekleme
> * Yönetilen kimlikler kullanarak bir Web uygulamasından Microsoft Graph çağırma

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* [App Service kimlik doğrulama/yetkilendirme modülü etkinleştirilmiş](scenario-secure-app-authentication-app-service.md)Azure App Service üzerinde çalışan bir Web uygulaması.

## <a name="enable-managed-identity-on-app"></a>Uygulamada yönetilen kimliği etkinleştir

Web uygulamanızı Visual Studio aracılığıyla oluşturup yayımlarsanız, yönetilen kimlik sizin için uygulamanızda etkinleştirilmiştir. App Service 'te, sol gezinti bölmesinde **kimlik** ' ü ve ardından **sistem atandı** ' ı seçin.  **Durumun** **Açık** olarak ayarlandığını doğrulayın.  Aksi takdirde, sistem tarafından atanan yönetilen kimliği etkinleştirmek için **Kaydet** ' e ve ardından **Evet** ' e tıklayın.  Yönetilen kimlik etkinleştirildiğinde durum *Açık* olarak ayarlanır ve nesne kimliği kullanılabilir.

Bir sonraki adımda ihtiyacınız olacak **nesne kimliğini** bir yere göz atın.

:::image type="content" alt-text="Sistem tarafından atanan kimlik" source="./media/scenario-secure-app-access-microsoft-graph/create-system-assigned-identity.png":::

## <a name="grant-access-to-microsoft-graph"></a>Microsoft Graph erişim izni verme

Microsoft Graph erişirken, yönetilen kimliğin gerçekleştirmek istediği işlem için doğru izinlere sahip olması gerekir. Şu anda Azure portal bu izinleri atama seçeneği yoktur. Aşağıdaki betik, istenen Microsoft Graph API izinlerini yönetilen kimlik hizmeti sorumlusu nesnesine ekler:

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell
# Install the module (You need admin on the machine)
#Install-Module AzureAD 

# Your tenant id (in Azure Portal, under Azure Active Directory -> Overview )
$TenantID="<tenant-id>"
$resourceGroup = "securewebappresourcegroup"
$webAppName="SecureWebApp-20201102125811"

# Get ID of the managed identity for the web app
$spID = (Get-AzWebApp -ResourceGroupName $resourceGroup -Name $webAppName).identity.principalid

# Check the Microsoft Graph documentation for the permission you need for the operation
$PermissionName = "User.Read.All"

Connect-AzureAD -TenantId $TenantID

# Get the service principal for Microsoft Graph
$GraphServicePrincipal = Get-AzureADServicePrincipal -SearchString "Microsoft Graph"

# Assign permissions to managed identity service principal
$AppRole = $GraphServicePrincipal.AppRoles | `
Where-Object {$_.Value -eq $PermissionName -and $_.AllowedMemberTypes -contains "Application"}

New-AzureAdServiceAppRoleAssignment -ObjectId $spID -PrincipalId $spID `
-ResourceId $GraphServicePrincipal.ObjectId -Id $AppRole.Id
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az login

webAppName="SecureWebApp-20201106120003"

spId=$(az resource list -n $webAppName --query [*].identity.principalId --out tsv)

graphResourceId=$(az ad sp list --display-name "Microsoft Graph" --query [0].objectId --out tsv)

appRoleId=$(az ad sp list --display-name "Microsoft Graph" --query "[0].appRoles[?value=='User.Read.All' && contains(allowedMemberTypes, 'Application')].id" --output tsv)

uri=https://graph.microsoft.com/v1.0/servicePrincipals/$spID/appRoleAssignments

body="{'principalId':'$spId','resourceId':'$graphResourceId','appRoleId':'$appRoleId'}"

az rest --method post --uri $uri --body $body --headers "Content-Type=application/json"
```

---

Betiği yürüttükten sonra, istenen API izinlerinin yönetilen kimliğe atandığını [Azure Portal](https://portal.azure.com) doğrulayabilirsiniz.  **Azure Active Directory** gidin ve **Kurumsal uygulamalar** ' ı seçin.  Bu dikey pencere, kiracınızdaki tüm hizmet sorumlularını görüntüler.  **Tüm uygulamalarda** , yönetilen kimlik için hizmet sorumlusu ' nı seçin.  Bu öğreticiyi takip ediyorsanız, aynı görünen ada (örneğin "SecureWebApp2020094113531") sahip iki hizmet sorumlusu vardır.  *Giriş sayfası URL 'si* olan hizmet sorumlusu kiracınızdaki Web uygulamasını temsil eder.  *Giriş sayfası URL 'si* olmadan hizmet sorumlusu, Web uygulamanız için sistem tarafından atanan yönetilen kimliği temsil eder. Yönetilen kimliğin nesne KIMLIĞI, daha önce oluşturduğunuz yönetilen kimliğin nesne KIMLIĞIYLE eşleşir.  

Yönetilen kimlik için hizmet sorumlusunu seçin.

:::image type="content" alt-text="Tüm uygulamalar" source="./media/scenario-secure-app-access-microsoft-graph/enterprise-apps-all-applications.png":::

**Genel bakış** bölümünde **izinler** ' i seçtiğinizde Microsoft Graph için eklenen izinleri görürsünüz.

:::image type="content" alt-text="İzinler" source="./media/scenario-secure-app-access-microsoft-graph/enterprise-apps-permissions.png":::

## <a name="call-microsoft-graph-net"></a>Microsoft Graph çağrısı (.NET)

[DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential) sınıfı, Azure depolama 'ya istekleri yetkilendirmek üzere kodunuzun belirteç kimlik bilgilerini almak için kullanılır.  Belirteçleri getirmek ve bunları hizmet istemcisine eklemek için yönetilen kimliği kullanan [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential) sınıfının bir örneğini oluşturun. Aşağıdaki kod örneği, kimliği doğrulanmış belirteç kimlik bilgisini alır ve onu gruptaki kullanıcıları alan bir hizmet istemci nesnesi oluşturmak için kullanır.  

### <a name="install-microsoftgraph-client-library-package"></a>Microsoft. Graph istemci kitaplığı paketini yükler

.NET Core komut satırı arabirimini veya Visual Studio 'da Paket Yöneticisi konsolunu kullanarak [Microsoft. Graph NuGet paketini](https://www.nuget.org/packages/Microsoft.Graph) projenize yükler.

# <a name="command-line"></a>[Komut satırı](#tab/command-line)

Bir komut satırı açın ve proje dosyanızı içeren dizine geçiş yapın.

Install komutlarını çalıştırın:

```dotnetcli
dotnet add package Microsoft.Graph
```

# <a name="package-manager"></a>[Paket Yöneticisi](#tab/package-manager)

Visual Studio 'da projeyi/çözümü açın ve **Araçlar**  >  **NuGet Paket Yöneticisi**  >  **Paket Yöneticisi konsolu** komutunu kullanarak konsolunu açın.

Install komutlarını çalıştırın:
```powershell
Install-Package Microsoft.Graph
```

---

### <a name="example"></a>Örnek

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc.RazorPages;
using Azure.Identity;
using Microsoft.Graph.Core;
using System.Net.Http.Headers;

...

public IList<MSGraphUser> Users { get; set; }

public async Task OnGetAsync()
{
    // Create the Graph service client with a DefaultAzureCredential which gets an access token using the available Managed Identity
    var credential = new DefaultAzureCredential();
    var token = credential.GetToken(
        new Azure.Core.TokenRequestContext(
            new[] { "https://graph.microsoft.com/.default" }));

    var accessToken = token.Token;
    var graphServiceClient = new GraphServiceClient(
        new DelegateAuthenticationProvider((requestMessage) =>
        {
            requestMessage
            .Headers
            .Authorization = new AuthenticationHeaderValue("bearer", accessToken);

            return Task.CompletedTask;
        }));

    List<MSGraphUser> msGraphUsers = new List<MSGraphUser>();
    try
    {
        var users =await graphServiceClient.Users.Request().GetAsync();
        foreach(var u in users)
        {
            MSGraphUser user = new MSGraphUser();
            user.userPrincipalName = u.UserPrincipalName;
            user.displayName = u.DisplayName;
            user.mail = u.Mail;
            user.jobTitle = u.JobTitle;

            msGraphUsers.Add(user);
        }
    }
    catch(Exception ex)
    {
        string msg = ex.Message;
    }

    Users = msGraphUsers;
}
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticiyle işiniz yaptıysanız ve artık Web uygulamasına veya ilişkili kaynaklara ihtiyacınız yoksa, [oluşturduğunuz kaynakları temizleyin](scenario-secure-app-clean-up-resources.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
>
> * Bir Web uygulamasında bir sistem tarafından atanmış yönetilen kimlik oluşturma
> * Yönetilen bir kimliğe Microsoft Graph API izinleri ekleme
> * Yönetilen kimlikler kullanarak bir Web uygulamasından Microsoft Graph çağırma

Bir [.NET Core uygulamasını](tutorial-dotnetcore-sqldb-app.md), [Python uygulamasını](tutorial-python-postgresql-app.md), [Java uygulamasını](tutorial-java-spring-cosmosdb.md)veya [Node.js uygulamasını](tutorial-nodejs-mongodb-app.md) bir veritabanına bağlamayı öğrenin.