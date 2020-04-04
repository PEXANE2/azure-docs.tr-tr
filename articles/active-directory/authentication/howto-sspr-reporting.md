---
title: Self servis parola sıfırlama raporları - Azure Etkin Dizini
description: Azure AD self servis parola sıfırlama olayları hakkında raporlama
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 02/01/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 348082ad96a5efa4d8f866c3675044edf7b6f8a8
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80652148"
---
# <a name="reporting-options-for-azure-ad-password-management"></a>Azure AD parola yönetimi için raporlama seçenekleri

Dağıtımdan sonra, birçok kuruluş self servis parola sıfırlamanın (SSPR) gerçekten nasıl kullanıldığını veya kullanıp kullanıldığını bilmek ister. Azure Active Directory'nin (Azure AD) sağladığı raporlama özelliği, önceden oluşturulmuş raporları kullanarak soruları yanıtlamanıza yardımcı olur. Uygun şekilde lisansınız varsa, özel sorgular da oluşturabilirsiniz.

![Azure AD'deki denetim günlüklerini kullanarak SSPR'de raporlama][Reporting]

Aşağıdaki sorular [Azure portalında](https://portal.azure.com/)bulunan raporlar tarafından yanıtlanabilir:

> [!NOTE]
> [Genel bir yönetici](../users-groups-roles/directory-assign-admin-roles.md)olmalısınız ve bu verilerin kuruluşunuz adına toplanıp atanmasını kabul etmelisiniz. Oturum açmak için **Raporlama** sekmesini veya denetim günlüklerini en az bir kez ziyaret etmelisiniz. O zamana kadar, verileriniz kuruluşunuz için toplanmaz.
>

* Parola sıfırlama için kaç kişi kayıt yaptırdı?
* Parola sıfırlama için kimler kayıt yaptırdı?
* İnsanlar hangi verileri kaydediyor?
* Son yedi günde kaç kişi parolalarını sıfırlar?
* Kullanıcıların veya yöneticilerin parolalarını sıfırlamak için en sık kullanılan yöntemler nelerdir?
* Parola sıfırlama yı kullanmaya çalışırken kullanıcıların veya yöneticilerin karşılaştığı sık karşılaşılan sorunlar nelerdir?
* Hangi yöneticiler kendi parolalarını sık sık sıfırlıyor?
* Parola sıfırlama ile ilgili şüpheli bir etkinlik var mı?

## <a name="how-to-view-password-management-reports-in-the-azure-portal"></a>Azure portalında parola yönetim raporlarını görüntüleme

Azure portalı deneyiminde, parola sıfırlama ve parola sıfırlama kaydı etkinliğini görüntüleme şeklinizi geliştirdik. Parola sıfırlama ve parola sıfırlama kayıt olaylarını bulmak için aşağıdaki adımları kullanın:

1. [Azure portalına](https://portal.azure.com)göz atın.
2. Sol bölmede **Tüm hizmetler**'i seçin.
3. Hizmetler listesinde **Azure Etkin Dizini'ni** arayın ve seçin.
4. Yönet bölümünden **Kullanıcıları** seçin.
5. **Kullanıcılar** bıçağından **Denetim Günlükleri'ni** seçin. Bu, dizininizdeki tüm kullanıcılara karşı gerçekleşen tüm denetim olaylarını gösterir. Parolayla ilgili tüm olayları görmek için bu görünüme filtre uygulayabilirsiniz.
6. Bölmenin üst kısmındaki **Filtre** menüsünden **Hizmet** açılır listesini seçin ve Self **servis Parola Yönetimi** hizmet türüne değiştirin.
7. İsteğe bağlı olarak, ilgilendiğiniz belirli **Etkinliği** seçerek listeye daha fazla filtre uygulayın.

### <a name="converged-registration-preview"></a>Yakınsanmış kayıt (önizleme)

Yakınsanmış kaydın genel önizlemesine katılıyorsanız, denetim günlüklerinde kullanıcı etkinliğine ilişkin bilgiler **Güvenlik** > **Kimlik Doğrulama Yöntemleri**altında bulunur.

## <a name="description-of-the-report-columns-in-the-azure-portal"></a>Azure portalındaki rapor sütunlarının açıklaması

Aşağıdaki liste, Azure portalındaki rapor sütunlarının her birini ayrıntılı olarak açıklar:

* **Kullanıcı**: Şifre sıfırlama kaydı işlemini yapmaya çalışan kullanıcı.
* **Rolü**: Dizininde kullanıcının rolü.
* **Tarih ve Saat**: Girişimin tarih ve saati.
* **Kayıtlı Veri**: Kullanıcının parola sıfırlama kaydı sırasında sağladığı kimlik doğrulama verileridir.

## <a name="description-of-the-report-values-in-the-azure-portal"></a>Azure portalındaki rapor değerlerinin açıklaması

Aşağıdaki tablo, Azure portalındaki her sütun için ayarlayabildiğiniz farklı değerleri açıklar:

| Sütun | İzin verilen değerler ve anlamları |
| --- | --- |
| Kayıtlı veriler |**Alternatif e-posta**: Kullanıcı kimlik doğrulaması için alternatif bir e-posta veya kimlik doğrulama e-postası kullansın.<p><p>**Ofis telefonu**: Kullanıcı kimlik doğrulaması yapmak için bir ofis telefonu kullandı.<p>**Cep telefonu**: Kullanıcı kimlik doğrulaması yapmak için bir cep telefonu veya kimlik doğrulama telefonu kullansın.<p>**Güvenlik soruları**: Kullanıcı kimlik doğrulaması için güvenlik sorularını kullansın.<p>**Önceki yöntemlerin herhangi bir kombinasyonu, örneğin, alternatif e-posta + cep telefonu**: İki kapılı bir ilke belirtildiğinde oluşur ve kullanıcının parola sıfırlama isteğini doğrulamak için hangi iki yöntem kullandığını gösterir. |

## <a name="self-service-password-management-activity-types"></a>Self Servis Şifre Yönetimi etkinlik türleri

Aşağıdaki etkinlik türleri **Self Servis Parola Yönetimi** denetimi etkinlik kategorisinde görünür:

* [Self servis parola sıfırlaması engellendi](#activity-type-blocked-from-self-service-password-reset): Kullanıcının parolayı sıfırlamaya, belirli bir kapıyı kullanmaya veya bir telefon numarasını 24 saat içinde toplam beş defadan fazla doğrulamaya çalıştığını gösterir.
* [Parolayı Değiştir (self servis)](#activity-type-change-password-self-service): Kullanıcının isteğe bağlı veya zorunlu (son kullanma tarihi nedeniyle) parola değişikliği gerçekleştirdiğini gösterir.
* [Parolayı sıfırlama (yöneticiye göre)](#activity-type-reset-password-by-admin): Bir yöneticinin Azure portalından bir kullanıcı adına parola sıfırlama gerçekleştirdiğini gösterir.
* [Parolayı sıfırlama (self servis)](#activity-type-reset-password-self-service): Kullanıcının [Parolasını Azure AD parola sıfırlama portalından](https://passwordreset.microsoftonline.com)başarıyla sıfırladığını gösterir.
* [Self servis parola sıfırlama akışı etkinliği ilerleme](#activity-type-self-serve-password-reset-flow-activity-progress): Parola sıfırlama işleminin bir parçası olarak, belirli bir parola sıfırlama kimlik doğrulama kapısını geçmek gibi kullanıcının geçtiği her adımı gösterir.
* [Kullanıcı hesabının kilidini açma (self servis)](#activity-type-unlock-a-user-account-self-service)): Kullanıcının, Azure AD parola sıfırlama [portalından](https://passwordreset.microsoftonline.com) parolasını sıfırlamadan Active Directory hesabının kilidini başarıyla açtığını ve hesap kilidini sıfırlamadan Active Directory özelliğini kullandığını gösterir.
* [Self servis şifre sıfırlama için kayıtlı kullanıcı](#activity-type-user-registered-for-self-service-password-reset): Kullanıcının, parolasını şu anda belirtilen kiracı parola sıfırlama ilkesine uygun olarak sıfırlayabilmek için gerekli tüm bilgileri kaydettiğini gösterir.

### <a name="activity-type-blocked-from-self-service-password-reset"></a>Etkinlik türü: Self servis parola sıfırlama engellendi

Aşağıdaki liste bu etkinliği ayrıntılı olarak açıklar:

* **Etkinlik açıklaması**: Kullanıcının parolayı sıfırlamaya, belirli bir kapıyı kullanmaya veya bir telefon numarasını 24 saat içinde toplam beş ten fazla doğrulamaya çalıştığını gösterir.
* **Etkinlik aktörü**: Ek sıfırlama işlemleri gerçekleştirmekten daralan kullanıcı. Kullanıcı bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedefi**: Ek sıfırlama işlemleri gerçekleştirmekten daralan kullanıcı. Kullanıcı bir son kullanıcı veya yönetici olabilir.
* **Etkinlik durumu**:
  * _Başarı_: Kullanıcının ek sıfırlamalar yapmaktan, ek kimlik doğrulama yöntemleridenemekten veya sonraki 24 saat boyunca ek telefon numaralarını doğrulamaktan dolayı azaltıldığını gösterir.
* **Etkinlik durumu hata nedeni**: Uygulanamaz.

### <a name="activity-type-change-password-self-service"></a>Etkinlik türü: Parolayı değiştir (self servis)

Aşağıdaki liste bu etkinliği ayrıntılı olarak açıklar:

* **Etkinlik açıklaması**: Kullanıcının isteğe bağlı veya zorunlu (son kullanma tarihi nedeniyle) parola değişikliği gerçekleştirdiğini gösterir.
* **Etkinlik aktörü**: Parolasını değiştiren kullanıcı. Kullanıcı bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedefi**: Parolasını değiştiren kullanıcı. Kullanıcı bir son kullanıcı veya yönetici olabilir.
* **Faaliyet durumları**:
  * _Başarı_: Kullanıcının parolalarını başarıyla değiştirdiğini gösterir.
  * _Hata_: Kullanıcının parolasını değiştiremediğini gösterir. Hatanın neden oluştuğu hakkında daha fazla bilgi edinmek için **Etkinlik durumu nedeni** kategorisini görmek için satırı seçebilirsiniz.
* **Etkinlik durumu hatası nedeni**:
  * _FuzzyPolicyViolationInvalidPassword_: Microsoft Yasaklı Parola Algılama özellikleri çok yaygın veya özellikle zayıf bulduğu için kullanıcı otomatik olarak yasaklanan bir parola seçti.

### <a name="activity-type-reset-password-by-admin"></a>Etkinlik türü: Parolayı sıfırla (yöneticiye göre)

Aşağıdaki liste bu etkinliği ayrıntılı olarak açıklar:

* **Etkinlik açıklaması**: Azure portalından bir kullanıcı adına bir yöneticinin parola sıfırlama gerçekleştirdiğini gösterir.
* **Etkinlik aktörü**: Parolasıfırlamayı başka bir son kullanıcı veya yönetici adına gerçekleştiren yönetici. Parola yöneticisi, kullanıcı yöneticisi veya yardım masası yöneticisi olmalıdır.
* **Etkinlik hedefi**: Parolası sıfırlanan kullanıcı. Kullanıcı son kullanıcı veya farklı bir yönetici olabilir.
* **Faaliyet durumları**:
  * _Başarı_: Yöneticinin kullanıcının parolasını başarıyla sıfırladığını gösterir.
  * _Hata_: Yöneticinin kullanıcının parolasını değiştiremediğini gösterir. Hatanın neden oluştuğu hakkında daha fazla bilgi edinmek için **Etkinlik durumu nedeni** kategorisini görmek için satırı seçebilirsiniz.

### <a name="activity-type-reset-password-self-service"></a>Etkinlik türü: Parolayı sıfırlama (self servis)

Aşağıdaki liste bu etkinliği ayrıntılı olarak açıklar:

* **Etkinlik açıklaması**: Kullanıcının [Parolasını Azure AD parola sıfırlama portalından](https://passwordreset.microsoftonline.com)başarıyla sıfırladığını gösterir.
* **Etkinlik aktörü**: Parolasını sıfırlayan kullanıcı. Kullanıcı bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedefi**: Parolasını sıfırlayan kullanıcı. Kullanıcı bir son kullanıcı veya yönetici olabilir.
* **Faaliyet durumları**:
  * _Başarı_: Kullanıcının kendi parolanı başarıyla sıfırladığını gösterir.
  * _Hata_: Kullanıcının kendi parolasını sıfırlamadığını gösterir. Hatanın neden oluştuğu hakkında daha fazla bilgi edinmek için **Etkinlik durumu nedeni** kategorisini görmek için satırı seçebilirsiniz.
* **Etkinlik durumu hatası nedeni**:
  * _FuzzyPolicyViolationInvalidPassword_: Yönetici, Microsoft Yasaklı Parola Algılama özellikleri çok yaygın veya özellikle zayıf bulduğu için otomatik olarak yasaklanan bir parola seçti.

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>Etkinlik türü: Self servis parola sıfırlama akış etkinliği ilerleme

Aşağıdaki liste bu etkinliği ayrıntılı olarak açıklar:

* **Etkinlik açıklaması**: Parola sıfırlama işleminin bir parçası olarak kullanıcının geçtiği her adımı (belirli bir parola sıfırlama kimlik doğrulama kapısını geçmek gibi) gösterir.
* **Etkinlik aktörü**: Parola sıfırlama akışının bir bölümünü gerçekleştiren kullanıcı. Kullanıcı bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedefi**: Parola sıfırlama akışının bir bölümünü gerçekleştiren kullanıcı. Kullanıcı bir son kullanıcı veya yönetici olabilir.
* **Faaliyet durumları**:
  * _Başarı_: Kullanıcının parola sıfırlama akışının belirli bir adımını başarıyla tamamladığını gösterir.
  * _Hata_: Parola sıfırlama akışının belirli bir adımının başarısız olduğunu gösterir. Hatanın neden oluştuğu hakkında daha fazla bilgi edinmek için **Etkinlik durumu nedeni** kategorisini görmek için satırı seçebilirsiniz.
* **Etkinlik durumu nedenleri**: Tüm izin verilen sıfırlama etkinlik durumu nedenleri için aşağıdaki [tabloya](#description-of-the-report-columns-in-the-azure-portal)bakın.

### <a name="activity-type-unlock-a-user-account-self-service"></a>Etkinlik türü: Kullanıcı hesabının kilidini açma (self servis)

Aşağıdaki liste bu etkinliği ayrıntılı olarak açıklar:

* **Etkinlik açıklaması**: Kullanıcının, azure [AD parola sıfırlama portalından](https://passwordreset.microsoftonline.com) parolasını sıfırlamadan Active Directory hesabının kilidini başarıyla açtığını ve hesap kilidini sıfırlamadan Active Directory özelliğini kullandığını gösterir.
* **Etkinlik aktörü**: Parolasını sıfırlamadan hesabının kilidini açan kullanıcı. Kullanıcı bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedefi**: Şifresini sıfırlamadan hesabının kilidini açan kullanıcı. Kullanıcı bir son kullanıcı veya yönetici olabilir.
* **İzin verilen etkinlik durumları:**
  * _Başarı_: Kullanıcının kendi hesabının kilidini başarıyla açtığını gösterir.
  * _Hata_: Kullanıcının hesabının kilidini açmadığını gösterir. Hatanın neden oluştuğu hakkında daha fazla bilgi edinmek için **Etkinlik durumu nedeni** kategorisini görmek için satırı seçebilirsiniz.

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>Etkinlik türü: Self servis parola sıfırlama için kayıtlı kullanıcı

Aşağıdaki liste bu etkinliği ayrıntılı olarak açıklar:

* **Etkinlik açıklaması**: Kullanıcının, parolasını şu anda belirtilen kiracı parola sıfırlama ilkesine uygun olarak sıfırlayabilmek için gerekli tüm bilgileri kaydettiğini gösterir. 
* **Etkinlik aktörü**: Parola sıfırlama için kaydolan kullanıcı. Kullanıcı bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedefi**: Parola sıfırlama için kaydolan kullanıcı. Kullanıcı bir son kullanıcı veya yönetici olabilir.
* **İzin verilen etkinlik durumları:**
  * _Başarı_: Kullanıcının geçerli ilke ye uygun olarak parola sıfırlama için başarıyla kaydolduğunu gösterir. 
  * _Hata_: Kullanıcının parola sıfırlama için kaydolmadığını gösterir. Hatanın neden oluştuğu hakkında daha fazla bilgi edinmek için **Etkinlik durumu nedeni** kategorisini görmek için satırı seçebilirsiniz.

     >[!NOTE]
     >Hata, kullanıcının kendi parolasını sıfırlayamayacağı anlamına gelmez. Bu, kayıt işlemini bitirmedikleri anlamına geliyor. Bu telefon numarasını doğrulamamış olsalar bile doğrulanmamış bir telefon numarası gibi, hesaplarında doğrulanmamış veriler varsa, parolalarını sıfırlamak için yine de kullanabilirler.

## <a name="next-steps"></a>Sonraki adımlar

* [SSPR ve MFA kullanımı ve öngörüraporlama](howto-authentication-methods-usage-insights.md)
* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](howto-sspr-deployment.md)
* [Parolanızı sıfırla veya değiştirin.](../user-help/active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolun.](../user-help/active-directory-passwords-reset-register.md)
* [Lisansla ilgili bir sorunuz mu var?](concept-sspr-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](howto-sspr-authenticationdata.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](concept-sspr-howitworks.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](concept-sspr-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](howto-sspr-writeback.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](concept-sspr-howitworks.md)
* [Sanırım bir şey kırıldı. SSPR'yi nasıl giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

[Reporting]: ./media/howto-sspr-reporting/sspr-reporting.png "Azure AD'deki SSPR etkinlik denetim günlükleri örneği"
