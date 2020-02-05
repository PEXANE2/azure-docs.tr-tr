---
title: Azure VMware çözümleri (AVS)-Şirket içi ve AVS özel bulutu arasında VPN 'yi yapılandırma
description: Şirket içi ağınız ve AVS özel bulutunuz arasında siteden siteye veya Noktadan siteye VPN bağlantısının nasıl yapılandırılacağını açıklar.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: fbd2b227c9292593a7652044ef4c013bf0cfaf8e
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77017012"
---
# <a name="configure-a-vpn-connection-to-your-avs-private-cloud"></a>AVS özel bulutunuz için bir VPN bağlantısı yapılandırma

VPN ağ geçitleri, AVS ağına şirket içi ağınızdan ve bir istemci bilgisayardan uzaktan bağlanmanızı sağlar. Bu makalede, AVS portalından VPN ağ geçitlerini ayarlama hakkında bilgi edinebilirsiniz. Şirket içi ağınız ve AVS ağınız arasındaki VPN bağlantısı, AVS özel bulutunuzda vCenter ve iş yüklerine erişim sağlar. AVS, Noktadan siteye VPN ve siteden siteye VPN ağ geçitlerini destekler.

## <a name="vpn-gateway-types"></a>VPN ağ geçidi türleri

* **Noktadan sıteye VPN** bağlantısı, bilgisayarınızdan AVS özel buluta bağlanmak için en kolay yoldur. AVS özel bulutuna uzaktan bağlanmak için Noktadan siteye VPN bağlantısı kullanın.
* **Siteden sıteye VPN** bağlantısı, şirket içi hizmetlere erışmek Için AVS özel bulut iş yüklerinizi ayarlamanıza olanak sağlar. Ayrıca, AVS özel bulut vCenter 'unuzda kimlik doğrulaması için şirket içi Active Directory bir kimlik kaynağı olarak kullanabilirsiniz. Şu anda, **Ilke tabanlı VPN** türü desteklenir.

Bir bölgede, bir siteden siteye VPN ağ geçidi ve bir noktadan siteye VPN ağ geçidi oluşturabilirsiniz.

## <a name="point-to-site-vpn"></a>Noktadan siteye VPN

