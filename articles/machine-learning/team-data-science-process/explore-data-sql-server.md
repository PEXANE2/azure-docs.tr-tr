---
title: SQL Server sanal makine-Team Data Science Işlemindeki verileri araştırma
description: Azure 'da bir SQL Server VM depolanan verileri SQL veya Python gibi bir programlama dili ile nasıl keşfedebileceğiniz.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76720104"
---
# <a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Azure üzerindeki SQL Server Sanal Makinesi verilerini keşfetme

Bu makalede, Azure 'da bir SQL Server VM depolanan verilerin nasıl araştırabileceği açıklanır. Verileri incelemek için SQL veya Python kullanın.

Bu görev, [ekip veri bilimi işlemindeki](overview.md)bir adımdır.

> [!NOTE]
> Bu belgedeki örnek SQL deyimleri, verilerin SQL Server olduğunu varsayar. Değilse, verilerinizi SQL Server nasıl taşıyacağınızı öğrenmek için bulut veri bilimi süreç haritasına bakın.
> 
> 

## <a name="explore-sql-data-with-sql-scripts"></a><a name="sql-dataexploration"></a>SQL betikleri ile SQL verilerini keşfet
Aşağıda, SQL Server veri depolarını araştırmak için kullanılabilecek birkaç örnek SQL komut dosyası verilmiştir.

1. Gün başına gözlemlerin sayısını Al
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. Kategorik bir sütundaki düzeyleri al
   
    `select  distinct <column_name> from <databasename>`
3. İki kategorik sütunun birleşimi içindeki düzeylerin sayısını Al 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. Sayısal sütunlara yönelik dağıtımı al
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [!NOTE]
> Pratik bir örnek için, [NYC TAXI veri kümesini](https://www.andresmh.com/nyctaxitrips/) kullanabilir ve [IPython Not defteri 'ni kullanarak ıpnb başlıklı NYC veri denetimi](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) öğesine başvurabilirsiniz ve bir uçtan uca izlenecek yol SQL Server.
> 
> 

## <a name="explore-sql-data-with-python"></a><a name="python"></a>Python ile SQL verilerini keşfet
Veri [bilimi ortamınızda Azure blob verilerini işleme](data-blob.md)bölümünde belgelendiği gibi verileri incelemek ve veri SQL Server özellikler oluşturmak için Python kullanma. Veritabanından verileri bir Pandas DataFrame 'e yükleyin ve daha sonra işlem yapabilirsiniz. Veritabanına bağlanma ve verileri bu bölümdeki DataFrame 'e yükleme işlemini belgeliyoruz.

Aşağıdaki bağlantı dizesi biçimi pyodbc (ServerName, dbname, username ve Password değerlerini belirli değerlerinizle değiştirin) kullanılarak Python 'dan bir SQL Server veritabanına bağlanmak için kullanılabilir:

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Python 'daki [Pandas kitaplığı](https://pandas.pydata.org/) , Python programlamasına yönelik veri işleme için zengin veri yapıları ve veri çözümleme araçları sağlar. Aşağıdaki kod, bir SQL Server veritabanından bir Pandas veri çerçevesine döndürülen sonuçları okur:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <columnname2>... from <tablename>''', conn)

Artık, [veri bilimi ortamınızda Azure blob verilerini işleme](data-blob.md)konusunda ele alınan Pandas dataframe ile çalışabilirsiniz.

## <a name="the-team-data-science-process-in-action-example"></a>Takım veri bilimi Işlemi eylem örneği
Ortak bir veri kümesi kullanan Cortana Analytics Işleminin uçtan uca bir anlatım örneği için, bkz: [SQL Server using the Team Data Science Process](sql-walkthrough.md).

