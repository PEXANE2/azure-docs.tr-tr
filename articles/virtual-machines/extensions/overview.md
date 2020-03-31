---
title: Azure sanal makine uzantıları ve özellikleri
description: Azure VM uzantılarının ne olduğunu ve Azure sanal makineleriyle bunları nasıl kullanacağınızı öğrenin
services: virtual-machines-linux
documentationcenter: ''
author: axayjo
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/12/2019
ms.author: akjosh
ms.openlocfilehash: bf17f499c1e8339a1e9abb13cffd5e35c390f564
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74072972"
---
# <a name="azure-virtual-machine-extensions-and-features"></a>Azure sanal makine uzantıları ve özellikleri
Azure sanal makine (VM) uzantıları, Azure VM'lerinde dağıtım sonrası yapılandırma ve otomasyon görevleri sağlayan küçük uygulamalardır, varolan görüntüleri kullanabilir ve dağıtımlarınızın bir parçası olarak özelleştirerek sizi özel işlerden çıkarabilirsiniz görüntü oluşturma.

Azure platformu, VM yapılandırması, izleme, güvenlik ve yardımcı program uygulamalarından oluşan birçok uzantıya ev sahipliği yapmaktadır. Yayıncılar bir uygulama alır, sonra bir uzantıya sarın ve yüklemeyi basitleştirir, bu nedenle tek yapmanız gereken zorunlu parametreleri sağlamaktır. 

 Uzantı deposundaki uygulama yoksa, özel komut dosyası uzantısını kullanabilir ve VM'nizi kendi komut dosyalarınız ve komutlarınızla yapılandırabilirsiniz.

Uzantıların kullanıldığı önemli senaryoörnekleri:
* VM yapılandırma, VM yapılandırma aracıları yüklemek ve VM yapılandırmak için Powershell DSC (İstenilen Durum Yapılandırma), Şef, Kukla ve Özel Komut Dosyası Uzantıları kullanabilirsiniz. 
* Symantec, ESET gibi AV ürünleri.
* Qualys, Rapid7, HPE gibi VM güvenlik açığı aracı.
* DynaTrace, Azure Ağ İzleyicisi, Site24x7 ve Stackify gibi VM ve Uygulama izleme aracı.

Uzantılar yeni bir VM dağıtımıyla birlikte kullanılabilir. Örneğin, vm hükmü üzerinde uygulamaları yapılandırma, daha büyük bir dağıtım parçası olabilir veya dağıtım sonrası desteklenen uzantılı sistemlere karşı çalıştırın.

## <a name="how-can-i-find-what-extensions-are-available"></a>Hangi uzantıların kullanılabilir olduğunu nasıl bulabilirim?
Portal VM bıçak mevcut uzantıları görüntüleyebilirsiniz, uzantıları altında, bu sadece küçük bir miktar temsil eder, tam liste için, CLI araçlarını kullanabilirsiniz, [Linux için VM Uzantıları Keşfetmek](features-linux.md) ve Windows için [VM Uzantıları Keşfetmek](features-windows.md)bakın .

## <a name="how-can-i-install-an-extension"></a>Uzantıyı nasıl yükleyebilirim?
Azure VM uzantıları, Azure CLI, Azure PowerShell, Azure Kaynak Yöneticisi şablonları ve Azure portalı kullanılarak yönetilebilir. Uzantıyı denemek için Azure portalına gidebilir, Özel Komut Dosyası Uzantısı'nı seçebilir, ardından komut /komut dosyasını geçirebilir ve uzantıları çalıştırabilirsiniz.

CLI veya Resource Manager şablonu yla portala eklediğiniz uzantıyı aynı şekilde yapmak istiyorsanız, [Windows Özel Komut Dosyası Uzantısı](custom-script-windows.md) ve Linux Özel Komut Dosyası [Uzantısı](custom-script-linux.md)gibi farklı uzantı belgelerine bakın.

## <a name="how-do-i-manage-extension-application-lifecycle"></a>Uzantılı uygulama yaşam döngüsünü nasıl yönetirim?
Uzantıyı yüklemek veya silmek için doğrudan bir VM'ye bağlanmanız gerekmez. Azure uzantısı uygulama yaşam döngüsü VM dışında yönetildiği ve Azure platformuna entegre olduğundan, uzantının tümleşik durumunu da elde edersiniz.

