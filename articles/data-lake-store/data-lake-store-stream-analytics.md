---
title: Akış Analizi'nden Azure Veri Gölü Depolama Gen1'e veri akışı | Microsoft Dokümanlar
description: Verileri Azure Veri Gölü Depolama Gen1'e aktarmak için Azure Akış Analizi'ni kullanın
services: data-lake-store,stream-analytics
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: edb58e0b-311f-44b0-a499-04d7e6c07a90
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: twooley
ms.openlocfilehash: d3dbacd58b3bda3fbf8ee8ad5f175eccc2cb2a24
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60194952"
---
# <a name="stream-data-from-azure-storage-blob-into-azure-data-lake-storage-gen1-using-azure-stream-analytics"></a>Azure Akış Analizi'ni kullanarak Azure Depolama Blob'undan Azure Veri Gölü Depolama Gen1'e veri akışı
Bu makalede, Azure Veri Gölü Depolama Gen1'inbir Azure Akışı Analizi işi için çıktı olarak nasıl kullanılacağını öğreneceksiniz. Bu makale, bir Azure Depolama blob (giriş) verileri okur ve Veri Gölü Depolama Gen1 (çıktı) verileri yazar basit bir senaryo gösterir.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Depolama hesabı.** Bir Stream Analytics işi için veri girişi yapmak için bu hesaptan bir blob kapsayıcısı kullanırsınız. Bu öğretici için, **storageforasa** adlı bir depolama hesabınız ve **depoforasacontainer**adı verilen hesap içinde bir kapsayıcınız olduğunu varsayalım. Kapsayıcıyı oluşturduktan sonra, ona örnek bir veri dosyası yükleyin. 
  
* **Bir Veri Gölü Depolama Gen1 hesabı**. [Azure Portalını kullanarak Azure Veri Gölü Depolama Gen1 ile başlayın](data-lake-store-get-started-portal.md)yönergeleri izleyin. **Myadlsg1**adında bir Veri Gölü Depolama Gen1 hesabınız olduğunu varsayalım. 

## <a name="create-a-stream-analytics-job"></a>Stream Analytics İşi oluşturma
Giriş kaynağı ve çıktı hedefi içeren bir Akış Analizi işi oluşturarak başlarsınız. Bu öğretici için kaynak bir Azure blob kapsayıcıve hedef Veri Gölü Depolama Gen1 olduğunu.

