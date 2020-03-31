---
title: Apache Ranger'da Apache Hive politikaları - Azure HDInsight
description: Kurumsal Güvenlik Paketi ile Bir Azure HDInsight hizmetinde Hive için Apache Ranger ilkelerini nasıl yapılandıracağınızı öğrenin.
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/27/2019
ms.openlocfilehash: 90d7da9c8ddd8c9c595f2209dcc34e2f595acfd2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78196935"
---
# <a name="configure-apache-hive-policies-in-hdinsight-with-enterprise-security-package"></a>Kurumsal Güvenlik Paketi ile HDInsight içinde Apache Hive ilkelerini yapılandırma

Apache Hive için Apache Ranger politikalarını nasıl yapılandıracağınızı öğrenin. Bu makalede hivesampletable erişimini kısıtlamak için iki Ranger ilkesi oluşturacaksınız. hivesampletable, HDInsight kümelerine sahiptir. İlkeleri yapılandırıldıktan sonra, HDInsight'taki Hive tablolarına bağlanmak için Excel ve ODBC sürücüsünü kullanırsınız.

## <a name="prerequisites"></a>Ön koşullar

* Kurumsal Güvenlik Paketi içeren bir HDInsight kümesi. Bkz. [ESP ile HDInsight kümelerini yapılandırın.](apache-domain-joined-configure.md)
* Office 2016, Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013’ün tek başına sürümü veya Office 2010 Professional Plus yüklü iş istasyonu.

## <a name="connect-to-apache-ranger-admin-ui"></a>Apache Ranger Yönetici Arabirimine bağlanma
**Ranger Yönetici Arabirimine bağlanmak için**

1. Bir tarayıcıdan CLUSTERNAME'nin kümenizin adı `https://CLUSTERNAME.azurehdinsight.net/Ranger/` olduğu Ranger Yönetici Kullanıcı Arabirimi'ne gidin.

   > [!NOTE]  
   > Ranger, Apache Hadoop kümesinden farklı kimlik bilgileri kullanır. Önbelleğe alınmış Hadoop kimlik bilgilerini kullanan tarayıcıları önlemek için Ranger Yönetici UI'ye bağlanmak için yeni InPrivate tarayıcı penceresini kullanın.

2. Küme yöneticisi etki alanı kullanıcı adı ve parolasını kullanarak oturum açın:

    ![HDInsight ESP Ranger giriş sayfası](./media/apache-domain-joined-run-hive/hdinsight-domain-joined-ranger-home-page.png)

    Ranger şu an için yalnızca Yarn ve Hive ile birlikte çalışmaktadır.

## <a name="create-domain-users"></a>Etki alanı kullanıcılarını oluşturma

Hiveruser1 ve hiveuser2'nin nasıl oluşturulabildiğini öğrenmek için [ESP'li bir HDInsight kümesi oluştur'a](apache-domain-joined-configure-using-azure-adds.md#create-an-hdinsight-cluster-with-esp)bakın. Bu makalede iki kullanıcı hesabını kullanırsınız.

## <a name="create-ranger-policies"></a>Ranger ilkelerini oluşturma

