---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 01/16/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 98172c2c487488a72bbfdd3a8205ac7d8668db60
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035755"
---
CA sertifikası oluşturun.

  ```
  ipsec pki --gen --outform pem > caKey.pem
  ipsec pki --self --in caKey.pem --dn "CN=VPN CA" --ca --outform pem > caCert.pem
  ```

CA sertifikasını Base64 biçiminde yazdır. Bu, Azure tarafından desteklenen biçimdir. Daha sonra bunu P2S yapılandırmanızın bir parçası olarak Azure 'a yükleyeceksiniz.

  ```
  openssl x509 -in caCert.pem -outform der | base64 -w0 ; echo
  ```

Kullanıcı sertifikası oluşturun.

  ```
  export PASSWORD="password"
  export USERNAME="client"

  ipsec pki --gen --outform pem > "${USERNAME}Key.pem"
  ipsec pki --pub --in "${USERNAME}Key.pem" | ipsec pki --issue --cacert caCert.pem --cakey caKey.pem --dn "CN=${USERNAME}" --san "${USERNAME}" --flag clientAuth --outform pem > "${USERNAME}Cert.pem"
  ```

Kullanıcı sertifikasını içeren bir P12 paketi oluşturun. Bu paket, istemci yapılandırma dosyalarıyla çalışırken sonraki adımlarda kullanılacaktır.

  ```
  openssl pkcs12 -in "${USERNAME}Cert.pem" -inkey "${USERNAME}Key.pem" -certfile caCert.pem -export -out "${USERNAME}.p12" -password "pass:${PASSWORD}"
  ```