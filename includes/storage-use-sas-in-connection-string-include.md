---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 2f27c50b1d016265c20102521a137bcbb0646115
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67188589"
---
Bir depolama hesabındaki kaynaklara erişmenizi sağlayan bir paylaşılan erişim imzası (SAS) URL 'sine sahipseniz, SAS 'yi bir bağlantı dizesinde kullanabilirsiniz. SAS, isteğin kimliğini doğrulamak için gereken bilgileri içerdiğinden, SAS içeren bir bağlantı dizesi protokol, hizmet uç noktası ve kaynağa erişmek için gerekli kimlik bilgilerini sağlar.

Paylaşılan erişim imzası içeren bir bağlantı dizesi oluşturmak için, dizeyi aşağıdaki biçimde belirtin:

```
BlobEndpoint=myBlobEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
FileEndpoint=myFileEndpoint;
SharedAccessSignature=sasToken
```

Her hizmet uç noktası isteğe bağlıdır, ancak bağlantı dizesinin en az bir içermesi gerekir.

> [!NOTE]
> En iyi uygulama olarak, bir SAS ile HTTPS kullanılması önerilir.
>
> Bir yapılandırma dosyasında bir bağlantı dizesinde SAS belirtiyorsanız, URL 'de özel karakterler kodlamak gerekebilir.
>
>

### <a name="service-sas-example"></a>Hizmet SAS örneği
Blob depolaması için bir hizmet SAS içeren bir bağlantı dizesi örneği aşağıda verilmiştir:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
SharedAccessSignature=sv=2015-04-05&sr=b&si=tutorial-policy-635959936145100803&sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D
```

Özel karakterlerin kodlanması ile aynı bağlantı dizesine bir örnek aşağıda verilmiştir:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
SharedAccessSignature=sv=2015-04-05&amp;sr=b&amp;si=tutorial-policy-635959936145100803&amp;sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D
```

### <a name="account-sas-example"></a>Hesap SAS örneği
Blob ve dosya depolama için hesap SAS içeren bir bağlantı dizesi örneği aşağıda verilmiştir. Her iki hizmet için uç noktaların belirtildiğine unutmayın:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
FileEndpoint=https://storagesample.file.core.windows.net;
SharedAccessSignature=sv=2015-07-08&sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&spr=https&st=2016-04-12T03%3A24%3A31Z&se=2016-04-13T03%3A29%3A31Z&srt=s&ss=bf&sp=rwl
```

URL kodlamalı aynı bağlantı dizesine bir örnek aşağıda verilmiştir:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
FileEndpoint=https://storagesample.file.core.windows.net;
SharedAccessSignature=sv=2015-07-08&amp;sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&amp;spr=https&amp;st=2016-04-12T03%3A24%3A31Z&amp;se=2016-04-13T03%3A29%3A31Z&amp;srt=s&amp;ss=bf&amp;sp=rwl
```

