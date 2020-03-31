---
title: Azure AD Etki Alanı Hizmetleri hakkında sık sorulan sorular | Microsoft Dokümanlar
description: Azure Etkin Dizin Etki Alanı Hizmetleri için yapılandırma, yönetim ve kullanılabilirlik ile ilgili sık sorulan sorulardan bazılarını okuyun ve anlayın
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 48731820-9e8c-4ec2-95e8-83dba1e58775
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 03/09/2020
ms.author: iainfou
ms.openlocfilehash: a57826c79babded6e616548879a5ec0c223307d0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78946429"
---
# <a name="frequently-asked-questions-faqs"></a>Sık sorulan sorular (SSS)

Bu sayfa, Azure Active Directory Domain Services hakkında sık sorulan soruları yanıtlar.

## <a name="configuration"></a>Yapılandırma

* [Tek bir Azure REKLAM dizini için birden çok yönetilen etki alanı oluşturabilir miyim?](#can-i-create-multiple-managed-domains-for-a-single-azure-ad-directory)
* [Azure AD Etki Alanı Hizmetlerini Klasik sanal ağda etkinleştirebilir miyim?](#can-i-enable-azure-ad-domain-services-in-a-classic-virtual-network)
* [Azure Kaynak Yöneticisi sanal ağında Azure AD Etki Alanı Hizmetlerini etkinleştirebilir miyim?](#can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network)
* [Varolan yönetilen etki alanımı klasik bir sanal ağdan Kaynak Yöneticisi sanal ağına geçirtebilir miyim?](#can-i-migrate-my-existing-managed-domain-from-a-classic-virtual-network-to-a-resource-manager-virtual-network)
* [Azure CSP (Bulut Çözüm Sağlayıcısı) aboneliğinde Azure AD Etki Alanı Hizmetlerini etkinleştirebilir miyim?](#can-i-enable-azure-ad-domain-services-in-an-azure-csp-cloud-solution-provider-subscription)
* [Azure AD Etki Alanı Hizmetlerini federe bir Azure AD dizininde etkinleştirebilir miyim? Parola hashelerini Azure AD ile senkronize etmeyim. Bu dizin için Azure AD Etki Alanı Hizmetlerini etkinleştirebilir miyim?](#can-i-enable-azure-ad-domain-services-in-a-federated-azure-ad-directory-i-do-not-synchronize-password-hashes-to-azure-ad-can-i-enable-azure-ad-domain-services-for-this-directory)
* [Azure AD Etki Alanı Hizmetlerini aboneliğim deki birden çok sanal ağda kullanılabilir hale getirebilir miyim?](#can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription)
* [PowerShell'i kullanarak Azure AD Etki Alanı Hizmetlerini etkinleştirebilir miyim?](#can-i-enable-azure-ad-domain-services-using-powershell)
* [Kaynak Yöneticisi Şablonu kullanarak Azure AD Etki Alanı Hizmetlerini etkinleştirebilir miyim?](#can-i-enable-azure-ad-domain-services-using-a-resource-manager-template)
* [Azure AD Etki Alanı Hizmetleri yönetilen etki alanına etki alanı denetleyicileri ekleyebilir miyim?](#can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain)
* [Dizine davet edilen konuk kullanıcılar Azure AD Etki Alanı Hizmetlerini kullanabilir mi?](#can-guest-users-invited-to-my-directory-use-azure-ad-domain-services)
* [Varolan bir Azure AD Etki Alanı Hizmetleri yönetilen etki alanını farklı bir aboneye, kaynak grubuna, bölgeye veya sanal ağa taşıyabilir miyim?](#can-i-move-an-existing-azure-ad-domain-services-managed-domain-to-a-different-subscription-resource-group-region-or-virtual-network)
* [Azure AD Etki Alanı Hizmetleri yüksek kullanılabilirlik seçenekleri içeriyor mu?](#does-azure-ad-domain-services-include-high-availability-options)

### <a name="can-i-create-multiple-managed-domains-for-a-single-azure-ad-directory"></a>Tek bir Azure REKLAM dizini için birden çok yönetilen etki alanı oluşturabilir miyim?
Hayır. Yalnızca tek bir Azure REKLAM dizini için Azure AD Etki Alanı Hizmetleri tarafından hizmet edilen tek bir yönetilen etki alanı oluşturabilirsiniz.

### <a name="can-i-enable-azure-ad-domain-services-in-a-classic-virtual-network"></a>Azure AD Etki Alanı Hizmetlerini Klasik sanal ağda etkinleştirebilir miyim?
Klasik sanal ağlar yeni dağıtımlar için desteklenmez. Klasik sanal ağlarda dağıtılan mevcut yönetilen etki alanları, 1 Mart 2023'te kullanımdan kaldırılınana kadar desteklenmeye devam eder. Varolan dağıtımlar için [Azure AD Etki Alanı Hizmetlerini Klasik sanal ağ modelinden Kaynak Yöneticisi'ne geçirebilirsiniz.](migrate-from-classic-vnet.md)

Daha fazla bilgi için [resmi amortisman bildirimine](https://azure.microsoft.com/updates/we-are-retiring-azure-ad-domain-services-classic-vnet-support-on-march-1-2023/)bakın.

### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network"></a>Azure Kaynak Yöneticisi sanal ağında Azure AD Etki Alanı Hizmetlerini etkinleştirebilir miyim?
Evet. Azure AD Etki Alanı Hizmetleri, bir Azure Kaynak Yöneticisi sanal ağında etkinleştirilebilir. Yönetilen bir etki alanı oluşturduğunuzda klasik Azure sanal ağları artık kullanılamaz.

### <a name="can-i-migrate-my-existing-managed-domain-from-a-classic-virtual-network-to-a-resource-manager-virtual-network"></a>Varolan yönetilen etki alanımı Klasik sanal ağdan Kaynak Yöneticisi sanal ağına geçirtebilir miyim?
Evet. Daha fazla bilgi için [bkz.](migrate-from-classic-vnet.md)

### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-csp-cloud-solution-provider-subscription"></a>Azure CSP (Bulut Çözüm Sağlayıcısı) aboneliğinde Azure AD Etki Alanı Hizmetlerini etkinleştirebilir miyim?
Evet. Daha fazla bilgi için Azure [CSP aboneliklerinde Azure AD Etki Alanı Hizmetlerini nasıl etkinleştirebilirsiniz'](csp.md)e bakın.

### <a name="can-i-enable-azure-ad-domain-services-in-a-federated-azure-ad-directory-i-do-not-synchronize-password-hashes-to-azure-ad-can-i-enable-azure-ad-domain-services-for-this-directory"></a>Azure AD Etki Alanı Hizmetlerini federe bir Azure AD dizininde etkinleştirebilir miyim? Parola hashelerini Azure AD ile senkronize etmeyim. Bu dizin için Azure AD Etki Alanı Hizmetlerini etkinleştirebilir miyim?
Hayır. Kullanıcıların Kimliğini NTLM veya Kerberos üzerinden doğrulamak için Azure AD Etki Alanı Hizmetlerinin kullanıcı hesaplarının parola hatalarına erişmesi gerekir. Federe bir dizinde parola varoşları Azure AD dizininde depolanır. Bu nedenle, Azure AD Etki Alanı Hizmetleri bu tür Azure REKLAM dizinleriyle çalışmaz.

### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription"></a>Azure AD Etki Alanı Hizmetlerini aboneliğim deki birden çok sanal ağda kullanılabilir hale getirebilir miyim?
Hizmetin kendisi bu senaryoyu doğrudan desteklemez. Yönetilen etki alanınız aynı anda yalnızca bir sanal ağda kullanılabilir. Ancak, Azure AD Etki Alanı Hizmetlerini diğer sanal ağlara maruz bırakmak için birden çok sanal ağ arasındaki bağlantıyı yapılandırabilirsiniz. Daha fazla bilgi için VPN ağ geçitlerini veya [sanal ağ eşlemeyi](../virtual-network/virtual-network-peering-overview.md) [kullanarak Azure'daki sanal ağları nasıl](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) bağlayabilirsiniz.

### <a name="can-i-enable-azure-ad-domain-services-using-powershell"></a>PowerShell'i kullanarak Azure AD Etki Alanı Hizmetlerini etkinleştirebilir miyim?
Evet. Daha fazla bilgi için [PowerShell'i kullanarak Azure AD Etki Alanı Hizmetlerini nasıl etkinleştirin.](powershell-create-instance.md)

### <a name="can-i-enable-azure-ad-domain-services-using-a-resource-manager-template"></a>Kaynak Yöneticisi Şablonu kullanarak Azure AD Etki Alanı Hizmetlerini etkinleştirebilir miyim?
Evet, Kaynak Yöneticisi şablonu kullanarak Azure AD Etki Alanı Hizmetleri yönetilen bir etki alanı oluşturabilirsiniz. Şablon dağıtılmadan önce Azure portalı veya Azure PowerShell kullanılarak bir hizmet sorumlusu ve yönetim için Azure AD grubu oluşturulmalıdır. Daha fazla bilgi için [bkz.](template-create-instance.md) Azure portalında Azure AD Etki Alanı Hizmetleri yönetilen bir etki alanı oluşturduğunuzda, şablonu ek dağıtımlarla kullanmak üzere dışa aktarma seçeneği de vardır.

### <a name="can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain"></a>Azure AD Etki Alanı Hizmetleri yönetilen etki alanına etki alanı denetleyicileri ekleyebilir miyim?
Hayır. Azure AD Etki Alanı Hizmetleri tarafından sağlanan etki alanı yönetilen bir etki alanıdır. Bu etki alanı için etki alanı denetleyicilerini sağlamanız, yapılandırmanız veya başka bir şekilde yönetmeniz gerekmez. Bu yönetim etkinlikleri Microsoft tarafından bir hizmet olarak sağlanır. Bu nedenle, yönetilen etki alanı için ek etki alanı denetleyicileri (okuma-yazma veya salt okunur) ekemezsiniz.

### <a name="can-guest-users-invited-to-my-directory-use-azure-ad-domain-services"></a>Dizine davet edilen konuk kullanıcılar Azure AD Etki Alanı Hizmetlerini kullanabilir mi?
Hayır. [Azure AD B2B](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md) davet işlemini kullanarak Azure REKLAM dizininize davet edilen konuk kullanıcılar, Azure AD Etki Alanı Hizmetleri yönetilen etki alanınızda senkronize edilir. Ancak, bu kullanıcıların parolaları Azure AD dizininizde depolanır. Bu nedenle, Azure AD Etki Alanı Hizmetleri'nin bu kullanıcılar için NTLM ve Kerberos karnelerini yönetilen etki alanınızda eşitleme nin hiçbir yolu yoktur. Bu tür kullanıcılar yönetilen etki alanında oturum açamaz veya bilgisayarlara katilemez.

### <a name="can-i-move-an-existing-azure-ad-domain-services-managed-domain-to-a-different-subscription-resource-group-region-or-virtual-network"></a>Varolan bir Azure AD Etki Alanı Hizmetleri yönetilen etki alanını farklı bir aboneye, kaynak grubuna, bölgeye veya sanal ağa taşıyabilir miyim?
Hayır. Azure AD Etki Alanı Hizmetleri yönetilen bir etki alanı oluşturduktan sonra, örneği farklı bir kaynak grubuna, sanal ağa, aboneye vb. taşıyamazsınız. Azure AD DS örneğini dağıttığınızda en uygun aboneliği, kaynak grubunu, bölgeyi ve sanal ağı seçmeye özen.

### <a name="does-azure-ad-domain-services-include-high-availability-options"></a>Azure AD Etki Alanı Hizmetleri yüksek kullanılabilirlik seçenekleri içeriyor mu?

Evet. Her Azure AD Etki Alanı Hizmetleri yönetilen etki alanı iki etki alanı denetleyicileri içerir. Bu etki alanı denetleyicilerini yönetmiyor veya bunlara bağlanmaz, bunlar yönetilen hizmetin bir parçasıdır. Azure AD Etki Alanı Hizmetlerini Kullanılabilirlik Bölgelerini destekleyen bir bölgeye dağıtırsanız, etki alanı denetleyicileri bölgelere dağıtılır. Kullanılabilirlik Bölgelerini desteklemeyen bölgelerde, etki alanı denetleyicileri Kullanılabilirlik Kümeleri arasında dağıtılır. Bu dağıtım üzerinde yapılandırma seçeneğiniz veya yönetim denetiminiz yoktur. Daha fazla bilgi için [Azure'daki sanal makinelerin Kullanılabilirlik seçeneklerine](../virtual-machines/windows/availability.md)bakın.

## <a name="administration-and-operations"></a>Yönetim ve operasyonlar

* [Uzak Masaüstü'nü kullanarak yönetilen etki alanım için etki alanı denetleyicisine bağlanabilir miyim?](#can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop)
* [Azure AD Etki Alanı Hizmetlerini etkinleştirdim. Bu etki alanına katılma makineleri için hangi kullanıcı hesabını kullanırım?](#ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-to-domain-join-machines-to-this-domain)
* [Azure AD Etki Alanı Hizmetleri tarafından sağlanan yönetilen etki alanı için etki alanı yöneticisi ayrıcalıklarım var mı?](#do-i-have-domain-administrator-privileges-for-the-managed-domain-provided-by-azure-ad-domain-services)
* [Yönetilen etki alanlarındaLDAP veya diğer AD yönetim araçlarını kullanarak grup üyeliklerini değiştirebilir miyim?](#can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-managed-domains)
* [Azure REKLAM dizinimde yaptığım değişikliklerin yönetilen etki alanımda görünür olması ne kadar sürer?](#how-long-does-it-take-for-changes-i-make-to-my-azure-ad-directory-to-be-visible-in-my-managed-domain)
* [Azure AD Etki Alanı Hizmetleri tarafından sağlanan yönetilen etki alanının şemalarını genişletebilir miyim?](#can-i-extend-the-schema-of-the-managed-domain-provided-by-azure-ad-domain-services)
* [Yönetilen etki alanımdaki DNS kayıtlarını değiştirebilir veya ekleyebilir miyim?](#can-i-modify-or-add-dns-records-in-my-managed-domain)
* [Yönetilen bir etki alanında parola yaşam boyu ilkesi nedir?](#what-is-the-password-lifetime-policy-on-a-managed-domain)
* [Azure AD Etki Alanı Hizmetleri AD hesap kilitleme koruması sağlıyor mu?](#does-azure-ad-domain-services-provide-ad-account-lockout-protection)

### <a name="can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop"></a>Uzak Masaüstü'nü kullanarak yönetilen etki alanım için etki alanı denetleyicisine bağlanabilir miyim?
Hayır. Uzak Masaüstü'nü kullanarak yönetilen etki alanı için etki alanı denetleyicilerine bağlanma izniniz yoktur. *AAD DC Yöneticileri* grubunun üyeleri, yönetilen etki alanını Active Directory Administration Center (ADAC) veya AD PowerShell gibi AD yönetim araçlarını kullanarak yönetebilir. Bu araçlar, yönetilen etki alanına katılan bir Windows sunucusundaki *Uzak Sunucu Yönetim Araçları* özelliği kullanılarak yüklenir. Daha fazla bilgi için, [Azure AD Etki Alanı Hizmetleri yönetilen bir etki alanını yapılandırmak ve yönetmek için bir yönetim VM'si oluştur'a](tutorial-create-management-vm.md)bakın.

### <a name="ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-to-domain-join-machines-to-this-domain"></a>Azure AD Etki Alanı Hizmetlerini etkinleştirdim. Bu etki alanına katılma makineleri için hangi kullanıcı hesabını kullanırım?
Azure AD DS yönetilen etki alanının bir parçası olan herhangi bir kullanıcı hesabı bir VM'ye katılabilir. *AAD DC Yöneticileri* grubunun üyelerine yönetilen etki alanına birleştirilmiş makinelere uzak masaüstü erişimi verilir.

### <a name="do-i-have-domain-administrator-privileges-for-the-managed-domain-provided-by-azure-ad-domain-services"></a>Azure AD Etki Alanı Hizmetleri tarafından sağlanan yönetilen etki alanı için etki alanı yöneticisi ayrıcalıklarım var mı?
Hayır. Yönetilen etki alanında size yönetim ayrıcalıkları verilmez. *Etki Alanı Yöneticisi* ve *Kurumsal Yönetici* ayrıcalıkları etki alanı içinde kullanmanız için kullanılamaz. Şirket içi Active Directory'nizdeki etki alanı yöneticisi veya kurumsal yönetici gruplarının üyelerine, yönetilen etki alanında etki alanı/ kurumsal yönetici ayrıcalıkları da verilmez.

### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-managed-domains"></a>Yönetilen etki alanlarındaLDAP veya diğer AD yönetim araçlarını kullanarak grup üyeliklerini değiştirebilir miyim?
Azure Etkin Dizin'den Azure AD Etki Alanı Hizmetlerine eşitlenen kullanıcılar ve gruplar, kaynak kaynakları Azure Active Directory olduğundan değiştirilemez. Yönetilen etki alanından kaynaklanan herhangi bir kullanıcı veya grup değiştirilebilir.

### <a name="how-long-does-it-take-for-changes-i-make-to-my-azure-ad-directory-to-be-visible-in-my-managed-domain"></a>Azure REKLAM dizinimde yaptığım değişikliklerin yönetilen etki alanımda görünür olması ne kadar sürer?
Azure REKLAM UI veya PowerShell kullanarak Azure REKLAM dizininizde yapılan değişiklikler yönetilen etki alanınızla otomatik olarak senkronize edilir. Bu eşitleme işlemi arka planda çalışır. Bu eşitlemenin tüm nesne değişikliklerini tamamlaması için tanımlanmış bir zaman dilimi yoktur.

### <a name="can-i-extend-the-schema-of-the-managed-domain-provided-by-azure-ad-domain-services"></a>Azure AD Etki Alanı Hizmetleri tarafından sağlanan yönetilen etki alanının şemalarını genişletebilir miyim?
Hayır. Şema, yönetilen etki alanı için Microsoft tarafından yönetilir. Şema uzantıları Azure AD Etki Alanı Hizmetleri tarafından desteklenmez.

### <a name="can-i-modify-or-add-dns-records-in-my-managed-domain"></a>Yönetilen etki alanımdaki DNS kayıtlarını değiştirebilir veya ekleyebilir miyim?
Evet. *AAD DC Yöneticileri* grubunun üyelerine, yönetilen etki alanında DNS kayıtlarını değiştirmek için *DNS Yöneticisi* ayrıcalıkları verilir. Bu kullanıcılar, DNS'yi yönetmek için yönetilen etki alanına katılan Windows Server çalıştıran bir makinede DNS Manager konsolu'nu kullanabilir. DNS Manager konsolu kullanmak için, sunucudaki Uzak Sunucu *Yönetim Araçları'nın* bir parçası olan *DNS Server Tools'u*yükleyin. Daha fazla bilgi için bkz: [Azure AD Etki Alanı Hizmetleri yönetilen etki alanında DNS'yi yönet.](manage-dns.md)

### <a name="what-is-the-password-lifetime-policy-on-a-managed-domain"></a>Yönetilen bir etki alanında parola yaşam boyu ilkesi nedir?
Azure AD Etki Alanı Hizmetleri yönetilen etki alanında varsayılan parola ömrü 90 gündür. Bu parola ömrü, Azure AD'de yapılandırılan parola ömrüyle eşitlenmez. Bu nedenle, kullanıcıların parolalarının yönetilen etki alanınızda süresinin dolduğu, ancak Azure AD'de hala geçerli olduğu bir durum la karşılaşabilirsiniz. Bu tür senaryolarda, kullanıcıların Azure AD'de parolalarını değiştirmeleri gerekir ve yeni parola yönetilen etki alanınızla eşitlenir. Ayrıca, kullanıcı hesapları için *parola-yapmaz-süresi dolmaz* ve *kullanıcı-değiştirme-parola-at-next-logon* öznitelikleri yönetilen etki alanı ile senkronize edilmez.

### <a name="does-azure-ad-domain-services-provide-ad-account-lockout-protection"></a>Azure AD Etki Alanı Hizmetleri AD hesap kilitleme koruması sağlıyor mu?
Evet. Yönetilen etki alanında 2 dakika içinde beş geçersiz parola denemesi, bir kullanıcı hesabının 30 dakika boyunca kilitlenmiş olmasınedeniyle. 30 dakika sonra, kullanıcı hesabı otomatik olarak açılır. Yönetilen etki alanında geçersiz parola denemeleri, Azure AD'deki kullanıcı hesabını kilitlemez. Kullanıcı hesabı yalnızca Azure AD Etki Alanı Hizmetleri yönetilen etki alanınızda kilitlenir. Daha fazla bilgi için, [yönetilen etki alanlarında parola ve hesap kilitleme ilkelerine](password-policy.md)bakın.

## <a name="billing-and-availability"></a>Faturalandırma ve kullanılabilirlik

* [Azure AD Etki Alanı Hizmetleri ücretli bir hizmet midir?](#is-azure-ad-domain-services-a-paid-service)
* [Hizmet için ücretsiz deneme var mı?](#is-there-a-free-trial-for-the-service)
* [Azure AD Etki Alanı Hizmetleri yönetilen etki alanını duraklatabilir miyim?](#can-i-pause-an-azure-ad-domain-services-managed-domain)
* [Bir DR etkinliği için Azure AD Etki Alanı Hizmetlerini başka bir bölgeye devredebilir miyim?](#can-i-pause-an-azure-ad-domain-services-managed-domain)
* [Kurumsal Mobilite Paketi'nin (EMS) bir parçası olarak Azure AD Etki Alanı Hizmetleri alabilir miyim? Azure AD Etki Alanı Hizmetlerini kullanmak için Azure AD Premium'a ihtiyacım var mı?](#can-i-failover-azure-ad-domain-services-to-another-region-for-a-dr-event)
* [Hizmet hangi Azure bölgelerinde kullanılabilir?](#can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems-do-i-need-azure-ad-premium-to-use-azure-ad-domain-services)

### <a name="is-azure-ad-domain-services-a-paid-service"></a>Azure AD Etki Alanı Hizmetleri ücretli bir hizmet midir?
Evet. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/active-directory-ds/).

### <a name="is-there-a-free-trial-for-the-service"></a>Hizmet için ücretsiz deneme var mı?
Azure AD Etki Alanı Hizmetleri, Azure için ücretsiz deneme sürümüne dahildir. Ücretsiz bir aylık [Azure denemesi](https://azure.microsoft.com/pricing/free-trial/)için kaydolabilirsiniz.

### <a name="can-i-pause-an-azure-ad-domain-services-managed-domain"></a>Azure AD Etki Alanı Hizmetleri yönetilen etki alanını duraklatabilir miyim?
Hayır. Azure AD Etki Alanı Hizmetleri yönetilen bir etki alanını etkinleştirdikten sonra, yönetilen etki alanını silene kadar hizmet seçili sanal ağınızda kullanılabilir. Hizmeti durdurmanın bir yolu yok. Faturalandırma, yönetilen etki alanını silene kadar saatlik olarak devam ediyor.

### <a name="can-i-failover-azure-ad-domain-services-to-another-region-for-a-dr-event"></a>Bir DR etkinliği için Azure AD Etki Alanı Hizmetlerini başka bir bölgeye devredebilir miyim?
Hayır. Azure AD Etki Alanı Hizmetleri şu anda coğrafi yedekli bir dağıtım modeli sağlamaz. Azure bölgesindeki tek bir sanal ağla sınırlıdır. Birden çok Azure bölgesi kullanmak istiyorsanız, Etkin Dizin Etki Alanı Denetleyicilerinizi Azure IaaS VM'lerinde çalıştırmanız gerekir. Mimari kılavuzu için bkz. Şirket [içi Active Directory etki alanınızı Azure'a genişletin.](https://docs.microsoft.com/azure/architecture/reference-architectures/identity/adds-extend-domain)

### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems-do-i-need-azure-ad-premium-to-use-azure-ad-domain-services"></a>Kurumsal Mobilite Paketi'nin (EMS) bir parçası olarak Azure AD Etki Alanı Hizmetleri alabilir miyim? Azure AD Etki Alanı Hizmetlerini kullanmak için Azure AD Premium'a ihtiyacım var mı?
Hayır. Azure AD Etki Alanı Hizmetleri, istediğiniz kadar öde Azure hizmetidir ve EMS'nin bir parçası değildir. Azure AD Etki Alanı Hizmetleri, Azure AD'nin tüm sürümlerinde (Ücretsiz ve Premium) kullanılabilir. Kullanıma bağlı olarak saatlik olarak faturalandırılırsınız.

### <a name="what-azure-regions-is-the-service-available-in"></a>Hizmet hangi Azure bölgelerinde kullanılabilir?
Azure AD Etki Alanı Hizmetlerinin kullanılabildiği Azure bölgelerinin listesini görmek için [bölgeye göre Azure Hizmetleri](https://azure.microsoft.com/regions/#services/) sayfasına bakın.

## <a name="troubleshooting"></a>Sorun giderme

Azure AD Etki Alanı Hizmetlerini yapılandırma veya yönetme yle ilgili sık karşılaşılan sorunların çözümleri için [Sorun Giderme kılavuzuna](troubleshoot.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD Etki Alanı Hizmetleri hakkında daha fazla bilgi edinmek için Azure [Etkin Dizin Etki Alanı Hizmetleri nedir?](overview.md)

Başlamak için azure [etkin dizin etki alanı hizmetleri örneği oluştur ve yapılandırma](tutorial-create-instance.md)kınızda.
