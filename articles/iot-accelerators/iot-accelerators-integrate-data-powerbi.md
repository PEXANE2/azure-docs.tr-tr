---
title: Power BI - Azure | Microsoft Dokümanlar
description: Bu öğretici, Uzaktan İzleme çözümündeki verileri özelleştirilmiş bir görselleştirmeye entegre etmek için Power BI Desktop ve Cosmos DB'yi kullanır. Bu şekilde kullanıcılar kendi özel panolarını oluşturabilir ve çözümde olmayan kullanıcılarla paylaşabilir.
author: dominicbetts
manager: hegate
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 05/01/2018
ms.topic: conceptual
ms.openlocfilehash: 8b0f395936c8628ff7b8dab601168538f97ccdb7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74184235"
---
# <a name="visualize-remote-monitoring-data-using-power-bi"></a>Power BI kullanarak Uzaktan İzleme verilerini görselleştirin

Bu öğretici, CosmosDB'den Power BI'ye Uzaktan İzleme çözüm verilerinizi nasıl takacağınız konusunda size yol göreceksiniz. Bu bağlantı kurulduğunda, kendi özel panolarınızı oluşturabilir ve bunları Uzaktan İzleme çözüm panonuza geri ekleyebilirsiniz. Bu çalışma akışı, kutunun dışında olanlara ek olarak daha özel grafikler oluşturulmasına olanak sağlar. Daha sonra bu öğreticiyi diğer veri akışlarıyla tümleştirmek veya Uzaktan İzleme Çözümünüz dışında tüketilecek özel panolar oluşturmak için kullanabilirsiniz. Power BI'de panolar oluşturmak, belirli parçaları seçerken her panelin birbiriyle etkileşimkurabileceği anlamına gelir. Örneğin, yalnızca simüle edilmiş kamyonlarınız hakkındaki bilgileri gösteren bir filtreniz olabilir ve gösterge panonuzun her parçası yalnızca simüle edilmiş kamyon bilgilerini göstermek için etkileşime girebiliyor. Power BI dışında bir araç kullanmak istiyorsanız, bu adımları seçtiğiniz görselleştirme aracını kullanmak ve Cosmos Veritabanı'na bağlamak için de genişletebilir veya bir tane ayarladıysanız özel veritabanına bağlayabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

