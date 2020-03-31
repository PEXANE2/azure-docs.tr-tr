---
title: Bulut Çözüm Sağlayıcıları için Azure AD Etki Alanı Hizmetleri | Microsoft Dokümanlar
description: Azure Bulut Çözüm Sağlayıcıları için Azure Active Directory Etki Alanı Hizmetleri.
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: mahesh-unnikrishnan
editor: curtand
ms.assetid: 56ccb219-11b2-4e43-9f07-5a76e3cd8da8
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/08/2017
ms.author: iainfou
ms.openlocfilehash: 1134c078ee36a146cb1e1cbf8ca46f6cd9f8d775
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72754435"
---
# <a name="azure-active-directory-ad-domain-services-for-azure-cloud-solution-providers-csp"></a>Azure Bulut Çözüm Sağlayıcıları (CSP) için Azure Etkin Dizin (AD) Etki Alanı Hizmetleri
Bu makalede, Azure AD Etki Alanı Hizmetlerini bir Azure CSP aboneliğinde nasıl kullanabileceğiniz açıklanmaktadır.

## <a name="overview-of-azure-csp"></a>Azure CSP'ye Genel Bakış
Azure CSP, Microsoft İş Ortakları için bir programdır ve çeşitli Microsoft bulut hizmetleri için bir lisans kanalı sağlar. Azure CSP, iş ortaklarının satışları yönetmesine, faturalandırma ilişkisine sahip olmasını, teknik ve faturalandırma desteği sağlamasına ve müşterinin tek iletişim noktası olmasını sağlar. Buna ek olarak, Azure CSP self servis portalı ve beraberindeki API'ler de dahil olmak üzere tam bir araç seti sağlar. Bu araçlar, CSP iş ortaklarının Azure kaynaklarını kolayca sağlamasını ve yönetmesini ve müşteriler ve abonelikleri için faturalandırma sağlamasını sağlar.

