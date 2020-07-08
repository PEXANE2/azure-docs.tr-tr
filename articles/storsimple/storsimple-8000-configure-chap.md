---
title: StorSimple 8000 serisi cihaz için CHAP yapılandırma | Microsoft Docs
description: Bir StorSimple cihazında çekişme el sıkışma kimlik doğrulama protokolü 'Nün (CHAP) nasıl yapılandırılacağını açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: TBD
ms.date: 05/09/2018
ms.author: alkohli
ms.openlocfilehash: 5f0c022de69823aedbbb34eb369de350b4999c69
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85514351"
---
# <a name="configure-chap-for-your-storsimple-device"></a>StorSimple cihazınız için CHAP yapılandırma

Bu öğreticide, StorSimple cihazınız için CHAP 'nin nasıl yapılandırılacağı açıklanmaktadır. Bu makalede ayrıntılı yordam, StorSimple 8000 serisi cihazlar için geçerlidir.

CHAP, sınama el sıkışma kimlik doğrulama protokolünü temsil eder. Sunucular tarafından uzak istemcilerin kimliğini doğrulamak için kullanılan bir kimlik doğrulama şemadır. Doğrulama, paylaşılan bir parolayı veya gizli anahtarı temel alır. CHAP bir yol (tek yönlü) veya karşılıklı (çift yönlü) olabilir. Tek yönlü CHAP, hedefin bir başlatıcısının kimliğini doğruladığında olur. Karşılıklı veya ters CHAP 'de hedef, başlatıcısının kimliğini doğrular ve ardından başlatıcıya hedefin kimliğini doğrular. Başlatıcı kimlik doğrulaması, hedef kimlik doğrulaması olmadan uygulanabilir. Ancak, hedef kimlik doğrulaması yalnızca başlatıcı kimlik doğrulaması da uygulanmışsa uygulanabilir.

En iyi uygulama olarak, Iscsı güvenliğini geliştirmek için CHAP kullanmanızı öneririz.

> [!NOTE]
> IPSEC 'in Şu anda StorSimple cihazlarında desteklenmediğini aklınızda bulundurun.

StorSimple cihazındaki CHAP ayarları aşağıdaki yollarla yapılandırılabilir:

* Tek yönlü veya tek yönlü kimlik doğrulama
* Çift yönlü veya karşılıklı veya ters kimlik doğrulama

Bu durumların her birinde, cihazın ve sunucu Iscsı Başlatıcısı yazılımının portalının yapılandırılması gerekir. Bu yapılandırma için ayrıntılı adımlar aşağıdaki öğreticide açıklanmıştır.

## <a name="unidirectional-or-one-way-authentication"></a>Tek yönlü veya tek yönlü kimlik doğrulama

Tek yönlü kimlik doğrulamasında hedef, başlatıcısının kimliğini doğrular. Bu kimlik doğrulaması, StorSimple cihazında CHAP başlatıcısı ayarlarını ve konaktaki Iscsı Başlatıcısı yazılımını yapılandırmanızı gerektirir. StorSimple cihazınız ve Windows ana bilgisayarınız için ayrıntılı yordamlar daha sonra açıklanmıştır.

#### <a name="to-configure-your-device-for-one-way-authentication"></a>Cihazınızı tek yönlü kimlik doğrulama için yapılandırmak için

1. Azure portal, StorSimple Aygıt Yöneticisi hizmetinize gidin. **Cihazlar** ' a tıklayın ve CHAP yapılandırmak istediğiniz bir cihazı seçin ve tıklatın. **Cihaz ayarları > güvenlik**' e gidin. **Güvenlik ayarları** dikey penceresinde **CHAP**' ye tıklayın.
   
    ![CHAP başlatıcısı](./media/storsimple-8000-configure-chap/configure-chap5.png)
2. **CHAP** dikey penceresinde ve **CHAP başlatıcısı** bölümünde:
   
   1. CHAP başlatıcısı için bir Kullanıcı adı sağlayın.
   2. CHAP başlatıcısı için bir parola sağlayın.
      
      > [!IMPORTANT]
      > CHAP Kullanıcı adı 233 karakterden daha azını içermelidir. CHAP parolası 12 ile 16 karakter arasında olmalıdır. Daha uzun bir Kullanıcı adı veya parola, Windows ana bilgisayarında bir kimlik doğrulama hatasına neden olur.
   
   3. Parola’yı onaylayın.

       ![CHAP başlatıcısı](./media/storsimple-8000-configure-chap/configure-chap6.png)
