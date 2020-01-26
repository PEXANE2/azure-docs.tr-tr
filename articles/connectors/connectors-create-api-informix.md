---
title: IBM Informix veritabanına Bağlan
description: Azure Logic Apps kullanarak IBM Informix ' de depolanan kaynakları yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
author: gplarsen
ms.author: plarsen
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 01/07/2020
tags: connectors
ms.openlocfilehash: dccb715c974037b4e3080f3e51576feae34c03df
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2020
ms.locfileid: "76757977"
---
# <a name="manage-ibm-informix-database-resources-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak IBM Informix veritabanı kaynaklarını yönetme

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve [Informix Bağlayıcısı](/connectors/informix/)sayesinde, bir IBM Informix veritabanında kaynakları yöneten otomatik görevler ve iş akışları oluşturabilirsiniz. Bu bağlayıcı, Şirket [içi veri ağ geçidini](../logic-apps/logic-apps-gateway-connection.md)kullanırken Azure sanallaştırma ve şirket içi veritabanlarında çalıştırılan WINDOWS Için IBM Informix gibi bulut tabanlı veritabanları da dahil olmak üzere bir TCP/IP ağı üzerinden uzak Informix sunucu bilgisayarlarıyla iletişim kuran bir Microsoft istemcisi içerir. Bu Informix platformları ve sürümlerine, dağıtılmış Ilişkisel veritabanı mimarisi (DRDA) istemci bağlantılarını destekleyecek şekilde yapılandırıldıysa bağlanabilirsiniz:

* IBM Informix 12.1
* IBM Informix 11,7

Bu konuda, bir mantıksal uygulamada, veritabanı işlemlerini işlemek için bağlayıcının nasıl kullanılacağı gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Şirket içi veritabanları için, Şirket [içi veri ağ geçidini](../logic-apps/logic-apps-gateway-install.md) yerel bir bilgisayara indirip yükleyin ve ardından [Azure Portal bir Azure Data Gateway kaynağı oluşturun](../logic-apps/logic-apps-gateway-connection.md).

* Informix veritabanınıza erişmeniz gereken mantıksal uygulama. Bu bağlayıcı yalnızca eylemleri sağlar, bu nedenle mantıksal uygulamanızın bir tetikleyiciyle başlaması gerekir, örneğin [yineleme tetikleyicisi](../connectors/connectors-native-recurrence.md). 

## <a name="add-an-informix-action"></a>Informix eylemi ekleme

