---
title: Azure Active Directory ile yönetilen bir kimlik doğrulama
description: Bu makalede, Azure Event Hubs kaynaklarına erişmek için Azure Active Directory bir yönetilen kimliğin kimlik doğrulaması hakkında bilgi sağlanır
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: 4e9bfb9c4649732ac5afc80dfb0a522ff9f333be
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85318105"
---
# <a name="authenticate-a-managed-identity-with-azure-active-directory-to-access-event-hubs-resources"></a>Event Hubs kaynaklara erişmek için Azure Active Directory ile yönetilen bir kimliğin kimliğini doğrulama
Azure Event Hubs, [Azure kaynakları için yönetilen kimliklerle](../active-directory/managed-identities-azure-resources/overview.md)Azure Active Directory (Azure AD) kimlik doğrulamasını destekler. Azure kaynakları için Yönetilen kimlikler, Azure sanal makinelerinde (VM), Işlev uygulamalarında, sanal makine ölçek kümelerinde ve diğer hizmetlerde çalışan uygulamalardan Azure AD kimlik bilgilerini kullanarak Event Hubs kaynaklarına erişim yetkisi verebilir. Azure AD kimlik doğrulamasıyla birlikte Azure kaynakları için Yönetilen kimlikler kullanarak, kimlik bilgilerini bulutta çalışan uygulamalarınızla depolamaktan kaçınabilirsiniz.

Bu makalede bir Azure VM 'den yönetilen kimlik kullanarak bir olay hub 'ına erişim yetkisi verme işlemleri gösterilmektedir.

## <a name="enable-managed-identities-on-a-vm"></a>VM 'de yönetilen kimlikleri etkinleştirme
VM 'nizden Event Hubs kaynaklarını yetkilendirmek üzere Azure kaynakları için Yönetilen kimlikler kullanabilmeniz için önce VM 'de Azure kaynakları için yönetilen kimlikleri etkinleştirmeniz gerekir. Azure kaynakları için yönetilen kimliklerin nasıl etkinleştireceğinizi öğrenmek için şu makalelerden birine bakın:

- [Azure portalındaki](../active-directory/managed-service-identity/qs-configure-portal-windows-vm.md)
- [Azure PowerShell](../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure CLI](../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager şablonu](../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure Resource Manager istemci kitaplıkları](../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

## <a name="grant-permissions-to-a-managed-identity-in-azure-ad"></a>Azure AD 'de yönetilen bir kimliğe izin verme
Uygulamanızdaki yönetilen bir kimlikle Event Hubs hizmetine bir istek yetkilendirmek için, önce bu yönetilen kimlik için rol tabanlı erişim denetimi (RBAC) ayarlarını yapılandırın. Azure Event Hubs Event Hubs gönderme ve okuma izinlerini çevreleyen RBAC rollerini tanımlar. RBAC rolü yönetilen bir kimliğe atandığında, yönetilen kimliğe uygun kapsamda Event Hubs verilerine erişim verilir.

RBAC rolleri atama hakkında daha fazla bilgi için bkz. [Event Hubs kaynaklara erişim için Azure Active Directory kimlik doğrulaması](authorize-access-azure-active-directory.md).

## <a name="use-event-hubs-with-managed-identities"></a>Event Hubs'ı yönetilen kimliklerle kullanma
Yönetilen kimliklerle Event Hubs kullanmak için, rol ve uygun kapsam kimliğini atamanız gerekir. Bu bölümdeki yordam, yönetilen bir kimlik altında çalışan ve Event Hubs kaynaklara erişen basit bir uygulama kullanır.

Burada [Azure App Service](https://azure.microsoft.com/services/app-service/)' de barındırılan örnek bir Web uygulaması kullanıyoruz. Web uygulaması oluşturmaya yönelik adım adım yönergeler için bkz. [Azure 'da ASP.NET Core Web uygulaması oluşturma](../app-service/app-service-web-get-started-dotnet.md)

Uygulama oluşturulduktan sonra aşağıdaki adımları izleyin: 

1. **Ayarlar** ' a gidin ve **kimlik**' i seçin. 
1. Görüntülenecek **durumu** **seçin.** 
1. Ayarları kaydetmek için **Kaydet**’i seçin. 

    ![Bir Web uygulaması için yönetilen kimlik](./media/authenticate-managed-identity/identity-web-app.png)

Bu ayarı etkinleştirdikten sonra, Azure Active Directory (Azure AD) ve App Service ana bilgisayarında yapılandırılmış yeni bir hizmet kimliği oluşturulur.

Şimdi, bu hizmet kimliğini Event Hubs kaynaklarınızın gerekli kapsamındaki bir role atayın.

### <a name="to-assign-rbac-roles-using-the-azure-portal"></a>Azure portal kullanarak RBAC rolleri atamak için
Event Hubs kaynaklara bir rol atamak için Azure portal bu kaynağa gidin. Kaynak için Access Control (ıAM) ayarlarını görüntüleyin ve rol atamalarını yönetmek için aşağıdaki yönergeleri izleyin:

> [!NOTE]
> Aşağıdaki adımlar Event Hubs ad alanlarınıza bir hizmet kimliği rolü atar. Herhangi bir Event Hubs kaynağına kapsamlı bir rol atamak için aynı adımları izleyebilirsiniz. 

1. Azure portal, Event Hubs ad alanına gidin ve ad alanı için **genel bakış** ' ı görüntüleyin. 
1. Olay Hub 'ının erişim denetimi ayarlarını göstermek için sol taraftaki menüden **Access Control (IAM)** seçeneğini belirleyin.
1.  Rol atamalarının listesini görmek için **rol atamaları** sekmesini seçin.
3.  Yeni bir rol eklemek için **Ekle** ' yi seçin.
4.  **Rol ataması Ekle** sayfasında, atamak istediğiniz Event Hubs rolleri seçin. Ardından, rolü atamak için kaydettiğiniz hizmet kimliğini bulmak için arama yapın.
    
    ![Rol atama sayfası ekle](./media/authenticate-managed-identity/add-role-assignment-page.png)
5.  **Kaydet**'i seçin. Rolü atadığınız kimlik söz konusu rol altında listelenir. Örneğin, aşağıdaki görüntüde, hizmet kimliğinin Event Hubs veri sahibi olduğunu gösterilmektedir.
    
    ![Bir role atanan kimlik](./media/authenticate-managed-identity/role-assigned.png)

Rolü atadıktan sonra, Web uygulamasının tanımlı kapsamda Event Hubs kaynaklara erişimi olur. 

### <a name="test-the-web-application"></a>Web uygulamasını test etme
1. Event Hubs bir ad alanı ve bir olay hub 'ı oluşturun. 
2. Web uygulamasını Azure 'a dağıtın. GitHub 'da Web uygulaması bağlantıları için aşağıdaki sekmeli bölüme bakın. 
3. SendReceive. aspx ' in Web uygulaması için varsayılan belge olarak ayarlandığından emin olun. 
3. Web uygulaması için **kimliği** etkinleştirin. 
4. Bu kimliği ad alanı düzeyinde veya Olay Hub 'ı düzeyinde **Event Hubs veri sahibi** rolüne atayın. 
5. Web uygulamasını çalıştırın, ad alanı adını ve Olay Hub 'ı adını, bir iletiyi girin ve **Gönder**' i seçin. Olayı almak için **Al**' ı seçin. 

#### <a name="azuremessagingeventhubs-latest"></a>[Azure. Messaging. EventHubs (en son)](#tab/latest)
Artık Web uygulamasını başlatabilir ve tarayıcınıza örnek aspx sayfasına işaret edebilirsiniz. [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Azure.Messaging.EventHubs/ManagedIdentityWebApp)deposunda Event Hubs kaynaklarından veri gönderen ve alan örnek Web uygulamasını bulabilirsiniz.

[NuGet](https://www.nuget.org/packages/Azure.Messaging.EventHubs/)'ten en son paketi yükler ve **EventHubProducerClient** kullanarak ve **eventhubconsumerclient**kullanarak olayları alarak Event Hubs olay göndermeye başlayın. 

> [!NOTE]
> Olayları bir olay hub 'ına yayımlamak için yönetilen kimlik kullanan bir Java örneği için bkz. [GitHub 'Da Azure kimlik örneği ile olayları yayımlama](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs).

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
Artık Web uygulamasını başlatabilir ve tarayıcınıza örnek aspx sayfasına işaret edebilirsiniz. [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac/ManagedIdentityWebApp)deposunda Event Hubs kaynaklarından veri gönderen ve alan örnek Web uygulamasını bulabilirsiniz.

[NuGet](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/)'den en son paketi yükler ve aşağıdaki kodda gösterildiği gibi EventHubClient kullanarak Olay Hub 'larına veri göndermeye ve bu verileri almaya başlayın: 

```csharp
var ehClient = EventHubClient.CreateWithManagedIdentity(new Uri($"sb://{EventHubNamespace}/"), EventHubName);
```
---

## <a name="event-hubs-for-kafka"></a>Kafka için Event Hubs
Yönetilen kimlik OAuth kullanarak Azure Event Hubs ileti göndermek ve iletileri almak için Apache Kafka uygulamalarını kullanabilirsiniz. GitHub 'da aşağıdaki örneğe bakın: [Kafka için Event Hubs-yönetilen kimlik OAuth kullanarak ileti gönderme ve alma](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/oauth/java/managedidentity).

## <a name="samples"></a>Örnekler
- **Azure. Messaging. EventHubs** örnekleri
    - [.NET](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Azure.Messaging.EventHubs/ManagedIdentityWebApp)
    - [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs)
- [Microsoft. Azure. EventHubs örnekleri](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac). 
    
    Bu örnekler eski **Microsoft. Azure. EventHubs** kitaplığını kullanır, ancak en son **Azure. Messaging. eventhubs** kitaplığını kullanarak kolayca güncelleştirebilirsiniz. Eski kitaplığı kullanarak örneği yeni bir tane ile taşımak için [Microsoft. Azure. eventhubs 'Den Azure. Messaging. eventhubs 'ye geçiş kılavuzu](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs/MigrationGuide.md)'na bakın.
    Bu örnek, en son **Azure. Messaging. EventHubs** kitaplığını kullanacak şekilde güncelleştirilmiştir.
- [Kafka için Event Hubs-yönetilen kimlik OAuth kullanarak ileti gönderme ve alma](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/oauth/java/managedidentity)


## <a name="next-steps"></a>Sonraki adımlar
- Azure kaynakları için Yönetilen kimlikler hakkında bilgi edinmek için şu makaleye bakın: [Azure kaynakları için Yönetilen kimlikler nedir?](../active-directory/managed-identities-azure-resources/overview.md)
- Aşağıdaki ilgili makalelere bakın:
    - [Azure Active Directory kullanarak bir uygulamadan Azure Event Hubs istek kimliklerini doğrulama](authenticate-application.md)
    - [Paylaşılan erişim Imzalarını kullanarak Azure Event Hubs istek kimliklerini doğrulama](authenticate-shared-access-signature.md)
    - [Azure Active Directory kullanarak Event Hubs kaynaklarına erişim yetkisi verme](authorize-access-azure-active-directory.md)
    - [Paylaşılan erişim Imzalarını kullanarak Event Hubs kaynaklarına erişim yetkisi verme](authorize-access-shared-access-signature.md)