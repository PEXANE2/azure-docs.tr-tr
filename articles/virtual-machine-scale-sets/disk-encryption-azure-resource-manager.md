---
title: Azure Kaynak Yöneticisi şablonları ile Sanal Makine Ölçeği Seti oluşturma ve şifreleme
description: Bu hızlı başlangıçta, Sanal Makine Ölçeği Kümesi oluşturmak ve şifrelemek için Azure Kaynak Yöneticisi şablonlarını nasıl kullanacağınızı öğrenirsiniz
author: msmbaldwin
ms.author: mbaldwin
ms.service: virtual-machine-scale-sets
ms.topic: quickstart
ms.date: 10/10/2019
ms.openlocfilehash: aeba9aee8dd10b1245fb0a1e1cc98a1951382c72
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "72530003"
---
# <a name="encrypt-virtual-machine-scale-sets-with-azure-resource-manager"></a>Azure Kaynak Yöneticisi ile sanal makine ölçek kümelerini şifreleme

Azure Kaynak Yöneticisi şablonlarını kullanarak Linux sanal makine ölçek kümelerini şifreleyebilir veya şifresini çözebilirsiniz.

## <a name="deploying-templates"></a>Şablonları dağıtma

İlk olarak, senaryonuza uyan şablonu seçin.

- [Çalışan Linux sanal makine ölçeği nde disk şifrelemeyi etkinleştirme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-linux)

- [Çalışan Windows sanal makine ölçeği kümesinde disk şifrelemeyi etkinleştirme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-windows)

  - [Sanal makine ölçeğinde bir linux vm setini atlama kutusuyla dağıtın ve Linux sanal makine ölçek kümelerinde şifreleme sağlar](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox)

  - [Sanal makine ölçeğinde bir Windows VM kümesini atlama kutusuyla dağıtın ve Windows sanal makine ölçek kümelerinde şifreleme sağlar](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox)

- [Çalışan Linux sanal makine ölçeği nde disk şifrelemeyi devre dışı bırak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux)

- [Çalışan Windows sanal makine ölçeği kümesinde disk şifrelemeyi devre dışı bırak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-windows)

Ardından aşağıdaki adımları izleyin:

     1. Click **Deploy to Azure**.
     2. Fill in the required fields then agree to the terms and conditions.
     3. Click **Purchase** to deploy the template.

## <a name="next-steps"></a>Sonraki adımlar

- [Sanal makine ölçek kümeleri için Azure Disk Şifrelemesi](disk-encryption-overview.md)
- [Azure CLI'yi kullanarak sanal makine ölçeği kümelerini şifreleme](disk-encryption-cli.md)
- [Azure PowerShell'i kullanarak sanal makine ölçeği kümelerini şifreleme](disk-encryption-powershell.md)
- [Azure Disk Şifreleme için önemli bir kasa oluşturma ve yapılandırma](disk-encryption-key-vault.md)
- [Sanal makine ölçeği kümesi uzantısı sıralaması ile Azure Disk Şifreleme'yi kullanma](disk-encryption-extension-sequencing.md)