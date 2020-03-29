---
title: SQL Server sanal makinesindeki verileri keşfedin - Takım Veri Bilimi Süreci
description: SQL veya Python gibi bir programlama dilini kullanarak Azure'da SQL Server VM'de depolanan verileri nasıl keşfedebilirsiniz?
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: ae8c7c43ecbf9bc625e1e46be3e2c71c8d57b6f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76720104"
---
# <a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Azure üzerindeki SQL Server Sanal Makinesi verilerini keşfetme

Bu makalede, Azure'da bir SQL Server VM'de depolanan verilerin nasıl keşfedileni kapsar. Verileri incelemek için SQL veya Python'u kullanın.

Bu [görev, Ekip Veri Bilimi Sürecinde](overview.md)bir adımdır.

> [!NOTE]
> Bu belgedeki örnek SQL deyimleri verilerin SQL Server'da olduğunu varsayar. Değilse, verilerinizi SQL Server'a nasıl taşıyarak taşıyabildiğini öğrenmek için bulut veri bilimi işlem haritasına bakın.
> 
> 

## <a name="explore-sql-data-with-sql-scripts"></a><a name="sql-dataexploration"></a>SQL komut dosyalarıyla SQL verilerini keşfedin
Burada, SQL Server'daki veri depolarını keşfetmek için kullanılabilecek birkaç örnek SQL komut dosyası verebilirsiniz.

1. Günlük gözlem sayısını alın
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. Düzeyleri kategorik bir sütunda alma
   
    `select  distinct <column_name> from <databasename>`
3. İki kategorik sütunun birleşimiyle düzey sayısını alma 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. Sayısal sütunlar için dağıtımı alın
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [!NOTE]
> Pratik bir örnek için, [NYC Taksi veri kümesini](https://www.andresmh.com/nyctaxitrips/) kullanabilir ve uçtan uca bir geçiş için [IPython Notebook ve SQL Server'ı kullanarak NYC Veri çekişmesi](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) başlıklı IPNB'ye bakabilirsiniz.
> 
> 

## <a name="explore-sql-data-with-python"></a><a name="python"></a>Python ile SQL verilerini keşfedin
Veriler SQL Server'da yken verileri keşfetmek ve özellikler oluşturmak için Python'u kullanmak, veri [bilimi ortamınızda İşlem Azure Blob verilerinde](data-blob.md)belgelenen Python kullanarak Azure blob'daki verileri işlemeye benzer. Veritabanındaki verileri pandadataframe'e yükleyin ve daha sonra daha fazla işlenebilir. Veritabanına bağlanma ve verileri bu bölümdeki DataFrame'e yükleme işlemini belgeliyoruz.

Aşağıdaki bağlantı dize biçimi pyodbc kullanarak Python'dan bir SQL Server veritabanına bağlanmak için kullanılabilir (sunucu adı, dbname, kullanıcı adı ve parolayı belirli değerlerle değiştirin):

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Python'daki [Pandalar kitaplığı,](https://pandas.pydata.org/) Python programlaması için veri işleme için zengin bir veri yapıları kümesi ve veri çözümleme araçları sağlar. Aşağıdaki kod, SQL Server veritabanından Pandas veri çerçevesine döndürülen sonuçları okur:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <columnname2>... from <tablename>''', conn)

Artık veri bilimi ortamınızda Konu [İşlemi Azure Blob verilerinde](data-blob.md)ele alınan Pandas DataFrame ile çalışabilirsiniz.

## <a name="the-team-data-science-process-in-action-example"></a>Eylem örneğinde Takım Veri Bilimi Süreci
Genel bir veri kümesi ni kullanarak Cortana Analitik Süreci'nin uçtan uca bir geçiş örneği için, [Takım Veri Bilimi Süreci'ni iş başında görün: SQL Server'ı kullanarak.](sql-walkthrough.md)

