---
title: Tek Kullanıcı/parola döndürme öğreticisi
description: Tek Kullanıcı/parola döndürmesini otomatik hale getirmek için bu öğreticiyi kullanın
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: rotation
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 01/26/2020
ms.author: mbaldwin
ms.openlocfilehash: 890932f7c0e46a2c9c0b6e1cf1461e4d7d25b409
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79239374"
---
# <a name="automate-the-rotation-of-a-secret-for-resources-with-single-userpassword-authentication"></a>Tek Kullanıcı/parola kimlik doğrulaması olan kaynaklar için gizli dizi döndürmeyi otomatikleştirin

Azure hizmetlerinde kimlik doğrulaması yapmanın en iyi yolu [yönetilen bir kimlik](managed-identity.md)kullanmakla birlikte, bunun bir seçenek olmadığı bazı senaryolar vardır. Bu durumlarda, erişim anahtarları veya gizli dizileri kullanılır. Erişim anahtarları veya gizli dizileri düzenli olarak döndürülmelidir.

Bu öğreticide, tek kullanıcı/parola kimlik doğrulamasıyla veritabanları ve hizmetler için düzenli aralıklarla yapılan gizli dizi döndürmenin nasıl otomatikleştirdiği gösterilmektedir. Özellikle, bu senaryo, Event Grid bildirimi tarafından tetiklenen bir işlev kullanılarak Anahtar Kasası 'nda depolanan SQL Server parolalarını döndürür:

![Döndürme diyagramı](./media/rotate1.png)

1. Bir parolanın sona erme tarihinden otuz gün önce, Event Grid için "sona erme" olayını yayımlayın Key Vault.
1. Event Grid olay aboneliklerini denetler ve http post kullanarak bu olaya abone olan İşlev Uygulaması uç noktasını çağırır.
1. İşlev uygulaması gizli bilgileri alır, yeni bir rastgele parola oluşturur ve Key Vault yeni bir parolayla gizli dizi için yeni bir sürüm oluşturur.
1. İşlev uygulaması, SQL 'i yeni parolayla güncelleştirir.

> [!NOTE]
> Adım 3 ile 4 arasında bir gecikme olabilir ve bu süre içinde gizli dizi Key Vault, SQL kimlik doğrulaması için geçerli değildir. Adımlarda bir hata olması durumunda 2 saat boyunca yeniden denemeler Event Grid.

## <a name="setup"></a>Kurulum

## <a name="create-a-key-vault-and-sql-server"></a>Anahtar Kasası ve SQL Server oluşturma

Başlamadan önce bir Key Vault oluşturmalı, bir SQL Server ve veritabanı oluşturmalı ve SQL Server yönetici parolasını Key Vault depolarız.

Bu öğretici, bileşen oluşturmak için önceden oluşturulmuş bir Azure Resource Manager şablonu kullanır. Kodun tamamını buradan bulabilirsiniz: [temel gizli döndürme şablonu örneği](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/arm-templates).

1. Azure şablonu dağıtım bağlantısı ' na tıklayın:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Finitial-setup%2Fazuredeploy.json" target="_blank"> <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. "Kaynak grubu" için "Yeni oluştur" seçeneğini belirleyin ve "simplerotation" adı verin.
1. "Satın al" seçeneğini belirleyin.

    ![Yeni kaynak grubu oluştur](./media/rotate2.png)

Bu adımları tamamladıktan sonra bir Anahtar Kasası, bir SQL Server ve bir SQL veritabanı olacaktır. Bunu çalıştırarak, bir Azure CLı terminalinde doğrulayabilirsiniz:

```azurecli
az resource list -o table
```

Sonuçlar şöyle görünür:

```console
Name                     ResourceGroup         Location    Type                               Status
-----------------------  --------------------  ----------  ---------------------------------  --------
simplerotation-kv             simplerotation             eastus      Microsoft.KeyVault/vaults
simplerotation-sql            simplerotation             eastus      Microsoft.Sql/servers
simplerotation-sql/master     simplerotation             eastus      Microsoft.Sql/servers/databases
```

## <a name="create-function-app"></a>İşlev Uygulaması Oluştur

Sistem tarafından yönetilen kimliğe ve gerekli ek bileşenlere sahip bir İşlev Uygulaması oluşturun: 

İşlev uygulaması için bileşenlerin ve yapılandırmanın altında aşağıdakiler gereklidir:
- App Service Planı
- Depolama Hesabı
- İşlev Uygulaması yönetilen kimlik kullanarak Key Vault gizli anahtarlara erişmek için erişim ilkesi

1. Azure şablonu dağıtım bağlantısı ' na tıklayın:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Ffunction-app%2Fazuredeploy.json" target="_blank"><img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. "Kaynak grubu" için "simplerotation" seçeneğini belirleyin.
1. "Satın al" seçeneğini belirleyin.

   ![Satın alma ekranı](./media/rotate3.png)

