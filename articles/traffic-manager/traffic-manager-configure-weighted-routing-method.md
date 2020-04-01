---
title: Öğretici - Azure Trafik Yöneticisi ile ağırlıklı trafik yönlendirmeyi yapılandırma
description: Bu öğretici, Trafik Yöneticisi'nde bir round-robin yöntemini kullanarak denge trafiğinin nasıl yüklenir olduğunu açıklar
services: traffic-manager
documentationcenter: ''
author: rohinkoul
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: rohink
ms.openlocfilehash: 8bdc710b36cae70d29d32333f431b8a9dda154cc
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76938704"
---
# <a name="tutorial-configure-the-weighted-traffic-routing-method-in-traffic-manager"></a>Öğretici: Trafik Yöneticisi'nde ağırlıklı trafik yönlendirme yöntemini yapılandırın

Ortak bir trafik yönlendirme yöntemi deseni, bulut hizmetlerini ve web sitelerini içeren aynı uç noktalar kümesini sağlamak ve her birine eşit olarak trafik göndermektir. Aşağıdaki adımlar, bu tür trafik yönlendirme yönteminin nasıl yapılandırılabildiğini ana hatlar.

> [!NOTE]
> Azure Web Uygulaması, azure bölgesindeki web siteleri için zaten yük dengeleme işlevi sağlar (birden çok veri merkezi kapsayabilir). Trafik Yöneticisi, trafiği farklı veri merkezlerindeweb siteleri arasında dağıtmanıza olanak tanır.

## <a name="to-configure-the-weighted-traffic-routing-method"></a>Ağırlıklı trafik yönlendirme yöntemini yapılandırmak için

1. Bir tarayıcıdan [Azure portalında](https://portal.azure.com) oturum açın. Henüz bir hesabınız yoksa, [bir aylık ücretsiz denemeye](https://azure.microsoft.com/free/) kaydolabilirsiniz. 
2. Portalın arama çubuğunda, Trafik **Yöneticisi profillerini** arayın ve ardından yönlendirme yöntemini yapılandırmak istediğiniz profil adını tıklatın.
3. Trafik **Yöneticisi profil** bıçak bölümünde, yapılandırmanıza eklemek istediğiniz bulut hizmetlerinin ve web sitelerinin mevcut olduğunu doğrulayın.
4. **Ayarlar** bölümünde Yapılandırma'yı ve **Yapılandırma** bıçağını **aşağıdaki**gibi tamamlayın:
    1. **Trafik yönlendirme yöntemi ayarları için,** trafik yönlendirme yönteminin **Ağırlıklı**olduğunu doğrulayın. Değilse, açılır listeden **Ağırlıklı'yı** tıklatın.
    2. Bu profildeki tüm uç nokta için **Bitiş Noktası monitör ayarlarını** aşağıdaki gibi ayarlayın:
        1. Uygun **Protokolü**seçin ve **Bağlantı Noktası** numarasını belirtin. 
        2. **Yol** için bir */* ileri eğik çizgi . Uç noktaları izlemek için bir yol ve dosya adı belirtmeniz gerekir. İleri ye doğru çizgi "/" göreli yol için geçerli bir giriştir ve dosyanın kök dizinde (varsayılan) olduğunu ima eder.
        3. Sayfanın üst kısmında **Kaydet'i**tıklatın.
5. Yapılandırmanızdaki değişiklikleri aşağıdaki gibi test edin:
    1.  Portalın arama çubuğunda, Trafik Yöneticisi profil adını arayın ve görüntülenen sonuçlarda Trafik Yöneticisi profilini tıklatın.
    2.  Trafik **Yöneticisi** profil bıçak, **Genel Bakış'ı**tıklatın.
    3.  **Trafik Yöneticisi profil** bıçağı, yeni oluşturduğunuz Trafik Yöneticisi profilinizin DNS adını görüntüler. Bu, yönlendirme türüne göre belirlendiği gibi doğru uç noktaya yönlendirilen almak için herhangi bir istemci (örneğin, bir web tarayıcısı kullanarak ona yönlendirilerek) tarafından kullanılabilir. Bu durumda tüm istekler bir round-robin moda her bitiş noktası yönlendirilir.
6. Trafik Yöneticisi profiliniz çalışmaya başladıktan sonra, yetkili DNS sunucunuzdaki DNS kaydını şirket alan adınızı Trafik Yöneticisi etki alanı adına işaret etmek için düzenleme.

![Trafik Yöneticisi kullanarak ağırlıklı trafik yönlendirme yöntemini yapılandırma][1]

## <a name="next-steps"></a>Sonraki adımlar

- Öncelikli [trafik yönlendirme yöntemi](traffic-manager-configure-priority-routing-method.md)hakkında bilgi edinin.
- Performans [trafiği yönlendirme yöntemi](traffic-manager-configure-performance-routing-method.md)hakkında bilgi edinin.
- [Coğrafi yönlendirme yöntemi](traffic-manager-configure-geographic-routing-method.md) hakkında bilgi edinin.
- [Trafik Yöneticisi ayarlarını](traffic-manager-testing-settings.md)nasıl test edebilirsiniz öğrenin.

<!--Image references-->
[1]: ./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-routing-method.png
