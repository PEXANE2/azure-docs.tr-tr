---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 07/20/2020
ms.author: msmbaldwin
ms.openlocfilehash: d1a67a131a94bc017eaf2199546ef08f0291cd54
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102244700"
---
**MySecret** adlı bir gizli anahtar oluşturalım **!**. Gizli anahtar, bir parola, bir SQL bağlantı dizesi veya hem güvenli hem de uygulamanız için kullanılabilir durumda tutmanız gereken diğer bilgiler olabilir. 

Yeni oluşturduğunuz anahtar kasanıza gizli dizi eklemek için Azure CLı [az keykasası Secret set](/cli/azure/keyvault/secret#az-keyvault-secret-set) komutunu kullanın:

```azurecli
az keyvault secret set --vault-name "<your-unique-keyvault-name>" --name "mySecret" --value "Success!"
```