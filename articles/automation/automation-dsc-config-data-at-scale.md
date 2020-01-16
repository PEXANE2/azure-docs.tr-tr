---
title: Ölçekli yapılandırma verileri-Azure Otomasyonu
description: Azure Automation 'da durum yapılandırması için ölçeklendirmeden verileri yapılandırmayı öğrenin.
keywords: DSC, PowerShell, yapılandırma, kurulum
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.date: 08/08/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1a77a366ee8e06b2d8c47eb3b47eeaf9ae809598
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76028294"
---
# <a name="configuration-data-at-scale"></a>Uygun ölçekte yapılandırma verileri

> Uygulama hedefi: Windows PowerShell 5,1

Yüzlerce veya binlerce sunucuyu yönetmek zor olabilir.
Müşteriler, en zor en iyi [yapılandırma verilerini](/powershell/scripting/dsc/configurations/configdata)yönetmekte olan geri bildirimde bulunmaları sağladı.
Konum, tür ve ortam gibi mantıksal yapılar arasında bilgi düzenleme.

> [!NOTE]
> Bu makale, açık kaynak topluluğu tarafından tutulan bir çözüme başvurur.
> Destek, Microsoft 'tan değil yalnızca GitHub işbirliği biçiminde kullanılabilir.

## <a name="community-project-datum"></a>Topluluk projesi: veri

Bu sorunu çözmek için [Datum](https://github.com/gaelcolas/Datum) adlı bir topluluk tarafından tutulan çözüm oluşturulmuştur.
Veri, diğer yapılandırma yönetimi platformlarından harika fikirler oluşturur ve PowerShell DSC için aynı tür çözümü uygular.
Bilgiler, mantıksal fikirlere göre [metin dosyalarında düzenlenir](https://github.com/gaelcolas/Datum#3-intended-usage) .
Örnekler şöyle olabilir:

- Genel olarak uygulanması gereken ayarlar
- Bir konumdaki tüm sunucular için uygulanması gereken ayarlar
- Tüm veritabanı sunucularına uygulanması gereken ayarlar
- Bireysel sunucu ayarları

Bu bilgiler, tercih ettiğiniz dosya biçiminde düzenlenmiştir (JSON, YAML veya PSD1).
Ardından cmdlet 'ler, içindeki her bir dosyadaki bilgileri bir sunucu veya sunucu rolünün tek bir görünümünde [birleştirerek](https://github.com/gaelcolas/Datum#datum-tree) yapılandırma verileri dosyaları oluşturmak için sağlanır.

Veri dosyaları oluşturulduktan sonra, MOF dosyaları oluşturmak ve [MOF dosyalarını Azure Otomasyonu 'na yüklemek](/azure/automation/tutorial-configure-servers-desired-state#create-and-upload-a-configuration-to-azure-automation)Için bunları [DSC yapılandırma betikleri](/powershell/scripting/dsc/configurations/write-compile-apply-configuration) ile birlikte kullanabilirsiniz.
Daha sonra, yapılandırma çekmek için [Şirket](/azure/automation/automation-dsc-onboarding#physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances) Içinden veya [Azure 'da](/azure/automation/automation-dsc-onboarding#azure-virtual-machines) sunucularınızı kaydettirin.

Datum denemek için [PowerShell Galerisi](https://www.powershellgallery.com/packages/datum/) ziyaret edin ve çözümü indirin veya "proje sitesi" ne tıklayarak [belgeleri](https://github.com/gaelcolas/Datum#2-getting-started--concepts)görüntüleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Windows PowerShell Istenen durum yapılandırmasına genel bakış](/powershell/scripting/dsc/overview/overview)
- [DSC kaynakları](/powershell/scripting/dsc/resources/resources)
- [Yerel Configuration Manager yapılandırma](/powershell/scripting/dsc/managing-nodes/metaconfig)
