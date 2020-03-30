---
title: Azure Etkin Dizini ile yönetilen bir kimlik kimlik doğrulama
description: Bu makalede, Azure Etkinlik Hub'ları kaynaklarına erişmek için Azure Etkin Dizini ile yönetilen bir kimliğin kimlik doğrulaması hakkında bilgi sağlanır
services: event-hubs
ms.service: event-hubs
documentationcenter: ''
author: spelluru
manager: ''
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: spelluru
ms.openlocfilehash: 672b663a9cab72d465ea00e0a5ade364eadbf64e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78251521"
---
# <a name="authenticate-a-managed-identity-with-azure-active-directory-to-access-event-hubs-resources"></a>Etkinlik Hub'ları Kaynaklarına erişmek için Azure Etkin Dizini ile yönetilen bir kimliğin kimliğini doğrulatın
Azure Etkinlik Hub'ları Azure kaynakları için yönetilen [kimliklerle](../active-directory/managed-identities-azure-resources/overview.md)Azure Etkin Dizin (Azure AD) kimlik doğrulamasını destekler. Azure kaynakları için yönetilen kimlikler, Azure Sanal Makinelerde (VM), İşlev uygulamalarında, Sanal Makine Ölçek Kümeleri'nde ve diğer hizmetlerde çalışan uygulamalardan Azure AD kimlik bilgilerini kullanarak Etkinlik Hub'ları kaynaklarına erişimi yetkilendirmeye izin verebilir. Azure kaynakları için yönetilen kimlikleri ve Azure AD kimlik doğrulaması kullanarak, bulutta çalışan uygulamalarınızla kimlik bilgilerini depolamaktan kaçınabilirsiniz.

Bu makalede, bir Azure VM'den yönetilen bir kimlik kullanarak bir etkinlik merkezine erişime nasıl yetki verileni gösterilmektedir.

## <a name="enable-managed-identities-on-a-vm"></a>VM'de yönetilen kimlikleri etkinleştirme
VM'nizden Etkinlik Hub'ları kaynaklarını yetkilendirmek için Azure Kaynakları için yönetilen kimlikleri kullanabilmeniz için önce VM'deki Azure Kaynakları için yönetilen kimlikleri etkinleştirmeniz gerekir. Azure Kaynakları için yönetilen kimlikleri nasıl etkinleştireceklerini öğrenmek için şu makalelerden birine bakın:

