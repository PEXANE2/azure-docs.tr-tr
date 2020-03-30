---
title: Azure Cosmos DB ve Active Directory ile sertifika tabanlı kimlik doğrulama
description: Azure Cosmos DB tuşlarına erişmek için sertifika tabanlı kimlik doğrulama için Azure AD kimliğini nasıl yapılandıracaksınız öğrenin.
author: voellm
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/11/2019
ms.author: tvoellm
ms.reviewer: sngun
ms.openlocfilehash: 085280a8064e4d12ac63939ada7cdb296d47dc70
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80365784"
---
# <a name="certificate-based-authentication-for-an-azure-ad-identity-to-access-keys-from-an-azure-cosmos-db-account"></a>Bir Azure Cosmos DB hesabından anahtarlara erişmek için Azure AD kimliği için sertifika tabanlı kimlik doğrulaması

Sertifika tabanlı kimlik doğrulaması, istemci uygulamanızın Azure Active Directory (Azure AD) kullanılarak bir istemci sertifikasıyla kimliğinin doğrulanmasına olanak tanır. Sertifika tabanlı kimlik doğrulamasını şirket içindeki makine veya Azure’daki sanal makine gibi kimliğinizin olmasını gerektiren bir makinede gerçekleştirebilirsiniz. Uygulamanız daha sonra tuşları doğrudan uygulamada olmadan Azure Cosmos DB tuşlarını okuyabilir. Bu makalede, örnek bir Azure AD uygulamasının nasıl oluşturulup sertifika tabanlı kimlik doğrulaması için yapılandırıştırılabilmek, yeni uygulama kimliğini kullanarak Azure'da oturum açma ve ardından Azure Cosmos hesabınızdan anahtarları nasıl alacak. Bu makalede, kimlikleri ayarlamak için Azure PowerShell kullanır ve Azure Cosmos hesabınızdan anahtarları doğrulayan ve bunlara erişen bir C# örnek uygulaması sağlar.  

## <a name="prerequisites"></a>Ön koşullar

* Azure PowerShell'in [en son sürümünü](/powershell/azure/install-az-ps) yükleyin.

* [Azure aboneliğiniz](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing) yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

## <a name="register-an-app-in-azure-ad"></a>Bir Uygulamayı Azure AD'ye kaydetme

