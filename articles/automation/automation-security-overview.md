---
title: Azure Otomasyonu 'nda kimlik doğrulamaya giriş
description: Bu makalede, Azure Otomasyonu’nda Otomasyon Hesapları için uygun Otomasyon güvenliği ve farklı kumluk doğrulasa yöntemlerine genel bakış verilmektedir.
keywords: otomasyon güvenliği, güvenli otomasyon; otomasyon kimlik doğrulaması
services: automation
ms.subservice: process-automation
ms.date: 04/23/2020
ms.topic: conceptual
ms.openlocfilehash: 109bb6dd29ea9c4239e0abcfc668f1185f7e9783
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82114539"
---
# <a name="introduction-to-authentication-in-azure-automation"></a>Azure Otomasyonu’nda kimlik doğrulamaya giriş

Azure Automation, Azure’deki, şirket içindeki kaynaklara karşı ve Amazon Web Hizmetleri (AWS) gibi diğer bulut sağlayıcılarıyla görevleri otomatikleştirmenizi sağlar. Runbook'un gerekli işlemlerini gerçekleştirebilmesi için, abonelikte gereken en düşük haklara sahip kaynaklara güvenli erişim izinlerinin olması gerekir.

Bu makale, Azure Otomasyonu tarafından desteklenen çeşitli kimlik doğrulama senaryolarına ve yönetmeniz gereken ortama veya ortamlara göre nasıl çalışmaya başladığına değinmelidir.  

## <a name="automation-account-overview"></a>Otomasyon Hesabına genel bakış

Azure Automation’u ilk kez başlattığınızda, en az bir Automation hesabı oluşturmanız gerekir. Automation hesapları Automation kaynaklarınızı (runbook'lar, varlıklar, yapılandırmalar) diğer Automation hesaplarında yer alan kaynaklarından yalıtmanızı sağlar. Kaynaklarını ayrı mantıksal ortamlara ayırmak için Automation hesaplarını kullanabilirsiniz. Örneğin, geliştirme için bir hesap, üretim için başka bir hesap ve şirket içi ortamınız için de başka bir hesap kullanabilirsiniz. Azure Automation hesabı, Azure aboneliğinizde oluşturduğunuz Microsoft hesabı veya hesaplarından farklıdır.

Her Otomasyon hesabı için Otomasyon kaynakları tek bir Azure bölgesiyle ilişkilendirilir, ancak Otomasyon hesapları aboneliğinizdeki tüm kaynakları yönetebilir. Farklı bölgelerde Automation hesapları oluşturmanın temel nedeni, veri ve kaynakların belirli bir bölgede yalıtılmasını gerektiren ilkelere sahip olmanız olabilir.

Azure Resource Manager ve Azure Otomasyonu’ndaki Azure cmdlet'lerini kullanan kaynaklara karşı gerçekleştirdiğiniz görevlerin tümü, Azure Active Directory kuruluş kimliğini kullanarak Azure’de kimlik bilgileri tabanlı kimlik doğrulamasını doğrulamalıdır. Azure Otomasyonu 'nda farklı çalıştır hesapları Azure cmdlet 'lerini kullanarak Azure 'da kaynak yönetimine yönelik kimlik doğrulaması sağlar. Farklı Çalıştır hesabı oluşturduğunuzda, Azure Active Directory (AD) içinde yeni bir hizmet sorumlusu kullanıcısı oluşturur ve bu kullanıcıya abonelik düzeyinde katılımcı rolü atar. Azure sanal makinelerinde karma runbook çalışanları kullanan runbook 'lar için Azure kaynaklarınızın kimliğini doğrulamak üzere farklı çalıştır hesapları yerine [Azure kaynakları için Yönetilen kimlikler](automation-hrw-run-runbooks.md#managed-identities-for-azure-resources) kullanabilirsiniz.

Farklı Çalıştır hesabı için hizmet sorumlusu varsayılan olarak Azure AD 'yi okuma izinlerine sahip değildir. Azure AD 'yi okuma veya yönetme izinleri eklemek istiyorsanız, **API izinleri**altında hizmet sorumlusu için izinleri vermeniz gerekir. Daha fazla bilgi için bkz. [Web API 'lerine erişim Izinleri ekleme](../active-directory/develop/quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

Rol tabanlı erişim denetimi, Azure AD kullanıcı hesabı ve Farklı Çalıştır hesabına izin verilen eylemleri vermek, ve bu hizmet sorumlusunun kimliğini doğrulamak için Azure Resource Manager ile kullanılabilir. Otomasyon izinlerinin yönetilmesi için modelinizin geliştirilmesine yardımcı olma hakkında daha fazla bilgi için [Azure Otomasyonu’nda rol tabanlı erişim denetimi](automation-role-based-access-control.md) makalesini okuyun.  

Veri merkezinizdeki karma runbook çalışanı üzerinde çalışan runbook 'larda veya AWS gibi diğer bulut ortamlarında bilgi işlem hizmetlerine karşı, genellikle Azure kaynaklarında kimlik doğrulaması yapan runbook 'lar için kullanılan yöntemi kullanamaz. Bunun nedeni, bu kaynakların Azure dışında çalışmasıdır; sonuç olarak da, yerel olarak erişecekleri kaynakların kimliğini doğrulamak için Otomasyon'da tanımlanan kendi güvenlik kimlik bilgileri gerekecektir. Runbook çalışanları ile runbook kimlik doğrulaması hakkında daha fazla bilgi için bkz. [karma runbook çalışanları için Runbook 'ları](automation-hrw-run-runbooks.md)doğrulama. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Portal bir Otomasyon hesabı oluşturun](automation-create-standalone-account.md).

* [Azure Resource Manager şablonu kullanarak bir Otomasyon hesabı oluşturun](automation-create-account-template.md).

* [Amazon Web Services (AWS) Ile kimlik doğrulaması](automation-config-aws-account.md)yapın.
