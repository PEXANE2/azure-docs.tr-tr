---
title: 'Öğretici: PostgreSQL için bir Azure Veritabanı Tasarla - Tek Sunucu - Azure portalı'
description: Bu öğretici, Azure portalını kullanarak PostgreSQL - Single Server için ilk Azure Veritabanınızı nasıl tasarlarsanız onu gösterir.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.custom: tutorial, mvc
ms.topic: tutorial
ms.date: 06/25/2019
ms.openlocfilehash: d7ddb286285de9be089c0a5176e33b7108021c77
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75459959"
---
# <a name="tutorial-design-an-azure-database-for-postgresql---single-server-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak PostgreSQL - Single Server için bir Azure Veritabanı tasarla

PostgreSQL için Azure Veritabanı, bulutta son derece kullanılabilir olan PostgreSQL veritabanları çalıştırmanızı, yönetmenizi ve ölçeklendirmenizi sağlayan ve yönetilen bir hizmettir. Azure portalını kullanarak, sunucunuzu kolayca yönetebilir ve bir veritabanı tasarlayabilirsiniz.

Bu öğreticide, Azure portalını kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz:
> [!div class="checklist"]
> * PostgreSQL için Azure Veritabanı sunucusu oluşturma
> * Sunucu güvenlik duvarını yapılandırma
> * Veritabanı oluşturmak için [**psql**](https://www.postgresql.org/docs/9.6/static/app-psql.html) yardımcı programını kullanma
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme
> * Verileri geri yükleme

## <a name="prerequisites"></a>Ön koşullar
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="create-an-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı oluşturma

PostgreSQL için Azure Veritabanı sunucusu, tanımlı bir dizi [işlem ve depolama kaynağı](./concepts-compute-unit-and-storage.md) ile oluşturulur. Sunucu, [Azure kaynak grubu](../azure-resource-manager/management/overview.md) içinde oluşturulur.

PostgreSQL için Azure veritabanı sunucusu oluşturmak üzere şu adımları uygulayın:
1. Azure portalının sol üst köşesinde **kaynak oluştur'u** tıklatın.
2. **Yeni** sayfasından **Veritabanları**’nı seçin ve **Veritabanları** sayfasından **PostgreSQL için Azure Veritabanı**’nı seçin.
   ![PostgreSQL için Azure Veritabanı - Veritabanı oluşturma](./media/tutorial-design-database-using-azure-portal/1-create-database.png)

3. Tek **sunucu** dağıtım seçeneğini seçin.

   ![PostgreSQL için Azure Veritabanı'nı seçin - Tek sunucu dağıtım seçeneği](./media/tutorial-design-database-using-azure-portal/select-deployment-option.png)

4. **Temel Bilgiler** formunu aşağıdaki bilgilerle doldurun:

    ![Sunucu oluşturma](./media/tutorial-design-database-using-azure-portal/create-basics.png)

    Ayar|Önerilen Değer|Açıklama
    ---|---|---
    Abonelik|Aboneliğinizin adı|Sunucunuz için kullanmak istediğiniz Azure aboneliği. Birden fazla aboneliğiniz varsa kaynağın faturalandırıldığı aboneliği seçin.
    Kaynak grubu|*myresourcegroup*| Yeni bir kaynak grubu adı veya aboneliğinizde var olan bir kaynak grubu.
    Sunucu adı |*mydemoserver*|PostgreSQL için Azure Veritabanı sunucunuzu tanıtan benzersiz bir ad. Girdiğiniz sunucu adına *postgres.database.azure.com* etki alanı adı eklenir. Sunucunuz yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. En az 3, en çok 63 karakterden oluşmalıdır.
    Veri kaynağı | *Yok* | Sıfırdan yeni bir sunucu oluşturmak için *Yok'u* seçin. (Mevcut bir PostgreSQL için Azure Veritabanı sunucusunun bir coğrafi yedeğinden bir sunucu oluşturuyorsanız, *Yedek* seçeneğini belirleyin).
    Yönetici kullanıcı adı |*myadmin*| Sunucuya bağlanırken kullanılacak kendi oturum açma hesabınız. Yönetici oturum açma adı **azure_superuser**, **azure_pg_admin**, **admin**, **administrator**, **root**, **guest** veya **public** olamaz. Bu ad **pg_** ile başlayamaz.
    Parola |Parolanız| Sunucu yönetici hesabı için yeni bir parola. 8 ile 128 arasında karakter içermelidir. Parolanız şu kategorilerden üçünde yer alan karakterlerden oluşmalıdır: İngilizce büyük harfler, İngilizce küçük harfler, sayılar (0 - 9) ve alfasayısal olmayan karakterler (!, $, #, %, vb.).
    Konum|Kullanıcılarınıza en yakın bölge| Kullanıcılarınız için en yakın olan konum.
    Sürüm|En son ana sürüm| Belirli gereksinimleriniz olmadığı sürece, en son PostgreSQL ana sürümü.
    İşlem + depolama | **Genel Amaçlı**, **5. Nesil**, **2 sanal çekirdek**, **5 GB**, **7 gün**, **Coğrafi Olarak Yedekli** | Yeni sunucunuz için işlem, depolama ve yedekleme yapılandırmaları. **Sunucuyapıla'yı**seçin. Sonraki, **Genel Amaç** sekmesini seçin. Gen *5*, *4 vCores*, *100 GB*, ve 7 *gün* **Hesaplama Oluşturma**için varsayılan değerlerdir , **vCore**, **Depolama**, ve Yedekleme **Saklama Dönemi**. Bu kaydırıcıları olduğu gibi bırakabilir veya ayarlayabilirsiniz. Coğrafi olarak yedekli depolamada sunucu yedeklerinizi etkinleştirmek için, **Fazladan Yedek Seçenekleri**’nde **Coğrafi Olarak Yedeklemeli**’yi seçin. Bu fiyatlandırma katmanı seçimini kaydetmek için **Tamam**’ı seçin. Sonraki ekran görüntüsü bu seçimleri yakalar.

   > [!NOTE]
   > Hafif işlem ve G/Ç iş yükünüz için yeterliyse Temel fiyatlandırma katmanını kullanmayı düşünün. Temel fiyatlandırma katmanında oluşturulan sunucuların daha sonra Genel Amaç veya Bellek Optimize Edilemeyeceğine dikkat edin. Daha fazla bilgi için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/postgresql/) bakın.
   > 

    !["Fiyatlandırma katmanı" bölmesi](./media/quickstart-create-database-portal/2-pricing-tier.png)

    > [!TIP]
    > **Otomatik büyüme** etkinken, sunucunuz, iş yükünüzü etkilemeden ayrılan sınıra yaklaşırken depolama alanını artırır.

5. Seçimlerinizi gözden geçirmek için **Gözden Geçir + oluşturun'u** seçin. Sunucuyu sağlamak için **Oluştur**’u seçin. Bu işlem birkaç dakika sürebilir.

6. Araç çubuğunda, dağıtım sürecini izlemek için **Bildirimler** simgesini (zil) seçin. Dağıtım tamamlandığında, bu sunucu için Azure portalı panonuzda sunucunun **Genel bakış** sayfasına bir kısayol ekleyen **Panoya sabitle** öğesini seçebilirsiniz. **Kaynağa git**’i seçmek sunucunun **Genel bakış** sayfasını açar.

    !["Bildirimler" bölmesi](./media/quickstart-create-database-portal/3-notifications.png)
   
   Varsayılan olarak, sunucunuzun altında bir **postgres** veritabanı oluşturulur. [Postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) veritabanı; kullanıcılar, yardımcı programlar ve üçüncü taraf uygulamaları tarafından kullanılmak üzere geliştirilmiş, varsayılan bir veritabanıdır. (Diğer varsayılan veritabanı: **azure_maintenance**. İşlevi, yönetilen hizmet işlemlerini kullanıcı eylemlerinden ayırmaktır. Bu veritabanına erişemezsiniz.)


## <a name="configure-a-server-level-firewall-rule"></a>Sunucu düzeyinde güvenlik duvarı kuralı oluşturma

PostgreSQL için Azure Veritabanı hizmeti, sunucu düzeyinde bir güvenlik duvarı kullanır. Varsayılan olarak bu güvenlik duvarı, belirli bir IP adresi aralığı için güvenlik duvarını açmak üzere bir güvenlik duvarı kuralı oluşturulmadıkça, tüm dış uygulama ve araçların sunucuya ve sunucu üzerindeki herhangi bir veritabanına bağlanmasını engeller. 

1. Dağıtım tamamlandıktan sonra, sol taraftaki menüden **Tüm Kaynaklar**’a tıklayın ve yeni oluşturduğunuz sunucuyu aramak için **demosunucum** adını yazın. Arama sonucunda listelenen sunucu adına tıklayın. Sunucunuzun **Genel bakış** sayfası açılır ve daha fazla yapılandırma seçenekleri sunulur.

   ![PostgreSQL için Azure Veritabanı - Sunucu arama](./media/tutorial-design-database-using-azure-portal/4-locate.png)

2. Sunucu sayfasında **Bağlantı güvenliği**’ni seçin. 

3. **Kural Adı** altındaki metin kutusuna tıklayın ve bağlantı için IP aralığını belirtmek için yeni bir güvenlik duvarı kuralı ekleyin. IP aralığınızı girin. **Kaydet**'e tıklayın.

   ![PostgreSQL için Azure Veritabanı - Güvenlik Duvarı Kuralı Oluşturma](./media/tutorial-design-database-using-azure-portal/5-firewall-2.png)

4. **Kaydet'i** tıklatın ve ardından **Bağlantılar güvenlik** sayfasını kapatmak için **X'i** tıklatın.

   > [!NOTE]
   > Azure PostgreSQL sunucusu, 5432 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 5432 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda BT departmanınız 5432 numaralı bağlantı noktasını açmadığı sürece Azure SQL Veritabanı sunucunuza bağlanamazsınız.
   >

## <a name="get-the-connection-information"></a>Bağlantı bilgilerini alma

PostgreSQL sunucusu için Azure Veritabanını oluşturduğunuzda, varsayılan **postgres** veritabanı da oluşturulmuştur. Veritabanı sunucunuza bağlanmak için ana bilgisayar bilgilerini ve erişim kimlik bilgilerini sağlamanız gerekir.

1. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’a tıklayın ve kısa süre önce oluşturduğunuz sunucuyu aratın.

   ![PostgreSQL için Azure Veritabanı - Sunucu arama](./media/tutorial-design-database-using-azure-portal/4-locate.png)

2. **demosunucum** sunucu adına tıklayın.

3. Sunucunun **Genel Bakış** sayfasını seçin. **Sunucu adını** ve **Sunucu yöneticisi oturum açma adını** not edin.

   ![PostgreSQL için Azure Veritabanı - Sunucu Yöneticisi Oturum Açma](./media/tutorial-design-database-using-azure-portal/6-server-name.png)


## <a name="connect-to-postgresql-database-using-psql"></a>psql’yi kullanarak PostgreSQL veritabanına bağlanma
İstemci bilgisayarınızda PostgreSQL yüklüyse, [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html)’nin yerel bir örneğini veya Azure Bulut Konsolu’nu kullanarak Azure PostgreSQL sunucusuna bağlanabilirsiniz. Şimdi PostgreSQL için Azure Veritabanı sunucusuna bağlanmak üzere psql komut satır yardımcı programını kullanalım.

1. PostgreSQL için Azure Veritabanı veritabanına bağlanmak üzere aşağıdaki psql komutunu çalıştırın:
   ```
   psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
   ```

   Örneğin aşağıdaki komut, erişim kimlik bilgilerini kullanarak **mydemoserver.postgres.database.azure.com** PostgreSQL sunucunuzda **postgres** adlı varsayılan veritabanına bağlanır. Parola istendiğinde seçtiğiniz `<server_admin_password>` değerini girin.
  
   ```
   psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=myadmin@mydemoserver --dbname=postgres
   ```

   > [!TIP]
   > Postgres'e bağlanmak için bir URL yolu kullanmayı tercih ediyorsanız, URL `%40`kullanıcı adı ile @ oturum açmasını kodlar. Örneğin psql bağlantı dizesi,
   > ```
   > psql postgresql://myadmin%40mydemoserver@mydemoserver.postgres.database.azure.com:5432/postgres
   > ```

2. Sunucuya bağlandıktan sonra, istemde boş bir veritabanı oluşturun:
   ```sql
   CREATE DATABASE mypgsqldb;
   ```

3. İstemde, yeni oluşturulan veritabanı **mypgsqldb**bağlantı geçmek için aşağıdaki komutu yürütmek:
   ```sql
   \c mypgsqldb
   ```

## <a name="create-tables-in-the-database"></a>Veritabanında tablo oluşturma
Artık PostgreSQL için Azure Veritabanına nasıl bağlanacağınızı bildiğinize göre bazı temel görevleri tamamlayabilirsiniz:

İlk olarak, bir tablo oluşturun ve bu tabloya bazı veriler yükleyin. Bu SQL kodunu kullanarak stok bilgilerini izleyen bir tablo oluşturalım:
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

Şimdi şunu yazarak tablo listesinde yeni oluşturulan tabloyu görebilirsiniz:
```sql
\dt
```

## <a name="load-data-into-the-tables"></a>Tablolara veri yükleme
Bir tablonuz olduğuna göre içine bazı veriler ekleyin. Açık komut istemi penceresinde, birkaç veri satırı eklemek için şu sorguyu çalıştırın.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Daha önce oluşturduğunuz stok tablosunda artık iki satırlık örnek verileriniz vardır.

## <a name="query-and-update-the-data-in-the-tables"></a>Tablolardaki verileri sorgulama ve güncelleştirme
Stok veritabanı tablosundan bilgileri almak için şu sorguyu yürütün. 
```sql
SELECT * FROM inventory;
```

Tablodaki verileri de güncelleştirebilirsiniz.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Verileri alırken güncelleştirilmiş değerleri görebilirsiniz.
```sql
SELECT * FROM inventory;
```

## <a name="restore-data-to-a-previous-point-in-time"></a>Verileri önceki bir noktaya geri yükleme
Bu tabloyu yanlışlıkla sildiğinizi düşünün. Bu, kolayca kurtaramayacağınız bir durumdur. PostgreSQL için Azure Veritabanı sunucunuzun yedeğinin olduğu herhangi bir noktaya dönerek (yapılandırdığınız yedekleme bekletme dönemine göre belirlenir) bu noktayı yeni bir sunucuya geri yükleyebilirsiniz. Bu yeni sunucuyu silinen verilerinizi kurtarmak için kullanabilirsiniz. Aşağıdaki adımlar, **demosunucum** sunucusunu envanter tablosu eklenmeden önceki bir noktaya geri yükler.

1. Sunucunuza yönelik PostgreSQL için Azure Veritabanı **Genel Bakış** sayfasında araç çubuğundaki **Geri Yükle** seçeneğine tıklayın. **Geri Yükle** sayfası açılır.

   ![Azure portalı - Geri yükleme formu seçenekleri](./media/tutorial-design-database-using-azure-portal/9-azure-portal-restore.png)

2. **Geri Yükleme** formunu gerekli bilgilerle doldurun:

   ![Azure portalı - Geri yükleme formu seçenekleri](./media/tutorial-design-database-using-azure-portal/10-azure-portal-restore.png)

   - **Geri yükleme noktası**: Sunucu değiştirilmeden önce gerçekleşen belirli bir nokta seçin
   - **Hedef sunucu**: Geri yüklemek istediğiniz yeni bir sunucu adı sağlayın
   - **Konum**: Bölgeyi seçemezsiniz, varsayılan olarak kaynak sunucuyla aynıdır
   - **Fiyatlandırma katmanı**: Bir sunucuyu geri yüklerken bu değeri değiştiremezsiniz. Kaynak sunucuyla aynıdır. 
3. Sunucuyu, [tablo silinmeden önceki belirli bir noktaya geri yüklemek için](./howto-restore-server-portal.md)**Tamam**’a tıklayın. Sunucunun farklı bir zaman noktasına geri yüklenmesi, [fiyatlandırma katmanınızın](./concepts-pricing-tiers.md) bekletme dönemi içinde olmak şartıyla, belirttiğiniz zaman noktasından itibaren özgün sunucu ile aynı yeni bir kopya sunucu oluşturur.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, aşağıdakileri yapmak için Azure portalını ve diğer yardımcı programları nasıl kullanacağınızı öğrendiniz:
> [!div class="checklist"]
> * PostgreSQL için Azure Veritabanı sunucusu oluşturma
> * Sunucu güvenlik duvarını yapılandırma
> * Veritabanı oluşturmak için [**psql**](https://www.postgresql.org/docs/9.6/static/app-psql.html) yardımcı programını kullanma
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme
> * Verileri geri yükleme

Daha sonra benzer görevleri yapmak üzere Azure CLI’nin nasıl kullanılacağını öğrenmek için şu öğreticiyi gözden geçirin: [Azure CLI kullanarak ilk PostgreSQL için Azure Veritabanınızı tasarlama](tutorial-design-database-using-azure-cli.md)
