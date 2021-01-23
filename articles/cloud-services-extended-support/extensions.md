---
title: Cloud Services uzantıları (genişletilmiş destek)
description: Cloud Services uzantıları (genişletilmiş destek)
ms.topic: how-to
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: c2eddf75f99b751773220ae677d66fe8c09abb0e
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2021
ms.locfileid: "98744830"
---
# <a name="extensions-for-cloud-services-extended-support"></a>Cloud Services uzantıları (genişletilmiş destek)

Uzantılar, roller üzerinde dağıtım sonrası yapılandırma ve otomasyon görevleri sağlayan küçük uygulamalardır. Örneğin, Uzak Masaüstü uzantısı 'nı kullanarak, bulut hizmeti (genişletilmiş destek) dağıtımı sırasında rolünüzde bir Uzak Masaüstü bağlantısı sağlayabilirsiniz.  

## <a name="remote-desktop-extension"></a>Uzak Masaüstü uzantısı

Uzak Masaüstü, Azure 'da çalışan bir rolün masaüstüne erişmenizi sağlar. Çalışırken uygulamanızdaki sorunları gidermek ve tanılamak için Uzak Masaüstü bağlantısı kullanabilirsiniz.

Hizmet tanımınıza uzak masaüstü modüllerini ekleyerek veya Uzak Masaüstü uzantısı aracılığıyla, geliştirme sırasında rolünüzde uzak masaüstü bağlantısını etkinleştirebilirsiniz. 

Daha fazla bilgi için bkz [. Azure Portal uzak masaüstünü yapılandırma](enable-rdp.md)

## <a name="windows-azure-diagnostics-extension"></a>Windows Azure Tanılama uzantısı

Herhangi bir bulut hizmeti için önemli performans ölçümlerini izleyebilirsiniz. Her bulut hizmeti rolü en az veri toplar: CPU kullanımı, ağ kullanımı ve disk kullanımı. Bulut hizmetinde bir role uygulanan Microsoft. Azure. Diagnostics uzantısı varsa, bu rol ek veri noktaları toplayabilir. 

Temel izleme ile, rol örneklerinden alınan performans sayacı verileri, 3 dakikalık aralıklarla örneklenir ve toplanır. Bu temel izleme verileri depolama hesabınızda depolanmaz ve bununla ilişkili ek bir ücret yoktur. 

Gelişmiş izleme sayesinde ek ölçümler, 5 dakika, 1 saat ve 12 saat aralıklarıyla örneklenir ve toplanır. Toplanan veriler bir depolama hesabında, tablolarda depolanır ve 10 gün sonra temizlenir. Kullanılan depolama hesabı role göre yapılandırılır; farklı roller için farklı depolama hesapları kullanabilirsiniz. 

Windows Azure Tanılama uzantısı, [PowerShell](deploy-powershell.md) veya [ARM şablonu](deploy-template.md) aracılığıyla Cloud Services (genişletilmiş destek) için etkinleştirilebilir


## <a name="next-steps"></a>Sonraki adımlar 
- Cloud Services için [dağıtım önkoşullarını](deploy-prerequisite.md) gözden geçirin (genişletilmiş destek).
- Cloud Services için [sık sorulan soruları](faq.md) gözden geçirin (genişletilmiş destek).
- [Azure Portal](deploy-portal.md), [PowerShell](deploy-powershell.md), [şablon](deploy-template.md) veya [Visual Studio](deploy-visual-studio.md)kullanarak bir bulut hizmeti (genişletilmiş destek) dağıtın.