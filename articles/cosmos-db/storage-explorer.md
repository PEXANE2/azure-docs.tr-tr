---
title: Azure Depolama Gezgini kullanarak Azure Cosmos DB kaynaklarını yönetme
description: Azure Depolama Gezgini kullanarak Azure Cosmos DB bağlanıp kaynaklarını yönetme hakkında bilgi edinin.
author: deborahc
ms.service: cosmos-db
ms.topic: how-to
ms.date: 08/24/2020
ms.author: dech
ms.custom: seodec18, has-adal-ref
ms.openlocfilehash: b892e4c5078b50bb865a715ddf12aebc1eb05f57
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88799120"
---
# <a name="work-with-data-using-azure-storage-explorer"></a>Azure Depolama Gezgini'ni kullanarak verilerle çalışma

Azure Depolama Gezgini’nde Azure Cosmos DB kullanılması, kullanıcıların Azure Cosmos DB varlıklarını yönetmesine, verileri düzenlemesine, saklı yordamların ve tetikleyicilerin yanı sıra Depolama blob’ları ve kuyrukları gibi diğer Azure varlıklarını güncelleştirmesine imkan tanır. Artık farklı Azure varlıklarını aynı aracı kullanarak tek bir yerde yönetebilirsiniz. Şu anda Azure Depolama Gezgini SQL, MongoDB, Graph ve tablo API 'Leri için yapılandırılmış Cosmos hesaplarını destekler.


## <a name="prerequisites"></a>Önkoşullar

MongoDB için SQL API veya Azure Cosmos DB API 'SI ile Cosmos hesabı. Hesabınız yoksa, [Azure Cosmos DB: .NET ve Azure portalı ile bir SQL API'si web uygulaması derleme](create-sql-api-dotnet.md) bölümünde açıklandığı gibi Azure portalından bir hesap oluşturabilirsiniz.

## <a name="installation"></a>Yükleme

Şuradan en yeni Azure Depolama Gezgini bileşenlerini yükleyin: [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) (Windows, Linux ve MAC sürümü artık desteklenmektedir).

## <a name="connect-to-an-azure-subscription"></a>Bir Azure aboneliğine Bağlanma

1. **Azure Depolama Gezgini**yükledikten sonra, aşağıdaki görüntüde gösterildiği gibi soldaki **eklenti** simgesini seçin:

   :::image type="content" source="./media/storage-explorer/plug-in-icon.png" alt-text="Bağlanmak için eklenti simgesini seçin":::

2. **Azure Hesabı Ekle**’yi seçip **Oturum açın**’a tıklayın.

   :::image type="content" source="./media/storage-explorer/connect-to-azure-subscription.png" alt-text="Gerekli Azure aboneliğine bağlanın":::

2. **Azure oturum açma** Iletişim kutusunda **oturum aç**' ı seçin ve Azure kimlik bilgilerinizi girin.

    :::image type="content" source="./media/storage-explorer/sign-in.png" alt-text="Azure aboneliğinizde oturum açın":::

3. Listeden aboneliğinizi seçin ve ardından **Uygula**' yı seçin.

    :::image type="content" source="./media/storage-explorer/apply-subscription.png" alt-text="Filtrelemek için listeden bir abonelik KIMLIĞI seçin":::

    Gezgin bölmesi güncelleştirilir ve seçili abonelikteki hesapları gösterir.

    :::image type="content" source="./media/storage-explorer/account-list.png" alt-text="Kullanılabilir listeden bir Azure Cosmos DB hesabı seçin":::

    **Cosmos DB hesabınızı** Azure aboneliğinize başarıyla bağladınız.

## <a name="connect-to-azure-cosmos-db-by-using-a-connection-string"></a>Bağlantı dizesi kullanarak Azure Cosmos DB’ye bağlanma

Azure Cosmos DB’ye bağlanmanın alternatif yollarından biri, bağlantı dizesi kullanmaktır. Bağlantı dizesi kullanarak bağlanmak için aşağıdaki adımları kullanın.

