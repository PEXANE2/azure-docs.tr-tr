---
title: Azure Anahtar Kasası'nı uçdan uca anahtar döndürme ve denetlemeyle ayarlama | Microsoft Dokümanlar
description: Anahtar döndürmeyi ayarlamanıza ve anahtar kasa günlüklerini izlemenize yardımcı olmak için bu nasıl yapılacağını kılavuzunu kullanın.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: ''
ms.service: key-vault
ms.subservice: secrets
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: d2981495a256ce5fb8f8f3584e68ac91541f9d62
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81430259"
---
# <a name="set-up-azure-key-vault-with-key-rotation-and-auditing"></a>Önemli döndürme ve denetleme yle Azure Anahtar Kasası'nı ayarlama

## <a name="introduction"></a>Giriş

Bir anahtar kasa sonra, anahtarları ve sırlarını saklamak için kullanmaya başlayabilirsiniz. Uygulamalarınızın artık anahtarlarınızı veya sırlarınızı devam etmesi gerekmez, ancak gerektiğinde kasadan talep edebilirsiniz. Anahtar kasası, uygulamanızın davranışını etkilemeden tuşları ve sırları güncellemenizi sağlar ve bu da anahtar ve gizli yönetiminiz için çok çeşitli olanaklar sunar.

Bu makalede, depolama hesabı anahtarlarının zamanlanmış bir döndürmesi nasıl uygulanacağı, anahtar kasa denetim günlüklerinin nasıl izlendiği ve beklenmeyen isteklerde bulunulduğunda uyarıların nasıl yükseltilen olduğu ele alınmaktadır. 

Öncelikle seçtiğiniz yöntemi kullanarak bir anahtar kasası oluşturmanız gerekir:

