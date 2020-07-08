---
title: Java SDK-Data Lake Storage 1. dosya sistemi işlemleri-Azure
description: Klasör oluşturma, veri dosyalarını karşıya yükleme ve indirme gibi Data Lake Storage 1. dosya sistemi işlemleri gerçekleştirmek için Azure Data Lake Storage 1. için Java SDK 'sını kullanın.
author: twooley
ms.service: data-lake-store
ms.topic: how-to
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 777f2dfdf9e9e6d80814a47101730ccb3f5ece68
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85985966"
---
# <a name="filesystem-operations-on-azure-data-lake-storage-gen1-using-java-sdk"></a>Java SDK kullanarak Azure Data Lake Storage 1. dosya sistemi işlemleri
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
> 

Klasör oluşturma, veri dosyalarını karşıya yükleme ve indirme gibi temel işlemleri gerçekleştirmek için Azure Data Lake Storage 1. Java SDK 'sını nasıl kullanacağınızı öğrenin. Data Lake Storage 1. hakkında daha fazla bilgi için bkz. [Azure Data Lake Storage 1.](data-lake-store-overview.md).

Data Lake Storage 1. için Java SDK API belgelerine, [Azure Data Lake Storage 1. Java API docs](https://azure.github.io/azure-data-lake-store-java/javadoc/)' de erişebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
* Java Development Kit (Java sürüm 1.7 veya üzerini kullanan JDK 7 ya da üzeri)
* Data Lake Storage 1. hesabı. [Azure Portal kullanarak Azure Data Lake Storage 1. kullanmaya başlama](data-lake-store-get-started-portal.md)yönergelerini izleyin.
* [Maven](https://maven.apache.org/install.html). Bu eğiticide, yapı ve proje bağımlılıkları için Maven kullanılır. Maven veya Gradle gibi bir yapı sistemi olmadan derleme yapmak mümkün olsa da bu sistemler bağımlılıkların yönetilmesini çok daha kolay hale getirir.
* (İsteğe bağlı) [IntelliJ IDEA](https://www.jetbrains.com/idea/download/), [Eclipse](https://www.eclipse.org/downloads/) vb. bir IDE.

## <a name="create-a-java-application"></a>Java uygulaması oluşturma
[GitHub’da](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) bulunan kod örneği, depoda dosya oluşturma, dosyaları birleştirme, dosya indirme ve depodaki bazı dosyaları silme işlemlerinde size yol gösterir. Makalenin bu bölümü, kodun ana bölümlerinde sizi yönlendirir.

1. Komut satırından [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) veya bir IDE kullanarak Maven projesi oluşturun. IntelliJ kullanarak Java projesi oluşturma yönergeleri için [buraya](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html) bakın. Eclipse kullanarak proje oluşturma yönergeleri için [buraya](https://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm) bakın. 

2. Maven **pom.xml** dosyanıza aşağıdaki bağımlılıkları ekleyin. Etiketinden önce aşağıdaki kod parçacığını ekleyin **\</project>** :
   
    ```xml
    <dependencies>
        <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-data-lake-store-sdk</artifactId>
        <version>2.1.5</version>
        </dependency>
        <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-nop</artifactId>
        <version>1.7.21</version>
        </dependency>
    </dependencies>
    ```
   
    Birinci bağımlılık, `azure-data-lake-store-sdk` Maven deposundan Data Lake Storage 1. SDK () kullanmaktır. İkinci bağımlılık, bu uygulama için hangi günlük altyapısının (`slf4j-nop`) kullanılacağını belirtmektir. Data Lake Storage 1. SDK, Log4J, Java günlüğü, Logback, vb. gibi birçok popüler günlük çerçevesi arasından seçim yapmanızı sağlayan [dolayısıyla slf4j](https://www.slf4j.org/) günlük façlade kullanır. Bu örnekte, günlük kaydını devre dışı bırakacak ve dolayısıyla **slf4j-nop** bağlamasını kullanacağız. Uygulamanızda diğer günlük seçeneklerini kullanmak için [buraya](https://www.slf4j.org/manual.html#projectDep) bakın.

3. Aşağıdaki içeri aktarma deyimlerini uygulamanıza ekleyin.

    ```java
    import com.microsoft.azure.datalake.store.ADLException;
    import com.microsoft.azure.datalake.store.ADLStoreClient;
    import com.microsoft.azure.datalake.store.DirectoryEntry;
    import com.microsoft.azure.datalake.store.IfExists;
    import com.microsoft.azure.datalake.store.oauth2.AccessTokenProvider;
    import com.microsoft.azure.datalake.store.oauth2.ClientCredsTokenProvider;

    import java.io.*;
    import java.util.Arrays;
    import java.util.List;
    ```

## <a name="authentication"></a>Kimlik Doğrulaması

* Uygulamanız için son kullanıcı kimlik doğrulaması için bkz. [Java kullanarak Data Lake Storage 1. Ile Son Kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-java-sdk.md).
* Uygulamanıza yönelik hizmetten hizmete kimlik doğrulaması için bkz. [Java kullanarak Data Lake Storage 1. ile hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-java.md).

## <a name="create-a-data-lake-storage-gen1-client"></a>Data Lake Storage 1. istemcisi oluşturma
[Adlstoreclient](https://azure.github.io/azure-data-lake-store-java/javadoc/) nesnesi oluşturmak, Data Lake Storage 1. Ile kimlik doğrulaması yaparken oluşturduğunuz Data Lake Storage 1. hesap adını ve belirteç sağlayıcısını belirtmenizi gerektirir (bkz. [kimlik doğrulama](#authentication) bölümü). Data Lake Storage 1. hesap adının tam etki alanı adı olması gerekir. Örneğin, **burada dolguyu** **mydatalakestoragegen1.azuredatalakestore.net**gibi bir şekilde değiştirin.

```java
private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just the account name
ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);
```

Aşağıdaki bölümlerde yer alan kod parçacıkları bazı ortak dosya sistemi işlemlerine örnekler içermektedir. Diğer işlemleri görmek için **Adlstoreclient** nesnesinin tam [Data Lake Storage 1. Java SDK API belgelerine](https://azure.github.io/azure-data-lake-store-java/javadoc/) bakabilirsiniz.

## <a name="create-a-directory"></a>Dizin oluşturma

Aşağıdaki kod parçacığı, belirttiğiniz Data Lake Storage 1. hesabının kökünde bir dizin yapısı oluşturur.

```java
// create directory
client.createDirectory("/a/b/w");
System.out.println("Directory created.");
```

## <a name="create-a-file"></a>Dosya oluşturma

Aşağıdaki kod parçacığı dizin yapısında bir dosya (c.txt) oluşturur ve dosyaya veri yazar.

```java
// create file and write some content
String filename = "/a/b/c.txt";
OutputStream stream = client.createFile(filename, IfExists.OVERWRITE  );
PrintStream out = new PrintStream(stream);
for (int i = 1; i <= 10; i++) {
    out.println("This is line #" + i);
    out.format("This is the same line (%d), but using formatted output. %n", i);
}
out.close();
System.out.println("File created.");
```

Bayt dizisi kullanarak da dosya (d.txt) oluşturabilirsiniz.

```java
// create file using byte arrays
stream = client.createFile("/a/b/d.txt", IfExists.OVERWRITE);
byte[] buf = getSampleContent();
stream.write(buf);
stream.close();
System.out.println("File created using byte array.");
```

Yukarıdaki kod parçacığında kullanılan `getSampleContent` işlevinin tanımı [GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/)'daki örnekle birlikte sunulmaktadır. 

## <a name="append-to-a-file"></a>Dosyanın sonuna ekleme

Aşağıdaki kod parçacığı var olan bir dosyaya içerik ekler.

```java
// append to file
stream = client.getAppendStream(filename);
stream.write(getSampleContent());
stream.close();
System.out.println("File appended.");
```

Yukarıdaki kod parçacığında kullanılan `getSampleContent` işlevinin tanımı [GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/)'daki örnekle birlikte sunulmaktadır.

## <a name="read-a-file"></a>Dosya okuma

Aşağıdaki kod parçacığı, Data Lake Storage 1. hesabındaki bir dosyanın içeriğini okur.

```java
// Read File
InputStream in = client.getReadStream(filename);
BufferedReader reader = new BufferedReader(new InputStreamReader(in));
String line;
while ( (line = reader.readLine()) != null) {
    System.out.println(line);
}
reader.close();
System.out.println();
System.out.println("File contents read.");
```

## <a name="concatenate-files"></a>Dosyaları birleştirme

Aşağıdaki kod parçacığı bir Data Lake Storage 1. hesabındaki iki dosyayı birleştirir. İşlem başarılı olursa birleştirilmiş dosya, var olan iki dosyanın yerini alır.

```java
// concatenate the two files into one
List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
client.concatenateFiles("/a/b/f.txt", fileList);
System.out.println("Two files concatenated into a new file.");
```

## <a name="rename-a-file"></a>Dosyayı yeniden adlandırma

Aşağıdaki kod parçacığı bir Data Lake Storage 1. hesabındaki bir dosyayı yeniden adlandırır.

```java
//rename the file
client.rename("/a/b/f.txt", "/a/b/g.txt");
System.out.println("New file renamed.");
```

## <a name="get-metadata-for-a-file"></a>Dosyanın meta verilerini alma

Aşağıdaki kod parçacığı bir Data Lake Storage 1. hesabındaki bir dosyanın meta verilerini alır.

```java
// get file metadata
DirectoryEntry ent = client.getDirectoryEntry(filename);
printDirectoryInfo(ent);
System.out.println("File metadata retrieved.");
```

## <a name="set-permissions-on-a-file"></a>Dosyanın izinlerini belirleme

Aşağıdaki kod parçacığı önceki bölümde oluşturduğunuz dosyanın izinlerini belirler.

```java
// set file permission
client.setPermission(filename, "744");
System.out.println("File permission set.");
```

## <a name="list-directory-contents"></a>Dizin içeriğini listeleme

Aşağıdaki kod parçacığı dizin içeriğini yinelemeli olarak listeler.

```java
// list directory contents
List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
System.out.println("Directory listing for directory /a/b:");
for (DirectoryEntry entry : list) {
    printDirectoryInfo(entry);
}
System.out.println("Directory contents listed.");
```

Yukarıdaki kod parçacığında kullanılan `printDirectoryInfo` işlevinin tanımı [GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/)'daki örnekle birlikte sunulmaktadır.

## <a name="delete-files-and-folders"></a>Dosyaları ve klasörleri silme

Aşağıdaki kod parçacığı, bir Data Lake Storage 1. hesabındaki belirtilen dosya ve klasörleri yinelemeli olarak siler.

```java
// delete directory along with all the subdirectories and files in it
client.deleteRecursive("/a");
System.out.println("All files and folders deleted recursively");
promptEnterKey();
```

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma
1. Bir IDE içinden çalıştırmak için **Çalıştır** düğmesini bulup basın. Maven’den çalıştırmak [exec: exec](https://www.mojohaus.org/exec-maven-plugin/exec-mojo.html)’i kullanın.
2. Komut satırından çalıştırabileceğiniz tek başına bir jar oluşturmak için jar’ı [Maven derleme eklentisini](https://maven.apache.org/plugins/maven-assembly-plugin/usage.html) kullanarak dahil edilen tüm bağımlılıklarla birlikte derleyin. [GitHub 'daki örnek kaynak kodundaki](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) pom.xml bir örnek vardır.

## <a name="next-steps"></a>Sonraki adımlar
* [Java SDK için JavaDoc’u keşfedin](https://azure.github.io/azure-data-lake-store-java/javadoc/)
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)


