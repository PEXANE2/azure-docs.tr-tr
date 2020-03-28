---
title: Azure CLI Betiği Dağıtma Örneği
description: Azure Komut Satırı Arabirimi 'ni (CLI) kullanarak Azure'da güvenli bir Hizmet Kumaşı Linux kümesi oluşturma.
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.topic: sample
ms.date: 01/18/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: 2ef8f322ff17eeb5d75d3cc8e4f8604f02d4ef0e
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366539"
---
# <a name="create-a-secure-service-fabric-linux-cluster-in-azure"></a>Azure’da güvenli bir Service Fabric Linux kümesi oluşturma

Bu komut, otomatik olarak imzalanan sertifika oluşturur, bu sertifikayı bir anahtar kasasına ekler ve sertifikayı yerel ortamda indirir.  Yeni sertifika, dağıtıldığında kümenin güvenliğini sağlamak için kullanılır.  Yeni bir sertifika oluşturmak yerine mevcut sertifikayı da kullanabilirsiniz.  Her iki şekilde de, sertifikanın konu adı, Service Fabric kümesine erişmek için kullandığınız etki alanıyla eşleşmelidir. Bu eşleşme, kümenin HTTPS yönetim uç noktaları ve Hizmet Kumaş Explorer için TLS sağlamak için gereklidir. `.cloudapp.azure.com` Etki alanı için bir CA'dan TLS/SSL sertifikası alamazsınız. Kümeniz için özel bir etki alanı adı edinmeniz gerekir. CA’dan sertifika istediğinizde sertifikanın konu adı, kümeniz için kullandığınız özel etki alanı adıyla eşleşmelidir.

Gerekirse, [Azure CLI](/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)’yi yükleyin.

## <a name="sample-script"></a>Örnek betik

[!code-sh[main](../../../cli_scripts/service-fabric/create-cluster/create-cluster.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu, kümeyi ve ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```azurecli
ResourceGroupName = "aztestclustergroup"
az group delete --name $ResourceGroupName
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az sf cluster create](https://docs.microsoft.com/cli/azure/sf/cluster?view=azure-cli-latest) | Yeni bir Service Fabric kümesi oluşturur.  |

## <a name="next-steps"></a>Sonraki adımlar

Azure Service Fabric’e yönelik ek Service Fabric CLI örnekleri [Service Fabric CLI örnekleri](../samples-cli.md)’nde bulunabilir.
