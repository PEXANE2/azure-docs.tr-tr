---
title: Azure SQL Edge 'i yapılandırma
description: Azure SQL Edge 'i yapılandırma hakkında bilgi edinin.
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 09/22/2020
ms.openlocfilehash: b2c52457972d94b2e999c137d19d3a434ff17a7d
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90888392"
---
# <a name="configure-azure-sql-edge"></a>Azure SQL Edge 'i yapılandırma

Azure SQL Edge, yapılandırmayı aşağıdaki iki seçenekten biri aracılığıyla destekler:

- Ortam değişkenleri
- /Var/seçenek/MSSQL klasörüne yerleştirilmiş bir MSSQL. conf dosyası

> [!NOTE]
> Ortam değişkenlerinin ayarlanması, MSSQL. conf dosyasında belirtilen ayarları geçersiz kılar.

## <a name="configure-by-using-environment-variables"></a>Ortam değişkenlerini kullanarak yapılandırma

Azure SQL Edge, SQL Edge kapsayıcısını yapılandırmak için kullanılabilecek çeşitli farklı ortam değişkenlerini kullanıma sunar. Bu ortam değişkenleri, Linux üzerinde SQL Server için kullanılabilir olanların bir alt kümesidir. Linux üzerinde SQL Server ortam değişkenleri hakkında daha fazla bilgi için bkz. [ortam değişkenleri](/sql/linux/sql-server-linux-configure-environment-variables/).

Aşağıdaki yeni ortam değişkenleri Azure SQL Edge 'e eklenmiştir. 

| Ortam değişkeni | Açıklama | Değerler |     
|-----|-----| ---------- |   
| **MSSQL_TELEMETRY_ENABLED** | Kullanım ve tanılama veri toplamayı etkinleştirin veya devre dışı bırakın. | TRUE veya FALSE |  
| **MSSQL_TELEMETRY_DIR** | Kullanım ve tanılama veri toplama denetim dosyaları için hedef dizini ayarlar. | SQL Edge kapsayıcısı içindeki klasör konumu. Bu klasör, bağlama noktaları veya veri birimleri kullanılarak bir konak birimi ile eşleştirilebilir. | 
| **MSSQL_PACKAGE** | Dağıtılacak dacpac veya bacpac paketinin konumunu belirtir. | Dacpac veya bacpac paketlerini içeren klasör, dosya veya SAS URL 'SI. Daha fazla bilgi için [SQL Edge 'de SQL veritabanı DACPAC ve BACPAC paketleri dağıtma](deploy-dacpac.md)konusuna bakın. |


Şu Linux üzerinde SQL Server ortam değişkeni Azure SQL Edge için desteklenmiyor. Tanımlandıysa, bu ortam değişkeni kapsayıcı başlatma sırasında yok sayılır.

| Ortam değişkeni | Açıklama |
|-----|-----|
| **MSSQL_ENABLE_HADR** | Kullanılabilirlik grubunu etkinleştirin. Örneğin, **1** etkindir ve **0** devre dışıdır. |

> [!IMPORTANT]
> SQL Edge için **MSSQL_PID** ortam değişkeni, geçerli değerler olarak yalnızca **Premium** ve **Geliştirici** kabul eder. Azure SQL Edge, bir ürün anahtarı kullanarak başlatmayı desteklemez.

### <a name="specify-the-environment-variables"></a>Ortam değişkenlerini belirtin

Hizmeti [Azure Portal](deploy-portal.md)aracılığıyla dağıtırken SQL Edge için ortam değişkenlerini belirtin. Bunları modül dağıtımının **ortam değişkenleri** bölümünde veya **kapsayıcı oluşturma seçeneklerinin**bir parçası olarak ekleyebilirsiniz.

**Ortam değişkenlerine**değer ekleyin.

![Ortam değişkenleri listesini kullanarak ayarla](media/configure/set-environment-variables.png)

**Kapsayıcı oluşturma seçeneklerinde**değer ekleyin.

![Kapsayıcı oluşturma seçeneklerini kullanarak ayarlama](media/configure/set-environment-variables-using-create-options.png)

> [!NOTE]
> Bağlantısı kesilmiş dağıtım modunda, ortam değişkenleri, `-e` veya `--env` `--env-file` komutunun seçeneği kullanılarak belirtilebilir `docker run` .

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

Azure SQL Edge için aşağıdaki yeni MSSQL. conf seçenekleri eklenmiştir. 

