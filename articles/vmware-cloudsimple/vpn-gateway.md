---
title: CloudSimple tarafından Azure VMware Çözümü - VPN ağ geçidi ayarlayın
description: Site'ye Nokta VPN ağ geçidini ve Siteden Siteye VPN ağ geçidini nasıl kurup şirket içi ağınızla CloudSimple Private Cloud'unuz arasında bağlantılar oluşturabilirsiniz
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: a8b7e238333196381524d189904871fe5933c906
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79279500"
---
# <a name="set-up-vpn-gateways-on-cloudsimple-network"></a>CloudSimple ağında VPN ağ geçitleri ayarlama

VPN ağ geçitleri, cloudsimple ağına şirket içi ağınızdan ve bir istemci bilgisayarından uzaktan bağlanmanızı sağlar. Şirket içi ağınızla CloudSimple ağınız arasındaki VPN bağlantısı, vCenter'a ve Özel Bulut'unuzdaki iş yüklerine erişim sağlar. CloudSimple hem Siteden Siteye VPN'i hem de Site'den Siteye VPN ağ geçitlerini destekler.

## <a name="vpn-gateway-types"></a>VPN ağ geçidi türleri

* **Siteden Siteye VPN** bağlantısı, Şirket içi hizmetlere erişmek için Özel Bulut iş yüklerinizi ayarlamanıza olanak tanır. Ayrıca, Özel Bulut vCenter'ınıza kimlik doğrulama yapmak için şirket içi Active Directory'yi kimlik kaynağı olarak da kullanabilirsiniz.  Şu anda yalnızca **İlke Tabanlı VPN** türü desteklenir.
* **Site'ye noktadan kullanıcıya VPN** bağlantısı, Özel Bulut'unuza bilgisayarınızdan bağlanmanın en basit yoludur. Özel Bulut'a uzaktan bağlanmak için Site'ye Nokta vpn bağlantısını kullanın. Bir Noktaya Sayfa VPN bağlantısı için istemci yükleme hakkında daha fazla bilgi için [bkz.](set-up-vpn.md)

Bir bölgede, bir Noktadan Siteye VPN ağ geçidi ve bir Siteden Siteye VPN ağ geçidi oluşturabilirsiniz.

## <a name="automatic-addition-of-vlansubnets"></a>VLAN/alt ağların otomatik olarak eklenmesi

CloudSimple VPN ağ geçitleri, VPN ağ geçitlerine VLAN/alt ağ ağları eklemek için ilkeler sağlar.  İlkeler, yönetim VLAN/alt ağları ve kullanıcı tanımlı VLAN/alt ağları için farklı kurallar belirtmenize olanak sağlar.  Yönetim Kuralları VLAN/alt ağları oluşturduğunuz tüm yeni Özel Bulutlar için geçerlidir.  Kullanıcı tanımlı VLAN'lar/alt ağlar için kurallar, siteden siteye VPN ağ geçidi için varolan veya yeni Özel Bulutlara otomatik olarak yeni VLAN/alt ağlar eklemenize olanak tanır, her bağlantı için ilkeyi tanımlarsınız.

VPN ağ geçitlerine VLAN'ler/alt ağlar ekleme politikaları hem Siteden Siteye VPN hem de Site'den Siteye VPN ağ geçitlerine uygulanır.

## <a name="automatic-addition-of-users"></a>Otomatik kullanıcı eklemesi

Bir Noktadan Siteye VPN ağ geçidi, yeni kullanıcılar için otomatik bir ekleme ilkesi tanımlamanıza olanak tanır. Varsayılan olarak, tüm sahipleri ve abonelik katkıda Bulunanlar CloudSimple portalına erişebilir.  Kullanıcılar yalnızca CloudSimple portalı ilk kez başlatıldığında oluşturulur.  Otomatik **ekleme** kurallarının seçilmesi, herhangi bir yeni kullanıcının Site'ye Puanlı VPN bağlantısını kullanarak CloudSimple ağına erişmesini sağlar.

