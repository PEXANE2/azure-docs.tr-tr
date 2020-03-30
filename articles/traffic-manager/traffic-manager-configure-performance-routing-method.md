---
title: Azure Trafik Yöneticisi'ni kullanarak performans trafiği yönlendirme yöntemini yapılandırma | Microsoft Dokümanlar
description: Bu makalede, trafik yöneticisinin en düşük gecikme gecikmesi ile bitiş noktasına yönlendirecek şekilde nasıl yapılandırılabildiğini açıklamaktadır.
services: traffic-manager
manager: twooley
documentationcenter: ''
author: rohinkoul
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: rohink
ms.openlocfilehash: f5e9b7690c28793a35c692a6125a6b11c7a140a4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76938768"
---
# <a name="configure-the-performance-traffic-routing-method"></a>Performans trafiği yönlendirme yöntemini yapılandırma

Performans trafiği yönlendirme yöntemi, istemcinin ağından gelen en düşük gecikme yle trafiği bitiş noktasına yönlendirmenize olanak tanır. Genellikle, en düşük gecikme gecikmesi ile veri merkezi coğrafi mesafe en yakın. Bu trafik yönlendirme yöntemi, ağ yapılandırmasında veya yükünde gerçek zamanlı değişiklikleri hesaba katamaz.

##  <a name="to-configure-performance-routing-method"></a>Performans yönlendirme yöntemini yapılandırmak için

1. Bir tarayıcıdan [Azure portalında](https://portal.azure.com) oturum açın. Henüz bir hesabınız yoksa, [bir aylık ücretsiz denemeye](https://azure.microsoft.com/free/) kaydolabilirsiniz. 
2. Portalın arama çubuğunda, Trafik **Yöneticisi profillerini** arayın ve ardından yönlendirme yöntemini yapılandırmak istediğiniz profil adını tıklatın.
3. Trafik **Yöneticisi profil** bıçak bölümünde, yapılandırmanıza eklemek istediğiniz bulut hizmetlerinin ve web sitelerinin mevcut olduğunu doğrulayın.
4. **Ayarlar** bölümünde Yapılandırma'yı ve **Yapılandırma** bıçağını **aşağıdaki**gibi tamamlayın:
    1. **Trafik yönlendirme yöntemi ayarları**için, Yönlendirme **yöntemi** için **Performans'ı**seçin.
    2. Bu profildeki tüm uç nokta için **Bitiş Noktası monitör ayarlarını** aşağıdaki gibi ayarlayın:
        1. Uygun **Protokolü**seçin ve **Bağlantı Noktası** numarasını belirtin. 
        2. **Yol** için bir */* ileri eğik çizgi . Uç noktaları izlemek için bir yol ve dosya adı belirtmeniz gerekir. İleri ye doğru çizgi "/" göreli yol için geçerli bir giriştir ve dosyanın kök dizinde (varsayılan) olduğunu ima eder.
        3. Sayfanın üst kısmında **Kaydet'i**tıklatın.
5.  Yapılandırmanızdaki değişiklikleri aşağıdaki gibi test edin:
    1.  Portalın arama çubuğunda, Trafik Yöneticisi profil adını arayın ve görüntülenen sonuçlarda Trafik Yöneticisi profilini tıklatın.
    2.  Trafik **Yöneticisi** profil bıçak, **Genel Bakış'ı**tıklatın.
    3.  **Trafik Yöneticisi profil** bıçağı, yeni oluşturduğunuz Trafik Yöneticisi profilinizin DNS adını görüntüler. Bu, yönlendirme türüne göre belirlendiği şekilde doğru uç noktaya yönlendirilen almak için herhangi bir istemci (örneğin, bir web tarayıcısı kullanarak ona yönlendirilerek) tarafından kullanılabilir. Bu durumda, tüm istekler istemcinin ağından en düşük gecikme yle bitiş noktasına yönlendirilir.
6. Trafik Yöneticisi profiliniz çalışmaya başladıktan sonra, yetkili DNS sunucunuzdaki DNS kaydını şirket alan adınızı Trafik Yöneticisi etki alanı adına işaret etmek için düzenleme.

![Trafik Yöneticisi'ni kullanarak performans trafiği yönlendirme yöntemini yapılandırma][1]

## <a name="next-steps"></a>Sonraki adımlar

- [Ağırlıklı trafik yönlendirme yöntemi](traffic-manager-configure-weighted-routing-method.md) hakkında bilgi edinin.
- [Öncelik yönlendirme yöntemi](traffic-manager-configure-priority-routing-method.md) hakkında bilgi edinin.
- [Coğrafi yönlendirme yöntemi](traffic-manager-configure-geographic-routing-method.md) hakkında bilgi edinin.
- [Trafik Yöneticisi ayarlarını](traffic-manager-testing-settings.md)nasıl test edebilirsiniz öğrenin.

<!--Image references-->
[1]: ./media/traffic-manager-performance-routing-method/traffic-manager-performance-routing-method.png