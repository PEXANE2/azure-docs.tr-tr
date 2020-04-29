---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 44823ce888e97b308f29403612f598c0eb585ae5
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80673341"
---
Kuyruğu [Azure Portal](../articles/storage/queues/storage-quickstart-queues-portal.md) veya [Microsoft Azure Depolama Gezgini](https://storageexplorer.com/)görüntüleyebilirsiniz. Ayrıca, aşağıdaki adımlarda açıklandığı gibi, kuyruğu Azure CLı 'de de görebilirsiniz:

1. İşlevin proje *yerel. Setting. JSON* dosyasını açın ve bağlantı dizesi değerini kopyalayın. Bir Terminal veya komut penceresinde, belirli bir Bağlantı dizenizi yerinde yapıştırarak adlı `AZURE_STORAGE_CONNECTION_STRING`bir ortam değişkeni oluşturmak için aşağıdaki komutu çalıştırın. `<MY_CONNECTION_STRING>` (Bu ortam değişkeni, `--connection-string` bağımsız değişkenini kullanarak sonraki her komuta bağlantı dizesi sağlamanız gerekmez.)

    # <a name="bash"></a>[Bash](#tab/bash)
    
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
    
1. Seçim Hesabınızdaki depolama [`az storage queue list`](/cli/azure/storage/queue#az-storage-queue-list) kuyruklarını görüntülemek için komutunu kullanın. Bu komutun çıktısı, işlevi bu sıraya ilk iletisini yazmışsa oluşturulmuş adlı `outqueue`bir kuyruk içermelidir.
    
    ```azurecli
    az storage queue list --output tsv
    ```

1. Bu kuyruktan [`az storage message get`](/cli/azure/storage/message#az-storage-message-get) iletiyi okumak için komutunu kullanın. Bu, daha önce işlev test edilirken kullandığınız ad olmalıdır. Komut kuyruktan ilk iletiyi okur ve kaldırır. 

    # <a name="bash"></a>[Bash](#tab/bash)
    
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

    Bu betik, yerel bir geçici dosyadan Base64 kodlamalı ileti koleksiyonunun kodunu çözmek için Certutil kullanır. Çıkış yoksa, bir hata olması durumunda certutil `> NUL` çıkışını gizlemeyi durdurmak için betikten kaldırmayı deneyin. 
    
    ---
    
    İleti gövdesi [Base64 kodlamalı](../articles/azure-functions/functions-bindings-storage-queue-trigger.md#encoding)depolandığından, iletinin görüntülenmeden önce kodu çözülemedi. Yürütmeden `az storage message get`sonra ileti kuyruktan kaldırılır. İçinde `outqueue`yalnızca bir ileti varsa, bu komutu ikinci kez çalıştırıp bir hata elde ettiğinizde bir ileti almazsınız.