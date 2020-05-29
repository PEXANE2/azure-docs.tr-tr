---
title: Azure SQL Edge 'i yapılandırma (Önizleme)
description: Azure SQL Edge 'i (Önizleme) yapılandırma hakkında bilgi edinin
keywords: ''
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: dc2b76a31982a3f72da02348c1a4796212887cb7
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84168259"
---
# <a name="configure-azure-sql-edge-preview"></a>Azure SQL Edge 'i yapılandırma (Önizleme)

Azure SQL Edge, yapılandırmayı aşağıdaki iki seçenekten biri aracılığıyla destekler:

- Ortam değişkenlerini kullanma.
- /Var/seçenek/MSSQL klasörüne yerleştirilmiş MSSQL. conf dosyasını kullanma.

> [!NOTE]
> Ortam değişkenlerinin ayarlanması, MSSQL. conf dosyasında belirtilen ayarları geçersiz kılar.

## <a name="configure-using-environment-variables"></a>Ortam değişkenlerini kullanarak yapılandırma

Azure SQL Edge, SQL Edge kapsayıcısını yapılandırmak için kullanılabilecek çeşitli farklı ortam değişkenlerini kullanıma sunar. Bu ortam değişkenleri Linux üzerinde SQL Server için kullanılabilir ortam değişkenlerinin bir alt kümesidir. Linux üzerinde SQL Server ortam değişkenleri hakkında daha fazla bilgi için bkz. [ortam değişkenleri](/sql/linux/sql-server-linux-configure-environment-variables/).

Aşağıdaki Linux üzerinde SQL Server ortam değişkenleri Azure SQL Edge için desteklenmez. Bu ortam değişkenleri tanımlanmışsa, kapsayıcı başlatma sırasında yok sayılır.

| Ortam değişkeni | Açıklama |
|-----|-----|
| **MSSQL_ENABLE_HADR** | Kullanılabilirlik grubunu etkinleştirin. Örneğin, ' 1 ' etkin ve ' 0 ' devre dışı |

> [!IMPORTANT]
> SQL Edge için *MSSQL_PID* ortam değişkeni, geçerli değerler olarak yalnızca **Premium** ve **Geliştirici** kabul eder. Azure SQL Edge, bir ürün anahtarı kullanarak başlatmayı desteklemez.

> [!NOTE]
> Azure SQL Edge Son Kullanıcı Lisans Sözleşmesi 'ni indirmek için [Son Kullanıcı Lisans Sözleşmesi](https://go.microsoft.com/fwlink/?linkid=2128283)'ne bakın.

### <a name="specifying-the-environment-variables"></a>Ortam değişkenlerini belirtme

[Azure Portal](deploy-portal.md)aracılığıyla Azure SQL Edge dağıtıldığında SQL Edge için ortam değişkenleri belirtilebilir. Bu, modül dağıtımının "ortam değişkenleri" bölümünde veya kapsayıcı oluşturma seçeneğinin bir parçası olarak aşağıda açıklandığı gibi eklenebilir.

*Ortam değişkenleri seçeneklerini kullanarak ayarlama*

![ortam değişkenleri listesini kullanarak ayarla](media/configure/set-environment-variables.png)

*Kapsayıcı oluşturma seçeneklerini kullanarak ayarlama*

![kapsayıcı oluşturma seçeneklerini kullanarak ayarlama](media/configure/set-environment-variables-using-create-options.png)

## <a name="configure-using-mssqlconf-file"></a>MSSQL. conf dosyasını kullanarak yapılandırma

MSSQL. conf dosyasının el ile yapılandırılması ve SQL Edge modülünde/var/seçenek/MSSQL/klasörüne eşlenmiş kalıcı depolama sürücüsüne yerleştirilmesi gerektiğinden, Azure SQL Edge Linux üzerinde SQL Server, gibi [MSSQL-conf yapılandırma yardımcı programını](/sql/linux/sql-server-linux-configure-mssql-conf/) içermez. Azure Marketi 'nden SQL Edge 'i dağıttığınızda, bu eşleme kapsayıcı oluşturma seçeneğinde * * takal "seçeneği olarak belirtilir

```json
    {
        "Mounts": [
          {
            "Type": "volume",
            "Source": "sqlvolume",
            "Target": "/var/opt/mssql"
          }
        ]
      }
    }
```

Aşağıdaki MSSQL. conf seçenekleri SQL Edge için geçerli değildir:

|Seçenek|Açıklama|
|:---|:---|
|**Müşteri geri bildirimi** | SQL Server Microsoft 'a geri bildirim gönderip göndermediğini seçin. |
|**Veritabanı Postası profili** | Linux üzerinde SQL Server için varsayılan veritabanı posta profilini ayarlayın. |
|**Yüksek kullanılabilirlik** | Kullanılabilirlik gruplarını etkinleştirin. |
|**Microsoft Dağıtılmış İşlem Düzenleyicisi** | Linux üzerinde MSDTC 'yi yapılandırma ve sorunlarını giderme. SQL Edge için ek dağıtılmış işlemle ilgili diğer yapılandırma seçenekleri de desteklenmez. Bu ek yapılandırma seçenekleri hakkında daha fazla bilgi için bkz. [MSDTC 'Yi yapılandırma](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-mssql-conf#msdtc) |
|**MLServices EULA 'Ları** | Machine Learning Services paketleri için R ve Python EULA 'Ları kabul edin. Yalnızca SQL Server 2019 için geçerlidir.|
|**outboundnetworkaccess** |[Machine Learning Services](/sql/linux/sql-server-linux-setup-machine-learning/) R, Python ve Java uzantıları için giden ağ erişimini etkinleştirin.|

Aşağıda SQL Edge için uygun olan bir MSSQL. conf dosyası aşağıda verilmiştir. MSSQL. conf dosyasının biçimi hakkında daha fazla bilgi için bkz. [MSSQL. conf biçimi](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-mssql-conf#mssql-conf-format).

```ini
[EULA]
accepteula = Y

[coredump]
captureminiandfull = true
coredumptype = full

[filelocation]
defaultbackupdir = /var/opt/mssql/backup/
defaultdatadir = /var/opt/mssql/data/
defaultdumpdir = /var/opt/mssql/data/
defaultlogdir = /var/opt/mssql/log/

[language]
lcid = 1033

[memory]
memorylimitmb = 6144

[sqlagent]
errorlogfile = /var/opt/mssql/log/sqlagentlog.log
errorlogginglevel = 7

[traceflag]
traceflag0 = 3604
traceflag1 = 3605
traceflag2 = 1204
```

## <a name="next-step"></a>Sonraki adım

- [Azure SQL Edge 'e bağlanma](connect.md)
- [SQL Edge ile uçtan uca IoT çözümü oluşturma](tutorial-deploy-azure-resources.md)
