---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/17/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: c61378510fbfc8bdc13f35ba1063a0d9316d88e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066202"
---
## <a name="1-download-the-file"></a>1. Dosyayı indirin

Aşağıdaki komutları çalıştırın. Profil zip dosyasını indirmek için sonuç URL'sini tarayıcınıza kopyalayın.

```azurepowershell-interactive
$profile = New-AzVpnClientConfiguration -ResourceGroupName AADAuth -Name AADauthGW -AuthenticationMethod "EapTls"
   
$PROFILE.VpnProfileSASUrl
```

## <a name="2-extract-the-zip-file"></a>2. Zip dosyasını ayıklayın

Zip dosyasını ayıklayın. Dosya aşağıdaki klasörleri içerir:

* AzureVPN
* Genel
* OpenVPN (Ağ geçidindeki OpenVPN ve Azure AD kimlik doğrulama ayarlarını etkinleştirdiyseniz. VPN Ağ Geçidi için [bkz.](../articles/vpn-gateway/openvpn-azure-ad-tenant.md) Sanal WAN için bkz: [Kiracı oluştur - VWAN.](../articles/virtual-wan/openvpn-azure-ad-tenant.md)

## <a name="3-retrieve-information"></a>3. Bilgi alma

**AzureVPN** klasöründe ***azurevpnconfig.xml*** dosyasına gidin ve Notepad ile açın. Aşağıdaki etiketler arasındaki metnin notunu yapın.

```
<audience>          </audience>
<issuer>            </issuer>
<tennant>           </tennant>
<fqdn>              </fqdn>
<serversecret>      </serversecret>
```

## <a name="profile-details"></a>Profil detayları

Bağlantı eklediğinizde, profil ayrıntıları sayfası için önceki adımda topladığınız bilgileri kullanın. Alanlar aşağıdaki bilgilere karşılık gelir:

   * **Hedef Kitle:** Belirteç için tasarlanan alıcı kaynağı tanımlar
   * **İhraççı:** Belirteci ve Azure AD kiracısını yayan Güvenlik Belirteci Hizmetini (STS) tanımlar
   * **Kiracı:** Belirteci veren dizin kiracısının değişmez, benzersiz tanımlayıcısını içerir
   * **FQDN:** Azure VPN ağ geçidindeki tam nitelikli alan adı (FQDN)
   * **ServerSecret:** VPN ağ geçidi önceden paylaşılan anahtar

## <a name="folder-contents"></a>Klasör içeriği

* **Genel klasör,** ortak sunucu sertifikasını ve VpnSettings.xml dosyasını içerir. VpnSettings.xml dosyası, genel bir istemciyi yapılandırmak için gereken bilgileri içerir.

* İndirilen zip dosyası **windowsamd64** ve **WindowsX86** klasörleri de içerebilir. Bu klasörler Windows istemcileri için SSTP ve IKEv2 için yükleyici içerir. Bunları yüklemek için istemci üzerinde yönetici hakları gerekir.