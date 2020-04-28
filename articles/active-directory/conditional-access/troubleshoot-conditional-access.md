---
title: Koşullu erişim ile oturum açma sorunlarını giderme-Azure Active Directory
description: Bu makalede, koşullu erişim ilkeleriniz beklenmedik sonuçlar elde edildiğinde ne yapmanız gerekenler açıklanmaktadır
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: troubleshooting
ms.date: 03/13/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, martinco
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdf1daca79f3ed20d9b7a89af20d74ff5f3148b5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80337447"
---
# <a name="troubleshooting-sign-in-problems-with-conditional-access"></a>Koşullu erişimle oturum açma sorunlarını giderme

Bu makaledeki bilgiler, hata iletileri ve Azure AD oturum açma günlüğü kullanılarak koşullu erişimle ilgili beklenmedik oturum açma sonuçlarının giderilmesi için kullanılabilir.

## <a name="conditional-access-sign-in-interrupt"></a>Koşullu erişim oturum açma kesmesi

İlk yöntem görüntülenen hata iletisini gözden geçirmesidir. Bir Web tarayıcısı kullanırken oturum açan sorunlar için, hata sayfasında ayrıntılı bilgiler bulunur. Bu bilgiler, yalnızca sorunun ne olduğunu ve bir çözüm önerebileceğini betimedebilir.

![Oturum açma hatasıyla uyumlu cihaz gerekiyor](./media/troubleshoot-conditional-access/image1.png)

Yukarıdaki hatada, ileti uygulamanın yalnızca şirketin mobil cihaz yönetimi ilkesini karşılayan cihazlardan veya istemci uygulamalarından erişilebilir olduğunu belirtir. Bu durumda, uygulama ve cihaz bu ilkeyi karşılamıyor.

## <a name="azure-ad-sign-in-events"></a>Azure AD oturum açma olayları

Oturum açma kesintisini hakkında ayrıntılı bilgi almak için ikinci yöntem, hangi koşullu erişim ilkesi veya ilkelerinin uygulanacağını ve bunun neden olduğunu görmek için Azure AD oturum açma olaylarını gözden geçirmelidir.

İlk hata sayfasında **daha fazla ayrıntı** öğesine tıklanarak sorun hakkında daha fazla bilgi bulabilirsiniz. **Daha fazla ayrıntı** için, kullanıcının Microsoft ile bir destek olayı görmekte veya hata OLUŞTUĞUNDA Azure AD oturum açma olaylarını ararken yararlı olan sorun giderme bilgileri ortaya çıkar.

![Koşullu erişim kesintiye uğramış Web tarayıcı oturum açma bilgileri hakkında daha fazla bilgi.](./media/troubleshoot-conditional-access/image2.png)

Hangi koşullu erişim ilkesinin veya ilkelerinin uygulanacağını ve bunun nedenini nasıl yapabileceğinizi öğrenmek için.

1. **Azure Portal** genel yönetici, güvenlik yöneticisi veya küresel okuyucu olarak oturum açın.
1. **Azure Active Directory** > **oturum açma**işlemlerini inceleyin.
1. Gözden geçirilecek oturum açma için olayı bulun. Gereksiz bilgileri filtrelemek için filtre ve sütun ekleyin veya kaldırın.
   1. Kapsamı daraltmak için filtre ekleyin:
      1. Araştırmanız için özel bir olaylarınızın olduğu **BAĞıNTı kimliği** .
      1. İlke hatasını ve başarısını görmek için **koşullu erişim** . Filtrenizi yalnızca sonuçları sınırlamaya yönelik hataların kapsamını gösterecek şekilde kapsama.
      1. Belirli kullanıcılarla ilgili bilgileri görmek için **Kullanıcı adı** .
      1. Söz konusu zaman dilimine göre kapsam **tarihi** .

   ![Oturum açma günlüğünde koşullu erişim filtresini seçme](./media/troubleshoot-conditional-access/image3.png)

1. Kullanıcının oturum açma hatasına karşılık gelen oturum açma olayı bulunursa **koşullu erişim** sekmesini seçin. Koşullu erişim sekmesi, oturum açma kesintiye neden olan belirli ilke veya ilkeleri gösterir.
   1. **Sorun giderme ve destek** sekmesindeki bilgiler, bir oturum açma nedeninin, uyumluluk gereksinimlerini karşılamayan bir cihaz gibi neden başarısız olduğu konusunda açık bir neden sağlayabilir.
   1. Daha fazla araştırmak için, **Ilke adına**tıklayarak ilkelerin yapılandırmasında ayrıntıya gidin. **Ilke adına** tıkladığınızda, seçilen ilke için ilke yapılandırma kullanıcı arabirimi gözden geçirme ve düzenlemeyle gösterilir.
   1. Koşullu erişim ilkesi değerlendirmesi için kullanılan **istemci kullanıcı** ve **cihaz ayrıntıları** **temel bilgiler**, **konum**, **cihaz bilgileri**, **kimlik doğrulama ayrıntıları**ve oturum açma olayının **ek ayrıntılar** sekmelerinde da kullanılabilir.

   ![Oturum açma olayı koşullu erişim sekmesi](./media/troubleshoot-conditional-access/image5.png)

Olaydaki bilgiler, oturum açma sonuçlarını anlamak veya istenen sonuçları almak için ilkeyi ayarlamak üzere yeterli değilse, bir destek olayı açılabilir. Bu oturum açma olayının **sorun giderme ve destek** sekmesine gidin ve **Yeni bir destek isteği oluştur**' u seçin.

![Oturum açma olayının sorun giderme ve destek sekmesi](./media/troubleshoot-conditional-access/image6.png)

Olayı gönderirken, olay gönderim ayrıntılarında oturum açma olayından istek KIMLIĞI ve saat ve tarih bilgilerini sağlayın. Bu bilgiler, Microsoft destek 'in ilgilendiğiniz olayı bulmasını sağlar.

### <a name="conditional-access-error-codes"></a>Koşullu erişim hata kodları

| Oturum açma hata kodu | Hata dizesi |
| --- | --- |
| 53000 | Devicenotuyumlu |
| 53001 | Devicenotdomainkatılmış |
| 53002 | ApplicationUsedIsNotAnApprovedApp |
| 53003 | BlockedByConditionalAccess |
| 53004 | Redakupblockedduetorisk |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory portaldaki oturum açma etkinlik raporları](../reports-monitoring/concept-sign-ins.md)
- [What If aracını kullanarak koşullu erişim sorunlarını giderme](troubleshoot-conditional-access-what-if.md)
- [Azure Active Directory 'de koşullu erişim](best-practices.md) için en iyi yöntemler
