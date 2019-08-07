---
title: Yönetim birimleri yönetimi (Önizleme)-Azure Active Directory | Microsoft Docs
description: Azure Active Directory izinlerin daha ayrıntılı olarak temsili için yönetim birimleri kullanma
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 08/01/2019
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7b4bdced50f806367a53881d5ef0abd0a3710496
ms.sourcegitcommit: c662440cf854139b72c998f854a0b9adcd7158bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68736779"
---
# <a name="administrative-units-management-in-azure-active-directory-public-preview"></a>Azure Active Directory 'de yönetim birimleri yönetimi (Genel Önizleme)

Bu makalede, kullanıcıların alt kümeleri üzerinde yönetici izinleri atamak ve kullanıcıların bir alt kümesine ilke uygulamak için kullanılabilecek bir kaynak kapsayıcısı olan Azure Active Directory (Azure AD) içindeki yönetim birimleri açıklanmaktadır. Azure AD 'de yönetim birimleri, yönetim yöneticilerinin izinleri bölgesel yöneticilere devredebilir veya ayrıntılı bir düzeyde ilke ayarlamaya olanak tanır.

Bu, bağımsız bölümler içeren kuruluşlarda (örneğin, birbirinden bağımsız çok sayıda otonom okuldan (Iş okul, Mühendislik Okulu vb.) oluşan büyük bir üniversiteniz için yararlıdır. Bu tür bölümler, erişimi denetleyen, kullanıcıları yöneten ve ilkeleri özellikle kendi bölümleri için kuran kendi BT yöneticilerine sahiptir. Merkezi Yöneticiler, belirli bölümlerinin kullanıcıları üzerinde bu yönetim izinlerini sağlamak istiyor. Daha belirgin bir şekilde, bu örneği kullanarak bir merkezi yönetici, örneğin belirli bir okul (Iş Okulu) için bir yönetim birimi oluşturabilir ve yalnızca Iş okul kullanıcılarıyla doldurabilir. Daha sonra bir merkezi yönetici, Iş Okulu BT personelini kapsamlı bir role ekleyebilir, diğer bir deyişle iş okulunun yönetim izinlerinin BT personeline yalnızca Iş okul yönetim birimi üzerinden izin verebilir.

> [!IMPORTANT]
> Yönetim birimlerini kullanmak için yönetim birimi yöneticisinin bir Azure Active Directory Premium lisansına sahip olması gerekir. Daha fazla bilgi için bkz. [Azure AD Premium kullanmaya](../fundamentals/active-directory-get-started-premium.md)başlama.
>

Merkezi yöneticinin görünüm noktasından, yönetim birimi, kaynaklarla oluşturulup doldurulabilen bir dizin nesnesidir. **Bu önizleme sürümünde, bu kaynaklar yalnızca Kullanıcı olabilir.** Oluşturulduktan ve doldurulduktan sonra, yönetim birimi yalnızca yönetim biriminde bulunan kaynaklar üzerinde verilen izni kısıtlamak için kapsam olarak kullanılabilir.

## <a name="managing-administrative-units"></a>Yönetim birimlerini yönetme

Bu önizleme sürümünde, Windows PowerShell cmdlet 'leri için Azure Active Directory modülünü kullanarak yönetim birimleri oluşturabilir ve yönetebilirsiniz. Bunun nasıl yapılacağı hakkında daha fazla bilgi edinmek için bkz. [yönetim birimleriyle çalışma](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)

Yazılım gereksinimleri hakkında daha fazla bilgi edinmek ve Azure AD modülünü yüklemek ve sözdizimi, parametre açıklamaları ve örnekler de dahil olmak üzere yönetim birimlerini yönetmek için Azure AD modülü cmdlet 'leri hakkında bilgi için bkz. [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).

## <a name="next-steps"></a>Sonraki adımlar

[Azure Active Directory sürümleri](../fundamentals/active-directory-whatis.md)
