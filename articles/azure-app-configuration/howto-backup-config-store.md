---
title: Azure uygulama yapılandırma deposundan anahtar değerlerini otomatik olarak yedekle
description: Uygulama yapılandırma depoları arasında anahtar değerlerinin otomatik yedeklemesini ayarlamayı öğrenin
services: azure-app-configuration
author: avanigupta
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: how-to
ms.date: 04/27/2020
ms.author: avgupta
ms.openlocfilehash: 6dd485adb71bf05be6499f2fc18572e8a28357d7
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86209901"
---
# <a name="backup-app-configuration-stores-automatically"></a>Uygulama yapılandırma depolarını otomatik olarak yedekle

Bu makalede, birincil uygulama yapılandırma deposundan bir ikincil depoya anahtar değerlerini otomatik olarak yedeklemeyi nasıl ayarlayacağınızı öğreneceksiniz. Uygulama yapılandırmasıyla Azure Event Grid tümleştirmesinden yararlanır. Ayarladıktan sonra, uygulama yapılandırması bir yapılandırma deposundaki anahtar değerlerinde yapılan değişiklikler için Event Grid olayları yayımlar. Event Grid, kullanıcıların anahtar değerleri oluşturulduğu, güncelleştirildiği veya silindiği her seferinde yayınlanan olaylara abone olabileceği çeşitli Azure hizmetlerini destekler.

## <a name="overview"></a>Genel Bakış

Bu öğreticide, Event Grid olayları almak için bir Azure depolama kuyruğu kullanacaksınız ve depolama sırasındaki olayları toplu halde işlemek için Azure Işlevlerinin Zamanlayıcı tetikleyicisini kullanacaksınız. Tetiklendiği zaman, işlev, birincil uygulama yapılandırma deposundan değiştirilen anahtarların en son değerlerini alır ve ikincil depoyu uygun şekilde güncelleştirir. Bu kurulum, tek bir yedekleme işleminde kısa bir dönemde gerçekleşen birden çok değişikliği birleştirmeye ve uygulama yapılandırma depolarınız üzerinde aşırı isteklerinizi önlemenize yardımcı olur.

![Uygulama yapılandırma deposu yedekleme mimarisi](./media/config-store-backup-architecture.png)

## <a name="resource-provisioning"></a>Kaynak Sağlama

Uygulama yapılandırma depolarını yedeklemenin arkasındaki mosyon, uygulamanızın coğrafi dayanıklılığını artırmak için farklı Azure bölgelerinde birden fazla yapılandırma deposu kullanmaktır. Bunu başarmak için birincil ve ikincil Mağazalarınız farklı Azure bölgelerinde olmalıdır. Bu öğreticide oluşturulan diğer tüm kaynaklar, seçtiğiniz herhangi bir bölgede sağlanabilir. Bunun nedeni birincil bölge kapalıysa, birincil bölgeye yeniden erişilene kadar yedekleme için yeni bir şey olmayacaktır.

