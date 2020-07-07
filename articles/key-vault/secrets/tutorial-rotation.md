---
title: Tek kimlik doğrulama kimlik bilgileri kümesine sahip kaynaklar için döndürme öğreticisi
description: Bir kimlik doğrulama kimlik bilgileri kümesi kullanan kaynaklar için gizli dizi döndürmeyi nasıl otomatikleştirebileceğinizi öğrenmek için bu öğreticiyi kullanın.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: rotation
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 01/26/2020
ms.author: mbaldwin
ms.openlocfilehash: 9bff8c040f4cfed612278dd83ebb354b31a3a1f3
ms.sourcegitcommit: a989fb89cc5172ddd825556e45359bac15893ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85801453"
---
# <a name="automate-the-rotation-of-a-secret-for-resources-that-use-one-set-of-authentication-credentials"></a>Bir kimlik doğrulama kimlik bilgileri kümesi kullanan kaynaklar için gizli dizi döndürmeyi otomatikleştirin

Azure hizmetlerinde kimlik doğrulaması yapmanın en iyi yolu [yönetilen bir kimlik](../general/managed-identity.md)kullanmaktır, ancak bir seçenek olmadığı durumlarda bazı senaryolar vardır. Bu durumlarda, erişim anahtarları veya gizli dizileri kullanılır. Erişim anahtarlarını veya gizli dizileri düzenli olarak döndürmelisiniz.

Bu öğreticide, tek bir kimlik doğrulama kimlik bilgileri kümesi kullanan veritabanları ve hizmetler için düzenli aralıklarla yapılan gizli dizi döndürmenin nasıl otomatikleştirdiği gösterilmektedir. Özellikle, bu öğretici Azure Event Grid bildirimi tarafından tetiklenen bir işlev kullanılarak Azure Key Vault depolanan SQL Server parolalarını döndürür:

![Döndürme çözümünün diyagramı](../media/rotate1.png)

1. Bir parolanın süre sonu tarihinden otuz gün önce, Key Vault Event Grid "sona erme" olayını yayınlar.
1. Event Grid olay aboneliklerini denetler ve olaya abone olan işlev uygulama uç noktasını çağırmak için HTTP POST kullanır.
1. İşlev uygulaması gizli bilgileri alır, yeni bir rastgele parola oluşturur ve Key Vault yeni parolayla gizli dizi için yeni bir sürüm oluşturur.
1. İşlev uygulaması güncelleştirmeleri yeni parolayla SQL Server.

> [!NOTE]
> 3 ve 4. adımlar arasında bir gecikme olabilir. Bu süre boyunca Key Vault gizli dizi SQL Server kimlik doğrulaması yapamaz. Adımların hiçbirinde hata olması durumunda iki saat boyunca yeniden denemeler Event Grid.

## <a name="create-a-key-vault-and-sql-server-instance"></a>Anahtar Kasası ve SQL Server örneği oluşturma

İlk adım, bir anahtar kasası ve bir SQL Server örneği ve veritabanı oluşturmak ve SQL Server yönetici parolasını Key Vault depooluşturmaktır.

Bu öğretici, bileşenleri oluşturmak için mevcut bir Azure Resource Manager şablonunu kullanır. Kodu buradan bulabilirsiniz: [temel gizli döndürme şablonu örneği](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/arm-templates).

1. Azure şablonu dağıtım bağlantısını seçin:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Finitial-setup%2Fazuredeploy.json" target="_blank"> <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. **Kaynak grubu**altında **Yeni oluştur**' u seçin. Grup **simpotation**'ı adlandırın.
1. **Satın al**'ı seçin.

    ![Kaynak grubu oluşturma](../media/rotate2.png)

Artık bir Anahtar Kasası, bir SQL Server örneği ve bir SQL veritabanı olacaktır. Aşağıdaki komutu çalıştırarak bu kurulumu Azure CLı 'da doğrulayabilirsiniz:

```azurecli
az resource list -o table
```

Sonuç aşağıdaki çıktıyı görür:

```console
Name                     ResourceGroup         Location    Type                               Status
-----------------------  --------------------  ----------  ---------------------------------  --------
simplerotation-kv          simplerotation      eastus      Microsoft.KeyVault/vaults
simplerotation-sql         simplerotation      eastus      Microsoft.Sql/servers
simplerotation-sql/master  simplerotation      eastus      Microsoft.Sql/servers/databases
```

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

