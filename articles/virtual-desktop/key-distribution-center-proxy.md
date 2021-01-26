---
title: Kerberos Anahtar Dağıtım Merkezi proxy Windows sanal masaüstü 'Nü ayarlama-Azure
description: Bir Windows sanal masaüstü konak havuzunu Kerberos Anahtar Dağıtım Merkezi proxy kullanacak şekilde ayarlama.
author: Heidilohr
ms.topic: how-to
ms.date: 01/26/2021
ms.author: helohr
manager: lizross
ms.openlocfilehash: f123659941eaeb3b8ceeb6a999abf836eb9cf5ea
ms.sourcegitcommit: 95c2cbdd2582fa81d0bfe55edd32778ed31e0fe8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98798479"
---
# <a name="configure-a-kerberos-key-distribution-center-proxy"></a>Kerberos Anahtar Dağıtım Merkezi proxy yapılandırma

Bu makalede, konak havuzunuz için bir Kerberos Key Distribiution Center (KDC) proxy 'sinin nasıl yapılandırılacağı gösterilir. Bu proxy, kuruluşların Kurumsal sınırlarının dışında Kerberos ile kimlik doğrulamasını sağlar. Örneğin, KDC proxy 'yi dış istemciler için akıllı kart kimlik doğrulamasını etkinleştirmek üzere kullanabilirsiniz.

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
