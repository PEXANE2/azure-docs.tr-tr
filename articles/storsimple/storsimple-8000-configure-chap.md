---
title: StorSimple 8000 serisi cihaz için CHAP'ı yapılandırın | Microsoft Dokümanlar
description: Bir StorSimple aygıtında Challenge Handshake Kimlik Doğrulama Protokolü'nün (CHAP) nasıl yapılandırılabildiğini açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: TBD
ms.date: 05/09/2018
ms.author: alkohli
ms.openlocfilehash: efc116c278bfe72419800603a3b365f461fe0a28
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267969"
---
# <a name="configure-chap-for-your-storsimple-device"></a>StorSimple cihazınız için CHAP'yi yapılandırın

Bu öğretici, StorSimple aygıtınız için CHAP'yi nasıl yapılandırıştırılabildiğini açıklar. Bu makalede ayrıntılı yordam StorSimple 8000 serisi cihazlar için geçerlidir.

CHAP Challenge El Sıkışma Kimlik Doğrulama Protokolü anlamına gelir. Uzak istemcilerin kimliğini doğrulamak için sunucular tarafından kullanılan bir kimlik doğrulama düzenidir. Doğrulama paylaşılan bir parolaya veya gizliye dayanır. CHAP tek yönlü (tek yönlü) veya karşılıklı (çift yönlü) olabilir. Chap'ın bir yolu, hedefin bir başlatıcıyı doğrulamasıdır. Karşılıklı veya ters CHAP'de, hedef başlatıcıyı doğrular ve başlatıcı hedefin kimliğini doğrular. Başlatıcı kimlik doğrulaması hedef kimlik doğrulaması olmadan uygulanabilir. Ancak, hedef kimlik doğrulaması yalnızca başlatıcı kimlik doğrulaması da uygulanıyorsa uygulanabilir.

En iyi uygulama olarak, iSCSI güvenliğini artırmak için CHAP'yi kullanmanızı öneririz.

> [!NOTE]
> IPSEC'in şu anda StorSimple cihazlarında desteklenmediğini unutmayın.

StorSimple cihazındaki CHAP ayarları aşağıdaki şekilde yapılandırılabilir:

* Tek yönlü veya tek yönlü kimlik doğrulama
* Çift yönlü veya karşılıklı veya ters kimlik doğrulama

Bu durumların her birinde, aygıt portalı ve sunucu iSCSI başlatıcı yazılımının yapılandırılması gerekir. Bu yapılandırma için ayrıntılı adımlar aşağıdaki öğretici açıklanmıştır.

## <a name="unidirectional-or-one-way-authentication"></a>Tek yönlü veya tek yönlü kimlik doğrulama

Tek yönlü kimlik doğrulamada, hedef başlatıcıyı doğrular. Bu kimlik doğrulaması, StorSimple aygıtındaki CHAP başlatıcısı ayarlarını ve ana bilgisayardaki iSCSI Başlatıcı yazılımını yapılandırmanızı gerektirir. StorSimple aygıtınız ve Windows ana bilgisayarınız için ayrıntılı yordamlar daha sonra açıklanmıştır.

#### <a name="to-configure-your-device-for-one-way-authentication"></a>Cihazınızı tek yönlü kimlik doğrulaması için yapılandırmak için

1. Azure portalında StorSimple Device Manager hizmetinize gidin. **Cihazlar'ı** tıklatın ve CHAP'yi yapılandırmak istediğiniz bir aygıtı seçin ve tıklatın. Güvenlik **> Aygıt ayarlarına**gidin. Güvenlik **ayarları** bıçak, **CHAP**tıklatın.
   
    ![CHAP Başlatıcısı](./media/storsimple-8000-configure-chap/configure-chap5.png)
2. **CHAP** bıçak ve CHAP **Başlatıcı** bölümünde:
   
   1. CHAP başlatıcınız için bir kullanıcı adı sağlayın.
   2. CHAP başlatıcınız için bir parola girin.
      
      > [!IMPORTANT]
      > CHAP kullanıcı adı 233 karakterden az olmalıdır. CHAP parolası 12 ile 16 karakter arasında olmalıdır. Daha uzun bir kullanıcı adı veya parola, Windows ana bilgisayarda kimlik doğrulama hatasıyla sonuçlanır.
   
   3. Parola’yı onaylayın.

       ![CHAP Başlatıcısı](./media/storsimple-8000-configure-chap/configure-chap6.png)
