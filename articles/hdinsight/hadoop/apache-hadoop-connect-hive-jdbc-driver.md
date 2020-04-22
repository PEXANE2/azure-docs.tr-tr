---
title: JDBC sürücüsü nden Apache Hive sorgula - Azure HDInsight
description: Apache Hive sorgularını HDInsight'ta Hadoop'a göndermek için Java uygulamasındaki JDBC sürücüsünü kullanın. Programlı ve SQuirrel SQL istemcisinden bağlanın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 04/20/2020
ms.openlocfilehash: 803256ab1c5201534cfbd8210f96040ba75081e5
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687288"
---
# <a name="query-apache-hive-through-the-jdbc-driver-in-hdinsight"></a>HDInsight’ta JDBC sürücüsü üzerinden Apache Hive’ı sorgulama

[!INCLUDE [ODBC-JDBC-selector](../../../includes/hdinsight-selector-odbc-jdbc.md)]

Java uygulamasından JDBC sürücüsünü nasıl kullanacağınızı öğrenin. Azure HDInsight'ta Apache Hive sorgularını Apache Hadoop'a göndermek için. Bu belgedeki bilgiler, programlı olarak ve SQuirreL SQL istemcisinden nasıl bağlanılabildiğini gösterir.

Hive JDBC Arabirimi hakkında daha fazla bilgi için [HiveJDBCInterface'e](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Bir HDInsight Hadoop kümesi. Bir tane oluşturmak için Azure [HDInsight ile başlayın'](apache-hadoop-linux-tutorial-get-started.md)a bakın. HiveServer2 hizmetinin çalıştığını sağlayın.
* [Java Geliştirici Kiti (JDK) sürüm 11](https://www.oracle.com/technetwork/java/javase/downloads/jdk11-downloads-5066655.html) veya daha yüksek.
* [SQuirreL SQL](http://squirrel-sql.sourceforge.net/). SQuirreL bir JDBC istemci uygulamasıdır.

## <a name="jdbc-connection-string"></a>JDBC bağlantı dizesi

Azure'daki bir HDInsight kümesine jdbc bağlantıları 443 bağlantı noktası üzerinden yapılır. Trafik TLS/SSL kullanılarak güvenlidir. Kümelerin arkasında yer alan ortak ağ geçidi, trafiği HiveServer2'nin gerçekten dinlediği bağlantı noktasına yönlendirir. Aşağıdaki bağlantı dizesi HDInsight için kullanılacak biçimi gösterir:

    jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2

`CLUSTERNAME` değerini HDInsight kümenizin adıyla değiştirin.

Ya da **Ambari UI > Hive > Configs > Advanced**ile bağlantı alabilirsiniz.

![Ambari üzerinden JDBC bağlantı dizealın](./media/apache-hadoop-connect-hive-jdbc-driver/hdinsight-get-connection-string-through-ambari.png)

### <a name="host-name-in-connection-string"></a>Bağlantı dizesinde ana bilgisayar adı

Bağlantı dizesinde 'CLUSTERNAME.azurehdinsight.net' ana bilgisayar adı küme URL'nizle aynıdır. Azure portalından edinebilirsiniz.

### <a name="port-in-connection-string"></a>Bağlantı dizesinde bağlantı noktası

Kümeye yalnızca Azure sanal ağının dışındaki bazı yerlerden bağlanmak için **port 443'ü** kullanabilirsiniz. HDInsight, kümeye tüm bağlantıların güvenli bir ağ geçidi üzerinden yönetildiği anlamına gelen yönetilen bir hizmettir. HiveServer 2'ye doğrudan 10001 veya 10000 bağlantı noktalarında bağlanamazsınız. Bu bağlantı noktaları dışarıya maruz değil.

## <a name="authentication"></a>Kimlik Doğrulaması

Bağlantıyı kurarken, kimlik doğrulaması yapmak için HDInsight küme yöneticisi adını ve parolasını kullanın. SQuirreL SQL gibi JDBC istemcilerinden, istemci ayarlarına yönetici adı ve parola girin.

Bir Java uygulamasından, bağlantı kurarken adı ve parolayı kullanmanız gerekir. Örneğin, aşağıdaki Java kodu yeni bir bağlantı açar:

```java
DriverManager.getConnection(connectionString,clusterAdmin,clusterPassword);
```

## <a name="connect-with-squirrel-sql-client"></a>SQuirreL SQL istemcisi ile bağlanın

SQuirreL SQL, HDInsight kümenizle Kovan sorgularını uzaktan çalıştırmak için kullanılabilecek bir JDBC istemcisidir. Aşağıdaki adımlar, SQuirreL SQL'i yüklediğinizi varsayar.

1. Kümenizden kopyalanacak belirli dosyaları içerecek bir dizin oluşturun.

2. Aşağıdaki komut dosyasında, küme için SSH kullanıcı hesabı adı ile değiştirin. `sshuser`  HDInsight küme adı ile değiştirin. `CLUSTERNAME`  Komut satırından, çalışma dizininizi önceki adımda oluşturulan adedine değiştirin ve ardından bir HDInsight kümesinden dosyaları kopyalamak için aşağıdaki komutu girin:

    ```cmd
    scp sshuser@CLUSTERNAME-ssh.azurehdinsight.net:/usr/hdp/current/hadoop-client/{hadoop-auth.jar,hadoop-common.jar,lib/log4j-*.jar,lib/slf4j-*.jar,lib/curator-*.jar} .

    scp sshuser@CLUSTERNAME-ssh.azurehdinsight.net:/usr/hdp/current/hive-client/lib/{commons-codec*.jar,commons-logging-*.jar,hive-*-*.jar,httpclient-*.jar,httpcore-*.jar,libfb*.jar,libthrift-*.jar} .
    ```

3. SQuirreL SQL uygulamasını başlatın. Pencerenin solundan **Sürücüler'i**seçin.

    ![Pencerenin solundaki sürücüler sekmesi](./media/apache-hadoop-connect-hive-jdbc-driver/hdinsight-squirreldrivers.png)

4. **Sürücüler** iletişim kutusunun üst kısmındaki simgelerden, **+** sürücü oluşturmak için simgeyi seçin.

    ![SQuirreL SQL uygulama sürücüleri simgesi](./media/apache-hadoop-connect-hive-jdbc-driver/hdinsight-driversicons.png)

5. Sürücü Ekle iletişim kutusunda aşağıdaki bilgileri ekleyin:

    |Özellik | Değer |
    |---|---|
    |Adı|Hive|
    |Örnek URL|`jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`|
    |Ekstra Sınıf Yolu|Daha önce indirilen tüm kavanoz dosyalarını eklemek için **Ekle** düğmesini kullanın.|
    |Sınıf Adı|org.apache.hive.jdbc.HiveDriver|

   ![parametrelerle sürücü iletişim kutusu ekleme](./media/apache-hadoop-connect-hive-jdbc-driver/hdinsight-add-driver.png)

   Bu ayarları kaydetmek için **Tamam'ı** seçin.

6. SQuirreL SQL penceresinin solunda **Diğer Adlar'ı**seçin. Ardından bağlantı **+** takma adı oluşturmak için simgeyi seçin.

    !['SQuirreL SQL yeni takma ad iletişim kutusu ekleyin'](./media/apache-hadoop-connect-hive-jdbc-driver/hdinsight-new-aliases.png)

7. **Diğer Ad Ekle** iletişim kutusu için aşağıdaki değerleri kullanın:

    |Özellik |Değer |
    |---|---|
    |Adı|HDInsight'ta Kovan|
    |Sürücü|**Kovan** sürücüsünü seçmek için açılır sürücüyü kullanın.|
    |URL'si|`jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2`. **CLUSTERNAME** değerini HDInsight kümenizin adıyla değiştirin.|
    |User Name|HDInsight kümenizin küme giriş hesabı adı. Varsayılan **yöneticidir.**|
    |Parola|Küme giriş hesabının şifresi.|

    ![parametrelerle diğer ad iletişim kutusu ekleme](./media/apache-hadoop-connect-hive-jdbc-driver/hdinsight-addalias-dialog.png)

    > [!IMPORTANT]
    > Bağlantının çalıştığını doğrulamak için **Test** düğmesini kullanın. **Bağlan: HDInsight** iletişim kutusunda hive göründüğünde, testi gerçekleştirmek için **Bağlan'ı** seçin. Test başarılı olursa, Bağlantı **başarılı** iletişim kutusu görürsünüz. Bir hata oluşursa, [sorun giderme](#troubleshooting)konusuna bakın.

    Bağlantı takma adını kaydetmek **için, Diğer Ad Ekle** iletişim kutusunun altındaki **Tamam** düğmesini kullanın.

8. **Connect'ten** SQuirreL SQL'in üst kısmındaki açılır yere **kadar HDInsight'ta Hive'ı**seçin. İstendiğinde **Bağlan'ı**seçin.

    ![parametrelerle bağlantı iletişim kutusu](./media/apache-hadoop-connect-hive-jdbc-driver/hdinsight-connect-dialog.png)

9. Bağlandıktan sonra, AŞAĞıDAKI sorguyu SQL sorgusuna girin ve ardından **Çalıştır** simgesini (çalışan kişi) seçin. Sonuç alanı sorgu sonuçlarını göstermelidir.

    ```hql
    select * from hivesampletable limit 10;
    ```

    ![sonuçlar da dahil olmak üzere sql sorgu iletişim kutusu](./media/apache-hadoop-connect-hive-jdbc-driver/hdinsight-sqlquery-dialog.png)

## <a name="connect-from-an-example-java-application"></a>Örnek bir Java uygulamasından bağlanın

HDInsight'ta Hive sorgusu yapmak için Java istemcisi kullanmanın bir örneği . [https://github.com/Azure-Samples/hdinsight-java-hive-jdbc](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc) Örneği oluşturmak ve çalıştırmak için depodaki yönergeleri izleyin.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="unexpected-error-occurred-attempting-to-open-an-sql-connection"></a>SQL bağlantısını açmaya çalışırken beklenmeyen hata oluştu

**Belirtiler**: Sürüm 3.3 veya daha büyük bir HDInsight kümesine bağlanırken, beklenmeyen bir hata oluştuğuna dair bir hata alabilirsiniz. Bu hata için yığın izleme aşağıdaki satırları ile başlar:

```java
java.util.concurrent.ExecutionException: java.lang.RuntimeException: java.lang.NoSuchMethodError: org.apache.commons.codec.binary.Base64.<init>(I)V
at java.util.concurrent.FutureTas...(FutureTask.java:122)
at java.util.concurrent.FutureTask.get(FutureTask.java:206)
```

**Neden**: Bu hata, SQuirreL ile birlikte verilen eski bir sürüm commons-codec.jar dosyasından kaynaklanır.

**Çözüm**: Bu hatayı düzeltmek için aşağıdaki adımları kullanın:

1. SQuirreL'den çıkın ve squirreL'in sisteminizde yüklü olduğu dizine gidin, belki `C:\Program Files\squirrel-sql-4.0.0\lib`. SquirreL dizininde, `lib` dizin altında, varolan commons-codec.jar'ı HDInsight kümesinden indirilen le değiştirin.

1. SQuirreL'i yeniden başlatın. HDInsight'ta Hive'a bağlanırken hata artık oluşmamalıdır.

### <a name="connection-disconnected-by-hdinsight"></a>HDInsight ile bağlantı kesildi

**Semptomlar**: JDBC/ODBC üzerinden büyük miktarda veri (birkaç GB) indirmeye çalışırken, indirme sırasında hdInsight ile bağlantı beklenmedik bir şekilde kesilir.

**Neden**: Bu hata Ağ Geçidi düğümleri üzerindeki sınırlama neden olur. JDBC/ODBC'den veri alırken, tüm verilerin Ağ Geçidi düğümünden geçmesi gerekir. Ancak, bir ağ geçidi büyük miktarda veri indirmek için tasarlanamadığından, ağ geçidi trafiği kaldıramıyorsa bağlantıyı kapatabilir.

**Çözünürlük**: Büyük miktarda veri indirmek için JDBC/ODBC sürücüsünü kullanmaktan kaçının. Bunun yerine verileri doğrudan blob depolamadan kopyalayın.

## <a name="next-steps"></a>Sonraki adımlar

Hive ile çalışmak için JDBC'yi nasıl kullanacağınızı öğrendiğiniz için, Azure HDInsight ile çalışmanın diğer yollarını keşfetmek için aşağıdaki bağlantıları kullanın.

* [Azure HDInsight'ta Microsoft Power BI ile Apache Hive verilerini görselleştirin.](apache-hadoop-connect-hive-power-bi.md)
* [Azure HDInsight'ta Power BI ile Etkileşimli Sorgu Kovanı verilerini görselleştirin.](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md)
* [Microsoft Hive ODBC Driver ile Excel'i HDInsight'a bağlayın.](apache-hadoop-connect-excel-hive-odbc-driver.md)
* [Güç Sorgusu'yu kullanarak Excel'i Apache Hadoop'a bağlayın.](apache-hadoop-connect-excel-power-query.md)
* [HDInsight ile Apache Hive'ı kullanma](hdinsight-use-hive.md)
* [HDInsight ile Apache Pig'i kullanma](../use-pig.md)
* [HDInsight ile MapReduce işleri kullanma](hdinsight-use-mapreduce.md)
