---
title: Azure Stack Edge cihazına erişen Windows istemcilerinde TLS 1,2 yapılandırma
description: Azure Stack Edge cihazına erişen Windows istemcilerinde TLS 1,2 ' nin nasıl yapılandırılacağı açıklanmaktadır.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 01/06/2020
ms.author: alkohli
ms.openlocfilehash: 552e44e4d3d1b9e237d6ee99d82b946d34fa5e3d
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89086936"
---
# <a name="configure-tls-12-on-windows-clients-accessing-azure-stack-edge-device"></a>Azure Stack Edge cihazına erişen Windows istemcilerinde TLS 1,2 yapılandırma

<!--[!INCLUDE [applies-to-skus](../../includes/azure-stack-edge-applies-to-all-sku.md)]-->

Azure Stack Edge cihazınıza erişmek için bir Windows istemcisi kullanıyorsanız, istemcinizin TLS 1,2 ' i yapılandırmanız gerekir. Bu makalede, Windows istemcisinde TLS 1,2 ' i yapılandırmaya yönelik kaynaklar ve yönergeler sağlanmaktadır. 

Burada sunulan yönergeler, Windows Server 2016 çalıştıran bir istemcide gerçekleştirilen teste dayalıdır.

## <a name="configure-tls-12-for-current-powershell-session"></a>Geçerli PowerShell oturumu için TLS 1,2 yapılandırma

İstemcinizi TLS 1,2 ' i yapılandırmak için aşağıdaki adımları uygulayın.

1. PowerShell 'i yönetici olarak çalıştırın.
2. Geçerli PowerShell oturumunda TLS 1,2 ayarlamak için şunu yazın:
  
    ```azurepowershell
    $TLS12Protocol = [System.Net.SecurityProtocolType] 'Ssl3 , Tls12'
    [System.Net.ServicePointManager]::SecurityProtocol = $TLS12Protocol
    ```
## <a name="configure-tls-12-on-client"></a>İstemcide TLS 1,2 yapılandırma

Ortamınız için sistem genelinde TLS 1,2 ayarlamak istiyorsanız, bu belgelerdeki yönergeleri izleyin:

- [Genel-TLS 1,2 nasıl etkinleştirilir](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-12)
- [İstemcilerde TLS 1,2 nasıl etkinleştirilir](https://docs.microsoft.com/configmgr/core/plan-design/security/enable-tls-1-2-client)
- [Site sunucularında ve uzak site sistemlerinde TLS 1,2 'yi etkinleştirme](https://docs.microsoft.com/configmgr/core/plan-design/security/enable-tls-1-2-server)
- [TLS/SSL (Schannel SSP) protokolleri](https://docs.microsoft.com/windows-server/security/tls/manage-tls#configuring-tls-ecc-curve-order)
- [Şifre paketleri](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-12): özellikle [TLS şifre paketi sırasını yapılandırma](https://docs.microsoft.com/windows-server/security/tls/manage-tls#configuring-tls-cipher-suite-order) şu listeden geçerli şifre paketlerinizi listelediğinizden ve yok edin:

    - TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
    - TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
    - TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
    - TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384

    Kayıt defteri ayarlarını doğrudan düzenleyerek da bu şifre paketlerini ekleyebilirsiniz.

    ```azurepowershell
    New-ItemProperty -Path "$HklmSoftwarePath\Policies\Microsoft\Cryptography\Configuration\SSL\00010002" -Name "Functions"  -PropertyType String -Value ("TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384, TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384, TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384")
    ```

- Elips eğrileri ayarlama

    Şu listeden geçerli elips eğrilerini listelediğinizden ve yok edin.

    - P-256 
    - P-384

    Bu elips eğrileri de kayıt defteri ayarlarını doğrudan düzenleyerek ekleyebilirsiniz.
    
    ```azurepowershell
    New-ItemProperty -Path "$HklmSoftwarePath\Policies\Microsoft\Cryptography\Configuration\SSL\00010002" -Name "EccCurves" -PropertyType MultiString -Value @("NistP256", "NistP384")
    ```
    
    - [MIN RSA anahtar değişim boyutunu 2048 olarak ayarlayın](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#keyexchangealgorithm---client-rsa-key-sizes).



## <a name="next-steps"></a>Sonraki adımlar

[Azure Resource Manager Bağlan](azure-stack-edge-j-series-connect-resource-manager.md)