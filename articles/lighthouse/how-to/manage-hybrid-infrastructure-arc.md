---
title: Azure Arc ile karma altyapıyı ölçeklendirerek yönetme
description: Müşterilerinizin makinelerini ve Kubernetes kümelerini Azure dışında etkili bir şekilde yönetmeyi öğrenin.
ms.date: 09/15/2020
ms.topic: how-to
ms.openlocfilehash: 2ffbe9019398896c594b7cb0e0424d2b5f4dc37a
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90605337"
---
# <a name="manage-hybrid-infrastructure-at-scale-with-azure-arc"></a>Azure Arc ile karma altyapıyı ölçeklendirerek yönetme

Hizmet sağlayıcı olarak, [Azure açık Thouse](../overview.md)için birden fazla müşteri kiracısından eklendi olabilirsiniz. Azure Mathouse, hizmet sağlayıcılarının birden çok Azure Active Directory (Azure AD) kiracılarının aynı anda, yönetim görevlerini daha verimli hale getiren işlemleri gerçekleştirmesine olanak tanır.

[Azure Arc](../../azure-arc/overview.md) , şirket içi, kenar ve çok yüksek ortamlarda karmaşık ve dağıtılmış ortamları basitleştirerek Azure hizmetlerinin her yerde dağıtımını sağlar ve Azure yönetimini herhangi bir altyapıya genişletmenize yardımcı olur.

[Azure Arc etkin sunucuları (Önizleme)](../../azure-arc/servers/overview.md)sayesinde, müşteriler, Azure dışında barındırılan tüm Windows ve Linux makinelerini, yerel Azure sanal makinelerini yönettikleri gibi şirket ağında yönetebilir. Karma makineyi Azure'a bağlama yoluyla Azure'la bağlantılı hale gelir ve bir kaynak olarak kabul edilir. Hizmet sağlayıcıları, bu Azure dışı makineleri müşterilerinin Azure kaynaklarıyla birlikte yönetebilir.

[Azure Arc etkin Kubernetes (Önizleme)](../../azure-arc/kubernetes/overview.md) , müşterilerin Azure içinde veya dışında Kubernetes kümeleri eklemesine ve yapılandırmasına izin verir. Bir Kubernetes kümesi Azure yaya iliştirdiğinde, Azure portal Azure Resource Manager KIMLIĞI ve yönetilen kimlik ile birlikte görüntülenir. Kümeler standart Azure aboneliklerine iliştirilir, bir kaynak grubunda bulunur ve diğer tüm Azure kaynakları gibi etiketleri alabilir.

Bu konuda, hizmet sağlayıcılarının Azure yay etkin sunucularını (Önizleme) ve Azure Arc 'ın, müşterilerinin karma ortamını yönetmek için ölçeklenebilir bir şekilde (Önizleme), tüm yönetilen müşteri kiracılarında görünebilen bir genel bakış sunulmaktadır.

> [!TIP]
> Bu konudaki hizmet sağlayıcılarına ve müşterilere başvurduğumuz halde, bu kılavuz [birden çok kiracıyı yönetmek Için Azure açık Thouse kullanan kuruluşlar](../concepts/enterprise.md)için de geçerlidir.

## <a name="manage-hybrid-servers-at-scale-with-azure-arc-enabled-servers-preview"></a>Azure Arc etkin sunucularıyla karma sunucuları ölçeklendirerek yönetme (Önizleme)

> [!NOTE]
> Azure Arc etkin sunucuları şu anda önizleme aşamasındadır. Bunu şu anda üretim iş yükleri için önermiyoruz.

Hizmet sağlayıcı olarak, Azure dışındaki şirket içi Windows Server veya Linux makinelerini, müşterilerinizin [Azure bağlı makine Aracısı](../../azure-arc/servers/agent-overview.md)'nı kullanarak aboneliğine bağladığını yönetebilirsiniz.

