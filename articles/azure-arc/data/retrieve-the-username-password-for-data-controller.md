---
title: Arc veri denetleyicisine bağlanmak için Kullanıcı adını ve parolayı alma
description: Arc veri denetleyicisine bağlanmak için Kullanıcı adını ve parolayı alma
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 1c33cf21be0b4b1ea39a568d6df9fd90507dd454
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941495"
---
# <a name="retrieve-the-user-name-and-password-to-connect-to-the-arc-data-controller"></a>Arc veri denetleyicisine bağlanmak için Kullanıcı adını ve parolayı alma

Veri denetleyicisi için Kullanıcı adını ve parolayı almanız gereken bir durumda olabilirsiniz. Bu komutlar, çalıştırdığınızda ihtiyacınız olan komutlardır. 

```console
azdata login
```

Küme için Kubernetes yöneticisiyseniz. Bu nedenle, Kubernetes gizliliğini almak için komut çalıştırma ayrıcalıklarına sahip olduğunuz için, Azure Arc 'ın devam ettiği bilgileri burada depolar.

> [!NOTE]
>  Veri denetleyicisinin oluşturulduğu ad alanı için farklı bir ad kullandıysanız, `-n arc` aşağıdaki komutlarda bulunan parametreyi, veri denetleyicisini oluşturduğunuz ad alanının adını kullanacak şekilde değiştirdiğinizden emin olun.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="linux"></a>Linux

Kullanıcı adını almak için aşağıdaki komutu çalıştırın:

```console
kubectl -n arc get secret controller-login-secret -o=jsonpath="{.data['username']}" | base64 -d
```

Parolayı almak için aşağıdaki komutu çalıştırın:

```console
kubectl -n arc get secret controller-login-secret -o=jsonpath="{.data['password']}" | base64 -d
```

## <a name="powershell"></a>PowerShell

Kullanıcı adını almak için aşağıdaki komutu çalıştırın:

```console
[Text.Encoding]::Utf8.GetString([Convert]::FromBase64String((kubectl -n arc get secret controller-login-secret -o=jsonpath="{.data['username']}")))
```

Parolayı almak için aşağıdaki komutu çalıştırın:

```console
[Text.Encoding]::Utf8.GetString([Convert]::FromBase64String((kubectl -n arc get secret controller-login-secret -o=jsonpath="{.data['password']}")))
```

## <a name="next-steps"></a>Sonraki adımlar

Diğer [senaryoları](https://github.com/microsoft/Azure-data-services-on-Azure-Arc/tree/master/scenarios) deneyin
