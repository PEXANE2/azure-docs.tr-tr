---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/14/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 4e699707db02de07f3d1ebb7d1fa8d0575a10aa3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67836892"
---
FTP ve yerel Git, bir *dağıtım kullanıcısı*kullanarak bir Azure web uygulamasına dağıtılabilir. Dağıtım kullanıcınızı yapılandırdıktan sonra, tüm Azure dağıtımlarınız için kullanabilirsiniz. Hesap düzeyinde dağıtım kullanıcı adınız ve parolanız Azure abonelik kimlik bilgilerinizden farklıdır. 

Dağıtım kullanıcısını yapılandırmak için Azure Bulut BulutU'nda [az webapp dağıtım kullanıcı kümesi](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set) komutunu çalıştırın. Kullanıcı \<adı> \<ve parola> dağıtım kullanıcı adı ve parolasıyla değiştirin. 

- Kullanıcı adı Azure içinde benzersiz olmalıdır ve yerel Git itmeleri için '@' simgesi içermemelidir. 
- Parola, aşağıdaki üç öğeden ikisi olan en az sekiz karakter uzunluğunda olmalıdır: harfler, sayılar ve semboller. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

JSON çıkışı parolayı `null`. `'Conflict'. Details: 409` hatası alırsanız kullanıcı adını değiştirin. `'Bad Request'. Details: 400` hatası alırsanız daha güçlü bir parola kullanın. 

Web uygulamalarınızı dağıtmak için kullanmak üzere kullanıcı adınızı ve parolanızı kaydedin.
