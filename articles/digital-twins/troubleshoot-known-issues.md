---
title: Bilinen sorunlar-Azure dijital TWINS
description: Azure dijital TWINS ile ilgili bilinen sorunları algılamayı ve azaltıcı yardım alın.
author: baanders
ms.author: baanders
ms.topic: troubleshooting
ms.service: digital-twins
ms.date: 07/14/2020
ms.openlocfilehash: bdde2076039a6f7687e06edef6dfd6f6f5148ce4
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87044149"
---
# <a name="known-issues-in-azure-digital-twins"></a>Azure dijital TWINS 'de bilinen sorunlar

Bu makalede, Azure dijital TWINS ile ilişkili bilinen sorunlar hakkında bilgi sağlanır.

## <a name="400-client-error-bad-request-in-cloud-shell"></a>"400 istemci hatası: Cloud Shell içinde bozuk Istek"

Cloud Shell Komutları zaman zaman "400 Istemci hatası: URL için hatalı Istek: http://localhost:50342/oauth2/token " ve ardından tam yığın izleme tarafından hata vererek başarısız olabilir.

### <a name="troubleshooting-steps"></a>Sorun giderme adımları

Bu, komutu yeniden çalıştırarak `az login` ve sonraki oturum açma adımları tamamlanırken çözülebilir.

Bundan sonra, komutunu yeniden çalıştırabilmelisiniz.

### <a name="possible-causes"></a>Olası nedenler

Bu Cloud Shell, bilinen bir sorunun sonucudur: [*Cloud Shell Token alma işlemi, 400 Istemci hatasıyla başarısız oluyor: Hatalı istek*](https://github.com/Azure/azure-cli/issues/11749).

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital TWINS üzerinde güvenlik ve izinler hakkında daha fazla bilgi edinin:
* [*Kavramlar: Azure dijital TWINS çözümleri için güvenlik*](concepts-security.md)