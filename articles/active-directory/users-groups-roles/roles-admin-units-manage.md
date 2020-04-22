---
title: Yönetim birimleri ekleme ve kaldırma (önizleme) - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure Etkin Dizini'nde rol izinlerinin kapsamını kısıtlamak için yönetim birimlerini kullanın.
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
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81684908"
---
# <a name="manage-administrative-units-in-azure-active-directory"></a>Azure Etkin Dizini'nde yönetim birimlerini yönetme

Azure Etkin Dizini'nde (Azure AD) daha ayrıntılı yönetim denetimi için, kullanıcıları bir veya daha fazla yönetim birimiyle (AUs) sınırlı bir kapsamda bir Azure REKLAM rolüne atayabilirsiniz.

## <a name="get-started"></a>başlarken

1. [Grafik Explorer](https://aka.ms/ge)üzerinden aşağıdaki yönergelerden sorguları çalıştırmak için aşağıdakileri yapın:

    a. Azure portalında Azure AD'ye gidin. Uygulamalar **listesinde, Grafik Gezgini'ni**seçin ve ardından **Graph Explorer'a Yönetici Onayı Ver'i**seçin.

    !["Yönetici onayı ver" bağlantısını gösteren ekran görüntüsü](./media/roles-admin-units-manage/select-graph-explorer.png)

    b. Grafik Gezgini'nde **beta** sürümünü seçin.

    ![Seçilen beta sürümünü gösteren ekran görüntüsü](./media/roles-admin-units-manage/select-beta-version.png)

1. Azure AD PowerShell'in önizleme sürümünü kullanın.

## <a name="add-an-administrative-unit"></a>İdari birim ekleme

### <a name="use-the-azure-portal"></a>Azure portalı kullanma

1. Azure portalında Azure AD'ye gidin ve ardından sol bölmede **Yönetim birimlerini**seçin.

    ![Azure AD'deki Yönetim birimleri (Önizleme) bağlantısının ekran görüntüsü](./media/roles-admin-units-manage/nav-to-admin-units.png)

1. **Ekle'yi** seçin ve ardından yönetim biriminin adını girin. İsteğe bağlı olarak, yönetim biriminin açıklamasını ekleyin.

    ![Ekle düğmesinin ekran görüntüsü ve yönetim biriminin adını girmek için metin kutusu](./media/roles-admin-units-manage/add-new-admin-unit.png)

1. Yönetim birimini sonuçlandırmak için **Ekle'yi** seçin.

### <a name="use-powershell"></a>PowerShell kullanma

Aşağıdaki komutları çalıştırmayı denemeden önce Azure AD PowerShell'i (önizleme) yükleyin:

    Connect-AzureAD
    New-AzureADAdministrativeUnit -Description "West Coast region" -DisplayName "West Coast"

Gerektiği gibi, tırnak işaretleriyle kapatılan değerleri değiştirebilirsiniz.

### <a name="use-microsoft-graph"></a>Microsoft Graph'ı kullanma

    Http Request
    POST /administrativeUnits
    Request body
    {
        "displayName": "North America Operations",
        "description": "North America Operations administration"
    }

## <a name="remove-an-administrative-unit"></a>İdari birimi kaldırma

Azure AD'de, yönetim rolleri için kapsam birimi olarak artık gereksinim duymadığınız bir yönetim birimini kaldırabilirsiniz.

### <a name="use-the-azure-portal"></a>Azure portalı kullanma

1. Azure portalında Azure **AD** > **Yönetim birimlerine**gidin. 
1. Silinecek yönetim birimini seçin ve sonra **Sil'i**seçin. 
1. Yönetim birimini silmek istediğinizi doğrulamak için **Evet'i**seçin. Yönetim birimi silinir.

![İdari birimin ekran görüntüsü Sil düğmesi ve onay penceresi](./media/roles-admin-units-manage/select-admin-unit-to-delete.png)

### <a name="use-powershell"></a>PowerShell kullanma

    $delau = Get-AzureADAdministrativeUnit -Filter "displayname eq 'DeleteMe Admin Unit'"
    Remove-AzureADAdministrativeUnit -ObjectId $delau.ObjectId

Belirli bir ortam için gerekli olduğu gibi, tırnak işaretleriyle kapatılan değerleri değiştirebilirsiniz.

### <a name="use-the-graph-api"></a>Grafik API'sini kullanma

    HTTP request
    DELETE /administrativeUnits/{Admin id}
    Request body
    {}

## <a name="next-steps"></a>Sonraki adımlar

* [Yönetim birimindeki kullanıcıları yönetme](roles-admin-units-add-manage-users.md)
* [Yönetim birimindeki grupları yönetme](roles-admin-units-add-manage-groups.md)