Daha sonra, sistem tarafından yönetilen kimliğe sahip bir işlev uygulaması oluşturun ve diğer gerekli bileşenlere ek olarak.

İşlev uygulaması bu bileşenleri gerektirir:
- Azure App Service planı
- Depolama hesabı
- İşlev uygulaması yönetilen kimliği aracılığıyla Key Vault parolalara erişmek için bir erişim ilkesi

1. Azure şablonu dağıtım bağlantısını seçin:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Ffunction-app%2Fazuredeploy.json" target="_blank"><img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. **Kaynak grubu** listesinde, **simplerotation**' ı seçin.
1. **Satın al**'ı seçin.

   ![Satın al 'ı seçin](../media/rotate3.png)

Yukarıdaki adımları tamamladıktan sonra bir depolama hesabınız, sunucu grubunuz ve bir işlev uygulamanız olacaktır. Aşağıdaki komutu çalıştırarak bu kurulumu Azure CLı 'da doğrulayabilirsiniz:

```azurecli
az resource list -o table
```

Sonuç aşağıdaki çıktıya benzer bir şekilde görünecektir:

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

Bir işlev uygulaması oluşturma ve Key Vault erişmek için yönetilen kimlik kullanma hakkında bilgi için, bkz. [Azure Portal bir işlev uygulaması oluşturma](../../azure-functions/functions-create-function-app-portal.md) ve [yönetilen bir kimlikle Key Vault kimlik doğrulaması sağlama](../general/managed-identity.md).

### <a name="rotation-function"></a>Döndürme işlevi
İşlevi, Key Vault ve SQL veritabanını güncelleştirerek bir gizli dizi dönüşü tetiklemek için bir olay kullanır.

#### <a name="function-trigger-event"></a>İşlev tetikleyici olayı

Bu işlev olay verilerini okur ve döndürme mantığını çalıştırır:

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
Bu döndürme yöntemi, gizli dizi veritabanı bilgilerini okur, gizliliğin yeni bir sürümünü oluşturur ve veritabanını yeni gizli anahtar ile güncelleştirir:

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
Tüm kodu [GitHub](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/rotation-function)' da bulabilirsiniz.

#### <a name="function-deployment"></a>İşlev dağıtımı

1. [GitHub](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/raw/master/simplerotationsample-fn.zip)'dan işlev uygulaması ZIP dosyasını indirin.

1. simplerotationsample-fn.zip dosyasını Azure Cloud Shell karşıya yükleyin.

   ![Dosyayı karşıya yükle](../media/rotate4.png)
1. ZIP dosyasını işlev uygulamasına dağıtmak için bu Azure CLı komutunu kullanın:

   ```azurecli
   az functionapp deployment source config-zip -g simplerotation -n simplerotation-fn --src /home/{firstname e.g jack}/simplerotationsample-fn.zip
   ```

İşlev dağıtıldıktan sonra, simplerotation-FN altında iki işlev görmeniz gerekir:

![SimpleRotation ve SimpleRotationHttpTest işlevleri](../media/rotate5.png)

## <a name="add-an-event-subscription-for-the-secretnearexpiry-event"></a>Secretyaklaştığında süre sonu olayı için bir olay aboneliği ekleyin

İşlev uygulamasının `eventgrid_extension` anahtarını kopyalayın:

   ![İşlev uygulaması ayarlarını seçin](../media/rotate6.png)

   ![eventgrid_extension anahtarı](../media/rotate7.png)

`eventgrid_extension`Olaylar için bir Event Grid aboneliği oluşturmak için aşağıdaki komutta kopyalanmış anahtarı ve ABONELIK kimliğinizi kullanın `SecretNearExpiry` :

```azurecli
az eventgrid event-subscription create --name simplerotation-eventsubscription --source-resource-id "/subscriptions/<subscription-id>/resourceGroups/simplerotation/providers/Microsoft.KeyVault/vaults/simplerotation-kv" --endpoint "https://simplerotation-fn.azurewebsites.net/runtime/webhooks/EventGrid?functionName=SimpleRotation&code=<extension-key>" --endpoint-type WebHook --included-event-types "Microsoft.KeyVault.SecretNearExpiry"
```

## <a name="add-the-secret-to-key-vault"></a>Gizli dizi Key Vault ekleyin
Kullanıcılara *gizli dizi yönetme* izinleri vermek için erişim ilkenizi ayarlayın:

```azurecli
az keyvault set-policy --upn <email-address-of-user> --name simplerotation-kv --secret-permissions set delete get list
```

SQL veritabanı veri kaynağını ve kullanıcı KIMLIĞINI içeren etiketlerle yeni bir gizli dizi oluşturun. Yarın için ayarlanmış bir sona erme tarihi ekleyin.

```azurecli
$tomorrowDate = (get-date).AddDays(+1).ToString("yyy-MM-ddThh:mm:ssZ")
az keyvault secret set --name sqluser --vault-name simplerotation-kv --value "Simple123" --tags "UserID=azureuser" "DataSource=simplerotation-sql.database.windows.net" --expires $tomorrowDate
```

Kısa süre sonu tarihiyle bir gizli dizi oluşturmak hemen bir olay yayımlayacak `SecretNearExpiry` , bu da bir olayı, gizli anahtarı döndürmek için tetikleyecektir.

## <a name="test-and-verify"></a>Test ve doğrulama
Birkaç dakika sonra `sqluser` gizli anahtar otomatik olarak döner.

Gizli dizinin döndürülmeyeceğini doğrulamak için **Key Vault**  >  **gizli**dizi sayfasına gidin:

![Gizli anahtarlara git](../media/rotate8.png)

**SQLUSER** parolasını açın ve özgün ve döndürülmüş sürümleri görüntüleyin:

![SQLUSER parolasını açın](../media/rotate9.png)

### <a name="create-a-web-app"></a>Web uygulaması oluşturma

SQL kimlik bilgilerini doğrulamak için bir Web uygulaması oluşturun. Bu Web uygulaması Key Vault parolayı alacak, SQL veritabanı bilgilerini ve kimlik bilgilerini gizli listeden ayıklayarak SQL Server bağlantısını test edecektir.

Web uygulaması şu bileşenleri gerektirir:
- Sistem tarafından yönetilen kimliğe sahip bir Web uygulaması
- Web uygulaması yönetilen kimliği aracılığıyla Key Vault parolalara erişim için erişim ilkesi

1. Azure şablonu dağıtım bağlantısını seçin:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Fweb-app%2Fazuredeploy.json" target="_blank"> <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. **Simplerotation** kaynak grubunu seçin.
1. **Satın al**'ı seçin.

### <a name="deploy-the-web-app"></a>Web uygulamasını dağıtma

[GitHub](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/test-webapp)'da Web uygulaması için kaynak kodu bulabilirsiniz.

Web uygulamasını dağıtmak için şu adımları izleyin:

1. [GitHub](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/raw/master/simplerotationsample-app.zip)'dan işlev uygulaması ZIP dosyasını indirin.
1. simplerotationsample-app.zip dosyasını Azure Cloud Shell karşıya yükleyin.
1. ZIP dosyasını işlev uygulamasına dağıtmak için bu Azure CLı komutunu kullanın:

   ```azurecli
   az webapp deployment source config-zip -g simplerotation -n simplerotation-app --src /home/{firstname e.g jack}/simplerotationsample-app.zip
   ```

### <a name="open-the-web-app"></a>Web uygulamasını açın

Dağıtılan uygulamaya gidin ve URL 'YI seçin:
 
![URL 'YI seçin](../media/rotate10.png)

Uygulama tarayıcıda açıldığında, **oluşturulan gizli değeri** ve bir **veritabanı bağlı** değeri *true*olarak görürsünüz.

## <a name="learn-more"></a>Daha fazla bilgi edinin

- Genel Bakış: [Azure Event Grid Key Vault izleme (Önizleme)](../general/event-grid-overview.md)
- Nasıl yapılır: [Anahtar Kasası gizli anahtarı değiştiğinde e-posta alma](../general/event-grid-logicapps.md)
- [Azure Key Vault için Azure Event Grid olay şeması (Önizleme)](../../event-grid/event-schema-key-vault.md)