## <a name="set-up-a-site-to-site-vpn-gateway"></a>Siteden Siteye VPN ağ geçidi ayarlama

1. [CloudSimple portalına erişin](access-cloudsimple-portal.md) ve **Ağ'ı**seçin.
2. **VPN Ağ Geçidi'ni**seçin.
3. **Yeni VPN Ağ Geçidi'ni**tıklatın.

    ![VPN ağ geçidi oluşturma](media/create-vpn-gateway.png)

4. **Ağ Geçidi yapılandırması**için aşağıdaki ayarları belirtin ve **İleri'yi**tıklatın.

    * Ağ geçidi türü olarak **Site'den Siteye VPN'i** seçin.
    * Ağ geçidini tanımlamak için bir ad girin.
    * CloudSimple hizmetinizin dağıtıldığı Azure konumunu seçin.
    * İsteğe bağlı olarak, Yüksek Kullanılabilirlik'i etkinleştirin.

    ![Siteden Siteye VPN ağ geçidi oluşturma](media/create-vpn-gateway-s2s.png)

    > [!WARNING]
    > Yüksek Kullanılabilirliği etkinleştirmek için şirket içi VPN cihazınızın iki IP adresine bağlanmayı desteklemesi gerekir. VPN ağ geçidi dağıtıldıktan sonra bu seçenek devre dışı tutulamaz.

5. Şirket içi ağınızdan ilk bağlantıyı oluşturun ve **İleri'yi**tıklatın.

    * Bağlantıyı tanımlamak için bir ad girin.
    * Eş IP için şirket içi VPN ağ geçidinizin genel IP adresini girin.
    * Şirket içi VPN ağ geçidinizin eş tanımlayıcısını girin.  Eş tanımlayıcısı genellikle şirket içi VPN ağ geçidinizin genel IP adresidir.  Ağ geçidinizde belirli bir tanımlayıcıyı yapılandırmışsanız, tanımlayıcıyı girin.
    * Şirket içi VPN ağ geçidinizden bağlantı için kullanmak üzere paylaşılan anahtarı kopyalayın.  Varsayılan paylaşılan anahtarı değiştirmek ve yeni bir anahtar belirtmek için, edit simgesini tıklatın.
    * **Şirket Içi Önekleri**için, CloudSimple ağına erişecek şirket içi CIDR önekleri girin.  Bağlantıyı oluştururken birden çok CIDR öneki ekleyebilirsiniz.

    ![Siteden Siteye VPN ağ geçidi bağlantısı oluşturma](media/create-vpn-gateway-s2s-connection.png)

6. Şirket içi ağdan erişilecek Olan Özel Bulut ağınızdaki VLAN/alt ağlarını etkinleştirin ve **İleri'yi**tıklatın.

    * Bir yönetim VLAN/subnet eklemek **için, Özel Bulutların Yönetim VLAN'larını/Alt Ağlarını Ekle'yi**etkinleştirin.  VMotion ve vSAN alt ağları için yönetim alt ağı gereklidir.
    * vMotion alt ağları eklemek için, **Özel Bulutların vMotion ağ ekle'sini etkinleştirin.**
    * vSAN alt ağları eklemek için **Özel Bulutlar vSAN alt netini ekleyin'i**etkinleştirin.
    * Belirli VLAN'ları seçin veya seçin.

    ![Bağlantı oluşturma](media/create-vpn-gateway-s2s-connection-vlans.png)

7. Ayarları gözden geçirin ve **Gönder'e**tıklayın.

    ![Siteden Siteye VPN ağ geçidi incelemesi ve oluşturma](media/create-vpn-gateway-s2s-review.png)

## <a name="create-point-to-site-vpn-gateway"></a>Noktadan Siteye VPN ağ geçidi oluşturma