- [Azure CLI'yi kullanarak Azure Key Vault'tan bir sır ayarlayın ve alın](quick-create-cli.md)
- [Azure PowerShell'i kullanarak Azure Key Vault'tan bir sır ayarlayın ve alın](quick-create-powershell.md)
- [Azure portalLarını kullanarak Azure Key Vault'tan bir sır ayarlayın ve alın](quick-create-portal.md)


## <a name="store-a-secret"></a>Bir sırrı saklayın

Bir uygulamanın Key Vault'tan bir sır alabilmesi için önce sırrı oluşturmalı ve kasanıza yüklemeniz gerekir.

Bir Azure PowerShell oturumu başlatın ve aşağıdaki komutla Azure hesabınızda oturum açın:

```powershell
Connect-AzAccount
```

Açılan tarayıcı penceresinde Azure hesabınızın kullanıcı adını ve parolasını girin. PowerShell bu hesapla ilişkili tüm abonelikleri alır. PowerShell varsayılan olarak ilkini kullanır.

Birden çok aboneliğiniz varsa, anahtar kasanızı oluşturmak için kullanılan aboneliği belirtmeniz gerekebilir. Hesabınızın aboneliklerini görmek için aşağıdakileri girin:

```powershell
Get-AzSubscription
```

Günlüğe kaydolacağınız anahtar kasasıyla ilişkili aboneliği belirtmek için girin:

```powershell
Set-AzContext -SubscriptionId <subscriptionID>
```

Bu makalede, bir depolama hesabı anahtarının gizli olarak depolandığı gösterildiğinden, bu depolama hesabı anahtarını almanız gerekir.

```powershell
Get-AzStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Sırrınızı aldıktan sonra (bu durumda, depolama hesabı anahtarınız), bu anahtarı güvenli bir dize dönüştürmeniz ve ardından anahtar kasanızda bu değere sahip bir sır oluşturmanız gerekir.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```

Sonra, yarattığınız sır için URI'yi getirin. Anahtar kasasını aramak ve sırrınızı geri almak için bu URI'ye daha sonraki bir adımda ihtiyacınız olacak. Aşağıdaki PowerShell komutunu çalıştırın ve sırrın URI'si olan kimlik değerini not edin:

```powershell
Get-AzKeyVaultSecret –VaultName <vaultName>
```

## <a name="set-up-the-application"></a>Uygulamayı ayarlama

Artık gizli bir deponuz olduğuna göre, birkaç adım daha yaptıktan sonra kodu almak ve kullanmak için kod kullanabilirsiniz.

Öncelikle uygulamanızı Azure Active Directory'ye kaydettirmelisiniz. Ardından Key Vault'a başvuru bilgilerinizi söyleyin, böylece uygulamanızdan gelen isteklere izin verebilir.

> [!NOTE]
> Uygulamanız, anahtar kasanızla aynı Azure Active Directory kiracısında oluşturulmalıdır.

1. **Azure Etkin Dizini**Açın.
2. **Uygulama kayıtları**'nı seçin. 
3. Azure Active Directory'ye uygulama eklemek için **Yeni uygulama kaydı'nı** seçin.

    ![Azure Active Directory'de uygulamaları açın](../media/keyvault-keyrotation/azure-ad-application.png)

4. **Create**altında, uygulama türünü **Web uygulaması / API** olarak bırakın ve uygulamanıza bir ad verin. Başvurunuza oturum **açma URL'si**verin. Bu URL, bu demo için istediğiniz her şey olabilir.

    ![Uygulama kaydı oluşturma](../media/keyvault-keyrotation/create-app.png)

5. Uygulama Azure Etkin Dizini'ne eklendikten sonra uygulama sayfası açılır. **Ayarlar'ı**seçin ve ardından **Özellikler'i**seçin. Uygulama **Kimliği** değerini kopyalayın. Daha sonraki adımlarda ihtiyacınız olacak.

Ardından, Azure Etkin Dizini ile etkileşimkurabilmesi için uygulamanız için bir anahtar oluşturun. Bir anahtar oluşturmak için Ayarlar altında **Anahtarlar'ı** seçin. **Settings** Azure Active Directory uygulamanız için yeni oluşturulan anahtara dikkat edin. Daha sonraki bir adımda gerekecektir. Bu bölümden ayrıldıktan sonra anahtar kullanılamaz. 

![Azure Active Directory uygulama anahtarları](../media/keyvault-keyrotation/create-key.png)

Başvurunuzdan anahtar kasasına herhangi bir arama yapmadan önce, anahtar kasasına başvurunuz ve izinleri hakkında bilgi vermelisiniz. Aşağıdaki komut, azure Active Directory uygulamanızdan kasa adını ve uygulama kimliğini kullanır ve uygulamaya anahtar kasanıza erişim **hakkı** verir.

```powershell
Set-AzKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

Artık başvuru çağrılarınızı oluşturmaya hazırsınız. Uygulamanızda, Azure Anahtar Kasası ve Azure Etkin Dizini ile etkileşim kurmak için gereken NuGet paketlerini yüklemeniz gerekir. Visual Studio Package Manager konsolundan aşağıdaki komutları girin. Bu makalenin yazımında, Azure Active Directory paketinin geçerli sürümü 3.10.305231913'tür, bu nedenle en son sürümü onaylayın ve gerektiğinde güncelleyin.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

Uygulama kodunuzda, Azure Etkin Dizin kimlik doğrulamanızın yöntemini tutmak için bir sınıf oluşturun. Bu örnekte, bu sınıf **Utils**denir. Aşağıdaki `using` ifadeyi ekleyin:

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Ardından, Azure Etkin Dizini'nden JWT belirteci almak için aşağıdaki yöntemi ekleyin. Sürdürülebilirlik için, sabit kodlanmış dize değerlerini web veya uygulama yapılandırmanıza taşımak isteyebilirsiniz.

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

Key Vault'u aramak ve gizli değerinizi almak için gerekli kodu ekleyin. Öncelikle aşağıdaki `using` ifadeyi eklemeniz gerekir:

```csharp
using Microsoft.Azure.KeyVault;
```

Key Vault çağırmak ve sırrınızı almak için yöntem aramaları ekleyin. Bu yöntemde, önceki adımda kaydettiğiniz gizli URI'yi sağlarsınız. Daha önce oluşturduğunuz **Utils** sınıfından **GetToken** yönteminin kullanımına dikkat edin.

```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Uygulamanızı çalıştırdığınızda, artık Azure Active Directory'nin kimliğini doğrulamanız ve ardından gizli değerinizi Azure Key Vault'tan almanız gerekir.

## <a name="key-rotation-using-azure-automation"></a>Azure Otomasyonu kullanarak anahtar döndürme

> [!IMPORTANT]
> Azure Otomasyon runbook'ları hala `AzureRM` modülün kullanımını gerektirir.

Artık Key Vault sırları olarak depoladığınız değerler için bir rotasyon stratejisi kurmaya hazırsınız. Sırlar çeşitli şekillerde döndürülebilir:

- Manuel işlemin bir parçası olarak
- API çağrılarını kullanarak programlanabilir
- Azure Otomasyon komut dosyası aracılığıyla

Bu makalenin amaçları doğrultusunda, Azure depolama hesabının erişim anahtarını değiştirmek için Azure Otomasyonu ile birlikte PowerShell'i kullanırsınız. Daha sonra bu yeni anahtar ile önemli bir kasa sırrı güncelleyin.

Azure Otomasyonu'nun anahtar kasanızda gizli değerler ayarlamasına izin vermek için **AzureRunAsConnection**adlı bağlantı için istemci kimliğini almanız gerekir. Bu bağlantı, Azure Otomasyon uçağını kurduğunuzda oluşturdu. Bu kimliği bulmak için Azure Otomasyon uyrunuzdan **Varlıklar'ı** seçin. Buradan **Bağlantılar'ı**seçin ve ardından **AzureRunAsConnection** hizmet ilkesini seçin. **ApplicationId** değerini not edin.

![Azure Otomasyon istemci kimliği](../media/keyvault-keyrotation/Azure_Automation_ClientID.png)

**Varlıklar'da** **Modülleri**seçin. **Galeri'yi**seçin ve ardından aşağıdaki modüllerin her birinin güncelleştirilmiş sürümlerini arayın ve içe aktarın:

    Azure
    Azure.Storage
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage

> [!NOTE]
> Bu makalenin yazımında, yalnızca daha önce kayda geçen modüllerin aşağıdaki komut dosyası için güncellenmesi gerekir. Otomasyon işiniz başarısız olursa, gerekli tüm modülleri ve bunların bağımlılıklarını içe aktardığınızı onaylayın.

Azure Otomasyon bağlantınız için uygulama kimliğini aldıktan sonra, anahtar kasanıza bu uygulamanın kasanızdaki sırları güncelleştirme izni olduğunu söylemeniz gerekir. Aşağıdaki PowerShell komutunu kullanın:

```powershell
Set-AzKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Ardından, Azure Otomasyonu örneğinizin altındaki **Runbook'ları** seçin ve ardından **Runbook Ekle'yi**seçin. **Hızlı Oluştur**’u seçin. Runbook'unuzu adlandırın ve runbook türü olarak **PowerShell'i** seçin. Açıklama ekleyebilirsiniz. Son olarak, **Oluştur'u**seçin.

![Runbook oluşturma](../media/keyvault-keyrotation/Create_Runbook.png)

Yeni runbook için editör bölmesine aşağıdaki PowerShell komut dosyası yapıştırın:

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection"
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Connect-AzureRmAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

# Optionally you can set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

Düzenleyici bölmesinde, komut dosyanızı sınamak için **Test bölmesini** seçin. Komut dosyası hatasız çalıştırdıktan sonra **Yayımla'yı**seçebilir ve runbook yapılandırma bölmesinde runbook için bir zamanlama uygulayabilirsiniz.

## <a name="key-vault-auditing-pipeline"></a>Key Vault denetim boru hattı

Bir anahtar kasası ayarladığınızda, anahtar kasasına yapılan erişim isteklerinin günlüklerini toplamak için denetimi açabilirsiniz. Bu günlükler belirlenmiş bir Azure depolama hesabında depolanır ve çıkarılabilir, izlenebilir ve analiz edilebilir. Aşağıdaki senaryo, web uygulamasının uygulama kimliğiyle eşleşmeyen bir uygulama kasadan sırları aldığında e-posta gönderen bir ardışık hat lar oluşturmak için Azure işlevlerini, Azure mantık uygulamalarını ve anahtar kasa denetim günlüklerini kullanır.

İlk olarak, anahtar kasasında oturum açmayı etkinleştirmelisiniz. Aşağıdaki PowerShell komutlarını kullanın. [(Anahtar-tonoz-günlük ile ilgili bu makalede](../general/logging.md)tüm ayrıntıları görebilirsiniz.)

```powershell
$sa = New-AzStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzKeyVault -VaultName '<vaultName>'
Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Category AuditEvent
```

Günlüğe kaydetme etkinleştirildikten sonra, denetim günlükleri belirlenen depolama hesabında depolanmaya başlar. Bu günlükler, anahtar kasalarınıza nasıl ve ne zaman ve kim tarafından erişilenlerle ilgili olayları içerir.

> [!NOTE]
> Anahtar kasa işleminden 10 dakika sonra günlük bilgilerinize erişebilirsiniz. Genellikle bundan daha erken satışa sunulacak.

Bir sonraki [adım, bir Azure Hizmet Veri Servisi kuyruğu oluşturmaktır.](../../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md) Bu sıra, anahtar kasa denetim günlüklerinin itildiği yerdir. Denetim günlüğü iletileri sırada olduğunda, mantık uygulaması onları alır ve üzerlerinde hareket eder. Aşağıdaki adımları içeren bir Hizmet Veri Servisi örneği oluşturun:

1. Hizmet Veri Servisi ad alanı oluşturun (zaten kullanmak istediğiniz bir tane varsa, adım 2'ye atlayın).
2. Azure portalındaki Hizmet Veri Servisi örneğine göz atın ve kuyruk oluşturmak istediğiniz ad alanını seçin.
3. Kaynak > **Kurumsal Tümleştirme** > Hizmeti Veri**Servisi** **Oluştur'u**seçin ve ardından gerekli ayrıntıları girin.
4. Ad alanını seçip **ardından Bağlantı Bilgilerini**seçerek Hizmet Veri Servisi bağlantı bilgilerini bulun. Bir sonraki bölüm için bu bilgilere ihtiyacınız olacak.

Ardından, depolama hesabındaki anahtar kasa günlüklerini yoklamak ve yeni etkinlikler almak için [bir Azure işlevi oluşturun.](../../azure-functions/functions-create-first-azure-function.md) Bu işlev bir zamanlamada tetiklenir.

Bir Azure işlev uygulaması oluşturmak için **kaynak oluştur'u,** **İşlev Uygulaması**için pazarda arama yapın'ı ve ardından **Oluştur'u**seçin. Oluşturma sırasında, varolan bir barındırma planı kullanabilir veya yeni bir barındırma planı oluşturabilirsiniz. Ayrıca dinamik barındırma için tercih edebilirsiniz. Azure İşlevleri için barındırma seçenekleri hakkında daha fazla bilgi için [Azure İşlevlerini nasıl ölçeklendirin'](../../azure-functions/functions-scale.md)e bakın.

Azure işlev uygulaması oluşturulduktan sonra uygulamanın üzerine gidin ve dil için **Zamanlayıcı** senaryosunu ve **\# C'yi** seçin. Sonra **bu işlevi oluştur'u**seçin.

![Azure Fonksiyonları Başlangıç bıçağı](../media/keyvault-keyrotation/Azure_Functions_Start.png)

**Geliştir** sekmesinde, run.csx kodunu aşağıdakilerle değiştirin:

```csharp
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.ServiceBus.Messaging;
using System.Text;

public static void Run(TimerInfo myTimer, TextReader inputBlob, TextWriter outputBlob, TraceWriter log)
{
    log.Info("Starting");

    CloudStorageAccount sourceStorageAccount = new CloudStorageAccount(new StorageCredentials("<STORAGE_ACCOUNT_NAME>", "<STORAGE_ACCOUNT_KEY>"), true);

    CloudBlobClient sourceCloudBlobClient = sourceStorageAccount.CreateCloudBlobClient();

    var connectionString = "<SERVICE_BUS_CONNECTION_STRING>";
    var queueName = "<SERVICE_BUS_QUEUE_NAME>";

    var sbClient = QueueClient.CreateFromConnectionString(connectionString, queueName);

    DateTime dtPrev = DateTime.UtcNow;
    if(inputBlob != null)
    {
        var txt = inputBlob.ReadToEnd();

        if(!string.IsNullOrEmpty(txt))
        {
            dtPrev = DateTime.Parse(txt);
            log.Verbose($"SyncPoint: {dtPrev.ToString("O")}");
        }
        else
        {
            dtPrev = DateTime.UtcNow;
            log.Verbose($"Sync point file didn't have a date. Setting to now.");
        }
    }

    var now = DateTime.UtcNow;

    string blobPrefix = "insights-logs-auditevent/resourceId=/SUBSCRIPTIONS/<SUBSCRIPTION_ID>/RESOURCEGROUPS/<RESOURCE_GROUP_NAME>/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/<KEY_VAULT_NAME>/y=" + now.Year +"/m="+now.Month.ToString("D2")+"/d="+ (now.Day).ToString("D2")+"/h="+(now.Hour).ToString("D2")+"/m=00/";

    log.Info($"Scanning:  {blobPrefix}");

    IEnumerable<IListBlobItem> blobs = sourceCloudBlobClient.ListBlobs(blobPrefix, true);

    log.Info($"found {blobs.Count()} blobs");

    foreach(var item in blobs)
    {
        if (item is CloudBlockBlob)
        {
            CloudBlockBlob blockBlob = (CloudBlockBlob)item;

            log.Info($"Syncing: {item.Uri}");

            string sharedAccessUri = GetContainerSasUri(blockBlob);

            CloudBlockBlob sourceBlob = new CloudBlockBlob(new Uri(sharedAccessUri));

            string text;
            using (var memoryStream = new MemoryStream())
            {
                sourceBlob.DownloadToStream(memoryStream);
                text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
            }

            dynamic dynJson = JsonConvert.DeserializeObject(text);

            //Required to order by time as they might not be in the file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending to ServiceBus, use the payloadStream and set keeporiginal to true
                    var message = new BrokeredMessage(payloadStream, true);
                    sbClient.Send(message);
                    dtPrev = dt;
                }
            }
        }
    }
    outputBlob.Write(dtPrev.ToString("o"));
}

