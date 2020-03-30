---
title: Azure Machine Learning - Team Data Science Process için senaryoları belirleme
description: Ekip Veri Bilimi Süreci ile gelişmiş tahmine dayalı analitik yapmak için uygun senaryoları seçin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79251628"
---
# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a>Azure Machine Learning’de gelişmiş analiz senaryoları
Bu makalede, [Takım Veri Bilimi Süreci (TDSP)](overview.md)tarafından işlenebilir örnek veri kaynakları ve hedef senaryoların çeşitliliği özetlenir. TDSP, ekiplerin akıllı uygulamalar oluşturma konusunda işbirliği yapmalarını sağlamak için sistematik bir yaklaşım sağlar. Burada sunulan senaryolar, Veri işleme iş akışında bulunan ve Azure'daki veri özelliklerine, kaynak konumlarına ve hedef depolarına bağlı olan seçenekleri göstermektedir.

Verileriniz ve amacınız için uygun örnek senaryoları seçmek için **karar ağacı** son bölümde sunulur.

Aşağıdaki bölümlerin her biri bir örnek senaryo sunar. Her senaryo için olası bir veri bilimi veya gelişmiş analitik akışı ve destekleyici Azure kaynakları listelenir.

> [!NOTE]
> **Aşağıdaki senaryoların tümü için şunları yapmanız gerekir:**
> <br/>
> 
> * [Depolama hesabı oluşturma](../../storage/common/storage-account-create.md)
>   <br/>
> * [Azure Machine Learning çalışma alanı oluşturma](../studio/create-workspace.md)
> 
> 

## <a name="scenario-1-small-to-medium-tabular-dataset-in-local-files"></a><a name="smalllocal"></a>Senaryo \#1: Yerel dosyalarda küçük ve orta tabular veri kümesi
![Küçük ve orta yerel dosyalar][1]

