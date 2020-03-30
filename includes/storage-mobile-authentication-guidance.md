---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: fe4ecc237b56575f99844d3ec074225fadb69d3c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67673557"
---
## <a name="configure-your-application-to-access-azure-storage"></a>Uygulamanızı Azure Depolama'ya erişmek için yapılandırın
Depolama hizmetlerine erişmek için uygulamanızın kimliğini doğrulamanın iki yolu vardır:

* Paylaşılan Anahtar: Paylaşılan Anahtarı yalnızca test amacıyla kullanın
* Paylaşılan Erişim İmzası (SAS): Üretim uygulamaları için SAS kullanın

### <a name="shared-key"></a>Paylaşılan Anahtar
Paylaşılan Anahtar kimlik doğrulaması, uygulamanızın Depolama hizmetlerine erişmek için hesap adınızı ve hesap anahtarınızı kullanacağı anlamına gelir. Bu kitaplığın nasıl kullanılacağını hızlı bir şekilde göstermek amacıyla, bu başlangıçta Paylaşılan Anahtar kimlik doğrulamasını kullanacağız.

> [!WARNING] 
> **Paylaşılan Anahtar kimlik doğrulamasını yalnızca test amacıyla kullanın!** İlişkili Depolama hesabına tam okuma/yazma erişimi sağlayan hesap adınız ve hesap anahtarınız, uygulamanızı indiren her kişiye dağıtılır. Bu, anahtarınızın güvenilmeyen müşteriler tarafından tehlikeye atılma riskiyle karşınıza iyi bir uygulama **değildir.**
> 
> 

Paylaşılan Anahtar kimlik doğrulamasını kullanırken, bir [bağlantı dizesi](../articles/storage/common/storage-configure-connection-string.md)oluşturursunuz. Bağlantı dizesi aşağıdakilerden oluşur:  

* **DefaultEndpointsProtocol** - HTTP veya HTTPS'yi seçebilirsiniz. Ancak, HTTPS'nin kullanılması önerilir.
* **Hesap Adı** - depolama hesabınızın adı
* **Hesap Anahtarı** - [Azure Portalı'nda,](https://portal.azure.com)depolama hesabınıza gidin ve bu bilgileri bulmak için **Anahtarlar** simgesine tıklayın.
* (İsteğe bağlı) **EndpointSonefix** - Azure Çinveya Azure Yönetimi gibi farklı uç nokta soneklerine sahip bölgelerdeki depolama hizmetleri için kullanılır.

Paylaşılan Anahtar kimlik doğrulamasını kullanarak bağlantı dizesinin bir örneği aşağıda verilmiştir:

`"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here"`

### <a name="shared-access-signatures-sas"></a>Paylaşılan Erişim İmzaları (SAS)
Mobil bir uygulama için, bir istemcinin Azure Depolama hizmetine karşı bir isteği doğrulamak için önerilen yöntem, Paylaşılan Erişim İmzası (SAS) kullanmaktır. SAS, bir istemciye belirli bir süre için belirli bir izin kümesiyle bir kaynağa erişim izni vermenize olanak tanır.
Depolama hesabı sahibi olarak, mobil müşterilerinizin tükettiği bir SAS oluşturmanız gerekir. SAS oluşturmak için, büyük olasılıkla müşterilerinize dağıtılacak SAS üreten ayrı bir hizmet yazmak isteyeceksiniz. SsAS oluşturmak için microsoft [Azure Depolama Gezgini'ni](https://storageexplorer.com) veya Azure [Portalını](https://portal.azure.com) sas oluşturmak için sis kullanmak için sis amacını kullanabilirsiniz. SAS'ı oluşturduğunuzda, SAS'ın geçerli olduğu zaman aralığını ve SAS'ın istemciye verdiği izinleri belirtebilirsiniz.

Aşağıdaki örnek, Bir SAS oluşturmak için Microsoft Azure Depolama Gezgini'nin nasıl kullanılacağını gösterir.

1. Henüz yapmadıysanız, [Microsoft Azure Depolama Gezgini'ni yükleyin](https://storageexplorer.com)
2. Aboneliğinize bağlanın.
3. Depolama hesabınıza tıklayın ve sol alttaki "Eylemler" sekmesine tıklayın. SAS'ınız için bir "bağlantı dizesi" oluşturmak için "Paylaşılan Erişim İmzası Alın"ı tıklatın.
4. Burada, Depolama hesabının blob hizmeti için hizmet, kapsayıcı ve nesne düzeyinde okuma ve yazma izinleri veren bir SAS bağlantı dizesinin bir örneği verilmiştir.
   
   `"SharedAccessSignature=sv=2015-04-05&ss=b&srt=sco&sp=rw&se=2016-07-21T18%3A00%3A00Z&sig=3ABdLOJZosCp0o491T%2BqZGKIhafF1nlM3MzESDDD3Gg%3D;BlobEndpoint=https://youraccount.blob.core.windows.net"`

Gördüğünüz gibi, SAS kullanırken, uygulamanızda hesap anahtarınızı ifşa etmiyorsunuz. Paylaşılan Erişim İmzalarını kontrol ederek SAS ve SAS'ı kullanmak için en iyi uygulamalar hakkında daha fazla bilgi [edinebilirsiniz: SAS modelini anlama.](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)

