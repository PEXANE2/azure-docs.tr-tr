---
title: Bulut Çözüm Sağlayıcıları için Azure AD Etki Alanı Hizmetleri | Microsoft Dokümanlar
description: Azure Bulut Çözüm Sağlayıcıları için Azure Active Directory Domain Services yönetilen etki alanlarını nasıl etkinleştirmenizi ve yöneteceklerini öğrenin
services: active-directory-ds
author: iainfoulds
ms.assetid: 56ccb219-11b2-4e43-9f07-5a76e3cd8da8
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 03/31/2020
ms.author: iainfou
ms.openlocfilehash: e7276dcfca6ba033942d62f347ac3a799524cac4
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80519100"
---
# <a name="azure-active-directory-domain-services-deployment-and-management-for-azure-cloud-solution-providers"></a>Azure Bulut Çözüm Sağlayıcıları için Azure Active Directory Etki Alanı Hizmetleri dağıtımı ve yönetimi

Azure Bulut Çözüm Sağlayıcıları (CSP), Microsoft İş Ortakları için bir programdır ve çeşitli Microsoft bulut hizmetleri için bir lisans kanalı sağlar. Azure CSP, iş ortaklarının satışları yönetmesine, faturalandırma ilişkisine sahip olmasını, teknik ve faturalandırma desteği sağlamasına ve müşterinin tek iletişim noktası olmasını sağlar. Buna ek olarak, Azure CSP self servis portalı ve beraberindeki API'ler de dahil olmak üzere tam bir araç seti sağlar. Bu araçlar, CSP iş ortaklarının Azure kaynaklarını kolayca sağlamasını ve yönetmesini ve müşteriler ve abonelikleri için faturalandırma sağlamasını sağlar.

[İş Ortağı Merkezi portalı,](https://docs.microsoft.com/azure/cloud-solution-provider/overview/partner-center-overview) tüm Azure CSP iş ortaklarının giriş noktasıdır ve zengin müşteri yönetimi özellikleri, otomatik işleme ve daha fazlasını sağlar. Azure CSP iş ortakları, web tabanlı bir web hizmeti sürümü kullanarak veya PowerShell ve çeşitli API çağrılarını kullanarak İş Ortağı Merkezi özelliklerini kullanabilir.

Aşağıdaki diyagram, CSP modelinin yüksek düzeyde nasıl çalıştığını göstermektedir. Burada, Contoso'nun bir Azure Etkin Dizin (Azure AD) kiracısı vardır. Azure CSP aboneliklerinde kaynakları dağıtan ve yöneten bir CSP ile ortaklıkları vardır. Contoso'nun düzenli (doğrudan) Azure abonelikleri de olabilir ve bunlar doğrudan Contoso'ya faturalandırılır.

![CSP modeline genel bakış](./media/csp/csp_model_overview.png)

CSP ortağının kiracısının üç özel ajan grubu vardır : *Yönetici* ajanlar, *Yardım Masası* temsilcileri ve *Satış* temsilcileri.

*Yönetici* aracıları grubu, Contoso'nun Azure AD kiracısında kiracı yönetici rolüne atanır. Sonuç olarak, CSP iş ortağının yönetici aracıları grubuna ait bir kullanıcı, Contoso'nun Azure AD kiracısında kiracı yönetici ayrıcalıklarına sahiptir.

CSP iş ortağı Contoso için bir Azure CSP aboneliği verdiğinde, yönetici aracılar grubu bu abonelik için sahip rolüne atanır. Sonuç olarak, CSP iş ortağının yönetici aracıları, Contoso adına sanal makineler, sanal ağlar ve Azure AD Etki Alanı Hizmetleri gibi Azure kaynaklarını sağlamak için gerekli ayrıcalıklara sahiptir.

Daha fazla bilgi için [Azure CSP genel bakış](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview)

## <a name="benefits-of-using-azure-ad-ds-in-an-azure-csp-subscription"></a>Azure CSP aboneliğinde Azure AD DS kullanmanın avantajları

Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS), Windows Server Active Directory Domain Services ile tam uyumlu etki alanı birleştirme, grup ilkesi, LDAP, Kerberos/NTLM kimlik doğrulaması gibi yönetilen etki alanı hizmetleri sağlar. On yıllar boyunca, birçok uygulama bu yetenekleri kullanarak AD karşı çalışmak için inşa edilmiştir. Birçok bağımsız yazılım satıcısı (ISV) müşterilerinin tesislerinde uygulamalar oluşturmuş ve dağıtmaktadır. Genellikle uygulamaların dağıtıldığı farklı ortamlara erişim gerektiren bu uygulamaları desteklemek zordur. Azure CSP abonelikleri ile Azure'un ölçeği ve esnekliği yle daha basit bir alternatife sahip olabilirsiniz.

