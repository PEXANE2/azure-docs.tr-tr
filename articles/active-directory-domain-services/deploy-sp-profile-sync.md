---
title: 'Azure Active Directory Domain Services: SharePoint kullanıcı profili hizmetini etkinleştirme | Microsoft Docs'
description: SharePoint Server için profil eşitlemesini desteklemek üzere Azure Active Directory Domain Services yönetilen etki alanlarını yapılandırma
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: iainfou
ms.openlocfilehash: 4a9ee05b37a69927d70269dccef2b74a2c251722
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234112"
---
# <a name="configure-a-managed-domain-to-support-profile-synchronization-for-sharepoint-server"></a>SharePoint Server için profil eşitlemesini desteklemek üzere yönetilen bir etki alanı yapılandırma
SharePoint Server, Kullanıcı profili eşitleme için kullanılan bir kullanıcı profili hizmeti içerir. Kullanıcı profili hizmetini ayarlamak için, Active Directory bir etki alanında uygun izinlerin verilmesi gerekir. Daha fazla bilgi için bkz. [SharePoint Server 2013 ' de profil eşitlemeye yönelik Active Directory Domain Services izinleri verme](https://technet.microsoft.com/library/hh296982.aspx).

Bu makalede SharePoint Server Kullanıcı profili eşitleme hizmeti 'ni dağıtmak için Azure AD Domain Services yönetilen etki alanlarını nasıl yapılandırabileceğiniz açıklanmaktadır.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="the-aad-dc-service-accounts-group"></a>' AAD DC hizmet hesapları ' grubu
'**AAD DC Service Accounts**' adlı bir güvenlik grubu, yönetilen etki alanındaki ' kullanıcılar ' kuruluş birimi içinde kullanılabilir. Bu grubu, yönetilen etki alanındaki **Active Directory Kullanıcıları ve bilgisayarları** MMC ek bileşeninde görebilirsiniz.

![AAD DC hizmet hesapları güvenlik grubu](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts.png)

Bu güvenlik grubunun üyelerine aşağıdaki ayrıcalıklar atanır:
- Yönetilen etki alanının kök DSE 'nin ' Dizin Değişikliklerini Çoğalt ' ayrıcalığı.
- Yönetilen etki alanının yapılandırma adlandırma bağlamı 'nda (cn = Yapılandırma kapsayıcısı) ' Dizin Değişikliklerini Çoğalt ' ayrıcalığı.

Bu güvenlik grubu, yerleşik grup **öncesi Windows 2000 Ile uyumlu erişim**'in de bir üyesidir.

![AAD DC hizmet hesapları güvenlik grubu](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-properties.png)


## <a name="enable-your-managed-domain-to-support-sharepoint-server-user-profile-sync"></a>Yönetilen etki alanınızı SharePoint Server Kullanıcı profili eşitlemesini destekleyecek şekilde etkinleştirme
SharePoint kullanıcı profili eşitleme için kullanılan hizmet hesabını **AAD DC hizmet hesapları** grubuna ekleyebilirsiniz. Sonuç olarak, eşitleme hesabı, değişiklikleri dizine çoğaltmak için yeterli ayrıcalıklara sahip olur. Bu yapılandırma adımı SharePoint Server Kullanıcı profili eşitlemesini doğru şekilde çalışacak şekilde sağlar.

![AAD DC hizmet hesapları-üye ekleme](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member.png)

![AAD DC hizmet hesapları-üye ekleme](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member2.png)

## <a name="related-content"></a>İlgili İçerik
* [Teknik başvuru-SharePoint Server 2013 ' de profil eşitlemeye yönelik Active Directory Domain Services izinleri verme](https://technet.microsoft.com/library/hh296982.aspx)
