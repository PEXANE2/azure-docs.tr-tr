---
title: Desteklenen veri platformları
titleSuffix: Azure Data Science Virtual Machine
description: Azure Veri Bilimi Sanal Makinesi için desteklenen veri platformları ve araçları hakkında bilgi edinin.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: lobrien
ms.author: laobri
ms.topic: conceptual
ms.date: 12/12/2019
ms.openlocfilehash: cd787881957d78f179107e46b2650de4618c7724
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80282333"
---
# <a name="data-platforms-supported-on-the-data-science-virtual-machine"></a>Veri Bilimi Sanal Makinesi desteklenen veri platformları

Veri Bilimi Sanal Makinesi (DSVM) ile analizlerinizi çok çeşitli veri platformları için oluşturabilirsiniz. Uzak veri platformlarına yönelik arabirimlerin yanı sıra, DSVM, hızlı geliştirme ve prototip oluşturma için yerel bir örnek sağlar.

DSVM 'de aşağıdaki veri platformu araçları desteklenir.

## <a name="sql-server-developer-edition"></a>SQL Server Developer Edition

| | |
| ------------- | ------------- |
| Bu nedir?   | Yerel bir ilişkisel veritabanı örneği      |
| Desteklenen DSVM sürümleri      | Windows 2016: SQL Server 2017, Windows 2019: SQL Server 2019      |
| Tipik kullanımlar      | Daha küçük veri kümesiyle yerel olarak hızlı geliştirme <br/> Veritabanı Içinde Çalıştır R   |
| Örneklere bağlantılar      |    Yeni bir York City veri kümesinin küçük bir örneği SQL veritabanına yüklenir:<br/>  `nyctaxi` <br/> Microsoft Machine Learning Server ve veritabanı içi analizlerini gösteren Jupyter örneği şurada bulunabilir:<br/> `~notebooks/SQL_R_Services_End_to_End_Tutorial.ipynb`  |
| DSVM 'deki ilgili araçlar       | SQL Server Management Studio <br/> ODBC/JDBC sürücüleri<br/> pyodbc, RODBC<br />Apache Drill      |

> [!NOTE]
> SQL Server Developer sürümü yalnızca geliştirme ve test amaçları için kullanılabilir. Üretimde çalıştırmak için bir lisansa veya SQL Server sanal makinelerinden birine ihtiyacınız vardır.


### <a name="setup"></a>Kurulum

Veritabanı sunucusu zaten önceden yapılandırılmıştır ve SQL Server (gibi `SQL Server (MSSQLSERVER)`) Ile ilgili Windows Hizmetleri otomatik olarak çalışacak şekilde ayarlanır. Tek el ile yapılan adım, Microsoft Machine Learning Server kullanarak veritabanı Içi analizlerin etkinleştirilmesini içerir. SQL Server Management Studio (SSMS) ' de bir kerelik eylem olarak aşağıdaki komutu çalıştırarak analizlerinizi etkinleştirebilirsiniz. Makine Yöneticisi olarak oturum açtıktan sonra SSMS 'de yeni bir sorgu açarak ve seçilen veritabanının şu olduğundan `master`emin olduktan sonra bu komutu çalıştırın:

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 

        (Replace %COMPUTERNAME% with your VM name.)
       
SQL Server Management Studio çalıştırmak için program listesinde "SQL Server Management Studio" araması yapabilir veya Windows Search kullanarak dosyayı bulup çalıştırabilirsiniz. Kimlik bilgileri istendiğinde, **Windows kimlik doğrulaması** ' nı seçin ve makine adını veya ```localhost``` **SQL Server adı** alanını kullanın.

### <a name="how-to-use-and-run-it"></a>Kullanma ve çalıştırma

Varsayılan olarak, varsayılan veritabanı örneğiyle veritabanı sunucusu otomatik olarak çalışır. SQL Server veritabanına yerel olarak erişmek için VM 'deki SQL Server Management Studio gibi araçları kullanabilirsiniz. Yerel yönetici hesaplarının, veritabanında Yönetici erişimi vardır.

Ayrıca, DSVM, Python ve Machine Learning Server dahil olmak üzere birden çok dilde yazılan uygulamalardan SQL Server, Azure SQL veritabanı ve Azure SQL veri ambarı ile konuşmak için ODBC ve JDBC sürücülerle birlikte gelir.

### <a name="how-is-it-configured-and-installed-on-the-dsvm"></a>DSVM 'de nasıl yapılandırılır ve yüklenir? 

 SQL Server, standart şekilde yüklenir. Bu, adresinde `C:\Program Files\Microsoft SQL Server`bulunabilir. Veritabanı Içi Machine Learning Server örneği konumunda `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES`bulunur. DSVM 'de `C:\Program Files\Microsoft\R Server\R_SERVER`' de yüklü olan ayrı bir tek başına Machine Learning Server örneği de vardır. Bu iki Machine Learning Server örneği kitaplıkları paylaşmaz.


