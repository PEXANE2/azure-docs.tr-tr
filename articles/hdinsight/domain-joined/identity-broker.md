---
title: Kimlik bilgisi yönetimi için KIMLIK Aracısı (Önizleme) kullanma-Azure HDInsight
description: Etki alanına katılmış Apache Hadoop kümelerinde kimlik doğrulamasını basitleştirmek için HDInsight KIMLIK Aracısı hakkında bilgi edinin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 09/23/2020
ms.openlocfilehash: 8f1e0a6aecc9702552a3dd66acc8dc7eb5bf1d85
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91529958"
---
# <a name="azure-hdinsight-id-broker-preview"></a>Azure HDInsight KIMLIK Aracısı (Önizleme)

Bu makalede, Azure HDInsight 'ta HDInsight KIMLIK broker (HIB) özelliğinin nasıl ayarlanacağı ve kullanılacağı açıklanır. Bu özelliği kullanarak Apache ambarı 'na modern OAuth kimlik doğrulaması, Azure Active Directory Domain Services (AAD-DS) içinde eski parola karmalarına gerek kalmadan Multi-Factor Authentication (MFA) zorlaması yapabilirsiniz.

## <a name="overview"></a>Genel Bakış

HIB, aşağıdaki senaryolarda karmaşık kimlik doğrulama kurulumlarını basitleştirir:

* Kuruluşunuz, bulut kaynaklarına erişmek için kullanıcıların kimliğini doğrulamak üzere federasyon kullanır. Daha önce, HDInsight Kurumsal Güvenlik Paketi (ESP) kümelerini kullanmak için şirket içi ortamınızdan Parola karması eşitlemesini Azure Active Directory (Azure AD) olarak etkinleştirmeniz gerekiyordu. Bu gereksinim bazı kuruluşlar için zor veya istenmeyen bir işlem olabilir.

* Kuruluşunuz, Apache ambarı ve diğer küme kaynaklarına Web/HTTP tabanlı erişim için MFA 'yı zorlamak istiyor.

HIB, parola karmalarının AAD-DS ile eşitlenmesi gerekmeden OAuth (modern) ile Kerberos (eski) arasında protokol geçişi sağlayan kimlik doğrulama altyapısını sağlar. Bu altyapı, küme ağ geçidi düğümleri ile birlikte bir Windows Server VM (KIMLIK Aracısı düğümü) üzerinde çalışan bileşenlerden oluşur.

Aşağıdaki diyagramda, KIMLIK Aracısı etkinleştirildikten sonra Federasyon kullanıcıları dahil tüm kullanıcılar için modern OAuth tabanlı kimlik doğrulama akışı gösterilmektedir:

:::image type="content" source="media/identity-broker/identity-broker-architecture.png" alt-text="Kimlik broker ile kimlik doğrulama akışı":::

Bu diyagramda, istemcinin (tarayıcı veya uygulamalar) önce OAuth belirtecini edinmesi ve sonra belirteci bir HTTP isteğindeki ağ geçidine sunması gerekir. Azure portal gibi diğer Azure hizmetlerinde zaten oturum açtıysanız, HDInsight kümenizde çoklu oturum açma (SSO) deneyimiyle oturum açabilirsiniz.

Yalnızca temel kimlik doğrulamasını (yani, Kullanıcı adı/parola) destekleyen birçok eski uygulama de olabilir. Bu senaryolarda, küme ağ geçitlerine bağlanmak için HTTP temel kimlik doğrulamasını kullanmaya devam edebilirsiniz. Bu kurulumda, ağ geçidi düğümlerinden doğrudan bir görüş satırı olduğundan emin olmak için ağ geçidi düğümlerinden Federasyon uç noktasına (ADFS uç noktası) ağ bağlantısı sağlanmalıdır.

:::image type="content" source="media/identity-broker/basic-authentication.png" alt-text="Kimlik broker ile kimlik doğrulama akışı":::

Kuruluşunuzun ihtiyaçlarına bağlı olarak en iyi kimlik doğrulama seçeneğini öğrenmek için aşağıdaki tabloyu kullanın:

|Kimlik doğrulaması seçenekleri |HDInsight yapılandırması | Göz önünde bulundurulması gereken etkenler |
|---|---|---|
| Tam OAuth | ESP + HIB | 1. en güvenli seçenek (MFA desteklenir) 2.    Geçiş karması eşitleme gerekli DEĞILDIR. 3.  AAD-DS 'de Parola karması olmayan şirket içi hesaplar için SSH/kinıt/keytab erişimi yok. 4.   Yalnızca bulut hesapları hala SSH/INIT/keytab olabilir. 5. OAuth 6 aracılığıyla ambarı 'na Web tabanlı erişim.  , OAuth 'ı desteklemek için eski uygulamaların (JDBC/ODBC, vb.) güncelleştirilmesini gerektirir.|
| OAuth + temel kimlik doğrulaması | ESP + HIB | 1. OAuth 2 aracılığıyla ambarı 'na Web tabanlı erişim. Eski uygulamalar temel kimlik doğrulama. 3 ' ü kullanmaya devam eder. Temel kimlik doğrulama erişimi için MFA devre dışı bırakılmalıdır. 4. Geçiş karması eşitleme gerekli DEĞILDIR. 5. AAD-DS 'de Parola karması olmayan şirket içi hesaplar için SSH/kinıt/keytab erişimi yok. 6. Yalnızca bulut hesapları yine SSH/kinit olabilir. |
| Tam temel kimlik doğrulama | ESP | 1. en çok şirket içi kurulumların çoğuna benzer. 2. AAD-DS için Parola karması eşitlemesi gereklidir. 3. Şirket içi hesaplar SSH/kinıt yapabilir veya keytab kullanabilir. 4. Yedekleme depolaması ADLS 2., MFA devre dışı bırakılmalıdır |


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

