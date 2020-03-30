---
title: Koşullu Erişimile ilgili oturum açma sorunlarını giderme - Azure Etkin Dizini
description: Bu makalede, Koşullu Erişim ilkeleriniz beklenmeyen sonuçlara neden olduğunda ne yapmanız gerektiğini açıklamaktadır
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80337447"
---
# <a name="troubleshooting-sign-in-problems-with-conditional-access"></a>Koşullu Erişim ile ilgili oturum açma sorunlarını giderme

Bu makaledeki bilgiler, hata iletileri ve Azure AD oturum açma günlüğünü kullanarak Koşullu Erişim ile ilgili beklenmeyen oturum açma sonuçlarını gidermek için kullanılabilir.

## <a name="conditional-access-sign-in-interrupt"></a>Koşullu Erişim oturum açma kesintisi

İlk yol, görünen hata iletisini gözden geçirmektir. Bir web tarayıcısı kullanırken oturum açma sorunları için, hata sayfasının kendisi ayrıntılı bilgilere sahiptir. Bu bilgiler tek başına sorunun ne olduğunu açıklayabilir ve bu bir çözüm önerebilir.

![Oturum açma hatası - uyumlu cihaz gerekli](./media/troubleshoot-conditional-access/image1.png)

Yukarıdaki hatada, ileti, uygulamaya yalnızca şirketin mobil cihaz yönetim ilkesini karşılayan aygıtlardan veya istemci uygulamalarından erişilebileni belirtir. Bu durumda, uygulama ve aygıt bu ilkeyi karşılamaz.

## <a name="azure-ad-sign-in-events"></a>Azure AD oturum açma etkinlikleri

Oturum açma kesintisi hakkında ayrıntılı bilgi almak için ikinci yöntem, Hangi Koşullu Erişim ilkesinin veya ilkelerinin uygulandığını ve neden uygulandığını görmek için Azure AD oturum açma olaylarını gözden geçirmektir.

İlk hata sayfasında **Daha Fazla Ayrıntı'yı** tıklatarak sorun hakkında daha fazla bilgi bulabilirsiniz. Daha **Fazla Ayrıntı'yı** tıklatmak, kullanıcının gördüğü belirli bir hata olayı için Azure AD oturum açma olaylarını ararken veya Microsoft ile bir destek olayı açarken yararlı olan sorun giderme bilgilerini ortaya çıkarır.

![Koşullu Erişim'den daha fazla ayrıntı web tarayıcısı oturumunu yarıda kesti.](./media/troubleshoot-conditional-access/image2.png)

Hangi Koşullu Erişim ilkesinin veya ilkelerinin uygulandığını ve neden aşağıdakileri uyguladığını öğrenmek için.

1. **Azure portalında** genel yönetici, güvenlik yöneticisi veya genel okuyucu olarak oturum açın.
1. **Azure Etkin Dizin** > **Oturum Açma'ya**göz atın.
1. Oturum açma nın gözden geçirilmesi için olayı bulun. Gereksiz bilgileri filtrelemek için filtre ler ve sütunlar ekleyin veya kaldırın.
   1. Kapsamı daraltmak için filtre ekleyin:
      1. Araştırmanız gereken belirli bir olay olduğunda **Korelasyon Kimliği.**
      1. İlke başarısızlığı nı ve başarısını görmek için **koşullu erişim.** Filtrenizi yalnızca sonuçları sınırlamak için yapılan hataları göstermek için kapsamda uygulayın.
      1. Belirli kullanıcılarla ilgili bilgileri görmek için **kullanıcı adı.**
      1. **Tarih,** söz konusu zaman dilimine göre kapsamdadır.

   ![Oturum açma günlüğünde Koşullu erişim filtresini seçme](./media/troubleshoot-conditional-access/image3.png)

1. Kullanıcının oturum açma hatasına karşılık gelen oturum açma olayı bulunduktan sonra **Koşullu Erişim** sekmesini seçin. Koşullu Erişim sekmesi, oturum açma kesintisiyle sonuçlanan belirli ilke veya ilkeleri gösterir.
   1. **Sorun Giderme ve destek** sekmesindeki bilgiler, uyumluluk gereksinimlerini karşılamayan bir aygıt gibi oturum açmanın neden başarısız olduğuna dair açık bir neden sağlayabilir.
   1. Daha fazla araştırmak için, **İlke Adı'nı**tıklayarak ilkelerin yapılandırmasını derinlemesine inceleyin. **İlke Adı'nı** tıklattığınızda, gözden geçirme ve düzenleme için seçili ilke için ilke yapılandırma sı kullanıcı arabirimi gösterilecek.
   1. Koşullu Erişim ilkesi değerlendirmesi için kullanılan **istemci kullanıcı** ve **aygıt ayrıntıları,** oturum açma olayının **Temel Bilgileri,** **Konumu,** **Cihaz Bilgileri,** **Kimlik Doğrulama Ayrıntıları**ve **Ek Ayrıntılar** sekmelerinde de bulunur.

   ![Oturum açma olayı Koşullu Erişim sekmesi](./media/troubleshoot-conditional-access/image5.png)

Etkinlikteki bilgiler oturum açma sonuçlarını anlamak veya istenen sonuçları alacak şekilde ilkeyi ayarlamak için yeterli değilse, bir destek olayı açılabilir. Oturum açma etkinliğinin Sorun **Giderme ve Destek** sekmesine gidin ve **yeni bir destek isteği oluştur'u**seçin.

![Oturum Açma etkinliğinin Sorun Giderme ve destek sekmesi](./media/troubleshoot-conditional-access/image6.png)

Olayı gönderirken, istek kimliğini ve olay gönderme ayrıntılarındaki oturum açma olayından gelen saati ve tarihi sağlayın. Bu bilgiler, Microsoft'un endişe duyduğunuz olayı bulması için destek sağlar.

### <a name="conditional-access-error-codes"></a>Koşullu Erişim hata kodları

| Oturum Açma Hata Kodu | Hata Dizesi |
| --- | --- |
| 53000 | Aygıt Uyumlu Değil |
| 53001 | DeviceNotDomainKatıldı |
| 53002 | UygulamaUsedIsNotanApprovedApp |
| 53003 | EngellenmişByConditionalAccess |
| 53004 | ProofUpBlockedDueToRisk |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory portaldaki oturum açma etkinlik raporları](../reports-monitoring/concept-sign-ins.md)
- [What If aracını kullanarak Koşullu Erişim Sorunu Giderme](troubleshoot-conditional-access-what-if.md)
- Azure Etkin [Dizini'nde Koşullu Erişim](best-practices.md) için en iyi uygulamalar
