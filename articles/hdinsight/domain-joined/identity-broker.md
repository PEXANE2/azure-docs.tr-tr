---
title: Kimlik bilgisi yönetimi için KIMLIK Aracısı'nı (önizleme) kullanın- Azure HDInsight
description: Etki alanına katılan Apache Hadoop kümeleri için kimlik doğrulamayı kolaylaştırmak için HDInsight ID Broker'ı hakkında bilgi edinin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 12/12/2019
ms.openlocfilehash: fb82cec6874f8ef4f41897cc22939fe69ed02ec2
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457425"
---
# <a name="use-id-broker-preview-for-credential-management"></a>Kimlik bilgisi yönetimi için Kimlik Aracısı 'nı (önizleme) kullanma

Bu makalede, Azure HDInsight'ta ID Aracısı özelliğinin nasıl ayarlanıp kullanılacağı açıklanmaktadır. Bu özelliği, Azure Çok Faktörlü Kimlik Doğrulama aracılığıyla Apache Ambari'de oturum açarak ve Azure Active Directory Domain Services 'de (Azure AD DS) parola kalıplarına gerek kalmadan gerekli Kerberos biletlerini almak için kullanabilirsiniz.

## <a name="overview"></a>Genel Bakış

ID Aracıcısı, aşağıdaki senaryolarda karmaşık kimlik doğrulama kurulumlarını basitleştirir:

* Kuruluşunuz, bulut kaynaklarına erişmek için kullanıcıların kimliğini doğrulamak için federasyona güvenir. Daha önce, HDInsight Kurumsal Güvenlik Paketi (ESP) kümelerini kullanmak için, şirket içi ortamınızdan Azure Active Directory'ye parola karma eşitlemesini etkinleştirmeniz gerekiyordu. Bu gereksinim, bazı kuruluşlar için zor veya istenmeyen olabilir.

* Farklı kimlik doğrulama mekanizmalarına dayanan teknolojiler kullanan çözümler oluşturuyorsunuz. Örneğin, Apache Hadoop ve Apache Ranger Kerberos'a güvenirken, Azure Veri Gölü Depolaması OAuth'a dayanır.

Kimlik Aracıcısı birleşik bir kimlik doğrulama altyapısı sağlar ve parola çözümlerini Azure AD DS ile eşitleme gereksinimini kaldırır. ID Broker, windows server VM (ID Broker düğümü) üzerinde çalışan bileşenlerin yanı sıra küme ağ geçidi düğümlerinden oluşur. 

Aşağıdaki diyagram, ID Broker etkinleştirildikten sonra federe kullanıcılar da dahil olmak üzere tüm kullanıcılar için kimlik doğrulama akışını gösterir:

![ID Broker ile kimlik doğrulama akışı](./media/identity-broker/identity-broker-architecture.png)

ID Broker, herhangi bir parola sağlamadan Çok Faktörlü Kimlik Doğrulama'yı kullanarak ESP kümelerinde oturum açmanızı sağlar. Azure portalı gibi diğer Azure hizmetlerinde oturum açtıysanız, tek bir oturum açma (SSO) deneyimiyle HDInsight kümenizde oturum açabilirsiniz.

## <a name="enable-hdinsight-id-broker"></a>HDInsight ID Aracısını Etkinleştir

ID Broker etkinleştirilmiş bir ESP kümesi oluşturmak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. BIR ESP kümesi için temel oluşturma adımlarını izleyin. Daha fazla bilgi için [esp ile hdinsight kümesi oluşturma'ya](apache-domain-joined-configure-using-azure-adds.md#create-an-hdinsight-cluster-with-esp)bakın.
1. **HDInsight ID Aracısını Etkinleştir'i**seçin.

ID Broker özelliği kümeye bir ekstra VM ekler. Bu VM, Kimlik Aracısı düğümüdür ve kimlik doğrulamasını desteklemek için sunucu bileşenleriiçerir. Kimlik Aracısı düğümü, Azure AD DS etki alanına birleştirilmiş etki alanıdır.

![Kimlik Aracısını etkinleştirme seçeneği](./media/identity-broker/identity-broker-enable.png)

## <a name="tool-integration"></a>Takım tümleştirmesi

HDInsight [IntelliJ eklentisi](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-intellij-tool-plugin#integrate-with-hdinsight-identity-broker-hib) OAuth'u destekleyecek şekilde güncellenir. Bu eklentiyi kümeye bağlanmak ve iş göndermek için kullanabilirsiniz.

## <a name="ssh-access-without-a-password-hash-in-azure-ad-ds"></a>Azure AD DS'de parola karma olmadan SSH erişimi

ID Aracıcısı etkinleştirildikten sonra, etki alanı hesapları olan SSH senaryoları için Azure AD DS'de depolanan bir parola karmasına ihtiyacınız vardır. Etki alanına katılan bir VM'ye SSH `kinit` veya komutu çalıştırmak için bir parola sağlamanız gerekir. 

SSH kimlik doğrulaması, karmanın Azure AD DS'de kullanılabilir olmasını gerektirir. Yalnızca yönetim senaryoları için SSH'yi kullanmak istiyorsanız, yalnızca bulutlara özel bir hesap oluşturabilir ve bunu kümeye göre SSH'de kullanabilirsiniz. Diğer kullanıcılar, Parola karmasını Azure AD DS'de kullanılabilir hale vermeden Ambari veya HDInsight araçlarını (IntelliJ eklentisi gibi) kullanmaya devam edebilir.

## <a name="clients-using-oauth-to-connect-to-hdinsight-gateway-with-id-broker-setup"></a>ID Broker kurulumu ile HDInsight ağ geçidine bağlanmak için OAuth kullanan istemciler

Kimlik aracısı kurulumunda, ağ geçidine bağlanan özel uygulamalar ve istemciler, önce gerekli OAuth belirteci'ni edinmek için güncelleştirilebilir. Belirteci aşağıdaki bilgilerle elde etmek için bu [belgedeki](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-app) adımları izleyebilirsiniz:

*   OAuth kaynak uri:https://hib.azurehdinsight.net 
* AppId: 7865c1d2-f040-46cc-875f-831a1ef6a28a
*   İzin: (isim: Cluster.ReadWrite, id:8f89faa0-ffef-4007-974d-4989b39ad77d)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory Etki Alanı Hizmetlerini kullanarak Kurumsal Güvenlik Paketi ile HDInsight kümesini yapılandırma](apache-domain-joined-configure-using-azure-adds.md)
* [Azure Active Directory kullanıcılarını HDInsight kümesine eşitleme](../hdinsight-sync-aad-users-to-cluster.md)
* [Küme performansını izleme](../hdinsight-key-scenarios-to-monitor.md)
