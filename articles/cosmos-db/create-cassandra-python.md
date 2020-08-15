---
title: 'Hızlı Başlangıç: Python ile Cassandra API’si - Azure Cosmos DB'
description: Bu hızlı başlangıçta Python ile profil uygulaması oluşturmak için Azure Cosmos DB Apache API’sinin nasıl kullanılacağı gösterilmektedir.
author: TheovanKraay
ms.author: thvankra
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.devlang: python
ms.topic: quickstart
ms.date: 08/13/2020
ms.custom: devx-track-python
ms.openlocfilehash: f376a1f3601c976ff1efdaee1da6181510a9cf64
ms.sourcegitcommit: 3bf69c5a5be48c2c7a979373895b4fae3f746757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88234950"
---
# <a name="quickstart-build-a-cassandra-app-with-python-sdk-and-azure-cosmos-db"></a>Hızlı başlangıç: Python SDK ve Azure Cosmos DB Cassandra uygulaması derleme

> [!div class="op_single_selector"]
> * [.NET](create-cassandra-dotnet.md)
> * [.NET Core](create-cassandra-dotnet-core.md)
> * [Java v3](create-cassandra-java.md)
> * [Java v4](create-cassandra-java-v4.md)
> * [Node.js](create-cassandra-nodejs.md)
> * [Python](create-cassandra-python.md)
>  

