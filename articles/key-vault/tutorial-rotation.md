---
title: Tek kullanıcı/tek parola döndürme öğreticisi
description: Tek kullanıcı/tek parola kimlik doğrulaması kullanan kaynaklar için bir sırrın döndürmesini otomatikleştirmek için bu öğreticiyi kullanın.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: rotation
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 01/26/2020
ms.author: mbaldwin
ms.openlocfilehash: 684bfccd698217acdafdaac8c33f1e5531cdb4bf
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80998907"
---
# <a name="automate-the-rotation-of-a-secret-for-resources-that-use-single-usersingle-password-authentication"></a>Tek kullanıcı/tek parola kimlik doğrulaması kullanan kaynaklar için sırrın döndürmesini otomatikleştirme

Azure hizmetlerine kimlik doğrulamanın en iyi yolu yönetilen bir [kimlik](managed-identity.md)kullanmaktır, ancak bunun bir seçenek olmadığı bazı senaryolar vardır. Bu gibi durumlarda, erişim anahtarları veya sırları kullanılır. Erişim anahtarlarını veya sırları düzenli aralıklarla döndürmeniz gerekir.

Bu öğretici, tek kullanıcı/tek parola kimlik doğrulaması kullanan veritabanları ve hizmetler için sırların periyodik olarak döndürülme sini nasıl otomatikleştirini gösterir. Özellikle, bu öğretici Azure Olay Ağı bildirimi tarafından tetiklenen bir işlevi kullanarak Azure Key Vault'ta depolanan SQL Server parolalarını döndürür:

![Döndürme çözeltisinin diyagramı](./media/rotate1.png)

1. Bir sırrın son kullanma tarihinden otuz gün önce, Key Vault "neredeyse son kullanma" olayını Event Grid'e yayınlar.
1. Olay Izgaraolay abonelikleri denetler ve olay abone işlev uygulaması bitiş noktası aramak için HTTP POST kullanır.
1. İşlev uygulaması gizli bilgileri alır, yeni bir rasgele parola oluşturur ve Key Vault'taki yeni parolayla gizli için yeni bir sürüm oluşturur.
1. İşlev uygulaması SQL Server'ı yeni parolayla güncelleştirir.

> [!NOTE]
> 3 ve 4. Bu süre zarfında, Key Vault'taki gizli SQL Server'a kimlik doğrulaması yapamaz. Adımlardan herhangi birinin başarısız olması durumunda, Olay Izgarası iki saat boyunca yeniden çalışır.

## <a name="create-a-key-vault-and-sql-server-instance"></a>Anahtar kasası ve SQL Server örneği oluşturma

İlk adım, anahtar kasası ve SQL Server örneği ve veritabanı oluşturmak ve SQL Server yönetici parolasını Key Vault'ta depolamaktır.

Bu öğretici, bileşenleri oluşturmak için varolan bir Azure Kaynak Yöneticisi şablonu kullanır. Kodu burada bulabilirsiniz: [Temel Gizli Rotasyon Şablonu Örneği](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/arm-templates).

1. Azure şablon dağıtım bağlantısını seçin:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Finitial-setup%2Fazuredeploy.json" target="_blank"> <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. **Kaynak grubu**altında, **yeni oluştur'u**seçin. Grup **simplerotation**adı .
1. **Satın al**'ı seçin.

    ![Kaynak grubu oluşturma](./media/rotate2.png)

Artık bir anahtar kasanız, bir SQL Server örneğiniz ve bir SQL veritabanınız olacak. Bu kurulumu Azure CLI'de aşağıdaki komutu çalıştırarak doğrulayabilirsiniz:

```azurecli
az resource list -o table
```

Sonuç bir şey aşağıdaki çıktı bakacağız:

```console
Name                     ResourceGroup         Location    Type                               Status
-----------------------  --------------------  ----------  ---------------------------------  --------
simplerotation-kv          simplerotation      eastus      Microsoft.KeyVault/vaults
simplerotation-sql         simplerotation      eastus      Microsoft.Sql/servers
simplerotation-sql/master  simplerotation      eastus      Microsoft.Sql/servers/databases
```

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

Ardından, gerekli diğer bileşenlere ek olarak sistem tarafından yönetilen bir kimliğe sahip bir işlev uygulaması oluşturun.

İşlev uygulaması şu bileşenleri gerektirir:
- Azure Uygulama Hizmeti planı
- Depolama hesabı
- İşlev uygulaması yönetilen kimlik aracılığıyla Key Vault'taki sırlara erişmek için bir erişim ilkesi

1. Azure şablon dağıtım bağlantısını seçin:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Ffunction-app%2Fazuredeploy.json" target="_blank"><img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. Kaynak **grup** listesinde **simplerotation'u**seçin.
1. **Satın al**'ı seçin.

   ![Satın Alma'yı Seçin](./media/rotate3.png)

Önceki adımları tamamladıktan sonra, bir depolama hesabınız, sunucu çiftliğiniz ve bir işlev uygulamanız olur. Bu kurulumu Azure CLI'de aşağıdaki komutu çalıştırarak doğrulayabilirsiniz:

```azurecli
az resource list -o table
```

Sonuç aşağıdaki çıktı gibi bir şey görünecektir:

```console
Name                     ResourceGroup         Location    Type                               Status
-----------------------  --------------------  ----------  ---------------------------------  --------
simplerotation-kv          simplerotation       eastus      Microsoft.KeyVault/vaults
simplerotation-sql         simplerotation       eastus      Microsoft.Sql/servers
simplerotation-sql/master  simplerotation       eastus      Microsoft.Sql/servers/databases
simplerotationstrg         simplerotation       eastus      Microsoft.Storage/storageAccounts
simplerotation-plan        simplerotation       eastus      Microsoft.Web/serverFarms
simplerotation-fn          simplerotation       eastus      Microsoft.Web/sites
```

Key Vault'a erişmek için bir işlev uygulaması oluşturma ve yönetilen kimliği kullanma hakkında bilgi için Azure [portalından bir işlev uygulaması oluşturma](../azure-functions/functions-create-function-app-portal.md) ve yönetilen bir [kimlikle Anahtar Kasa kimlik doğrulaması sağlayın'a](managed-identity.md)bakın.

### <a name="rotation-function"></a>Döndürme fonksiyonu
İşlev, Key Vault ve SQL veritabanını güncelleştirerek bir sırrın döndürülmesini tetiklemek için bir olay kullanır.

#### <a name="function-trigger-event"></a>Fonksiyon tetikleyici olay

Bu işlev olay verilerini okur ve döndürme mantığını çalıştırAr:

```csharp
public static class SimpleRotationEventHandler
{
    [FunctionName("SimpleRotation")]
       public static void Run([EventGridTrigger]EventGridEvent eventGridEvent, ILogger log)
       {
            log.LogInformation("C# Event trigger function processed a request.");
            var secretName = eventGridEvent.Subject;
            var secretVersion = Regex.Match(eventGridEvent.Data.ToString(), "Version\":\"([a-z0-9]*)").Groups[1].ToString();
            var keyVaultName = Regex.Match(eventGridEvent.Topic, ".vaults.(.*)").Groups[1].ToString();
            log.LogInformation($"Key Vault Name: {keyVaultName}");
            log.LogInformation($"Secret Name: {secretName}");
            log.LogInformation($"Secret Version: {secretVersion}");

            SeretRotator.RotateSecret(log, secretName, secretVersion, keyVaultName);
        }
}
```

#### <a name="secret-rotation-logic"></a>Gizli döndürme mantığı
Bu döndürme yöntemi, veritabanı bilgilerini gizliden okur, sırrın yeni bir sürümünü oluşturur ve veritabanını yeni sırla güncelleştirir:

```csharp
public class SecretRotator
    {
       private const string UserIdTagName = "UserID";
       private const string DataSourceTagName = "DataSource";
       private const int SecretExpirationDays = 31;

    public static void RotateSecret(ILogger log, string secretName, string secretVersion, string keyVaultName)
    {
           //Retrieve current secret
           var kvUri = "https://" + keyVaultName + ".vault.azure.net";
           var client = new SecretClient(new Uri(kvUri), new DefaultAzureCredential());
           KeyVaultSecret secret = client.GetSecret(secretName, secretVersion);
           log.LogInformation("Secret Info Retrieved");
        
           //Retrieve secret info
           var userId = secret.Properties.Tags.ContainsKey(UserIdTagName) ?  
                        secret.Properties.Tags[UserIdTagName] : "";
           var datasource = secret.Properties.Tags.ContainsKey(DataSourceTagName) ? 
                            secret.Properties.Tags[DataSourceTagName] : "";
           log.LogInformation($"Data Source Name: {datasource}");
           log.LogInformation($"User Id Name: {userId}");
        
           //Create new password
           var randomPassword = CreateRandomPassword();
           log.LogInformation("New Password Generated");
        
           //Check DB connection using existing secret
           CheckServiceConnection(secret);
           log.LogInformation("Service Connection Validated");
                    
           //Create new secret with generated password
           CreateNewSecretVersion(client, secret, randomPassword);
           log.LogInformation("New Secret Version Generated");
        
           //Update DB password
           UpdateServicePassword(secret, randomPassword);
           log.LogInformation("Password Changed");
           log.LogInformation($"Secret Rotated Succesffuly");
    }
}
```
Tüm kodu [GitHub'da](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/rotation-function)bulabilirsiniz.

#### <a name="function-deployment"></a>Fonksiyon dağıtımı

1. GitHub'dan fonksiyon uygulaması zip dosyasını [indirin.](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/raw/master/simplerotationsample-fn.zip)

1. Basit rotasyon örneği-fn.zip dosyasını Azure Cloud Shell'e yükleyin.

   ![Dosyayı yükleyin](./media/rotate4.png)
1. Zip dosyasını işlev uygulamasına dağıtmak için bu Azure CLI komutunu kullanın:

   ```azurecli
   az functionapp deployment source config-zip -g simplerotation -n simplerotation-fn --src /home/{firstname e.g jack}/simplerotationsample-fn.zip
   ```

İşlev dağıtıldıktan sonra, simplerotation-fn altında iki işlev görmeniz gerekir:

![SimpleRotation ve SimpleRotationHttpTest fonksiyonları](./media/rotate5.png)

## <a name="add-an-event-subscription-for-the-secretnearexpiry-event"></a>SecretNearExpiry etkinliği için etkinlik aboneliği ekleme

İşlev uygulamasının `eventgrid_extension` anahtarını kopyalayın:

   ![İşlev uygulama ayarlarını seçin](./media/rotate6.png)

   ![eventgrid_extension anahtar](./media/rotate7.png)

Olaylar için `eventgrid_extension` olay Izgara aboneliği oluşturmak için `SecretNearExpiry` aşağıdaki komutta kopyalanan anahtarı ve abonelik kimliğinizi kullanın:

```azurecli
az eventgrid event-subscription create --name simplerotation-eventsubscription --source-resource-id "/subscriptions/<subscription-id>/resourceGroups/simplerotation/providers/Microsoft.KeyVault/vaults/simplerotation-kv" --endpoint "https://simplerotation-fn.azurewebsites.net/runtime/webhooks/EventGrid?functionName=SimpleRotation&code=<extension-key>" --endpoint-type WebHook --included-event-types "Microsoft.KeyVault.SecretNearExpiry"
```

## <a name="add-the-secret-to-key-vault"></a>Key Vault için gizli ekleyin
Erişim politikanızı, *kullanıcılara sırlar yönetme* izinleri verecek şekilde ayarlayın:

```azurecli
az keyvault set-policy --upn <email-address-of-user> --name simplerotation-kv --secret-permissions set delete get list
```

SQL veritabanı veri kaynağını ve kullanıcı kimliğini içeren etiketlerle yeni bir sır oluşturun. Yarın için ayarlanmış bir son kullanma tarihi ekleyin.

```azurecli
$tomorrowDate = (get-date).AddDays(+1).ToString("yyy-MM-ddThh:mm:ssZ")
az keyvault secret set --name sqluser --vault-name simplerotation-kv --value "Simple123" --tags "UserID=azureuser" "DataSource=simplerotation-sql.database.windows.net" --expires $tomorrowDate
```

Kısa bir son kullanma tarihi olan `SecretNearExpiry` bir sır oluşturmak hemen bir olay yayımlar, bu da sırrı döndürmek için işlevi tetikler.

## <a name="test-and-verify"></a>Test edin ve doğrulayın
Birkaç dakika sonra, `sqluser` gizli otomatik olarak döndürmelidir.

Sırrın döndüğünü doğrulamak için Key **Vault** > **Secrets'a**gidin:

![Sırlara Git](./media/rotate8.png)

**Sqluser** sırrını açın ve özgün ve döndürülmüş sürümleri görüntüleyin:

![sqluser sırrını açma](./media/rotate9.png)

### <a name="create-a-web-app"></a>Web uygulaması oluşturma

SQL kimlik bilgilerini doğrulamak için bir web uygulaması oluşturun. Bu web uygulaması Key Vault'tan sırrı alır, SQL veritabanı bilgilerini ve kimlik bilgilerini secret'tan ayıklar ve SQL Server bağlantısını test edecektir.

Web uygulaması şu bileşenleri gerektirir:
- Sistem tarafından yönetilen kimliğe sahip bir web uygulaması
- Web uygulaması yönetilen kimlik üzerinden Key Vault'taki sırlara erişmek için bir erişim ilkesi

1. Azure şablon dağıtım bağlantısını seçin:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Fweb-app%2Fazuredeploy.json" target="_blank"> <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. Basit **döndürme** kaynak grubunu seçin.
1. **Satın al**'ı seçin.

### <a name="deploy-the-web-app"></a>Web uygulamasını dağıtma

[GitHub'da](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/test-webapp)web uygulaması nın kaynak kodunu bulabilirsiniz.

Web uygulamasını dağıtmak için aşağıdaki adımları tamamlayın:

1. GitHub'dan fonksiyon uygulaması zip dosyasını [indirin.](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/raw/master/simplerotationsample-app.zip)
1. Basit rotasyon örneği-app.zip dosyasını Azure Cloud Shell'e yükleyin.
1. Zip dosyasını işlev uygulamasına dağıtmak için bu Azure CLI komutunu kullanın:

   ```azurecli
   az webapp deployment source config-zip -g simplerotation -n simplerotation-app --src /home/{firstname e.g jack}/simplerotationsample-app.zip
   ```

### <a name="open-the-web-app"></a>Web uygulamasını açma

Dağıtılan uygulamaya gidin ve URL'yi seçin:
 
![URL'yi seçin](./media/rotate10.png)

Oluşturulan Gizli Değer'i true değerine bağlı bir Veritabanı ile görmeniz gerekir.

## <a name="learn-more"></a>Daha fazla bilgi edinin

- Genel Bakış: [Azure Olay Izgarasıyla Anahtar Kasası'nı İzleme (önizleme)](event-grid-overview.md)
- Nasıl yapılsın: [Önemli bir kasa gizli değiştiğinde e-posta alın](event-grid-logicapps.md)
- [Azure Anahtar Kasası için Azure Olay Izgara olay şeması (önizleme)](../event-grid/event-schema-key-vault.md)
