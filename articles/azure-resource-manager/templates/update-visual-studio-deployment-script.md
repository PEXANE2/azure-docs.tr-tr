---
title: Visual Studio 'nun şablon dağıtım betiğini az PowerShell kullanacak şekilde Güncelleştir
description: Azurerd 'den az PowerShell 'e Visual Studio şablon dağıtım betiğini güncelleştirme
author: cweining
ms.topic: conceptual
ms.date: 01/31/2020
ms.author: cweining
ms.openlocfilehash: 357e0289f3237ed32b0801280316225ba5530282
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76963878"
---
# <a name="update-visual-studio-template-deployment-script-to-use-az-powershell-module"></a>Visual Studio şablon dağıtım betiğini az PowerShell modülünü kullanacak şekilde Güncelleştir

Visual Studio 16,4, şablon dağıtım betiğinde az PowerShell modülünün kullanılmasını destekler. Ancak, Visual Studio bu modülü otomatik olarak yüklemez. Az modülünü kullanmak için dört adım gerçekleştirmeniz gerekir:

1. [Azurerd modülünü kaldır](/powershell/azure/uninstall-az-ps#uninstall-the-azurerm-module)
1. [Install az Module](/powershell/azure/install-az-ps)
1. Visual Studio 'Yu 16,4 olarak güncelleştirme
1. Projenizdeki dağıtım betiğini güncelleştirin.

## <a name="update-visual-studio-to-164"></a>Visual Studio 'Yu 16,4 olarak güncelleştirme

Visual Studio yüklemenizi 16,4 veya sonraki bir sürüme güncelleştirin. Yükseltme sırasında Azure PowerShell bileşenin işaretli olmadığından emin olun. Galeri aracılığıyla az Module yükletiğinden Azurere modülünü yeniden yüklemek istemezsiniz.

Zaten 16,4 sürümüne yükselttiyseniz ve Azure PowerShell bileşen denetlendiyse, Visual Studio Yükleyicisi çalıştırarak kaldırabilirsiniz. Azure Iş yükünde veya tek tek bileşenler sayfasında Azure PowerShell bileşeni seçmeyin.

## <a name="update-the-deployment-script-in-your-project"></a>Projenizdeki dağıtım betiğini güncelleştirme

' Azurerd ' dizesinin tüm oluşumlarını dağıtım betiğinizdeki ' az ' ile değiştirin. Değişiklikleri görmek için bu [GİST](https://gist.github.com/cweining/d2da2479418ea403499c4306dcf4f619) 'teki düzeltmelere başvurabilirsiniz. Betikleri az modüle yükseltme hakkında daha fazla bilgi için bkz. [Azurerd 'Den az ' a geçiş Azure PowerShell](/powershell/azure/migrate-from-azurerm-to-az).

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio projesini kullanma hakkında bilgi edinmek için bkz. [Visual Studio aracılığıyla Azure Kaynak grubu projeleri oluşturma ve dağıtma](create-visual-studio-deployment-project.md).
