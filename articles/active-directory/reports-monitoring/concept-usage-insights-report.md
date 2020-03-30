---
title: Kullanım ve öngörüler raporu | Microsoft Dokümanlar
description: Azure Active Directory portalında kullanıma giriş ve öngörüler raporu
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 3fba300d-18fc-4355-9924-d8662f563a1f
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 05/13/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: b3db86137207ae726c7befc393f62590fd1456d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74008259"
---
# <a name="usage-and-insights-report-in-the-azure-active-directory-portal"></a>Azure Active Directory portalında kullanım ve öngörüler raporu

Kullanım ve öngörüler raporuyla, oturum açma verilerinizin uygulama merkezli bir görünümünü elde edebilirsiniz. Aşağıdaki soruların yanıtlarını bulabilirsiniz:

*   Kuruluşumda en çok kullanılan uygulamalar nelerdir?
*   En başarısız oturum açma uygulamaları hangileridir? 
*   Her uygulama için üst oturum açma hataları nelerdir?

## <a name="prerequisites"></a>Ön koşullar 

Kullanım ve öngörüler raporundaki verilere erişmek için şunları yapmanız gerekir:

* Azure AD kiracı
* Oturum açma verilerini görüntülemek için Azure AD premium (P1/P2) lisansı
* Genel yönetici, güvenlik yöneticisi, güvenlik okuyucu veya rapor okuyucu rollerindeki bir kullanıcı. Buna ek olarak, herhangi bir kullanıcı (yönetici olmayanlar) kendi oturum açma larına erişebilir. 

## <a name="access-the-usage-and-insights-report"></a>Kullanım ve öngörüler raporuna erişin

1. [Azure portalına](https://portal.azure.com) gidin.
2. Doğru dizini seçin, ardından **Azure Etkin Dizini'ni** seçin ve **Kurumsal uygulamaları**seçin.
3. **Etkinlik** bölümünden, raporu açmak için **Kullanım & öngörülerini** seçin. 

![Kullanım ve İçgörüler raporu](./media/concept-usage-insights-report/main-menu.png)
                                     

## <a name="use-the-report"></a>Raporu kullanma

Kullanım ve öngörüler raporu, bir veya daha fazla oturum açma girişiminde bulunan uygulamaların listesini gösterir ve başarılı oturum açma sayısına, başarısız oturum açma sayısına ve başarı oranına göre sıralamayapmanızı sağlar.

Listenin altındaki daha fazla yükle'yi tıklattığınızda, sayfadaki ek uygulamaları görüntülemenize olanak tanır. Aralık içinde kullanılan tüm uygulamaları görüntülemek için tarih aralığını seçebilirsiniz.

Ayrıca belirli bir uygulamaya odaklanabilirsiniz. Uygulama için zaman içinde oturum açma etkinliğini ve üst teki hataları görmek için **oturum açma etkinliğini** seçin.  

Uygulama kullanım grafiğinde bir gün seçtiğinizde, uygulama için oturum açma etkinliklerinin ayrıntılı bir listesini alırsınız.  

![Kullanım ve İçgörüler raporu](./media/concept-usage-insights-report/usage-and-insights-report.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Oturum açma raporu](concept-sign-ins.md)