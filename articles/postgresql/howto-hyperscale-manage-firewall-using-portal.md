---
title: Güvenlik duvarı kurallarını yönetme - Hyperscale (Citus) - PostgreSQL için Azure Veritabanı
description: Azure portalını kullanarak PostgreSQL - Hyperscale (Citus) için Azure Veritabanı için güvenlik duvarı kuralları oluşturma ve yönetme
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 9/12/2019
ms.openlocfilehash: 660c395e6cff81b0abcac07e66385f80a538695f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74977548"
---
# <a name="manage-firewall-rules-for-azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL için Azure Veritabanı için güvenlik duvarı kurallarını yönetme - Hyperscale (Citus)
Sunucu düzeyinde güvenlik duvarı kuralları, belirtilen bir IP adresinden veya IP adresi aralığından Bir Hyperscale (Citus) koordinatör düğümüne erişimi yönetmek için kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunda adım atmak için şunları yapmanız gerekir:
- Bir sunucu grubu [PostgreSQL için bir Azure Veritabanı oluşturun – Hyperscale (Citus) sunucu grubu.](quickstart-create-hyperscale-portal.md)

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Azure portalında sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

> [!NOTE]
> Bu ayarlara PostgreSQL - Hyperscale (Citus) sunucu grubu için bir Azure Veritabanı oluşturulması sırasında da erişilebilir. **Ağ** sekmesinin **altında, Ortak bitiş noktasını**tıklatın.
> ![Azure portalı - ağ sekmesi](./media/howto-hyperscale-manage-firewall-using-portal/0-create-public-access.png)

1. PostgreSQL sunucu grubu sayfasında, Güvenlik başlığı altında, Güvenlik Duvarı kurallarını açmak için **Ağ'ı** tıklatın.

   ![Azure portalı - Ağ'ı tıklatın](./media/howto-hyperscale-manage-firewall-using-portal/1-connection-security.png)

2. Araç çubuğunda (aşağıdaki A seçeneği) veya bağlantıda (B seçeneği) **istemci IP ekle'yi**tıklatın. Her iki şekilde de otomatik olarak, Azure sistemi tarafından algılandığı gibi bilgisayarınızın genel IP adresiyle bir güvenlik duvarı kuralı oluşturur.

   ![Azure portalı - istemci IP ekle'yi tıklatın](./media/howto-hyperscale-manage-firewall-using-portal/2-add-my-ip.png)

Alternatif olarak, **+Ekle 0.0.0.0 - 255.255.255.255** (B seçeneğinin sağında) tıklayarak yalnızca IP'nizi değil, tüm internetin koordinatör düğümün 5432 numaralı bağlantı noktasına erişmesine izin verir. Bu durumda, istemcilerin kümeyi kullanmak için doğru kullanıcı adı ve parolayla oturum açması gerekir. Bununla birlikte, dünya çapında erişime yalnızca kısa bir süre ve yalnızca üretim dışı veritabanları için izin vermenizi öneririz.

3. Yapılandırmayı kaydetmeden önce IP adresinizi doğrulayın. Bazı durumlarda, Azure portalı tarafından gözlenen IP adresi, Internet ve Azure sunucularına erişirken kullanılan IP adresinden farklıdır. Bu nedenle, kuralın beklendiği gibi çalışmasını sağlamak için Start IP ve End IP'yi değiştirmeniz gerekebilir.
   Kendi IP adresinizi kontrol etmek için bir arama motoru veya başka bir çevrimiçi araç kullanın. Örneğin, "IP'm nedir" araması yapın.

   ![Benim IP nedir Bing arama](./media/howto-hyperscale-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Ek adres aralıkları ekleyin. Güvenlik duvarı kurallarında, tek bir IP adresi veya çeşitli adresler belirtebilirsiniz. Kuralı tek bir IP adresiyle sınırlamak istiyorsanız, Start IP ve End IP için alana aynı adresi yazın. Güvenlik duvarını açmak, yöneticilerin, kullanıcıların ve uygulamaların 5432 numaralı bağlantı noktasındaki koordinatör düğüme erişmesini sağlar.

5. Bu sunucu düzeyindeki güvenlik duvarı kuralını kaydetmek için araç çubuğunda **Kaydet'i** tıklatın. Güvenlik duvarı kurallarına güncelleştirmenin başarılı olduğunu onaylamayı bekleyin.

## <a name="connecting-from-azure"></a>Azure'dan bağlanma

Azure'da barındırılan uygulamalara (Azure Web Apps uygulaması gibi) veya Azure VM'de çalışan uygulamalara Hiper ölçekli veritabanı erişimi vermenin kolay bir yolu vardır. **Azure hizmetlerine ve kaynaklarına bu sunucu grubu seçeneğine erişmesi için** **Ağ** bölmesinden portalda **Evet'e** ayarlayın ve **Kaydet'e**çarptınız.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Azure portalı aracılığıyla mevcut sunucu düzeyinde güvenlik duvarı kurallarını yönetme
Güvenlik duvarı kurallarını yönetmek için adımları yineleyin.
* Geçerli bilgisayarı eklemek için + **İstemci IP ekle**düğmesine tıklayın. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Daha fazla IP adresi eklemek için Kural Adı, Başlangıç IP Adresi ve Bitiş IP Adresi’ni yazın. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Mevcut bir kuralı değiştirmek için kuraldaki alanlardan dilediğinize tıklayıp değiştirin. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Varolan bir kuralı silmek için elipsis [...] 'yi tıklatın ve kuralı kaldırmak için **Sil'i** tıklatın. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
- Bağlantı sorunlarını nasıl gidereceklerini de içeren [güvenlik duvarı kuralları kavramı](concepts-hyperscale-firewall-rules.md)hakkında daha fazla bilgi edinin.
