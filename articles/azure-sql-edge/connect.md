---
title: Azure SQL Edge 'e bağlanma ve sorgulama (Önizleme)
description: Azure SQL Edge 'i bağlama ve sorgulama hakkında bilgi edinin (Önizleme)
keywords: ''
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 17bb5afebe0335bcdcf40298490f94999c04a621
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83596921"
---
# <a name="connect-and-query-azure-sql-edge-preview"></a>Azure SQL Edge 'e bağlanma ve sorgulama (Önizleme)

Azure SQL Edge kapsayıcısını dağıttıktan sonra, aşağıdaki konumlardan herhangi birinden SQL veritabanı altyapısına bağlanabilirsiniz.

- Kapsayıcının içinde
- Aynı konakta çalışan başka bir Docker kapsayıcısında.
- Ana makineden
- Ağ üzerindeki herhangi bir diğer istemci makineden.

## <a name="tools-to-connect-to-azure-sql-edge"></a>Azure SQL Edge 'e bağlanmak için Araçlar

Azure SQL Edge örneğine bağlantılar aşağıda bahsedilen ortak araçlardan herhangi birinden yapılabilir.

* [sqlcmd](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-tools) -sqlcmd istemci araçları, Azure SQL Edge kapsayıcı görüntüsüne zaten dahil edilmiştir. Etkileşimli bash kabuğu ile çalışan bir kapsayıcıya eklerseniz, araçları yerel olarak çalıştırabilirsiniz.
* [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms)
* [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio)
* [Visual Studio Code](https://docs.microsoft.com/sql/visual-studio-code/sql-server-develop-use-vscode)

Bir ağ makinesinden Azure SQL Edge veritabanı altyapısına bağlanmak için aşağıdakiler gerekir

- *Konak makinenin IP adresi veya ağ adı* -bu, Azure SQL Edge kapsayıcısının çalıştığı ana makinedir.
- *Azure SQL Edge kapsayıcı ana bilgisayar bağlantı noktası eşleme* -bu, Docker kapsayıcı bağlantı noktası için konaktaki bağlantı noktası eşleştirmesinin bağlantı noktasıdır. Kapsayıcı SQL Edge içinde her zaman 1433 numaralı bağlantı noktasına eşlenir. Bu, Azure SQL Edge dağıtımının bir parçası olarak değiştirilebilir. Bağlantı noktası numarasını değiştirmek için, Azure IoT Edge SQL Edge modülünün "kapsayıcı oluşturma seçeneklerini" güncelleştirin. Aşağıda belirtilen örnekte, kapsayıcıda bağlantı noktası 1433, konaktaki bağlantı noktası 1600 ile eşleştirilir.

    ```JSON
    {
        "PortBindings": {
          "1433/tcp": [
            {
              "HostPort": "1600"
            }
          ]
        }
    }
    ```

- *SQL Edge örneği Için sa parolası* -bu, SQL Edge dağıtımı sırasında **SA_PASSWORD** ortam değişkeni için belirtilen değerdir.

## <a name="connecting-to-the-database-engine-from-within-the-container"></a>Kapsayıcı içinden veritabanı altyapısına bağlanma

[SQL Server komut satırı araçları](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-tools) , Azure SQL Edge kapsayıcı görüntüsüne dahildir. Etkileşimli bir komut istemiyle kapsayıcıya eklerseniz, araçları yerel olarak çalıştırabilirsiniz.

1. `docker exec -it`Çalışan kapsayıcının içinde etkileşimli bir bash kabuğu başlatmak için komutunu kullanın. Aşağıdaki örnekte `e69e056c702d` KAPSAYıCı kimliğidir.

    ```bash
    docker exec -it <Azure SQL Edge container id or name> /bin/bash
    ```

    > [!TIP]
    > Her zaman kapsayıcı kimliğinin tamamını belirtmeniz gerekmez. Benzersiz bir şekilde tanımlamak için yeterli karakter belirtmeniz yeterlidir. Bu nedenle, bu örnekte, `e6` tam kimlik yerine veya kullanmak yeterli olabilir `e69` .

2. Kapsayıcının içindeyken sqlcmd ile yerel olarak bağlanın. Sqlcmd, varsayılan olarak yolda değildir, bu nedenle tam yolu belirtmeniz gerekir.

    ```bash
    /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P '<YourPassword>'
    ```

3. Sqlcmd ile işiniz bittiğinde, yazın `exit` .

4. Etkileşimli komut istemiyle işiniz bittiğinde yazın `exit` . Etkileşimli bash kabuğu 'ndan çıktıktan sonra Kapsayıcınız çalışmaya devam eder.

## <a name="connect-to-sql-edge-from-another-container-on-the-same-host"></a>Aynı konaktaki başka bir kapsayıcıdan SQL Edge 'e bağlanma

Aynı konakta çalışan iki kapsayıcı aynı Docker ağı üzerinde olduğundan, hizmet için kapsayıcı adı ve bağlantı noktası adresi kullanılarak kolayca erişilebilir. Örneğin, aynı konaktaki başka bir Python modülünden (container) SQL Edge örneğine bağlanıyorsanız aşağıdakine benzer bir bağlantı dizesi kullanabilirsiniz. Aşağıdaki örnekte, SQL Edge 'in varsayılan bağlantı noktasında dinlemek için yapılandırıldığı varsayılır.

```python

import pyodbc
server = 'MySQLEdgeContainer' # Replace this with the actual name of your SQL Edge Docker container
username = 'sa' # SQL Server username
password = 'MyStrongestP@ssword' # Replace this with the actual SA password from your deployment
database = 'MyEdgeDatabase' # Replace this with the actual database name from your deployment. If you do not have a database created, you can use Master database.
db_connection_string = "Driver={ODBC Driver 17 for SQL Server};Server=" + server + ";Database=" + database + ";UID=" + username + ";PWD=" + password + ";"
conn = pyodbc.connect(db_connection_string, autocommit=True)

```

## <a name="connect-to-sql-edge-from-another-network-machine"></a>Başka bir ağ makinesinden SQL Edge 'e Bağlan

Ağ üzerindeki başka bir makineden SQL Edge örneğine bağlanmak için, Docker konağının IP adresini ve SQL Edge kapsayıcısının eşlendiği ana bilgisayar bağlantı noktasını kullanmanız gerekir. Örneğin, Docker konağının IP adresi * xxx.xxx.xxx.xxx "ve SQL Edge kapsayıcısı ana bilgisayar bağlantı noktası *1600*' e eşlenmişse, SQL Edge örneği için sunucu adresi xxx. xxx. **xxx. xxx, 1600**olur. Güncelleştirilmiş Python betiği

```python

import pyodbc
server = 'xxx.xxx.xxx.xxx,1600' # Replace this with the actual name of your SQL Edge Docker container
username = 'sa' # SQL Server username
password = 'MyStrongestP@ssword' # Replace this with the actual SA password from your deployment
database = 'MyEdgeDatabase' # Replace this with the actual database name from your deployment. If you do not have a database created, you can use Master database.
db_connection_string = "Driver={ODBC Driver 17 for SQL Server};Server=" + server + ";Database=" + database + ";UID=" + username + ";PWD=" + password + ";"
conn = pyodbc.connect(db_connection_string, autocommit=True)

```

Bir Windows makinesinde çalışan SQL Server Management Studio kullanarak bir SQL Edge örneğine bağlanmak için, [SQL Server Management Studio](https://docs.microsoft.com/sql/linux/sql-server-linux-manage-ssms)başvurun.

Bir Windows, Mac veya Linux makinesinde Visual Studio Code kullanarak bir SQL Edge örneğine bağlanmak için [Visual Studio Code](https://docs.microsoft.com/sql/visual-studio-code/sql-server-develop-use-vscode)başvurun.

Windows, Mac veya Linux makinesinde Azure Data Studio kullanarak bir SQL Edge örneğine bağlanmak için [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-server)başvurun.

## <a name="see-also"></a>Ayrıca bkz.

[Bağlanma ve sorgulama](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-docker#connect-and-query)

[Linux 'ta SQL Server araçları 'nı yükler](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-tools)
