---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/14/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: f322803d3484b4ec2d5449e19d67d75b35d6d92f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75752119"
---
[!INCLUDE [P2S FAQ All](vpn-gateway-faq-p2s-all-include.md)]

### <a name="what-should-i-do-if-im-getting-a-certificate-mismatch-when-connecting-using-certificate-authentication"></a>Sertifika kimlik doğrulaması kullanarak bağlanırken sertifika uyuşmazlığı alıyorsam ne yapmalıyım?

**"Sertifikayı doğrulayarak sunucunun kimliğini doğrulayın"** veya el ile profil oluştururken **sunucu FQDN'yi sertifikayla birlikte ekleyin.** Bunu, bir komut isteminden **rasphone** çalıştırarak ve açılan listeden profili seçerek yapabilirsiniz.

Sunucu kimlik doğrulamasını atlayarak genel olarak önerilmez, ancak Azure sertifikası kimlik doğrulaması ile aynı sertifika VPN tünelleme protokolünde (IKEv2/SSTP) ve EAP protokolünde sunucu doğrulaması için kullanılır. Sunucu sertifikası ve FQDN vpn tünelleme protokolü tarafından zaten doğrulandığı için, EAP'de aynı şeyi yeniden doğrulamak gereksizdir.

![noktadan siteye](./media/vpn-gateway-faq-p2s-all-include/servercert.png "Sunucu Sertifikası")

### <a name="can-i-use-my-own-internal-pki-root-ca-to-generate-certificates-for-point-to-site-connectivity"></a>Site'ye Nokta bağlantısı için sertifikalar oluşturmak için kendi dahili PKI kök CA'mı kullanabilir miyim?

Evet. Önceden, yalnızca otomatik olarak imzalanan kök sertifikalar kullanılabiliyordu. 20 kök sertifika yükleyebilirsiniz.

### <a name="can-i-use-certificates-from-azure-key-vault"></a>Azure Key Vault'un sertifikalarını kullanabilir miyim?

Hayır.

### <a name="what-tools-can-i-use-to-create-certificates"></a>Sertifikaları oluşturmak için hangi araçları kullanabilirim?

Kurumsal PKI çözümünüzü (dahili PKI'nizi), Azure PowerShell'i, MakeCert'i ve OpenSSL'yi kullanabilirsiniz.

### <a name="are-there-instructions-for-certificate-settings-and-parameters"></a><a name="certsettings"></a>Sertifika ayarları ve parametreler için yönergeler var mı?

* **Dahili PKI/Kurumsal PKI çözümü:**[Sertifikaları oluşturma](../articles/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert) adımlarına bakın.

* **Azure PowerShell:** Adımlar için [Azure PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md) makalesine bakın.

* **MakeCert:** Adımlar için [MakeCert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md) makalesine bakın.

* **Openssl:** 

    * Sertifikaları dışarı aktarırken kök sertifikayı Base64'e dönüştürdüğünüzden emin olun.

    * İstemci sertifikası için:

      * Özel anahtarı oluştururken uzunluğu 4096 olarak belirtin.
      * Sertifikayı oluştururken, *-extensions* parametresini *usr_cert* olarak belirtin.
