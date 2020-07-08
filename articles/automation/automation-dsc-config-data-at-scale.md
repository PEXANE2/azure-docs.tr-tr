---
title: Verileri Azure Otomasyonu durum yapılandırması için ölçeklendirerek yapılandırma
description: Bu makalede, verileri Azure Otomasyonu durum yapılandırması için ölçeklendirerek nasıl yapılandıracağınız açıklanır.
keywords: DSC, PowerShell, yapılandırma, kurulum
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.date: 08/08/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: fbd721bc54ce2de6b7923b0f582c53ffc7da1a73
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83837034"
---
# <a name="configure-data-at-scale-for-azure-automation-state-configuration"></a>Verileri Azure Otomasyonu durum yapılandırması için ölçeklendirerek yapılandırma

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
Daha sonra, yapılandırma çekmek için [Şirket](/azure/automation/automation-dsc-onboarding#onboarding-physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances) Içinden veya [Azure 'da](/azure/automation/automation-dsc-onboarding#onboarding-azure-vms) sunucularınızı kaydettirin.

Datum denemek için [PowerShell Galerisi](https://www.powershellgallery.com/packages/datum/) ziyaret edin ve çözümü indirin veya "proje sitesi" ne tıklayarak [belgeleri](https://github.com/gaelcolas/Datum#2-getting-started--concepts)görüntüleyin.

## <a name="next-steps"></a>Sonraki adımlar

- PowerShell DSC 'yi anlamak için bkz. [Windows PowerShell Istenen durum yapılandırmasına genel bakış](/powershell/scripting/dsc/overview/overview).
- [DSC kaynaklarında](/powershell/scripting/dsc/resources/resources)PowerShell DSC kaynakları hakkında bilgi edinin.
- Yerel Configuration Manager yapılandırması hakkında daha fazla bilgi için bkz. [yerel Configuration Manager yapılandırma](/powershell/scripting/dsc/managing-nodes/metaconfig).
