---
title: 'Hızlı başlangıç: Kurumsal katman önbelleği oluşturma'
description: Bu hızlı başlangıçta, Redsıs kurumsal katmanı için bir Azure önbelleği örneği oluşturmayı öğrenin
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.custom: mvc
ms.topic: quickstart
ms.date: 05/12/2020
ms.openlocfilehash: f3eee85c600d40b3997a0e6dff6a9b218242feb4
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83204721"
---
# <a name="quickstart-create-an-enterprise-tier-cache-preview"></a>Hızlı başlangıç: Kurumsal katman önbelleği oluşturma (Önizleme)

Redsıs ' Kurumsal katmanları için Azure önbelleği, Azure 'da tamamen tümleşik ve yönetilen [Redsıs Enterprise](https://redislabs.com/redis-enterprise/) sağlar. Şu anda önizleme olarak kullanılabilir. Bu önizlemede iki yeni katman vardır:
* Verileri depolamak için bir sanal makinede geçici bellek (DRAM) kullanan kuruluş
* Verileri depolamak için hem geçici hem de geçici olmayan belleği (NVMe) kullanan Enterprise SSD.

Önizlemeye katılmak için herhangi bir maliyet yoktur. İlgilendiğiniz [Azure Marketi](https://aka.ms/redispreviewsignup/) ' ne kaydolun. Çok sınırlı sayıda katılımcı noktası sunuyoruz ve önizlemeye kabul edileceği garanti edilemez.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce bir Azure aboneliğine sahip olmanız gerekir. Bir tane yoksa, önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-a-cache"></a>Bir önbellek oluşturma
1. Önbellek oluşturmak için, önizleme davetinizdeki bağlantıyı kullanarak Azure portal oturum açın ve **kaynak oluştur**' u seçin.

   > [!IMPORTANT] 
   > Doğrudan Market 'teki *Redsıs kurumsal katmanları Için Azure önbelleğine* abone olun.
   > Bu adım Red, Portal Kullanıcı arabirimi için Azure önbelleği tarafından otomatik olarak gerçekleştirilir.
   >
   
1. **Yeni** sayfada **veritabanları** ' nı seçin ve ardından **redsıs için Azure önbelleği**' ni seçin.
   
   ![Redsıs için Azure önbelleğini seçin](media/cache-create/new-cache-menu.png)
   
1. **Yeni Redis Cache** sayfasında, yeni önbelleğiniz için ayarları yapılandırın.
   
   | Ayar      | Önerilen değer  | Açıklama |
   | ------------ |  ------- | -------------------------------------------------- |
   | **Abonelik** | Açılır ve aboneliğinizi seçin. | Redsıs örneği için bu yeni Azure önbelleğinin oluşturulacağı abonelik. | 
   | **Kaynak grubu** | Açılır ve bir kaynak grubu seçin veya **Yeni oluştur** ' u seçin ve yeni bir kaynak grubu adı girin. | Önbelleğinizin ve diğer kaynaklarınızın oluşturulacağı kaynak grubunun adı. Tüm uygulama kaynaklarınızı tek bir kaynak grubuna yerleştirerek, bunları birlikte kolayca yönetebilir veya silebilirsiniz. | 
   | **DNS adı** | Genel olarak benzersiz bir ad girin. | Önbellek adı, yalnızca rakam, harf veya kısa çizgi içeren 1 ile 63 karakter arasında bir dize olmalıdır. Ad bir sayı veya harfle başlamalı ve bitmeli ve ardışık kısa çizgi içeremez. Önbellek örneğinizin *ana bilgisayar adı* * \<> DNS adı olacaktır. <Azure region> . redisenterprise.cache.azure.net*. | 
   | **Konum** | Açılır ve bir konum seçin. | Kurumsal katmanlar Batı ABD, Doğu ABD 2 ve Batı Avrupa kullanılabilir. |
   | **Önbellek katmanı** | Aşağı açılan bir *kuruluş Dram* veya *Kurumsal SSD* katmanı ve boyut seçin. |  Katman, önbellek için kullanılabilen boyut, performans ve özellikleri belirler. |
   
   ![Kurumsal katman temelleri](media/cache-create/enterprise-tier-basics.png) 

1. **İleri ' yi seçin: ağ oluşturma** ve atlama.

   > [!NOTE] 
   > Özel bağlantı desteği daha sonra gelecektir.
   >

1. **İleri ' yi seçin: Gelişmiş**.
   
   Varsayılan ayarları koruyabilir veya uygun şekilde değiştirebilirsiniz. **Yalnızca TLS aracılığıyla erişime Izin ver**seçeneği açıldığında, uygulamanızdaki yeni önbelleğe erışmek için TLS kullanmanız gerekir.

   ![Kurumsal katman gelişmiş](media/cache-create/enterprise-tier-advanced.png) 

   > [!NOTE] 
   > Redsıs modülleri henüz Enterprise SSD katmanında desteklenmiyor. Redl modülünü kullanmayı planlıyorsanız, bir kurumsal katman önbelleği seçtiğinizden emin olun.
   >
   
1. **İleri ' yi seçin: Etiketler** ve atla.

1. **İleri ' yi seçin: Özet**.

   ![Kurumsal katman Özeti](media/cache-create/enterprise-tier-summary.png) 

1. **Şartlar**altında bulunan onay kutusuna tıklayın, ayarları gözden geçirin ve ardından **gözden geçir + oluştur**' u seçin.
   
   Önbelleğin oluşturulması biraz zaman alır. Redsıs **genel bakış** sayfasında ilerlemeyi izleyebilirsiniz. **Durum** **çalışıyor**olarak görüntülendiğinde, önbellek kullanıma hazırdır.

   > [!NOTE] 
   > Kurumsal katman önbelleği oluşturulduktan sonra bir süre sonra Azure Marketi 'nden Azure *önbelleğini redsıs, kurumsal katmanlara yönelik*olarak yapılandırmak için **gerekli bir işlem** e-postası alacaksınız. Bu eylem gerekli değildir. Bu e-postayı güvenle yoksayabilirsiniz.
   >

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, redin için Azure önbelleğinin kurumsal katman örneğini oluşturmayı öğrendiniz.

> [!div class="nextstepaction"]
> [Redsıs için Azure önbelleği kullanan bir ASP.NET Web uygulaması oluşturun.](./cache-web-app-howto.md)

