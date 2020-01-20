---
title: Azure Application Gateway sertifikasını yenileme
description: Bir uygulama ağ geçidi dinleyicisiyle ilişkili bir sertifikayı yenilemeyi öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 8/15/2018
ms.author: victorh
ms.openlocfilehash: c7c27f00b9f8b4fdcd8f735f842edb8f66803c6a
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76278010"
---
# <a name="renew-application-gateway-certificates"></a>Application Gateway sertifikalarını Yenile

Bir noktada, uygulama ağ geçidinizi SSL şifrelemesi için yapılandırdıysanız sertifikalarınızı yenilemeniz gerekir.

Azure portal, Azure PowerShell ya da Azure CLı kullanarak bir dinleyiciyle ilişkili bir sertifikayı yenileyebilirsiniz:

## <a name="azure-portal"></a>Azure portalında

Portaldan bir dinleyici sertifikasını yenilemek için uygulama ağ geçidi dinleyicilerine gidin. Yenilenmesi gereken sertifikaya sahip olan dinleyiciye tıklayın ve ardından **Yenile veya seçili sertifikayı Düzenle**' ye tıklayın.

![Sertifikayı Yenile](media/renew-certificate/ssl-cert.png)

Yeni PFX sertifikanızı karşıya yükleyin, bir ad verin, parolayı yazın ve ardından **Kaydet**' e tıklayın.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure PowerShell kullanarak sertifikanızı yenilemek için aşağıdaki betiği kullanın:

```azurepowershell-interactive
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName <ResourceGroup> `
  -Name <AppGatewayName>

$password = ConvertTo-SecureString `
  -String "<password>" `
  -Force `
  -AsPlainText

set-AzApplicationGatewaySSLCertificate -Name <oldcertname> `
-ApplicationGateway $appgw -CertificateFile <newcertPath> -Password $password

Set-AzApplicationGateway -ApplicationGateway $appgw
```
## <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az network application-gateway ssl-cert update \
  -n "<CertName>" \
  --gateway-name "<AppGatewayName>" \
  -g "ResourceGroupName>" \
  --cert-file <PathToCerFile> \
  --cert-password "<password>"
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Application Gateway ile SSL yük boşaltma yapılandırma hakkında bilgi edinmek için bkz. [SSL yük boşaltma yapılandırma](application-gateway-ssl-portal.md)
