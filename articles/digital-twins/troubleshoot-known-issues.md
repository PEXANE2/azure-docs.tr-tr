---
title: Bilinen sorunlar-Azure dijital TWINS
description: Azure dijital TWINS ile ilgili bilinen sorunları algılamayı ve azaltıcı yardım alın.
author: baanders
ms.author: baanders
ms.topic: troubleshooting
ms.service: digital-twins
ms.date: 07/14/2020
ms.openlocfilehash: 8fce451d9b806d2fa9a4f3d9e1c117de0aaa9fc0
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86532295"
---
# <a name="known-issues-in-azure-digital-twins"></a>Azure dijital TWINS 'de bilinen sorunlar

Bu makalede, Azure dijital TWINS ile ilişkili bilinen sorunlar hakkında bilgi sağlanır.

## <a name="managing-event-routes-in-the-azure-portal"></a>Azure portal olay yollarını yönetme

Portalda bir hesap gibi kişisel [**Microsoft hesabı (MSA)**](https://account.microsoft.com/account/Account)oturum açtıysanız, *@outlook.com* izin düzeyinizden bağımsız olarak portalda olay yollarını yönetmeye çalışırken *olay yollarını görüntüleme izninizin olması gerektiğini* belirten bir ekran görürsünüz.

:::image type="content" source="media/troubleshoot-known-issues/event-route-need-permission.png" alt-text="Azure dijital TWINS örneğinde olay yolları oluşturmaya çalışırken izin hatasının Azure portal ekran görüntüsü":::

### <a name="troubleshooting-steps"></a>Sorun giderme adımları

Şu anda portalda olay yollarını yönetemeyen kullanıcılar, Azure Digital TWINS API 'Leri veya CLı kullanarak olay yollarını yine de yönetebilir. Bu araçlardan birine geçiş yapmak, bu sorunu azaltmak için önerilen stratejidir.

Bu yönergeler, [*nasıl yapılır: uç noktaların ve yolların yönetilmesi*](how-to-manage-routes.md)bölümünde bulunabilir.

### <a name="possible-causes"></a>Olası nedenler

Portalda, hesap gibi bir kişisel [Microsoft hesabı (MSA)](https://account.microsoft.com/account/Account)oturum açtınız *@outlook.com* . Azure portal olay yollarının yönetilmesi Şu anda yalnızca kurumsal etki alanı hesaplarında Azure kullanıcıları tarafından kullanılabilir.

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