---
title: Azure CLI - Laboratuvardaki sanal makineyi durdurup silme
description: Bu makale, Azure DevTest Labs'daki bir laboratuvarda sanal bir makineyi durduran ve silen bir Azure CLI komut dosyası sağlar.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2020
ms.author: spelluru
ms.custom: mvc
ms.openlocfilehash: c4f315593da270155998c3c5bba8def2778f0d41
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76760465"
---
# <a name="use-azure-cli-to-stop-and-delete-a-virtual-machine-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs’deki bir sanal makineyi durdurmak ve silmek için Azure CLI’yı kullanma

Bu Azure CLI betiği, laboratuvardaki bir sanal makineyi (VM) durdurur ve siler. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/devtest-lab/stop-delete-virtual-machine-in-lab/stop-delete-virtual-machine-in-lab.sh "Stop and delete a VM in a lab")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır:

| Komut | Notlar |
|---|---|
| [az lab vm stop](/cli/azure/lab/vm?view=azure-cli-latest#az-lab-vm-stop) | Laboratuvardaki bir sanal makineyi (VM) durdurur. Bu işlemin tamamlanması biraz zaman alabilir. |
| [az lab vm delete](/cli/azure/lab/vm?view=azure-cli-latest#az-lab-vm-delete) | Laboratuvardaki bir sanal makineyi (VM) siler Bu işlemin tamamlanması biraz zaman alabilir. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure Lab Services CLI betik örnekleri, [Azure Lab Services CLI örnekleri](../samples-cli.md) içinde bulunabilir.
