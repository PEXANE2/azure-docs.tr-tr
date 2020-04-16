---
title: Personelim ile parolaları ve telefon numaralarını yönetme (önizleme) - Azure AD | Microsoft Dokümanlar
description: Personelim ile kullanıcılarınızın parolalarını ve telefon numaralarını yönetme
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.topic: article
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.date: 04/14/2020
ms.author: curtand
ms.reviewer: sahenry
ms.custom: oldportal;it-pro;
ms.openlocfilehash: c5804d9e386ff1b17c08fc4ce4f3a232ebe1a39b
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81394442"
---
# <a name="delegate-user-management-with-my-staff-preview"></a>Personelim ile kullanıcı yönetimini temsilci (önizleme)

Kuruluşunuz, personelinin gereksinim duydukları uygulamalara erişmelerine yardımcı olmak için kullanıcı yönetimi görevlerini mağaza yöneticisi veya ekip lideri gibi yetkili kişilere devretmek için **Personelimi** kullanabilir. Ekip üyeniz parolayı unuttukları için bir uygulamaya erişemezse üretkenlik kaybolur. Bu aynı zamanda destek maliyetlerini de azaltır ve idari işlemlerinizde darboğaza neden olur.  Personelim ile, hesabına erişemeyen bir ekip üyesi, yönetici yardımı gerektirmeden sadece birkaç tıklamayla yeniden erişebilir.

## <a name="manage-your-staff-in-my-staff"></a>Personelinizi Personelim'de yönetin

Ekibinizi Personelim'de yönetmek çok kolay. Başlamak için [Personelim'e gidin,](https://aka.ms/mystaff)bir takım veya konum seçin ve ardından bir kullanıcı seçin. Konumlar ve bir konumdaki ekip üyeleri BT yöneticiniz tarafından belirlenir ve bunları değiştiremezsiniz.

Birden fazla konum yönetiyorsanız, Personelim'e gittiğinizde, konuma atanan ekip üyesini görmek için bir konum seçmeniz gerekir.

Personelime erişmek için henüz yeterli izniniz yoksa, şu mesajı görürsünüz : "Aman tanrım, şu anda Personelimi görme yetkiniz yok gibi görünüyor. Daha fazla bilgi için lütfen yöneticinizle iletişime geçin."

### <a name="find-a-staff-member-in-my-staff"></a>Personelim'de bir personel bulun

Yönetmeye başlamadan önce bir personelin profilini açmanız gerekir.

1. [Personelimi açın](https://aka.ms/mystaff) ve gerekirse bir konum seçin.

    ![Personelim'deki bir ekip üyesi için konum seçin](media/my-staff-team-manager/allaus.png)

1. Bir ekip üyesinin profilini açın.

    ![Personelim'deki bir konumdaki kullanıcılardan birini seçin](media/my-staff-team-manager/aupage.png)

## <a name="reset-a-user-password"></a>Kullanıcı parolasını sıfırlama

Kuruluşunuz size izin verdiyse, personelinizin parolalarını sıfırlayabilirsiniz.

1. [Personelimi Açın.](https://aka.ms/mystaff)
1. Bir personelin profilini açın.
1. **Parolayı Sıfırla'yı**seçin.

    ![Personelim'de kullanıcı parolası sıfırlama](media/my-staff-team-manager/resetpassword1.png)

1. Yeni parola oluşturun veya girin. Otomatik olarak oluşturulan geçici bir parola gösterilebilir veya kullanıcı için geçici bir parola girmeniz istenebilir.

    ![Personelim'de sıfırladıktan sonra geçici kullanıcı parolasını kopyalama](media/my-staff-team-manager/resetpassword2.png)

Kullanıcının parolasını sıcandıktan sonra, kullanıcıya geçici parolayı verin. Kullanıcı geçici parolasıyla birlikte kaydolduğunda, parolayı değiştirmeleri gerekir.

## <a name="manage-a-users-phone-number"></a>Kullanıcının telefon numarasını yönetme

Kuruluşunuz size izin verdiyse, personelinizin telefon numaralarını yönetebilirsiniz.

### <a name="add-a-phone-number"></a>Telefon numarası ekleme

1. [Personelimi Açın.](https://aka.ms/mystaff)
1. Bir personelin profilini açın.
1. **Telefon numarası ekle'yi**seçin.

    ![Personelime kullanıcı telefon numarası ekleme](media/my-staff-team-manager/addphone1.png)

1. Telefon numarasını ekleyin ve **Kaydet'i**seçin.

    ![Eklenen kullanıcı telefon numarasını Personelim'e kaydetme](media/my-staff-team-manager/addphone2.png)

Bir kullanıcı için bir telefon numarası kaydettikten sonra, bu numarayı SMS ile oturum açma, iki aşamalı doğrulama gerçekleştirmek veya kuruluşunuzun ayarlarına bağlı olarak parolalarını sıfırlamak için kullanabilirler.

![Personelime kayıtlı yeni telefon numarası](media/my-staff-team-manager/addphone3.png)

### <a name="edit-a-phone-number"></a>Telefon numarasını edin

1. [Personelimi Açın.](https://aka.ms/mystaff)
1. Bir personelin profilini açın.
1. **Telefon numarasını edit'i**seçin.

    ![Personelim'deki kullanıcı profilinden Edit'i seçin](media/my-staff-team-manager/editphone2.png)

1. Yeni telefon numarasını girin ve **Kaydet'i**seçin.

    ![Personelim'de çalışan bir telefon numarasını edin](media/my-staff-team-manager/editphone1.png)

### <a name="enable-phone-number-sign-in-for-a-user"></a>Bir kullanıcı için telefon numarası oturum açmayı etkinleştirme

Kuruluşunuzda kullanıcı adı (SMS oturum açma) olarak bir telefon numarası kullanarak oturum açma etkinse, bu kimlik doğrulamasını varolan bir kullanıcı telefon numarasına ekleyebilirsiniz.

1. [Personelimi Açın.](https://aka.ms/mystaff)
1. Bir personelin profilini açın.
1. Ekranın alt kısmında, telefon numaranızla oturum açmanın sizin için kullanılabilir olduğunu belirten bir ileti varsa, işlemi başlatmak için **Etkinleştir'i** seçin. Kullanıcının telefon numarasıyla oturum açması etkinleştirildiyse, bu ileti görüntülenir.

    ![Personelim'deki bir konumda telefonla oturum açma desteklendiğinde iletiyi görün](media/my-staff-team-manager/enableforms1.png)

1. Bittiğinde **Tamam'ı** seçin.

    ![Personelimdeki bir personel telefon numarasını kaldırma](media/my-staff-team-manager/enableforms2.png)

### <a name="remove-a-phone-number"></a>Telefon numarasını kaldırma

1. [Personelimi Açın.](https://aka.ms/mystaff)
1. Bir personelin profilini açın.
1. **Telefon numarasını kaldır'ı**seçin.
1. Bittiğinde **Sil'i** seçin.

    ![Personelimdeki bir personel telefon numarasını kaldırma](media/my-staff-team-manager/deletephone1.png)
