---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 07/20/2020
ms.author: msmbaldwin
ms.openlocfilehash: efa380ac243338a91354f948fdc77b2da8dd5406
ms.sourcegitcommit: 29400316f0c221a43aff3962d591629f0757e780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2020
ms.locfileid: "87512831"
---
**MySecret**adlı bir gizli anahtar oluşturalım **!**. Gizli anahtar, bir parola, bir SQL bağlantı dizesi veya hem güvenli hem de uygulamanız için kullanılabilir durumda tutmanız gereken diğer bilgiler olabilir. 

Yeni oluşturduğunuz anahtar kasanıza gizli dizi eklemek için Azure CLı [az keykasası Secret set](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-set) komutunu kullanın:

```azurecli
az keyvault secret set --vault-name "<your-unique-keyvault-name>" --name "mySecret" --value "Success!"
```