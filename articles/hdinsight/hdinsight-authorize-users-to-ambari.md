---
title: Ambari Views için kullanıcıları yetkilendirme - Azure HDInsight
description: ESP etkinleştirilmiş HDInsight kümeleri için Ambari kullanıcı ve grup izinleri nasıl yönetilir.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/27/2019
ms.openlocfilehash: 3bb58334e96bf5378fb78b70125f9c7994a7c2fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75435655"
---
# <a name="authorize-users-for-apache-ambari-views"></a>Kullanıcıları Apache Ambari Görünümleri için yetkilendirme

[Kurumsal Güvenlik Paketi (ESP) etkin HDInsight kümeleri,](./domain-joined/hdinsight-security-overview.md) Azure Active Directory tabanlı kimlik doğrulaması da dahil olmak üzere kurumsal dereceli özellikler sağlar. Kümeye erişim sağlanmış olan Azure REKLAM gruplarına eklenen [yeni kullanıcıları eşitleyerek](hdinsight-sync-aad-users-to-cluster.md) belirli kullanıcıların belirli eylemleri gerçekleştirmesine izin verebilirsiniz. [Apache Ambari'deki](https://ambari.apache.org/) kullanıcılar, gruplar ve izinlerle çalışmak hem ESP HDInsight kümeleri hem de standart HDInsight kümeleri için desteklenir.

Etkin Dizin kullanıcıları etki alanı kimlik bilgilerini kullanarak küme düğümlerinde oturum açabilir. Ayrıca, [Hue,](https://gethue.com/)Ambari Views, ODBC, JDBC, PowerShell ve REST API'leri gibi diğer onaylı uç noktalarıyla küme etkileşimlerini doğrulamak için etki alanı kimlik bilgilerini de kullanabilirler.

> [!WARNING]  
> Linux tabanlı HDInsight kümenizdeki Ambari izleme örgütünün (hdinsightwatchdog) parolasını değiştirmeyin. Parolayı değiştirmek, komut dosyası eylemlerini kullanma veya kümenizle ölçekleme işlemleri gerçekleştirme yeteneğini bozar.

Bunu zaten yapmadıysanız, yeni bir ESP kümesi sağlamak için [bu yönergeleri](./domain-joined/apache-domain-joined-configure.md) izleyin.

## <a name="access-the-ambari-management-page"></a>Ambari yönetim sayfasına erişin

[Apache Ambari Web UI'deki Ambari](hdinsight-hadoop-manage-ambari.md) **yönetim sayfasına** `https://CLUSTERNAME.azurehdinsight.net`ulaşmak için . Küme oluştururken tanımladığınız küme yöneticisi kullanıcı adını ve parolasını girin. Ardından, Ambari **panosundan, yönetici** menüsünün altındaki **Ambari'yi** yönet'i'ni seçin:

![Apache Ambari pano yönetmek](./media/hdinsight-authorize-users-to-ambari/manage-apache-ambari.png)

## <a name="add-users"></a>Kullanıcı ekle

### <a name="add-users-through-the-portal"></a>Portal üzerinden kullanıcı ekleme

1. Yönetim sayfasından, **Kullanıcılar'ı**seçin.

    ![Apache Ambari yönetim sayfası kullanıcıları](./media/hdinsight-authorize-users-to-ambari/apache-ambari-management-page-users.png)

1. Seçin **+ Yerel Kullanıcı Oluştur**.

1. **Kullanıcı adı** ve **şifre**sağlayın. **Kaydet'i**seçin.

### <a name="add-users-through-powershell"></a>PowerShell ile kullanıcı ekleme

Aşağıdaki değişkenleri , , `CLUSTERNAME` `NEWUSER`ve `PASSWORD` uygun değerleri değiştirerek düzenleme.

```powershell
# Set-ExecutionPolicy Unrestricted

# Begin user input; update values
$clusterName="CLUSTERNAME"
$user="NEWUSER"
$userpass='PASSWORD'
# End user input

$adminCredentials = Get-Credential -UserName "admin" -Message "Enter admin password"

$clusterName = $clusterName.ToLower()
$createUserUrl="https://$($clusterName).azurehdinsight.net/api/v1/users"

$createUserBody=@{
    "Users/user_name" = "$user"
    "Users/password" = "$userpass"
    "Users/active" = "$true"
    "Users/admin" = "$false"
} | ConvertTo-Json

# Create user
$statusCode =
Invoke-WebRequest `
    -Uri $createUserUrl `
    -Credential $adminCredentials `
    -Method POST `
    -Headers @{"X-Requested-By" = "ambari"} `
    -Body $createUserBody | Select-Object -Expand StatusCode

if ($statusCode -eq 201) {
    Write-Output "User is created: $user"
}
else
{
    Write-Output 'User is not created'
    Exit
}

$grantPrivilegeUrl="https://$($clusterName).azurehdinsight.net/api/v1/clusters/$($clusterName)/privileges"

$grantPrivilegeBody=@{
    "PrivilegeInfo" = @{
        "permission_name" = "CLUSTER.USER"
        "principal_name" = "$user"
        "principal_type" = "USER"
    }
} | ConvertTo-Json

# Grant privileges
$statusCode =
Invoke-WebRequest `
    -Uri $grantPrivilegeUrl `
    -Credential $adminCredentials `
    -Method POST `
    -Headers @{"X-Requested-By" = "ambari"} `
    -Body $grantPrivilegeBody | Select-Object -Expand StatusCode

if ($statusCode -eq 201) {
    Write-Output 'Privilege is granted'
}
else
{
    Write-Output 'Privilege is not granted'
    Exit
}

Write-Host "Pausing for 100 seconds"
Start-Sleep -s 100

$userCredentials = "$($user):$($userpass)"
$encodedUserCredentials = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($userCredentials))
$zookeeperUrlHeaders = @{ Authorization = "Basic $encodedUserCredentials" }
$getZookeeperurl="https://$($clusterName).azurehdinsight.net/api/v1/clusters/$($clusterName)/services/ZOOKEEPER/components/ZOOKEEPER_SERVER"

# Perform query with new user
$zookeeperHosts =
Invoke-WebRequest `
    -Uri $getZookeeperurl `
    -Method Get `
    -Headers $zookeeperUrlHeaders

Write-Output $zookeeperHosts
```

### <a name="add-users-through-curl"></a>Curl ile kullanıcı ekleme

Aşağıdaki değişkenleri `CLUSTERNAME`, , `ADMINPASSWORD` `NEWUSER`, ve `USERPASSWORD` uygun değerleri değiştirerek düzenleme. Komut dosyası bash ile yürütülecek şekilde tasarlanmıştır. Windows komut istemi için küçük değişiklikler gerekir.

```bash
export clusterName="CLUSTERNAME"
export adminPassword='ADMINPASSWORD'
export user="NEWUSER"
export userPassword='USERPASSWORD'

# create user
curl -k -u admin:$adminPassword -H "X-Requested-By: ambari" -X POST \
-d "{\"Users/user_name\":\"$user\",\"Users/password\":\"$userPassword\",\"Users/active\":\"true\",\"Users/admin\":\"false\"}" \
https://$clusterName.azurehdinsight.net/api/v1/users

echo "user created: $user"

# grant permissions
curl -k -u admin:$adminPassword -H "X-Requested-By: ambari" -X POST \
-d '[{"PrivilegeInfo":{"permission_name":"CLUSTER.USER","principal_name":"'$user'","principal_type":"USER"}}]' \
https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/privileges

echo "Privilege is granted"

echo "Pausing for 100 seconds"
sleep 10s

# perform query using new user account
curl -k -u $user:$userPassword -H "X-Requested-By: ambari" \
-X GET "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER"
```

## <a name="grant-permissions-to-apache-hive-views"></a>Apache Hive görünümlerine izin verme

Ambari diğerleri arasında [Apache Hive](https://hive.apache.org/) ve [Apache TEZ](https://tez.apache.org/)için görünüm örnekleri ile birlikte gelir. Bir veya daha fazla Hive görünümü örneğine erişim sağlamak için **Ambari yönetim sayfasına**gidin.

1. Yönetim sayfasından, soldaki **Görünümler** menüsünün altındaki **Görünümler** bağlantısını seçin.

    ![Apache Ambari görünüm bağlantıları](./media/hdinsight-authorize-users-to-ambari/apache-ambari-views-link.png)

2. Görünümler **sayfasında, HIVE** satırını genişletin. Hive hizmeti kümeye eklendiğinde oluşturulan varsayılan bir Kovan görünümü vardır. Gerektiğinde daha fazla Kovan görünümü örneği de oluşturabilirsiniz. Hive görünümü seçin:

    ![HDInsight Görünümleri - Apache Hive görünümü](./media/hdinsight-authorize-users-to-ambari/views-apache-hive-view.png)

3. Görünüm sayfasının altına doğru ilerleyin. *İzinler* bölümünde, etki alanı kullanıcılarına görünüme izinlerini vermek için iki seçeneğiniz vardır:

**Bu kullanıcılara** ![izin verme Bu kullanıcılara izin verme](./media/hdinsight-authorize-users-to-ambari/hdi-add-user-to-view.png)

**Bu gruplara** ![izin verme Bu gruplara izin verme](./media/hdinsight-authorize-users-to-ambari/add-group-to-view-permission.png)

1. Kullanıcı eklemek için **Kullanıcı Ekle** düğmesini seçin.

   * Kullanıcı adını yazmaya başlayın ve daha önce tanımlanmış adların açılır listesini görürsünüz.

     ![Apache Ambari kullanıcı otomatik tamamlar](./media/hdinsight-authorize-users-to-ambari/ambari-user-autocomplete.png)

   * Kullanıcı adını seçin veya yazmayı bitirin. Bu kullanıcı adını yeni bir kullanıcı olarak eklemek için **Yeni** düğmesini seçin.

   * Değişikliklerinizi kaydetmek için **mavi onay kutusunu**seçin.

     ![Apache Ambari kullanıcı izni verir](./media/hdinsight-authorize-users-to-ambari/user-entered-permissions.png)

1. Grup eklemek için **Grup Ekle** düğmesini seçin.

   * Grup adını yazmaya başlayın. Varolan bir grup adını seçme veya yeni bir grup ekleme işlemi, kullanıcı eklemekle aynıdır.
   * Değişikliklerinizi kaydetmek için **mavi onay kutusunu**seçin.

     ![Apache Ambari hibe izinleri](./media/hdinsight-authorize-users-to-ambari/ambari-group-entered.png)

Kullanıcıları doğrudan bir görünüme eklemek, bir kullanıcıya bu görünümü kullanması için izin atamak istediğinizde, ancak ek izinleri olan bir grubun üyesi olmalarını istemediğiniz de yararlıdır. İdari ek yükü miktarını azaltmak için gruplara izin atamak daha kolay olabilir.

## <a name="grant-permissions-to-apache-tez-views"></a>Apache TEZ görüşlerine izin verme

[Apache TEZ](https://tez.apache.org/) görünüm örnekleri, kullanıcıların [Apache Hive](https://hive.apache.org/) sorguları ve [Apache Pig](https://pig.apache.org/) komut dosyaları tarafından gönderilen tüm Tez işlerini izlemelerine ve hata ayıklamalarına olanak sağlar. Küme sağlandığında oluşturulan bir varsayılan Tez görünümü örneği vardır.

Kullanıcıları ve grupları Tez görünüm örneğine atamak için, daha önce açıklandığı gibi Görünümler sayfasındaki **TEZ** satırını genişletin.

![HDInsight Görünümleri - Apache Tez görünümü](./media/hdinsight-authorize-users-to-ambari/views-apache-tez-view.png)

Kullanıcı veya grup eklemek için önceki bölümde 3 - 5 adımlarını yineleyin.

## <a name="assign-users-to-roles"></a>Kullanıcıları rollere atama

Kullanıcılar ve gruplar için, erişim izinlerini azaltmak amacıyla listelenen beş güvenlik rolü vardır:

* Küme Yöneticisi
* Küme Operatörü
* Hizmet Yöneticisi
* Servis Operatörü
* Küme Kullanıcısı

Rolleri yönetmek için **Ambari yönetim sayfasına**gidin ve ardından soldaki *Kümeler* menü grubundaki **Roller** bağlantısını seçin.

![Apache Ambari rolleri menü bağlantıları](./media/hdinsight-authorize-users-to-ambari/cluster-roles-menu-link.png)

Her role verilen izinlerin listesini görmek için, Roller sayfasındaki **Roller** tablosu üstbilgisinin yanındaki mavi soru işaretini tıklatın.

![Apache Ambari rolleri menü bağlantı izinleri](./media/hdinsight-authorize-users-to-ambari/roles-menu-permissions.png "Apache Ambari rolleri menü bağlantı izinleri")

Bu sayfada, kullanıcılar ve gruplar için rolleri yönetmek için kullanabileceğiniz iki farklı görünüm vardır: Engelleme ve Liste.

### <a name="block-view"></a>Engelleme görünümü

Blok görünümü her rolü kendi satırında görüntüler ve **bu kullanıcılara atama rolleri** sağlar ve daha önce açıklandığı gibi bu **gruplara rolleri ata.**

![Apache Ambari rolleri blok görünümü](./media/hdinsight-authorize-users-to-ambari/ambari-roles-block-view.png)

### <a name="list-view"></a>Liste görünümü

Liste görünümü iki kategoride hızlı düzenleme özellikleri sağlar: Kullanıcılar ve Gruplar.

* Liste görünümünün Kullanıcılar kategorisi, açılır listedeki her kullanıcı için bir rol seçmenize olanak tanıyan tüm kullanıcıların listesini görüntüler.

    ![Apache Ambari rolleri liste görünümü - kullanıcılar](./media/hdinsight-authorize-users-to-ambari/roles-list-view-users.png)

* Liste görünümünün Gruplar kategorisi tüm grupları ve her gruba atanan rolü görüntüler. Örneğimizde, grup listesi, kümenin Etki Alanı ayarlarının Access kullanıcı grubu özelliğinde belirtilen Azure REKLAM **gruplarından** senkronize edilir. Bkz. [ESP etkinleştirilmiş bir HDInsight kümesi oluşturun.](./domain-joined/apache-domain-joined-configure-using-azure-adds.md#create-an-hdinsight-cluster-with-esp)

    ![Apache Ambari rolleri liste görünümü - gruplar](./media/hdinsight-authorize-users-to-ambari/roles-list-view-groups.png)

    Yukarıdaki resimde, "kovan kullanıcıları" grubuna *Cluster User* rolü atanır. Bu, bu grubun kullanıcılarının hizmet yapılandırmalarını ve küme ölçümlerini görüntülemesine ancak değiştirmemesine olanak tanıyan salt okunur bir roldür.

## <a name="log-in-to-ambari-as-a-view-only-user"></a>Ambari'de yalnızca görüntüleme kullanıcısı olarak oturum açma

Azure AD etki alanı kullanıcımızı Hive ve Tez görünümlerine "hiveuser1" izinleri atadık. Ambari Web Kullanıcı Arabirimi'ni başlattığımızda ve bu kullanıcının etki alanı kimlik bilgilerini (e-posta biçiminde Azure AD kullanıcı adı ve parola) girdiğimizde, kullanıcı Ambari Görünümleri sayfasına yönlendirilir. Buradan, kullanıcı erişilebilir herhangi bir görünümü seçebilir. Kullanıcı, pano, hizmetler, ana bilgisayarlar, uyarılar veya yönetici sayfaları da dahil olmak üzere sitenin başka bir bölümünü ziyaret edemez.

![Yalnızca görünümleri olan Apache Ambari kullanıcısı](./media/hdinsight-authorize-users-to-ambari/ambari-user-views-only.png)

## <a name="log-in-to-ambari-as-a-cluster-user"></a>Küme kullanıcısı olarak Ambari'de oturum açma

Azure AD etki alanı kullanıcımızı *Cluster User* rolüne "hiveuser2" olarak atadık. Bu rol panoya ve tüm menü öğelerine erişebilir. Küme kullanıcısının yöneticiden daha az izin verilen seçeneği vardır. Örneğin, hiveuser2 her hizmet için yapılandırmaları görüntüleyebilir, ancak bunları kaldıramaz.

![Apache Ambari pano ekranı](./media/hdinsight-authorize-users-to-ambari/user-cluster-user-role.png)

## <a name="next-steps"></a>Sonraki adımlar

* [ESP ile HDInsight'ta Apache Hive ilkelerini yapılandırın](./domain-joined/apache-domain-joined-run-hive.md)
* [ESP HDInsight kümelerini yönetme](./domain-joined/apache-domain-joined-manage.md)
* [HDInsight'ta Apache Hadoop ile Apache Hive Görünümü'ni kullanın](hadoop/apache-hadoop-use-hive-ambari-view.md)
* [Azure AD kullanıcılarını kümeyle senkronize etme](hdinsight-sync-aad-users-to-cluster.md)
* [Apache Ambari REST API'yi kullanarak HDInsight kümelerini yönetme](./hdinsight-hadoop-manage-ambari-rest-api.md)
