---
title: Azure Machine Learning - Team Data Science Process için senaryoları tanımlama
description: Team Data Science Process ile Tahmine dayalı analiz Gelişmiş yapmak için uygun senaryoları seçin.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 48b51c40e5de8f10d9d1d16b02e2c70b045816b3
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76710482"
---
# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a>Azure Machine Learning’de gelişmiş analiz senaryoları
Bu makalede, [ekip veri bilimi işlemi (TDSP)](overview.md)tarafından işlenebilen çeşitli örnek veri kaynakları ve hedef senaryolar özetlenmektedir. TDSP ekiplerin akıllı uygulamalar oluşturmaya birlikte çalışması sistematik bir yaklaşım sağlar. Burada sunulan senaryoları veri özelliklerine, kaynak konumları ve hedef Azure depolarında bağlıdır veri işleme iş akışı içinde kullanılabilir seçenekler gösterilmektedir.

Verilerinize ve amacınıza uygun örnek senaryolar seçmeye yönelik **karar ağacı** , son bölümde sunulur.

Aşağıdaki bölümlerde, bir örnek senaryo sunar. Her senaryo, olası veri bilimi veya Gelişmiş analiz için akışı ve destekleyici Azure kaynakları listelenir.

> [!NOTE]
> **Aşağıdaki senaryoların tümü için şunları yapmanız gerekir:**
> <br/>
> 
> * [Depolama hesabı oluşturma](../../storage/common/storage-account-create.md)
>   <br/>
> * [Azure Machine Learning çalışma alanı oluşturma](../studio/create-workspace.md)
> 
> 

## <a name="smalllocal"></a>Senaryo \#1: yerel dosyalardaki küçük ve orta ölçekli tablo veri kümesi
![Küçük ve orta yerel dosyaları][1]