3. **Kaydet**'e tıklayın. Bir onay iletisi görüntülenir. Değişiklikleri kaydetmek için **Tamam**’a tıklayın.

#### <a name="to-configure-one-way-authentication-on-the-windows-host-server"></a>Windows ana bilgisayar sunucusunda tek yönlü kimlik doğrulaması yapılandırmak için
1. Windows ana bilgisayar sunucusunda iSCSI Başlatıcısını başlatın.
2. **iSCSI Başlatıcı Özellikleri** penceresinde aşağıdaki adımları gerçekleştirin:
   
   1. **Bulma** sekmesini tıklatın.
      
       ![iSCSI başlatıcısı özellikleri](./media/storsimple-configure-chap/IC740944.png)
   2. **Portalı Keşfet'i**tıklatın.
3. Hedef **Portalı Keşfet** iletişim kutusunda:
   
   1. Cihazınızın IP adresini belirtin.
   2. **Gelişmiş**'e tıklayın.
      
       ![Hedef portalı keşfedin](./media/storsimple-configure-chap/IC740945.png)
4. Gelişmiş **Ayarlar** iletişim kutusunda:
   
   1. Onay kutusundaKI **CHAP oturum aç'ını etkinleştir'i** seçin.
   2. **Ad** alanında, Azure portalında CHAP Başlatıcısı için belirttiğiniz kullanıcı adını girin.
   3. Hedef **gizli** alanında, Azure portalında CHAP Başlatıcısı için belirlediğiniz parolayı girin.
   4. **Tamam**'a tıklayın.
      
       ![Gelişmiş ayarlar genel](./media/storsimple-configure-chap/IC740946.png)
5. **iSCSI Başlatıcı Özellikleri** penceresinin **Hedefler** sekmesinde, aygıt durumu **Bağlı**olarak görünmelidir. StorSimple 1200 aygıtı kullanıyorsanız, her birim iSCSI hedefi olarak monte edilir. Bu nedenle, adımlar 3-4 her ses için tekrarlanmalıdır.
   
    ![Ayrı hedef olarak monte edilen birimler](./media/storsimple-configure-chap/chap4.png)
   
   > [!IMPORTANT]
   > iSCSI adını değiştirirseniz, yeni iSCSI oturumları için yeni ad kullanılır. Yeni ayarlar, siz oturumu kapatın ve yeniden oturum açtırana kadar varolan oturumlar için kullanılmaz.

CHAP'yi Windows ana bilgisayar sunucusunda yapılandırma hakkında daha fazla bilgi için [Ek hususlara](#additional-considerations)gidin.

## <a name="bidirectional-or-mutual-authentication"></a>Çift yönlü veya karşılıklı kimlik doğrulama

Çift yönlü kimlik doğrulamada, hedef başlatıcıyı doğrular ve başlatıcı hedefin kimliğini doğrular. Bu yordam, kullanıcının CHAP başlatıcıayarlarını yapılandırmasını, aygıttaki CHAP ayarlarını tersine çevirmesini ve ana bilgisayardaki iSCSI Başlatıcı yazılımını yapılandırmasını gerektirir. Aşağıdaki yordamlar, aygıtta ve Windows ana bilgisayarda karşılıklı kimlik doğrulaması yapılandırmaadımlarını açıklar.

#### <a name="to-configure-your-device-for-mutual-authentication"></a>Cihazınızı karşılıklı kimlik doğrulaması için yapılandırmak için

1. Azure portalında StorSimple Device Manager hizmetinize gidin. **Cihazlar'ı** tıklatın ve CHAP'yi yapılandırmak istediğiniz bir aygıtı seçin ve tıklatın. Güvenlik **> Aygıt ayarlarına**gidin. Güvenlik **ayarları** bıçak, **CHAP**tıklatın.
   
    ![CHAP hedefi](./media/storsimple-8000-configure-chap/configure-chap5.png)
2. Bu sayfada ve **CHAP Hedef** bölümünde aşağı kaydırın:
   
   1. Cihazınız için bir **Ters CHAP kullanıcı adı** sağlayın.
   2. Cihazınız için ters **CHAP parolası** girin.
   3. Parola’yı onaylayın.
3. CHAP **Başlatıcı** bölümünde:
   
   1. Cihazınız için bir **kullanıcı adı** sağlayın.
   2. Cihazınız için bir **parola** sağlayın.
   3. Parola’yı onaylayın.

       ![CHAP Başlatıcısı](./media/storsimple-8000-configure-chap/configure-chap11.png)
