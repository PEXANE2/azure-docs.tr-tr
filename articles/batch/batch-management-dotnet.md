---
title: Hesap kaynaklarını yönetmek için Toplu İşlem Yönetimi .NET kitaplığını kullanma
description: Toplu İşlem Yönetimi .NET kitaplığı yla Azure Toplu Iş hesabı kaynaklarını oluşturun, silin ve değiştirin.
services: batch
documentationcenter: .net
author: LauraBrenner
manager: evansma
editor: ''
tags: azure-resource-manager
ms.assetid: 16279b23-60ff-4b16-b308-5de000e4c028
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/24/2017
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 79916d769ad8a7228aec8db965c29506ccd78ece
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77023693"
---
# <a name="manage-batch-accounts-and-quotas-with-the-batch-management-client-library-for-net"></a>.NET için Toplu Yönetim istemci kitaplığı ile Toplu Hesapları ve kotaları yönetme

> [!div class="op_single_selector"]
> * [Azure portalında](batch-account-create-portal.md)
> * [Batch Yönetimi .NET](batch-management-dotnet.md)
> 
> 

Toplu iş hesabı oluşturma, silme, anahtar yönetimi ve kota bulmayı otomatikleştirmek için [Toplu İşlem Yönetimi .NET][api_mgmt_net] kitaplığını kullanarak Azure Toplu İşlem uygulamalarınızdaki bakım ek yüküazaltabilirsiniz.

