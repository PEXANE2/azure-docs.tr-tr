---
title: SaaS uygulamalarına otomatik Kullanıcı hesabı sağlamayı bildir | Microsoft Docs
description: Otomatik Kullanıcı hesabı sağlama işlerinin durumunu denetlemeyi ve bireysel kullanıcıların sağlanması hakkında nasıl sorun gidermeyi öğrenin.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.date: 09/09/2018
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: fda7654ca2d825ae4112dd06021c7e83ed6867cd
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68381250"
---
# <a name="tutorial-reporting-on-automatic-user-account-provisioning"></a>Öğretici: Otomatik Kullanıcı hesabı sağlama üzerinde raporlama

Azure Active Directory (Azure AD), uçtan uca kimlik yaşam döngüsü yönetimi amacıyla SaaS uygulamalarındaki ve diğer sistemlerdeki Kullanıcı hesaplarının sağlamasını otomatik hale getirmeye yardımcı olan bir [Kullanıcı hesabı sağlama hizmeti](user-provisioning.md) içerir. Azure AD, [Azure AD uygulama galerisinin](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1&subcategories=featured)"öne çıkan" bölümündeki tüm uygulamalar ve sistemler için önceden tümleştirilmiş Kullanıcı sağlama bağlayıcılarını destekler.

Bu makalede, kurulumu yapıldıktan sonra sağlama işlerinin durumunun nasıl denetleneceği ve bireysel kullanıcıların ve grupların sağlanmasından nasıl giderileceği açıklanmaktadır.

## <a name="overview"></a>Genel Bakış

