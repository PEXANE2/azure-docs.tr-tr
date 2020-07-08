---
title: Azure SQL Edge 'i yapılandırma (Önizleme)
description: Azure SQL Edge 'i (Önizleme) yapılandırma hakkında bilgi edinin.
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: c38bb6100665cc9456b66608660bdca520b934c6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84636249"
---
# <a name="configure-azure-sql-edge-preview"></a>Azure SQL Edge 'i yapılandırma (Önizleme)

Azure SQL Edge, yapılandırmayı aşağıdaki iki seçenekten biri aracılığıyla destekler:

- Ortam değişkenleri
- /Var/seçenek/MSSQL klasörüne yerleştirilmiş bir MSSQL. conf dosyası

> [!NOTE]
> Ortam değişkenlerinin ayarlanması, MSSQL. conf dosyasında belirtilen ayarları geçersiz kılar.

## <a name="configure-by-using-environment-variables"></a>Ortam değişkenlerini kullanarak yapılandırma

Azure SQL Edge, SQL Edge kapsayıcısını yapılandırmak için kullanılabilecek çeşitli farklı ortam değişkenlerini kullanıma sunar. Bu ortam değişkenleri, Linux üzerinde SQL Server için kullanılabilir olanların bir alt kümesidir. Linux üzerinde SQL Server ortam değişkenleri hakkında daha fazla bilgi için bkz. [ortam değişkenleri](/sql/linux/sql-server-linux-configure-environment-variables/).

Şu Linux üzerinde SQL Server ortam değişkeni Azure SQL Edge için desteklenmiyor. Tanımlandıysa, bu ortam değişkeni kapsayıcı başlatma sırasında yok sayılır.

| Ortam değişkeni | Açıklama |
|-----|-----|
| **MSSQL_ENABLE_HADR** | Kullanılabilirlik grubunu etkinleştirin. Örneğin, **1** etkindir ve **0** devre dışıdır. |

> [!IMPORTANT]
> SQL Edge için **MSSQL_PID** ortam değişkeni, geçerli değerler olarak yalnızca **Premium** ve **Geliştirici** kabul eder. Azure SQL Edge, bir ürün anahtarı kullanarak başlatmayı desteklemez.

> [!NOTE]
> Azure SQL Edge için [Microsoft yazılımı lisans koşulları](https://go.microsoft.com/fwlink/?linkid=2128283) 'nı indirin.

### <a name="specify-the-environment-variables"></a>Ortam değişkenlerini belirtin

Hizmeti [Azure Portal](deploy-portal.md)aracılığıyla dağıtırken SQL Edge için ortam değişkenlerini belirtin. Bunları modül dağıtımının **ortam değişkenleri** bölümünde veya **kapsayıcı oluşturma seçeneklerinin**bir parçası olarak ekleyebilirsiniz.

**Ortam değişkenlerine**değer ekleyin.

![Ortam değişkenleri listesini kullanarak ayarla](media/configure/set-environment-variables.png)

**Kapsayıcı oluşturma seçeneklerinde**değer ekleyin.

![Kapsayıcı oluşturma seçeneklerini kullanarak ayarlama](media/configure/set-environment-variables-using-create-options.png)

## <a name="configure-by-using-an-mssqlconf-file"></a>MSSQL. conf dosyası kullanarak yapılandırma

Azure SQL Edge, Linux üzerinde SQL Server gibi [MSSQL-conf yapılandırma yardımcı programını](/sql/linux/sql-server-linux-configure-mssql-conf/) içermez. MSSQL. conf dosyasını el ile yapılandırmanız ve SQL Edge modülünde/var/seçenek/MSSQL/klasörüne eşlenmiş kalıcı depolama sürücüsüne yerleştirmeniz gerekir. Azure Marketi 'nden SQL Edge 'i dağıttığınızda, bu eşleme **kapsayıcı oluşturma seçeneklerinde** **bağlama** seçeneği olarak belirtilir.

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
|**Veritabanı posta profili** | Linux üzerinde SQL Server için varsayılan veritabanı posta profilini ayarlayın. |
|**Yüksek kullanılabilirlik** | Kullanılabilirlik gruplarını etkinleştirin. |
|**Microsoft Dağıtılmış İşlem Düzenleyicisi** | Linux üzerinde MSDTC 'yi yapılandırma ve sorunlarını giderme. SQL Edge için ek dağıtılmış işlem ile ilgili yapılandırma seçenekleri desteklenmez. Bu ek yapılandırma seçenekleri hakkında daha fazla bilgi için bkz. [MSDTC 'Yi yapılandırma](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-mssql-conf#msdtc). |
|**MLServices EULA 'Ları** | Azure Machine Learning paketleri için R ve Python EULA 'Ları kabul edin. Yalnızca SQL Server 2019 için geçerlidir.|
|**outboundnetworkaccess** |[Machine Learning Services](/sql/linux/sql-server-linux-setup-machine-learning/) R, Python ve Java uzantıları için giden ağ erişimini etkinleştirin.|

Aşağıdaki örnek MSSQL. conf dosyası SQL Edge için geçerlidir. MSSQL. conf dosyasının biçimi hakkında daha fazla bilgi için bkz. [MSSQL. conf biçimi](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-mssql-conf#mssql-conf-format).

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

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL Edge 'e bağlanma](connect.md)
- [SQL Edge ile uçtan uca IoT çözümü oluşturma](tutorial-deploy-azure-resources.md)
