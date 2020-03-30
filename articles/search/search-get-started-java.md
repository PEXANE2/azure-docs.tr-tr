---
title: "Quickstart: REST API'lerini kullanarak Java'da arama dizini oluşturma"
titleSuffix: Azure Cognitive Search
description: Bu Java hızlı başlangıcında, Azure Bilişsel Arama REST API'lerini kullanarak bir dizin oluşturmayı, veri yüklemeyi ve sorguları çalıştırmayı öğrenin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.devlang: java
ms.service: cognitive-search
ms.topic: quickstart
ms.date: 02/25/2020
ms.openlocfilehash: 0b07e934625b09e7f6249dc00865465147f6f0ba
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77624013"
---
# <a name="quickstart-create-an-azure-cognitive-search-index-in-java-using-rest-apis"></a>Quickstart: REST API'lerini kullanarak Java'da Azure Bilişsel Arama dizini oluşturma
> [!div class="op_single_selector"]
> * [Javascript](search-get-started-nodejs.md)
> * [C #](search-get-started-dotnet.md)
> * [Java](search-get-started-java.md)
> * [Portal](search-get-started-portal.md)
> * [Powershell](search-create-index-rest-api.md)
> * [Python](search-get-started-python.md)
> * [Postman](search-get-started-postman.md)

[IntelliJ](https://www.jetbrains.com/idea/), [Java 11 SDK](/java/azure/jdk/?view=azure-java-stable)ve [Azure Bilişsel Arama REST API'sini](/rest/api/searchservice/)kullanarak bir arama dizini oluşturan, yükleyen ve sorgulayan bir Java konsolu uygulaması oluşturun. Bu makalede, uygulama oluşturmak için adım adım yönergeler sağlar. Alternatif olarak, [tam uygulamayı indirebilir ve çalıştırabilirsiniz.](/samples/azure-samples/azure-search-java-samples/java-sample-quickstart/)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı oluşturmak ve test etmek için aşağıdaki yazılım ve hizmetleri kullandık:

+ [IntelliJ IDEA](https://www.jetbrains.com/idea/)

+ [Java 11 SDK](/java/azure/jdk/?view=azure-java-stable)

+ [Bir Azure Bilişsel Arama hizmeti oluşturun](search-create-service-portal.md) veya geçerli aboneliğiniz altında [varolan bir hizmeti bulun.](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) Bu hızlı başlangıç için ücretsiz bir hizmet kullanabilirsiniz.

<a name="get-service-info"></a>

## <a name="get-a-key-and-url"></a>Bir anahtar ve URL alın

Hizmete yapılan aramalar için her istekte bir URL bitiş noktası ve erişim anahtarı gerekir. Her ikisiyle de bir arama hizmeti oluşturulur, bu nedenle aboneliğinize Azure Bilişsel Arama eklediyseniz, gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinize **Genel Bakış** sayfanızda URL'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

2. **Ayarlar** > **Tuşları'nda,** hizmetteki tüm haklar için bir yönetici anahtarı alın. İki değiştirilebilir yönetici anahtarları, bir üzerinde rulo gerekir durumda iş sürekliliği için sağlanan vardır. Nesneleri ekleme, değiştirme ve silme isteklerinde birincil veya ikincil anahtarı kullanabilirsiniz.

   Bir sorgu anahtarı da oluşturun. Salt okunur erişimle sorgu isteklerini vermek en iyi yöntemdir.

![Hizmet adını ve yöneticive sorgu anahtarlarını alın](media/search-get-started-nodejs/service-name-and-keys.png)

Hizmetinize gönderilen her istek bir api anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="set-up-your-environment"></a>Ortamınızı ayarlama

IntelliJ IDEA'yı açarak ve yeni bir proje kurarak başlayın.

### <a name="create-the-project"></a>Proje oluşturma

1. IntelliJ IDEA'yı açın ve **Yeni Proje Oluştur'u**seçin.
1. **Maven'i**seçin.
1. Project **SDK** listesinde Java 11 SDK'yı seçin.

    ![Bir maven projesi oluşturma](media/search-get-started-java/java-quickstart-create-new-maven-project.png) 

1. **GroupId** ve **ArtifactId** `AzureSearchQuickstart`için , girin.
1. Projeyi açmak için kalan varsayılanları kabul edin.

### <a name="specify-maven-dependencies"></a>Maven bağımlılıklarını belirtin

1. **Dosya** > **Ayarlarını**seçin.
1. **Ayarlar** penceresinde, **Yap, Yürütme, Dağıtım** > **Oluşturma Araçları** > **Maven** > **Alma'yı**seçin.
1. **Maven projelerini otomatik olarak** onay kutusunu seçin ve pencereyi kapatmak için **Tamam'ı** tıklatın. Maven eklentileri ve diğer bağımlılıklar, bir sonraki adımda pom.xml dosyasını güncellediğinizde otomatik olarak senkronize edilecektir.

    ![IntelliJ ayarlarında Maven alma seçenekleri](media/search-get-started-java/java-quickstart-settings-import-maven-auto.png)

1. pom.xml dosyasını açın ve içindekileri aşağıdaki Maven yapılandırma ayrıntılarıyla değiştirin. Bu [Exec Maven Plugin](https://www.mojohaus.org/exec-maven-plugin/) ve [JSON arayüzü API referansları](https://javadoc.io/doc/org.glassfish/javax.json/1.0.2) içerir

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
    
        <groupId>AzureSearchQuickstart</groupId>
        <artifactId>AzureSearchQuickstart</artifactId>
        <version>1.0-SNAPSHOT</version>
        <build>
            <sourceDirectory>src</sourceDirectory>
            <plugins>
                <plugin>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.1</version>
                    <configuration>
                        <source>11</source>
                        <target>11</target>
                    </configuration>
                </plugin>
                <plugin>
                    <groupId>org.codehaus.mojo</groupId>
                    <artifactId>exec-maven-plugin</artifactId>
                    <version>1.6.0</version>
                    <executions>
                        <execution>
                            <goals>
                                <goal>exec</goal>
                            </goals>
                        </execution>
                    </executions>
                    <configuration>
                        <mainClass>main.java.app.App</mainClass>
                        <cleanupDaemonThreads>false</cleanupDaemonThreads>
                    </configuration>
                </plugin>
            </plugins>
        </build>
        <dependencies>
            <dependency>
                <groupId>org.glassfish</groupId>
                <artifactId>javax.json</artifactId>
                <version>1.0.2</version>
            </dependency>
        </dependencies>   
    </project>
    ```

### <a name="set-up-the-project-structure"></a>Proje yapısını ayarlama

1. **Dosya** > **Proje Yapısını**seçin.
1. **Modülleri**seçin ve klasörün içeriğine erişmek `src`  >   `main` için kaynak ağacı genişletin.
1. `src`  >   `main`  >  Klasörde, `java` ekle `app` `service` ve klasörler. Bunu yapmak için `java` klasörü seçin, Alt + Ekle tuşuna basın ve ardından klasör adını girin.
1. `src`  >   `main`  > Klasörde, `resources` ekle `app` `service` ve klasörler.

    İşi bittiğinde, proje ağacı aşağıdaki resim gibi görünmelidir.

    ![Proje dizin yapısı](media/search-get-started-java/java-quickstart-basic-code-tree.png)

1. Pencereyi kapatmak için **Tamam**’a tıklayın.

### <a name="add-azure-cognitive-search-service-information"></a>Azure Bilişsel Arama hizmeti bilgileri ekleme

1. **Proje** penceresinde, `src`  >   `main`  > `resources`  >  `app` klasöre erişmek için kaynak ağacı genişletin ve bir `config.properties` dosya ekleyin. Bunu yapmak için `app` klasörü seçin, Alt + Ekle tuşuna basın, **Dosya'yı**seçin ve ardından dosya adını girin.

1. Aşağıdaki ayarları yeni dosyaya kopyalayın `<YOUR-ADMIN-KEY>`ve `<YOUR-QUERY-KEY>` hizmet adınız ve anahtarlarınızı değiştirin. `<YOUR-SEARCH-SERVICE-NAME>` Hizmet bitiş noktanız `https://mydemo.search.windows.net`ise, hizmet adı "mydemo" olacaktır.

    ```java
        SearchServiceName=<YOUR-SEARCH-SERVICE-NAME>
        SearchServiceAdminKey=<YOUR-ADMIN-KEY>
        SearchServiceQueryKey=<YOUR-QUERY-KEY>
        IndexName=hotels-quickstart
        ApiVersion=2019-05-06
    ```

### <a name="add-the-main-method"></a>Ana yöntemi ekleme

1. `src`  >   `main` Klasöre  >  `app` bir `App` sınıf ekleyin. `java`  >  Bunu yapmak için `app` klasörü seçin, Alt + Ekle tuşuna basın, **Java Sınıfı'nı**seçin ve ardından sınıf adını girin.
1. `App` Sınıfı açın ve içeriği aşağıdaki kodla değiştirin. Bu kod `main` yöntemi içerir. 

    Yorumlanmamış kod, arama hizmeti parametrelerini okur ve arama hizmeti istemcisinin bir örneğini oluşturmak için bunları kullanır. Arama hizmeti istemci kodu sonraki bölümde eklenecektir.

    Bu sınıftaki yorumlanan kod, bu hızlı başlatmanın sonraki bir bölümünde yorumlanmayacak.

    ```java
    package main.java.app;
    
    import main.java.service.SearchServiceClient;
    import java.io.IOException;
    import java.util.Properties;
    
    public class App {
    
        private static Properties loadPropertiesFromResource(String resourcePath) throws IOException {
            var inputStream = App.class.getResourceAsStream(resourcePath);
            var configProperties = new Properties();
            configProperties.load(inputStream);
            return configProperties;
        }
    
        public static void main(String[] args) {
            try {
                var config = loadPropertiesFromResource("/app/config.properties");
                var client = new SearchServiceClient(
                        config.getProperty("SearchServiceName"),
                        config.getProperty("SearchServiceAdminKey"),
                        config.getProperty("SearchServiceQueryKey"),
                        config.getProperty("ApiVersion"),
                        config.getProperty("IndexName")
                );
    
    
    //Uncomment the next 3 lines in the 1 - Create Index section of the quickstart
    //            if(client.indexExists()){ client.deleteIndex();}
    //            client.createIndex("/service/index.json");
    //            Thread.sleep(1000L); // wait a second to create the index
    
    //Uncomment the next 2 lines in the 2 - Load Documents section of the quickstart
    //            client.uploadDocuments("/service/hotels.json");
    //            Thread.sleep(2000L); // wait 2 seconds for data to upload
    
    //Uncomment the following 5 search queries in the 3 - Search an index section of the quickstart
    //            // Query 1
    //            client.logMessage("\n*QUERY 1****************************************************************");
    //            client.logMessage("Search for: Atlanta'");
    //            client.logMessage("Return: All fields'");
    //            client.searchPlus("Atlanta");
    //
    //            // Query 2
    //            client.logMessage("\n*QUERY 2****************************************************************");
    //            client.logMessage("Search for: Atlanta");
    //            client.logMessage("Return: HotelName, Tags, Address");
    //            SearchServiceClient.SearchOptions options2 = client.createSearchOptions();
    //            options2.select = "HotelName,Tags,Address";
    //            client.searchPlus("Atlanta", options2);
    //
    //            //Query 3
    //            client.logMessage("\n*QUERY 3****************************************************************");
    //            client.logMessage("Search for: wifi & restaurant");
    //            client.logMessage("Return: HotelName, Description, Tags");
    //            SearchServiceClient.SearchOptions options3 = client.createSearchOptions();
    //            options3.select = "HotelName,Description,Tags";
    //            client.searchPlus("wifi,restaurant", options3);
    //
    //            // Query 4 -filtered query
    //            client.logMessage("\n*QUERY 4****************************************************************");
    //            client.logMessage("Search for: all");
    //            client.logMessage("Filter: Ratings greater than 4");
    //            client.logMessage("Return: HotelName, Rating");
    //            SearchServiceClient.SearchOptions options4 = client.createSearchOptions();
    //            options4.filter="Rating%20gt%204";
    //            options4.select = "HotelName,Rating";
    //            client.searchPlus("*",options4);
    //
    //            // Query 5 - top 2 results, ordered by
    //            client.logMessage("\n*QUERY 5****************************************************************");
    //            client.logMessage("Search for: boutique");
    //            client.logMessage("Get: Top 2 results");
    //            client.logMessage("Order by: Rating in descending order");
    //            client.logMessage("Return: HotelId, HotelName, Category, Rating");
    //            SearchServiceClient.SearchOptions options5 = client.createSearchOptions();
    //            options5.top=2;
    //            options5.orderby = "Rating%20desc";
    //            options5.select = "HotelId,HotelName,Category,Rating";
    //            client.searchPlus("boutique", options5);
    
            } catch (Exception e) {
                System.err.println("Exception:" + e.getMessage());
                e.printStackTrace();
            }
        }
    }
    ```

### <a name="add-the-http-operations"></a>HTTP işlemlerini ekleme

1. `src`  >   `main` Klasöre  >  `service` bir`SearchServiceClient` sınıf ekleyin. `java`  >  Bunu yapmak için `service` klasörü seçin, Alt + Ekle tuşuna basın, **Java Sınıfı'nı**seçin ve ardından sınıf adını girin.
1. `SearchServiceClient` Sınıfı açın ve içindekileri aşağıdaki kodla değiştirin. Bu kod, Azure Bilişsel Arama REST API'sını kullanmak için gereken HTTP işlemlerini sağlar. Dizin oluşturma, belge yükleme ve dizin sorgulama için ek yöntemler daha sonraki bir bölüme eklenir.

    ```java
    package main.java.service;

    import javax.json.Json;
    import javax.net.ssl.HttpsURLConnection;
    import java.io.IOException;
    import java.io.StringReader;
    import java.net.HttpURLConnection;
    import java.net.URI;
    import java.net.http.HttpClient;
    import java.net.http.HttpRequest;
    import java.net.http.HttpResponse;
    import java.nio.charset.StandardCharsets;
    import java.util.Formatter;
    import java.util.function.Consumer;
    
        /* This class is responsible for implementing HTTP operations for creating the index, uploading documents and searching the data*/
        public class SearchServiceClient {
            private final String _adminKey;
            private final String _queryKey;
            private final String _apiVersion;
            private final String _serviceName;
            private final String _indexName;
            private final static HttpClient client = HttpClient.newHttpClient();
    
        public SearchServiceClient(String serviceName, String adminKey, String queryKey, String apiVersion, String indexName) {
            this._serviceName = serviceName;
            this._adminKey = adminKey;
            this._queryKey = queryKey;
            this._apiVersion = apiVersion;
            this._indexName = indexName;
        }

        private static HttpResponse<String> sendRequest(HttpRequest request) throws IOException, InterruptedException {
            logMessage(String.format("%s: %s", request.method(), request.uri()));
            return client.send(request, HttpResponse.BodyHandlers.ofString());
        }

        private static URI buildURI(Consumer<Formatter> fmtFn)
                {
                    Formatter strFormatter = new Formatter();
                    fmtFn.accept(strFormatter);
                    String url = strFormatter.out().toString();
                    strFormatter.close();
                    return URI.create(url);
        }
    
        public static void logMessage(String message) {
            System.out.println(message);
        }
    
        public static boolean isSuccessResponse(HttpResponse<String> response) {
            try {
                int responseCode = response.statusCode();
    
                logMessage("\n Response code = " + responseCode);
    
                if (responseCode == HttpURLConnection.HTTP_OK || responseCode == HttpURLConnection.HTTP_ACCEPTED
                        || responseCode == HttpURLConnection.HTTP_NO_CONTENT || responseCode == HttpsURLConnection.HTTP_CREATED) {
                    return true;
                }
    
                // We got an error
                var msg = response.body();
                if (msg != null) {
                    logMessage(String.format("\n MESSAGE: %s", msg));
                }
    
            } catch (Exception e) {
                e.printStackTrace();
            }
    
            return false;
        }
    
        public static HttpRequest httpRequest(URI uri, String key, String method, String contents) {
            contents = contents == null ? "" : contents;
            var builder = HttpRequest.newBuilder();
            builder.uri(uri);
            builder.setHeader("content-type", "application/json");
            builder.setHeader("api-key", key);
    
            switch (method) {
                case "GET":
                    builder = builder.GET();
                    break;
                case "HEAD":
                    builder = builder.GET();
                    break;
                case "DELETE":
                    builder = builder.DELETE();
                    break;
                case "PUT":
                    builder = builder.PUT(HttpRequest.BodyPublishers.ofString(contents));
                    break;
                case "POST":
                    builder = builder.POST(HttpRequest.BodyPublishers.ofString(contents));
                    break;
                default:
                    throw new IllegalArgumentException(String.format("Can't create request for method '%s'", method));
            }
            return builder.build();
        }
    }
    
    ```

### <a name="build-the-project"></a>Projeyi derleme

1. Projenizin aşağıdaki yapıya sahip olduğunu doğrulayın.

    ![Proje dizin yapısı](media/search-get-started-java/java-quickstart-basic-code-tree-plus-classes.png)

1. **Maven** araç penceresini açın ve bu maven hedefini uygulayın: `verify exec:java` 
 ![Maven hedefini uygulayın: exec:java'yı doğrulayın](media/search-get-started-java/java-quickstart-execute-maven-goal.png)

İşlem tamamlandığında, sıfır (0) çıkış kodu ardından bir BUILD SUCCESS iletisi arayın.

## <a name="1---create-index"></a>1 - Dizin oluşturma

Oteller indeks tanımı basit alanlar ve karmaşık bir alan içerir. Basit bir alana örnek olarak "HotelName" veya "Açıklama" verilebilir. "Adres" alanı karmaşık bir alandır, çünkü "Sokak Adresi" ve "Şehir" gibi alt alanları vardır. Bu hızlı başlatmada, dizin tanımı JSON kullanılarak belirtilir.

1. **Proje** penceresinde, `src`  >   `main`  > `resources`  >  `service` klasöre erişmek için kaynak ağacı genişletin ve bir `index.json` dosya ekleyin. Bunu yapmak için `app` klasörü seçin, Alt + Ekle tuşuna basın, **Dosya'yı**seçin ve ardından dosya adını girin.

1. Dosyayı `index.json` açın ve aşağıdaki dizin tanımını ekleyin.

    ```json
    {
      "name": "hotels-quickstart",
      "fields": [
        {
          "name": "HotelId",
          "type": "Edm.String",
          "key": true,
          "filterable": true
        },
        {
          "name": "HotelName",
          "type": "Edm.String",
          "searchable": true,
          "filterable": false,
          "sortable": true,
          "facetable": false
        },
        {
          "name": "Description",
          "type": "Edm.String",
          "searchable": true,
          "filterable": false,
          "sortable": false,
          "facetable": false,
          "analyzer": "en.lucene"
        },
        {
          "name": "Description_fr",
          "type": "Edm.String",
          "searchable": true,
          "filterable": false,
          "sortable": false,
          "facetable": false,
          "analyzer": "fr.lucene"
        },
        {
          "name": "Category",
          "type": "Edm.String",
          "searchable": true,
          "filterable": true,
          "sortable": true,
          "facetable": true
        },
        {
          "name": "Tags",
          "type": "Collection(Edm.String)",
          "searchable": true,
          "filterable": true,
          "sortable": false,
          "facetable": true
        },
        {
          "name": "ParkingIncluded",
          "type": "Edm.Boolean",
          "filterable": true,
          "sortable": true,
          "facetable": true
        },
        {
          "name": "LastRenovationDate",
          "type": "Edm.DateTimeOffset",
          "filterable": true,
          "sortable": true,
          "facetable": true
        },
        {
          "name": "Rating",
          "type": "Edm.Double",
          "filterable": true,
          "sortable": true,
          "facetable": true
        },
        {
          "name": "Address",
          "type": "Edm.ComplexType",
          "fields": [
            {
              "name": "StreetAddress",
              "type": "Edm.String",
              "filterable": false,
              "sortable": false,
              "facetable": false,
              "searchable": true
            },
            {
              "name": "City",
              "type": "Edm.String",
              "searchable": true,
              "filterable": true,
              "sortable": true,
              "facetable": true
            },
            {
              "name": "StateProvince",
              "type": "Edm.String",
              "searchable": true,
              "filterable": true,
              "sortable": true,
              "facetable": true
            },
            {
              "name": "PostalCode",
              "type": "Edm.String",
              "searchable": true,
              "filterable": true,
              "sortable": true,
              "facetable": true
            },
            {
              "name": "Country",
              "type": "Edm.String",
              "searchable": true,
              "filterable": true,
              "sortable": true,
              "facetable": true
            }
          ]
        }
      ]
    }
    ```

    Dizin adı "oteller-quickstart" olacaktır. Dizin alanlarındaki öznitelikler, dizinlenen verilerin bir uygulamada nasıl aranabileceğini belirler. Örneğin, `IsSearchable` öznitelik tam metin arama dahil edilmesi gereken her alana atanmalıdır. Öznitelikler hakkında daha fazla bilgi edinmek için [Alanlar koleksiyonu ve alan özniteliklerine](search-what-is-an-index.md#fields-collection)bakın.
    
    Bu `Description` dizindeki alan, `analyzer` varsayılan Lucene dil çözümleyicisini geçersiz kılmak için isteğe bağlı özelliği kullanır. Alan, `Description_fr` Fransızca metin depoladığı için `fr.lucene` Fransız Lucene çözümleyicisini kullanıyor. İsteğe `Description` bağlı Microsoft dil çözümleyicisi en.lucene kullanıyor. Çözümleyiciler hakkında daha fazla bilgi edinmek [için Azure Bilişsel Arama'da metin işleme için Çözümleyiciler'e](search-analyzers.md)bakın.

1. `SearchServiceClient` Sınıfa aşağıdaki kodu ekleyin. Bu yöntemler, bir dizin oluşturan ve silen ve bir dizinin var olup olmadığını belirleyen Azure Bilişsel Arama REST hizmet URL'leri oluşturur. Yöntemler de HTTP isteği yapmak.

    ```java
    public boolean indexExists() throws IOException, InterruptedException {
        logMessage("\n Checking if index exists...");
        var uri = buildURI(strFormatter -> strFormatter.format(
                "https://%s.search.windows.net/indexes/%s/docs?api-version=%s&search=*",
                _serviceName,_indexName,_apiVersion));
        var request = httpRequest(uri, _adminKey, "HEAD", "");
        var response = sendRequest(request);
        return isSuccessResponse(response);
    }
    
    public boolean deleteIndex() throws IOException, InterruptedException {
        logMessage("\n Deleting index...");
        var uri = buildURI(strFormatter -> strFormatter.format(
                "https://%s.search.windows.net/indexes/%s?api-version=%s",
                _serviceName,_indexName,_apiVersion));
        var request = httpRequest(uri, _adminKey, "DELETE", "*");
        var response = sendRequest(request);
        return isSuccessResponse(response);
    }
    
    
    public boolean createIndex(String indexDefinitionFile) throws IOException, InterruptedException {
        logMessage("\n Creating index...");
        //Build the search service URL
        var uri = buildURI(strFormatter -> strFormatter.format(
                "https://%s.search.windows.net/indexes/%s?api-version=%s",
                _serviceName,_indexName,_apiVersion));
        //Read in index definition file
        var inputStream = SearchServiceClient.class.getResourceAsStream(indexDefinitionFile);
        var indexDef = new String(inputStream.readAllBytes(), StandardCharsets.UTF_8);
        //Send HTTP PUT request to create the index in the search service
        var request = httpRequest(uri, _adminKey, "PUT", indexDef);
        var response = sendRequest(request);
        return isSuccessResponse(response);
    }
    ```

1. `App` Sınıftaaşağıdaki kodu açıklamayı bırakın. Bu kod varsa "otellerhızlı başlat" dizini siler ve "index.json" dosyasındaki dizin tanımına göre yeni bir dizin oluşturur. 

    Dizin oluşturma isteğinden sonra bir saniyelik bir duraklama eklenir. Bu duraklama, belgeleri yüklemeden önce dizinin oluşturulmasını sağlar.

    ```java
        if (client.indexExists()) { client.deleteIndex();}
          client.createIndex("/service/index.json");
          Thread.sleep(1000L); // wait a second to create the index
    ```

1. **Maven** araç penceresini açın ve bu maven hedefini uygulayın:`verify exec:java`

    Kod çalışırken, ardından 201 yanıt kodu yla "Dizin oluşturma" iletisi arayın. Bu yanıt kodu dizinin oluşturulduğunu doğrular. Çalıştırma, BUILD SUCCESS iletisi ve sıfır (0) çıkış koduyla sona ermelidir.
    
## <a name="2---load-documents"></a>2 - Belgeleri yükleyin

1. **Proje** penceresinde, `src`  >   `main`  > `resources`  >  `service` klasöre erişmek için kaynak ağacı genişletin ve bir `hotels.json` dosya ekleyin. Bunu yapmak için `app` klasörü seçin, Alt + Ekle tuşuna basın, **Dosya'yı**seçin ve ardından dosya adını girin.
1. Aşağıdaki otel belgelerini dosyaya ekleyin.

    ```json
    {
      "value": [
        {
          "@search.action": "upload",
          "HotelId": "1",
          "HotelName": "Secret Point Motel",
          "Description": "The hotel is ideally located on the main commercial artery of the city in the heart of New York. A few minutes away is Time's Square and the historic centre of the city, as well as other places of interest that make New York one of America's most attractive and cosmopolitan cities.",
          "Description_fr": "L'hôtel est idéalement situé sur la principale artère commerciale de la ville en plein cœur de New York. A quelques minutes se trouve la place du temps et le centre historique de la ville, ainsi que d'autres lieux d'intérêt qui font de New York l'une des villes les plus attractives et cosmopolites de l'Amérique.",
          "Category": "Boutique",
          "Tags": [ "pool", "air conditioning", "concierge" ],
          "ParkingIncluded": "false",
          "LastRenovationDate": "1970-01-18T00:00:00Z",
          "Rating": 3.60,
          "Address": {
            "StreetAddress": "677 5th Ave",
            "City": "New York",
            "StateProvince": "NY",
            "PostalCode": "10022",
            "Country": "USA"
          }
        },
        {
          "@search.action": "upload",
          "HotelId": "2",
          "HotelName": "Twin Dome Motel",
          "Description": "The hotel is situated in a  nineteenth century plaza, which has been expanded and renovated to the highest architectural standards to create a modern, functional and first-class hotel in which art and unique historical elements coexist with the most modern comforts.",
          "Description_fr": "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
          "Category": "Boutique",
          "Tags": [ "pool", "free wifi", "concierge" ],
          "ParkingIncluded": "false",
          "LastRenovationDate": "1979-02-18T00:00:00Z",
          "Rating": 3.60,
          "Address": {
            "StreetAddress": "140 University Town Center Dr",
            "City": "Sarasota",
            "StateProvince": "FL",
            "PostalCode": "34243",
            "Country": "USA"
          }
        },
        {
          "@search.action": "upload",
          "HotelId": "3",
          "HotelName": "Triple Landscape Hotel",
          "Description": "The Hotel stands out for its gastronomic excellence under the management of William Dough, who advises on and oversees all of the Hotel’s restaurant services.",
          "Description_fr": "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
          "Category": "Resort and Spa",
          "Tags": [ "air conditioning", "bar", "continental breakfast" ],
          "ParkingIncluded": "true",
          "LastRenovationDate": "2015-09-20T00:00:00Z",
          "Rating": 4.80,
          "Address": {
            "StreetAddress": "3393 Peachtree Rd",
            "City": "Atlanta",
            "StateProvince": "GA",
            "PostalCode": "30326",
            "Country": "USA"
          }
        },
        {
          "@search.action": "upload",
          "HotelId": "4",
          "HotelName": "Sublime Cliff Hotel",
          "Description": "Sublime Cliff Hotel is located in the heart of the historic center of Sublime in an extremely vibrant and lively area within short walking distance to the sites and landmarks of the city and is surrounded by the extraordinary beauty of churches, buildings, shops and monuments. Sublime Cliff is part of a lovingly restored 1800 palace.",
          "Description_fr": "Le sublime Cliff Hotel est situé au coeur du centre historique de sublime dans un quartier extrêmement animé et vivant, à courte distance de marche des sites et monuments de la ville et est entouré par l'extraordinaire beauté des églises, des bâtiments, des commerces et Monuments. Sublime Cliff fait partie d'un Palace 1800 restauré avec amour.",
          "Category": "Boutique",
          "Tags": [ "concierge", "view", "24-hour front desk service" ],
          "ParkingIncluded": "true",
          "LastRenovationDate": "1960-02-06T00:00:00Z",
          "Rating": 4.60,
          "Address": {
            "StreetAddress": "7400 San Pedro Ave",
            "City": "San Antonio",
            "StateProvince": "TX",
            "PostalCode": "78216",
            "Country": "USA"
          }
        }
      ]
    }
    ```

1. `SearchServiceClient` Sınıfa aşağıdaki kodu ekleyin. Bu kod, otel belgelerini dizine yüklemek için REST hizmet URL'sini oluşturur ve ardından HTTP POST isteğini yapar.

    ```java
    public boolean uploadDocuments(String documentsFile) throws IOException, InterruptedException {
        logMessage("\n Uploading documents...");
        //Build the search service URL
        var endpoint = buildURI(strFormatter -> strFormatter.format(
                "https://%s.search.windows.net/indexes/%s/docs/index?api-version=%s",
                _serviceName,_indexName,_apiVersion));
        //Read in the data to index
        var inputStream = SearchServiceClient.class.getResourceAsStream(documentsFile);
        var documents = new String(inputStream.readAllBytes(), StandardCharsets.UTF_8);
        //Send HTTP POST request to upload and index the data
        var request = httpRequest(endpoint, _adminKey, "POST", documents);
        var response = sendRequest(request);
        return isSuccessResponse(response);
    }
    ```

1. `App` Sınıftaaşağıdaki kodu açıklamayı bırakın. Bu kod, "hotels.json" adresindeki belgeleri dizine yükler.

    ```java
    client.uploadDocuments("/service/hotels.json");
    Thread.sleep(2000L); // wait 2 seconds for data to upload
    ```

    Dizin sorgulamadan önce belge yükleme işleminin tamamlandığından emin olmak için yükleme isteğinden sonra iki saniyelik bir duraklama eklenir.

1. **Maven** araç penceresini açın ve bu maven hedefini uygulayın:`verify exec:java`

    Önceki adımda bir "oteller hızlı başlatma" dizini oluşturduğunuziçin, kod artık silecek ve otel belgelerini yüklemeden önce yeniden oluşturur.

    Kod çalışırken, 200 yanıt kodu ardından bir "Belgeleri Yükleme" iletisini arayın. Bu yanıt kodu, belgelerin dizine yüklendiğini doğrular. Çalıştırma, BUILD SUCCESS iletisi ve sıfır (0) çıkış koduyla sona ermelidir.

## <a name="3---search-an-index"></a>3 - Dizin arama

Artık otel belgelerini yüklediğinize göre, otel verilerine erişmek için arama sorguları oluşturabilirsiniz.

1. `SearchServiceClient` Sınıfa aşağıdaki kodu ekleyin. Bu kod, dizinlenen verileri aramak için Azure Bilişsel Arama REST hizmet URL'leri oluşturur ve arama sonuçlarını yazdırır.

    Sınıf `SearchOptions` ve `createSearchOptions` yöntem, kullanılabilir Azure Bilişsel Arama REST API sorgu seçeneklerinin bir alt kümesini belirtmenize izin sağlar. REST API sorgu seçenekleri hakkında daha fazla bilgi için [Arama Belgeleri (Azure Bilişsel Arama REST API)](/rest/api/searchservice/search-documents)bakın.

    Yöntem, `SearchPlus` arama sorgusu URL'sini oluşturur, arama isteği yapar ve sonuçları konsola yazdırır. 

    ```java
    public SearchOptions createSearchOptions() { return new SearchOptions();}

    //Defines available search parameters that can be set
    public static class SearchOptions {

        public String select = "";
        public String filter = "";
        public int top = 0;
        public String orderby= "";
    }

    //Concatenates search parameters to append to the search request
    private String createOptionsString(SearchOptions options)
    {
        String optionsString = "";
        if (options != null) {
            if (options.select != "")
                optionsString = optionsString + "&$select=" + options.select;
            if (options.filter != "")
                optionsString = optionsString + "&$filter=" + options.filter;
            if (options.top != 0)
                optionsString = optionsString + "&$top=" + options.top;
            if (options.orderby != "")
                optionsString = optionsString + "&$orderby=" +options.orderby;
        }
        return optionsString;
    }
    
    public void searchPlus(String queryString)
    {
        searchPlus( queryString, null);
    }
    
    public void searchPlus(String queryString, SearchOptions options) {
    
        try {
            String optionsString = createOptionsString(options);
            var uri = buildURI(strFormatter -> strFormatter.format(
                    "https://%s.search.windows.net/indexes/%s/docs?api-version=%s&search=%s%s",
                    _serviceName, _indexName, _apiVersion, queryString, optionsString));
            var request = httpRequest(uri, _queryKey, "GET", null);
            var response = sendRequest(request);
            var jsonReader = Json.createReader(new StringReader(response.body()));
            var jsonArray = jsonReader.readObject().getJsonArray("value");
            var resultsCount = jsonArray.size();
            logMessage("Results:\nCount: " + resultsCount);
            for (int i = 0; i <= resultsCount - 1; i++) {
                logMessage(jsonArray.get(i).toString());
            }
    
            jsonReader.close();
    
        }
        catch (Exception e) {
            e.printStackTrace();
        }
    
    }
    ```

1. `App` Sınıfta, aşağıdaki kodu açıklamayı bırakın. Bu kod, döndürülecek arama metni, sorgu parametreleri ve veri alanları da dahil olmak üzere beş farklı sorgu ayarlar. 

    ```java
    // Query 1
    client.logMessage("\n*QUERY 1****************************************************************");
    client.logMessage("Search for: Atlanta");
    client.logMessage("Return: All fields'");
    client.searchPlus("Atlanta");

    // Query 2
    client.logMessage("\n*QUERY 2****************************************************************");
    client.logMessage("Search for: Atlanta");
    client.logMessage("Return: HotelName, Tags, Address");
    SearchServiceClient.SearchOptions options2 = client.createSearchOptions();
    options2.select = "HotelName,Tags,Address";
    client.searchPlus("Atlanta", options2);

    //Query 3
    client.logMessage("\n*QUERY 3****************************************************************");
    client.logMessage("Search for: wifi & restaurant");
    client.logMessage("Return: HotelName, Description, Tags");
    SearchServiceClient.SearchOptions options3 = client.createSearchOptions();
    options3.select = "HotelName,Description,Tags";
    client.searchPlus("wifi,restaurant", options3);

    // Query 4 -filtered query
    client.logMessage("\n*QUERY 4****************************************************************");
    client.logMessage("Search for: all");
    client.logMessage("Filter: Ratings greater than 4");
    client.logMessage("Return: HotelName, Rating");
    SearchServiceClient.SearchOptions options4 = client.createSearchOptions();
    options4.filter="Rating%20gt%204";
    options4.select = "HotelName,Rating";
    client.searchPlus("*",options4);

    // Query 5 - top 2 results, ordered by
    client.logMessage("\n*QUERY 5****************************************************************");
    client.logMessage("Search for: boutique");
    client.logMessage("Get: Top 2 results");
    client.logMessage("Order by: Rating in descending order");
    client.logMessage("Return: HotelId, HotelName, Category, Rating");
    SearchServiceClient.SearchOptions options5 = client.createSearchOptions();
    options5.top=2;
    options5.orderby = "Rating%20desc";
    options5.select = "HotelId,HotelName,Category,Rating";
    client.searchPlus("boutique", options5);
    ```



    [Bir sorguda terimleri eşleştirmenin](search-query-overview.md#types-of-queries)iki yolu vardır: tam metin arama ve filtreler. Tam metin arama sorgusu, dizininizdeki `IsSearchable` alanlarda bir veya daha fazla terim arar. Filtre, dizindeki alanlar üzerinde `IsFilterable` değerlendirilen bir boolean ifadesidir. Tam metin arama ve filtreleri birlikte veya ayrı ayrı kullanabilirsiniz.

1. **Maven** araç penceresini açın ve bu maven hedefini uygulayın:`verify exec:java`

    Her sorgunun ve sonuçlarının bir özetini arayın. Çalıştırma, BUILD SUCCESS iletisi ve sıfır (0) çıkış kodu ile tamamlanmalıdır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda, artık ihtiyacınız olmayan kaynakları kaldırmak iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol navigasyon bölmesindeki **Tüm kaynaklar** veya **Kaynak grupları** bağlantısını kullanarak portaldaki kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç dizin, dizin ve veri kaynağıyla sınırlı olduğunuzu unutmayın. Sınırın altında kalmak için portaldaki tek tek öğeleri silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu Java hızlı başlangıcında, bir dizin oluşturmak, belgelerle yüklemek ve sorguları çalıştırmak için bir dizi görev üzerinde çalıştınız. Temel kavramlardan memnunsanız, REST'teki dizin oluşturma işlemlerini listeleyen aşağıdaki makaleyi öneririz.

> [!div class="nextstepaction"]
> [Dizinleyici işlemleri](/rest/api/searchservice/indexer-operations)