static string GetContainerSasUri(CloudBlockBlob blob)
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```

> [!NOTE]
> Önceki koddaki değişkenleri, anahtar kasa günlüklerinin yazıldığı depolama hesabınıza, daha önce oluşturduğunuz Hizmet Veri Yolu örneğine ve anahtar kasa depolama günlüklerine giden belirli yolu işaret etmek üzere değiştirin.

İşlev, anahtar kasa günlüklerinin yazıldığı depolama hesabından en son günlük dosyasını alır, bu dosyadaki en son olayları yakalar ve servis veri hizmeti kuyruğuna iter. 

Tek bir dosyada birden çok olay olabileceğinden, işlevin de ele alınan son olayın zaman damgasını belirlemek için baktığı bir eşitleme.txt dosyası oluşturmanız gerekir. Bu dosyayı kullanmak, aynı olayı birden çok kez zorlamamanızı sağlar. 

Sync.txt dosyası, son karşılaşılan olay için bir zaman damgası içerir. Günlükler yüklendiğinde, doğru şekilde sipariş edildiklerinden emin olmak için zaman damgalarına göre sıralanmalıdır.

Bu işlev için, Azure İşlevler'de kutunun dışında bulunmayan birkaç ek kitaplıktan başvuruluyuz. Bu kitaplıkları eklemek için, NuGet'i kullanarak bunları çekmek için Azure Fonksiyonları'na ihtiyacımız vardır. **Kod** kutusunun altında **Dosyaları Görüntüle'yi**seçin.

!["Dosyaları görüntüle" seçeneği](../media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

Project.json adlı dosyayı aşağıdaki içeriğe ekleyin:

```json
    {
      "frameworks": {
        "net46":{
          "dependencies": {
                "WindowsAzure.Storage": "7.0.0",
                "WindowsAzure.ServiceBus":"3.2.2"
          }
        }
       }
    }
