---
title: Kerberos Anahtar Dağıtım Merkezi proxy Windows sanal masaüstü 'Nü ayarlama-Azure
description: Bir Windows sanal masaüstü konak havuzunu Kerberos Anahtar Dağıtım Merkezi proxy kullanacak şekilde ayarlama.
author: Heidilohr
ms.topic: how-to
ms.date: 01/30/2021
ms.author: helohr
manager: lizross
ms.openlocfilehash: 102ddc1c8937c66a92416ddb6d5f2d25f2a3c349
ms.sourcegitcommit: 54e1d4cdff28c2fd88eca949c2190da1b09dca91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2021
ms.locfileid: "99219664"
---
# <a name="configure-a-kerberos-key-distribution-center-proxy-preview"></a>Kerberos Anahtar Dağıtım Merkezi ara sunucusunu yapılandırma (Önizleme)

> [!IMPORTANT]
> Bu özellik şu anda genel önizleme aşamasındadır.
> Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve bunu üretim iş yükleri için kullanmanızı önermiyoruz. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede, konak havuzunuz için Kerberos Anahtar Dağıtım Merkezi (KDC) proxy 'sinin (Önizleme) nasıl yapılandırılacağı gösterilir. Bu proxy, kuruluşların Kurumsal sınırlarının dışında Kerberos ile kimlik doğrulamasını sağlar. Örneğin, KDC proxy 'yi dış istemciler için akıllı kart kimlik doğrulamasını etkinleştirmek üzere kullanabilirsiniz.

## <a name="how-to-configure-the-kdc-proxy"></a>KDC proxy 'yi yapılandırma

KDC proxy 'yi yapılandırmak için:

1. Azure Portal’da yönetici olarak oturum açın.

2. Windows sanal masaüstü sayfasına gidin.

3. KDC ara sunucusunu etkinleştirmek istediğiniz konak havuzunu seçin ve ardından **RDP özellikleri**' ni seçin.

    > [!div class="mx-imgBorder"]
    > ![Bir kullanıcının konak havuzlarını seçip örnek ana bilgisayar havuzunun adı ve ardından RDP özellikleri gösteren Azure portal sayfasının ekran görüntüsü.](media/rdp-properties.png)

4. **Gelişmiş** sekmesini seçin ve ardından boşluk olmadan aşağıdaki biçimde bir değer girin:

    > kdcproxyname: s:\<fqdn\>

    > [!div class="mx-imgBorder"]
    > ![Adım 4 ' te açıklanan değer ile, seçilen Gelişmiş sekmesini gösteren ekran görüntüsü.](media/advanced-tab-selected.png)

5. **Kaydet**’i seçin.

6. Seçili konak havuzu artık, dahil ettiğiniz kdcproxyname alanıyla RDP bağlantı dosyaları vermeye başlamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Uzak Masaüstü Hizmetleri 'daki RDGateway rolü bir KDC proxy hizmeti içerir. Windows sanal masaüstü için bir hedefin hedefi olacak şekilde nasıl ayarlanacağı hakkında bilgi için bkz. [Windows sanal masaüstü 'nde RD Ağ geçidi rolünü dağıtma](rd-gateway-role.md) .
