---
title: Yönetim birimleri yönetimi (önizleme) - Azure AD | Microsoft Dokümanlar
description: Azure Etkin Dizini'nde daha ayrıntılı izin ler delegasyonu için yönetim birimlerini kullanma
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: article
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 11/13/2019
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3c4958975a080a98fb900bf317229ba9eda795d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74028421"
---
# <a name="administrative-units-management-in-azure-active-directory-preview"></a>Azure Active Directory'de yönetim birimleri yönetimi (önizleme)

Bu makalede, Azure Etkin Dizin (Azure AD) yönetim birimleri açıklanmaktadır. Yönetim birimi, diğer Azure REKLAM kaynakları için kapsayıcı olabilecek bir Azure REKLAM kaynağıdır. Bu önizleme sürümünde, bu kaynaklar yalnızca kullanıcılar olabilir. Örneğin, bir yönetim birimi kapsamındaki Kullanıcı hesabı yöneticisi profil bilgilerini güncelleyebilir, parolaları sıfırlayabilir ve yalnızca yönetim birimindeki kullanıcılar için lisansatabilir.

Yönetim izinlerini alt kullanıcı alt kümeleri üzerinde devretmek ve ilkeleri bir kullanıcı alt kümesine uygulamak için yönetim birimlerini kullanabilirsiniz. İzinleri bölge yöneticilerine devretmek veya ilkeyi parçalı düzeyde ayarlamak için yönetim birimlerini kullanabilirsiniz.

## <a name="deployment-scenario"></a>Dağıtım senaryosu

İdari birimler bağımsız bölümlere sahip kuruluşlarda yararlı olabilir. Birçok özerk okuldan (School of Business, School of Engineering vb.) oluşan büyük bir üniversite örneğini göz önünde bulundurun, her birinin kendi BT yöneticilerinin erişimi kontrol eden, kullanıcıları yöneten ve okulları için politikalar belirleyen kendi BT yöneticileri vardır. Merkezi bir yönetici İşletme Fakültesi için bir idari birim oluşturabilir ve onu yalnızca işletme fakültesi öğrencileri ve personeliyle doldurabilir. Daha sonra merkezi yönetici, İşletme fakültesi BT personelini işletme okulu yönetim birimindeki yalnızca Azure AD kullanıcıları üzerinde yönetim izinleri veren kapsamlı bir role ekleyebilir.

## <a name="license-requirements"></a>Lisans gereksinimleri

Yönetim birimlerini kullanmak için her yönetim birimi yöneticisi için bir Azure Active Directory Premium lisansı gerektirir. Daha fazla bilgi için Azure [AD Premium ile başlarken](../fundamentals/active-directory-get-started-premium.md)bakın.

## <a name="managing-administrative-units"></a>İdari birimlerin yönetimi

Bu önizleme sürümünde, yönetim birimleri oluşturmanın ve yönetmenin tek yolu, [Yönetim Birimleriyle Çalışma'da](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0) açıklandığı gibi Windows PowerShell cmdlets için Azure Etkin Dizin Modülü'ni kullanmaktır.

Yazılım gereksinimleri ve Azure AD modüllerini yükleme hakkında daha fazla bilgi ve sözdizimi, parametre açıklamaları ve örnekler de dahil olmak üzere yönetim birimlerini yönetmek için Azure AD Modülü cmdlets'i [hakkında](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0)referans bilgileri için bkz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Active Directory sürümleri](../fundamentals/active-directory-whatis.md)
