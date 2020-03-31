---
title: Oturum açma hataları raporları nasıl giderilir | Microsoft Dokümanlar
description: Azure portalındaki Azure Active Directory raporlarını kullanarak oturum açma hatalarını nasıl gidereceklerini öğrenin
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: ec5fe7f62e8537a7f687202d365eb37d43b48b78
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74008052"
---
# <a name="how-to-troubleshoot-sign-in-errors-using-azure-active-directory-reports"></a>Nasıl yapilir: Azure Active Directory raporlarını kullanarak oturum açma hatalarını sorun giderme

Azure Etkin Dizini'ndeki (Azure AD) [oturum açma raporu,](concept-sign-ins.md) aşağıdakiler de dahil olmak üzere kuruluşunuzdaki uygulamalara erişimi yönetme yle ilgili soruların yanıtlarını bulmanıza olanak tanır:

- Belirli bir kullanıcının oturum açma düzeni nedir?
- Bir hafta içerisinde kaç adet kullanıcı oturum açtı?
- Bu açılan oturumların durumu nedir?


Ayrıca, oturum açma raporu, kuruluşunuzdaki kullanıcılar için oturum açma hatalarını gidermenize de yardımcı olabilir. Bu kılavuzda, oturum açma raporunda oturum açma hatasını nasıl yalıtabileceğinizi ve hatanın temel nedenini anlamak için nasıl kullanacağınızı öğrenirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Gerekenler:

* Premium (P1/P2) lisansına sahip bir Azure AD kiracısı. Azure Active Directory baskınızı yükseltmek için [Azure Active Directory Premium ile başlarken](../fundamentals/active-directory-get-started-premium.md) bakın.
* **Genel yönetici,** **güvenlik yöneticisi,** **güvenlik okuyucu**veya kiracı için **rapor okuyucu** rolü olan bir kullanıcı. Buna ek olarak, herhangi bir kullanıcı kendi oturum açma larına erişebilir. 

## <a name="troubleshoot-sign-in-errors-using-the-sign-ins-report"></a>Oturum açma raporunu kullanarak oturum açma hatalarını giderme

1. [Azure portalına](https://portal.azure.com) gidin ve diziniseçin.
2. **Azure Etkin Dizini'ni** seçin ve **İzleme** bölümünden **Oturum Açma'yı** seçin. 
3. Kullanıcı adı veya nesne tanımlayıcısı, uygulama adı veya tarih tarafından, hata daraltmak için sağlanan filtreleri kullanın. Ayrıca, yalnızca başarısız oturum açma ları görüntülemek için **Durum** açılır tarafından **Hata'yı** seçin. 

    ![Sonuçları filtreleme](./media/howto-troubleshoot-sign-in-errors/filters.png)
        
4. Araştırmak istediğiniz başarısız oturum açma'yı tanımlayın. Başarısız oturum açma hakkında daha fazla bilgi içeren ek ayrıntılar penceresini açmak için seçin. **Oturum açma hata kodunu** ve Hata **nedenini**not edin. 

    ![Kaydı seçin](./media/howto-troubleshoot-sign-in-errors/sign-in-failures.png)
        
5. Bu bilgileri, **Ayrıntı Giderme ve destek** sekmesinde ayrıntılar penceresinde de bulabilirsiniz.

    ![Sorun giderme ve destek](./media/howto-troubleshoot-sign-in-errors/troubleshooting-and-support.png)

6. Hata nedeni hatayı açıklar. Örneğin, yukarıdaki senaryoda, hata nedeni **Geçersiz kullanıcı adı veya parola veya Geçersiz şirket içi kullanıcı adı veya paroladır.** Düzeltme sadece doğru kullanıcı adı ve parola ile yeniden oturum açma etmektir.

7. Bu örnekte **50126** olan hata kodunu arayarak, düzeltme fikirleri de dahil olmak [sign-ins error codes reference](reference-sign-ins-error-codes.md)üzere ek bilgiler alabilirsiniz. 

8. Her şey başarısız olursa veya önerilen eylem etabına rağmen sorun devam ederse, **Sorun Giderme ve destek** sekmesindeki adımları izleyerek bir destek bileti [açın.](../fundamentals/active-directory-troubleshooting-support-howto.md) 

## <a name="next-steps"></a>Sonraki adımlar

* [Oturum açma hata kodları başvurusu](reference-sign-ins-error-codes.md)
* [Oturum açma raporuna genel bakış](concept-sign-ins.md)
