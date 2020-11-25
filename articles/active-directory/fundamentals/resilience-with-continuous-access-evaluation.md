---
title: Azure Active Directory içinde sürekli erişim değerlendirmesi kullanarak esnekliği oluşturun
description: CAE kullanarak mimarlar ve BT yöneticileri için kılavuz
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 11/30/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ad36c2a7f47948d9362b85e78355e6046cda703
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95919866"
---
# <a name="build-resilience-by-using-continuous-access-evaluation"></a>Sürekli erişim değerlendirmesi kullanarak esnekliği derleme

[Sürekli erişim değerlendirmesi](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-continuous-access-evaluation) (CAE), Azure AD uygulamalarının, daha sonra değerlendirilebilecek ve zorlanacak kritik olaylara abone olmasına olanak sağlar. Buna aşağıdaki olayların değerlendirmesi dahildir:

* Silinmekte veya devre dışı bırakılmakta olan kullanıcı hesabı

* Kullanıcı parolası değiştirildi

* MFA, Kullanıcı için etkinleştirilmiştir.

* Yönetici bir belirteci açıkça iptal eder.

* Yükseltilmiş Kullanıcı riski algılanır.

Sonuç olarak, uygulamalar, aşağıdaki diyagramda gösterildiği gibi Azure AD tarafından işaret edilen olaylara dayalı olarak, süre dolunsız belirteçleri reddedebilir.

![Kae conceptualiagram](./media/resilience-with-cae/admin-resilience-continuous-access-evaluation.png)

## <a name="how-does-cae-help"></a>CAE nasıl yardımcı olur?

Bu mekanizma, Azure AD 'nin daha uzun süreli belirteçler vermesine olanak sağlarken, uygulamaları erişimi iptal etmek ve gerektiğinde yeniden kimlik doğrulamaya zorlamak için bir yol sağlar. Bu düzenin net sonucu, belirteçlerin elde edilmesine yönelik daha az çağrıdır. Bu, uçtan uca akışın daha dayanıklı olduğu anlamına gelir. 

CAE 'yi kullanmak için hem hizmet hem de istemci CAE özellikli olmalıdır. Exchange Online, takımlar ve SharePoint Online destek kae gibi Microsoft 365 hizmetleri. İstemci tarafında, bu Office 365 hizmetlerini (örn. Outlook Web App) ve Office 365 yerel istemcilerinin belirli sürümlerini kullanan tarayıcı tabanlı deneyimler CAE özellikli bir uygulamadır. Daha fazla Microsoft bulut hizmeti, CAE yeteneğine sahip olacak.

Microsoft, üçüncü taraf uygulamaların bu özelliği kullanmasına imkan tanıyan [standartlar](https://openid.net/wg/sse/) oluşturmak için sektörle çalışmaktadır. Ayrıca, CAE özellikli uygulamalar da geliştirebilirsiniz. Daha fazla bilgi için bkz. uygulamanızda esnekliği oluşturma.

## <a name="how-do-i-implement-cae"></a>CAE uygulama Nasıl yaparım??

* Azure AD güvenlik yapılandırması 'nda [CAE 'Yi etkinleştirin](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-continuous-access-evaluation) .

* Kuruluşunuzun Microsoft Office yerel uygulamaların [uyumlu sürümlerini](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-continuous-access-evaluation) kullandığından emin olun.

* Yeniden [kimlik doğrulama istemlerinizi iyileştirin](https://docs.microsoft.com/azure/active-directory/authentication/concepts-azure-multi-factor-authentication-prompts-session-lifetime).

 
## <a name="next-steps"></a>Sonraki adımlar
Yöneticiler ve Mimarlar için esnekliği kaynakları
 
* [Kimlik bilgisi yönetimiyle derleme esnekliği](resilience-in-credentials.md)

* [Cihaz durumlarıyla esnekliği derleme](resilience-with-device-states.md)

* [Dış Kullanıcı kimlik doğrulamasında esnekliği derleme](resilience-b2b-authentication.md)

* [Karma kimlik doğrulamalarınız içinde esnekliği oluşturma](resilience-in-hybrid.md)

* [Uygulama proxy 'Si ile uygulama erişimi içinde derleme esnekliği](resilience-on-premises-access.md)

Geliştiriciler için esnekliği kaynakları

* [Uygulamalarınızda ıAM esnekliği oluşturma](resilience-app-development-overview.md)

* [CıHAR sistemlerinizde derleme esnekliği](resilience-b2c.md)
