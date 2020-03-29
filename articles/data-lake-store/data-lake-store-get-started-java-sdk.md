---
title: 'Java SDK: Azure Veri Gölü Depolama Gen1 dosya sistemi işlemleri | Microsoft Dokümanlar'
description: Klasör oluşturma vb. gibi Veri Gölü Depolama Gen1'de dosya sistemi işlemleri gerçekleştirmek için Azure Veri Gölü Depolama Gen1 Java SDK'yı kullanın.
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: d10e09db-5232-4e84-bb50-52efc2c21887
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: bc6e0718cdc4ccb18480dc760279da9c177db4cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60877473"
---
# <a name="filesystem-operations-on-azure-data-lake-storage-gen1-using-java-sdk"></a>Java SDK kullanarak Azure Veri Gölü Depolama Gen1'de dosya sistemi işlemleri
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
> 

Klasör oluşturma, veri dosyalarını yükleme ve indirme gibi temel işlemleri gerçekleştirmek için Azure Veri Gölü Depolama Gen1 Java SDK'yı nasıl kullanacağınızı öğrenin. Veri Gölü Depolama Gen1 hakkında daha fazla bilgi için [Azure Veri Gölü Depolama Gen1'e](data-lake-store-overview.md)bakın.

[Azure Veri Gölü Depolama Gen1 Java API dokümanlarında](https://azure.github.io/azure-data-lake-store-java/javadoc/)Veri Gölü Depolama Gen1 için Java SDK API dokümanlarına erişebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
* Java Development Kit (Java sürüm 1.7 veya üzerini kullanan JDK 7 ya da üzeri)
* Veri Gölü Depolama Gen1 hesabı. [Azure portalını kullanarak Azure Veri Gölü Depolama Gen1 ile başlayın'daki](data-lake-store-get-started-portal.md)yönergeleri izleyin.
* [Maven,](https://maven.apache.org/install.html)ne kadar. Bu eğiticide, yapı ve proje bağımlılıkları için Maven kullanılır. Maven veya Gradle gibi bir yapı sistemi olmadan derleme yapmak mümkün olsa da bu sistemler bağımlılıkların yönetilmesini çok daha kolay hale getirir.
* (İsteğe bağlı) [IntelliJ IDEA](https://www.jetbrains.com/idea/download/), [Eclipse](https://www.eclipse.org/downloads/) vb. bir IDE.

## <a name="create-a-java-application"></a>Java uygulaması oluşturma
[GitHub’da](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) bulunan kod örneği, depoda dosya oluşturma, dosyaları birleştirme, dosya indirme ve depodaki bazı dosyaları silme işlemlerinde size yol gösterir. Makalenin bu bölümü, kodun ana bölümlerinde sizi yönlendirir.

1. Komut satırından [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) veya bir IDE kullanarak Maven projesi oluşturun. IntelliJ kullanarak Java projesi oluşturma yönergeleri için [buraya](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html) bakın. Eclipse kullanarak proje oluşturma yönergeleri için [buraya](https://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm) bakın. 

2. Maven **pom.xml** dosyanıza aşağıdaki bağımlılıkları ekleyin. ** \</project>** etiketinden önce aşağıdaki snippet'i ekleyin:
   
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
   
    İlk bağımlılık, maven deposundan Veri Gölü`azure-data-lake-store-sdk`Depolama Gen1 SDK () kullanmaktır. İkinci bağımlılık, bu uygulama için hangi günlük altyapısının (`slf4j-nop`) kullanılacağını belirtmektir. Data Lake Storage Gen1 SDK, log4j, Java günlük, logback, vb. gibi bir dizi popüler günlük çerçevesi arasından seçim yapmanızı sağlayan [slf4j](https://www.slf4j.org/) günlük cephesikullanır veya günlük kullanmamanızı sağlar. Bu örnekte, günlük kaydını devre dışı bırakacak ve dolayısıyla **slf4j-nop** bağlamasını kullanacağız. Uygulamanızda diğer günlük seçeneklerini kullanmak için [buraya](https://www.slf4j.org/manual.html#projectDep) bakın.

3. Aşağıdaki içeri aktarma deyimlerini uygulamanıza ekleyin.

        import com.microsoft.azure.datalake.store.ADLException;
        import com.microsoft.azure.datalake.store.ADLStoreClient;
        import com.microsoft.azure.datalake.store.DirectoryEntry;
        import com.microsoft.azure.datalake.store.IfExists;
        import com.microsoft.azure.datalake.store.oauth2.AccessTokenProvider;
        import com.microsoft.azure.datalake.store.oauth2.ClientCredsTokenProvider;

        import java.io.*;
        import java.util.Arrays;
        import java.util.List;

## <a name="authentication"></a>Kimlik doğrulaması

* Uygulamanızın son kullanıcı kimlik doğrulaması için, [Java kullanarak Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması'na](data-lake-store-end-user-authenticate-java-sdk.md)bakın.
* Uygulamanız için servise hizmet kimlik doğrulaması için, [Java kullanarak Data Lake Storage Gen1 ile hizmete hizmet kimlik doğrulaması'na](data-lake-store-service-to-service-authenticate-java.md)bakın.

## <a name="create-a-data-lake-storage-gen1-client"></a>Veri Gölü Depolama Gen1 istemcisi oluşturma
[ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) nesnesi oluşturmak, Veri Gölü Depolama Gen1 hesap adını ve Veri Gölü Depolama Gen1 ile kimlik doğrulaması yaptığınızda oluşturduğunuz belirteç sağlayıcısını belirtmenizi gerektirir (bkz. [Kimlik Doğrulama](#authentication) bölümü). Veri Gölü Depolama Gen1 hesap adı tam nitelikli bir etki alanı adı olması gerekir. Örneğin, MYDATALAKESTORAGEGEN1.AZUREDATALAKESTORE.NET **gibi**bir şeyle **DOLDURUN-IN-HERE'ı** değiştirin.

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just the account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);

Aşağıdaki bölümlerde yer alan kod parçacıkları bazı ortak dosya sistemi işlemlerine örnekler içermektedir. Diğer işlemleri görmek için **ADLStoreClient** nesnesinin tam [Veri Gölü Depolama Gen1 Java SDK API dokümanlarına](https://azure.github.io/azure-data-lake-store-java/javadoc/) bakabilirsiniz.

## <a name="create-a-directory"></a>Dizin oluşturma

Aşağıdaki parçacık, belirttiğiniz Veri Gölü Depolama Gen1 hesabının kökünde bir dizin yapısı oluşturur.

    // create directory
    client.createDirectory("/a/b/w");
    System.out.println("Directory created.");

## <a name="create-a-file"></a>Dosya oluşturma

Aşağıdaki kod parçacığı dizin yapısında bir dosya (c.txt) oluşturur ve dosyaya veri yazar.

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

Bayt dizisi kullanarak da dosya (d.txt) oluşturabilirsiniz.

    // create file using byte arrays
    stream = client.createFile("/a/b/d.txt", IfExists.OVERWRITE);
    byte[] buf = getSampleContent();
    stream.write(buf);
    stream.close();
    System.out.println("File created using byte array.");

Yukarıdaki kod parçacığında kullanılan `getSampleContent` işlevinin tanımı [GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/)'daki örnekle birlikte sunulmaktadır. 

## <a name="append-to-a-file"></a>Dosyanın sonuna ekleme

Aşağıdaki kod parçacığı var olan bir dosyaya içerik ekler.

    // append to file
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();
    System.out.println("File appended.");

Yukarıdaki kod parçacığında kullanılan `getSampleContent` işlevinin tanımı [GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/)'daki örnekle birlikte sunulmaktadır.

## <a name="read-a-file"></a>Dosya okuma

Aşağıdaki parçacık, Veri Gölü Depolama Gen1 hesabındaki bir dosyadaki içeriği okur.

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

## <a name="concatenate-files"></a>Dosyaları birleştirme

Aşağıdaki parçacık, bir Veri Gölü Depolama Gen1 hesabında iki dosyayı birleştirir. İşlem başarılı olursa birleştirilmiş dosya, var olan iki dosyanın yerini alır.

    // concatenate the two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);
    System.out.println("Two files concatenated into a new file.");

## <a name="rename-a-file"></a>Dosyayı yeniden adlandırma

Aşağıdaki parçacık, Bir Veri Gölü Depolama Gen1 hesabındaki bir dosyayı yeniden adlandırır.

    //rename the file
    client.rename("/a/b/f.txt", "/a/b/g.txt");
    System.out.println("New file renamed.");

## <a name="get-metadata-for-a-file"></a>Dosyanın meta verilerini alma

Aşağıdaki parçacık, Bir Veri Gölü Depolama Gen1 hesabındaki bir dosyanın meta verilerini alır.

    // get file metadata
    DirectoryEntry ent = client.getDirectoryEntry(filename);
    printDirectoryInfo(ent);
    System.out.println("File metadata retrieved.");

## <a name="set-permissions-on-a-file"></a>Dosyanın izinlerini belirleme

Aşağıdaki kod parçacığı önceki bölümde oluşturduğunuz dosyanın izinlerini belirler.

    // set file permission
    client.setPermission(filename, "744");
    System.out.println("File permission set.");

## <a name="list-directory-contents"></a>Dizin içeriğini listeleme

Aşağıdaki kod parçacığı dizin içeriğini yinelemeli olarak listeler.

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }
    System.out.println("Directory contents listed.");

Yukarıdaki kod parçacığında kullanılan `printDirectoryInfo` işlevinin tanımı [GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/)'daki örnekle birlikte sunulmaktadır.

## <a name="delete-files-and-folders"></a>Dosyaları ve klasörleri silme

Aşağıdaki parçacık, Data Lake Storage Gen1 hesabında belirtilen dosya ve klasörleri özyinelemeli olarak siler.

    // delete directory along with all the subdirectories and files in it
    client.deleteRecursive("/a");
    System.out.println("All files and folders deleted recursively");
    promptEnterKey();

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma
1. Bir IDE içinden çalıştırmak için **Çalıştır** düğmesini bulup basın. Maven’den çalıştırmak [exec: exec](https://www.mojohaus.org/exec-maven-plugin/exec-mojo.html)’i kullanın.
2. Komut satırından çalıştırabileceğiniz tek başına bir jar oluşturmak için jar’ı [Maven derleme eklentisini](https://maven.apache.org/plugins/maven-assembly-plugin/usage.html) kullanarak dahil edilen tüm bağımlılıklarla birlikte derleyin. [GitHub'daki örnek kaynak kodundaki](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) pom.xml'in bir örneği vardır.

## <a name="next-steps"></a>Sonraki adımlar
* [Java SDK için JavaDoc’u keşfedin](https://azure.github.io/azure-data-lake-store-java/javadoc/)
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)