- Şu anda çalışan bir Uzaktan İzleme çözümüne sahip olmalısınız
- [Azure portalına](https://portal.azure.com) ve IoT Hub ve Çözümü'nün çalıştırıldığı aboneliğinize erişiminiz olmalıdır
- [Power BI masaüstü](https://powerbi.microsoft.com) yüklü olmalıdır, herhangi bir sürümü yapacak


## <a name="information-needed-from-azure-portal"></a>Azure portalından Gerekli Bilgiler

1. Azure [portalına](https://portal.azure.com) gidin ve gerekirse oturum açın

2. Sol panelde Kaynak grupları'nı tıklatın

    ![Yan Panel Nav](./media/iot-accelerators-integrate-data-powerbi/side_panel.png)

3. Iot Çözümünüzün üzerinde çalışmakta olduğu Kaynak Grubuna gidin ve kaynak grubunun Genel Bakış sayfasına alınmak üzere tıklayın. 

4. Bu genel bakış sayfasında "Azure Cosmos DB Hesabı" türüne sahip öğeyi tıklatın, ardından bu IoT Çözümü için Cosmos DB akışının genel bakış sayfasına götürülür.

    ![Kaynak Grubu](./media/iot-accelerators-integrate-data-powerbi/resource_groups.png)

5. Soldaki panelde "Tuşlar" bölümüne tıklayın ve Power BI'de kullanılacak aşağıdaki değerlere dikkat edin:

   - URI
   - Birincil Anahtar

     ![keys](./media/iot-accelerators-integrate-data-powerbi/keys.png)

## <a name="setting-up-the-stream-in-power-bi"></a>Power BI'de Stream'i ayarlama
  
1. Power BI masaüstü uygulamasını açın ve sol üst köşeden "Veri Al"a tıklayın. 

    ![Veri Al](./media/iot-accelerators-integrate-data-powerbi/get_data.png)

2. Veri girmeniz istendiğinde, "Azure Cosmos DB"yi aramayı seçin ve bu bağlayıcıyı seçin. Bu bağlayıcı aslında Azure IoT Çözümünüzün kozmosun veritabanından doğrudan veri çeker
  
    ![Cosmos DB](./media/iot-accelerators-integrate-data-powerbi/cosmos_db.png)
  
3. Yukarıda kaydettiğiniz bilgileri girin:

    * URI
    * Birincil Anahtar

4. Power BI'ye aktarılacak tüm tabloları seçin. Bu eylem, verilerin yüklenmesi başlatacak. Çözümünüz ne kadar uzun süre çalışıyorsa, verilerin yüklenmesi o kadar uzun sürebilir (birkaç saate kadar). 

    ![İthalat Tabloları](./media/iot-accelerators-integrate-data-powerbi/import_tables.png)

5. Veriler yüklemeyi bitirdikten sonra, Power BI'nin üst satırında "Sorguları Düzenleme"yi tıklatın ve her tablo için sarı çubuktaki okları tıklatarak tüm tabloları genişletin. Bu temelde tüm sütunları göstermek için genişletecektir. Nem, hız süresi, vb. gibi şeyler için verilerin nasıl doğru olmadığını fark edeceksiniz.

    ![Yeni Sütun](./media/iot-accelerators-integrate-data-powerbi/new_column.png)
  
    Örneğin, Power BI'ye gelen veriler konektör den geldiğinde UNIX saatine dönüştürüldü. Bu dönüştürmeiçin ayarlamak için, ileriye doğru yeni bir sütun oluşturabilir ve bu denklemi tarih zaman biçimine sokmak için kullanabilirsiniz: 

    ```text
    #datetime(1970, 1, 1, 0, 0, 0) + #duration(0, 0, 0, [Document.device.msg.received]/1000)
    ```

    ![Güncellenen Tablo](./media/iot-accelerators-integrate-data-powerbi/updated_table.png)
  
    Document.device.msg.received, UNIX biçimlendirmesi yapılan sütunlardan sadece biridir ve dönüştürmesi gereken diğer sütunlarla değiştirilebilir. 
  
    Diğer veri noktaları, yukarıdaki gibi aynı adımlar kullanılarak uygun olduğu durumlarda Çift veya Int olarak değiştirilebilir String türüne dönüştürüldü.

## <a name="creating-a-dashboard"></a>Pano oluşturma

Akış bağlandıktan sonra, kişiselleştirilmiş panolarınızı oluşturmaya hazırsınız! Aşağıdaki pano, simüle edilmiş cihazlarımız tarafından yayılan telemetriyi alıp, etrafındaki farklı pivotları aşağıdaki gibi gösteren bir örnektir: 

* Haritadaki Aygıt Konumu (sağda)
* Durumları ve şiddeti olan aygıtlar. (sol üstte)
* Kuralları yerinde olan aygıtlar ve onlar için herhangi bir uyarı varsa (sol altta)

![PowerBi Görselleştirme](./media/iot-accelerators-integrate-data-powerbi/visual_data.png)

## <a name="publishing-the-dashboard-and-refreshing-the-data"></a>Panoyayımlama ve verileri yenileme

Panolarınızı başarıyla oluşturduktan sonra, Power BI panolarınızı başkalarıyla paylaşmak üzere [yayımlamanızı](https://docs.microsoft.com/power-bi/desktop-upload-desktop-files) öneririz.

Ayrıca, en son veri kümesine sahip olduğundan emin olmak için yayımlanmış panodaki [verileri yenilemek](https://docs.microsoft.com/power-bi/refresh-data) de isteyeceksiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Power BI kullanarak uzaktan izleme verilerini nasıl görselleştirileceğinizi öğrendiniz

Uzaktan İzleme çözümünü özelleştirme hakkında daha fazla bilgi için bkz:

* [Uzaktan İzleme Çözümü Kullanıcı Arabirimi'ni özelleştirme](iot-accelerators-remote-monitoring-customize.md)
* [Geliştirici Başvuru Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Geliştirici Sorun Giderme Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

