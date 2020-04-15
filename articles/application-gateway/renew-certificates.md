---
title: Azure Uygulama Ağ Geçidi sertifikasını yenileme
description: Uygulama ağ geçidi dinleyicisiyle ilişkili bir sertifikayı nasıl yenileyeceğinizi öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 8/15/2018
ms.author: victorh
ms.openlocfilehash: 4c0c0ab84e60335f58ac075459b95cfabb9135ac
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81311955"
---
# <a name="renew-application-gateway-certificates"></a>Uygulama Ağ Geçidi sertifikalarını yenileme

Bir noktada, uygulama ağ geçidinizi TLS/SSL şifrelemesi için yapılandırıyorsanız sertifikalarınızı yenilemeniz gerekir.

Azure portalı, Azure PowerShell veya Azure CLI'yi kullanarak bir dinleyiciyle ilişkili bir sertifikayı yenileyebilirsiniz:

## <a name="azure-portal"></a>Azure portal

Portaldan bir dinleyici sertifikası yenilemek için uygulama ağ geçidi dinleyicilerinize gidin. Yenilenmesi gereken bir sertifikaya sahip dinleyiciyi tıklatın ve ardından **Seçili Sertifikayı Yenile veya Yeniden Lendir'i**tıklatın.

![Sertifikayı yenileme](media/renew-certificate/ssl-cert.png)

Yeni PFX sertifikanızı yükleyin, ona bir ad verin, parolayı yazın ve sonra **Kaydet'i**tıklatın.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure PowerShell kullanarak sertifikanızı yenilemek için aşağıdaki komut dosyasını kullanın:

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

AZURE Uygulama Ağ Geçidi ile TLS Boşaltma'yı nasıl yapılandıracağımı öğrenmek için [TLS Boşaltmayı Yapılandırma](application-gateway-ssl-portal.md)
