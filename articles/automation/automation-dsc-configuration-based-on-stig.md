---
title: Azure Otomasyonu durum yapılandırması için STIG tabanlı verileri yapılandırma
description: Bu makalede, Azure Otomasyonu durum yapılandırması için STIG tabanlı verilerin nasıl yapılandırılacağı açıklanır.
keywords: DSC, PowerShell, yapılandırma, kurulum
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.date: 08/08/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: e17ae51addce2bb156b6f1c54e29fd777699516f
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83711389"
---
# <a name="configure-data-based-on-stig-for-azure-automation-state-configuration"></a>Azure Otomasyonu durum yapılandırması için STIG tabanlı verileri yapılandırma

> Uygulama hedefi: Windows PowerShell 5,1

İlk kez yapılandırma içeriği oluşturma zor olabilir.
Çoğu durumda, amaç bir sektör önerisini tamamen hizalayan bir "taban çizgisi" takip eden sunucuların yapılandırılmasını otomatik hale getirmektir.

> [!NOTE]
> Bu makale, açık kaynak topluluğu tarafından tutulan bir çözüme başvurur.
> Destek, Microsoft 'tan değil yalnızca GitHub işbirliği biçiminde kullanılabilir.

## <a name="community-project-powerstig"></a>Topluluk projesi: PowerSTIG

Bu sorunu çözmek için [powerstig](https://github.com/microsoft/powerstig) amaçlar adlı bir topluluk projesi, Stig (güvenlik teknik uygulama kılavuzu) hakkında sunulan [genel bilgilere](https://public.cyber.mil/stigs/) dayanarak DSC içeriği oluşturarak bu sorunu çözer.

Taban çizgileri ile ilgilenirken bu seslerden daha karmaşıktır.
Birçok kuruluşun [özel durumları kurallara göre belgelenmesi](https://github.com/microsoft/powerstig#powerstigdata) ve bu verileri ölçeklendirerek yönetmesi gerekir.
PowerSTIG, tek bir büyük dosyadaki tüm ayar aralığını ele almak yerine, yapılandırmanın her bir alanını ele almak için [bileşik kaynaklar](https://github.com/microsoft/powerstig#powerstigdsc) sağlayarak sorunu giderir.

Yapılandırmalar oluşturulduktan sonra, MOF dosyaları oluşturmak ve [MOF dosyalarını Azure Otomasyonu 'na yüklemek](/azure/automation/tutorial-configure-servers-desired-state#create-and-upload-a-configuration-to-azure-automation)Için [DSC yapılandırma betikleri](/powershell/scripting/dsc/configurations/configurations) ' ni kullanabilirsiniz.
Daha sonra, yapılandırma çekmek için [Şirket](/azure/automation/automation-dsc-onboarding#onboarding-physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances) Içinden veya [Azure 'da](/azure/automation/automation-dsc-onboarding#onboarding-azure-vms) sunucularınızı kaydettirin.

PowerSTIG 'yi denemek için [PowerShell Galerisi](https://www.powershellgallery.com) ziyaret edin ve çözümü indirin veya "proje sitesi" ne tıklayarak [belgeleri](https://github.com/microsoft/powerstig)görüntüleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Windows PowerShell Istenen durum yapılandırmasına genel bakış](/powershell/scripting/dsc/overview/overview)
- [DSC kaynakları](/powershell/scripting/dsc/resources/resources)
- [Yerel Configuration Manager yapılandırma](/powershell/scripting/dsc/managing-nodes/metaconfig)
