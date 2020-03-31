---
title: Azure Active Directory kullanıcılarını HDInsight kümesiyle senkronize etme
description: Azure Active Directory'den kimlik doğrulaması yapılan kullanıcıları bir HDInsight kümesine senkronize edin.
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/21/2019
ms.openlocfilehash: 299d242c38152db6a471159d1f3d2803598c1832
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75744853"
---
# <a name="synchronize-azure-active-directory-users-to-an-hdinsight-cluster"></a>Azure Active Directory kullanıcılarını HDInsight kümesine eşitleme

[Kurumsal Güvenlik Paketi (ESP) ile HDInsight kümeleri,](hdinsight-domain-joined-introduction.md) Azure Active Directory (Azure AD) kullanıcılarıyla güçlü kimlik doğrulamanın yanı sıra *rol tabanlı erişim denetimi* (RBAC) ilkelerini de kullanabilir. Azure AD'ye kullanıcı ve grup eklediğinizde, kümenize erişmeniz gereken kullanıcıları eşitleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bunu zaten yapmadıysanız, [Kurumsal Güvenlik Paketi ile bir HDInsight kümesi oluşturun.](hdinsight-domain-joined-configure.md)

## <a name="add-new-azure-ad-users"></a>Yeni Azure AD kullanıcıları ekleme

To view your hosts, open the Ambari Web UI. Her düğüm yeni katılımsız yükseltme ayarlarıyla güncelleştirilir.

