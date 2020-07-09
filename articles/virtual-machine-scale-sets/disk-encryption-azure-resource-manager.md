---
title: Azure Resource Manager şablonlarıyla bir sanal makine ölçek kümesi oluşturma ve şifreleme
description: Bu hızlı başlangıçta, bir sanal makine ölçek kümesi oluşturmak ve şifrelemek için Azure Resource Manager şablonlarını kullanmayı öğreneceksiniz
author: ju-shim
ms.author: jushiman
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: disks
ms.date: 10/10/2019
ms.reviewer: mimckitt
ms.custom: mimckitt
ms.openlocfilehash: 4284e94f8d8d0effd160c5048f54fcbede417e38
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86129755"
---
# <a name="encrypt-virtual-machine-scale-sets-with-azure-resource-manager"></a>Sanal makine ölçek kümelerini Azure Resource Manager ile şifreleyin

Azure Resource Manager şablonları kullanarak Linux sanal makine ölçek kümelerini şifreleyebilir veya şifrelerini çözebilirsiniz.

## <a name="deploying-templates"></a>Şablonları dağıtma

İlk olarak, senaryonuza uygun olan şablonu seçin.

- [Çalışan bir Linux sanal makine ölçek kümesi üzerinde disk şifrelemeyi etkinleştir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-linux)

- [Çalışan bir Windows sanal makine ölçek kümesi üzerinde disk şifrelemeyi etkinleştir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-windows)

  - [Bir sıçrama kutusu ile Linux VM 'leri sanal makine ölçek kümesi dağıtma ve Linux sanal makine ölçek kümelerinde şifrelemeyi mümkün](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox)

  - [Bir sıçrama kutusu ile bir sanal makine ölçek kümesi dağıtın ve Windows sanal makine ölçek kümelerinde şifrelemeyi etkinleştirilir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox)

- [Çalışan bir Linux sanal makine ölçek kümesi üzerinde disk şifrelemesini devre dışı bırak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux)

- [Çalışan bir Windows sanal makine ölçek kümesi üzerinde disk şifrelemesini devre dışı bırak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-windows)

Ardından aşağıdaki adımları izleyin:

1. **Azure’a dağıt**’a tıklayın.
2. Gerekli alanları doldurup hüküm ve koşulları kabul edin.
3. Şablonu dağıtmak için **satın al** ' a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Sanal Makine Ölçek Kümeleri için Azure disk şifrelemesi](disk-encryption-overview.md)
- [Azure CLı kullanarak sanal makine ölçek kümelerini şifreleme](disk-encryption-cli.md)
- [Azure PowerShell kullanarak bir sanal makine ölçek kümelerini şifreleme](disk-encryption-powershell.md)
- [Azure disk şifrelemesi için Anahtar Kasası oluşturma ve yapılandırma](disk-encryption-key-vault.md)
- [Sanal makine ölçek kümesi uzantısı sıralaması ile Azure disk şifrelemesini kullanma](disk-encryption-extension-sequencing.md)
