---
title: Grup lisanslarına doğrudan lisans alan kullanıcılar ekleme - Azure AD | Microsoft Dokümanlar
description: Azure Active Directory'yi kullanarak bireysel kullanıcı lisanslarından grup tabanlı lisansa geçiş nasıl
services: active-directory
keywords: Azure AD lisanslama
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: users-groups-roles
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: sumitp
ms.custom: seohack1;it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6c06d81f2f3f6cee781889d05ae08a1fd125df52
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74025674"
---
# <a name="how-to-migrate-users-with-individual-licenses-to-groups-for-licensing"></a>Tek tek lisansları olan kullanıcıların lisans gruplarına nasıl geçirilir?

Kuruluşlardaki kullanıcılara doğrudan atama yoluyla dağıtılan varolan lisanslarınız olabilir; diğer bir deyişle, tek tek kullanıcı lisansları atamak için PowerShell komut dosyaları veya diğer araçları kullanarak. Kuruluşunuzdaki lisansları yönetmek için grup tabanlı lisanslama kullanmaya başlamadan önce, varolan çözümleri grup tabanlı lisanslamayla sorunsuz bir şekilde değiştirmek için bu geçiş planını kullanabilirsiniz.

Akılda tutulması gereken en önemli şey, grup tabanlı lisansgeçiş kullanıcıların geçici olarak atanan lisansları kaybetme neden olacağı bir durumdan kaçınmak gerektiğidir. Kullanıcıların hizmetlere ve verilerine erişimini kaybetme riskini ortadan kaldırmak için lisansların kaldırılmasıyla sonuçlanabilecek her türlü işlemden kaçınılmalıdır.

## <a name="recommended-migration-process"></a>Önerilen geçiş işlemi

1. Kullanıcılar için lisans atamasını ve kaldırılmasını yöneten varolan otomasyona (örneğin PowerShell) sahipsiniz. Olduğu gibi çalışır durumda bırak.

1. Yeni bir lisans grubu oluşturun (veya hangi grupları kullanacağına karar verin) ve gerekli tüm kullanıcıların üye olarak eklendiğine emin olun.

1. Bu gruplara gerekli lisansları atamak; amacınız, mevcut otomasyonunuzun (örneğin PowerShell) bu kullanıcılara uyguladığı lisans durumunu yansıtmak olmalıdır.