#### <a name="additional-azure-resources-none"></a>Ek Azure kaynaklarını: yok
1. [Azure Machine Learning Studio](https://studio.azureml.net/)oturum açın.
1. Bir veri kümesi karşıya yükleyin.
1. Karşıya yüklenen veri kümeleri ile başlayan bir Azure Machine Learning deneme akışı oluşturun.

## <a name="smalllocalprocess"></a>Senaryo \#2: yerel dosyaların işlenmesi gereken küçük ve orta ölçekli veri kümesi
![Küçük ve orta yerel dosyaları işleme][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Ek Azure kaynaklarını: Azure sanal makinesi (Ipython not defteri sunucusu)
1. Ipython Notebook çalıştıran bir Azure sanal makinesi oluşturun.
1. Verileri bir Azure depolama kapsayıcısına yükleyin.
1. IPython not defterindeki verileri Azure depolama kapsayıcısından erişen, önceden işleyin ve temizleyin.
1. Verileri temizlenmiş bir tablo biçimine dönüştürme.
1. Dönüştürülmüş verileri Azure blob'larda kaydedin.
1. [Azure Machine Learning Studio](https://studio.azureml.net/)oturum açın.
1. [Verileri Içeri aktarma][import-data] modülünü kullanarak Azure Bloblarındaki verileri okuyun.
1. Alınan veri kümelerindeki ile başlayan bir Azure Machine Learning deneme akışı oluşturun.

## <a name="largelocal"></a>Senaryo \#3: yerel dosyaların büyük veri kümesi, Azure Bloblarını hedefleme
![Büyük yerel dosyaları][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Ek Azure kaynaklarını: Azure sanal makinesi (Ipython not defteri sunucusu)
1. Ipython Notebook çalıştıran bir Azure sanal makinesi oluşturun.
1. Verileri bir Azure depolama kapsayıcısına yükleyin.
1. Önceden işleme ve verileri Azure bloblarından veri erişimi, Ipython Notebook temizleyin.
1. Gerekirse, verileri temizlenmiş tablolu bir forma dönüştürün.
1. Verileri araştırmak ve gerektiği gibi özellikler oluşturun.
1. Küçük ve orta veri örneği ayıklayın.
1. Örneklenen verileri Azure blob'larda kaydedin.
1. [Azure Machine Learning Studio](https://studio.azureml.net/)oturum açın.
1. [Verileri Içeri aktarma][import-data] modülünü kullanarak Azure Bloblarındaki verileri okuyun.
1. Alınan veri kümelerindeki ile başlayan bir Azure Machine Learning deneme akış oluşturacaksınız.

## <a name="smalllocaltodb"></a>Senaryo \#4: yerel dosyaların Azure sanal makinesinde SQL Server hedefleme için küçük ve orta ölçekli veri kümesi
![Küçük ve orta yerel dosyaları Azure SQL DB'ye][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Ek Azure kaynaklarını: Azure sanal makinesi (SQL Server / Ipython not defteri sunucusu)
1. SQL Server + Ipython Notebook çalıştıran bir Azure sanal makinesi oluşturun.
1. Verileri bir Azure depolama kapsayıcısına yükleyin.
1. IPython Not defteri 'ni kullanarak Azure depolama kapsayıcısında önceden işlem yapın ve verileri temizleyin.
1. Gerekirse, verileri temizlenmiş tablolu bir forma dönüştürün.
1. VM yerel dosyaya veri kaydetme (Ipython Notebook VM üzerinde çalışan, yerel sürücülere VM sürücülere bakın).
1. Verileri bir Azure sanal makinesinde çalışan SQL Server veritabanına yükleyin.
   
   Seçenek \#1: SQL Server Management Studio kullanma.
   
   * SQL Server VM oturum açın
   * SQL Server Management Studio'da çalıştırın.
   * Veritabanı ve hedef tablolar oluşturun.
   * Verileri VM yerel dosyadan yüklemek için yöntemleri içe toplu birini kullanın.
   
   Seçenek \#2: IPython Not defterini kullanma – orta ve daha büyük veri kümeleri için önerilmez
   
   <!-- -->    
   * ODBC bağlantı dizesi, SQL Server VM üzerinde erişmek için kullanın.
   * Veritabanı ve hedef tablolar oluşturun.
   * Verileri VM yerel dosyadan yüklemek için yöntemleri içe toplu birini kullanın.
1. Verileri keşfedin, gerektiği gibi özellikler oluşturun. Özelliklerin veritabanı tablolarında gerçekleştirilmiş olması gerekmez. Yalnızca bunları oluşturmak için gerekli sorguyu unutmayın.
1. Gerekli ve/veya istenen veri örnek boyutuna karar.
1. [Azure Machine Learning Studio](https://studio.azureml.net/)oturum açın.
1. Verileri [Içeri aktarma][import-data] modülünü kullanarak doğrudan SQL Server verileri okuyun. Doğrudan [Içeri aktarma verileri][import-data] sorgusunda alanları çıkaran, özellik oluşturan ve örnek verileri içeren gerekli sorguyu yapıştırın.
1. Alınan veri kümelerindeki ile başlayan bir Azure Machine Learning deneme akış oluşturacaksınız.

## <a name="largelocaltodb"></a>Senaryo \#5: yerel dosyalardaki büyük veri kümesi, Azure VM 'de hedef SQL Server
![Azure SQL DB'de büyük yerel dosyalar][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Ek Azure kaynaklarını: Azure sanal makinesi (SQL Server / Ipython not defteri sunucusu)
1. SQL Server ve Ipython Notebook sunucusu çalıştıran bir Azure sanal makinesi oluşturun.
1. Verileri bir Azure depolama kapsayıcısına yükleyin.
1. (İsteğe bağlı) Önceden işleme ve verileri temizledik.
   
    a.  Önceden işleme ve verileri Azure bloblarından veri erişimi, Ipython Notebook temizleyin.
   
    b.  Gerekirse, verileri temizlenmiş tablolu bir forma dönüştürün.
   
    c.  VM yerel dosyaya veri kaydetme (Ipython Notebook VM üzerinde çalışan, yerel sürücülere VM sürücülere bakın).
1. Verileri bir Azure sanal makinesinde çalışan SQL Server veritabanına yükleyin.
   
    a.  SQL Server VM oturum açın.
   
    b.  Veriler henüz kaydedilmediğinden, Azure depolama kapsayıcısından yerel VM klasörüne veri dosyalarını indirin.
   
    c.  SQL Server Management Studio'da çalıştırın.
   
    d.  Veritabanı ve hedef tablolar oluşturun.
   
    e.  Verileri yüklemek için yöntemleri içe toplu birini kullanın.
   
    f.  Tablo birleştirmelerde gerekiyorsa, birleştirmeler hızlandırmak için dizin oluşturun.
   
   > [!NOTE]
   > Büyük boyutlu veriler için daha hızlı yüklenmesini bölümlenmiş tablolar oluşturmak ve toplu verileri paralel alma önerilir. Daha fazla bilgi için bkz. [SQL bölümlenmiş tablolara paralel veri aktarma](parallel-load-sql-partitioned-tables.md).
   > 
   > 
1. Verileri keşfedin, gerektiği gibi özellikler oluşturun. Özelliklerin veritabanı tablolarında gerçekleştirilmiş olması gerekmez. Yalnızca bunları oluşturmak için gerekli sorguyu unutmayın.
1. Gerekli ve/veya istenen veri örnek boyutuna karar.
1. [Azure Machine Learning Studio](https://studio.azureml.net/)oturum açın.
1. Verileri [Içeri aktarma][import-data] modülünü kullanarak doğrudan SQL Server verileri okuyun. Doğrudan [Içeri aktarma verileri][import-data] sorgusunda alanları çıkaran, özellik oluşturan ve örnek verileri içeren gerekli sorguyu yapıştırın.
1. Karşıya yüklenen veri kümesi ile başlayan basit Azure Machine Learning deneme akış

## <a name="largedbtodb"></a>\#6. Senaryo: bir Azure sanal makinesinde SQL Server hedefleme, şirket içi bir SQL Server veritabanında büyük veri kümesi
![Büyük SQL DB şirket içinden Azure SQL DB'de][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Ek Azure kaynaklarını: Azure sanal makinesi (SQL Server / Ipython not defteri sunucusu)
1. SQL Server ve Ipython Notebook sunucusu çalıştıran bir Azure sanal makinesi oluşturun.
1. Veri birini verileri SQL Server'dan döküm dosyalarını dışarı aktarmak için yöntemleri dışarı aktarın.
   
   > [!NOTE]
   > Şirket içi veritabanındaki tüm verileri taşımaya karar verirseniz, tam veritabanını Azure 'daki SQL Server örneğine taşımak için alternatif (daha hızlı) bir yöntem. Verileri dışarı aktarma, veritabanı oluşturun ve hedef veritabanına veri yükleme/içeri aktarma ve Alternatif yöntem izleyin adımı atlayın.
   > 
   > 
1. Döküm dosyalarını Azure depolama kapsayıcısına yükleyin.
1. Verileri bir Azure sanal makinesinde çalışan SQL Server veritabanına yükleyin.
   
   a.  SQL Server VM oturum açın.
   
   b.  Veri dosyalarını bir Azure depolama kapsayıcısından yerel VM klasörüne indirin.
   
   c.  SQL Server Management Studio'da çalıştırın.
   
   d.  Veritabanı ve hedef tablolar oluşturun.
   
   e.  Verileri yüklemek için yöntemleri içe toplu birini kullanın.
   
   f.  Tablo birleştirmelerde gerekiyorsa, birleştirmeler hızlandırmak için dizin oluşturun.
   
   > [!NOTE]
   > Büyük veri boyutları, daha hızlı yükleme oluşturmak için bölümlenmiş tabloları ve toplu verileri paralel içeri aktarın. Daha fazla bilgi için bkz. [SQL bölümlenmiş tablolara paralel veri aktarma](parallel-load-sql-partitioned-tables.md).
   > 
   > 
1. Verileri keşfedin, gerektiği gibi özellikler oluşturun. Özelliklerin veritabanı tablolarında gerçekleştirilmiş olması gerekmez. Yalnızca bunları oluşturmak için gerekli sorguyu unutmayın.
1. Gerekli ve/veya istenen veri örnek boyutuna karar.
1. [Azure Machine Learning Studio](https://studio.azureml.net/)oturum açın.
1. Verileri [Içeri aktarma][import-data] modülünü kullanarak doğrudan SQL Server verileri okuyun. Doğrudan [Içeri aktarma verileri][import-data] sorgusunda alanları çıkaran, özellik oluşturan ve örnek verileri içeren gerekli sorguyu yapıştırın.
1. Basit Azure Machine Learning deneme akış karşıya yüklenen veri kümesi ile başlatılıyor.

### <a name="alternate-method-to-copy-a-full-database-from-an-on-premises--sql-server-to-azure-sql-database"></a>Tam bir veritabanı bir şirket içi SQL Server'dan Azure SQL veritabanı'na kopyalamak için alternatif yöntem
![Yerel DB ayırabilir ve Azure SQL DB'de ekleme][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Ek Azure kaynaklarını: Azure sanal makinesi (SQL Server / Ipython not defteri sunucusu)
Tüm SQL Server VM'nize SQL Server veritabanında çoğaltmak için bir veritabanı bir konum/sunucudan diğerine, veritabanı geçici olarak çevrimdışı duruma getirilmesini varsayılarak kopyalamanız gerekir. SQL Server Management Studio Nesne Gezgini veya eşdeğer Transact-SQL komutlarını kullanabilirsiniz.

1. Kaynak konumundaki veritabanı ayrılmadı. Daha fazla bilgi için bkz. [veritabanını ayırma](https://technet.microsoft.com/library/ms191491\(v=sql.110\).aspx).
1. Windows Gezgini veya Windows komut istemi penceresinde, ayrılan veritabanı dosya veya dosyalar ve günlük dosya veya dosyalar azure'da SQL Server sanal makinesi üzerinde hedef konuma kopyalayın.
1. Kopyalanan dosyalar, hedef SQL Server örneğinde ekleyin. Daha fazla bilgi için bkz. [veritabanı iliştirme](https://technet.microsoft.com/library/ms190209\(v=sql.110\).aspx).

[Ayırma ve Iliştirme kullanarak bir veritabanını taşıma (Transact-SQL)](https://technet.microsoft.com/library/ms187858\(v=sql.110\).aspx)

## <a name="largedbtohive"></a>Senaryo \#7: yerel dosyalardaki büyük veri, Azure HDInsight Hadoop kümelerinde hedef Hive veritabanı
![Yerel hedef Hive büyük veri][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a>Ek Azure kaynaklarını: Azure HDInsight Hadoop kümesi ve Azure sanal makine (Ipython not defteri sunucusu)
1. Ipython Notebook sunucusu çalıştıran bir Azure sanal makinesi oluşturun.
1. Azure HDInsight Hadoop kümesi oluşturun.
1. (İsteğe bağlı) Önceden işleme ve verileri temizledik.
   
   a.  Ipython Notebook Azure'dan verilerine erişme, verileri önceden işleme ve temizleme
   
       blobs.
   
   b.  Gerekirse, verileri temizlenmiş tablolu bir forma dönüştürün.
   
   c.  VM yerel dosyaya veri kaydetme (Ipython Notebook VM üzerinde çalışan, yerel sürücülere VM sürücülere bakın).
1. 2 adımda seçtiğiniz Hadoop kümesi varsayılan kapsayıcı veri yükleyin.
1. Azure HDInsight Hadoop kümesindeki Hive veritabanı verileri yükleyin.
   
   a.  Hadoop kümesinin baş düğümünü için oturum açın
   
   b.  Hadoop komut satırı açın.
   
   c.  Hadoop komut satırında komut `cd %hive_home%\bin` ile Hive kök dizinini girin.
   
   d.  Veritabanı ve tablo oluşturma için Hive sorguları çalıştırmak ve verileri blob depolama alanından Hive tablolarına yükleme.
   
   > [!NOTE]
   > Veriler büyük ise, kullanıcıların bölümlerle Hive tablosu oluşturabilirsiniz. Ardından kullanıcılar, Bölüm tarafından bölümlenmiş Hive tablosuna veri yüklemek için baş düğümdeki Hadoop komut satırında bir `for` döngüsü kullanabilir.
   > 
   > 
1. Verileri araştırmak ve Hadoop komut satırında gerektiği gibi özellikler oluşturun. Özelliklerin veritabanı tablolarında gerçekleştirilmiş olması gerekmez. Yalnızca bunları oluşturmak için gerekli sorguyu unutmayın.
   
   a.  Hadoop kümesinin baş düğümünü için oturum açın
   
   b.  Hadoop komut satırı açın.
   
   c.  Hadoop komut satırında komut `cd %hive_home%\bin` ile Hive kök dizinini girin.
   
   d.  Hive sorguları, verileri araştırabilir ve gerektiği gibi özellikler oluşturmak için Hadoop kümesi baş düğümünde Hadoop komut satırında çalıştırın.
1. Gerekli ve/veya istenen, verileri Azure Machine Learning Studio'da uyacak şekilde örnek.
1. [Azure Machine Learning Studio](https://studio.azureml.net/)oturum açın.
1. Verileri [Içeri aktarma][import-data] modülünü kullanarak doğrudan `Hive Queries` verileri okuyun. Doğrudan [Içeri aktarma verileri][import-data] sorgusunda alanları çıkaran, özellik oluşturan ve örnek verileri içeren gerekli sorguyu yapıştırın.
1. Basit Azure Machine Learning deneme akış karşıya yüklenen veri kümesi ile başlatılıyor.

## <a name="decisiontree"></a>Senaryo seçimi için karar ağacı
---
Aşağıdaki diyagramda, yukarıda açıklanan senaryoları ve Gelişmiş analitik işlemi ve listelenen senaryolar için aldığınız yapılan teknoloji seçimleri özetler. Veri işleme, araştırma, özellik Mühendisliği ve örnekleme, kaynak, ara ve/veya hedef ortamlarda bir veya daha fazla yöntem/ortamda olabilir ve gerektiğinde yinelemeli olarak devam edebilir. Diyagram yalnızca bazı olası akışlar çizim görev yapar ve kapsamlı bir numaralandırma sağlamaz.

![DS işlem kılavuzu senaryolar][8]

### <a name="advanced-analytics-in-action-examples"></a>Örnek uygulamada Gelişmiş analiz
Gelişmiş analitik işlemi ve teknolojisi genel veri kümeleri kullanılarak kullanan uçtan uca Azure Machine Learning izlenecek yollar için bkz:

* [Ekip veri bilimi işlemi işlem içinde: SQL Server kullanılıyor](sql-walkthrough.md).
* [Team Data Science süreci: HDInsight Hadoop kümelerini kullanma](hive-walkthrough.md).

[1]: ./media/plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/plan-sample-scenarios/dsp-plan-sample-scenarios.png
[9]: ./media/plan-sample-scenarios/dsp-plan-local-to-hive.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
