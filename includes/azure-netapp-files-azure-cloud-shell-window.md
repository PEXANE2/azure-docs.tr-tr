---
title: include dosyası
description: include dosyası
services: azure-netapp-files
author: b-juche
ms.service: azure-netapp-files
ms.topic: include
ms.date: 09/10/2019
ms.author: b-juche
ms.custom: include file
ms.openlocfilehash: 3a63fd96b09910b0cd7ee8c3ab9947b034173c8f
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "71836303"
---
1. Azure NetApp Files için beyaz listeye alınmış aboneliği belirtin:
    
    ```azurecli-interactive
    az account set --subscription <subscriptionId>
    ```

1. Azure Kaynak sağlayıcısını kaydedin: 
    
    ```azurecli-interactive
    az provider register --namespace Microsoft.NetApp --wait  
    ```