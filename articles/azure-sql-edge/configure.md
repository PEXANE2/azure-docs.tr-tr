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
ms.date: 07/28/2020
ms.openlocfilehash: 722d33e76b6009a44811dfcb8a3238b042ec6918
ms.sourcegitcommit: d39f2cd3e0b917b351046112ef1b8dc240a47a4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88816890"
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

Azure SQL Edge CTP 2.2 ile başlayarak, SQL Edge kapsayıcıları kök olmayan kullanıcı/grup ile çalışabilir. Azure Marketi aracılığıyla dağıtıldığında, farklı bir Kullanıcı/Grup belirtilmedikçe, SQL Edge kapsayıcıları MSSQL (kök olmayan) Kullanıcı olarak başlar. Dağıtım sırasında farklı bir kök olmayan kullanıcı belirtmek için `*"User": "<name|uid>[:<group|gid>]"*` kapsayıcı oluşturma seçenekleri altına anahtar-değer çiftini ekleyin. SQL Edge aşağıdaki örnekte Kullanıcı olarak başlatılacak şekilde yapılandırılmıştır `*IoTAdmin*` .

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

Azure SQL Edge 'in önceki CTP, kök kullanıcılar olarak çalışacak şekilde yapılandırılmıştır. Önceki CTP 'lerden yükseltirken aşağıdaki seçenekler mevcuttur

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
  - Kapsayıcı `*"User": "user_name | user_id*` oluşturma seçenekleri altında anahtar-değer çifti Ekle öğesini belirtmek için kapsayıcı oluşturma seçeneklerini güncelleştirin. Lütfen user_name veya user_id, Docker konağından gerçek user_name veya user_id değiştirin. 
  - Dizin/bağlama birimi üzerindeki izinleri değiştirin.

## <a name="persist-your-data"></a>Verilerinizi kalıcı hale getirme

Kapsayıcısını ve ile yeniden başlatsanız bile, Azure SQL Edge yapılandırması değişiklikleriniz ve veritabanı dosyalarınız kapsayıcıda kalıcı hale getirilir `docker stop` `docker start` . Ancak, kapsayıcıyı ile kaldırırsanız `docker rm` , kapsayıcıdaki her şey Azure SQL Edge ve veritabanlarınız dahil olmak üzere silinir. Aşağıdaki bölümde, ilişkili kapsayıcılar silinse bile veritabanı dosyalarınızı kalıcı hale getirmek için **veri birimlerinin** nasıl kullanılacağı açıklanmaktadır.

> [!IMPORTANT]
> Azure SQL Edge için, Docker 'da veri kalıcılığını anlamanız kritik öneme sahiptir. Bu bölümdeki tartışmaya ek olarak, [Docker kapsayıcılarındaki verileri yönetme](https://docs.docker.com/engine/tutorials/dockervolumes/)hakkında Docker 'ın belgelerine bakın.

### <a name="mount-a-host-directory-as-data-volume"></a>Konak dizinini veri birimi olarak bağlama

İlk seçenek, ana bilgisayarınızda bir dizini bir veri birimi olarak kapsayıcınıza bağlamasıdır. Bunu yapmak için, `docker run` bayrağıyla komutunu kullanın `-v <host directory>:/var/opt/mssql` . Bu, verilerin kapsayıcı yürütmeler arasında geri yüklenmesine izin verir.

```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/azure-sql-edge-developer
```

```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/azure-sql-edge-developer
```

Bu teknik Ayrıca, ana bilgisayardaki dosyaları Docker dışında paylaşmanıza ve görüntülemenize olanak sağlar.

> [!IMPORTANT]
> **Windows üzerinde Docker** için konak birimi eşlemesi Şu anda tüm dizin eşlemesini desteklemiyor `/var/opt/mssql` . Ancak, ana bilgisayar makineniz gibi bir alt dizini eşleyebilirsiniz `/var/opt/mssql/data` .

> [!IMPORTANT]
> Azure SQL Edge görüntüsüyle **Mac üzerinde Docker** için konak birimi eşlemesi Şu anda desteklenmiyor. Bunun yerine veri birimi kapsayıcıları kullanın. Bu kısıtlama `/var/opt/mssql` dizine özeldir. Bağlı bir dizinden okuma sorunsuz çalışıyor. Örneğin, Mac üzerinde-v ' y i kullanarak bir konak dizini bağlayabilir ve konakta bulunan bir. bak dosyasından bir yedeklemeyi geri yükleyebilirsiniz.

### <a name="use-data-volume-containers"></a>Veri birimi kapsayıcıları kullanma

İkinci seçenek, bir veri birimi kapsayıcısı kullanmaktır. Parametresi ile bir konak dizini yerine bir birim adı belirterek bir veri birimi kapsayıcısı oluşturabilirsiniz `-v` . Aşağıdaki örnek, **sqlvolume**adlı bir paylaşılan veri birimi oluşturur.

```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/azure-sql-edge-developer
```

```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/azure-sql-edge-developer
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