[İş Ortağı Merkezi portalı,](https://docs.microsoft.com/azure/cloud-solution-provider/overview/partner-center-overview) tüm Azure CSP iş ortakları için bir giriş noktası görevi görür. Zengin müşteri yönetimi özellikleri, otomatik işleme ve daha fazlasını sağlar. Azure CSP iş ortakları, web tabanlı bir web hizmeti sürümü kullanarak veya PowerShell ve çeşitli API çağrılarını kullanarak İş Ortağı Merkezi özelliklerini kullanabilir.

Aşağıdaki diyagram, CSP modelinin yüksek düzeyde nasıl çalıştığını göstermektedir. Contoso'nun Azure AD Etkin Dizini vardır. Azure CSP aboneliklerinde kaynakları dağıtan ve yöneten bir CSP ile ortaklıkları vardır. Contoso'nun düzenli (doğrudan) Azure abonelikleri de olabilir ve bunlar doğrudan Contoso'ya faturalandırılır.

![CSP modeline genel bakış](./media/csp/csp_model_overview.png)

CSP ortağının kiracısının üç özel ajan grubu vardır : Yönetici ajanlar, Yardım Masası temsilcileri ve Satış temsilcileri. Yönetici aracıları grubu, Contoso'nun Azure REKLAM dizinindeki kiracı yönetici rolüne atanır. Sonuç olarak, CSP iş ortağının yönetici aracıları grubuna ait bir kullanıcı, Contoso'nun Azure REKLAM dizininde kiracı yönetici ayrıcalıklarına sahiptir. CSP iş ortağı Contoso için bir Azure CSP aboneliği verdiğinde, yönetici aracılar grubu bu abonelik için sahip rolüne atanır. Sonuç olarak, CSP iş ortağının yönetici aracıları, Contoso adına sanal makineler, sanal ağlar ve Azure AD Etki Alanı Hizmetleri gibi Azure kaynaklarını sağlamak için gerekli ayrıcalıklara sahiptir.

Daha fazla bilgi için [Azure CSP genel bakış](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview)

## <a name="benefits-of-using-azure-ad-domain-services-in-an-azure-csp-subscription"></a>Azure CSP aboneliğinde Azure AD Etki Alanı Hizmetlerini kullanmanın avantajları
Azure AD Etki Alanı Hizmetleri, LDAP, Kerberos/NTLM kimlik doğrulama, etki alanı birleştirme, grup ilkesi ve DNS gibi Azure'da Windows Server AD uyumlu hizmetler sağlar. On yıllar boyunca, birçok uygulama bu yetenekleri kullanarak AD karşı çalışmak için inşa edilmiştir. Birçok bağımsız yazılım satıcısı (ISV) müşterilerinin tesislerinde uygulamalar oluşturmuş ve dağıtmaktadır. Genellikle bu uygulamaların dağıtıldığı farklı ortamlara erişim gerektirdiğinden, bu uygulamaların desteklenmesi zor durumdadır. Azure CSP abonelikleri ile Azure'un ölçeği ve esnekliği yle daha basit bir alternatife sahip olabilirsiniz.

Azure AD Etki Alanı Hizmetleri artık Azure CSP aboneliklerini destekliyor. Artık uygulamanızı, müşterinizin Azure AD dizinine bağlı bir Azure CSP aboneliğinde dağıtabilirsiniz. Sonuç olarak, çalışanlarınız (destek personeli) kuruluşunuzun kurumsal kimlik bilgilerini kullanarak uygulamanızın dağıtıldığı sanal makineleri yönetebilir, yönetebilir ve hizmet verebilir. Ayrıca, müşterinizin Azure REKLAM dizini için Azure AD Etki Alanı Hizmetleri yönetilen bir etki alanı sağlayabilirsiniz. Uygulamanız müşterinizin yönetilen etki alanına bağlıdır. Bu nedenle, Uygulamanızdaki Kerberos/NTLM, LDAP veya [System.DirectoryServices API'ye](/dotnet/api/system.directoryservices) dayanan yetenekler, müşteri dizini yle sorunsuz bir şekilde çalışır. Son müşterileriniz, uygulamanın dağıtılan altyapıyı koruma konusunda endişelenmenize gerek kalmadan, uygulamanızı hizmet olarak tüketmekten büyük ölçüde yararlanır.

Azure AD Etki Alanı Hizmetleri de dahil olmak üzere, bu abonelikte tükettiğiniz Azure kaynaklarının tüm faturaları size geri yüklenir. Satış, faturalama, teknik destek vb. söz konusu olduğunda müşteriyle olan ilişki üzerinde tam kontrole sahip siniz. Azure CSP platformunun esnekliğiyle, küçük bir destek aracıekibi, uygulamanızın örnekleri dağıtılmış olan birçok müşteriye hizmet verebilir.


## <a name="csp-deployment-models-for-azure-ad-domain-services"></a>Azure AD Etki Alanı hizmetleri için CSP dağıtım modelleri
Azure CSP aboneliğiyle Azure AD Etki Alanı Hizmetlerini kullanmanın iki yolu vardır. Müşterilerinizin sahip olduğu güvenlik ve basitlik hususlarına göre doğru olanı seçin.

### <a name="direct-deployment-model"></a>Doğrudan dağıtım modeli
Bu dağıtım modelinde, Azure AD Etki Alanı Hizmetleri, Azure CSP aboneliğine ait sanal ağ içinde etkinleştirilir. CSP iş ortağının yönetici temsilcileri aşağıdaki ayrıcalıklara sahiptir:
* Müşterinin Azure REKLAM dizininde genel yönetici ayrıcalıkları.
* Azure CSP aboneliğinde abonelik sahibi ayrıcalıkları.

![Doğrudan dağıtım modeli](./media/csp/csp_direct_deployment_model.png)

Bu dağıtım modelinde, CSP sağlayıcısının yönetici aracıları müşteri için kimlikleri yönetebilir. Bu yönetici aracıları, müşterinin Azure REKLAM dizini vb. içinde yeni kullanıcılar, gruplar, uygulamalar ekleme olanağına sahiptir. Bu dağıtım modeli, özel bir kimlik yöneticisi olmayan veya CSP iş ortağının kendi adlarına kimlik ler yönetmesini tercih eden küçük kuruluşlar için uygun olabilir.


### <a name="peered-deployment-model"></a>Eşli dağıtım modeli
Bu dağıtım modelinde, Azure AD Etki Alanı Hizmetleri müşteriye ait bir sanal ağ da etkinleştirilir - yani müşteri tarafından ödenen doğrudan bir Azure aboneliği. CSP iş ortağı daha sonra uygulamaları müşterinin CSP aboneliğine ait sanal ağ içinde dağıtabilir. Sanal ağlar daha sonra Azure sanal ağ eşleme kullanılarak bağlanabilir. Sonuç olarak, Azure CSP aboneliğinde CSP iş ortağı tarafından dağıtılan iş yükleri/uygulamaları, müşterinin doğrudan Azure aboneliğinde sağlanan yönetilen etki alanına bağlanabilir.

![Eşli dağıtım modeli](./media/csp/csp_peered_deployment_model.png)

Bu dağıtım modeli ayrıcalıkların ayrılmasını sağlar ve CSP iş ortağının yardım masası aracılarının Azure aboneliğini yönetmesini ve içindeki kaynakları dağıtmasını ve yönetmesini sağlar. Ancak, CSP iş ortağının yardım masası aracılarının müşterinin Azure REKLAM dizininde genel yönetici ayrıcalıklarına sahip olması gerekmez. Müşterinin kimlik yöneticileri, kuruluşlarının kimliklerini yönetmeye devam edebilir.

Bu dağıtım modeli, bir ISV'nin (bağımsız yazılım satıcısı) şirket içi uygulamalarının barındırılan bir sürümünü sağladığı ve müşterinin AD'sine de bağlanması gereken senaryolar için uygun olabilir.


## <a name="administering-azure-ad-domain-services-managed-domains-in-csp-subscriptions"></a>Azure AD Etki Alanı Hizmetlerini Yönetme, CSP aboneliklerinde etki alanlarını yönet
Azure CSP aboneliğinde yönetilen bir etki alanını yönetirken aşağıdaki önemli hususlar geçerlidir:

* **CSP yönetici aracıları, kimlik bilgilerini kullanarak yönetilen bir etki alanı sağlayabilir:** Azure AD Etki Alanı Hizmetleri, Azure CSP aboneliklerini destekler. Bu nedenle, bir CSP iş ortağının yönetici aracıları grubuna ait kullanıcılar, yeni bir Azure AD Etki Alanı Hizmetleri yönetilen etki alanı sağlayabilir.

* **CSP'ler PowerShell'i kullanarak müşterileri için yeni yönetilen etki alanlarının oluşturulmasını komut dosyası olarak yazabilir:** Ayrıntılar için [PowerShell'i kullanarak Azure AD Etki Alanı Hizmetlerini nasıl etkinleştirin.](powershell-create-instance.md)

* **CSP yönetici aracıları, kimlik bilgilerini kullanarak yönetilen etki alanında devam eden yönetim görevlerini gerçekleştiremez:** CSP yönetici kullanıcıları, kimlik bilgilerini kullanarak yönetilen etki alanı içinde rutin yönetim görevlerini gerçekleştiremez. Bu kullanıcılar müşterinin Azure REKLAM dizininin dışındadır ve kimlik bilgileri müşterinin Azure REKLAM dizininde kullanılamaz. Bu nedenle, Azure AD Etki Alanı Hizmetleri bu kullanıcılar için Kerberos ve NTLM parola kalıplarına erişimi yoktur. Sonuç olarak, bu tür kullanıcılar Azure AD Etki Alanı Hizmetleri yönetilen etki alanlarında kimliği doğrulanamamaktadır.

  > [!WARNING]
  > **Yönetilen etki alanında devam eden yönetim görevlerini gerçekleştirmek için müşterinin dizininde bir kullanıcı hesabı oluşturmanız gerekir.**
  > CSP yönetici kullanıcıkimlik bilgilerini kullanarak yönetilen etki alanında oturum açamazsınız. Bunu yapmak için müşterinin Azure REKLAM dizinine ait bir kullanıcı hesabının kimlik bilgilerini kullanın. Sanal makineleri yönetilen etki alanına katılma, DNS'yi yönetme, Grup İlkesi vb. yönetme gibi görevler için bu kimlik bilgilerine ihtiyacınız vardır.
  >

* **Devam eden yönetim için oluşturulan kullanıcı hesabı 'AAD DC Yöneticileri' grubuna eklenmelidir:** 'AAD DC Yöneticileri' grubu, yönetilen etki alanında belirli temsilci yönetim görevlerini gerçekleştirmek için ayrıcalıklara sahiptir. Bu görevler dns yapılandırma, organizasyon birimleri oluşturma, grup ilkesi vb yönetme içerir. Bir CSP iş ortağının bu tür görevleri yönetilen bir etki alanında gerçekleştirebilmesi için müşterinin Azure AD dizininde bir kullanıcı hesabı oluşturulması gerekir. Bu hesabın kimlik bilgileri CSP iş ortağının yönetici temsilcileriyle paylaşılmalıdır. Ayrıca, yönetilen etki alanında yapılandırma görevlerinin bu kullanıcı hesabı kullanılarak gerçekleştirilebilmesi için bu kullanıcı hesabının 'AAD DC Yöneticileri' grubuna eklenmesi gerekir.


## <a name="next-steps"></a>Sonraki adımlar
* [Azure CSP programına kaydolun](https://docs.microsoft.com/partner-center/enrolling-in-the-csp-program) ve Azure CSP ile iş oluşturmaya başlayın.
* [Azure CSP'de bulunan Azure hizmetlerinin](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services)listesini inceleyin.
* [PowerShell kullanarak Azure AD Domain Services'ı etkinleştirme](powershell-create-instance.md)
* [Azure AD Etki Alanı Hizmetleri’ni kullanmaya başlama](tutorial-create-instance.md)
