---
title: Desteklenen veri platformları
titleSuffix: Azure Data Science Virtual Machine
description: Azure Veri Bilimi Sanal Makinesi için desteklenen veri platformları ve araçları hakkında bilgi edinin.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 03/16/2018
ms.openlocfilehash: 5dbaf969420f066698a07b8d137d2ba44fc99080
ms.sourcegitcommit: 5f67772dac6a402bbaa8eb261f653a34b8672c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2019
ms.locfileid: "70208126"
---
# <a name="data-platforms-supported-on-the-data-science-virtual-machine"></a>Veri bilimi sanal makinesi üzerinde desteklenen veri platformları

Veri Bilimi Sanal Makinesi (DSVM) ile analizlerinizi çok çeşitli veri platformları için oluşturabilirsiniz. Uzak Veri platformları arabirimlerine ek olarak, DSVM prototip oluşturma ve hızlı geliştirme için yerel bir örnek sağlar.

DSVM 'de aşağıdaki veri platformu araçları desteklenir.

## <a name="sql-server-2016-developer-edition"></a>SQL Server 2016 Geliştirici sürümü

| | |
| ------------- | ------------- |
| Nedir?   | Yerel bir ilişkisel veritabanı      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanımları      | Yerel olarak daha küçük veri kümesi ile hızlı geliştirme <br/> Veritabanında R çalıştırın   |
| Örneklere bağlantılar      |    Yeni bir York City veri kümesinin küçük bir örneği SQL veritabanına yüklenir:<br/>  `nyctaxi` <br/> Microsoft Machine Learning Server ve veritabanı içi analizlerini gösteren Jupyter örneği şurada bulunabilir:<br/> `~notebooks/SQL_R_Services_End_to_End_Tutorial.ipynb`  |
| DSVM 'deki ilgili araçlar       | SQL Server Management Studio <br/> ODBC/JDBC sürücüleri<br/> pyodbc, RODBC<br />Apache ayrıntıya      |

> [!NOTE]
> SQL Server 2016 geliştirici sürümü yalnızca geliştirme ve test amaçları için kullanılabilir. Bir lisans ya da bir üretim ortamında çalıştırmak için SQL Server Vm'leri gerekir.


### <a name="setup"></a>Kurulum

Veritabanı sunucusu zaten önceden yapılandırılmıştır ve SQL Server (gibi `SQL Server (MSSQLSERVER)`) ile ilgili Windows Hizmetleri otomatik olarak çalışacak şekilde ayarlanır. Tek el ile yapılan adım, Microsoft Machine Learning Server kullanarak veritabanı Içi analizlerin etkinleştirilmesini içerir. Bunu, SQL Server Management Studio (SSMS) ' de bir kerelik eylem olarak aşağıdaki komutu çalıştırarak yapabilirsiniz. Makine Yöneticisi olarak oturum açtıktan sonra SSMS 'de yeni bir sorgu açarak ve seçilen veritabanının şu olduğundan `master`emin olduktan sonra bu komutu çalıştırın:

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 

        (Replace %COMPUTERNAME% with your VM name.)
       
SQL Server Management Studio çalıştırmak için program listesinde "SQL Server Management Studio" araması yapabilir veya Windows Search kullanarak dosyayı bulup çalıştırabilirsiniz. Kimlik bilgileri istendiğinde, **Windows kimlik doğrulaması** ' nı seçin ve makine adını veya ```localhost``` **SQL Server adı** alanını kullanın.

### <a name="how-to-use-and-run-it"></a>Kullanma ve çalıştırma

Varsayılan olarak, varsayılan veritabanı örneğiyle veritabanı sunucusu otomatik olarak çalışır. Yerel SQL Server veritabanına erişmek için VM üzerinde SQL Server Management Studio gibi araçlar kullanın. Yerel yönetici hesaplarının, veritabanında Yönetici erişimi vardır.

Ayrıca, DSVM, Python ve Machine Learning Server dahil olmak üzere birden çok dilde yazılan uygulamalardan SQL Server, Azure SQL veritabanı ve Azure SQL veri ambarı ile konuşmak için ODBC ve JDBC sürücülerle birlikte gelir.

### <a name="how-is-it-configured-and-installed-on-the-dsvm"></a>DSVM 'de nasıl yapılandırılır ve yüklenir? 

 SQL Server, standart şekilde yüklenir. Şurada bulunabilir `C:\Program Files\Microsoft SQL Server`. Veritabanı Içi Machine Learning Server örneği konumunda `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES`bulunur. DSVM 'de `C:\Program Files\Microsoft\R Server\R_SERVER`' de yüklü olan ayrı bir tek başına Machine Learning Server örneği de vardır. Bu iki Machine Learning Server örneği kitaplıkları paylaşmaz.


## <a name="apache-spark-2x-standalone"></a>Apache Spark 2.x (tek başına)

