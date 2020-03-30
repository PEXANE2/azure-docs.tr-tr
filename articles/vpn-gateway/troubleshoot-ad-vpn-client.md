---
title: 'VPN Ağ Geçidi: Sorun Giderme VPN istemcisi - Azure AD kimlik doğrulaması'
description: Sorun Giderme VPN Ağ Geçidi P2S Azure AD kimlik doğrulama istemcileri
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: cherylmc
ms.openlocfilehash: 8871e92f0911c4d3cbcc1772bef1daeb5c70b5d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74151964"
---
# <a name="troubleshoot-an-azure-ad-authentication-vpn-client"></a>Azure AD kimlik doğrulama VPN istemcisi sorun giderme

Bu makale, Bir SANAL ağa bağlanmak için bir VPN istemcisini sorun gidermenize yardımcı olur ve Sayfa-To-Site VPN kimlik doğrulaması.

## <a name="view-status-log"></a><a name="status"></a>Durum Günlüğünü Görüntüle

Hata iletileri için durum günlüğünü görüntüleyin.

![günlükler](./media/troubleshoot-ad-vpn-client/1.png)

1. **Durum Günlüklerini**göstermek için istemci penceresinin sağ alt köşesindeki oklar simgesini tıklatın.
2. Sorunu gösteren hatalar için günlükleri denetleyin.
3. Hata iletileri kırmızı renkte görüntülenir.

## <a name="clear-sign-in-information"></a><a name="clear"></a>Oturum açma bilgilerini temizle

Oturum açma bilgilerini temizleyin.

![oturum aç](./media/troubleshoot-ad-vpn-client/2.png)

1. Seçin ... sorun gidermek istediğiniz profilin yanında. **Yapıla -> Kaydedilmiş Hesabı**Nı Seçin.
2. **Kaydet'i**seçin.
3. Bağlanmayı deneyin.
4. Bağlantı hala başarısız olursa, sonraki bölüme devam edin.

## <a name="run-diagnostics"></a><a name="diagnostics"></a>Tanılama çalıştırma

VPN istemcisi üzerinde tanılama çalıştırın.

![tanılama](./media/troubleshoot-ad-vpn-client/3.png)

1. Tıklayın **...** üzerinde tanılama çalıştırmak istediğiniz profilin yanında. **Tanı -> Çalıştır Tanı'yı**seçin.
2. İstemci bir dizi test çalıştıracak ve testin sonucunu görüntüleyecek

   * Internet Erişimi – İstemcinin Internet bağlantısı olup olmadığını denetler
   * İstemci Kimlik Bilgileri – Azure Etkin Dizin kimlik doğrulama bitiş noktasına erişilip erişilemeyip erişilemeyini denetleyin
   * Server Resolvable – Yapılandırılan VPN sunucusunun IP adresini çözmek için DNS sunucusuyla bağlantı kurun
   * Sunucuya Erişilebilir – VPN sunucusunun yanıt verilip vermediğini kontrol eder
3. Sınamalardan herhangi biri başarısız olursa, sorunu gidermek için ağ yöneticinize başvurun.
4. Sonraki bölümde, gerekirse günlükleri nasıl toplayabileceğinizi gösterir.

## <a name="collect-client-log-files"></a><a name="logfiles"></a>İstemci günlük dosyalarını toplama

VPN istemci günlüğü dosyalarını toplayın. Günlük dosyaları seçtiğiniz bir yöntemle destek/yöneticiye gönderilebilir. Örneğin, e-posta.

1. "..." seçeneğini tıklayın. üzerinde tanılama çalıştırmak istediğiniz profilin yanında. **Tanıla -> Günlükleri Göster Dizinini**seçin.

   ![günlükleri göster](./media/troubleshoot-ad-vpn-client/4.png)
2. Windows Gezgini günlük dosyalarını içeren klasöre açılır.

   ![dosyayı görüntüleme](./media/troubleshoot-ad-vpn-client/5.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [bkz.](openvpn-azure-ad-tenant.md)