Yukarıdaki adımları tamamladıktan sonra bir depolama hesabınız, sunucu grubunuz ve bir İşlev Uygulaması olacaktır.  Bunu çalıştırarak, bir Azure CLı terminalinde doğrulayabilirsiniz:

```azurecli
az resource list -o table
```

Sonuçlar şöyle görünür:

```console
Name                     ResourceGroup         Location    Type                               Status
-----------------------  --------------------  ----------  ---------------------------------  --------
simplerotation-kv             simplerotation             eastus      Microsoft.KeyVault/vaults
simplerotation-sql            simplerotation             eastus      Microsoft.Sql/servers
simplerotation-sql/master     simplerotation             eastus      Microsoft.Sql/servers/databases
simplerotationstrg            simplerotation             eastus      Microsoft.Storage/storageAccounts
simplerotation-plan           simplerotation             eastus      Microsoft.Web/serverFarms
simplerotation-fn             simplerotation             eastus      Microsoft.Web/sites
```

İşlev Uygulaması oluşturma ve Key Vault erişmek için yönetilen kimlik kullanma hakkında bilgi için, bkz [. Azure Portal bir işlev uygulaması oluşturma](../azure-functions/functions-create-function-app-portal.md) ve [yönetilen kimlik ile Key Vault kimlik doğrulaması sağlama](managed-identity.md)

### <a name="rotation-function-and-deployment"></a>Döndürme işlevi ve dağıtımı
İşlev tetikleyici olarak olay kullanıyor ve gizli bir güncelleştirme Key Vault ve SQL veritabanı dönüşü gerçekleştirir.

#### <a name="function-event-trigger-handler"></a>İşlev olayı tetikleyici Işleyicisi

Aşağıdaki Işlev olay verilerini okur ve döndürme mantığını yürütür

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

#### <a name="secret-rotation-logic"></a>Gizli anahtar döndürme mantığı
Bu döndürme yöntemi, gizli dizi veritabanı bilgilerini okur, gizliliğin yeni bir sürümünü oluşturur ve veritabanını yeni bir gizli anahtar ile güncelleştirir.

```csharp
public class SecretRotator
    {
       private const string UserIdTagName = "UserID";
       private const string DataSourceTagName = "DataSource";
       private const int SecretExpirationDays = 31;

    public static void RotateSecret(ILogger log, string secretName, string secretVersion, string keyVaultName)
    {
           //Retrieve Current Secret
           var kvUri = "https://" + keyVaultName + ".vault.azure.net";
           var client = new SecretClient(new Uri(kvUri), new DefaultAzureCredential());
           KeyVaultSecret secret = client.GetSecret(secretName, secretVersion);
           log.LogInformation("Secret Info Retrieved");
        
           //Retrieve Secret Info
           var userId = secret.Properties.Tags.ContainsKey(UserIdTagName) ?  
                        secret.Properties.Tags[UserIdTagName] : "";
           var datasource = secret.Properties.Tags.ContainsKey(DataSourceTagName) ? 
                            secret.Properties.Tags[DataSourceTagName] : "";
           log.LogInformation($"Data Source Name: {datasource}");
           log.LogInformation($"User Id Name: {userId}");
        
           //create new password
           var randomPassword = CreateRandomPassword();
           log.LogInformation("New Password Generated");
        
           //Check db connection using existing secret
           CheckServiceConnection(secret);
           log.LogInformation("Service Connection Validated");
                    
           //Create new secret with generated password
           CreateNewSecretVersion(client, secret, randomPassword);
           log.LogInformation("New Secret Version Generated");
        
           //Update db password
           UpdateServicePassword(secret, randomPassword);
           log.LogInformation("Password Changed");
           log.LogInformation($"Secret Rotated Succesffuly");
    }
}
```
Kodun tamamını buradan bulabilirsiniz: https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/rotation-function

#### <a name="function-deployment"></a>İşlev dağıtımı

1. İşlev uygulaması ZIP dosyasını indir: https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/raw/master/simplerotationsample-fn.zip

1. Simplerotationsample-FN. zip dosyasını Azure Cloud Shell karşıya yükleyin.
 
1. ZIP dosyasını işlev uygulamasına dağıtmak için aşağıdaki CLı komutunu kullanın:

```azurecli
az functionapp deployment source config-zip -g simplerotation -n simplerotation-fn --src /home/{firstname e.g jack}/simplerotationsample-fn.zip
```
![Satın alma ekranı](./media/rotate4.png)

Dağıtımdan sonra, simplerotation-FN altında iki işlev fark etmelisiniz:

![Azure Cloud Shell](./media/rotate5.png)

### <a name="add-event-subscription-for-secretnearexpiry-event"></a>"Secretyaklaştığında süre sonu" olayı için olay aboneliği Ekle

