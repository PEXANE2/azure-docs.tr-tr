---
title: Bağlantı dizelerini yapılandırma
titleSuffix: Azure Storage
description: Bir Azure depolama hesabı için bağlantı dizesini yapılandırın. Bağlantı dizesi, Paylaşılan Anahtar yetkilendirmesini kullanarak çalışma zamanında uygulamanızdan bir depolama hesabına erişimi yetkilendirmek için gereken bilgileri içerir.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 12/20/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: f617beec8a53570ede7755040cfbb92a7d1712b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79268359"
---
# <a name="configure-azure-storage-connection-strings"></a>Azure Storage bağlantı dizelerini yapılandırma

Bağlantı dizesi, Paylaşılan Anahtar yetkilendirmesini kullanarak çalışma zamanında bir Azure Depolama hesabındaki verilere erişmek için uygulamanız için gereken yetkilendirme bilgilerini içerir. Bağlantı dizelerini şu şekilde yapılandırabilirsiniz:

* Azure depolama emülatörüne bağlanın.
* Azure'da bir depolama hesabına erişin.
* Azure'da belirtilen kaynaklara paylaşılan erişim imzası (SAS) aracılığıyla erişin.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

## <a name="view-and-copy-a-connection-string"></a>Bağlantı dizelerini görüntüleme ve kopyalama

[!INCLUDE [storage-view-keys-include](../../../includes/storage-view-keys-include.md)]

## <a name="store-a-connection-string"></a>Bağlantı dizelerini depolama

Azure Depolama'ya yapılan istekleri yetkilendirmek için uygulamanızın çalışma zamanında bağlantı dizesine erişmesi gerekir. Bağlantı dizenizi depolamak için birkaç seçeneğiniz var:

