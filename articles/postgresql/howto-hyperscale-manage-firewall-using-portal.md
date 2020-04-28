---
title: Güvenlik duvarı kurallarını yönetme-hiper ölçek (Citus)-PostgreSQL için Azure veritabanı
description: Azure portal kullanarak PostgreSQL için Azure veritabanı-hiper ölçek (Citus) için güvenlik duvarı kuralları oluşturun ve yönetin
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 9/12/2019
ms.openlocfilehash: 660c395e6cff81b0abcac07e66385f80a538695f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74977548"
---
# <a name="manage-firewall-rules-for-azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL için Azure veritabanı 'nın güvenlik duvarı kurallarını yönetme-hiper ölçek (Citus)
Sunucu düzeyinde güvenlik duvarı kuralları, belirtilen IP adreslerinden veya IP adresi aralığından bir Hyperscale (Citus) düzenleyici düğümüne erişimi yönetmek için kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılır kılavuzunda ilerlemek için şunlar gerekir:
- Bir sunucu grubu [PostgreSQL Için Azure veritabanı – Hyperscale (Citus) sunucu grubu oluşturun](quickstart-create-hyperscale-portal.md).

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Azure portalında sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

> [!NOTE]
> Bu ayarlar, PostgreSQL için Azure veritabanı-hiper ölçek (Citus) sunucu grubu oluşturma sırasında da erişilebilir. **Ağ** sekmesinde, **genel uç nokta**' ı tıklatın.
> ![Azure portal-ağ sekmesi](./media/howto-hyperscale-manage-firewall-using-portal/0-create-public-access.png)

1. PostgreSQL sunucu grubu sayfasında, güvenlik başlığı altında, güvenlik duvarı kurallarını açmak için **ağ** ' ı tıklatın.

   ![Azure portal-ağ ' a tıklayın](./media/howto-hyperscale-manage-firewall-using-portal/1-connection-security.png)

2. Araç çubuğunda **istemci IP 'Si Ekle**' ye (aşağıdan seçenek) veya bağlantıda (seçenek B) tıklayın. Her iki yöntem de, Azure sistemi tarafından algılanan şekilde bilgisayarınızın genel IP adresi ile bir güvenlik duvarı kuralı oluşturur.

   ![Azure portal-istemci IP 'si Ekle ' ye tıklayın](./media/howto-hyperscale-manage-firewall-using-portal/2-add-my-ip.png)

Alternatif olarak, **+ 0.0.0.0-255.255.255.255** (B seçeneğinin sağında) öğesine TıKLADıĞıNıZDA yalnızca IP 'niz olamaz, ancak düzenleyici düğümünün 5432 numaralı bağlantı noktasına erişmek için internet 'in tamamına izin verir. Bu durumda, istemcilerin kümeyi kullanmak için yine de doğru Kullanıcı adı ve parolayla oturum açması gerekir. Bununla birlikte, yalnızca kısa süreler için ve yalnızca üretim dışı veritabanları için dünya genelindeki erişime izin vermeyi öneririz.

3. Yapılandırmayı kaydetmeden önce IP adresinizi doğrulayın. Bazı durumlarda Azure portal tarafından gözlenen IP adresi, internet ve Azure sunucularına erişirken kullanılan IP adresinden farklıdır. Bu nedenle, kural işlevini beklendiği gibi yapmak için başlangıç IP 'sini ve bitiş IP 'sini değiştirmeniz gerekebilir.
   Kendi IP adresinizi denetlemek için bir arama altyapısı veya başka bir çevrimiçi araç kullanın. Örneğin, "IP nedir?" ifadesini aratın.

   ![IP 'im nedir için Bing arama](./media/howto-hyperscale-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Ek adres aralıkları ekleyin. Güvenlik duvarı kurallarında, tek bir IP adresi veya bir adres aralığı belirtebilirsiniz. Kuralı tek bir IP adresiyle sınırlandırmak istiyorsanız, başlangıç IP 'si ve bitiş IP 'si için alana aynı adresi yazın. Güvenlik duvarının açılması, yöneticilerin, kullanıcıların ve uygulamaların 5432 numaralı bağlantı noktasındaki düzenleyici düğümüne erişmesine olanak sağlar.

5. Bu sunucu düzeyinde güvenlik duvarı kuralını kaydetmek için araç çubuğunda **Kaydet** ' e tıklayın. Güvenlik duvarı kuralları güncelleştirmesi başarılı oldu onay için bekleyin.

## <a name="connecting-from-azure"></a>Azure'dan bağlanma

Azure 'da barındırılan uygulamalara (Azure Web Apps uygulaması veya bir Azure VM 'de çalışan) hiper ölçekli veritabanı erişimi sağlamanın kolay bir yolu vardır. **Ağ** bölmesine ve **Kaydet**'e, portalda **Azure hizmetleri ve kaynaklarına bu sunucu grubuna erişim izni ver** seçeneğini **Evet** olarak ayarlayın.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Azure portalı aracılığıyla mevcut sunucu düzeyinde güvenlik duvarı kurallarını yönetme
Güvenlik duvarı kurallarını yönetmek için adımları yineleyin.
* Geçerli bilgisayarı eklemek için + **istemci IP 'Si Ekle**düğmesine tıklayın. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Daha fazla IP adresi eklemek için Kural Adı, Başlangıç IP Adresi ve Bitiş IP Adresi’ni yazın. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Mevcut bir kuralı değiştirmek için kuraldaki alanlardan dilediğinize tıklayıp değiştirin. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Mevcut bir kuralı silmek için, üç nokta (...] simgesine tıklayın ve **Sil** ' e tıklayarak kuralı kaldırın. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
- Bağlantı sorunlarının nasıl giderileceği dahil olmak üzere [güvenlik duvarı kuralları kavramı](concepts-hyperscale-firewall-rules.md)hakkında daha fazla bilgi edinin.