## <a name="apache-spark-2x-standalone"></a>Apache Spark 2. x (tek başına)

| | |
| ------------- | ------------- |
| Bu nedir?   | Popüler Apache Spark platformunun tek başına (işlem içi tek düğümlü) örneği; hızlı, büyük ölçekli veri işleme ve makine öğrenimi için bir sistem     |
| Desteklenen DSVM sürümleri      | Linux     |
| Tipik kullanımlar      | * Azure HDInsight gibi büyük Spark kümelerinde daha küçük bir veri kümesiyle ve daha sonra dağıtımıyla Spark/PySpark uygulamalarının hızla geliştirilmesi<br/> * Test Microsoft Machine Learning Server Spark bağlamı <br />* ML uygulamaları derlemek için parlak ml veya Microsoft 'un açık kaynaklı bir [Mmlspark](https://github.com/Azure/mmlspark) kitaplığı kullanın |
| Örneklere bağlantılar      |    Jupyter örneği: <br />&nbsp;&nbsp;* ~/Notebooks/parlak ml/pyspark <br /> &nbsp;&nbsp;* ~/notebooks/MMLSpark <br /> Microsoft Machine Learning Server (Spark bağlamı):/Dsvm/Samples/Mrs/mrsmini Contextsample.exe |
| DSVM 'deki ilgili araçlar       | PySpark, Scala<br/>Jupyter (Spark/PySpark kernels)<br/>Microsoft Machine Learning Server, parlak r, parlak LYR <br />Apache Drill      |

### <a name="how-to-use-it"></a>Nasıl kullanılır?
`spark-submit` Veya `pyspark` komutunu çalıştırarak Spark işlerini komut satırına gönderebilirsiniz. Spark çekirdeğiyle yeni bir not defteri oluşturarak da Jupyter Not defteri oluşturabilirsiniz.

R 'den Spark kullanarak, DSVM 'de bulunan parlak r, parlak LYR ve Microsoft Machine Learning Server gibi kitaplıkları kullanabilirsiniz. Yukarıdaki tablodaki örneklere yönelik işaretçilere bakın.

### <a name="setup"></a>Kurulum
Ubuntu Linux DSVM sürümünde Microsoft Machine Learning Server bir Spark bağlamında çalışmadan önce, yerel tek düğümlü Hadoop ve Yarn örneğini etkinleştirmek için bir kerelik kurulum adımını doldurmanız gerekir. Hadoop Hizmetleri varsayılan olarak DSVM 'de yüklüdür ancak devre dışı bırakılır. Bunları etkinleştirmek için, ilk kez kök olarak aşağıdaki komutları çalıştırın:

    echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
    cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
    chmod 0600 ~hadoop/.ssh/authorized_keys
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
    chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
    systemctl start hadoop-namenode hadoop-datanode hadoop-yarn

Artık bu hizmetleri çalıştırarak ```systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn```Hadoop ile ilgili hizmetleri durdurabilirsiniz.

Bir uzak Spark bağlamında MRS 'nin nasıl geliştirileceğini ve test alınacağını gösteren bir örnek (DSVM 'deki tek başına Spark örneği), `/dsvm/samples/MRS` dizinde sağlanır ve kullanılabilir.


### <a name="how-is-it-configured-and-installed-on-the-dsvm"></a>DSVM 'de nasıl yapılandırılır ve yüklenir? 
|Platform|Konum yüklemeyi ($SPARK _HOME)|
|:--------|:--------|
|Linux   | /dsvm/tools/spark-X.X.X-bin-hadoopX.X|


Microsoft MMLSpark Machine-Learning kitaplıklarını kullanarak Azure Blob depolama veya Azure Data Lake Storage verilere erişmek için kitaplıklar, $SPARK _HOME/jars. Spark başladığında bu Jliler otomatik olarak yüklenir. Spark, varsayılan olarak yerel diskte verileri kullanır. 

DSVM 'deki Spark örneğinin blob depolamada veya Azure Data Lake Storage depolanan verilere erişmesi için, $SPARK _HOME/conf/Core-site.xml.Template. ' de bulunan şablona göre dosyayı oluşturmanız `core-site.xml` ve yapılandırmanız gerekir. Ayrıca blob depolamaya ve Azure Data Lake Storage erişmek için uygun kimlik bilgilerine sahip olmanız gerekir. (Şablon dosyalarının BLOB depolama ve Azure Data Lake Storage yapılandırmalarına yönelik yer tutucuları kullandığına unutmayın.)

Azure Data Lake Storage hizmeti kimlik bilgileri oluşturma hakkında daha ayrıntılı bilgi için bkz. [Azure Data Lake Storage 1. Ile kimlik doğrulama](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory). Blob Storage veya Azure Data Lake Storage kimlik bilgileri Core-site. xml dosyasına girildikten sonra, bu kaynaklarda depolanan verilere wasb://veya adl://URI öneki aracılığıyla başvurabilirsiniz.

