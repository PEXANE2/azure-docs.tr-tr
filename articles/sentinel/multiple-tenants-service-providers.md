---
title: Azure Sentinel 'de birden çok kiracıyı yönetilen güvenlik hizmeti sağlayıcısı olarak yönetme | Microsoft Docs
description: Azure 'un açık bir güvenlik hizmet sağlayıcısı (MSSP) olarak Azure 'da bulunan birden çok kiracıyı Azure açık Thouse ile ekleme ve yönetme.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/29/2020
ms.author: yelevin
ms.openlocfilehash: 011ddb883c028a954a8b0683c220bf6341eddb66
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91578164"
---
# <a name="manage-multiple-tenants-in-azure-sentinel-as-an-mssp"></a>Azure Sentinel 'de birden fazla kiracıyı MSSP olarak yönetme

Yönetilen bir güvenlik hizmeti sağlayıcısıysanız (MSSP) ve müşterilerinize güvenlik işlemleri Merkezi (SOC) hizmetleri sunmak için [Azure Mathouse](../lighthouse/overview.md) kullanıyorsanız, müşterilerin kiracısına bağlanabilmek gerekmeden müşterilerinizin Azure Sentinel kaynaklarını doğrudan kendi Azure kiracınızdan yönetebilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

- [Azure ışıklı kullanımı](../lighthouse/how-to/onboard-customer.md)

- Bunun düzgün çalışması için kiracınızda (MSSP kiracısı) Azure Sentinel kaynak sağlayıcılarının en az bir abonelikte kayıtlı olması gerekir. Ayrıca, müşterilerinizin kiracılarının her birinde kaynak sağlayıcılarının kayıtlı olması gerekir. Kiracınızda Azure Sentinel 'e kaydolduysanız ve müşterileriniz içindeki müşterileriniz, kullanmaya başlamak için hazırlanın. Kaydı doğrulamak için aşağıdaki adımları uygulayın:

    1. Azure portal **abonelikler** ' i seçin ve ardından menüden ilgili bir abonelik seçin.

    1. Abonelik ekranındaki gezinti menüsünde, **Ayarlar**' ın altında **kaynak sağlayıcıları**' nı seçin.

    1. ** *Abonelik adından* | Kaynak sağlayıcıları** ekranı, *Microsoft. Operationalınsights* ve *Microsoft. securityınsights*'ı arayıp seçin ve **durum** sütununu kontrol edin. Sağlayıcının durumu *Notregistered*Ise, **Kaydet**' i seçin.
    
        :::image type="content" source="media/multiple-tenants-service-providers/check-resource-provider.png" alt-text="Kaynak sağlayıcılarını denetleme":::

## <a name="how-to-access-azure-sentinel-in-managed-tenants"></a>Yönetilen kiracılarda Azure Sentinel 'e erişme

1. **Dizin + abonelik**altında, temsilcili dizinler (Dizin = kiracı) ve müşterinizin Azure Sentinel çalışma alanlarının bulunduğu abonelikler ' i seçin.

    :::image type="content" source="media/multiple-tenants-service-providers/directory-subscription.png" alt-text="Kaynak sağlayıcılarını denetleme":::

1. Azure Sentinel 'i açın. Seçili aboneliklerdeki tüm çalışma alanlarını görürsünüz ve kendi kiracınızdaki herhangi bir çalışma alanı gibi bunlarla sorunsuz bir şekilde çalışabilirsiniz.

> [!NOTE]
> Yönetilen bir çalışma alanının içinden Azure Sentinel 'e bağlayıcılar dağıtamazsınız. Bir bağlayıcıyı dağıtmak için, bağlayıcı dağıtmak istediğiniz kiracıya doğrudan oturum açmanız ve gerekli izinlerle ilgili olarak kimlik doğrulaması yapmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, birden çok Azure Sentinel kiracıyı sorunsuz bir şekilde yönetmeyi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.

