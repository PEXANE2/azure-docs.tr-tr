---
title: Şirket içi uygulamaları Bulut Uygulaması Güvenliği ile tümleştirme - Azure AD
description: Azure Active Directory'deki şirket içi bir uygulamayı Microsoft Cloud App Security (MCAS) ile çalışacak şekilde yapılandırın. Koşullu Erişim ilkelerine dayalı olarak oturumları gerçek zamanlı olarak izlemek ve denetlemek için MCAS Koşullu Erişim Uygulama Denetimini kullanın. Bu ilkeleri Azure Etkin Dizini'nde (Azure AD) Uygulama Proxy'sini kullanan şirket içi uygulamalara uygulayabilirsiniz.
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 12/19/2018
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: eb97f9dd87277215a5d4708d3a6f49564c490204
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74275506"
---
# <a name="configure-real-time-application-access-monitoring-with-microsoft-cloud-app-security-and-azure-active-directory"></a>Microsoft Cloud App Security ve Azure Active Directory ile gerçek zamanlı uygulama erişim izlemeyi yapılandırma
Azure Active Directory'de (Azure AD) bir şirket içi uygulamayı, gerçek zamanlı izleme için Microsoft Bulut Uygulama Güvenliği'ni (MCAS) kullanacak şekilde yapılandırın. MCAS, Koşullu Erişim ilkelerine dayalı olarak oturumları gerçek zamanlı olarak izlemek ve denetlemek için Koşullu Erişim Uygulama Denetimi'ni kullanır. Bu ilkeleri Azure Etkin Dizini'nde (Azure AD) Uygulama Proxy'sini kullanan şirket içi uygulamalara uygulayabilirsiniz.

MCAS ile oluşturabileceğiniz ilke türlerine bazı örnekler aşağıda verilmiştir:

- Yönetilmeyen aygıtlarda hassas belgelerin karşıdan yüklendiğini engelleyin veya koruyun.
- Yüksek riskli kullanıcıların uygulamalarda oturum açmalarını izleyin ve eylemlerini oturum içinden günlüğe kaydedin. Bu bilgilerle, oturum ilkelerinin nasıl uygulanacağınızı belirlemek için kullanıcı davranışını çözümleyebilirsiniz.
- Yönetilmeyen aygıtlardan belirli uygulamalara erişimi engellemek için istemci sertifikalarını veya aygıt uyumluluğunu kullanın.
- Şirket dışı ağlardaki kullanıcı oturumlarını kısıtlayın. Bir uygulamaya şirket ağınızın dışından erişen kullanıcılara kısıtlı erişim izni verebilirsiniz. Örneğin, bu kısıtlı erişim kullanıcının hassas belgeleri karşıdan yüklemesini engelleyebilir.

Daha fazla bilgi için Bkz. [Microsoft Cloud App Security Koşullu Erişim Uygulama Denetimi'ne sahip uygulamaları koru.](/cloud-app-security/proxy-intro-aad)

## <a name="requirements"></a>Gereksinimler

Lisans:

- EMS E5 lisansı veya 
- Azure Active Directory Premium P1 ve MCAS Standalone.

Şirket içi başvuru:

- Şirket içi uygulama Kerberos Kısıtlı Delegasyonu (KCD) kullanmalıdır

Uygulama Proxy'yi yapılandır:

- Azure AD'yi ortamınızı hazırlama ve Uygulama Proxy bağlayıcısını yükleme dahil olmak üzere Uygulama Proxy'sini kullanacak şekilde yapılandırın. Bir öğretici için bkz: [Azure AD'deki Application Proxy aracılığıyla uzaktan erişim için şirket içi uygulamalar ekleyin.](application-proxy-add-on-premises-application.md) 

## <a name="add-on-premises-application-to-azure-ad"></a>Azure AD'ye şirket içi uygulama ekleme

Azure AD'ye şirket içi bir uygulama ekleyin. Hızlı bir başlangıç için azure [AD'ye şirket içi uygulama ekleme'ye](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad)bakın. Uygulamayı eklerken, **şirket içi uygulama** bıçak ekle'de aşağıdaki iki ayarı ayarladığınızdan emin olun:

- **Ön Kimlik Doğrulama**: **Azure Etkin Dizini**girin.
- **URL'leri Uygulama Gövdesinde Çevir**: **Evet'i**seçin .

Uygulamanın MCAS ile çalışması için bu iki ayar gereklidir.

## <a name="test-the-on-premises-application"></a>Şirket içi başvuruyu test edin

Uygulamanızı Azure AD'ye ekledikten sonra, test için bir kullanıcı eklemek ve oturum açmayı test etmek için [uygulamayı test](application-proxy-add-on-premises-application.md#test-the-application) etme adımlarını kullanın. 

## <a name="deploy-conditional-access-app-control"></a>Koşullu Erişim Uygulama Denetimini Dağıt

Uygulamanızı Koşullu Erişim Uygulama Denetimi ile yapılandırmak [için, Azure AD uygulamaları için Koşullu Erişim Uygulama Denetimini Dağıt'taki](/cloud-app-security/proxy-deployment-aad)yönergeleri izleyin.


## <a name="test-conditional-access-app-control"></a>Test Koşullu Erişim Uygulama Kontrolü

Koşullu Erişim Uygulama Denetimi ile Azure AD uygulamalarının dağıtımını test etmek [için, Azure AD uygulamaları için dağıtımı test](/cloud-app-security/proxy-deployment-aad)etme yönergelerini izleyin.





