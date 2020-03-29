---
title: Yönetici rol delegasyonuna anlam kazandırın - Azure Active Directory | Microsoft Dokümanlar
description: Azure Etkin Dizini'nde delegasyon modelleri, örnekler ve rol güvenliği
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6fa3c6bf39dbef601fe64e125999f519f725f2e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67083766"
---
# <a name="delegate-administration-in-azure-active-directory"></a>Azure Etkin Dizini'nde temsilci yönetimi

Örgütsel büyüme ile karmaşıklık geliyor. Ortak yanıtlardan biri, Azure Active Directory (AD) yönetici rolleriyle erişim yönetiminin bazı iş yükünü azaltmaktır. Kullanıcılara uygulamalarına erişmeleri ve görevlerini gerçekleştirmeleri için mümkün olan en az ayrıcalığı atayabilirsiniz. Her uygulama sahibine Genel Yönetici rolünü atamasanız bile, uygulama yönetimi sorumluluklarını varolan Global Yöneticilere yerine koyuyorsunuz. Bir organizasyonun daha merkezi olmayan bir yönetime doğru ilerlemesinin birçok nedeni vardır. Bu makale, kuruluşunuzdaki temsilcilik için plan lar düzenlemenize yardımcı olabilir.

<!--What about reporting? Who has which role and how do I audit?-->

## <a name="centralized-versus-delegated-permissions"></a>Merkezi ve temsilcili izinler

Bir kuruluş büyüdükçe, hangi kullanıcıların belirli yönetici rollerine sahip olduğunu izlemek zor olabilir. Bir çalışanın olmaması gereken yönetici hakları varsa, kuruluşunuz güvenlik ihlallerine karşı daha duyarlı olabilir. Genel olarak, kaç yöneticiyi desteklediğiniz ve izinlerinin ne kadar ayrıntılı olduğu dağıtımınızın boyutuna ve karmaşıklığına bağlıdır.

* Küçük veya kavram kanıtı dağıtımlarında, bir veya birkaç yönetici her şeyi yapar; Heyet falan yok. Bu durumda, Genel Yönetici rolüne sahip her yöneticiyi oluşturun.
* Daha fazla makine, uygulama ve masaüstü yle daha büyük dağıtımlarda daha fazla delegasyon gereklidir. Bazı yöneticilerin daha özel işlevsel sorumlulukları (rolleri) olabilir. Örneğin, bazıları Ayrıcalıklı Kimlik Yöneticileri, bazıları ise Uygulama Yöneticileri olabilir. Ayrıca, bir yönetici aygıtlar gibi yalnızca belirli nesne gruplarını yönetebilir.
* Daha büyük dağıtımlar daha da ayrıntılı izinler, artı muhtemelen alışılmadık veya karma rolleri olan yöneticiler gerektirebilir.

Azure AD portalında, dağıtım ve temsilci izinlerinizi hızlı bir şekilde kontrol etmenize yardımcı olabilecek [tüm rol üyelerini görüntüleyebilirsiniz.](directory-manage-roles-portal.md)

Azure AD'de yönetim erişimi yerine Azure kaynaklarına erişimi atamak istiyorsanız, [bkz.](../../role-based-access-control/role-assignments-portal.md)

## <a name="delegation-planning"></a>Delegasyon planlaması

İhtiyaçlarınıza uygun bir delegasyon modeli geliştirmek için çalışmaktır. Bir delegasyon modeli geliştirmek yinelemeli bir tasarım sürecidir ve aşağıdaki adımları izlemenizi öneririz:

* İhtiyacınız olan rolleri tanımlayın
* Temsilci uygulaması yönetimi
* Başvuruları kaydetme olanağı tanıyın
* Uygulama sahipliğini devredin
* Bir güvenlik planı geliştirme
* Acil durum hesapları oluşturma
* Yönetici rollerinizi güvence altına alma
* Ayrıcalıklı yükseklik geçici olun

## <a name="define-roles"></a>Rolleri tanımlama

Yöneticiler tarafından yürütülen Etkin Dizin görevlerini ve rollerle nasıl eşleneklerini belirleyin. Azure portalında [ayrıntılı rol açıklamalarını görüntüleyebilirsiniz.](directory-manage-roles-portal.md)

Her görev sıklık, önem ve zorluk açısından değerlendirilmelidir. Bu ölçütler görev tanımının hayati yönleridir, çünkü bir izin verilip verilmeyeceğini yönetirler:

* Rutin olarak yaptığınız, sınırlı risk altında olduğunuz ve tamamlamanız önemsiz olan görevler, delegasyon için mükemmel adaylardır.
* Nadiren yaptığınız ancak kuruluş genelinde büyük etkisi olan ve yüksek beceri düzeyleri gerektiren görevler, ataramadan önce çok dikkatli bir şekilde düşünülmelidir. Bunun yerine, [bir hesabı geçici olarak gerekli role yükseltebilir](../active-directory-privileged-identity-management-configure.md) veya görevi yeniden atayabilirsiniz.

## <a name="delegate-app-administration"></a>Temsilci uygulaması yönetimi

Kuruluşunuzdaki uygulamaların çoğalması delegasyon modelinizi zorlayabilir. Uygulama erişim yönetimi yükünü Global Administrator'a yüklerse, zaman geçtikçe modelin genel giderini artırması olasıdır. İnsanlara kurumsal uygulamaları yapılandırma gibi şeyler için Global Administrator rolü verdiyseniz, artık bunları aşağıdaki daha az ayrıcalıklı rollere boşaltabilirsiniz. Bunu yapmak güvenlik duruşunuzu iyileştirmenize yardımcı olur ve talihsiz hatalar yapma potansiyelini azaltır. En ayrıcalıklı uygulama yöneticisi rolleri şunlardır:

* Kayıtlar, tek oturum açma ayarları, kullanıcı ve grup atamaları ve lisanslama, Uygulama Proxy ayarları ve onay dahil olmak üzere dizindeki tüm uygulamaları yönetme olanağı sağlayan **Uygulama Yöneticisi** rolü. Koşullu Erişimi yönetme olanağı vermez.
* Uygulama Proxy ayarlarına erişim izni verilmemesi dışında (şirket içi izni olmadığı için) Uygulama Yöneticisi'nin tüm yeteneklerini sağlayan **Bulut Uygulama Yöneticisi** rolü.

## <a name="delegate-app-registration"></a>Temsilci uygulaması kaydı

Varsayılan olarak, tüm kullanıcılar uygulama kayıtları oluşturabilir. Uygulama kayıtları oluşturma olanağını seçerek vermek için:

* Set **Kullanıcıları, Kullanıcı ayarlarında** uygulamaları Hayır'a **kaydedebilir**
* Kullanıcıyı Uygulama Geliştiricisi rolüne atama

Bir uygulamanın verilere erişmesine izin vermek için seçici olarak izin vermek için:

* Set **Kullanıcıları, şirket verilerine kendi adlarına erişen uygulamaların** **Kullanıcı ayarlarında** Hayır'a ulaşmasını onaylayabilir
* Kullanıcıyı Uygulama Geliştiricisi rolüne atama

Bir Uygulama Geliştiricisi yeni bir uygulama kaydı oluşturduğunda, otomatik olarak ilk sahibi olarak eklenir.

## <a name="delegate-app-ownership"></a>Uygulama sahipliğini devredin

Daha da ince taneli uygulama erişim delegasyonu için, tek tek kurumsal uygulamalara sahiplik atayabilirsiniz. Bu, uygulama kayıt sahiplerini atamak için varolan desteği tamamlar. Sahiplik, Enterprise Applications blade'de şirket başına uygulama esasına göre atanır. Avantajı sahipleri sadece sahip oldukları kurumsal uygulamaları yönetebilirsiniz. Örneğin, Salesforce uygulaması için bir sahip atayabilirsiniz ve bu sahibi Salesforce'a erişimi ve yapılandırmayı yönetebilir ve başka uygulama yapamaz. Bir kuruluş uygulamasının birçok sahibi olabilir ve kullanıcı birçok kurumsal uygulamanın sahibi olabilir. İki uygulama sahibi rolü vardır:

* **Kurumsal Uygulama Sahibi** rolü, tek oturum açma ayarları, kullanıcı ve grup atamaları ve ek sahipler ekleme dahil olmak üzere kullanıcının sahip olduğu 'kurumsal uygulamaları" yönetme olanağı sağlar. Uygulama Proxy ayarlarını veya Koşullu Erişimi yönetme olanağı vermez.
* **Uygulama Kayıt Sahibi** rolü, kullanıcının sahip olduğu uygulama için uygulama bildirimi ve ek sahipler ekleme dahil olmak üzere uygulama kayıtlarını yönetme olanağı sağlar.

## <a name="develop-a-security-plan"></a>Bir güvenlik planı geliştirme

Azure AD, Azure AD yönetici rollerinizde bir güvenlik planı planlamak ve yürütmek için kapsamlı bir kılavuz sağlar, [karma ve bulut dağıtımları için ayrıcalıklı erişimi güvence altına](directory-admin-roles-secure.md)alar.

## <a name="establish-emergency-accounts"></a>Acil durum hesapları oluşturma

Sorun çıktığında kimlik yönetimi mağazanıza erişimi korumak için, [acil erişim li yönetim hesaplarına](directory-emergency-access.md)göre acil erişim hesapları hazırlayın.

## <a name="secure-your-administrator-roles"></a>Yönetici rollerinizi güvence altına alma

Ayrıcalıklı hesapların denetimini alan saldırganlar büyük zarar verebilir, bu nedenle tüm Azure AD kiracıları (genel önizlemede) varsayılan olarak kullanılabilen [temel erişim ilkesini](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/22/baseline-security-policy-for-azure-ad-admin-accounts-in-public-preview/) kullanarak önce bu hesapları koruyun. İlke, ayrıcalıklı Azure AD hesaplarında çok faktörlü kimlik doğrulaması uygular. Aşağıdaki Azure REKLAM rolleri Azure AD temel ilkesi kapsamındadır:

* Genel yönetici
* SharePoint yöneticisi
* Exchange yöneticisi
* Koşullu Erişim yöneticisi
* Güvenlik yöneticisi

## <a name="elevate-privilege-temporarily"></a>Gizliliği geçici olarak yükseltin

Günlük etkinliklerin çoğunda, tüm kullanıcıların genel yönetici haklarına ihtiyacı yoktur ve bunların tümü Global Administrator rolüne kalıcı olarak atanmamalıdır. Kullanıcılar bir Global Administrator'ın izinlerine ihtiyaç duyduklarında, Azure AD [Ayrıcalıklı Kimlik Yönetimi'ndeki](../active-directory-privileged-identity-management-configure.md) rol atamasını kendi hesaplarında veya alternatif bir yönetim hesabında etkinleştirmeleri gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD rol açıklamalarına başvurmak için Azure [AD'de yönetici rollerini atama](directory-assign-admin-roles.md)
