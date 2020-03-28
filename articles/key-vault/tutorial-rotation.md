---
title: Tek Kullanıcı/Parola Döndürme Öğreticisi
description: Tek kullanıcı/parola döndürmeyi otomatikleştirmek için bu öğreticiyi kullanın
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
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79239374"
---
# <a name="automate-the-rotation-of-a-secret-for-resources-with-single-userpassword-authentication"></a>Tek kullanıcı/parola kimlik doğrulaması yla kaynaklar için bir sırrın döndürmesini otomatikleştirme

Azure hizmetlerine kimlik doğrulamanın en iyi yolu yönetilen bir kimlik kullanmak olsa [da,](managed-identity.md)bunun bir seçenek olmadığı bazı senaryolar vardır. Bu gibi durumlarda, erişim anahtarları veya sırları kullanılır. Erişim anahtarları veya sırlar periyodik olarak döndürülmelidir.

Bu öğretici, tek kullanıcı/parola kimlik doğrulaması ile veritabanları ve hizmetler için sırların periyodik olarak döndürülmenin nasıl otomatikleştirilebildiğini gösterir. Özellikle, bu senaryo, Olay Grid bildirimi tarafından tetiklenen bir işlevi kullanarak anahtar kasasında depolanan SQL sunucu parolalarını döndürür:

![Dönüş diyagramı](./media/rotate1.png)

1. Bir sırrın son kullanma tarihinden otuz gün önce, Key Vault "neredeyse son kullanma" olayını Event Grid'e yayınlar.
1. Olay Izgara olay abonelikleri denetler ve http post kullanarak, Bu etkinliğe abone Fonksiyon Uygulaması bitiş noktası çağırır.
1. İşlev Uygulaması gizli bilgileri alır, yeni bir rasgele parola oluşturur ve Key Vault'ta yeni bir parolayla gizli için yeni bir sürüm oluşturur.
1. İşlev App, SQL'i yeni parolayla güncelleştirir.

> [!NOTE]
> Adım 3 ve 4 arasında bir gecikme olabilir ve bu süre zarfında Key Vault'ta gizli olan sql'e kimlik doğrulamak için geçerli olmaz. Herhangi bir adımda arıza olması durumunda Olay Izgarası 2 saat boyunca yeniden çalışır.

## <a name="setup"></a>Kurulum

## <a name="create-a-key-vault-and-sql-server"></a>Önemli bir kasa ve SQL sunucusu oluşturma

Başlamadan önce bir Key Vault oluşturmalı, bir SQL Server ve veritabanı oluşturmalı ve SQL Server yönetici parolasını Key Vault'ta depolamalıyız.

Bu öğretici, bileşenleri oluşturmak için önceden oluşturulmuş bir Azure Kaynak Yöneticisi şablonu kullanır. Kodun tamamını burada bulabilirsiniz: [Temel Gizli Rotasyon Şablonu Örneği.](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/arm-templates)

1. Azure şablon dağıtım bağlantısını tıklatın:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Finitial-setup%2Fazuredeploy.json" target="_blank"> <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. "Kaynak Grubu" için "Yeni Oluştur" seçeneğini belirleyin ve "simplerotation" adını verin.
1. "Satın Al"ı seçin.

    ![Yeni kaynak grubu oluşturma](./media/rotate2.png)

Bu adımları tamamladıktan sonra, bir anahtar kasası, bir SQL sunucusu ve bir SQL veritabanı nız olacaktır. Bunu bir Azure CLI terminalinde çalıştırarak doğrulayabilirsiniz:

```azurecli
az resource list -o table
```

Sonuçlar bir şey bu bakacağız:

```console
Name                     ResourceGroup         Location    Type                               Status
-----------------------  --------------------  ----------  ---------------------------------  --------
simplerotation-kv             simplerotation             eastus      Microsoft.KeyVault/vaults
simplerotation-sql            simplerotation             eastus      Microsoft.Sql/servers
simplerotation-sql/master     simplerotation             eastus      Microsoft.Sql/servers/databases
```

## <a name="create-function-app"></a>Fonksiyon Uygulaması Oluştur

