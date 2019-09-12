---
title: Azure Active Directory kullanıcılarını bir kümeyle eşitler-Azure HDInsight
description: Kimliği doğrulanmış kullanıcıları Azure Active Directory bir HDInsight kümesine eşitler.
ms.service: hdinsight
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: f58c847f512f2db72fdca823637192c3b638b1ae
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70879360"
---
# <a name="synchronize-azure-active-directory-users-to-an-hdinsight-cluster"></a>Azure Active Directory kullanıcılarını HDInsight kümesine eşitleme

[Kurumsal güvenlik paketi (ESP) Içeren HDInsight kümeleri](hdinsight-domain-joined-introduction.md) , Azure Active Directory (Azure AD) kullanıcılarıyla güçlü kimlik doğrulaması ve *rol tabanlı erişim denetimi* (RBAC) ilkeleri kullanabilir. Azure AD 'ye kullanıcılar ve gruplar eklerken, kümenize erişmesi gereken kullanıcıları da eşzamanlı hale getirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Daha önce yapmadıysanız, [Kurumsal güvenlik paketi bir HDInsight kümesi oluşturun](hdinsight-domain-joined-configure.md).

## <a name="add-new-azure-ad-users"></a>Yeni Azure AD kullanıcıları ekleme

Konaklarınızı görüntülemek için, ambarı Web Kullanıcı arabirimini açın. Her düğüm, yeni katılımsız yükseltme ayarlarıyla güncelleştirilir.

