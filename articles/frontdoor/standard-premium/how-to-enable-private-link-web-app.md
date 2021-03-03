---
title: Azure ön kapısı Premium 'u özel bağlantıyla bir Web uygulaması kaynağına bağlama
titleSuffix: Azure Private Link
description: Azure ön kapısı Premium uygulamanızı özel olarak bir WebApp ile bağlamayı öğrenin.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2021
ms.author: tyao
ms.openlocfilehash: f3250101449971ffd985fcfcce400870e2a3d50f
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101746372"
---
# <a name="connect-azure-front-door-premium-to-a-web-app-origin-with-private-link"></a>Azure ön kapısı Premium 'u özel bağlantıyla bir Web uygulaması kaynağına bağlama

Bu makalede, Azure ön kapısının Azure özel bağlantı hizmeti kullanarak Web uygulamanıza özel olarak bağlanması için nasıl yapılandırılacağı açıklanır.

## <a name="prerequisites"></a>Önkoşullar

* Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Kaynak web sunucularınız için [özel bir bağlantı](../../private-link/create-private-link-service-portal.md) hizmeti oluşturun.

> [!Note]
> Özel uç nokta, PremiumV2-Tier, PremiumV3 katmanlı Windows Web uygulamaları, Linux Web Apps ve Azure Işlevleri Premium planı (bazen elastik Premium plan olarak adlandırılır) için genel bölgelerde kullanılabilir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com) oturum açın.

## <a name="enable-private-link-to-a-web-app-in-azure-front-door-premium"></a>Azure ön kapısı Premium 'da bir Web uygulamasına özel bağlantıyı etkinleştir
 
Bu bölümde, özel bağlantı hizmetini Azure ön kapısının özel ağında oluşturulan özel bir uç nokta ile eşlersiniz. 

1. Azure ön kapısı Premium profilinizde, *Ayarlar*' ın altında, **kaynak grupları**' nı seçin.

1. Özel bağlantısını etkinleştirmek istediğiniz Web uygulaması kaynağını içeren kaynak grubunu seçin.

1. Yeni bir Web uygulaması kaynağı eklemek için **+ Kaynak Ekle** ' yi seçin veya listeden daha önce oluşturulmuş bir Web uygulaması kaynağı seçin.

    :::image type="content" source="../media/how-to-enable-private-link-web-app/private-endpoint-web-app.png" alt-text="Web uygulamasına özel bağlantıyı etkinleştirme ekran görüntüsü.":::

1. **Azure kaynağı seç** için **dizinimde** öğesini seçin. Azure ön kapı Premium 'un özel olarak bağlanmasını istediğiniz siteyi yapılandırmak için aşağıdaki ayarları seçin veya girin.

    | Ayar | Değer |
    | ------- | ----- |
    | Region | Aynı veya kaynağınıza en yakın bölgeyi seçin. |
    | Kaynak türü | **Microsoft. Web/Sites** öğesini seçin. |
    | Kaynak | **Myprivatelinkservice** öğesini seçin. |
    | Hedef alt kaynak | Siteler |
    | İstek iletisi | İletiyi özelleştirin veya varsayılanı seçin. |

1. Ardından, yapılandırmanızı kaydetmek için **Ekle** ' yi seçin.

## <a name="approve-azure-front-door-premium-private-endpoint-connection-from-web-app"></a>Web uygulamasından Azure ön kapısı Premium özel uç nokta bağlantısını onaylama

1. Son bölümde özel bağlantısını yapılandırdığınız Web uygulamasına gidin. **Ayarlar** altında **ağ** ' ı seçin.

1. **Ağ**' da **Özel uç nokta bağlantılarınızı yapılandırın**' ı seçin.

    :::image type="content" source="../media/how-to-enable-private-link-web-app/web-app-configure-endpoint.png" alt-text="Bir Web uygulamasındaki ağ ayarlarının ekran görüntüsü.":::

1. Azure ön kapısı Premium 'dan *bekleyen* özel uç nokta isteği ' ni seçin ve **Onayla**' yı seçin.

    :::image type="content" source="../media/how-to-enable-private-link-web-app/private-endpoint-pending-approval.png" alt-text="Bekleyen özel uç nokta isteğinin ekran görüntüsü.":::

1. Onaylandığında, aşağıdaki ekran görüntüsü gibi görünmelidir. Bağlantının tam olarak kurması birkaç dakika sürer. Artık Azure ön kapısı Premium 'dan Web uygulamanıza erişebilirsiniz. Genel internet 'ten Web uygulamasına doğrudan erişim, Özel uç nokta etkinleştirildikten sonra devre dışı bırakılır.

    :::image type="content" source="../media/how-to-enable-private-link-web-app/private-endpoint-approved.png" alt-text="Onaylanan uç nokta isteğinin ekran görüntüsü.":::

## <a name="next-steps"></a>Sonraki adımlar

[Azure Web App Ile özel bağlantı hizmeti](../../app-service/networking/private-endpoint)hakkında bilgi edinin.