1. Soldaki ağaçtan **Yerel ve Bağlı**’yı bulun ve **Cosmos DB Hesapları**’na sağ tıklayıp **Cosmos DB’ye bağlan...** seçeneğini belirleyin.

    :::image type="content" source="./media/storage-explorer/connect-to-db-by-connection-string.png" alt-text="Bağlantı dizesi kullanarak Azure Cosmos DB’ye bağlanma":::

2. Yalnızca SQL ve Tablo API’sini destekler. API 'yi seçin, **bağlantı dizesini**yapıştırın, **Hesap etiketini**girin, Özeti denetlemek için **İleri** ' yi seçin ve ardından Azure Cosmos DB **hesabına Bağlan ' ı seçin.** Birincil bağlantı dizesini alma hakkında daha fazla bilgi için bkz. [bağlantı dizesini alma](manage-with-powershell.md#list-keys).

    :::image type="content" source="./media/storage-explorer/connection-string.png" alt-text="Bağlantı dizenizi girin":::

## <a name="connect-to-azure-cosmos-db-by-using-local-emulator"></a>Yerel öykünücüyü kullanarak Azure Cosmos DB’ye bağlanma

Öykünücü ile bir Azure Cosmos DB’ye bağlanmak için aşağıdaki adımları uygulayın. Şu anda yalnızca SQL hesabı desteklenmektedir.