1. Lisansların bu gruplardaki tüm kullanıcılara uygulandığını doğrulayın. Bu uygulama, her gruptaki işlem durumunu kontrol ederek ve Denetim Günlükleri'ni kontrol ederek yapılabilir.

   - Tek tek kontrol bilgilerini lisans bilgilerine bakarak tek tek kontrol edebilirsiniz. Gruplardan "doğrudan" ve "devralınan" aynı lisanslara sahip olduklarını göreceksiniz.

   - [Lisansların kullanıcılara nasıl atandığını doğrulamak için](licensing-group-advanced.md#use-powershell-to-see-who-has-inherited-and-direct-licenses)bir PowerShell komut dosyası çalıştırabilirsiniz.

   - Aynı ürün lisansı kullanıcıya hem doğrudan hem de bir grup aracılığıyla atandığında, kullanıcı tarafından yalnızca bir lisans tüketilir. Bu nedenle geçiş gerçekleştirmek için ek lisans gerekmez.

1. Hata durumundaki kullanıcılar için her grubu denetleyerek hiçbir lisans atamalarının başarısız olmadığını doğrulayın. Daha fazla bilgi için [bkz.](licensing-groups-resolve-problems.md)

Özgün doğrudan atamaları kaldırmayı düşünün. Bunu kademeli olarak yapmanızı ve sonucu önce bir kullanıcı alt kümesinde izlemenizi öneririz. Orijinal doğrudan atamaları kullanıcılara bırakabilirseniz, ancak kullanıcılar lisanslı gruplarından ayrıldıklarında doğrudan atanan lisansları korurlar, bu da istediğiniz gibi olmayabilir.

## <a name="an-example"></a>Bir örnek

Bir kuruluşun 1.000 kullanıcısı vardır. Tüm kullanıcılar Office 365 Enterprise E3 lisanslarına ihtiyaç duyar. Şu anda kuruluş, gelir ve gider gibi kullanıcıların lisansları ekleyerek ve kaldırarak, şirket içinde çalışan bir PowerShell komut dosyası vardır. Ancak kuruluş, lisansların Azure AD tarafından otomatik olarak yönetilebilmeleri için komut dosyasını grup tabanlı lisanslamayla değiştirmek istiyor.

Geçiş işlemi şu şekilde görünebilir:

1. Azure portalını kullanarak, Office 365 E3 lisansını Azure AD'deki **Tüm kullanıcılar** grubuna atayın.

1. Lisans atamanın tüm kullanıcılar için tamamlandığını doğrulayın. Grup için genel bakış sayfasına gidin, **Lisanslar'ı**seçin ve **Lisanslar** bıçağının üst kısmındaki işlem durumunu kontrol edin.

   - İşlemin tamamlandığını doğrulamak için "Tüm kullanıcılara en son lisans değişiklikleri uygulandı" diye bakın.

   - Lisansları başarıyla atanmamış olabilecek kullanıcılar hakkında en üstte bir bildirim arayın. Bazı kullanıcıların lisansları bitti mi? Bazı kullanıcıların grup lisanslarını devralmalarını engelleyen çakışan lisans planları var mı?

1. Hem doğrudan hem de grup lisanslarına sahip olduklarını doğrulamak için bazı kullanıcıları işaretleyin. Bir kullanıcının profil sayfasına gidin, **Lisanslar'ı**seçin ve lisansların durumunu inceleyin.

   - Bu, geçiş sırasında beklenen kullanıcı durumudur:

      ![geçiş sırasında beklenen kullanıcı durumu](./media/licensing-groups-migrate-users/expected-user-state.png)

     Bu, kullanıcının hem doğrudan hem de devralınan lisanslara sahip olduğunu doğrular. Office 365 E3'ün atandığını görüyoruz.

   - Hangi hizmetlerin etkin olduğunu görmek için her lisansı seçin. Doğrudan ve grup lisanslarının kullanıcı için tam olarak aynı hizmetleri etkinleştirdiğini doğrulamak için **Atamalar'ı**seçin.

1. Hem doğrudan hem de grup lisanslarının eşdeğer olduğunu doğruladıktan sonra, kullanıcılardan doğrudan lisansları kaldırmaya başlayabilirsiniz. Bunu portaldaki tek tek kullanıcılar için kaldırarak sınayabilir ve ardından bunları toplu olarak kaldırmak için otomasyon komut dosyalarını çalıştırabilirsiniz. Burada portal üzerinden kaldırılan doğrudan lisansları ile aynı kullanıcının bir örnektir. Lisans durumunun değişmeden kaldığına dikkat edin, ancak artık doğrudan atamalar görmüyoruz.

   ![doğrudan lisansların kaldırıldığını onaylamak](./media/licensing-groups-migrate-users/direct-licenses-removed.png)

## <a name="next-steps"></a>Sonraki adımlar

Grup lisans yönetimi için diğer senaryolar hakkında daha fazla bilgi edinin:

- [Azure Active Directory'de grup tabanlı lisanslama nedir?](../fundamentals/active-directory-licensing-whatis-azure-portal.md)
- [Azure Active Directory'de gruba lisans atama](licensing-groups-assign.md)
- [Azure Active Directory'de grubun lisans sorunlarını tanımlama ve çözme](licensing-groups-resolve-problems.md)
- [Azure Active Directory'de grup tabanlı lisanslama yı kullanarak kullanıcıların ürün lisansları arasında geçiş ilertirme](licensing-groups-change-licenses.md)
- [Azure Active Directory grup tabanlı lisanslamayla ilgili ek senaryolar](licensing-group-advanced.md)
- [Azure Active Directory'de grup tabanlı lisanslama için PowerShell örnekleri](licensing-ps-examples.md)
