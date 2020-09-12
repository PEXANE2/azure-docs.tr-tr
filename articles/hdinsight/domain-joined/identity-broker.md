---
title: Kimlik bilgisi yönetimi için KIMLIK Aracısı (Önizleme) kullanma-Azure HDInsight
description: Etki alanına katılmış Apache Hadoop kümelerinde kimlik doğrulamasını basitleştirmek için HDInsight KIMLIK Aracısı hakkında bilgi edinin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 12/12/2019
ms.openlocfilehash: 3b2807ccd6d83511dd0c9a32a177ea9fe2c4b642
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89662096"
---
# <a name="use-id-broker-preview-for-credential-management"></a>Kimlik bilgisi yönetimi için KIMLIK Aracısı (Önizleme) kullan

Bu makalede, Azure HDInsight 'ta KIMLIK Aracısı özelliğinin nasıl ayarlanacağı ve kullanılacağı açıklanır. Bu özelliği, Azure Multi-Factor Authentication aracılığıyla Apache ambarı 'nda oturum açmak ve Azure Active Directory Domain Services (Azure AD DS) parola karmaları gerekmeden gerekli Kerberos biletlerini almak için kullanabilirsiniz.

## <a name="overview"></a>Genel Bakış

KIMLIK Aracısı, aşağıdaki senaryolarda karmaşık kimlik doğrulama kurulumlarını basitleştirir:

* Kuruluşunuz, bulut kaynaklarına erişmek için kullanıcıların kimliğini doğrulamak üzere federasyon kullanır. Daha önce, HDInsight Kurumsal Güvenlik Paketi (ESP) kümelerini kullanmak için şirket içi ortamınızdan Parola karması eşitlemesini Azure Active Directory olarak etkinleştirmeniz gerekiyordu. Bu gereksinim bazı kuruluşlar için zor veya istenmeyen bir işlem olabilir.

* Farklı kimlik doğrulama mekanizmalarına dayanan teknolojiler kullanan çözümler oluşturacağız. Örneğin, Apache Hadoop ve Apache Ranger Kerberos kullanır, ancak Azure Data Lake Storage OAuth kullanır.

KIMLIK Aracısı Birleşik bir kimlik doğrulama altyapısı sağlar ve parola karmalarını Azure AD DS eşitleme gereksinimini ortadan kaldırır. KIMLIK Aracısı, küme ağ geçidi düğümleri ile birlikte bir Windows Server VM (KIMLIK Aracısı düğümü) üzerinde çalışan bileşenlerden oluşur. 

Aşağıdaki diyagramda, KIMLIK Aracısı etkinleştirildikten sonra Federasyon kullanıcıları dahil tüm kullanıcılar için kimlik doğrulama akışı gösterilmektedir:

![Kimlik broker ile kimlik doğrulama akışı](./media/identity-broker/identity-broker-architecture.png)

KIMLIK Aracısı, parola sağlamadan Multi-Factor Authentication kullanarak ESP kümelerinde oturum açmanıza olanak tanır. Azure portal gibi diğer Azure hizmetlerinde zaten oturum açtıysanız, HDInsight kümenizde çoklu oturum açma (SSO) deneyimiyle oturum açabilirsiniz.

## <a name="enable-hdinsight-id-broker"></a>HDInsight KIMLIK Aracısı 'nı etkinleştir

