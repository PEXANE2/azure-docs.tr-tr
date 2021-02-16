---
title: Oturum yönetimi REST API
description: Oturumların nasıl yönetileceğini açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: 9eb1bb87792dbc61e7c85dbc20c136499f23f67c
ms.sourcegitcommit: 7ec45b7325e36debadb960bae4cf33164176bc24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2021
ms.locfileid: "100530221"
---
# <a name="use-the-session-management-rest-api"></a>Oturum yönetimi REST API’yi kullanma

Azure uzaktan Işleme işlevselliğini kullanmak için bir *oturum* oluşturmanız gerekir. Her oturum, bir istemci cihazının bağlanabileceği Azure 'da ayrılan bir sunucuya karşılık gelir. Bir cihaz bağlandığı zaman sunucu, istenen verileri işler ve sonucu bir video akışı olarak sunar. Oturum oluşturma sırasında, hangi [tür sunucuyu](../reference/vm-sizes.md) çalıştırmak istediğinizi tercih edersiniz ve bu da fiyatlandırmayı belirler. Oturum artık gerekmiyorsa, durdurulmalıdır. El ile durdurulmamışsa, oturumun *kira süresi* sona erdiğinde otomatik olarak kapatılır.

## <a name="rest-api-reference"></a>REST API başvurusu

REST API başvurusu [burada ve Swagger](https://docs.microsoft.com/rest/api/mixedreality/2021-01-01preview/remoterendering) [tanımlarında bulunabilir](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mixedreality/data-plane/Microsoft.MixedReality).
*RenderingSession.ps1* adlı *betikler* klasöründeki [ARR örnekleri deposunda](https://github.com/Azure/azure-remote-rendering) , hizmetimizin kullanımını gösteren bir PowerShell betiği sağlıyoruz. Betik ve yapılandırması burada açıklanmıştır: [örnek PowerShell betikleri](../samples/powershell-example-scripts.md).
Ayrıca, [.net](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/mixedreality/Azure.MixedReality.RemoteRendering), Java ve Python için SDK 'lar sunuyoruz.

> [!IMPORTANT]
> Gecikme, uzaktan işleme kullanılırken önemli bir faktördür. En iyi deneyim için, size en yakın bölgede oturum oluşturun. [Azure gecikme testi](https://www.azurespeed.com/Azure/Latency) , size en yakın bölgeyi tespit etmek için kullanılabilir.

> [!IMPORTANT]
> İstemci cihazının bir işleme oturumuna bağlanması için bir ARR çalışma zamanı SDK 'Sı gerekir. Bu SDK 'lar [.net](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering?view=remoterendering) ve [C++](https://docs.microsoft.com/cpp/api/remote-rendering/)' da kullanılabilir. Hizmete bağlandıktan sonra, bu SDK 'lar oturumları başlatmak ve durdurmak için de kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Kimlik doğrulaması için Azure ön uç API 'Lerini kullanma](frontend-apis.md)
* [Örnek PowerShell betikleri](../samples/powershell-example-scripts.md)
