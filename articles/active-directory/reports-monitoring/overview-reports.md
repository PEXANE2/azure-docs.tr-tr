---
title: Azure Active Directory raporları nelerdir? | Microsoft Belgeleri
description: Azure Active Directory raporlarının genel bakışını sağlar.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 6141a333-38db-478a-927e-526f1e7614f4
ms.service: active-directory
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: b655fa2f12eac894e97c61cc931983aa58fff7a9
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74007919"
---
# <a name="what-are-azure-active-directory-reports"></a>Azure Active Directory raporları nelerdir?

Azure Etkin Dizin (Azure AD) raporları, ortamınızdaki etkinliğin kapsamlı bir görünümünü sağlar. Sağlanan verilerle:

- Uygulama ve hizmetlerinizin kullanıcılarınız tarafından nasıl kullanıldığını saptayabilirsiniz
- Ortamınızın durumunu etkileyebilecek olası riskleri algılayabilirsiniz
- Kullanıcılarınızın işlerini yapmalarını engelleyen sorunları giderebilirsiniz  

Raporlama mimarisinin iki ana dayanağı vardır:

- [Güvenlik raporları](#security-reports)
- [Etkinlik raporları](#activity-reports)

![Raporlama](./media/overview-reports/01.png)


## <a name="security-reports"></a>Güvenlik raporları

Güvenlik raporları kuruluşunuzun kimliklerini korumanıza yardımcı olur. İki tür güvenlik raporu vardır:

- **Riskli oldukları belirlenen kullanıcılar** - [Riskli oldukları belirlenen kullanıcılar güvenlik raporundan](concept-user-at-risk.md), gizliliği bozulmuş olabilecek kullanıcı hesaplarına genel bir bakış elde edersiniz.

- **Riskli oturum açma işlemleri** - [Riskli oturum açma işlemleri güvenlik raporuyla](concept-risky-sign-ins.md), kullanıcı hesabının meşru sahibi olmayan biri tarafından gerçekleştirilmiş olabilecek oturum açma işlemleriyle ilgili göstergeler elde edersiniz. 

### <a name="what-azure-ad-license-do-you-need-to-access-a-security-report"></a>Güvenlik raporuna erişebilmek için hangi Azure AD lisansınızın olması gerekir?  

Azure AD'nin tüm sürümleri, risk ve riskli oturum açma raporları için işaretlenmiş kullanıcıları size sağlar. Bununla birlikte, rapordaki ayrıntı düzeyi sürümler arasında değişiklik gösterir: 

- Azure **Active Directory Free ve Basic sürümlerinde,** risk ve riskli oturum açma için işaretlenmiş kullanıcıların listesini alırsınız. 

- **Azure Active Directory Premium 1** sürümü, her rapor için algılanan temel risk algılamalarından bazılarını incelemenize olanak sağlayarak bu modeli genişletir. 

- **Azure Active Directory Premium 2** sürümü, temel risk algılamaları hakkında en ayrıntılı bilgileri sağlar ve ayrıca yapılandırılmış risk düzeylerine otomatik olarak yanıt veren güvenlik ilkelerini yapılandırmanızı sağlar.


## <a name="activity-reports"></a>Etkinlik raporları

Etkinlik raporları, kuruluşunuzdaki kullanıcıların davranışlarını anlamanıza yardımcı olur. Azure AD'de iki tür etkinlik raporu vardır:

- **Denetim günlükleri** - [Denetim günlükleri etkinlik raporu](concept-audit-logs.md), kiracınızda gerçekleştirilen her görevin geçmişine erişmenizi sağlar.

- **Oturum açma işlemleri** - [Oturum açma işlemleri etkinlik raporuyla](concept-sign-ins.md), denetim günlükleri raporunda bildirilen görevleri kimlerin gerçekleştirdiğini saptayabilirsiniz.


### <a name="audit-logs-report"></a>Denetim günlükleri raporu 

[Denetim günlükleri raporu](concept-audit-logs.md) uyumluluk amacıyla sistem etkinliklerinin kayıtlarını sağlar. Bu veriler, şu yaygın senaryolara çözüm getirmenize olanak sağlar:

- Kiracımda birisi yönetici grubuna erişim aldı. Ona kim erişim verdi? 

- Belirli bir uygulamayı yeni ekledim ve iyi çalışıp çalışmadığını bilmek istiyorum; bu nedenle uygulamada oturum açan kullanıcıların listesini öğrenmek istiyorum

- Kiracımda kaç parola sıfırlama işlemi yapıldığını bilmek istiyorum


#### <a name="what-azure-ad-license-do-you-need-to-access-the-audit-logs-report"></a>Denetim günlükleri raporuna erişebilmek için hangi Azure AD lisansınızın olması gerekir?  

Denetim günlükleri raporu, lisansınız olan özellikler için sağlanır. Belirli bir özelliğin lisansına sahipseniz, o özelliğin denetim günlüğü bilgilerine de erişebilirsiniz. Daha fazla ayrıntı için [Azure Active Directory özellikleri ve özelliklerine](https://www.microsoft.com/cloud-platform/azure-active-directory-features)bakın.   

### <a name="sign-ins-report"></a>Oturum açma raporu

[Oturum açma raporu,](concept-sign-ins.md) aşağıdaki gibi soruların yanıtlarını bulmanızı sağlar:

- Belirli bir kullanıcının oturum açma düzeni nedir?
- Bir hafta içerisinde kaç adet kullanıcı oturum açtı?
- Bu açılan oturumların durumu nedir?

#### <a name="what-azure-ad-license-do-you-need-to-access-the-sign-ins-activity-report"></a>Oturum açma işlemleri etkinlik raporuna erişebilmek için hangi Azure AD lisansınızın olması gerekir?  

Oturum açma işlemleri etkinlik raporuna erişebilmek için, kiracınızın ilişkili bir Azure AD Premium lisansı olması gerekir.

## <a name="programmatic-access"></a>Programlı erişim

Azure AD, kullanıcı arabirimine ek olarak, REST tabanlı API'ler kümesi aracılığıyla raporlar verilerine [programlı erişim](concept-reporting-api.md) sağlar. Çeşitli programlama dilleri ve araçlarından bu API'leri çağırabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

- [Riskli oturum açma işlemleri raporu](concept-risky-sign-ins.md)
- [Denetim günlükleri raporu](concept-audit-logs.md)
- [Oturum açma günlükleri raporu](concept-sign-ins.md)
