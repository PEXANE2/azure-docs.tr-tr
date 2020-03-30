---
title: Azure HDInsight için bir Java HBase istemcisi oluşturmak için Apache Maven'i kullanın
description: Java tabanlı Bir Apache HBase uygulaması oluşturmak için Apache Maven'i nasıl kullanacağınızı ve ardından Azure HDInsight'ta HBase'e nasıl dağıtacağını öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,seodec18
ms.date: 12/24/2019
ms.openlocfilehash: 3e9b23ce450e45dfedcee8b20e09b1c2b52b6e68
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75495791"
---
# <a name="build-java-applications-for-apache-hbase"></a>Apache HBase için Java uygulamaları oluşturun

Java'da nasıl bir [Apache HBase](https://hbase.apache.org/) uygulaması oluşturacağınızı öğrenin. Ardından Azure HDInsight'ta HBase ile uygulamayı kullanın.

Bu belgedeki adımlar, projeyi oluşturmak ve oluşturmak için [Apache Maven'i](https://maven.apache.org/) kullanır. Maven, Java projeleri için yazılım, dokümantasyon ve raporlar oluşturmanıza olanak tanıyan bir yazılım proje yönetimi ve anlama aracıdır.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight'ta bir Apache HBase kümesi. Bkz. [Apache HBase ile başlayın.](./apache-hbase-tutorial-get-started-linux.md)

* [Java Geliştirici Kiti (JDK) sürüm 8](https://aka.ms/azure-jdks).

* [Apache Maven](https://maven.apache.org/download.cgi) düzgün Apache göre [yüklü.](https://maven.apache.org/install.html)  Maven Java projeleri için bir proje inşa sistemidir.

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](../hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

* PowerShell kullanıyorsanız, [AZ Modülü](https://docs.microsoft.com/powershell/azure/overview)gerekir.

* Bir metin düzenleyici. Bu makalede Microsoft Not Defteri kullanır.

## <a name="test-environment"></a>Test ortamı

Bu makale için kullanılan ortam, Windows 10 çalıştıran bir bilgisayardı.  Komutlar bir komut istemiyle yürütüldü ve çeşitli dosyalar Not Defteri ile düzenlendi. Ortamınız için buna göre değiştirin.

Komut isteminden, çalışma ortamı oluşturmak için aşağıdaki komutları girin:

```cmd
IF NOT EXIST C:\HDI MKDIR C:\HDI
cd C:\HDI
```

## <a name="create-a-maven-project"></a>Bir Maven projesi oluşturma

1. **Hbaseapp**adlı bir Maven projesi oluşturmak için aşağıdaki komutu girin:

    ```cmd
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    cd hbaseapp
    mkdir conf
    ```

    Bu komut, temel bir `hbaseapp` Maven projesi içeren geçerli konumda adlı bir dizin oluşturur. İkinci komut çalışma dizini `hbaseapp`'ni ' olarak değiştirir. Üçüncü komut, `conf`daha sonra kullanılacak yeni bir dizin oluşturur. Dizin `hbaseapp` aşağıdaki öğeleri içerir:

    * `pom.xml`: Proje Nesnesi Modeli[(POM),](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html)projeyi oluşturmak için kullanılan bilgi ve yapılandırma ayrıntılarını içerir.
    * `src\main\java\com\microsoft\examples`: Uygulama kodunuzu içerir.
    * `src\test\java\com\microsoft\examples`: Uygulamanız için testler içerir.

2. Oluşturulan örnek kodu kaldırın. Oluşturulan test ve uygulama `AppTest.java`dosyalarını silin ve `App.java` aşağıdaki komutları girerek:

    ```cmd
    DEL src\main\java\com\microsoft\examples\App.java
    DEL src\test\java\com\microsoft\examples\AppTest.java
    ```

## <a name="update-the-project-object-model"></a>Proje Nesnesi Modelini Güncelleştirme

pom.xml dosyasının tam referansı https://maven.apache.org/pom.htmliçin bkz.  Aşağıdaki `pom.xml` komutu girerek açın:

```cmd
notepad pom.xml
```

### <a name="add-dependencies"></a>Bağımlılıkekleme

In `pom.xml`, `<dependencies>` bölüme aşağıdaki metni ekleyin:

```xml
<dependency>
    <groupId>org.apache.hbase</groupId>
    <artifactId>hbase-shaded-client</artifactId>
    <version>1.1.2</version>
</dependency>
<dependency>
    <groupId>org.apache.phoenix</groupId>
    <artifactId>phoenix-core</artifactId>
    <version>4.14.1-HBase-1.1</version>
</dependency>
```  

Bu bölümde, projenin **hbase-istemci** ve **anka çekirdeği** bileşenlerine ihtiyacı olduğunu gösterir. Derleme zamanında, bu bağımlılıklar varsayılan Maven deposundan indirilir. Bu bağımlılık hakkında daha fazla bilgi edinmek için [Maven Merkezi Depo Aramasını](https://search.maven.org/artifact/org.apache.hbase/hbase-client/1.1.2/jar) kullanabilirsiniz.

> [!IMPORTANT]  
> Hbase istemcisinin sürüm numarası, HDInsight kümenizle birlikte sağlanan Apache HBase sürümüyle eşleşmelidir. Doğru sürüm numarasını bulmak için aşağıdaki tabloyu kullanın.

| HDInsight küme sürümü | Kullanılacak Apache HBase sürümü |
| --- | --- |
| 3.6 | 1.1.2 |
| 4.0 | 2.0.0 |

HDInsight sürümleri ve bileşenleri hakkında daha fazla bilgi için, [HDInsight ile kullanılabilen farklı Apache Hadoop bileşenleri nelerdir](../hdinsight-component-versioning.md)bakın.

### <a name="build-configuration"></a>Yapı yapılandırması

Maven eklentileri, projenin yapı aşamalarını özelleştirmenize olanak sağlar. Bu bölüm eklentileri, kaynakları ve diğer yapı yapılandırma seçenekleri eklemek için kullanılır.

Aşağıdaki kodu dosyaya `pom.xml` ekleyin ve ardından dosyayı kaydedip kapatın. Bu metin, dosyadaki etiketlerin `<project>...</project>` içinde olmalıdır, `</dependencies>` `</project>`örneğin, arasında ve .

```xml
<build>
    <sourceDirectory>src</sourceDirectory>
    <resources>
    <resource>
        <directory>${basedir}/conf</directory>
        <filtering>false</filtering>
        <includes>
        <include>hbase-site.xml</include>
        </includes>
    </resource>
    </resources>
    <plugins>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
        <configuration>
            <source>1.8</source>
            <target>1.8</target>
        </configuration>
        </plugin>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
        <version>3.2.1</version>
        <configuration>
        <transformers>
            <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
            </transformer>
        </transformers>
        </configuration>
        <executions>
        <execution>
            <phase>package</phase>
            <goals>
            <goal>shade</goal>
            </goals>
        </execution>
        </executions>
    </plugin>
    </plugins>
</build>
```

Bu bölüm, HBase`conf/hbase-site.xml`için yapılandırma bilgilerini içeren bir kaynağı yapılandırır.

> [!NOTE]  
> Yapılandırma değerlerini kod üzerinden de ayarlayabilirsiniz. Örnekteki yorumlara `CreateTable` bakın.

Bu bölümde ayrıca [Apache Maven Derleyici Eklentisi](https://maven.apache.org/plugins/maven-compiler-plugin/) ve [Apache Maven Gölge Eklentisi](https://maven.apache.org/plugins/maven-shade-plugin/)yapılandırır. Derleyici eklentisi topolojiderlemek için kullanılır. Gölge eklentisi, Maven tarafından üretilen JAR paketinde lisans yinelemesini önlemek için kullanılır. Bu eklenti, HDInsight kümesinde çalışma zamanında bir "yinelenen lisans dosyaları" hatasını önlemek için kullanılır. Uygulama ile maven-gölge eklentisi `ApacheLicenseResourceTransformer` kullanarak hata önler.

Maven-shade-plugin de uygulama tarafından gerekli tüm bağımlılıkları içeren bir uber kavanoz üretir.

### <a name="download-the-hbase-sitexml"></a>Hbase-site.xml indirin

HBase kümesinden `conf` dizine HBase yapılandırmasını kopyalamak için aşağıdaki komutu kullanın. HDInsight küme adınız ile değiştirin `CLUSTERNAME` ve ardından komutu girin:

```cmd
scp sshuser@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml
```

## <a name="create-the-application"></a>Uygulama oluşturma

### <a name="implement-a-createtable-class"></a>CreateTable sınıfı uygulama

Yeni bir dosya oluşturmak ve açmak `CreateTable.java`için aşağıdaki komutu girin. Yeni bir dosya oluşturmak için istek te **Evet'i** seçin.

```cmd
notepad src\main\java\com\microsoft\examples\CreateTable.java
```

Ardından aşağıdaki java kodunu kopyalayıp yeni dosyaya yapıştırın. O zaman dosyayı kapat.

```java
package com.microsoft.examples;
import java.io.IOException;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.hbase.HBaseConfiguration;
import org.apache.hadoop.hbase.client.HBaseAdmin;
import org.apache.hadoop.hbase.HTableDescriptor;
import org.apache.hadoop.hbase.TableName;
import org.apache.hadoop.hbase.HColumnDescriptor;
import org.apache.hadoop.hbase.client.HTable;
import org.apache.hadoop.hbase.client.Put;
import org.apache.hadoop.hbase.util.Bytes;

public class CreateTable {
    public static void main(String[] args) throws IOException {
    Configuration config = HBaseConfiguration.create();

    // Example of setting zookeeper values for HDInsight
    // in code instead of an hbase-site.xml file
    //
    // config.set("hbase.zookeeper.quorum",
    //            "zookeepernode0,zookeepernode1,zookeepernode2");
    //config.set("hbase.zookeeper.property.clientPort", "2181");
    //config.set("hbase.cluster.distributed", "true");
    //
    //NOTE: Actual zookeeper host names can be found using Ambari:
    //curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts"

    //Linux-based HDInsight clusters use /hbase-unsecure as the znode parent
    config.set("zookeeper.znode.parent","/hbase-unsecure");

    // create an admin object using the config
    HBaseAdmin admin = new HBaseAdmin(config);

    // create the table...
    HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
    // ... with two column families
    tableDescriptor.addFamily(new HColumnDescriptor("name"));
    tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
    admin.createTable(tableDescriptor);

    // define some people
    String[][] people = {
        { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
        { "2", "Franklin", "Holtz", "franklin@contoso.com" },
        { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
        { "4", "Rae", "Schroeder", "rae@contoso.com" },
        { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
        { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

    HTable table = new HTable(config, "people");

    // Add each person to the table
    //   Use the `name` column family for the name
    //   Use the `contactinfo` column family for the email
    for (int i = 0; i< people.length; i++) {
        Put person = new Put(Bytes.toBytes(people[i][0]));
        person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
        person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
        person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
        table.put(person);
    }
    // flush commits and close the table
    table.flushCommits();
    table.close();
    }
}
```

Bu kod, `CreateTable` adında `people` bir tablo oluşturan ve bazı önceden tanımlanmış kullanıcılarla dolduran sınıftır.

### <a name="implement-a-searchbyemail-class"></a>SearchByEmail sınıfı uygulayın

Yeni bir dosya oluşturmak ve açmak `SearchByEmail.java`için aşağıdaki komutu girin. Yeni bir dosya oluşturmak için istek te **Evet'i** seçin.

```cmd
notepad src\main\java\com\microsoft\examples\SearchByEmail.java
```

Ardından aşağıdaki java kodunu kopyalayıp yeni dosyaya yapıştırın. O zaman dosyayı kapat.

```java
package com.microsoft.examples;
import java.io.IOException;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.hbase.HBaseConfiguration;
import org.apache.hadoop.hbase.client.HTable;
import org.apache.hadoop.hbase.client.Scan;
import org.apache.hadoop.hbase.client.ResultScanner;
import org.apache.hadoop.hbase.client.Result;
import org.apache.hadoop.hbase.filter.RegexStringComparator;
import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
import org.apache.hadoop.hbase.util.Bytes;
import org.apache.hadoop.util.GenericOptionsParser;

public class SearchByEmail {
    public static void main(String[] args) throws IOException {
    Configuration config = HBaseConfiguration.create();

    // Use GenericOptionsParser to get only the parameters to the class
    // and not all the parameters passed (when using WebHCat for example)
    String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
    if (otherArgs.length != 1) {
        System.out.println("usage: [regular expression]");
        System.exit(-1);
    }

    // Open the table
    HTable table = new HTable(config, "people");

    // Define the family and qualifiers to be used
    byte[] contactFamily = Bytes.toBytes("contactinfo");
    byte[] emailQualifier = Bytes.toBytes("email");
    byte[] nameFamily = Bytes.toBytes("name");
    byte[] firstNameQualifier = Bytes.toBytes("first");
    byte[] lastNameQualifier = Bytes.toBytes("last");

    // Create a regex filter
    RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
    // Attach the regex filter to a filter
    //   for the email column
    SingleColumnValueFilter filter = new SingleColumnValueFilter(
        contactFamily,
        emailQualifier,
        CompareOp.EQUAL,
        emailFilter
    );

    // Create a scan and set the filter
    Scan scan = new Scan();
    scan.setFilter(filter);

    // Get the results
    ResultScanner results = table.getScanner(scan);
    // Iterate over results and print  values
    for (Result result : results ) {
        String id = new String(result.getRow());
        byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
        String firstName = new String(firstNameObj);
        byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
        String lastName = new String(lastNameObj);
        System.out.println(firstName + " " + lastName + " - ID: " + id);
        byte[] emailObj = result.getValue(contactFamily, emailQualifier);
        String email = new String(emailObj);
        System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
    }
    results.close();
    table.close();
    }
}
```

Sınıf, `SearchByEmail` satırlar için e-posta adresine göre sorgu yapmak için kullanılabilir. Normal bir ifade filtresi kullandığından, sınıfı kullanırken bir dize veya normal bir ifade sağlayabilirsiniz.

### <a name="implement-a-deletetable-class"></a>DeleteTable sınıfı uygulama

Yeni bir dosya oluşturmak ve açmak `DeleteTable.java`için aşağıdaki komutu girin. Yeni bir dosya oluşturmak için istek te **Evet'i** seçin.

```cmd
notepad src\main\java\com\microsoft\examples\DeleteTable.java
```

Ardından aşağıdaki java kodunu kopyalayıp yeni dosyaya yapıştırın. O zaman dosyayı kapat.

```java
package com.microsoft.examples;
import java.io.IOException;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.hbase.HBaseConfiguration;
import org.apache.hadoop.hbase.client.HBaseAdmin;

public class DeleteTable {
    public static void main(String[] args) throws IOException {
    Configuration config = HBaseConfiguration.create();

    // Create an admin object using the config
    HBaseAdmin admin = new HBaseAdmin(config);

    // Disable, and then delete the table
    admin.disableTable("people");
    admin.deleteTable("people");
    }
}
```

Sınıf, `DeleteTable` bu örnekte oluşturulan HBase tablolarını `CreateTable` sınıf tarafından oluşturulan tabloyu devre dışı bırakarak temizler.

## <a name="build-and-package-the-application"></a>Uygulamayı oluşturma ve paketleme

1. `hbaseapp` Dizinden, uygulamayı içeren bir JAR dosyası oluşturmak için aşağıdaki komutu kullanın:

    ```cmd
    mvn clean package
    ```

    Bu komut, uygulamayı bir .jar dosyasına oluşturur ve paketler.

2. Komut tamamlandığında, `hbaseapp/target` dizin . `hbaseapp-1.0-SNAPSHOT.jar`

   > [!NOTE]  
   > Dosya `hbaseapp-1.0-SNAPSHOT.jar` bir uber kavanozu. Uygulamayı çalıştırmak için gereken tüm bağımlılıkları içerir.

## <a name="upload-the-jar-and-run-jobs-ssh"></a>JAR'ı yükleyin ve işleri çalıştırın (SSH)

Aşağıdaki adımlar, `scp` JAR'ı HDInsight kümesindeki Apache HBase'inizin birincil baş düğümüne kopyalamak için kullanılır. Komut `ssh` daha sonra kümeye bağlanmak ve örneği doğrudan baş düğümünde çalıştırmak için kullanılır.

1. Kavanozu kümeye yükleyin. HDInsight küme adınız ile değiştirin `CLUSTERNAME` ve ardından aşağıdaki komutu girin:

    ```cmd
    scp ./target/hbaseapp-1.0-SNAPSHOT.jar sshuser@CLUSTERNAME-ssh.azurehdinsight.net:hbaseapp-1.0-SNAPSHOT.jar
    ```

2. HBase kümesine bağlanın. HDInsight küme adınız ile değiştirin `CLUSTERNAME` ve ardından aşağıdaki komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

3. Java uygulamasını kullanarak bir HBase tablosu oluşturmak için açık ssh bağlantınızda aşağıdaki komutu kullanın:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable
    ```

    Bu **komut, kişiler**adlı bir HBase tablosu oluşturur ve verileriyle doldurur.

4. Tabloda depolanan e-posta adreslerini aramak için aşağıdaki komutu kullanın:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com
    ```

    Aşağıdaki sonuçları alırsınız:

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

5. Tabloyu silmek için aşağıdaki komutu kullanın:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable
    ```

## <a name="upload-the-jar-and-run-jobs-powershell"></a>JAR'ı yükleyin ve işleri çalıştırın (PowerShell)

Aşağıdaki adımlar, Jar'ı Apache HBase kümeniz için varsayılan depolama alanına yüklemek için Azure PowerShell [AZ modüllerini](https://docs.microsoft.com/powershell/azure/new-azureps-module-az) kullanır. HDInsight cmdlets sonra uzaktan örnekleri çalıştırmak için kullanılır.

1. AZ modül'üne yüklendikten ve yapılandırıldıktan sonra `hbase-runner.psm1`. Bu dosyanın içeriği olarak aşağıdaki metni kullanın:

   ```powershell
    <#
    .SYNOPSIS
    Copies a file to the primary storage of an HDInsight cluster.
    .DESCRIPTION
    Copies a file from a local directory to the blob container for
    the HDInsight cluster.
    .EXAMPLE
    Start-HBaseExample -className "com.microsoft.examples.CreateTable"
    -clusterName "MyHDInsightCluster"

    .EXAMPLE
    Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
    -clusterName "MyHDInsightCluster"
    -emailRegex "contoso.com"

    .EXAMPLE
    Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
    -clusterName "MyHDInsightCluster"
    -emailRegex "^r" -showErr
    #>

    function Start-HBaseExample {
    [CmdletBinding(SupportsShouldProcess = $true)]
    param(
    #The class to run
    [Parameter(Mandatory = $true)]
    [String]$className,

    #The name of the HDInsight cluster
    [Parameter(Mandatory = $true)]
    [String]$clusterName,

    #Only used when using SearchByEmail
    [Parameter(Mandatory = $false)]
    [String]$emailRegex,

    #Use if you want to see stderr output
    [Parameter(Mandatory = $false)]
    [Switch]$showErr
    )

    Set-StrictMode -Version 3

    # Is the Azure module installed?
    FindAzure

    # Get the login for the HDInsight cluster
    $creds=Get-Credential -Message "Enter the login for the cluster" -UserName "admin"

    # The JAR
    $jarFile = "wasb:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"

    # The job definition
    $jobDefinition = New-AzHDInsightMapReduceJobDefinition `
        -JarFile $jarFile `
        -ClassName $className `
        -Arguments $emailRegex

    # Get the job output
    $job = Start-AzHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzHDInsightJob `
        -ClusterName $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds
    if($showErr)
    {
    Write-Host "STDERR"
    Get-AzHDInsightJobOutput `
                -Clustername $clusterName `
                -JobId $job.JobId `
                -HttpCredential $creds `
                -DisplayOutputType StandardError
    }
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzHDInsightJobOutput `
                -Clustername $clusterName `
                -JobId $job.JobId `
                -HttpCredential $creds
    }

    <#
    .SYNOPSIS
    Copies a file to the primary storage of an HDInsight cluster.
    .DESCRIPTION
    Copies a file from a local directory to the blob container for
    the HDInsight cluster.
    .EXAMPLE
    Add-HDInsightFile -localPath "C:\temp\data.txt"
    -destinationPath "example/data/data.txt"
    -ClusterName "MyHDInsightCluster"
    .EXAMPLE
    Add-HDInsightFile -localPath "C:\temp\data.txt"
    -destinationPath "example/data/data.txt"
    -ClusterName "MyHDInsightCluster"
    -Container "MyContainer"
    #>

    function Add-HDInsightFile {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
            #The path to the local file.
            [Parameter(Mandatory = $true)]
            [String]$localPath,

            #The destination path and file name, relative to the root of the container.
            [Parameter(Mandatory = $true)]
            [String]$destinationPath,

            #The name of the HDInsight cluster
            [Parameter(Mandatory = $true)]
            [String]$clusterName,

            #If specified, overwrites existing files without prompting
            [Parameter(Mandatory = $false)]
            [Switch]$force
        )

        Set-StrictMode -Version 3

        # Is the Azure module installed?
        FindAzure

        # Get authentication for the cluster
        $creds=Get-Credential

        # Does the local path exist?
        if (-not (Test-Path $localPath))
        {
            throw "Source path '$localPath' does not exist."
        }

        # Get the primary storage container
        $storage = GetStorage -clusterName $clusterName

        # Upload file to storage, overwriting existing files if -force was used.
        Set-AzStorageBlobContent -File $localPath `
            -Blob $destinationPath `
            -force:$force `
            -Container $storage.container `
            -Context $storage.context
    }

    function FindAzure {
        # Is there an active Azure subscription?
        $sub = Get-AzSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Connect-AzAccount
        }
    }

    function GetStorage {
        param(
            [Parameter(Mandatory = $true)]
            [String]$clusterName
        )
        $hdi = Get-AzHDInsightCluster -ClusterName $clusterName
        # Does the cluster exist?
        if (!$hdi)
        {
            throw "HDInsight cluster '$clusterName' does not exist."
        }
        # Create a return object for context & container
        $return = @{}
        $storageAccounts = @{}

        # Get storage information
        $resourceGroup = $hdi.ResourceGroup
        $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
        $container=$hdi.DefaultStorageContainer
        $storageAccountKey=(Get-AzStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Get the resource group, in case we need that
        $return.resourceGroup = $resourceGroup
        # Get the storage context, as we can't depend
        # on using the default storage context
        $return.context = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        # Get the container, so we know where to
        # find/store blobs
        $return.container = $container
        # Return storage accounts to support finding all accounts for
        # a cluster
        $return.storageAccount = $storageAccountName
        $return.storageAccountKey = $storageAccountKey

        return $return
    }
    # Only export the verb-phrase things
    export-modulemember *-*
   ```

    Bu dosya iki modül içerir:

   * **Add-HDInsightFile** - kümeye dosya yüklemek için kullanılır
   * **Start-HBaseExample** - daha önce oluşturulan sınıfları çalıştırmak için kullanılır

2. Dosyayı `hbase-runner.psm1` dizine `hbaseapp` kaydedin.

3. Modülleri Azure PowerShell'e kaydedin. Yeni bir Azure PowerShell penceresi açın ve kümenizin adını değiştirerek `CLUSTERNAME` aşağıdaki komutu düzenleme. Ardından aşağıdaki komutları girin:

    ```powershell
    cd C:\HDI\hbaseapp
    $myCluster = "CLUSTERNAME"
    Import-Module .\hbase-runner.psm1
    ```

4. Kümenize yüklemek `hbaseapp-1.0-SNAPSHOT.jar` için aşağıdaki komutu kullanın.

    ```powershell
    Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName $myCluster
    ```

    İstendiğinde, küme giriş (yönetici) adını ve parolasını girin. Komut, kümenizin `example/jars` birincil deposundaki konuma yükler. `hbaseapp-1.0-SNAPSHOT.jar`

5. Bir tablo oluşturmak `hbaseapp`için aşağıdaki komutu kullanın:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName $myCluster
    ```

    İstendiğinde, küme giriş (yönetici) adını ve parolasını girin.

    Bu komut, HDInsight kümenizde HBase'deki **kişiler** adlı bir tablo oluşturur. Bu komut konsol penceresinde herhangi bir çıkış göstermez.

6. Tablodaki girişleri aramak için aşağıdaki komutu kullanın:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName $myCluster -emailRegex contoso.com
    ```

    İstendiğinde, küme giriş (yönetici) adını ve parolasını girin.

    Bu komut, `SearchByEmail` `contactinformation` sütun ailesi ve `email` sütun, dize `contoso.com`içeren herhangi bir satır aramak için sınıf kullanır. Aşağıdaki sonuçları almalısınız:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Değer için fabrikam.com kullanmak, e-posta alanında **fabrikam.com** olan kullanıcıları döndürür. **fabrikam.com** `-emailRegex` Arama terimi olarak normal ifadeleri de kullanabilirsiniz. Örneğin, **^r** 'r' harfiyle başlayan e-posta adreslerini döndürür.

7. Tabloyu silmek için aşağıdaki komutu kullanın:

    ```PowerShell
    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName $myCluster
    ```

### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Start-HBaseExample'ı kullanırken sonuç veya beklenmeyen sonuç yok

İşi `-showErr` çalıştırırken üretilen standart hatayı (STDERR) görüntülemek için parametreyi kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[Apache HBase ile SQLLine'ı nasıl kullanacağınızı öğrenin](apache-hbase-query-with-phoenix.md)
