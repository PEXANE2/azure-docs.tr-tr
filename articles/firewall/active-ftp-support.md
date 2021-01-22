---
title: Azure Güvenlik Duvarı etkin FTP desteği
description: Azure Güvenlik duvarında etkin FTP varsayılan olarak devre dışıdır. PowerShell, CLı ve ARM şablonunu kullanarak etkinleştirebilirsiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 01/21/2021
ms.author: victorh
ms.openlocfilehash: 2ff61d06885c182454c99ee7e982a3a1a1f5013c
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98690727"
---
# <a name="azure-firewall-active-ftp-support"></a>Azure Güvenlik Duvarı etkin FTP desteği

Etkin FTP ile FTP sunucusu, belirlenen FTP istemci veri bağlantı noktasına veri bağlantısını başlatır. İstemci tarafı ağdaki güvenlik duvarları normalde iç istemci bağlantı noktasına bir dış bağlantı isteğini engeller. Daha fazla bilgi için, bkz. [ACTIVE FTP vs. pasıf FTP ve kesin bir açıklama](https://slacksite.com/other/ftp.html).

Varsayılan olarak, FTP komutu kullanılarak FTP sıçrama saldırılarına karşı korumak için etkin FTP desteği Azure Güvenlik duvarında devre dışıdır `PORT` . Ancak, Azure PowerShell, Azure CLı veya Azure ARM şablonunu kullanarak dağıtırken etkin FTP 'yi etkinleştirebilirsiniz.

## <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell kullanarak dağıtmak için `AllowActiveFTP` parametresini kullanın. Daha fazla bilgi için bkz. [ETKIN FTP 'ye Izin ver Ile güvenlik duvarı oluşturma](/powershell/module/az.network/new-azfirewall?view=azps-5.4.0#16---create-a-firewall-with-allow-active-ftp-).

## <a name="azure-cli"></a>Azure CLI

Azure CLı kullanarak dağıtmak için `--allow-active-ftp` parametresini kullanın. Daha fazla bilgi için bkz. [az Network Firewall Create](/cli/azure/ext/azure-firewall/network/firewall?view=azure-cli-latest#ext_azure_firewall_az_network_firewall_create-optional-parameters). 

## <a name="azure-resource-manager-arm-template"></a>Azure Resource Manager (ARM) şablonu

ARM şablonunu kullanarak dağıtmak için `AdditionalProperties` alanını kullanın:

```json
"additionalProperties": {
            "Network.FTP.AllowActiveFTP": "True"
        },
```
Daha fazla bilgi için bkz. [Microsoft. Network azureFirewalls](/azure/templates/microsoft.network/azurefirewalls).

## <a name="next-steps"></a>Sonraki adımlar

Azure Güvenlik duvarının nasıl dağıtılacağını öğrenmek için, bkz. [Azure PowerShell kullanarak Azure Güvenlik Duvarı dağıtma ve yapılandırma](deploy-ps.md).