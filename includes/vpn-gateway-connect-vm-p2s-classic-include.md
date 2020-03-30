---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/06/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 6d0737a7300b2a6025f776c1ed65a05cacf2141a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188237"
---
VNet'inize dağıtılan bir VM'ye bağlanmak için Uzak Masaüstü Bağlantısı oluşturun. VM'nize bağlanabileceğinizi doğrulamanın en iyi yolu, bilgisayar adı yerine özel IP adresiyle bağlantı kurmaktır. Bu şekilde, ad çözümlemesi düzgün yapılandırılıp yapılandırılmadığını değil, bağlanıp bağlanamayacağınızı görmek için sınamış olursunuz. 

1. Sanal makinenizin özel IP adresini bulun. Bir VM'nin özel IP adresini bulmak için Azure portalındaki VM özelliklerini görüntüleyin veya PowerShell'i kullanın.
2. VNet'inize Yer Noktada VPN bağlantısıyla bağlı olduğunuzu doğrulayın. 
3. Uzak Masaüstü Bağlantısı'nı açmak için görev çubuğundaki arama kutusuna *RDP* veya *Uzak Masaüstü Bağlantısı* girin ve ardından Uzak Masaüstü **Bağlantısı'nı**seçin. PowerShell'deki **mstsc** komutunu kullanarak da açabilirsiniz. 
3. **Uzak Masaüstü Bağlantısı'nda,** VM'nin özel IP adresini girin. Gerekirse, ek ayarları ayarlamak için **Seçenekleri Göster'i** seçin ve ardından bağlanın.

### <a name="to-troubleshoot-an-rdp-connection-to-a-vm"></a>VM ile RDP bağlantısı sorunlarını giderme

VPN bağlantınız üzerinden sanal bir makineye bağlanmada sorun yaşıyorsanız, kontrol edebilirsiniz birkaç şey vardır. 

- VPN bağlantınızın başarılı olduğunu doğrulayın.
- VM'nin özel IP adresine bağlandığınızı doğrulayın.
- Bağlandığınız bilgisayardaki Ethernet bağdaştırıcısına atanan IPv4 adresini kontrol etmek için **ipconfig** girin. Ip adresi bağlandığınız VNet'in adres aralığında veya VPNClientAddressPool'unuzun adres aralığında olduğunda çakışan bir adres alanı oluşur. Adres alanınız bu şekilde çakıştığında, ağ trafiği Azure’a ulaşmaz ve yerel ağda kalır.
- Bilgisayar adını değil de özel IP adresini kullanarak VM'ye bağlanabiliyorsanız, DNS'yi doğru şekilde yapılandırdığınızı doğrulayın. VM’ler için ad çözümlemesinin nasıl çalıştığı hakkında daha fazla bilgi için bkz. [VM'ler için Ad Çözümlemesi](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).
- VNet için DNS sunucu IP adreslerini belirttikten sonra VPN istemcisi yapılandırma paketinin oluşturulduğunu doğrulayın. DNS sunucu IP adreslerini güncellerseniz, yeni bir VPN istemci yapılandırma paketi oluşturun ve yükleyin.

Daha fazla sorun giderme bilgisi için bkz. [VM ile Uzak Masaüstü bağlantılarında sorun giderme](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md).