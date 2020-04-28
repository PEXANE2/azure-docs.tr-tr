---
title: Güvenlik duvarı kurallarını yönetme-Azure portal-PostgreSQL için Azure veritabanı-tek sunucu
description: PostgreSQL için Azure veritabanı için güvenlik duvarı kuralları oluşturma ve yönetme-Azure portal kullanarak tek sunucu
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: aeef22bf96221061a444f40e16e33343fafe511c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74770314"
---
# <a name="create-and-manage-firewall-rules-for-azure-database-for-postgresql---single-server-using-the-azure-portal"></a>PostgreSQL için Azure veritabanı için güvenlik duvarı kuralları oluşturma ve yönetme-Azure portal kullanarak tek sunucu
Sunucu düzeyinde güvenlik duvarı kuralları, bir PostgreSQL için Azure veritabanı sunucusunun belirtilen IP adreslerinden veya IP adresi aralığından erişimini yönetmek için kullanılabilir.

Sanal ağ (VNet) kuralları, sunucunuza erişimi güvenli hale getirmek için de kullanılabilir. [Azure Portal kullanarak sanal ağ hizmet uç noktaları ve kuralları oluşturma ve yönetme](howto-manage-vnet-using-portal.md)hakkında daha fazla bilgi edinin.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılır kılavuzunda ilerlemek için şunlar gerekir:
- Bir sunucu [PostgreSQL Için Azure veritabanı oluşturma](quickstart-create-server-database-portal.md)

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Azure portalında sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma
1. PostgreSQL sunucusu sayfasında, ayarlar başlığı altında **bağlantı güvenliği** ' ne tıklayarak PostgreSQL Için Azure veritabanı bağlantı güvenliği sayfasını açın.

   ![Azure portal-bağlantı güvenliği ' ne tıklayın](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Araç çubuğunda **IP Ekle** ' ye tıklayın. Bu otomatik olarak, Azure sistemi tarafından algılanan şekilde bilgisayarınızın genel IP adresi ile bir güvenlik duvarı kuralı oluşturur.

   ![Azure portal-IP Ekle 'ye tıklayın](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Yapılandırmayı kaydetmeden önce IP adresinizi doğrulayın. Bazı durumlarda Azure portal tarafından gözlenen IP adresi, internet ve Azure sunucularına erişirken kullanılan IP adresinden farklıdır. Bu nedenle, kural işlevini beklendiği gibi yapmak için başlangıç IP 'sini ve bitiş IP 'sini değiştirmeniz gerekebilir.
   Kendi IP adresinizi denetlemek için bir arama altyapısı veya başka bir çevrimiçi araç kullanın. Örneğin, "IP nedir?" ifadesini aratın.

   ![IP 'im nedir için Bing arama](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Ek adres aralıkları ekleyin. PostgreSQL için Azure veritabanı güvenlik duvarı kurallarında, tek bir IP adresi veya bir adres aralığı belirtebilirsiniz. Kuralı tek bir IP adresiyle sınırlandırmak istiyorsanız, başlangıç IP 'si ve bitiş IP 'si için alana aynı adresi yazın. Güvenlik duvarının açılması, yöneticilerin, kullanıcıların ve uygulamaların geçerli kimlik bilgilerine sahip oldukları PostgreSQL sunucusundaki herhangi bir veritabanına erişmesine olanak sağlar.

   ![Azure portal-güvenlik duvarı kuralları](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. Bu sunucu düzeyinde güvenlik duvarı kuralını kaydetmek için araç çubuğunda **Kaydet** ' e tıklayın. Güvenlik duvarı kuralları güncelleştirmesi başarılı oldu onay için bekleyin.

   ![Azure portal-kaydet 'e tıklayın](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)

## <a name="connecting-from-azure"></a>Azure'dan bağlanma
Azure 'daki uygulamaların PostgreSQL için Azure veritabanı sunucusuna bağlanmasına izin vermek için, Azure bağlantılarının etkinleştirilmesi gerekir. Örneğin, bir Azure Web Apps uygulamasını veya bir Azure VM 'de çalışan bir uygulamayı barındırmak ya da bir Azure Data Factory veri yönetimi ağ geçidinden bağlanmak için. Bu bağlantıları etkinleştirmek için kaynakların, güvenlik duvarı kuralı için aynı sanal ağ (VNet) veya kaynak grubunda olması gerekmez. Azure’dan bir uygulama, veritabanı sunucunuza bağlanmayı denediğinizde güvenlik duvarı Azure bağlantılarına izin verildiğini doğrular. Bu tür bağlantıları etkinleştirmek için birkaç yöntem vardır. Başlangıç ve bitiş adresi 0.0.0.0’a eşit olan bir güvenlik duvarı ayarı, bu bağlantılara izin verildiğini gösterir. Alternatif olarak, **bağlantı güvenliği** bölmesinden portalda **Azure hizmetlerine erişime izin ver** seçeneğini ayarlayabilir ve **Kaydet** **' e basın** . Bağlantı girişimine izin verilmiyorsa, istek PostgreSQL için Azure veritabanı sunucusuna ulaşmaz.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Azure portalı aracılığıyla mevcut sunucu düzeyinde güvenlik duvarı kurallarını yönetme
Güvenlik duvarı kurallarını yönetmek için adımları yineleyin.
* Geçerli bilgisayarı eklemek için düğmeye tıklayarak **IP 'Umu ekleyin**. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Daha fazla IP adresi eklemek için Kural Adı, Başlangıç IP Adresi ve Bitiş IP Adresi’ni yazın. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Mevcut bir kuralı değiştirmek için kuraldaki alanlardan dilediğinize tıklayıp değiştirin. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Mevcut bir kuralı silmek için, üç nokta (...] simgesine tıklayın ve **Sil** ' e tıklayarak kuralı kaldırın. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
- Benzer şekilde, [Azure CLI kullanarak PostgreSQL Için Azure veritabanı güvenlik duvarı kuralları oluşturmak ve yönetmek](howto-manage-firewall-using-cli.md)üzere komut dosyası oluşturabilirsiniz.
- [Azure Portal kullanarak sanal ağ hizmet uç noktaları ve kuralları oluşturup yöneterek](howto-manage-vnet-using-portal.md)sunucunuza daha güvenli bir şekilde erişin.
- PostgreSQL için Azure veritabanı sunucusuna bağlanma konusunda yardım için bkz. [PostgreSQL Için Azure veritabanı bağlantı kitaplıkları](concepts-connection-libraries.md).