## <a name="anything-else-i-should-be-thinking-about-for-extensions"></a>Uzatmalar için düşünmem gereken başka bir şey var mı?
Uzantılar, tüm uygulamalar gibi uygulamaları yükler, bazı gereksinimler vardır, uzantılar için desteklenen Windows ve Linux İş'lerinin bir listesi vardır ve Azure VM aracılarının yüklü olması gerekir. Bazı bireysel VM uzantı uygulamaları, bitiş noktasına erişim gibi kendi çevresel ön koşullara sahip olabilir.

## <a name="troubleshoot-extensions"></a>Uzantı sorunlarını giderme

Her uzantı için sorun giderme bilgileri, uzantına genel bakışta **Sorun Giderme ve destek** bölümünde bulunabilir. Kullanılabilir sorun giderme bilgilerinin bir listesi aşağıda veda edilebilebilir:

| Ad Alanı | Sorun giderme |
|-----------|-----------------|
| microsoft.azure.monitoring.dependencyagent.dependencyagent.dependencyagentlinux | [Linux için Azure Monitör Bağımlılığı](agent-dependency-linux.md#troubleshoot-and-support) |
| microsoft.azure.monitoring.dependencyagent.dependencyagent.dependencyagentwindows | [Windows için Azure Monitör Ükacı](agent-dependency-windows.md#troubleshoot-and-support) |
| microsoft.azure.security.azurediskencryptionforlinux | [Linux için Azure Disk Şifrelemesi](azure-disk-enc-linux.md#troubleshoot-and-support) |
| microsoft.azure.security.azurediskşifreleme | [Windows için Azure Disk Şifrelemesi](azure-disk-enc-windows.md#troubleshoot-and-support) |
| microsoft.compute.customscriptuzan | [Windows için Özel Komut Dosyası](custom-script-windows.md#troubleshoot-and-support) |
| microsoft.ostcextensions.customscriptforlinux | [Linux için İstenen Devlet Yapılandırması](dsc-linux.md#troubleshoot-and-support) |
| microsoft.powershell.dsc | [Windows için İstenilen Durum Yapılandırması](dsc-windows.md#troubleshoot-and-support) |
| microsoft.hpccompute.nvidiagpudriverlinux | [Linux için NVIDIA GPU Sürücü Uzantısı](hpccompute-gpu-linux.md#troubleshoot-and-support) |
| microsoft.hpccompute.nvidiagpudriverwindows | [Windows için NVIDIA GPU Sürücü Uzantısı](hpccompute-gpu-windows.md#troubleshoot-and-support) |
| microsoft.azure.security.iaasantimalware | [Windows için Kötü Amaçlı Yazılımdan Koruma Uzantısı](iaas-antimalware-windows.md#troubleshoot-and-support) |
| microsoft.enterprisecloud.monitoring.omsagentforlinux | [Linux için Azure Monitör](oms-linux.md#troubleshoot-and-support)
| microsoft.enterprisecloud.monitoring.microsoftmonitoringagent | [Windows için Azure Monitör](oms-windows.md#troubleshoot-and-support) |
| stackify.linuxagent.extension.stackifylinuxagent uzantı | [Linux için Stackify Retrace](stackify-retrace-linux.md#troubleshoot-and-support) |
| vmaccessforlinux.microsoft.ostcextensions | [Linux için parolayı (VMAccess) sıfırlama](vmaccess.md#troubleshoot-and-support) |
| microsoft.recoveryservices.vmsnapshot | [Linux için Anlık Görüntü](vmsnapshot-linux.md#troubleshoot-and-support) |
| microsoft.recoveryservices.vmsnapshot | [Windows için Anlık Görüntü](vmsnapshot-windows.md#troubleshoot-and-support) |


## <a name="next-steps"></a>Sonraki adımlar
* Linux Aracısı ve Uzantıları'nın nasıl çalıştığı hakkında daha fazla bilgi için [Azure VM uzantıları ve Linux özellikleri ne](features-linux.md)redesilere bakın.
* Windows Konuk Aracı'nın ve Uzantıların nasıl çalıştığı hakkında daha fazla bilgi için [Windows için Azure VM uzantıları ve özellikleri](features-windows.md)ne redeleyin.  
* Windows Guest Agent'ı yüklemek için [Azure Windows Sanal Makine Aracısına Genel Bakış'a](agent-windows.md)bakın.  
* Linux Aracısını yüklemek için [Azure Linux Sanal Makine Aracısına Genel Bakış'a](agent-linux.md)bakın.  

