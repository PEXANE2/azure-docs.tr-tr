---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 894a89126d1ee3ed909134f3e0dd914166568654
ms.sourcegitcommit: 6e1124fc25c3ddb3053b482b0ed33900f46464b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90606344"
---
Kuyruğu [Azure Portal](../articles/storage/queues/storage-quickstart-queues-portal.md) veya  [Microsoft Azure Depolama Gezgini](https://storageexplorer.com/)görüntüleyebilirsiniz. Ayrıca, aşağıdaki adımlarda açıklandığı gibi, kuyruğu Azure CLı 'de de görebilirsiniz:

1. İşlev projesinin *local.setting.jsdosya üzerinde* açın ve bağlantı dizesi değerini kopyalayın. Bir Terminal veya komut penceresinde, `AZURE_STORAGE_CONNECTION_STRING` belirli bir Bağlantı dizenizi yerinde yapıştırarak adlı bir ortam değişkeni oluşturmak için aşağıdaki komutu çalıştırın  `<MY_CONNECTION_STRING>` . (Bu ortam değişkeni, bağımsız değişkenini kullanarak sonraki her komuta bağlantı dizesi sağlamanız gerekmez `--connection-string` .)

    # <a name="bash"></a>[Bash](#tab/bash)
    
    ```bash
    export AZURE_STORAGE_CONNECTION_STRING="<MY_CONNECTION_STRING>"
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
    
1. Seçim [`az storage queue list`](/cli/azure/storage/queue#az-storage-queue-list) Hesabınızdaki depolama kuyruklarını görüntülemek için komutunu kullanın. Bu komutun çıktısı, `outqueue` işlevi bu sıraya ilk iletisini yazmışsa oluşturulmuş adlı bir kuyruk içermelidir.
    
    ```azurecli
    az storage queue list --output tsv
    ```

1. [`az storage message get`](/cli/azure/storage/message#az-storage-message-get)Bu kuyruktan iletiyi okumak için komutunu kullanın. Bu, daha önce işlev test edilirken kullandığınız ad olmalıdır. Komut kuyruktan ilk iletiyi okur ve kaldırır. 

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

    Bu betik, yerel bir geçici dosyadan Base64 kodlamalı ileti koleksiyonunun kodunu çözmek için Certutil kullanır. Çıkış yoksa, `> NUL` bir hata olması durumunda certutil çıkışını gizlemeyi durdurmak için betikten kaldırmayı deneyin. 
    
    ---
    
    İleti gövdesi [Base64 kodlamalı](../articles/azure-functions/functions-bindings-storage-queue-trigger.md#encoding)depolandığından, iletinin görüntülenmeden önce kodu çözülemedi. Yürütmeden sonra `az storage message get` ileti kuyruktan kaldırılır. İçinde yalnızca bir ileti varsa `outqueue` , bu komutu ikinci kez çalıştırıp bir hata elde ettiğinizde bir ileti almazsınız.
