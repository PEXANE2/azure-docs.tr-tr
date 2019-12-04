---
title: IBM Informix veritabanına Bağlan
description: IBM Informix REST API 'Leri ve Azure Logic Apps kaynakları yönetme
services: logic-apps
ms.suite: integration
author: gplarsen
ms.author: plarsen
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 09/26/2016
tags: connectors
ms.openlocfilehash: d6f768bc76d19c0aa21a245c008a4b05588f8f43
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74789737"
---
# <a name="get-started-with-the-informix-connector"></a>Informix bağlayıcısını kullanmaya başlama
Informix için Microsoft Bağlayıcısı, bir IBM Informix veritabanında depolanan kaynaklara Logic Apps bağlanır. Informix Bağlayıcısı, bir TCP/IP ağı üzerinden uzak Informix sunucu bilgisayarlarıyla iletişim kurmak için bir Microsoft istemcisi içerir. Bu, Azure sanallaştırmaya çalışan Windows için IBM Informix ve şirket içi veri ağ geçidini kullanan şirket içi veritabanları gibi bulut veritabanlarını içerir. Bkz. IBM Informix platformları ve sürümlerinin [desteklenen listesi](connectors-create-api-informix.md#supported-informix-platforms-and-versions) (Bu konuda).

Bağlayıcı aşağıdaki veritabanı işlemlerini destekler:

* Veritabanı tablolarını listeleme
* Seç kullanarak bir satırı okuma
* Seç kullanarak tüm satırları oku
* INSERT kullanarak bir satır ekleme
* GÜNCELLEŞTIRME kullanarak bir satırı değiştirme
* SIL kullanarak bir satırı kaldırma

Bu konuda, bir mantıksal uygulamada, veritabanı işlemlerini işlemek için bağlayıcının nasıl kullanılacağı gösterilmektedir.

Logic Apps hakkında daha fazla bilgi edinmek için bkz. [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="available-actions"></a>Kullanılabilir eylemler
Bu bağlayıcı aşağıdaki mantıksal uygulama eylemlerini destekler:

* Getables
* GetRow
* Almak
* InsertRow
* UpdateRow
* DeleteRow

## <a name="list-tables"></a>Liste tabloları
Herhangi bir işlem için mantıksal uygulama oluşturmak, Microsoft Azure portal gerçekleştirilen birçok adımdan oluşur.

Mantıksal uygulama içinde, bir Informix veritabanındaki tabloları listelemek için bir eylem ekleyebilirsiniz. Bu eylem, bağlayıcının `CALL SYSIBM.SQLTABLES`gibi bir Informix şema ifadesini işlemesini söyler.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. **Azure başlangıç panosunda** **+** (artı işareti), **Web ve mobil**ve ardından **mantıksal uygulama**' yı seçin.
2. `InformixgetTables`, **abonelik**, **kaynak grubu**, **konum**ve **App Service planı**gibi bir **ad**girin. **Panoya sabitle**' yi seçin ve ardından **Oluştur**' u seçin.

### <a name="add-a-trigger-and-action"></a>Tetikleyici ve eylem ekleme
1. **Logic Apps tasarımcısında**, **Şablonlar** listesinde **boş logicapp** ' i seçin.
2. **Tetikleyiciler** listesinde **yinelenme**' yi seçin. 
3. **Yineleme** tetikleyicisinde **Düzenle**' yi seçin, **sıklığı** seçin ' i seçin ve ardından **7**yazmak için **Interval** ' ı seçin.  
4. **+ Yeni adım** kutusunu seçin ve ardından **Eylem Ekle**' yi seçin.
5. **Eylemler** listesinde, **daha fazla eylem ara** düzenleme kutusuna **Informix** yazın ve ardından **Informix-tabloları al (Önizleme)** öğesini seçin.
   
   ![](./media/connectors-create-api-informix/InformixconnectorActions.png)  
6. **Şirket içi veri ağ geçidi aracılığıyla bağlanmayı**etkinleştirmek Için **Informix-tabloları al** Yapılandırma bölmesinde **onay kutusunu** seçin. Ayarların buluttan şirket içine değişdiğine dikkat edin.
   
   * **Sunucu**için, adres veya diğer ad iki nokta üst üste bağlantı noktası numarası biçiminde tür değeri. Örneğin `ibmserver01:9089`yazın.
   * **Veritabanı**için tür değeri. Örneğin `nwind`yazın.
   * **Kimlik doğrulaması**için değer seçin. Örneğin, **temel**öğesini seçin.
   * **Kullanıcı adı**için tür değeri. Örneğin `informix`yazın.
   * **Parola**için değer yazın. Örneğin `Password1`yazın.
   * **Ağ geçidi**için değer seçin. Örneğin, **datagateway01**öğesini seçin.
7. **Oluştur**' u ve ardından **Kaydet**' i seçin. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)
8. **Informixgettables** dikey penceresinde, **Özet**altındaki **Tüm çalıştırmalar** listesinde, ilk listelenen öğeyi (en son çalıştırma) seçin.
9. **Mantıksal uygulama çalıştırma** dikey penceresinde **ayrıntıları Çalıştır**' ı seçin. **Eylem** listesi içinde **Get_tables**' yi seçin. **Durum**Için, **başarılı**olması gereken değere bakın. Girişleri görüntülemek için **girişler bağlantısını** seçin. **Çıktılar bağlantısını**seçin ve çıkışları görüntüleyin; bir tablo listesi içermelidir.
   
   ![](./media/connectors-create-api-informix/InformixconnectorGetTablesLogicAppRunOutputs.png)

## <a name="create-the-connections"></a>Bağlantıları oluşturma
Bu bağlayıcı, aşağıdaki bağlantı özelliklerini kullanarak şirket içinde ve bulutta bulunan veritabanı bağlantılarını destekler. 

| Özellik | Açıklama |
| --- | --- |
| sunucu |Gereklidir. TCP/IP adresini veya diğer adı temsil eden bir dize değerini, IPv4 veya IPv6 biçiminde, ardından bir TCP/IP bağlantı noktası numarası ile (iki nokta-sınırlı) kabul eder. |
| veritabanı |Gereklidir. Bir DRDA Ilişkisel veritabanı adını (RDBNAM) temsil eden bir dize değeri kabul eder. Informix bir 128 baytlık dizeyi kabul eder (veritabanı IBM Informix veritabanı adı (dbname) olarak bilinir). |
| kimlik doğrulaması |İsteğe bağlı. Temel veya Windows (Kerberos) liste öğesi değerini kabul eder. |
| kullanıcı adı |Gereklidir. Bir dize değeri kabul eder. |
| password |Gereklidir. Bir dize değeri kabul eder. |
| Geçidinde |Gereklidir. Depolama grubu içinde Logic Apps için tanımlanan şirket içi veri ağ geçidini temsil eden bir liste öğesi değeri kabul eder. |

## <a name="create-the-on-premises-gateway-connection"></a>Şirket içi ağ geçidi bağlantısı oluşturma
Bu bağlayıcı şirket içi veri ağ geçidini kullanarak şirket içi bir Informix veritabanına erişebilir. Daha fazla bilgi için bkz. ağ geçidi konuları. 

1. Ağ **geçitleri** yapılandırması bölmesinde, **Ağ Geçidi aracılığıyla bağlanmayı**etkinleştirmek için **onay kutusunu** seçin. Ayarları buluttan şirket içine değiştirme.
2. **Sunucu**için, adres veya diğer ad iki nokta üst üste bağlantı noktası numarası biçiminde tür değeri. Örneğin `ibmserver01:9089`yazın.
3. **Veritabanı**için tür değeri. Örneğin `nwind`yazın.
4. **Kimlik doğrulaması**için değer seçin. Örneğin, **temel**öğesini seçin.
5. **Kullanıcı adı**için tür değeri. Örneğin `informix`yazın.
6. **Parola**için değer yazın. Örneğin `Password1`yazın.
7. **Ağ geçidi**için değer seçin. Örneğin, **datagateway01**öğesini seçin.
8. Devam etmek için **Oluştur** ' u seçin. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)

## <a name="create-the-cloud-connection"></a>Bulut bağlantısı oluşturma
Bu bağlayıcı, bir bulut Informix veritabanına erişebilir. 

1. Ağ **geçitleri** yapılandırması bölmesinde, **onay kutusunu** devre dışı bırakın (tıklanmadan) **Ağ Geçidi aracılığıyla bağlayın**. 
2. **Bağlantı adı**için tür değeri. Örneğin `hisdemo2`yazın.
3. Adres veya diğer ad üst üste bağlantı noktası numarası biçimindeki **Informix sunucu adı**için tür değeri. Örneğin `hisdemo2.cloudapp.net:9089`yazın.
4. **Informix veritabanı adı**için tür değeri. Örneğin `nwind`yazın.
5. **Kullanıcı adı**için tür değeri. Örneğin `informix`yazın.
6. **Parola**için değer yazın. Örneğin `Password1`yazın.
7. Devam etmek için **Oluştur** ' u seçin. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Seç kullanarak tüm satırları getir
Informix tablosundaki tüm satırları getirmek için bir mantıksal uygulama eylemi oluşturabilirsiniz. Bu eylem, bağlayıcının `SELECT * FROM AREA`gibi bir Informix SELECT ifadesini işlemesini söyler.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. **Azure başlangıç panosunda** **+** (artı işareti), **Web ve mobil**ve ardından **mantıksal uygulama**' yı seçin.
2. **Adı** (örn. "**ınformixgetrows**"), **aboneliği**, **kaynak grubunu**, **konumu**ve **App Service planı**girin. **Panoya sabitle**' yi seçin ve ardından **Oluştur**' u seçin.

### <a name="add-a-trigger-and-action"></a>Tetikleyici ve eylem ekleme
1. **Logic Apps tasarımcısında**, **Şablonlar** listesinde **boş logicapp** ' i seçin.
2. **Tetikleyiciler** listesinde **yinelenme**' yi seçin. 
3. **Yineleme** tetikleyicisinde **Düzenle**' yi seçin, **sıklığı** seçin ' i seçin ve ardından **7**yazmak için **Interval** ' ı seçin. 
4. **+ Yeni adım** kutusunu seçin ve ardından **Eylem Ekle**' yi seçin.
5. **Eylemler** listesinde, **daha fazla eylem ara** düzenleme kutusuna **Informix** yazın ve ardından **Informix-satırları al (Önizleme)** öğesini seçin.
6. **Satırları al (Önizleme)** eyleminde **Bağlantıyı Değiştir**' i seçin.
7. **Bağlantılar** yapılandırması bölmesinde **Yeni oluştur**' u seçin. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorNewConnection.png)
8. Ağ **geçitleri** yapılandırması bölmesinde, **onay kutusunu** devre dışı bırakın (tıklanmadan) **Ağ Geçidi aracılığıyla bağlayın**.
   
   * **Bağlantı adı**için tür değeri. Örneğin `HISDEMO2`yazın.
   * Adres veya diğer ad üst üste bağlantı noktası numarası biçimindeki **Informix sunucu adı**için tür değeri. Örneğin `HISDEMO2.cloudapp.net:9089`yazın.
   * **Informix veritabanı adı**için tür değeri. Örneğin `NWIND`yazın.
   * **Kullanıcı adı**için tür değeri. Örneğin `informix`yazın.
   * **Parola**için değer yazın. Örneğin `Password1`yazın.
9. Devam etmek için **Oluştur** ' u seçin.
   
    ![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)
10. **Tablo adı** listesinde **aşağı oku**seçin ve ardından **alan**' ı seçin.
11. İsteğe bağlı olarak, sorgu seçeneklerini belirtmek için **Gelişmiş seçenekleri göster** ' i seçin.
12. **Kaydet**’i seçin. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowsTableName.png)
13. **Informixgetrows** dikey penceresinde, **Özet**altındaki **Tüm çalıştırmalar** listesinde, ilk listelenen öğeyi (en son çalıştırma) seçin.
14. **Mantıksal uygulama çalıştırma** dikey penceresinde **ayrıntıları Çalıştır**' ı seçin. **Eylem** listesi içinde **Get_rows**' yi seçin. **Durum**Için, **başarılı**olması gereken değere bakın. Girişleri görüntülemek için **girişler bağlantısını** seçin. **Çıktılar bağlantısını**seçin ve çıkışları görüntüleyin; Bu, bir satır listesi içermelidir.
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>INSERT kullanarak bir satır ekleme
Bir Informix tablosuna bir satır eklemek için bir mantıksal uygulama eylemi oluşturabilirsiniz. Bu eylem, bağlayıcının `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`gibi bir Informix INSERT ifadesini işlemesini söyler.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. **Azure başlangıç panosunda** **+** (artı işareti), **Web ve mobil**ve ardından **mantıksal uygulama**' yı seçin.
2. `InformixinsertRow`, **abonelik**, **kaynak grubu**, **konum**ve **App Service planı**gibi bir **ad**girin. **Panoya sabitle**' yi seçin ve ardından **Oluştur**' u seçin.

