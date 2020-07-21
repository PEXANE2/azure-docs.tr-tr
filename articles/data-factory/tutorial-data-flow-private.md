---
title: Azure Data Factory yönetilen VNet eşleme veri akışı ile veri dönüştürme
description: Bu öğretici, veri akışı eşleme ile verileri dönüştürmek için Azure Data Factory kullanmaya yönelik adım adım yönergeler sağlar
author: djpmsft
ms.author: daperlov
ms.reviewer: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 05/19/2019
ms.openlocfilehash: 5515f6c4dfbc5d3c0e391373e763597d7fdc89ed
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86532316"
---
# <a name="transform-data-securely-using-mapping-data-flows"></a>Veri akışlarını eşleme kullanarak güvenli bir şekilde veri dönüştürme

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Azure Data Factory kullanmaya yeni başlıyorsanız bkz. [Azure Data Factory'ye giriş](https://docs.microsoft.com/azure/data-factory/introduction).

Bu öğreticide, bir Azure Data Lake Storage (ADLS) Gen2 kaynağından verileri kopyalayan ve dönüştüren bir işlem hattı oluşturmak için Azure Data Factory Kullanıcı arabirimini (UX), [Azure Data Factory yönetilen sanal ağdaki](managed-virtual-network-private-endpoint.md)veri akışını eşleme kullanarak bir **ADLS 2. havuzuna (her ikisi de yalnızca seçilen ağlara erişime izin vermek)** kullanacaksınız. Bu öğreticideki yapılandırma deseninin verileri, eşleme veri akışı kullanılarak dönüştürülürken genişletilebilir.

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
>
> * Veri fabrikası oluşturma.
> * Veri akışı etkinliğine sahip bir işlem hattı oluşturun.
> * Dört dönüşümle bir eşleme veri akışı oluşturun.
> * İşlem hattında test çalıştırması yapma.
> * Veri akışı etkinliğini izleme

## <a name="prerequisites"></a>Önkoşullar
* **Azure aboneliği**. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* **Azure depolama hesabı**. ADLS depolamayı *kaynak* ve *Havuz* veri depoları olarak kullanırsınız. Depolama hesabınız yoksa, oluşturma adımları için bkz. [Azure depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-account-create?tabs=azure-portal). **Depolama hesabının yalnızca ' seçili ağlardan ' erişime izin verdiğinden emin olun.** 

Bu öğreticide dönüştürtiğimiz dosya MoviesDB.csv, [burada](https://raw.githubusercontent.com/djpmsft/adf-ready-demo/master/moviesDB.csv)bulunabilir. Dosyayı GitHub 'dan almak için, bir. csv dosyası olarak yerel olarak kaydetmek üzere içeriği istediğiniz bir metin düzenleyicisine kopyalayın. Dosyayı depolama hesabınıza yüklemek için bkz. [Azure Portal Blobları karşıya yükleme](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal). Örnekler ' Sample-Data ' adlı bir kapsayıcıya başvuracaktır.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

Bu adımda, bir veri fabrikası oluşturur ve veri fabrikasında bir işlem hattı oluşturmak için Data Factory UX 'i açarsınız.

1. **Microsoft Edge** veya **Google Chrome**'ı açın. Şu anda Data Factory UI yalnızca Microsoft Edge ve Google Chrome Web tarayıcılarında desteklenir.
2. Sol menüde, **kaynak**  >  **Analizi**  >  **Data Factory**oluştur ' u seçin.
3. **Yeni veri fabrikası** sayfasında **Ad** bölümüne **ADFTutorialDataFactory** girin.

   Azure veri fabrikasının adı *genel olarak benzersiz*olmalıdır. Ad değeriyle ilgili bir hata iletisi alırsanız, veri fabrikası için farklı bir ad girin. (örneğin, Adınızadftutorialdatafactory). Data Factory yapıtlarının adlandırma kuralları için bkz.[Data Factory adlandırma kuralları](naming-rules.md).

4. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğinizi** seçin.
5. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:

    a. **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin.

    b. **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin. 
         
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/management/overview.md). 
6. **Sürüm** bölümünde **V2**'yi seçin.
7. **Konum** bölümünden veri fabrikası için bir konum seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri Fabrikası tarafından kullanılan veri depoları (örneğin, Azure depolama ve SQL veritabanı) ve işlemler (örneğin, Azure HDInsight) başka bölgelerde olabilir.

8. **Oluştur**’u seçin.

9. Oluşturma işlemi tamamlandıktan sonra, Bildirim Merkezi ' nde bildirimi görürsünüz. Data Factory sayfasına gitmek için **Kaynağa Git** ' i seçin.
10. Data Factory Kullanıcı Arabirimini (UI) ayrı bir sekmede başlatmak için **Geliştir ve İzle**’yi seçin.

## <a name="create-an-azure-integration-runtime-in-adf-managed-virtual-network"></a>ADF tarafından yönetilen sanal ağda Azure Integration Runtime oluşturma
Bu adımda, bir Azure Integration Runtime oluşturup yönetilen sanal ağı etkinleştirirsiniz.

1. ADF portalında, **hub 'ı Yönet** ' e gidin ve yeni bir Azure Integration Runtime oluşturmak için **Yeni** ' ye tıklayın.
   ![Yeni Azure Integration Runtime oluştur](./media/tutorial-copy-data-portal-private/create-new-azure-ir.png)
2. **Azure** Integration Runtime oluşturmayı seçin.
   ![Yeni Azure Integration Runtime](./media/tutorial-copy-data-portal-private/azure-ir.png)
3. **Sanal ağı**etkinleştirin.
   ![Yeni Azure Integration Runtime](./media/tutorial-copy-data-portal-private/enable-managed-vnet.png)
4. **Oluştur**’u seçin.

## <a name="create-a-pipeline-with-a-data-flow-activity"></a>Veri akışı etkinliği ile işlem hattı oluşturma

Bu adımda, bir veri akışı etkinliği içeren bir işlem hattı oluşturacaksınız.

1. **Kullanmaya başlama** sayfasında **İşlem hattı oluştur** seçeneğini belirleyin.

   ![İşlem hattı oluşturma](./media/doc-common-process/get-started-page.png)

1. İşlem hattının **Özellikler** bölmesinde, Işlem hattının **adı** için **transformfilmlerini** girin.
1. Fabrika üst çubuğunda, **veri akışı hata ayıklama** kaydırıcısını üzerine kaydırın. Hata ayıklama modu, canlı bir Spark kümesine karşı dönüştürme mantığının etkileşimli olarak test edilmesine olanak tanır. Veri akışı kümelerinin ısınma için 5-7 dakika sürer ve veri akışı geliştirmeyi planlarsa, kullanıcıların hata ayıklamanın ilk kez etkinleştirilmesi önerilir. Daha fazla bilgi için bkz. [hata ayıklama modu](https://docs.microsoft.com/azure/data-factory/concepts-data-flow-debug-mode).

    ![Veri akışı hata ayıklaması](media/tutorial-data-flow-private/dataflow-debug.png)
1. **Etkinlikler** bölmesinde, **taşıma ve dönüştürme** Accordion ' ı genişletin. Bölmedeki **veri akışı** etkinliğini, işlem hattı tuvaline sürükleyin ve bırakın.

1. **Veri akışı ekleme** açılır penceresinde **Yeni veri akışı oluştur** ' u seçin ve ardından **veri akışını eşleme**' yi seçin. Bittiğinde **Tamam** ' a tıklayın.

    ![Eşleme Veri Akışı](media/tutorial-data-flow-private/mapping-dataflow.png)

1. Özellikler bölmesinde veri akışı **Transformfilmlerinizi** adlandırın.

## <a name="build-transformation-logic-in-the-data-flow-canvas"></a>Veri akışı tuvalinde dönüştürme mantığı oluşturma

Veri akışınızı oluşturduktan sonra otomatik olarak veri akışı tuvaline gönderilir. Bu adımda, ADLS depolamadaki moviesDB.csv alan bir veri akışı oluşturacaksınız ve 1910 'den 2000 ' e kadar olan ortalama Comedies sayısını toplayacaksınız. Daha sonra bu dosyayı ADLS depolamasına geri yazacaksınız.

### <a name="add-the-source-transformation"></a>Kaynak dönüşümünü ekleme

Bu adımda Azure Data Lake Storage 2. kaynak olarak ayarlarsınız.

1. Veri akışı tuvalinde, **Kaynak Ekle** kutusuna tıklayarak bir kaynak ekleyin.

1. Kaynak **MoviesDB**adlandırın. Yeni kaynak veri kümesi oluşturmak için **Yeni** ' ye tıklayın.

1. **Azure Data Lake Storage 2.** seçin. Devam’a tıklayın.

1. **Delimitedtext**öğesini seçin. Devam’a tıklayın.

1. Veri kümenizi **MoviesDB**olarak adlandırın. Bağlı hizmet açılan menüsünde **Yeni**' yi seçin.

1. Bağlı hizmet oluşturma ekranında, ADLS Gen2 Linked Service **ADLSGen2** ' yi adlandırın ve kimlik doğrulama yönteminizi belirtin. Ardından bağlantı kimlik bilgilerinizi girin. Bu öğreticide, depolama hesabımızla bağlantı kurmak için hesap anahtarı 'nı kullanıyoruz. 

1. **Etkileşimli yazma**özelliğini etkinleştirdiğinizden emin olun. 1 dakika içinde etkinleştirilmesi gerekebilir.

    ![Etkileşimli yazma](./media/tutorial-data-flow-private/interactive-authoring.png)

1. **Bağlantıyı test**et ' i seçtiğinizde, depolama hesabı özel bir uç noktanın oluşturulması ve onaylanması olmadan bu hesaba erişimi etkinleştirmediğinden başarısız olması gerekir. Hata iletisinde, yönetilen özel uç nokta oluşturmak için izleyebileceğiniz **özel bir uç nokta** oluşturmak için bir bağlantı görmeniz gerekir. *Alternatif olarak, Yönet sekmesine doğrudan gitmeniz ve yönetilen özel uç nokta oluşturmak için [Bu bölümdeki](#create-a-managed-private-endpoint) yönergeleri izlemeniz*

1. İletişim kutusunu açık tutun ve yukarıda seçili depolama hesabınıza gidin.

1. Özel bağlantıyı onaylamak için [Bu bölümdeki](#approval-of-a-private-link-in-storage-account) yönergeleri izleyin.

1. İletişim kutusuna geri dönün. **Bağlantıyı yeniden test** edin ve bağlı hizmeti dağıtmak için **Oluştur** ' u seçin.

1. Veri kümesi oluşturma ekranına geri döndüğünüzde, dosyanın **dosya yolu** alanının altında bulunduğu yeri girin. Bu öğreticide, dosya moviesDB.csv kapsayıcı örnek verilerinde bulunur. Dosya üst bilgileri içerdiğinden **ilk satırı üst bilgi olarak**denetleyin. Üst bilgi şemasını depolama alanındaki dosyadan doğrudan içeri aktarmak için **bağlantı/depolama alanından** seçim yapın. Bittiğinde Tamam ' a tıklayın.

    ![Kaynak yol](media/tutorial-data-flow-private/source-file-path.png)

1. Hata ayıklama kümeniz başlatıldıysa, kaynak dönüşümünün **veri önizleme** sekmesine gidin ve verilerin anlık görüntüsünü almak için **Yenile** ' ye tıklayın. Dönüşümün doğru yapılandırıldığını doğrulamak için veri önizlemeyi kullanabilirsiniz.

    ![Veri önizlemesi](media/tutorial-data-flow-private/data-preview.png)

#### <a name="create-a-managed-private-endpoint"></a>Yönetilen özel uç nokta oluşturma

Yukarıdaki bağlantıyı sınarken köprüye tıklamadığınızda aşağıdaki yolu izleyin. Şimdi, yukarıda oluşturulan bağlı hizmete bağlanacak bir yönetilen özel uç noktası oluşturmanız gerekir.

1. Yönet sekmesine gidin.
> [!NOTE]
> Yönet sekmesi, tüm Data Factory örnekleri için kullanılamayabilir. Bunu görmüyorsanız, '**Yazar**' sekmesi aracılığıyla özel uç noktalara erişmeye devam edebilirsiniz--> '**Bağlantılar**'--> '**Özel uç nokta**'
1. Yönetilen özel uç noktalar bölümüne gidin.
1. Yönetilen özel uç noktalar altında **+ Yeni** ' yi seçin.

    ![Yeni yönetilen özel uç nokta](./media/tutorial-data-flow-private/new-managed-private-endpoint.png) 

1. Listeden Azure Data Lake Storage 2. kutucuğunu seçin ve **devam**' ı seçin.
1. Yukarıda oluşturduğunuz depolama hesabının adını girin.
1. **Oluştur**’u seçin.
1. Özel bağlantının oluşturulduğu saniye bekledikten sonra bir onay gerektiğini görmeniz gerekir.
1. Yukarıda oluşturduğunuz özel uç noktayı seçin. Özel uç noktayı, depolama hesabı düzeyinde onaylamaya yol açacak bir köprü görebilirsiniz.

    ![Özel uç noktayı Yönet](./media/tutorial-data-flow-private/manage-private-endpoint.png) 

#### <a name="approval-of-a-private-link-in-storage-account"></a>Depolama hesabındaki özel bağlantının onaylanması

1. Depolama hesabında, ayarlar bölümünde **Özel uç nokta bağlantıları** ' na gidin.

1. Yukarıda oluşturduğunuz özel uç noktayı işaret edin ve **Onayla**' yı seçin.

    ![Özel uç noktayı Onayla](./media/tutorial-data-flow-private/approve-private-endpoint.png)

1. Bir açıklama ekleyin ve **Evet**' e tıklayın.
1. Azure Data Factory ' deki **Yönet** sekmesinin **yönetilen özel uç noktalar** bölümüne geri dönün.
1. Onayın özel uç noktanıza yansıtılmasıyla ilgili olarak yaklaşık 1 dakika sürer.

### <a name="add-the-filter-transformation"></a>Filtre dönüşümünü ekleme

1. Veri akışı tuvalindeki kaynak Düğümünüzün yanında, yeni bir dönüşüm eklemek için artı simgesine tıklayın. Eklemekte olduğunuz ilk dönüşüm bir **filtredir**.

    ![Filtre ekle](media/tutorial-data-flow-private/add-filter.png)
1. Filtre dönüştürmesinin **Filtreyıllarınızı**adlandırın. **Filtre Uygula** ' nın yanındaki ifade kutusuna tıklayıp deyim Oluşturucu 'yu açın. Burada filtreleme koşullarınızı belirtirsiniz.

    ![Yılları filtrele](media/tutorial-data-flow-private/filter-years.png)
1. Veri akışı ifade Oluşturucusu, çeşitli dönüşümlerde kullanılacak ifadeleri etkileşimli bir şekilde oluşturmanıza olanak tanır. İfadeler, yerleşik işlevleri, giriş şemasından sütunları ve Kullanıcı tanımlı parametreleri içerebilir. İfadelerin nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [veri akışı ifade Oluşturucusu](https://docs.microsoft.com/azure/data-factory/concepts-data-flow-expression-builder).

    * Bu öğreticide, 1910 ve 2000 yılları arasında gelen tarz komedi filmlerini filtrelemek istersiniz. Yıl şu anda bir dize olduğu için işlevi kullanarak bir tamsayıya dönüştürmeniz gerekir ```toInteger()``` . 1910 ve 200-sabit yıl değerlerine göre karşılaştırmak için büyüktür veya eşittir (>=) ve küçüktür veya eşittir (<=) işleçlerini kullanın. Bu ifadeleri ve (&&) işleciyle birlikte toplayın. İfade şu şekilde gelir:

        ```toInteger(year) >= 1910 && toInteger(year) <= 2000```

    * Hangi filmlerin ortak olduğunu bulmak için, ```rlike()``` ' komedi ' stilini sütun tarzları içinde bulmak için işlevini kullanabilirsiniz. Şunun için yıl karşılaştırmasına sahip rlıke ifadesini birleşim:

        ```toInteger(year) >= 1910 && toInteger(year) <= 2000 && rlike(genres, 'Comedy')```

    * Etkin bir hata ayıklama kümeniz varsa, kullanılan girişlerle karşılaştırılan ifade çıktısını görmek için **Yenile** ' ye tıklayarak mantığınızı doğrulayabilirsiniz. Veri akışı ifade dilini kullanarak bu mantığı nasıl gerçekleştirebileceğiniz üzerinde birden fazla doğru yanıt vardır.

        ![Filtre ifadesi](media/tutorial-data-flow-private/filter-expression.png)

    * Deyiminizi tamamladıktan sonra Kaydet ' e tıklayın **ve son** ' a tıklayın.

1. Filtrenin düzgün çalıştığını doğrulamak için bir **veri önizlemesi** getirin.

    ![Veri önizlemeyi filtrele](media/tutorial-data-flow-private/filter-data.png)

### <a name="add-the-aggregate-transformation"></a>Toplam dönüşümü ekleme

1. Ekleyeceğiniz bir sonraki dönüşüm, **şema değiştiricisi**altında bir **Toplam** dönüşümdir.

    ![Toplama Ekle](media/tutorial-data-flow-private/add-aggregate.png)
1. Toplam dönüşüm **Aggregatecomedyıderecelendirmeleri**olarak adlandırın. **Gruplandırma ölçütü** sekmesinde, toplamaların filmi filmin geldiği yıla göre gruplamak için açılan listeden **yıl** ' ı seçin.

    ![Toplama grubu](media/tutorial-data-flow-private/group-by-year.png)
1. **Toplamlar** sekmesine gidin. Sol metin kutusunda, toplam sütununu **Averagecomedyıderecelendirme**olarak adlandırın. Deyim Oluşturucu aracılığıyla toplama ifadesini girmek için sağ ifade kutusuna tıklayın.

    ![Birleşik sütun adı](media/tutorial-data-flow-private/name-column.png)
1. Sütun **derecelendirmesinin**ortalamasını almak için ```avg()``` toplama işlevini kullanın. **Derecelendirme** bir dize olduğu ve ```avg()``` sayısal bir girişi aldığı için değeri işlev aracılığıyla bir sayıya dönüştürmemiz gerekir ```toInteger()``` . Bu ifade şöyle görünür:

    ```avg(toInteger(Rating))```

    İşiniz bittiğinde **Kaydet ve son** ' a tıklayın.

    ![Toplamı Kaydet](media/tutorial-data-flow-private/save-aggregate.png)
1. Dönüştürme çıkışını görüntülemek için **veri önizleme** sekmesine gidin. Yalnızca iki sütun varsa, **yıl** ve **Averageıda ortalama derecelendirmesine**dikkat edin.

### <a name="add-the-sink-transformation"></a>Havuz dönüşümünü ekleme

1. Ardından, **hedef**altına bir **Havuz** dönüştürmesi eklemek istersiniz.

    ![Havuz Ekle](media/tutorial-data-flow-private/add-sink.png)
1. Havuz **havuzunuzu**adlandırın. Havuz veri kümenizi oluşturmak için **Yeni** ' ye tıklayın.

    ![Havuz oluştur](media/tutorial-data-flow-private/create-sink.png)
1. Yeni veri kümesi sayfasında **Azure Data Lake Storage 2.**' yi seçin. Devam’a tıklayın.

1. Biçim Seç sayfasında, **Delimitedtext**' i seçin. Devam’a tıklayın.

1. Havuz veri kümenizi **MoviesSink**olarak adlandırın. Bağlı hizmet için, kaynak dönüştürmesi için oluşturduğunuz ADLSGen2 bağlı hizmeti seçin. Verilerinizi yazmak için bir çıkış klasörü girin. Bu öğreticide, ' Sample-Data ' kapsayıcısının ' output ' klasörüne yazıyoruz. Klasörün önceden var olması gerekmez ve dinamik olarak oluşturulabilir. **İlk satırı üst bilgi olarak** ayarlayın ve **içeri aktarma şeması**için **hiçbiri** ' ni seçin. Tamam'a tıklayın.

    ![Havuz yolu](media/tutorial-data-flow-private/sink-file-path.png)

Artık veri akışınızı oluşturmayı tamamladınız. İşlem hattınızda çalıştırmak için hazırsınız.

## <a name="running-and-monitoring-the-data-flow"></a>Veri akışını çalıştırma ve izleme

Yayımlamadan önce bir işlem hattında hata ayıklaması yapabilirsiniz. Bu adımda, veri akışı işlem hattının hata ayıklama çalıştırmasını tetikleyeceksiniz. Veri önizleme verileri yazmazsa, bir hata ayıklama çalıştırması havuz Hedefinizdeki verileri yazar.

1. İşlem hattı tuvaline gidin. Hata ayıklama çalıştırmasını tetiklemek için **Hata Ayıkla** ' ya tıklayın.

1. Veri akışı etkinliklerinin ardışık düzen hata ayıklaması, etkin hata ayıklama kümesini kullanır, ancak yine de başlatmak için en az bir dakika sürer. İlerlemeyi **Çıkış** sekmesi aracılığıyla izleyebilirsiniz. Çalıştırma başarılı olduktan sonra, çalışma ayrıntıları için göz gözlük simgesine tıklayın.

1. Ayrıntılar sayfasında, her bir dönüştürme adımında harcanan satır sayısını ve süreyi görebilirsiniz.

    ![İzleme çalıştırması](media/tutorial-data-flow-private/run-details.png)
1. Verilerin sütunları ve bölümlenmesi hakkında ayrıntılı bilgi almak için bir dönüşüme tıklayın.

Bu öğreticiyi izlediyseniz, havuz klasörünüze 83 satır ve 2 sütun yazmış olmanız gerekir. BLOB depolama alanınızı denetleyerek verilerin doğru olduğunu doğrulayabilirsiniz.

## <a name="summary"></a>Özet

Bu öğreticide, bir Azure Data Lake Storage (ADLS) Gen2 kaynağından verileri kopyalayan ve dönüştüren bir işlem hattı oluşturmak için Azure Data Factory Kullanıcı arabirimini (UX), [Azure Data Factory yönetilen sanal ağdaki](managed-virtual-network-private-endpoint.md)veri akışını eşleme kullanarak bir **ADLS 2. havuzuna (her ikisi de yalnızca seçilen ağlara erişime izin vermek)** kullanacaksınız.