HDIsngith araçları, OAuth 'ı yerel olarak destekleyecek şekilde güncelleştirildi. Kümelere modern OAuth tabanlı erişim için bu araçları kullanmanızı kesinlikle öneririz. HDInsight [IntelliJ eklentisi](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-intellij-tool-plugin#integrate-with-hdinsight-identity-broker-hib) , Scala gibi Java tabanlı uygulamalar için kullanılabilir. [Vs Code Için Spark & Hive araçları](https://docs.microsoft.com/azure/hdinsight/hdinsight-for-vscode) , pyspark ve Hive işlerinin kullanılabilir. Bunlar hem Batch hem de etkileşimli işleri destekler.

## <a name="ssh-access-without-a-password-hash-in-azure-ad-ds"></a>Azure AD DS Parola karması olmadan SSH erişimi

|SSH seçenekleri |Göz önünde bulundurulması gereken etkenler |
|---|---|
| Yerel VM hesabı (örneğin, sshuser) | 1. bu hesabı küme oluşturma zamanında sağladınız. 2.  Bu hesap için Kerberos kimlik doğrulama yok |
| Yalnızca bulut hesabı (ör. alice@contoso.onmicrosoft.com ) | 1. Parola karması AAD-DS 2 ' de kullanılabilir. Kerberos kimlik doğrulaması SSH Kerberos aracılığıyla yapılabilir |
| Şirket içi hesap (ör. alice@contoso.com ) | 1. SSH Kerberos kimlik doğrulaması yalnızca AAD-DS ' d e Parola karması kullanılabiliyorsa mümkün değilse, bu kullanıcı kümeye SSH kullanamaz |

Etki alanına katılmış bir VM 'ye SSH eklemek veya komutu çalıştırmak için `kinit` bir parola sağlamanız gerekir. SSH Kerberos kimlik doğrulaması, karma 'in AAD-DS ' d e kullanılabilir olmasını gerektirir. SSH 'yi yalnızca yönetim senaryoları için kullanmak istiyorsanız yalnızca bir bulut hesabı oluşturabilir ve bunu kümeye SSH için kullanabilirsiniz. Diğer şirket içi kullanıcılar aynı zamanda AAD veya HDInsight araçları veya HTTP temel kimlik doğrulaması kullanarak AAD-DS ' d i n parola karması kullanmanıza gerek kalmadan yine de kullanılabilir.

Kuruluşunuz parola karmalarını AAD 'lere eşitlemiyor, en iyi uygulama olarak, Azure AD 'de yalnızca bir bulut kullanıcısı oluşturun ve kümeyi oluştururken Küme Yöneticisi olarak atayın ve VM 'ye SSH aracılığıyla kök erişimi almayı içeren yönetim amaçları için bunu kullanın.

Kimlik doğrulama sorunlarını gidermek için lütfen bu [kılavuza](https://docs.microsoft.com/azure/hdinsight/domain-joined/domain-joined-authentication-issues)bakın.

## <a name="clients-using-oauth-to-connect-to-hdinsight-gateway-with-hib"></a>HIB ile HDInsight ağ geçidine bağlanmak için OAuth kullanan istemciler

HIB kurulumunda, ağ geçidine bağlanan özel uygulamalar ve istemciler, önce gerekli OAuth belirtecini almak üzere güncelleştirilebilen olabilir. Bu [belgedeki](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-app) adımları izleyerek belirteci aşağıdaki bilgilerle elde edebilirsiniz:

*   OAuth Kaynak URI 'si: `https://hib.azurehdinsight.net` 
*   AppID: 7865c1d2-F040-46cc-875f-831a1ef6a28a
*   İzin: (ad: Cluster. ReadWrite, ID: 8f89faa0-ffef-4007-974d-4989b39ad77d)

OAuth belirtecini aldıktan sonra, bunu küme ağ geçidine (ör. https://-int.azurehdinsight.net) HTTP isteğinin yetkilendirme üstbilgisinde kullanabilirsiniz <clustername> . Örneğin, Apache Livy API 'sine örnek bir kıvrımlı komut şöyle görünebilir:
    
```bash
curl -k -v -H "Authorization: Bearer Access_TOKEN" -H "Content-Type: application/json" -X POST -d '{ "file":"wasbs://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://<clustername>-int.azurehdinsight.net/livy/batches" -H "X-Requested-By:<username@domain.com>"
``` 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory Domain Services kullanarak Kurumsal Güvenlik Paketi bir HDInsight kümesi yapılandırma](apache-domain-joined-configure-using-azure-adds.md)
* [Azure Active Directory kullanıcılarını HDInsight kümesine eşitleme](../hdinsight-sync-aad-users-to-cluster.md)
* [Küme performansını izleme](../hdinsight-key-scenarios-to-monitor.md)