Sistem tarafından yönetilen bir kimliğin yanı sıra ek gerekli bileşenlerle bir İşlev Uygulaması oluşturun: 

Fonksiyon uygulaması aşağıda bileşenleri ve yapılandırma gerektirir:
- App Service Planı
- Depolama Hesabı
- İşlev Uygulaması Yönetilen Kimliğini kullanarak Key Vault'taki sırlara erişmek için erişim ilkesi

1. Azure şablon dağıtım bağlantısını tıklatın:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Ffunction-app%2Fazuredeploy.json" target="_blank"><img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. "Kaynak Grubu" için "simplerotation" seçeneğini belirleyin.
1. "Satın Al"ı seçin.

   ![Satın alma ekranı](./media/rotate3.png)

Yukarıdaki adımları tamamladıktan sonra, bir depolama hesabı, bir sunucu çiftliği ve bir İşlev Uygulaması olacaktır.  Bunu bir Azure CLI terminalinde çalıştırarak doğrulayabilirsiniz:

```azurecli
az resource list -o table
```

Sonuçlar bir şey bu bakacağız:

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

İşlev Uygulaması'nı nasıl oluşturup Key Vault'a erişmek için Yönetilen Kimlik'i kullanma hakkında bilgi için [Azure portalından bir işlev uygulaması oluşturma](../azure-functions/functions-create-function-app-portal.md) ve yönetilen bir [kimlikle Anahtar Kasa kimlik doğrulaması sağlayın](managed-identity.md)

### <a name="rotation-function-and-deployment"></a>Döndürme işlevi ve dağıtım
İşlev, olayı tetikleyici olarak kullanıyor ve Key Vault ve SQL veritabanını güncelleştiren gizli bir döndürme gerçekleştirin.

#### <a name="function-event-trigger-handler"></a>İşlev Olay Tetikleyici İşleyicisi

Aşağıda Fonksiyon olay verilerini okur ve döndürme mantığını yürütür

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

#### <a name="secret-rotation-logic"></a>Gizli Döndürme Mantığı
Bu döndürme yöntemi, veritabanı bilgilerini gizliden okur, sırrın yeni bir sürümünü oluşturur ve veritabanını yeni bir sırla güncelleştirir.

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
Kodun tamamını burada bulabilirsiniz:https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/rotation-function

#### <a name="function-deployment"></a>Fonksiyon dağıtımı

1. Download fonksiyonu uygulaması zip dosyası:https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/raw/master/simplerotationsample-fn.zip

1. Azure Cloud Shell'e dosya basit rotasyonörnek-fn.zip yükleyin.
 
1. Zip dosyasını işlev uygulamasına dağıtmak için aşağıda CLI komutunu kullanın:

```azurecli
az functionapp deployment source config-zip -g simplerotation -n simplerotation-fn --src /home/{firstname e.g jack}/simplerotationsample-fn.zip
```
![Satın alma ekranı](./media/rotate4.png)

Dağıtımdan sonra simplerotation-fn altında iki işlev fark etmelisiniz:

![Azure Cloud Shell](./media/rotate5.png)

### <a name="add-event-subscription-for-secretnearexpiry-event"></a>"SecretNearExpiry" etkinliği için etkinlik aboneliği ekleme

İşlev uygulamasını eventgrid_extension tuşa kopyalayın.

![Azure Cloud Shell](./media/rotate6.png)

![Test edin ve doğrulayın](./media/rotate7.png)

SecretNearExpiry olayları için bir olay ızgarası aboneliği oluşturmak için aşağıdaki komutta kopyalanan eventgrid uzantı anahtarını ve abonelik kimliğinizi kullanın.

```azurecli
az eventgrid event-subscription create --name simplerotation-eventsubscription --source-resource-id "/subscriptions/<subscription-id>/resourceGroups/simplerotation/providers/Microsoft.KeyVault/vaults/simplerotation-kv" --endpoint "https://simplerotation-fn.azurewebsites.net/runtime/webhooks/EventGrid?functionName=SimpleRotation&code=<extension-key>" --endpoint-type WebHook --included-event-types "Microsoft.KeyVault.SecretNearExpiry"
```