İşlev uygulaması eventgrid_extension anahtarı ' nı kopyalayın.

![Azure Cloud Shell](./media/rotate6.png)

![Test ve doğrulama](./media/rotate7.png)

Secretbir süre sonu olayları için bir Event Grid aboneliği oluşturmak için aşağıdaki komutta kopyalanmış eventgrid uzantı anahtarını ve abonelik KIMLIĞINIZI kullanın.

```azurecli
az eventgrid event-subscription create --name simplerotation-eventsubscription --source-resource-id "/subscriptions/<subscription-id>/resourceGroups/simplerotation/providers/Microsoft.KeyVault/vaults/simplerotation-kv" --endpoint "https://simplerotation-fn.azurewebsites.net/runtime/webhooks/EventGrid?functionName=SimpleRotation&code=<extension-key>" --endpoint-type WebHook --included-event-types "Microsoft.KeyVault.SecretNearExpiry"
```

### <a name="add-secret-to-key-vault"></a>Key Vault için gizli dizi ekleme
Kullanıcılara "gizli dizileri Yönet" izni vermek için erişim ilkenizi ayarlayın.

```azurecli
az keyvault set-policy --upn <email-address-of-user> --name simplerotation-kv --secret-permissions set delete get list
```

Şimdi SQL veritabanı veri kaynağı ve Kullanıcı KIMLIĞI içeren etiketlerle yeni bir gizli anahtar oluşturarak yarın için sona erme tarihi ayarlanmıştır.

```azurecli
$tomorrowDate = (get-date).AddDays(+1).ToString("yyy-MM-ddThh:mm:ssZ")
az keyvault secret set --name sqluser --vault-name simplerotation-kv --value "Simple123" --tags "UserID=azureuser" "DataSource=simplerotation-sql.database.windows.net" --expires $tomorrowDate
```

Kısa süre sonu tarihi ile bir gizli dizi oluşturmak, hemen bir Secretbir süre sonu olayı yayımlayacak ve bu da, bir parolayı, gizli anahtarı döndürmek için tetikleyecektir.

### <a name="test-and-verify"></a>Test ve doğrulama
Birkaç dakika sonra, SQLUSER gizli anahtarı otomatik olarak döner.

Gizli anahtar döndürme doğrulamasını doğrulamak için Key Vault > Gizli dizileri ' ne gidin

![Test ve doğrulama](./media/rotate8.png)

"SQLUSER" gizliliğini açın ve özgün ve döndürülmüş sürümü görüntüleyin

![Test ve doğrulama](./media/rotate9.png)

## <a name="create-web-app"></a>Web Uygulaması Oluşturma

SQL kimlik bilgilerini doğrulamak için bir Web uygulaması oluşturun. Bu Web uygulaması, anahtar kasasından gizli dizi alır, SQL veritabanı bilgilerini ve kimlik bilgilerini gizli listeden ayıklar ve SQL bağlantısını test eder.

Web uygulaması, bileşenleri ve yapılandırmayı gerektirir:
- Sistem tarafından yönetilen kimliğe sahip Web uygulaması
- Web uygulaması tarafından yönetilen kimliği kullanarak Key Vault gizli anahtarlara erişim ilkesi

1. Azure şablonu dağıtım bağlantısı ' na tıklayın:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Fweb-app%2Fazuredeploy.json" target="_blank"> <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. **Simplerotation** kaynak grubunu seçin
1. Satın al 'a tıklayın

### <a name="deploy-web-app"></a>Web uygulaması dağıtma

Web uygulamasının dağıtımı Için https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/test-webapp bulabileceğiniz Web uygulaması için kaynak kodu, şunları yapın:

1. https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/raw/master/simplerotationsample-app.zip 'den uygulama ZIP dosyasını indirin
1. Dosya `simplerotationsample-app.zip` Azure Cloud Shell karşıya yükleyin.
1. ZIP dosyasını işlev uygulamasına dağıtmak için bu Azure CLı komutunu kullanın:

   ```azurecli
   az webapp deployment source config-zip -g simplerotation -n simplerotation-app --src /home/{firstname e.g jack}/simplerotationsample-app.zip
   ```

#### <a name="open-web-application"></a>Web uygulamasını aç

Dağıtılan uygulamaya gidin ve "URL" ye tıklayın:
 
![Test ve doğrulama](./media/rotate10.png)

Oluşturulan gizli değer, veritabanı doğru olarak bağlı olarak gösterilmelidir.

## <a name="learn-more"></a>Daha fazla bilgi edinin:

- Genel Bakış: [Azure Event Grid Key Vault izleme (Önizleme)](event-grid-overview.md)
- Nasıl yapılır: [Anahtar Kasası gizli anahtarı değiştiğinde e-posta alma](event-grid-logicapps.md)
- [Azure Key Vault için Azure Event Grid olay şeması (Önizleme)](../event-grid/event-schema-key-vault.md)
