---
title: Azure ön kapı Premium 'u özel bağlantıyla kaynağınıza bağlayın
description: Azure portal kullanarak Azure ön kapılarınızı özel bağlantı hizmetiyle kaynağınıza bağlamayı öğrenin.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2021
ms.author: tyao
ms.openlocfilehash: 3015a0560171b61aaab05e27b369d9ca531e81c1
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "101100376"
---
# <a name="connect-azure-front-door-premium-to-your-origin-with-private-link"></a>Azure ön kapı Premium 'u özel bağlantıyla kaynağınıza bağlayın

Bu makalede, Azure özel bağlantı hizmeti kullanılarak sanal bir ağda barındırılan uygulamalarınıza bağlanmak için Azure ön kapısının nasıl yapılandırılacağı konusunda size kılavuzluk edilir.

## <a name="prerequisites"></a>Önkoşullar

* Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Kaynak web sunucularınız için [özel bir bağlantı](../../private-link/create-private-link-service-portal.md) hizmeti oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com) oturum açın.

## <a name="enable-private-endpoint-in-azure-front-door-service"></a>Azure ön kapı hizmetinde özel uç noktayı etkinleştirme

Bu bölümde, Azure özel bağlantı hizmetini Azure ön kapısı Premium SKU 'sunun özel ağında oluşturulan özel bir uç nokta ile eşlersiniz. 

1. Azure ön kapısı Premium profilinizde, *Ayarlar*' ın altında, **kaynak grupları**' nı seçin.

1. Özel bağlantısını etkinleştirmek istediğiniz kaynağı içeren kaynak grubunu seçin.

1. Yeni bir kaynak eklemek için **+ Kaynak Ekle** ' yi seçin veya listeden daha önce oluşturulmuş bir kaynağı seçin. Ardından **özel bağlantı hizmetini etkinleştirmek** için onay kutusunu seçin.

    :::image type="content" source="../media/how-to-enable-private-link/front-door-private-endpoint-private-link.png" alt-text="Kaynak Ekle sayfasında özel bağlantının etkinleştirilmesinin ekran görüntüsü.":::

1. **Azure kaynağı seç** için **dizinimde** öğesini seçin. Azure ön kapı Premium 'un özel olarak bağlanmasını istediğiniz kaynağı yapılandırmak için aşağıdaki ayarı seçin veya girin.
    
    | Ayar | Değer |
    | ------- | ----- |
    | Region | Aynı veya kaynağınıza en yakın bölgeyi seçin. |
    | Kaynak türü | **Microsoft. Network/privateLinkServices** öğesini seçin. |
    | Kaynak | **Myprivatelinkservice** öğesini seçin. |
    | Hedef alt kaynak | Bu alanı boş bırakın. |
    | İstek iletisi | İletiyi özelleştirin veya varsayılan iletiyi seçin. |

## <a name="next-steps"></a>Sonraki Adımlar

[Azure ön kapısı Premium özel bağlantısı](concept-private-link.md)hakkında bilgi edinin.
