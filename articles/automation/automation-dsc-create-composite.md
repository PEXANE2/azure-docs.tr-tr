---
title: Yapılandırmaları durum yapılandırması için bileşik kaynaklara dönüştürme - Azure Otomasyonu
description: Azure Otomasyonu'nda yapılandırmaları devlet yapılandırması için bileşik kaynaklara nasıl dönüştüreceklerini öğrenin.
keywords: dsc, powershell, yapılandırma, kurulum
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.date: 08/08/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 05f0a81a738688df15ea9060071d9e266b54b7d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79136664"
---
# <a name="convert-configurations-to-composite-resources"></a>Yapılandırmaları bileşik kaynaklara dönüştürme

> Geçerlidir: Windows PowerShell 5.1

Yapılandırmaları yazmaya başladıktan sonra, hızlı bir şekilde ayar gruplarını yöneten "senaryolar" oluşturabilirsiniz.
Örnekler olacaktır:

- bir web sunucusu oluşturma
- Bir DNS sunucusu oluşturma
- SharePoint sunucusu oluşturma
- bir SQL kümesini yapılandırma
- güvenlik duvarı ayarlarını yönetme
- parola ayarlarını yönetme

Bu çalışmayı başkalarıyla paylaşmak istiyorsanız, en iyi seçenek yapılandırmayı [Bileşik Kaynak](/powershell/scripting/dsc/resources/authoringresourcecomposite)olarak paketlemektir.
İlk kez bileşik kaynaklar oluşturmak ezici olabilir.

> [!NOTE]
> Bu makalede, Açık Kaynak topluluğu tarafından korunan bir çözüm anlamına gelir.
> Destek, Microsoft'tan değil, yalnızca GitHub işbirliği biçiminde kullanılabilir.

## <a name="community-project-compositeresource"></a>Topluluk projesi: CompositeResource

Bu sorunu çözmek için [CompositeResource](https://github.com/microsoft/compositeresource) adlı bir topluluk tarafından korunan çözüm oluşturuldu.

CompositeResource yapılandırmanızdan yeni bir modül oluşturma işlemini otomatikleştirir.
İş istasyonunuzdaki (veya sunucu oluşturma) yapılandırma komut dosyasını [oluşturarak](https://blogs.technet.microsoft.com/heyscriptingguy/2010/08/10/how-to-reuse-windows-powershell-functions-in-scripts/) başlarsınız, böylece bellekte yüklenir.
Ardından, bir MOF dosyası oluşturmak için yapılandırmayı çalıştırmak yerine, dönüştürmeyi otomatikleştirmek için CompositeResource modülü tarafından sağlanan işlevi kullanın.
Cmdlet yapılandırmanızın içeriğini yükleyecek, parametrelerin listesini alacak ve ihtiyacınız olan her şeyi içeren yeni bir modül oluşturacaktır.

Bir modül oluşturduktan sonra, sürümü artırıp her değişiklik yaptığınızda sürüm notları ekleyebilir ve kendi [PowerShellGet deponuzda](https://powershellexplained.com/2018-03-03-Powershell-Using-a-NuGet-server-for-a-PSRepository/?utm_source=blog&utm_medium=blog&utm_content=psscriptrepo)yayınlayabilirsiniz.

Yapılandırmanızı (veya birden çok yapılandırmanızı) içeren bir bileşik kaynak modülü oluşturduktan sonra, bunları Azure'daki [Tek Kullanılabilir Yazma Deneyimi'nde](/azure/automation/compose-configurationwithcompositeresources) kullanabilir veya MOF dosyaları oluşturmak ve [MOF dosyalarını Azure Otomasyonuna yüklemek](/azure/automation/tutorial-configure-servers-desired-state#create-and-upload-a-configuration-to-azure-automation)için [DSC Yapılandırma komut dosyalarına](/powershell/scripting/dsc/configurations/configurations) ekleyebilirsiniz.
Ardından yapılandırmaları çekmek için sunucularınızı [şirket içinde](/azure/automation/automation-dsc-onboarding#physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances) veya [Azure'dan](/azure/automation/automation-dsc-onboarding#azure-virtual-machines) kaydedin.
Projeyle ilgili son güncelleme, PowerShell Galerisi'nden yapılandırma alma işlemini otomatikleştirmek için Azure Automation için [runbook'lar](https://www.powershellgallery.com/packages?q=DscGallerySamples) da yayımladı.

DSC için bileşik kaynakların otomatikleştirilmiş oluşturulmasını denemek için [PowerShell Galerisi'ni](https://www.powershellgallery.com/packages/compositeresource/) ziyaret edin ve çözümü indirin veya [belgeleri](https://github.com/microsoft/compositeresource)görüntülemek için "Proje Sitesi"ne tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Windows PowerShell İstenilen Durum Yapılandırmagenel Bakış](/powershell/scripting/dsc/overview/overview)
- [DSC Kaynakları](/powershell/scripting/dsc/resources/resources)
- [Yerel Yapılandırma Yöneticisini Yapılandırma](/powershell/scripting/dsc/managing-nodes/metaconfig)
