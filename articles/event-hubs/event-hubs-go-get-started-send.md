---
title: "Hızlı başlangıç: Go - Azure Etkinlik Hub'larını kullanarak etkinlik gönderme ve alma"
description: "Hızlı başlangıç: Bu makalede, Azure Olay Hub'larından olay gönderen bir Go uygulaması oluşturmak için bir iz yol sağlar."
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.custom: seodec18
ms.date: 11/05/2019
ms.author: shvija
ms.openlocfilehash: e5f52d0ddbf9a66d974732d6d98ca8a5b09cc2d0
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73720578"
---
# <a name="quickstart-send-events-to-or-receive-events-from-event-hubs-using-go"></a>Hızlı başlangıç: Go'u kullanarak Olay Hub'larına etkinlik gönderme veya olay hub'larından etkinlik alma
Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md).

Bu öğretici, etkinlik hub'ına olay göndermek veya olay hub'ından almak için Go uygulamalarını nasıl yazacağımı açıklar. 

> [!NOTE]
> Bu hızlı başlangıcı [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/eventhubs)’dan örnek olarak indirebilir, `EventHubConnectionString` ve `EventHubName` dizelerini olay hub’ınızdaki değerlerle değiştirebilir ve çalıştırabilirsiniz. Alternatif olarak bu öğreticideki adımları izleyerek kendi çözümünüzü de oluşturabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

- Yerel olarak yüklenmiş gidin. Gerekirse [bu yönergeleri](https://golang.org/doc/install) izleyin.
- Etkin bir Azure hesabı. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz][] bir hesap oluşturun.
- **Olay Hub'ları ad alanı ve olay hub'ı oluşturun.** Etkinlik Hub'larının ad alanını oluşturmak ve uygulamanızın etkinlik merkeziyle iletişim kurmak için ihtiyaç duyduğu yönetim kimlik bilgilerini elde etmek için [Azure portalını](https://portal.azure.com) kullanın. Ad alanı ve olay hub'ı oluşturmak için [bu makaledeki](event-hubs-create.md)yordamı izleyin.

## <a name="send-events"></a>Olayları gönderme
Bu bölümde, olayları bir olay merkezine göndermek için nasıl bir Git uygulaması oluşturulabileceğiniz gösterilmektedir. 

### <a name="install-go-package"></a>Go paketini yükle

Olay Hub'ları için Go `go get` `dep`paketini alın veya . Örnek:

```bash
go get -u github.com/Azure/azure-event-hubs-go
go get -u github.com/Azure/azure-amqp-common-go/...

# or

dep ensure -add github.com/Azure/azure-event-hubs-go
dep ensure -add github.com/Azure/azure-amqp-common-go
```

### <a name="import-packages-in-your-code-file"></a>Kod dosyanızdaki paketleri içe aktarma

Go paketlerini almak için aşağıdaki kod örneğini kullanın:

```go
import (
    aad "github.com/Azure/azure-amqp-common-go/aad"
    eventhubs "github.com/Azure/azure-event-hubs-go"
)
```

### <a name="create-service-principal"></a>Hizmet sorumlusu oluşturma

