---
title: SCIM, Microsoft Graph ve Azure AD sağlama hizmetini kullanarak kullanıcılara hizmet vermek ve uygulamanızı ihtiyacı olan verilerle zenginleştirmek | Microsoft Dokümanlar
description: SCIM ve Microsoft Graph'ı birlikte kullanarak kullanıcılara sağlama ve uygulamanızı ihtiyacı olan verilerle zenginleştirme .
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 134237b66a803abaf07621112e3a4a518a3ae8a7
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82087627"
---
# <a name="using-scim-and-microsoft-graph-together-to-provision-users-and-enrich-your-application-with-the-data-it-needs"></a>SciM ve Microsoft Graph'ı kullanıcılara sağlama ve uygulamanızı ihtiyaç duyduğu verilerle zenginleştirmek için birlikte kullanma

**Hedef kitle:** Bu belge, Azure AD ile tümleşik uygulamalar oluşturan geliştiricilere yöneliktir. Zoom, ServiceNow ve DropBox gibi varolan bir uygulamayı entegre etmek isteyen başkaları için bunu atlayabilir ve uygulamaya özel [öğreticileri](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)gözden geçirebilirsiniz. 

**Yaygın senaryolar**

> [!div class="checklist"]
> * Uygulamamda otomatik olarak kullanıcı oluşturma
> * Artık erişememeleri gerektiğinde kullanıcıları uygulamamdan otomatik olarak kaldırın
> * Sağlama için başvurumu birden çok kimlik sağlayıcısıyla tümleştir
> * Uygulamamı Sharepoint, Outlook ve Office gibi Microsoft hizmetlerinden gelen verilerle zenginleştirin.
> * Azure AD ve Active Directory'deki kullanıcıları ve grupları otomatik olarak oluşturun, güncelleyin ve silin

![SCIM Grafik karar ağacı](./media/user-provisioning/scim-graph.png)

