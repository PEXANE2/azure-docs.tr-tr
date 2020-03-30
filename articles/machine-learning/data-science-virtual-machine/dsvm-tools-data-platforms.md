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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80282333"
---
# <a name="data-platforms-supported-on-the-data-science-virtual-machine"></a>Veri Bilimi Sanal Makine'de desteklenen veri platformları

Veri Bilimi Sanal Makinesi (DSVM) ile analizlerinizi çok çeşitli veri platformlarına karşı oluşturabilirsiniz. Uzak veri platformlarına arayüzlere ek olarak, DSVM hızlı geliştirme ve prototipleme için yerel bir örnek sağlar.

Aşağıdaki veri platformu araçları DSVM'de desteklenir.

## <a name="sql-server-developer-edition"></a>SQL Server Developer Edition

| | |
| ------------- | ------------- |
| Bu nedir?   | Yerel ilişkisel veritabanı örneği      |
| Desteklenen DSVM sürümleri      | Windows 2016: SQL Server 2017, Windows 2019: SQL Server 2019      |
| Tipik kullanımlar      | Daha küçük veri kümesi ile yerel olarak hızlı gelişme <br/> Veritabanı nda R çalıştır   |
| Örneklere bağlantılar      |    New York'taki bir veri kümesinin küçük bir örneği SQL veritabanına yüklenir:<br/>  `nyctaxi` <br/> Microsoft Machine Learning Server ve veritabanı içi analizlerini gösteren jupyter örneğine şu adresi bulabilirsiniz:<br/> `~notebooks/SQL_R_Services_End_to_End_Tutorial.ipynb`  |
| DSVM ile ilgili araçlar       | SQL Server Management Studio <br/> ODBC/JDBC sürücüleri<br/> pyodbc, RODBC<br />Apache Drill      |

> [!NOTE]
> SQL Server Developer Edition yalnızca geliştirme ve test amacıyla kullanılabilir. Üretimde çalıştırmak için bir lisansa veya SQL Server VM'lerden birine ihtiyacınız vardır.


### <a name="setup"></a>Kurulum

Veritabanı sunucusu zaten önceden yapılandırılmıştır ve SQL Server ile `SQL Server (MSSQLSERVER)`ilgili Windows hizmetleri (benzeri) otomatik olarak çalışacak şekilde ayarlanmıştır. Tek manuel adım, Microsoft Machine Learning Server'ı kullanarak veritabanı içi analizleri etkinleştirmektir. Aşağıdaki komutu SQL Server Management Studio'da (SSMS) tek seferlik bir eylem olarak çalıştırarak analitiği etkinleştirebilirsiniz. Makine yöneticisi olarak oturum açtıktan sonra bu komutu çalıştırın, SSMS'te `master`yeni bir sorgu açın ve seçili veritabanının aşağıdakilerden emin olun:

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 

        (Replace %COMPUTERNAME% with your VM name.)
       
SQL Server Management Studio'yu çalıştırmak için program listesinde "SQL Server Management Studio"yu arayabilir veya bulmak ve çalıştırmak için Windows Arama'yı kullanabilirsiniz. Kimlik bilgileri istendiğinde, **Windows Kimlik Doğrulaması'nı** seçin ve makine adını veya ```localhost``` SQL Server **Name** alanında kullanın.

### <a name="how-to-use-and-run-it"></a>Nasıl kullanılır ve çalıştırın

Varsayılan olarak, varsayılan veritabanı örneği olan veritabanı sunucusu otomatik olarak çalışır. SQL Server veritabanına yerel olarak erişmek için VM'deki SQL Server Management Studio gibi araçları kullanabilirsiniz. Yerel yönetici hesaplarıveritabanında yönetici erişimine sahiptir.

Ayrıca, DSVM, Python ve Machine Learning Server dahil olmak üzere birden çok dilde yazılmış uygulamalardan SQL Server, Azure SQL veritabanları ve Azure SQL Veri Ambarı ile konuşmak için ODBC ve JDBC sürücüleri ile birlikte gelir.

### <a name="how-is-it-configured-and-installed-on-the-dsvm"></a>Nasıl yapılandırılır ve DSVM yüklü? 

 SQL Server standart şekilde yüklenir. Bu da `C:\Program Files\Microsoft SQL Server`bulunabilir . Veritabanı içi Machine Learning Server örneğinde `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES`bulunur. DSVM de yüklü ayrı bir bağımsız Machine Learning Server `C:\Program Files\Microsoft\R Server\R_SERVER`örneği vardır. Bu iki Machine Learning Server örneği kitaplıkları paylaşmaz.


## <a name="apache-spark-2x-standalone"></a>Apache Spark 2.x (Tek Başına)

