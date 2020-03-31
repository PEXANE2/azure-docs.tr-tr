---
title: Belirli bir kullanıcının bir uygulamaya ne zaman erişeceğini öğrenin
description: Kritik öneme sahip bir kullanıcının Azure AD ile kullanıcı sağlama için yapılandırdığınız bir uygulamaya ne zaman erişebildiğini nasıl öğrenebilirsiniz?
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/03/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 48370e2806b70d550bce95ceff3857a79561f247
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79264147"
---
# <a name="check-the-status-of-user-provisioning"></a>Kullanıcı sağlama durumunu kontrol edin

Azure AD sağlama hizmeti, kaynak sisteme ve hedef sisteme karşı bir başlangıç sağlama döngüsü ve ardından periyodik artımlı döngüler çalıştırır. Bir uygulama için sağlama yı yapılandırdığınızda, sağlama hizmetinin geçerli durumunu kontrol edebilir ve bir kullanıcının bir uygulamaya ne zaman erişebileceğini görebilirsiniz.

## <a name="view-the-provisioning-progress-bar"></a>Sağlama ilerleme çubuğunu görüntüleyin

 Bir uygulamanın **Sağlama** sayfasında, Azure AD sağlama hizmetinin durumunu görüntüleyebilirsiniz. Sayfanın altındaki **Geçerli Durum** bölümü, bir sağlama döngüsünün kullanıcı hesaplarını sağlamaya başlayıp başlamadığını gösterir. Döngünün ilerlemesini izleyebilir, kaç kullanıcı ve grubun sağlandığını görebilir ve kaç rol oluşturulduğunu görebilirsiniz.

