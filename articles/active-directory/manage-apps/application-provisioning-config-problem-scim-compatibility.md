---
title: SCıM 2,0 protokol uyumluluğuyla ilgili bilinen sorunlar-Azure AD
description: Azure AD 'de SCıM 2,0 ' i destekleyen Galeri dışı bir uygulama eklenirken karşılaşılan yaygın protokol uyumluluk sorunlarını çözme
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
ms.date: 12/03/2018
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: c6da171db6535100342342571a5c1f6468abd0fc
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76712353"
---
# <a name="known-issues-and-resolutions-with-scim-20-protocol-compliance-of-the-azure-ad-user-provisioning-service"></a>Azure AD Kullanıcı sağlama hizmeti 'nin SCıM 2,0 protokol uyumluluğuyla ilgili bilinen sorunlar ve çözümleri

Azure Active Directory (Azure AD), [etki alanları arası kimlik yönetimi (SCıM) 2,0 protokol belirtimi Için sistemde](https://tools.ietf.org/html/draft-ietf-scim-api-19)tanımlanan arabirimi içeren bir Web hizmeti tarafından belirtilen herhangi bir uygulamaya veya sisteme Kullanıcı ve grupları otomatik olarak sağlayabilir. 

Azure AD 'nin SCıM 2,0 protokolü desteği, kullanıcıların ve grupların Azure AD 'den SCıM 2,0 ' i destekleyen uygulamalara otomatik olarak sağlanması için kullandığı protokolün belirli kısımlarını listeleyen, [etki alanları arası kimlik yönetimi (scım Azure Active Directory) Için sistem kullanma](use-scim-to-provision-users-and-groups.md)bölümünde açıklanmıştır.

Bu makalede, Azure AD Kullanıcı sağlama hizmeti 'nin SCıM 2,0 protokolüne uygunluğunu ve bu sorunları geçici olarak nasıl çözebileceğinizi açıklayan geçerli ve geçmişteki sorunlar açıklanmaktadır.

> [!IMPORTANT]
> Azure AD Kullanıcı sağlama hizmeti SCıM istemcisinin en son güncelleştirmesi 18 Aralık 2018 tarihinde yapıldı. Bu güncelleştirme, aşağıdaki tabloda listelenen bilinen uyumluluk sorunlarını ele alıyor. Bu güncelleştirme hakkında daha fazla bilgi için aşağıdaki sık sorulan sorulara bakın.

## <a name="scim-20-compliance-issues-and-status"></a>SCıM 2,0 uyumluluk sorunları ve durumu

| **SCıM 2,0 uyumluluk sorunu** |  **Düzenle?** | **Onarma tarihi**  |  
|---|---|---|
| Azure AD, uygulamanın SCıM uç nokta URL 'sinin kökünde olması için "/Scim" gerektirir  | Evet  |  18 Aralık 2018 | 
| Uzantı öznitelikleri, ":" gösterimi yerine öznitelik adlarından önce nokta "." gösterimini kullanır |  Evet  | 18 Aralık 2018  | 
|  Çok değerli öznitelikler için düzeltme eki istekleri geçersiz yol filtresi sözdizimi içeriyor | Evet  |  18 Aralık 2018  | 
|  Grup oluşturma istekleri geçersiz bir şema URI 'SI içeriyor | Evet  |  18 Aralık 2018  |  

## <a name="were-the-services-fixes-described-automatically-applied-to-my-pre-existing-scim-app"></a>Hizmet düzeltmeleri önceden mevcut SCIM uygulamama otomatik olarak uygulandı mi?

Hayır. Eski davranışla çalışacak şekilde kodlanmış SCIM uygulamalarına bir kırılırdı, değişiklikler mevcut uygulamalara otomatik olarak uygulanmaz.

Değişiklikler, düzeltilme tarihinden sonra Azure portal yapılandırılan tüm yeni [Galeri olmayan SCIM uygulamalarına](configure-single-sign-on-non-gallery-applications.md) uygulanır.

Önceden var olan bir Kullanıcı sağlama işini en son düzeltmeleri içerecek şekilde geçirme hakkında daha fazla bilgi için sonraki bölüme bakın.

## <a name="can-i-migrate-an-existing-scim-based-user-provisioning-job-to-include-the-latest-service-fixes"></a>Mevcut bir SCıM tabanlı Kullanıcı sağlama işini en son hizmet düzeltmelerini içerecek şekilde geçirebilir miyim?

Evet. Bu uygulama örneğini çoklu oturum açma için zaten kullanıyorsanız ve mevcut sağlama işini en son düzeltmeleri içerecek şekilde geçirmeniz gerekiyorsa aşağıdaki yordamı izleyin. Bu yordamda, mevcut SCıM uygulamanızdan eski sağlama işinizi kaldırmak ve yeni davranışı gösteren yeni bir tane oluşturmak için Microsoft Graph API ve Microsoft Graph API Explorer 'ın nasıl kullanılacağı açıklanmaktadır.

> [!NOTE]
> Uygulamanız hala geliştirilmeye devam ediyorsa ve henüz çoklu oturum açma ya da Kullanıcı sağlama için dağıtılmamışsa, en kolay çözüm Azure portal **Azure Active Directory > kurumsal uygulamalar** bölümünde uygulama girişini silmek ve uygulama Için **Galeri olmayan bir seçenek > Oluştur** seçeneğini kullanarak uygulama için yeni bir giriş eklemektir. Bu, aşağıdaki yordamı çalıştırmaya alternatiftir.
 
1. https://portal.azure.com Azure portal oturum açın.
2. Azure portal **Azure Active Directory > Enterprise Applications** bölümünde, mevcut SCIM uygulamanızı bulun ve seçin.
3. Mevcut SCıM uygulamanızın **Özellikler** bölümünde, **nesne kimliğini**kopyalayın.
4. Yeni bir Web tarayıcısı penceresinde, https://developer.microsoft.com/graph/graph-explorer gidin ve uygulamanızın eklendiği Azure AD kiracısı için yönetici olarak oturum açın.
5. Graph Explorer 'da, sağlama işinizin KIMLIĞINI bulmak için aşağıdaki komutu çalıştırın. "[Nesne-kimliği]" değerini üçüncü adımdan kopyalanmış hizmet sorumlusu KIMLIĞI (nesne KIMLIĞI) ile değiştirin.
 
   `GET https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs` 

   ![Işleri al](media/application-provisioning-config-problem-scim-compatibility/get-jobs.PNG "Işleri al") 


6. Sonuçlarda, "customappsso" veya "Scim" ile başlayan tam "ID" dizesini kopyalayın.
7. Öznitelik eşleme yapılandırmasını almak için aşağıdaki komutu çalıştırın, böylece bir yedekleme yapabilirsiniz. Daha önce olduğu gibi aynı [nesne-kimliği] kullanın ve [iş-kimliği] değerini son adımdan kopyalanmış sağlama işi KIMLIĞIYLE değiştirin.
 
   `GET https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs/[job-id]/schema`
 
   ![Şemayı al](media/application-provisioning-config-problem-scim-compatibility/get-schema.PNG "Şemayı al") 

8. JSON çıkışını son adımdan kopyalayın ve bir metin dosyasına kaydedin. Bu, eski uygulamanıza eklediğiniz tüm özel öznitelik eşlemelerini içerir ve yaklaşık binlerce satır JSON satırı olmalıdır.
9. Sağlama işini silmek için aşağıdaki komutu çalıştırın:
 
   `DELETE https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs/[job-id]`

10. En son hizmet düzeltmelerini içeren yeni bir sağlama işi oluşturmak için aşağıdaki komutu çalıştırın.

 `POST https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs`
 `{   templateId: "scim"   }`
   
11. Son adımın sonuçlarında, "Scim" ile başlayan tam "ID" dizesini kopyalayın. İsteğe bağlı olarak, aşağıdaki komutu çalıştırarak eski öznitelik eşlemelerinizi yeniden uygulayın, [New-id] öğesini yeni kopyaladığınız iş KIMLIĞIYLE değiştirerek ve adım #7, istek gövdesi olarak JSON çıkışını girerek.

 `POST https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs/[new-job-id]/schema`
 `{   <your-schema-json-here>   }`

12. İlk web tarayıcı penceresine dönün ve uygulamanız için **sağlama** sekmesini seçin.
13. Yapılandırmanızı doğrulayın ve sonra sağlama işini başlatın. 

## <a name="can-i-add-a-new-non-gallery-app-that-has-the-old-user-provisioning-behavior"></a>Eski Kullanıcı sağlama davranışına sahip yeni Galeri olmayan bir uygulama ekleyebilir miyim?

Evet. Düzeltmelerden önce varolan eski davranışa bir uygulamayı kodlandırdıysanız ve yeni bir örneğini dağıtmanız gerekiyorsa, aşağıdaki yordamı izleyin. Bu yordamda, eski davranışı gösteren bir SCıM sağlama işi oluşturmak için Microsoft Graph API ve Microsoft Graph API Explorer 'ın nasıl kullanılacağı açıklanmaktadır.
 
1. https://portal.azure.com Azure portal oturum açın.
2. **Azure Active Directory > kurumsal uygulamalar > uygulama oluştur** Azure Portal bölümünde, **Galeri olmayan** yeni bir uygulama oluşturun.
3. Yeni özel uygulamanızın **Özellikler** bölümünde, **nesne kimliğini**kopyalayın.
4. Yeni bir Web tarayıcısı penceresinde, https://developer.microsoft.com/graph/graph-explorer gidin ve uygulamanızın eklendiği Azure AD kiracısı için yönetici olarak oturum açın.
5. Graph Explorer 'da, uygulamanız için sağlama yapılandırmasını başlatmak üzere aşağıdaki komutu çalıştırın.
   "[Nesne-kimliği]" değerini üçüncü adımdan kopyalanmış hizmet sorumlusu KIMLIĞI (nesne KIMLIĞI) ile değiştirin.

   `POST https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs`
   `{   templateId: "customappsso"   }`
 
6. İlk web tarayıcı penceresine dönün ve uygulamanız için **sağlama** sekmesini seçin.
7. Normalde yaptığınız gibi Kullanıcı sağlama yapılandırmasını doldurun.


## <a name="next-steps"></a>Sonraki adımlar
[SaaS uygulamalarına sağlama ve sağlamayı kaldırma hakkında daha fazla bilgi edinin](user-provisioning.md)