```

**Kaydet'i**seçtikten sonra Azure İşlevleri gerekli ikilileri karşıdan yükleyecek.

**Tümleştir** sekmesine geçin ve zamanlayıcı parametresine işlev içinde kullanılacak anlamlı bir ad verin. Önceki kodda, işlev zamanlayıcının *myTimer*olarak çağrılmasını bekler. Zamanlayıcı için [cron ifadesini](../../app-service/webjobs-create.md#CreateScheduledCRON) aşağıdaki `0 * * * * *`gibi belirtin: . Bu ifade, işlevin dakikada bir çalışmasına neden olur.

Aynı **Tümleştir** sekmesinde, **Azure Blob depolama**türünden bir giriş ekleyin. Bu giriş, işlev tarafından incelenen son olayın zaman damgasını içeren sync.txt dosyasını gösterir. Bu giriş, parametre adı kullanılarak işlev içinde erişilecektir. Önceki kodda, Azure Blob depolama girişi parametre adının *blob*olmasını bekler. sync.txt dosyasının bulunduğu depolama hesabını seçin (aynı veya farklı bir depolama hesabı olabilir). Yol alanında, dosyaya giden yolu biçimde `{container-name}/path/to/sync.txt`sağlayın.

**Azure Blob depolama**türünden bir çıktı ekleyin. Bu çıktı, girişte tanımladığınız sync.txt dosyasını gösterir. Bu çıktı, son olayın baktığı zaman damgasını yazmak için işlev tarafından kullanılır. Önceki kod bu *parametrenin outputBlob*olarak adlandırılmasını bekler.

İşlev artık hazır. **Geliştirme** sekmesine geri dönüp kodu kaydettiğinden emin olun. Tüm derleme hataları için çıktı penceresini denetleyin ve gerektiğinde düzeltin. Kod derleniyorsa, kod artık anahtar kasagünlüklerini her dakika kontrol etmeli ve yeni olayları tanımlanan Hizmet Veri Servisi kuyruğuna itmelidir. İşlev her tetikleninde günlük penceresine günlüğe kaydetme bilgilerinin yazılması gerektiğini görmeniz gerekir.

### <a name="azure-logic-app"></a>Azure mantık uygulaması

Ardından, işlevin Hizmet Veri Servisi kuyruğuna ittiği olayları alan, içeriği ayrıştıran ve eşleşen bir duruma dayalı bir e-posta gönderen bir Azure mantık uygulaması oluşturmanız gerekir.

 > Kaynak**Tümleştirme** > **Mantığı Uygulaması** **Oluştur'u**seçerek [bir mantık uygulaması oluşturun.](../../logic-apps/quickstart-create-first-logic-app-workflow.md)

Mantık uygulaması oluşturulduktan sonra, ona gidin ve **Edit'i**seçin. Mantık uygulaması düzenleyicisinde **Service Bus Queue'yi** seçin ve kuyruğa bağlamak için Servis Veri Aracı kimlik bilgilerinizi girin.

![Azure Mantık Uygulama Servis Veri Servisi](../media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

**Koşul ekle'yi**seçin. Bu durumda, gelişmiş düzenleyiciye geçin ve aşağıdaki kodu girin. *APP_ID* web uygulamanızın gerçek uygulama kimliğiyle değiştirin:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Gelen olaydan (Hizmet Veri Günü iletisinin gövdesi) *gelen uygulama* uygulamanın *uygulaması* değilse, bu ifade aslında **yanlış** döndürür.

Şimdi, EĞER NO altında bir eylem **oluşturmak, HIÇBIR ŞEY YAPMAYın**.

![Azure Logic Apps eylemi seçin](../media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Eylem için **Office 365'i seçin - e-posta gönderin.** Tanımlanan koşul **yanlış**döndürdüğünde göndermek için bir e-posta oluşturmak için alanları doldurun. Office 365'inyoksa, aynı sonuçları elde etmek için alternatifler arayın.

Artık dakikada bir kez yeni anahtar kasa denetim günlüklerini arayan uçlardan uca bir ardışık ardışık ardışık noktanız var. Bulduğu yeni günlükleri Servis Veri Servisi kuyruğuna iter. Yeni bir ileti sıraya düştüğünde mantık uygulaması tetiklenir. Etkinlik içindeki *uygulama* arama uygulamasının uygulama kimliğiyle eşleşmiyorsa, bir e-posta gönderir.