1. Öykünücüyü yükleyip başlatın. Öykünücünün nasıl yükleneceği hakkında bilgi için bkz. [Cosmos DB Öykünücüsü](https://docs.microsoft.com/azure/cosmos-db/local-emulator)

2. Soldaki ağaçtan **Yerel ve Bağlı**’yı bulun ve **Cosmos DB Hesapları**’na sağ tıklayıp **Cosmos DB Öykünücüsüne bağlan...** seçeneğini belirleyin.

    :::image type="content" source="./media/storage-explorer/emulator-entry.png" alt-text="Öykünücüden Azure Cosmos DB bağlanma":::

3. Şu anda yalnızca SQL API’si desteklenmektedir. **Bağlantı dizesini**yapıştırın, **Hesap etiketi**girin, Özeti denetlemek için **İleri** ' yi seçin ve ardından Azure Cosmos DB **hesabına Bağlan ' ı seçin.** Birincil bağlantı dizesini alma hakkında daha fazla bilgi için bkz. [bağlantı dizesini alma](manage-with-powershell.md#list-keys).

    :::image type="content" source="./media/storage-explorer/emulator-dialog.png" alt-text="Öykünücü iletişim kutusundan Cosmos DB bağlanma":::


## <a name="azure-cosmos-db-resource-management"></a>Azure Cosmos DB kaynak yönetimi

Bir Azure Cosmos DB hesabını aşağıdaki işlemleri gerçekleştirerek yönetebilirsiniz:
* Azure portalında hesabı açın
* Kaynağı Hhızlı Erişim listesine ekleyin
* Kaynakları arayın ve yenileyin
* Veritabanı oluşturma ve silme
* Koleksiyon oluşturma ve silme
* Belgeleri oluşturun, düzenleyin, silin ve filtreleyin
* Saklı yordamları, tetikleyicileri ve kullanıcı tanımlı işlevleri yönetin

### <a name="quick-access-tasks"></a>Hızlı erişim görevleri

Gezgin bölmesindeki bir aboneliğe sağ tıklayarak birçok hızlı eylem görevi gerçekleştirebilirsiniz:

* Bir Azure Cosmos DB hesabına veya veritabanına sağ tıklayın; **Portalda Aç**’ı seçerek kaynağı Azure portalında, tarayıcıda yönetebilirsiniz.

     :::image type="content" source="./media/storage-explorer/open-in-portal.png" alt-text="Portalda açma":::

* Ayrıca, **Hızlı Erişim**’e Azure Cosmos DB hesabı, veritabanı, koleksiyonu ekleyebilirsiniz.
* **Buradan Arayın** özelliği, seçili yol altında anahtar sözcük aramayı etkinleştirir.

    :::image type="content" source="./media/storage-explorer/search-from-here.png" alt-text="buradan arayın":::

### <a name="database-and-collection-management"></a>Veritabanı ve koleksiyon yönetimi

#### <a name="create-a-database"></a>Veritabanı oluşturma

-   Azure Cosmos DB hesabına sağ tıklayın, **Veritabanı Oluştur**’u seçin, veritabanı adını girin ve işlemi tamamlamak için **Enter** tuşuna basın.

    :::image type="content" source="./media/storage-explorer/create-database.png" alt-text="Azure Cosmos hesabınızda veritabanı oluşturma":::

#### <a name="delete-a-database"></a>Veritabanı silme

- Veritabanına sağ tıklayın, **veritabanını sil**' i seçin ve açılır pencerede **Evet** ' i seçin. Veritabanı düğümü silinir ve Azure Cosmos DB hesabı otomatik olarak yenilenir.

    :::image type="content" source="./media/storage-explorer/delete-database1.png" alt-text="İlk veritabanını sil":::

    :::image type="content" source="./media/storage-explorer/delete-database2.png" alt-text="İkinci veritabanlarını silme":::

#### <a name="create-a-collection"></a>Koleksiyon oluşturma

1. Veritabanınıza sağ tıklayın, **koleksiyon oluştur**' u seçin ve ardından **koleksiyon kimliği**, **depolama kapasitesi**vb. gibi aşağıdaki bilgileri sağlayın. **Tamam** ' a tıklayarak işlemi sona erdirin.

    :::image type="content" source="./media/storage-explorer/create-collection.png" alt-text="Veritabanında ilk koleksiyon oluştur":::

    :::image type="content" source="./media/storage-explorer/create-collection2.png" alt-text="Veritabanında ikinci koleksiyon oluştur":::

2. Bölüm anahtarını belirtmek için **sınırsız** ' ı seçin ve ardından son ' u seçerek **Tamam** ' ı seçin.

    Bir koleksiyon oluşturulurken bölüm anahtarı kullanılırsa, oluşturma işlemi tamamlandıktan sonra koleksiyondaki bölüm anahtarı değeri değiştirilemez.

    :::image type="content" source="./media/storage-explorer/partitionkey.png" alt-text="Bölüm anahtarını yapılandırma":::

#### <a name="delete-a-collection"></a>Koleksiyonu silme

- Koleksiyona sağ tıklayın, **koleksiyonu Sil**' i seçin ve ardından açılır pencerede **Evet** ' i seçin.

    Koleksiyon düğümü silinir ve veritabanı otomatik olarak yenilenir.

    :::image type="content" source="./media/storage-explorer/delete-collection.png" alt-text="Koleksiyonlardan birini silme":::

### <a name="document-management"></a>Belge yönetimi

#### <a name="create-and-modify-documents"></a>Belge oluşturma ve değiştirme

- Yeni bir belge oluşturmak için sol penceredeki **belgeleri** açın, **Yeni belge**' yi seçin, sağ bölmedeki Içeriği düzenleyin ve ardından **Kaydet**' i seçin. Ayrıca var olan bir belgeyi güncelleştirebilir ve ardından **Kaydet**' i seçebilirsiniz. Değişiklikler **At** seçeneğine tıklanarak atılabilir.

    :::image type="content" source="./media/storage-explorer/document.png" alt-text="Yeni belge oluştur":::

#### <a name="delete-a-document"></a>Bir belgeyi silme

- Seçili belgeyi silmek için **Sil** düğmesine tıklayın.

#### <a name="query-for-documents"></a>Belgeler için sorgu

- Belge filtresini bir [SQL sorgusu](how-to-sql-query.md) girerek düzenleyin ve ardından **Uygula**' yı seçin.

    :::image type="content" source="./media/storage-explorer/document-filter.png" alt-text="Belirli belgeler için sorgu":::

### <a name="graph-management"></a>Graf yönetimi

#### <a name="create-and-modify-vertex"></a>Köşe oluşturma ve değiştirme

1. Yeni bir köşe oluşturmak için sol penceredeki **grafik** ' i açın, **Yeni köşe**' i seçin, içeriği düzenleyin ve **Tamam**' ı seçin.
2. Varolan bir köşeyi değiştirmek için sağ bölmedeki kalem simgesini seçin.

    :::image type="content" source="./media/storage-explorer/vertex.png" alt-text="Grafiğin izdüşümünü değiştirme":::

#### <a name="delete-a-graph"></a>Graf silme

- Bir köşeyi silmek için köşe adının yanındaki geri dönüşüm kutusu simgesini seçin.

#### <a name="filter-for-graph"></a>Grafik filtresi

- Bir [Gremlin sorgusu](gremlin-support.md) girip **Filtre Uygula**' yı seçerek grafik filtresini düzenleyin.

    :::image type="content" source="./media/storage-explorer/graph-filter.png" alt-text="Grafik sorgusu çalıştırma":::

### <a name="table-management"></a>Tablo yönetimi

#### <a name="create-and-modify-table"></a>Tablo oluşturma ve değiştirme

1. Yeni bir tablo oluşturmak için, sol penceredeki **varlıkları** açın, **Ekle**' yi seçin, **varlık Ekle** Iletişim kutusunda içeriği düzenleyin, özellik **Ekle**' ye tıklayıp **Ekle**' yi seçin.
2. Bir tabloyu değiştirmek için **Düzenle**' yi seçin, içeriği değiştirin ve ardından **Güncelleştir**' i seçin.

    :::image type="content" source="./media/storage-explorer/table.png" alt-text="Tablo oluşturma ve değiştirme":::

#### <a name="import-and-export-table"></a>Tabloyu içeri ve dışarı aktarma

1. İçeri aktarmak için **Içeri aktar** düğmesini seçin ve var olan bir tabloyu seçin.
2. Dışarı aktarmak için **dışarı aktar** düğmesini seçin ve bir hedef seçin.

    :::image type="content" source="./media/storage-explorer/table-import-export.png" alt-text="Tablo içeri veya dışarı aktarma":::

#### <a name="delete-entities"></a>Varlıkları silme

- Varlıkları seçin ve düğme **Sil**' i seçin.

    :::image type="content" source="./media/storage-explorer/table-delete.png" alt-text="Bir tablo silme":::

#### <a name="query-table"></a>Sorgu tablosu

- **Sorgu** düğmesine tıklayın, sorgu koşulunu girin ve ardından **sorgu Çalıştır** düğmesini seçin. **Sorguyu Kapat** düğmesine tıklayarak Sorgu bölmesini kapatın.

    :::image type="content" source="./media/storage-explorer/table-query.png" alt-text="Tablodaki verileri sorgulama":::

### <a name="manage-stored-procedures-triggers-and-udfs"></a>Saklı yordamları, tetikleyicileri ve UDF'leri yönetme

* Saklı yordam oluşturmak için, sol ağaçta, **saklı yordam**' i sağ tıklatın, **saklı yordam oluştur**' u seçin, sola bir ad girin, doğru pencerede saklı yordam betikleri yazın ve ardından **Oluştur**' u seçin.
* Ayrıca, mevcut saklı yordamları, güncelleştirmeyi yaparak ve sonra kaydetmek için **Güncelleştir** ' e tıklayarak düzenleyebilir veya değişikliği iptal etmek için **at** ' ı seçebilirsiniz.

    :::image type="content" source="./media/storage-explorer/stored-procedure.png" alt-text="Saklı yordamlar oluşturma ve yönetme":::
* **Tetikleyicilere** ve **UDF**’ye yönelik işlemler, **Saklı Yordamlara** benzer.

## <a name="troubleshooting"></a>Sorun giderme

[Azure Depolama Gezgini’ndeki Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/storage-explorer), Windows, macOS veya Linux’tan Sovereign Clouds ve Azure’da barındırılan Azure Cosmos DB hesaplarına bağlanmanıza olanak sağlayan tek başına bir uygulamadır. Azure Cosmos DB varlıklarını yönetmenize, verileri işlemenize, saklı yordamların ve tetikleyicilerin yanı sıra Depolama blobları ve kuyrukları gibi diğer Azure varlıklarını güncelleştirmesine olanak sağlar.

Bunlar Depolama Gezgini’nde Azure Cosmos DB için görülen yaygın sorunların çözümleridir.

### <a name="sign-in-issues"></a>Oturum açma sorunları

Devam etmeden önce uygulamanızı yeniden başlatmayı deneyin ve sorunların düzeltilip düzeltilmediğine bakın.

#### <a name="self-signed-certificate-in-certificate-chain"></a>Sertifika zincirindeki otomatik olarak imzalanan sertifika

Bu hatayı görmenizin birkaç nedeni vardır. En yaygın iki neden şudur:

+ Bir saydam proxy 'nin arkasında olursunuz, yani birisi (BT departmanınız gibi) HTTPS trafiğini kesintiye *uğratır*, şifrelerini çözerek otomatik olarak imzalanan bir sertifika kullanarak şifreleniyor.

+ Aldığınız HTTPS iletilerine otomatik olarak imzalanan bir TLS/SSL sertifikası ekleme olan virüsten koruma yazılımı gibi yazılımlar çalıştırıyorsunuz.

Depolama Gezgini bu "otomatik olarak imzalanan sertifikalardan" biriyle karşılaştığında artık aldığı HTTPS iletisinin kurcalanıp kurcalanmadığını bilemez. Otomatik olarak imzalanan sertifikanın bir kopyası varsa, Depolama Gezgini’ne buna güvenmesi gerektiğini bildirebilirsiniz. Sertifikayı kimin eklediğinden emin değilseniz, aşağıdaki adımları uygulayarak kendiniz bulmaya çalışabilirsiniz:

1. OpenSSL 'yi yükler
     - [Windows](https://slproweb.com/products/Win32OpenSSL.html) (basit sürümlerden herhangi biri olabilir)
     - Mac ve Linux: İşletim sisteminize eklenmelidir
2. OpenSSL çalıştırma
    - Windows: Yükleme dizinine gidin, ardından **/bin/** konumuna gidip **openssl.exe** dosyasına çift tıklayın.
    - Mac ve Linux: Bir terminalden **openssl** komutunu yürütün
3. `s_client -showcerts -connect microsoft.com:443` yürütme
4. Otomatik olarak imzalanan sertifikaları bulun. Hangisinin otomatik olarak imzalandığından emin değilseniz, konu ("s:") ve veren ("i:") aynı olan sertifikaları bulun.
5.  Otomatik olarak imzalanan bir sertifika bulduktan sonra, **-----BEGIN CERTIFICATE-----** ile **-----END CERTIFICATE-----** arasındaki (bu kısımlar da dahil) her şeyi kopyalayıp yeni bir .cer dosyasına yapıştırın.
6.  Depolama Gezgini açın ve ardından **Edit**  >  **SSL sertifikalarını**Düzenle  >  **sertifikaları içeri aktar**' a gidin. Dosya seçicisini kullanarak, oluşturduğunuz .cer dosyalarını bulun, seçin ve açın.

Yukarıdaki adımları kullanarak otomatik olarak imzalanan bir sertifika bulamazsanız daha fazla yardım için geri bildirim gönderebilirsiniz.

#### <a name="unable-to-retrieve-subscriptions"></a>Abonelikler alınamıyor

Başarıyla oturum açtıktan sonra aboneliklerinizi alamıyorsanız:

- [Azure Portal](https://portal.azure.com/) oturum açarak hesabınızın aboneliğe erişimi olduğunu doğrulayın
- Doğru ortamı kullanarak oturum açtığınızdan emin olun ([Azure](https://portal.azure.com/), [Azure Çin](https://portal.azure.cn/), [Azure Almanya](https://portal.microsoftazure.de/), [Azure ABD Kamu](https://portal.azure.us/) veya Özel Ortam/Azure Stack)
- Bir ara sunucunun ardından değilseniz, Depolama Gezgini ara sunucusunu düzgün şekilde yapılandırdığınızdan emin olun
- Hesabı kaldırıp yeniden eklemeyi deneyin
- Aşağıdaki dosyaları giriş dizininizden (örn: C:\Users\ContosoUser) silmeyi ve sonra hesabı yeniden eklemeyi deneyin:
  - .adalcache
  - .devaccounts
  - .extaccounts
- Oturum açarken geliştirici araçları konsolunda (f12) herhangi bir hata iletisi olup olmadığını gözlemleyin

:::image type="content" source="./media/storage-explorer/console.png" alt-text="Tüm hatalar için geliştirici araçları konsoluna bakın":::

#### <a name="unable-to-see-the-authentication-page"></a>Kimlik doğrulaması sayfası görülemiyor

Kimlik doğrulaması sayfasını göremiyorsanız:

- Bağlantınızın hızına bağlı olarak, oturum açma sayfasının yüklenmesi biraz zaman alabilir, kimlik doğrulaması iletişim kutusunu kapatmadan önce en az bir dakika bekleyin
- Bir ara sunucunun ardından değilseniz, Depolama Gezgini ara sunucusunu düzgün şekilde yapılandırdığınızdan emin olun
- F12 tuşuna basarak geliştirici konsolunu getirin. Geliştirici konsolundan gelen yanıtları izleyin ve kimlik doğrulamasının neden çalışmadığına dair bir ipucu bulup bulamayacağınızı görün

#### <a name="cannot-remove-account"></a>Hesap kaldırılamıyor

Bir hesabı kaldıramıyorsanız veya yeniden kimlik doğrulama bağlantısı bir işlem yapmazsa

- Aşağıdaki dosyaları giriş dizininizden silmeyi ve sonra hesabı yeniden eklemeyi deneyin:
  - .adalcache
  - .devaccounts
  - .extaccounts
- SAS bağlı Depolama kaynaklarını kaldırmak istiyorsanız şunları silin:
  - Windows için %AppData%/StorageExplorer klasörü
  - /Users/<your_name>/Library/Application Support desteği/Mac için StorageExplorer
  - Linux için ~/.config/StorageExplorer
  - Bu dosyaları silerseniz **tüm kimlik bilgilerinizi yeniden girmeniz gerekir**


### <a name="httphttps-proxy-issue"></a>Http/Https ara sunucu sorunu

ASE’de http/https ara sunucusunu yapılandırırken sol ağaçta Azure Cosmos DB düğümlerini listeleyemezsiniz. Şimdilik geçici bir çözüm olarak Azure portalında Azure Cosmos DB veri gezginini kullanabilirsiniz.

### <a name="development-node-under-local-and-attached-node-issue"></a>"Yerel ve Bağlı" düğümünün altındaki "Geliştirme" düğümü sorunu

Sol ağaçta "yerel ve bağlı" düğümünün altında "geliştirme" düğümü seçildikten sonra yanıt yok.  Bu beklenen bir davranıştır. Azure Cosmos DB yerel öykünücüsü, sonraki yayında desteklenecektir.

:::image type="content" source="./media/storage-explorer/development.png" alt-text="Geliştirme düğümü":::

### <a name="attaching-azure-cosmos-db-account-in-local-and-attached-node-error"></a>"Yerel ve Bağlı" düğümünde Azure Cosmos DB hesabını ekleme hatası

"Yerel ve Bağlı" düğümünde Azure Cosmos DB hesabını ekledikten sonra aşağıdaki hatayı görürseniz, doğru bağlantı dizesini kullanıp kullanmadığınızı denetleyin.

:::image type="content" source="./media/storage-explorer/attached-error.png" alt-text="Yerel ve Bağlı düğümünde Azure Cosmos DB ekleme hatası":::

### <a name="expand-azure-cosmos-db-node-error"></a>Azure Cosmos DB düğümünü genişletme hatası

Soldaki ağaç düğümlerini genişletmeye çalışırken aşağıdaki hatayı görebilirsiniz.

:::image type="content" source="./media/storage-explorer/expand-error.png" alt-text="Azure Cosmos DB düğümünü genişletme hatası":::

Aşağıdaki önerileri deneyin:

- Azure Cosmos DB hesabının, sağlama durumunda olup olmadığını denetleyin ve hesap başarıyla oluşturulduğunda yeniden deneyin.
- Hesap, "Hızlı Erişim" düğümünün veya "Yerel ve Bağlı" düğümlerinin altındaysa, hesabın silinip silinmediğini denetleyin. Silindiyse, düğümü sizin kaldırmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Depolama Gezgini’nde Azure Cosmos DB’yi kullanmayı öğrenmek için şu videoyu izleyin: [Azure Depolama Gezgini’nde Azure Cosmos DB kullanma](https://www.youtube.com/watch?v=iNIbg1DLgWo&feature=youtu.be).
* [Depolama Gezgini ile çalışmaya başlama](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) konusunda Depolama Gezgini hakkında daha fazla bilgi edinin ve daha fazla hizmet bağlayın.
