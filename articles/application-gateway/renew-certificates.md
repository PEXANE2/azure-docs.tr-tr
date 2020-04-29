---
title: Azure Application Gateway sertifikasını yenileme
description: Bir uygulama ağ geçidi dinleyicisiyle ilişkili bir sertifikayı yenilemeyi öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 8/15/2018
ms.author: victorh
ms.openlocfilehash: 4c0c0ab84e60335f58ac075459b95cfabb9135ac
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81311955"
---
# <a name="renew-application-gateway-certificates"></a>Application Gateway sertifikalarını Yenile

Bir noktada, TLS/SSL şifrelemesi için uygulama ağ geçidinizi yapılandırdıysanız sertifikalarınızı yenilemeniz gerekir.

Azure portal, Azure PowerShell ya da Azure CLı kullanarak bir dinleyiciyle ilişkili bir sertifikayı yenileyebilirsiniz:

## <a name="azure-portal"></a>Azure portal

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

Azure Application Gateway ile TLS yük boşaltma yapılandırma hakkında bilgi edinmek için bkz. [TLS yük boşaltma yapılandırma](application-gateway-ssl-portal.md)
