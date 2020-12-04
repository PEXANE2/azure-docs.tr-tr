---
title: 'Azure ExpressRoute: Azure portal kullanarak devre eşlemesini sıfırlama'
description: Azure portal kullanarak bir Azure ExpressRoute bağlantı hattı eşayarlarını devre dışı bırakmayı ve etkinleştirmeyi öğrenin.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 11/30/2020
ms.author: duau
ms.openlocfilehash: d4d6b0b0cce4f5304f7c5790ef2bda05633be52f
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96582986"
---
# <a name="reset-expressroute-circuit-peerings-use-the-azure-portal"></a>ExpressRoute devre eşayarlarını sıfırlayın Azure portal kullanın

Bu makalede, Azure portal kullanarak bir ExpressRoute bağlantı hattı eşayarlarını devre dışı bırakma ve etkinleştirme açıklanmaktadır. Bir eşlemeyi devre dışı bıraktığınızda, ExpressRoute devreniz için hem birincil hem de ikincil bağlantı için BGP oturumu kapatılır. Bu eşleme ile Microsoft 'a bağlantıyı kaybedeceksiniz. Bir eşlemeyi etkinleştirdiğinizde, ExpressRoute bağlantı hattının hem birincil hem de ikincil bağlantısı üzerinde BGP oturumu başlatılır. Bu eşleme ile bağlantıyı Microsoft 'a geri kazanacaksınız. ExpressRoute bağlantı hattındaki Microsoft eşlemesini ve Azure özel eşlemesini bağımsız olarak etkinleştirebilir ve devre dışı bırakabilirsiniz. ExpressRoute bağlantı hattınızı ilk kez yapılandırdığınızda, eşlemeler varsayılan olarak etkindir.

ExpressRoute eşlerinizi sıfırlamanızı yararlı bulabileceğiniz birkaç senaryo vardır.
* Olağanüstü durum kurtarma tasarımınızı ve uygulamanızı test edin. Örneğin, iki ExpressRoute devreniz vardır. Bir devrenin eşayarlarını devre dışı bırakabilir ve ağ trafiğinizi diğer devreye devretmek için zorlayabilirsiniz.
* Azure özel eşleme veya ExpressRoute devrenizin Microsoft eşlemesi üzerinde çift yönlü Iletme algılamasını (BFD) etkinleştirin. ExpressRoute bağlantı hattı 1 2018 Ağustos 'Tan sonra ve Microsoft eşlemesi 'nde oluşturulduysa, BFD varsayılan olarak Azure özel eşleme 'de etkinleştirilir. ExpressRoute devreniz 10 2020 Ocak 'tan sonra oluşturulduysa. Devreniz daha önce oluşturulduysa BFD etkin değildi. Eşlemeyi devre dışı bırakarak ve yeniden etkinleştirerek BFD 'yi etkinleştirebilirsiniz. 

### <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Bir tarayıcıdan [Azure portalına](https://portal.azure.com) gidin ve Azure hesabınızla oturum açın.

## <a name="reset-a-peering"></a>Eşlemeyi sıfırlama

1. Eşleme yapılandırma değişikliklerini yapmak istediğiniz devreyi seçin.

    :::image type="content" source="./media/expressroute-howto-reset-peering-portal/expressroute-circuit-list.png" alt-text="ExpressRoute devre listesi":::

1. Etkinleştirmek veya devre dışı bırakmak istediğiniz eşleme yapılandırmasını seçin.

    :::image type="content" source="./media/expressroute-howto-reset-peering-portal/expressroute-circuit.png" alt-text="ExpressRoute devresine genel bakış":::

1. Eşlemeyi **Etkinleştir** seçeneğinin işaretini kaldırın ve eşleme yapılandırmasını devre dışı bırakmak için **Kaydet** 'i seçin.

    :::image type="content" source="./media/expressroute-howto-reset-peering-portal/disable-peering.png" alt-text="Özel eşlemeyi devre dışı bırak":::

1. **Eşlemeyi etkinleştir** ' i seçip **Kaydet**' i seçerek eşlemeyi yeniden etkinleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
ExpressRoute sorununun giderilmesi için yardıma ihtiyacınız varsa aşağıdaki makalelere göz atın:
* [ExpressRoute bağlantısını doğrulama](expressroute-troubleshooting-expressroute-overview.md)
* [Ağ performansı sorunlarını giderme](expressroute-troubleshooting-network-performance.md)
