---
title: Azure ön kapısı Premium 'u özel bağlantıyla bir depolama hesabı kaynağına bağlama
titleSuffix: Azure Private Link
description: Azure ön kapı Premium uygulamanızı özel olarak bir depolama hesabına bağlamayı öğrenin.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 03/04/2021
ms.author: tyao
ms.openlocfilehash: 885b4d132208ab6f8b470d147438e26a5fd4bab7
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102201677"
---
# <a name="connect-azure-front-door-premium-to-a-storage-account-origin-with-private-link"></a>Azure ön kapısı Premium 'u özel bağlantıyla bir depolama hesabı kaynağına bağlama

Bu makalede, Azure ön kapısının Azure özel bağlantı hizmeti kullanarak depolama hesabınızın kaynağına özel olarak bağlanması için nasıl yapılandırılacağı açıklanır.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com) oturum açın.

## <a name="enable-private-link-to-a-storage-account"></a>Bir depolama hesabına özel bağlantıyı etkinleştir
 
Bu bölümde, özel bağlantı hizmetini Azure ön kapısının özel ağında oluşturulan özel bir uç nokta ile eşlersiniz. 

1. Azure ön kapısı Premium profilinizde, *Ayarlar*' ın altında, **kaynak grupları**' nı seçin.

1. Özel bağlantısını etkinleştirmek istediğiniz depolama hesabı kaynağını içeren kaynak grubunu seçin.

1. Yeni bir depolama hesabı kaynağı eklemek için **+ Kaynak Ekle** ' yi seçin veya listeden daha önce oluşturulmuş bir depolama hesabı kaynağını seçin.

    :::image type="content" source="../media/how-to-enable-private-link-storage-account/private-endpoint-storage-account.png" alt-text="Bir depolama hesabına özel bağlantıyı etkinleştirme ekran görüntüsü.":::

1. **Azure kaynağı seç** için **dizinimde** öğesini seçin. Azure ön kapı Premium 'un özel olarak bağlanmasını istediğiniz siteyi yapılandırmak için aşağıdaki ayarları seçin veya girin.

    | Ayar | Değer |
    | ------- | ----- |
    | Region | Aynı veya kaynağınıza en yakın bölgeyi seçin. |
    | Kaynak türü | **Microsoft. Storage/storageAccounts** öğesini seçin. |
    | Kaynak | Depolama hesabınızı seçin. |
    | Hedef alt kaynak | *BLOB* veya *Web*' i seçebilirsiniz. |
    | İstek iletisi | İletiyi özelleştirin veya varsayılanı seçin. |

1. Ardından, yapılandırmanızı kaydetmek için **Ekle** ' yi seçin.

## <a name="approve-private-endpoint-connection-from-the-storage-account"></a>Depolama hesabından özel uç nokta bağlantısını onayla

1. Son bölümde özel bağlantısını yapılandırdığınız depolama hesabına gidin. **Ayarlar** altında **ağ** ' ı seçin.

1. **Ağ** bölümünde **Özel uç nokta bağlantıları**' nı seçin. 

    :::image type="content" source="../media/how-to-enable-private-link-storage-account/storage-account-configure-endpoint.png" alt-text="Bir Web uygulamasındaki ağ ayarlarının ekran görüntüsü.":::

1. Azure ön kapısı Premium 'dan *bekleyen* özel uç nokta isteği ' ni seçin ve **Onayla**' yı seçin.

    :::image type="content" source="../media/how-to-enable-private-link-storage-account/private-endpoint-pending-approval.png" alt-text="Bekleyen depolama özel uç noktası isteğinin ekran görüntüsü.":::

1. Onaylandığında, aşağıdaki ekran görüntüsü gibi görünmelidir. Bağlantının tam olarak kurması birkaç dakika sürer. Artık Azure ön kapısı Premium 'dan depolama hesabınıza erişebilirsiniz.

    :::image type="content" source="../media/how-to-enable-private-link-storage-account/private-endpoint-approved.png" alt-text="Onaylanan depolama uç noktası isteğinin ekran görüntüsü.":::

## <a name="next-steps"></a>Sonraki adımlar

[Depolama hesabı Ile özel bağlantı hizmeti](../../storage/common/storage-private-endpoints.md)hakkında bilgi edinin.
