---
title: Trafik Yöneticisi ile birden çok Azure CDN uç noktasında başarısız
description: Azure CDN uç noktalarıyla Azure Trafik Yöneticisi'ni nasıl ayarlayatılabildiğini öğrenin.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: magattus
ms.custom: ''
ms.openlocfilehash: 8d44e53520481e4ada5c2f16f0c56a4a6a724ec1
ms.sourcegitcommit: df8b2c04ae4fc466b9875c7a2520da14beace222
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80892487"
---
# <a name="set-up-failover-across-multiple-azure-cdn-endpoints-with-azure-traffic-manager"></a>Azure Trafik Yöneticisi ile birden çok Azure CDN uç noktasında hata ayarlama

Azure İçerik Teslim Ağı'nı (CDN) yapılandırdığınızda, gereksinimleriniz için en uygun sağlayıcıyı ve fiyatlandırma katmanını seçebilirsiniz. Azure CDN, küresel olarak dağıtılan altyapısıyla varsayılan olarak, hizmet kullanılabilirliğini ve performansını iyileştirmek için yerel ve coğrafi artıklık ve küresel yük dengelemesi oluşturur. İçerik sunmak için bir konum yoksa, istekler otomatik olarak başka bir konuma yönlendirilir ve her istemci isteğine hizmet etmek için en uygun POP (istek konumu ve sunucu yükü gibi etkenlere bağlı olarak) kullanılır. 
 
Birden çok CDN profiliniz varsa, Azure Trafik Yöneticisi ile kullanılabilirliği ve performansı daha da artırabilirsiniz. Azure Trafik Yöneticisi'ni Azure CDN ile kullanarak birden çok CDN uç noktası arasında bakiye yüklemeyi, aşırı yükleme, coğrafi yük dengeleme ve diğer senaryoları kullanabilirsiniz. Tipik bir başarısız senaryoda, tüm istemci istekleri ilk olarak birincil CDN profiline yönlendirilir; profil kullanılamıyorsa, istekleriniz birincil CDN profiliniz çevrimiçi olana kadar ikincil CDN profiline aktarılır. Azure Trafik Yöneticisi'ni bu şekilde kullanmak, web uygulamanızın her zaman kullanılabilir olmasını sağlar. 

Bu makalede, **Akamai profillerinden** Verizon ve Azure **CDN Standard'dan Azure CDN Standard** ile nasıl başarısız olunca yada başarısız oluna ilgili bir örnek ve kılavuz luk lar verilmektedir.

## <a name="set-up-azure-cdn"></a>Azure CDN'yi ayarlama 
Farklı sağlayıcılarla iki veya daha fazla Azure CDN profili ve uç noktası oluşturun.