Bu öğreticide, bölgesinde `centralus` ve bölgesindeki tüm diğer kaynaklarda ikincil depo oluşturacaksınız `westus` .

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/). İsteğe bağlı olarak Azure Cloud Shell kullanabilirsiniz.
- Azure geliştirme iş yüküyle [Visual Studio 2019](https://visualstudio.microsoft.com/vs) .
- [.NET Core SDK](https://dotnet.microsoft.com/download)indirin ve yükleyin.
- Azure CLı 'nın en son sürümü (2.3.1 veya üzeri). Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli). Azure CLı kullanıyorsanız, önce kullanarak oturum açmanız gerekir `az login` . İsteğe bağlı olarak Azure Cloud Shell kullanabilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal koleksiyondur.

[az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun.

Aşağıdaki örnek `westus` konumunda `<resource_group_name>` adlı bir kaynak grubu oluşturur.  `<resource_group_name>` değerini kaynak grubunuz için benzersiz bir adla değiştirin.

```azurecli-interactive
resourceGroupName="<resource_group_name>"
az group create --name $resourceGroupName --location westus
```

## <a name="create-app-configuration-stores"></a>Uygulama yapılandırma depoları oluşturma

Birincil ve ikincil uygulama yapılandırma depolarınızı farklı bölgelerde oluşturun.
 `<primary_appconfig_name>`Ve `<secondary_appconfig_name>` yapılandırma depolarınız için benzersiz adlarla değiştirin. Depo adı, DNS adı olarak kullanıldığı için benzersiz olmalıdır.

```azurecli-interactive
primaryAppConfigName="<primary_appconfig_name>"
secondaryAppConfigName="<secondary_appconfig_name>"
az appconfig create \
  --name $primaryAppConfigName \
  --location westus \
  --resource-group $resourceGroupName\
  --sku standard

az appconfig create \
  --name $secondaryAppConfigName \
  --location centralus \
  --resource-group $resourceGroupName\
  --sku standard
```

## <a name="create-azure-storage-queue"></a>Azure depolama kuyruğu oluşturma

Event Grid tarafından yayınlanan olayları almak için bir depolama hesabı ve depolama kuyruğu oluşturun.

```azurecli-interactive
storageName="<unique_storage_name>"
queueName="<queue_name>"
az storage account create -n $storageName -g $resourceGroupName -l westus --sku Standard_LRS
az storage queue create --name $queueName --account-name $storageName --auth-mode login
```

[!INCLUDE [event-grid-register-provider-cli.md](../../includes/event-grid-register-provider-cli.md)]

## <a name="subscribe-to-your-app-configuration-store-events"></a>Uygulama yapılandırma deposu olaylarınızı abone olma

Birincil uygulama yapılandırma deposundan bu iki olaya abone olursunuz:

- `Microsoft.AppConfiguration.KeyValueModified`
- `Microsoft.AppConfiguration.KeyValueDeleted`

Aşağıdaki komut, uç nokta türünün olarak ayarlandığı `storagequeue` ve uç noktanın sıra kimliği olarak ayarlandığı depolama kuyruğuna gönderilen iki olay için bir Event Grid aboneliği oluşturur. `<event_subscription_name>`Olay aboneliği için tercih ettiğiniz adla değiştirin.

```azurecli-interactive
storageId=$(az storage account show --name $storageName --resource-group  $resourceGroupName --query id --output tsv)
queueId="$storageId/queueservices/default/queues/$queueName"
appconfigId=$(az appconfig show --name $primaryAppConfigName --resource-group $resourceGroupName --query id --output tsv)
eventSubscriptionName="<event_subscription_name>"
az eventgrid event-subscription create \
  --source-resource-id $appconfigId \
  --name $eventSubscriptionName \
  --endpoint-type storagequeue \
  --endpoint $queueId \
  --included-event-types Microsoft.AppConfiguration.KeyValueModified Microsoft.AppConfiguration.KeyValueDeleted 
```

## <a name="create-azure-functions-for-handling-events-from-storage-queue"></a>Depolama kuyruğundan olayları işlemek için Azure Işlevleri oluşturma

### <a name="setup-with-ready-to-use-azure-functions"></a>Kullanıma hazırlama Azure Işlevleri ile kurulum

Bu öğreticide, C# Azure Işlevleri ile aşağıdaki özelliklerle çalışmaya çalışacaktır:
- Çalışma zamanı yığını .NET Core 3,1
- Azure Işlevleri çalışma zamanı sürüm 3. x
- Her 10 dakikada bir Zamanlayıcı tarafından tetiklenen işlev

Verilerinizin yedeklenmesini daha kolay hale getirmek için, kodda herhangi bir değişiklik yapmadan kullanabileceğiniz [Azure işlevlerini](https://github.com/Azure/AppConfiguration/tree/master/examples/ConfigurationStoreBackup) test ediyoruz ve yayımladık. Proje dosyalarını indirin ve [Visual Studio 'dan kendi Azure işlev uygulaması yayımlayın.](/azure/azure-functions/functions-develop-vs#publish-to-azure)

> [!IMPORTANT]
> İndirdiğiniz koddaki ortam değişkenlerinde herhangi bir değişiklik yapmayın. Sonraki bölümde gerekli uygulama ayarlarını oluşturacaksınız.
>

### <a name="build-your-own-azure-functions"></a>Kendi Azure Işlevlerinizi oluşturun

Yukarıda belirtilen örnek kod gereksinimlerinizi karşılamıyorsa, kendi Azure Işlevlerinizi de oluşturabilirsiniz. Yedekleme işlemini tamamlamak için işleviniz aşağıdaki görevleri gerçekleştirebilmelidir:
- Event Grid bir bildirim içerip içermediğinden, depolama kuyruğunuzun içeriğini düzenli aralıklarla okuyun. Uygulama ayrıntıları için [depolama kuyruğu SDK 'sına](/azure/storage/queues/storage-quickstart-queues-dotnet) bakın.
- Depolama kuyruğunuz [Event Grid olay bildirimleri](/azure/azure-app-configuration/concept-app-configuration-event?branch=pr-en-us-112982#event-schema)içeriyorsa, tüm benzersiz <anahtarı (etiket>) olay iletilerinden ayıklayın. Anahtar ve etiket birleşimi, birincil depodaki anahtar-değer değişikliklerinin benzersiz tanımlayıcısıdır.
- Birincil depodan tüm ayarları okuyun. Yalnızca ikincil depoda, depolama kuyruğunda karşılık gelen bir olaya sahip olan ayarları güncelleştirin. İkincil depodan, depolama sırasında bulunan ancak birincil depoda bulunmayan tüm ayarları silin. Yapılandırma depolarına programlı olarak erişmek için [uygulama yapılandırma SDK 'sını](https://github.com/Azure/AppConfiguration#sdks) kullanabilirsiniz.
- İşlem sırasında özel durum yoksa, depolama kuyruğundan iletileri silin.
- Gereksinimlerinize göre hata işlemeyi uyguladığınızdan emin olun. İşlemek isteyebileceğiniz bazı yaygın özel durumları görmek için yukarıdaki kod örneğine başvurabilirsiniz.

Azure Işlevleri oluşturma hakkında daha fazla bilgi edinmek için bkz. [Azure 'da bir Zamanlayıcı tarafından tetiklenen](/azure/azure-functions/functions-create-scheduled-function) ve [Visual Studio kullanarak Azure işlevleri geliştiren](/azure/azure-functions/functions-develop-vs)bir işlev oluşturma.


> [!IMPORTANT]
> Birincil yapılandırma deponuzda ne sıklıkta değişiklik yapmanız gerektiğini gösteren Zamanlayıcı zamanlamasını seçmek için en iyi kararlarınızı kullanın. İşlevin çok sık çalıştırılması, deponuzda azaltma isteklerini sona erdirmek için unutmayın.
>


## <a name="create-azure-function-app-settings"></a>Azure İşlev Uygulaması ayarları oluşturma

Sağladığımız Azure Işlevlerini kullanıyorsanız, Azure İşlev Uygulaması aşağıdaki uygulama ayarlarına ihtiyacınız vardır:
- `PrimaryStoreEndpoint`: Birincil uygulama yapılandırma deposu için uç nokta. Örneğin, `https://{primary_appconfig_name}.azconfig.io`
- `SecondaryStoreEndpoint`: İkincil uygulama yapılandırma deposu için uç nokta. Örneğin, `https://{secondary_appconfig_name}.azconfig.io`
- `StorageQueueUri`: Depolama kuyruğu URI 'SI. Örneğin, `https://{unique_storage_name}.queue.core.windows.net/{queue_name}`

Aşağıdaki komut, Azure İşlev Uygulaması gerekli uygulama ayarlarını oluşturur. Öğesini `<function_app_name>` Azure işlev uygulaması adıyla değiştirin.

```azurecli-interactive
functionAppName="<function_app_name>"
primaryStoreEndpoint="https://$primaryAppConfigName.azconfig.io"
secondaryStoreEndpoint="https://$secondaryAppConfigName.azconfig.io"
storageQueueUri="https://$storageName.queue.core.windows.net/$queueName"
az functionapp config appsettings set --name $functionAppName --resource-group $resourceGroupName --settings StorageQueueUri=$storageQueueUri PrimaryStoreEndpoint=$primaryStoreEndpoint SecondaryStoreEndpoint=$secondaryStoreEndpoint
```


## <a name="grant-access-to-the-managed-identity-of-azure-function-app"></a>Azure İşlev Uygulaması yönetilen kimliğine erişim izni verin

Azure İşlev Uygulaması için sistem tarafından atanan yönetilen bir kimlik eklemek için aşağıdaki komutu veya [Azure Portal](/azure/app-service/overview-managed-identity#add-a-system-assigned-identity) kullanın.

```azurecli-interactive
az functionapp identity assign --name $functionAppName --resource-group $resourceGroupName
```

> [!NOTE]
> Gerekli kaynak oluşturma ve rol yönetimini gerçekleştirmek için hesabınızın `'Owner'` uygun kapsamda (aboneliğiniz veya kaynak grubunuz) izinleri olması gerekir. Rol atamasıyla ilgili yardıma ihtiyacınız varsa [Azure Portal kullanarak Azure rol atamalarını ekleme veya kaldırma hakkında](/azure/role-based-access-control/role-assignments-portal)bilgi edinin.

Azure İşlev Uygulaması yönetilen kimliğini uygulama yapılandırma Depolarınıza erişim izni vermek için aşağıdaki komutları veya [Azure Portal](/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity#grant-access-to-app-configuration) kullanın.
- `App Configuration Data Reader`Birincil uygulama yapılandırma deposunda rol atayın.
- `App Configuration Data Owner`Ikincil uygulama yapılandırma deposunda rol atayın.

```azurecli-interactive
functionPrincipalId=$(az functionapp identity show --name $functionAppName --resource-group  $resourceGroupName --query principalId --output tsv)
primaryAppConfigId=$(az appconfig show -n $primaryAppConfigName --query id --output tsv)
secondaryAppConfigId=$(az appconfig show -n $secondaryAppConfigName --query id --output tsv)

az role assignment create \
    --role "App Configuration Data Reader" \
    --assignee $functionPrincipalId \
    --scope $primaryAppConfigId

az role assignment create \
    --role "App Configuration Data Owner" \
    --assignee $functionPrincipalId \
    --scope $secondaryAppConfigId
```

Azure İşlev Uygulaması 'ın yönetilen kimliğine depolama kuyruğuna erişim izni vermek için aşağıdaki komutu veya [Azure Portal](/azure/storage/common/storage-auth-aad-rbac-portal#assign-rbac-roles-using-the-azure-portal) kullanın. 
- `Storage Queue Data Contributor`Depolama kuyruğuna rol atayın.

```azurecli-interactive
az role assignment create \
    --role "Storage Queue Data Contributor" \
    --assignee $functionPrincipalId \
    --scope $queueId
```

## <a name="trigger-an-app-configuration-event"></a>Uygulama yapılandırma olayını tetikleme

Her şeyin çalıştığından test etmek için birincil depodan bir anahtar değeri oluşturabilir/güncelleştirebilir/silebilirsiniz. Azure Işlevleri Zamanlayıcı tarafından tetiklendikten sonra bu değişikliği ikincil depoda otomatik olarak görmeniz gerekir.

```azurecli-interactive
az appconfig kv set --name $primaryAppConfigName --key Foo --value Bar --yes
```

Olayı tetiklediyseniz ve birkaç dakika içinde olay bildirimini Azure Storage kuyruğuna gönderecek Event Grid. ***Azure işlevlerinizin zamanlanan bir sonraki çalıştırıldıktan sonra***, ikincil deponuzdaki yapılandırma ayarlarını görüntüleyerek birincil depodan güncelleştirilmiş anahtar değerini içerip içermediğinden öğrenin.

> [!NOTE]
> Test ve sorun giderme işlemleri sırasında [Azure işlevlerinizi el ile](/azure/azure-functions/functions-manually-run-non-http) , zamanlanmış Zamanlayıcı tetikleyicisi için beklemeden tetikleyebilirsiniz.

Yedekleme işlevinin başarıyla çalıştırıldığından emin olduktan sonra, anahtarın artık ikincil deponuzda bulunduğunu görebilirsiniz.

```azurecli-interactive
az appconfig kv show --name $secondaryAppConfigName --key Foo
```

```json
{
  "contentType": null,
  "etag": "eVgJugUUuopXnbCxg0dB63PDEJY",
  "key": "Foo",
  "label": null,
  "lastModified": "2020-04-27T23:25:08+00:00",
  "locked": false,
  "tags": {},
  "value": "Bar"
}
```

## <a name="troubleshooting"></a>Sorun giderme

İkincil deponuzda yeni ayarı görmüyorsanız:

- Birincil deponuzda ayarı oluşturduktan ***sonra*** yedekleme işlevinin tetiklendiğinden emin olun.
- Event Grid, olay bildirimini zamanında depolama kuyruğuna gönderemeyebilir. Depolama kuyruğunuzun birincil deponuzdaki olay bildirimini hala içerip içermediğinden emin olun ve varsa, yedekleme işlevini yeniden tetikleyin.
- Herhangi bir hata veya uyarı için [Azure işlevleri günlüklerine](/azure/azure-functions/functions-create-scheduled-function#test-the-function) bakın.
- Azure İşlev Uygulaması Azure Işlevlerinizin okumaya çalıştığı uygulama ayarları için doğru değerler içerdiğinden emin olmak için [Azure Portal](/azure/azure-functions/functions-how-to-use-azure-function-app-settings#get-started-in-the-azure-portal) kullanın.
- Azure Application Insights kullanarak Azure Işlevleriniz için izleme ve uyarı ayarlama da yapabilirsiniz [.](/azure/azure-functions/functions-monitoring?tabs=cmd) 


## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu uygulama yapılandırması ve olay aboneliğiyle çalışmaya devam etmeyi planlıyorsanız, bu makalede oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, aşağıdaki komutu kullanarak bu makalede oluşturduğunuz kaynakları silin.

```azurecli-interactive
az group delete --name $resourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

Anahtar değerlerinizin otomatik yedeklemesini ayarlamayı öğrenmiş olduğunuza göre, uygulamanızın coğrafi dayanıklılığını nasıl artırabileceğiniz hakkında daha fazla bilgi edinin:

- [Dayanıklılık ve olağanüstü durum kurtarma](concept-disaster-recovery.md)
