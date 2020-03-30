---
title: Güvenlik duvarı kurallarını yönetme - Azure portalı - MariaDB için Azure Veritabanı
description: Azure portalını kullanarak MariaDB güvenlik duvarı kuralları için Azure Veritabanı oluşturma ve yönetme
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 8be8e948595cfb93049c0d6c93f421e4902e771d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79530674"
---
# <a name="create-and-manage-azure-database-for-mariadb-firewall-rules-by-using-the-azure-portal"></a>Azure portalını kullanarak MariaDB güvenlik duvarı kuralları için Azure Veritabanı oluşturma ve yönetme
Sunucu düzeyinde güvenlik duvarı kuralları, MariaDB Server için bir Azure Veritabanına erişimi belirli bir IP adresinden veya çeşitli IP adreslerinden yönetmek için kullanılabilir.

Sanal Ağ (VNet) kuralları sunucunuza erişimi güvence altına almak için de kullanılabilir. [Azure portalını kullanarak Sanal Ağ hizmeti uç noktalarını ve kurallarını oluşturma ve yönetme](howto-manage-vnet-portal.md)hakkında daha fazla bilgi edinin.

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Azure portalında sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

1. MariaDB sunucu sayfasında, Ayarlar başlığı altında, MariaDB için Azure Veritabanı'nın Bağlantı Güvenliği sayfasını açmak için **Bağlantı Güvenliği'ni** tıklatın.

   ![Azure portalı - Bağlantı güvenliği'ni tıklatın](./media/howto-manage-firewall-portal/1-connection-security.png)

2. Araç çubuğunda **IP'mi ekle'yi** tıklatın. Bu, Azure sistemi tarafından algılandığı gibi bilgisayarınızın genel IP adresiyle otomatik olarak bir güvenlik duvarı kuralı oluşturur.

   ![Azure portalı - IP'mi ekle'yi tıklatın](./media/howto-manage-firewall-portal/2-add-my-ip.png)

3. Yapılandırmayı kaydetmeden önce IP adresinizi doğrulayın. Bazı durumlarda, Azure portalı tarafından gözlenen IP adresi, Internet ve Azure sunucularına erişirken kullanılan IP adresinden farklıdır. Bu nedenle, kuralın beklendiği gibi çalışmasını sağlamak için Start IP ve End IP'yi değiştirmeniz gerekebilir.

   Kendi IP adresinizi kontrol etmek için bir arama motoru veya başka bir çevrimiçi araç kullanın. Örneğin, "IP adresim nedir" araması yapın.

4. Ek adres aralıkları ekleyin. MariaDB için Azure Veritabanı'nın güvenlik duvarı kurallarında, tek bir IP adresi veya çeşitli adresler belirtebilirsiniz. Kuralı tek bir IP adresiyle sınırlamak istiyorsanız, AYNı adresi IP Başlat ve Ip Sonu alanlarına yazın. Güvenlik duvarını açmak, yöneticilerin, kullanıcıların ve uygulamaların geçerli kimlik bilgilerine sahip oldukları MariaDB sunucusundaki herhangi bir veritabanına erişmesini sağlar.

   ![Azure portalı - güvenlik duvarı kuralları](./media/howto-manage-firewall-portal/4-specify-addresses.png)

5. Bu sunucu düzeyindeki güvenlik duvarı kuralını kaydetmek için araç çubuğunda **Kaydet'i** tıklatın. Güvenlik duvarı kurallarına güncelleştirmenin başarılı olduğunu onaylamayı bekleyin.

   ![Azure portalı - Kaydet'i tıklatın](./media/howto-manage-firewall-portal/5-save-firewall-rule.png)

## <a name="connecting-from-azure"></a>Azure'dan bağlanma
Azure uygulamalarının MariaDB sunucusu için Azure Veritabanınıza bağlanmasına izin vermek için Azure bağlantılarının etkinleştirilmesi gerekir. Örneğin, bir Azure Web Apps uygulamasını veya Azure VM'de çalışan bir uygulamayı barındırmak veya Azure Veri Fabrikası veri yönetimi ağ geçidinden bağlanmak için. Bu bağlantıları etkinleştirmek için güvenlik duvarı kuralı için kaynakların aynı Sanal Ağ (VNet) veya Kaynak Grubu'nda olması gerekmez. Azure’dan bir uygulama, veritabanı sunucunuza bağlanmayı denediğinizde güvenlik duvarı Azure bağlantılarına izin verildiğini doğrular. Bu tür bağlantıları etkinleştirmek için birkaç yöntem vardır. Başlangıç ve bitiş adresi 0.0.0.0’a eşit olan bir güvenlik duvarı ayarı, bu bağlantılara izin verildiğini gösterir. Alternatif olarak, **Bağlantı güvenlik** bölmesinden portalda **Azure hizmetlerine Erişime İzin Ver** seçeneğini **ASıK** olarak ayarlayabilir ve **Kaydet'i tıklatabilirsiniz.** Bağlantı denemesine izin verilmezse, istek MariaDB sunucusu için Azure Veritabanı'na ulaşmaz.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

## <a name="manage-existing-firewall-rules-in-the-azure-portal"></a>Azure portalında varolan güvenlik duvarı kurallarını yönetme
Güvenlik duvarı kurallarını yönetmek için adımları yineleyin.
* Geçerli bilgisayarı eklemek için **+ IP'mi ekle'yi**tıklatın. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Ek IP adresleri eklemek için **RULE NAME,** **START IP**ve **END IP'yi**yazın. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Varolan bir kuralı değiştirmek için, kuraldaki alanlardan herhangi birini tıklatın ve sonra değiştirin. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Varolan bir kuralı silmek için elipsis [...]'yi tıklatın ve sonra **Sil'i**tıklatın. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
 - Benzer şekilde, Azure [CLI'yi kullanarak MariaDB güvenlik duvarı kuralları için Azure Veritabanı oluşturma ve yönetme](howto-manage-firewall-cli.md)komut dosyası oluşturabilirsiniz.
 - [Azure portalını kullanarak Sanal Ağ hizmeti uç noktalarını ve kurallarını oluşturarak ve yöneterek sunucunuza](howto-manage-vnet-portal.md)daha fazla erişim sağleyin.