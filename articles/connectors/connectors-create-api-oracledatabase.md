---
title: Oracle Veritabanına Bağlan
description: Oracle Database REST API'leri ve Azure Logic Apps ile kayıt ekleme ve yönetme
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 03/29/2017
tags: connectors
ms.openlocfilehash: 99abd48bde97c2a2e085688cdfbb365e5e4cfd56
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789435"
---
# <a name="get-started-with-the-oracle-database-connector"></a>Oracle Database bağlayıcısı ile başlayın

Oracle Veritabanı bağlayıcısını kullanarak, varolan veritabanınızdaki verileri kullanan kuruluş iş akışları oluşturursunuz. Bu bağlayıcı, şirket içinde bir Oracle Veritabanı'na veya Oracle Veritabanı yüklü bir Azure sanal makinesine bağlanabilir. Bu bağlayıcı ile şunları yapabilirsiniz:

* Müşteri veritabanına yeni bir müşteri ekleyerek veya siparişler veritabanındaki siparişi güncelleştirerek iş akışınızı oluşturun.
* Bir veri satırı almak, yeni bir satır eklemek ve hatta silmek için eylemleri kullanın. Örneğin, Dynamics CRM Online'da (tetikleyici) bir kayıt oluşturulduğunda, Oracle Veritabanına (eylem) bir satır ekleyin. 

Bu makalede, bir mantık uygulamasında Oracle Veritabanı bağlayıcısının nasıl kullanılacağı gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

* Desteklenen Oracle sürümleri: 
    * Oracle 9 ve üstü
    * Oracle istemci yazılımı 8.1.7 ve üstü

* Şirket içi veri ağ geçidini yükleyin. [Mantık uygulamalarından şirket içi verilere bağlanAdımlar](../logic-apps/logic-apps-gateway-connection.md) listeler. Ağ geçidi, şirket içinde bir Oracle Veritabanına veya Oracle DB yüklü bir Azure VM'ye bağlanmak için gereklidir. 

    > [!NOTE]
    > Şirket içi veri ağ geçidi bir köprü görevi görür ve şirket içi veriler (bulutta olmayan veriler) ile mantıksal uygulamalarınız arasında güvenli bir veri aktarımı sağlar. Aynı ağ geçidi birden çok hizmet ve birden çok veri kaynağıyla kullanılabilir.Bu nedenle, ağ geçidini yalnızca bir kez yüklemeniz gerekebilir.