1. [CloudSimple portalına erişin](access-cloudsimple-portal.md) ve **Ağ'ı**seçin.
2. **VPN Ağ Geçidi'ni**seçin.
3. **Yeni VPN Ağ Geçidi'ni**tıklatın.

    ![VPN ağ geçidi oluşturma](media/create-vpn-gateway.png)

4. **Ağ Geçidi yapılandırması**için aşağıdaki ayarları belirtin ve **İleri'yi**tıklatın.

    * Ağ geçidi türü olarak **Site'ye Nokta VPN'i** seçin.
    * Ağ geçidini tanımlamak için bir ad girin.
    * CloudSimple hizmetinizin dağıtıldığı Azure konumunu seçin.
    * Siteye Giriş ağ geçidi için istemci alt netini belirtin.  Bağlandığınızda istemci alt netinden DHCP adresleri verilecektir.

5. **Bağlantı/Kullanıcı**için aşağıdaki ayarları belirtin ve **İleri'yi**tıklatın.

    * Tüm mevcut ve gelecekteki kullanıcıların Siteye Nokta ağ geçidinden Özel Bulut'a otomatik olarak erişmesine izin vermek **için, tüm kullanıcıları otomatik olarak ekleyin'i**seçin. Seçeneği seçtiğinizde, kullanıcı listesindeki tüm kullanıcılar otomatik olarak seçilir. Listedeki tek tek kullanıcıları seçerek otomatik seçeneği geçersiz kılabilirsiniz.
    * Tek tek kullanıcıları seçmek için, kullanıcı listesindeki onay kutularını tıklatın.

6. VN'ler/Alt Ağlar bölümü, ağ geçidi ve bağlantılar için yönetim ve kullanıcı VLAN'leri/alt ağlarını belirtmenize olanak tanır.

    * **Otomatik olarak ekleme** seçenekleri ağ geçidi için genel ilkeyi ayarlar. Ayarlar geçerli ağ geçidine uygulanır. Ayarlar **Seç** alanında geçersiz kılınabilir.
    * **Özel Bulutların Yönetim VLAN'leri/Alt Ağlarını Ekle'yi**seçin. 
    * Tüm kullanıcı tanımlı VLAN'lar/alt ağlar eklemek **için, kullanıcı tanımlı VLAN/Subnet ekle'yi**tıklatın.
    * **Select** **ayarları, Otomatik olarak ekle'nin**altındaki genel ayarları geçersiz kılar.

7. Ayarları gözden geçirmek için **İleri'yi** tıklatın. Değişiklik yapmak için Simgeleri Edit'i tıklatın.
8. VPN ağ geçidini oluşturmak için **Oluştur'u** tıklatın.

### <a name="client-subnet-and-protocols-for-point-to-site-vpn-gateway"></a>Site noktası VPN ağ geçidi için istemci alt ağı ve protokolleri

Noktadan Siteye VPN ağ geçidi TCP ve UDP bağlantılarına izin verir.  TCP veya UDP yapılandırmasını seçerek bilgisayarınızdan bağlandığınızda kullanılacak protokolü seçin.

Yapılandırılan istemci alt ağı hem TCP hem de UDP istemcileri için kullanılır.  CIDR öneki, biri TCP, diğeri UDP istemcileri olmak üzere iki alt ağlara ayrılmıştır. Aynı anda bağlanacak VPN kullanıcılarının sayısına göre önek maskesini seçin.  

Aşağıdaki tabloda önek maskesi için eşzamanlı istemci bağlantı sayısı listelenir.

| Önek Maskesi | /24 | /25 | /26 | /27 | /28 |
|-------------|-----|-----|-----|-----|-----|
| Eşzamanlı TCP bağlantı sayısı | 124 | 60 | 28 | 12 | 4 |
| Eşzamanlı UDP bağlantı sayısı | 124 | 60 | 28 | 12 | 4 |

Yer-To-Site VPN kullanarak bağlanmak için [bkz.](set-up-vpn.md#connect-to-cloudsimple-using-point-to-site-vpn)