3. **Kaydet**’e tıklayın. Bir onay iletisi görüntülenir. Değişiklikleri kaydetmek için **Tamam**’a tıklayın.

#### <a name="to-configure-one-way-authentication-on-the-windows-host-server"></a>Windows konak sunucusunda tek yönlü kimlik doğrulamasını yapılandırmak için
1. Windows konak sunucusunda, Iscsı Başlatıcısı 'nı başlatın.
2. **Iscsı Başlatıcısı Özellikler** penceresinde aşağıdaki adımları uygulayın:
   
   1. **Bulma** sekmesine tıklayın.
      
       ![iSCSI başlatıcısı özellikleri](./media/storsimple-configure-chap/IC740944.png)
   2. **Portalı bul**' a tıklayın.
3. **Hedef portalı bul** iletişim kutusunda:
   
   1. Cihazınızın IP adresini belirtin.
   2. **Gelişmiş**'e tıklayın.
      
       ![Hedef portalı bul](./media/storsimple-configure-chap/IC740945.png)
4. **Gelişmiş ayarlar** iletişim kutusunda:
   
   1. **CHAP oturumunu etkinleştir** onay kutusunu seçin.
   2. **Ad** alanına, Azure Portal CHAP başlatıcısı için belirttiğiniz kullanıcı adını sağlayın.
   3. **Hedef gizli** alanına, Azure Portal CHAP başlatıcısı için belirttiğiniz parolayı girin.
   4. **Tamam**'a tıklayın.
      
       ![Gelişmiş ayarlar genel](./media/storsimple-configure-chap/IC740946.png)
5. **Iscsı Başlatıcısı özellikleri** penceresinin **hedefler** sekmesinde cihaz durumu **bağlı**olarak görünmelidir. StorSimple 1200 cihazı kullanıyorsanız, her birim bir Iscsı hedefi olarak bağlanır. Bu nedenle, her birim için 3-4 adımlarının tekrarlanması gerekecektir.
   
    ![Ayrı hedefler olarak bağlanmış birimler](./media/storsimple-configure-chap/chap4.png)
   
   > [!IMPORTANT]
   > Iscsı adını değiştirirseniz yeni ad yeni Iscsı oturumları için kullanılır. Yeni ayarlar, oturumunuzu kapatıp yeniden oturum açana kadar mevcut oturumlar için kullanılmaz.