1. [Yeni bir CDN profili oluştur'daki](cdn-create-new-endpoint.md#create-a-new-cdn-profile)adımları izleyerek Akamai profilinden Verizon ve Azure **CDN Standard'ından** bir **Azure CDN Standardı** oluşturun.
 
   ![CDN çoklu profilleri](./media/cdn-traffic-manager/cdn-multiple-profiles.png)

2. Yeni profillerin her birinde, [yeni bir CDN bitiş noktası oluşturma'daki](cdn-create-new-endpoint.md#create-a-new-cdn-endpoint)adımları izleyerek en az bir bitiş noktası oluşturun.

## <a name="set-up-azure-traffic-manager"></a>Azure Trafik Yöneticisi'ni ayarlama
Bir Azure Trafik Yöneticisi profili oluşturun ve CDN uç noktalarınızda yük dengelemesi ayarlayın. 

1. [Trafik Yöneticisi Oluştur profilindeki](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-create-profile)adımları izleyerek bir Azure Trafik Yöneticisi profili oluşturun. 

    **Yönlendirme yöntemi için** **Öncelik'i**seçin.

2. Trafik Yöneticisi uç noktaları ekle adımlarını izleyerek Trafik Yöneticisi profilinize CDN uç [noktalarınızı](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-create-profile#add-traffic-manager-endpoints) ekleyin

    **Türü**için, **Dış uç noktaları**seçin. **Öncelik**için bir sayı girin.

    Örneğin, *1* önceliği olan *cdndemo101akamai.azureedge.net* ve *2*önceliği *cdndemo101verizon.azureedge.net.*

   ![CDN trafik yöneticisi bitiş noktaları](./media/cdn-traffic-manager/cdn-traffic-manager-endpoints.png)


## <a name="set-up-custom-domain-on-azure-cdn-and-azure-traffic-manager"></a>Azure CDN ve Azure Trafik Yöneticisi'nde özel etki alanı ayarlama
CDN ve Trafik Yöneticisi profillerinizi ayarladıktan sonra, DNS eşleme eklemek ve CDN bitiş noktalarına özel etki alanını kaydetmek için aşağıdaki adımları izleyin. Bu örnek için, özel etki alanı adı *cdndemo101.dustydogpetcare.online*.

1. GoDaddy gibi özel etki alanınızın etki alanı sağlayıcısının web sitesine gidin ve iki DNS CNAME girişi oluşturun. 

    a. İlk CNAME girişi için, cdnverify alt etki alanı yla özel etki alanAdınızı CDN bitiş noktanıza eşleştirin. Bu giriş, özel etki alanını 2.

      Örneğin: 

      `cdnverify.cdndemo101.dustydogpetcare.online  CNAME  cdnverify.cdndemo101akamai.azureedge.net`  

    b. İkinci CNAME girişi için, CDNverify alt etki alanı olmadan özel etki alanadınızı CDN bitiş noktanıza eşleştirin. Bu giriş, özel etki alanını Trafik Yöneticisi ile eşler. 

      Örneğin: 
      
      `cdndemo101.dustydogpetcare.online  CNAME  cdndemo101.trafficmanager.net`   

    > [!NOTE]
    > Etki alanınız şu anda canlıysa ve kesintiye uğrayamıyorsa, bu adımı son yapın. Özel etki alanı DNS'nizi Trafik Yöneticisi'ne güncellemeden önce CDN uç noktalarının ve trafik yöneticisi etki alanlarının canlı olduğunu doğrulayın.
    >


2.  Azure CDN profilinizden ilk CDN bitiş noktasını (Akamai) seçin. **Özel etki alanı ve** giriş *cdndemo101.dustydogpetcare.online*ekle seçin. Özel etki alanını doğrulamak için onay işaretinin yeşil olduğunu doğrulayın. 

    Azure CDN, bu kayıt işlemini tamamlamak için DNS eşleciliğini doğrulamak için *cdnverify* alt etki alanını kullanır. Daha fazla bilgi için [cname DNS kaydı oluşturma'ya](cdn-map-content-to-custom-domain.md#create-a-cname-dns-record)bakın. Bu adım, Azure CDN'nin isteklerine yanıt verebilmek için özel etki alanını tanımasını sağlar.
    
    > [!NOTE]
    > **Akamai profillerinden** bir Azure CDN'inde TLS'yi etkinleştirmek için, özel etki alanını doğrudan bitiş noktanıza açıklamanız gerekir. TLS etkinleştirmek için cdnverify henüz desteklenmez. 
    >

3.  Özel etki alanınızın etki alanı sağlayıcısı için web sitesine dönün ve özel etki alanının ikinci CDN bitiş noktanıza eşlenmesi için oluşturduğunuz ilk DNS eşleciliğini güncelleştirin.
                             
    Örneğin: 

    `cdnverify.cdndemo101.dustydogpetcare.online  CNAME  cdnverify.cdndemo101verizon.azureedge.net`  

4. Azure CDN profilinizden ikinci CDN bitiş noktasını (Verizon) seçin ve adımı 2'yi yineleyin. **Özel etki alanı ekle**ve *cdndemo101.dustydogpetcare.online*giriş seçin.
 
Bu adımları tamamladıktan sonra, başarısız özelliklere sahip çoklu CDN hizmetiniz Azure Trafik Yöneticisi ile ayarlanır. Test URL'lerine özel etki alanınızdan erişebilirsiniz. İşlevselliği sınamak için birincil CDN bitiş noktasını devre dışı bırak ve isteğin ikincil CDN bitiş noktasına doğru şekilde taşındığını doğrulayın. 

## <a name="next-steps"></a>Sonraki adımlar
Yükü farklı CDN uç noktaları arasında dengelemek için coğrafi gibi diğer yönlendirme yöntemleride de ayarlayabilirsiniz. Daha fazla bilgi için bkz. [Trafik Yöneticisi'ni kullanarak coğrafi trafik yönlendirme yöntemini yapılandırın.](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-configure-geographic-routing-method)



