---
title: Eşleme veri akışını kullanarak verileri dönüştürme
description: Bu öğretici, veri akışını eşlemeyle verileri dönüştürmek için Azure Veri Fabrikası'nı kullanmak için adım adım yönergeler sağlar
author: djpmsft
ms.author: daperlov
ms.reviewer: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/07/2019
ms.openlocfilehash: e6ca8007a96cc63b51b4f79b69029cbf0799e71c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75979189"
---
# <a name="transform-data-using-mapping-data-flows"></a>Veri akışlarını eşleme kullanarak verileri dönüştürme

Azure Data Factory kullanmaya yeni başlıyorsanız bkz. [Azure Data Factory'ye giriş](introduction.md).

Bu eğitimde, veri akışını eşleme kullanarak Azure Veri Gölü Depolama (ADLS) Gen2 kaynağındaki verileri kopyalayan ve dönüştüren bir ardışık hat lar oluşturmak için Azure Veri Fabrikası kullanıcı arabirimini (UX) kullanırsınız. Bu öğreticideki yapılandırma deseni, veri akışını eşleme kullanarak verileri dönüştürürken genişletilebilir

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Veri Akışı etkinliği içeren bir ardışık hatlar oluşturma.
> * Dört dönüşümiçeren bir eşleme veri akışı oluşturun.
> * İşlem hattında test çalıştırması yapma.
> * Veri Akışı etkinliğini izleme

## <a name="prerequisites"></a>Ön koşullar
* **Azure aboneliği.** Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* **Azure depolama hesabı**. ADLS depolama alanını *kaynak* ve *lavabo* veri depoları olarak kullanırsınız. Depolama hesabınız yoksa, oluşturma adımları için bkz. [Azure depolama hesabı oluşturma](../storage/common/storage-account-create.md).

Biz bu öğretici dönüştüren dosya MoviesDB.csv, [burada](https://raw.githubusercontent.com/djpmsft/adf-ready-demo/master/moviesDB.csv)bulabilirsiniz . Dosyayı GitHub'dan almak için, içeriği seçtiğiniz bir metin düzenleyicisine kopyalayın ve yerel olarak .csv dosyası olarak kaydedin. Dosyayı depolama hesabınıza yüklemek için [Azure Portalı ile Yükleme bloblarını](../storage/blobs/storage-quickstart-blobs-portal.md)görün. Örnekler ,'örnek veri' adlı bir kapsayıcıya başvurulacaktır.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

Bu adımda, bir veri fabrikası oluşturur ve veri fabrikasında bir ardışık hat lar oluşturmak için Veri Fabrikası UX'yi açarsınız.

1. **Microsoft Edge** veya **Google Chrome'u**açın. Şu anda, Veri Fabrikası UI yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenir.
2. Sol menüde, **bir kaynak** > **Analiz** > **Veri Fabrikası**Oluştur'u seçin:

   ![“Yeni” bölmesinde Data Factory seçimi](./media/doc-common-process/new-azure-data-factory-menu.png)

3. **Yeni veri fabrikası** sayfasında **Ad** bölümüne **ADFTutorialDataFactory** girin.

   Azure veri fabrikasının adı *genel olarak benzersiz*olmalıdır. Ad değeriyle ilgili bir hata iletisi alırsanız, veri fabrikası için farklı bir ad girin. (örneğin, adınızADFTutorialDataFactory). Data Factory yapıtlarının adlandırma kuralları için bkz.[Data Factory adlandırma kuralları](naming-rules.md).

     ![Yeni veri fabrikası](./media/doc-common-process/name-not-available-error.png)
4. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğinizi** seçin.
5. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:

    a. **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin.

    b. **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin. 
         
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/management/overview.md). 
6. **Sürüm** bölümünde **V2**'yi seçin.
7. **Konum** bölümünden veri fabrikası için bir konum seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları (örneğin, Azure Depolama ve SQL Veritabanı) ve bilgi işlem (örneğin, Azure HDInsight) başka bölgelerde olabilir.
8. **Oluştur'u**seçin.
9. Oluşturma tamamlandıktan sonra bildirimler merkezinde bildirimi görürsünüz. Veri fabrikası sayfasına gitmek için **kaynağa** git'i seçin.
10. Data Factory Kullanıcı Arabirimini (UI) ayrı bir sekmede başlatmak için **Geliştir ve İzle**’yi seçin.

## <a name="create-a-pipeline-with-a-data-flow-activity"></a>Veri Akışı etkinliği olan bir ardışık hatlar oluşturma

Bu adımda, veri akışı etkinliği içeren bir ardışık işlem hattı oluşturursunuz.

1. **Kullanmaya başlama** sayfasında **İşlem hattı oluştur** seçeneğini belirleyin.

   ![İşlem hattı oluşturma](./media/doc-common-process/get-started-page.png)

1. Ardışık hatlar için **Genel** sekmesinde, ardışık adı **için** **TransformMovies'i** girin.
1. Fabrikanın üst çubuğunda, **Veri Akışı hata ayıklama** kaydırıcısını kaydırın. Hata ayıklama modu, dönüşüm mantığının canlı bir Kıvılcım kümesine karşı etkileşimli sınamasına olanak tanır. Veri Akışı kümelerinin ısınması 5-7 dakika sürer ve kullanıcıların Veri Akışı geliştirmesini yapmayı planlıyorlarsa önce hata ayıklamayı açmaları önerilir. Daha fazla bilgi için [Hata Ayıklama Modu'na](concepts-data-flow-debug-mode.md)bakın.

    ![Veri Akışı Etkinliği](media/tutorial-data-flow/dataflow1.png)
1. **Etkinlikler** bölmesinde, Taşı **ve Dönüştür** akordeonu genişletin. **Veri Akışı** etkinliğini bölmeden boru hattı tuvaline sürükleyin ve bırakın.

    ![Veri Akışı Etkinliği](media/tutorial-data-flow/activity1.png)
1. Veri **Akışı Ekleme** açılır penceresinde, **yeni Veri Akışı Oluştur'u** seçin ve ardından veri akışınızı **TransformMovies**olarak adlandırın. Bittiğinde Bitir'i tıklatın.

    ![Veri Akışı Etkinliği](media/tutorial-data-flow/activity2.png)

## <a name="build-transformation-logic-in-the-data-flow-canvas"></a>Veri akışı tuvalinde dönüşüm mantığı oluşturma

Veri Akışınızı oluşturduktan sonra, otomatik olarak veri akışı tuvaline gönderilirsiniz. Bu adımda, ADLS depolama filmDB.csv alır ve 1910-2000 yılları arasında komedi ortalama derecelendirme toplar bir veri akışı oluşturmak. Daha sonra bu dosyayı ADLS depolama alanına geri yazarsınız.

1. Veri akışı tuvalinde Kaynak **Ekle** kutusunu tıklatarak bir kaynak ekleyin.

    ![Veri Akışı Tuvali](media/tutorial-data-flow/dataflow2.png)
1. Kaynak **MoviesDB**adlandırın. Yeni bir kaynak veri kümesi oluşturmak için **Yeni'yi** tıklatın.

    ![Veri Akışı Tuvali](media/tutorial-data-flow/dataflow3.png)
1. **Azure Veri Gölü Depolama Gen2'yi**seçin. Devam’a tıklayın.

    ![Veri kümesi](media/tutorial-data-flow/dataset1.png)
1. **DelimitedText'i**seçin. Devam’a tıklayın.

    ![Veri kümesi](media/tutorial-data-flow/dataset2.png)
1. Veri setinizi **FilmDB**olarak adlandırın. Bağlantılı hizmet açılır düşüşünde **Yeni'yi**seçin.

    ![Veri kümesi](media/tutorial-data-flow/dataset3.png)
1. Bağlantılı hizmet oluşturma ekranında ADLS gen2 bağlantılı hizmetinizi **ADLSGen2** olarak adlandırın ve kimlik doğrulama yönteminizi belirtin. Ardından bağlantı kimlik bilgilerinizi girin. Bu eğitimde, depolama hesabımıza bağlanmak için Hesap tuşunu kullanıyoruz. Kimlik bilgilerinizin doğru girilmesini doğrulamak için **Test bağlantısını** tıklatabilirsiniz. Tamamlandığında Oluştur'u tıklatın.

    ![Bağlantılı Hizmet](media/tutorial-data-flow/ls1.png)
1. Veri kümesi oluşturma ekranına geri döndüğünüzde, **Dosya yol** alanının altında dosyanızın bulunduğu yeri girin. Bu öğreticide, dosya moviesDB.csv kapsayıcı örnek veri bulunur. Dosyanın üstbilgisi olduğu için, **İlk satırı üstbilgi olarak**denetleyin. Üstbilgi şemasını doğrudan depolama daki dosyadan almak için **Bağlantıdan/Mağaza'dan'ı** seçin. Bittiğinde Tamam'ı tıklatın.

    ![Veri kümeleri](media/tutorial-data-flow/dataset4.png)
1. Hata ayıklama kümeniz başladıysa, kaynak dönüşümünün **Veri Önizleme** sekmesine gidin ve verilerin anlık görüntüsünü almak için **Yenile'yi** tıklatın. Dönüşümünüzün doğru şekilde yapılandırıldığından doğrulamak için veri önizlemesini kullanabilirsiniz.

    ![Veri Akışı Tuvali](media/tutorial-data-flow/dataflow4.png)
1. Veri akışı tuvalindeki kaynak düğümünüzün yanında, yeni bir dönüşüm eklemek için artı simgesine tıklayın. Eklediğiniz ilk dönüşüm bir **Filtre'dir.**

    ![Veri Akışı Tuvali](media/tutorial-data-flow/dataflow5.png)
1. Filtre **dönüşümünüzü filtreyıl**adlandırın. İfade oluşturucuyu açmak için **Filtre'nin** yanındaki ifade kutusunu tıklatın. Burada filtreleme durumunuzu belirteceksiniz.

    ![Filtre](media/tutorial-data-flow/filter1.png)
1. Veri akışı ifade oluşturucu, çeşitli dönüşümlerde kullanmak üzere etkileşimli ifadeler oluşturmanıza olanak tanır. İfadeler yerleşik işlevleri, giriş şemasından sütunlar ve kullanıcı tanımlı parametreler içerebilir. İfadelerin nasıl oluşturulabildiğini hakkında daha fazla bilgi için Bkz. [Veri Akışı ifade oluşturucusu.](concepts-data-flow-expression-builder.md)

    Bu eğitimde, 1910 ve 2000 yılları arasında ortaya çıkan tür komedi filmleri filtrelemek istiyorum. Yıl şu anda bir dize olduğundan, ```toInteger()``` işlevi kullanarak bir tamsayıya dönüştürmeniz gerekir. 1910 ve 200-'deki gerçek yıl değerleriyle karşılaştırmak için (>=) ve (<=) işleçlerden daha az veya eşit olan büyük veya eşit leri kullanın. Bu ifadeleri ve (&&) işleciyle birleştirin. İfade olarak çıkar:

    ```toInteger(year) >= 1910 && toInteger(year) <= 2000```

    Hangi filmlerin komedi olduğunu bulmak için, ```rlike()``` sütun türlerinde desen 'Komedi' bulmak için işlevi kullanabilirsiniz. Birlik yıl karşılaştırma ile rlike ifade almak için:

    ```toInteger(year) >= 1910 && toInteger(year) <= 2000 && rlike(genres, 'Comedy')```

    Hata ayıklama kümeniz etkinse, kullanılan girişlere kıyasla ifade çıktısını görmek için **Yenile'yi** tıklatarak mantığınızı doğrulayabilirsiniz. Veri akışı ifade dilini kullanarak bu mantığı nasıl başarabileceğinize dair birden fazla doğru yanıt vardır.

    ![Filtre](media/tutorial-data-flow/filter2.png)

    İfadenizi bitirdikten sonra **Kaydet ve Bitir'i** tıklatın.

1. Filtrenin doğru çalıştığını doğrulamak için **Veri Önizlemesi** getirin.

    ![Filtre](media/tutorial-data-flow/filter3.png)
1. Ekleyeceğiniz bir sonraki dönüşüm, **Schema değiştirici**altında **toplu** dönüşümdür.

    ![Toplama](media/tutorial-data-flow/agg1.png)
1. Toplu dönüşüm **AggregateComedyRatings**adı . **Sekmeye göre** Grup'ta, açılan alt tan film çıktığı yıla göre toplamaları gruplandırmak için **yılı** seçin.

    ![Toplama](media/tutorial-data-flow/agg2.png)
1. **Agregalar** sekmesine gidin. Sol metin kutusunda, toplam sütun **OrtalamaComedyRating**adını. İfade oluşturucu aracılığıyla toplu ifadeyi girmek için sağ ifade kutusuna tıklayın.

    ![Toplama](media/tutorial-data-flow/agg3.png)
1. Sütun **Derecelendirmesi**ortalamasını almak ```avg()``` için toplam işlevini kullanın. **Rating** bir dize ```avg()``` olduğundan ve sayısal bir giriş aldığından, ```toInteger()``` işlevi aracılığıyla değeri bir sayıya dönüştürmemiz gerekir. Bu ifade gibi görünüyor:

    ```avg(toInteger(Rating))```

    Bittiğinde **Kaydet ve Bitir'i** tıklatın.

    ![Toplama](media/tutorial-data-flow/agg4.png)
1. Dönüşüm çıktısını görüntülemek için **Veri Önizleme** sekmesine gidin. Dikkat sadece iki sütun var, **yıl** ve **AverageComedyRating**.

    ![Toplama](media/tutorial-data-flow/agg3.png)
1. Ardından, **Hedef**altında bir **Lavabo** dönüşümü eklemek istiyorsunuz.

    ![Havuz](media/tutorial-data-flow/sink1.png)
1. Lavabonuna **Sink**isim söyle. Lavabo veri kümenizi oluşturmak için **Yeni'yi** tıklatın.

    ![Havuz](media/tutorial-data-flow/sink2.png)
1. **Azure Veri Gölü Depolama Gen2'yi**seçin. Devam’a tıklayın.

    ![Veri kümesi](media/tutorial-data-flow/dataset1.png)
1. **DelimitedText'i**seçin. Devam’a tıklayın.

    ![Veri kümesi](media/tutorial-data-flow/dataset2.png)
1. Lavabo veri setinizi adlandırın **MoviesSink**. Bağlantılı hizmet için, adım 6'da oluşturduğunuz ADLS gen2 bağlantılı hizmeti seçin. Verilerinizi yazmak için bir çıktı klasörü girin. Bu öğreticide, kapsayıcı 'örnek-veri'deki 'çıktı' klasörüne yazıyoruz. Klasörün önceden var olması gerekmez ve dinamik olarak oluşturulabilir. İlk satırı doğru **olarak üstbilgi olarak** ayarlayın ve **Alma şeması**için **Yok'u** seçin. Son'a tıklayın.

    ![Havuz](media/tutorial-data-flow/sink3.png)

Şimdi veri akışınızı oluşturmayı bitirdiniz. Boru hattında çalıştırmaya hazırsın.

## <a name="running-and-monitoring-the-data-flow"></a>Veri Akışının Çalıştırılması ve İzlenmesi

Yayımlamadan önce bir ardışık bölümü hata ayıklayabilirsiniz. Bu adımda, veri akışı ardışık bir hata ayıklama çalışmasını tetikleyebilirsiniz. Veri önizlemesi veri yazmasa da, hata ayıklama çalışması lavabo hedefinize veri yazar.

1. Boru hattı tuvaline git. Hata **ayıklama** çalışmasını tetiklemek için hata ayıklama'yı tıklatın.

    ![İşlem hattı](media/tutorial-data-flow/pipeline1.png)
1. Veri Akışı etkinliklerinin ardışık alt bilgi ayıklama etkin hata ayıklama kümesini kullanır, ancak başlatması yine de en az bir dakika sürer. **İlerleme** sekmesinden ilerlemeyi izleyebilirsiniz. Çalıştırma başarılı olduktan sonra, izleme bölmesini açmak için gözlük simgesine tıklayın.

    ![İşlem hattı](media/tutorial-data-flow/pipeline2.png)
1. İzleme bölmesinde, her dönüşüm adımında harcanan satır sayısını ve zamanı görebilirsiniz.

    ![İzleme](media/tutorial-data-flow/pipeline3.png)
1. Sütunlar ve verilerin bölümleme hakkında ayrıntılı bilgi almak için bir dönüşüm etıklayın.

    ![İzleme](media/tutorial-data-flow/pipeline4.png)

Bu öğreticiyi doğru bir şekilde izlediyseniz, lavabo klasörünüze 83 satır ve 2 sütun yazmış olmalısınız. Blob depolamanızı kontrol ederek verilerin doğru luğunu doğrulayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticideki ardışık işlem, 1910'dan 2000'e kadar komedilerin ortalama derecelendirmesini toplayan ve verileri ADLS'ye yazan bir veri akışı çalıştırıyor. Şunları öğrendiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Veri Akışı etkinliği içeren bir ardışık hatlar oluşturma.
> * Dört dönüşümiçeren bir eşleme veri akışı oluşturun.
> * İşlem hattında test çalıştırması yapma.
> * Veri Akışı etkinliğini izleme

Veri akışı [ifade dili](data-flow-expression-functions.md)hakkında daha fazla bilgi edinin.
