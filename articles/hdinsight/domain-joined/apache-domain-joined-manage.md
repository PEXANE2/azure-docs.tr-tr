---
title: Kurumsal Güvenlik Paketi kümelerini yönetme - Azure HDInsight
description: Kurumsal Güvenlik Paketi ile Azure HDInsight kümelerini nasıl yönetebilirsiniz öğrenin.
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/04/2019
ms.openlocfilehash: 93880269edd72477f3aa85b2dbdc9d9f3ec8ef25
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75435925"
---
# <a name="manage-hdinsight-clusters-with-enterprise-security-package"></a>Kurumsal Güvenlik Paketi ile HDInsight kümelerini yönetme

HDInsight Kurumsal Güvenlik Paketi'ndeki (ESP) kullanıcıları ve rolleri ve ESP kümelerini nasıl yönetişten öğrenin.

## <a name="use-vscode-to-link-to-domain-joined-cluster"></a>Etki alanına katılmış kümeye bağlamak için VSCode kullanma

Apache Ambari yönetilen kullanıcı adını kullanarak normal bir kümebağlayabilir, ayrıca etki alanı kullanıcı adını kullanarak bir `user1@contoso.com`güvenlik Apache Hadoop kümesini bağlayabilirsiniz (örneğin: ).

1. Açık [Görsel Stüdyo Kodu](https://code.visualstudio.com/). Kıvılcım [& Kovan Araçları](../hdinsight-for-vscode.md) uzantısı yüklü olduğundan emin olun.

1. Visual Studio Code için [bir kümeye bağlantı](../hdinsight-for-vscode.md#link-a-cluster) kurun' dan adımları izleyin.

## <a name="use-intellij-to-link-to-domain-joined-cluster"></a>Etki alanına katılmış kümeye bağlamak için IntelliJ kullanma

Ambari yönetilen kullanıcı adını kullanarak normal bir kümebağlayabilirsiniz, ayrıca etki alanı kullanıcı adını `user1@contoso.com`kullanarak bir güvenlik hadoop kümesibağ (örneğin: ).

1. IntelliJ IDEA’yı açın. Tüm [ön koşulların](../spark/apache-spark-intellij-tool-plugin.md#prerequisites) karşılandığından emin olun.

1. IntelliJ için [bir kümeye bağlantı](../spark/apache-spark-intellij-tool-plugin.md#link-a-cluster) kurun adımlarını izleyin.

## <a name="use-eclipse-to-link-to-domain-joined-cluster"></a>Etki alanına katılmış kümeye bağlamak için Eclipse kullanma

Ambari yönetilen kullanıcı adını kullanarak normal bir kümebağlayabilirsiniz, ayrıca etki alanı kullanıcı adını `user1@contoso.com`kullanarak bir güvenlik hadoop kümesibağ (örneğin: ).

1. Eclipse’i açın. Tüm [ön koşulların](../spark/apache-spark-eclipse-tool-plugin.md#prerequisites) karşılandığından emin olun.

1. Eclipse için [bir kümeye bağlantı](../spark/apache-spark-eclipse-tool-plugin.md#link-a-cluster) kurun' daki adımları izleyin.

## <a name="access-the-clusters-with-enterprise-security-package"></a>Kurumsal Güvenlik Paketi ile kümelere erişin

Kurumsal Güvenlik Paketi (daha önce HDInsight Premium olarak da bilinir), kimlik doğrulamanın Active Directory ve Apache Ranger ve Storage ACLs (ADLS ALA'lar) tarafından yetkilendirilmesiyle yapıldığı kümeye çok kullanıcılı erişim sağlar. Yetkilendirme, birden çok kullanıcı arasında güvenli sınırlar sağlar ve yalnızca ayrıcalıklı kullanıcıların yetkilendirme ilkelerine dayalı verilere erişmesine olanak tanır.

Kurumsal Güvenlik Paketi ile hdinsight kümesi için güvenlik ve kullanıcı yalıtımı önemlidir. Bu gereksinimleri karşılamak için, Kurumsal Güvenlik Paketi ile kümeye SSH erişimi engellenir. Aşağıdaki tablo, her küme türü için önerilen erişim yöntemlerini gösterir:

|İş yükü|Senaryo|Erişim Yöntemi|
|--------|--------|-------------|
|Apache Hadoop|Kovan – İnteraktif İşler/Sorgular  |<ul><li>[Beeline](#beeline)</li><li>[Hive Görünümü](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Güç BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Görsel Stüdyo Araçları](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Apache Spark|Etkileşimli İşler/Sorgular, PySpark etkileşimli|<ul><li>[Beeline](#beeline)</li><li>[Livy ile Zeppelin](../spark/apache-spark-zeppelin-notebook.md)</li><li>[Hive Görünümü](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Güç BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Görsel Stüdyo Araçları](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Apache Spark|Toplu Senaryolar - Spark gönder, PySpark|<ul><li>[Livy](../spark/apache-spark-livy-rest-interface.md)</li></ul>|
|Etkileşimli Sorgu (LLAP)|Etkileşimli|<ul><li>[Beeline](#beeline)</li><li>[Hive Görünümü](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Güç BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Görsel Stüdyo Araçları](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Herhangi biri|Özel Uygulama Yükle|<ul><li>[Komut Dosyası Eylemleri](../hdinsight-hadoop-customize-cluster-linux.md)</li></ul>|

   > [!NOTE]  
   > Jupyter, Kurumsal Güvenlik Paketi'ne yüklenmez/desteklenmez.

Standart API'lerin kullanılması güvenlik açısından yardımcı olur. Ayrıca aşağıdaki avantajlardan da yararlanırsınız:

- **Yönetim** – Standart API'leri kullanarak kodunuzu yönetebilir ve işleri otomatikleştirebilirsiniz – Livy, HS2 vb.
- **Denetim** – SSH ile, hangi kullanıcıların kümeye ssh'd'i tetkik etmenin bir yolu yoktur. İşler, kullanıcı bağlamında yürütüleceği için standart uç noktalar üzerinden oluşturulduğunda bu durum böyle olmaz.

### <a name="use-beeline"></a><a name="beeline"></a>Beeline kullanma

Makinenize Beeline yükleyin ve genel internet üzerinden bağlanın, aşağıdaki parametreleri kullanın:

```
- Connection string: -u 'jdbc:hive2://<clustername>.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'
- Cluster login name: -n admin
- Cluster login password -p 'password'
```

Beeline yerel olarak yüklüyse ve bir Azure Sanal Ağı üzerinden bağlanın, aşağıdaki parametreleri kullanın:

```
Connection string: -u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'
```

Bir headnodun tam nitelikli etki alanı adını bulmak için Ambari REST API belgesini kullanarak HDInsight'ı yönet'deki bilgileri kullanın.

## <a name="users-of-hdinsight-clusters-with-esp"></a>ESP ile HDInsight kümelerinin kullanıcıları

ESP olmayan bir HDInsight kümesinde küme oluşturma sırasında oluşturulan iki kullanıcı hesabı vardır:

- **Ambari admin**: Bu hesap *hadoop kullanıcı* veya HTTP *kullanıcı*olarak da bilinir. Bu hesap Ambari'de oturum açabilmek için `https://CLUSTERNAME.azurehdinsight.net`kullanılabilir. Ayrıca Ambari görünümlerinde sorguları çalıştırmak, harici araçlar (örneğin PowerShell, Templeton, Visual Studio) aracılığıyla işleri yürütmek ve Hive ODBC sürücüsü ve BI araçları (örneğin, Excel, Power BI veya Tableau) ile kimlik doğrulaması yapmak için de kullanılabilir.

ESP'li bir HDInsight kümesinin Ambari Admin'e ek olarak üç yeni kullanıcısı vardır.

- **Ranger admin**: Bu hesap yerel Apache Ranger yönetici hesabıdır. Etkin bir dizin etki alanı kullanıcısı değildir. Bu hesap, ilkeleri kurulum ve diğer kullanıcıların yönetici veya temsilci yöneticileri (böylece bu kullanıcıların ilkeleri yönetebilirsiniz) yapmak için kullanılabilir. Varsayılan olarak, kullanıcı adı *yöneticidir* ve parola Ambari yönetici parolasıyla aynıdır. Parola Ranger'daki Ayarlar sayfasından güncellenebilir.

- **Cluster admin etki alanı kullanıcısı**: Bu hesap Ambari ve Ranger dahil hadoop küme yöneticisi olarak atanan etkin bir dizin etki alanı kullanıcısıdır. Küme oluşturma sırasında bu kullanıcının kimlik bilgilerini sağlamanız gerekir. Bu kullanıcı aşağıdaki ayrıcalıklara sahiptir:
    - Makinelere etki alanına katılın ve küme oluşturma sırasında belirttiğiniz OU'nun içine yerleştirin.
    - Küme oluşturma sırasında belirttiğiniz OU içinde hizmet ilkeleri oluşturun.
    - Ters DNS girişleri oluşturun.

    Diğer AD kullanıcılarının da bu ayrıcalıklara sahip olduğunu unutmayın.

    Küme içinde ranger tarafından yönetilmeyen ve dolayısıyla güvenli olmayan bazı uç noktalar (örneğin, Templeton) vardır. Bu uç noktalar küme yöneticisi etki alanı kullanıcısı dışındaki tüm kullanıcılar için kilitlenir.

- **Regular**: Küme oluşturma sırasında birden çok etkin dizin grubu sağlayabilirsiniz. Bu gruplardaki kullanıcılar Ranger ve Ambari ile senkronize edilir. Bu kullanıcılar etki alanı kullanıcılarıdır ve yalnızca Ranger tarafından yönetilen uç noktalara (örneğin, Hiveserver2) erişebilirler. Tüm RBAC politikaları ve denetimi bu kullanıcılar için geçerli olacaktır.

## <a name="roles-of-hdinsight-clusters-with-esp"></a>ESP ile HDInsight kümelerinin rolleri

HDInsight Kurumsal Güvenlik Paketi aşağıdaki rolleri vardır:

- Küme Yöneticisi
- Küme Operatörü
- Hizmet Yöneticisi
- Servis Operatörü
- Küme Kullanıcısı

**Bu rollerin izinlerini görmek için**

1. Ambari Management UI'yi açın.  [Bkz. Ambari Management UI'yi açın.](#open-the-ambari-management-ui)
2. Sol menüden **Roller'i**seçin.
3. İzinleri görmek için mavi soru işaretini seçin:

    ![ESP HDInsight rolleri izinleri](./media/apache-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-the-ambari-management-ui"></a>Ambari Management UI'yi açın

1. CLUSTERNAME'nin `https://CLUSTERNAME.azurehdinsight.net/` kümenizin adı olduğu yere gidin.
1. Küme yöneticisi etki alanı kullanıcı adı ve parolasını kullanarak Ambari'de oturum açın.
1. Sağ üst köşeden **yönetici** açılır menüsünü seçin ve ardından **Ambari'yi Yönet'i**seçin.

    ![ESP HDInsight Apache Ambari yönetmek](./media/apache-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    UI gibi görünüyor:

    ![ESP HDInsight Apache Ambari yönetim UI](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-the-domain-users-synchronized-from-your-active-directory"></a>Etkin Dizininizden senkronize edilen etki alanı kullanıcılarını listele

1. Ambari Management UI'yi açın.  [Bkz. Ambari Management UI'yi açın.](#open-the-ambari-management-ui)
2. Sol menüden **Kullanıcılar'ı**seçin. Aktif Dizininizden HDInsight kümesine senkronize edilen tüm kullanıcıları göreceksiniz.

    ![ESP HDInsight Ambari yönetimi kullanıcı listesi kullanıcıları](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-the-domain-groups-synchronized-from-your-active-directory"></a>Etkin Dizininizden eşitlenen etki alanı gruplarını listele

1. Ambari Management UI'yi açın.  [Bkz. Ambari Management UI'yi açın.](#open-the-ambari-management-ui)
2. Sol menüden **Gruplar'ı**seçin. Active Directory'nizden HDInsight kümesine senkronize edilen tüm grupları göreceksiniz.

    ![ESP HDInsight Ambari yönetimi UI liste grupları](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a>Hive Görünümlerini Yapılandırma izinleri

1. Ambari Management UI'yi açın.  [Bkz. Ambari Management UI'yi açın.](#open-the-ambari-management-ui)
2. Sol menüden **Görünümler'i**seçin.
3. Ayrıntıları göstermek için **KOVAN'ı** seçin.

    ![ESP HDInsight Ambari yönetimi UI Hive Görünümler](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)

4. Hive Görünümlerini yapılandırmak için **Kovan Görünümü** bağlantısını seçin.
5. **İzinler** bölümüne gidin.

    ![ESP HDInsight Ambari yönetimi UI Hive Görünümleri yapılandırma izinleri](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)

6. **Kullanıcı Ekle** veya **Ekle Grubu'nu**seçin ve ardından Kovan Görünümleri'ni kullanabilen kullanıcıları veya grupları belirtin.

## <a name="configure-users-for-the-roles"></a>Roller için kullanıcıları yapılandırma

 Rollerin ve izinlerinin listesini görmek için ESP'li HDInsight kümelerinin rolleri bölümüne bakın.

1. Ambari Management UI'yi açın.  [Bkz. Ambari Management UI'yi açın.](#open-the-ambari-management-ui)
2. Sol menüden **Roller'i**seçin.
3. Kullanıcıları ve grupları farklı rollere atamak için **Kullanıcı Ekle** veya **Grup Ekle'yi** seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Bir HDInsight kümesini Kurumsal Güvenlik Paketi ile yapılandırmak için [ESP ile HDInsight kümelerini yapılandır'a](apache-domain-joined-configure.md)bakın.
- Hive ilkelerini yapılandırmak ve Hive sorgularını çalıştırmak [için, ESP ile HDInsight kümeleri için Apache Hive ilkelerini yapılandırın'a](apache-domain-joined-run-hive.md)bakın.