1. [Azure Portal](https://portal.azure.com), ESP kümeniz Ile ILIŞKILI Azure AD dizinine gidin.

2. Sol taraftaki menüden **tüm kullanıcılar** ' ı seçin ve ardından **Yeni Kullanıcı**' yı seçin.

    ![Tüm kullanıcılar bölmesi](./media/hdinsight-sync-aad-users-to-cluster/aad-users.png)

3. Yeni Kullanıcı formunu doldurun. Küme tabanlı izinleri atamak için oluşturduğunuz grupları seçin. Bu örnekte, Yeni Kullanıcı atayabilmeniz için "HiveUsers" adlı bir grup oluşturun. Bir ESP kümesi oluşturmak için [örnek yönergeler](hdinsight-domain-joined-configure.md) , ve `HiveUsers` `AAD DC Administrators`olmak üzere iki grup ekleme içerir.

    ![Yeni Kullanıcı bölmesi](./media/hdinsight-sync-aad-users-to-cluster/aad-new-user.png)

4. **Oluştur**’u seçin.

## <a name="use-the-apache-ambari-rest-api-to-synchronize-users"></a>Kullanıcıları eşleştirmek için Apache ambarı REST API kullanma

Küme oluşturma işlemi sırasında belirtilen kullanıcı grupları o anda eşitlenir. Kullanıcı eşitleme her saatte otomatik olarak gerçekleşir. Kullanıcıları hemen eşitlemesini veya küme oluşturma sırasında belirtilen gruplardan farklı bir grubu eşitlemesini sağlamak için, ambarı REST API kullanın.

Aşağıdaki yöntem REST API ambarı ile GÖNDERI kullanır. Daha fazla bilgi için bkz. [Apache ambarı REST API kullanarak HDInsight kümelerini yönetme](hdinsight-hadoop-manage-ambari-rest-api.md).

1. [SSH ile kümenize bağlanın](hdinsight-hadoop-linux-use-ssh-unix.md). Azure portal kümenizin genel bakış bölmesinden **Secure Shell (SSH)** düğmesini seçin.

    ![Secure Shell (SSH)](./media/hdinsight-sync-aad-users-to-cluster/hdinsight-secure-shell.png)

2. Görüntülenmiş `ssh` komutu kopyalayın ve SSH istemcisine yapıştırın. İstendiğinde SSH kullanıcı parolasını girin.

3. Kimlik doğrulamasından sonra aşağıdaki komutu girin:

    ```bash
    curl -u admin:<YOUR PASSWORD> -sS -H "X-Requested-By: ambari" \
    -X POST -d '{"Event": {"specs": [{"principal_type": "groups", "sync_type": "existing"}]}}' \
    "https://<YOUR CLUSTER NAME>.azurehdinsight.net/api/v1/ldap_sync_events"
    ```
    
    Yanıt şuna benzemelidir:

    ```json
    {
      "resources" : [
        {
          "href" : "http://hn0-hadoop.<YOUR DOMAIN>.com:8080/api/v1/ldap_sync_events/1",
          "Event" : {
            "id" : 1
          }
        }
      ]
    }
    ```

4. Eşitleme durumunu görmek için yeni `curl` bir komut yürütün:

    ```bash
    curl -u admin:<YOUR PASSWORD> https://<YOUR CLUSTER NAME>.azurehdinsight.net/api/v1/ldap_sync_events/1
    ```
    
    Yanıt şuna benzemelidir:
    
    ```json
    {
      "href" : "http://hn0-hadoop.YOURDOMAIN.com:8080/api/v1/ldap_sync_events/1",
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

5. Bu sonuç, durumun **tamamlandığını**, yeni bir kullanıcının oluşturulduğunu ve kullanıcıya bir üyelik atandığını gösterir. Bu örnekte, Kullanıcı Azure AD 'de aynı gruba eklendiğinden, Kullanıcı "HiveUsers" ile eşitlenen LDAP grubuna atanır.

> [!NOTE]  
> Önceki yöntem, yalnızca küme oluşturma sırasında etki alanı ayarlarının **Erişim Kullanıcı grubu** özelliğinde BELIRTILEN Azure AD gruplarını eşitler. Daha fazla bilgi için bkz. [HDInsight kümesi oluşturma](domain-joined/apache-domain-joined-configure.md).

## <a name="verify-the-newly-added-azure-ad-user"></a>Yeni eklenen Azure AD kullanıcısını doğrulama

Yeni Azure AD kullanıcısının eklendiğini doğrulamak için [Apache ambarı Web Kullanıcı arabirimini](hdinsight-hadoop-manage-ambari.md) açın. Ambari Web kullanıcı arabirimini göz atarak erişim **`https://<YOUR CLUSTER NAME>.azurehdinsight.net`** . Küme Yöneticisi Kullanıcı adı ve parolasını girin.

1. Ambarı panosundan **yönetici** menüsünde **ambarı Yönet** ' i seçin.

    ![Ambarı yönetme](./media/hdinsight-sync-aad-users-to-cluster/manage-ambari.png)

2. Sayfanın sol tarafındaki **Kullanıcı + Grup Yönetimi** menü grubu altında **Kullanıcılar** ' ı seçin.

    ![Kullanıcılar menü öğesi](./media/hdinsight-sync-aad-users-to-cluster/users-link.png)

3. Yeni kullanıcının kullanıcı tablosu içinde listelenmesi gerekir. Türü yerine olarak `LDAP` `Local`ayarlanır.

    ![Kullanıcılar sayfası](./media/hdinsight-sync-aad-users-to-cluster/hdinsight-users-page.png)

## <a name="log-in-to-ambari-as-the-new-user"></a>Yeni Kullanıcı olarak ambarı 'nda oturum açın

Yeni Kullanıcı (veya başka bir etki alanı kullanıcısı), ambarı 'nda oturum açtığında, tam Azure AD Kullanıcı adı ve etki alanı kimlik bilgilerini kullanır.  Ambarı, Azure AD 'de kullanıcının görünen adı olan bir kullanıcı diğer adı görüntüler. Yeni örnek Kullanıcı Kullanıcı adına `hiveuser3@contoso.com`sahiptir. Bu yeni Kullanıcı, ambarı 'nda farklı şekilde gösterilir `hiveuser3` , ancak Kullanıcı ambarı 'nda olarak `hiveuser3@contoso.com`oturum açar.

## <a name="see-also"></a>Ayrıca bkz.

* [HDInsight 'ta ESP ile Apache Hive ilkelerini yapılandırma](hdinsight-domain-joined-run-hive.md)
* [HDInsight kümelerini ESP ile yönetme](hdinsight-domain-joined-manage.md)
* [Kullanıcıları Apache ambarı 'na yetkilendir](hdinsight-authorize-users-to-ambari.md)
