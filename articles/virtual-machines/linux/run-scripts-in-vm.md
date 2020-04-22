---
title: Azure Linux VM'de komut dosyaları çalıştırma
description: Bu konu, sanal bir makine içinde komut dosyalarının nasıl çalıştırılabildiğini açıklar
services: automation
ms.service: virtual-machines-linux
author: bobbytreed
ms.author: robreed
ms.date: 05/02/2018
ms.topic: article
ms.openlocfilehash: 9dbfb3e76fe1c0e80dd8020b79f4edd6d32a24a3
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81758591"
---
# <a name="run-scripts-in-your-linux-vm"></a>Linux VM'nizde komut dosyaları çalıştırın

Görevleri otomatikleştirmek veya sorunları gidermek için vm komutları çalıştırmanız gerekebilir. Aşağıdaki makalede, VM'lerinizde komut dosyaları ve komutları çalıştırmak için kullanılabilen özellikler hakkında kısa bir genel bakış sunulmaktadır.

## <a name="custom-script-extension"></a>Özel Betik Uzantısı

[Özel Komut Dosyası Uzantısı](../extensions/custom-script-linux.md) öncelikle dağıtım sonrası yapılandırma ve yazılım yüklemesi için kullanılır.

* Azure sanal makinelerinde komut dosyalarını indirin ve çalıştırın.
* Azure Kaynak Yöneticisi şablonları, Azure CLI, REST API, PowerShell veya Azure portalı kullanılarak çalıştırılabilir.
* Komut dosyası dosyaları Azure depolama veya GitHub'dan indirilebilir veya Azure portalından çalıştırıldığında bilgisayarınızdan sağlanabilir.
* Windows makinelerinde PowerShell komut dosyasını ve Linux makinelerinde Bash komut dosyasını çalıştırın.
* Dağıtım sonrası yapılandırma, yazılım yüklemesi ve diğer yapılandırma veya yönetim görevleri için kullanışlıdır.

## <a name="run-command"></a>Çalıştır komutu

[Çalıştır Komutu](run-command.md) özelliği, komut dosyalarını kullanarak sanal makine ve uygulama yönetimi ve sorun giderme olanağı sağlar ve örneğin konuk güvenlik duvarı RDP veya SSH bağlantı noktası açık değilse, makineye erişilemese bile kullanılabilir.

* Azure sanal makinelerinde komut dosyaları çalıştırın.
* [Azure portalı](run-command.md), [REST API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand), [Azure CLI](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke)veya [PowerShell](https://docs.microsoft.com/powershell/module/az.compute/invoke-azvmruncommand) kullanılarak çalıştırılabilir
* Hızlı bir şekilde bir komut dosyası çalıştırın ve çıktıgörüntülemek ve Azure portalında gerektiği gibi tekrarlayın.
* Komut dosyası doğrudan yazılabilir veya yerleşik komut dosyalarından birini çalıştırabilirsiniz.
* Windows makinelerinde PowerShell komut dosyasını ve Linux makinelerinde Bash komut dosyasını çalıştırın.
* Sanal makine ve uygulama yönetimi ve ulaşılamayan sanal makinelerde komut dosyaları çalıştırmak için yararlıdır.

## <a name="hybrid-runbook-worker"></a>Karma Runbook Çalışanı

[Karma Runbook Worker,](../../automation/automation-hybrid-runbook-worker.md) genel makine, uygulama ve ortam yönetimini, kullanıcının otomasyon hesabında depolanan özel komut dosyalarıyla birlikte sağlar.

* Azure ve Azure olmayan makinelerde komut dosyaları çalıştırın.
* Azure portalı, Azure CLI, REST API, PowerShell, webhook kullanılarak çalıştırılabilir.
* Bir Otomasyon Hesabında depolanan ve yönetilen komut dosyaları.
* PowerShell, PowerShell İş Akışı, Python veya Grafik çalışma kitaplarını çalıştırın
* Komut dosyası çalışma süresinde zaman sınırı yok.
* Birden çok komut dosyası aynı anda çalıştırılabilir.
* Tam komut dosyası çıktısı döndürülür ve depolanır.
* İş geçmişi 90 gün için kullanılabilir.
* Komut dosyaları Yerel Sistem olarak veya kullanıcı tarafından sağlanan kimlik bilgileriyle çalıştırılabilir.
* [Manuel kurulum](../../automation/automation-windows-hrw-install.md) gerektirir

## <a name="serial-console"></a>Seri konsol

[Seri konsol,](serial-console.md) VM'ye bağlı bir klavyeye sahip olmaya benzer bir VM'ye doğrudan erişim sağlar.

* Azure sanal makinelerinde komutları çalıştırın.
* Azure portalındaki makineye metin tabanlı bir konsol kullanılarak çalıştırılabilir.
* Yerel bir kullanıcı hesabıyla makineye giriş yapın.
* Makinenin ağ veya işletim sistemi durumuna bakılmaksızın sanal makineye erişim gerektiğinde kullanışlıdır.

## <a name="next-steps"></a>Sonraki adımlar

VM'lerinizde komut dosyalarını ve komutları çalıştırmak için kullanılabilen farklı özellikler hakkında daha fazla bilgi edinin.

* [Özel Betik Uzantısı](../extensions/custom-script-linux.md)
* [Çalıştır Komutu](run-command.md)
* [Karma Runbook Çalışanı](../../automation/automation-hybrid-runbook-worker.md)
* [Seri konsol](serial-console.md)