---
title: include dosyası
description: include dosyası
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 02/20/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: dc80141d796b66dd7e610342166f7b88df58f530
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75927851"
---
## <a name="create-a-namespace-in-the-azure-portal"></a>Azure portalında bir ad alanı oluşturma
Azure'da Service Bus mesajlaşma varlıklarını kullanmaya başlamak için öncelikle Azure'da benzersiz olan bir ad alanı oluşturmanız gerekir. Ad alanı, uygulamanızda bulunan Service Bus kaynaklarını adreslemek için içeriğin kapsamını belirleyen bir kapsayıcı sunar.

Ad alanı oluşturmak için:

1. [Azure portalında](https://portal.azure.com) oturum açın
2. Portalın sol gezinti bölmesinde + **Kaynak Oluştur,** **Tümleştirme'yi**seçin ve ardından **Servis Veri Servisi'ni**seçin.

    ![Kaynak oluşturma -> Tümleştirme -> Hizmet Veri Servisi](./media/service-bus-create-namespace-portal/create-resource-service-bus-menu.png)
3. Ad **alanı oluştur** iletişim kutusunda aşağıdaki adımları yapın: 
    1. Ad **alanı için**bir ad girin. Adın kullanılabilirliği sistem tarafından hemen kontrol edilir. Ad alanlarını adlandırma kuralları listesi için [bkz.](/rest/api/servicebus/create-namespace)
    2. Ad alanı için fiyatlandırma katmanını (Temel, Standart veya Premium) seçin. [Konuları ve abonelikleri](../articles/service-bus-messaging/service-bus-queues-topics-subscriptions.md#topics-and-subscriptions)kullanmak istiyorsanız, Standart veya Premium'u seçin. Konular/abonelikler, Temel fiyatlandırma katmanında desteklenmez.
    3. **Premium** fiyatlandırma katmanını seçtiyseniz aşağıdaki adımları izleyin: 
        1. **İleti birimlerinin**sayısını belirtin. Premium katman, her iş yükünün izole olarak çalıştırabilmesi için CPU ve bellek düzeyinde kaynak yalıtımı sağlar. Bu kaynak kapsayıcısı mesajlaşma birimi olarak adlandırılır. Premium ad alanının en az bir ileti birimi vardır. Her Service Bus Premium ad alanı için 1, 2 veya 4 ileti birimi seçebilirsiniz. Daha fazla bilgi için [Service Bus Premium Mesajlaşma'ya](../articles/service-bus-messaging/service-bus-premium-messaging.md)bakın.
        2. Ad alanı **bölgesini gereksiz**yapmak isteyip istemediğinizi belirtin. Bölge artıklığı, yinelemeleri ek ücret ödemeden bir bölge içindeki kullanılabilirlik bölgeleri arasında yayarak gelişmiş kullanılabilirlik sağlar. Daha fazla bilgi için [Azure'daki Kullanılabilirlik bölgelerine](../articles/availability-zones/az-overview.md)bakın.
    4. **Abonelik**için, ad alanını oluşturmak için bir Azure aboneliği seçin.
    5. **Kaynak grubu**için, ad alanının yaşayacağı varolan bir kaynak grubu seçin veya yeni bir tane oluşturun.      
    6. **Konum**için, ad alanınızın barındırılması gereken bölgeyi seçin.
    7. **Oluştur'u**seçin. Artık sistem ad alanınızı oluşturur ve kullanıma açar. Sistem, hesabınıza yönelik kaynakları sağlarken birkaç dakika beklemeniz gerekebilir.
   
        ![Ad alanı oluşturma](./media/service-bus-create-namespace-portal/create-namespace.png)
4. Servis veri günü ad alanının başarıyla dağıtılmış olduğunu doğrulayın. Bildirimleri görmek için araç çubuğundaki **çan simgesini (Uyarılar)** seçin. Bildirimdeki **kaynak grubunun adını** resimde gösterildiği şekilde seçin. Servis veri günü ad alanını içeren kaynak grubunu görürsünüz.

    ![Dağıtım uyarısı](./media/service-bus-create-namespace-portal/deployment-alert.png)
5. Kaynak grubunuzun **Kaynak grubu** **sayfasında, servis veri yolundaki ad alanınızı**seçin. 

    ![Kaynak grubu sayfası - servis veri aracı ad alanınızı seçin](./media/service-bus-create-namespace-portal/resource-group-select-service-bus.png)
6. Servis veri günü ad alanınızın ana sayfasını görürsünüz. 

    ![Servis veri günü ad alanınız için giriş sayfası](./media/service-bus-create-namespace-portal/service-bus-namespace-home-page.png)

## <a name="get-the-connection-string"></a>Bağlantı dizesini alma 
Yeni bir ad alanı oluşturulduğunda, her biri ad alanının tüm yönleri üzerinde tam denetim veren ilişkili bir çift birincil ve ikincil anahtara sahip bir ilk Paylaşılan Erişim İmzası (SAS) kuralı otomatik olarak oluşturulur. Normal gönderenler ve alıcılar için daha kısıtlı haklara sahip kurallar oluşturma hakkında bilgi için [Hizmet Veri Servisi kimlik doğrulaması](../articles/service-bus-messaging/service-bus-authentication-and-authorization.md) ve yetkilendirmeye bakın. Ad alanınız için birincil ve ikincil tuşları kopyalamak için aşağıdaki adımları izleyin: 

1. **Tüm kaynaklar**’a ve sonra yeni oluşturulan ad alanı adına tıklayın.
2. Ad alanı penceresinde **Paylaşılan erişim ilkeleri**'ne tıklayın.
3. **Paylaşılan erişim ilkeleri** ekranında **RootManageSharedAccessKey** seçeneğine tıklayın.
   
    ![bağlantı bilgisi](./media/service-bus-create-namespace-portal/connection-info.png)
4. **İlke: RootManageSharedAccessKey** penceresinde, bağlantı dizesini daha sonra kullanmak üzere panonuza kopyalamak için Birincil Bağlantı Dizesi'nin yanındaki kopya düğmesini tıklatın. **Primary Connection String** Bu değeri Not Defteri veya başka bir geçici konuma yapıştırın.
   
    ![bağlantı dizesi](./media/service-bus-create-namespace-portal/connection-string.png)
5. Önceki adımı, **birincil anahtarın** değerini daha sonra kullanmak üzere geçici bir konuma kopyalama ve yapıştırma işlemi nin tekrarını yineleyin.

<!--Image references-->

