---
title: Power BI kullanarak Azure Veri Gölü Depolama Gen1'deki verileri analiz edin | Microsoft Dokümanlar
description: Azure Veri Gölü Depolama Gen1'de depolanan verileri analiz etmek için Power BI'yi kullanın
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 57d19d27-e135-49d9-a7ea-46c48ef4e3bd
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: d8717b8f365e692b5f27bf8a04d65c5147b8f31b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65603201"
---
# <a name="analyze-data-in-azure-data-lake-storage-gen1-by-using-power-bi"></a>Power BI kullanarak Azure Veri Gölü Depolama Gen1'deki verileri analiz edin
Bu makalede, Azure Veri Gölü Depolama Gen1'de depolanan verileri analiz etmek ve görselleştirmek için Power BI Desktop'ı nasıl kullanacağınızı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Veri Gölü Depolama Gen1 hesabı**. [Azure portalını kullanarak Azure Veri Gölü Depolama Gen1 ile başlayın'daki](data-lake-store-get-started-portal.md)yönergeleri izleyin. Bu makalede, **myadlsg1**adı verilen bir Veri Gölü Depolama Gen1 hesabı oluşturduğunuzu ve bir örnek veri dosyası **(Drivers.txt)** yüklediğinizi varsayar. Bu örnek dosya Azure [Veri Gölü Git Deposu'ndan](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt)indirilebilir.
* **Güç BI Masaüstü**. Bunu [Microsoft Download Center'dan](https://www.microsoft.com/en-us/download/details.aspx?id=45331)indirebilirsiniz. 

## <a name="create-a-report-in-power-bi-desktop"></a>Power BI Desktop'ta rapor oluşturma
1. Power BI Desktop'ı bilgisayarınızda başlatın.
2. **Giriş** şeridinden **Veri Al'ı**tıklatın ve sonra Daha Fazla'yı tıklatın. Veri **Al** iletişim kutusunda **Azure'u**tıklatın, **Azure Veri Gölü Deposu'nu**tıklatın ve sonra **Bağlan'ı**tıklatın.
   
    ![Veri Gölü Depolama Gen1'e bağlanın](./media/data-lake-store-power-bi/get-data-lake-store-account.png "Veri Gölü Depolama Gen1'e bağlanın")
3. Bağlayıcının geliştirme aşamasında olduğuyla ilgili bir iletişim kutusu görürseniz, devam etmeyi tercih edin.
4. Azure **Veri Gölü Deposu** iletişim kutusunda, Veri Gölü Depolama Gen1 hesabınıza URL sağlayın ve ardından **Tamam'ı**tıklatın.
   
    ![Veri Gölü Depolama Gen1 için URL](./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "Veri Gölü Depolama Gen1 için URL")
5. Bir sonraki iletişim kutusunda, Veri Gölü Depolama Gen1 hesabında oturum açmak için **Oturum Aç'ı** tıklatın. Kuruluşunuzun oturum açma sayfasına yönlendirilirsiniz. Hesapta oturum açma istemlerini izleyin.
   
    ![Veri Gölü Depolama Gen1 oturum açın](./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "Veri Gölü Depolama Gen1 oturum açın")
6. Başarılı bir şekilde oturum imzaladıktan sonra **Bağlan'ı**tıklatın.
   
    ![Veri Gölü Depolama Gen1'e bağlanın](./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "Veri Gölü Depolama Gen1'e bağlanın")
7. Bir sonraki iletişim kutusu, Veri Gölü Depolama Gen1 hesabınıza yüklediğiniz dosyayı gösterir. Bilgileri doğrulayın ve ardından **Yükle'yi**tıklatın.
   
    ![Veri Gölü Depolama Gen1'den yük verileri](./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "Veri Gölü Depolama Gen1'den yük verileri")
8. Veriler Power BI'ye başarıyla yüklendikten **sonra, Alanlar** sekmesinde aşağıdaki alanları görürsünüz.
   
    ![İçe aktarılan alanlar](./media/data-lake-store-power-bi/imported-fields.png "İçe aktarılan alanlar")
   
    Ancak, verileri görselleştirmek ve analiz etmek için, verilerin aşağıdaki alanlara göre kullanılabilir olmasını tercih ederiz
   
    ![İstenilen alanlar](./media/data-lake-store-power-bi/desired-fields.png "İstenilen alanlar")
   
    Sonraki adımlarda, içe aktarılan verileri istenilen biçimde dönüştürmek için sorguyu güncelleştireceğiz.
9. **Giriş** şeridinden **Sorguları Edit'i**tıklatın.
   
    ![Sorguları düzenle](./media/data-lake-store-power-bi/edit-queries.png "Sorguları düzenle")
10. Sorgu Düzenleyicisi'nde, **İçerik** sütununaltında **İkili'yi**tıklatın.
    
    ![Sorguları düzenle](./media/data-lake-store-power-bi/convert-query1.png "Sorguları düzenle")
11. Yüklediğiniz **Drivers.txt** dosyasını temsil eden bir dosya simgesi görürsünüz. Dosyayı sağ tıklatın ve **CSV'yi**tıklatın.    
    
    ![Sorguları düzenle](./media/data-lake-store-power-bi/convert-query2.png "Sorguları düzenle")
12. Aşağıda gösterildiği gibi bir çıktı görmeniz gerekir. Verileriniz artık görselleştirmeler oluşturmak için kullanabileceğiniz bir biçimde kullanılabilir.
    
    ![Sorguları düzenle](./media/data-lake-store-power-bi/convert-query3.png "Sorguları düzenle")
13. **Giriş** şeridinden **Kapat ve Uygula'yı**tıklatın ve sonra Kapat ve **Uygula'yı**tıklatın.
    
    ![Sorguları düzenle](./media/data-lake-store-power-bi/load-edited-query.png "Sorguları düzenle")
14. Sorgu güncelleştirildikten sonra, **Alanlar** sekmesi görselleştirme için kullanılabilen yeni alanları gösterir.
    
    ![Güncelleştirilmiş alanlar](./media/data-lake-store-power-bi/updated-query-fields.png "Güncelleştirilmiş alanlar")
15. Belirli bir ülke/bölge için her şehirdeki sürücüleri temsil edecek bir pasta grafiği oluşturalım. Bunu yapmak için aşağıdaki seçimleri yapın.
    
    1. Görseller sekmesinden, pasta grafiği için simgeyi tıklatın.
       
        ![Pasta grafiği oluşturma](./media/data-lake-store-power-bi/create-pie-chart.png "Pasta grafiği oluşturma")
    2. Kullanacağımız sütunlar **Sütun 4** (şehrin adı) ve **Sütun 7** (ülkenin/bölgenin adı) dir. Bu sütunları **Alanlar** sekmesinden **Visualizations** sekmesine aşağıda gösterildiği gibi sürükleyin.
       
        ![Görselleştirmeler oluşturma](./media/data-lake-store-power-bi/create-visualizations.png "Görselleştirmeler oluşturma")
    3. Pasta grafiği şimdi aşağıda gösterilen gibi benzemeye caydırmalıdır.
       
        ![Pasta grafiği](./media/data-lake-store-power-bi/pie-chart.png "Görselleştirmeler oluşturma")
16. Sayfa düzeyi filtrelerinden belirli bir ülke/bölge seçerek, artık seçilen ülkenin/bölgenin her şehrindeki sürücü sayısını görebilirsiniz. Örneğin, **Görselleştirmeler** sekmesinin altında, **Sayfa düzeyi filtreleri** **altında, Brezilya'yı**seçin.
    
    ![Ülke seçin](./media/data-lake-store-power-bi/select-country.png "Ülke/bölge seçin")
17. Pasta grafiği, Brezilya'daki sürücüleri görüntülemek için otomatik olarak güncelleştirilir.
    
    ![Bir ülkedeki sürücüler](./media/data-lake-store-power-bi/driver-per-country.png "Ülke/bölge başına sürücüler")
18. **Dosya** menüsünden, visualization'ı Power BI Desktop dosyası olarak kaydetmek için **Kaydet'i** tıklatın.

## <a name="publish-report-to-power-bi-service"></a>Raporu Power BI hizmetine yayınlama
Power BI Desktop'daki görselleştirmeleri oluşturduktan sonra, Power BI hizmetine yayınlayarak başkalarıyla paylaşabilirsiniz. Bunun nasıl yapılacağını anlatan talimatlar için Power [BI Desktop'dan Yayımla'ya](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/)bakın.

## <a name="see-also"></a>Ayrıca bkz.
* [Data Lake Analytics'i kullanarak Veri Gölü Depolama Gen1'deki verileri analiz edin](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

