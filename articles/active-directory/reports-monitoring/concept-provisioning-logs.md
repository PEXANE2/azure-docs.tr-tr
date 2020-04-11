---
title: Azure Active Directory portalında günlük sağlama (önizleme) | Microsoft Dokümanlar
description: Azure Active Directory portalında etkinlik raporları nın sağlanmasına giriş
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/04/2019
ms.author: markvi
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 30cc8be6ad9ebffcad58c5b2412ae15ff3f26fa5
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81113365"
---
# <a name="provisioning-reports-in-the-azure-active-directory-portal-preview"></a>Azure Etkin Dizin portalında rapor sağlama (önizleme)

Azure Etkin Dizin (Azure AD) raporlama mimarisi aşağıdaki bileşenlerden oluşur:

- **Etkinlik** 
    - **Oturum açma lar** – Yönetilen uygulamaların kullanımı ve kullanıcı oturum açma etkinlikleri hakkında bilgi.
    - **Denetim günlükleri** - [Denetim günlükleri,](concept-audit-logs.md) kullanıcılar ve grup yönetimi, yönetilen uygulamalar ve dizin etkinlikleri hakkında sistem etkinlik bilgileri sağlar.
    - **Günlükleri sağlama** - Azure AD sağlama hizmeti tarafından sağlanan kullanıcı, gruplar ve roller le ilgili sistem etkinliği sağlayın. 

- **Güvenlik** 
    - **Riskli oturum açmalar** - [Riskli oturum](concept-risky-sign-ins.md) açma, kullanıcı hesabının yasal sahibi olmayan biri tarafından gerçeklegelmiş olabilecek oturum açma girişiminin göstergesidir.
    - **Risk için işaretlenmiş kullanıcılar** - Riskli bir [kullanıcı,](concept-user-at-risk.md) gizliliği ihlal edilmiş olabilecek bir kullanıcı hesabı için bir göstergedir.

Bu konu, sağlama raporunun genel görünümünü sağlar.

## <a name="prerequisites"></a>Ön koşullar

### <a name="who-can-access-the-data"></a>Verilere kimler erişebilir?
* Güvenlik Yöneticisi, Güvenlik Okuyucusu, Rapor Okuyucusu, Uygulama Yöneticisi ve Bulut Uygulama Yöneticisi rollerindeki kullanıcılar
* Global Yöneticiler


### <a name="what-azure-ad-license-do-you-need-to-access-provisioning-activities"></a>Sağlama etkinliklerine erişmek için hangi Azure REKLAM lisansına ihtiyacınız var?

Kiracınızın, tüm sağlama etkinlik raporunu görmek için onunla ilişkili bir Azure AD Premium lisansına sahip olması gerekir. Azure Active Directory baskınızı yükseltmek için [Azure Active Directory Premium ile başlarken](../fundamentals/active-directory-get-started-premium.md) bakın. 

## <a name="provisioning-logs"></a>Sağlama günlükleri

Sağlama günlükleri aşağıdaki soruların yanıtlarını sağlar:

* ServiceNow'da hangi gruplar başarıyla oluşturuldu?
* Amazon Web Services'ten roller nasıl alındı?
* DropBox'ta başarısız olarak hangi kullanıcılar oluşturuldu?

