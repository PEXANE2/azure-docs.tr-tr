---
title: Otomatik kullanıcı hesabı sağlamayı SaaS uygulamalarına bildirme
description: Otomatik kullanıcı hesabı sağlama işlerinin durumunu nasıl denetleştirip tek tek kullanıcıların sağlanmasında nasıl sorun giderilenöğrenin.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.tgt_pltfrm: app-provisioning
ms.devlang: na
ms.topic: conceptual
ms.date: 09/09/2018
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 19d76f69669ffa13d1d55ffa807e6c4818b8840c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80282197"
---
# <a name="tutorial-reporting-on-automatic-user-account-provisioning"></a>Öğretici: Otomatik kullanıcı hesabı sağlama hakkında raporlama

Azure Etkin Dizin (Azure AD), uçtan uca kimlik yaşam döngüsü yönetimi amacıyla SaaS uygulamalarında ve diğer sistemlerde kullanıcı hesaplarının sağlanmasının otomatikleştirilmesine yardımcı olan bir [kullanıcı hesabı sağlama hizmeti](user-provisioning.md) içerir. Azure AD, tüm uygulamalar ve sistemler için önceden tümleşik kullanıcı sağlama bağlayıcılarını [burada](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)kullanıcı sağlama öğreticileri ile destekler.

Bu makalede, iş verme durumu ayarlandıktan sonra nasıl denetlendirilebilen ve tek tek kullanıcıların ve grupların sağlanmasında nasıl sorun giderilen açıklanmaktadır.

## <a name="overview"></a>Genel Bakış

Sağlama bağlayıcıları, desteklenen uygulama için [sağlanan belgeleri](../saas-apps/tutorial-list.md) izleyerek [Azure portalı](https://portal.azure.com)kullanılarak ayarlanır ve yapılandırılır. Yapılandırıldıktan ve çalıştırıldıktan sonra, sağlama işleri iki yöntemden biri kullanılarak raporlanabilir:

* **Azure portalı** - Bu makalede, öncelikle, hem bir sağlama özeti raporu hem de belirli bir uygulama için ayrıntılı sağlama denetim günlükleri sağlayan [Azure portalından](https://portal.azure.com)rapor bilgilerinin alınması açıklanmaktadır.
* **Denetim API' si** - Azure Active Directory, ayrıntılı sağlama denetim günlüklerinin programlı olarak alınmasını sağlayan bir Denetim API'si de sağlar. Bu API'yi kullanmaya özgü belgeler için [Azure Etkin Dizin denetimi API başvurusuna](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) bakın. Bu makalede API'nin nasıl kullanılacağı özel olarak kapsamaktadır, ancak denetim günlüğüne kaydedilen sağlama olaylarının türlerini ayrıntılandırmaz.

### <a name="definitions"></a>Tanımlar

Bu makalede, aşağıda tanımlanan aşağıdaki terimler kullanınız:

* **Kaynak Sistem** - Azure AD sağlama hizmetinin senkronize ettiği kullanıcı deposu. Azure Etkin Dizin, önceden tümleşik sağlama bağlayıcılarının çoğunun kaynak sistemidir, ancak bazı özel durumlar vardır (örnek: İş günü Gelen Eşitleme).
* **Hedef Sistem** - Azure AD sağlama hizmetinin eşitletirolduğu kullanıcı deposu. Bu genellikle bir SaaS uygulamasıdır (örnekler: Salesforce, ServiceNow, G Suite, Dropbox for Business), ancak bazı durumlarda Active Directory (örnek: İş Günü Gelen Eşzamanlılık Active Directory) gibi bir şirket içi sistem olabilir.

## <a name="getting-provisioning-reports-from-the-azure-portal"></a>Azure portalından sağlama raporları alma

Belirli bir uygulama için sağlama raporu bilgilerini almak için, **Etkinlik** bölümünde [Azure portalı](https://portal.azure.com) ve Azure **Active Directory** &gt; **Enterprise Apps** &gt; **Sağlama günlüklerini (önizleme)** başlatarak başlayın. Ayrıca, sağlamanın yapılandırıldığı Kurumsal Uygulama'ya da göz atabilirsiniz. Örneğin, kullanıcıları LinkedIn Yükseltme'ye uygun olarak salarsanız, uygulama ayrıntılarına giden gezinti yolu şudur:

**Azure Active Directory > Kurumsal Uygulamalar > Tüm uygulamalar LinkedIn Yükseltme >**

Buradan, hem sağlama ilerleme çubuğuna hem de aşağıda açıklanan sağlama günlüklerine erişebilirsiniz.

## <a name="provisioning-progress-bar"></a>İlerleme çubuğu sağlama

[Sağlama ilerleme çubuğu,](application-provisioning-when-will-provisioning-finish-specific-user.md#view-the-provisioning-progress-bar) verilen uygulama için **Sağlama** sekmesinde görünür. **Ayarlar'ın**altındaki **Geçerli Durum** bölümünde yer alır ve geçerli başlangıç veya artımlı çevrimin durumunu gösterir. Bu bölümde ayrıca şunları da gösterilmektedir:

* Senkronize edilmiş ve şu anda kaynak sistem ile hedef sistem arasında sağlama kapsamında olan toplam kullanıcı ve/grup sayısı.
* Eşitleme en son çalıştırıldı. Eşitlemeler genellikle her 20-40 dakikada bir, bir [ilk döngü](../app-provisioning/how-provisioning-works.md#provisioning-cycles-initial-and-incremental) tamamlandıktan sonra gerçekleşir.
* [İlk döngütamamlanıp](../app-provisioning/how-provisioning-works.md#provisioning-cycles-initial-and-incremental) tamamlanmadığı.
* Sağlama işleminin karantinaya alınıp alınmadığı ve karantina durumunun nedeninin ne olduğu (örneğin, geçersiz yönetici kimlik bilgileri nedeniyle hedef sistemle iletişim kurmama).

**Mevcut Durum,** yöneticilerin, tedarik işinin operasyonel durumunu kontrol etmek için baktıkları ilk yer olmalıdır.

 ![Özet rapor](./media/check-status-user-account-provisioning/provisioning-progress-bar-section.png)

## <a name="provisioning-logs-preview"></a>Günlükleri sağlama (önizleme)

Sağlama hizmeti tarafından gerçekleştirilen tüm etkinlikler Azure AD [sağlama günlüklerine](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context)kaydedilir. **Etkinlik** bölümünde **Azure Active Directory** &gt; **Enterprise Apps** &gt; **Sağlama günlüklerini (önizleme)** seçerek Azure portalındaki sağlama günlüklerine erişebilirsiniz. Sağlama verilerini, kaynak sistemde veya hedef sistemde kullanıcının adına veya tanımlayıcıya göre arayabilirsiniz. Ayrıntılar için, [Sağlama günlükleri (önizleme)](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context)bakın. Günlüğe kaydedilmiş etkinlik olay türleri şunlardır:

## <a name="troubleshooting"></a>Sorun giderme

Özet raporu ve sağlama günlükleri, yöneticilerin çeşitli kullanıcı hesabı sağlama sorunlarını gidermelerine yardımcı olmak için önemli bir rol oynar.

Otomatik kullanıcı sağlama sorunlarının nasıl giderilene ilişkin senaryo tabanlı kılavuz için, [kullanıcıları bir uygulamaya yapılandırma ve sağlama sorunları](../app-provisioning/application-provisioning-config-problem.md)konusuna bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
