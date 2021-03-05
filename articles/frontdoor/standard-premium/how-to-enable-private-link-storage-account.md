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
ms.openlocfilehash: 885582481e3783bdd4fbad40a24499f42a40ce24
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102193515"
---
# <a name="connect-azure-front-door-premium-to-a-storage-account-origin-with-private-link"></a>Azure ön kapısı Premium 'u özel bağlantıyla bir depolama hesabı kaynağına bağlama

Bu makalede, Azure ön kapısının Azure özel bağlantı hizmeti kullanarak depolama hesabınızın kaynağına özel olarak bağlanması için nasıl yapılandırılacağı açıklanır.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com) oturum açın.

## <a name="enable-private-link-to-a-storage-account-in-azure-front-door-premium"></a>Azure ön kapısı Premium 'da bir depolama hesabına özel bağlantıyı etkinleştir
 
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

## <a name="approve-azure-front-door-premium-private-endpoint-connection-from-the-storage-account"></a>Depolama hesabından Azure ön kapı Premium özel uç nokta bağlantısını onaylama

1. Son bölümde özel bağlantısını yapılandırdığınız depolama hesabına gidin. **Ayarlar** altında **ağ** ' ı seçin.

1. **Ağ** bölümünde **Özel uç nokta bağlantıları**' nı seçin. 

    :::image type="content" source="../media/how-to-enable-private-link-storage-account/storage-account-configure-endpoint.png" alt-text="Bir Web uygulamasındaki ağ ayarlarının ekran görüntüsü.":::

1. Azure ön kapısı Premium 'dan *bekleyen* özel uç nokta isteği ' ni seçin ve **Onayla**' yı seçin.

    :::image type="content" source="../media/how-to-enable-private-link-storage-account/private-endpoint-pending-approval.png" alt-text="Bekleyen depolama özel uç noktası isteğinin ekran görüntüsü.":::

1. Onaylandığında, aşağıdaki ekran görüntüsü gibi görünmelidir. Bağlantının tam olarak kurması birkaç dakika sürer. Artık Azure ön kapısı Premium 'dan depolama hesabınıza erişebilirsiniz.

    :::image type="content" source="../media/how-to-enable-private-link-storage-account/private-endpoint-approved.png" alt-text="Onaylanan depolama uç noktası isteğinin ekran görüntüsü.":::

## <a name="next-steps"></a>Sonraki adımlar

[Depolama hesabı Ile özel bağlantı hizmeti](../../storage/common/storage-private-endpoints.md)hakkında bilgi edinin.
