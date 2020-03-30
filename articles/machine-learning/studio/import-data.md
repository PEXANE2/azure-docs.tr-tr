---
title: Eğitim verilerini içeri aktarma
titleSuffix: ML Studio (classic) - Azure
description: Verilerinizi çeşitli veri kaynaklarından Azure Machine Learning Studio'ya (klasik) nasıl aktarabilirsiniz? Hangi veri türlerinin ve veri biçimlerinin desteklenedildiğini öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 02/01/2019
ms.openlocfilehash: cee49124a7547399889e425008a8580b9b25945a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79217989"
---
# <a name="import-your-training-data-into-azure-machine-learning-studio-classic-from-various-data-sources"></a>Eğitim verilerinizi çeşitli veri kaynaklarından Azure Machine Learning Studio'ya (klasik) aktarın

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Tahmine dayalı bir analitik çözümü geliştirmek ve eğitmek için Machine Learning Studio'da (klasik) kendi verilerinizi kullanmak için şu verilerden veri kullanabilirsiniz: 

* **Yerel dosya** - Çalışma alanınızda bir veri kümesi modülü oluşturmak için yerel verileri sabit sürücünüzden önceden yükleyin
* **Çevrimiçi veri kaynakları** - Denemeniz çalışırken birkaç çevrimiçi kaynaktan birinden gelen verilere erişmek için [Veri Alma][import-data] modüllerini kullanın
* **Machine Learning Studio (klasik) deneyi** - Machine Learning Studio'da veri seti olarak kaydedilen verileri kullanın (klasik)
* [**Şirket içi SQL Server veritabanı**](use-data-from-an-on-premises-sql-server.md) - Verileri el ile kopyalamak zorunda kalmadan şirket içi SQL Server veritabanındaki verileri kullanın

> [!NOTE]
> Machine Learning Studio'da (klasik) eğitim verileri için kullanabileceğiniz bir dizi örnek veri kümesi vardır. Bunlar hakkında daha fazla bilgi için [bkz.](use-sample-datasets.md)

## <a name="prepare-data"></a>Verileri hazırlama

Machine Learning Studio (klasik), bazı durumlarda dikdörtgen olmayan veriler kullanılsa da, veritabanından sınırlı veya yapılandırılmış veri içeren metin verileri gibi dikdörtgen veya düz verilerle çalışmak üzere tasarlanmıştır.

Studio (klasik) içine almadan önce veri nispeten temiz olması en iyisidir. Örneğin, tırnak içinde olmayan dizeleri gibi sorunları halletmek isteyeceksiniz.

Ancak, Studio'da (klasik) verilerinizi aldıktan sonra denemenizdeki verilerin bazı şekilde manipüle edilmesini sağlayan modüller vardır. Kullanacağınız makine öğrenimi algoritmalarına bağlı olarak, eksik değerler ve seyrek veriler gibi veri yapısal sorunlarını nasıl işleyeceğinizkonusunda karar vermeniz gerekebilir ve bu konuda yardımcı olabilecek modüller vardır. Bu işlevleri gerçekleştiren modüller için modül paletinin **Veri Dönüşümü** bölümüne bakın.

Denemenizin herhangi bir noktasında, çıkış bağlantı noktasını tıklatarak bir modül tarafından üretilen verileri görüntüleyebilir veya indirebilirsiniz. Modüle bağlı olarak, farklı indirme seçenekleri olabilir veya Studio'da (klasik) web tarayıcınızdaki verileri görselleştirebilirsiniz.

## <a name="supported-data-formats-and-data-types"></a>Desteklenen veri biçimleri ve veri türleri

Verileri almak için hangi mekanizmayı kullandığınıza ve nereden geldiğine bağlı olarak denemenize bir dizi veri türü aktarabilirsiniz:

* Düz metin (.txt)
* Virgülle ayrılmış değerler (CSV) üstbilgi (.csv) veya olmadan (.nh.csv)
* Üstbilgi (.tsv) veya (.nh.tsv) olmayan sekme ayrılmış değerler (TSV)
* Excel dosyası
* Azure tablosu
* Kovan masası
* SQL veritabanı tablosu
* OData değerleri
* SVMLight verileri (.svmlight) (biçim bilgileri için [SVMLight tanımına](http://svmlight.joachims.org/) bakın)
* Öznitelik İlişkisi Dosya Biçimi (ARFF) verileri (.arff) (biçim bilgileri için [ARFF tanımına](https://weka.wikispaces.com/ARFF) bakın)
* Zip dosyası (.zip)
* R nesnesi veya çalışma alanı dosyası (. RData)

Meta verileri içeren ARFF gibi bir biçimde veri alıyorsanız, Studio (klasik) her sütunun başlık ve veri türünü tanımlamak için bu meta verileri kullanır.

Bu meta verileri içermeyen TSV veya CSV biçimi gibi verileri içe aktarıyorsanız, Studio (klasik) verileri örnekleyerek her sütun için veri türünü çıkartır. Verilerde sütun başlıkları da yoksa, Studio (klasik) varsayılan adlar sağlar.

[Meta verileri edit][edit-metadata] modülünü kullanarak sütunların başlıklarını ve veri türlerini açıkça belirtebilir veya değiştirebilirsiniz.

Aşağıdaki veri türleri Studio (klasik) tarafından tanınır:

* Dize
* Tamsayı
* Çift
* Boole
* DateTime
* TimeSpan

Studio, modüller arasında veri aktarmak için ***veri tablosu*** adı verilen bir dahili veri türü kullanır. [Dataset'e Dönüştür][convert-to-dataset] modüllerini kullanarak verilerinizi açıkça veri tablosu biçimine dönüştürebilirsiniz.

Veri tablosu dışındaki biçimleri kabul eden herhangi bir modül, verileri bir sonraki modüle geçirmeden önce sessizce veri tablosuna dönüştürür.

Gerekirse, diğer dönüşüm modüllerini kullanarak veri tablosu biçimini CSV, TSV, ARFF veya SVMLight biçimine dönüştürebilirsiniz.
Bu işlevleri gerçekleştiren modüller için modül paletinin **Veri Biçimi Dönüşümleri** bölümüne bakın.

## <a name="data-capacities"></a>Veri kapasiteleri

Machine Learning Studio'daki (klasik) modüller, yaygın kullanım durumları için 10 GB'a kadar yoğun sayısal veri veri kümelerini destekler. Bir modülün birden fazla giriş aldığı durumlarda 10 GB değeri tüm giriş boyutlarının toplamıdır. Hive veya Azure SQL Veritabanı'ndaki sorguları kullanarak daha büyük veri kümelerini örnekleyebilir veya verileri almadan önce Öğrenme yi Saymalar adedine göre önceden işleme yi kullanabilirsiniz.  

Aşağıdaki veri türleri, özellik normalleştirme sırasında daha büyük veri kümelerine genişleyebilir ve boyutu 10 GB’den az olacak şekilde sınırlıdır:

* Seyrek
* Kategorik
* Dizeler
* İkili veriler

Aşağıdaki modüller, boyutu 10 GB'den az veri kümeleriyle sınırlıdır:

* Öneren modüller
* Synthetic Minority Oversampling Technique (SMOTE) modülü
* Betik modülleri: R, Python, SQL
* Katılma veya Özellik Karma gibi çıkış veri boyutunun giriş veri boyutundan büyük olabileceği modüller
* Yineleme sayısının çok büyük olduğu durumlarda Çapraz doğrulama, Model Ayarlama Hiperparametreleri, Sıralı Regresyon ve Tek veya Tüm Çoklu Sınıflar

Birkaç GB'den daha büyük veri kümeleri için verileri Azure Depolama veya Azure SQL Veritabanı'na yükleyin veya doğrudan yerel bir dosyadan yüklemek yerine Azure HDInsight'ı kullanın.

Görüntü verileri hakkında bilgileri [Alma Görüntüleri](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/import-images#bkmk_Notes) modülü referansında bulabilirsiniz.

## <a name="import-from-a-local-file"></a>Yerel bir dosyadan alma

Studio'da (klasik) eğitim verisi olarak kullanmak için sabit diskinizden bir veri dosyası yükleyebilirsiniz. Bir veri dosyası içe aktarırken, çalışma alanınızdaki denemelerde kullanıma hazır bir veri kümesi modülü oluşturursunuz.

Yerel bir sabit diskten veri almak için aşağıdakileri yapın:

1. Studio (klasik) penceresinin altındaki **+Yenİ'yi** tıklatın.
2. **DATASET** ve **YEREL DOSYADAN'ı**seçin.
3. Yeni **bir veri seti yükle** iletişim kutusunda, yüklemek istediğiniz dosyaya göz atın.
4. Bir ad girin, veri türünü tanımlayın ve isteğe bağlı olarak bir açıklama girin. Bir açıklama önerilir - gelecekte verileri kullanırken hatırlamak istediğiniz verilerle ilgili tüm özellikleri kaydetmenize olanak tanır.
5. Onay kutusu **Varolan bir veri kümesinin yeni sürümüdür,** varolan bir veri kümesini yeni verilerle güncelleştirmenize olanak tanır. Bunu yapmak için bu onay kutusunu tıklatın ve ardından varolan bir veri kümesinin adını girin.

![Yeni bir veri kümesi yükleme](./media/import-data/upload-dataset-from-local-file.png)

Yükleme süresi, verilerinizin boyutuna ve hizmete olan bağlantınızın hızına bağlıdır. Dosyanın uzun zaman alacağını biliyorsanız, beklerken Studio (klasik) içinde başka şeyler yapabilirsiniz. Ancak, veri yüklemesi tamamlanmadan tarayıcıyı kapatmak yüklemenin başarısız olmasının nedenidir.

Verileriniz yüklendikten sonra, bir veri kümesi modülünde depolanır ve çalışma alanınızdaki tüm denemeler için kullanılabilir.

Bir denemeyi düzenlerken, yüklediğiniz veri kümelerini modül paletinde Kayıtlı Veri **Kümeleri** listesinin altında **Veri Kümelerim** listesinde bulabilirsiniz. Daha fazla analitik ve makine öğrenimi için veri kümesini kullanmak istediğinizde veri kümesini sürükleyip deneme tuvaline bırakabilirsiniz.

## <a name="import-from-online-data-sources"></a>Çevrimiçi veri kaynaklarından alma

[Denemeniz Veri Alma][import-data] modüllerini kullanarak, deneme çalışırken çeşitli çevrimiçi veri kaynaklarından veri aktarabilir.

> [!NOTE]
> Bu [makalede, Alma Verileri][import-data] modülü hakkında genel bilgiler verilmektedir. Erişebileceğiniz veri türleri, biçimler, parametreler ve sık sorulan soruların yanıtları hakkında daha ayrıntılı bilgi [için, İçe İşlem Verileri][import-data] modülü için modül başvuru konusuna bakın.

[Veri Alma][import-data] modüllerini kullanarak, denemeniz çalışırken birkaç çevrimiçi veri kaynağından verilere erişebilirsiniz:

* HTTP'yi kullanan bir Web URL'si
* HiveQL kullanarak Hadoop
* Azure blob depolama
* Azure tablosu
* Azure VM'de Azure SQL veritabanı veya SQL Server
* Şirket içi SQL Server veritabanı
* Bir veri akışı sağlayıcısı, OData şu anda
* Azure Cosmos DB

Bu eğitim verilerine denemeniz çalışırken erişilenbu değer olduğundan, yalnızca bu denemede kullanılabilir. Buna karşılık, bir veri kümesi modülünde depolanan veriler çalışma alanınızdaki tüm denemeler için kullanılabilir.

Studio (klasik) denemenizde çevrimiçi veri kaynaklarına erişmek için denemenize [Veri Aktar][import-data] modüllerini ekleyin. Ardından, veri kaynağını seçmek ve yapılandırmak için adım adım kılavuzlu yönergeler için **Özellikler** altında **Veri Al'ı Başlat'ı** seçin. Alternatif olarak, **Özellikler** altında **Veri kaynağını** el ile seçebilir ve verilere erişmek için gereken parametreleri sağlayabilirsiniz.

Desteklenen çevrimiçi veri kaynakları aşağıdaki tabloda açıklanmıştır. Bu tablo, desteklenen dosya biçimlerini ve verilere erişmek için kullanılan parametreleri de özetler.

> [!IMPORTANT]
> Şu anda, [Veri Ve][import-data] [Dışa Aktarma Verileri][export-data] modülleri verileri yalnızca Klasik dağıtım modeli kullanılarak oluşturulan Azure depolamasından okuyabilir ve yazabilir. Başka bir deyişle, sıcak depolama erişim katmanı veya havalı depolama erişim katmanı sunan yeni Azure Blob Depolama hesabı türü henüz desteklenmedi.
>
> Genellikle, bu hizmet seçeneği kullanıma sunulmadan önce oluşturmuş olabileceğiniz Azure depolama hesapları etkilenmemelidir.
> Yeni bir hesap oluşturmanız gerekiyorsa, Dağıtım modeli için **Klasik'i** seçin veya Kaynak yöneticisini kullanın ve **Hesap türü**için **Blob depolama alanı** yerine Genel **amaçlı'yı** seçin.
>
> Daha fazla bilgi için Azure [Blob Depolama: Sıcak ve Serin Depolama Katmanları'na](../../storage/blobs/storage-blob-storage-tiers.md)bakın.

### <a name="supported-online-data-sources"></a>Desteklenen çevrimiçi veri kaynakları
Azure Machine Learning Studio (klasik) **Alma Verimodülü** aşağıdaki veri kaynaklarını destekler:

| veri kaynağı | Açıklama | Parametreler |
| --- | --- | --- |
| HTTP üzerinden Web URL |Virgülle ayrılmış değerler (CSV), sekme ayrılmış değerler (TSV), öznitelik ilişkisi dosya biçimi (ARFF) ve Destek Vektör Makineleri (SVM-ışık) biçimlerindeki verileri HTTP kullanan herhangi bir web URL'sinden okur |<b>URL</b>: Site URL'si ve dosya adı da dahil olmak üzere dosyanın tam adını herhangi bir uzantıyla belirtir. <br/><br/><b>Veri biçimi</b>: Desteklenen veri biçimlerinden birini belirtir: CSV, TSV, ARFF veya SVM-light. Verilerin üstbilgi satırı varsa, sütun adlarını atamak için kullanılır. |
| Hadoop/HDFS |Hadoop'taki dağıtılmış depolamadan gelen verileri okur. SQL benzeri bir sorgu dili olan HiveQL kullanarak istediğiniz verileri belirtirsiniz. HiveQL, verileri Studio'ya (klasik) eklemeden önce verileri toplamak ve veri filtreleme gerçekleştirmek için de kullanılabilir. |<b>Hive veritabanı sorgusu</b>: Verileri oluşturmak için kullanılan Hive sorgusunu belirtir.<br/><br/><b>HCatalog server URI</b> : * &lt;Küme&gt;adınızı .azurehdinsight.net biçimini kullanarak kümenizin adını belirtir.*<br/><br/><b>Hadoop kullanıcı hesabı adı</b>: Kümeyi sağlamak için kullanılan Hadoop kullanıcı hesap adını belirtir.<br/><br/><b>Hadoop kullanıcı hesabı parolası</b> : Kümeyi sağlarken kullanılan kimlik bilgilerini belirtir. Daha fazla bilgi için [HDInsight'ta Hadoop kümeleri oluştur'a](/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters)bakın.<br/><br/><b>Çıktı verilerinin konumu</b>: Verilerin Hadoop dağıtılmış dosya sisteminde (HDFS) mi yoksa Azure'da mı depolandığını belirtir. <br/><ul>Çıktı verilerini HDFS'de depolarsanız, HDFS sunucusu URI'yi belirtin. (HTTPS:// öneki olmadan HDInsight küme adını kullandığınızdan emin olun). <br/><br/>Çıktı verilerinizi Azure'da depolarsanız, Azure depolama hesabı adını, Depolama erişim anahtarını ve Depolama kapsayıcı adını belirtmeniz gerekir.</ul> |
| SQL veritabanı |Azure SQL veritabanında veya Azure sanal makinesinde çalışan bir SQL Server veritabanında depolanan verileri okur. |<b>Veritabanı sunucu adı</b>: Veritabanının çalıştırıldığı sunucunun adını belirtir.<br/><ul>Azure SQL Veritabanı durumunda oluşturulan sunucu adını girin. Genellikle * &lt;&gt;generated_identifier .database.windows.net biçimine sahiptir.* <br/><br/>Azure Sanal makinede barındırılan bir SQL sunucusu durumunda *tcp&lt;&gt;girin: Virtual Machine DNS Adı , 1433*</ul><br/><b>Veritabanı adı </b>: Sunucudaki veritabanının adını belirtir. <br/><br/><b>Sunucu kullanıcı hesabı adı</b>: Veritabanı için erişim izinleri olan bir hesap için kullanıcı adı belirtir. <br/><br/><b>Sunucu kullanıcı hesabı parolası</b>: Kullanıcı hesabının parolasını belirtir.<br/><br/><b>Veritabanı sorgusu</b>:Okumak istediğiniz verileri açıklayan bir SQL deyimi girin. |
| Şirket içi SQL veritabanı |Şirket içi BIR SQL veritabanında depolanan verileri okur. |<b>Veri ağ geçidi</b>: SQL Server veritabanınıza erişebileceği bir bilgisayarda yüklü olan Veri Yönetimi Ağ Geçidi'nin adını belirtir. Ağ geçidini ayarlama hakkında daha fazla bilgi için, [şirket içi bir SQL sunucusundan gelen verileri kullanarak Azure Machine Learning Studio (klasik) ile gelişmiş analizler gerçekleştirin'e](use-data-from-an-on-premises-sql-server.md)bakın.<br/><br/><b>Veritabanı sunucu adı</b>: Veritabanının çalıştırıldığı sunucunun adını belirtir.<br/><br/><b>Veritabanı adı </b>: Sunucudaki veritabanının adını belirtir. <br/><br/><b>Sunucu kullanıcı hesabı adı</b>: Veritabanı için erişim izinleri olan bir hesap için kullanıcı adı belirtir. <br/><br/><b>Kullanıcı adı ve şifre</b>: Veritabanı kimlik bilgilerinizi girmek için <b>değerleri girin'i</b> tıklatın. Şirket içi SQL Server'ınızın nasıl yapılandırıldığına bağlı olarak Windows Tümleşik Kimlik Doğrulama veya SQL Server Kimlik Doğrulaması'nı kullanabilirsiniz.<br/><br/><b>Veritabanı sorgusu</b>:Okumak istediğiniz verileri açıklayan bir SQL deyimi girin. |
| Azure Tablosu |Azure Depolama'daki Tablo hizmetinden gelen verileri okur.<br/><br/>Büyük miktarda veriyi seyrek okuyorsanız, Azure Tablo Hizmeti'ni kullanın. Esnek, ilişkisel olmayan (NoSQL), büyük ölçüde ölçeklenebilir, ucuz ve yüksek kullanılabilir depolama çözümü sağlar. |**İçe Aktar** Verisi'ndeki seçenekler, herkese açık bilgilere mi yoksa giriş kimlik bilgilerini gerektiren özel bir depolama hesabına mı erişiyor olduğunuza bağlı olarak değişir. Bu, her biri kendi parametre kümesine sahip olan "PublicOrSAS" veya "Account" değerine sahip olan <b>Kimlik Doğrulama Türü</b> tarafından belirlenir. <br/><br/><b>Ortak veya Paylaşılan Erişim İmzası (SAS) URI</b>: Parametreler şunlardır:<br/><br/><ul><b>Tablo URI</b>: Tablo için Genel veya SAS URL'sini belirtir.<br/><br/><b>Özellik adlarını tarayan satırları belirtir:</b>Belirtilen satır sayısını tarayan <i>topn</i> veya tablodaki tüm satırları almak için <i>ScanAll</i> değerleridir. <br/><br/>Veriler homojen ve öngörülebilirse, *TopN'u* seçmeniz ve N için bir sayı girmeniz önerilir. Büyük tablolar için bu, daha hızlı okuma sürelerine neden olabilir.<br/><br/>Veriler, tablonun derinliğine ve konumuna bağlı olarak değişen özellik kümeleriyle yapılandırılmışsa, tüm satırları tarayıp *ScanAll* seçeneğini belirleyin. Bu, ortaya çıkan özelliğinizin ve meta veri dönüştürmenizin bütünlüğünü sağlar.<br/><br/></ul><b>Özel Depolama Hesabı</b>: Parametreler şunlardır: <br/><br/><ul><b>Hesap adı</b>: Okunacak tabloyu içeren hesabın adını belirtir.<br/><br/><b>Hesap anahtarı</b>: Hesapla ilişkili depolama anahtarını belirtir.<br/><br/><b>Tablo adı</b> : Okunacak verileri içeren tablonun adını belirtir.<br/><br/><b>Özellik adlarını tarayan satırlar</b>: Belirtilen satır sayısını tarayan <i>TopN</i> veya tablodaki tüm satırları almak için <i>ScanAll'dır.</i><br/><br/>Veriler homojen ve öngörülebilirse, *TopN'u* seçmenizi ve N için bir sayı girmenizi öneririz. Büyük tablolar için bu, daha hızlı okuma sürelerine neden olabilir.<br/><br/>Veriler, tablonun derinliğine ve konumuna bağlı olarak değişen özellik kümeleriyle yapılandırılmışsa, tüm satırları tarayıp *ScanAll* seçeneğini belirleyin. Bu, ortaya çıkan özelliğinizin ve meta veri dönüştürmenizin bütünlüğünü sağlar.<br/><br/> |
| Azure Blob Depolama |Görüntüler, yapılandırılmamış metin veya ikili veriler de dahil olmak üzere Azure Depolama'daki Blob hizmetinde depolanan verileri okur.<br/><br/>Verileri herkese açık olarak ortaya çıkarmak veya uygulama verilerini özel olarak depolamak için Blob hizmetini kullanabilirsiniz. HTTP veya HTTPS bağlantılarını kullanarak verilerinize her yerden erişebilirsiniz. |**Veri Alma** modülündeki seçenekler, herkese açık bilgilere mi yoksa giriş kimlik bilgilerini gerektiren özel bir depolama hesabına mı erişiyor olduğunuza bağlı olarak değişir. Bu, "PublicOrSAS" veya "Account" değerine sahip olabilecek <b>Kimlik Doğrulama Türü</b> tarafından belirlenir.<br/><br/><b>Ortak veya Paylaşılan Erişim İmzası (SAS) URI</b>: Parametreler şunlardır:<br/><br/><ul><b>URI</b>: Depolama blob için Genel veya SAS URL'sini belirtir.<br/><br/><b>Dosya Biçimi</b>: Blob hizmetindeki verilerin biçimini belirtir. Desteklenen biçimler CSV, TSV ve ARFF'dir.<br/><br/></ul><b>Özel Depolama Hesabı</b>: Parametreler şunlardır: <br/><br/><ul><b>Hesap adı</b>: Okumak istediğiniz blob içeren hesabın adını belirtir.<br/><br/><b>Hesap anahtarı</b>: Hesapla ilişkili depolama anahtarını belirtir.<br/><br/><b>Kapsayıcıya, dizin veya blob yolu</b> : Okunacak verileri içeren blob'un adını belirtir.<br/><br/><b>Blob dosya biçimi</b>: Blob hizmetindeki verilerin biçimini belirtir. Desteklenen veri biçimleri CSV, TSV, ARFF, CSV ve Excel'dir. <br/><br/><ul>Biçim CSV veya TSV ise, dosyanın üstbilgi satırı bulunup içermediğini belirttiğinden emin olun.<br/><br/>Excel çalışma kitaplarındaki verileri okumak için Excel seçeneğini kullanabilirsiniz. Excel <i>veri biçimi</i> seçeneğinde, verilerin Excel çalışma sayfası aralığında mı yoksa Excel tablosunda mı olduğunu belirtin. Excel <i>sayfası veya katıştırılmış tablo </i>seçeneğinde, okumak istediğiniz sayfanın veya tablonun adını belirtin.</ul><br/> |
| Veri Akışı Sağlayıcısı |Desteklenen bir akış sağlayıcısından gelen verileri okur. Şu anda yalnızca Açık Veri Protokolü (OData) biçimi desteklenmektedir. |<b>Veri içeriği türü</b>: OData biçimini belirtir.<br/><br/><b>Kaynak URL</b>: Veri akışı için tam URL'yi belirtir. <br/>Örneğin, Northwind örnek veritabanından aşağıdaki URL okur:https://services.odata.org/northwind/northwind.svc/ |

## <a name="import-from-another-experiment"></a>Başka bir denemeden alma

Bir denemeden ara sonuç alıp başka bir denemenin parçası olarak kullanmak isteyeceğiniz zamanlar olacaktır. Bunu yapmak için modülü veri kümesi olarak kaydedebilirsiniz:

1. Veri kümesi olarak kaydetmek istediğiniz modülün çıktısını tıklatın.
2. **Veri Kümesi Olarak Kaydet'i**tıklatın.
3. İstendiğinde, veri kümesini kolayca tanımlamanızı sağlayacak bir ad ve açıklama girin.
4. **Tamam** onay işaretini tıklatın.

Kaydetme bittiğinde, veri kümesi çalışma alanınızdaki herhangi bir denemede kullanılabilir. **Kaydedilmiş Veri Setleri** listesinde modül paletinde bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Veri Alma ve Veri Verme modüllerini kullanan Azure Machine Learning Studio web hizmetlerini dağıtma](web-services-that-use-import-export-modules.md)


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/


<!-- Module References -->
[convert-to-dataset]: https://msdn.microsoft.com/library/azure/72bf58e0-fc87-4bb1-9704-f1805003b975/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
