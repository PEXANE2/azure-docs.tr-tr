---
title: Az PowerShell'i kullanmak için Visual Studio'nun şablon dağıtım komut dosyasını güncelleştirin
description: Visual Studio şablon dağıtım komut dosyasını AzureRM'den Az PowerShell'e güncelleştirme
author: cweining
ms.topic: conceptual
ms.date: 01/31/2020
ms.author: cweining
ms.openlocfilehash: 357e0289f3237ed32b0801280316225ba5530282
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76963878"
---
# <a name="update-visual-studio-template-deployment-script-to-use-az-powershell-module"></a>Az PowerShell modüllerini kullanmak için Visual Studio şablon dağıtım komut dosyasını güncelleştirin

Visual Studio 16.4 şablon dağıtım komut dosyasında Az PowerShell modüllerini kullanmayı destekler. Ancak Visual Studio bu modülü otomatik olarak yüklemez. Az modülünü kullanmak için dört adım atmalısınız:

1. [AzureRM modüllerini kaldırma](/powershell/azure/uninstall-az-ps#uninstall-the-azurerm-module)
1. [Az modül yükleme](/powershell/azure/install-az-ps)
1. Visual Studio'yı 16.4'e güncelleştirin
1. Projenizdeki dağıtım komut dosyasını güncelleştirin.

## <a name="update-visual-studio-to-164"></a>Visual Studio'yı 16.4'e güncelleştirin

Visual Studio yüklemenizi sürüm 16.4 veya sonraki sürümle güncelleyin. Yükseltme sırasında Azure PowerShell bileşeninin kontrol olmadığından emin olun. Az modüllerini galeri üzerinden yüklediğiniz için AzureRM modüllerini yeniden yüklemek istemezsiniz.

Zaten 16.4'e yükselttiyseniz ve Azure PowerShell bileşeni denetlendiyse, Visual Studio Installer'ı çalıştırarak kaldırabilirsiniz. Azure İş Yükü'nde veya tek tek bileşenler sayfasında Azure PowerShell bileşenini seçmeyin.

## <a name="update-the-deployment-script-in-your-project"></a>Projenizdeki dağıtım komut dosyasını güncelleştirme

'AzureRm' dizesinin tüm oluşumlarını dağıtım komut dosyanızda 'Az' ile değiştirin. Değişiklikleri görmek için bu [alt](https://gist.github.com/cweining/d2da2479418ea403499c4306dcf4f619) taki revizyonlara başvurabilirsiniz. Komut dosyalarını Az modülüne yükseltme hakkında daha fazla bilgi için [azure'u Azure PowerShell'den Az'a geçirin.](/powershell/azure/migrate-from-azurerm-to-az)

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio projesini kullanma hakkında bilgi edinmek için [Visual Studio aracılığıyla Azure kaynak grubu projeleri oluşturma ve dağıtma](create-visual-studio-deployment-project.md)hakkında bilgi edinin.