Bu adımda, Azure REKLAM hesabınıza örnek bir web uygulaması kaydedebilirsiniz. Bu uygulama daha sonra Azure Cosmos DB hesabınızdaki anahtarları okumak için kullanılır. Bir uygulamayı kaydetmek için aşağıdaki adımları kullanın: 

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. Azure **Etkin Dizin** bölmesini açın, **Uygulama kayıtları** bölmesine gidin ve **Yeni kayıt'ı**seçin. 

   ![Active Directory'de yeni başvuru kaydı](./media/certificate-based-authentication/new-app-registration.png)

1. **Kayıt** başvuru formunu aşağıdaki ayrıntılarla doldurun:  

   * **Ad** – Uygulamanız için bir ad verin, "sampleApp" gibi herhangi bir ad olabilir.
   * **Desteklenen hesap türleri** – Geçerli dizininizdeki kaynakların bu uygulamaya erişmesine izin vermek için **yalnızca bu kuruluş dizinindeki Hesapları (Varsayılan Dizini)** seçin. 
   * **URL'yi Yeniden Yönlendirme** – **Web** türü uygulamasını seçin ve uygulamanızın barındırıldığı bir URL sağlayın, herhangi bir URL olabilir. Bu örnekte, uygulama yok olsa `https://sampleApp.com` bile sorun olmadığı gibi bir test URL'si sağlayabilirsiniz.

   ![Örnek bir web uygulaması nın kaydedilmesi](./media/certificate-based-authentication/register-sample-web-app.png)

1. Formu doldurduktan sonra **Kaydol'u** seçin.

1. Uygulama kaydedildikten **sonra, Uygulama(istemci) kimliği** ve **Nesne Kimliği**bir not yapmak, sonraki adımlarda bu ayrıntıları kullanacaktır. 

   ![Uygulama ve nesne dislerini alma](./media/certificate-based-authentication/get-app-object-ids.png)

## <a name="install-the-azuread-module"></a>AzureAD modüllerini yükleme

Bu adımda Azure AD PowerShell modüllerini yüklersiniz. Bu modül, bir önceki adımda kaydettiğiniz başvurunun kimliğini almak ve kendi imzalı bir sertifikayı bu uygulamaya ilişkilendirmek için gereklidir. 

1. Windows PowerShell ISE'yi yönetici haklarıyla açın. Henüz yapmadıysanız, AZ PowerShell modülünü yükleyin ve aboneliğinize bağlanın. Birden çok aboneliğiniz varsa, geçerli aboneliğin bağlamını aşağıdaki komutlarda gösterildiği gibi ayarlayabilirsiniz:

   ```powershell
   Install-Module -Name Az -AllowClobber
   Connect-AzAccount

   Get-AzSubscription
   $context = Get-AzSubscription -SubscriptionId <Your_Subscription_ID>
   Set-AzContext $context 
   ```

1. [AzureAD](/powershell/module/azuread/?view=azureadps-2.0) modüllerini yükleme ve alma

   ```powershell
   Install-Module AzureAD
   Import-Module AzureAD 
   ```

## <a name="sign-into-your-azure-ad"></a>Azure REKLAM'ınızda oturum açın

Uygulamayı kaydettiğiniz Azure REKLAM'ınızda oturum açın. Hesabınızda oturum açmak için AzureAD bağlantısını kullanın, açılan pencereye Azure hesap kimlik bilgilerinizi girin. 

```powershell
Connect-AzureAD 
```

## <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma

Windows PowerShell ISE'nin başka bir örneğini açın ve kendi imzalı bir sertifika oluşturmak ve sertifikayla ilişkili anahtarı okumak için aşağıdaki komutları çalıştırın:

```powershell
$cert = New-SelfSignedCertificate -CertStoreLocation "Cert:\CurrentUser\My" -Subject "CN=sampleAppCert" -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData()) 
```

## <a name="create-the-certificate-based-credential"></a>Sertifika tabanlı kimlik bilgileri oluşturma 

Sonraki uygulamanızın nesne kimliğini almak ve sertifika tabanlı kimlik oluşturmak için aşağıdaki komutları çalıştırın. Bu örnekte, sertifikayı bir yıl sonra sona erecek şekilde ayarlıyoruz, gerekli bitiş tarihine ayarlayabilirsiniz.

```powershell
$application = Get-AzureADApplication -ObjectId <Object_ID_of_Your_Application>

New-AzureADApplicationKeyCredential -ObjectId $application.ObjectId -CustomKeyIdentifier "Key1" -Type AsymmetricX509Cert -Usage Verify -Value $keyValue -EndDate "2020-01-01"
```

Yukarıdaki komut aşağıdaki ekran görüntüsüne benzer çıktı ile sonuçlanır:

![Sertifika tabanlı kimlik bilgileri oluşturma çıktısı](./media/certificate-based-authentication/certificate-based-credential-output.png)

## <a name="configure-your-azure-cosmos-account-to-use-the-new-identity"></a>Azure Cosmos hesabınızı yeni kimliği kullanacak şekilde yapılandırın

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. Azure Cosmos hesabınıza gidin, **Access denetimi (IAM)** bıçaklarını açın.

1. Rol Atama **ekle** ve **ekle'yi**seçin. Önceki adımda oluşturduğunuz örnekApp'ı aşağıdaki ekran görüntüsünde gösterildiği gibi **Katkıda Bulunan** rolüyle ekleyin:

   ![Azure Cosmos hesabını yeni kimliği kullanacak şekilde yapılandırın](./media/certificate-based-authentication/configure-cosmos-account-with-identify.png)

1. Formu doldurduktan sonra **Kaydet'i** seçin

## <a name="register-your-certificate-with-azure-ad"></a>Sertifikanızı Azure AD ile kaydedin

Azure portalından Azure AD'deki istemci uygulamasıyla sertifika tabanlı kimlik bilgisini ilişkilendirebilirsiniz. Kimlik belgesini ilişkilendirmek için sertifika dosyasını aşağıdaki adımlarla yüklemeniz gerekir:

İstemci uygulaması için Azure uygulama kaydında:

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. Azure **Etkin Dizin** bölmesini açın, **Uygulama kayıtları** bölmesine gidin ve önceki adımda oluşturduğunuz örnek uygulamayı açın. 

1. **Sertifikalar & sırları** seçin ve ardından **sertifika yükleyin.** Yüklemek için önceki adımda oluşturduğunuz sertifika dosyasına göz atın.

1. **Ekle'yi**seçin. Sertifika yüklendikten sonra parmak izi, başlangıç tarihi ve son kullanma değerleri görüntülenir.

## <a name="access-the-keys-from-powershell"></a>PowerShell'den anahtarlara erişin

Bu adımda, uygulamayı ve oluşturduğunuz sertifikayı kullanarak Azure'da oturum açacak ve Azure Cosmos hesabınızın anahtarlarına erişebilirsiniz. 

1. Başlangıçta hesabınızda oturum açtırdığınız Azure hesabının kimlik bilgilerini temizleyin. Aşağıdaki komutu kullanarak kimlik bilgilerini temizleyebilirsiniz:

   ```powershell
   Disconnect-AzAccount -Username <Your_Azure_account_email_id> 
   ```

1. Ardından, uygulamanın kimlik bilgilerini kullanarak Azure portalında oturum açabileceğinizi ve Azure Cosmos DB tuşlarına erişebileceğinizi doğrulayın:

   ```powershell
   Login-AzAccount -ApplicationId <Your_Application_ID> -CertificateThumbprint $cert.Thumbprint -ServicePrincipal -Tenant <Tenant_ID_of_your_application>

   Get-AzCosmosDBAccountKey `
      -ResourceGroupName "<Resource_Group_Name_of_your_Azure_Cosmos_account>" `
      -Name "<Your_Azure_Cosmos_Account_Name>" `
      -Type "Keys"
   ```

Önceki komut, Azure Cosmos hesabınızın birincil ve ikincil ana anahtarlarını görüntüler. Alma anahtarları isteğinin başarılı olduğunu ve etkinliğin "sampleApp" uygulaması tarafından başlatıldığını doğrulamak için Azure Cosmos hesabınızın Etkinlik günlüğünü görüntüleyebilirsiniz.

![Azure AD'deki get keys çağrısını doğrulayın](./media/certificate-based-authentication/activity-log-validate-results.png)

## <a name="access-the-keys-from-a-c-application"></a>C# uygulamasından anahtarlara erişin 

C# uygulamasından anahtarlara erişerek de bu senaryoyu doğrulayabilirsiniz. Active Directory'de kayıtlı uygulamayı kullanarak Azure Cosmos DB tuşlarına erişebilen aşağıdaki C# konsol uygulaması. Kodu çalıştırmadan önce kiracı Kimliği, clientID, certName, kaynak grubu adı, abonelik kimliği, Azure Cosmos hesap adı ayrıntılarını güncelleştirdiğinizden emin olun. 

```csharp
using System;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Linq;
using System.Net.Http;
using System.Security.Cryptography.X509Certificates;
using System.Threading;
using System.Threading.Tasks;
 
namespace TodoListDaemonWithCert
{
    class Program
    {
        private static string aadInstance = "https://login.windows.net/";
        private static string tenantId = "<Your_Tenant_ID>";
        private static string clientId = "<Your_Client_ID>";
        private static string certName = "<Your_Certificate_Name>";
 
        private static int errorCode = 0;
        static int Main(string[] args)
        {
            MainAync().Wait();
            Console.ReadKey();
 
            return 0;
        } 
 
        static async Task MainAync()
        {
            string authContextURL = aadInstance + tenantId;
            AuthenticationContext authContext = new AuthenticationContext(authContextURL);
            X509Certificate2 cert = ReadCertificateFromStore(certName);
 
            ClientAssertionCertificate credential = new ClientAssertionCertificate(clientId, cert);
            AuthenticationResult result = await authContext.AcquireTokenAsync("https://management.azure.com/", credential);
            if (result == null)
            {
                throw new InvalidOperationException("Failed to obtain the JWT token");
            }
 
            string token = result.AccessToken;
            string subscriptionId = "<Your_Subscription_ID>";
            string rgName = "<ResourceGroup_of_your_Cosmos_account>";
            string accountName = "<Your_Cosmos_account_name>";
            string cosmosDBRestCall = $"https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{rgName}/providers/Microsoft.DocumentDB/databaseAccounts/{accountName}/listKeys?api-version=2015-04-08";
 
            Uri restCall = new Uri(cosmosDBRestCall);
            HttpClient httpClient = new HttpClient();
            httpClient.DefaultRequestHeaders.Remove("Authorization");
            httpClient.DefaultRequestHeaders.Add("Authorization", "Bearer " + token);
            HttpResponseMessage response = await httpClient.PostAsync(restCall, null);
 
            Console.WriteLine("Got result {0} and keys {1}", response.StatusCode.ToString(), response.Content.ReadAsStringAsync().Result);
        }
 
        /// <summary>
        /// Reads the certificate
        /// </summary>
        private static X509Certificate2 ReadCertificateFromStore(string certName)
        {
            X509Certificate2 cert = null;
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            store.Open(OpenFlags.ReadOnly);
            X509Certificate2Collection certCollection = store.Certificates;
 
            // Find unexpired certificates.
            X509Certificate2Collection currentCerts = certCollection.Find(X509FindType.FindByTimeValid, DateTime.Now, false);
 
            // From the collection of unexpired certificates, find the ones with the correct name.
            X509Certificate2Collection signingCert = currentCerts.Find(X509FindType.FindBySubjectName, certName, false);
 
            // Return the first certificate in the collection, has the right name and is current.
            cert = signingCert.OfType<X509Certificate2>().OrderByDescending(c => c.NotBefore).FirstOrDefault();
            store.Close();
            return cert;
        }
    }
}
```

Bu komut dosyası, aşağıdaki ekran görüntüsünde gösterildiği gibi birincil ve ikincil ana anahtarları çıkar:

![csharp uygulama çıktısı](./media/certificate-based-authentication/csharp-application-output.png)

Önceki bölüme benzer şekilde, "sampleApp" uygulaması tarafından anahtar alma isteği etkinliğinin başlatıldığını doğrulamak için Azure Cosmos hesabınızın Etkinlik günlüğünü görüntüleyebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Key Vault kullanarak Azure Cosmos anahtarlarının güvenliğini sağlama](access-secrets-from-keyvault.md)

* [Azure Cosmos DB için güvenlik taban çizgisi](security-baseline.md)