4. **Kaydet**'e tıklayın. Bir onay iletisi görüntülenir. Değişiklikleri kaydetmek için **Tamam**’a tıklayın.

#### <a name="to-configure-bidirectional-authentication-on-the-windows-host-server"></a>Windows ana bilgisayar sunucusunda çift yönlü kimlik doğrulamayı yapılandırmak için

1. Windows ana bilgisayar sunucusunda iSCSI Başlatıcısını başlatın.
2. **iSCSI Başlatıcı Özellikleri** penceresinde **Yapılandırma** sekmesini tıklatın.
3. **CHAP'yi**tıklatın.
4. **iSCSI Başlatıcı Karşılıklı CHAP Gizli** iletişim kutusunda:
   
   1. Azure portalında yapılandırdığınız **Ters CHAP Parolasını** yazın.
   2. **Tamam**'a tıklayın.
      
       ![iSCSI başlatıcı karşılıklı CHAP gizli](./media/storsimple-configure-chap/IC740949.png)
5. **Hedefler** sekmesini tıklatın.
6. **Bağlan** düğmesini tıklatın. 
7. Hedefe **Bağlan** iletişim kutusunda **Gelişmiş'i**tıklatın.
8. Gelişmiş **Özellikler** iletişim kutusunda:
   
   1. Onay kutusundaKI **CHAP oturum aç'ını etkinleştir'i** seçin.
   2. **Ad** alanında, Azure portalında CHAP Başlatıcısı için belirttiğiniz kullanıcı adını girin.
   3. Hedef **gizli** alanında, Azure portalında CHAP Başlatıcısı için belirlediğiniz parolayı girin.
   4. Ortak **kimlik doğrulama onay** kutusunu gerçekleştir'i seçin.
      
       ![Gelişmiş ayarlar karşılıklı kimlik doğrulama](./media/storsimple-configure-chap/IC740950.png)
   5. CHAP yapılandırmasını tamamlamak için **Tamam'ı** tıklatın

CHAP'yi Windows ana bilgisayar sunucusunda yapılandırma hakkında daha fazla bilgi için [Ek hususlara](#additional-considerations)gidin.

## <a name="additional-considerations"></a>Diğer konular

**Hızlı Bağlantı** özelliği, CHAP özellikli bağlantıları desteklemez. CHAP etkinleştirildiğinde, hedefe bağlanmak için **Hedefler** sekmesinde bulunan **Bağlan** düğmesini kullandığınızdan emin olun.

![Hedefe bağlanma](./media/storsimple-configure-chap/IC740947.png)

Sunulan **Hedefe Bağlan** iletişim kutusunda, Bu bağlantıyı Sık Kullanılan Hedefler onay **kutusuna ekle'yi** seçin. Bu seçim, bilgisayar her yeniden başlatıncı, iSCSI sık kullanılan hedeflere bağlantıyı geri yüklemek için bir girişimde bulunmayı sağlar.

## <a name="errors-during-configuration"></a>Yapılandırma sırasındaki hatalar

CHAP yapılandırmanız yanlışsa, kimlik doğrulama **hatası** iletisi görme olasılığınız yüksektir.

## <a name="verification-of-chap-configuration"></a>CHAP yapılandırmasının doğrulanması

Aşağıdaki adımları tamamlayarak CHAP'nin kullanıldığını doğrulayabilirsiniz.

#### <a name="to-verify-your-chap-configuration"></a>CHAP yapılandırmanızı doğrulamak için
1. **Sık Kullanılan Hedefler'i**tıklatın.
2. Kimlik doğrulamasını etkinleştirdiğiniz hedefi seçin.
3. **Ayrıntılar'ı**tıklatın.
   
    ![iSCSI başlatıcı özellikleri favori hedefleri](./media/storsimple-configure-chap/IC740951.png)
4. Sık **Kullanılan Hedef Ayrıntıları** iletişim kutusunda, Kimlik Doğrulama alanındaki girişi not **edin.** Yapılandırma başarılı olduysa, **CHAP**demelidir.
   
    ![Favori hedef ayrıntıları](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a>Sonraki adımlar

* [StorSimple güvenliği](storsimple-8000-security.md)hakkında daha fazla bilgi edinin.
* [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanma](storsimple-8000-manager-service-administration.md)hakkında daha fazla bilgi edinin.