Bu hızlı başlangıçta, bir Azure Cosmos DB Cassandra API hesabı oluşturur ve GitHub 'dan kopyalanmış bir Cassandra Python uygulaması kullanarak Cassandra veritabanı ve kapsayıcısı oluşturursunuz. Azure Cosmos DB, genel dağıtım ve yatay ölçeklendirme özellikleri ile belge, tablo, anahtar değer ve grafik veritabanlarını hızlıca oluşturmanıza ve sorgulamanızı sağlayan çok modelli bir veritabanı hizmetidir.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio). Veya Azure aboneliği olmadan [ücretsiz Azure Cosmos DB deneyin](https://azure.microsoft.com/try/cosmosdb/) .
- [Python 2.7.14 + veya 3.4 +](https://www.python.org/downloads/).
- [Git](https://git-scm.com/downloads).
- [Apache Cassandra Için Python sürücüsü](https://github.com/datastax/python-driver).

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

Bir belge veritabanı oluşturmadan önce Azure Cosmos DB ile bir Cassandra hesabı oluşturmanız gerekir.

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](../../includes/cosmos-db-create-dbaccount-cassandra.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub 'dan bir Cassandra API uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle programlı bir şekilde çalışmanın ne kadar kolay olduğunu görüyorsunuz. 

1. Bir komut istemi açın. `git-samples` adlı yeni bir klasör oluşturun. Ardından, komut istemini kapatın.

    ```bash
    md "C:\git-samples"
    ```

2. Git Bash gibi bir Git terminal penceresi açın ve örnek uygulamayı yüklemek üzere yeni bir klasör olarak değiştirmek için `cd` komutunu kullanın.

    ```bash
    cd "C:\git-samples"
    ```

3. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-python-getting-started.git
    ```

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Kodun veritabanı kaynaklarını nasıl oluşturduğunu öğrenmek istiyorsanız aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Kod parçacıklarının tümü *pyquickstart.py* dosyasından alınır. Aksi durumda, [Bağlantı dizenizi güncelleştirme](#update-your-connection-string) bölümüne atlayabilirsiniz. 

* , `cluster` İle başlatılır `contactPoint` ve `port` Azure Portal alınan bilgiler. `cluster`Sonra, yöntemini kullanarak Azure Cosmos DB Cassandra API bağlanır `connect()` . Yetkili bir bağlantı, Kullanıcı adı, parola ve varsayılan sertifika veya yapılandırma dosyası içinde bir tane sağlarsanız açık bir sertifika kullanılarak oluşturulur.

  :::code language="python" source="~/cosmosdb-cassandra-python-sample/pyquickstart.py" id="authenticateAndConnect":::

* Yeni bir anahtar alanı oluşturulur.

  :::code language="python" source="~/cosmosdb-cassandra-python-sample/pyquickstart.py" id="createKeyspace":::

* Yeni bir tablo oluşturulur.

  :::code language="python" source="~/cosmosdb-cassandra-python-sample/pyquickstart.py" id="createTable":::

* Anahtar/değer varlıkları eklenir.

  :::code language="python" source="~/cosmosdb-cassandra-python-sample/pyquickstart.py" id="insertData":::

* Tüm anahtar değerlerini almak için sorgu.

  :::code language="python" source="~/cosmosdb-cassandra-python-sample/pyquickstart.py" id="queryAllItems":::
    
* Bir anahtar-değeri almak için sorgu.

  :::code language="python" source="~/cosmosdb-cassandra-python-sample/pyquickstart.py" id="queryByID":::

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin. Bağlantı dizesi, uygulamanızın barındırılan veritabanıyla iletişim kurmasına olanak tanır.

1. [Azure portal](https://portal.azure.com/)Azure Cosmos DB hesabınızda **bağlantı dizesi**' ni seçin. 

1. :::image type="icon" source="./media/create-cassandra-python/copy.png":::En üstteki değeri, ILETIŞIM noktasını kopyalamak için ekranın sağ tarafındaki düğmeyi kullanın. 

   :::image type="content" source="./media/create-cassandra-python/keys.png" alt-text="Azure portalında erişim için kullanıcı adı, parola ve erişim noktasını görüntüleme ve kopyalama, bağlantı dizesi dikey penceresi":::

1. *Config.py* dosyasını açın. 

1. Portaldan CONTACT POINT değerini 10. satırda `<FILLME>` üzerine yapıştırın.

    10. satır şuna benzer şekilde görünmelidir: 

    `'contactPoint': 'cosmos-db-quickstarts.cassandra.cosmosdb.azure.com:10350'`

1. Portaldan USERNAME değerini kopyalayın ve 6. satırda `<FILLME>` üzerine yapıştırın.

    6. satır şuna benzer şekilde görünmelidir: 

    `'username': 'cosmos-db-quickstart',`
    
1. Portaldan PASSWORD değerini kopyalayın ve 8. satırda `<FILLME>` üzerine yapıştırın.

    8. satır şuna benzer şekilde görünmelidir:

    `'password' = '2Ggkr662ifxz2Mg==`';`

1. *Config.py* dosyasını kaydedin.
    
## <a name="use-the-x509-certificate"></a>X509 sertifikası kullanma

1. Baltimore CyberTrust kök sertifikasını öğesinden yerel olarak indirin [https://cacert.omniroot.com/bc2025.crt](https://cacert.omniroot.com/bc2025.crt) . Dosyayı *. cer*dosya uzantısını kullanarak yeniden adlandırın.

   Sertifika `02:00:00:b9` seri numarasına ve `d4🇩🇪20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` SHA1 parmak izine sahiptir.

2. *Pyquickstart.py* ' i açın ve `path\to\cert` yeni sertifikanızı işaret etmek üzere değiştirin.

3. *Pyquickstart.py*Kaydet.

## <a name="run-the-python-app"></a>Python uygulamasını çalıştırma

1. `azure-cosmos-db-cassandra-python-getting-started` klasörüne geçmek için git terminalindeki cd komutunu kullanın. 

2. Gerekli modülleri yüklemek için aşağıdaki komutları çalıştırın:

    ```python
    python -m pip install cassandra-driver
    python -m pip install prettytable
    python -m pip install requests
    python -m pip install pyopenssl
    ```

2. Python uygulamanızı başlatmak için aşağıdaki komutu çalıştırın:

    ```
    python pyquickstart.py
    ```

3. Sonuçların beklendiği gibi olduğunu komut satırından kontrol edin.

    Programın yürütülmesini durdurmak için CTRL + C tuşlarına basın ve konsol penceresini kapatın. 

    :::image type="content" source="./media/create-cassandra-python/output.png" alt-text="Çıktıyı görüntüleme ve doğrulama":::
    
4. Azure portalında bu yeni verileri sorgulamak, değiştirmek ve birlikte çalışmak için **Veri Gezgini**'ni açın. 

    :::image type="content" source="./media/create-cassandra-python/data-explorer.png" alt-text="Veri Gezgini’nde verileri görüntüleme":::

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Cassandra API sahip bir Azure Cosmos DB hesabı oluşturmayı ve Cassandra veritabanı ve kapsayıcısı oluşturan Cassandra Python uygulamasını çalıştırmayı öğrendiniz. Artık Azure Cosmos DB hesabınıza daha fazla veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Cassandra verilerini Azure Cosmos DB’ye aktarma](cassandra-import-data.md)