* Herhangi bir bölgedeki **Toplu Iş hesapları oluşturun ve silin.** Örneğin, bağımsız bir yazılım satıcısı (ISV) olarak, müşterilerinize her biri faturalandırma amacıyla ayrı bir Toplu Iş hesabı atandığı bir hizmet sağlarsanız, müşteri portalınıza hesap oluşturma ve silme özellikleri ekleyebilirsiniz.
* Toplu İşlem hesaplarınızdan herhangi biri için hesap anahtarlarını programlı olarak **alın ve yeniden oluşturun.** Bu, hesap anahtarlarının periyodik olarak devrini veya son kullanma tarihini zorunlu eden güvenlik ilkelerine uymanıza yardımcı olabilir. Çeşitli Azure bölgelerinde birden fazla Toplu İşlem hesabınız olduğunda, bu rollover işleminin otomasyonu çözümünüzün verimliliğini artırır.
* **Hesap kotalarını kontrol edin** ve hangi Toplu Iş hesaplarının hangi sınırlara sahip olduğunu belirlemek için deneme yanılma tahminini kaldırın. İş başlatmadan, havuz oluşturmadan veya işlem düğümleri eklemeden önce hesap kotalarınızı kontrol ederek, bu hesaplama kaynaklarının nerede veya ne zaman oluşturulduğunu proaktif olarak ayarlayabilirsiniz. Bu hesaplara ek kaynak ayırmadan önce hangi hesapların kota artışları gerektirdiğini belirleyebilirsiniz.
* Toplu Yönetim .NET, [Azure Etkin Dizin][aad_about]ve [Azure Kaynak Yöneticisi'ni][resman_overview] aynı uygulamada birlikte kullanarak tam özellikli bir yönetim deneyimi için diğer Azure hizmetlerinin **özelliklerini birleştirin.** Bu özellikleri ve API'lerini kullanarak sürtünmesiz bir kimlik doğrulama deneyimi, kaynak grupları oluşturma ve silme olanağı ve uçtan uca yönetim çözümü için yukarıda açıklanan yetenekleri sağlayabilirsiniz.

> [!NOTE]
> Bu makalede Toplu İşlem hesaplarınızın, anahtarlarınızın ve kotalarınızın programlı yönetimi odaklansa da, [Azure portalını][azure_portal]kullanarak bu etkinliklerin çoğunu gerçekleştirebilirsiniz. Daha fazla bilgi için, Azure Portalı ve [Kotaları ve Azure Toplu İş hizmeti için sınırları](batch-quota-limit.md)kullanarak bir Azure Toplu İş hesabı [oluşturun'a](batch-account-create-portal.md) bakın.
> 
> 

## <a name="create-and-delete-batch-accounts"></a>Toplu iş hesapları oluşturma ve silme
Belirtildiği gibi, Toplu İşlem Yönetimi API'sinin birincil özelliklerinden biri, Bir Azure bölgesinde Toplu İş hesapları oluşturmak ve silmektir. Bunu yapmak için [BatchManagementClient.Account.CreateAsync][net_create] ve [DeleteAsync][net_delete]veya eşzamanlı karşılıklarını kullanın.

Aşağıdaki kod snippet bir hesap oluşturur, Toplu Iş hizmetinden yeni oluşturulan hesabı alır ve sonra siler. Bu snippet ve bu makalede `batchManagementClient` diğerleri, [BatchManagementClient][net_mgmt_client]tam olarak başharfli bir örneğidir.

```csharp
// Create a new Batch account
await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get the new account from the Batch service
AccountResource account = await batchManagementClient.Account.GetAsync(
    "MyResourceGroup",
    "mynewaccount");

// Delete the account
await batchManagementClient.Account.DeleteAsync("MyResourceGroup", account.Name);
```

> [!NOTE]
> Toplu İşlem Yönetimi .NET kitaplığını ve BatchManagementClient sınıfını kullanan uygulamalar, **hizmet yöneticisinin** veya toplu iş hesabının sahibi olan aboneliğin yönetilmesini **gerektirir.** Daha fazla bilgi için Azure Etkin Dizin bölümüne ve [Hesap Yönetimi][acct_mgmt_sample] kodu örneğine bakın.
> 
> 

## <a name="retrieve-and-regenerate-account-keys"></a>Hesap anahtarlarını alma ve yenileme
[ListKeysAsync'i][net_list_keys]kullanarak aboneliğinizdeki herhangi bir Toplu Iş hesabından birincil ve ikincil hesap anahtarlarını edinin. [RereKeyAsync][net_regenerate_keys]kullanarak bu anahtarları yeniden oluşturabilirsiniz.

```csharp
// Get and print the primary and secondary keys
BatchAccountListKeyResult accountKeys =
    await batchManagementClient.Account.ListKeysAsync(
        "MyResourceGroup",
        "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.Primary);
Console.WriteLine("Secondary key: {0}", accountKeys.Secondary);

// Regenerate the primary key
BatchAccountRegenerateKeyResponse newKeys =
    await batchManagementClient.Account.RegenerateKeyAsync(
        "MyResourceGroup",
        "mybatchaccount",
        new BatchAccountRegenerateKeyParameters() {
            KeyName = AccountKeyType.Primary
            });
```

> [!TIP]
> Yönetim uygulamalarınız için kolaylaştırılmış bir bağlantı iş akışı oluşturabilirsiniz. İlk olarak, [ListKeysAsync][net_list_keys]ile yönetmek istediğiniz Toplu iş hesabı için bir hesap anahtarı edinin. Daha sonra, Toplu İşlem .NET kitaplığında [Toplu Müşteri'yi][net_batch_client]başlatmada kullanılan [Toplu Paylaşımlı Kimlik Bilgileri][net_sharedkeycred] sınıfını başlatmada bu anahtarı kullanın.
> 
> 

## <a name="check-azure-subscription-and-batch-account-quotas"></a>Azure aboneliği ve Toplu Iş hesabı kotalarını kontrol edin
Azure abonelikleri ve Toplu İşlem gibi tek tek Azure hizmetlerinin tümü, içlerindeki belirli varlıkların sayısını sınırlayan varsayılan kotalara sahiptir. Azure abonelikleri için varsayılan kotalar için [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalara](../azure-resource-manager/management/azure-subscription-service-limits.md)bakın. Toplu İşlem hizmetinin varsayılan kotaları [için Azure Toplu İşlem hizmeti için Kotalar ve sınırlar'a](batch-quota-limit.md)bakın. Toplu İşlem Yönetimi .NET kitaplığını kullanarak, uygulamalarınızda bu kotaları kontrol edebilirsiniz. Bu, hesap eklemeden veya havuzlar ve bilgi işlem düğümleri gibi kaynakları hesaplamadan önce ayırma kararları vermenizi sağlar.

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a>Toplu iş hesabı kotaları için Azure aboneliğini denetleme
Bir bölgede Toplu Iş hesabı oluşturmadan önce, azure aboneliğinizi kontrol ederek o bölgeye hesap ekleyip eklemeyeceğinize bakabilirsiniz.

Aşağıdaki kod snippet'inde, öncelikle abonelik içinde bulunan tüm Toplu Iş hesaplarının bir koleksiyonunu almak için [BatchManagementClient.Account.ListAsync'i][net_mgmt_listaccounts] kullanıyoruz. Bu koleksiyonu aldıktan sonra, hedef bölgede kaç hesap olduğunu belirleriz. Daha sonra Toplu Hesap kotasını elde etmek ve o bölgede kaç hesap (varsa) oluşturulabileceğini belirlemek için [BatchManagementClient.Subscriptions'ı][net_mgmt_subscriptions] kullanırız.

```csharp
// Get a collection of all Batch accounts within the subscription
BatchAccountListResponse listResponse =
        await batchManagementClient.Account.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}",
    creds.SubscriptionId,
    accounts.Count);

// Get a count of all accounts within the target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get the account quota for the specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in the target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in the {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

Yukarıdaki snippet olarak, `creds` [TokenCloud Credentials][azure_tokencreds]bir örneğidir. Bu nesneyi oluşturmaya ilişkin bir örnek görmek için GitHub'daki [Hesap Yönetimi][acct_mgmt_sample] kodu örneğine bakın.

### <a name="check-a-batch-account-for-compute-resource-quotas"></a>Bilgi işlem kaynak kotaları için Toplu Iş hesabını denetleme
Toplu İşlem çözümünüzün hesaplama kaynaklarını artırmadan önce, ayırmak istediğiniz kaynakların hesabın kotalarını aşmamasını denetleyebilirsiniz. Aşağıdaki kod snippet'inde, Batch hesabının kota `mybatchaccount`bilgilerini yazdırıyoruz. Kendi uygulamanızda, hesabın oluşturulacak ek kaynakları işleyip işleyebileceğini belirlemek için bu tür bilgileri kullanabilirsiniz.

```csharp
// First obtain the Batch account
BatchAccountGetResponse getResponse =
    await batchManagementClient.Account.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print the compute resource quotas for the account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [!IMPORTANT]
> Azure abonelikleri ve hizmetleri için varsayılan kotalar olsa da, bu sınırların çoğu [Azure portalında][azure_portal]bir istek yayınlayarak yükseltilebilir. Örneğin, Toplu İş hesabı kotalarınızı artırmaya ilişkin talimatlar için [Azure Toplu İş hizmetinin Kotaları ve sınırları](batch-quota-limit.md) na bakın.
> 
> 

## <a name="use-azure-ad-with-batch-management-net"></a>Toplu İşlem Yönetimi ile Azure AD'yi kullanma .NET

Toplu İşlem Yönetimi .NET kitaplığı bir Azure kaynak sağlayıcısı istemcisidir ve hesap kaynaklarını programlı bir şekilde yönetmek için [Azure Kaynak Yöneticisi][resman_overview] ile birlikte kullanılır. Toplu İşlem Yönetimi .NET kitaplığı ve [Azure Kaynak Yöneticisi][resman_overview]de dahil olmak üzere herhangi bir Azure kaynak sağlayıcısı istemcisi aracılığıyla yapılan istekleri doğrulamak için Azure AD'nin gerçeklemesi gerekir. Toplu İşlem Yönetimi .NET kitaplığıyla Azure AD'yi kullanma hakkında daha fazla bilgi için [Toplu İş çözümlerini doğrulamak için Azure Etkin Dizini'ni kullanın'a](batch-aad-auth.md)bakın. 

## <a name="sample-project-on-github"></a>GitHub'da örnek proje

Toplu İşlem Yönetimi .NET'i iş başında görmek için GitHub'daki [Hesap Yönetimi][acct_mgmt_sample] örnek projesine göz atın. Hesap Yönetimi örnek uygulaması aşağıdaki işlemleri gösterir:

1. [ADAL'ı][aad_adal]kullanarak Azure AD'den bir güvenlik belirteci edinin. Kullanıcı zaten oturum açmışsa, Azure kimlik bilgileri istenir.
2. Azure AD'den elde edilen güvenlik belirteciyle, hesapla ilişkili aboneliklerin listesi için Azure'u sorgulamak için bir [Abonelik İstemci][resman_subclient] oluşturun. Kullanıcı, birden fazla abonelik içeriyorsa listeden bir abonelik seçebilir.
3. Seçili abonelikle ilişkili kimlik bilgilerini alın.
4. Kimlik bilgilerini kullanarak bir [Kaynak Yönetimiİste][resman_client] nesnesi oluşturun.
5. Kaynak grubu oluşturmak için [Kaynak Yönetimiİste][resman_client] nesnesini kullanın.
6. Birkaç Toplu Iş hesabı işlemi gerçekleştirmek için [BatchManagementClient][net_mgmt_client] nesnesini kullanın:
   * Yeni kaynak grubunda bir Toplu Iş hesabı oluşturun.
   * Toplu İşlem hizmetinden yeni oluşturulan hesabı alın.
   * Yeni hesabın hesap anahtarlarını yazdırın.
   * Hesap için yeni bir birincil anahtarı yeniden oluşturun.
   * Hesabın kota bilgilerini yazdırın.
   * Abonelik için kota bilgilerini yazdırın.
   * Abonelik içindeki tüm hesapları yazdırın.
   * Yeni oluşturulan hesabı silin.
7. Kaynak grubunu silin.

Yeni oluşturulan Toplu Iş hesabı ve kaynak grubunu silmeden önce azure [portalında][azure_portal]görüntüleyebilirsiniz:

Örnek uygulamayı başarıyla çalıştırmak için, önce Azure portalında Azure AD kiracınıza kaydetmeniz ve Azure Kaynak Yöneticisi API'sine izin vermeniz gerekir. [Active Directory ile Toplu Yönetimi Doğrulama çözümlerinde](batch-aad-auth-management.md)sağlanan adımları izleyin.


[aad_about]:../active-directory/fundamentals/active-directory-whatis.md "Azure Etkin Dizin nedir?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]:../active-directory/develop/authentication-scenarios.md "Azure AD için Kimlik Doğrulama Senaryoları"
[aad_integrate]:../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md "Uygulamaları Azure Etkin Dizini ile Tümleştirme"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_mgmt_net]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[azure_portal]: https://portal.azure.com
[azure_storage]: https://azure.microsoft.com/services/storage/
[azure_tokencreds]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.tokencloudcredentials.aspx
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_list_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.listkeysasync.aspx
[net_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.createasync.aspx
[net_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.deleteasync.aspx
[net_regenerate_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.regeneratekeyasync.aspx
[net_sharedkeycred]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.auth.batchsharedkeycredentials.aspx
[net_mgmt_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.aspx
[net_mgmt_subscriptions]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.subscriptions.aspx
[net_mgmt_listaccounts]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.iaccountoperations.listasync.aspx
[resman_api]: https://msdn.microsoft.com/library/azure/mt418626.aspx
[resman_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx
[resman_subclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.subscriptions.subscriptionclient.aspx
[resman_overview]: ../azure-resource-manager/management/overview.md

[1]: ./media/batch-management-dotnet/portal-01.png
[2]: ./media/batch-management-dotnet/portal-02.png
[3]: ./media/batch-management-dotnet/portal-03.png