1. [Azure Portal](https://portal.azure.com), zaten açık değilse mantıksal uygulama Tasarımcısı 'nda mantıksal uygulamanızı açın.

1. Informix eylemini eklemek istediğiniz adım altında **yeni adım**' ı seçin.

   Varolan adımlar arasında bir eylem eklemek için farenizi bağlantı oku üzerine taşıyın. Görüntülenen artı işaretini ( **+** ) seçin ve ardından **Eylem Ekle**' yi seçin.

1. Arama kutusuna filtreniz olarak `informix` girin. Eylemler listesinden istediğiniz eylemi seçin, örneğin:

   ![Çalıştırılacak Informix eylemini seçin](./media/connectors-create-api-informix/select-informix-connector-action.png)

   Bağlayıcı, karşılık gelen veritabanı işlemlerini çalıştıran bu eylemleri sağlar:

   * Tabloları al-`CALL` bir ifade kullanarak veritabanı tablolarını listeleme
   * Satırları al-`SELECT *` bir ifade kullanarak tüm satırları oku
   * Satırı al-`SELECT WHERE` ifadesini kullanarak bir satırı okuyun
   * `INSERT` ifadesini kullanarak satır ekleme
   * `UPDATE` ifadesini kullanarak bir satırı düzenleme
   * `DELETE` ifadesini kullanarak bir satırı silme

1. Informix veritabanınız için bağlantı ayrıntıları sağlamanız istenirse, [bağlantıyı oluşturma adımlarını](#create-connection)izleyin ve ardından sonraki adımla devam edin.

1. Seçtiğiniz eyleme ilişkin bilgileri belirtin:

   | Eylem | Açıklama | Özellikler ve açıklamalar |
   |--------|-------------|-----------------------------|
   | **Tabloları al** | Bir Informix çağrı ekstresi çalıştırarak veritabanı tablolarını listeleyin. | Hiçbiri |
   | **Satırları al** | Bir Informix `SELECT *` ifadesini çalıştırarak belirtilen tablodaki tüm satırları getir. | **Tablo adı**: Istediğiniz Informix tablosunun adı <p><p>Bu eyleme başka özellikler eklemek için, **yeni parametre Ekle** listesinden bunları seçin. Daha fazla bilgi için [bağlayıcının başvuru konusuna](/connectors/informix/)bakın. |
   | **Satırı al** | Bir Informix `SELECT WHERE` ifadesini çalıştırarak, belirtilen tablodan bir satır getirir. | - **tablo adı**: Istediğiniz Informix tablosunun adı <br>- **satır kimliği**: SATıRıN benzersiz kimliği, örneğin, `9999` |
   | **Satır Ekle** | Bir Informix `INSERT` ifadesini çalıştırarak belirtilen Informix tablosuna bir satır ekleyin. | - **tablo adı**: Istediğiniz Informix tablosunun adı <br>- **öğesi**: eklenecek değerler içeren satır |
   | **Satırı Güncelleştir** | Bir Informix `UPDATE` ifadesini çalıştırarak belirtilen Informix tablosundaki bir satırı değiştirin. | - **tablo adı**: Istediğiniz Informix tablosunun adı <br>- **satır kimliği**: güncelleştirilecek SATıRıN benzersiz kimliği, örneğin, `9999` <br>- **satır**: güncelleştirilmiş değerleri içeren satır, örneğin, `102` |
   | **Satırı Sil** | Bir Informix `DELETE` ifadesini çalıştırarak belirtilen Informix tablosundan bir satırı kaldırın. | - **tablo adı**: Istediğiniz Informix tablosunun adı <br>- **satır kimliği**: silinecek SATıRıN benzersiz kimliği, örneğin, `9999` |
   ||||

1. Mantıksal uygulamanızı kaydedin. Şimdi [mantıksal uygulamanızı test](#test-logic-app) edin ya da mantıksal uygulamanızı oluşturmaya devam edin.

<a name="create-connection"></a>

## <a name="connect-to-informix"></a>Informix ' ye bağlanma

1. Mantıksal uygulamanız şirket içi bir veritabanına bağlanıyorsa Şirket **içi veri ağ geçidi üzerinden Bağlan**' ı seçin.

1. Bu bağlantı bilgilerini girip **Oluştur**' u seçin.

   | Özellik | JSON özelliği | Gereklidir | Örnek değer | Açıklama |
   |----------|---------------|----------|---------------|-------------|
   | Bağlantı adı | `name` | Evet | `informix-demo-connection` | Informix veritabanınıza bağlantı için kullanılacak ad |
   | Sunucu | `server` | Evet | -Bulut: `informixdemo.cloudapp.net:9089` <br>-Şirket içi: `informixdemo:9089` | IPv4 veya IPv6 biçiminde olan TCP/IP adresi veya diğer ad, ardından iki nokta üst üste ve TCP/IP bağlantı noktası numarası |
   | Veritabanı | `database` | Evet | `nwind` | DRDA Ilişkisel veritabanı adı (RDBNAM) veya Informix veritabanı adı (dbname). Informix bir 128 baytlık dizeyi kabul eder. |
   | Kimlik Doğrulaması | `authentication` | Yalnızca şirket içi | **Temel** veya **Windows** (Kerberos) | Informix veritabanınız için gereken kimlik doğrulama türü. Bu özellik yalnızca şirket **içi veri ağ geçidi üzerinden Bağlan '** ı seçtiğinizde görünür. |
   | Kullanıcı adı | `username` | Hayır | <*veritabanı-Kullanıcı adı*> | Veritabanı için Kullanıcı adı |
   | Parola | `password` | Hayır | <*Veritabanı-parola*> | Veritabanı için bir parola |
   | Ağ Geçidi | `gateway` | Yalnızca şirket içi | -<*Azure-subscription*> <br>-<*Azure-şirket içi-veri ağ geçidi-kaynak*> | Azure portal oluşturduğunuz şirket içi veri ağ geçidi için Azure aboneliği ve Azure Kaynak adı. **Ağ geçidi** özelliği ve alt özellikler yalnızca şirket **içi veri ağ geçidi üzerinden Bağlan '** ı seçtiğinizde görünür. |
   ||||||

   Örneğin:

   * **Bulut veritabanı**

     ![Bulut veritabanı bağlantı bilgileri](./media/connectors-create-api-informix/informix-cloud-connection.png)

   * **Şirket içi veritabanı**

     ![Şirket içi veritabanı bağlantı bilgileri](./media/connectors-create-api-informix/informix-on-premises-connection.png)

1. Mantıksal uygulamanızı kaydedin.

<a name="test-logic-app"></a>

## <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test etme

1. Mantıksal uygulama Tasarımcısı araç çubuğunda **Çalıştır**' ı seçin. Mantıksal uygulamanız çalıştıktan sonra, Bu çalıştırmaların çıkışlarını görüntüleyebilirsiniz.

1. Mantıksal uygulamanızın menüsünde **genel bakış**' ı seçin. Genel Bakış bölmesinde, **özet** > çalışma **geçmişi**altında, en son Çalıştır ' ı seçin.

1. **Mantıksal uygulama çalıştırma**altında, **Çalıştır ayrıntıları**' nı seçin.

1. Eylemler listesinden, görüntülemek istediğiniz çıktıları içeren eylemi seçin, örneğin **Get_tables**.

   Eylem başarılı olduysa, **Status** özelliği **başarılı**olarak işaretlenir.

1. Girişleri görüntülemek için, **girişler bağlantısı**altında URL bağlantısını seçin. Çıkışları görüntülemek için, **çıktılar bağlantı** BAĞLANTıSı altında URL bağlantısını seçin. Örnek çıktıları aşağıda verilmiştir:

   * **Get_tables** , tabloların bir listesini gösterir:

     !["Tabloları al" eyleminden çıkış](./media/connectors-create-api-informix/InformixconnectorGetTablesLogicAppRunOutputs.png)

   * **Get_rows** satırların bir listesini gösterir:

     !["Satırları al" eyleminden çıkış](./media/connectors-create-api-informix/InformixconnectorGetRowsOutputs.png)

   * **Get_row** belirtilen satırı gösterir:

     !["Satırı al" eyleminden çıkış](./media/connectors-create-api-informix/InformixconnectorGetRowOutputs.png)

   * **Insert_row** yeni satırı gösterir:

     !["Satır ekle" eyleminden çıkış](./media/connectors-create-api-informix/InformixconnectorInsertRowOutputs.png)

   * **Update_row** güncelleştirilmiş satırı gösterir:

     !["Satırı güncelleştirme" eyleminden çıkış](./media/connectors-create-api-informix/InformixconnectorUpdateRowOutputs.png)

   * **Delete_row** silinen satırı gösterir:

     !["Satırı sil" eyleminden çıkış](./media/connectors-create-api-informix/InformixconnectorDeleteRowOutputs.png)

## <a name="connector-specific-details"></a>Bağlayıcıya özgü ayrıntılar

Bağlayıcının Swagger açıklaması tarafından tanımlanan Tetikleyiciler, Eylemler ve limitlerle ilgili teknik ayrıntılar için [bağlayıcının başvuru sayfasını](/connectors/informix/)gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](apis-list.md) hakkında bilgi edinin