Otomatik sağlamayı ilk yapılandırdığınızda, sayfanın altındaki **Geçerli Durum** bölümü ilk sağlama döngüsünün durumunu gösterir. Bu bölüm, her artımlı döngü çalıştığında güncelleştirir. Aşağıdaki ayrıntılar gösterilmiştir:
- Şu anda çalışan veya en son tamamlanan sağlama döngüsü (başlangıç veya artımlı) türü.
- Tamamlanan sağlama döngüsünün yüzdesini gösteren bir **ilerleme çubuğu.** Yüzde, sağlanan sayfa sayısını yansıtır. Her sayfanın birden çok kullanıcı veya grup içerebileceğini, bu nedenle yüzdenin sağlanan kullanıcı, grup veya rol sayısıyla doğrudan ilişkili olmadığını unutmayın.
- Görünümü güncel tutmak için kullanabileceğiniz **yenile** düğmesi.
- Bağlayıcı veri deposundaki **Kullanıcı** ve **Grup** sayısı. Bir nesne sağlama kapsamına ekleninher zaman sayım artar. Bu, nesneyi bağlayıcı veri deposundan kaldırmadığı için, kullanıcı yumuşak silinmiş veya sert silinmişse sayım aşağı gitmez. CDS [sıfırladıktan](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-restart?view=graph-rest-beta&tabs=http) sonra sayım ilk eşitleme yeniden 
- Tek tek kullanıcılar için sağlama durumu da dahil olmak üzere, kullanıcı sağlama hizmeti tarafından yürütülen tüm işlemlerle ilgili ayrıntılar için Azure AD sağlama günlüklerini açan Bir **Görünüm Denetim Günlükleri** bağlantısı (aşağıdaki [Kullanım sağlama günlükleri](#use-provisioning-logs-to-check-a-users-provisioning-status) bölümüne bakın).

Bir sağlama döngüsü tamamlandıktan sonra, **İstatistikler bölümü,** son çevrimin tamamlanma tarihi ve süresiyle birlikte, bugüne kadar sağlanan kullanıcı ve grupların kümülatif sayılarını gösterir. **Etkinlik Kimliği,** en son sağlama döngüsünü benzersiz olarak tanımlar. **İş Kimliği,** sağlama işi için benzersiz bir tanımlayıcıdır ve kiracınızdaki uygulamaya özgüdür.

Sağlama ilerlemesi Azure portalında, Azure Active **Directory &gt; Enterprise Apps &gt; \[uygulama\] &gt; adı Sağlama** sekmesinde görüntülenebilir.

![Sağlama sayfası ilerleme çubuğu](./media/application-provisioning-when-will-provisioning-finish-specific-user/provisioning-progress-bar-section.png)

## <a name="use-provisioning-logs-to-check-a-users-provisioning-status"></a>Kullanıcının sağlama durumunu denetlemek için sağlama günlüklerini kullanma

Seçili bir kullanıcının sağlama durumunu görmek için Azure AD'deki [Sağlama günlüklerine (önizleme)](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context) başvurun. Kullanıcı sağlama hizmeti tarafından yürütülen tüm işlemler Azure AD sağlama günlüklerine kaydedilir. Bu, kaynak ve hedef sistemlere yapılan tüm okuma ve yazma işlemlerini ve her işlem sırasında okunan veya yazılan kullanıcı verilerini içerir.

**Etkinlik** bölümünde **Azure Active Directory** &gt; **Enterprise Apps** &gt; **Sağlama günlüklerini (önizleme)** seçerek Azure portalındaki sağlama günlüklerine erişebilirsiniz. Sağlama verilerini, kaynak sistemde veya hedef sistemde kullanıcının adına veya tanımlayıcıya göre arayabilirsiniz. Ayrıntılar için, [Sağlama günlükleri (önizleme)](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context)bakın. 

Sağlama günlükleri, aşağıdakiler de dahil olmak üzere, sağlama hizmeti tarafından gerçekleştirilen tüm işlemleri kaydeder:

* Sağlama kapsamında olan atanmış kullanıcılar için Azure AD'yi sorgulama
* Bu kullanıcıların varlığı için hedef uygulamayı sorgulama
* Kullanıcı nesnelerinin sistem arasında karşılaştırılması
* Karşılaştırmaya dayalı olarak hedef sistemde kullanıcı hesabı ekleme, güncelleme veya devre dışı bırakma

Azure portalındaki sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi [için, sağlama raporlama kılavuzuna](check-status-user-account-provisioning.md)bakın.

## <a name="how-long-will-it-take-to-provision-users"></a>Kullanıcılara sağlama ne kadar sürer?
Azure AD, bir uygulamayla otomatik kullanıcı sağlama kullanırken, kullanıcı hesaplarını genellikle her 40 dakikada bir, düzenli olarak zamanlanmış bir zaman aralığında [kullanıcı ve grup ataması](../manage-apps/assign-user-or-group-access-portal.md) gibi şeylere göre otomatik olarak sağlar ve güncelleştirir.

Belirli bir kullanıcının sağlanması için gereken süre, esas olarak sağlama işinizin bir başlangıç döngüsü mü yoksa artımlı bir döngü mü çalıştırdığına bağlıdır.

- **İlk döngü**için, iş süresi, sağlama kapsamındaki kullanıcı ve grup sayısı ve kaynak sistemdeki toplam kullanıcı ve grup sayısı da dahil olmak üzere birçok etkene bağlıdır. Azure AD ile uygulama arasındaki ilk eşitleme, Azure REKLAM dizininin boyutuna ve sağlama kapsamındaki kullanıcı sayısına bağlı olarak 20 dakika ile birkaç saat arasında sürebilir. İlk çevrim performansını etkileyen etkenlerin kapsamlı bir listesi daha sonra bu bölümde özetlenmiştir.

- İlk döngüden sonraki **artımlı döngüler** için, sağlama hizmeti, sonraki eşitlemelerin performansını artırarak, her iki sistemdeki durumu temsil eden filigranları depoladıktan sonra, iş süreleri daha hızlı olma eğilimindedir (örn. 10 dakika içinde). İş süresi, bu sağlama döngüsünde algılanan değişikliklerin sayısına bağlıdır. 5.000'den az kullanıcı veya grup üyeliği değişikliği varsa, iş tek bir artımlı sağlama döngüsü içinde sona erebilir. 

Aşağıdaki tabloda ortak sağlama senaryoları için eşitleme süreleri özetlenir. Bu senaryolarda, kaynak sistem Azure AD ve hedef sistem bir SaaS uygulamasıdır. Eşitleme süreleri, SaaS uygulamaları ServiceNow, Workplace, Salesforce ve G Suite için eşitleme işlerinin istatistiksel analizinden türetilmiştir.


| Kapsam yapılandırması | Kapsamdaki kullanıcılar, gruplar ve üyeler | İlk çevrim süresi | Artımlı çevrim süresi |
| -------- | -------- | -------- | -------- |
| Yalnızca atanan kullanıcıları ve grupları eşitleme |  < 1.000 |  30 dakika < | 30 dakika < |
| Yalnızca atanan kullanıcıları ve grupları eşitleme |  1,000 - 10,000 | 142 - 708 dakika | 30 dakika < |
| Yalnızca atanan kullanıcıları ve grupları eşitleme |   10,000 - 100,000 | 1.170 - 2.340 dakika | 30 dakika < |
| Azure AD'deki tüm kullanıcıları ve grupları senkronize edin |  < 1.000 | 30 dakika <  | 30 dakika < |
| Azure AD'deki tüm kullanıcıları ve grupları senkronize edin |  1,000 - 10,000 | < 30 - 120 dakika | 30 dakika < |
| Azure AD'deki tüm kullanıcıları ve grupları senkronize edin |  10,000 - 100,000  | 713 - 1.425 dakika | 30 dakika < |
| Azure AD'deki tüm kullanıcıları senkronize edin|  < 1.000  | 30 dakika < | 30 dakika < |
| Azure AD'deki tüm kullanıcıları senkronize edin | 1,000 - 10,000  | 43 - 86 dakika | 30 dakika < |


Yalnızca **atanan kullanıcı ve grupları eşitleyen**yapılandırma için, yaklaşık minimum ve maksimum beklenen ilk **çevrim** sürelerini belirlemek için aşağıdaki formülleri kullanabilirsiniz:

    Minimum minutes =  0.01 x [Number of assigned users, groups, and group members]
    Maximum minutes = 0.08 x [Number of assigned users, groups, and group members] 
    
**Bir başlangıç döngüsünü**tamamlamak için gereken süreyi etkileyen faktörlerin özeti:

- Sağlama kapsamındaki toplam kullanıcı ve grup sayısı.

- Kaynak sistemde (Azure AD) bulunan toplam kullanıcı, grup ve grup üyesi sayısı.

- Sağlama kapsamındaki kullanıcıların hedef uygulamadaki varolan kullanıcılarla eşleşip eşleşmediği veya ilk kez oluşturulması gerekip gerekmediği. Tüm kullanıcıların ilk kez oluşturulduğu eşitleme işleri, tüm kullanıcıların varolan kullanıcılarla eşleştirilen eşitleme işleri kadar *yaklaşık iki kat* daha uzun sürer.

- [Sağlama günlüklerinde](check-status-user-account-provisioning.md)hata sayısı. Çok sayıda hata varsa ve sağlama hizmeti karantina durumuna geçmişse performans daha yavaştır. 

- Hedef sistem tarafından uygulanan talep oranı limitleri ve azaltma. Bazı hedef sistemler, büyük eşitleme işlemleri sırasında performansı etkilenebilen istek oranı sınırları ve azaltma uygular. Bu koşullar altında, çok fazla istek alan bir uygulama yanıt hızını yavaşlatabilir veya bağlantıyı kapatabilir. Performansı artırmak için, konektörün uygulama isteklerini uygulamanın işleyebileceğinden daha hızlı göndermeyerak ayarlaması gerekir. Bu ayarlamayı Microsoft tarafından oluşturulmuş olan sağlama bağlayıcıları yapar. 

- Atanan grupların sayısı ve boyutları. Atanan grupları eşitleme, kullanıcıları eşitlemeden daha uzun sürer. Atanan grupların hem sayısı hem de boyutları performansı etkiler. Bir [uygulama, grup nesnesi eşitleme için etkin eşlemeleri](customize-application-attributes.md#editing-group-attribute-mappings)varsa, grup adları ve üyelikler gibi grup özellikleri kullanıcılara ek olarak eşitlenir. Bu ek eşitlemeler yalnızca kullanıcı nesnelerini eşitlemedaha uzun sürer.

- Performans bir sorun haline geliyorsa ve kiracınızdaki kullanıcıların ve grupların çoğunluğunu sağlamayı denerseniz, kapsam filtreleri kullanın. Kapsam filtreleri, kullanıcıları belirli öznitelik değerlerine göre filtreleyerek, sağlama hizmetinin Azure AD'den çıkardığı verileri ince ayarlaya biliyor. Kapsam filtreleri hakkında daha fazla bilgi için, [kapsam filtreleri ile Öznitelik tabanlı uygulama sağlama](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile SaaS uygulamalarına kullanıcı sağlama ve buuygulamaları sağlama yı otomatikleştirme](user-provisioning.md)