1. [Azure Portal](https://portal.azure.com)'da oturum açın.

2. Sol **bölmeden, Akış Analizi işlerini**tıklatın ve sonra **Ekle'yi**tıklatın.

    ![Akış Analizi İşi Oluşturma](./media/data-lake-store-stream-analytics/create.job.png "Akış Analizi işi oluşturma")

    > [!NOTE]
    > Depolama hesabıyla aynı bölgede iş oluşturduğunuzdan emin olun, yoksa bölgeler arasında veri taşıma nın ek maliyetine tabi olacaktır.
    >

## <a name="create-a-blob-input-for-the-job"></a>İş için blob girişi oluşturma

1. **Girişler** sekmesini tıklatın ve sonra **Ekle'yi**tıklatın, sol bölmeden Akış Analizi işi için sayfayı açın.

    ![İşinize giriş ekleme](./media/data-lake-store-stream-analytics/create.input.1.png "İşinize giriş ekleme")

2. Yeni **giriş** bıçağında aşağıdaki değerleri sağlayın.

    ![İşinize giriş ekleme](./media/data-lake-store-stream-analytics/create.input.2.png "İşinize giriş ekleme")

   * **Giriş takma adı**için, iş girişi için benzersiz bir ad girin.
   * **Kaynak türü için**Veri **akışı'nı**seçin.
   * **Kaynak**için **Blob depolama alanını**seçin.
   * **Abonelik**için, **geçerli abonelikten blob depolama yı kullan'ı**seçin.
   * **Depolama hesabı**için, ön koşulların bir parçası olarak oluşturduğunuz depolama hesabını seçin. 
   * **Kapsayıcı**için, seçili depolama hesabında oluşturduğunuz kapsayıcıyı seçin.
   * **Olay serileştirme biçimi**için **CSV'yi**seçin.
   * **Delimiter**için **sekme**seçin.
   * **Kodlama**için **UTF-8'i**seçin.

     **Oluştur'u**tıklatın. Portal şimdi girişi ekler ve ona bağlantı sınar.


## <a name="create-a-data-lake-storage-gen1-output-for-the-job"></a>İş için veri gölü depolama gen1 çıktısı oluşturma

1. Akış Analizi işi için sayfayı açın, **Çıktılar** sekmesini tıklatın, **Ekle'yi**tıklatın ve **Veri Gölü Depolama Gen1'i**seçin.

    ![İşinize çıktı ekleme](./media/data-lake-store-stream-analytics/create.output.1.png "İşinize çıktı ekleme")

2. Yeni **çıkış** bıçağında aşağıdaki değerleri sağlayın.

    ![İşinize çıktı ekleme](./media/data-lake-store-stream-analytics/create.output.2.png "İşinize çıktı ekleme")

    * **Çıktı diğer adı için,** iş çıktısı için benzersiz bir ad girin. Bu, sorgu çıktısını bu Veri Gölü Depolama Gen1 hesabına yönlendirmek için sorgularda kullanılan kolay bir addır.
    * Veri Gölü Depolama Gen1 hesabına erişim izni istenecektir. **Yetkilendir'i**tıklatın.

3. Yeni **çıkış** bıçağında, aşağıdaki değerleri sağlamaya devam edin.

    ![İşinize çıktı ekleme](./media/data-lake-store-stream-analytics/create.output.3.png "İşinize çıktı ekleme")

   * **Hesap adı için,** iş çıktısının gönderilmesini istediğiniz yerde oluşturduğunuz Veri Gölü Depolama Gen1 hesabını seçin.
   * **Yol öneki deseni**için, belirtilen Veri Gölü Depolama Gen1 hesabına dosyalarınızı yazmak için kullanılan bir dosya yolu girin.
   * **Tarih biçimi**için, önek yolunda bir tarih belirteci kullandıysanız, dosyalarınızın düzenlendiği tarih biçimini seçebilirsiniz.
   * **Zaman biçimi**için, önek yolunda bir zaman belirteci kullandıysanız, dosyalarınızın düzenlendiği zaman biçimini belirtin.
   * **Olay serileştirme biçimi**için **CSV'yi**seçin.
   * **Delimiter**için **sekme**seçin.
   * **Kodlama**için **UTF-8'i**seçin.
    
     **Oluştur'u**tıklatın. Portal şimdi çıktıyı ekler ve bağlantıyı sınar.
    
## <a name="run-the-stream-analytics-job"></a>Stream Analytics işini çalıştırma

1. Akış Analizi işini çalıştırmak için **Sorgu** sekmesinden bir sorgu çalıştırmanız gerekir. Bu öğretici için, aşağıdaki ekran yakalamada gösterildiği gibi, yer tutucuları iş giriş ve çıktı takma adlarıyla değiştirerek örnek sorguyu çalıştırabilirsiniz.

    ![Sorgu çalıştırma](./media/data-lake-store-stream-analytics/run.query.png "Sorgu çalıştırma")

2. Ekranın üst kısmından **Kaydet'i** tıklatın ve ardından **Genel Bakış** sekmesinden **Başlat'ı**tıklatın. İletişim kutusundan Özel **Saat'i**seçin ve ardından geçerli tarih ve saati ayarlayın.

    ![İş zamanını ayarlama](./media/data-lake-store-stream-analytics/run.query.2.png "İş zamanını ayarlama")

    İşe başlamak için **Başlat'ı** tıklatın. İşe başlamak birkaç dakika kadar sürebilir.

3. Blob'dan verileri seçmek için işi tetiklemek için, örnek bir veri dosyasını blob kapsayıcısına kopyalayın. [Azure Veri Gölü Git Deposu'ndan](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt)örnek bir veri dosyası alabilirsiniz. Bu öğretici için, **dosyavehicle1_09142014.csv**kopyalayalım. Verileri bir blob kapsayıcısına yüklemek için [Azure Depolama Gezgini](https://storageexplorer.com/)gibi çeşitli istemciler kullanabilirsiniz.

4. **İzleme**altında **Genel Bakış** sekmesinden verilerin nasıl işlendiğini görün.

    ![İşi izleme](./media/data-lake-store-stream-analytics/run.query.3.png "İşi izleme")

5. Son olarak, iş çıktısı verilerinin Veri Gölü Depolama Gen1 hesabında kullanılabildiğini doğrulayabilirsiniz. 

    ![Çıkışı doğrulama](./media/data-lake-store-stream-analytics/run.query.4.png "Çıkışı doğrulama")

    Veri Gezgini bölmesinde, çıktının Veri Gölü Depolama Gen1 çıktı ayarlarında belirtildiği gibi`streamanalytics/job/output/{date}/{time}`bir klasör yoluna yazıldığına dikkat edin ( ).  

## <a name="see-also"></a>Ayrıca bkz.
* [Veri Gölü Depolama Gen1'i kullanmak için bir HDInsight kümesi oluşturun](data-lake-store-hdinsight-hadoop-use-portal.md)