Windows konak sunucusunda CHAP yapılandırma hakkında daha fazla bilgi için, [ek noktalara göz önüne](#additional-considerations)geçin.

## <a name="bidirectional-or-mutual-authentication"></a>Çift yönlü veya karşılıklı kimlik doğrulaması

Çift yönlü kimlik doğrulamasında hedef, başlatıcısının kimliğini doğrular ve ardından başlatıcıya hedefin kimliğini doğrular. Bu yordam, kullanıcının CHAP başlatıcısı ayarlarını, cihazdaki ters CHAP ayarlarını ve konaktaki Iscsı Başlatıcısı yazılımını yapılandırmasını gerektirir. Aşağıdaki yordamlarda, cihazda ve Windows ana bilgisayarında karşılıklı kimlik doğrulamayı yapılandırma adımları açıklanır.

#### <a name="to-configure-your-device-for-mutual-authentication"></a>Cihazınızı karşılıklı kimlik doğrulaması için yapılandırmak için

1. Azure portal, StorSimple Aygıt Yöneticisi hizmetinize gidin. **Cihazlar** ' a tıklayın ve CHAP yapılandırmak istediğiniz bir cihazı seçin ve tıklatın. **Cihaz ayarları > güvenlik**' e gidin. **Güvenlik ayarları** dikey penceresinde **CHAP**' ye tıklayın.
   
    ![CHAP hedefi](./media/storsimple-8000-configure-chap/configure-chap5.png)
2. Bu sayfada ve **CHAP hedefi** bölümünde aşağı kaydırın:
   
   1. Cihazınız için **ters CHAP Kullanıcı adı** sağlayın.
   2. Cihazınız için **ters CHAP parolası** sağlayın.
   3. Parola’yı onaylayın.
3. **CHAP başlatıcısı** bölümünde:
   
   1. Cihazınız için bir **Kullanıcı adı** sağlayın.
   2. Cihazınız için bir **parola** girin.
   3. Parola’yı onaylayın.

       ![CHAP başlatıcısı](./media/storsimple-8000-configure-chap/configure-chap11.png)
4. **Kaydet**’e tıklayın. Bir onay iletisi görüntülenir. Değişiklikleri kaydetmek için **Tamam**’a tıklayın.

#### <a name="to-configure-bidirectional-authentication-on-the-windows-host-server"></a>Windows konak sunucusunda çift yönlü kimlik doğrulamasını yapılandırmak için

1. Windows konak sunucusunda, Iscsı Başlatıcısı 'nı başlatın.
2. **Iscsı Başlatıcısı Özellikler** penceresinde **yapılandırma** sekmesine tıklayın.
3. **CHAP**' ye tıklayın.
4. **Iscsı Başlatıcısı KARŞıLıKLı CHAP gizli** iletişim kutusunda:
   
   1. Azure portal yapılandırdığınız **ters CHAP parolasını** yazın.
   2. **Tamam**'a tıklayın.
      
       ![Iscsı Başlatıcısı karşılıklı CHAP parolası](./media/storsimple-configure-chap/IC740949.png)
5. **Hedefler** sekmesine tıklayın.
6. **Bağlan** düğmesine tıklayın. 
7. **Hedefe Bağlan** Iletişim kutusunda **Gelişmiş**' e tıklayın.
8. **Gelişmiş Özellikler** iletişim kutusunda:
   
   1. **CHAP oturumunu etkinleştir** onay kutusunu seçin.
   2. **Ad** alanına, Azure Portal CHAP başlatıcısı için belirttiğiniz kullanıcı adını sağlayın.
   3. **Hedef gizli** alanına, Azure Portal CHAP başlatıcısı için belirttiğiniz parolayı girin.
   4. **Karşılıklı kimlik doğrulaması gerçekleştir** onay kutusunu seçin.
      
       ![Gelişmiş ayarlar karşılıklı kimlik doğrulaması](./media/storsimple-configure-chap/IC740950.png)
   5. CHAP yapılandırmasını gerçekleştirmek için **Tamam** 'a tıklayın

Windows konak sunucusunda CHAP yapılandırma hakkında daha fazla bilgi için, [ek noktalara göz önüne](#additional-considerations)geçin.

## <a name="additional-considerations"></a>Diğer konular

**Hızlı Bağlan** ÖZELLIĞI, CHAP özellikli bağlantıları desteklemez. CHAP etkinleştirildiğinde, hedefle bağlantı kurmak için **hedefler** sekmesinde bulunan **Bağlan** düğmesini kullandığınızdan emin olun.

![Hedefe Bağlan](./media/storsimple-configure-chap/IC740947.png)

Sunulan **hedefe Bağlan** iletişim kutusunda **Bu bağlantıyı sık kullanılan hedefler listesine ekle** onay kutusunu seçin. Bu seçim, bilgisayar her yeniden başlatıldığında Iscsı Sık kullanılan hedeflerine bağlantıyı geri yüklemek için bir deneme yapılır.

## <a name="errors-during-configuration"></a>Yapılandırma sırasında hatalar

CHAP yapılandırmanız yanlışsa, büyük olasılıkla bir **kimlik doğrulama hatası** iletisi görürsünüz.

## <a name="verification-of-chap-configuration"></a>CHAP yapılandırması doğrulaması

Aşağıdaki adımları tamamlayarak CHAP 'nin kullanıldığını doğrulayabilirsiniz.

#### <a name="to-verify-your-chap-configuration"></a>CHAP yapılandırmanızı doğrulamak için
1. **Sık kullanılan hedefler**' e tıklayın.
2. Kimlik doğrulamasını etkinleştirdiğiniz hedefi seçin.
3. **Ayrıntılar**' a tıklayın.
   
    ![Iscsı başlatıcı özellikleri sık kullanılan hedefler](./media/storsimple-configure-chap/IC740951.png)
4. **Sık kullanılan hedef ayrıntıları** Iletişim kutusunda **kimlik doğrulaması** alanındaki girişi aklınızda edin. Yapılandırma başarılı olduysa, **CHAP**deyin.
   
    ![Sık kullanılan hedef ayrıntıları](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a>Sonraki adımlar

* [StorSimple güvenliği](storsimple-8000-security.md)hakkında daha fazla bilgi edinin.
* StorSimple [cihazınızı yönetmek Için storsimple Aygıt Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md)hakkında daha fazla bilgi edinin.

