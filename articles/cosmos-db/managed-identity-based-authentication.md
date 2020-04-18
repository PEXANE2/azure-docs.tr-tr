---
title: Azure Cosmos DB verilerine erişmek için sistem tarafından atanmış yönetilen kimlik nasıl kullanılır?
description: Azure Cosmos DB'den anahtarlara erişmek için Azure Active Directory (Azure AD) sistem tarafından atanmış yönetilen kimliği (yönetilen hizmet kimliği) nasıl yapılandırabilirsiniz öğrenin.
author: j-patrick
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/20/2020
ms.author: justipat
ms.reviewer: sngun
ms.openlocfilehash: 8136ad7a1fe29bc3394e959c10aafc52988c0a23
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641199"
---
# <a name="use-system-assigned-managed-identities-to-access-azure-cosmos-db-data"></a>Azure Cosmos DB verilerine erişmek için sistem tarafından atanmış yönetilen kimlikleri kullanma

Bu makalede, [yönetilen kimlikleri](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)kullanarak Azure Cosmos DB tuşlarına erişmek için *sağlam, anahtar döndürme agnostik* bir çözüm kuracaksınız. Bu makaledeki örnekte Azure İşlevleri kullanılır, ancak yönetilen kimlikleri destekleyen tüm hizmetleri kullanabilirsiniz. 