### <a name="add-a-trigger-and-action"></a>Tetikleyici ve eylem ekleme
1. **Logic Apps tasarımcısında**, **Şablonlar** listesinde **boş logicapp** ' i seçin.
2. **Tetikleyiciler** listesinde **yinelenme**' yi seçin. 
3. **Yineleme** tetikleyicisinde **Düzenle**' yi seçin, **sıklığı** seçin ' i seçin ve ardından **7**yazmak için **Interval** ' ı seçin. 
4. **+ Yeni adım** kutusunu seçin ve ardından **Eylem Ekle**' yi seçin.
5. **Eylemler** listesinde, **daha fazla eylem ara** düzenleme kutusuna **Informix** yazın ve ardından **Informix-satır ekle (Önizleme)** öğesini seçin.
6. **Satırları al (Önizleme)** eyleminde **Bağlantıyı Değiştir**' i seçin. 
7. **Bağlantılar** yapılandırması bölmesinde bir bağlantı seçmek için seçin. Örneğin, **hisdemo2**öğesini seçin.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. **Tablo adı** listesinde **aşağı oku**seçin ve ardından **alan**' ı seçin.
9. Tüm gerekli sütunlar için değer girin (kırmızı yıldız işaretine bakın). Örneğin, **AREAID**için `99999` yazın, `Area 99999`yazın ve **regionıd**için `102` yazın. 
10. **Kaydet**’i seçin.
    
    ![](./media/connectors-create-api-informix/InformixconnectorInsertRowValues.png)
