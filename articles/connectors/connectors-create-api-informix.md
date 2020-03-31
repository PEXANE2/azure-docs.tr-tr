---
title: IBM Informix veritabanına bağlanın
description: Azure Mantık Uygulamaları'nı kullanarak IBM Informix'te depolanan kaynakları yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
author: gplarsen
ms.author: plarsen
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 01/07/2020
tags: connectors
ms.openlocfilehash: dccb715c974037b4e3080f3e51576feae34c03df
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76757977"
---
# <a name="manage-ibm-informix-database-resources-by-using-azure-logic-apps"></a>Azure Logic Apps'ı kullanarak IBM Informix veritabanı kaynaklarını yönetme

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve [Informix konektörü](/connectors/informix/)ile bir IBM Informix veritabanında kaynakları yöneten otomatik görevler ve iş akışları oluşturabilirsiniz. Bu bağlayıcı, azure sanallaştırmasında çalışan IBM Informix windows veritabanları ve [şirket içi veri ağ geçidini](../logic-apps/logic-apps-gateway-connection.md)kullandığınızda şirket içi veritabanları gibi bulut tabanlı veritabanları da dahil olmak üzere, bir TCP/IP ağındaki uzak Informix sunucu bilgisayarlarıyla iletişim kuran bir Microsoft istemcisi içerir. Dağıtılmış İlişkisel Veritabanı Mimarisi (DRDA) istemci bağlantılarını destekleyecek şekilde yapılandırılırlarsa, bu Informix platformlarına ve sürümlerine bağlanabilirsiniz:

* IBM Informix 12.1
* IBM Informix 11.7

Bu konu, veritabanı işlemlerini işlemek için bir mantık uygulamasında bağlayıcıyı nasıl kullanacağınızı gösterir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Şirket içi veritabanları için, şirket içi veri ağ geçidini yerel bir bilgisayara [indirip yükleyin](../logic-apps/logic-apps-gateway-install.md) ve ardından [Azure portalında bir Azure veri ağ geçidi kaynağı oluşturun.](../logic-apps/logic-apps-gateway-connection.md)

* Informix veritabanınıza erişmeniz gereken mantık uygulaması. Bu bağlayıcı yalnızca eylemler sağlar, bu nedenle mantık uygulamanız zaten bir tetikleyiciyle başlamalıdır, [örneğin, Yineleme](../connectors/connectors-native-recurrence.md)tetikleyicisi. 

## <a name="add-an-informix-action"></a>Informix aksiyonu ekleme

