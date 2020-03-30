---
title: Ölçekte yapılandırma verileri - Azure Otomasyonu
description: Azure Otomasyonu'nda durum yapılandırması için verileri ölçekte nasıl yapılandırıştırmayı öğrenin.
keywords: dsc, powershell, yapılandırma, kurulum
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.date: 08/08/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1a77a366ee8e06b2d8c47eb3b47eeaf9ae809598
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76028294"
---
# <a name="configuration-data-at-scale"></a>Uygun ölçekte yapılandırma verileri

> Geçerlidir: Windows PowerShell 5.1

Yüzlerce veya binlerce sunucuyönetmek zor olabilir.
Müşteriler, en zor yönü aslında [yapılandırma verilerini](/powershell/scripting/dsc/configurations/configdata)yönetmek olduğunu geribildirim sağlamıştır.
Konum, tür ve ortam gibi mantıksal yapılar arasında bilgi düzenleme.

> [!NOTE]
> Bu makalede, Açık Kaynak topluluğu tarafından korunan bir çözüm anlamına gelir.
> Destek, Microsoft'tan değil, yalnızca GitHub işbirliği biçiminde kullanılabilir.

## <a name="community-project-datum"></a>Topluluk projesi: Datum

Bu sorunu çözmek için [Datum](https://github.com/gaelcolas/Datum) adında bir topluluk tarafından sürdürülen bir çözüm oluşturuldu.
Datum, diğer yapılandırma yönetim platformlarından harika fikirler üzerine kuruludur ve PowerShell DSC için aynı tür çözümü uygular.
Bilgiler, mantıksal fikirlere dayalı [metin dosyalarına düzenlenir.](https://github.com/gaelcolas/Datum#3-intended-usage)
Örnekler olacaktır:

- Genel olarak uygulanması gereken ayarlar
- Bir konumdaki tüm sunuculara uygulanması gereken ayarlar
- Tüm veritabanı sunucularına uygulanması gereken ayarlar
- Tek tek sunucu ayarları

Bu bilgiler tercih ettiğiniz dosya biçiminde (JSON, Yaml veya PSD1) düzenlenir.
Daha sonra cmdlets bir sunucu veya sunucu rolü tek bir görünüm için her dosyadaki [bilgileri birleştirerek](https://github.com/gaelcolas/Datum#datum-tree) yapılandırma veri dosyaları oluşturmak için sağlanır.

Veri dosyaları oluşturulduktan sonra, MOF dosyaları oluşturmak ve [MOF dosyalarını Azure Otomasyonu'na yüklemek için](/azure/automation/tutorial-configure-servers-desired-state#create-and-upload-a-configuration-to-azure-automation) [bunları DSC Configuration komut dosyalarıyla](/powershell/scripting/dsc/configurations/write-compile-apply-configuration) kullanabilirsiniz.
Ardından yapılandırmaları çekmek için sunucularınızı [şirket içinde](/azure/automation/automation-dsc-onboarding#physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances) veya [Azure'dan](/azure/automation/automation-dsc-onboarding#azure-virtual-machines) kaydedin.

Datum'u denemek için [PowerShell Galerisi'ni](https://www.powershellgallery.com/packages/datum/) ziyaret edin ve çözümü indirin veya [belgeleri](https://github.com/gaelcolas/Datum#2-getting-started--concepts)görüntülemek için "Proje Sitesi"ne tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Windows PowerShell İstenilen Durum Yapılandırmagenel Bakış](/powershell/scripting/dsc/overview/overview)
- [DSC Kaynakları](/powershell/scripting/dsc/resources/resources)
- [Yerel Yapılandırma Yöneticisini Yapılandırma](/powershell/scripting/dsc/managing-nodes/metaconfig)