11. **Informixınsertrow** dikey penceresinde, **Özet**altındaki **Tüm çalıştırmalar** listesinde, ilk listelenen öğeyi (en son çalıştırma) seçin.
12. **Mantıksal uygulama çalıştırma** dikey penceresinde **ayrıntıları Çalıştır**' ı seçin. **Eylem** listesi içinde **Get_rows**' yi seçin. **Durum**Için, **başarılı**olması gereken değere bakın. Girişleri görüntülemek için **girişler bağlantısını** seçin. **Çıktılar bağlantısını**seçin ve çıkışları görüntüleyin; yeni satırı içermelidir.
    
    ![](./media/connectors-create-api-informix/InformixconnectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>Seç kullanarak bir satır getir
Bir Informix tablosunda bir satır getirmek için bir mantıksal uygulama eylemi oluşturabilirsiniz. Bu eylem, bağlayıcının `SELECT FROM AREA WHERE AREAID = '99999'`gibi bir Informix SELECT WHERE ifadesini işlemesini söyler.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. **Azure başlangıç panosunda** **+** (artı işareti), **Web ve mobil**ve ardından **mantıksal uygulama**' yı seçin.
2. `InformixgetRow`, **abonelik**, **kaynak grubu**, **konum**ve **App Service planı**gibi bir **ad**girin. **Panoya sabitle**' yi seçin ve ardından **Oluştur**' u seçin.

### <a name="add-a-trigger-and-action"></a>Tetikleyici ve eylem ekleme
1. **Logic Apps tasarımcısında**, **Şablonlar** listesinde **boş logicapp** ' i seçin.
2. **Tetikleyiciler** listesinde **yinelenme**' yi seçin. 
3. **Yineleme** tetikleyicisinde **Düzenle**' yi seçin, **sıklığı** seçin ' i seçin ve ardından **7**yazmak için **Interval** ' ı seçin. 
4. **+ Yeni adım** kutusunu seçin ve ardından **Eylem Ekle**' yi seçin.
5. **Eylemler** listesinde, **daha fazla eylem ara** düzenleme kutusuna **Informix** yazın ve ardından **Informix-satırları al (Önizleme)** öğesini seçin.
6. **Satırları al (Önizleme)** eyleminde **Bağlantıyı Değiştir**' i seçin. 
7. **Bağlantı** yapılandırması bölmesinde, var olan bir bağlantıyı seçin. Örneğin, **hisdemo2**öğesini seçin.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. **Tablo adı** listesinde **aşağı oku**seçin ve ardından **alan**' ı seçin.
9. Tüm gerekli sütunlar için değer girin (kırmızı yıldız işaretine bakın). Örneğin, **AreaID**için `99999` yazın. 
10. İsteğe bağlı olarak, sorgu seçeneklerini belirtmek için **Gelişmiş seçenekleri göster** ' i seçin.
11. **Kaydet**’i seçin. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowValues.png)
12. **Informixgetrow** dikey penceresinde, **Özet**altındaki **Tüm çalıştırmalar** listesinde, ilk listelenen öğeyi (en son çalıştırma) seçin.
13. **Mantıksal uygulama çalıştırma** dikey penceresinde **ayrıntıları Çalıştır**' ı seçin. **Eylem** listesi içinde **Get_rows**' yi seçin. **Durum**Için, **başarılı**olması gereken değere bakın. Girişleri görüntülemek için **girişler bağlantısını** seçin. **Çıktılar bağlantısını**seçin ve çıkışları görüntüleyin; satır içermelidir.
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>GÜNCELLEŞTIRME kullanarak bir satırı değiştirme
Bir Informix tablosundaki bir satırı değiştirmek için bir mantıksal uygulama eylemi oluşturabilirsiniz. Bu eylem, bağlayıcının `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`gibi bir Informix UPDATE ifadesini işlemesini söyler.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. **Azure başlangıç panosunda** **+** (artı işareti), **Web ve mobil**ve ardından **mantıksal uygulama**' yı seçin.
2. `InformixupdateRow`, **abonelik**, **kaynak grubu**, **konum**ve **App Service planı**gibi bir **ad**girin. **Panoya sabitle**' yi seçin ve ardından **Oluştur**' u seçin.

