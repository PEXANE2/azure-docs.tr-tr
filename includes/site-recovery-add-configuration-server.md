---
title: include dosyası
description: include dosyası
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: include
ms.date: 02/28/2019
ms.author: mayg
ms.custom: include file
ms.openlocfilehash: 7c682105113dac7c1d457489cf926210ead77993
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188085"
---
1. Birleşik Kurulum yükleme dosyasını çalıştırın.
2. **Başlamadan Önce,** **yapılandırma sunucusu ve işlem sunucusu yükleyin**seçin.

    ![Başlamadan önce](./media/site-recovery-add-configuration-server/combined-wiz1.png)

3. MySQL indirip yüklemek için **Üçüncü Taraf Yazılım Lisansı** bölümünde **Kabul Ediyorum**’a tıklayın.

    ![Üçüncü taraf yazılım](./media/site-recovery-add-configuration-server/combined-wiz2.png)
4. **Kayıt** menüsünde kasadan indirdiğiniz kayıt defteri anahtarını seçin.

    ![Kayıt](./media/site-recovery-add-configuration-server/combined-wiz3.png)
5. **İnternet Ayarları** alanında, yapılandırma sunucusunda çalışan Sağlayıcının Azure Site Recovery'ye İnternet üzerinden nasıl bağlanacağını belirtin. Gerekli URL'lere izin verdiğınızdan emin olun.

    - Şu anda makinede ayarlanmış olan proxy ile bağlanmak istiyorsanız, **proxy sunucusu kullanarak Azure Site Kurtarma'ya Bağlan'ı**seçin.
    - Sağlayıcının doğrudan bağlanmasını istiyorsanız, **proxy sunucusu olmadan doğrudan Azure Site Kurtarma'ya bağlan'ı**seçin.
    - Varolan proxy kimlik doğrulaması gerektiriyorsa veya Sağlayıcı bağlantısı için özel bir proxy kullanmak istiyorsanız, **özel proxy ayarlarıyla bağlan'ı**seçin ve adresi, bağlantı noktasını ve kimlik bilgilerini belirtin.
     ![Güvenlik duvarı](./media/site-recovery-add-configuration-server/combined-wiz4.png)
6. **Önkoşul Denetimi** menüsünde Kurulum, yüklemenin çalışabildiğinden emin olmak üzere bir denetim gerçekleştirir. **Genel saat eşitleme denetimi** hakkında bir uyarı görünürse, sistem saatindeki zamanın (**Tarih ve Saat** ayarları) saat dilimiyle aynı olduğunu doğrulayın.

    ![Ön koşullar](./media/site-recovery-add-configuration-server/combined-wiz5.png)
7. **MySQL Yapılandırması** menüsünde, yüklü MySQL sunucu örneğinde oturum açmak için kimlik bilgileri oluşturun.

    ![MySQL](./media/site-recovery-add-configuration-server/combined-wiz6.png)
8. **Ortam**Ayrıntıları'nda, Azure Stack VM'lerini veya fiziksel sunucularını çoğaltıyorsanız Hayır'ı seçin. 
9. **Yükleme Konumu** alanında ikili dosyaları yüklemek ve önbelleği depolamak istediğiniz konumu seçin. Seçtiğiniz sürücü en az 5 GB kullanılabilir disk alanına sahip olmalıdır, ancak en az 600 GB boş alanı olan bir önbellek sürücüsü seçmeniz önerilir.

    ![Yükleme konumu](./media/site-recovery-add-configuration-server/combined-wiz8.png)
10. **Ağ**Seçimi'nde, önce yerleşik işlem sunucusunun kaynak makinelerde mobilite hizmetinin keşfi ve itme yüklemesi için kullandığı NIC'i seçin ve ardından Configuration Server'ın Azure ile bağlantı için kullandığı NIC'i seçin. Bağlantı noktası 9443, çoğaltma trafiğini gönderip almak için kullanılan varsayılan bağlantı noktasıdır, ancak bu bağlantı noktası numarasını ortamınızın gereksinimlerine uyacak şekilde değiştirebilirsiniz. Bağlantı noktası 9443’e ek olarak, çoğaltma işlemlerini düzenlemek için web sunucusu tarafından kullanılan bağlantı noktası 443 de açılır. Çoğaltma trafiği göndermek veya almak için 443 bağlantı noktasını kullanmayın.

    ![Ağ seçimi](./media/site-recovery-add-configuration-server/combined-wiz9.png)


11. **Özet** alanındaki bilgileri gözden geçirin ve **Yükle**’ye tıklayın. Yükleme tamamlandığında bir parola oluşturulur. Çoğaltmayı etkinleştirdiğinizde bu parola gerekli olacaktır; bu yüzden kopyalayıp güvenli bir yerde saklayın.

    ![Özet](./media/site-recovery-add-configuration-server/combined-wiz10.png)

Kayıt bittikten sonra sunucu kasadaki **Ayarlar** > **Sunucuları** belinde görüntülenir.
