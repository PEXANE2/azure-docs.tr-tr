---
title: Öğretici-.NET 'te Azure WebApp ile Azure Key Vault kullanma | Microsoft Docs
description: Bu öğreticide, anahtar kasaınızdan gizli dizi okumak için bir ASP.NET Core uygulaması yapılandırırsınız.
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 05/06/2020
ms.author: mbaldwin
ms.openlocfilehash: f6e70caaedf906142b19ba45f0eb4d818e2955e7
ms.sourcegitcommit: ff19f4ecaff33a414c0fa2d4c92542d6e91332f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2020
ms.locfileid: "85051901"
---
# <a name="tutorial-use-a-managed-identity-to-connect-key-vault-to-an-azure-web-app-with-net"></a>Öğretici: bir Azure Web uygulamasına .NET ile Key Vault bağlamak için yönetilen bir kimlik kullanma

Azure Key Vault, kimlik bilgilerini ve diğer gizli dizileri güvenli bir şekilde depolamanıza olanak tanır, ancak kodunuzun bunları almak için Key Vault kimlik doğrulaması gerekir. Azure [kaynaklarına genel bakış Için Yönetilen kimlikler](../../active-directory/managed-identities-azure-resources/overview.md) , Azure hIzmetlerIne Azure AD 'de otomatik olarak yönetilen bir kimlik vererek bu sorunu çözmeye yardımcı olur. Kodunuzda kimlik bilgilerini görüntülemesi gerekmeden Key Vault dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmak için bu kimliği kullanabilirsiniz.

Bu öğretici, bir Azure Web uygulamasının kimliğini Azure Key Vault kimlik doğrulaması için yönetilen bir kimlik kullanır. Adımlarda, .NET ve [Azure CLI](/cli/azure/get-started-with-azure-cli) [için Azure Key Vault v4 istemci kitaplığı](/dotnet/api/overview/azure/key-vault?view=azure-dotnet) kullanılsa da, tercih ettiğiniz geliştirme dilini, Azure PowerShell ve/veya Azure Portal kullanırken aynı temel ilkeler de geçerlidir.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için:

* Bir Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [.NET Core 3,1 SDK veya üzeri](https://dotnet.microsoft.com/download/dotnet-core/3.1).
* [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) veya [Azure PowerShell](/powershell/azure/overview)

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. [Az Group Create](/cli/azure/group?view=azure-cli-latest#az-group-create) komutuyla hem anahtar kasasını hem de Web uygulamanızı barındırmak için bir kaynak grubu oluşturun:

```azurecli-interactive
az group create --name "myResourceGroup" -l "EastUS"
```

## <a name="set-up-your-key-vault"></a>Anahtar kasanızı ayarlama

Artık bir Anahtar Kasası oluşturacak ve bu öğreticide daha sonra kullanmak üzere bir gizli dizi oluşturacak.

Bir Anahtar Kasası oluşturmak için [az keykasa Create](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) komutunu kullanın:

> [!Important]
> Her Anahtar Kasası benzersiz bir ada sahip olmalıdır. <-keykasasının adı> aşağıdaki örneklerde anahtar kasanızın adıyla değiştirin.

```azurecli-interactive
az keyvault create --name "<your-keyvault-name>" -g "myResourceGroup"
```

Döndürülen `vaultUri` "https://<-keykasasının adı>. Vault.Azure.net/" biçiminde olacak döndürülen bir kopyasını oluşturun. [Kodu Güncelleştir](#update-the-code) adımında kullanılacaktır.

Artık [az keykasasecret set](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-set) komutuyla anahtar kasanıza bir gizli dizi yerleştirebilirsiniz. Gizli anahtar adını "MySecret" olarak ve değeri "başarılı!" olarak ayarlayın.

```azurecli-interactive
az keyvault secret set --vault-name "<your-keyvault-name>" --name "MySecret" --value "Success!"
```

## <a name="create-a-net-web-app"></a>.NET web uygulaması oluşturma

### <a name="create-a-local-app"></a>Yerel uygulama oluşturma

Makinenizde bir terminal penceresinde, `akvwebapp` adlı bir dizin oluşturup geçerli dizin olarak değiştirin.

```bash
mkdir akvwebapp
cd akvwebapp
```

Şimdi [DotNet yeni Web](/dotnet/core/tools/dotnet-new) komutuyla yeni bir .NET Core uygulaması oluşturun:

```bash
dotnet new web
```

Azure'a dağıttığınızda nasıl görüneceğini görmek için uygulamayı yerel olarak çalıştırın. 

```bash
dotnet run
```

Bir web tarayıcısı açın ve `http://localhost:5000` konumundaki uygulamaya gidin.

Sayfada görünen örnek uygulamadaki **Merhaba Dünya** iletisini görürsünüz.

### <a name="initialize-the-git-repository"></a>Git deposunu başlatma

Terminal pencerenizde **Ctrl+C** tuşlarına basarak web sunucusundan çıkın.  .NET Core projesi için bir Git deposu başlatın.

```bash
git init
git add .
git commit -m "first commit"
```

### <a name="configure-a-deployment-user"></a>Dağıtım kullanıcısı yapılandırma

FTP ve yerel git, bir *dağıtım kullanıcısı*kullanarak bir Azure Web uygulamasına dağıtabilir. Dağıtım kullanıcısını yapılandırdıktan sonra tüm Azure dağıtımlarınız için kullanabilirsiniz. Hesap düzeyinde dağıtım Kullanıcı adınız ve parolanız, Azure aboneliği kimlik bilgilerinizden farklı. 

Dağıtım kullanıcısını yapılandırmak için [az WebApp Deployment User set](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set) komutunu çalıştırın. Bu yönergelere uygun bir Kullanıcı adı ve parola seçin: 

- Kullanıcı adı Azure içinde benzersiz olmalıdır ve yerel git gönderimleri için ' @ ' sembolünü içermemelidir. 
- Parola en az sekiz karakter uzunluğunda olmalıdır ve şu üç öğeden ikisi vardır: harfler, rakamlar ve semboller. 

```azurecli-interactive
az webapp deployment user set --user-name "<username>" --password "<password>"
```

JSON çıktısı parolayı olarak gösterir `null` . `'Conflict'. Details: 409` hatası alırsanız kullanıcı adını değiştirin. `'Bad Request'. Details: 400` hatası alırsanız daha güçlü bir parola kullanın. 

Web uygulamalarınızı dağıtmak için kullanmak üzere Kullanıcı adınızı ve parolanızı kaydedin.

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

Azure CLı [az appservice plan Create](/cli/azure/appservice/plan?view=azure-cli-latest) komutuyla bir App Service planı oluşturun. Aşağıdaki örnekte `myAppServicePlan` , **ücretsiz** fiyatlandırma katmanında adlı bir App Service planı oluşturulur:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

App Service planı oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

<pre>
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "West Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  &lt; JSON data removed for brevity. &gt;
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
</pre>


### <a name="create-a-remote-web-app"></a>Uzak Web uygulaması oluşturma

App Service planında bir [Azure Web uygulaması](../../app-service/containers/app-service-linux-intro.md) oluşturun `myAppServicePlan` . 

> [!Important]
> Key Vault benzer şekilde, bir Azure Web uygulaması benzersiz bir ada sahip olmalıdır. \<your-webapp-name\>Aşağıdaki örnekleri Web uygulamanızın adıyla değiştirin.


```azurecli-interactive
az webapp create --resource-group "myResourceGroup" --plan "myAppServicePlan" --name "<your-webapp-name>" --deployment-local-git
```

Web uygulaması oluşturulduğunda Azure CLI aşağıda yer alan çıktıdaki gibi bilgiler gösterir:

<pre>
Local git is configured with url of 'https://&lt;username&gt;@&lt;your-webapp-name&gt;.scm.azurewebsites.net/&lt;ayour-webapp-name&gt;.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "clientCertExclusionPaths": null,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "&lt;your-webapp-name&gt;.azurewebsites.net",
  "deploymentLocalGitUrl": "https://&lt;username&gt;@&lt;your-webapp-name&gt;.scm.azurewebsites.net/&lt;your-webapp-name&gt;.git",
  "enabled": true,
  &lt; JSON data removed for brevity. &gt;
}
</pre>


Git uzak URL’si `deploymentLocalGitUrl` özelliği içinde `https://<username>@<your-webapp-name>.scm.azurewebsites.net/<your-webapp-name>.git` biçiminde gösterilir. Bu URL 'yi daha sonra gerekli olduğu gibi kaydedin.

Yeni oluşturduğunuz uygulamanıza gidin. _ &lt; -WebApp-Name>_ , uygulamanızın adıyla değiştirin.

```bash
https://<your-webapp-name>.azurewebsites.net
```

Yeni oluşturulan bir Azure Web uygulaması için varsayılan Web sayfasını görürsünüz.

### <a name="deploy-your-local-app"></a>Yerel uygulamanızı dağıtma

Yerel Terminal penceresine geri döndüğünüzde, *\<deploymentLocalGitUrl-from-create-step>* [Uzak Web uygulaması oluşturma](#create-a-remote-web-app) adımından KAYDETTIĞINIZ git uzak URL 'Siyle değiştirerek yerel git deponuza bir Azure uzak sürümü ekleyin.

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

Aşağıdaki komutla uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Git kimlik bilgileri Yöneticisi kimlik bilgilerini sizden isterse, [bir dağıtım kullanıcısı yapılandırma](#configure-a-deployment-user) adımını kullanarak oluşturduğunuz kimlik bilgilerini kullanın.

```bash
git push azure master
```

Bu komutun çalıştırılması birkaç dakika sürebilir. Çalıştırıldığında, aşağıdaki örneğe benzer bilgiler görüntüler:
<pre>
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 285 bytes | 95.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Deploy Async
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'd6b54472f7'.
remote: Repository path is /home/site/repository
remote: Running oryx build...
remote: Build orchestrated by Microsoft Oryx, https://github.com/Microsoft/Oryx
remote: You can report issues at https://github.com/Microsoft/Oryx/issues
remote:
remote: Oryx Version      : 0.2.20200114.13, Commit: 204922f30f8e8d41f5241b8c218425ef89106d1d, ReleaseTagName: 20200114.13
remote: Build Operation ID: |imoMY2y77/s=.40ca2a87_
remote: Repository Commit : d6b54472f7e8e9fd885ffafaa64522e74cf370e1
.
.
.
remote: Deployment successful.
remote: Deployment Logs : 'https://&lt;your-webapp-name&gt;.scm.azurewebsites.net/newui/jsonviewer?view_url=/api/deployments/d6b54472f7e8e9fd885ffafaa64522e74cf370e1/log'
To https://&lt;your-webapp-name&gt;.scm.azurewebsites.net:443/&lt;your-webapp-name&gt;.git
   d87e6ca..d6b5447  master -> master
</pre>

Web tarayıcınızı kullanarak dağıtılan uygulamayı inceleyin veya yenileyin.

```bash
http://<your-webapp-name>.azurewebsites.net
```

"Merhaba Dünya!" görüntülenir ziyaret edildiğinde daha önce gördüğünüz ileti `http://localhost:5000` .

## <a name="create-and-assign-a-managed-identity"></a>Yönetilen kimlik oluşturma ve atama

Azure CLı 'de, bu uygulamanın kimliğini oluşturmak için [az WebApp-Identity Assign](/cli/azure/webapp/identity?view=azure-cli-latest#az-webapp-identity-assign) komutunu çalıştırın:

```azurecli-interactive
az webapp identity assign --name "<your-webapp-name>" --resource-group "myResourceGroup"
```

İşlem bu JSON kod parçacığını döndürür:

```json
{
  "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "SystemAssigned"
}
```

Web uygulamanıza, Anahtar Kasanızda **Get** ve **Listeleme** işlemlerine Izin vermek için, PRINCIPALıD 'yi Azure CLI [az keykasası Set-Policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutuna geçirin:

```azurecli-interactive
az keyvault set-policy --name "<your-keyvault-name>" --object-id "<principalId>" --secret-permissions get list
```


## <a name="modify-the-app-to-access-your-key-vault"></a>Anahtar kasanıza erişmek için uygulamayı değiştirin

### <a name="install-the-packages"></a>Paketleri yükler

Terminal penceresinde, .NET paketleri için Azure Key Vault istemci Kitaplığı ' nı yükledikten sonra:

```console
dotnet add package Azure.Identity
dotnet add package Azure.Security.KeyVault.Secrets
```

### <a name="update-the-code"></a>Kodu güncelleştirme

Startup.cs dosyasını, akvwebapp projenizde bulun ve açın. 

Bu iki satırı üstbilgiye ekleyin:

```csharp
using Azure.Identity;
using Azure.Security.KeyVault.Secrets;
using Azure.Core;
```

Çağrıdan önce bu satırları ekleyin `app.UseEndpoints` , URI 'yi anahtar kasasının yansıtacak şekilde güncelleyerek `vaultUri` . Aşağıdaki kod, kimlik doğrulaması için uygulama tarafından yönetilen kimliğin belirtecini kullanan Anahtar Kasası kimlik doğrulaması için [' DefaultAzureCredential () '](/dotnet/api/azure.identity.defaultazurecredential?view=azure-dotnet) kullanıyor. Ayrıca, Anahtar Kasası kısıtlanmadan yeniden denemeler için üstel geri alma özelliği de kullanılıyor.

```csharp
SecretClientOptions options = new SecretClientOptions()
    {
        Retry =
        {
            Delay= TimeSpan.FromSeconds(2),
            MaxDelay = TimeSpan.FromSeconds(16),
            MaxRetries = 5,
            Mode = RetryMode.Exponential
         }
    };
var client = new SecretClient(new Uri("https://<your-unique-key-vault-name>.vault.azure.net/"), new DefaultAzureCredential(),options);

KeyVaultSecret secret = client.GetSecret("mySecret");

string secretValue = secret.Value;
```

Okunacak satırı güncelleştirin `await context.Response.WriteAsync("Hello World!");` :

```csharp
await context.Response.WriteAsync(secretValue);
```

Sonraki adıma geçmeden önce yaptığınız değişiklikleri kaydetmek için bazı değişiklikler yapın.

### <a name="redeploy-your-web-app"></a>Web uygulamanızı yeniden dağıtın

Kodunuzu güncelleştirdikten sonra, aşağıdaki git komutlarıyla Azure 'a yeniden dağıtabilirsiniz:

```bash
git add .
git commit -m "Updated web app to access my key vault"
git push azure master
```

## <a name="visit-your-completed-web-app"></a>Tamamlanmış Web uygulamanızı ziyaret edin

```bash
http://<your-webapp-name>.azurewebsites.net
```

**Merhaba Dünya**gördüğünüz, artık gizli dizi değerinin görüntülendiğini görmeniz gerekir: **başarılı!**

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için Yönetilen kimlikler](../../active-directory/managed-identities-azure-resources/overview.md) hakkında daha fazla bilgi edinin
- [App Service için Yönetilen kimlikler](../../app-service/overview-managed-identity.md?tabs=dotnet) hakkında daha fazla bilgi edinin
- Bkz. [.NET API başvurusu için Azure Key Vault istemci kitaplığı](/dotnet/api/overview/azure/key-vault?view=azure-dotnet)
- Bkz. [.net kaynak kodu için Azure Key Vault istemci kitaplığı](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/keyvault)
- [.Net NuGet paketi için v4 Azure Key Vault istemci kitaplığı](https://www.nuget.org/packages/Azure.Security.KeyVault.Secrets/) ' na bakın


