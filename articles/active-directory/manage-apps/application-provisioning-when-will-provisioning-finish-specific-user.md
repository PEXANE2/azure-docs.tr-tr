---
title: Belirli bir kullanıcının bir uygulamaya ne zaman erişebileceğini öğrenin | Microsoft Docs
description: Kritik öneme sahip bir kullanıcının Azure AD ile Kullanıcı sağlaması için yapılandırdığınız bir uygulamaya ne zaman erişebileceğini bulma
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3c6ad7e305958131c4f544dfa2022e7471e9adac
ms.sourcegitcommit: aaa82f3797d548c324f375b5aad5d54cb03c7288
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70147100"
---
# <a name="check-the-status-of-user-provisioning"></a>Kullanıcı hazırlama durumunu denetleme

Azure AD sağlama hizmeti, kaynak sistem ve hedef sisteme karşı bir ilk sağlama döngüsü çalıştırır ve ardından düzenli artımlı Döngülerde. Bir uygulama için sağlama yapılandırdığınızda, sağlama hizmetinin geçerli durumunu denetleyebilir ve bir kullanıcının uygulamaya ne zaman erişebileceğini görebilirsiniz.

## <a name="view-the-provisioning-progress-bar"></a>Sağlama ilerleme çubuğunu görüntüleme

 Bir uygulamanın **sağlama** sayfasında, Azure AD sağlama hizmeti 'nin durumunu görüntüleyebilirsiniz. Sayfanın alt kısmındaki **geçerli durum** bölümü, sağlama döngüsünün Kullanıcı hesapları sağlamaya başlayıp başlamadığına ilişkin gösterir. Döngüsünün ilerlemesini izleyebilir, kaç Kullanıcı ve grup sağlandığını görebilir ve kaç rolün oluşturulduğunu görebilirsiniz.

