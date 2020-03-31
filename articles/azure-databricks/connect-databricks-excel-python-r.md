---
title: "Excel, Python veya R'den Azure Veri Tuğlalarına bağlanma "
description: Azure Databricks'i Excel, Python veya R'ye bağlamak için Simba sürücüsünü nasıl kullanacağınızı öğrenin.
services: azure-databricks
author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: mamccrea
ms.openlocfilehash: f7494d36cf9b16ac6c7a1287a6ff96dd2285c6e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73601939"
---
# <a name="connect-to-azure-databricks-from-excel-python-or-r"></a>Excel, Python veya R'den Azure Veri Tuğlalarına bağlanma

Bu makalede, Azure Databricks'i Microsoft Excel, Python veya R diline bağlamak için Databricks ODBC sürücüsünü nasıl kullanacağınızı öğreneceksiniz. Bağlantıyı kurduktan sonra, Azure Veri Tuğlaları'ndaki verilere Excel, Python veya R istemcilerinden erişebilirsiniz. Verileri daha fazla çözümlemek için istemcileri de kullanabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

* Bir Azure Databricks çalışma alanınız, Bir Kıvılcım kümesiniz ve kümenizle ilişkili örnek verilere sahip olmalısınız. Bu ön koşullara zaten sahip değilseniz, Azure [portalını kullanarak Azure Databricks'te Bir Spark çalıştır işini](quickstart-create-databricks-workspace-portal.md)hızlı bir şekilde tamamlayın.

* [Databricks sürücü indirme sayfasından Databricks](https://databricks.com/spark/odbc-driver-download)ODBC sürücüsünü indirin. Azure Databricks'e bağlanmak istediğiniz uygulamadan bağlı olarak 32 bit veya 64 bit sürümü yükleyin. Örneğin, Excel'den bağlanmak için sürücünün 32 bit sürümünü yükleyin. R ve Python'dan bağlanmak için sürücünün 64 bit sürümünü yükleyin.

* Databricks'te kişisel erişim jetonu ayarlayın. Talimatlar [için, Bkz. Token yönetimi.](/azure/databricks/dev-tools/api/latest/authentication)

## <a name="set-up-a-dsn"></a>Bir DSN ayarlama

Bir veri kaynağı adı (DSN) belirli bir veri kaynağı hakkında bilgi içerir. Bir ODBC sürücüsünün bir veri kaynağına bağlanmak için bu DSN'ye ihtiyacı vardır. Bu bölümde, Microsoft Excel, Python veya R gibi istemcilerden Azure Databricks'e bağlanmak için Databricks ODBC sürücüsüyle birlikte kullanılabilecek bir DSN ayarlarsınız.

1. Azure Databricks çalışma alanından Databricks kümesine gidin.

    ![Açık Databricks kümesi](./media/connect-databricks-excel-python-r/open-databricks-cluster.png "Açık Databricks kümesi")

2. **Yapılandırma** sekmesinin **altında, JDBC/ODBC** sekmesini tıklatın ve **Server Hostname** ve **HTTP Path**değerlerini kopyalayın. Bu makaledeki adımları tamamlamak için bu değerlere ihtiyacınız var.

    ![Databricks yapılandırması alın](./media/connect-databricks-excel-python-r/get-databricks-jdbc-configuration.png "Databricks yapılandırması alın")

3. Bilgisayarınızda, uygulamaya bağlı olarak **ODBC Veri Kaynakları** uygulamasını (32 bit veya 64 bit) başlatın. Excel'den bağlanmak için 32 bit sürümünü kullanın. R ve Python'dan bağlanmak için 64 bit sürümünü kullanın.

    ![Fırlatma ODBC](./media/connect-databricks-excel-python-r/launch-odbc-app.png "ODBC uygulamasını başlatın")

4. Kullanıcı **DSN** sekmesinin altında **Ekle'yi**tıklatın. Yeni **Veri Kaynağı Oluştur** iletişim kutusunda **Simba Spark ODBC Sürücüsü'nü**seçin ve ardından **Bitir'i**tıklatın.

    ![Fırlatma ODBC](./media/connect-databricks-excel-python-r/add-new-user-dsn.png "ODBC uygulamasını başlatın")

5. **Simba Spark ODBC Sürücüsü** iletişim kutusunda aşağıdaki değerleri sağlayın:

    ![DSN'yi yapılandırma](./media/connect-databricks-excel-python-r/odbc-dsn-setup.png "DSN'yi yapılandırma")

    Aşağıdaki tablo, iletişim kutusunda sağlayabilecek değerler hakkında bilgi sağlar.
    
    |Alan  | Değer  |
    |---------|---------|
    |**Data Source Name**     | Veri kaynağı için bir ad sağlayın.        |
    |**Ana bilgisayar(lar)**     | *Sunucu ana bilgisayar adı*için Databricks çalışma alanından kopyaladığınız değeri sağlayın.        |
    |**Bağlantı noktası**     | *443'e*girin.        |
    |**Kimlik Doğrulama****Mekanizması**  >      | *Kullanıcı adı ve parolayı*seçin.        |
    |**Kullanıcı adı**     | *Belirteci*girin.        |
    |**Parola**     | Databricks çalışma alanından kopyaladığınız belirteç değerini girin. |
    
    DSN kurulum iletişim kutusunda aşağıdaki ek adımları gerçekleştirin.
    
    * **HTTP Seçenekleri'ni**tıklatın. Açılan iletişim kutusuna, Databricks çalışma alanından kopyaladığınız *HTTP Yolu* değerini yapıştırın. **Tamam**'a tıklayın.
    * **SSL Seçenekleri'ni**tıklatın. Açılan iletişim **kutusunda, SSL'yi etkinleştir** onay kutusunu seçin. **Tamam**'a tıklayın.
    * Azure Veri Tuğlaları bağlantısını test etmek için **Test'i** tıklatın. Yapılandırmayı kaydetmek için **Tamam'ı** tıklatın.
    * **ODBC Veri Kaynağı Yöneticisi** iletişim kutusunda **Tamam'ı**tıklatın.

Artık DSN'nizi ayarladınız. Sonraki bölümlerde, Excel, Python veya R'den Azure Veri Tuğlalarına bağlanmak için bu DSN'yi kullanırsınız.

## <a name="connect-from-microsoft-excel"></a>Microsoft Excel'den bağlan

Bu bölümde, daha önce oluşturduğunuz DSN'yi kullanarak Azure Databricks'ten Microsoft Excel'e veri çekersiniz. Başlamadan önce bilgisayarınızda Microsoft Excel yüklü olduğundan emin olun. [Microsoft Excel deneme bağlantısından](https://products.office.com/excel)Excel'in deneme sürümünü kullanabilirsiniz.

1. Microsoft Excel'de boş bir çalışma kitabı açın. **Veri** şeridinden **Veri Al'ı**tıklatın. **Diğer Kaynaklardan'ı** tıklatın ve sonra **ODBC'den'i**tıklatın.

    ![Excel'den ODBC başlat](./media/connect-databricks-excel-python-r/launch-odbc-from-excel.png "Excel'den ODBC başlat")

2. **ODBC** iletişim kutusunda, daha önce oluşturduğunuz DSN'yi seçin ve ardından **Tamam'ı**tıklatın.

    ![DSN'yi seçin](./media/connect-databricks-excel-python-r/excel-select-dsn.png "DSN'yi seçin")

3. Kimlik bilgileri için istenirse, kullanıcı adı için **belirteç**girin. Parola için, Databricks çalışma alanından aldığınız belirteç değerini sağlayın.

    ![Databricks için kimlik bilgileri sağlama](./media/connect-databricks-excel-python-r/excel-databricks-token.png "DSN'yi seçin")

4. Gezgin penceresinden, Excel'e yüklemek istediğiniz Databricks tablosunu seçin ve sonra **Yükle'yi**tıklatın. 

    ![DTA'yı Excel'e yükleme](./media/connect-databricks-excel-python-r/excel-load-data.png "DTA'yı Excel'e yükleme")

Excel çalışma kitabınızda verileri aldıktan sonra, bu veriler üzerinde analitik işlemler gerçekleştirebilirsiniz.

## <a name="connect-from-r"></a>R'den bağlan

> [!NOTE]
> Bu bölümde, masaüstünüzde çalışan bir R Studio istemcisini Azure Veri Tuğlaları ile nasıl tümleştirileştiriniz hakkında bilgi verilmektedir. Azure Databricks kümesinde R Studio'nun nasıl kullanılacağına ilişkin talimatlar için [Azure Databricks'teki R Studio'ya](/azure/databricks/spark/latest/sparkr/rstudio)bakın.

Bu bölümde, Azure Veri Tuğlaları'nda bulunan verilere başvurmak için Bir R dili IDE'si kullanırsınız. Başlamadan önce, aşağıdakileri bilgisayara yüklemiş olmalısınız.

* R dili için bir IDE. Bu makalede Masaüstü için RStudio kullanır. [R Studio indir](https://www.rstudio.com/products/rstudio/download/)yükleyebilirsiniz.
* IDE olarak Masaüstü için RStudio kullanıyorsanız, microsoft [https://aka.ms/rclient/](https://aka.ms/rclient/)r istemcisi de yükleyin. 

RStudio'yu açın ve aşağıdaki adımları yapın:

- Pakete `RODBC` başvurun. Bu, daha önce oluşturduğunuz DSN'yi kullanarak Azure Veri Tuğlaları'na bağlanmanızı sağlar.
- DSN'yi kullanarak bir bağlantı kurun.
- Azure Databricks'teki verilerüzerinde bir SQL sorgusu çalıştırın. Aşağıdaki snippet'te, *radio_sample_data* Azure Veri Tuğlaları'nda zaten var olan bir tablodur.
- Çıktıyı doğrulamak için sorguda bazı işlemler gerçekleştirin. 

Aşağıdaki kod parçacığı şu görevleri gerçekleştirir:

    # reference the 'RODBC' package
    require(RODBC)
    
    # establish a connection using the DSN you created earlier
    conn <- odbcConnect("<ENTER DSN NAME HERE>")
    
    # run a SQL query using the connection you created
    res <- sqlQuery(conn, "SELECT * FROM radio_sample_data")
    
    # print out the column names in the query output
    names(res) 
        
    # print out the number of rows in the query output
    nrow (res)

## <a name="connect-from-python"></a>Python'dan bağlan

Bu bölümde, Azure Veri Tuğlaları'nda bulunan verilere başvurmak için bir Python IDE (IDLE gibi) kullanırsınız. Başlamadan önce aşağıdaki ön koşulları tamamlayın:

* Python'u [buradan](https://www.python.org/downloads/)yükleyin. Python'u bu bağlantıdan yüklemek de IDLE'yi yükler.

* Bilgisayardaki komut isteminden paketi `pyodbc` yükleyin. Şu komutu çalıştırın:

      pip install pyodbc

IDLE'yi açın ve aşağıdaki adımları yapın:

- Paketi `pyodbc` içeri aktarın. Bu, daha önce oluşturduğunuz DSN'yi kullanarak Azure Veri Tuğlaları'na bağlanmanızı sağlar.
- Daha önce oluşturduğunuz DSN'yi kullanarak bir bağlantı kurun.
-  Oluşturduğunuz bağlantıyı kullanarak bir SQL sorgusu çalıştırın. Aşağıdaki snippet'te, *radio_sample_data* Azure Veri Tuğlaları'nda zaten var olan bir tablodur.
- Çıktıyı doğrulamak için sorguda işlemleri gerçekleştirin.

Aşağıdaki kod parçacığı şu görevleri gerçekleştirir:

```python
# import the `pyodbc` package:
import pyodbc

# establish a connection using the DSN you created earlier
conn = pyodbc.connect("DSN=<ENTER DSN NAME HERE>", autocommit=True)

# run a SQL query using the connection you created
cursor = conn.cursor()
cursor.execute("SELECT * FROM radio_sample_data")

# print the rows retrieved by the query.
for row in cursor.fetchall():
    print(row)
```

## <a name="next-steps"></a>Sonraki adımlar

* Verileri Azure Databricks'e aktarabileceğiniz kaynaklar hakkında bilgi edinmek için Azure [Veri Tuğlaları için Veri kaynaklarına](/azure/databricks/data/data-sources/index) bakın


