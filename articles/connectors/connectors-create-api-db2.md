---
title: IBM DB2 kaynaklarına erişin ve yönetin
description: Azure Mantık Uygulamaları kullanarak otomatik iş akışları oluşturarak IBM DB2 kaynaklarını okuyun, güncelleyin, güncelleyin ve yönetin
services: logic-apps
ms.suite: integration
ms.reviewer: plarsen, logicappspm
ms.topic: conceptual
ms.date: 08/23/2018
tags: connectors
ms.openlocfilehash: 32b482607827ee4420e39b1936586d64f9ea3139
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77651391"
---
# <a name="access-and-manage-ibm-db2-resources-by-using-azure-logic-apps"></a>Azure Logic Apps'ı kullanarak IBM DB2 kaynaklarına erişin ve yönetin

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve IBM [DB2 konektörü ile DB2](/connectors/db2/)veritabanınızda depolanan kaynaklara dayalı olarak otomatik görevler ve iş akışları oluşturabilirsiniz. İş akışlarınız veritabanınızdaki kaynaklara bağlanabilir, veritabanı tablolarınızı okuyabilir ve listeleyebilir, satırlar ekleyebilir, satırları değiştirebilir, satırları silebilir ve daha fazlasını yapabilir. Veritabanınızdan yanıt alan ve çıktıyı diğer eylemler için kullanılabilir hale getiren eylemleri mantık uygulamalarınıza ekleyebilirsiniz.

Bu makalede, çeşitli veritabanı işlemleri gerçekleştiren bir mantık uygulaması oluşturmak nasıl gösterilmektedir. Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir'yi inceleyin?](../logic-apps/logic-apps-overview.md)

## <a name="supported-platforms-and-versions"></a>Desteklenen platformlar ve sürümler

DB2 bağlayıcısı, bir TCP/IP ağındaki uzak DB2 sunucularıyla iletişim kuran bir Microsoft istemcisi içerir. Bu bağlayıcıyı, Azure sanallaştırmasında çalışan IBM DB2 windows için bulut veritabanlarına erişmek için kullanabilirsiniz. Şirket içi [veri ağ geçidini yükledikten ve ayarladıktan](../logic-apps/logic-apps-gateway-connection.md)sonra şirket içi DB2 veritabanlarına da erişebilirsiniz.

IBM DB2 konektörü, dağıtılmış İlişkisel Veritabanı Mimarisi (DRDA) SQL Access Manager (SQLAM) 10 ve 11 sürümlerini destekleyen IBM DB2 uyumlu ürünlerle birlikte bu IBM DB2 platformlarını ve sürümlerini destekler:

| Platform | Sürüm | 
|----------|---------|
| z/OS için IBM DB2 | 11.1, 10.1 |
| ibm DB2 için i | 7.3, 7.2, 7.1 |
| LUW için IBM DB2 | 11, 10.5 |
|||

## <a name="supported-database-operations"></a>Desteklenen veritabanı işlemleri

IBM DB2 bağlayıcısı, konektördeki ilgili eylemlerle eşleyen bu veritabanı işlemlerini destekler:

| Veritabanı işlemi | Bağlayıcı eylemi |
|--------------------|------------------|
| Veritabanı tablolarını listele | Tabloları alın |
| SELECT'i kullanarak bir satırı okuma | Satır al |
| SELECT'i kullanarak tüm satırları okuma | Satırları alın |
| INSERT kullanarak bir satır ekleme | Satır ekle |
| UPDATE kullanarak bir satır ı edin | Satırı güncelleştir |
| DELETE kullanarak bir satırı kaldırma | Satırı silme |
|||

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Bulut tabanlı veya şirket içi bir IBM DB2 veritabanı

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* DB2 veritabanınıza erişmek istediğiniz mantık uygulaması. Bu bağlayıcı yalnızca eylemler sağlar, bu nedenle mantık uygulamanızı başlatmak için, örneğin **Yinelenen** Tetikleyici gibi ayrı bir tetikleyici seçin.
Bu makaledeki **örneklerde Yineleme** tetikleyicisi kullanılır.

<a name="add-db2-action"></a>

## <a name="add-db2-action---get-tables"></a>DB2 eylem ekle - Tabloları al