* Bağlantı dizenizi bir ortam değişkeninde depolayabilirsiniz.
* Masaüstünde veya aygıtta çalışan bir uygulama bağlantı dizesini bir **app.config** veya **web.config** dosyasında saklayabilir. Bu dosyalardaki **AppSettings** bölümüne bağlantı dizesini ekleyin.
* Azure bulut hizmetinde çalışan bir uygulama bağlantı dizesini [Azure hizmet yapılandırma şemasında (.cscfg)](https://msdn.microsoft.com/library/ee758710.aspx)depolayabilir. Bağlantı dizesini hizmet yapılandırma dosyasının **ConfigurationSettings** bölümüne ekleyin.

Bağlantı dizenizi bir yapılandırma dosyasında depolamak, buluttaki depolama emülatörü ile Azure depolama hesabı arasında geçiş yapmak için bağlantı dizesini güncelleştirmeyi kolaylaştırır. Hedef ortamınızı işaret etmek için bağlantı dizesini düzenlemesi gerekir.

Uygulamanızın nerede çalıştığına bakılmaksızın, bağlantı dizenize çalışma zamanında erişmek için [Microsoft Azure Configuration Manager'ı](https://www.nuget.org/packages/Microsoft.Azure.ConfigurationManager/) kullanabilirsiniz.

## <a name="configure-a-connection-string-for-the-storage-emulator"></a>Depolama emülatörü için bağlantı dizesini yapılandırma

[!INCLUDE [storage-emulator-connection-string-include](../../../includes/storage-emulator-connection-string-include.md)]

Depolama emülatörü hakkında daha fazla bilgi için [bkz.](storage-use-emulator.md)

## <a name="configure-a-connection-string-for-an-azure-storage-account"></a>Azure depolama hesabı için bağlantı dizesini yapılandırma

Azure depolama hesabınız için bir bağlantı dizesi oluşturmak için aşağıdaki biçimi kullanın. HTTPS (önerilen) veya HTTP aracılığıyla depolama hesabına bağlanmak `myAccountName` isteyip istemediğinizi, depolama `myAccountKey` hesabınızın adı ile değiştirin ve hesap erişim anahtarınızla değiştirin:

`DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey`

Örneğin, bağlantı dizeniz şuna benzer görünebilir:

`DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>`

Azure Depolama bir bağlantı dizesinde hem HTTP'yi hem de HTTPS'yi desteklese de, *HTTPS önerilir.*

> [!TIP]
> [Azure portalında](https://portal.azure.com)depolama hesabınızın bağlantı dizelerini bulabilirsiniz. Hem birincil hem de ikincil erişim anahtarları için bağlantı dizelerini görmek için depolama hesabınızın menü**bıçaklarında** **AYARLAR'a** > gidin.
>

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>Paylaşılan erişim imzası kullanarak bağlantı dizesi oluşturma

[!INCLUDE [storage-use-sas-in-connection-string-include](../../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="create-a-connection-string-for-an-explicit-storage-endpoint"></a>Açık depolama bitiş noktası için bağlantı dizesi oluşturma

Varsayılan uç noktaları kullanmak yerine bağlantı dizenizde açık hizmet uç noktaları belirtebilirsiniz. Açık bir bitiş noktası belirten bir bağlantı dizesi oluşturmak için, protokol belirtimi (HTTPS (önerilen) veya HTTP dahil olmak üzere her hizmet için tam hizmet bitiş noktasını aşağıdaki biçimde belirtin:

```
DefaultEndpointsProtocol=[http|https];
BlobEndpoint=myBlobEndpoint;
FileEndpoint=myFileEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
AccountName=myAccountName;
AccountKey=myAccountKey
```

Açık bir bitiş noktası belirtmek isteyebileceğiniz senaryolardan biri, Blob depolama bitiş noktanızı özel bir [etki alanına](../blobs/storage-custom-domain-name.md)eşlediğinizde n dir. Bu durumda, bağlantı dizenizde Blob depolama için özel bitiş noktanızı belirtebilirsiniz. Uygulamanız bunları kullanıyorsa, diğer hizmetler için varsayılan uç noktaları isteğe bağlı olarak belirtebilirsiniz.

Blob hizmeti için açık bir bitiş noktası belirten bir bağlantı dizesi örneği aşağıda verilmiştir:

```
# Blob endpoint only
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
AccountName=storagesample;
AccountKey=<account-key>
```

Bu örnek, Blob hizmeti için özel bir etki alanı da dahil olmak üzere tüm hizmetler için açık uç noktaları belirtir:

```
# All service endpoints
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
FileEndpoint=https://myaccount.file.core.windows.net;
QueueEndpoint=https://myaccount.queue.core.windows.net;
TableEndpoint=https://myaccount.table.core.windows.net;
AccountName=storagesample;
AccountKey=<account-key>
```

Bağlantı dizesindeki uç nokta değerleri, depolama hizmetlerine istek URI'lerini oluşturmak ve kodunuza döndürülen Herhangi Bir URI'nin biçimini dikte etmek için kullanılır.

Bir depolama bitiş noktasını özel bir etki alanına eşlediyseniz ve bağlantı dizesinden bu bitiş noktasını atladıysanız, bu bağlantı dizesini kodunuzdan bu hizmetteki verilere erişmek için kullanamazsınız.

> [!IMPORTANT]
> Bağlantı dizelerinizdeki servis uç noktası değerleri, `https://` (önerilen) veya `http://`. Azure Depolama henüz özel etki alanları için HTTPS'yi desteklemediği için, özel bir etki alanına işaret eden herhangi bir bitiş noktası URI için belirtmeniz `http://` *gerekir.*
>

### <a name="create-a-connection-string-with-an-endpoint-suffix"></a>Bitiş noktası sonekile bağlantı dizesi oluşturma

Azure China 21Vianet veya Azure Kamu gibi farklı uç nokta soneklerine sahip bölgelerdeki veya örneklerdeki bir depolama hizmeti için bağlantı dizesi oluşturmak için aşağıdaki bağlantı dize biçimini kullanın. HTTPS (önerilen) veya HTTP aracılığıyla depolama hesabına bağlanmak, `myAccountName` depolama hesabınızın adı ile `myAccountKey` değiştirmek, hesap erişim `mySuffix` anahtarınızla değiştirin ve URI sonekiyle değiştirmek isteyip istemediğinizbelirtin:

```
DefaultEndpointsProtocol=[http|https];
AccountName=myAccountName;
AccountKey=myAccountKey;
EndpointSuffix=mySuffix;
```

Azure China 21Vianet'teki depolama hizmetleri için örnek bir bağlantı dizesi aşağıda verilmiştir:

```
DefaultEndpointsProtocol=https;
AccountName=storagesample;
AccountKey=<account-key>;
EndpointSuffix=core.chinacloudapi.cn;
```

## <a name="parsing-a-connection-string"></a>Bağlantı dizelerini ayrıştma

[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Geliştirme ve sınama için Azure depolama emülatörü'ni kullanma](storage-use-emulator.md)
* [Azure Depolama kaşifleri](storage-explorers.md)
* [Paylaşılan Erişim İmzalarını (SAS) Kullanma](storage-sas-overview.md)