Azure Cosmos DB anahtarlarını kopyalamaya gerek kalmadan Azure Cosmos DB verilerine erişebilen bir işlev uygulaması oluşturmayı öğreneceksiniz. Fonksiyon uygulaması her dakika uyanır ve bir akvaryum akvaryum mevcut sıcaklığını kaydeder. Zamanlayıcı tarafından tetiklenen bir işlev uygulamasını nasıl ayarladığını öğrenmek için [Azure'da zamanlayıcı](../azure-functions/functions-create-scheduled-function.md) makalesi tarafından tetiklenen bir işlev oluştur'a bakın.

Senaryoyu basitleştirmek için, [Bir Time To Live](./time-to-live.md) ayarı zaten eski sıcaklık belgelerini temizlemek için yapılandırılmıştır. 

## <a name="assign-a-system-assigned-managed-identity-to-a-function-app"></a>Bir işlev uygulamasına sistem tarafından atanan yönetilen kimlik atama

Bu adımda, işlev uygulamanıza sistem tarafından atanmış yönetilen bir kimlik atarsınız.

1. Azure [portalında](https://portal.azure.com/)Azure **İşlevi** bölmesi'ni açın ve işlev uygulamanıza gidin. 

1. Platform **özelliklerini** > **kimlik** sekmesini açın: 

   ![İşlev uygulaması için Platform özelliklerini ve Kimlik seçeneklerini gösteren ekran görüntüsü.](./media/managed-identity-based-authentication/identity-tab-selection.png)

1. **Kimlik** sekmesinde, sistem kimlik **durumunu** **açın** ve **Kaydet'i**seçin. **Kimlik** bölmesi aşağıdaki gibi görünmelidir:  

   ![Sistem kimlik durumunu n için ayarlanmış olarak gösteren ekran görüntüsü.](./media/managed-identity-based-authentication/identity-tab-system-managed-on.png)

## <a name="grant-access-to-your-azure-cosmos-account"></a>Azure Cosmos hesabınıza erişim izni verme

Bu adımda, işlev uygulamasının sistem tarafından atanan yönetilen kimliğine bir rol atarsınız. Azure Cosmos DB'de yönetilen kimliğe atayabileceğiniz birden çok yerleşik rol vardır. Bu çözüm için aşağıdaki iki rolü kullanırsınız:

|Yerleşik rol  |Açıklama  |
|---------|---------|
|[DocumentDB Hesap Katılımcısı](../role-based-access-control/built-in-roles.md#documentdb-account-contributor)|Azure Cosmos DB hesaplarını yönetebilirsiniz. Okuma/yazma tuşlarının alınmasını sağlar. |
|[Cosmos DB Hesap Okuyucu](../role-based-access-control/built-in-roles.md#cosmos-db-account-reader-role)|Azure Cosmos DB hesap verilerini okuyabilir. Okuma anahtarlarının alınmasını sağlar. |

> [!IMPORTANT]
> Azure Cosmos DB'de rol tabanlı erişim denetimi desteği yalnızca düzlem işlemlerini denetlemek için geçerlidir. Veri düzlemi işlemleri ana anahtarlar veya kaynak belirteçleri ile güvenlidir. Daha fazla bilgi edinmek [için veri makalesine Güvenli erişim](secure-access-to-data.md) e bakın.

> [!TIP] 
> Roller atadığınızda, yalnızca gerekli erişimi atayın. Hizmetiniz yalnızca okuma verileri gerektiriyorsa, **Cosmos DB Hesap Okuyucu** rolünü yönetilen kimliğe atayın. En az ayrıcalık erişiminin önemi hakkında daha fazla bilgi [için, ayrıcalıklı hesaplar makalesinin daha düşük pozlama bölümüne](../security/fundamentals/identity-management-best-practices.md#lower-exposure-of-privileged-accounts) bakın.

Bu senaryoda, işlev uygulaması akvaryumun sıcaklığını okur ve bu verileri Azure Cosmos DB'deki bir kapsayıcıya yazar. İşlev uygulamasının verileri yazması gerektiğinden, **DocumentDB Hesap Katılımcısı** rolünü atamanız gerekir. 

1. Azure portalında oturum açın ve Azure Cosmos DB hesabınıza gidin. Access **denetimi (IAM)** bölmesini ve ardından **Rol atamaları** sekmesini açın:

   ![Access denetim bölmesini ve Rol atamaları sekmesini gösteren ekran görüntüsü.](./media/managed-identity-based-authentication/cosmos-db-iam-tab.png)

1. Select **+ Ekle** > **rol ataması**.

1. **Rol atama ekle** paneli sağa açılır:

   ![Rol atama ekle bölmesini gösteren ekran görüntüsü.](./media/managed-identity-based-authentication/cosmos-db-iam-tab-add-role-pane.png)

   * **Rolü**: **DocumentDB Hesap Katılımcısı'nı** seçin
   * **Access'i atayın**: **Sistemi atanmış yönetilen kimlik** alt bölümünde **İşlev Uygulaması'nı**seçin.
   * **Select**: Bölme, aboneliğinizdeki **Yönetilen Sistem Kimliğine**sahip tüm işlev uygulamalarıyla doldurulacaktır. Bu durumda, **FishTankTemperatureService** fonksiyonu uygulamasını seçin: 

      ![Örneklerle dolu rol atama sıfatını ekle'yi gösteren ekran görüntüsü.](./media/managed-identity-based-authentication/cosmos-db-iam-tab-add-role-pane-filled.png)

1. İşlev uygulamanızı seçtikten sonra **Kaydet'i**seçin.

## <a name="programmatically-access-the-azure-cosmos-db-keys"></a>Azure Cosmos DB tuşlarına programlı olarak erişin

Artık Azure Cosmos DB izinlerinde **DocumentDB Hesap Katılımcısı** rolüne sahip sistem tarafından atanmış yönetilen bir kimliğe sahip bir işlev uygulamamız var. Aşağıdaki işlev uygulama kodu Azure Cosmos DB anahtarlarını alır, bir CosmosClient nesnesi oluşturur, akvaryumun sıcaklığını alır ve bunu Azure Cosmos DB'ye kaydeder.

Bu örnek, Azure Cosmos DB hesap anahtarlarınıza erişmek için [Liste Tuşları API'sini](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/DatabaseAccounts/ListKeys) kullanır.

> [!IMPORTANT] 
> [Cosmos DB Hesap Okuyucu](#grant-access-to-your-azure-cosmos-account) rolünü atamak istiyorsanız, Yalnızca Liste [Oku Tuşlarını API'yi](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/DatabaseAccounts/ListReadOnlyKeys)kullanmanız gerekir. Bu sadece okunan anahtarları dolduracaktır.

Liste Anahtarları API `DatabaseAccountListKeysResult` nesnesini döndürür. Bu tür C# kitaplıklarında tanımlanmamıştır. Aşağıdaki kod bu sınıfın uygulanmasını gösterir:  

```csharp 
namespace Monitor 
{
  public class DatabaseAccountListKeysResult
  {
      public string primaryMasterKey {get;set;}
      public string primaryReadonlyMasterKey {get; set;}
      public string secondaryMasterKey {get; set;}
      public string secondaryReadonlyMasterKey {get;set;}
  }
}
```

Örnek, aşağıdaki gibi tanımlanan "TemperatureRecord" adlı basit bir belge de kullanır:

```csharp
using System;

namespace Monitor
{
    public class TemperatureRecord
    {
        public string id { get; set; } = Guid.NewGuid().ToString();
        public DateTime RecordTime { get; set; }
        public int Temperature { get; set; }

    }
}
```

Sistem tarafından atanan yönetilen kimlik belirteci almak için [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) kitaplığını kullanırsınız. Belirteci almanın diğer yollarını öğrenmek ve `Microsoft.Azure.Service.AppAuthentication` kitaplık hakkında daha fazla bilgi edinmek için [Hizmetten Hizmete kimlik doğrulama makalesine](../key-vault/general/service-to-service-authentication.md) bakın.


```csharp
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Threading.Tasks;
using Microsoft.Azure.Cosmos;
using Microsoft.Azure.Services.AppAuthentication;
using Microsoft.Azure.WebJobs;
using Microsoft.Extensions.Logging;

namespace Monitor
{
    public static class FishTankTemperatureService
    {
        private static string subscriptionId =
        "<azure subscription id>";
        private static string resourceGroupName =
        "<name of your azure resource group>";
        private static string accountName =
        "<Azure Cosmos DB account name>";
        private static string cosmosDbEndpoint =
        "<Azure Cosmos DB endpoint>";
        private static string databaseName =
        "<Azure Cosmos DB name>";
        private static string containerName =
        "<container to store the temperature in>";

        [FunctionName("FishTankTemperatureService")]
        public static async Task Run([TimerTrigger("0 * * * * *")]TimerInfo myTimer, ILogger log)
        {
            log.LogInformation($"Starting temperature monitoring: {DateTime.Now}");

            // AzureServiceTokenProvider will help us to get the Service Managed token.
            var azureServiceTokenProvider = new AzureServiceTokenProvider();

            // Authenticate to the Azure Resource Manager to get the Service Managed token.
            string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://management.azure.com/");

            // Setup the List Keys API to get the Azure Cosmos DB keys.
            string endpoint = $"https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DocumentDB/databaseAccounts/{accountName}/listKeys?api-version=2019-12-12";

            // Setup an HTTP Client and add the access token.
            HttpClient httpClient = new HttpClient();
            httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

            // Post to the endpoint to get the keys result.
            var result = await httpClient.PostAsync(endpoint, new StringContent(""));

            // Get the result back as a DatabaseAccountListKeysResult.
            DatabaseAccountListKeysResult keys = await result.Content.ReadAsAsync<DatabaseAccountListKeysResult>();

            log.LogInformation("Starting to create the client");

            CosmosClient client = new CosmosClient(cosmosDbEndpoint, keys.primaryMasterKey);

            log.LogInformation("Client created");

            var database = client.GetDatabase(databaseName);
            var container = database.GetContainer(containerName);

            log.LogInformation("Get the temperature.");

            var tempRecord = new TemperatureRecord() { RecordTime = DateTime.UtcNow, Temperature = GetTemperature() };

            log.LogInformation("Store temperature");

            await container.CreateItemAsync<TemperatureRecord>(tempRecord);

            log.LogInformation($"Ending temperature monitor: {DateTime.Now}");
        }

        private static int GetTemperature()
        {
            // Fake the temperature sensor for this demo.
            Random r = new Random(DateTime.UtcNow.Second);
            return r.Next(0, 120);
        }
    }
}
```

Artık [işlev uygulamanızı dağıtmaya](../azure-functions/functions-create-first-function-vs-code.md)hazırsınız.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB ve Azure Active Directory ile sertifika tabanlı kimlik doğrulama](certificate-based-authentication.md)
* [Azure Key Vault'u kullanarak Azure Cosmos DB tuşlarını güvenli hale](access-secrets-from-keyvault.md)
* [Azure Cosmos DB için güvenlik taban çizgisi](security-baseline.md)
