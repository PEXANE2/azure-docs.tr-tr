---
title: Devlet yapılandırmasında kullanılacak STIG tabanlı yapılandırma - Azure Otomasyonu
description: Azure Otomasyonu'nda durum yapılandırması için STIG'e dayalı yapılandırmalar hakkında bilgi edinin.
keywords: dsc, powershell, yapılandırma, kurulum
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.date: 08/08/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 275b3bd25f931b73e8a378433899ef9ade4d47c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76028284"
---
# <a name="configuration-based-on-stig"></a>STIG tabanlı yapılandırma

> Geçerlidir: Windows PowerShell 5.1

Yapılandırma içeriğini ilk kez oluşturmak zor olabilir.
Çoğu durumda amaç, bir endüstri önerisine hizalanır umarım bir "taban çizgisi" aşağıdaki sunucuların yapılandırmasını otomatikleştirmektir.

> [!NOTE]
> Bu makalede, Açık Kaynak topluluğu tarafından korunan bir çözüm anlamına gelir.
> Destek, Microsoft'tan değil, yalnızca GitHub işbirliği biçiminde kullanılabilir.

## <a name="community-project-powerstig"></a>Topluluk projesi: PowerSTIG

[PowerSTIG](https://github.com/microsoft/powerstig) adlı bir topluluk projesi, STIG (Güvenlik Teknik Uygulama Kılavuzu) hakkında sağlanan [genel bilgilere](https://public.cyber.mil/stigs/) dayanarak DSC içeriği oluşturarak bu sorunu çözmeyi amaçlamaktadır.

Taban çizgileri ile başa çıkmak göründüğünden daha karmaşıktır.
Birçok kuruluşun kurallara [özel durumları belgelemelidir](https://github.com/microsoft/powerstig#powerstigdata) ve bu verileri ölçekte yönetmesi gerekir.
PowerSTIG, büyük bir dosyadaki tüm ayar aralığını ele almaya çalışmak yerine yapılandırmanın her alanını gidermek için [Bileşik Kaynaklar](https://github.com/microsoft/powerstig#powerstigdsc) sağlayarak sorunu giderer.

Yapılandırmalar oluşturulduktan sonra, MOF dosyaları oluşturmak ve [MOF dosyalarını Azure Otomasyonu'na yüklemek için](/azure/automation/tutorial-configure-servers-desired-state#create-and-upload-a-configuration-to-azure-automation) [DSC Yapılandırma komut dosyalarını](/powershell/scripting/dsc/configurations/configurations) kullanabilirsiniz.
Ardından yapılandırmaları çekmek için sunucularınızı [şirket içinde](/azure/automation/automation-dsc-onboarding#physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances) veya [Azure'dan](/azure/automation/automation-dsc-onboarding#azure-virtual-machines) kaydedin.

PowerSTIG'i denemek için [PowerShell Galerisi'ni](https://www.powershellgallery.com) ziyaret edin ve çözümü indirin veya [belgeleri](https://github.com/microsoft/powerstig)görüntülemek için "Proje Sitesi"ne tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Windows PowerShell İstenilen Durum Yapılandırmagenel Bakış](/powershell/scripting/dsc/overview/overview)
- [DSC Kaynakları](/powershell/scripting/dsc/resources/resources)
- [Yerel Yapılandırma Yöneticisini Yapılandırma](/powershell/scripting/dsc/managing-nodes/metaconfig)
