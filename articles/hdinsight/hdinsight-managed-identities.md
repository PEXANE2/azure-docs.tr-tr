---
title: Azure HDInsight 'ta Yönetilen kimlikler
description: Azure HDInsight 'ta yönetilen kimliklerin uygulanmasına genel bir bakış sağlar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/20/2019
ms.openlocfilehash: 934c99c25ca37526ac31fd9bbaf58623fdbdf166
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76764393"
---
# <a name="managed-identities-in-azure-hdinsight"></a>Azure HDInsight 'ta Yönetilen kimlikler

Yönetilen kimlik, kimlik bilgileri Azure tarafından yönetilen Azure Active Directory (Azure AD) ' de kayıtlı bir kimliktir. Yönetilen kimliklerle, hizmet sorumlularını Azure AD 'ye kaydetmeniz veya sertifikalar gibi kimlik bilgilerini korumanız gerekmez.

Yönetilen kimlikler Azure AD etki alanı hizmetlerine erişmek veya gerektiğinde Azure Data Lake Storage 2. dosyalara erişmek için Azure HDInsight 'ta kullanılır.

İki tür yönetilen kimlik vardır: Kullanıcı tarafından atanan ve sistem tarafından atanan. Azure HDInsight Kullanıcı tarafından atanan yönetilen kimlikleri kullanır. Kullanıcı tarafından atanan yönetilen kimlik, tek başına bir Azure kaynağı olarak oluşturulur. Bu, daha sonra bir veya daha fazla Azure hizmet örneğine atayabilirsiniz. Buna karşılık, Azure AD 'de sistem tarafından atanan yönetilen bir kimlik oluşturulur ve ardından doğrudan belirli bir Azure Hizmeti örneğinde otomatik olarak etkinleştirilir. Bu durumda, sistem tarafından atanan yönetilen kimliğin yaşam süresi daha sonra etkinleştirilmiş olan hizmet örneği ömrü ile bağlantılıdır.

## <a name="hdinsight-managed-identity-implementation"></a>HDInsight Yönetilen kimlik uygulama

Azure HDInsight 'ta, Yönetilen kimlikler kümenin her bir düğümünde sağlanır. Ancak, bu kimlik bileşenleri yalnızca HDInsight hizmeti tarafından kullanılabilir. Şu anda HDInsight küme düğümlerinde yüklü yönetilen kimlikleri kullanarak erişim belirteçleri oluşturmanız için desteklenen bir yöntem yoktur. Bazı Azure hizmetleri için Yönetilen kimlikler, kendi kendinize yönelik diğer Azure hizmetleriyle etkileşim kurmak için erişim belirteçleri elde etmek üzere kullanabileceğiniz bir uç nokta ile uygulanır.

## <a name="create-a-managed-identity"></a>Yönetilen kimlik oluşturma

Yönetilen kimlikler aşağıdaki yöntemlerden biriyle oluşturulabilir:

* [Azure Portal](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)
* [Azure PowerShell](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md)
* [Azure Resource Manager](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md)
* [Azure CLI](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)

Yönetilen kimliği yapılandırmaya yönelik kalan adımlar, kullanılacağı senaryoya bağlıdır.

## <a name="managed-identity-scenarios-in-azure-hdinsight"></a>Azure HDInsight 'ta yönetilen kimlik senaryoları

Yönetilen kimlikler, Azure HDInsight 'ta birden çok senaryoda kullanılır. Ayrıntılı kurulum ve yapılandırma yönergeleri için bkz. ilgili belgeler:

* [Azure Data Lake Storage 2. Nesil](hdinsight-hadoop-use-data-lake-storage-gen2.md#create-a-user-assigned-managed-identity)
* [Kurumsal Güvenlik Paketi](domain-joined/apache-domain-joined-configure-using-azure-adds.md#create-and-authorize-a-managed-identity)
* [Kafka Kendi Anahtarını Getir (BYOK)](kafka/apache-kafka-byok.md#get-started-with-byok)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure kaynakları için yönetilen kimlikler nedir?](../active-directory/managed-identities-azure-resources/overview.md)