Otomatik sağlamayı ilk yapılandırdığınızda, sayfanın en altındaki **geçerli durum** bölümü ilk sağlama döngüsünün durumunu gösterir. Bu bölüm, her bir artımlı döngüsünün çalıştırıldığı her seferinde güncellenir. Aşağıdaki ayrıntılar gösterilir:
- Şu anda çalışmakta olan veya en son tamamlanan sağlama döngüsünün (ilk veya artımlı) türü.
- Tamamlanan sağlama döngüsünün yüzdesini gösteren bir **ilerleme çubuğu** . Yüzde değeri, sağlanan sayfa sayısını yansıtır. Her sayfanın birden çok kullanıcı veya grup içerebileceğini unutmayın. bu nedenle yüzde, izin sağlanan Kullanıcı, Grup veya rol sayısıyla doğrudan bağıntılı değildir.
- Görünümü güncelleştirilmesini sağlamak için kullanabileceğiniz bir **yenileme** düğmesi.
- Sağlanan **Kullanıcı** ve **grupların** sayısı ve oluşturulan rol sayısı. İlk zaman içinde, Kullanıcı numarası 1 ' den bir kullanıcı oluşturulduğunda veya güncelleştirilirken, bir Kullanıcı silindiğinde 1 olarak sayılır. Artımlı bir döngüde, Kullanıcı güncelleştirmeleri Kullanıcı sayısını etkilemez; sayı yalnızca kullanıcılar oluşturulduğunda veya silindiğinde değişir.
- Bireysel kullanıcıların sağlama durumu da dahil olmak üzere, Kullanıcı sağlama hizmeti tarafından çalıştırılan tüm işlemler hakkındaki ayrıntılar için Azure AD denetim günlüklerini **görüntüle bağlantısını görüntüleyin** (aşağıdaki [Denetim günlüklerini kullanma](#use-audit-logs-to-check-a-users-provisioning-status) bölümüne bakın).

Bir sağlama süresi tamamlandıktan sonra, tarihe göre **İstatistikler** bölümü, son döngüsünün tamamlanma tarihi ve süresiyle birlikte tarih olarak sağlanan Toplam Kullanıcı ve grup sayısını gösterir. **Etkınlık kimliği** en son sağlama döngüsünü benzersiz şekilde tanımlar. **Iş kimliği** , sağlama işi için benzersiz bir tanımlayıcıdır ve kiracınızdaki uygulamaya özeldir.

Sağlama ilerlemesi, Azure Portal **Azure Active Directory &gt; Enterprise Apps &gt; \[\] uygulama adı&gt; sağlama** sekmesinde görüntülenebilir.

![Sağlama sayfası ilerleme çubuğu](media/application-provisioning-when-will-provisioning-finish-specific-user/provisioning-progress-bar-section.png)

## <a name="use-audit-logs-to-check-a-users-provisioning-status"></a>Kullanıcının sağlama durumunu denetlemek için Denetim günlüklerini kullanma

Seçilen bir kullanıcının sağlama durumunu görmek için Azure AD 'deki denetim günlüklerine başvurun. Kullanıcı sağlama hizmeti tarafından çalıştırılan tüm işlemler, Azure AD denetim günlüklerine kaydedilir. Bu, kaynak ve hedef sistemlere yapılan tüm okuma ve yazma işlemlerini ve her işlem sırasında okunan veya yazılan kullanıcı verilerini içerir.

Sağlama denetim günlüklerine Azure Portal **Azure Active Directory &gt; Enterprise Apps &gt; \[uygulama\] adı&gt; denetim günlükleri** sekmesinde erişilebilir. **Hesap sağlama** kategorisindeki günlüklere yalnızca uygulamanın sağlama olaylarını görmek için filtre uygulayın. Kullanıcılar için, öznitelik eşlemelerinde kendileri için yapılandırılmış "eşleşen KIMLIĞI" temelinde arama yapabilirsiniz. 

Örneğin, "Kullanıcı asıl adı" veya "e-posta adresi" ni Azure AD tarafında eşleşen öznitelik olarak yapılandırdıysanız ve sağlanmakta olmadığı Kullanıcı "audrey@contoso.com" değerine sahipse, "audrey@contoso.com" için denetim günlüklerinde arama yapın ve ardından girişleri gözden geçirin döndürüldüğünde.

Sağlama denetim günlükleri, aşağıdakiler dahil olmak üzere, sağlama hizmeti tarafından gerçekleştirilen tüm işlemleri kaydeder:

* Sağlama kapsamında olan atanmış kullanıcılar için Azure AD sorgulama
* Hedef uygulama, bu kullanıcıların varlığı için sorgulanıyor
* Sistem arasındaki kullanıcı nesnelerini karşılaştırma
* Karşılaştırma temelinde hedef sistemde Kullanıcı hesabını ekleme, güncelleştirme veya devre dışı bırakma

Azure portal denetim günlüklerini okuma hakkında daha fazla bilgi için bkz. [sağlama Raporlama Kılavuzu](check-status-user-account-provisioning.md).

## <a name="how-long-will-it-take-to-provision-users"></a>Kullanıcıları sağlamak için ne kadar sürer?
Azure AD, bir uygulamayla otomatik Kullanıcı sağlamayı kullanırken, düzenli olarak zamanlanan bir zaman aralığında, genellikle her 40 dakikada bir [Kullanıcı ve grup ataması](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) gibi öğelere dayalı olarak bir uygulamadaki Kullanıcı hesaplarını otomatik olarak sağlar ve güncelleştirir.

Belirli bir kullanıcının sağlanması için gereken süre, genellikle sağlama işinizin ilk eşitleme veya artımlı eşitleme çalıştırıyor olmasına bağlıdır.

- **İlk eşitlemeler**için iş süresi, sağlama için kapsamdaki Kullanıcı ve grup sayısı ve kaynak sistemdeki Toplam Kullanıcı ve grup sayısı dahil olmak üzere birçok faktöre bağlıdır. Azure AD ve uygulama arasındaki ilk eşitleme, Azure AD dizininin boyutuna ve sağlama kapsamındaki kullanıcı sayısına bağlı olarak 20 dakikadan birkaç saate kadar sürebilir. İlk eşitleme performansını etkileyen faktörlerin kapsamlı bir listesi bu bölümün ilerleyen kısımlarında özetlenmektedir.

- İlk eşitlemeden sonra **artımlı eşitlemeler** için iş süreleri daha hızlı (ör. 10 dakika içinde) olur, çünkü sağlama hizmeti ilk eşitlemeden sonra her iki sistemin durumunu temsil eden filigranları depolar, sonraki eşitlemeler performansını geliştirir. İş saati, bu sağlama döngüsündeki algılanan değişiklik sayısına bağlıdır. 5\.000 'den az Kullanıcı veya grup üyeliği değişikliği varsa, iş tek bir artımlı sağlama döngüsünün içinde bitebilirler. 

Aşağıdaki tabloda, yaygın sağlama senaryolarının eşitleme süreleri özetlenmektedir. Bu senaryolarda, kaynak sistem Azure AD ve hedef sistem bir SaaS uygulamasıdır. Eşitleme süreleri, SaaS uygulamaları ServiceNow, Workplace, Salesforce ve G Suite için eşitleme işlerinin istatistiksel analizinden türetilir.


| Kapsam yapılandırması | Kapsam içindeki kullanıcılar, gruplar ve Üyeler | İlk eşitleme zamanı | Artımlı eşitleme süresi |
| -------- | -------- | -------- | -------- |
| Yalnızca atanan kullanıcıları ve grupları Eşitle |  < 1.000 |  < 30 dakika | < 30 dakika |
| Yalnızca atanan kullanıcıları ve grupları Eşitle |  1\.000 - 10.000 | 142-708 dakika | < 30 dakika |
| Yalnızca atanan kullanıcıları ve grupları Eşitle |   10.000-100.000 | 1\.170-2.340 dakika | < 30 dakika |
| Azure AD 'de tüm kullanıcıları ve grupları eşitleme |  < 1.000 | < 30 dakika  | < 30 dakika |
| Azure AD 'de tüm kullanıcıları ve grupları eşitleme |  1\.000 - 10.000 | < 30-120 dakika | < 30 dakika |
| Azure AD 'de tüm kullanıcıları ve grupları eşitleme |  10.000-100.000  | 713-1.425 dakika | < 30 dakika |
| Azure AD 'de tüm kullanıcıları eşitleme|  < 1.000  | < 30 dakika | < 30 dakika |
| Azure AD 'de tüm kullanıcıları eşitleme | 1\.000 - 10.000  | 43-86 dakika | < 30 dakika |


**Yalnızca yapılandırma eşitlemesi atanan kullanıcı ve gruplar**için aşağıdaki formülleri kullanarak yaklaşık en düşük ve en fazla beklenen **ilk eşitleme** sürelerini belirleyebilirsiniz:

    Minimum minutes =  0.01 x [Number of assigned users, groups, and group members]
    Maximum minutes = 0.08 x [Number of assigned users, groups, and group members] 
    
**İlk eşitlemenin**tamamlanışında geçen süreyi etkileyen faktörlerin Özeti:

- Sağlama için kapsamdaki Toplam Kullanıcı ve grup sayısı.

- Kaynak sistemde (Azure AD) bulunan kullanıcıların, grupların ve grup üyelerinin toplam sayısı.

- Sağlama kapsamındaki kullanıcıların hedef uygulamadaki mevcut kullanıcılarla eşleştirilip eşleştirilmediğini veya ilk kez oluşturulması gerektiğini belirtir. Tüm kullanıcılar için ilk kez oluşturulan eşitleme işleri, tüm kullanıcıların mevcut kullanıcılarla eşleştiği eşitleme işleri kadar *iki kez* sürer.

- [Denetim günlüklerindeki](check-status-user-account-provisioning.md)hataların sayısı. Çok sayıda hata varsa ve sağlama hizmeti bir karantina durumuna gittiğinden performans daha yavaştır.    

- Hedef sistem tarafından uygulanan istek hızı sınırları ve azaltma. Bazı hedef sistemler, büyük eşitleme işlemleri sırasında performansı etkileyebilecek istek hızı sınırlarını ve azaltmasını uygular. Bu koşullarda çok fazla sayıda istek alan bir uygulama, yanıt oranını yavaşlatabilir veya bağlantıyı kapatabilir. Performansı artırmak için bağlayıcının uygulama isteklerini, uygulamanın işleyebileceğinden daha hızlı göndermediğinden ayarlanması gerekir. Microsoft tarafından oluşturulan sağlama bağlayıcıları bu ayarlamayı yapar. 

- Atanan grupların sayısı ve boyutları. Atanan grupların eşitlenmesi, kullanıcıları eşitlemeye göre daha uzun sürer. Atanan grupların sayısı ve boyutları, performansı etkiler. Bir uygulamada, [Grup nesnesi eşitlemesi için etkin eşlemeler](customize-application-attributes.md#editing-group-attribute-mappings)varsa, grup adları ve üyelikleri gibi Grup Özellikleri, kullanıcılara ek olarak eşitlenir. Bu ek eşitlemeler, yalnızca kullanıcı nesnelerinden eşitlemeden daha uzun sürer.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile SaaS uygulamalarına kullanıcı hazırlama ve sağlamayı kaldırma işlemlerini otomatik hale getirme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning)
