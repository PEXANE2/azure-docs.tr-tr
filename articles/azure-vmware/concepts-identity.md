---
title: Kavramlar-kimlik ve erişim
description: Azure VMware çözümünün kimlik ve erişim kavramları hakkında bilgi edinin
ms.topic: conceptual
ms.date: 03/18/2021
ms.openlocfilehash: 07a7ac8093524ef4240b8f7607d649520b9439e1
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104586259"
---
# <a name="azure-vmware-solution-identity-concepts"></a>Azure VMware Çözüm kimliği kavramları

Azure VMware çözümü özel bulutları bir vCenter Server ve NSX-T Yöneticisi ile sağlanır. Sanal makine (VM) iş yüklerini yönetmek için vCenter 'ı kullanın. Özel bulut ağını yönetmek ve genişletmek için NSX-T yöneticisini kullanın.

VCenter erişimi ve kimlik yönetimi BuildIn CloudAdmin Group ayrıcalıklarını kullanır. NSX-T Yöneticisi kısıtlı yönetici izinleri kullanır. Bu, yönetilen hizmetin doğası gereği, özel bulut platformunuzun beklenmek üzere en yeni özelliklerle ve düzeltme ekleriyle yükseltmelerini sağlar.  Daha fazla bilgi için bkz. [özel bulut yükseltmeleri kavramları makalesi][concepts-upgrades].

## <a name="vcenter-access-and-identity"></a>vCenter erişimi ve kimliği

VCenter CloudAdmin grubu, vCenter içindeki ayrıcalıkları tanımlar ve sağlar. Başka bir seçenek de Azure Active Directory ile vCenter LDAP çoklu oturum açma tümleştirmesi aracılığıyla erişim ve kimlik sağlamaktır. Özel bulutunuzu dağıttıktan sonra bu tümleştirmeyi etkinleştirirsiniz. 

Tabloda **CloudAdmin** ve **CloudGlobalAdmin** ayrıcalıkları gösterilmektedir.

|  Ayrıcalık kümesi           | CloudAdmin | CloudGlobalAdmin | Yorum |
| :---                     |    :---:   |       :---:      |   :--:  |
|  Alarmlar                  | Bir CloudAdmin Kullanıcı, Compute-ResourcePool ve VM 'lerdeki alarmlar için tüm alarmlar ayrıcalıklarına sahiptir.     |          --        |  -- |
|  Otomatik dağıt             |  --  |        --        |  Microsoft, ana bilgisayar yönetimi yapar.  |
|  Sertifikalar            |  --  |        --       |  Microsoft, sertifika yönetimi yapar.  |
|  İçerik Kitaplığı         | Bir CloudAdmin kullanıcısının bir Içerik kitaplığındaki dosyaları oluşturma ve kullanma ayrıcalıkları vardır.    |         SSO ile etkinleştirildi.         |  Microsoft, dosyaları Içerik kitaplığındaki ESXi konaklarına dağıtır.  |
|  Veri merkezi              |  --  |        --          |  Microsoft tüm veri merkezi işlemlerini yapar.  |
|  Veri Deposu               | DataStore. AllocateSpace, DataStore. gözatıp, Datastore.Config, DataStore. DeleteFile, DataStore. FileManagement, DataStore. UpdateVirtualMachineMetadata     |    --    |   -- |
|  ESX Aracısı Yöneticisi       |  --  |         --       |  Microsoft tüm işlemleri yapar.  |
|  Klasör                  |  Bir CloudAdmin kullanıcısının tüm klasör ayrıcalıkları vardır.     |  --  |  --  |
|  Genel                  |  Global. CancelTask, Global. GlobalTag, Global. Health, Global. LogEvent, Global. ManageCustomFields, Global. Serviceyöneticileri, Global. SetCustomField, Global.SysTıtemtag         |                  |    |
|  Ana bilgisayar                    |  Host. HBR. HbrManagement      |        --          |  Microsoft, diğer tüm konak işlemlerini yapar.  |
|  InventoryService        |  InventoryService. etiketleme      |        --          |  --  |
|  Ağ                 |  Network. assign    |                  |  Microsoft, diğer tüm ağ işlemlerini yapar.  |
|  İzinler             |  --  |        --       |  Microsoft tüm Izin işlemlerini yapar.  |
|  Profil temelli depolama  |  --  |        --       |  Microsoft tüm profil işlemlerini yapar.  |
|  Kaynak                |  Bir CloudAdmin kullanıcısının tüm kaynak ayrıcalıkları vardır.        |      --       | --   |
|  Zamanlanmış görev          |  Bir CloudAdmin kullanıcısının tüm ScheduleTask ayrıcalıkları vardır.   |   --   | -- |
|  Oturumlar                |  Sessions. GlobalMessage, Sessions. ValidateSession      |   --   |  Microsoft tüm diğer oturum işlemlerini yapar.  |
|  Depolama görünümleri           |  StorageViews. View   |        --          |  Microsoft, diğer tüm depolama görünüm işlemlerini (hizmeti yapılandırır) yapar.  |
|  Görevler                   |  --  |  --   |  Microsoft, görevleri yöneten uzantıları yönetir.  |
|  vApp                    |  Bir CloudAdmin kullanıcısının tüm vApp ayrıcalıkları vardır.  |  --  |  --  |
|  Sanal Makine         |  Bir CloudAdmin kullanıcısının tüm VirtualMachine ayrıcalıkları vardır.  |  --  |  --  |
|  vService                |  Bir CloudAdmin kullanıcısının tüm vService ayrıcalıkları vardır.  |  --  |  --  |

## <a name="nsx-t-manager-access-and-identity"></a>NSX-T Yöneticisi erişimi ve kimliği

NSX-T Yöneticisi 'ne erişmek için *yönetici* hesabını kullanın. Tam ayrıcalıkları vardır ve Katman 1 (T1) ağ geçitleri, segmentler (mantıksal anahtarlar) ve tüm hizmetler oluşturup yönetmenize olanak sağlar. Bu hesap, NSX-T katmanı-0 (T0) ağ geçidine de erişim sağlar. Bu değişikliklerin yapılması, ağ performansının düşmesine neden olabilir veya özel bir bulut erişimi olmayabilir. NSX-T T0 ağ geçidinizdeki herhangi bir değişiklik istemek için Azure portal bir destek isteği açın.
  
## <a name="next-steps"></a>Sonraki adımlar

Azure VMware Çözüm erişimi ve kimlik kavramlarını kapsadığınıza göre, şunları öğrenmek isteyebilirsiniz:

- [Özel bulut yükseltmesi kavramları](concepts-upgrades.md).
- [Azure VMware çözümü Için vSphere rol tabanlı erişim denetimi](concepts-role-based-access-control.md).
- [Azure VMware Çözüm kaynağını etkinleştirme](enable-azure-vmware-solution.md).

<!-- LINKS - external -->

<!-- LINKS - internal -->
[concepts-upgrades]: ./concepts-upgrades.md