### <a name="add-a-trigger-and-action"></a>Tetikleyici ve eylem ekleme
1. **Logic Apps tasarımcısında**, **Şablonlar** listesinde **boş logicapp** ' i seçin.
2. **Tetikleyiciler** listesinde **yinelenme**' yi seçin. 
3. **Yineleme** tetikleyicisinde **Düzenle**' yi seçin, **sıklığı** seçin ' i seçin ve ardından **7**yazmak için **Interval** ' ı seçin. 
4. **+ Yeni adım** kutusunu seçin ve ardından **Eylem Ekle**' yi seçin.
5. **Eylemler** listesinde, **daha fazla eylem ara** düzenleme kutusuna **Informix** yazın ve ardından **Informix-satırı Güncelleştir ' i (Önizleme)** seçin.
6. **Satırları al (Önizleme)** eyleminde **Bağlantıyı Değiştir**' i seçin. 
7. **Bağlantı** yapılandırması bölmesinde, var olan bir bağlantıyı seçin. Örneğin, **hisdemo2**öğesini seçin.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. **Tablo adı** listesinde **aşağı oku**seçin ve ardından **alan**' ı seçin.
9. Tüm gerekli sütunlar için değer girin (kırmızı yıldız işaretine bakın). Örneğin, **AREAID**için `99999` yazın, `Updated 99999`yazın ve **regionıd**için `102` yazın. 
10. **Kaydet**’i seçin. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorUpdateRowValues.png)
11. **Informixupdaterow** dikey penceresinde, **Özet**altındaki **Tüm çalıştırmalar** listesinde, ilk listelenen öğeyi (en son çalıştırma) seçin.
12. **Mantıksal uygulama çalıştırma** dikey penceresinde **ayrıntıları Çalıştır**' ı seçin. **Eylem** listesi içinde **Get_rows**' yi seçin. **Durum**Için, **başarılı**olması gereken değere bakın. Girişleri görüntülemek için **girişler bağlantısını** seçin. **Çıktılar bağlantısını**seçin ve çıkışları görüntüleyin; yeni satırı içermelidir.
    
    ![](./media/connectors-create-api-informix/InformixconnectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>SIL kullanarak bir satırı kaldırma
Bir Informix tablosundaki bir satırı kaldırmak için bir mantıksal uygulama eylemi oluşturabilirsiniz. Bu eylem, bağlayıcının `DELETE FROM AREA WHERE AREAID = '99999'`gibi bir Informix DELETE ifadesini işlemesini söyler.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. **Azure başlangıç panosunda** **+** (artı işareti), **Web ve mobil**ve ardından **mantıksal uygulama**' yı seçin.
2. `InformixdeleteRow`, **abonelik**, **kaynak grubu**, **konum**ve **App Service planı**gibi bir **ad**girin. **Panoya sabitle**' yi seçin ve ardından **Oluştur**' u seçin.

### <a name="add-a-trigger-and-action"></a>Tetikleyici ve eylem ekleme
1. **Logic Apps tasarımcısında**, **Şablonlar** listesinde **boş logicapp** ' i seçin.
2. **Tetikleyiciler** listesinde **yinelenme**' yi seçin. 
3. **Yineleme** tetikleyicisinde **Düzenle**' yi seçin, **sıklığı** seçin ' i seçin ve ardından **7**yazmak için **Interval** ' ı seçin. 
4. **+ Yeni adım** kutusunu seçin ve ardından **Eylem Ekle**' yi seçin.
5. **Eylemler** listesinde, **daha fazla eylem ara** düzenleme kutusuna **Informix** yazın ve ardından **Informix-satırı sil (Önizleme)** öğesini seçin.
6. **Satırları al (Önizleme)** eyleminde **Bağlantıyı Değiştir**' i seçin. 
7. **Bağlantı** yapılandırması bölmesinde, varolan bir bağlantıyı seçin. Örneğin, **hisdemo2**öğesini seçin.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. **Tablo adı** listesinde **aşağı oku**seçin ve ardından **alan**' ı seçin.
9. Tüm gerekli sütunlar için değer girin (kırmızı yıldız işaretine bakın). Örneğin, **AreaID**için `99999` yazın. 
10. **Kaydet**’i seçin. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorDeleteRowValues.png)
11. **Informixdeleterow** dikey penceresinde, **Özet**altındaki **Tüm çalıştırmalar** listesinde, ilk listelenen öğeyi (en son çalıştırma) seçin.
12. **Mantıksal uygulama çalıştırma** dikey penceresinde **ayrıntıları Çalıştır**' ı seçin. **Eylem** listesi içinde **Get_rows**' yi seçin. **Durum**Için, **başarılı**olması gereken değere bakın. Girişleri görüntülemek için **girişler bağlantısını** seçin. **Çıktılar bağlantısını**seçin ve çıkışları görüntüleyin; Silinen satırı içermesi gerekir.
    
    ![](./media/connectors-create-api-informix/InformixconnectorDeleteRowOutputs.png)

## <a name="supported-informix-platforms-and-versions"></a>Desteklenen Informix platformları ve sürümleri
Bu bağlayıcı, dağıtılmış Ilişkisel veritabanı mimarisi (DRDA) istemci bağlantılarını destekleyecek şekilde yapılandırıldığında aşağıdaki IBM Informix sürümlerini destekler.

* IBM Informix 12,1
* IBM Informix 11,7

## <a name="connector-specific-details"></a>Bağlayıcıya özgü ayrıntılar

Swagger 'da tanımlanan Tetikleyicileri ve eylemleri görüntüleyin ve ayrıca [bağlayıcı ayrıntılarında](/connectors/informix/)tüm limitleri inceleyin. 

## <a name="next-steps"></a>Sonraki adımlar
[Mantıksal uygulama oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md). Logic Apps ' deki diğer kullanılabilir bağlayıcıları [API listemize](apis-list.md)göz atın.

