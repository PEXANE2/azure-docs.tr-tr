---
title: Grup tabanlı lisanslama nedir-Azure Active Directory | Microsoft Docs
description: Nasıl çalıştığı ve en iyi yöntemler dahil olmak üzere Azure Active Directory grup tabanlı lisanslama hakkında bilgi edinin.
services: active-directory
keywords: Azure AD lisanslama
author: ajburnle
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
ms.openlocfilehash: 0490334c759da6ef7ba7ff2535f5f561cdb7a9bf
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92369820"
---
# <a name="what-is-group-based-licensing-in-azure-active-directory"></a>Azure Active Directory 'de grup tabanlı lisanslama nedir?

Microsoft 365, Enterprise Mobility + Security, Dynamics 365 ve diğer benzer ürünler gibi ücretli Microsoft bulut Hizmetleri, lisans gerektirir. Bu lisanslar, bu hizmetlere erişmesi gereken her kullanıcıya atanır. Lisansları yönetmek için, yöneticiler yönetim portallarından birini (Office veya Azure) ve PowerShell cmdlet'lerini kullanır. Azure Active Directory (Azure AD), tüm Microsoft bulut hizmetleri için kimlik yönetimini destekleyen temel altyapıdır. Azure AD kullanıcıların lisans atama durumları hakkındaki bilgileri depolar.

Şimdiye kadar, lisanslar yalnızca tek tek kullanıcı düzeyinde atanabiliyordu ve bu da geniş ölçekli yönetimi zorlaştırabiliyordu. Örneğin, kuruluşta veya bölümde yeni katılan veya ayrılan kullanıcılar gibi kurumsal değişikliklere göre kullanıcı lisansları eklemek veya kaldırmak için, yöneticinin çoğunlukla karmaşık bir PowerShell betiği yazması gerekir. Bu betik bulut hizmetine tek tek çağrılar yapar.

Bu zorlukları gidermek için, Azure AD artık grup tabanlı lisanslama özelliğini içermektedir. Gruba bir veya birden çok ürün lisansı atayabilirsiniz. Azure AD lisansların tüm grup üyelerine atanmasını güvence altına alır. Gruba katılan tüm yeni üyelere uygun lisanslar atanır. Kullanıcılar gruptan ayrıldığında, söz konusu lisanslar kaldırılır. Bu lisans yönetimi, yönetim aracılığıyla lisans yönetimini otomatik hale getirmeye ve Kullanıcı başına departman yapısındaki değişiklikleri yansıtacak şekilde ortadan kaldırır.

## <a name="licensing-requirements"></a>Lisanslama gereksinimleri
Grup tabanlı lisanslama kullanmak için aşağıdaki lisanslardan birine sahip olmanız gerekir:

- Azure AD Premium P1 ve üzeri için ücretli veya deneme aboneliği

- Office 365 Enterprise E3 veya Office 365 a3 veya Office 365 GCC G3 ya da Office 365 E3 for the DOD and üzeri for GCCH veya Office 365 E3 ücretli veya deneme sürümü

### <a name="required-number-of-licenses"></a>Gerekli sayıda lisans
Lisans atanmış tüm gruplar için, her benzersiz üye için de lisansa sahip olmanız gerekir. Grubun her üyesine bir lisans atamanız gerekmiyorsa, tüm üyeleri dahil etmek için en az lisansa sahip olmanız gerekir. Örneğin, kiracınızdaki lisanslı grupların parçası olan 1.000 benzersiz üyesi varsa, lisans sözleşmesini karşılamak için en az 1.000 lisansa sahip olmanız gerekir.

## <a name="features"></a>Özellikler

Grup tabanlı lisanslamanın başlıca özellikleri şunlardır:

- Lisanslar Azure AD'de her güvenlik grubuna atanabilir. Güvenlik grupları, Azure AD Connect kullanılarak Şirket içinden eşitlenebilir. Ayrıca, güvenlik gruplarını doğrudan Azure AD'de (yalnızca bulut grupları olarak da adlandırılır) veya Azure AD dinamik grup özelliği yoluyla otomatik olarak oluşturabilirsiniz.

