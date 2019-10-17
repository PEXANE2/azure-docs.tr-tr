---
title: Azure AD Yetkilendirme Yönetimi 'nde (Önizleme) bir erişim paketi için istekleri görüntüleme ve yönetme-Azure Active Directory
description: Azure Active Directory yetkilendirme yönetimi 'nde (Önizleme) bir erişim paketi için istekleri görüntülemeyi, yeniden işlemeyi ve iptal etmeyi öğrenin.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/15/2019
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: f7211fea4781904b9d97428ecf00b57970e4b57d
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72430287"
---
# <a name="view-and-manage-requests-for-an-access-package-in-azure-ad-entitlement-management-preview"></a>Azure AD Yetkilendirme Yönetimi 'nde erişim paketi isteklerini görüntüleme ve yönetme (Önizleme)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) yetkilendirme yönetimi şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure AD Yetkilendirme Yönetimi 'nde kimin erişim paketleri, ilkeleri ve durumu olduğunu görebilirsiniz. Bu makalede, bir erişim paketleri için istekleri görüntüleme, yeniden işleme ve iptal etme işlemlerinin nasıl yapılacağı açıklanır.

## <a name="view-requests"></a>İstekleri görüntüle

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi, Katalog sahibi veya erişim paketi Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri** ' ne tıklayın ve ardından erişim paketini açın.

1. **İstekler**' e tıklayın.

1. Ek ayrıntıları görmek için belirli bir isteğe tıklayın.

    ![Erişim paketi isteklerinin listesi](./media/entitlement-management-access-package-requests/requests-list.png)

## <a name="view-a-requests-delivery-errors"></a>Bir isteğin teslim hatalarını görüntüleme

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi, Katalog sahibi veya erişim paketi Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri** ' ne tıklayın ve ardından erişim paketini açın.

1. **İstekler**' e tıklayın.

1. Görüntülemek istediğiniz isteği seçin.

    İstekte herhangi bir teslim hatası varsa, istek durumu **teslim** edilmemiş veya **kısmen teslim**edilir.

    Herhangi bir teslim hatası varsa, isteğin ayrıntı bölmesinde teslim hatalarının sayısı olacaktır.

1. Tüm isteğin teslim hatalarını görmek için sayıma tıklayın.

## <a name="reprocess-a-request"></a>İsteği yeniden işleme

Bir istek bir hatayla karşılaşırsa, yeniden denemek için isteği yeniden işleyebilirsiniz. Yalnızca **teslim durumu başarısız** veya **kısmen teslim** edildi olan bir isteği ve bir haftadan daha az tamamlanma tarihini yeniden işleyebilirsiniz.

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi, Katalog sahibi veya erişim paketi Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri** ' ne tıklayın ve ardından erişim paketini açın.

1. **İstekler**' e tıklayın.

1. Yeniden işlemek istediğiniz isteğe tıklayın.

1. İstek ayrıntıları bölmesinde, **Isteği yeniden işle**' ye tıklayın.

    ![Başarısız bir isteği yeniden işleme](./media/entitlement-management-access-package-requests/reprocess-request.png)

## <a name="cancel-a-pending-request"></a>Bekleyen bir isteği iptal etme

Yalnızca teslim edilmemiş veya teslimi başarısız olan bekleyen bir isteği iptal edebilirsiniz.

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi, Katalog sahibi veya erişim paketi Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri** ' ne tıklayın ve ardından erişim paketini açın.

1. **İstekler**' e tıklayın.

1. İptal etmek istediğiniz isteğe tıklayın.

1. İstek ayrıntıları bölmesinde **Isteği Iptal et**' e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim paketi için değişiklik isteği ve onay ayarları](entitlement-management-access-package-request-policy.md)
- [Erişim paketi için atamaları görüntüleme, ekleme ve kaldırma](entitlement-management-access-package-assignments.md)