| | |
| ------------- | ------------- |
| Bu nedir?   | Popüler Apache Spark platformunun tek başına (tek düğüm işlem) örneği; hızlı, büyük ölçekli veri işleme ve makine öğrenimi için bir sistem     |
| Desteklenen DSVM sürümleri      | Linux     |
| Tipik kullanımlar      | * Daha küçük bir veri seti ile Yerel olarak Spark/PySpark uygulamalarının hızlı gelişimi ve daha sonra Azure HDInsight gibi büyük Spark kümelerinde dağıtım<br/> * Test Microsoft Machine Learning Server Kıvılcım bağlam <br />* ML uygulamaları oluşturmak için SparkML veya Microsoft'un açık kaynak [MMLSpark](https://github.com/Azure/mmlspark) kitaplığını kullanın |
| Örneklere bağlantılar      |    Jupyter örnek: <br />&nbsp;&nbsp;* ~/dizüstü bilgisayarlar/SparkML/pySpark <br /> &nbsp;&nbsp;* ~/dizüstü bilgisayarlar/MMLSpark <br /> Microsoft Machine Learning Server (Spark bağlamı): /dsvm/samples/MRS/MRSSparkContextSample.R |
| DSVM ile ilgili araçlar       | PySpark, Scala<br/>Jupyter (Kıvılcım / PySpark Çekirdekleri)<br/>Microsoft Machine Learning Server, SparkR, Sparklyr <br />Apache Drill      |

### <a name="how-to-use-it"></a>Nasıl kullanılır?
Spark işlerini komut satırında `spark-submit` veya `pyspark` komutu çalıştırarak gönderebilirsiniz. Ayrıca, Spark çekirdeğiyle yeni bir dizüstü bilgisayar oluşturarak bir Jupyter dizüstü bilgisayar oluşturabilirsiniz.

Spark from R'yi, DSVM'de bulunan SparkR, Sparklyr ve Microsoft Machine Learning Server gibi kitaplıkları kullanarak kullanabilirsiniz. Önceki tablodaki örneklerin işaretçilerine bakın.

### <a name="setup"></a>Kurulum
Ubuntu Linux DSVM sürümünde Microsoft Machine Learning Server'da Spark bağlamında çalışmadan önce, yerel bir tek düğüm Hadoop HDFS ve İplik örneğini etkinleştirmek için tek seferlik bir kurulum adımLarını tamamlamanız gerekir. Varsayılan olarak, Hadoop hizmetleri yüklenir, ancak DSVM'de devre dışı bırakılır. Bunları etkinleştirmek için, aşağıdaki komutları ilk kez kök olarak çalıştırın:

    echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
    cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
    chmod 0600 ~hadoop/.ssh/authorized_keys
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
    chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
    systemctl start hadoop-namenode hadoop-datanode hadoop-yarn

Artık çalıştırarak ```systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn```bunlara ihtiyacınız olmadığında Hadoop ile ilgili hizmetleri durdurabilirsiniz.

Mrs'in uzak bir Kıvılcım bağlamında (DSVM'deki tek tek Kıvılcım örneği) nasıl geliştirilip test edilebildiğini `/dsvm/samples/MRS` gösteren bir örnek sağlanır ve dizinde kullanılabilir.


### <a name="how-is-it-configured-and-installed-on-the-dsvm"></a>Nasıl yapılandırılır ve DSVM yüklü? 
|Platform|Konum Yükle ($SPARK_HOME)|
|:--------|:--------|
|Linux   | /dsvm/tools/spark-X.X.X-bin-hadoopX.X|


Microsoft MMLSpark makine öğrenimi kitaplıklarını kullanarak Azure Blob depolama veya Azure Veri Gölü Depolama'dan verilere erişmek için kitaplıklar $SPARK_HOME/jar'larda önceden yüklenir. Spark başlatıldığında bu JA'lar otomatik olarak yüklenir. Varsayılan olarak, Spark yerel diskteki verileri kullanır. 

DSVM'deki Kıvılcım örneğinin Blob depolamaveya Azure Veri Gölü Depolama'da depolanan verilere `core-site.xml` erişmesi için dosyayı $SPARK_HOME/conf/core-site.xml.template'de bulunan şablona göre oluşturmanız ve yapılandırmanız gerekir. Blob depolama ve Azure Veri Gölü Depolama erişmek için uygun kimlik bilgilerine de sahip olmalısınız. (Şablon dosyalarının Blob depolama ve Azure Veri Gölü Depolama yapılandırmaları için yer tutucular kullandığını unutmayın.)

Azure Veri Gölü Depolama hizmeti kimlik bilgilerini oluşturma hakkında daha ayrıntılı bilgi için Bkz. [Azure Veri Gölü Depolama Gen1 ile Kimlik Doğrulama](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory). Blob depolama veya Azure Veri Gölü Depolama kimlik bilgileri core-site.xml dosyasına girilen sonra, wasb:// veya adl:// URI öneki aracılığıyla bu kaynaklarda depolanan verilere başvurulayabilirsiniz.