1. Azure [portalında,](https://portal.azure.com)mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Tetikleyicinin altında **Yeni adımı**seçin.

1. Arama kutusuna filtreniz olarak "db2" girin. Bu örnekiçin, eylemler listesinin altında şu eylemi seçin: **Tabloları al (Önizleme)**

   ![Eylem seçin](./media/connectors-create-api-db2/select-db2-action.png)

   Şimdi DB2 veritabanınız için bağlantı ayrıntılarını sağlamanız istenir.

1. [Bulut veritabanları](#cloud-connection) veya şirket [içi veritabanları](#on-premises-connection)için bağlantı oluşturmak için adımları izleyin.

<a name="cloud-connection"></a>

## <a name="connect-to-cloud-db2"></a>BulutA bağlan DB2

Bağlantınızı kurmak için istendiğinde bu bağlantı ayrıntılarını sağlayın, **Oluştur'u**seçin ve ardından mantık uygulamanızı kaydedin:

| Özellik | Gerekli | Açıklama |
|----------|----------|-------------|
| **Şirket içi ağ geçidi üzerinden bağlanın** | Hayır | Yalnızca şirket içi bağlantılar için geçerlidir. |
| **Bağlantı Adı** | Evet | Bağlantınızın adı, örneğin, "MyLogicApp-DB2-connection" |
| **Sunucu** | Evet | DB2 sunucunuzun adresi veya diğer nokta noktası numarası, örneğin, "myDB2server.cloudapp.net:50000" <p><p>**Not**: Bu değer, IPv4 veya IPv6 biçiminde bir TCP/IP adresini veya diğer adını temsil eden bir dizedir ve ardından bir üst üste ve TCP/IP bağlantı noktası numarasıdır. |
| **Database** | Evet | Veritabanınızın adı <p><p>**Not**: Bu değer, DRDA İlişkisel Veritabanı Adını (RDBNAM) temsil eden bir dizedir: <p>- z/OS için DB2, veritabanının "z/OS için IBM DB2" konumu olarak bilinen 16 baytlık bir dize kabul eder. <br>- DB2 i veritabanı "IBM DB2 i için" ilişkisel veritabanı olarak bilinen bir 18 bayt dize kabul eder. <br>- LUW için DB2 8 baytlık bir dize kabul eder. |
| **Username** | Evet | Veritabanı için kullanıcı adınız <p><p>**Not**: Bu değer, uzunluğu belirli bir veritabanına dayanan bir dizedir: <p><p>- Z/OS için DB2 8 baytlık bir dize kabul eder. <br>- DB2 i için 10 bayt dize kabul eder. <br>- Linux veya UNIX için DB2 8 baytlık bir dize kabul eder. <br>- Windows için DB2 30 baytlık bir dize kabul eder. |
| **Parola** | Evet | Veritabanı için parolanız |
||||

Örnek:

![Bulut tabanlı veritabanları için bağlantı ayrıntıları](./media/connectors-create-api-db2/create-db2-cloud-connection.png)

<a name="on-premises-connection"></a>

## <a name="connect-to-on-premises-db2"></a>Şirket içi DB2'ye bağlanın

Bağlantınızı oluşturmadan önce şirket içi veri ağ geçidinizin zaten yüklü olması gerekir. Aksi takdirde, bağlantınızı kurmayı bitiremezsiniz. Ağ geçidi yüklemeniz varsa, bu bağlantı ayrıntılarını sağlamaya devam edin ve ardından **Oluştur'u**seçin.

| Özellik | Gerekli | Açıklama |
|----------|----------|-------------|
| **Şirket içi ağ geçidi üzerinden bağlanın** | Evet | Şirket içi bağlantı istediğinizde ve şirket içi bağlantı özelliklerini gösterdiğinde geçerlidir. |
| **Bağlantı Adı** | Evet | Bağlantınızın adı, örneğin, "MyLogicApp-DB2-connection" | 
| **Sunucu** | Evet | DB2 sunucunuzun adresi veya diğer adı kolon bağlantı noktası numarası, örneğin, "myDB2server:50000" <p><p>**Not**: Bu değer, IPv4 veya IPv6 biçiminde bir TCP/IP adresini veya diğer adını temsil eden bir dizedir ve ardından bir üst üste ve TCP/IP bağlantı noktası numarasıdır. |
| **Database** | Evet | Veritabanınızın adı <p><p>**Not**: Bu değer, DRDA İlişkisel Veritabanı Adını (RDBNAM) temsil eden bir dizedir: <p>- z/OS için DB2, veritabanının "z/OS için IBM DB2" konumu olarak bilinen 16 baytlık bir dize kabul eder. <br>- DB2 i veritabanı "IBM DB2 i için" ilişkisel veritabanı olarak bilinen bir 18 bayt dize kabul eder. <br>- LUW için DB2 8 baytlık bir dize kabul eder. |
| **Kimlik doğrulaması** | Evet | Bağlantınızın kimlik doğrulama türü, örneğin, "Temel" <p><p>**Not**: Temel veya Windows (Kerberos) içeren listeden bu değeri seçin. |
| **Username** | Evet | Veritabanı için kullanıcı adınız <p><p>**Not**: Bu değer, uzunluğu belirli bir veritabanına dayanan bir dizedir: <p><p>- Z/OS için DB2 8 baytlık bir dize kabul eder. <br>- DB2 i için 10 bayt dize kabul eder. <br>- Linux veya UNIX için DB2 8 baytlık bir dize kabul eder. <br>- Windows için DB2 30 baytlık bir dize kabul eder. |
| **Parola** | Evet | Veritabanı için parolanız |
| **Ağ geçidi** | Evet | Yüklü şirket içi veri ağ geçidinizin adı <p><p>**Not**: Azure aboneliğiniz ve kaynak grubunuzdaki tüm yüklü veri ağ geçitlerini içeren listeden bu değeri seçin. |
||||

Örnek:

![Şirket içi veritabanları için bağlantı ayrıntıları](./media/connectors-create-api-db2/create-db2-on-premises-connection.png)

### <a name="view-output-tables"></a>Çıktı tablolarını görüntüleme

Mantık uygulamanızı el ile çalıştırmak için, tasarımcı araç çubuğunda **Çalıştır'ı**seçin. Mantık uygulamanız çalışmaya bittikten sonra, çıktıyı çalıştırmadan görüntüleyebilirsiniz.

1. Mantık uygulaması menüsünde **Genel Bakış'ı**seçin.

1. **Özet**altında, **Çalıştır'lar geçmişi** bölümünde, listedeki ilk öğe olan en son çalıştır'ı seçin.

   ![Çalıştırma geçmişini görüntüleme](./media/connectors-create-api-db2/run-history.png)

1. **Logic uygulaması altında,** artık mantık uygulamanızdaki her adım için durum, giriş ve çıktıları inceleyebilirsiniz.
Tabloları **Al** eylemini genişletin.

   ![Eylemi genişletme](./media/connectors-create-api-db2/expand-action-step.png)

1. Girişleri görüntülemek için **ham girişleri göster'i**seçin.

1. Çıktıları görüntülemek için **ham çıktıları göster'i**seçin.

   Çıktılar tabloların bir listesini içerir.

   ![Çıktı tablolarını görüntüleme](./media/connectors-create-api-db2/db2-connector-get-tables-outputs.png)

## <a name="get-row"></a>Satır al

DB2 veritabanı tablosunda bir kayıt almak için mantık uygulamanızda **satır al** eylemini kullanın. Bu eylem, örneğin `SELECT WHERE` bir DB2 `SELECT FROM AREA WHERE AREAID = '99999'`deyimi çalıştırır.

1. Mantık uygulamanızda daha önce DB2 eylemlerini hiç kullanmadıysanız, [DB2 ekle eylemindeki](#add-db2-action) adımları gözden geçirin - Tablolar bölümünü alın, ancak bunun yerine **satır eylemini al'ı** ekleyin ve devam etmek için buraya dönün.

   **Satır Al** eylemini ekledikten sonra, örnek mantık uygulamanız şu şekilde görünür:

   ![Satır eylemi alın](./media/connectors-create-api-db2/db2-get-row-action.png)

1. Gerekli tüm özellikler için değerleri belirtin (*). Bir tablo seçtikten sonra, eylem bu tablodaki kayıtlara özgü alakalı özellikleri gösterir.

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Tablo adı** | Evet | Bu örnekte "ALAN" gibi istediğiniz kaydın bulunduğu tablo |
   | **Alan Kimliği** | Evet | Bu örnekte "99999" gibi istediğiniz kaydın kimliği |
   ||||

   ![Tablo seçin](./media/connectors-create-api-db2/db2-get-row-action-select-table.png)

1. Bittiğinde, tasarımcı araç çubuğunda **Kaydet'i**seçin.

### <a name="view-output-row"></a>Çıktı satırını görüntüleme

Mantık uygulamanızı el ile çalıştırmak için, tasarımcı araç çubuğunda **Çalıştır'ı**seçin. Mantık uygulamanız çalışmaya bittikten sonra, çıktıyı çalıştırmadan görüntüleyebilirsiniz.

1. Mantık uygulaması menüsünde **Genel Bakış'ı**seçin.

1. **Özet**altında, **Çalıştır'lar geçmişi** bölümünde, listedeki ilk öğe olan en son çalıştır'ı seçin.

1. **Logic uygulaması altında,** artık mantık uygulamanızdaki her adım için durum, giriş ve çıktıları inceleyebilirsiniz.
Satır **Al** eylemini genişletin.

1. Girişleri görüntülemek için **ham girişleri göster'i**seçin.

1. Çıktıları görüntülemek için **ham çıktıları göster'i**seçin.

   Çıktılar belirtilen satırı içerir.

   ![Çıktı satırını görüntüleme](./media/connectors-create-api-db2/db2-connector-get-row-outputs.png)

## <a name="get-rows"></a>Satırları alın

DB2 veritabanı tablosundaki tüm kayıtları almak için mantık uygulamanızdaki **satır ları al** eylemini kullanın. Bu eylem, örneğin `SELECT` bir DB2 `SELECT * FROM AREA`deyimi çalıştırır.

1. Mantık uygulamanızda daha önce DB2 eylemlerini hiç kullanmadıysanız, [DB2 ekle eylemindeki](#add-db2-action) adımları gözden geçirin - Tablolar bölümünü alın, ancak bunun yerine **satır ları al** eylemini ekleyin ve devam etmek için buraya dönün.

   **Satırları Al** eylemini ekledikten sonra, örnek mantık uygulamanız şu şekilde görünür:

   ![Satır eylemi alma](./media/connectors-create-api-db2/db2-get-rows-action.png)

1. Tablo **ad** listesini açın ve ardından bu örnekte "ALAN" olan istediğiniz tabloyu seçin:

   ![Tablo seçin](./media/connectors-create-api-db2/db2-get-rows-action-select-table.png)

1. Sonuçlar için filtre veya sorgu belirtmek için **gelişmiş seçenekleri göster'i**seçin.

1. Bittiğinde, tasarımcı araç çubuğunda **Kaydet'i**seçin.

### <a name="view-output-rows"></a>Çıktı satırlarını görüntüleme

Mantık uygulamanızı el ile çalıştırmak için, tasarımcı araç çubuğunda **Çalıştır'ı**seçin. Mantık uygulamanız çalışmaya bittikten sonra, çıktıyı çalıştırmadan görüntüleyebilirsiniz.

1. Mantık uygulaması menüsünde **Genel Bakış'ı**seçin.

1. **Özet**altında, **Çalıştır'lar geçmişi** bölümünde, listedeki ilk öğe olan en son çalıştır'ı seçin.

1. **Logic uygulaması altında,** artık mantık uygulamanızdaki her adım için durum, giriş ve çıktıları inceleyebilirsiniz.
Satır **al eylemini** genişletin.

1. Girişleri görüntülemek için **ham girişleri göster'i**seçin.

1. Çıktıları görüntülemek için **ham çıktıları göster'i**seçin.

   Çıktılar, belirtilen tablonuzdaki tüm kayıtları içerir.

   ![Çıktı satırlarını görüntüleme](./media/connectors-create-api-db2/db2-connector-get-rows-outputs.png)

## <a name="insert-row"></a>Satır ekle

DB2 veritabanı tablosuna tek bir kayıt eklemek için mantık uygulamanızdaki **Satır Ekle** eylemini kullanın. Bu eylem, örneğin `INSERT` bir DB2 `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`deyimi çalıştırır.

1. Mantık uygulamanızda daha önce DB2 eylemlerini hiç kullanmadıysanız, [DB2 ekle eylemindeki](#add-db2-action) adımları gözden geçirin - Tablolar bölümünü alın, ancak bunun yerine **Satır Ekle** eylemini ekleyin ve devam etmek için buraya dönün.

   **Satır Ekle** eylemini ekledikten sonra, örnek mantık uygulamanız şu şekilde görünür:

   ![Satır ekleme eylemi](./media/connectors-create-api-db2/db2-insert-row-action.png)

1. Gerekli tüm özellikler için değerleri belirtin (*). Bir tablo seçtikten sonra, eylem bu tablodaki kayıtlara özgü alakalı özellikleri gösterir.

   Bu örnekiçin, aşağıda özellikleri verilmiştir:

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Tablo adı** | Evet | "ALAN" gibi kaydın eklendiği tablo |
   | **Alan Kimliği** | Evet | "99999" gibi eklenecek alanın kimliği |
   | **Alan açıklaması** | Evet | Eklenecek alan için açıklama, örneğin "Alan 99999" |
   | **Bölge Kimliği** | Evet | "102" gibi eklenecek bölge kimliği |
   |||| 

   Örnek:

   ![Tablo seçin](./media/connectors-create-api-db2/db2-insert-row-action-select-table.png)

1. Bittiğinde, tasarımcı araç çubuğunda **Kaydet'i**seçin.

### <a name="view-insert-row-outputs"></a>Satır ekleme çıktılarını görüntüleme

Mantık uygulamanızı el ile çalıştırmak için, tasarımcı araç çubuğunda **Çalıştır'ı**seçin. Mantık uygulamanız çalışmaya bittikten sonra, çıktıyı çalıştırmadan görüntüleyebilirsiniz.

1. Mantık uygulaması menüsünde **Genel Bakış'ı**seçin.

1. **Özet**altında, **Çalıştır'lar geçmişi** bölümünde, listedeki ilk öğe olan en son çalıştır'ı seçin.

1. **Logic uygulaması altında,** artık mantık uygulamanızdaki her adım için durum, giriş ve çıktıları inceleyebilirsiniz.
Satır **Ekle** eylemini genişletin.

1. Girişleri görüntülemek için **ham girişleri göster'i**seçin.

1. Çıktıları görüntülemek için **ham çıktıları göster'i**seçin.

   Çıktılar, belirttiğiniz tabloya eklediğiniz kaydı içerir.

   ![Eklenen satırla çıktıyı görüntüleme](./media/connectors-create-api-db2/db2-connector-insert-row-outputs.png)

## <a name="update-row"></a>Satırı güncelleştir

DB2 veritabanı tablosundaki tek bir kaydı güncelleştirmek için mantık uygulamanızdaki **Güncelleştirme satırı** eylemini kullanın. Bu eylem, örneğin `UPDATE` bir DB2 `UPDATE AREA SET AREAID = '99999', AREADESC = 'Updated 99999', REGIONID = 102)`deyimi çalıştırır.

1. Mantık uygulamanızda daha önce DB2 eylemlerini hiç kullanmadıysanız, [DB2 ekle eylemindeki](#add-db2-action) adımları gözden geçirin - Tablolar bölümünü alın, ancak bunun yerine **Güncelleştirme satırı** eylemini ekleyin ve devam etmek için buraya dönün.

   **Güncelleştirme satırı** eylemini ekledikten sonra, örnek mantık uygulamanız şu şekilde görünür:

   ![Satır eylemini güncelleştirme](./media/connectors-create-api-db2/db2-update-row-action.png)

1. Gerekli tüm özellikler için değerleri belirtin (*). Bir tablo seçtikten sonra, eylem bu tablodaki kayıtlara özgü alakalı özellikleri gösterir.

   Bu örnekiçin, aşağıda özellikleri verilmiştir:

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Tablo adı** | Evet | "ALAN" gibi kaydın güncelleştirilen tablo |
   | **Satır Kimliği** | Evet | "99999" gibi kaydın güncelleştirilen kimliği |
   | **Alan Kimliği** | Evet | "99999" gibi yeni alan kimliği |
   | **Alan açıklaması** | Evet | "Güncellendi 99999" gibi yeni alan açıklaması |
   | **Bölge Kimliği** | Evet | "102" gibi yeni bölge kimliği |
   ||||

   Örnek:

   ![Tablo seçin](./media/connectors-create-api-db2/db2-update-row-action-select-table.png)

1. Bittiğinde, tasarımcı araç çubuğunda **Kaydet'i**seçin.

### <a name="view-update-row-outputs"></a>Güncelleştirme satırı çıktılarını görüntüleme

Mantık uygulamanızı el ile çalıştırmak için, tasarımcı araç çubuğunda **Çalıştır'ı**seçin. Mantık uygulamanız çalışmaya bittikten sonra, çıktıyı çalıştırmadan görüntüleyebilirsiniz.

1. Mantık uygulaması menüsünde **Genel Bakış'ı**seçin.

1. **Özet**altında, **Çalıştır'lar geçmişi** bölümünde, listedeki ilk öğe olan en son çalıştır'ı seçin.

1. **Logic uygulaması altında,** artık mantık uygulamanızdaki her adım için durum, giriş ve çıktıları inceleyebilirsiniz.
Güncelleştirme **satırı** eylemini genişletin.

1. Girişleri görüntülemek için **ham girişleri göster'i**seçin.

1. Çıktıları görüntülemek için **ham çıktıları göster'i**seçin.

   Çıktılar, belirtilen tablonuzda güncelleştirdiğiniz kaydı içerir.

   ![Güncelleştirilmiş satırla çıktıgörüntüleme](./media/connectors-create-api-db2/db2-connector-update-row-outputs.png)

## <a name="delete-row"></a>Satırı silme

DB2 veritabanı tablosundan tek bir kaydı silmek için mantık uygulamanızdaki **Sil satır** eylemini kullanın. Bu eylem, örneğin `DELETE` bir DB2 `DELETE FROM AREA WHERE AREAID = '99999'`deyimi çalıştırır.

1. Mantık uygulamanızda daha önce DB2 eylemlerini hiç kullanmadıysanız, [DB2 ekle eylemindeki](#add-db2-action) adımları gözden geçirin - Tablolar bölümünü alın, ancak bunun yerine **satır eylemini sil** ve devam etmek için buraya dönün.

   **Sil satırı** eylemini ekledikten sonra, örnek mantık uygulamanız şu şekilde görünür:

   ![Satır eylemini silme](./media/connectors-create-api-db2/db2-delete-row-action.png)

1. Gerekli tüm özellikler için değerleri belirtin (*). Bir tablo seçtikten sonra, eylem bu tablodaki kayıtlara özgü alakalı özellikleri gösterir.

   Bu örnekiçin, aşağıda özellikleri verilmiştir:

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Tablo adı** | Evet | "ALAN" gibi kaydın silinebileceği tablo |
   | **Satır Kimliği** | Evet | "99999" gibi kaydın silinecek kimliği |
   ||||

   Örnek:

   ![Tablo seçin](./media/connectors-create-api-db2/db2-delete-row-action-select-table.png)

1. Bittiğinde, tasarımcı araç çubuğunda **Kaydet'i**seçin.

### <a name="view-delete-row-outputs"></a>Satır çıktılarını silme

Mantık uygulamanızı el ile çalıştırmak için, tasarımcı araç çubuğunda **Çalıştır'ı**seçin. Mantık uygulamanız çalışmaya bittikten sonra, çıktıyı çalıştırmadan görüntüleyebilirsiniz.

1. Mantık uygulaması menüsünde **Genel Bakış'ı**seçin.

1. **Özet**altında, **Çalıştır'lar geçmişi** bölümünde, listedeki ilk öğe olan en son çalıştır'ı seçin.

1. **Logic uygulaması altında,** artık mantık uygulamanızdaki her adım için durum, giriş ve çıktıları inceleyebilirsiniz.
Sil **satır** eylemini genişletin.

1. Girişleri görüntülemek için **ham girişleri göster'i**seçin.

1. Çıktıları görüntülemek için **ham çıktıları göster'i**seçin.

   Çıktılar artık belirttiğiniz tablodan sildiğiniz kaydı içermez.

   ![Silinmiş satır olmadan çıktıgörüntüleme](./media/connectors-create-api-db2/db2-connector-delete-row-outputs.png)

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler, eylemler ve konektörün Swagger dosyasında açıklandığı gibi sınırlar gibi bu bağlayıcı hakkında daha fazla teknik ayrıntı için [bağlayıcının başvuru sayfasına](https://docs.microsoft.com/connectors/db2/)bakın.

> [!NOTE]
> [Bir entegrasyon hizmeti ortamındaki (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)mantık uygulamaları için, bu bağlayıcının İmKB etiketli sürümü bunun yerine [İmKB ileti sınırlarını](../logic-apps/logic-apps-limits-and-config.md#message-size-limits) kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
