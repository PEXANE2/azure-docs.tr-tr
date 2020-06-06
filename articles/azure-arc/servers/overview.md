---
title: Sunucular için Azure Arc (Önizleme) genel bakış
description: Azure 'un dışında barındırılan makineleri Azure kaynağı olarak yönetmek üzere sunucular için Azure Arc 'ı nasıl kullanacağınızı öğrenin.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
keywords: Azure Otomasyonu, DSC, PowerShell, istenen durum yapılandırması, güncelleştirme yönetimi, değişiklik izleme, envanter, runbook 'lar, Python, grafik, karma
ms.date: 03/24/2020
ms.topic: overview
ms.openlocfilehash: c9e69e221dc21baae96ab814cb9fb0b65ce98126
ms.sourcegitcommit: 0a5bb9622ee6a20d96db07cc6dd45d8e23d5554a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84449490"
---
# <a name="what-is-azure-arc-for-servers-preview"></a>Sunucular için Azure Arc nedir (Önizleme)

Sunucular için Azure Arc (Önizleme), yerel Azure sanal makinelerini yönetme sürecinize benzer şekilde, kurumsal ağınızda veya diğer bulut sağlayıcıınızda Azure dışında barındırılan Windows ve Linux makinelerinizi yönetmenizi sağlar. Bir karma makine Azure 'a bağlıyken, bağlı bir makine olur ve Azure 'da kaynak olarak kabul edilir. Her bağlı makinenin bir kaynak KIMLIĞI vardır, bir aboneliğin içindeki bir kaynak grubunun parçası olarak yönetilir ve Azure Ilkesi gibi standart Azure yapılarından ve Etiketler uygulayarak faydalanır.

Bu deneyimi Azure dışında barındırılan karma makinelerinizle birlikte sunmak için Azure 'a bağlanmayı planladığınız her makinede Azure bağlı makine aracısının yüklü olması gerekir. Bu aracı başka bir işlevsellik sunmaz ve Azure [Log Analytics aracısının](../../azure-monitor/platform/log-analytics-agent.md)yerini almaz. Makinede çalışan işletim sistemi ve iş yüklerini önceden izlemek, Otomasyon Runbook 'larını veya Güncelleştirme Yönetimi gibi çözümleri kullanarak yönetmek ya da [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md)gibi diğer Azure hizmetlerini kullanmak istediğinizde Windows ve Linux için Log Analytics Aracısı gerekir.

>[!NOTE]
>Bu önizleme sürümü değerlendirme amaçlarına yöneliktir ve kritik üretim makinelerini yönetmemenizi öneririz.
>

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Sunucular için Azure Arc (Önizleme) bağlı makinelerle aşağıdaki senaryoları destekler:

- Azure sanal makineleri için ilke atamayla aynı deneyimi kullanarak [Azure ilke Konuk yapılandırması](../../governance/policy/concepts/guest-configuration.md) atayın.
- Log Analytics Aracısı tarafından toplanan günlük verileri, makinenin kayıtlı olduğu Log Analytics çalışma alanında depolanır. Karma makineden alınan günlük verileri artık makineye özgü, [kaynak bağlamı](../../azure-monitor/platform/design-logs-deployment.md#access-mode) günlük erişimini desteklemek için kullanılabilen BIR kaynak kimliği gibi özellikleri içerir.

## <a name="supported-regions"></a>Desteklenen bölgeler

Sunucular için Azure Arc (Önizleme) ile yalnızca belirli bölgeler desteklenir:

- EastUS
- WestUS2
- WestEurope
- Güneydoğu

Çoğu durumda, yükleme betiğini oluştururken seçtiğiniz konum coğrafi olarak makinenizin konumuna en yakın olan Azure bölgesi olmalıdır. Bekleyen veriler, belirttiğiniz bölgeyi içeren Azure Coğrafya içinde depolanır ve bu da veri yerleşimi gereksinimleriniz varsa bölge seçiminizi de etkileyebilir. Makinenizin bağlandığı Azure bölgesi bir kesinti nedeniyle etkileniyorsa, bağlı makine etkilenmez, ancak Azure 'u kullanan yönetim işlemleri tamamlanmayabilir. Bölgesel bir kesinti durumunda esnekliği için, coğrafi olarak yedekli bir hizmet sağlayan birden çok konumunuz varsa, her konumdaki makineleri farklı bir Azure bölgesine bağlamak en iyisidir.

### <a name="agent-status"></a>Aracı durumu

Bağlı makine Aracısı, her 5 dakikada bir hizmete düzenli bir sinyal iletisi gönderir. Hizmet, bu sinyal mesajlarını bir makineden almayı durduruyor, bu makine çevrimdışı olarak değerlendirilir ve portalda 15 ila 30 dakika içinde otomatik olarak, durum **bağlantısı kesilecek** şekilde değiştirilir. Bağlı makine aracısından sonraki bir sinyal iletisi alındıktan sonra, durumu otomatik olarak **bağlı**olarak değiştirilir.

## <a name="next-steps"></a>Sonraki adımlar

Birden çok karma makinede sunucular için yayı değerlendirmeden veya etkinleştirmeden önce, nelerin gerekli olduğunu, aracı hakkındaki teknik ayrıntıları ve dağıtım yöntemlerini öğrenmek için [bağlı makine aracısına genel bakış](agent-overview.md) makalesini gözden geçirin.