1. Azure [portalından](https://portal.azure.com)ESP kümenizle ilişkili Azure REKLAM dizinine gidin.

2. Sol menüden **Tüm kullanıcıları** seçin ve ardından **Yeni kullanıcıyı**seçin.

    ![Azure portalı kullanıcıları ve grupları tümü](./media/hdinsight-sync-aad-users-to-cluster/users-and-groups-new.png)

3. Yeni kullanıcı formunu doldurun. Küme tabanlı izinler atamak için oluşturduğunuz grupları seçin. Bu örnekte, yeni kullanıcılar atayabileceğiniz "HiveUsers" adlı bir grup oluşturun. ESP kümesi oluşturmak için [örnek yönergeler](hdinsight-domain-joined-configure.md) iki grup `HiveUsers` eklemeyi içerir ve. `AAD DC Administrators`

    ![Azure portal kullanıcı bölmesi seçili gruplar](./media/hdinsight-sync-aad-users-to-cluster/hdinsight-new-user-form.png)

4. **Oluştur'u**seçin.

## <a name="use-the-apache-ambari-rest-api-to-synchronize-users"></a>Kullanıcıları senkronize etmek için Apache Ambari REST API'yi kullanın

Küme oluşturma işlemi sırasında belirtilen kullanıcı grupları o anda eşitlenir. Kullanıcı eşitleme işlemi saatte bir olmak üzere otomatik olarak gerçekleşir. Kullanıcıları hemen eşitlemek veya küme oluşturma sırasında belirtilen gruplar dışında bir grubu eşitlemek için Ambari REST API'yi kullanın.

Aşağıdaki yöntem Ambari REST API ile POST kullanır. Daha fazla bilgi için [Apache Ambari REST API'yi kullanarak HDInsight kümelerini yönet'e](hdinsight-hadoop-manage-ambari-rest-api.md)bakın.

1. Kümenize bağlanmak için [ssh komutunu](hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. Kümenizin adını değiştirerek `CLUSTERNAME` aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Kimlik doğrulaması yaptıktan sonra aşağıdaki komutu girin:

    ```bash
    curl -u admin:PASSWORD -sS -H "X-Requested-By: ambari" \
    -X POST -d '{"Event": {"specs": [{"principal_type": "groups", "sync_type": "existing"}]}}' \
    "https://CLUSTERNAME.azurehdinsight.net/api/v1/ldap_sync_events"
    ```

    Yanıt şu şekilde görünmelidir:

    ```json
    {
      "resources" : [
        {
          "href" : "http://<ACTIVE-HEADNODE-NAME>.<YOUR DOMAIN>.com:8080/api/v1/ldap_sync_events/1",
          "Event" : {
            "id" : 1
          }
        }
      ]
    }
    ```

1. Eşitleme durumunu görmek için yeni `curl` bir komut uygulayın:

    ```bash
    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/ldap_sync_events/1
    ```

    Yanıt şu şekilde görünmelidir:

    ```json
    {
      "href" : "http://<ACTIVE-HEADNODE-NAME>.YOURDOMAIN.com:8080/api/v1/ldap_sync_events/1",
      "Event" : {
        "id" : 1,
        "specs" : [
          {
            "sync_type" : "existing",
            "principal_type" : "groups"
          }
        ],
        "status" : "COMPLETE",
        "status_detail" : "Completed LDAP sync.",
        "summary" : {
          "groups" : {
            "created" : 0,
            "removed" : 0,
            "updated" : 0
          },
          "memberships" : {
            "created" : 1,
            "removed" : 0
          },
          "users" : {
            "created" : 1,
            "removed" : 0,
            "skipped" : 0,
            "updated" : 0
          }
        },
        "sync_time" : {
          "end" : 1497994072182,
          "start" : 1497994071100
        }
      }
    }
    ```

1. Bu sonuç, durumun **TAM**olduğunu, yeni bir kullanıcı oluşturulduğunu ve kullanıcıya bir üyelik atandığını gösterir. Bu örnekte, kullanıcı Azure AD'de aynı gruba eklenmiştir, çünkü kullanıcı "HiveUsers" senkronize LDAP grubuna atanır.

    > [!NOTE]  
    > Önceki yöntem, yalnızca küme oluşturma sırasında etki alanı ayarlarının **Access kullanıcı grubu** özelliğinde belirtilen Azure REKLAM gruplarını eşitler. Daha fazla bilgi için [bkz.](domain-joined/apache-domain-joined-configure.md)

## <a name="verify-the-newly-added-azure-ad-user"></a>Yeni eklenen Azure AD kullanıcısını doğrulama

Yeni Azure AD kullanıcısının eklandığını doğrulamak için [Apache Ambari Web Kullanıcı](hdinsight-hadoop-manage-ambari.md) Arabirimi'ni açın. Ambari Web UI'ye göz **`https://CLUSTERNAME.azurehdinsight.net`** atarak erişin. Küme yöneticisi kullanıcı adı ve parolasını girin.

1. Ambari **panosundan, yönetici** menüsü altında **Ambari'yi** Yönet'i'ni seçin.

    ![Apache Ambari panosu Ambari'yi Yönet](./media/hdinsight-sync-aad-users-to-cluster/manage-apache-ambari.png)

2. Sayfanın sol tarafındaki **Kullanıcı + Grup Yönetimi** menü grubu altındaki **Kullanıcıları** seçin.

    ![HDInsight kullanıcıları ve grupları menüsü](./media/hdinsight-sync-aad-users-to-cluster/hdinsight-users-menu-item.png)

3. Yeni kullanıcı Kullanıcılar tablosunda listelenmelidir. Tür `LDAP` yerine `Local`ayarlanır.

    ![HDInsight aad kullanıcıları sayfasına genel bakış](./media/hdinsight-sync-aad-users-to-cluster/hdinsight-users-page.png)

## <a name="log-in-to-ambari-as-the-new-user"></a>Ambari'ye yeni kullanıcı olarak giriş yapın

Yeni kullanıcı (veya başka bir etki alanı kullanıcısı) Ambari'de oturum açtıklarında, tam Azure AD kullanıcı adını ve etki alanı kimlik bilgilerini kullanır.  Ambari, Azure AD'de kullanıcının görüntü adı olan bir kullanıcı takma adını görüntüler.
Yeni örnek kullanıcı kullanıcı `hiveuser3@contoso.com`adı vardır. Ambari'de, bu yeni `hiveuser3` kullanıcı olarak ortaya çıkar, `hiveuser3@contoso.com`ancak kullanıcı Ambari'ye .

## <a name="see-also"></a>Ayrıca bkz.

* [ESP ile HDInsight'ta Apache Hive ilkelerini yapılandırın](hdinsight-domain-joined-run-hive.md)
* [ESP ile HDInsight kümelerini yönetme](hdinsight-domain-joined-manage.md)
* [Kullanıcıları Apache Ambari'ye yetkilendirmek](hdinsight-authorize-users-to-ambari.md)
