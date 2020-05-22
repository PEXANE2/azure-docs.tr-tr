---
title: Azure Otomasyonu hesabı kimlik doğrulamasına genel bakış
description: Bu makalede, Azure Otomasyonu hesabı kimlik doğrulamasına genel bakış sunulmaktadır.
keywords: otomasyon güvenliği, güvenli otomasyon; otomasyon kimlik doğrulaması
services: automation
ms.subservice: process-automation
ms.date: 04/23/2020
ms.topic: conceptual
ms.openlocfilehash: f78161590c897e6337e8694acb9f8a6b34258246
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83743813"
---
# <a name="automation-account-authentication-overview"></a>Otomasyon hesabı kimlik doğrulamasına genel bakış

Azure Automation, Azure’deki, şirket içindeki kaynaklara karşı ve Amazon Web Hizmetleri (AWS) gibi diğer bulut sağlayıcılarıyla görevleri otomatikleştirmenizi sağlar. Görevlerinizi otomatik hale getirmek için Runbook 'ları veya yönetilecek Azure dışı görevleriniz varsa karma runbook çalışanını kullanabilirsiniz. Her iki ortamda da, Azure aboneliğinde gereken en düşük haklara sahip kaynaklara güvenli bir şekilde erişmek için izinler gerekir.

Bu makalede, Azure Otomasyonu tarafından desteklenen kimlik doğrulama senaryoları ele alınmaktadır ve yönetmeniz gereken ortam veya ortamlara göre nasıl çalışmaya başladığına değinirsiniz.

## <a name="automation-account"></a>Otomasyon hesabı 

Azure Automation’u ilk kez başlattığınızda, en az bir Automation hesabı oluşturmanız gerekir. Otomasyon hesapları, diğer hesapların kaynaklarından Otomasyon kaynaklarınızı, runbook 'larını, varlıkları ve konfigürasyonları ayırmanızı sağlar. Kaynaklarını ayrı mantıksal ortamlara ayırmak için Automation hesaplarını kullanabilirsiniz. Örneğin, geliştirme için bir hesap, üretim için başka bir hesap ve şirket içi ortamınız için de başka bir hesap kullanabilirsiniz. Azure Automation hesabı, Azure aboneliğinizde oluşturduğunuz Microsoft hesabı veya hesaplarından farklıdır. Otomasyon hesabı oluşturmaya giriş için bkz. [Otomasyon hesabı oluşturma](automation-quickstart-create-account.md).

## <a name="automation-resources"></a>Otomasyon kaynakları

Her Otomasyon hesabının Otomasyon kaynakları tek bir Azure bölgesiyle ilişkilendirilir, ancak hesap Azure aboneliğinizdeki tüm kaynakları yönetebilir. Farklı bölgelerde Otomasyon hesapları oluşturmanın temel nedeni, verilerin ve kaynakların belirli bir bölgeye yalıtılması gereken ilkelerinize sahiptir.

Azure Automation 'daki Azure Resource Manager ve PowerShell cmdlet 'lerini kullanarak kaynaklarda oluşturduğunuz tüm görevlerin, Azure Active Directory (Azure AD) kuruluş kimliği kimlik bilgisi tabanlı kimlik doğrulaması kullanarak Azure 'da kimlik doğrulaması yapması gerekir. 

## <a name="run-as-account"></a>Farklı Çalıştır hesabı

Azure Otomasyonu 'nda farklı çalıştır hesapları, PowerShell cmdlet 'lerini kullanarak Azure kaynaklarını yönetmek için kimlik doğrulaması sağlar. Farklı Çalıştır hesabı oluşturduğunuzda, Azure AD 'de yeni bir hizmet sorumlusu kullanıcısı oluşturur ve bu kullanıcıya abonelik düzeyinde katılımcı rolü atar. Azure VM 'lerinde karma runbook çalışanları kullanan runbook 'lar için, Azure kaynaklarınızın kimliğini doğrulamak üzere farklı çalıştır hesapları yerine [yönetilen kimliklerle runbook kimlik doğrulamasını](automation-hrw-run-runbooks.md#runbook-auth-managed-identities) kullanabilirsiniz.

## <a name="service-principal-for-run-as-account"></a>Farklı Çalıştır hesabı için hizmet sorumlusu

Farklı Çalıştır hesabı için hizmet sorumlusu varsayılan olarak Azure AD 'yi okuma izinlerine sahip değildir. Azure AD 'yi okuma veya yönetme izinleri eklemek istiyorsanız, **API izinleri**altında hizmet sorumlusu için izinleri vermeniz gerekir. Daha fazla bilgi için bkz. [Web API 'lerine erişim Izinleri ekleme](../active-directory/develop/quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Rol tabanlı erişim denetimi, Azure AD Kullanıcı hesabı ve farklı çalıştır hesabı için izin verilen eylemleri vermek ve hizmet sorumlusunun kimliğini doğrulamak için Azure Resource Manager ile kullanılabilir. Otomasyon izinlerinin yönetilmesi için modelinizin geliştirilmesine yardımcı olma hakkında daha fazla bilgi için [Azure Otomasyonu’nda rol tabanlı erişim denetimi](automation-role-based-access-control.md) makalesini okuyun.  

## <a name="runbook-authentication-with-hybrid-runbook-worker"></a>Karma Runbook Worker ile runbook kimlik doğrulaması 

Veri merkezinizdeki bir karma runbook çalışanı üzerinde çalışan runbook 'larda veya AWS gibi diğer bulut ortamlarında bilgi işlem hizmetlerine karşı, genellikle Azure kaynaklarında kimlik doğrulaması yapan runbook 'lar için kullanılan yöntemi kullanamaz. Bunun nedeni, bu kaynakların Azure dışında çalışmasıdır; sonuç olarak da, yerel olarak erişecekleri kaynakların kimliğini doğrulamak için Otomasyon'da tanımlanan kendi güvenlik kimlik bilgileri gerekecektir. Runbook çalışanları ile runbook kimlik doğrulaması hakkında daha fazla bilgi için bkz. runbook ['Ları karma Runbook Worker üzerinde çalıştırma](automation-hrw-run-runbooks.md). 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portalından Otomasyon hesabı oluşturma](automation-create-standalone-account.md)
* [Azure Resource Manager şablonu kullanarak Otomasyon hesabı oluşturma](automation-create-account-template.md)
* [Amazon Web Services (AWS) ile kimlik doğrulama](automation-config-aws-account.md)