|Seçenek|Açıklama|
|:---|:---|
|**CustomerFeedback** | SQL Server Microsoft 'a geri bildirim gönderip göndermediğini seçin. Daha fazla bilgi için bkz. [kullanım ve tanılama veri toplamayı devre dışı bırakma](usage-and-diagnostics-data-configuration.md#disable-usage-and-diagnostic-data-collection)|      
|**userrequestedlocalauditdirectory** | Kullanım ve tanılama veri toplama denetim dosyaları için hedef dizini ayarlar. Daha fazla bilgi için bkz. [kullanım ve Tanılama verileri toplama Için yerel denetim](usage-and-diagnostics-data-configuration.md#local-audit-of-usage-and-diagnostic-data-collection) |        

Aşağıdaki MSSQL. conf seçenekleri SQL Edge için geçerli değildir:

|Seçenek|Açıklama|
|:---|:---|
|**Müşteri geri bildirimi** | SQL Server Microsoft 'a geri bildirim gönderip göndermediğini seçin. |
|**Veritabanı posta profili** | Linux üzerinde SQL Server için varsayılan veritabanı posta profilini ayarlayın. |
|**Yüksek kullanılabilirlik** | Kullanılabilirlik gruplarını etkinleştirin. |
|**Microsoft Dağıtılmış İşlem Düzenleyicisi** | Linux üzerinde MSDTC 'yi yapılandırma ve sorunlarını giderme. SQL Edge için ek dağıtılmış işlem ile ilgili yapılandırma seçenekleri desteklenmez. Bu ek yapılandırma seçenekleri hakkında daha fazla bilgi için bkz. [MSDTC 'Yi yapılandırma](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-mssql-conf#msdtc). |
|**ML Hizmetleri EULA 'Ları** | Azure Machine Learning paketleri için R ve Python EULA 'Ları kabul edin. Yalnızca SQL Server 2019 için geçerlidir.|
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

## <a name="run-azure-sql-edge-as-non-root-user"></a>Azure SQL Edge 'i kök olmayan kullanıcı olarak çalıştır

Varsayılan olarak, Azure SQL Edge kapsayıcıları kök olmayan kullanıcı/grup ile çalışır. Azure Marketi aracılığıyla (veya Docker Run kullanılarak) dağıtıldığında, farklı bir Kullanıcı/Grup belirtilmediği sürece SQL Edge kapsayıcıları MSSQL (kök olmayan) Kullanıcı olarak başlatılır. Dağıtım sırasında farklı bir kök olmayan kullanıcı belirtmek için `*"User": "<name|uid>[:<group|gid>]"*` kapsayıcı oluşturma seçenekleri altına anahtar-değer çiftini ekleyin. SQL Edge aşağıdaki örnekte Kullanıcı olarak başlatılacak şekilde yapılandırılmıştır `*IoTAdmin*` .

```json
{
    ..
    ..
    ..
    "User": "IoTAdmin",
    "Env": [
        "MSSQL_AGENT_ENABLED=TRUE",
        "ClientTransportType=AMQP_TCP_Only",
        "MSSQL_PID=Premium"
    ]
}
```

Kök olmayan kullanıcının bağlı birimlerde bulunan DB dosyalarına erişmesine izin vermek için, kapsayıcıyı çalıştırdığınız Kullanıcı/grubun, kalıcı dosya depolaması üzerinde okuma & yazma izinlerine sahip olduğundan emin olun. Aşağıdaki örnekte, kök olmayan kullanıcıyı dosyaların sahibi olarak user_id 10001 ile ayarlayacağız. 

```bash
chown -R 10001:0 <database file dir>
```

### <a name="upgrading-from-earlier-ctp-releases"></a>Önceki CTP sürümlerinden yükseltme

Azure SQL Edge 'in önceki tanıtıcısı, kök kullanıcılar olarak çalışacak şekilde yapılandırılmıştır. Önceki CTPs 'lerden yükseltme yaparken aşağıdaki seçenekler mevcuttur.

- Kök kullanıcıyı kullanmaya devam et-kök kullanıcıyı kullanmaya devam etmek Için `*"User": "0:0"*` kapsayıcı oluşturma seçenekleri altına anahtar-değer çiftini ekleyin.
- Varsayılan MSSQL kullanıcısını kullanın-varsayılan MSSQL kullanıcısını kullanmak Için aşağıdaki adımları izleyin
  - Docker konağına MSSQL adlı bir kullanıcı ekleyin. Aşağıdaki örnekte, KIMLIĞI 10001 olan bir Kullanıcı MSSQL 'si ekleyeceğiz. Bu Kullanıcı kök gruba da eklenir.
    ```bash
    sudo useradd -M -s /bin/bash -u 10001 -g 0 mssql
    ```
  - Veritabanı dosyasının bulunduğu dizin/bağlama biriminde izinleri değiştirme 
    ```bash
    sudo chgrp -R 0 /var/lib/docker/volumes/kafka_sqldata/
    sudo chmod -R g=u /var/lib/docker/volumes/kafka_sqldata/
    ```
- Farklı bir kök olmayan kullanıcı hesabı kullanın-kök olmayan farklı bir kullanıcı hesabı kullanmak Için
  - Kapsayıcı `*"User": "user_name | user_id*` oluşturma seçenekleri altında anahtar-değer çifti Ekle öğesini belirtmek için kapsayıcı oluşturma seçeneklerini güncelleştirin. User_name veya user_id, Docker konağından gerçek user_name veya user_id değiştirin. 
  - Dizin/bağlama birimi üzerindeki izinleri değiştirin.

## <a name="persist-your-data"></a>Verilerinizi kalıcı hale getirme

Kapsayıcısını ve ile yeniden başlatsanız bile, Azure SQL Edge yapılandırması değişiklikleriniz ve veritabanı dosyalarınız kapsayıcıda kalıcı hale getirilir `docker stop` `docker start` . Ancak, kapsayıcıyı ile kaldırırsanız `docker rm` , kapsayıcıdaki her şey Azure SQL Edge ve veritabanlarınız dahil olmak üzere silinir. Aşağıdaki bölümde, ilişkili kapsayıcılar silinse bile veritabanı dosyalarınızı kalıcı hale getirmek için **veri birimlerinin** nasıl kullanılacağı açıklanmaktadır.

> [!IMPORTANT]
> Azure SQL Edge için, Docker 'da veri kalıcılığını anlamanız kritik öneme sahiptir. Bu bölümdeki tartışmaya ek olarak, [Docker kapsayıcılarındaki verileri yönetme](https://docs.docker.com/engine/tutorials/dockervolumes/)hakkında Docker 'ın belgelerine bakın.

### <a name="mount-a-host-directory-as-data-volume"></a>Konak dizinini veri birimi olarak bağlama

İlk seçenek, ana bilgisayarınızda bir dizini bir veri birimi olarak kapsayıcınıza bağlamasıdır. Bunu yapmak için, `docker run` bayrağıyla komutunu kullanın `-v <host directory>:/var/opt/mssql` . Bu, verilerin kapsayıcı yürütmeler arasında geri yüklenmesine izin verir.

```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/azure-sql-edge
```

```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/azure-sql-edge
```

Bu teknik Ayrıca, ana bilgisayardaki dosyaları Docker dışında paylaşmanıza ve görüntülemenize olanak sağlar.

> [!IMPORTANT]
> **Windows üzerinde Docker** için konak birimi eşlemesi Şu anda tüm dizin eşlemesini desteklemiyor `/var/opt/mssql` . Ancak, ana bilgisayar makineniz gibi bir alt dizini eşleyebilirsiniz `/var/opt/mssql/data` .

> [!IMPORTANT]
> Azure SQL Edge görüntüsüyle **Mac üzerinde Docker** için konak birimi eşlemesi Şu anda desteklenmiyor. Bunun yerine veri birimi kapsayıcıları kullanın. Bu kısıtlama `/var/opt/mssql` dizine özeldir. Bağlı bir dizinden okuma sorunsuz çalışıyor. Örneğin, Mac üzerinde-v ' y i kullanarak bir konak dizini bağlayabilir ve konakta bulunan bir. bak dosyasından bir yedeklemeyi geri yükleyebilirsiniz.

### <a name="use-data-volume-containers"></a>Veri birimi kapsayıcıları kullanma

İkinci seçenek, bir veri birimi kapsayıcısı kullanmaktır. Parametresi ile bir konak dizini yerine bir birim adı belirterek bir veri birimi kapsayıcısı oluşturabilirsiniz `-v` . Aşağıdaki örnek, **sqlvolume**adlı bir paylaşılan veri birimi oluşturur.

```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/azure-sql-edge
```

```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/azure-sql-edge
```

> [!NOTE]
> Bu teknik, Çalıştır komutunda örtük olarak bir veri birimi oluşturmak için daha eski Docker sürümleriyle çalışmaz. Bu durumda, Docker belgelerinde belirtilen açık adımları kullanın, [bir veri birimi kapsayıcısı oluşturup bağlama](https://docs.docker.com/engine/tutorials/dockervolumes/#creating-and-mounting-a-data-volume-container).

Bu kapsayıcıyı durdurup kaldırsanız bile, veri hacmi devam ettirir. `docker volume ls`Komutunu komutuyla görüntüleyebilirsiniz.

```bash
docker volume ls
```

Daha sonra aynı birim adına sahip başka bir kapsayıcı oluşturursanız, yeni kapsayıcı birimde bulunan aynı Azure SQL Edge verilerini kullanır.

Bir veri birimi kapsayıcısını kaldırmak için `docker volume rm` komutunu kullanın.

> [!WARNING]
> Veri birimi kapsayıcısını silerseniz, kapsayıcıdaki tüm Azure SQL Edge verileri *kalıcı olarak* silinir.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL Edge 'e bağlanma](connect.md)
- [SQL Edge ile uçtan uca IoT çözümü oluşturma](tutorial-deploy-azure-resources.md)