KIMLIK Aracısı etkin bir ESP kümesi oluşturmak için aşağıdaki adımları uygulayın:

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Bir ESP kümesi için temel oluşturma adımlarını izleyin. Daha fazla bilgi için bkz. [ESP Ile HDInsight kümesi oluşturma](apache-domain-joined-configure-using-azure-adds.md#create-an-hdinsight-cluster-with-esp).
1. **HDıNSIGHT kimlik Broker 'ı etkinleştir**' i seçin.

KIMLIK Aracısı özelliği kümeye bir ek VM ekler. Bu VM kimlik Aracısı düğümüdür ve kimlik doğrulamasını desteklemek için sunucu bileşenleri içerir. KIMLIK Aracısı düğümü, Azure AD DS etki alanına katılmış etki alanıdır.

![KIMLIK Broker 'ı etkinleştirme seçeneği](./media/identity-broker/identity-broker-enable.png)

### <a name="using-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanma
`idbrokernode`Şablonunuzun işlem profiline aşağıdaki özniteliklere sahip adlı yeni bir rol eklerseniz, küme, kimlik Aracısı düğümü etkinleştirilmiş olarak oluşturulur:

```json
.
.
.
"computeProfile": {
    "roles": [
        {
            "autoscale": null,
            "name": "headnode",
           ....
        },
        {
            "autoscale": null,
            "name": "workernode",
            ....
        },
        {
            "autoscale": null,
            "name": "idbrokernode",
            "targetInstanceCount": 1,
            "hardwareProfile": {
                "vmSize": "Standard_A2_V2"
            },
            "virtualNetworkProfile": {
                "id": "string",
                "subnet": "string"
            },
            "scriptActions": [],
            "dataDisksGroups": null
        }
    ]
}
.
.
.
```

## <a name="tool-integration"></a>Araç tümleştirmesi

HDInsight [IntelliJ eklentisi](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-intellij-tool-plugin#integrate-with-hdinsight-identity-broker-hib) , OAuth 'ı destekleyecek şekilde güncelleştirilir. Kümeye bağlanmak ve işleri göndermek için bu eklentiyi kullanabilirsiniz.

Not defterinden yararlanmak ve işleri göndermek için [vs Code Için Spark & Hive araçlarını](https://docs.microsoft.com/azure/hdinsight/hdinsight-for-vscode) da kullanabilirsiniz.

## <a name="ssh-access-without-a-password-hash-in-azure-ad-ds"></a>Azure AD DS Parola karması olmadan SSH erişimi

KIMLIK Aracısı etkinleştirildikten sonra, etki alanı hesaplarıyla SSH senaryolarında Azure AD DS depolanan bir parola karması gerekecektir. Etki alanına katılmış bir VM 'ye SSH eklemek veya komutu çalıştırmak için `kinit` bir parola sağlamanız gerekir. 

SSH kimlik doğrulaması, karmasının Azure AD DS kullanılabilir olmasını gerektirir. SSH 'yi yalnızca yönetim senaryoları için kullanmak istiyorsanız yalnızca bir bulut hesabı oluşturabilir ve bunu kümeye SSH için kullanabilirsiniz. Diğer kullanıcılar, Azure AD DS 'de Parola karması olmadan ambarı veya HDInsight araçlarını (IntelliJ eklentisi gibi) kullanmaya devam edebilir.

Kimlik doğrulama sorunlarını gidermek için lütfen bu [kılavuza](https://docs.microsoft.com/azure/hdinsight/domain-joined/domain-joined-authentication-issues)bakın.

## <a name="clients-using-oauth-to-connect-to-hdinsight-gateway-with-id-broker-setup"></a>KIMLIK Aracısı kurulumu ile HDInsight ağ geçidine bağlanmak için OAuth kullanan istemciler

KIMLIK Aracısı kurulumunda, ağ geçidine bağlanan özel uygulamalar ve istemciler, önce gerekli OAuth belirtecini almak üzere güncelleştirilebilen olabilir. Bu [belgedeki](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-app) adımları izleyerek belirteci aşağıdaki bilgilerle elde edebilirsiniz:

*   OAuth Kaynak URI 'si: `https://hib.azurehdinsight.net` 
* AppID: 7865c1d2-F040-46cc-875f-831a1ef6a28a
*   İzin: (ad: Cluster. ReadWrite, ID: 8f89faa0-ffef-4007-974d-4989b39ad77d)

OAuth belirtecini gönderdikten sonra, küme ağ geçidine (ör.-int.azurehdinsight.net) HTTP isteğinin yetkilendirme üstbilgisinde bunu kullanabilirsiniz <clustername> . Örneğin, Livy API 'ye örnek bir kıvrımlı komut aşağıdaki gibi görünebilir:
    
```bash
curl -k -v -H "Authorization: Bearer Access_TOKEN" -H "Content-Type: application/json" -X POST -d '{ "file":"wasbs://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://<clustername>-int.azurehdinsight.net/livy/batches" -H "X-Requested-By:<username@domain.com>"
``` 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory Domain Services kullanarak Kurumsal Güvenlik Paketi bir HDInsight kümesi yapılandırma](apache-domain-joined-configure-using-azure-adds.md)
* [Azure Active Directory kullanıcılarını HDInsight kümesine eşitleme](../hdinsight-sync-aad-users-to-cluster.md)
* [Küme performansını izleme](../hdinsight-key-scenarios-to-monitor.md)