* Oracle İstemciyi şirket içi veri ağ geçidini yüklediğiniz makineye yükleyin.Oracle'dan .NET için 64 bit Oracle Veri Sağlayıcısı'nı yüklediğinizden emin olun:  

  [Windows x64 için 64 bit ODAC 12c Sürüm 4 (12.1.0.2.4)](https://www.oracle.com/technetwork/database/windows/downloads/index-090165.html)

    > [!TIP]
    > Oracle istemcisi yüklenmezse, bağlantıyı oluşturmaya veya kullanmaya çalıştığınızda bir hata oluşur. Bu makaledeki yaygın hataları görün.


## <a name="add-the-connector"></a>Konektörü ekleme

> [!IMPORTANT]
> Bu bağlayıcının tetikleyicisi yoktur. Sadece eylemleri var. Bu nedenle, mantık uygulamanızı oluşturduğunuzda, Zamanlama - **Yineleme veya** **İstek / Yanıt - Yanıt**gibi mantık uygulamanızı başlatmak için başka bir tetikleyici ekleyin. 

1. Azure [portalında](https://portal.azure.com)boş bir mantık uygulaması oluşturun.

2. Mantık uygulamanızın başında **İstek / Yanıt - İstek** tetikleyicisini seçin: 

    ![](./media/connectors-create-api-oracledatabase/request-trigger.png)

3. **Kaydet'i**seçin. Kaydettiğinizde, bir istek URL'si otomatik olarak oluşturulur. 

4. **Yeni adım**’ı ve **Eylem ekle**’yi seçin. Kullanılabilir `oracle` eylemleri görmek için yazın: 

    ![](./media/connectors-create-api-oracledatabase/oracledb-actions.png)

    > [!TIP]
    > Bu, herhangi bir bağlayıcı için kullanılabilir tetikleyicileri ve eylemleri görmenin en hızlı yoludur. Bağlayıcı adının bir kısmını yazın, `oracle`örneğin. Tasarımcı tetikleyicileri ve eylemleri listeler. 

5. Oracle Database - Get **row**gibi eylemlerden birini seçin. **Şirket içi veri ağ geçidi üzerinden Bağlan'ı**seçin. Oracle sunucu adını, kimlik doğrulama yöntemini, kullanıcı adını, parolayı girin ve ağ geçidini seçin:

    ![](./media/connectors-create-api-oracledatabase/create-oracle-connection.png)

6. Bağlandıktan sonra listeden bir tablo seçin ve tablonuza satır kimliği girin. Masanın tanımlayıcısını bilmelisin. Bilmiyorsanız, Oracle DB yöneticinize başvurun ve çıktıyı `select * from yourTableName`'dan alın. Bu, devam etmek için gereken tanımlanabilir bilgileri sağlar.

    Aşağıdaki örnekte, iş verileri Bir İnsan Kaynakları veritabanından döndürülmektedir: 

    ![](./media/connectors-create-api-oracledatabase/table-rowid.png)

7. Bir sonraki adımda, iş akışınızı oluşturmak için diğer bağlayıcılardan herhangi birini kullanabilirsiniz. Oracle'dan veri almayı test etmek istiyorsanız, office 365 veya Gmail gibi e-posta gönderim bağlayıcılarından birini kullanarak Oracle verilerini içeren bir e-posta gönderin. E-postanızı `Subject` ve e-postanızı `Body` oluşturmak için Oracle tablosundaki dinamik belirteçleri kullanın:

    ![](./media/connectors-create-api-oracledatabase/oracle-send-email.png)

8. Mantık uygulamanızı **kaydedin** ve ardından **Çalıştır'ı**seçin. Tasarımcıyı kapatın ve durum için çalışan geçmişe bakın. Başarısız olursa, başarısız ileti satırını seçin. Tasarımcı açılır ve hangi adımın başarısız olduğunu gösterir ve hata bilgilerini de gösterir. Başarılı olursa, eklediğiniz bilgileri içeren bir e-posta almanız gerekir.


### <a name="workflow-ideas"></a>İş akışı fikirleri

* #oracle hashtag'ini izlemek ve tweetleri sorgulanabilmeleri ve diğer uygulamalarda kullanılabilleri için bir veritabanına koymak istiyorsunuz. Bir mantık uygulamasında, `Twitter - When a new tweet is posted` tetikleyiciyi ekleyin ve **#oracle** hashtag'ini girin. `Oracle Database - Insert row` Ardından, eylemi ekleyin ve tablonuzu seçin:

    ![](./media/connectors-create-api-oracledatabase/twitter-oracledb.png)

* İletiler Servis Veri Servisi kuyruğuna gönderilir. Bu iletileri alıp bir veritabanına koymak istiyorsunuz. Bir mantık uygulamasında, `Service Bus - when a message is received in a queue` tetikleyiciyi ekleyin ve sırayı seçin. `Oracle Database - Insert row` Ardından, eylemi ekleyin ve tablonuzu seçin:

    ![](./media/connectors-create-api-oracledatabase/sbqueue-oracledb.png)

## <a name="common-errors"></a>Sık karşılaşılan hatalar

#### <a name="error-cannot-reach-the-gateway"></a>**Hata**: Ağ Geçidine ulaşamıyorum

**Neden**: Şirket içi veri ağ geçidi buluta bağlanamaz. 

**Azaltma**: Ağ geçidinizin kurduğunuz şirket içi makinede çalıştığından ve internete bağlanabildiğinizden emin olun.Kapatılabilecek veya uykuda uyuyabilen bir bilgisayara ağ geçidini yüklememenizi öneririz.Şirket içi veri ağ geçidi hizmetini (PBIEgwService) yeniden başlatabilirsiniz.

#### <a name="error-the-provider-being-used-is-deprecated-systemdataoracleclient-requires-oracle-client-software-version-817-or-greater-see-httpsgomicrosoftcomfwlinkplinkid272376-to-install-the-official-provider"></a>**Hata**: Kullanılan sağlayıcı amortismana tabidir: 'System.Data.OracleClient Oracle istemci yazılım sürümü 8.1.7 veya daha fazla gerektirir.'. Resmi [https://go.microsoft.com/fwlink/p/?LinkID=272376](https://go.microsoft.com/fwlink/p/?LinkID=272376) sağlayıcıyı yüklemek için bkz.

**Neden**: Oracle istemcisi SDK, şirket içi veri ağ geçidinin çalıştığı makineye yüklenmez.  

**Çözünürlük**: Oracle istemcisi SDK'yı şirket içi veri ağ geçidiyle aynı bilgisayara indirin ve kurun.

#### <a name="error-table-tablename-does-not-define-any-key-columns"></a>**Hata**: Tablo '[Tablo adı]' anahtar sütunları tanımlamaz

**Neden**: Tabloda birincil anahtar yoktur.  

**Çözünürlük**: Oracle Database bağlayıcısı, birincil anahtar sütunu olan bir tablonun kullanılmasını gerektirir.

#### <a name="currently-not-supported"></a>Şu anda desteklenmiyor

* Görünümler 
* Kompozit tuşlu herhangi bir tablo
* Tablolarda iç içe nesne türleri
 
## <a name="connector-specific-details"></a>Bağlayıcıya özel ayrıntılar

Swagger'da tanımlanan tetikleyicileri ve eylemleri görüntüleyin ve [bağlayıcı ayrıntılarında](/connectors/oracle/)da herhangi bir sınır görün. 

## <a name="get-some-help"></a>Yardım alın

[Azure Logic Apps forumu,](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) soru sormak, soruları yanıtlamak ve diğer Logic Apps kullanıcılarının ne yaptığını görmek için harika bir yerdir. 

Fikirlerinizi göndererek ve oy vererek Mantık Uygulamaları'nı [https://aka.ms/logicapps-wish](https://aka.ms/logicapps-wish)ve bağlayıcılarını geliştirmeye yardımcı olabilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar
[Bir mantık uygulaması oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md)ve [API'ler listesinde](apis-list.md)Logic Apps'taki kullanılabilir bağlayıcıları keşfedin.
