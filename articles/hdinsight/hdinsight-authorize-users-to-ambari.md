---
title: Kullanıcıları, ambarı görünümleri için yetkilendirme-Azure HDInsight
description: ESP özellikli HDInsight kümeleri için ambarı Kullanıcı ve grup izinlerini yönetme.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/26/2017
ms.author: hrasheed
ms.openlocfilehash: 28f30270ab0a6c057ee583ccebc2a8540980c6cc
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68442179"
---
# <a name="authorize-users-for-apache-ambari-views"></a>Kullanıcıları Apache Ambari Görünümleri için yetkilendirme

[Kurumsal güvenlik paketi (ESP) etkin HDInsight kümeleri](./domain-joined/hdinsight-security-overview.md) , Azure Active Directory tabanlı kimlik doğrulaması da dahil olmak üzere kurumsal düzeyde yetenekler sağlar. Kümeye erişim sağlanmış olan Azure AD gruplarına eklenen [Yeni kullanıcıları eşitleyebilir](hdinsight-sync-aad-users-to-cluster.md) ve bu kullanıcıların belirli eylemleri gerçekleştirmesine izin verebilirsiniz. [Apache ambarı](https://ambari.apache.org/) 'nda kullanıcılar, gruplar ve izinlerle çalışma, hem ESP HDInsight kümeleri hem de standart HDInsight kümeleri için desteklenir.

Active Directory Kullanıcılar, etki alanı kimlik bilgilerini kullanarak küme düğümlerinde oturum açabilirler. Ayrıca, [ton](https://gethue.com/), ambarı GÖRÜNÜMLERI, ODBC, JDBC, POWERSHELL ve REST API 'ler gibi onaylanan diğer uç noktalarla küme etkileşimlerinin kimliğini doğrulamak için etki alanı kimlik bilgilerini de kullanabilirler.

> [!WARNING]  
> Linux tabanlı HDInsight kümenizdeki ambarı izleme (hdinsightwatchdog) parolasını değiştirmeyin. Parola değiştirme, betik eylemlerini kullanma veya kümeniz ile ölçeklendirme işlemleri gerçekleştirme yeteneğini keser.

Daha önce yapmadıysanız, yeni bir ESP kümesi sağlamak için [Bu yönergeleri](./domain-joined/apache-domain-joined-configure.md) izleyin.

## <a name="access-the-ambari-management-page"></a>Ambarı yönetim sayfasına erişin

[Apache ambarı Web Kullanıcı arabirimindeki](hdinsight-hadoop-manage-ambari.md) **`https://<YOUR CLUSTER NAME>.azurehdinsight.net`** **ambarı yönetim sayfasına** ulaşmak için, sayfasına gidin. Kümeyi oluştururken tanımladığınız küme yönetici kullanıcı adını ve parolasını girin. Ardından, ambarı panosundan **Yönetim** menüsünün altındaki **ambarı Yönet** ' i seçin:

![Ambarı yönetme](./media/hdinsight-authorize-users-to-ambari/manage-ambari.png)

## <a name="grant-permissions-to-apache-hive-views"></a>Apache Hive görünümlerine izin verme

Ambarı, diğerleri arasında [Apache Hive](https://hive.apache.org/) ve [Apache tez](https://tez.apache.org/)için görünüm örnekleriyle gelir. Bir veya daha fazla Hive görünümü örneğine erişim vermek için, **ambarı yönetim sayfasına**gidin.

1. Yönetim sayfasından, sol taraftaki **Görünümler** menü başlığı altında bulunan **Görünümler** bağlantısını seçin.

    ![Görünümler bağlantısı](./media/hdinsight-authorize-users-to-ambari/views-link.png)

2. Görünümler sayfasında **HIVE** satırını genişletin. Hive hizmeti kümeye eklendiğinde oluşturulan bir varsayılan Hive görünümü vardır. Ayrıca, gerektiğinde daha fazla Hive görünüm örneği de oluşturabilirsiniz. Hive görünümü seçin:

    ![Görünümler-Hive görünümü](./media/hdinsight-authorize-users-to-ambari/views-hive-view.png)

3. Görünüm sayfasının alt kısmına doğru ilerleyin. *İzinler* bölümünde, etki alanı kullanıcılarının görünüme izinleri vermek için iki seçeneğiniz vardır:

**Bu kullanıcılara Izin ver** ![Bu kullanıcılara izin ver](./media/hdinsight-authorize-users-to-ambari/add-user-to-view.png)

**Bu gruplara Izin ver** ![Bu gruplara izin ver](./media/hdinsight-authorize-users-to-ambari/add-group-to-view.png)

1. Kullanıcı eklemek için **Kullanıcı Ekle** düğmesini seçin.

   * Kullanıcı adını yazmaya başlayın ve önceden tanımlanmış adların açılan listesini görürsünüz.

     ![Kullanıcı oto tamamlandığında](./media/hdinsight-authorize-users-to-ambari/user-autocomplete.png)

   * Kullanıcı adını seçin veya yazın. Bu Kullanıcı adını yeni bir kullanıcı olarak eklemek için **Yeni** düğmesini seçin.

   * Değişikliklerinizi kaydetmek için **mavi onay kutusunu**seçin.

     ![Kullanıcı girdi](./media/hdinsight-authorize-users-to-ambari/user-entered.png)

1. Bir grup eklemek için **Grup Ekle** düğmesini seçin.

   * Grup adını yazmaya başlayın. Var olan bir grup adını seçme veya yeni bir grup ekleme işlemi, Kullanıcı ekleme ile aynıdır.
   * Değişikliklerinizi kaydetmek için **mavi onay kutusunu**seçin.

     ![Girilen Grup](./media/hdinsight-authorize-users-to-ambari/group-entered.png)

Kullanıcıları doğrudan bir görünüme eklemek, bu görünümü kullanmak için bir kullanıcıya izin atamak istediğinizde, ancak ek izinlere sahip bir grubun üyesi olmasını istemediğiniz durumlarda faydalıdır. Yönetim yükü miktarını azaltmak için gruplara izin atama daha kolay olabilir.

## <a name="grant-permissions-to-apache-tez-views"></a>Apache TEZ görünümlerine izin verme

[Apache tez](https://tez.apache.org/) görünüm örnekleri, kullanıcıların [Apache Hive](https://hive.apache.org/) sorguları ve [Apache Pig](https://pig.apache.org/) betikleri tarafından gönderilen tüm tez işlerini izlemelerine ve hatalarını ayıklamasına olanak tanır. Küme sağlandığında oluşturulan bir varsayılan tez görünüm örneği vardır.

Bir tez görünümü örneğine kullanıcılar ve gruplar atamak için, görünümler sayfasındaki **tez** satırını daha önce açıklandığı gibi genişletin.

![Görünümler-tez görünümü](./media/hdinsight-authorize-users-to-ambari/views-tez-view.png)

Kullanıcı veya grup eklemek için önceki bölümde 3-5 arasındaki adımları yineleyin.

## <a name="assign-users-to-roles"></a>Rollere kullanıcı atama

Kullanıcılar ve gruplar için, erişim izinleri azaltma sırasıyla listelenen beş güvenlik rolü vardır:

* Küme Yöneticisi
* Küme Işleci
* Hizmet Yöneticisi
* Hizmet operatörü
* Küme kullanıcısı

Rolleri yönetmek için, **ambarı yönetimi sayfasına**gidin, ardından sol taraftaki *kümeler* menü grubunda bulunan **Roller** bağlantısını seçin.

![Roller menü bağlantısı](./media/hdinsight-authorize-users-to-ambari/roles-link.png)

Her role verilen izinlerin listesini görmek için Roller sayfasındaki **Roller** tablosu üst bilgisinin yanındaki mavi soru işaretine tıklayın.

![Roller menü bağlantısı](./media/hdinsight-authorize-users-to-ambari/roles-permissions.png)

Bu sayfada, Kullanıcı ve grupların rollerini yönetmek için kullanabileceğiniz iki farklı görünüm vardır: Engelle ve listele.

### <a name="block-view"></a>Görünümü engelle

Blok görünümü her bir rolü kendi satırında görüntüler ve daha önce açıklandığı gibi bu **kullanıcılara roller ata** ve **Bu gruplara roller ata** seçeneklerini sunar.

![Rol blok görünümü](./media/hdinsight-authorize-users-to-ambari/roles-block-view.png)

### <a name="list-view"></a>Liste görünümü

Liste görünümü iki kategoride Hızlı Düzenle özellikleri sağlar: Kullanıcılar ve gruplar.

* Liste görünümünün kullanıcılar kategorisi, tüm kullanıcıların bir listesini görüntüleyerek, açılan listeden her bir kullanıcı için bir rol seçmenize olanak sağlar.

    ![Rol listesi görünümü-kullanıcılar](./media/hdinsight-authorize-users-to-ambari/roles-list-view-users.png)

*  Liste görünümünün gruplar kategorisi, tüm grupları ve her gruba atanan rolü görüntüler. Örneğimizde, grupların listesi, kümenin etki alanı ayarlarının **Erişim Kullanıcı grubu** özelliğinde BELIRTILEN Azure AD gruplarından eşitlenir. Bkz. [ESP özellikli HDInsight kümesi oluşturma](./domain-joined/apache-domain-joined-configure-using-azure-adds.md#create-a-hdinsight-cluster-with-esp).

    ![Rol listesi görünümü-gruplar](./media/hdinsight-authorize-users-to-ambari/roles-list-view-groups.png)

    Yukarıdaki görüntüde, "hiveusers" grubuna *küme Kullanıcı* rolü atanır. Bu, bu grubun kullanıcılarının hizmet yapılandırmasını ve küme ölçümlerini görüntülemesine izin veren, ancak değiştirmediğinden yalnızca bir salt okuma rolüdür.

## <a name="log-in-to-ambari-as-a-view-only-user"></a>Yalnızca bir görüntüleme kullanıcısı olarak ambarı 'nda oturum açın

"Hiveuser1 kullanıcısının" Azure AD etki alanı kullanıcımızı Hive ve tez görünümlerine atamamız gerekir. Ambarı Web Kullanıcı arabirimini başlatırken ve bu kullanıcının etki alanı kimlik bilgilerini (e-posta biçiminde ve parola olarak Azure AD Kullanıcı adı) girerken, Kullanıcı, ambarı görünümleri sayfasına yönlendirilir. Kullanıcı, buradan erişilebilir bir görünüm seçebilir. Kullanıcı Pano, hizmetler, konaklar, uyarılar veya yönetici sayfaları dahil olmak üzere sitenin başka bir bölümünü ziyaret edemez.

![Yalnızca görünümlere sahip Kullanıcı](./media/hdinsight-authorize-users-to-ambari/user-views-only.png)

## <a name="log-in-to-ambari-as-a-cluster-user"></a>Bir küme kullanıcısı olarak ambarı 'nda oturum açın

"Hiveuser2" Azure AD etki alanı kullanıcımızı *küme Kullanıcı* rolüne atadık. Bu rol panoya ve tüm menü öğelerine erişebiliyor. Bir küme kullanıcısı bir yöneticiden daha az izin verilen seçeneklere sahiptir. Örneğin, hiveuser2 her bir hizmetin yapılandırmasını görüntüleyebilir, ancak düzenleyemezsiniz.

![Küme kullanıcı rolüne sahip Kullanıcı](./media/hdinsight-authorize-users-to-ambari/user-cluster-user-role.png)

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight 'ta ESP ile Apache Hive ilkelerini yapılandırma](./domain-joined/apache-domain-joined-run-hive.md)
* [ESP HDInsight kümelerini yönetme](./domain-joined/apache-domain-joined-manage.md)
* [HDInsight 'ta Apache Hadoop ile Apache Hive görünümünü kullanma](hadoop/apache-hadoop-use-hive-ambari-view.md)
* [Azure AD kullanıcılarını kümeyle eşitler](hdinsight-sync-aad-users-to-cluster.md)