1. Azure [portalında,](https://portal.azure.com)mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Informix eylemini eklemek istediğiniz adımın altında **Yeni adımı**seçin.

   Varolan adımlar arasında eylem eklemek için farenizi bağlanan okun üzerine taşıyın. Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.

1. Arama kutusuna filtreniz olarak girin. `informix` Eylemler listesinden, örneğin istediğiniz eylemi seçin:

   ![Çalıştırmak için Informix eylemini seçin](./media/connectors-create-api-informix/select-informix-connector-action.png)

   Bağlayıcı, ilgili veritabanı işlemlerini çalıştıran şu eylemleri sağlar:

   * Tabloları alma - İfade kullanarak `CALL` veritabanı tablolarını listele
   * Satır alma - `SELECT *` Bir deyim kullanarak tüm satırları okuyun
   * Satır alma - `SELECT WHERE` Bir deyim kullanarak satır okuma
   * `INSERT` Deyim kullanarak satır ekleme
   * `UPDATE` Bir deyim kullanarak satırı düzenleme
   * `DELETE` İfade kullanarak satırı silme

1. Informix veritabanınız için bağlantı ayrıntıları sağlamanız [istenirse, bağlantıyı oluşturmak için adımları](#create-connection)izleyin ve bir sonraki adımla devam edin.

1. Seçtiğiniz eylem için bilgileri sağlayın:

   | Eylem | Açıklama | Özellikler ve açıklamalar |
   |--------|-------------|-----------------------------|
   | **Tabloları alın** | Bir Informix CALL deyimi çalıştırarak veritabanı tablolarını listeleyin. | None |
   | **Satırları alın** | Bir Informix `SELECT *` deyimi çalıştırarak belirtilen tablodaki tüm satırları getirin. | **Tablo adı**: İstediğindiğiniz Informix tablosunun adı <p><p>Bu eyleme başka özellikler eklemek **için, bunları yeni parametre** ekle listesinden seçin. Daha fazla bilgi için [bağlayıcının başvuru konusuna](/connectors/informix/)bakın. |
   | **Satır al** | Bir Informix `SELECT WHERE` deyimi çalıştırarak belirtilen tablodan bir satır getirin. | - **Tablo adı**: İstediğindiğiniz Informix tablosunun adı <br>- **Satır Kimliği**: Satır için benzersiz kimlik, örneğin,`9999` |
   | **Satır ekle** | Bir Informix `INSERT` deyimi çalıştırarak belirtilen Informix tablosuna bir satır ekleyin. | - **Tablo adı**: İstediğindiğiniz Informix tablosunun adı <br>- **item**: Eklenecek değerlere sahip satır |
   | **Satırı güncelleştir** | Bir Informix `UPDATE` deyimi çalıştırarak belirtilen Informix tablosundaki bir satırı değiştirin. | - **Tablo adı**: İstediğindiğiniz Informix tablosunun adı <br>- **Satır Kimliği**: Satırın güncelleştirilen benzersiz kimliği, örneğin,`9999` <br>- **Satır**: Güncelleştirilmiş değerlere sahip satır, örneğin,`102` |
   | **Satırı silme** | Bir Informix `DELETE` deyimi çalıştırarak belirtilen Informix tablosundan bir satır kaldırın. | - **Tablo adı**: İstediğindiğiniz Informix tablosunun adı <br>- **Satır Kimliği**: Satırın silinecek benzersiz kimliği, örneğin,`9999` |
   ||||

1. Mantıksal uygulamanızı kaydedin. Şimdi, ya [mantık uygulamanızı test edin](#test-logic-app) ya da mantık uygulamanızı oluşturmaya devam edin.

<a name="create-connection"></a>

## <a name="connect-to-informix"></a>Informix'e bağlanın

1. Mantık uygulamanız şirket içi bir veritabanına bağlanıyorsa, **şirket içi veri ağ geçidi üzerinden Bağlan'ı**seçin.

1. Bu bağlantı bilgilerini sağlayın ve ardından **Oluştur'u**seçin.

   | Özellik | JSON özelliği | Gerekli | Örnek değer | Açıklama |
   |----------|---------------|----------|---------------|-------------|
   | Bağlantı adı | `name` | Evet | `informix-demo-connection` | Informix veritabanınıza bağlantı için kullanılacak ad |
   | Sunucu | `server` | Evet | - Bulut:`informixdemo.cloudapp.net:9089` <br>- Tesis içi:`informixdemo:9089` | IPv4 veya IPv6 formatında olan TCP/IP adresi veya diğer ad, ardından bir üst üste ve TCP/IP bağlantı noktası numarası |
   | Database | `database` | Evet | `nwind` | DRDA İlişkisel Veritabanı Adı (RDBNAM) veya Informix veritabanı adı (dbname). Informix 128 baytlık bir dize kabul eder. |
   | Kimlik doğrulaması | `authentication` | Yalnızca şirket içi | **Temel** veya **Windows** (kerberos) | Informix veritabanınız tarafından gerekli olan kimlik doğrulama türü. Bu özellik yalnızca şirket **içi veri ağ geçidi üzerinden Bağlan'ı**seçtiğinizde görünür. |
   | Kullanıcı adı | `username` | Hayır | <*veritabanı-kullanıcı adı*> | Veritabanı için bir kullanıcı adı |
   | Parola | `password` | Hayır | <*veritabanı-şifre*> | Veritabanı için bir parola |
   | Ağ geçidi | `gateway` | Yalnızca şirket içi | - *Azure aboneliği<*> <br>- <*Azure-şirket içi veri ağ geçidi-kaynak*> | Azure portalında oluşturduğunuz şirket içi veri ağ geçidi için Azure aboneliği ve Azure kaynak adı. **Ağ Geçidi** özelliği ve alt özellikleri yalnızca şirket içi veri ağ geçidi **üzerinden Bağlan'ı**seçtiğinizde görünür. |
   ||||||

   Örnek:

   * **Bulut veritabanı**

     ![Bulut veritabanı bağlantı bilgileri](./media/connectors-create-api-informix/informix-cloud-connection.png)

   * **Şirket içi veritabanı**

     ![Şirket içi veritabanı bağlantı bilgileri](./media/connectors-create-api-informix/informix-on-premises-connection.png)

1. Mantıksal uygulamanızı kaydedin.

<a name="test-logic-app"></a>

## <a name="test-your-logic-app"></a>Mantık uygulamanızı test edin

1. Logic App Designer araç çubuğunda **Çalıştır'ı**seçin. Mantık uygulamanız çalıştırdıktan sonra, bu çalıştırmanın çıktılarını görüntüleyebilirsiniz.

1. Mantık uygulamanızın menüsünden **Genel Bakış'ı**seçin. Özet > **Çalışır geçmişi**altında **Summary**genel bakış bölmesinde, en son çalıştırmayı seçin.

1. **Mantık uygulaması altında çalıştır**, Ayrıntıları **Çalıştır'ı**seçin.

1. Eylemler listesinden, görüntülemek istediğiniz çıktıları içeren eylemi seçin( örneğin, **Get_tables.**

   Eylem başarılı olduysa, **Durum** özelliği **Başarılı**olarak işaretlenir.

1. Girişleri görüntülemek **için, GirişLer Bağlantısı**altında, URL bağlantısını seçin. **Çıktıları** görüntülemek için Çıktılar Bağlantısı bağlantısı altında URL bağlantısını seçin. Aşağıda bazı örnek çıktılar verilmiştir:

   * **Get_tables** tabloların listesini gösterir:

     !["Tablo ları Al" eyleminden çıktılar](./media/connectors-create-api-informix/InformixconnectorGetTablesLogicAppRunOutputs.png)

   * **Get_rows** satır listesini gösterir:

     !["Satır al" eyleminden çıktılar](./media/connectors-create-api-informix/InformixconnectorGetRowsOutputs.png)

   * **Get_row** belirtilen satırı gösterir:

     !["Satır al" eyleminden çıktılar](./media/connectors-create-api-informix/InformixconnectorGetRowOutputs.png)

   * **Insert_row** yeni satırı gösterir:

     !["Satır ekle" eyleminden çıktılar](./media/connectors-create-api-informix/InformixconnectorInsertRowOutputs.png)

   * **Update_row** güncelleştirilmiş satırı gösterir:

     !["Satırı güncelleştirme" eyleminden çıktılar](./media/connectors-create-api-informix/InformixconnectorUpdateRowOutputs.png)

   * **Delete_row** silinen satırı gösterir:

     !["Satırı sil" eyleminden çıktılar](./media/connectors-create-api-informix/InformixconnectorDeleteRowOutputs.png)

## <a name="connector-specific-details"></a>Bağlayıcıya özel ayrıntılar

Bağlayıcının Swagger açıklamasıyla açıklanan tetikleyiciler, eylemler ve sınırlar hakkındaki teknik ayrıntılar için [bağlayıcının başvuru sayfasını](/connectors/informix/)inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](apis-list.md) hakkında bilgi edinin