## <a name="scenario-1-automatically-create-users-in-my-app"></a>Senaryo 1: Uygulamamda otomatik olarak kullanıcı oluşturma
Bugün, BT yöneticileri uygulamamda her erişim gerektiğinde veya csv dosyalarını düzenli olarak yüklediklerinde el ile kullanıcı hesapları oluşturur. Süreç müşteriler için zaman alıcı ve benim uygulama nın benimsenmesini yavaşlatıyor. Tek ihtiyacım olan bir kullanıcı oluşturmak için ad, e-posta ve userPrincipalName gibi temel [kullanıcı](https://docs.microsoft.com/graph/api/resources/user?view=graph-rest-1.0) bilgileridir. Ayrıca, müşterilerim çeşitli IDP'ler kullanmak ve ben her IdP ile bir eşitleme motoru ve özel entegrasyonlar korumak için kaynaklara sahip değilsiniz. 

**Öneri**: SCIM uyumlu [/Users](https://aka.ms/scimreferencecode) bitiş noktasını destekleyin. Müşterileriniz, Azure AD sağlama hizmetiyle tümleştirmek ve erişime ihtiyaç duyduklarında otomatik olarak kullanıcı hesapları oluşturmak için bu bitiş noktasını kolayca kullanabilecektir. Bitiş noktasını bir kez oluşturabilirsiniz ve eşitleme motoru tutmak zorunda kalmadan tüm IDP'lerle uyumlu olacaktır. Bir kullanıcının nasıl oluşturulacağı yla ilgili aşağıdaki örnek isteğe göz atın.

```json
POST /Users
{
    "schemas": [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"],
    "externalId": "0a21f0f2-8d2a-4f8e-bf98-7363c4aed4ef",
    "userName": "BillG",
    "active": true,
    "meta": {
        "resourceType": "User"
    },
    "name": {
        "formatted": "Bill Gates",
        "familyName": "Gates",
        "givenName": "Bill"
    },
    "roles": []
}
```
    
## <a name="scenario-2-automatically-remove-users-from-my-app"></a>Senaryo 2: Kullanıcıları uygulamamdan otomatik olarak kaldırma
Uygulamamı kullanan müşteriler güvenlik odaklıdır ve çalışanların artık ihtiyaç duymadığında hesapları kaldırmak için yönetim gereksinimleri vardır. Uygulamamdan kaynaklanan hükmü nasıl otomatikleştirebilirim?

**Tavsiye:** SCIM uyumlu /Users bitiş noktasını destekleyin. Azure AD sağlama hizmeti, kullanıcının artık erişimi olmadığında devre dışı bırakma ve silme isteği gönderir. Kullanıcıları hem devre dışı bırakma hem de silmeyi desteklemenizi öneririz. Devre dışı ve silme isteğinin nasıl göründüğüne yönelik aşağıdaki örneklere bakın. 

Kullanıcıyı devre dışı
```json
PATCH /Users/5171a35d82074e068ce2 HTTP/1.1
{
    "Operations": [
        {
            "op": "Replace",
            "path": "active",
            "value": false
        }
    ],
    "schemas": [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"
    ]
}
```
Kullanıcıyı silme
```json
DELETE /Users/5171a35d82074e068ce2 HTTP/1.1
```

## <a name="scenario-3-automate-managing-group-memberships-in-my-app"></a>Senaryo 3: Uygulamamdaki grup üyeliklerini yönetmeyi otomatikleştirin
Uygulamam çeşitli kaynaklara erişmek için gruplara dayanır ve müşteriler Azure AD'deki grupları yeniden kullanmak ister. Grupları Azure AD'den nasıl içe aktarıp üyelikler değiştikçe güncel tutabilirim?  

**Tavsiye:** SCIM uyumlu /Gruplar [bitiş noktasını](https://aka.ms/scimreferencecode)destekleyin. Azure AD sağlama hizmeti, uygulamanızda gruplar oluşturma ve üyelik güncelleştirmelerini yönetme yi dikkate alır. 

## <a name="scenario-4-enrich-my-app-with-data-from-microsoft-services-such-as-teams-outlook-and-onedrive"></a>Senaryo 4: Uygulamamı Takımlar, Outlook ve OneDrive gibi Microsoft hizmetlerinden gelen verilerle zenginleştirin.
Uygulamam Microsoft Teams'te yerleşiktir ve ileti verilerine dayanır. Buna ek olarak, OneDrive'da kullanıcılar için dosyaları da saklarız. Uygulamamı bu hizmetlerden ve Microsoft genelinden gelen verilerle nasıl zenginleştirebilirim?

**Tavsiye:** [Microsoft Graph,](https://docs.microsoft.com/graph/) Microsoft verilerine erişmek için giriş noktanızdır. Her iş yükü, ihtiyacınız olan verilerle API'leri ortaya çıkarır. Microsoft grafiği, yukarıdaki senaryolar için [SCIM sağlama](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups) ile birlikte kullanılabilir. İhtiyacınız olan diğer verileri almak için grafiğe ararken uygulamanıza temel kullanıcı özniteliklerini sağlamak için SCIM'i kullanabilirsiniz. 

## <a name="scenario-5-track-changes-in-microsoft-services-such-as-teams-outlook-and-azure-ad"></a>Senaryo 5: Takımlar, Outlook ve Azure AD gibi Microsoft hizmetlerindeki değişiklikleri izleyin.
Takımlar ve Outlook iletilerindeki değişiklikleri izleyebilmem ve bunlara gerçek zamanlı olarak tepki verebilmem gerekir. Bu değişiklikleri başvuruma nasıl itebilirim?

**Tavsiye:** Microsoft Graph, çeşitli kaynaklar için [değişiklik bildirimleri](https://docs.microsoft.com/graph/webhooks) ve değişiklik izleme sağlar. Değişiklik bildirimlerinin aşağıdaki sınırlamalarına dikkat edin:
- Bir olay alıcısı bir olayı kabul ederse, ancak herhangi bir nedenle bu olay üzerinde hareket etmezse, olay kaybedilebilir
- Bir olay alıcısı bir olayı kabul ederse, ancak herhangi bir nedenle bu olay üzerinde hareket etmezse, olay kaybedilebilir
- Değişiklik bildirimleri her zaman [kaynak verilerini](https://docs.microsoft.com/graph/webhooks-with-resource-data) içermez Yukarıdaki nedenlerden dolayı, geliştiriciler genellikle eşitleme senaryoları için değişiklik izlemeile birlikte değişiklik bildirimlerini kullanır. 

## <a name="scenario-6-provision-users-and-groups-in-azure-ad"></a>Senaryo 6: Azure AD'de kullanıcıları ve grupları sağlama.
Uygulamam, müşterilerin Azure AD'de gereksinim duyduğu bir kullanıcı hakkında bilgi oluşturur. Bu, işe alma, kullanıcılar için telefon numaraları oluşturan bir iletişim uygulaması veya Azure AD'de değerli olabilecek veriler oluşturan başka bir uygulama dan daha iyi bir İk uygulaması olabilir. Azure AD'deki kullanıcı kaydını bu verilerle nasıl dolduracağım? 

**Tavsiye** Microsoft grafiği, kullanıcıları Azure AD'ye sağlamak için bugün tümleştirebileceğiniz /Users ve/Groups uç noktalarını ortaya çıkarır. Azure Active Directory'nin bu kullanıcıların Active Directory'ye geri yazılmasına destek olmadığını lütfen unutmayın. 

> [!NOTE]
> Microsoft'un, İş Günü ve Başarı Faktörleri gibi İk uygulamalarından veri çeken bir sağlama hizmeti vardır. Bu tümleştirmeler Microsoft tarafından oluşturulur ve yönetilir. Hizmetimize yeni bir İk uygulaması onboarding için, [UserVoice](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests)üzerinde talep edebilirsiniz. 

## <a name="related-articles"></a>İlgili makaleler:

- [Eşitleme Microsoft Graph belgelerini gözden geçirme](https://docs.microsoft.com/graph/api/resources/synchronization-overview?view=graph-rest-beta)
- [Azure AD ile özel bir SCIM uygulamasını tümleştirme](use-scim-to-provision-users-and-groups.md)