Bu bölümde, kovan örneklemesi erişmek için iki Ranger ilkeleri oluşturun. Farklı sütun kümelerine select izni vereceksiniz. Her iki kullanıcı da [ESP ile HDInsight kümesi oluştur](apache-domain-joined-configure-using-azure-adds.md#create-an-hdinsight-cluster-with-esp)kullanılarak oluşturuldu. Sonraki bölümde, Excel'de iki ilkeyi sınayacaksınız.

**Ranger ilkeleri oluşturmak için**

1. Ranger Yönetici Arabirimini açın. Bkz. Apache Ranger Yönetici Arabirimine bağlanma.
2. **Hive**altında **CLUSTERNAME_Hive**seçin. Önceden yapılandırılmış iki ilke göreceksiniz.
3. **Yeni İlke Ekle'yi**seçin ve ardından aşağıdaki değerleri girin:

    |Özellik |Değer |
    |---|---|
    |İlke Adı|okuma-kovan örnek tablosu-hepsi|
    |Kovan Veritabanı|default|
    |tablo|kovan örnekleme tablosu|
    |Kovan Sütunu|*|
    |Kullanıcı Seçin|kovan user1|
    |İzinler|Seçin|

    ![HDInsight ESP Ranger Kovan politikaları yapılandırmak](./media/apache-domain-joined-run-hive/hdinsight-domain-joined-configure-ranger-policy.png).

    > [!NOTE]  
    > Select User alanında etki alanı kullanıcısı yoksa Ranger’ın AAD ile eşitlenmesi için birkaç dakika bekleyin.

4. İlkeyi kaydetmek için **Ekle'yi** seçin.

5. Son iki adımı tekrarlayarak aşağıdaki özelliklere sahip yeni bir ilke oluşturun:

    |Özellik |Değer |
    |---|---|
    |İlke Adı|okuma-kovan örnektablosu-devicemake|
    |Kovan Veritabanı|default|
    |tablo|kovan örnekleme tablosu|
    |Kovan sütunu|clientid, aygıt make|
    |Kullanıcı Seçin|kovan kullanıcısı2|
    |İzinler|Seçin|

## <a name="create-hive-odbc-data-source"></a>Hive ODBC veri kaynağı oluşturma

Talimatlara [Hive ODBC veri kaynağı oluşturma](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md) sayfasından erişebilirsiniz.  

 | Özellik  |Açıklama |
 | --- | --- |
 | Data Source Name | Veri kaynağınız için bir ad verin |
 | Host | CLUSTERNAME.azurehdinsight.net girin. Örnek: HDIKumesi.azurehdinsight.net |
 | Bağlantı noktası | **443** yazın. (Önceden 563 olan bu bağlantı noktası 443 olarak değiştirilmiştir.) |
 | Database | **Varsayılan**kullanın. |
 | Hive Server Type | **Hive Server 2**’yi seçin |
 | Mechanism | **Azure HDInsight Service**’i seçin |
 | HTTP Path | Boş bırakın. |
 | User Name | hiveuser1@contoso158.onmicrosoft.com yazın. Farklıysa etki alanı adını güncelleştirin. |
 | Parola | hiveuser1 kullanıcısının parolasını girin. |

Veri kaynağını kaydetmeden önce **Test**’e tıklayın.

## <a name="import-data-into-excel-from-hdinsight"></a>HDInsight’tan Excel’e veri aktarma

Son bölümde, iki ilke yapılandırıldınız.  hiveuser1 tüm sütunlarda select iznine, hiveuser2 ise iki sütunda select iznine sahiptir. Bu bölümde verileri Excel'e aktarmak için iki kullanıcının kimliğine bürüneceksiniz.

1. Excel’de yeni veya mevcut bir çalışma kitabını açın.

1. **Veri** sekmesinden, ODBC'den Veri Almak için**From Other Sources** > **ODBC'den** veri >  **alın'a**gidin. **From ODBC**

    ![Açık veri bağlantısı sihirbazı](./media/apache-domain-joined-run-hive/simbahiveodbc-excel-dataconnection1.png)

1. Açılan listeden, son bölümde oluşturduğunuz veri kaynağı adını seçin ve ardından **Tamam'ı**seçin.

1. İlk kullanım için bir **ODBC sürücü** iletişim kutusu açılır. Sol menüden **Windows'u** seçin. Ardından **Gezgin** penceresini açmak için **Bağlan'ı** seçin.

1. **Veritabanı ve Tablo Seç** iletişim kutusunun açılmasını bekleyin. Bu işlem birkaç saniye sürebilir.

1. **Kovan örnekleme tablosunu**seçin ve sonra **İleri'yi**seçin.

1. **Bitiş'i**seçin.

1. **Verileri İçeri Aktar** iletişim kutusunda sorguyu değiştirebilir veya belirtebilirsiniz. Bunu yapmak için **Özellikler'i**seçin. Bu işlem birkaç saniye sürebilir.

1. **Tanım** sekmesini seçin. Komut metni:

       SELECT * FROM "HIVE"."default"."hivesampletable"

   Tanımladığınız Ranger ilkelerine göre hiveuser1 kullanıcısı tüm sütunlarda select iznine sahiptir.  Yani bu sorgu hiveuser1 kimlik bilgileri ile çalışır, ancak bu sorgu hiveuser2 kimlik bilgileri ile çalışmıyor.

1. Bağlantı Özellikleri iletişim kutusunu kapatmak için **Tamam'ı** seçin.

1. **Verileri İçe Aktar** iletişim kutusunu kapatmak için **Tamam'ı** seçin.  

1. hiveuser1 kullanıcısının parolasını tekrar girin ve **Tamam**’a tıklayın. Verilerin Excel’e aktarılması birkaç saniye sürer. Bittiğinde, 11 veri sütunu göreceksiniz.

İkinci ilkeyi (read-hivesampletable-devicemake) test etmek için, son bölümde oluşturduğunuz

1. Excel'de yeni bir sayfa ekleyin.
2. Verileri içeri aktarmak için son yordamı uygulayın.  Yaptığınız tek değişiklik hiveuser2 kimlik bilgilerini kullanmak yerine hiveuser1's. Hiveuser2 yalnızca iki sütunu görme izni olduğundan bu başarısız olur. Şu hatayı alacaksınız:

        [Microsoft][HiveODBC] (35) Error from Hive: error code: '40000' error message: 'Error while compiling statement: FAILED: HiveAccessControlException Permission denied: user [hiveuser2] does not have [SELECT] privilege on [default/hivesampletable/clientid,country ...]'.
        
3. Verileri içe aktarmak için aynı yordamı uygulayın. Bu sefer hiveuser2 kullanıcısının kimlik bilgilerini kullanın ve select deyimi için:

        SELECT * FROM "HIVE"."default"."hivesampletable"

    yerine şunu yazın:

        SELECT clientid, devicemake FROM "HIVE"."default"."hivesampletable"

    Bu iş bittiğinde, içe aktarılan iki veri sütunu görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

* Bir HDInsight kümesini Kurumsal Güvenlik Paketi ile yapılandırmak için [ESP ile HDInsight kümelerini yapılandır'a](apache-domain-joined-configure.md)bakın.
* ESP ile HDInsight kümesini yönetmek için [ESP ile HDInsight kümelerini yönet'e](apache-domain-joined-manage.md)bakın.
* ESP ile HDInsight kümelerinde SSH kullanarak Hive sorguları çalıştırmak için, [HDInsight ile SSH'yi kullan'a](../hdinsight-hadoop-linux-use-ssh-unix.md#authentication-domain-joined-hdinsight)bakın.
* Hive JDBC kullanarak Hive'ı bağlamak için, [Hive JDBC sürücüsünü kullanarak Azure HDInsight'ta Apache Hive'a Bağlan'a](../hadoop/apache-hadoop-connect-hive-jdbc-driver.md) bakın
* Hive ODBC kullanarak Excel'i Hadoop'a bağlamak [için Microsoft Hive ODBC sürücüsüyle Excel'i Apache Hadoop'a bağlayın](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md)
* Güç Sorgusu kullanarak Excel'i Hadoop'a bağlamak için Power [Query kullanarak Excel'i Apache Hadoop'a bağlayın](../hadoop/apache-hadoop-connect-excel-power-query.md)
