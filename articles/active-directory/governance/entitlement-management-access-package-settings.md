---
title: Azure AD yetkilendirme yönetiminde erişim paketi istemek için bağlantıyı paylaşın - Azure Active Directory
description: Azure Active Directory yetkilendirme yönetiminde bir erişim paketi istemek için bağlantıyı nasıl paylaşılacın gerektiğini öğrenin.
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
ms.openlocfilehash: ea90032b1f0cfe598ffdb3d35448a996f3111036
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78968757"
---
# <a name="share-link-to-request-an-access-package-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde bir erişim paketi istemek için bağlantıyı paylaşma

Dizininizdeki çoğu kullanıcı Erişimim portalında oturum açabilir ve isteyebilecekleri erişim paketlerinin listesini otomatik olarak görebilir. Ancak, henüz dizininizde olmayan harici iş ortağı kullanıcılar için, onlara bir erişim paketi istemek için kullanabilecekleri bir bağlantı göndermeniz gerekir. 

Erişim paketinin kataloğu [harici kullanıcılar için etkinleştirildiğinden](entitlement-management-catalog-create.md) ve harici [kullanıcıdizini için](entitlement-management-access-package-request-policy.md)bir politikanız olduğu sürece, harici kullanıcı erişim paketini istemek için Erişim portalım bağlantısını kullanabilir.

## <a name="share-link-to-request-an-access-package"></a>Erişim paketi istemek için bağlantıyı paylaşma

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi, Katalog sahibi veya Access paket yöneticisi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde, **Access paketlerini** tıklatın ve ardından erişim paketini açın.

1. Genel Bakış sayfasında, **Erişim portalım bağlantısını**kopyalayın.

    ![Erişim paketine genel bakış - Erişim portalı bağlantım](./media/entitlement-management-shared/my-access-portal-link.png)

    Bir iç iş ortağına gönderirken Tüm Erişim portalıbağlantımı kopyalamanız önemlidir. Bu, ortağın istekte bulunmak için dizininizin portalına erişmesini sağlar. Bağlantı, `myaccess`bir dizin ipucu içerir ve bir erişim paketi kimliği ile sona erer.  (ABD Hükümeti için, Erişim portalıbağlantımdaki `myaccess.microsoft.us`etki alanı olacaktır.)

    `https://myaccess.microsoft.com/@<directory_hint>#/access-packages/<access_package_id>`

1. Bağlantıyı harici iş ortağınıza e-posta ile gönderin veya gönderin. Erişim paketini istemek için bağlantıyı kullanıcılarıyla paylaşabilirler.

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim paketine erişim isteği](entitlement-management-request-access.md)
- [Erişim isteklerini onaylama veya reddetme](entitlement-management-request-approve.md)