### <a name="add-secret-to-key-vault"></a>Key Vault'a sır ekleme
Kullanıcılara "sırları yönetme" izni verecek erişim ilkenizi ayarlayın.

```azurecli
az keyvault set-policy --upn <email-address-of-user> --name simplerotation-kv --secret-permissions set delete get list
```

Şimdi sql veritabanı veri kaynağı ve kullanıcı kimliği içeren etiketleri ile yeni bir sır oluşturun, yarın için son kullanma tarihi ayarlanır.

```azurecli
$tomorrowDate = (get-date).AddDays(+1).ToString("yyy-MM-ddThh:mm:ssZ")
az keyvault secret set --name sqluser --vault-name simplerotation-kv --value "Simple123" --tags "UserID=azureuser" "DataSource=simplerotation-sql.database.windows.net" --expires $tomorrowDate
```

Kısa bir son kullanma tarihi olan bir sır oluşturmak, sırrı döndürmek için işlevi tetikleyecek bir SecretNearExpiry olayını hemen yayımlar.

### <a name="test-and-verify"></a>Test edin ve doğrulayın
Birkaç dakika sonra, sqluser gizli otomatik olarak döndürmek gerekir.

Gizli rotasyon doğrulamasını doğrulamak için Key Vault > Secrets'a gidin

![Test edin ve doğrulayın](./media/rotate8.png)

"sqluser" sırrını açın ve özgün ve döndürülmüş sürümü görüntüleyin

![Test edin ve doğrulayın](./media/rotate9.png)

## <a name="create-web-app"></a>Web Uygulaması Oluşturma

SQL kimlik bilgilerini doğrulamak için bir web uygulaması oluşturun. Bu web uygulaması anahtar kasasından sırrı alacak, sql veritabanı bilgilerini ve kimlik bilgilerini secret'tan ayıklayacak ve sql bağlantısını test edecektir.

Web uygulaması aşağıdaki bileşenleri ve yapılandırmayı gerektirir:
- Sistem Tarafından Yönetilen Kimlikle Web Uygulaması
- Web App Yönetilen Kimliği kullanarak Key Vault'taki sırlara erişmek için erişim ilkesi

1. Azure şablon dağıtım bağlantısını tıklatın:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Fweb-app%2Fazuredeploy.json" target="_blank"> <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. Basit **döndürme** kaynak grubunu seçin
1. Satın Alma'yı tıklatın

### <a name="deploy-web-app"></a>Web Uygulaması Dağıtma

Web uygulamasının dağıtım Için web https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/test-webapp uygulamasında bulabileceğiniz kaynak kodu aşağıdakileri yapın:

1. Fonksiyon uygulaması zip dosyasını indirinhttps://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/raw/master/simplerotationsample-app.zip
1. Dosyayı `simplerotationsample-app.zip` Azure Bulut BulutU'na yükleyin.
1. Zip dosyasını işlev uygulamasına dağıtmak için bu Azure CLI komutunu kullanın:

   ```azurecli
   az webapp deployment source config-zip -g simplerotation -n simplerotation-app --src /home/{firstname e.g jack}/simplerotationsample-app.zip
   ```

#### <a name="open-web-application"></a>Açık web Uygulaması

Dağıtılan uygulamaya gidin ve "URL"yi tıklatın:
 
![Test edin ve doğrulayın](./media/rotate10.png)

Oluşturulan Gizli Değer, Veritabanı Bağlı ile doğru olarak gösterilmelidir.

## <a name="learn-more"></a>Daha fazla bilgi edinin:

- Genel Bakış: [Azure Olay Izgarasıyla Anahtar Kasası'nı İzleme (önizleme)](event-grid-overview.md)
- Nasıl yapılsın: [Önemli bir kasa gizli değiştiğinde e-posta alın](event-grid-logicapps.md)
- [Azure Anahtar Kasası için Azure Olay Izgara olay şeması (önizleme)](../event-grid/event-schema-key-vault.md)
