---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 05/13/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 1d14ee849c89e6c3807636d0a728157abd9de97a
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83650668"
---
## <a name="1-download-the-file"></a>1. dosyayı indirin

Aşağıdaki komutları çalıştırın. Profil ZIP dosyasını indirmek için sonuç URL 'sini tarayıcınıza kopyalayın.

```azurepowershell-interactive
$profile = New-AzVpnClientConfiguration -ResourceGroupName AADAuth -Name AADauthGW -AuthenticationMethod "EapTls"
   
$PROFILE.VpnProfileSASUrl
```

## <a name="2-extract-the-zip-file"></a>2. zip dosyasını Ayıkla

Zip dosyasını ayıklayın. Dosya aşağıdaki klasörleri içerir:

* AzureVPN
* Genel
* OpenVPN (ağ geçidinde OpenVPN ve Azure AD kimlik doğrulaması ayarlarını etkinleştirdiyseniz). VPN Gateway için bkz. [kiracı oluşturma](../articles/vpn-gateway/openvpn-azure-ad-tenant.md). Sanal WAN için bkz. [kiracı oluşturma-VWAN](../articles/virtual-wan/openvpn-azure-ad-tenant.md).

## <a name="3-retrieve-information"></a>3. bilgileri alma

**AzureVPN** klasöründe, ***azurevpnconfig. xml*** dosyasına gidin ve Not defteri ile açın. Aşağıdaki Etiketler arasındaki metni bir yere unutmayın.

```
<audience>          </audience>
<issuer>            </issuer>
<tennant>           </tennant>
<fqdn>              </fqdn>
<serversecret>      </serversecret>
```

## <a name="profile-details"></a>Profil ayrıntıları

Bir bağlantı eklediğinizde, profil ayrıntıları sayfası için önceki adımda topladığınız bilgileri kullanın. Alanlar aşağıdaki bilgilere karşılık gelir:

   * **Hedef kitle:** Belirtecin hedeflenen alıcı kaynağını tanımlar
   * **Veren:** Belirteci oluşturan güvenlik belirteci hizmetini (STS) ve Azure AD kiracısını tanımlar
   * **Kiracı:** Belirteci veren dizin kiracının sabit, benzersiz bir tanımlayıcısını içerir
   * **FQDN:** Azure VPN Gateway 'de tam etki alanı adı (FQDN)
   * **Sunucusecret:** VPN Gateway önceden paylaşılan anahtarı

## <a name="folder-contents"></a>Klasör içeriği

* **Genel klasör** , ortak sunucu sertifikasını ve vpnsettings. xml dosyasını içerir. VpnSettings. xml dosyası, genel bir istemciyi yapılandırmak için gereken bilgileri içerir.

* İndirilen ZIP dosyası, **WindowsAmd64** ve **WindowsX86** klasörlerini de içerebilir. Bu klasörler, Windows istemcileri için SSTP ve Ikev2 için yükleyiciyi içerir. İstemci üzerinde yönetici haklarına sahip olmanız gerekir.
