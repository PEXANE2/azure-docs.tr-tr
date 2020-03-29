---
title: Yetkilendirme yönetiminde sık karşılaşılan senaryolar - Azure AD
description: Azure Active Directory yetkilendirme yönetiminde sık karşılaşılan senaryolar için izlemeniz gereken üst düzey adımları öğrenin.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/28/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: d9d259c6e2a6ac9ced5f9a1c29d4aec08010f4dc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78190560"
---
# <a name="common-scenarios-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde sık karşılaşılan senaryolar

Kuruluşunuz için yetkilendirme yönetimini yapılandırmanın birkaç yolu vardır. Ancak, yeni başlıyorsanız, yöneticiler, katalog sahipleri, erişim paketi yöneticileri, onaylayıcılar ve istekte bulundurcular için sık karşılaşılan senaryoları anlamak yararlıdır.

## <a name="delegate"></a>Temsilci

### <a name="administrator-delegate-management-of-resources"></a>Yönetici: Kaynakların temsilci yönetimi

1. [Videoyu izleyin: BT'den departman yöneticisine heyet](https://www.microsoft.com/videoplayer/embed/RE3Lq00)
1. [Kullanıcıları katalog oluşturucu rolüne temsilci olarak](entitlement-management-delegate-catalog.md)

### <a name="catalog-creator-delegate-management-of-resources"></a>Katalog oluşturucu: Kaynakların temsilci yönetimi

- [Yeni bir katalog oluşturma](entitlement-management-catalog-create.md#create-a-catalog)

### <a name="catalog-owner-delegate-management-of-resources"></a>Katalog sahibi: Kaynakların temsilci yönetimi

1. [Ortak sahiplerini kataloğerlere ekleme](entitlement-management-catalog-create.md#add-additional-catalog-owners)
1. [Kataloğuna kaynak ekleme](entitlement-management-catalog-create.md#add-resources-to-a-catalog)

### <a name="catalog-owner-delegate-management-of-access-packages"></a>Katalog sahibi: Erişim paketlerinin temsilci yönetimi

1. [Videoyu izleyin: Katalog sahibinden paket yöneticisine erişim için temsilci](https://www.microsoft.com/videoplayer/embed/RE3Lq08)
1. [Kullanıcıları paket yöneticisi rolüne erişmek için temsilci olarak](entitlement-management-delegate-managers.md)

## <a name="govern-access-for-users-in-your-organization"></a>Kuruluşunuzdaki kullanıcıların erişimini yönetin

### <a name="access-package-manager-allow-employees-in-your-organization-to-request-access-to-resources"></a>Erişim paketi yöneticisi: Kuruluşunuzdaki çalışanların kaynaklara erişim isteğinde bulunmalarına izin verin

1. [Yeni erişim paketi oluşturma](entitlement-management-access-package-create.md#start-new-access-package)
1. [Pakete erişmek için gruplar, Takımlar, uygulamalar veya SharePoint siteleri ekleme](entitlement-management-access-package-create.md#resource-roles)
1. [Dizininizdeki kullanıcıların erişim istemesine izin vermek için bir istek ilkesi ekleme](entitlement-management-access-package-create.md#for-users-in-your-directory)
1. [Son kullanma ayarlarını belirtin](entitlement-management-access-package-create.md#lifecycle)

### <a name="requestor-request-access-to-resources"></a>İstekçi: Kaynaklara erişim isteği

1. [Erişimim portalında oturum açın](entitlement-management-request-access.md#sign-in-to-the-my-access-portal)
1. Erişim paketini bulma
1. [Erişim izni isteme](entitlement-management-request-access.md#request-an-access-package)

### <a name="approver-approve-requests-to-resources"></a>Onaylayıcı: Kaynaklardaki istekleri onaylama

1. [Access portalımda açık istek](entitlement-management-request-approve.md#open-request)
1. [Erişim isteğini onaylama veya reddetme](entitlement-management-request-approve.md#approve-or-deny-request)

### <a name="requestor-view-the-resources-you-already-have-access-to"></a>İstekçi: Zaten erişebildiğiniz kaynakları görüntüleyin

1. [Erişimim portalında oturum açın](entitlement-management-request-access.md#sign-in-to-the-my-access-portal)
1. Etkin erişim paketlerini görüntüleme

## <a name="govern-access-for-users-outside-your-organization"></a>Kuruluşunuz dışındaki kullanıcılar için erişimi yönetin

### <a name="administrator-collaborate-with-an-external-partner-organization"></a>Yönetici: Harici bir ortak kuruluşla işbirliği yapma

1. [Dış kullanıcılar için erişimin nasıl çalıştığını okuyun](entitlement-management-external-users.md#how-access-works-for-external-users)
1. [Harici kullanıcılar için ayarları gözden geçirme](entitlement-management-external-users.md#settings-for-external-users)
1. [Dış kuruluşa bağlantı ekleme](entitlement-management-organization.md)

### <a name="access-package-manager-collaborate-with-an-external-partner-organization"></a>Erişim paketi yöneticisi: Harici bir iş ortağı kuruluşuyla işbirliği yapma

1. [Yeni erişim paketi oluşturma](entitlement-management-access-package-create.md#start-new-access-package)
1. [Pakete erişmek için gruplar, Takımlar, uygulamalar veya SharePoint siteleri ekleme](entitlement-management-access-package-resources.md#add-resource-roles)
1. [Dizininizde olmayan kullanıcıların erişim istemesine izin vermek için bir istek ilkesi ekleme](entitlement-management-access-package-request-policy.md#for-users-not-in-your-directory)
1. [Son kullanma ayarlarını belirtin](entitlement-management-access-package-create.md#lifecycle)
1. [Erişim paketini istemek için bağlantıyı kopyalama](entitlement-management-access-package-settings.md)
1. Kullanıcılarıyla paylaşmak için bağlantıyı harici iş ortağınla paylaşmaya gönder

### <a name="requestor-request-access-to-resources-as-an-external-user"></a>İstekçi: Harici kullanıcı olarak kaynaklara erişim isteğinde bulunma

1. İlgili kişinizden aldığınız erişim paketi bağlantısını bulun
1. [Erişimim portalında oturum açın](entitlement-management-request-access.md#sign-in-to-the-my-access-portal)
1. [Erişim izni isteme](entitlement-management-request-access.md#request-an-access-package)

### <a name="approver-approve-requests-to-resources"></a>Onaylayıcı: Kaynaklardaki istekleri onaylama

1. [Access portalımda açık istek](entitlement-management-request-approve.md#open-request)
1. [Erişim isteğini onaylama veya reddetme](entitlement-management-request-approve.md#approve-or-deny-request)

### <a name="requestor-view-the-resources-your-already-have-access-to"></a>İstekçi: Zaten erişebildiğiniz kaynakları görüntüleyin

1. [Erişimim portalında oturum açın](entitlement-management-request-access.md#sign-in-to-the-my-access-portal)
1. Etkin erişim paketlerini görüntüleme

## <a name="day-to-day-management"></a>Günlük yönetim

### <a name="access-package-manager-update-the-resources-for-a-project"></a>Erişim paketi yöneticisi: Proje için kaynakları güncelleştirme

1. [Videoyu izleyin: Günlük yönetim: İşler değişti](https://www.microsoft.com/videoplayer/embed/RE3LD4Z)
1. Erişim paketini açma
1. [Gruplar, Takımlar, uygulamalar veya SharePoint siteleri ekleme veya kaldırma](entitlement-management-access-package-resources.md#add-resource-roles)

### <a name="access-package-manager-update-the-duration-for-a-project"></a>Erişim paketi yöneticisi: Projenin süresini güncelleştirme

1. [Videoyu izleyin: Günlük yönetim: İşler değişti](https://www.microsoft.com/videoplayer/embed/RE3LD4Z)
1. Erişim paketini açma
1. [Yaşam döngüsü ayarlarını açma](entitlement-management-access-package-lifecycle-policy.md#open-lifecycle-settings)
1. [Son kullanma ayarlarını güncelleştirme](entitlement-management-access-package-lifecycle-policy.md#lifecycle)

### <a name="access-package-manager-update-how-access-is-approved-for-a-project"></a>Erişim paketi yöneticisi: Bir proje için erişimin onaylanma şeklini güncelleştirin

1. [Videoyu izleyin: Günlük yönetim: İşler değişti](https://www.microsoft.com/videoplayer/embed/RE3LD4Z)
1. [Varolan istek ve onay ayarları ilkesini açma](entitlement-management-access-package-request-policy.md#open-an-existing-policy-of-request-and-approval-settings)
1. [Onay ayarlarını güncelleştirme](entitlement-management-access-package-request-policy.md#approval)

### <a name="access-package-manager-update-the-people-for-a-project"></a>Erişim paketi yöneticisi: Proje için kişileri güncelleştirme

1. [Videoyu izleyin: Günlük yönetim: İşler değişti](https://www.microsoft.com/videoplayer/embed/RE3LD4Z)
1. [Artık erişime ihtiyaç duymadan kullanıcıları kaldırma](entitlement-management-access-package-assignments.md)
1. [Varolan istek ve onay ayarları ilkesini açma](entitlement-management-access-package-request-policy.md#open-an-existing-policy-of-request-and-approval-settings)
1. [Erişime ihtiyaç duyan kullanıcıları ekleme](entitlement-management-access-package-request-policy.md#for-users-in-your-directory)

### <a name="access-package-manager-directly-assign-specific-users-to-an-access-package"></a>Erişim paketi yöneticisi: Belirli kullanıcıları doğrudan bir erişim paketine atama

1. [Kullanıcıların farklı yaşam döngüsü ayarlarına ihtiyacı varsa, erişim paketine yeni bir ilke ekleyin](entitlement-management-access-package-request-policy.md#add-a-new-policy-of-request-and-approval-settings)
1. [Erişim paketine doğrudan belirli kullanıcıları atama](entitlement-management-access-package-assignments.md#directly-assign-a-user)

## <a name="assignments-and-reports"></a>Atamalar ve raporlar

### <a name="administrator-view-who-has-assignments-to-an-access-package"></a>Yönetici: Erişim paketinde kimlerin atamaları olduğunu görüntüleme

1. Erişim paketi açma
1. [Atamaları görüntüleme](entitlement-management-access-package-assignments.md#view-who-has-an-assignment)
1. [Arşiv raporları ve günlükleri](entitlement-management-logs-and-reporting.md)

### <a name="administrator-view-resources-assigned-to-users"></a>Yönetici: Kullanıcılara atanan kaynakları görüntüleme

1. [Bir kullanıcı için erişim paketlerini görüntüleme](entitlement-management-reports.md#view-access-packages-for-a-user)
1. [Bir kullanıcı için kaynak atamalarını görüntüleme](entitlement-management-reports.md#view-resource-assignments-for-a-user)

## <a name="programmatic-administration"></a>Programlı yönetim

Microsoft Graph'ı kullanarak erişim paketlerini, katalogları, ilkeleri, istekleri ve atamaları da yönetebilirsiniz.  Yetkilendirme `EntitlementManagement.ReadWrite.All` izniolan bir uygulama yla uygun bir roldeki bir [kullanıcı, yetkilendirme yönetimi API'sini](https://docs.microsoft.com/graph/api/resources/entitlementmanagement-root?view=graph-rest-beta)arayabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Temsil ve roller](entitlement-management-delegate.md)
- [İstek süreci ve e-posta bildirimleri](entitlement-management-process.md)
