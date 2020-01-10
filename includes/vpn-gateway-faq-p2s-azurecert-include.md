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
ms.sourcegitcommit: c32050b936e0ac9db136b05d4d696e92fefdf068
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75752119"
---
[!INCLUDE [P2S FAQ All](vpn-gateway-faq-p2s-all-include.md)]

### <a name="what-should-i-do-if-im-getting-a-certificate-mismatch-when-connecting-using-certificate-authentication"></a>Sertifika kimlik doğrulamasını kullanarak bağlanırken bir sertifika uyumsuzluğu alıyorsanız ne yapmam gerekir?

**"Sertifikayı doğrulayarak sunucu kimliğini doğrula"** işaretini kaldırın veya el ile profil oluştururken **SERTIFIKAYLA birlikte sunucu FQDN 'sini ekleyin** . Bunu komut isteminden **RASPHONE** çalıştırarak ve açılan listeden profili seçerek yapabilirsiniz.

Sunucu kimlik doğrulamasını atlama, genel olarak önerilmez, ancak Azure sertifika kimlik doğrulaması ile, VPN Tünel Protokolü (Ikev2/SSTP) ve EAP protokolünde sunucu doğrulaması için aynı sertifika kullanılır. Sunucu sertifikası ve FQDN, VPN tünel oluşturma protokolü tarafından zaten doğrulandığından, EAP 'de yeniden doğrulanması gereksizdir.

![Noktadan siteye](./media/vpn-gateway-faq-p2s-all-include/servercert.png "Sunucu sertifikası")

### <a name="can-i-use-my-own-internal-pki-root-ca-to-generate-certificates-for-point-to-site-connectivity"></a>Noktadan siteye bağlantı için sertifika oluşturmak üzere kendi iç PKI kök sertifika yetkilimi kullanabilir miyim?

Evet. Önceden, yalnızca otomatik olarak imzalanan kök sertifikalar kullanılabiliyordu. 20 kök sertifika yükleyebilirsiniz.

### <a name="can-i-use-certificates-from-azure-key-vault"></a>Azure Key Vault sertifikaları kullanabilir miyim?

Hayır.

### <a name="what-tools-can-i-use-to-create-certificates"></a>Sertifikaları oluşturmak için hangi araçları kullanabilirim?

Kurumsal PKI çözümünüzü (dahili PKI'nizi), Azure PowerShell'i, MakeCert'i ve OpenSSL'yi kullanabilirsiniz.

### <a name="certsettings"></a>Sertifika ayarları ve parametreler için yönergeler var mı?

* **Dahili PKI/Kurumsal PKI çözümü:** [Sertifikaları oluşturma](../articles/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert) adımlarına bakın.

* **Azure PowerShell:** Adımlar için [Azure PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md) makalesine bakın.

* **MakeCert:** Adımlar için [MakeCert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md) makalesine bakın.

* **OpenSSL:** 

    * Sertifikaları dışarı aktarırken kök sertifikayı Base64'e dönüştürdüğünüzden emin olun.

    * İstemci sertifikası için:

      * Özel anahtarı oluştururken uzunluğu 4096 olarak belirtin.
      * Sertifikayı oluştururken, *-extensions* parametresini *usr_cert* olarak belirtin.
