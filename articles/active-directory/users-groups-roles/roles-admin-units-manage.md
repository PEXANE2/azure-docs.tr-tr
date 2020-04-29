---
title: Yönetim birimleri ekleme ve kaldırma (Önizleme)-Azure Active Directory | Microsoft Docs
description: Azure Active Directory içindeki rol izinlerinin kapsamını kısıtlamak için yönetim birimlerini kullanın.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: article
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 04/16/2020
ms.author: curtand
ms.reviewer: anandy
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7a4a0dfaeda18b3f68ddc3c7cc7333b8c994d174
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81684908"
---
# <a name="manage-administrative-units-in-azure-active-directory"></a>Azure Active Directory Yönetim birimlerini yönetme

Azure Active Directory (Azure AD) içinde daha ayrıntılı yönetim denetimi için, bir veya daha fazla yönetim birimiyle (AU) sınırlı bir kapsama sahip bir Azure AD rolüne kullanıcı atayabilirsiniz.

## <a name="get-started"></a>başlarken

1. [Grafik Gezgini](https://aka.ms/ge)aracılığıyla aşağıdaki yönergelerden sorguları çalıştırmak için aşağıdakileri yapın:

    a. Azure portal Azure AD 'ye gidin. Uygulamalar listesinde, **Graph Explorer**' ı seçin ve ardından **Graph Explorer 'A yönetici izni ver**' i seçin.

    !["Yönetici onayı verme" bağlantısını gösteren ekran görüntüsü](./media/roles-admin-units-manage/select-graph-explorer.png)

    b. Graph Explorer 'da **Beta** sürümünü seçin.

    ![Beta sürümünün seçili olduğunu gösteren ekran görüntüsü](./media/roles-admin-units-manage/select-beta-version.png)

1. Azure AD PowerShell 'in önizleme sürümünü kullanın.

## <a name="add-an-administrative-unit"></a>Yönetim birimi ekleme

### <a name="use-the-azure-portal"></a>Azure portalı kullanma

1. Azure portal Azure AD ' a gidin ve ardından sol bölmedeki **yönetim birimleri**' ni seçin.

    ![Azure AD 'de yönetim birimleri (Önizleme) bağlantısının ekran görüntüsü](./media/roles-admin-units-manage/nav-to-admin-units.png)

1. **Ekle** ' yi seçin ve ardından yönetim biriminin adını girin. İsteğe bağlı olarak, yönetim biriminin bir açıklamasını ekleyin.

    ![Yönetim biriminin adını girmek için Ekle düğmesinin ve metin kutusunun ekran görüntüsü](./media/roles-admin-units-manage/add-new-admin-unit.png)

1. Yönetim birimini sonlandırmak için **Ekle** ' yi seçin.

### <a name="use-powershell"></a>PowerShell kullanma

Aşağıdaki komutları çalıştırmayı denemeden önce Azure AD PowerShell 'i (Önizleme) yükleyebilirsiniz:

    Connect-AzureAD
    New-AzureADAdministrativeUnit -Description "West Coast region" -DisplayName "West Coast"

Gerektiğinde, tırnak işaretleri içine alınmış değerleri değiştirebilirsiniz.

### <a name="use-microsoft-graph"></a>Microsoft Graph kullan

    Http Request
    POST /administrativeUnits
    Request body
    {
        "displayName": "North America Operations",
        "description": "North America Operations administration"
    }

## <a name="remove-an-administrative-unit"></a>Yönetim birimini kaldırma

Azure AD 'de, artık yönetici rolleri için bir kapsam birimi olarak ihtiyacınız olmayan bir yönetim birimini kaldırabilirsiniz.

### <a name="use-the-azure-portal"></a>Azure portalı kullanma

1. Azure Portal **Azure AD** > **yönetim birimleri**' ne gidin. 
1. Silinecek yönetim birimini seçin ve **Sil**' i seçin. 
1. Yönetim birimini silmek istediğinizi onaylamak için **Evet**' i seçin. Yönetim birimi silinir.

![Yönetim birimi silme düğmesi ve onay penceresinin ekran görüntüsü](./media/roles-admin-units-manage/select-admin-unit-to-delete.png)

### <a name="use-powershell"></a>PowerShell kullanma

    $delau = Get-AzureADAdministrativeUnit -Filter "displayname eq 'DeleteMe Admin Unit'"
    Remove-AzureADAdministrativeUnit -ObjectId $delau.ObjectId

Belirli bir ortam için gereken şekilde, tırnak işaretleri içine alınmış değerleri değiştirebilirsiniz.

### <a name="use-the-graph-api"></a>Graph API kullanın

    HTTP request
    DELETE /administrativeUnits/{Admin id}
    Request body
    {}

## <a name="next-steps"></a>Sonraki adımlar

* [Yönetici birimindeki kullanıcıları yönetme](roles-admin-units-add-manage-users.md)
* [Yönetim birimindeki grupları yönetme](roles-admin-units-add-manage-groups.md)
