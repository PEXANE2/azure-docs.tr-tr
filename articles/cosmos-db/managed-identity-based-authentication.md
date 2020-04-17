---
title: Azure Cosmos DB verilerine erişmek için sistem tarafından atanmış yönetilen kimlik nasıl kullanılır?
description: Azure Cosmos DB tuşlarına erişmek için Azure AD sistem tarafından atanmış yönetilen bir kimliği nasıl yapılandırabilirsiniz öğrenin. msi, yönetilen hizmet kimliği, aad, azure etkin dizini, kimlik
author: j-patrick
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/20/2020
ms.author: justipat
ms.reviewer: sngun
ms.openlocfilehash: 37e5cb817db2c54a07ab04c4dcc31b1976fdf03d
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81450063"
---
# <a name="how-to-use-a-system-assigned-managed-identity-to-access-azure-cosmos-db-data"></a>Azure Cosmos DB verilerine erişmek için sistem tarafından atanmış yönetilen kimlik nasıl kullanılır?

Bu makalede, [yönetilen kimliklerden](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)yararlanarak Azure Cosmos DB tuşlarına erişmek için **sağlam, anahtarlı bir döndürme agnostik,** çözüm kuracağınız. Bu makaledeki örnekte bir Azure İşlevi kullanır. Ancak, yönetilen kimlikleri destekleyen herhangi bir hizmeti kullanarak bu çözüme ulaşabilirsiniz. 

Azure Cosmos DB tuşlarını kopyalamaya gerek kalmadan Azure Cosmos DB'ye erişebilen bir Azure İşlevi oluşturmayı öğreneceksiniz. Fonksiyonu her dakika uyanmak ve bir akvaryum akvaryum mevcut sıcaklığını kaydedecektir. Zamanlayıcı tetiklenen Bir Zamanlayıcının nasıl ayarlandığını öğrenmek için [Azure'da zamanlayıcı](../azure-functions/functions-create-scheduled-function.md) makalesi tarafından tetiklenen bir işlev oluştur'a bakın.

Senaryoyu basitleştirmek için, eski sıcaklık belgelerinin temizlenmesi zaten yapılandırılmış bir [Time To Live](./time-to-live.md) ayarı tarafından gerçekleştirilir. 

## <a name="assign-a-system-assigned-managed-identity-to-an-azure-function"></a>Bir Azure İşlevine sistem tarafından atanan yönetilen kimlik atama

Bu adımda, Azure İşlevinize sistem tarafından atanmış yönetilen bir kimlik atarsınız.

