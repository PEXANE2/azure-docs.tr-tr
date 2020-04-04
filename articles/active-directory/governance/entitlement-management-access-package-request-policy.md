---
title: Azure AD yetkilendirme yönetiminde bir erişim paketi için istek ve onay ayarlarını değiştirme - Azure Etkin Dizini
description: Azure Active Directory yetkilendirme yönetiminde bir erişim paketi için istek ve onay ayarlarını nasıl değiştireceğinizi öğrenin.
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
ms.date: 03/30/2020
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: b0eae4f53283c33f3d7372a606f8c0a3ac27d079
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80655951"
---
# <a name="change-request-and-approval-settings-for-an-access-package-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde bir erişim paketi için istek ve onay ayarlarını değiştirme

Erişim paketi yöneticisi olarak, ilkeyi düzenleyerek veya yeni bir ilke ekleyerek istediğiniz zaman bir erişim paketi talep edebilirsiniz kullanıcıları değiştirebilirsiniz. Onay ayarlarını da değiştirebilirsiniz.

Bu makalede, varolan bir erişim paketi için istek ve onay ayarlarının nasıl değiştirilen açıklanmaktadır.

## <a name="choose-between-one-or-multiple-polices"></a>Bir veya birden çok polis arasında seçim yapın

Bir erişim paketini kimlerin isteyebileceğini belirtme şekliniz bir ilkeyle dir. Bir erişim paketi oluşturduğunuzda, bir ilke oluşturan istek ve onay ayarını belirtirsiniz. Çoğu erişim paketinin tek bir ilkesi vardır, ancak tek bir erişim paketinin birden çok ilkesi olabilir. Farklı kullanıcı kümelerine farklı istek ve onay ayarlarıyla atama verilmesine izin vermek istiyorsanız, bir erişim paketi için birden çok ilke oluşturursunuz. Örneğin, tek bir ilke, dahili ve harici kullanıcıları aynı erişim paketine atamak için kullanılamaz. Ancak, aynı erişim paketinde biri dahili kullanıcılar, diğeri dış kullanıcılar için olmak üzere iki ilke oluşturabilirsiniz. Bir kullanıcı için geçerli olan birden çok ilke varsa, bu ilkeler, atanmak istedikleri ilkeyi seçmeleri istendiği anda istenir. Aşağıdaki diyagram, iki ilke içeren bir erişim paketini gösterir.

![Bir erişim paketinde birden çok ilke](./media/entitlement-management-access-package-request-policy/access-package-policy.png)

| Senaryo | İlke sayısı |
| --- | --- |
| Dizinimdeki tüm kullanıcıların bir erişim paketi için aynı istek ve onay ayarlarına sahip olmasını istiyorum | Bir |
| Belirli bağlı kuruluşlardaki tüm kullanıcıların bir erişim paketi isteyebilmesini istiyorum | Bir |
| Dizinimdeki ve dizinim dışındaki kullanıcıların bir erişim paketi istemesine izin vermek istiyorum | Birden çok |
| Bazı kullanıcılar için farklı onay ayarları belirtmek istiyorum | Birden çok |
| Bazı kullanıcıların paket atamalarına erişim süresinin dolmasını ve diğer kullanıcıların erişimlerini genişletmesini istiyorum | Birden çok |

Birden çok ilke uygulandığında kullanılan öncelik mantığı hakkında bilgi [için](entitlement-management-troubleshoot.md#multiple-policies
)bkz.

### <a name="open-an-existing-policy-of-request-and-approval-settings"></a>Varolan istek ve onay ayarları ilkesini açma

Bir erişim paketi için istek ve onay ayarlarını değiştirmek için ilgili ilkeyi açmanız gerekir. Bir erişim paketi için istek ve onay ayarlarını açmak için aşağıdaki adımları izleyin.

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi, Katalog sahibi veya Access paket yöneticisi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde, **Access paketlerini** tıklatın ve ardından erişim paketini açın.

1. **İlkeler'i** tıklatın ve ardından görüntülemek istediğiniz ilkeyi tıklatın.

    İlke ayrıntıları bölmesi sayfanın alt kısmında açılır.

    ![Erişim paketi - İlke ayrıntıları bölmesi](./media/entitlement-management-shared/policy-details.png)

1. İlkeyi yeniden yapmak için **Edit'i** tıklatın.

    ![Erişim paketi - İlkeyi güncelle](./media/entitlement-management-shared/policy-edit.png)

1. İstek ve onay ayarlarını açmak için **İstekler** sekmesini tıklatın.

1. Aşağıdaki istek bölümlerinden birinde adımları gerçekleştirin.

### <a name="add-a-new-policy-of-request-and-approval-settings"></a>İstek ve onay ayarlarıyla ilgili yeni bir ilke ekleme

Farklı istek ve onay ayarlarına sahip olması gereken bir kullanıcı kümeniz varsa, büyük olasılıkla yeni bir ilke oluşturmanız gerekir. Varolan bir erişim paketine yeni bir ilke eklemeye başlamak için aşağıdaki adımları izleyin.

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi, Katalog sahibi veya Access paket yöneticisi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde, **Access paketlerini** tıklatın ve ardından erişim paketini açın.

1. **İlkeler'i** tıklatın ve ardından **ilke ekleyin.**

1. İlke için bir ad ve açıklama yazın.

    ![Ad ve açıklama ile ilke oluşturma](./media/entitlement-management-access-package-request-policy/policy-name-description.png)

1. **İstekler** sekmesini açmak için **İleri'yi** tıklatın.

1. Aşağıdaki istek bölümlerinden birinde adımları gerçekleştirin.

[!INCLUDE [Entitlement management request policy](../../../includes/active-directory-entitlement-management-request-policy.md)]

Bir ilke düzenliyorsanız **Güncelleştir'i**tıklatın. Yeni bir ilke ekliyorsanız, **Oluştur'u**tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim paketi için yaşam döngüsü ayarlarını değiştirme](entitlement-management-access-package-lifecycle-policy.md)
- [Erişim paketi isteklerini görüntüleme](entitlement-management-access-package-requests.md)