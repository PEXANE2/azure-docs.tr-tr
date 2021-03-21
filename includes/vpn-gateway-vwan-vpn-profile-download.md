---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/08/2021
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: f3eb2d9469ab3a3d2c1d09e4adc3ee2cb1f86e6e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "99979082"
---
## <a name="extract-the-zip-file"></a>ZIP dosyasını Ayıkla

Zip dosyasını ayıklayın. Dosya aşağıdaki klasörleri içerir:

* AzureVPN
* Genel
* OpenVPN ( **Azure sertifikası** Ile OpenVPN 'i veya ağ geçidinde **RADIUS kimlik doğrulama** ayarlarını etkinleştirdiyseniz). Kiracı oluşturmak için yapılandırmanıza karşılık gelen ilgili makaleyi seçin.

  * [VPN Gateway-kiracı oluşturun](../articles/vpn-gateway/openvpn-azure-ad-tenant.md).
  * [Sanal WAN-kiracı oluşturun](../articles/virtual-wan/openvpn-azure-ad-tenant.md).

## <a name="retrieve-information"></a>Bilgi alma

**AzureVPN** klasöründe **_azurevpnconfig.xml_** dosyasına gidin ve Not defteri ile açın. Aşağıdaki Etiketler arasındaki metni bir yere unutmayın.

```
<audience>          </audience>
<issuer>            </issuer>
<tennant>           </tennant>
<fqdn>              </fqdn>
<serversecret>      </serversecret>
```

## <a name="profile-details"></a>Profil ayrıntıları

Bir bağlantı eklediğinizde, profil ayrıntıları sayfası için önceki adımda topladığınız bilgileri kullanın. Alanlar aşağıdaki bilgilere karşılık gelir:

* **Hedef kitle:** Belirtecin hedeflenen alıcı kaynağını tanımlar.
* **Veren:** Belirteci oluşturan güvenlik belirteci hizmetini (STS) ve Azure AD kiracısını tanımlar.
* **Kiracı:** Belirteci veren dizin kiracının sabit, benzersiz bir tanımlayıcısını içerir.
* **FQDN:** Azure VPN ağ geçidinde tam etki alanı adı (FQDN).
* **Sunucusecret:** VPN Gateway önceden paylaşılan anahtarı.

## <a name="folder-contents"></a>Klasör içeriği

* **Genel klasör** , ortak sunucu sertifikasını ve VpnSettings.xml dosyasını içerir. VpnSettings.xml dosyası bir genel istemciyi yapılandırmak için gereken bilgileri içerir.

* İndirilen ZIP dosyası, **WindowsAmd64** ve **WindowsX86** klasörlerini de içerebilir. Bu klasörler, Windows istemcileri için SSTP ve Ikev2 için yükleyiciyi içerir. İstemci üzerinde yönetici haklarına sahip olmanız gerekir.