Noktadan siteye VPN ağ geçidi oluşturmak için bkz. [noktadan sıteye VPN ağ geçidi oluşturma](vpn-gateway.md#create-point-to-site-vpn-gateway).

### <a name="connect-to-avs-using-point-to-site-vpn"></a>Noktadan siteye VPN kullanarak AVS 'ye bağlanma

Bilgisayarınızı bilgisayarınızdan AVS 'ye bağlamak için VPN istemcisi gerekir. MacOS ve OS X için Windows veya [viscosity](https://www.sparklabs.com/viscosity/download/) Için [OpenVPN istemcisini](https://openvpn.net/community-downloads/) indirin.

1. AVS portalını başlatın ve **ağ**' ı seçin.
2. **VPN Gateway**seçin.
3. VPN ağ geçitleri listesinden Noktadan siteye VPN Gateway ' e tıklayın.
4. **Kullanıcıları**seçin.
5. **VPN yapılandırması 'Nı indir** 'e tıklayın

    ![VPN yapılandırmasını indirme](media/download-p2s-vpn-configuration.png)

6. VPN istemcinizdeki yapılandırmayı içeri aktarın

    * [Windows istemcisinde yapılandırmayı içeri aktarma](https://openvpn.net/vpn-server-resources/connecting-to-access-server-with-windows/#openvpn-open-source-openvpn-gui-program) yönergeleri
    * [MacOS veya OS X üzerinde yapılandırmayı içeri aktarma](https://www.sparklabs.com/support/kb/article/getting-started-with-viscosity-mac/#creating-your-first-connection) yönergeleri

7. AVS VPN Gateway 'e bağlanın.

Aşağıdaki örnekte, **viscosity istemcisi**kullanılarak bağlantı içeri aktarma gösterilmektedir.

#### <a name="import-connection-on-viscosity-client"></a>Viscosity istemcisinde bağlantı içeri aktar

1. İndirilen. zip dosyasından VPN yapılandırmasının içeriğini ayıklayın.

2. Bilgisayarınızda viscosity 'yi açın.

3. **+** simgesine tıklayın ve bağlantı ** > dosyadan** **içeri aktar** ' ı seçin.

    ![VPN yapılandırmasını dosyadan içeri aktar](media/import-p2s-vpn-config.png)

4. Kullanmak istediğiniz protokol için OpenVPN yapılandırma dosyasını (. ovpn) seçin ve **Aç**' a tıklayın.

    ![VPN](media/import-p2s-vpn-config-choose-ovpn.png)

Bağlantı artık viscosity menüsünde görüntülenir.

#### <a name="connect-to-the-vpn"></a>VPN'e bağlanın

Viscosity OpenVPN istemcisini kullanarak VPN 'ye bağlanmak için, menüden bağlantıyı seçin. Menü simgesi, bağlantının oluşturulduğuna işaret etmek için güncellenir.

![VPN](media/vis03.png)

### <a name="connecting-to-multiple-avs-private-clouds"></a>Birden çok AVS özel bulutlarına bağlanma

Noktadan siteye VPN bağlantısı, oluşturduğunuz ilk AVS özel bulutunun DNS adlarını çözer. Diğer AVS özel bulutlarına erişmek istediğinizde, VPN istemcinizdeki DNS sunucusunu güncelleştirmeniz gerekir.

1. [AVS portalını](access-cloudsimple-portal.md)başlatın.

2. Bir **AVS özel bulutları** > **kaynaklar** ' a gıdın ve bağlanmak Istediğiniz AVS özel bulutunu seçin.

3. AVS özel bulutunun **Özet** sayfasında, **temel bilgiler**altında AVS özel bulut DNS sunucusu IP adresini kopyalayın.

    ![AVS özel bulut DNS sunucuları](media/private-cloud-dns-server.png)

4. Bilgisayarınızın sistem tepsisindeki viscosity simgesine sağ tıklayın ve **Tercihler**' i seçin.

    ![VPN](media/vis00.png)

5. AVS VPN bağlantısını seçin.

    ![VPN bağlantısı](media/viscosity-client.png)

6. Bağlantı özelliklerini değiştirmek için **Düzenle** ' ye tıklayın.

    ![VPN bağlantısını Düzenle](media/viscosity-edit-connection.png)

7. **Ağ** sekmesine tıklayın ve bir virgül ya da boşluk ve etki alanı ile ayrılmış olan AVS özel bulut DNS sunucusu ıp adreslerini ```AVS.io```olarak girin. **VPN sunucusu tarafından GÖNDERILEN DNS ayarlarını yoksay**' ı seçin.

    ![VPN ağı](media/viscosity-edit-connection-networking.png)

> [!IMPORTANT]
> İlk AVS özel bulutunuzu bağlamak için bu ayarları kaldırın ve VPN sunucusuna bağlanın.

## <a name="site-to-site-vpn"></a>Siteden siteye VPN

Siteden siteye VPN ağ geçidi oluşturmak için bkz. [siteden sıteye VPN ağ geçidi oluşturma](vpn-gateway.md#set-up-a-site-to-site-vpn-gateway). Şirket içi ağınızdan AVS özel bulutuna siteden siteye VPN bağlantısı bu avantajları sağlar. 

* Şirket içi ağınızdaki herhangi bir iş istasyonundan AVS özel bulut vCenter ' nin erişilebilirliği
* Bir vCenter Identity kaynağı olarak şirket içi Active Directory kullanımı
* VM şablonlarının, IOS 'nin ve diğer dosyaların şirket içi kaynaklarınızdan AVS özel bulut vCenter 'larınıza uygun şekilde aktarılması
* Şirket içi ağınızdan AVS özel bulutunuzda çalışan iş yüklerinin erişilebilirliği

Şirket içi VPN ağ geçidinizi yüksek kullanılabilirlik modunda ayarlamak için bkz. [yüksek kullanılabilirliğe sahıp VPN bağlantısını yapılandırma](high-availability-vpn-connection.md).

> [!IMPORTANT]
>    1. VPN cihazınızda 1200 ' de TCP ve üst sınırı ayarlayın. Ya da VPN cihazlarınız, sahip olma özelliğini desteklemiyorsa, bunun yerine tünel arabirimindeki MTU değerini 1240 bayta ayarlayabilirsiniz.
> 2. Siteden siteye VPN kurulduktan sonra, * için DNS isteklerini iletin. AVS.io, AVS özel bulut DNS sunucularına gidin. Şirket [ıçı DNS kurulumu](on-premises-dns-setup.md)'ndaki yönergeleri izleyin.