| | |
| ------------- | ------------- |
| Nedir?   | Popüler Apache Spark platformunun tek başına (işlem içi tek düğümlü) örneği; hızlı, büyük ölçekli veri işleme ve makine öğrenimi için bir sistem     |
| Desteklenen DSVM sürümleri      | Linux <br /> Windows (Deneysel)      |
| Tipik kullanımları      | * Azure HDInsight gibi büyük Spark kümelerinde daha küçük bir veri kümesiyle ve daha sonra dağıtımıyla Spark/PySpark uygulamalarının hızla geliştirilmesi<br/> * Test Microsoft Machine Learning Server Spark bağlamı <br />* ML uygulamaları derlemek için parlak ml veya Microsoft 'un açık kaynaklı bir [Mmlspark](https://github.com/Azure/mmlspark) kitaplığı kullanın |
| Örneklere bağlantılar      |    Jupyter örneği: <br />&nbsp;&nbsp;* ~/notebooks/SparkML/pySpark <br /> &nbsp;&nbsp;* ~/notebooks/MMLSpark <br /> Microsoft Machine Learning Server (Spark bağlamı):/Dsvm/Samples/Mrs/mrsmini Contextsample.exe |
| DSVM 'deki ilgili araçlar       | PySpark, Scala<br/>Jupyter (Spark/PySpark çekirdekleri)<br/>Microsoft Machine Learning Server, parlak r, parlak LYR <br />Apache ayrıntıya      |

### <a name="how-to-use-it"></a>Kullanımı
`spark-submit` Veya`pyspark` komutunu çalıştırarak Spark işlerini komut satırına gönderebilirsiniz. Yeni bir not defteri ile Spark çekirdek oluşturarak, Jupyter Not Defteri de oluşturabilirsiniz.

R 'den Spark kullanarak, DSVM 'de bulunan parlak r, parlak LYR ve Microsoft Machine Learning Server gibi kitaplıkları kullanabilirsiniz. Önceki tabloda örnekleri işaretçileri bakın.

> [!NOTE]
> DSVM 'nin Spark bağlamında Microsoft Machine Learning Server çalıştırmak yalnızca Ubuntu Linux DSVM sürümünde desteklenir.



### <a name="setup"></a>Kurulum
Ubuntu Linux DSVM sürümünde Microsoft Machine Learning Server bir Spark bağlamında çalışmadan önce, yerel tek düğümlü Hadoop ve Yarn örneğini etkinleştirmek için bir kerelik kurulum adımını doldurmanız gerekir. Varsayılan olarak, Hadoop Hizmetleri yüklendi ancak DSVM'nin devre dışı. Bunları etkinleştirmek için, ilk kez kök olarak aşağıdaki komutları çalıştırın:

    echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
    cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
    chmod 0600 ~hadoop/.ssh/authorized_keys
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
    chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
    systemctl start hadoop-namenode hadoop-datanode hadoop-yarn

Artık bu hizmetleri çalıştırarak ```systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn```Hadoop ile ilgili hizmetleri durdurabilirsiniz.

Bir uzak Spark bağlamında Mrs 'nin nasıl geliştirileceğini ve test alınacağını gösteren bir örnek (dsvm 'deki tek başına Spark örneği), `/dsvm/samples/MRS` dizinde sağlanır ve kullanılabilir.


### <a name="how-is-it-configured-and-installed-on-the-dsvm"></a>DSVM 'de nasıl yapılandırılır ve yüklenir? 
|Platform|Yükleme konumu ($SPARK_HOME)|
|:--------|:--------|
|Windows | c:\dsvm\tools\spark-X.X.X-bin-hadoopX.X|
|Linux   | /dsvm/Tools/Spark-X.X.X-bin-hadoopX.X|


Microsoft MMLSpark makine öğrenimi kitaplıklarını kullanarak Azure Blob Storage veya Azure Data Lake Storage verilerine erişmek için kitaplıklar, $SPARK _HOME/jars ' a önceden yüklenir. Bu jar dosyaları dışındaki, Spark başlatıldığında otomatik olarak yüklenir. Spark, varsayılan olarak yerel diskte verileri kullanır. 

Dsvm 'deki Spark örneğinin blob depolamada veya Azure Data Lake Storage depolanan verilere erişmesi için, $Spark _home/conf/Core-site. xml. `core-site.xml` Template dosyasında bulunan şablona göre dosyayı oluşturmanız ve yapılandırmanız gerekir. Ayrıca blob depolamaya ve Azure Data Lake Storage erişmek için uygun kimlik bilgilerine sahip olmanız gerekir. (Şablon dosyalarının BLOB depolama ve Azure Data Lake Storage yapılandırmalarına yönelik yer tutucuları kullandığına unutmayın.)

Azure Data Lake Storage hizmeti kimlik bilgileri oluşturma hakkında daha ayrıntılı bilgi için bkz. [Azure Data Lake Storage 1. Ile kimlik doğrulama](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory). Blob Storage veya Azure Data Lake Storage kimlik bilgileri Core-site. xml dosyasına girildikten sonra, bu kaynaklarda depolanan verilere wasb://veya adl://URI öneki aracılığıyla başvurabilirsiniz.