[Azure CLI 2.0 ile Bir Azure hizmet ilkesi oluştur'daki](/cli/azure/create-an-azure-service-principal-azure-cli)yönergeleri izleyerek yeni bir hizmet ilkesi oluşturun. Sağlanan kimlik bilgilerini ortamınızda aşağıdaki adlarla kaydedin. Hem Go için Azure SDK hem de Etkinlik Hub'ları paketleri bu değişken adlarını aramak için önceden yapılandırılmıştır:

```bash
export AZURE_CLIENT_ID=
export AZURE_CLIENT_SECRET=
export AZURE_TENANT_ID=
export AZURE_SUBSCRIPTION_ID= 
```

Şimdi, Olay Hub'larınız için bu kimlik bilgilerini kullanan bir yetkilendirme sağlayıcısı oluşturun:

```go
tokenProvider, err := aad.NewJWTProvider(aad.JWTProviderWithEnvironmentVars())
if err != nil {
    log.Fatalf("failed to configure AAD JWT provider: %s\n", err)
}
```

### <a name="create-event-hubs-client"></a>Olay Hub'ları istemcisi oluşturma

Aşağıdaki kod bir Olay Hub'ları istemcisi oluşturur:

```go
hub, err := eventhubs.NewHub("namespaceName", "hubName", tokenProvider)
ctx := context.WithTimeout(context.Background(), 10 * time.Second)
defer hub.Close(ctx)
if err != nil {
    log.Fatalf("failed to get hub %s\n", err)
}
```

### <a name="write-code-to-send-messages"></a>İleti göndermek için kod yazma

Aşağıdaki snippet'te ( 1) bir terminalden etkileşimli olarak ileti göndermek veya (2) programınızda ileti göndermek için kullanın:

```go
// 1. send messages at the terminal
ctx = context.Background()
reader := bufio.NewReader(os.Stdin)
for {
    fmt.Printf("Input a message to send: ")
    text, _ := reader.ReadString('\n')
    hub.Send(ctx, eventhubs.NewEventFromString(text))
}

// 2. send messages within program
ctx = context.Background()
hub.Send(ctx, eventhubs.NewEventFromString("hello Azure!"))
```

### <a name="extras"></a>Ek Özellikler

Etkinlik merkezinizdeki bölümlerin kÜnatlarını alın:

```go
info, err := hub.GetRuntimeInformation(ctx)
if err != nil {
    log.Fatalf("failed to get runtime info: %s\n", err)
}
log.Printf("got partition IDs: %s\n", info.PartitionIDs)
```

Olayları olay merkezine göndermek için uygulamayı çalıştırın. 

Tebrikler! Bir olay hub'ına ileti gönderdiniz.

## <a name="receive-events"></a>Olayları alma

### <a name="create-a-storage-account-and-container"></a>Depolama hesabı ve kapsayıcı oluşturma

Olay akışındaki bölümlerdeki kiralamalar ve denetim noktaları gibi durum, bir Azure Depolama kapsayıcısı kullanılarak alıcılar arasında paylaşılır. Go SDK ile bir depolama hesabı ve kapsayıcı oluşturabilirsiniz, ancak Azure [depolama hesapları hakkındaki](../storage/common/storage-create-storage-account.md)yönergeleri izleyerek bir depolama hesabı da oluşturabilirsiniz.

Go SDK ile Depolama yapıları oluşturmak için örnekler [Go örnekleri repo](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage) ve bu öğretici karşılık gelen örnek mevcuttur.

### <a name="go-packages"></a>Paketleri git

İletileri almak için Olay Hub'ları için `go get` Go `dep`paketlerini şu veya:

```bash
go get -u github.com/Azure/azure-event-hubs-go/...
go get -u github.com/Azure/azure-amqp-common-go/...
go get -u github.com/Azure/go-autorest/...

# or

dep ensure -add github.com/Azure/azure-event-hubs-go
dep ensure -add github.com/Azure/azure-amqp-common-go
dep ensure -add github.com/Azure/go-autorest
```

### <a name="import-packages-in-your-code-file"></a>Kod dosyanızdaki paketleri içe aktarma

Go paketlerini almak için aşağıdaki kod örneğini kullanın:

```go
import (
    aad "github.com/Azure/azure-amqp-common-go/aad"
    eventhubs "github.com/Azure/azure-event-hubs-go"
    eph "github.com/Azure/azure-event-hubs-go/eph"
    storageLeaser "github.com/Azure/azure-event-hubs-go/storage"
    azure "github.com/Azure/go-autorest/autorest/azure"
)
```

### <a name="create-service-principal"></a>Hizmet sorumlusu oluşturma

[Azure CLI 2.0 ile Bir Azure hizmet ilkesi oluştur'daki](/cli/azure/create-an-azure-service-principal-azure-cli)yönergeleri izleyerek yeni bir hizmet ilkesi oluşturun. Sağlanan kimlik bilgilerini ortamınızda aşağıdaki adlarla kaydedin: Hem Go için Azure SDK hem de Olay Hub'ları paketi bu değişken adlarını aramak için önceden yapılandırılmıştır.

```bash
export AZURE_CLIENT_ID=
export AZURE_CLIENT_SECRET=
export AZURE_TENANT_ID=
export AZURE_SUBSCRIPTION_ID= 
```

Ardından, Olay Hub'larınız için bu kimlik bilgilerini kullanan bir yetkilendirme sağlayıcısı oluşturun:

```go
tokenProvider, err := aad.NewJWTProvider(aad.JWTProviderWithEnvironmentVars())
if err != nil {
    log.Fatalf("failed to configure AAD JWT provider: %s\n", err)
}
```

### <a name="get-metadata-struct"></a>Meta veri struct alın

Azure Go SDK'sını kullanarak Azure ortamınızla ilgili meta verileriçeren bir yapı alın. Daha sonraki işlemler, doğru uç noktaları bulmak için bu yapıyı kullanır.

```go
azureEnv, err := azure.EnvironmentFromName("AzurePublicCloud")
if err != nil {
    log.Fatalf("could not get azure.Environment struct: %s\n", err)
}
```

### <a name="create-credential-helper"></a>Kimlik bilgisi yardımcısı oluşturma 

Depolama için Paylaşılan Erişim İmzası (SAS) kimlik bilgilerini oluşturmak için önceki Azure Etkin Dizin (AAD) kimlik bilgilerini kullanan bir kimlik bilgisi yardımcısı oluşturun. Son parametre bu oluşturucuya daha önce kullanılan aynı ortam değişkenlerini kullanmasını söyler:

```go
cred, err := storageLeaser.NewAADSASCredential(
    subscriptionID,
    resourceGroupName,
    storageAccountName,
    storageContainerName,
    storageLeaser.AADSASCredentialWithEnvironmentVars())
if err != nil {
    log.Fatalf("could not prepare a storage credential: %s\n", err)
}
```

### <a name="create-a-check-pointer-and-a-leaser"></a>Çek işaretçisi ve kiralayan oluşturma 

Diğer alıcıların doğru ofsetten okumaya başlayabilmesi için ileti akışı için denetim noktaları yazmaktan sorumlu bir **kiralayan,** belirli bir alıcıya bir bölüm kiralamaktan sorumlu **bir**kiralayıcı oluşturun.

Şu anda, hem kiralamaları hem de denetim noktalarını yönetmek için aynı Depolama kapsayıcısını kullanan tek bir **StorageLeaserCheckpointer** kullanılabilir. Depolama hesabı ve kapsayıcı adlarına ek olarak, **StorageLeaserCheckpointer'ın** kapsayıcıya doğru şekilde erişebilmesi için önceki adımda oluşturulan kimlik bilgisine ve Azure ortamına ihtiyacı vardır.

```go
leaserCheckpointer, err := storageLeaser.NewStorageLeaserCheckpointer(
    cred,
    storageAccountName,
    storageContainerName,
    azureEnv)
if err != nil {
    log.Fatalf("could not prepare a storage leaserCheckpointer: %s\n", err)
}
```

### <a name="construct-event-processor-host"></a>Olay İşlemcisi Ana Bilgisayar Oluştur

Şimdi aşağıdaki gibi, bir EventProcessorHost oluşturmak için gerekli parçalar var. Aynı **StorageLeaserCheckpointer,** daha önce açıklandığı gibi hem kiralayan hem de onay işaretçisi olarak kullanılır:

```go
ctx := context.Background()
p, err := eph.New(
    ctx,
    nsName,
    hubName,
    tokenProvider,
    leaserCheckpointer,
    leaserCheckpointer)
if err != nil {
    log.Fatalf("failed to create EPH: %s\n", err)
}
defer p.Close(context.Background())
```

### <a name="create-handler"></a>İşleyici oluşturma 

Şimdi bir işleyici oluşturun ve Olay İşlemci Ana Bilgisayar'a kaydedin. Ana bilgisayar başlatıldığında, bunu ve diğer belirtilen işleyicileri gelen iletilere uygular:

```go
handler := func(ctx context.Context, event *eventhubs.Event) error {
    fmt.Printf("received: %s\n", string(event.Data))
    return nil
}

// register the handler with the EPH
_, err := p.RegisterHandler(ctx, handler)
if err != nil {
    log.Fatalf("failed to register handler: %s\n", err)
}
```

### <a name="write-code-to-receive-messages"></a>İleti almak için kod yazma

Her şey ayarlandığınızda, Olay İşlemcisi `Start(context)` Ana Bilgisayar'ı kalıcı olarak `StartNonBlocking(context)` çalışmasını sağlamak için veya yalnızca iletiler kullanılabilir olduğu sürece çalıştırmak için başlatabilirsiniz.

Bu öğretici aşağıdaki gibi başlar ve çalışır; kullanarak `StartNonBlocking`bir örnek için GitHub örneğine bakın:

```go
ctx := context.Background()
err = p.Start()
if err != nil {
    log.Fatalf("failed to start EPH: %s\n", err)
}
```

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleleri okuyun:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Azure Event Hubs'ın özellikleri ve terminolojisi](event-hubs-features.md)
- [Event Hubs ile ilgili SSS](event-hubs-faq.md)


<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[ücretsiz bir hesap]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
