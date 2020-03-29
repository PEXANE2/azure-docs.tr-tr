---
title: Güvenlik duvarı kurallarını yönetme - Azure portalı - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Azure portalını kullanarak PostgreSQL - Single Server için Azure Veritabanı için güvenlik duvarı kuralları oluşturma ve yönetme
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: aeef22bf96221061a444f40e16e33343fafe511c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74770314"
---
# <a name="create-and-manage-firewall-rules-for-azure-database-for-postgresql---single-server-using-the-azure-portal"></a>Azure portalını kullanarak PostgreSQL - Single Server için Azure Veritabanı için güvenlik duvarı kuralları oluşturma ve yönetme
Sunucu düzeyinde güvenlik duvarı kuralları, belirtilen bir IP adresinden veya IP adresi aralığından PostgreSQL Server için Azure Veritabanına erişimi yönetmek için kullanılabilir.

Sanal Ağ (VNet) kuralları sunucunuza erişimi güvence altına almak için de kullanılabilir. [Azure portalını kullanarak Sanal Ağ hizmeti uç noktalarını ve kurallarını oluşturma ve yönetme](howto-manage-vnet-using-portal.md)hakkında daha fazla bilgi edinin.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunda adım atmak için şunları yapmanız gerekir:
- Sunucu [PostgreSQL için Azure Veritabanı Oluşturma](quickstart-create-server-database-portal.md)

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Azure portalında sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma
1. PostgreSQL sunucu sayfasında, Ayarlar başlığı altında, PostgreSQL için Azure Veritabanı'nın Bağlantı güvenlik sayfasını açmak için **Bağlantı güvenliği'ni** tıklatın.

   ![Azure portalı - Bağlantı Güvenliği'ni tıklatın](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Araç çubuğunda **IP'mi ekle'yi** tıklatın. Bu, Azure sistemi tarafından algılandığı gibi bilgisayarınızın genel IP adresiyle otomatik olarak bir güvenlik duvarı kuralı oluşturur.

   ![Azure portalı - IP'mi ekle'yi tıklatın](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Yapılandırmayı kaydetmeden önce IP adresinizi doğrulayın. Bazı durumlarda, Azure portalı tarafından gözlenen IP adresi, Internet ve Azure sunucularına erişirken kullanılan IP adresinden farklıdır. Bu nedenle, kuralın beklendiği gibi çalışmasını sağlamak için Start IP ve End IP'yi değiştirmeniz gerekebilir.
   Kendi IP adresinizi kontrol etmek için bir arama motoru veya başka bir çevrimiçi araç kullanın. Örneğin, "IP'm nedir" araması yapın.

   ![Benim IP nedir Bing arama](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Ek adres aralıkları ekleyin. PostgreSQL için Azure Veritabanı'nın güvenlik duvarı kurallarında, tek bir IP adresi veya çeşitli adresler belirtebilirsiniz. Kuralı tek bir IP adresiyle sınırlamak istiyorsanız, Start IP ve End IP için alana aynı adresi yazın. Güvenlik duvarını açmak, yöneticilerin, kullanıcıların ve uygulamaların geçerli kimlik bilgilerine sahip oldukları PostgreSQL sunucusundaki herhangi bir veritabanına erişmesini sağlar.

   ![Azure portalı - güvenlik duvarı kuralları](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. Bu sunucu düzeyindeki güvenlik duvarı kuralını kaydetmek için araç çubuğunda **Kaydet'i** tıklatın. Güvenlik duvarı kurallarına güncelleştirmenin başarılı olduğunu onaylamayı bekleyin.

   ![Azure portalı - Kaydet'i tıklatın](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)

## <a name="connecting-from-azure"></a>Azure'dan bağlanma
Azure uygulamalarının PostgreSQL sunucusu için Azure Veritabanınıza bağlanmasına izin vermek için Azure bağlantılarının etkinleştirilmesi gerekir. Örneğin, bir Azure Web Apps uygulamasını veya Azure VM'de çalışan bir uygulamayı barındırmak veya Azure Veri Fabrikası veri yönetimi ağ geçidinden bağlanmak için. Bu bağlantıları etkinleştirmek için güvenlik duvarı kuralı için kaynakların aynı Sanal Ağ (VNet) veya Kaynak Grubu'nda olması gerekmez. Azure’dan bir uygulama, veritabanı sunucunuza bağlanmayı denediğinizde güvenlik duvarı Azure bağlantılarına izin verildiğini doğrular. Bu tür bağlantıları etkinleştirmek için birkaç yöntem vardır. Başlangıç ve bitiş adresi 0.0.0.0’a eşit olan bir güvenlik duvarı ayarı, bu bağlantılara izin verildiğini gösterir. Alternatif olarak, **Bağlantı güvenlik** bölmesinden portalda **Azure hizmetlerine Erişime İzin Ver** seçeneğini **ASıK** olarak ayarlayabilir ve **Kaydet'e basabilirsiniz.** Bağlantı denemesine izin verilmezse, istek PostgreSQL sunucusu için Azure Veritabanı'na ulaşmaz.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Azure portalı aracılığıyla mevcut sunucu düzeyinde güvenlik duvarı kurallarını yönetme
Güvenlik duvarı kurallarını yönetmek için adımları yineleyin.
* Geçerli bilgisayarı eklemek için + **IP'mi ekle**düğmesine tıklayın. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Daha fazla IP adresi eklemek için Kural Adı, Başlangıç IP Adresi ve Bitiş IP Adresi’ni yazın. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Mevcut bir kuralı değiştirmek için kuraldaki alanlardan dilediğinize tıklayıp değiştirin. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Varolan bir kuralı silmek için elipsis [...] 'yi tıklatın ve kuralı kaldırmak için **Sil'i** tıklatın. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
- Benzer şekilde, Azure [CLI'yi kullanarak PostgreSQL güvenlik duvarı kuralları için Azure Veritabanı oluşturma ve yönetme](howto-manage-firewall-using-cli.md)komut dosyası oluşturabilirsiniz.
- [Azure portalını kullanarak Sanal Ağ hizmeti uç noktalarını ve kurallarını oluşturarak ve yöneterek sunucunuza](howto-manage-vnet-using-portal.md)daha fazla erişim sağleyin.
- PostgreSQL sunucusu için bir Azure Veritabanına bağlanma konusunda yardım için, [PostgreSQL için Azure Veritabanı bağlantı kitaplıklarına](concepts-connection-libraries.md)bakın.
