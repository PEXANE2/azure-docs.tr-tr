---
title: Grup tabanlı lisanslama nedir - Azure Active Directory | Microsoft Dokümanlar
description: Azure Active Directory grup tabanlı lisanslama hakkında, bunların nasıl çalıştığı ve en iyi uygulamalar da dahil olmak üzere öğrenin.
services: active-directory
keywords: Azure AD lisanslama
author: msaburnley
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.topic: conceptual
ms.workload: identity
ms.date: 10/29/2018
ms.author: ajburnle
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9eec896e6cccaf58c83820161c54c4f10cfadadd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77561570"
---
# <a name="what-is-group-based-licensing-in-azure-active-directory"></a>Azure Active Directory'de grup tabanlı lisanslama nedir?

Microsoft'un ücretli bulut hizmetleri Office 365, Enterprise Mobility + Security, Dynamics 365 ve diğer benzer ürünler lisans gerektirir. Bu lisanslar, bu hizmetlere erişmesi gereken her kullanıcıya atanır. Lisansları yönetmek için, yöneticiler yönetim portallarından birini (Office veya Azure) ve PowerShell cmdlet'lerini kullanır. Azure Active Directory (Azure AD), tüm Microsoft bulut hizmetleri için kimlik yönetimini destekleyen temel altyapıdır. Azure AD kullanıcıların lisans atama durumları hakkındaki bilgileri depolar.

Şimdiye kadar, lisanslar yalnızca tek tek kullanıcı düzeyinde atanabiliyordu ve bu da geniş ölçekli yönetimi zorlaştırabiliyordu. Örneğin, kuruluşta veya bölümde yeni katılan veya ayrılan kullanıcılar gibi kurumsal değişikliklere göre kullanıcı lisansları eklemek veya kaldırmak için, yöneticinin çoğunlukla karmaşık bir PowerShell betiği yazması gerekir. Bu betik bulut hizmetine tek tek çağrılar yapar.

Bu zorlukları gidermek için, Azure AD artık grup tabanlı lisanslama özelliğini içermektedir. Gruba bir veya birden çok ürün lisansı atayabilirsiniz. Azure AD lisansların tüm grup üyelerine atanmasını güvence altına alır. Gruba katılan tüm yeni üyelere uygun lisanslar atanır. Kullanıcılar gruptan ayrıldığında, söz konusu lisanslar kaldırılır. Bu lisans yönetimi, kullanıcı başına kuruluş ve departman yapısındaki değişiklikleri yansıtmak için PowerShell üzerinden lisans yönetimini otomatikleştirme gereksinimini ortadan kaldırır.

## <a name="licensing-requirements"></a>Lisanslama gereksinimleri
Grup tabanlı lisanslamayı kullanmak için aşağıdaki lisanslardan birine sahip olmalısınız:

- Azure AD Premium P1 ve üzeri için ücretli veya deneme aboneliği

- DoD ve üzeri için GCCH veya Office 365 E3 için Office 365 A3 veya Office 365 GCC G3 veya Office 365 E3'ün ücretli veya deneme sürümü

### <a name="required-number-of-licenses"></a>Gerekli lisans sayısı
Lisans atanan gruplar için, her benzersiz üye için de bir lisansa sahip olmalısınız. Grubun her üyesine bir lisans atamak zorunda olmamakla birlikte, en azından tüm üyeleri içerecek kadar lisansa sahip olmalısınız. Örneğin, kiracınızdaki lisanslı grupların bir parçası olan 1.000 benzersiz üyeniz varsa, lisans sözleşmesini karşılamak için en az 1.000 lisansınız olmalıdır.

## <a name="features"></a>Özellikler

Grup tabanlı lisanslamanın başlıca özellikleri şunlardır:

- Lisanslar Azure AD'de her güvenlik grubuna atanabilir. Güvenlik grupları, Azure AD Connect kullanılarak şirket içinde senkronize edilebilir. Ayrıca, güvenlik gruplarını doğrudan Azure AD'de (yalnızca bulut grupları olarak da adlandırılır) veya Azure AD dinamik grup özelliği yoluyla otomatik olarak oluşturabilirsiniz.

