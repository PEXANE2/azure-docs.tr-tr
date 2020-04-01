---
title: Öğretici - Azure Trafik Yöneticisi ile öncelikli trafik yönlendirmeyi yapılandırın
description: Bu öğretici, Trafik Yöneticisi'nde öncelikli trafik yönlendirme yönteminin nasıl yapılandırılabildiğini açıklar
services: traffic-manager
documentationcenter: ''
author: rohinkoul
manager: kumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: rohink
ms.openlocfilehash: ca223a19ff7ddeae95878f941f3cf295664e62b4
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76938721"
---
# <a name="tutorial-configure-priority-traffic-routing-method-in-traffic-manager"></a>Öğretici: Trafik Yöneticisi'nde öncelikli trafik yönlendirme yöntemini yapılandırın

Azure Web Siteleri, web sitesi modune bakılmaksızın, veri merkezi içindeki (bölge olarak da bilinir) web siteleri için zaten başarısız işlev sağlar. Trafik Yöneticisi farklı veri merkezlerinde web siteleri için failover sağlar.

Hizmet başarısızlığı için yaygın bir desen, trafiği birincil hizmete göndermek ve başarısız olmak için bir dizi aynı yedekleme hizmeti sağlamaktır. Aşağıdaki adımlar, azure bulut hizmetleri ve web siteleri ile bu öncelikli başarısızlık nasıl yapılandırılabildiğini açıklar:

## <a name="to-configure-the-priority-traffic-routing-method"></a>Öncelikli trafik yönlendirme yöntemini yapılandırmak için

1. Bir tarayıcıdan [Azure portalında](https://portal.azure.com) oturum açın. Henüz bir hesabınız yoksa, [bir aylık ücretsiz denemeye](https://azure.microsoft.com/free/) kaydolabilirsiniz. 
2. Portalın arama çubuğunda, Trafik **Yöneticisi profillerini** arayın ve ardından yönlendirme yöntemini yapılandırmak istediğiniz profil adını tıklatın.
3. Trafik **Yöneticisi profil** bıçak bölümünde, yapılandırmanıza eklemek istediğiniz bulut hizmetlerinin ve web sitelerinin mevcut olduğunu doğrulayın.
4. **Ayarlar** bölümünde Yapılandırma'yı ve **Yapılandırma** bıçağını **aşağıdaki**gibi tamamlayın:
    1. **Trafik yönlendirme yöntemi ayarları için,** trafik yönlendirme yönteminin **Öncelik**olduğunu doğrulayın. Değilse, açılır listeden **Öncelik'i** tıklatın.
    2. Bu profildeki tüm uç nokta için **Bitiş Noktası monitör ayarlarını** aşağıdaki gibi ayarlayın:
        1. Uygun **Protokolü**seçin ve **Bağlantı Noktası** numarasını belirtin. 
        2. **Yol** için bir */* ileri eğik çizgi . Uç noktaları izlemek için bir yol ve dosya adı belirtmeniz gerekir. İleri ye doğru çizgi "/" göreli yol için geçerli bir giriştir ve dosyanın kök dizinde (varsayılan) olduğunu ima eder.
        3. Sayfanın üst kısmında **Kaydet'i**tıklatın.
5. **Ayarlar** **bölümünde, Bitiş Noktaları'nı**tıklatın.
6. Uç **Noktalar** bıçağında, uç noktalarınız için öncelik sırasını gözden geçirin. **Öncelikli** trafik yönlendirme yöntemini seçtiğinizde, seçili uç noktaların sırası önemlidir. Uç noktaların öncelik sırasını doğrulayın.  Birincil bitiş noktası üsttedir. Görüntülenme sırasını iki kez kontrol edin. tüm istekler ilk bitiş noktasına yönlendirilecek ve Trafik Yöneticisi sağlıksız olduğunu algılarsa, trafik otomatik olarak bir sonraki bitiş noktasına geçemezse. 
7. Bitiş noktası öncelik sırasını değiştirmek için bitiş noktasını tıklatın ve görüntülenen **Bitiş Noktası** bıtır'da **Düzenle'yi** tıklatın ve **öncelik** değerini gerektiği gibi değiştirin. 
8. Bitiş noktası ayarlarını değiştirmek için **Kaydet'i** tıklatın.
9. Yapılandırma değişikliklerinizi tamamladıktan sonra sayfanın altındaki **Kaydet'i** tıklatın.
10. Yapılandırmanızdaki değişiklikleri aşağıdaki gibi test edin:
    1.  Portalın arama çubuğunda, Trafik Yöneticisi profil adını arayın ve görüntülenen sonuçlarda Trafik Yöneticisi profilini tıklatın.
    2.  Trafik **Yöneticisi** profil bıçak, **Genel Bakış'ı**tıklatın.
    3.  **Trafik Yöneticisi profil** bıçağı, yeni oluşturduğunuz Trafik Yöneticisi profilinizin DNS adını görüntüler. Bu, yönlendirme türüne göre belirlendiği şekilde doğru uç noktaya yönlendirilen almak için herhangi bir istemci (örneğin, bir web tarayıcısı kullanarak ona yönlendirilerek) tarafından kullanılabilir. Bu durumda tüm istekler ilk bitiş noktasına yönlendirilir ve Trafik Yöneticisi sağlıksız olduğunu algılarsa, trafik otomatik olarak bir sonraki bitiş noktasına geçmezse.
11. Trafik Yöneticisi profiliniz çalışmaya başladıktan sonra, yetkili DNS sunucunuzdaki DNS kaydını şirket alan adınızı Trafik Yöneticisi etki alanı adına işaret etmek için düzenleme.

![Trafik Yöneticisi'ni kullanarak öncelikli trafik yönlendirme yöntemini yapılandırma][1]

## <a name="next-steps"></a>Sonraki adımlar


- [Ağırlıklı trafik yönlendirme yöntemi](traffic-manager-configure-weighted-routing-method.md) hakkında bilgi edinin.
- Performans [yönlendirme yöntemi](traffic-manager-configure-performance-routing-method.md)hakkında bilgi edinin.
- [Coğrafi yönlendirme yöntemi](traffic-manager-configure-geographic-routing-method.md) hakkında bilgi edinin.
- [Trafik Yöneticisi ayarlarını](traffic-manager-testing-settings.md)nasıl test edebilirsiniz öğrenin.

<!--Image references-->
[1]: ./media/traffic-manager-priority-routing-method/traffic-manager-priority-routing-method.png