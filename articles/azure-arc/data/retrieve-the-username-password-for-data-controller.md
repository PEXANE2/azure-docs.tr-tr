---
title: Arc Veri Denetleyicisi'ne bağlanmak için kullanıcı adı ve parolayı alma
description: Arc Veri Denetleyicisi'ne bağlanmak için kullanıcı adı ve parolayı alma
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 8cabb48e3620f1a17d1bb9b87e1646ce2793143b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91761711"
---
# <a name="retrieve-the-user-name-and-password-to-connect-to-the-arc-data-controller"></a>Arc Veri Denetleyicisi'ne bağlanmak için kullanıcı adı ve parolayı alma

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

Diğer [senaryoları](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/active-directory-domain-services/scenarios.md) deneyin
