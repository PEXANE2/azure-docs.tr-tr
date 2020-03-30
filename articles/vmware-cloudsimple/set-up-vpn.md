---
title: CloudSimple tarafından Azure VMware Çözümü - Şirket içi ve Özel Bulut arasında VPN yapılandırın
description: Şirket içi ağınızla CloudSimple Private Cloud'unuz arasında Siteden Siteye veya Site'ye VPN bağlantısının nasıl yapılandırılabildiğini açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: d000d8390375466232c7daac2a4a056ef424be79
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77087133"
---
# <a name="configure-a-vpn-connection-to-your-cloudsimple-private-cloud"></a>CloudSimple Private Cloud'unuza VPN bağlantısı yapılandırma

VPN ağ geçitleri, cloudsimple ağına şirket içi ağınızdan ve bir istemci bilgisayarından uzaktan bağlanmanızı sağlar.  Bu makalede, CloudSimple portalından VPN ağ geçitleri kurma hakkında bilgi bulabilirsiniz.  Şirket içi ağınızla CloudSimple ağınız arasındaki VPN bağlantısı, vCenter'a ve Özel Bulut'unuzdaki iş yüklerine erişim sağlar. CloudSimple hem Noktadan Siteye VPN'i hem de Siteden Siteye VPN ağ geçitlerini destekler.

## <a name="vpn-gateway-types"></a>VPN ağ geçidi türleri

* **Site'ye noktadan kullanıcıya VPN** bağlantısı, Özel Bulut'unuza bilgisayarınızdan bağlanmanın en basit yoludur. Özel Bulut'a uzaktan bağlanmak için Site'ye Noktavpn bağlantısını kullanın.
* **Siteden Siteye VPN** bağlantısı, Şirket içi hizmetlere erişmek için Özel Bulut iş yüklerinizi ayarlamanıza olanak tanır. Ayrıca, Özel Bulut vCenter'ınıza kimlik doğrulama yapmak için şirket içi Active Directory'yi kimlik kaynağı olarak da kullanabilirsiniz.  Şu anda, **İlke Tabanlı VPN** türü desteklenir.

Bir bölgede, bir Siteden Siteye VPN ağ geçidi ve bir Noktadan Siteye VPN ağ geçidi oluşturabilirsiniz.

## <a name="point-to-site-vpn"></a>Noktadan Siteye VPN

