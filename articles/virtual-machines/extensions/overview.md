---
title: Azure sanal makine uzantıları ve özellikleri | Microsoft Docs
description: Azure VM uzantılarının ne olduğunu ve bunları Azure sanal makineleri ile nasıl kullanacağınızı öğrenin
services: virtual-machines-linux
documentationcenter: ''
author: roiyz-msft
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/30/2018
ms.author: roiyz
ms.openlocfilehash: 8c63df2d9a7e398fb9b67edd3b57a3ba06cbe7a1
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70084318"
---
# <a name="azure-virtual-machine-extensions-and-features"></a>Azure sanal makine uzantıları ve özellikleri
Azure sanal makinesi (VM) uzantıları, Azure VM 'lerinde dağıtım sonrası yapılandırma ve otomasyon görevleri sağlayan küçük uygulamalardır, var olan görüntüleri kullanabilir ve sonra, özel bir iş elde etmeniz için, dağıtımlarınızın bir parçası olarak bunları özelleştirebilirsiniz görüntü oluşturma.

Azure platformu, VM yapılandırması, izleme, güvenlik ve yardımcı program uygulamalarından çok çeşitli uzantılar barındırır. Yayımcılar bir uygulamayı alır, ardından bir uzantıya sarın ve yüklemeyi basitleştirerek, tüm yapmanız gereken zorunlu parametreler sağlamalıdır. 

 Birinci ve üçüncü taraf uzantılarının büyük bir seçimi vardır, uzantı deposundaki uygulama yoksa, Özel Betik uzantısını kullanabilir ve VM 'nizi kendi komut dosyalarınız ve komutlarınız ile yapılandırabilirsiniz.

Uzantıların kullanıldığı önemli senaryolar örnekleri:
* VM yapılandırması, VM yapılandırma aracılarını yüklemek ve VM 'nizi yapılandırmak için PowerShell DSC (Istenen durum yapılandırması), Chef, Pupevcil hayvan ve özel betik uzantılarını kullanabilirsiniz. 
* Symantec, ESET gibi AV ürünleri.
* Qualys, Rapid7, HPE gibi VM Güvenlik Açığı aracı.
* DynaTrace, Azure ağ Izleyicisi, Site24x7 ve Stacking gibi VM ve uygulama izleme araçları.

Uzantılar, yeni bir VM dağıtımıyla birlikte paketlenmiştir. Örneğin, bu uygulamalar daha büyük bir dağıtımın parçası olabilirler, VM sağlaması üzerinde uygulamaları yapılandırıyor veya desteklenen tüm uzantı çalıştırılan sistemlerin dağıtımı sonrasında çalıştırılabilir.

## <a name="how-can-i-find-what-extensions-are-available"></a>Hangi uzantıların kullanılabilir olduğunu nasıl bulabilirim?
Kullanılabilir uzantıları portaldaki VM dikey penceresinde görüntüleyebilirsiniz, Uzantılar altında bu yalnızca küçük bir miktarı temsil eder, tam liste için CLı araçlarını kullanabilir, [Linux IÇIN VM uzantılarını bulma](features-linux.md) ve [Windows Için VM uzantılarını bulma](features-windows.md)bölümüne bakın.

## <a name="how-can-i-install-an-extension"></a>Bir uzantıyı nasıl yükleyebilirim?
Azure VM uzantıları, Azure CLı, Azure PowerShell, Azure Resource Manager şablonları ve Azure portal kullanılarak yönetilebilir. Bir uzantıyı denemek için Azure portal gidebilir, Özel Betik uzantısını seçebilir, sonra bir komut/betik geçirebilir ve uzantıları çalıştırabilirsiniz.

Portala CLı veya Kaynak Yöneticisi şablonuna göre eklediğiniz aynı uzantıya isterseniz, bkz. [Windows Özel Betik uzantısı](custom-script-windows.md) ve [Linux özel Betik uzantısı](custom-script-linux.md)gibi farklı uzantı belgeleri.

## <a name="how-do-i-manage-extension-application-lifecycle"></a>Nasıl yaparım? uzantısı uygulama yaşam döngüsünü yönetmek mi istiyorsunuz?
Uzantıyı yüklemek veya silmek için bir VM 'ye doğrudan bağlanmanız gerekmez. Azure uzantı uygulaması yaşam döngüsü VM dışında yönetiliyor ve Azure platformuyla tümleştirildiği için, uzantının tümleşik durumunu da alırsınız.

## <a name="anything-else-i-should-be-thinking-about-for-extensions"></a>Uzantılar için göz önünde bulundurulması gereken başka bir şey var mı?
Uzantılar yükleme uygulamalar gibi bazı gereksinimler vardır, uzantılar için desteklenen Windows ve Linux Işletim sistemleri listesi ve Azure VM aracılarının yüklü olması gerekir. Bazı bağımsız VM Uzantısı uygulamalarının bir uç noktaya erişim gibi kendi ortam önkoşulları olabilir.

## <a name="next-steps"></a>Sonraki adımlar
* Linux aracısının ve uzantılarının nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Linux Için Azure VM uzantıları ve özellikleri](features-linux.md).
* Windows Konuk aracısının ve uzantılarının nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Windows Için Azure VM uzantıları ve özellikleri](features-windows.md).  
* Windows Konuk Aracısı 'nı yüklemek için bkz. [Azure Windows sanal makine aracısına genel bakış](agent-windows.md).  
* Linux Aracısı 'nı yüklemek için bkz. [Azure Linux sanal makine aracısına genel bakış](agent-linux.md).  