#### <a name="additional-azure-resources-none"></a>Ek Azure kaynakları: Yok
1. Azure Machine [Learning Studio'da](https://studio.azureml.net/)oturum açın.
1. Bir veri seti yükleyin.
1. Yüklenen dataset(ler) ile başlayan bir Azure Machine Learning deneme akışı oluşturun.

## <a name="scenario-2-small-to-medium-dataset-of-local-files-that-require-processing"></a><a name="smalllocalprocess"></a>Senaryo \#2: İşlenmesi gereken yerel dosyaların küçük ve orta veri kümesi
![İşleme ile küçük ve orta yerel dosyalar][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Ek Azure kaynakları: Azure Virtual Machine (IPython Notebook sunucusu)
1. IPython Notebook çalıştıran bir Azure Sanal Makine oluşturun.
1. Verileri bir Azure Depolama kapsayıcısına yükleyin.
1. Azure Depolama kapsayıcısından verilere erişen IPython Notebook'ta verileri önceden işleme ve temizleme.
1. Verileri temizlenmiş bir tabular forma dönüştürün.
1. Dönüştürülmüş verileri Azure lekelerine kaydedin.
1. Azure Machine [Learning Studio'da](https://studio.azureml.net/)oturum açın.
1. [Verileri İçe Aktar][import-data] modüllerini kullanarak Azure blobs'daki verileri okuyun.
1. Yutulan dataset(ler) ile başlayan bir Azure Machine Learning deneme akışı oluşturun.

## <a name="scenario-3-large-dataset-of-local-files-targeting-azure-blobs"></a><a name="largelocal"></a>Senaryo \#3: Azure Blobs'ı hedefleyen yerel dosyaların büyük veri seti
![Büyük yerel dosyalar][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Ek Azure kaynakları: Azure Virtual Machine (IPython Notebook sunucusu)
1. IPython Notebook çalıştıran bir Azure Sanal Makine oluşturun.
1. Verileri bir Azure Depolama kapsayıcısına yükleyin.
1. IPython Notebook'ta Azure lekelerinden verilere erişen verileri önceden işleme ve temizleme.
1. Gerekirse verileri temizlenmiş bir tabular forma dönüştürün.
1. Verileri keşfedin ve gerektiğinde özellikler oluşturun.
1. Küçük ve orta ölçekli bir veri örneği ayıklayın.
1. Örneklenmiş verileri Azure lekelerine kaydedin.
1. Azure Machine [Learning Studio'da](https://studio.azureml.net/)oturum açın.
1. [Verileri İçe Aktar][import-data] modüllerini kullanarak Azure blobs'daki verileri okuyun.
1. Yutulan dataset(ler) ile başlayan Azure Machine Learning deneme akışını oluşturun.

## <a name="scenario-4-small-to-medium-dataset-of-local-files-targeting-sql-server-in-an-azure-virtual-machine"></a><a name="smalllocaltodb"></a>Senaryo \#4: Azure Sanal Makine'de SQL Server'ı hedefleyen yerel dosyaların küçük ve orta veri kümesi
![Azure'da SQL DB'ye küçük ve orta yerel dosyalar][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Ek Azure kaynakları: Azure Virtual Machine (SQL Server / IPython Notebook sunucusu)
1. SQL Server + IPython Notebook çalıştıran bir Azure Sanal Makine oluşturun.
1. Verileri bir Azure Depolama kapsayıcısına yükleyin.
1. IPython Notebook kullanarak Azure Depolama kapsayıcısında verileri önceden işleme ve temizleme.
1. Gerekirse verileri temizlenmiş bir tabular forma dönüştürün.
1. Verileri VM yerel dosyalara kaydedin (IPython Notebook VM'de çalışıyor, yerel sürücüler VM sürücülere başvuruyor).
1. Azure VM üzerinde çalışan SQL Server veritabanına veri yükleyin.
   
   Seçenek \#1: SQL Server Management Studio kullanma.
   
   * SQL Server VM'de oturum açın
   * SQL Server Management Studio'u çalıştırın.
   * Veritabanı ve hedef tablolar oluşturun.
   * VM-yerel dosyalardan verileri yüklemek için toplu alma yöntemlerinden birini kullanın.
   
   Seçenek \#2: IPython Notebook kullanma – orta ve daha büyük veri kümeleri için tavsiye edilmez
   
   <!-- -->    
   * VM'de SQL Server'a erişmek için ODBC bağlantı dizesini kullanın.
   * Veritabanı ve hedef tablolar oluşturun.
   * VM-yerel dosyalardan verileri yüklemek için toplu alma yöntemlerinden birini kullanın.
1. Verileri keşfedin, gerektiğinde özellikler oluşturun. Özelliklerin veritabanı tablolarında somutlaştırılması gerekmez. Yalnızca bunları oluşturmak için gerekli sorguyu not edin.
1. Gerekirse ve/veya istenirse veri örnekboyutuna karar verin.
1. Azure Machine [Learning Studio'da](https://studio.azureml.net/)oturum açın.
1. Verileri doğrudan SQL Server'dan [Alma Verisi][import-data] modüllerini kullanarak okuyun. Alanları ayıklayan, özellikler oluşturan ve gerekirse verileri doğrudan [İçe Aktar sorgusunda][import-data] örnekler le ilgili gerekli sorguyu yapıştırın.
1. Yutulan dataset(ler) ile başlayan Azure Machine Learning deneme akışını oluşturun.

## <a name="scenario-5-large-dataset-in-local-files-target-sql-server-in-azure-vm"></a><a name="largelocaltodb"></a>Senaryo \#5: Yerel dosyalarda büyük veri seti, Azure VM'de SQL Server'ı hedef
![Azure'da SQL DB'ye büyük yerel dosyalar][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Ek Azure kaynakları: Azure Virtual Machine (SQL Server / IPython Notebook sunucusu)
1. SQL Server ve IPython Notebook sunucusu çalıştıran bir Azure Sanal Makine oluşturun.
1. Verileri bir Azure Depolama kapsayıcısına yükleyin.
1. (İsteğe bağlı) Ön işlem ve temiz veri.
   
    a.  IPython Notebook'ta Azure lekelerinden verilere erişen verileri önceden işleme ve temizleme.
   
    b.  Gerekirse verileri temizlenmiş bir tabular forma dönüştürün.
   
    c.  Verileri VM yerel dosyalara kaydedin (IPython Notebook VM'de çalışıyor, yerel sürücüler VM sürücülere başvuruyor).
1. Azure VM üzerinde çalışan SQL Server veritabanına veri yükleyin.
   
    a.  SQL Server VM'de oturum açın.
   
    b.  Veriler zaten kaydedilmediyse, veri dosyalarını Azure depolama kapsayıcısından yerel VM klasörüne indirin.
   
    c.  SQL Server Management Studio'u çalıştırın.
   
    d.  Veritabanı ve hedef tablolar oluşturun.
   
    e.  Verileri yüklemek için toplu alma yöntemlerinden birini kullanın.
   
    f.  Tablo birleştirmeleri gerekiyorsa, birleştirmeleri hızlandırmak için dizinler oluşturun.
   
   > [!NOTE]
   > Büyük veri boyutlarının daha hızlı yüklenmesi için, bölümlenmiş tablolar oluşturmanız ve verileri paralel olarak toplu olarak aktarmanız önerilir. Daha fazla bilgi için [bkz.](parallel-load-sql-partitioned-tables.md)
   > 
   > 
1. Verileri keşfedin, gerektiğinde özellikler oluşturun. Özelliklerin veritabanı tablolarında somutlaştırılması gerekmez. Yalnızca bunları oluşturmak için gerekli sorguyu not edin.
1. Gerekirse ve/veya istenirse veri örnekboyutuna karar verin.
1. Azure Machine [Learning Studio'da](https://studio.azureml.net/)oturum açın.
1. Verileri doğrudan SQL Server'dan [Alma Verisi][import-data] modüllerini kullanarak okuyun. Alanları ayıklayan, özellikler oluşturan ve gerekirse verileri doğrudan [İçe Aktar sorgusunda][import-data] örnekler le ilgili gerekli sorguyu yapıştırın.
1. Yüklenen veri kümesiyle başlayan Basit Azure Machine Learning deneme akışı

## <a name="scenario-6-large-dataset-in-a-sql-server-database-on-premises-targeting-sql-server-in-an-azure-virtual-machine"></a><a name="largedbtodb"></a>Senaryo \#6: Sql Server veritabanında şirket içinde büyük veri seti, Azure Sanal Makine'de SQL Server'ı hedefleme
![Azure'da SQL DB'ye büyük SQL DB on-prem][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Ek Azure kaynakları: Azure Virtual Machine (SQL Server / IPython Notebook sunucusu)
1. SQL Server ve IPython Notebook sunucusu çalıştıran bir Azure Sanal Makine oluşturun.
1. Dosyaları dökümü için SQL Server verileri dışa aktarmak için veri verme yöntemlerinden birini kullanın.
   
   > [!NOTE]
   > Tüm verileri şirket içi veritabanından taşımaya karar verirseniz, tam veritabanını Azure'daki SQL Server örneğine taşımak için alternatif (daha hızlı) bir yöntem. Verileri dışa aktarmak, veritabanı oluşturmak ve verileri hedef veritabanına yüklemek/almak için adımları atlayın ve alternatif yöntemi izleyin.
   > 
   > 
1. Döküm dosyaları Azure Depolama kapsayıcısına yükleyin.
1. Verileri Azure Sanal Makine'de çalışan bir SQL Server veritabanına yükleyin.
   
   a.  SQL Server VM'de oturum açın.
   
   b.  Veri dosyalarını bir Azure Depolama kapsayıcısından yerel VM klasörüne indirin.
   
   c.  SQL Server Management Studio'u çalıştırın.
   
   d.  Veritabanı ve hedef tablolar oluşturun.
   
   e.  Verileri yüklemek için toplu alma yöntemlerinden birini kullanın.
   
   f.  Tablo birleştirmeleri gerekiyorsa, birleştirmeleri hızlandırmak için dizinler oluşturun.
   
   > [!NOTE]
   > Büyük veri boyutlarının daha hızlı yüklenmesi için, bölümlenmiş tablolar oluşturun ve verileri paralel olarak toplu olarak aktarın. Daha fazla bilgi için [bkz.](parallel-load-sql-partitioned-tables.md)
   > 
   > 
1. Verileri keşfedin, gerektiğinde özellikler oluşturun. Özelliklerin veritabanı tablolarında somutlaştırılması gerekmez. Yalnızca bunları oluşturmak için gerekli sorguyu not edin.
1. Gerekirse ve/veya istenirse veri örnekboyutuna karar verin.
1. Azure Machine [Learning Studio'da](https://studio.azureml.net/)oturum açın.
1. Verileri doğrudan SQL Server'dan [Alma Verisi][import-data] modüllerini kullanarak okuyun. Alanları ayıklayan, özellikler oluşturan ve gerekirse verileri doğrudan [İçe Aktar sorgusunda][import-data] örnekler le ilgili gerekli sorguyu yapıştırın.
1. Yüklenen veri kümesiyle başlayan basit Azure Machine Learning deneme akışı.

### <a name="alternate-method-to-copy-a-full-database-from-an-on-premises--sql-server-to-azure-sql-database"></a>Şirket içi bir SQL Server'dan Azure SQL Veritabanına tam bir veritabanı kopyalamak için alternatif yöntem
![Azure'da yerel DB'yi ayırın ve SQL DB'ye takın][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Ek Azure kaynakları: Azure Virtual Machine (SQL Server / IPython Notebook sunucusu)
SQL Server VM'nizdeki TÜM SQL Server veritabanını çoğaltmak için, veritabanının geçici olarak çevrimdışı duruma getirilebilen bir veritabanını bir konumdan/sunucudan diğerine kopyalamanız gerekir. SQL Server Management Studio Object Explorer'ı veya eşdeğer Transact-SQL komutlarını kullanabilirsiniz.

1. Veritabanını kaynak konumda ayırın. Daha fazla bilgi için [bir veritabanını ayırma](https://technet.microsoft.com/library/ms191491\(v=sql.110\).aspx)bölümüne bakın.
1. Windows Gezgini veya Windows Komut Komut Ustem penceresinde, ayrılmış veritabanı dosyasını veya dosyalarını kopyalayın ve dosya veya dosyaları Azure'daki SQL Server VM'deki hedef konuma kaydedin.
1. Kopyalanan dosyaları hedef SQL Server örneğine takın. Daha fazla bilgi için veritabanı [ekle'ye](https://technet.microsoft.com/library/ms190209\(v=sql.110\).aspx)bakın.

[Ayır ve Ekle (Transact-SQL) kullanarak Veritabanını Taşıma](https://technet.microsoft.com/library/ms187858\(v=sql.110\).aspx)

## <a name="scenario-7-big-data-in-local-files-target-hive-database-in-azure-hdinsight-hadoop-clusters"></a><a name="largedbtohive"></a>Senaryo \#7: Yerel dosyalarda büyük veri, Azure HDInsight Hadoop kümelerinde hedef Hive veritabanı
![Yerel hedef Hive'da büyük veri][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a>Ek Azure kaynakları: Azure HDInsight Hadoop Cluster ve Azure Virtual Machine (IPython Notebook sunucusu)
1. IPython Notebook sunucusu çalıştıran bir Azure Sanal Makine oluşturun.
1. Azure HDInsight Hadoop kümesi oluşturun.
1. (İsteğe bağlı) Ön işlem ve temiz veri.
   
   a.  IPython Notebook'ta ön işlem ve temiz veriler, Azure'dan verilere erişme
   
       blobs.
   
   b.  Gerekirse verileri temizlenmiş bir tabular forma dönüştürün.
   
   c.  Verileri VM yerel dosyalara kaydedin (IPython Notebook VM'de çalışıyor, yerel sürücüler VM sürücülere başvuruyor).
1. 2. adımda seçilen Hadoop kümesinin varsayılan kapsayıcısına veri yükleyin.
1. Azure HDInsight Hadoop kümesinde verileri Hive veritabanına yükleyin.
   
   a.  Hadoop kümesinin baş düğümüne giriş yapın
   
   b.  Hadoop Komuta Hattı'nı açın.
   
   c.  Hadoop Komut Hattı'nda `cd %hive_home%\bin` komutla Hive kök dizini girin.
   
   d.  Veritabanı ve tablolar oluşturmak için Kovan sorgularını çalıştırın ve blob depolamadan Kovan tablolarına veri yükleyin.
   
   > [!NOTE]
   > Veriler büyükse, kullanıcılar bölümleri olan Hive tablosunu oluşturabilir. Daha sonra, kullanıcılar `for` bölümle bölümlenmiş Hive tablosuna veri yüklemek için baş düğümündeki Hadoop Komut Satırındaki bir döngüyü kullanabilirler.
   > 
   > 
1. Hadoop Komuta Hattı'nda verileri keşfedin ve gerektiğinde özellikler oluşturun. Özelliklerin veritabanı tablolarında somutlaştırılması gerekmez. Yalnızca bunları oluşturmak için gerekli sorguyu not edin.
   
   a.  Hadoop kümesinin baş düğümüne giriş yapın
   
   b.  Hadoop Komuta Hattı'nı açın.
   
   c.  Hadoop Komut Hattı'nda `cd %hive_home%\bin` komutla Hive kök dizini girin.
   
   d.  Verileri keşfetmek ve gerektiğinde özellikler oluşturmak için Hadoop kümenin baş düğümünde Hadoop Komut Satırındaki Hive sorgularını çalıştırın.
1. Gerekirse ve/veya istenirse, Azure Machine Learning Studio'ya sığacak verileri örnekleyin.
1. Azure Machine [Learning Studio'da](https://studio.azureml.net/)oturum açın.
1. Verileri doğrudan Veri `Hive Queries` [Aktar'ı][import-data] kullanarak okuyun. Alanları ayıklayan, özellikler oluşturan ve gerekirse verileri doğrudan [İçe Aktar sorgusunda][import-data] örnekler le ilgili gerekli sorguyu yapıştırın.
1. Yüklenen veri kümesiyle başlayan basit Azure Machine Learning deneme akışı.

## <a name="decision-tree-for-scenario-selection"></a><a name="decisiontree"></a>Senaryo seçimi için karar ağacı
---
Aşağıdaki diyagram, yukarıda açıklanan senaryoları ve sizi ayrıntılı senaryoların her birine götürecek Gelişmiş Analitik Süreci ve Teknoloji seçimlerini özetler. Veri işleme, keşif, özellik mühendisliği ve örnekleme, kaynak, ara ve/veya hedef ortamlarda bir veya daha fazla yöntem/ortamda gerçekleşebilir ve gerektiğinde yinelemeli olarak ilerleyebilir. Diyagram yalnızca bazı olası akışların bir örneği olarak hizmet vermektedir ve ayrıntılı bir numaralandırma sağlamaz.

![Örnek DS süreci gözden geçirme senaryoları][8]

### <a name="advanced-analytics-in-action-examples"></a>Eylemde Gelişmiş Analitik Örnekleri
Ortak veri kümelerini kullanarak Gelişmiş Analitik Süreci ve Teknolojisini kullanan uçtan uca Azure Machine Learning walkthroughs için bkz:

* [Takım Veri Bilimi Süreci iş başında: SQL Server kullanarak](sql-walkthrough.md).
* [Takım Veri Bilimi Süreci iş başında: HDInsight Hadoop kümelerini kullanarak.](hive-walkthrough.md)

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