Sağlama bağlayıcıları, desteklenen uygulama için [belirtilen belgeleri](../saas-apps/tutorial-list.md) izleyerek [Azure Portal](https://portal.azure.com)kullanılarak ayarlanır ve yapılandırılır. Yapılandırılıp çalıştırıldığında, sağlama işleri iki yöntemden birini kullanarak bildirilebilir:

* **Azure Portal** -Bu makalede öncelikle, bir sağlama Özeti raporu ve belirli bir uygulama için ayrıntılı sağlama denetim günlükleri sağlayan [Azure Portal](https://portal.azure.com)rapor bilgilerinin alınması açıklanmaktadır.
* **Denetim API 'si** -Azure Active Directory ayrıntılı sağlama denetim günlüklerinin programlı bir şekilde alınmasına izin veren bir denetim API 'si de sağlar. Bu API 'yi kullanmaya özgü belgeler için bkz. [Azure Active Directory Denetim API 'si başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) . Bu makale, API 'yi nasıl kullanacağınızı özellikle kapsamadığından, denetim günlüğüne kaydedilen sağlama olaylarının türlerini ayrıntılandırır.

### <a name="definitions"></a>Tanımlar

Bu makale aşağıda tanımlanan aşağıdaki terimleri kullanır:

* **Kaynak sistem** -Azure AD sağlama hizmeti 'nin eşitlendiği kullanıcı deposu. Azure Active Directory, önceden tümleştirilmiş sağlama bağlayıcılarının çoğunluğunun kaynak sistemidir, ancak bazı özel durumlar vardır (örnek: İşgünü gelen eşitleme).
* **Hedef sistem** -Azure AD sağlama hizmeti 'nin eşitlendiği kullanıcı deposu. Bu genellikle bir SaaS uygulamasıdır (örnekler: Salesforce, ServiceNow, G Suite, Iş için Dropbox), ancak bazı durumlarda Active Directory gibi bir şirket içi sistem olabilir (örnek: Active Directory için Workday gelen eşitlemesi).

## <a name="getting-provisioning-reports-from-the-azure-portal"></a>Azure portal sağlama raporlarının alınması

Belirli bir uygulama için sağlama raporu bilgilerini almak üzere [Azure Portal](https://portal.azure.com) başlatarak ve sağlama yapılandırılan kurumsal uygulamaya göz atarak başlatın. Örneğin, kullanıcıları LinkedIn 'e sağlıyorsanız, uygulama ayrıntılarının gezinti yolu şu şekilde olur:

**Azure Active Directory tüm uygulamalar > > Kurumsal uygulamalar > LinkedIn yükselt**

Buradan, sağlama Özeti raporuna ve sağlama denetim günlüklerine her ikisi de açıklanan şekilde erişebilirsiniz.

## <a name="provisioning-summary-report"></a>Sağlama Özeti raporu

Sağlama Özeti raporu, verilen uygulamanın **sağlama** sekmesinde görünür. Bu, **Ayarlar**' ın altındaki **eşitleme ayrıntıları** bölümünde bulunur ve aşağıdaki bilgileri sağlar:

* Eşitlenmiş ve kaynak sistem ile hedef sistem arasında sağlama için kapsamda bulunan ve şu anda kapsamdaki Toplam Kullanıcı ve/grup sayısı.
* Eşitlemenin son çalıştırıldığı zaman. Eşitlemeler tipik olarak, [ilk eşitleme](user-provisioning.md#what-happens-during-provisioning) tamamlandıktan sonra her 20-40 dakikada bir gerçekleşir.
* Bir [İlk eşitlemenin](user-provisioning.md#what-happens-during-provisioning) tamamlanıp tamamlanmadığını belirtir.
* Sağlama işleminin karantinaya alınmış olup olmadığı ve karantina durumunun nedenleri (örneğin, geçersiz yönetici kimlik bilgileri nedeniyle hedef sistemle iletişim kurma hatası).

Sağlama Özeti raporu, yöneticinin sağlama işinin işletimsel durumunu denetlemek için ilk yerde yönetici tarafından bakabilmelidir.

 ![Özet rapor](./media/check-status-user-account-provisioning/summary_report.PNG)

## <a name="provisioning-audit-logs"></a>Denetim günlüklerini sağlama

Sağlama hizmeti tarafından gerçekleştirilen tüm etkinlikler, **hesap sağlama** kategorisi altındaki **Denetim günlükleri** sekmesinde görüntülenebilen Azure AD denetim günlüklerine kaydedilir. Günlüğe kaydedilen etkinlik olay türleri şunları içerir:

* **Olayları Içeri aktarma** -Azure AD sağlama hizmeti 'nin bir kaynak sistem veya hedef sistemden tek bir kullanıcı veya grup hakkındaki bilgileri aldığı her seferinde bir "içeri aktarma" olayı kaydedilir. Eşitleme sırasında, kullanıcılar ilk olarak kaynak sistemden alınır ve sonuçlar "içeri aktarma" olayları olarak kaydedilir. Daha sonra, alınan kullanıcıların eşleşen kimlikleri, mevcut olup olmadığını denetlemek için hedef sisteme göre sorgulanır ve ayrıca "içeri aktar" olayları olarak kaydedilir. Bu olaylar, tüm eşlenmiş Kullanıcı özniteliklerini ve olay sırasında Azure AD sağlama hizmeti tarafından görülen değerleri kaydeder.
* **Eşitleme kuralı olayları** -bu olaylar, Kullanıcı verileri alındıktan ve kaynak ve hedef sistemlerden hesaplandıktan sonra, öznitelik eşleme kurallarının ve yapılandırılmış tüm kapsam filtrelerinin sonuçları hakkında raporlar. Örneğin, bir kaynak sistemdeki bir kullanıcının sağlama için kapsam içinde olduğu kabul edilir ve hedef sistemde mevcut değil olarak kabul edilir. bu olay, kullanıcının hedef sistemde sağlanacağı kayıt olur.
* **Olayları dışarı aktar** -Azure AD sağlama hizmeti bir hedef sisteme bir kullanıcı hesabı veya grup nesnesi yazdığında bir "dışarı aktar" olayı kaydedilir. Bu olaylar, olay sırasında Azure AD sağlama hizmeti tarafından yazılan tüm Kullanıcı özniteliklerini ve değerlerini kaydeder. Hedef sisteme Kullanıcı hesabı veya grup nesnesi yazılırken bir hata oluşursa burada görüntülenecek.
* **İşlem Emanet olayları** -işlem escrows, sağlama hizmeti bir işlem gerçekleştirmeye çalışırken bir hatayla karşılaştığında oluşur ve işlemi bir geri alma zaman aralığında yeniden denemeye başlar. Bir sağlama işlemi yeniden denenilişinde "Emanet" olayı kaydedilir.

Tek bir kullanıcıya yönelik sağlama olayları aranırken, olaylar normalde bu sırada oluşur:

1. İçeri aktarma olayı: Kullanıcı kaynak sistemden alındı.
1. İçeri aktarma olayı: Hedef sistem, alınan kullanıcının varlığını denetlemek için sorgulanır.
1. Eşitleme kuralı olayı: Kaynak ve hedef sistemlerden Kullanıcı verileri, varsa hangi eylemin gerçekleştirilmesi gerektiğini belirlemek için yapılandırılmış öznitelik eşleme kuralları ve kapsam filtreleriyle değerlendirilir.
1. Dışarı aktarma olayı: Eşitleme kuralı olayı bir eylemin gerçekleştirilmesi gerektiğini (Ekle, Güncelleştir, Sil) dikte ederseniz, eylemin sonuçları dışarı aktarma olayına kaydedilir.

   ![Örnek: Etkinlikleri ve durumu gösteren denetim günlüğü sayfası](./media/check-status-user-account-provisioning/audit_logs.PNG)

### <a name="looking-up-provisioning-events-for-a-specific-user"></a>Belirli bir kullanıcı için sağlama olayları aranıyor

Sağlama denetim günlükleri için en yaygın kullanım durumu, bireysel bir kullanıcı hesabının sağlama durumunu denetkullanmaktır. Belirli bir kullanıcı için son sağlama olaylarını aramak için:

1. **Denetim günlükleri** bölümüne gidin.
1. **Kategori** menüsünde **hesap sağlama**' yı seçin.
1. **Tarih aralığı** menüsünde, aramak istediğiniz tarih aralığını seçin.
1. **Arama** çubuğunda, aramak istediğiniz kullanıcının kullanıcı kimliğini girin. KIMLIK değeri biçimi öznitelik eşleme yapılandırmasında (örneğin, userPrincipalName veya çalışan KIMLIĞI numarası) birincil eşleşen KIMLIK olarak seçtiğiniz her şeyle eşleşmelidir. Gerekli KIMLIK değeri, hedef sütununda görünür olacak.
1. Aramak için ENTER tuşuna basın. En son sağlama olayları ilk olarak döndürülecek.
1. Olaylar döndürülürse, etkinlik türleri ve başarılı ya da başarısız olup olmadığını dikkate alın. Hiçbir sonuç döndürülmezse, Kullanıcı yok ya da tam eşitleme henüz tamamlanmadıysa sağlama işlemi tarafından henüz algılanmadı demektir.
1. Alınan, değerlendirilen veya olayın bir parçası olarak yazılmış tüm Kullanıcı özellikleri dahil olmak üzere, genişletilmiş ayrıntıları görüntülemek için tek tek olaylara tıklayın.

Denetim günlüklerinin kullanımıyla ilgili bir gösterim için aşağıdaki videoya bakın. Denetim günlükleri 5:30 işareti etrafında sunulmaktadır:

> [!VIDEO https://www.youtube.com/embed/pKzyts6kfrw]

### <a name="tips-for-viewing-the-provisioning-audit-logs"></a>Sağlama denetim günlüklerini görüntüleme ipuçları

Azure portal en iyi okunabilirlik için, **sütunlar** düğmesini seçin ve şu sütunları seçin:

* **Tarih** -olayın gerçekleştiği tarihi gösterir.
* **Hedef (ler)** -olayın konuları olan uygulama adını ve Kullanıcı kimliğini gösterir.
* **Etkinlik** -daha önce açıklandığı gibi etkinlik türü.
* **Durum** -olayın başarılı olup olmadığını belirtir.
* **Durum nedeni** -sağlama olayında ne olduğunu bir Özet.

## <a name="troubleshooting"></a>Sorun giderme

Sağlama Özeti raporu ve denetim günlükleri, yöneticilerin çeşitli kullanıcı hesabı sağlama sorunlarını gidermesine yardımcı olan bir anahtar rol oynar.

Otomatik Kullanıcı sağlama sorunlarını gidermeye yönelik senaryo tabanlı yönergeler için bkz. [kullanıcıları yapılandırma ve uygulama sağlama sorunları](application-provisioning-config-problem.md).

## <a name="additional-resources"></a>Ek Kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](what-is-single-sign-on.md)