1. Azure [portalında](https://portal.azure.com/)Azure **İşlevi** bölmesi'ni açın ve işlev uygulamanıza gidin. 

1. Platform **özelliklerini** > **kimlik** sekmesini açın: 

   ![Kimlik Sekmesi](./media/managed-identity-based-authentication/identity-tab-selection.png)

1. **Kimlik** **sekmesinde** **Sistem Kimlik** durumunu açın. **Kaydet'i**seçtiğinizden emin olun ve sistem kimliğini açmak istediğinizi onaylayın. Sonunda Sistem **Kimlik** bölmesi aşağıdaki gibi görünmelidir:  

   ![Sistem Kimliği açık](./media/managed-identity-based-authentication/identity-tab-system-managed-on.png)

## <a name="grant-the-managed-identity-access-to-your-azure-cosmos-account"></a>Azure Cosmos hesabınıza yönetilen kimlik erişimi verme

Bu adımda, Azure İşlevi'nin sistem tarafından atanan yönetilen kimliğine bir rol atarsınız. Azure Cosmos DB'de yönetilen kimliğe atayabileceğiniz birden çok yerleşik rol vardır. Bu çözüm için aşağıdaki iki rolü kullanırsınız:

|Yerleşik rol  |Açıklama  |
|---------|---------|
|[DocumentDB Hesap Katılımcısı](../role-based-access-control/built-in-roles.md#documentdb-account-contributor)|Azure Cosmos DB hesaplarını yönetebilirsiniz. Okuma/yazma tuşlarının alınmasını sağlar. |
|[Cosmos DB Hesap Okuyucu](../role-based-access-control/built-in-roles.md#cosmos-db-account-reader-role)|Azure Cosmos DB hesap verilerini okuyabilir. Okuma anahtarlarının alınmasını sağlar. |

> [!IMPORTANT]
> Azure Cosmos DB'deki RBAC desteği yalnızca uçak işlemlerini denetlemek için geçerlidir. Veri düzlemi işlemleri ana anahtarlar veya kaynak belirteçleri kullanılarak güvenlihale alınır. Daha fazla bilgi edinmek [için veri makalesine Güvenli erişim](secure-access-to-data.md) e bakın.

> [!TIP] 
> Roller atarken, yalnızca gerekli erişimi atayın. Hizmetiniz yalnızca veri okumayı gerektiriyorsa, yönetilen kimliği **Cosmos DB Account Reader** rolüne atayın. En az ayrıcalık erişiminin önemi hakkında daha fazla bilgi [için, ayrıcalıklı hesaplar makalesinin daha düşük pozlama bölümüne](../security/fundamentals/identity-management-best-practices.md#lower-exposure-of-privileged-accounts) bakın.

Senaryonuz için sıcaklığı okuyacak ve bu verileri Azure Cosmos DB'deki bir kapsayıcıya geri yazacaksınız. Verileri yazmanız gerekdığından **DocumentDB Hesap Katılımcısı** rolünü kullanırsınız. 

1. Azure portalında oturum açın ve Azure Cosmos DB hesabınıza gidin. Erişim **Yönetimi (IAM) Bölmesini**ve ardından **Rol Atamaları** sekmesini açın:

   ![IAM Bölmesi](./media/managed-identity-based-authentication/cosmos-db-iam-tab.png)

1. + **Ekle** düğmesini seçin, ardından **rol ataması ekleyin.**

1. **Rol Atama Ekle** paneli sağa açılır:

   ![Rol Ekle](./media/managed-identity-based-authentication/cosmos-db-iam-tab-add-role-pane.png)

   * **Rol** - **DocumentDB Hesap Katılımcısı'nı** seçin
   * **Erişim ata -** **Sistemi atanmış yönetilen kimlik** alt bölümünde **İşlev Uygulaması'nı**seçin.
   * **Select** - Bölme, aboneliğinizde **Yönetilen Sistem Kimliğine**sahip tüm işlev uygulamalarıyla doldurulacaktır. Bizim durumumuzda **SummaryService** fonksiyon uygulamasını seçiyorum: 

      ![Atama Seçin](./media/managed-identity-based-authentication/cosmos-db-iam-tab-add-role-pane-filled.png)

1. İşlev uygulamasının kimliği seçildikten sonra **Kaydet'e**tıklayın.

## <a name="programmatically-access-the-azure-cosmos-db-keys-from-the-azure-function"></a>Azure İşlevi'nden Azure Cosmos DB tuşlarına programlı olarak erişin

Şimdi sistem tarafından atanmış yönetilen bir kimliğe sahip bir işlev uygulamamız var. Bu kimliğe Azure Cosmos DB izinlerinde **DocumentDB Hesap Katılımcısı** rolü verilir. Aşağıdaki işlev uygulama kodu Azure Cosmos DB tuşlarını alır, bir CosmosClient nesnesi oluşturur, sıcaklığı alır ve bunu Cosmos DB'ye kaydeder.

Bu örnek, Azure Cosmos DB hesap anahtarlarınıza erişmek için [Liste Tuşları API'sini](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/DatabaseAccounts/ListKeys) kullanır.

> [!IMPORTANT] 
> [ **Cosmos DB Hesap Okuyucu** ](#grant-the-managed-identity-access-to-your-azure-cosmos-account) rolünü atamak istiyorsanız, yalnızca Liste Anahtarları [api'sini](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/DatabaseAccounts/ListReadOnlyKeys)kullanmanız gerekir. Bu yalnızca okunan anahtarları dolduracaktır.

Liste Anahtarları API `DatabaseAccountListKeysResult` nesnesini döndürür. Bu tür C# kitaplıklarında tanımlanmamıştır. Aşağıdaki kod bu sınıfın uygulanmasını gösterir:  

```csharp 
namespace SummarizationService 
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

Örnek, "TemperatureRecord" adlı basit bir belge yi de kullanır ve bu belge aşağıdaki gibi tanımlanır:

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

Sistem tarafından atanan yönetilen kimlik belirteci almak için [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) kitaplığını kullanırsınız. Kitaplık hakkında belirteç ve daha fazla bilgi almanın diğer yollarını öğrenmek [için Hizmet Kimlik Doğrulamasına Hizmet](../key-vault/general/service-to-service-authentication.md) makalesine bakın. `Microsoft.Azure.Service.AppAuthentication`

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
    public static class TemperatureMonitor
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

        [FunctionName("TemperatureMonitor")]
        public static async Task Run([TimerTrigger("0 * * * * *")]TimerInfo myTimer, ILogger log)
        {
            log.LogInformation($"Starting temperature monitoring: {DateTime.Now}");

            // AzureServiceTokenProvider will help us to get the Service Managed token.
            var azureServiceTokenProvider = new AzureServiceTokenProvider();

            // In order to get the Service Managed token we need to authenticate to the Azure Resource Manager.
            string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://management.azure.com/");

            // To get the Azure Cosmos DB keys setup the List Keys API:
            string endpoint = $"https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DocumentDB/databaseAccounts/{accountName}/listKeys?api-version=2019-12-12";

            // setup an HTTP Client and add the access token.
            HttpClient httpClient = new HttpClient();
            httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

            // Post to the endpoint to get the keys result.
            var result = await httpClient.PostAsync(endpoint, new StringContent(""));

            // Get the Result back as a DatabaseAccountListKeysResult.
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
            // fake the temperature sensor for this demo
            Random r = new Random(DateTime.UtcNow.Second);
            return r.Next(0, 120);
        }
    }
}
```

Azure [İşlevinizi dağıtmaya](../azure-functions/functions-create-first-function-vs-code.md)artık hazırsınız.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB ve Active Directory ile sertifika tabanlı kimlik doğrulama](certificate-based-authentication.md)
* [Azure Key Vault kullanarak Azure Cosmos anahtarlarının güvenliğini sağlama](access-secrets-from-keyvault.md)
* [Azure Cosmos DB için güvenlik taban çizgisi](security-baseline.md)