- Gruba bir ürün lisansı atandığında, yönetici üründe bir veya birden çok hizmet planını devre dışı bırakabilir. Genellikle, bu atama, kuruluş henüz bir ürüne dahil olan bir hizmeti kullanmaya başlamaya Hazırlanmadıysa yapılır. Örneğin, yönetici bir departmana Microsoft 365 atayabilir, ancak Yammer hizmetini geçici olarak devre dışı bırakabilir.

- Kullanıcı düzeyi lisanslama gerektiren tüm Microsoft bulut hizmetleri desteklenir. Bu destek, tüm Microsoft 365 ürünlerini, Enterprise Mobility + Security ve Dynamics 365 ' i içerir.

- Grup tabanlı lisanslama Şu anda yalnızca [Azure Portal](https://portal.azure.com)üzerinden kullanılabilir. Birincil olarak [Microsoft 365 Yönetim Merkezi](https://admin.microsoft.com)gibi Kullanıcı ve Grup yönetimi için diğer yönetim portallarını kullanıyorsanız, bunu yapmaya devam edebilirsiniz. Ancak, lisansları grup düzeyinde yönetmek için Azure portalını kullanmalısınız.

- Azure AD, grup üyeliği değişikliklerinden kaynaklanan lisans değişikliklerini otomatik olarak yönetir. Normalde, lisans değişiklikleri üyelik değişikliğini izleyen birkaç dakika içinde geçerlilik kazanır.

- Kullanıcı, lisans ilkeleri belirtilmiş olan birden çok gruba üye olabilir. Ayrıca kullanıcının, herhangi bir grubun dışında doğrudan atanmış bazı lisansları da olabilir. Sonuçta elde edilen kullanıcı durumu tüm atanan ürün ve hizmet lisanslarının bir bileşimidir. Bir kullanıcıya birden çok kaynaktan aynı lisans atanırsa, lisans yalnızca bir kez tüketilecektir.

- Bazı durumlarda, bir kullanıcıya lisans atanamayabilir. Örneğin, kiracıda yeterli kullanılabilir lisans olmayabilir veya aynı anda çakışan hizmetler atanmış olabilir. Yöneticiler, Azure AD'nin grup lisanslarını tam olarak işleyemediği kullanıcılar hakkındaki bilgilere erişebilir. Bu bilgiler temelinde düzeltme eylemi gerçekleştirebilir.

## <a name="your-feedback-is-welcome"></a>Geri bildiriminiz bizim için çok önemlidir!

Geri bildirim veya özellik istekleriniz varsa, [Azure AD yönetici Forumu](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=162510)' nu kullanarak bunları bizimle paylaşabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Grup tabanlı lisanslama aracılığıyla lisans yönetimine yönelik diğer senaryolar hakkında daha fazla bilgi edinmek için bkz:

* [Azure Active Directory'de gruba lisans atama](../enterprise-users/licensing-groups-assign.md)
* [Azure Active Directory'de grubun lisans sorunlarını tanımlama ve çözme](../enterprise-users/licensing-groups-resolve-problems.md)
* [Azure Active Directory'de tek tek lisanslı kullanıcıları grup tabanlı lisanslamaya geçirme](../enterprise-users/licensing-groups-migrate-users.md)
* [Azure Active Directory 'de grup tabanlı lisanslama kullanarak kullanıcıları ürün lisansları arasında geçirme](../enterprise-users/licensing-groups-change-licenses.md)
* [Azure Active Directory grup tabanlı lisanslamayla ilgili ek senaryolar](../enterprise-users/licensing-group-advanced.md)
* [Azure Active Directory 'de grup tabanlı lisanslama için PowerShell örnekleri](../enterprise-users/licensing-ps-examples.md)
