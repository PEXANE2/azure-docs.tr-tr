---
title: Azure AD'de Koşullu Erişim ile Azure yönetimine erişimi yönetme
description: Azure yönetimine erişimi yönetmek için Azure AD'de Koşullu Erişim'i kullanma hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: 0adc8b11-884e-476c-8c43-84f9bf12a34b
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2019
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: f3341f1c30a1581b8507652c322c00581e3972aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77137403"
---
# <a name="manage-access-to-azure-management-with-conditional-access"></a>Koşullu Erişim ile Azure yönetimine erişimi yönetme

> [!CAUTION]
> Azure yönetimine erişimi yönetmek için bir ilke ayarlamadan önce Koşullu Erişim'in nasıl çalıştığını anladığınızdan emin olun. Portala kendi erişiminizi engelleyebilecek koşullar oluşturmadığınızdan emin olun.

Azure Etkin Dizininde Koşullu Erişim (Azure AD), belirttiğiniz belirli koşullara bağlı olarak bulut uygulamalarına erişimi denetler. Erişime izin vermek için, ilkedeki gereksinimlerin karşılanıp karşılanmadığına bağlı olarak erişime izin veren veya engelleyen Koşullu Erişim ilkeleri oluşturursunuz. 

Genellikle, bulut uygulamalarınız için erişimi denetlemek için Koşullu Erişim'i kullanırsınız. Azure yönetimine erişimi belirli koşullara (oturum açma riski, konum veya aygıt gibi) göre denetlemek ve çok faktörlü kimlik doğrulama gibi gereksinimleri zorlamak için ilkeler de ayarlayabilirsiniz.

Azure yönetimi için bir ilke oluşturmak için, ilkeyi uygulayacağınız uygulamayı seçerken **Bulut uygulamaları** altında **Microsoft Azure Yönetimi'ni** seçersiniz.

![Azure yönetimi için Koşullu Erişim](./media/conditional-access-azure-management/conditional-access-azure-mgmt.png)

Oluşturduğunuz ilke, aşağıdakiler de dahil olmak üzere tüm Azure yönetim uç noktaları için geçerlidir:

- Azure portalında
- Azure Kaynak Yöneticisi sağlayıcısı
- Klasik Servis Yönetimi API'leri
- Azure PowerShell
- Visual Studio abonelikleri yönetici portalı
- Azure DevOps
- Azure Veri Fabrikası portalı

İlkenin Azure Kaynak Yöneticisi API'sini çağıran Azure PowerShell için geçerli olduğunu unutmayın. Microsoft Graph olarak adlandırdığı [Azure AD PowerShell](/powershell/azure/active-directory/install-adv2)için geçerli değildir.


Koşullu Erişim'in nasıl ayarlanıp kullanılacağı hakkında daha fazla bilgi için [Azure Etkin Dizini'nde Koşullu Erişim'e](../active-directory/active-directory-conditional-access-azure-portal.md)bakın.