- [Azure portalında](../active-directory/managed-service-identity/qs-configure-portal-windows-vm.md)
- [Azure PowerShell](../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure CLI](../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager şablonu](../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure Kaynak Yöneticisi istemci kitaplıkları](../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

## <a name="grant-permissions-to-a-managed-identity-in-azure-ad"></a>Azure AD'de yönetilen bir kimliğe izin verme
Uygulamanızdaki yönetilen bir kimlikten Olay Hub'ları hizmetine bir isteği yetkilendirmek için, önce yönetilen kimlik için rol tabanlı erişim denetimi (RBAC) ayarlarını yapılandırın. Azure Olay Hub'ları, Olay Hub'ları gönderme ve okuma izinlerini kapsayan RBAC rollerini tanımlar. RBAC rolü yönetilen bir kimliğe atandığında, yönetilen kimliğe uygun kapsamda Olay Hub'ları verilerine erişim izni verilir.

RBAC rolleri atama hakkında daha fazla bilgi [için, Olay Hub'ları kaynaklarına erişmek için Azure Etkin Dizinile Kimlik Doğrulaması](authorize-access-azure-active-directory.md)konusuna bakın.

## <a name="use-event-hubs-with-managed-identities"></a>Event Hubs'ı yönetilen kimliklerle kullanma
Yönetilen kimliklere sahip Olay Hub'larını kullanmak için, kimliği rolü ve uygun kapsamı atamanız gerekir. Bu bölümdeki yordam, yönetilen bir kimlik altında çalışan ve Olay Hub kaynaklarına erişen basit bir uygulama kullanır.

Burada [Azure Uygulama Hizmeti'nde](https://azure.microsoft.com/services/app-service/)barındırılan örnek bir web uygulaması kullanıyoruz. Bir web uygulaması oluşturmak için adım adım talimatlar için [Azure'da ASP.NET Core web uygulaması oluşturma](../app-service/app-service-web-get-started-dotnet.md)

Uygulama oluşturulduktan sonra aşağıdaki adımları izleyin: 

1. **Ayarlar'a** gidin ve **Kimlik'i**seçin. 
1. **Üzerinde**olmak için **Durumu** seçin. 
1. Ayarları kaydetmek için **Kaydet**’i seçin. 

    ![Bir web uygulaması için yönetilen kimlik](./media/authenticate-managed-identity/identity-web-app.png)

Bu ayarı etkinleştirdikten sonra, Azure Etkin Dizininizde (Azure AD) yeni bir hizmet kimliği oluşturulur ve Uygulama Hizmeti ana bilgisayarolarak yapılandırılır.

Şimdi, bu hizmet kimliğini Olay Hub'ları kaynaklarınızda gerekli kapsamdaki bir role atayın.

### <a name="to-assign-rbac-roles-using-the-azure-portal"></a>Azure portalını kullanarak RBAC rollerini atamak için
Etkinlik Hub'ları kaynaklarına bir rol atamak için Azure portalındaki kaynağa gidin. Kaynak için Erişim Denetimi (IAM) ayarlarını görüntüleyin ve rol atamalarını yönetmek için aşağıdaki yönergeleri izleyin:

> [!NOTE]
> Aşağıdaki adımlar, Olay Hub'ları ad alanlarınıza bir hizmet kimliği rolü atar. Herhangi bir Olay Hub'ları kaynağına kapsamlı bir rol atamak için aynı adımları izleyebilirsiniz. 

1. Azure portalında, Etkinlik Hub'larınız ad alanına gidin ve ad alanı için **Genel Bakış'ı** görüntüleyin. 
1. Olay merkezinin erişim denetim ayarlarını görüntülemek için sol menüde **Erişim Denetimi 'ni (IAM)** seçin.
1.  Rol **atamaları** listesini görmek için Rol atamaları sekmesini seçin.
3.  Yeni bir rol eklemek için **Ekle'yi** seçin.
4.  Rol **atama ekle** sayfasında, atamak istediğiniz Olay Hub'ları rollerini seçin. Ardından, rolü atamak için kaydolduğunuz hizmet kimliğini bulmak için arama yapın.
    
    ![Rol atama sayfası ekleme](./media/authenticate-managed-identity/add-role-assignment-page.png)
5.  **Kaydet'i**seçin. Rolü atadığınız kimlik, bu rolün altında listelenir. Örneğin, aşağıdaki resimde hizmet kimliğinin Olay Hub'ları Veri sahibi olduğu gösterilmektedir.
    
    ![Bir role atanan kimlik](./media/authenticate-managed-identity/role-assigned.png)

Rolü atadıktan sonra, web uygulaması tanımlanan kapsam altında Olay Hub'ları kaynaklarına erişebilir. 

### <a name="test-the-web-application"></a>Web uygulamasını test etme
1. Olay Hub'ları ad alanı ve olay hub'ı oluşturun. 
2. Web uygulamasını Azure'a dağıtın. GitHub'daki web uygulamasına bağlantılar için aşağıdaki sekmeli bölüme bakın. 
3. SendReceive.aspx'In web uygulaması için varsayılan belge olarak ayarlandığından emin olun. 
3. Web uygulaması için **kimliği** etkinleştirin. 
4. Bu kimliği ad alanı düzeyinde veya olay hub düzeyinde **Olay Hub'ları Veri Sahibi** rolüne atayın. 
5. Web uygulamasını çalıştırın, ad alanı adı ve olay merkezi adını, bir iletiyi girin ve **Gönder'i**seçin. Etkinliği almak için **Al'ı**seçin. 

#### <a name="azuremessagingeventhubs-latest"></a>[Azure.Messaging.EventHubs (en son)](#tab/latest)
Artık web uygulamanızı başlatabilir ve tarayıcınızı örnek aspx sayfasına yönlendirebilirsiniz. [GitHub repo'sunda](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Azure.Messaging.EventHubs/ManagedIdentityWebApp)Olay Hub'ları kaynaklarından veri gönderen ve alan örnek web uygulamasını bulabilirsiniz.

[NuGet'in](https://www.nuget.org/packages/Azure.Messaging.EventHubs/)en son paketini yükleyin ve **EventHubProducerClient'ı** kullanarak Olay Hub'larına etkinlik göndermeye başlayın ve **EventHubConsumerClient'ı**kullanarak etkinlik almaya başlayın. 

> [!NOTE]
> Olayları bir etkinlik hub'ında yayımlamak için yönetilen bir kimlik kullanan bir Java [örneği](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs)için bkz.

```csharp
protected async void btnSend_Click(object sender, EventArgs e)
{
    await using (EventHubProducerClient producerClient = new EventHubProducerClient(txtNamespace.Text, txtEventHub.Text, new DefaultAzureCredential()))
    {
        // create a batch
        using (EventDataBatch eventBatch = await producerClient.CreateBatchAsync())
        {

            // add events to the batch. only one in this case. 
            eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes(txtData.Text)));

            // send the batch to the event hub
            await producerClient.SendAsync(eventBatch);
        }

        txtOutput.Text = $"{DateTime.Now} - SENT{Environment.NewLine}{txtOutput.Text}";
    }
}
protected async void btnReceive_Click(object sender, EventArgs e)
{
    await using (var consumerClient = new EventHubConsumerClient(EventHubConsumerClient.DefaultConsumerGroupName, $"{txtNamespace.Text}.servicebus.windows.net", txtEventHub.Text, new DefaultAzureCredential()))
    {
        int eventsRead = 0;
        try
        {
            using CancellationTokenSource cancellationSource = new CancellationTokenSource();
            cancellationSource.CancelAfter(TimeSpan.FromSeconds(5));

            await foreach (PartitionEvent partitionEvent in consumerClient.ReadEventsAsync(cancellationSource.Token))
            {
                txtOutput.Text = $"Event Read: { Encoding.UTF8.GetString(partitionEvent.Data.Body.ToArray()) }{ Environment.NewLine}" + txtOutput.Text;
                eventsRead++;
            }
        }
        catch (TaskCanceledException ex)
        {
            txtOutput.Text = $"Number of events read: {eventsRead}{ Environment.NewLine}" + txtOutput.Text;
        }
    }
}
```

#### <a name="microsoftazureeventhubs-legacy"></a>[Microsoft.Azure.EventHubs (eski)](#tab/old)
Artık web uygulamanızı başlatabilir ve tarayıcınızı örnek aspx sayfasına yönlendirebilirsiniz. [GitHub repo'sunda](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac/ManagedIdentityWebApp)Olay Hub'ları kaynaklarından veri gönderen ve alan örnek web uygulamasını bulabilirsiniz.

[NuGet'ten](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/)en son paketi yükleyin ve aşağıdaki kodda gösterildiği gibi EventHubClient'ı kullanarak Olay hub'larına veri göndermeye ve almaya başlayın: 

```csharp
var ehClient = EventHubClient.CreateWithManagedIdentity(new Uri($"sb://{EventHubNamespace}/"), EventHubName);
```
---

## <a name="event-hubs-for-kafka"></a>Kafka için Event Hubs
Apache Kafka uygulamalarını, Yönetilen kimlik OAuth'u kullanarak Azure Etkinlik Hub'larına ileti göndermek ve almak için kullanabilirsiniz. GitHub'daki aşağıdaki örneğe bakın: [Kafka için Etkinlik Hub'ları - yönetilen kimlik OAuth'u kullanarak mesaj gönderip alın.](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/oauth/java/managedidentity)

## <a name="samples"></a>Örnekler
- **Azure.Messaging.EventHubs** örnekleri
    - [.NET](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Azure.Messaging.EventHubs/ManagedIdentityWebApp)
    - [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs)
- [Microsoft.Azure.EventHubs örnekleri](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac). 
    
    Bu örnekler eski **Microsoft.Azure.EventHubs** kitaplığını kullanır, ancak en son **Azure.Messaging.EventHubs** kitaplığını kullanarak kolayca güncelleştirebilirsiniz. Örneği eski kitaplığı kullanmaktan yenisine taşımak [için Microsoft.Azure.EventHub'larından Azure.Messaging.EventHubs'a geçiş kılavuzuna](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs/migration-guide-from-v4.md)bakın.
    Bu örnek en son **Azure.Messaging.EventHubs** kitaplığını kullanacak şekilde güncelleştirildi.
- [Kafka için Etkinlik Hub'ları - yönetilen kimlik OAuth kullanarak mesaj gönderip alın](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/oauth/java/managedidentity)


## <a name="next-steps"></a>Sonraki adımlar
- Azure kaynakları için yönetilen kimlikler hakkında bilgi edinmek için aşağıdaki makaleye bakın: [Azure kaynakları için yönetilen kimlikler nedir?](../active-directory/managed-identities-azure-resources/overview.md)
- Aşağıdaki ilgili makalelere bakın:
    - [Azure Etkin Dizin'i kullanarak bir uygulamadan Azure Etkinlik Hub'larına yönelik istekleri kimlik doğrulaması](authenticate-application.md)
    - [Paylaşılan Erişim İmzalarını kullanarak Azure Etkinlik Hub'larına istekleri nidoğrulatın](authenticate-shared-access-signature.md)
    - [Azure Active Directory'yi kullanarak Etkinlik Hub kaynaklarına erişimi yetkilendirme](authorize-access-azure-active-directory.md)
    - [Paylaşılan Erişim İmzalarını kullanarak Etkinlik Hubkaynaklarına erişimi yetkilendirme](authorize-access-shared-access-signature.md)