---
title: En son oturum açma etkinliğinizi Oturum Açma (önizleme) sayfasından görüntüleyin ve arama yapın - Azure Etkin Dizini | Microsoft Dokümanlar
description: Hesabım portalının Oturum Açma lar sayfasından son oturum açma etkinliğinizi nasıl görüntüleyip arayacağınız hakkında ayrıntılar.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: rhicock
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 10/28/2019
ms.author: curtand
ms.openlocfilehash: b68e7b517ddaa9b2aaef00cf87d5b6e63871654b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77064028"
---
# <a name="view-and-search-your-recent-sign-in-activity-from-the-my-sign-ins-preview-page"></a>En son oturum açma etkinliğinizi Oturum Açma (önizleme) sayfasından görüntüleyin ve arama yapın

**Hesabım** portalının **Oturum Açmalar** sayfasından, son çalışma veya okul hesabı oturum açma etkinliğinizi görüntüleyebilirsiniz. Oturum açma geçmişinizi gözden geçirmek, görmenize yardımcı olarak olağandışı etkinlikleri kontrol etmeye yardımcı olur:

- Birisi parolanızı tahmin etmeye çalışıyorsa.

- Bir saldırgan hesabınızda başarılı bir şekilde oturum açmışsa ve hangi konumdan.

- Saldırganın erişmeye çalıştığı uygulamalar.

## <a name="view-your-recent-sign-in-activity"></a>Son oturum açma etkinliğinizi görüntüleme

1. İş veya okul hesabınızda oturum açın https://myprofile.microsoft.com/ ve sayfanıza gidin.

2. Sol daki gezinme bölmesinden **Oturum Açma(önizleme)** seçeneğini belirleyin veya **Oturum Açma (önizleme)** bloğundan son etkinlik bağlantısını **gözden geçir'i** seçin.

    ![Vurgulanan Son etkinlik bağlantılarını gösteren Hesabım sayfam](media/my-account-portal/my-account-portal-sign-ins.png)

3. Oturum açma öğelerinin her birini genişletin ve gözden geçirerek her birini tanıdığınızı belirtin. Tanıdık görünmeyen bir oturum açma öğesi bulursanız, hesabınızın gizliliğine yardımcı olmak için parolanızı değiştirmenizi öneririz.

    ![Genişletilmiş oturum açma ayrıntılarını içeren son etkinlik sayfası](media/my-account-portal/my-account-portal-sign-ins-page.png)

### <a name="if-you-see-a-successful-sign-in"></a>Başarılı bir oturum açma görürseniz

Kendi aktivitenizi normal olarak tanımalısınız. Ancak, garip bir konumdan, tarayıcıdan veya işletim sisteminden başarılı bir oturum açma fark ederseniz, bu saldırganın hesabınıza erişmiş olduğu anlamına gelebilir. Bu durumda, parolanızı hemen değiştirmenizi ve ardından güvenlik ayarlarınızı güncellemek için [Güvenlik bilgileri](https://mysignins.microsoft.com/security-info) sayfasına gitmenizi öneririz.

Bir şeyin yanlış olduğunu belirlemeden önce, yanlış pozitif görmediğinden emin olun (öğenin şüpheli göründüğü, ancak iyi olduğu durumlarda). Örneğin, ip adresinizi temel alan yaklaşık konumunuzu ve haritanızı belirleriz. Mobil ağları niçin bazen uzak konumlara yönlendirdikleri için tespit etmek özellikle zordur. Bu nedenle, Washington eyaletinde mobil cihazınızı kullanarak oturum açtıysanız, konum California'dan gelen oturum açma yı gösterebilir. Bu nedenle, sadece konumun ötesinde daha fazla ayrıntıyı kontrol etmenizi öneririz. Ayrıca işletim sistemi, tarayıcı ve uygulama tüm mantıklı olduğundan emin olmalısınız, çok.

### <a name="if-you-see-an-unsuccessful-sign-in"></a>Başarısız bir oturum açma görürseniz

Oturum etkinliği olmayan başarısız bir oturum açma, birincil doğrulama yönteminizin (kullanıcı adı/parola) başarısız olduğu anlamına gelir. Bu, kullanıcı adınızı veya parolanızı yanlış yazdığınız anlamına gelebilir, ancak saldırganın parolanızı tahmin etmeye çalıştığı anlamına da gelebilir. Parolanızı tahmin etmek için başarısız bir şekilde çalışan saldırgan olduğunu düşünüyorsanız, parolanızı değiştirmeniz gerekmemektedir, ancak Azure Çok Faktörlü Kimlik Doğrulaması 'na (MFA) kaydolmanızı öneririz. MFA ile, bilgisayar korsanı sonunda parolanızı tahmin etse bile, hesabınıza erişmek için yeterli olmayacaktır.

Oturum etkinliği altında ek **doğrulama başarısız, geçersiz kod**yazan bir notla başarısız bir oturum açma görürseniz, bu birincil kimlik doğrulamanızın (kullanıcı adı/parola) başarılı olduğu, ancak MFA'nın başarısız olduğu anlamına gelir. Bu bir saldırgansa, parolanızı doğru tahmin ettiler, ancak MFA meydan okumasını yine de geçemediler. Bu durumda, saldırgan bu bölümü doğru yaptığından parolanızı değiştirmeye devam etmenizi ve ardından güvenlik ayarlarınızı güncelleştirmek için [Güvenlik bilgileri](https://mysignins.microsoft.com/security-info) sayfasına gitmenizi öneririz.

## <a name="search-for-specific-sign-in-activity"></a>Belirli oturum açma etkinliğini arama

En son oturum açma etkinliğinizde mevcut bilgilerden herhangi biri tarafından arama yapabilirsiniz. Örneğin, son oturum açma etkinliğinizi işletim sistemine, konuma, uygulamaya ve benzeri ne redeleyerek arayabilirsiniz.

1. Son **etkinliği İncele** sayfasında, aramak istediğiniz bilgileri **Arama** çubuğuna yazın. Örneğin, Hesabım uygulaması tarafından toplanan tüm etkinlikleri aramak için yazın. `My Account`

2. Aramaya başlamak için **Arama** düğmesini seçin.

    ![Vurgulanan arama çubuğu, arama düğmesi ve sonuçları gösteren Son Etkinlik sayfası](media/my-account-portal/my-account-portal-sign-ins-page-search.png)

## <a name="next-steps"></a>Sonraki adımlar

Son oturum açma etkinliğinizi görüntüledikten sonra şunları yapabilirsiniz:

- [Güvenlik bilgilerinizi](user-help-security-info-overview.md)görüntüleyin veya yönetin.

- Bağlı [cihazlarınızı](my-account-portal-devices-page.md)görüntüleyin veya yönetin.

- [Kuruluşlarınızı](my-account-portal-organizations-page.md)görüntüleyin veya yönetin.

- Kuruluşunuzun [gizlilikle ilgili verilerinizi](my-account-portal-privacy-page.md)nasıl kullandığını görüntüleyin.
