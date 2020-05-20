---
title: Azure Otomasyonu hesabı kimlik doğrulamasına genel bakış
description: Bu makalede, Otomasyon hesapları için Azure Otomasyonu güvenlik ve kimlik doğrulama yöntemlerine bir genel bakış sunulmaktadır.
keywords: otomasyon güvenliği, güvenli otomasyon; otomasyon kimlik doğrulaması
services: automation
ms.subservice: process-automation
ms.date: 04/23/2020
ms.topic: conceptual
ms.openlocfilehash: c9e3c2b15b45eb15b40782a5d0ecfe1e736fc013
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83683528"
---
# <a name="azure-automation-account-authentication-overview"></a>Azure Otomasyonu hesabı kimlik doğrulamasına genel bakış

Azure Automation, Azure’deki, şirket içindeki kaynaklara karşı ve Amazon Web Hizmetleri (AWS) gibi diğer bulut sağlayıcılarıyla görevleri otomatikleştirmenizi sağlar. Bir runbook 'un gerekli eylemlerini gerçekleştirmesi için, abonelikte gereken en düşük haklara sahip kaynaklara güvenli bir şekilde erişmek için gerekli izinlere sahip olması gerekir.

Azure Automation’u ilk kez başlattığınızda, en az bir Automation hesabı oluşturmanız gerekir. Automation hesapları Automation kaynaklarınızı (runbook'lar, varlıklar, yapılandırmalar) diğer Automation hesaplarında yer alan kaynaklarından yalıtmanızı sağlar. Kaynaklarını ayrı mantıksal ortamlara ayırmak için Automation hesaplarını kullanabilirsiniz. Örneğin, geliştirme için bir hesap, üretim için başka bir hesap ve şirket içi ortamınız için de başka bir hesap kullanabilirsiniz. Azure Automation hesabı, Azure aboneliğinizde oluşturduğunuz Microsoft hesabı veya hesaplarından farklıdır.

Her Otomasyon hesabı için Otomasyon kaynakları tek bir Azure bölgesiyle ilişkilendirilir, ancak Otomasyon hesapları aboneliğinizdeki tüm kaynakları yönetebilir. Farklı bölgelerde Automation hesapları oluşturmanın temel nedeni, veri ve kaynakların belirli bir bölgede yalıtılmasını gerektiren ilkelere sahip olmanız olabilir.

Azure Automation 'daki Azure Resource Manager ve Azure cmdlet 'lerini kullanarak kaynaklara karşı gerçekleştirdiğiniz tüm görevlerin, Azure Active Directory (Azure AD) kuruluş kimliği kimlik bilgisi tabanlı kimlik doğrulaması kullanarak Azure 'da kimlik doğrulaması yapması gerekir. Azure Otomasyonu 'nda farklı çalıştır hesapları Azure cmdlet 'lerini kullanarak Azure 'da kaynak yönetimine yönelik kimlik doğrulaması sağlar. Farklı Çalıştır hesabı oluşturduğunuzda, Azure AD 'de yeni bir hizmet sorumlusu kullanıcısı oluşturur ve bu kullanıcıya abonelik düzeyinde katılımcı rolü atar. Azure sanal makinelerinde karma runbook çalışanları kullanan runbook 'lar için, Azure kaynaklarınızın kimliğini doğrulamak üzere farklı çalıştır hesapları yerine [yönetilen kimliklerle runbook kimlik doğrulamasını](automation-hrw-run-runbooks.md#runbook-auth-managed-identities) kullanabilirsiniz.

Farklı Çalıştır hesabı için hizmet sorumlusu varsayılan olarak Azure AD 'yi okuma izinlerine sahip değildir. Azure AD 'yi okuma veya yönetme izinleri eklemek istiyorsanız, **API izinleri**altında hizmet sorumlusu için izinleri vermeniz gerekir. Daha fazla bilgi için bkz. [Web API 'lerine erişim Izinleri ekleme](../active-directory/develop/quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

Rol tabanlı erişim denetimi, Azure AD kullanıcı hesabı ve Farklı Çalıştır hesabına izin verilen eylemleri vermek, ve bu hizmet sorumlusunun kimliğini doğrulamak için Azure Resource Manager ile kullanılabilir. Otomasyon izinlerinin yönetilmesi için modelinizin geliştirilmesine yardımcı olma hakkında daha fazla bilgi için [Azure Otomasyonu’nda rol tabanlı erişim denetimi](automation-role-based-access-control.md) makalesini okuyun.  

Veri merkezinizdeki karma runbook çalışanı üzerinde çalışan runbook 'larda veya AWS gibi diğer bulut ortamlarında bilgi işlem hizmetlerine karşı, genellikle Azure kaynaklarında kimlik doğrulaması yapan runbook 'lar için kullanılan yöntemi kullanamaz. Bunun nedeni, bu kaynakların Azure dışında çalışmasıdır; sonuç olarak da, yerel olarak erişecekleri kaynakların kimliğini doğrulamak için Otomasyon'da tanımlanan kendi güvenlik kimlik bilgileri gerekecektir. Runbook çalışanları ile runbook kimlik doğrulaması hakkında daha fazla bilgi için bkz. [karma runbook çalışanları için Runbook 'ları](automation-hrw-run-runbooks.md)doğrulama. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Portal bir Otomasyon hesabı oluşturun](automation-create-standalone-account.md).
* [Azure Resource Manager şablonu kullanarak bir Otomasyon hesabı oluşturun](automation-create-account-template.md).
* [Amazon Web Services (AWS) Ile kimlik doğrulaması](automation-config-aws-account.md)yapın.
