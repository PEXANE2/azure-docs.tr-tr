---
title: Apache HBase & Kurumsal Güvenlik Paketi - Azure HDInsight
description: Öğretici - Kurumsal Güvenlik Paketi ile Azure HDInsight'ta HBase için Apache Ranger politikalarını nasıl yapılandıracağınızı öğrenin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: tutorial
ms.date: 09/04/2019
ms.openlocfilehash: 89e9faeb3c67d0fd0c57adea3a3f69ec5438e3a0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73044654"
---
# <a name="tutorial-configure-apache-hbase-policies-in-hdinsight-with-enterprise-security-package"></a>Öğretici: Kurumsal Güvenlik Paketi ile HDInsight'ta Apache HBase politikalarını yapılandırın

Kurumsal Güvenlik Paketi (ESP) Apache HBase kümeleri için Apache Ranger ilkelerini nasıl yapılandıracağınızı öğrenin. ESP kümeleri bir etki alanına bağlıdır ve kullanıcıların etki alanı kimlik bilgileriyle kimlik doğrulaması yapmasına olanak sağlar. Bu öğreticide, HBase tablosunda farklı sütun ailelerine erişimi kısıtlamak için iki Ranger ilkeleri oluşturursunuz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Etki alanı kullanıcılarını oluşturma
> * Ranger ilkelerini oluşturma
> * HBase kümesinde tablo oluşturma
> * Ranger ilkelerini test etme

## <a name="before-you-begin"></a>Başlamadan önce

* Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/)oluşturun.

