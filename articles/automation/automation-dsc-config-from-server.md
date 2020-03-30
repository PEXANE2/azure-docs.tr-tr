---
title: Varolan sunuculardan yapılandırmalar oluşturma - Azure Otomasyonu
description: Azure Otomasyonu için mevcut sunuculardan yapılandırmaları nasıl oluşturacağınızı öğrenin.
keywords: dsc, powershell, yapılandırma, kurulum
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.date: 08/08/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: d888c8fd3bf7cd44c37e7225618bd197f419d275
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76030134"
---
# <a name="create-configurations-from-existing-servers"></a>Varolan sunuculardan yapılandırmalar oluşturma

> Geçerlidir: Windows PowerShell 5.1

Varolan sunuculardan yapılandırmalar oluşturmak zor bir görev olabilir.
*Tüm* ayarları istemeyebilirsiniz, sadece önemsediğiniz ayarları.
O zaman bile yapılandırmanın başarılı bir şekilde uygulanabilmesi için ayarların hangi sırada uygulanması gerektiğini bilmeniz gerekir.

> [!NOTE]
> Bu makalede, Açık Kaynak topluluğu tarafından korunan bir çözüm anlamına gelir.
> Destek, Microsoft'tan değil, yalnızca GitHub işbirliği biçiminde kullanılabilir.

## <a name="community-project-reversedsc"></a>Topluluk projesi: ReverseDSC

SharePoint'i başlatan bu alanda çalışmak üzere [ReverseDSC](https://github.com/microsoft/reversedsc) adlı bir topluluk tarafından korunan çözüm oluşturuldu.

Çözüm, [SharePointDSC kaynağıüzerinde](https://github.com/powershell/sharepointdsc) bir oluşturur ve varolan SharePoint sunucularından [bilgi toplamayı](https://github.com/Microsoft/sharepointDSC.reverse#how-to-use) düzenlemeye genişletir.
En son sürüm, hangi bilgi düzeyini içereceklerini belirlemek için birden çok [çıkarma moduna](https://github.com/Microsoft/SharePointDSC.Reverse/wiki/Extraction-Modes) sahiptir.

Çözümü kullanmanın sonucu, SharePointDSC yapılandırma komut dosyalarıyla kullanılmak üzere [Yapılandırma Verileri](https://github.com/Microsoft/sharepointDSC.reverse#configuration-data) oluşturmaktır.

Veri dosyaları oluşturulduktan sonra, MOF dosyaları oluşturmak ve [MOF dosyalarını Azure Otomasyonu'na yüklemek için](/azure/automation/tutorial-configure-servers-desired-state#create-and-upload-a-configuration-to-azure-automation) [bunları DSC Configuration komut dosyalarıyla](/powershell/scripting/dsc/overview/overview) kullanabilirsiniz.
Ardından yapılandırmaları çekmek için sunucularınızı [şirket içinde](/azure/automation/automation-dsc-onboarding#physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances) veya [Azure'dan](/azure/automation/automation-dsc-onboarding#azure-virtual-machines) kaydedin.

ReverseDSC'yi denemek için [PowerShell Galerisi'ni](https://www.powershellgallery.com/packages/ReverseDSC/) ziyaret edin ve çözümü indirin veya [belgeleri](https://github.com/Microsoft/sharepointDSC.reverse)görüntülemek için "Proje Sitesi"ne tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Windows PowerShell İstenilen Durum Yapılandırmagenel Bakış](/powershell/scripting/dsc/overview/overview)
- [DSC Kaynakları](/powershell/scripting/dsc/resources/resources)
- [Yerel Yapılandırma Yöneticisini Yapılandırma](/powershell/scripting/dsc/managing-nodes/metaconfig)
