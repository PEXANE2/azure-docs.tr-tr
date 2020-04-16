---
title: Yönetim birimleri ekleme ve kaldırma (önizleme) - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure Etkin Dizini'nde rol izinlerinin kapsamını kısıtlamak için yönetim birimlerini kullanma
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
ms.openlocfilehash: 351cbee9cbc7cc02c7f20553b6fc8c3f861541df
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81428166"
---
# <a name="manage-administrative-units-in-azure-active-directory"></a>Azure Etkin Dizini'nde yönetim birimlerini yönetme

Azure Etkin Dizini'nde (Azure AD) daha ayrıntılı yönetim denetimi için, kullanıcıları bir veya daha fazla yönetim birimiyle (AUs) sınırlı bir kapsamla Azure AD rolüne atayabilirsiniz.

## <a name="getting-started"></a>Başlarken

1. [Grafik Explorer](https://aka.ms/ge)aracılığıyla aşağıdaki talimatlardan sorguları çalıştırmak için lütfen aşağıdakileri sağlayın:

    1. Portaldaki Azure AD'ye gidin ve uygulamalarda Grafik Gezgini'ni seçin ve Grafik Gezgini'ne yönetici onayı sağlayın.

        ![Grafik Explorer'ı seçin ve bu sayfada yönetici onayı sağlayın](./media/roles-admin-units-manage/select-graph-explorer.png)

    1. Grafik Gezgini'nde, beta sürümünü seçtiğinizden emin olun.

        ![POST işleminden önce beta sürümünü seçin](./media/roles-admin-units-manage/select-beta-version.png)

1. Lütfen Azure AD PowerShell'in önizleme sürümünü kullanın. Ayrıntılı talimatlar burada.

## <a name="add-an-administrative-unit"></a>İdari birim ekleme

### <a name="azure-portal"></a>Azure portal

1. Portaldaki Active Directory'ye gidin ve sol panelde İdari Birimleri seçin.

    ![Azure Etkin Dizini'ndeki Yönetim birimlerine gidin](./media/roles-admin-units-manage/nav-to-admin-units.png)

1. **Ekle*** seçeneğini belirleyin ve yönetim biriminin adını sağlayın ve isteğe bağlı olarak yönetim birimi için bir açıklama ekleyebilirsiniz.

    ![ekle'yi seçin ve ardından yönetim birimi için bir ad girin](./media/roles-admin-units-manage/add-new-admin-unit.png)

1. Yönetim birimini sonuçlandırmak için **Ekle'yi** seçin.

### <a name="powershell"></a>PowerShell

Aşağıdaki eylemleri gerçekleştirmeye çalışmadan önce Azure AD PowerShell'i (önizleme sürümü) yükleyin:

    Connect-AzureAD
    New-AzureADAdministrativeUnit -Description "West Coast region" -DisplayName "West Coast"

Yukarıda vurgulanan değerler gerektiği gibi değiştirilebilir.

### <a name="microsoft-graph"></a>Microsoft Graph

    Http Request
    POST /administrativeUnits
    Request body
    {
        "displayName": "North America Operations",
        "description": "North America Operations administration"
    }

## <a name="remove-an-administrative-unit"></a>İdari birimi kaldırma

Azure Etkin Dizini'nde (Azure AD), yönetim rolleri için kapsam birimi olarak artık gereksinim duymadığınız bir yönetici birimini kaldırabilirsiniz.

### <a name="azure-portal"></a>Azure portal

Portaldaki **Azure AD > Yönetim birimlerine** gidin. Silinecek yönetim birimini seçin ve sonra **Sil'i**seçin. **Evet**onaylandıktan sonra, yönetim birimi silinir.

![Silmek için bir yönetim birimi seçin](./media/roles-admin-units-manage/select-admin-unit-to-delete.png)

### <a name="powershell"></a>PowerShell

    $delau = Get-AzureADAdministrativeUnit -Filter "displayname eq 'DeleteMe Admin Unit'"
    Remove-AzureADAdministrativeUnit -ObjectId $delau.ObjectId

Vurgulanan bölüm, belirli bir ortam için gerektiği gibi değiştirilebilir.

### <a name="graph-api"></a>Graph API

    HTTP request
    DELETE /administrativeUnits/{Admin id}
    Request body
    {}

## <a name="next-steps"></a>Sonraki adımlar

[Yönetim birimindeki](roles-admin-units-add-manage-users.md)
kullanıcıları[yönetme İdari birimdeki grupları yönetme](roles-admin-units-add-manage-groups.md)
