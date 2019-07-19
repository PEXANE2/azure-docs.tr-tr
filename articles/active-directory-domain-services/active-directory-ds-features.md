---
title: 'Azure Active Directory Domain Services: Özellikler | Microsoft Docs'
description: Azure Active Directory Domain Services özellikleri
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 8d1c3eb3-1022-4add-a919-c98cc6584af1
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: iainfou
ms.openlocfilehash: e0f38ed8367f076c9f9c4da77440bb3e9603d514
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234203"
---
# <a name="features-provided-by-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services tarafından sunulan özellikler

Azure AD Domain Services yönetilen etki alanlarında aşağıdaki özellikler mevcuttur.

* **Basit dağıtım deneyimi:** Yalnızca birkaç tıklama kullanarak Azure AD dizininiz için Azure AD Domain Services etkinleştirebilirsiniz. Yönetilen etki alanınız, yalnızca bulutta yer alan Kullanıcı hesaplarını ve şirket içi dizinden eşitlenen Kullanıcı hesaplarını içerir.
* **Etki alanına ekleme desteği:** Yönetilen etki alanınıza, Azure sanal ağ 'daki bilgisayarları kolayca katabilirsiniz. Windows istemci ve sunucu işletim sistemlerindeki etki alanı ekleme deneyimi Azure AD Domain Services tarafından hizmet verilen etki alanlarına sorunsuz şekilde çalışır. Bu tür etki alanlarında otomatik etki alanı birleştirme araçları da kullanabilirsiniz.
* **Azure AD dizini başına bir etki alanı örneği:** Her Azure AD dizini için tek bir Active Directory etki alanı oluşturabilirsiniz.
* **Özel adlara sahip etki alanları oluşturun:** Azure AD Domain Services kullanarak özel adlara sahip etki alanları (örneğin, ' contoso100.com ') oluşturabilirsiniz. Doğrulanmış ya da doğrulanmamış etki alanı adlarını kullanabilirsiniz. İsteğe bağlı olarak, Azure AD dizininiz tarafından sunulan yerleşik etki alanı sonekine (yani, ' *. onmicrosoft.com ') sahip bir etki alanı da oluşturabilirsiniz.
* **Azure AD ile tümleşik:** Azure AD Domain Services için çoğaltmayı yapılandırmanız veya yönetmeniz gerekmez. Azure AD dizininizdeki Kullanıcı hesapları, grup üyelikleri ve Kullanıcı kimlik bilgileri (parolalar) Azure AD Domain Services ' de otomatik olarak kullanılabilir. Azure AD kiracınızdan veya şirket içi dizininizde bulunan yeni kullanıcılar, gruplar veya özniteliklerde yapılan değişiklikler Azure AD Domain Services otomatik olarak eşitlenir.
* **NTLM ve Kerberos kimlik doğrulaması:** NTLM ve Kerberos kimlik doğrulaması desteğiyle, Windows ile tümleşik kimlik doğrulamasına dayanan uygulamalar dağıtabilirsiniz.
* **Şirket kimlik bilgilerinizi/parolalarınızı kullanın:** Azure AD kiracınızdaki kullanıcılar için parolalar Azure AD Domain Services çalışır. Kullanıcılar, Şirket kimlik bilgilerini etki alanına katmak, etkileşimli olarak veya uzak masaüstü üzerinden oturum açmak ve yönetilen etki alanında kimlik doğrulaması yapmak için kullanabilir.
* **LDAP bağlama & LDAP okuma desteği:** Azure AD Domain Services tarafından hizmet verilen etki alanlarında kullanıcıların kimliğini doğrulamak için LDAP bağlamaları kullanan uygulamaları kullanabilirsiniz. Ayrıca, dizinden Kullanıcı/bilgisayar özniteliklerini sorgulamak için LDAP okuma işlemleri kullanan uygulamalar Azure AD Domain Services de çalışabilir.
* **Güvenli LDAP (LDAPS):** Güvenli LDAP (LDAPS) üzerinden dizine erişimi etkinleştirebilirsiniz. Güvenli LDAP erişim, sanal ağ içinde varsayılan olarak kullanılabilir. Ancak, isteğe bağlı olarak Internet üzerinden güvenli LDAP erişimini de etkinleştirebilirsiniz.
* **Grup İlkesi:** Kullanıcı hesapları ve etki alanına katılmış bilgisayarlar için gerekli güvenlik ilkeleriyle uyumluluğu zorlamak için, her biri Kullanıcı ve bilgisayar kapsayıcıları için tek bir yerleşik GPO kullanabilirsiniz. Ayrıca, kendi özel GPO 'larınızı oluşturabilir ve [Grup ilkesini yönetmek](manage-group-policy.md)için özel kuruluş birimlerine atayabilirsiniz.
* **DNS yönetme:** ' AAD DC Administrators ' grubunun üyeleri, DNS Yönetimi MMC ek bileşeni gibi tanıdık DNS yönetim araçlarını kullanarak, yönetilen etki alanınız için DNS 'i yönetebilir.
* **Özel kuruluş birimleri (OU) oluşturma:** ' AAD DC Administrators ' grubunun üyeleri, yönetilen etki alanında özel OU 'Lar oluşturabilir. Bu kullanıcılara özel OU 'Lar üzerinde tam yönetim ayrıcalıkları verilir, bu nedenle hizmet hesapları, bilgisayarlar, gruplar vb. bu özel OU 'Lar içinde eklenebilir/kaldırabilir.
* **Birçok Azure genel bölgesinde kullanılabilir:** Azure AD Domain Services'in kullanılabildiği Azure bölgelerini öğrenmek için [bölgeye göre Azure hizmetleri](https://azure.microsoft.com/regions/#services/) sayfasına bakın.
* **Yüksek kullanılabilirlik:** Azure AD Domain Services, etki alanınız için yüksek kullanılabilirlik sağlar. Bu özellik, daha yüksek hizmet çalışma süresi ve esnekliği hatalara karşı garanti sağlar. Yerleşik sistem durumu izleme, başarısız örnekleri değiştirmek ve etki alanınız için devam eden hizmeti sağlamak üzere yeni örnekler girerek hatalardan otomatik düzeltme sağlar.
* **AD hesabı kilitleme koruması:** 2 dakika içinde beş geçersiz parola kullanılırsa, Kullanıcı hesapları 30 dakika boyunca kilitlenir. Hesaplar 30 dakika sonra otomatik olarak açılır.
* **Tanıdık yönetim araçlarını kullanın:** Yönetilen etki alanlarını yönetmek için Active Directory Yönetim Merkezi veya Active Directory PowerShell gibi tanıdık Windows Server Active Directory yönetim araçlarını kullanabilirsiniz.
