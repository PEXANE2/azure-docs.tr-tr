---
title: PıM-Azure Active Directory 'de Azure AD rolleri için istekleri onaylama veya reddetme | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içinde Azure AD rolleri için istekleri onaylamayı veya reddetmeyi öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 60a8d373a7e6edeaefd933e4f8ec8ee11e3c14ee
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804031"
---
# <a name="approve-or-deny-requests-for-azure-ad-roles-in-pim"></a>PıM 'de Azure AD rolleri için istekleri onaylama veya reddetme

Azure Active Directory (Azure AD) Privileged Identity Management (PıM) sayesinde, rolleri etkinleştirme için onay gerektirecek şekilde yapılandırabilir ve bir ya da birden çok kullanıcı ya da grubu, temsilci onaylayan olarak seçebilirsiniz. Temsilci onaylamalardan isteklerin onaylaması 24 saati vardır. İstek 24 saat içinde onaylanmamışsa, uygun Kullanıcı yeni bir isteği yeniden göndermesi gerekir. 24 saat onay zaman penceresi yapılandırılamaz.

Azure AD rolleri için istekleri onaylamak veya reddetmek için bu makaledeki adımları izleyin.

## <a name="view-pending-requests"></a>Bekleyen istekleri görüntüle

Bir temsilci onaylayıcı olarak, Onayınızı bekleyen bir Azure AD rol isteği olduğunda bir e-posta bildirimi alırsınız. Bu bekleyen istekleri PıM ' de görebilirsiniz.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure AD rolleri**' ne tıklayın.

1. **Istekleri Onayla**' ya tıklayın.

    ![Azure AD rolleri-istekleri onaylama](./media/azure-ad-pim-approval-workflow/pim-directory-roles-approve-requests.png)

    Onayınızı bekleyen isteklerin bir listesini görürsünüz.

## <a name="approve-requests"></a>İstekleri onayla

1. Onaylamak istediğiniz istekleri seçin ve ardından **Onayla** ' ya tıklayarak seçili istekleri Onayla bölmesini açın.

    ![Onaylama seçeneği vurgulanmış şekilde istekleri Onayla listesi](./media/azure-ad-pim-approval-workflow/pim-approve-requests-list.png)

1. **Onay nedeni** kutusuna bir neden yazın.

    ![Seçili istekler bölmesini onay nedeni ile Onayla](./media/azure-ad-pim-approval-workflow/pim-approve-selected-requests.png)

1. **Onayla**' ya tıklayın.

    Durum simgesi onayınız ile güncelleştirilir.

    ![Onay düğmesine tıkladıktan sonra seçili istekleri Onayla bölmesi](./media/azure-ad-pim-approval-workflow/pim-approve-status.png)

## <a name="deny-requests"></a>Reddetme istekleri

1. Reddetmek istediğiniz istekleri seçin ve sonra seçili istekleri Reddet bölmesini açmak için **Reddet** ' e tıklayın.

    ![Reddetme seçeneği vurgulanmış şekilde istek listesini Onayla](./media/azure-ad-pim-approval-workflow/pim-deny-requests-list.png)

1. **Reddetme nedeni** kutusuna bir neden yazın.

    ![Seçili istekler bölmesini reddetme nedeni ile reddetme](./media/azure-ad-pim-approval-workflow/pim-deny-selected-requests.png)

1. **Reddet**' e tıklayın.

    Durum simgesi, geri çevirme ile güncelleştirilir.

## <a name="next-steps"></a>Sonraki adımlar

- [PıM 'de e-posta bildirimleri](pim-email-notifications.md)
- [PıM 'de Azure Kaynak rolleri için istekleri onaylama veya reddetme](pim-resource-roles-approval-workflow.md)