[Azure portalındaki](https://portal.azure.com) **Azure Active Directory** bıçağının **İzleme** **bölümündeKi Sağlama Günlükleri'ni** seçerek sağlama günlüklerine erişebilirsiniz. Bazı kayıt kayıtlarının portalda gösterilmesi iki saat kadar sürebilir.

![Sağlama günlükleri](./media/concept-provisioning-logs/access-provisioning-logs.png "Sağlama günlükleri")


Bir sağlama günlüğü, şunları gösteren bir varsayılan liste görünümüne sahiptir:

- Kimlik
- Eylem
- Kaynak sistem
- Hedef sistem
- Durum
- Tarih


![Varsayılan sütunlar](./media/concept-provisioning-logs/default-columns.png "Varsayılan sütunlar")

Araç çubuğunda **Sütunlar**’a tıklayarak liste görünümünü özelleştirebilirsiniz.

![Sütun seçici](./media/concept-provisioning-logs/column-chooser.png "Sütun seçici")

Bu sayede ek alanları görüntüleyebilir ya da zaten görüntülenen alanları kaldırabilirsiniz.

![Kullanılabilir sütunlar](./media/concept-provisioning-logs/available-columns.png "Kullanılabilir sütunlar")

Daha ayrıntılı bilgi almak için liste görünümünde bir öğe seçin.

![Detaylı bilgi](./media/concept-provisioning-logs/steps.png "Filtre")


## <a name="filter-provisioning-activities"></a>Filtre sağlama faaliyetleri

Sağlama verilerinizi filtreleyebilirsiniz. Bazı filtre değerleri kiracınıza göre dinamik olarak doldurulur. Örneğin, kiracınızda herhangi bir oluşturma olayı yoksa, oluşturmak için bir filtre seçeneği olmaz.
Varsayılan görünümde aşağıdaki filtreleri seçebilirsiniz:

- Kimlik
- Tarih
- Durum
- Eylem


![Filtrele](./media/concept-provisioning-logs/default-filter.png "Filtre")

**Kimlik** filtresi, önem verdiğiniz adı veya kimliği belirtmenizi sağlar. Bu kimlik bir kullanıcı, grup, rol veya başka bir nesne olabilir. Nesnenin adına veya kimliğine göre arama yapabilirsiniz. Kimlik senaryoya göre değişir. Örneğin, Bir nesneyi Azure AD'den SalesForce'a sağlarken, Kaynak Kimliği Azure AD'deki kullanıcının nesne kimliği, TargetID ise Salesforce'taki kullanıcının kimliğidir. İş Günü'nden Etkin Dizine geçerken, Kaynak Kimlik, İş Günü çalışanı çalışan kimliğidir. Kullanıcı adının Kimlik sütununda her zaman bulunamayabileceğini unutmayın. Her zaman bir kimlik olacak. 


**Tarih** filtresi, döndürülen veriler için bir zaman çerçevesi tanımlamanıza olanak sağlar.  
Olası değerler şunlardır:

- 1 ay
- 7 gün
- 30 gün
- 24 saat
- Özel zaman aralığı

Özel bir zaman dilimi seçtiğinizde, bir başlangıç tarihi ve bitiş tarihi yapılandırabilirsiniz.


**Durum** filtresi şunları seçmenizi sağlar:

- Tümü
- Başarılı
- Hata
- Atlandı



**Eylem** filtresi aşağıdakileri filtrelemenizi sağlar:

- Oluştur 
- Güncelleştir
- Sil
- Devre Dışı Bırak
- Diğer

Buna ek olarak, varsayılan görünümün filtrelerine aşağıdaki filtreleri de ayarlayabilirsiniz:

- İş Kimliği
- Döngü Kimliği
- Kimliği Değiştir
- Kaynak Kimliği
- Hedef Kimliği
- Uygulama


![Bir alan seçin](./media/concept-provisioning-logs/add-filter.png "Bir alan seçin")


- **İş Kimliği** - Benzersiz bir İş Kimliği, sağlama yı etkinleştirdiğiniz her uygulamayla ilişkilidir.   

- **Döngü Kimliği** - Benzersiz sağlama döngüsünü tanımlar. Bu olayın meydana geldiği döngüyü aramak için bu kimliği desteklemek için bu kimliği paylaşabilirsiniz.

- **Kimlik Değiştirme** - Sağlama olayı için benzersiz tanımlayıcı. Sağlama olayını aramak için bu kimliği destekleyerek paylaşabilirsiniz.   


- **Kaynak Sistem** - Kimliğin nereden sağlandığınızı belirtmenizi sağlar. Örneğin, Azure AD'den ServiceNow'a bir nesne rirken Kaynak sistem Azure AD'dir. 

- **Hedef Sistem** - Kimliğin nereye sağlandığınızı belirtmenizi sağlar. Örneğin, Azure AD'den ServiceNow'a bir nesne rirken Hedef Sistem ServiceNow'dur. 

- **Uygulama** - Yalnızca belirli bir dize içeren bir görüntü adı olan uygulamaların kayıtlarını göstermenizi sağlar.

 

## <a name="provisioning-details"></a>Sağlama ayrıntıları 

Sağlama listesi görünümünde bir öğe seçtiğinizde, bu öğe hakkında daha fazla ayrıntı alırsınız.
Ayrıntılar aşağıdaki kategorilere göre gruplandırılır:

- Adımlar

- Sorun giderme ve öneriler

- Değiştirilen özellikler

- Özet


![Filtrele](./media/concept-provisioning-logs/provisioning-tabs.png "Sekmeler")



### <a name="steps"></a>Adımlar

**Adımlar** sekmesi, bir nesneyi sağlamak için atılan adımları özetler. Bir nesneyi sağlama dört adımdan oluşabilir: 

- Nesnealma
- Nesnenin kapsamda olup olmadığını belirleme
- Nesneyi kaynak ve hedef arasında eşleştirme
- Sağlama nesnesi (harekete geçin - bu bir oluşturma, güncelleştirme, silme veya devre dışı kaldırabilir)



![Filtrele](./media/concept-provisioning-logs/steps.png "Filtre")


### <a name="troubleshoot-and-recommendations"></a>Sorun giderme ve öneriler


**Sorun giderme ve öneriler** sekmesi hata kodu ve nedenini sağlar. Hata bilgileri yalnızca bir hata durumunda kullanılabilir. 


### <a name="modified-properties"></a>Değiştirilen özellikler

**Değiştirilen özellikler** eski değeri ve yeni değeri gösterir. Eski bir değerin olmadığı durumlarda eski değer sütunu boştur. 


### <a name="summary"></a>Özet

**Özet** sekmesi, kaynak ve hedef sistemdeki nesne için ne olduğuna ve tanımlayıcıların genel görünümünü sağlar. 

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

- Azure portalı, premium sürümünüz varsa 30 gün, ücretsiz bir sürümünüz varsa 7 gün boyunca veri depolarını depolar...

- Kimlik değiştir özniteliğini benzersiz tanımlayıcı olarak kullanabilirsiniz. Bu, örneğin, ürün desteği ile etkileşimde yararlıdır.

- Şu anda veri sağlama indirme seçeneği yoktur.

- Şu anda günlük analitiği için destek yok.

- Bir uygulama bağlamından sağlama günlüklerine erişdiğinizde, olayları denetim günlüklerinin olduğu şekilde belirli uygulamaya otomatik olarak filtrelemez.

## <a name="error-codes"></a>Hata Kodları

Sağlama günlüklerinde bulabileceğiniz hataları nasıl çözeceğinizdaha iyi anlamak için aşağıdaki tabloyu kullanın. Eksik olan hata kodları için, bu sayfanın altındaki bağlantıyı kullanarak geri bildirim sağlayın. 

|Hata Kodu|Açıklama|
|---|---|
|Çatışma, EntryConflict|Azure AD veya uygulamadaki çakışan öznitelik değerlerini düzeltin veya çakışan kullanıcı hesabının eşleştirilmesi ve devralınması gerekiyorsa eşleşen öznitelik yapılandırmanızı gözden geçirin. Eşleşen öznitelikleri yapılandırma hakkında daha fazla bilgi için aşağıdaki [belgeleri](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes) gözden geçirin.|
|TooManyRequests|Hedef uygulama, aşırı yüklendiği ve çok fazla istek aldığı için kullanıcıyı güncelleme girişimini reddetti. Yapacak bir şey yok. Bu girişim otomatik olarak kullanımdan kaldırılacaktır. Microsoft da bu sorundan haberdar edilmiştir.|
|InternalServerError |Hedef uygulama beklenmeyen bir hata döndürür. Hedef uygulamayla ilgili bir hizmet sorunu olabilir ve bu durum çalışmasını engelliyor olabilir. Bu deneme otomatik olarak 40 dakika içinde kullanımdan kaldırılacaktır.|
|Yetersiz Haklar, Metodİzin Verilmiyor, İzin Verilmiyor, İzin Verilmedi| Azure AD hedef uygulamayla kimlik doğrulaması yapabildi, ancak güncelleştirmeyi gerçekleştirme yetkisi ne kadar yüksekti. Lütfen hedef uygulama nın yanı sıra ilgili uygulama [öğreticisi](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)tarafından sağlanan talimatları gözden geçirin.|
|İşlenebilir Varlık|Hedef uygulama beklenmeyen bir yanıt döndürür. Hedef uygulamanın yapılandırması doğru olmayabilir veya hedef uygulamada bunun çalışmasını engelleyen bir hizmet sorunu olabilir.|
|WebExceptionProtocolError |Hedef uygulamaya bağlanırken bir HTTP protokolü hatası oluştu. Yapacak bir şey yok. Bu deneme otomatik olarak 40 dakika içinde kullanımdan kaldırılacaktır.|
|Geçersiz Çapa|Daha önce oluşturulan veya sağlama hizmeti tarafından eşleşen bir kullanıcı artık yok. Kullanıcının var olduğundan emin olun. Tüm kullanıcıların yeniden eşleşmesini zorlamak [için, işi yeniden başlatmak](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-restart?view=graph-rest-beta&tabs=http)için MS Graph API'yi kullanın. Yeniden başlatma sağlamanın tamamlanması zaman alabilecek bir başlangıç döngüsünü tetiklediğini unutmayın. Ayrıca, sağlama hizmetinin işletmek için kullandığı önbelleği de siler, yani kiracıdaki tüm kullanıcıların ve grupların yeniden değerlendirilmesi ve belirli sağlama olaylarının bırakılabileceği anlamına gelir.|
|Uygulanmadı | Hedef uygulama beklenmeyen bir yanıt döndürür. Uygulamanın yapılandırması doğru olmayabilir veya hedef uygulamayla ilgili bir hizmet sorunu olabilir ve bu sorun çalışmayı engelliyor olabilir. Lütfen hedef uygulama nın yanı sıra ilgili uygulama [öğreticisi](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)tarafından sağlanan talimatları gözden geçirin. |
|Zorunlu Alanlar Eksik, Eksik Değerler |Gerekli değerler eksik olduğundan kullanıcı oluşturulamadı. Kaynak kaydındaki eksik öznitelik değerlerini düzeltin veya gerekli alanların atlanmadığından emin olmak için eşleşen öznitelik yapılandırmanızı gözden geçirin. Eşleşen öznitelikleri yapılandırma hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)|
|ŞemaAttributeNotFound |Hedef uygulamada olmayan bir öznitelik belirtildiği için işlem gerçekleştirilemedi. Öznitelik özelleştirmeyle ilgili [belgelere](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes) bakın ve yapılandırmanızın doğru olduğundan emin olun.|
|InternalError |Azure AD sağlama hizmetinde bir dahili hizmet hatası oluştu. Yapacak bir şey yok. Bu deneme otomatik olarak 40 dakika içinde yeniden denenecektir.|
|Geçersiz Etki Alanı |Geçersiz bir etki alanı adı içeren bir öznitelik değeri nedeniyle işlem gerçekleştirilemedi. Kullanıcıdaki alan adını güncelleştirin veya hedef uygulamada izin verilen listeye ekleyin. |
|Zaman aşımı |Hedef uygulamanın yanıt vermesi çok uzun sürdüğü için işlem tamamlanamadı. Yapacak bir şey yok. Bu deneme otomatik olarak 40 dakika içinde yeniden denenecektir.|
|LisansLimitaşıldı|Bu kullanıcı için kullanılabilir lisans olmadığından, hedef uygulamada kullanıcı oluşturulamadı. Hedef uygulama için ek lisanslar satın almak veya doğru kullanıcıların doğru özniteliklerle atandığından emin olmak için kullanıcı atamalarınızı ve öznitelik eşleme yapılandırmanızı gözden geçirin.|
|Yinelenen Hedef Girişleri  |Hedef uygulamada birden fazla kullanıcı yapılandırılan eşleşen özniteliklerle bulunduğundan işlem tamamlanamadı. Yinelenen kullanıcıyı hedef uygulamadan kaldırın veya [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)açıklandığı gibi öznitelik eşlemelerinizi yeniden yapılandırın.|
|YinelenenKaynak Girişleri | Yapılandırılan eşleşen özniteliklerle birden fazla kullanıcı bulunduğundan işlem tamamlanamadı. Yinelenen kullanıcıyı kaldırın veya [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)açıklandığı gibi öznitelik eşlemelerinizi yeniden yapılandırın.|

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanıcı sağlama durumunu kontrol edin](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user)
* [Kullanıcı sağlamayı azure AD Galerisi uygulamasına yapılandırma sorunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-config-problem)


