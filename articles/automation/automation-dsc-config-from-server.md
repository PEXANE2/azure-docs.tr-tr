---
title: Azure Otomasyonu durum yapılandırması için mevcut sunuculardan yapılandırmalar oluşturma
description: Bu makalede, Azure Otomasyonu durum yapılandırması için mevcut sunuculardan yapılandırmaların nasıl oluşturulacağı açıklanır.
keywords: DSC, PowerShell, yapılandırma, kurulum
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.date: 08/08/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: d783ce7193faed4110d07988d3f3f05951c80a45
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83711406"
---
# <a name="create-configurations-from-existing-servers-for-azure-automation-state-configuration"></a>Azure Otomasyonu durum yapılandırması için mevcut sunuculardan yapılandırmalar oluşturma

> Uygulama hedefi: Windows PowerShell 5,1

Mevcut sunuculardan yapılandırmaların oluşturulması zorlu bir görev olabilir.
Yalnızca ilgilendiğiniz kişiler için *Tüm* ayarları istemezsiniz.
Hatta, yapılandırmanın başarıyla uygulanabilmesi için ayarların ne sırada uygulanması gerektiğini bilmeniz gerekir.

> [!NOTE]
> Bu makale, açık kaynak topluluğu tarafından tutulan bir çözüme başvurur.
> Destek, Microsoft 'tan değil yalnızca GitHub işbirliği biçiminde kullanılabilir.

## <a name="community-project-reversedsc"></a>Topluluk projesi: Smardsc

[Smardsc](https://github.com/microsoft/reversedsc) adlı bir topluluk tarafından korunan çözüm, SharePoint 'i başlatan bu alanda çalışacak şekilde oluşturulmuştur.

Çözüm, [Sharepointdsc kaynağında](https://github.com/powershell/sharepointdsc) oluşturulur ve mevcut SharePoint sunucularından [bilgi toplamak](https://github.com/Microsoft/sharepointDSC.reverse#how-to-use) için onu genişletir.
En son sürümde hangi bilgi düzeyinin ekleneceğini belirlemek için birden çok [ayıklama modu](https://github.com/Microsoft/SharePointDSC.Reverse/wiki/Extraction-Modes) vardır.

Çözümü kullanmanın sonucu, SharePointDSC yapılandırma betiklerine göre kullanılacak [yapılandırma verilerini](https://github.com/Microsoft/sharepointDSC.reverse#configuration-data) oluşturuyor.

Veri dosyaları oluşturulduktan sonra, MOF dosyaları oluşturmak ve [MOF dosyalarını Azure Otomasyonu 'na yüklemek](/azure/automation/tutorial-configure-servers-desired-state#create-and-upload-a-configuration-to-azure-automation)Için bunları [DSC yapılandırma betikleri](/powershell/scripting/dsc/overview/overview) ile birlikte kullanabilirsiniz.
Daha sonra, yapılandırma çekmek için [Şirket](/azure/automation/automation-dsc-onboarding#onboarding-physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances) Içinden veya [Azure 'da](/azure/automation/automation-dsc-onboarding#onboarding-azure-vms) sunucularınızı kaydettirin.

Smardsc 'yi denemek için [PowerShell Galerisi](https://www.powershellgallery.com/packages/ReverseDSC/) ziyaret edin ve çözümü indirin veya "proje sitesi" ' ne tıklayarak [belgeleri](https://github.com/Microsoft/sharepointDSC.reverse)görüntüleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Windows PowerShell Istenen durum yapılandırmasına genel bakış](/powershell/scripting/dsc/overview/overview)
- [DSC kaynakları](/powershell/scripting/dsc/resources/resources)
- [Yerel Configuration Manager yapılandırma](/powershell/scripting/dsc/managing-nodes/metaconfig)
