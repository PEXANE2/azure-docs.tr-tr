---
title: Azure Röle .NET Standart API'lere Genel Bakış | Microsoft Dokümanlar
description: Bu makalede, Azure Röle Karma Bağlantıları .NET Standart API'nin bazı önemli genel bakışı özetlenmiştir.
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: b1da9ac1-811b-4df7-a22c-ccd013405c40
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: spelluru
ms.openlocfilehash: 18eaf2d2daae817107be6cdb0da9359bb5f9b4e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76514544"
---
# <a name="azure-relay-hybrid-connections-net-standard-api-overview"></a>Azure Röle Karma Bağlantılar .NET Standart API'ye genel bakış

Bu makalede, bazı önemli Azure Röle Karma Bağlantılar .NET Standart [istemci API'leri](/dotnet/api/microsoft.azure.relay)özetlenir.
  
## <a name="relay-connection-string-builder-class"></a>Röle Bağlantı String Builder sınıfı

[RelayConnectionStringBuilder][RelayConnectionStringBuilder] sınıfı, Röle Hibrit Bağlantılara özgü bağlantı dizelerini biçimlendirin. Bağlantı dizesinin biçimini doğrulamak veya sıfırdan bir bağlantı dizesi oluşturmak için kullanabilirsiniz. Bir örnek için aşağıdaki koda bakın:

```csharp
var endpoint = "[Relay namespace]";
var entityPath = "[Name of the Hybrid Connection]";
var sharedAccessKeyName = "[SAS key name]";
var sharedAccessKey = "[SAS key value]";

var connectionStringBuilder = new RelayConnectionStringBuilder()
{
    Endpoint = endpoint,
    EntityPath = entityPath,
    SharedAccessKeyName = sasKeyName,
    SharedAccessKey = sasKeyValue
};
```

Ayrıca, doğrudan `RelayConnectionStringBuilder` yönteme bir bağlantı dizesi geçirebilirsiniz. Bu işlem, bağlantı dizesinin geçerli bir biçimde olduğunu doğrulamanızı sağlar. Parametrelerden herhangi biri geçersizse, oluşturucu `ArgumentException`bir .

```csharp
var myConnectionString = "[RelayConnectionString]";
// Declare the connectionStringBuilder so that it can be used outside of the loop if needed
RelayConnectionStringBuilder connectionStringBuilder;
try
{
    // Create the connectionStringBuilder using the supplied connection string
    connectionStringBuilder = new RelayConnectionStringBuilder(myConnectionString);
}
catch (ArgumentException ae)
{
    // Perform some error handling
}
```

## <a name="hybrid-connection-stream"></a>Karma bağlantı akışı

HybridConnectionStream sınıfı, ister [Bir HybridConnectionClient][HCClient]ile çalışıyor olun ister Bir [HybridConnectionStream][HCStream] [HybridConnectionListener][HCListener]ile çalışıyor olun, bir Azure Röle bitiş noktasından veri göndermek ve almak için kullanılan birincil nesnedir.

### <a name="getting-a-hybrid-connection-stream"></a>Karma bağlantı akışı alma

#### <a name="listener"></a>Dinleyici

[HybridConnectionListener][HCListener] nesnesini kullanarak aşağıdaki `HybridConnectionStream` gibi bir nesne elde edebilirsiniz:

```csharp
// Use the RelayConnectionStringBuilder to get a valid connection string
var listener = new HybridConnectionListener(csb.ToString());
// Open a connection to the Relay endpoint
await listener.OpenAsync();
// Get a `HybridConnectionStream`
var hybridConnectionStream = await listener.AcceptConnectionAsync();
```

#### <a name="client"></a>İstemci

[HybridConnectionClient][HCClient] nesnesi kullanarak aşağıdaki `HybridConnectionStream` gibi bir nesne elde edebilirsiniz:

```csharp
// Use the RelayConnectionStringBuilder to get a valid connection string
var client = new HybridConnectionClient(csb.ToString());
// Open a connection to the Relay endpoint and get a `HybridConnectionStream`
var hybridConnectionStream = await client.CreateConnectionAsync();
```

### <a name="receiving-data"></a>Veri alma

[HybridConnectionStream][HCStream] sınıfı iki yönlü iletişim sağlar. Çoğu durumda, sürekli olarak akıştan alırsınız. Akıştan metin okuyorsanız, verilerin daha kolay ayrışmasını sağlayan bir [StreamReader](https://msdn.microsoft.com/library/system.io.streamreader(v=vs.110).aspx) nesnesi de kullanmak isteyebilirsiniz. Örneğin, verileri metin olarak değil, ', ', ') olarak `byte[]`okuyabilirsiniz.

Aşağıdaki kod, iptal talep edilene kadar akıştan tek tek metin satırlarını okur:

```csharp
// Create a CancellationToken, so that we can cancel the while loop
var cancellationToken = new CancellationToken();
// Create a StreamReader from the 'hybridConnectionStream`
var streamReader = new StreamReader(hybridConnectionStream);

while (!cancellationToken.IsCancellationRequested)
{
    // Read a line of input until a newline is encountered
    var line = await streamReader.ReadLineAsync();
    if (string.IsNullOrEmpty(line))
    {
        // If there's no input data, we will signal that 
        // we will no longer send data on this connection
        // and then break out of the processing loop.
        await hybridConnectionStream.ShutdownAsync(cancellationToken);
        break;
    }
}
```

### <a name="sending-data"></a>Veri gönderme

Bir bağlantı kurduktan sonra, Röle bitiş noktasına bir ileti gönderebilirsiniz. Bağlantı nesnesi [Akış'ı](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx)devraldığından, verilerinizi `byte[]`'' olarak gönder. Aşağıdaki örnekte, bunun nasıl yapılacağını gösterilmektedir:

```csharp
var data = Encoding.UTF8.GetBytes("hello");
await clientConnection.WriteAsync(data, 0, data.Length);
```

Ancak, her seferinde dizekodlamak gerek kalmadan doğrudan metin göndermek istiyorsanız, `hybridConnectionStream` bir [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) nesnesi ile nesne sarabilirsiniz.

```csharp
// The StreamWriter object only needs to be created once
var textWriter = new StreamWriter(hybridConnectionStream);
await textWriter.WriteLineAsync("hello");
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Röle hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları ziyaret edin:

* [Microsoft.Azure.Relay başvurusu](/dotnet/api/microsoft.azure.relay)
* [Azure Geçiş nedir?](relay-what-is-it.md)
* [Kullanılabilir Röle API'leri](relay-api-overview.md)

[RelayConnectionStringBuilder]: /dotnet/api/microsoft.azure.relay.relayconnectionstringbuilder
[HCStream]: /dotnet/api/microsoft.azure.relay.hybridconnectionstream
[HCClient]: /dotnet/api/microsoft.azure.relay.hybridconnectionclient
[HCListener]: /dotnet/api/microsoft.azure.relay.hybridconnectionlistener