Bir Noktaya Sayfa VPN ağ geçidi oluşturmak için [bkz.](vpn-gateway.md#create-point-to-site-vpn-gateway)

### <a name="connect-to-cloudsimple-using-point-to-site-vpn"></a>Siteye Göre VPN kullanarak CloudSimple'a bağlanın

Bilgisayarınızdan CloudSimple'a bağlanmak için VPN istemcisi gereklidir.  macOS ve OS X için Windows veya [Viskozite](https://www.sparklabs.com/viscosity/download/) için [OpenVPN istemcisini](https://openvpn.net/community-downloads/) indirin.

1. CloudSimple portalını başlatın ve **Ağ'ı**seçin.
2. **VPN Ağ Geçidi'ni**seçin.
3. VPN ağ geçitleri listesinden, Siteye Giriş VPN ağ geçidine tıklayın.
4. **Kullanıcılar**’ı seçin.
5. VPN **yapılandırmamı indir'e** tıklayın

    ![VPN yapılandırmasını indirme](media/download-p2s-vpn-configuration.png)

6. VPN istemcinizde yapılandırmayı alma

    * Windows [istemcisi üzerinde yapılandırma alma](https://openvpn.net/vpn-server-resources/connecting-to-access-server-with-windows/#openvpn-open-source-openvpn-gui-program) yönergeleri
    * [macOS veya OS X'te yapılandırma alma](https://www.sparklabs.com/support/kb/article/getting-started-with-viscosity-mac/#creating-your-first-connection) yönergeleri

7. CloudSimple VPN ağ geçidine bağlanın.

Aşağıdaki **örnekviskozite istemcisi**kullanarak bağlantı alma gösterir.

#### <a name="import-connection-on-viscosity-client"></a>Viskozite istemcisi üzerinde alma bağlantısı

1. Vpn yapılandırmasının içeriğini indirilen .zip dosyasından ayıklayın.

2. Bilgisayarınızda Viskozite'yi açın.

3. Simgeyi **+** tıklatın ve**Dosyadan** **Bağlantı** > Aktar'ı seçin.

    ![Dosyadan VPN yapılandırmasını alma](media/import-p2s-vpn-config.png)

4. Kullanmak istediğiniz protokol için OpenVPN yapılandırma dosyasını (.ovpn) seçin ve **Aç'ı**tıklatın.

    ![VPN](media/import-p2s-vpn-config-choose-ovpn.png)

Bağlantı artık Viskozite menüsünde görünür.

#### <a name="connect-to-the-vpn"></a>VPN'e bağlanın

Viscosity OpenVPN istemcisini kullanarak VPN'e bağlanmak için menüden bağlantıyı seçin. Bağlantının kurulduğunu belirtmek için menü simgesi güncellenir.

![VPN](media/vis03.png)

### <a name="connecting-to-multiple-private-clouds"></a>Birden Çok Özel Bulutlara Bağlanma

Site'ye Noktavpn bağlantısı, oluşturduğunuz ilk Özel Bulut'un DNS adlarını çözer. Diğer Özel Bulutlara erişmek istediğinizde, VPN istemcinizdeki DNS sunucusunu güncelleştirmeniz gerekir.

1. [CloudSimple portalına](access-cloudsimple-portal.md)başlatın.

2. Özel **Kaynaklar** > **Bulutları'na** gidin ve bağlanmak istediğiniz Özel Bulut'u seçin.

3. Private Cloud'un **Özet** sayfasında, Özel Bulut DNS sunucu IP adresini **Temel Bilgiler**altında kopyalayın.

    ![Özel Bulut DNS sunucuları](media/private-cloud-dns-server.png)

4. Bilgisayarınızın sistem tepsisindeki Viskozite simgesine sağ tıklayın ve **Tercihler'i**seçin.

    ![VPN](media/vis00.png)

5. CloudSimple VPN bağlantısını seçin.

    ![VPN Bağlantısı](media/viscosity-client.png)

6. Bağlantı özelliklerini değiştirmek için **Edit'i** tıklatın.

    ![VPN Bağlantısını Edin](media/viscosity-edit-connection.png)

7. **Ağ** sekmesini tıklatın ve virgül veya boşluk ve etki alanı ile ```cloudsimple.io```ayrılmış Özel Bulut DNS sunucusu IP adreslerini girin.  **VPN sunucusu tarafından gönderilen DNS ayarlarını yoksay'ı**seçin.

    ![VPN Ağ](media/viscosity-edit-connection-networking.png)

> [!IMPORTANT]
> İlk Özel Bulut'unuza bağlanmak için bu ayarları kaldırın ve VPN sunucusuna bağlanın.

## <a name="site-to-site-vpn"></a>Siteler arası VPN

Siteden Siteye VPN ağ geçidi oluşturmak için [bkz.](vpn-gateway.md#set-up-a-site-to-site-vpn-gateway)  Şirket içi ağınızdan Özel Bulut'unuza site-to-Site VPN bağlantısı bu avantajları sağlar.  

* Özel Bulut vCenter'ınızın şirket içi ağınızdaki herhangi bir iş istasyonundan erişilebilirliği
* Şirket içi Active Directory'nizi vCenter kimlik kaynağı olarak kullanma
* Şirket içi kaynaklarınızdan Özel Bulut vCenter'ınıza VM şablonlarının, ISO'larının ve diğer dosyaların rahat çalarından aktarılması
* Şirket içi ağınızdan Özel Bulut'unuzda çalışan iş yüklerinin erişilebilirliği

Şirket içi VPN ağ geçidinizi yüksek kullanılabilirlik modunda kurmak için [bkz.](high-availability-vpn-connection.md)

> [!IMPORTANT]
>    1. VPN cihazınızda TCP MSS Bağlamayı 1200 olarak ayarlayın. Veya VPN aygıtlarınız MSS bağlamayı desteklemiyorsa, tünel arabirimindeki MTU'yu alternatif olarak 1240 bayt olarak ayarlayabilirsiniz.
> 2. Siteden Siteye VPN kurulduktan sonra, *.cloudsimple.io için DNS isteklerini Özel Bulut DNS sunucularına iletin.  Şirket Içi [DNS Kurulumu'ndaki](on-premises-dns-setup.md)yönergeleri izleyin.
