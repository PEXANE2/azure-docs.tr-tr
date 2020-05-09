---
title: Kavramlar-kimlik ve erişim
description: Azure VMware çözümünün (AVS) kimlik ve erişim kavramları hakkında bilgi edinin
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: ce85b60b38db86af14b9e9aa06e568436dc76658
ms.sourcegitcommit: d9cd51c3a7ac46f256db575c1dfe1303b6460d04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2020
ms.locfileid: "82740960"
---
# <a name="azure-vmware-solution-avs-identity-concepts"></a>Azure VMware çözümü (AVS) kimlik kavramları

Bir vCenter Server ve NSX-T Yöneticisi, özel bir bulut dağıtıldığında temin edilir. Özel bulut yazılım tanımlı ağını genişletmek üzere sanal makine iş yüklerini ve NSX-T yöneticisini yönetmek için vCenter 'ı kullanın.

Erişim ve kimlik yönetimi NSX-T Manager için vCenter ve kısıtlı yönetici hakları için CloudAdmin Grup ayrıcalıklarını kullanın. Bu ilke, özel bulut platformunuzun otomatik olarak yükseltilmesini sağlar. Bu, en yeni özellikleri ve düzeltme eklerini düzenli olarak sunar. Özel bulut yükseltmeleri hakkında daha fazla bilgi için bkz. [özel bulut yükseltmeleri kavramları makalesi][concepts-upgrades] .

## <a name="vcenter-access-and-identity"></a>vCenter erişimi ve kimliği

VCenter 'daki ayrıcalıklar CloudAdmin grubu üzerinden sağlanır. Bu grup vCenter 'da yerel olarak yönetilebilir veya Azure Active Directory vCenter LDAP çoklu oturum açma tümleştirmesi aracılığıyla yapılandırılabilir. Özel bir bulut dağıttıktan sonra bu tümleştirmeyi etkinleştirebilme olanağı sunulur.

CloudAdmin ve CloudGlobalAdmin ayrıcalıkları aşağıdaki tabloda gösterilmiştir.

|  Ayrıcalık kümesi           | CloudAdmin | CloudGlobalAdmin | Açıklama |
| :---                     |    :---:   |       :---:      |   :--:  |
|  Alarmlar                  | CloudAdmin Kullanıcı, Işlem-ResourcePool ve VM 'lerdeki alarmlar için tüm alarmlar ayrıcalıklarına sahiptir.     |          --        |  -- |
|  Otomatik dağıt             |  --  |        --        |  Microsoft, ana bilgisayar yönetimi yapar.  |
|  Sertifikalar            |  --  |        --       |  Microsoft, sertifika yönetimi yapar.  |
|  İçerik Kitaplığı         | Bir CloudAdmin kullanıcısının bir Içerik kitaplığındaki dosyaları oluşturma ve kullanma ayrıcalıkları vardır.    |         SSO ile etkinleştirildi.         |  Microsoft, dosyaları Içerik kitaplığındaki ESXi konaklarına dağıtır.  |
|  Veri merkezi              |  --  |        --          |  Microsoft tüm veri merkezi işlemlerini yapar.  |
|  Veri Deposu               | DataStore. AllocateSpace, DataStore. gözatmayı, DataStore. config, DataStore. DeleteFile, DataStore. FileManagement, DataStore. UpdateVirtualMachineMetadata     |    --    |   -- |
|  ESX Aracısı Yöneticisi       |  --  |         --       |  Microsoft tüm işlemleri yapar.  |
|  Klasör                  |  Bir CloudAdmin kullanıcısının tüm klasör ayrıcalıkları vardır.     |  --  |  --  |
|  Genel                  |  Global. CancelTask, Global. GlobalTag, Global. Health, Global. LogEvent, Global. ManageCustomFields, Global. Serviceyöneticileri, Global. SetCustomField, Global. SystemTag         |                  |    |
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

NSX-T yöneticisine "Yönetici" hesabını kullanarak erişirsiniz. Bu hesabın tam ayrıcalıkları vardır ve T1 yönlendiricileri, mantıksal anahtarlar ve tüm hizmetler oluşturup yönetmenize olanak sağlar. NSX-T ' deki tüm ayrıcalıklar Ayrıca NSX-T T0 yönlendiricisine erişim sağlar. T0 yönlendiricisinde yapılan bir değişiklik, ağ performansının düşmesine veya özel bir buluta erişim kaybına neden olabilir. Destek gereksinimlerini karşılamak için, NSX-T T0 yönlendiricinizde herhangi bir değişiklik istemek üzere Azure portal bir destek isteği açmanız gerekir.
  
## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki adım, [özel bulut yükseltmesi kavramları][concepts-upgrades]hakkında bilgi almak için kullanılır.

<!-- LINKS - external -->

<!-- LINKS - internal -->
[concepts-upgrades]: ./concepts-upgrades.md