* [Azure portalında](https://portal.azure.com/)oturum açın.

* Kurumsal [Güvenlik Paketi ile hdinsight HBase kümesi](apache-domain-joined-configure-using-azure-adds.md)oluşturun.

## <a name="connect-to-apache-ranger-admin-ui"></a>Apache Ranger Yönetici Arabirimine bağlanma

1. Bir tarayıcıdan, `https://<ClusterName>.azurehdinsight.net/Ranger/` URL’sini kullanarak Ranger Yönetici kullanıcı arabirimine bağlanın. HBase `<ClusterName>` kümenizin adını değiştirmeyi unutmayın.

    > [!NOTE]  
    > Ranger kimlik bilgileri Hadoop kümesi kimlik bilgileriyle aynı değildir. Tarayıcıların ön belleğe alınmış Hadoop kimlik bilgilerini kullanmasını önlemek için Ranger Yönetici Arabirimine yeni bir InPrivate tarayıcı penceresinden bağlanın.

2. Azure Active Directory (AD) yönetici kimlik bilgilerinizi kullanarak oturum açın. Azure AD yönetici kimlik bilgileri HDInsight küme kimlik bilgileri veya Linux HDInsight düğümü SSH kimlik bilgileriyle aynı değildir.

## <a name="create-domain-users"></a>Etki alanı kullanıcılarını oluşturma

**sales_user1** ve **marketing_user1** etki alanı kullanıcılarını nasıl oluşturup marketing_user1 öğrenmek için Kurumsal Güvenlik Paketi [ile hdinsight kümesi oluşturma'yı](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds)ziyaret edin. Bir üretim senaryosunda, etki alanı kullanıcıları Active Directory kiracınızdan gelir.

## <a name="create-hbase-tables-and-import-sample-data"></a>HBase tabloları oluşturma ve örnek verileri alma

HBase kümelerine bağlanmak için SSH'yi ve ardından HBase tabloları, veri eklemek ve sorgu verileri oluşturmak için [Apache HBase Shell'i](https://hbase.apache.org/0.94/book/shell.html) kullanabilirsiniz. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

### <a name="to-use-the-hbase-shell"></a>HBase kabuğunu kullanmak için

1. SSH'den aşağıdaki HBase komutu çalıştırın:
   
    ```bash
    hbase shell
    ```

2. İki sütunlu `Customers` ailelerden bir HBase tablosu oluşturun: `Name` ve `Contact`.

    ```hbaseshell   
    create 'Customers', 'Name', 'Contact'
    list
    ```
3. Bazı verileri ekleyin:
    
    ```hbaseshell   
    put 'Customers','1001','Name:First','Alice'
    put 'Customers','1001','Name:Last','Johnson'
    put 'Customers','1001','Contact:Phone','333-333-3333'
    put 'Customers','1001','Contact:Address','313 133rd Place'
    put 'Customers','1001','Contact:City','Redmond'
    put 'Customers','1001','Contact:State','WA'
    put 'Customers','1001','Contact:ZipCode','98052'
    put 'Customers','1002','Name:First','Robert'
    put 'Customers','1002','Name:Last','Stevens'
    put 'Customers','1002','Contact:Phone','777-777-7777'
    put 'Customers','1002','Contact:Address','717 177th Ave'
    put 'Customers','1002','Contact:City','Bellevue'
    put 'Customers','1002','Contact:State','WA'
    put 'Customers','1002','Contact:ZipCode','98008'
    ```
4. Tablonun içeriğini görüntüleyin:
    
    ```hbaseshell
    scan 'Customers'
    ```

    ![HDInsight Hadoop HBase kabuk çıkışı](./media/apache-domain-joined-run-hbase/hbase-shell-scan-table.png)

## <a name="create-ranger-policies"></a>Ranger ilkelerini oluşturma

**sales_user1** ve **marketing_user1**için bir Ranger politikası oluşturun.

1. **Ranger Yönetici Arabirimini** açın. **HBase**altında ** \<ClusterName>_hbase'ı** tıklatın.

   ![HDInsight Apache Ranger Yönetici UI](./media/apache-domain-joined-run-hbase/apache-ranger-admin-login.png)

2. **İlkeler Listesi** ekranı, bu küme için oluşturulan tüm Ranger ilkelerini görüntüler. Bir önceden yapılandırılmış ilke listelenebilir. **Yeni İlke Ekle'yi**tıklatın.

    ![Apache Ranger HBase politikaları listesi](./media/apache-domain-joined-run-hbase/apache-ranger-hbase-policies-list.png)

3. **İlke Oluştur** ekranında aşağıdaki değerleri girin:

   |**Ayar**  |**Önerilen değer**  |
   |---------|---------|
   |İlke Adı  |  sales_customers_name_contact   |
   |HBase Tablosu   |  Müşteriler |
   |HBase Sütun-aile   |  İsim, İletişim |
   |HBase Sütunu   |  * |
   |Grup Seçiniz  | |
   |Kullanıcı Seçin  | sales_user1 |
   |İzinler  | Okuma |

   Konu adında şu joker karakterler bulunabilir:

   * `*`karakterlerin sıfır veya daha fazla oluşumlarını gösterir.
   * `?`tek bir karakteri gösterir.

   ![Apache Ranger ilkesi satış oluşturmak](./media/apache-domain-joined-run-hbase/apache-ranger-hbase-policy-create-sales.png)

   >[!NOTE]
   >**Select User** için bir etki alanı kullanıcısı otomatik olarak doldurulmazsa, Ranger’ın Azure AD ile eşitlenmesi için birkaç dakika bekleyin.

4. **Add**’e tıklayarak ilkeyi kaydedin.

5. **Yeni İlke Ekle'yi** tıklatın ve ardından aşağıdaki değerleri girin:

   |**Ayar**  |**Önerilen değer**  |
   |---------|---------|
   |İlke Adı  |  marketing_customers_contact   |
   |HBase Tablosu   |  Müşteriler |
   |HBase Sütun-aile   |  İletişim |
   |HBase Sütunu   |  * |
   |Grup Seçiniz  | |
   |Kullanıcı Seçin  | marketing_user1 |
   |İzinler  | Okuma |

   ![Apache Ranger politikası pazarlama oluşturmak](./media/apache-domain-joined-run-hbase/apache-ranger-hbase-policy-create-marketing.png)  

6. **Add**’e tıklayarak ilkeyi kaydedin.

## <a name="test-the-ranger-policies"></a>Ranger ilkelerini test etme

Yapılandırılmış Ranger ilkelerine bağlı olarak, **sales_user1** hem sütun `Name` `Contact` ailelerindeki sütunların tüm verilerini görüntüleyebilir. **marketing_user1** yalnızca sütun ailesindeki `Contact` verileri görüntüleyebilir.

### <a name="access-data-as-sales_user1"></a>Verilere sales_user1 olarak erişin

1. Kümeye yeni bir SSH bağlantısı açın. Kümede oturum açabilmek için aşağıdaki komutu kullanın:

   ```bash
   ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
   ```

1. İstenilen kullanıcının bağlamına değiştirmek için kinit komutunu kullanın.

   ```bash
   kinit sales_user1
   ```

2. HBase kabuğunu açın ve `Customers`tabloyu tarayıp tarayıp tarayıp.

   ```hbaseshell
   hbase shell
   scan `Customers`
   ```

3. Satış kullanıcısının `Customers` `Name` sütun ailesindeki iki sütun ve `Contact` sütun ailesindeki beş sütun da dahil olmak üzere tablonun tüm sütunlarını görüntülenebildiğine dikkat edin.

    ```hbaseshell
    ROW                                COLUMN+CELL
     1001                              column=Contact:Address, timestamp=1548894873820, value=313 133rd Place
     1001                              column=Contact:City, timestamp=1548895061523, value=Redmond
     1001                              column=Contact:Phone, timestamp=1548894871759, value=333-333-3333
     1001                              column=Contact:State, timestamp=1548895061613, value=WA
     1001                              column=Contact:ZipCode, timestamp=1548895063111, value=98052
     1001                              column=Name:First, timestamp=1548894871561, value=Alice
     1001                              column=Name:Last, timestamp=1548894871707, value=Johnson
     1002                              column=Contact:Address, timestamp=1548894899174, value=717 177th Ave
     1002                              column=Contact:City, timestamp=1548895103129, value=Bellevue
     1002                              column=Contact:Phone, timestamp=1548894897524, value=777-777-7777
     1002                              column=Contact:State, timestamp=1548895103231, value=WA
     1002                              column=Contact:ZipCode, timestamp=1548895104804, value=98008
     1002                              column=Name:First, timestamp=1548894897419, value=Robert
     1002                              column=Name:Last, timestamp=1548894897487, value=Stevens
    2 row(s) in 0.1000 seconds
    ```

### <a name="access-data-as-marketing_user1"></a>Verilere marketing_user1 olarak erişin

1. Kümeye yeni bir SSH bağlantısı açın. **Marketing_user1**olarak oturum vermek için aşağıdaki komutu kullanın:

   ```bash
   ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
   ```

1. İstenilen kullanıcının bağlamına değiştirmek için kinit komutunu kullanın

   ```bash
   kinit marketing_user1
   ```

1. HBase kabuğunu açın ve `Customers`tabloyu tarayıp tarayıp:

    ```hbaseshell
    hbase shell
    scan `Customers`
    ```

1. Pazarlama kullanıcısının sütun ailesinin yalnızca beş `Contact` sütununu görüntülenebildiğine dikkat edin.

    ```hbaseshell
    ROW                                COLUMN+CELL
     1001                              column=Contact:Address, timestamp=1548894873820, value=313 133rd Place
     1001                              column=Contact:City, timestamp=1548895061523, value=Redmond
     1001                              column=Contact:Phone, timestamp=1548894871759, value=333-333-3333
     1001                              column=Contact:State, timestamp=1548895061613, value=WA
     1001                              column=Contact:ZipCode, timestamp=1548895063111, value=98052
     1002                              column=Contact:Address, timestamp=1548894899174, value=717 177th Ave
     1002                              column=Contact:City, timestamp=1548895103129, value=Bellevue
     1002                              column=Contact:Phone, timestamp=1548894897524, value=777-777-7777
     1002                              column=Contact:State, timestamp=1548895103231, value=WA
     1002                              column=Contact:ZipCode, timestamp=1548895104804, value=98008
    2 row(s) in 0.0730 seconds
    ```

1. Ranger kullanıcı arabiriminden denetim erişimi olaylarını görüntüleyin.

   ![HDInsight Ranger UI Politika Denetimi](./media/apache-domain-joined-run-hbase/apache-ranger-admin-audit.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz, oluşturduğunuz HBase kümesini aşağıdaki adımlarla silin:

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Üstteki **Arama** kutusuna **HDInsight**yazın. 
1. **Hizmetler**altında **HDInsight kümelerini** seçin.
1. Görünen HDInsight kümeleri listesinde, bu öğretici için oluşturduğunuz kümenin yanındaki **...** 'yı tıklatın. 
1. **Sil'i**tıklatın. **Evet'i**tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir Apache HBase ile başlayın](../hbase/apache-hbase-tutorial-get-started-linux.md)