Azure portal bir temsilci abonelik için kaynakları görüntülerken, **Azure Arc**ile etiketlenmiş bu bağlı makineleri görürsünüz. Bu bağlı makineleri Azure Ilkesi ve etiketleme gibi Azure yapılarını kullanarak, müşterinin Azure kaynaklarını yönettiğiniz şekilde yönetebilirsiniz. Ayrıca, tüm bağlı karma makineleri birlikte yönetmek için müşteri kiracılarında da çalışabilirsiniz.

Örneğin, [müşterilerin karma makineler arasında aynı ilke kümesinin uygulandığından emin](../../azure-arc/servers/learn/tutorial-assign-policy-portal.md)olabilirsiniz. Ayrıca, Azure Güvenlik Merkezi 'ni kullanarak tüm müşterilerinizin karma ortamlarında uyumluluğu izleyebilir veya [Azure izleyici 'yi kullanarak doğrudan karma makinelerinizden](../../azure-arc/servers/learn/tutorial-enable-vm-insights.md) bir Log Analytics çalışma alanına veri toplayabilirsiniz. [Sanal makine uzantıları](../../azure-arc/servers/manage-vm-extensions.md) , Azure olmayan Windows ve Linux VM 'lerine dağıtılabilir ve müşterinin karma makinelerinin yönetimini basitleştirir.

## <a name="manage-hybrid-kubernetes-clusters-at-scale-with-azure-arc-enabled-kubernetes-preview"></a>Azure Arc etkin Kubernetes (Önizleme) ile karma Kubernetes kümelerini ölçeklendirerek yönetme

> [!NOTE]
> Azure Arc etkin Kubernetes Şu anda önizleme aşamasındadır. Bunu şu anda üretim iş yükleri için önermiyoruz.

Azure 'da çalıştığı gibi, [bir müşterinin Azure Arc aboneliğine bağlanmış](../../azure-arc/kubernetes/connect-cluster.md)Kubernetes kümelerini yönetebilirsiniz.

Müşteriniz, [Kubernetes kümelerini Azure yaya eklemek için bir hizmet sorumlusu hesabı](../../azure-arc/kubernetes/create-onboarding-service-principal.md)oluşturmışsa, kümeleri eklemek ve yönetmek için bu hizmet sorumlusu hesabına erişebilirsiniz. Bu işlem, hizmet sorumlusu hesabını içeren aboneliğin [Azure Athouse eklendi](onboard-customer.md)olduğu durumlarda, "Kubernetes kümesi-Azure yay ekleme" adlı kiracı, yönetim kiracısındaki kullanıcılar tarafından yapılabilir.

Bağlı kümeler için Gilar kullanarak [yapılandırma](../../azure-arc/kubernetes/use-gitops-connected-cluster.md) ve [Held grafikleri](../../azure-arc/kubernetes/use-gitops-with-helm.md) dağıtabilirsiniz.

Ayrıca, Azure Izleyici ile bağlı kümeleri izleyebilir ve [Azure ilkesi 'ni kullanarak küme yapılandırmalarının ölçeğini de uygulayabilirsiniz](../../azure-arc/kubernetes/use-azure-policy.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Arc GitHub deposundaki](https://github.com/microsoft/azure_arc)atlamalı ve örnekleri araştırın. 
- [Azure Arc etkin sunucuları için desteklenen senaryolar](../../azure-arc/servers/overview.md#supported-scenarios)hakkında bilgi edinin.
- [Azure Arc tarafından desteklenen Kubernetes dağıtımları](../../azure-arc/kubernetes/overview.md#supported-kubernetes-distributions)hakkında bilgi edinin.
- [Bir ilkeyi ölçeklendirerek dağıtmayı](policy-at-scale.md)öğrenin.
- [Azure Izleyici günlüklerini ölçekli](monitor-at-scale.md)olarak nasıl kullanacağınızı öğrenin.

