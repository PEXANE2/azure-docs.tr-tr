---
title: SCIM 2.0 protokol uyumluluğu ile bilinen sorunlar - Azure AD
description: SCIM 2.0'ı Azure AD'ye destekleyen galeri dışı bir uygulama eklerken karşılaşılan ortak protokol uyumluluğu sorunları nasıl çözülür?
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
ms.date: 12/03/2018
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6dff0d4f8f0062c00351b60174c63d9c19bdfa15
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77522943"
---
# <a name="known-issues-and-resolutions-with-scim-20-protocol-compliance-of-the-azure-ad-user-provisioning-service"></a>Azure AD Kullanıcı Sağlama hizmetinin SCIM 2.0 protokolü uyumluluğu ile bilinen sorunlar ve çözümler

Azure Etkin Dizin (Azure AD), kullanıcıları ve grupları, [Etki Alanı Arası Kimlik Yönetimi Sistemi (SCIM) 2.0 protokol belirtiminde](https://tools.ietf.org/html/draft-ietf-scim-api-19)tanımlanan arabirimi yle bir web hizmeti tarafından ön koşulan herhangi bir uygulamaya veya sisteme otomatik olarak sağlayabilir. 

Azure AD'nin SCIM 2.0 protokolüne verdiği destek, [kullanıcıları ve grupları Azure Active Directory'den uygulamalara otomatik olarak sağlamak için Alan Arası Kimlik Yönetimi Için Sistemi Kullanma 'da (SCIM)](use-scim-to-provision-users-and-groups.md)açıklanmıştır ve bu protokol, Azure AD'den kullanıcıları ve grupları SCIM 2.0'ı destekleyen uygulamalara otomatik olarak sağlamak için uyguladığı protokolün belirli bölümlerini listeler.

Bu makalede, Azure AD kullanıcı sağlama hizmetinin SCIM 2.0 protokolüne bağlılığı yla ilgili güncel ve geçmiş sorunlar ve bu sorunların nasıl çözüme bu şekilde çalışılalı şıyıyış lar açıklanmaktadır.

> [!IMPORTANT]
> Azure AD kullanıcı sağlama hizmeti SCIM istemcisi için yapılan en son güncelleştirme 18 Aralık 2018 tarihinde yapılmıştır. Bu güncelleştirme, aşağıdaki tabloda listelenen bilinen uyumluluk sorunlarını ele aldı. Bu güncelleştirme hakkında daha fazla bilgi için aşağıdaki sık sorulan sorulara bakın.

## <a name="scim-20-compliance-issues-and-status"></a>SCIM 2.0 uyumluluk sorunları ve durumu

| **SCIM 2.0 uyumluluk sorunu** |  **Sabit?** | **Tarihi düzeltme**  |  
|---|---|---|
| Azure AD, uygulamanın SCIM bitiş noktası URL'sinin kökünde "/scim" olmasını gerektirir  | Evet  |  18 Aralık 2018 | 
| Uzantı öznitelikleri nokta kullanın "." notasyon önce öznitelik adları yerine iki nokta ":" gösterimi |  Evet  | 18 Aralık 2018  | 
|  Çok değerli öznitelikler için yama istekleri geçersiz yol filtresi sözdizimini içerir | Evet  |  18 Aralık 2018  | 
|  Grup oluşturma istekleri geçersiz bir şema URI içerir | Evet  |  18 Aralık 2018  |  

## <a name="were-the-services-fixes-described-automatically-applied-to-my-pre-existing-scim-app"></a>Açıklanan hizmet düzeltmeleri önceden varolan SCIM uygulamama otomatik olarak uygulandı mı?

Hayır. Eski davranışla çalışmak üzere kodlanmış SCIM uygulamalarında bir kırılma değişikliği oluşturacağı için, değişiklikler otomatik olarak varolan uygulamalara uygulanmadı.

Değişiklikler, düzeltme tarihinden sonra Azure portalında yapılandırılan tüm [galeri dışı SCIM uygulamalarına](../manage-apps/configure-single-sign-on-non-gallery-applications.md) uygulanır.

En son düzeltmeleri içerecek şekilde önceden varolan bir kullanıcı sağlama işini nasıl geçirteceğimüz hakkında bilgi için bir sonraki bölüme bakın.

## <a name="can-i-migrate-an-existing-scim-based-user-provisioning-job-to-include-the-latest-service-fixes"></a>Varolan bir SCIM tabanlı kullanıcı sağlama işini en son hizmet düzeltmelerini içerecek şekilde geçirebilir miyim?

Evet. Bu uygulama örneğini zaten tek oturum açma için kullanıyorsanız ve en son düzeltmeleri içerecek şekilde varolan sağlama işini geçirmeniz gerekiyorsa, aşağıdaki yordamı izleyin. Bu yordam, eski sağlama işinizi varolan SCIM uygulamanızdan kaldırmak ve yeni davranışı sergileyen yeni bir iş oluşturmak için Microsoft Graph API ve Microsoft Graph API gezgininin nasıl kullanılacağıaçıklanır.

> [!NOTE]
> Uygulamanız hala geliştiriliyorsa ve tek oturum açma veya kullanıcı sağlama için henüz dağıtılmadıysa, en kolay çözüm Azure portalının **Azure Etkin Dizin > Kurumsal Uygulamalar** bölümündeki uygulama girişini silmek ve uygulama oluşturma > Galeri **Dışı** seçeneğini kullanarak uygulama için yeni bir giriş eklemektir. Bu yordamı aşağıda çalıştırmak için bir alternatiftir.
 
1. Azure portalında oturum https://portal.azure.comaç.
2. Azure portalının **Azure Etkin Dizin > Kurumsal Uygulamalar** bölümünde, mevcut SCIM uygulamanızı bulun ve seçin.
3. Mevcut SCIM uygulamanızın **Özellikler** bölümünde **Nesne Kimliğini**kopyalayın.
4. Yeni bir web tarayıcısı https://developer.microsoft.com/graph/graph-explorer penceresinde, uygulamanızın eklendiği Azure AD kiracısının yöneticisi olarak oturum açın.
5. Grafik Gezgini'nde, sağlama işinizin kimliğini bulmak için aşağıdaki komutu çalıştırın. Üçüncü adımdan kopyalanan hizmet temel kimliği (nesne kimliği) ile "[object-id]" değiştirin.
 
   `GET https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs` 

   ![İş Bul](media/application-provisioning-config-problem-scim-compatibility/get-jobs.PNG "İş Bul") 


6. Sonuçlarda, "customappsso" veya "scim" ile başlayan tam "ID" dizesini kopyalayın.
7. Öznitelik eşleme yapılandırmasını almak için aşağıdaki komutu çalıştırın, böylece yedekleme yapabilirsiniz. Eskisi gibi aynı [nesne-id]'i kullanın ve [iş kimliği] ile son adımdan kopyalanan iş kimliği değiştirin.
 
   `GET https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs/[job-id]/schema`
 
   ![Şema alın](media/application-provisioning-config-problem-scim-compatibility/get-schema.PNG "Şema alın") 

8. Son adımdaki JSON çıktısını kopyalayın ve bir metin dosyasına kaydedin. Bu, eski uygulamanıza eklediğiniz özel öznitelik eşlemelerini içerir ve yaklaşık birkaç bin JSON satırı olmalıdır.
9. Sağlama işini silmek için aşağıdaki komutu çalıştırın:
 
   `DELETE https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs/[job-id]`

10. En son hizmet düzeltmeleri olan yeni bir sağlama işi oluşturmak için aşağıdaki komutu çalıştırın.

 `POST https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs`
 `{   templateId: "scim"   }`
   
11. Son adımın sonuçlarında, "scim" ile başlayan tam "ID" dizesini kopyalayın. İsteğe bağlı olarak, aşağıdaki komutu çalıştırarak, [yeni iş kimliği] yerine yeni iş kimliği ni ekleyerek ve istek gövdesi olarak adım #7 JSON çıktısını girerek eski öznitelik haritalarınızı yeniden uygulayın.

 `POST https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs/[new-job-id]/schema`
 `{   <your-schema-json-here>   }`

12. İlk web tarayıcısı penceresine dönün ve uygulamanız için **Sağlama** sekmesini seçin.
13. Yapılandırmanızı doğrulayın ve sağlama işini başlatın. 

## <a name="can-i-add-a-new-non-gallery-app-that-has-the-old-user-provisioning-behavior"></a>Eski kullanıcı sağlama davranışına sahip yeni bir galeri dışı uygulama ekleyebilir miyim?

Evet. Düzeltmelerden önce var olan eski davranışa bir uygulama kodladıysanız ve bunun yeni bir örneğini dağıtmanız gerekiyorsa, aşağıdaki yordamı izleyin. Bu yordam, eski davranışı sergileyen bir SCIM sağlama işi oluşturmak için Microsoft Graph API ve Microsoft Graph API gezgininin nasıl kullanılacağıaçıklanır.
 
1. Azure portalında oturum https://portal.azure.comaç.
2. Azure **Etkin Dizin > Kurumsal Uygulamalar >** Azure portalının uygulama bölümünü oluşturun, yeni bir galeri **dışı** uygulama oluşturun.
3. Yeni özel uygulamanızın **Özellikler** bölümünde **Nesne Kimliğini**kopyalayın.
4. Yeni bir web tarayıcısı https://developer.microsoft.com/graph/graph-explorer penceresinde, uygulamanızın eklendiği Azure AD kiracısının yöneticisi olarak oturum açın.
5. Grafik Gezgini'nde, uygulamanızın sağlama yapılandırmasını başlatmayı sağlamak için aşağıdaki komutu çalıştırın.
   Üçüncü adımdan kopyalanan hizmet temel kimliği (nesne kimliği) ile "[object-id]" değiştirin.

   `POST https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs`
   `{   templateId: "customappsso"   }`
 
6. İlk web tarayıcısı penceresine dönün ve uygulamanız için **Sağlama** sekmesini seçin.
7. Normalde yaptığınız gibi kullanıcı sağlama yapılandırmasını tamamlayın.


## <a name="next-steps"></a>Sonraki adımlar
[SaaS uygulamalarına sağlama ve sağlamanın geri verilmesi hakkında daha fazla bilgi edinin](user-provisioning.md)

