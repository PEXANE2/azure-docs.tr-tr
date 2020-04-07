---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 44823ce888e97b308f29403612f598c0eb585ae5
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673341"
---
[Kuyruğa Azure portalında](../articles/storage/queues/storage-quickstart-queues-portal.md) veya [Microsoft Azure Depolama Gezgini'nde](https://storageexplorer.com/)görüntüleyebilirsiniz. Aşağıdaki adımlarda açıklandığı gibi, Azure CLI'deki sırayı da görüntüleyebilirsiniz:

1. İşlev projesinin *yerel.setting.json* dosyasını açın ve bağlantı dize değerini kopyalayın. Bir terminal veya komut penceresinde, aşağıdaki komutu çalıştırarak bir ortam değişkeni oluşturmak için , `AZURE_STORAGE_CONNECTION_STRING`'yerine belirli bağlantı dizesi yapıştırma `<MY_CONNECTION_STRING>`. (Bu ortam değişkeni, `--connection-string` bağımsız değişkeni kullanarak sonraki her komuta bağlantı dizesi sağlamanız gerekolmadığı anlamına gelir.)

    # <a name="bash"></a>[bash](#tab/bash)
    
    ```bash
    AZURE_STORAGE_CONNECTION_STRING="<MY_CONNECTION_STRING>"
    ```
    
    # <a name="powershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    $env:AZURE_STORAGE_CONNECTION_STRING = "<MY_CONNECTION_STRING>"
    ```
    
    # <a name="azure-cli"></a>[Azure CLI](#tab/cmd)
    
    ```azurecli
    set AZURE_STORAGE_CONNECTION_STRING="<MY_CONNECTION_STRING>"
    ```
    
    ---
    
1. (İsteğe bağlı) Hesabınızdaki [`az storage queue list`](/cli/azure/storage/queue#az-storage-queue-list) Depolama kuyruklarını görüntülemek için komutu kullanın. Bu komutun çıktısı, işlev `outqueue`bu kuyruğa ilk iletisini yazdığında oluşturulan , adlı bir sıra içermelidir.
    
    ```azurecli
    az storage queue list --output tsv
    ```

1. İşlevdaha önce test ederken kullandığınız ilk ad olmalıdır bu kuyruktaki iletiyi okumak için [`az storage message get`](/cli/azure/storage/message#az-storage-message-get) komutu kullanın. Komut ilk iletiyi sıradan okur ve kaldırır. 

    # <a name="bash"></a>[bash](#tab/bash)
    
    ```bash
    echo `echo $(az storage message get --queue-name outqueue -o tsv --query '[].{Message:content}') | base64 --decode`
    ```
    
    # <a name="powershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($(az storage message get --queue-name outqueue -o tsv --query '[].{Message:content}')))
    ```
    
    # <a name="azure-cli"></a>[Azure CLI](#tab/cmd)
    
    ```azurecli
    az storage message get --queue-name outqueue -o tsv --query [].{Message:content} > %TEMP%out.b64 && certutil -decode -f %TEMP%out.b64 %TEMP%out.txt > NUL && type %TEMP%out.txt && del %TEMP%out.b64 %TEMP%out.txt /q
    ```

    Bu komut dosyası, yerel bir geçici dosyadan base64 kodlanmış ileti koleksiyonunu çözmek için certutil kullanır. Çıktı yoksa, bir hata olması `> NUL` durumunda certutil çıktısını bastırmayı durdurmak için komut dosyasından kaldırmayı deneyin. 
    
    ---
    
    İleti gövdesi [base64 kodlu](../articles/azure-functions/functions-bindings-storage-queue-trigger.md#encoding)olarak depolandığı için, ileti görüntülenmeden önce deşifre edilmelidir. Yürütmeden `az storage message get`sonra ileti kuyruktan kaldırılır. Yalnızca bir ileti `outqueue`varsa, bu komutu ikinci kez çalıştırdığınızda bir ileti almazsınız ve bunun yerine bir hata alırsınız.