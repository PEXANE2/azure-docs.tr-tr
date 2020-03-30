---
title: Web sayfaları ile Gerçek Kullanıcı Ölçümleri - Azure Trafik Yöneticisi
description: Bu makalede, Azure Trafik Yöneticisi'ne Gerçek Kullanıcı Ölçümleri göndermek için web sayfalarınızı nasıl ayarlayamanız gerektiğini öğrenin.
services: traffic-manager
documentationcenter: traffic-manager
author: rohinkoul
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/16/2018
ms.author: rohink
ms.custom: ''
ms.openlocfilehash: 927d774ee30a291607a8a47fc2fd6878c1bc6fee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76938690"
---
# <a name="how-to-send-real-user-measurements-to-azure-traffic-manager-using-web-pages"></a>Web sayfalarını kullanarak Azure Trafik Yöneticisi'ne Gerçek Kullanıcı Ölçümleri nasıl gönderilir?

Gerçek Kullanıcı Ölçümleri (RUM) anahtarı nı alarak ve oluşturulan kodu web sayfasına katıştırarak web sayfalarınızı Trafik Yöneticisi'ne Gerçek Kullanıcı Ölçümleri gönderecek şekilde yapılandırabilirsiniz.

## <a name="obtain-a-real-user-measurements-key"></a>Gerçek Kullanıcı Ölçümleri anahtarı edinin

Müşteri uygulamanızdan alıp Trafik Yöneticisi'ne gönderdiğiniz **ölçümler, gerçek kullanıcı ölçümleri (RUM) Anahtarı**adı verilen benzersiz bir dize kullanılarak servis tarafından tanımlanır. Azure portalını, REST API'sini kullanarak veya PowerShell veya Azure CLI'yi kullanarak bir RUM anahtarı alabilirsiniz.

Azure portalını kullanarak RUM Anahtarı'nı elde etmek için:
1. Bir tarayıcıdan Azure portalında oturum açın. Henüz bir hesabınız yoksa, bir aylık ücretsiz denemeye kaydolabilirsiniz.
2. Portalın arama çubuğunda, değiştirmek istediğiniz Traffic Manager profili adını arayın ve ardından gösterilen sonuçlardaki Traffic Manager profiline tıklayın.
3. Trafik Yöneticisi profil çubuğunda, **Ayarlar**altında **Gerçek Kullanıcı Ölçümleri'ni** tıklatın.
4. Yeni bir RUM Anahtarı oluşturmak için **Oluştur Tuşu'na** tıklayın.
 
   ![Gerçek Kullanıcı Ölçümleri oluşturma anahtarı](./media/traffic-manager-create-rum-visual-studio/generate-rum-key.png)

   **Şekil 1: Gerçek Kullanıcı Ölçümleri Anahtar Üretimi**

5. Bıçak artık oluşturulan RUM Anahtarını ve HTML sayfanıza katıştıolması gereken bir JavaScript kod parçacığı görüntüler.
 
    ![Gerçek Kullanıcı Ölçümleri anahtarı için Javascript kodu](./media/traffic-manager-create-rum-web-pages/rum-javascript-code.png)

    **Şekil 2: Gerçek Kullanıcı Ölçümleri Anahtar ve Ölçüm JavaScript**
 
6. JavaScript kodunu kopyalamak için **Kopyala** düğmesini tıklatın. 

>[!IMPORTANT]
> Düzgün çalışması için oluşturulan Gerçek Kullanıcı Ölçümleri için JavaScript özelliğini kullanın. Bu komut dosyasında veya Gerçek Kullanıcı Ölçümleri tarafından kullanılan komut dosyalarında yapılan değişiklikler öngörülemeyen davranışlara neden olabilir.

## <a name="embed-the-code-to-an-html-web-page"></a>Kodu HTML web sayfasına gömme

RUM anahtarını aldıktan sonra, bir sonraki adım, bu kopyalanmış JavaScript'i son kullanıcılarınızın ziyaret ettiği bir HTML sayfasına yerleştirmektir. HTML düzenleme birçok şekilde yapılabilir ve farklı araçlar ve iş akışları kullanılarak. Bu örnek, bu komut dosyasını eklemek için bir HTML sayfasının nasıl güncelleştirilebildiğini gösterir. Bu kılavuzu, HTML kaynak yönetimi iş akışınıza uyarlamak için kullanabilirsiniz.

1.  Metin düzenleyicisinde HTML sayfasını açma
2.  Html'in GÖVDE bölümüne önceki adımda kopyaladığınız JavaScript kodunu yapıştırın (kopyalanan kod 8 & 9 satırındadır, şekil 3'e bakın).
 
    ![Gerçek Kullanıcı Ölçümleri için Javascript kodunu web sayfasına göm](./media/traffic-manager-create-rum-web-pages/real-user-measurement-embed-script.png)  

    **Şekil 3: Gömülü Gerçek Kullanıcı Ölçümleri JavaScript ile Basit HTML**

3.  HTML dosyasını kaydedin ve internete bağlı bir web sunucusunda barındırın. 
4. Bu sayfa bir web tarayıcısı üzerinde bir sonraki kez işlenir, JavaScript başvurulan indirilir ve komut dosyası ölçüm ve raporlama işlemleri yürütür.


## <a name="next-steps"></a>Sonraki adımlar
- [Gerçek Kullanıcı Ölçümleri](traffic-manager-rum-overview.md) hakkında daha fazla bilgi edinin
- [Trafik Yöneticisi'nin nasıl çalıştığını](traffic-manager-overview.md) öğrenin
- Trafik Yöneticisi tarafından desteklenen [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md) hakkında daha fazla bilgi edinin
- Trafik Yöneticisi profilini nasıl [oluşturabilirsiniz](traffic-manager-create-profile.md) öğrenin

