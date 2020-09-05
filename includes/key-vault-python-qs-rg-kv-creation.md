---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 09/03/2020
ms.author: msmbaldwin
ms.openlocfilehash: 5e912b76c2ef68aa01dae57d1b42abc386a8e67b
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89482156"
---
1. `az group create`Bir kaynak grubu oluşturmak için komutunu kullanın:

    ```azurecli
    az group create --name KeyVault-PythonQS-rg --location eastus
    ```

    "Eastus" öğesini tercih ediyorsanız, sizin için bir konum için "" olarak değiştirebilirsiniz.

1. `az keyvault create`Anahtar Kasası oluşturmak için kullanın:

    ```azurecli
    az keyvault create --name <your-unique-keyvault-name> --resource-group KeyVault-PythonQS-rg
    ```

    `<your-unique-keyvault-name>`Tüm Azure genelinde benzersiz olan bir adla değiştirin. Genellikle kişisel veya şirket adınızı diğer sayı ve tanımlayıcılarla birlikte kullanırsınız. 

1. Koda Key Vault adını sağlayan bir ortam değişkeni oluşturun:

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```cmd
    set KEY_VAULT_NAME=<your-unique-keyvault-name>
    ```

    # <a name="bash"></a>[Bash](#tab/bash)

    ```bash
    export KEY_VAULT_NAME=<your-unique-keyvault-name>
    ```

    ---
