---
title: Azure ön kapı hizmeti 'nde HTTP üstbilgileri için protokol desteği | Microsoft Docs
description: Bu makalede, ön kapı hizmeti 'nin desteklediği HTTP üst bilgi protokolleri açıklanmaktadır.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 7c77527b7300c1149e96c94a4dbe122da226ac6d
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79280826"
---
# <a name="protocol-support-for-http-headers-in-azure-front-door-service"></a>Azure ön kapı hizmeti 'nde HTTP üstbilgileri için protokol desteği
Bu makalede, ön kapı hizmeti 'nin, çağrı yolunun bölümleriyle desteklediği protokol (bkz. görüntü) özetlenmektedir. Aşağıdaki bölümlerde, ön kapı hizmeti tarafından desteklenen HTTP üstbilgileri hakkında daha fazla bilgi sağlanmaktadır.

![Azure ön kapı hizmeti HTTP üstbilgileri Protokolü][1]

>[!IMPORTANT]
>Ön kapı hizmeti burada belgelenmeyen HTTP üstbilgilerini onaylayamaz.

## <a name="client-to-front-door-service"></a>İstemciden ön kapıya hizmet
Ön kapı hizmeti, gelen istekten çok sayıda üstbilgiyi değiştirmeden kabul eder. X-FD-* ön ekine sahip üstbilgiler dahil, gönderildiyse, bazı ayrılmış üst bilgiler gelen istekten kaldırılır.

## <a name="front-door-service-to-backend"></a>Ön uç hizmeti arka uca

Ön kapı hizmeti, kısıtlamalar nedeniyle kaldırılmadığı takdirde gelen bir isteğin üst bilgilerini içerir. Ön kapı aşağıdaki üst bilgileri de ekler:

| Üst bilgi  | Örnek ve açıklama |
| ------------- | ------------- |
| Yazıcısıyla |  Aracılığıyla: 1,1 Azure </br> Ön kapı, istemci HTTP sürümünü ve ardından *Azure* 'un arkasından, Via üstbilgisi için değer olarak ekler. Bu, istemcinin HTTP sürümünü ve bu ön kapıın istemci ile arka uç arasındaki istek için bir ara alıcı olduğunu gösterir.  |
| X-Azure-Clienentip | X-Azure-Clienentip: 127.0.0.1 </br> İşlenmekte olan istekle ilişkili istemci IP adresini temsil eder. Örneğin, bir proxy 'den gelen bir istek, özgün çağıranın IP adresini belirtmek için X-Iletilmiş-for üst bilgisini ekleyebilir. |
| X-Azure-SocketIP |  X-Azure-SocketIP: 127.0.0.1 </br> Geçerli isteğin kaynaklandığı TCP bağlantısıyla ilişkili yuva IP adresini temsil eder. İsteğin istemci IP adresi, bir kullanıcı tarafından rastgele üzerine yazılabileceğinden, yuva IP adresine eşit olmayabilir.|
| X-Azure-ref |  X-Azure-Ref: 0zxV + XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTkANDM3YzgyY2QtMzYwYS00YTU0LTk0YzMtNWZmNzA3NjQ3Nzgz </br> Ön kapıya göre sunulan isteği tanımlayan benzersiz bir başvuru dizesi. Bu, erişim günlüklerini aramak için kullanılır ve sorun giderme için kritik öneme sahiptir.|
| X-Azure-Requestzincirine |  X-Azure-Requestzincirine: atlama = 1 </br> Ön kapı, istek döngülerini algılamak için kullanılan bir üst bilgi ve kullanıcılar buna bağımlılığı almaz. |
| X-Iletilmiş-Için | X-Iletildi-for: 127.0.0.1 </br> X-Iletilmiş-for (XFF) HTTP üstbilgisi alanı, genellikle bir HTTP proxy veya yük dengeleyici aracılığıyla bir Web sunucusuna bağlanan bir istemcinin kaynak IP adresini belirler. Var olan bir XFF üst bilgisi varsa, ön kapı istemci yuvasını IP 'ye ekler veya XFF üstbilgisini istemci yuvası IP 'si ile ekler. |
| X-Iletilen-konak | X-Iletilen-ana bilgisayar: contoso.azurefd.net </br> X-Iletilen ana bilgisayar HTTP üst bilgisi alanı, istemci tarafından istenen özgün Konağı tanımlamak için kullanılan ortak bir yöntemdir, ana bilgisayar HTTP istek üst bilgisi. Bunun nedeni, ön kapıdan ana bilgisayar adının isteği işleyen arka uç sunucusu için farklı olabilir. |
| X-Iletilen-proto | X-Iletilen-proto: http </br> , Yapılandırmaya bağlı olan ön kapı, HTTPS kullanarak arka uca iletişim kurabildiğinden, bir HTTP isteğinin kaynak protokolünü belirlemek için genellikle X-Iletilen-proto HTTP üstbilgisi alanı kullanılır. Bu, ters proxy 'ye yönelik istek HTTP olsa bile geçerlidir. |
| X-FD-Healtharaştırması | X-FD-Healtharaştırması HTTP üstbilgisi alanı, ön kapıdan sistem durumu araştırmasını belirlemek için kullanılır. Bu üstbilgi 1 olarak ayarlandıysa, istek sistem durumu araştırmasına sahip olur. X ile Iletilen ana bilgisayar üst bilgisi alanı ile belirli ön kapıdan katı erişim sağlamak istediğinizde ' i kullanabilirsiniz. |

## <a name="front-door-service-to-client"></a>Ön kapı hizmeti istemciye

Arka uçtan ön kapıya gönderilen tüm üstbilgiler da istemciye geçirilir. Ön kapıdan istemcilere gönderilen üstbilgiler aşağıda verilmiştir.

| Üst bilgi  | Örnek |
| ------------- | ------------- |
| X-Azure-ref |  *X-Azure-Ref: 0zxV + XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTkANDM3YzgyY2QtMzYwYS00YTU0LTk0YzMtNWZmNzA3NjQ3Nzgz* </br> Bu, ön kapıya göre sunulan isteği tanımlayan benzersiz bir başvuru dizesidir. Bu, erişim günlüklerini aramak için kullanıldığı için sorun giderme açısından önemlidir.|

## <a name="next-steps"></a>Sonraki adımlar

- [Ön kapı oluşturma](quickstart-create-front-door.md)
- [Ön kapı nasıl kullanılır?](front-door-routing-architecture.md)

<!--Image references-->
[1]: ./media/front-door-http-headers-protocol/front-door-protocol-summary.png