Azure AD DS, Azure CSP aboneliklerini destekler. Uygulamanızı, müşterinizin Azure AD kiracısına bağlı bir Azure CSP aboneliğinde dağıtabilirsiniz. Sonuç olarak, çalışanlarınız (destek personeli) kuruluşunuzun kurumsal kimlik bilgilerini kullanarak uygulamanızın dağıtıldığı VM'leri yönetebilir, yönetebilir ve hizmet verebilir.

Ayrıca, müşterinizin Azure AD kiracısında Azure AD DS yönetilen bir etki alanı da dağıtabilirsiniz. Uygulamanız daha sonra müşterinizin yönetilen etki alanına bağlanır. Uygulamanızdaki Kerberos / NTLM, LDAP veya [System.DirectoryServices API'ye](/dotnet/api/system.directoryservices) dayanan yetenekler, müşteri nizamına karşı sorunsuz bir şekilde çalışır. Son müşteriler, uygulamanın dağıtılan altyapıyı koruma konusunda endişelenmenize gerek kalmadan, uygulamanızı hizmet olarak tüketmekten yararlanır.

Azure AD DS dahil olmak üzere bu abonelikte tükettiğiniz Azure kaynaklarının tüm faturaları size geri yüklenir. Satış, faturalama, teknik destek vb. söz konusu olduğunda müşteriyle olan ilişki üzerinde tam kontrole sahip siniz. Azure CSP platformunun esnekliğiyle, küçük bir destek aracıekibi, uygulamanızın örnekleri dağıtılmış olan birçok müşteriye hizmet verebilir.

## <a name="csp-deployment-models-for-azure-ad-ds"></a>Azure AD DS için CSP dağıtım modelleri

Azure CSP aboneliğiyle Azure AD DS'yi kullanmanın iki yolu vardır. Müşterilerinizin sahip olduğu güvenlik ve basitlik hususlarına göre doğru olanı seçin.

### <a name="direct-deployment-model"></a>Doğrudan dağıtım modeli

Bu dağıtım modelinde, Azure AD DS, Azure CSP aboneliğine ait sanal bir ağ içinde etkinleştirilir. CSP iş ortağının yönetici temsilcileri aşağıdaki ayrıcalıklara sahiptir:

* Müşterinin Azure AD kiracısında *genel yönetici* ayrıcalıkları.
* Azure CSP aboneliğinde *abonelik sahibi* ayrıcalıkları.

![Doğrudan dağıtım modeli](./media/csp/csp_direct_deployment_model.png)

Bu dağıtım modelinde, CSP sağlayıcısının yönetici aracıları müşteri için kimlikleri yönetebilir. Bu yönetici aracıları, yeni kullanıcılar veya gruplar sağlama gibi görevleri gerçekleştirebilir veya müşterinin Azure AD kiracısı içindeki uygulamaları ekleyebilir.

Bu dağıtım modeli, özel bir kimlik yöneticisi olmayan veya CSP iş ortağının kendi adlarına kimlik ler yönetmesini tercih eden küçük kuruluşlar için uygun olabilir.

### <a name="peered-deployment-model"></a>Eşli dağıtım modeli

Bu dağıtım modelinde, Azure AD DS müşteriye ait bir sanal ağ içinde etkinleştirilir - müşteri tarafından ödenen doğrudan bir Azure aboneliği. CSP iş ortağı, uygulamaları müşterinin CSP aboneliğine ait sanal ağ içinde dağıtabilir. Sanal ağlar daha sonra Azure sanal ağ eşleme kullanılarak bağlanabilir.

Bu dağıtımla birlikte, Azure CSP aboneliğinde CSP iş ortağı tarafından dağıtılan iş yükleri veya uygulamalar, müşterinin doğrudan Azure aboneliğinde sağlanan yönetilen etki alanına bağlanabilir.

![Eşli dağıtım modeli](./media/csp/csp_peered_deployment_model.png)

Bu dağıtım modeli ayrıcalıkların ayrılmasını sağlar ve CSP iş ortağının yardım masası aracılarının Azure aboneliğini yönetmesini ve içindeki kaynakları dağıtmasını ve yönetmesini sağlar. Ancak, CSP iş ortağının yardım masası aracılarının müşterinin Azure AD dizininde genel yönetici ayrıcalıklarına sahip olması gerekmez. Müşterinin kimlik yöneticileri, kuruluşlarının kimliklerini yönetmeye devam edebilir.

Bu dağıtım modeli, BIR ISV'nin şirket içi uygulamalarının barındırılan bir sürümünü sağladığı ve müşterinin Azure AD'sine de bağlanması gereken senaryolar için uygun olabilir.

## <a name="administer-azure-ad-ds-in-csp-subscriptions"></a>Azure AD DS'yi CSP aboneliklerinde yönetme

Azure CSP aboneliğinde yönetilen bir etki alanını yönetirken aşağıdaki önemli hususlar geçerlidir:

* **CSP yönetici aracıları, kimlik bilgilerini kullanarak yönetilen bir etki alanı sağlayabilir:** Azure AD DS, Azure CSP aboneliklerini destekler. Bir CSP iş ortağının yönetici aracıları grubuna ait kullanıcılar, yeni bir Azure AD DS yönetilen etki alanı sağlayabilir.

* **CSP'ler PowerShell'i kullanarak müşterileri için yeni yönetilen etki alanlarının oluşturulmasını komut dosyası olarak yazabilir:** Ayrıntılar için [PowerShell'i kullanarak Azure AD DS'yi nasıl etkinleştirin.](powershell-create-instance.md)

* **CSP yönetici aracıları, kimlik bilgilerini kullanarak yönetilen etki alanında devam eden yönetim görevlerini gerçekleştiremez:** CSP yönetici kullanıcıları, kimlik bilgilerini kullanarak yönetilen etki alanı içinde rutin yönetim görevlerini gerçekleştiremez. Bu kullanıcılar müşterinin Azure AD kiracısının dışındadır ve kimlik bilgileri müşterinin Azure AD kiracısı içinde kullanılamaz. Azure AD DS'nin bu kullanıcılar için Kerberos ve NTLM parola kalıplarına erişimi yoktur, bu nedenle kullanıcılar Azure AD DS yönetilen etki alanlarında kimliği doğrulanamamaktadır.

  > [!WARNING]
  > Yönetilen etki alanında devam eden yönetim görevlerini gerçekleştirmek için müşterinin dizininde bir kullanıcı hesabı oluşturmanız gerekir.
  >
  > CSP yönetici kullanıcıkimlik bilgilerini kullanarak yönetilen etki alanında oturum açamazsınız. Bunu yapmak için müşterinin Azure AD kiracısına ait bir kullanıcı hesabının kimlik bilgilerini kullanın. VM'leri yönetilen etki alanına katılma, DNS'yi yönetme veya Grup İlkesi'ni yönetme gibi görevler için bu kimlik bilgilerine ihtiyacınız vardır.

* **Devam eden yönetim için oluşturulan kullanıcı hesabı *AAD DC Yöneticileri* grubuna eklenmelidir:** *AAD DC Yöneticileri* grubu, yönetilen etki alanında belirli devredilen yönetim görevlerini gerçekleştirmek için ayrıcalıklara sahiptir. Bu görevler, DNS'yi yapılandırmayı, kuruluş birimleri oluşturmayı ve grup ilkesini yönetmeyi içerir.
    
    Bir CSP iş ortağının bu görevleri yönetilen bir etki alanında gerçekleştirebilmek için müşterinin Azure AD kiracısı içinde bir kullanıcı hesabı oluşturulması gerekir. Bu hesabın kimlik bilgileri CSP iş ortağının yönetici temsilcileriyle paylaşılmalıdır. Ayrıca, yönetilen etki alanında yapılandırma görevlerinin bu kullanıcı hesabı kullanılarak gerçekleştirilebilmesi için bu kullanıcı hesabının *AAD DC Yöneticileri* grubuna eklenmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için [Azure CSP programına kaydolun.](/partner-center/enrolling-in-the-csp-program) Daha sonra [Azure portalını](tutorial-create-instance.md) veya Azure [PowerShell'i](powershell-create-instance.md)kullanarak Azure AD Etki Alanı Hizmetlerini etkinleştirebilirsiniz.
