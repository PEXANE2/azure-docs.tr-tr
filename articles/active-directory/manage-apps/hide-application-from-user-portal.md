---
title: Azure AD 'de kullanıcının deneyiminden bir uygulamayı gizleme
description: Azure Active Directory erişim panellerinde veya Office 365 launchers 'da bir uygulamayı kullanıcının deneyiminden gizleme.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: mimart
ms.reviewer: kasimpso
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1e01c79c5cc9391922333af4e9a60ba44a6a6b13
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74273996"
---
# <a name="hide-applications-from-end-users-in-azure-active-directory"></a>Azure Active Directory ' deki son kullanıcılardan uygulamaları gizleyin

Son kullanıcıların Uygulamaps panelinden veya Office 365 başlatıcısı 'ndan uygulama gizleme yönergeleri. Bir uygulama gizli olduğunda, kullanıcıların uygulama izinleri hala vardır. 

## <a name="prerequisites"></a>Önkoşullar

Uygulama Yöneticisi ayrıcalıkları, uygulamayı Uygps panelinden ve Office 365 başlatıcısı 'ndan gizlemek için gereklidir.

Tüm Office 365 uygulamalarını gizlemek için genel yönetici ayrıcalıkları gerekir.


## <a name="hide-an-application-from-the-end-user"></a>Bir uygulamayı son kullanıcıdan gizleme
Uygulamaps panelinden ve Office 365 uygulama başlatıcısı 'ndan bir uygulamayı gizlemek için aşağıdaki adımları kullanın.

1.  [Azure Portal](https://portal.azure.com) , dizininiz için genel yönetici olarak oturum açın.
2.  **Azure Active Directory**'yi seçin.
3.  **Kurumsal uygulamalar**' ı seçin. **Kurumsal uygulamalar-tüm uygulamalar** dikey penceresi açılır.
4.  **Uygulama türü**' nün altında, henüz seçili değilse **Kurumsal uygulamalar**' ı seçin.
5.  Gizlemek istediğiniz uygulamayı arayın ve uygulamayı tıklatın.  Uygulamanın genel görünümü açılır.
6.  **Özellikler**'e tıklayın. 
7.  **Kullanıcılar Için görünebilir mi?** sorusu için **Hayır**'a tıklayın.
8.  **Kaydet**’e tıklayın.


## <a name="hide-office-365-applications-from-the-myapps-panel"></a>Uygulamaps panelinden Office 365 uygulamalarını gizleme

Uygulamaps panelinden tüm Office 365 uygulamalarını gizlemek için aşağıdaki adımları kullanın. Uygulamalar hala Office 365 portalında görünür.

1.  [Azure Portal](https://portal.azure.com) , dizininiz için genel yönetici olarak oturum açın.
2.  **Azure Active Directory**'yi seçin.
3.  **Kullanıcı ayarları**' nı seçin.
4.  **Kurumsal uygulamalar**altında **son kullanıcıların uygulamalarını nasıl başlatıp görüntüleyebileceğini Yönet** ' e tıklayın.
5.  **Kullanıcılar yalnızca office 365 portalında office 365 uygulamalarını görebilir**, **Evet**' e tıklayın.
6.  **Kaydet**’e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar
* [Tüm gruplarımı gör](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Kurumsal uygulamaya Kullanıcı veya Grup atama](assign-user-or-group-access-portal.md)
* [Bir kurumsal uygulamadan Kullanıcı veya grup atamasını kaldırma](remove-user-or-group-access-portal.md)
* [Kurumsal uygulamanın adını veya logosunu değiştirme](change-name-or-logo-portal.md)