- Gruba bir ürün lisansı atandığında, yönetici üründe bir veya birden çok hizmet planını devre dışı bırakabilir. Genellikle, bu atama, kuruluş bir ürüne dahil edilmiş bir hizmeti kullanmaya henüz hazır olmadığında yapılır. Örneğin, yönetici bir bölüme Office 365'i atayabilir ama Yammer hizmetini geçici olarak devre dışı bırakabilir.

- Kullanıcı düzeyi lisanslama gerektiren tüm Microsoft bulut hizmetleri desteklenir. Bu destek tüm Office 365 ürünlerini, Enterprise Mobility + Security'yi ve Dynamics 365'i içerir.

- Grup tabanlı lisanslama şu anda yalnızca [Azure portalı](https://portal.azure.com)üzerinden kullanılabilir. Öncelikle [Microsoft 365 yönetici merkezi](https://admin.microsoft.com)gibi kullanıcı ve grup yönetimi için diğer yönetim portallarını kullanıyorsanız, bunu yapmaya devam edebilirsiniz. Ancak, lisansları grup düzeyinde yönetmek için Azure portalını kullanmalısınız.

- Azure AD, grup üyeliği değişikliklerinden kaynaklanan lisans değişikliklerini otomatik olarak yönetir. Normalde, lisans değişiklikleri üyelik değişikliğini izleyen birkaç dakika içinde geçerlilik kazanır.

- Kullanıcı, lisans ilkeleri belirtilmiş olan birden çok gruba üye olabilir. Ayrıca kullanıcının, herhangi bir grubun dışında doğrudan atanmış bazı lisansları da olabilir. Sonuçta elde edilen kullanıcı durumu tüm atanan ürün ve hizmet lisanslarının bir bileşimidir. Bir kullanıcıya birden çok kaynaktan aynı lisans atanırsa, lisans yalnızca bir kez tüketilir.

- Bazı durumlarda, bir kullanıcıya lisans atanamayabilir. Örneğin, kiracıda yeterli kullanılabilir lisans olmayabilir veya aynı anda çakışan hizmetler atanmış olabilir. Yöneticiler, Azure AD'nin grup lisanslarını tam olarak işleyemediği kullanıcılar hakkındaki bilgilere erişebilir. Bu bilgiler temelinde düzeltme eylemi gerçekleştirebilir.

## <a name="your-feedback-is-welcome"></a>Geri bildiriminiz bizim için çok önemlidir!

Geri bildiriminiz veya özellik istekleriniz varsa, [Azure AD yönetici forumlarını](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=162510)kullanarak bunları bizimle paylaşın.

## <a name="next-steps"></a>Sonraki adımlar

Grup tabanlı lisanslama aracılığıyla lisans yönetimine yönelik diğer senaryolar hakkında daha fazla bilgi edinmek için bkz:

* [Azure Active Directory'de gruba lisans atama](../users-groups-roles/licensing-groups-assign.md)
* [Azure Active Directory'de grubun lisans sorunlarını tanımlama ve çözme](../users-groups-roles/licensing-groups-resolve-problems.md)
* [Azure Active Directory'de tek tek lisanslı kullanıcıları grup tabanlı lisanslamaya geçirme](../users-groups-roles/licensing-groups-migrate-users.md)
* [Azure Active Directory'de grup tabanlı lisanslama yı kullanarak kullanıcıların ürün lisansları arasında geçiş ilertirme](../users-groups-roles/licensing-groups-change-licenses.md)
* [Azure Active Directory grup tabanlı lisanslamayla ilgili ek senaryolar](../users-groups-roles/licensing-group-advanced.md)
* [Azure Active Directory'de grup tabanlı lisanslama için PowerShell örnekleri](../users-groups-roles/licensing-ps-examples.md)
