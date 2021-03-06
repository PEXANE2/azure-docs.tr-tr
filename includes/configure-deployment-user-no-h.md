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
ms.openlocfilehash: a972b4738ce5646a1ee9eed6495bdc43a40826fd
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102219054"
---
FTP ve yerel git, bir *dağıtım kullanıcısı* kullanarak bir Azure Web uygulamasına dağıtabilir. Dağıtım kullanıcısını yapılandırdıktan sonra tüm Azure dağıtımlarınız için kullanabilirsiniz. Hesap düzeyinde dağıtım Kullanıcı adınız ve parolanız, Azure aboneliği kimlik bilgilerinizden farklı. 

Dağıtım kullanıcısını yapılandırmak için, Azure Cloud Shell bölümünde [az WebApp Deployment User set](/cli/azure/webapp/deployment/user#az-webapp-deployment-user-set) komutunu çalıştırın. \<username>Ve öğesini \<password> bir dağıtım kullanıcısı Kullanıcı adı ve parolasıyla değiştirin. 

- Kullanıcı adı Azure içinde benzersiz olmalıdır ve yerel git gönderimleri için ' \@ ' sembolünü içermemelidir. 
- Parola en az sekiz karakter uzunluğunda olmalıdır ve şu üç öğeden ikisi vardır: harfler, rakamlar ve semboller. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

JSON çıktısı parolayı olarak gösterir `null` . `'Conflict'. Details: 409` hatası alırsanız kullanıcı adını değiştirin. `'Bad Request'. Details: 400` hatası alırsanız daha güçlü bir parola kullanın. 

Web uygulamalarınızı dağıtmak için kullanmak üzere Kullanıcı adınızı ve parolanızı kaydedin.
