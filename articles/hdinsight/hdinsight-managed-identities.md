---
title: Azure HDInsight'ta yönetilen kimlikler
description: Azure HDInsight'ta yönetilen kimliklerin uygulanmasına genel bir bakış sağlar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/20/2019
ms.openlocfilehash: daae9c16797ad9c1b85635f5aec7d0cf884e003f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78206019"
---
# <a name="managed-identities-in-azure-hdinsight"></a>Azure HDInsight'ta yönetilen kimlikler

Yönetilen kimlik, kimlik bilgileri Azure Tarafından yönetilen Azure Etkin Dizini'nde (Azure AD) kayıtlı bir kimliktir. Yönetilen kimliklerle, Hizmet ilkelerini Azure AD'ye kaydetmeniz veya sertifikalar gibi kimlik bilgilerini korumanız gerekmez.

Azure AD etki alanı hizmetlerine erişmek veya gerektiğinde Azure Veri Gölü Depolama Gen2'deki dosyalara erişmek için Azure HDInsight'ta yönetilen kimlikler kullanılır.

Yönetilen kimliklerin iki türü vardır: kullanıcı tarafından atanan ve sistem atanan. Azure HDInsight yalnızca kullanıcı tarafından atanan yönetilen kimlikleri destekler. HDInsight, sistem tarafından atanan yönetilen kimlikleri desteklemez. Kullanıcı tarafından atanan yönetilen kimlik, bağımsız bir Azure kaynağı olarak oluşturulur ve bu bilgiler bir veya daha fazla Azure hizmet örneğine atayabilirsiniz. Bunun aksine, Azure AD'de sistem tarafından atanmış yönetilen bir kimlik oluşturulur ve ardından doğrudan belirli bir Azure hizmet örneğinde otomatik olarak etkinleştirilir. Sistem tarafından atanan yönetilen kimliğin ömrü, etkinleştirildiği hizmet örneğinin ömrüne bağlıdır.

## <a name="hdinsight-managed-identity-implementation"></a>HDInsight yönetilen kimlik uygulaması

Azure HDInsight'ta yönetilen kimlikler kümenin her düğümünde sağlanır. Ancak bu kimlik bileşenleri yalnızca HDInsight hizmeti tarafından kullanılabilir. Şu anda HDInsight küme düğümlerine yüklenen yönetilen kimlikleri kullanarak erişim belirteçleri oluşturmanız için desteklenen bir yöntem yok. Bazı Azure hizmetlerinde, yönetilen kimlikler, diğer Azure hizmetleriyle tek başına etkileşim de dahil olmak üzere erişim belirteçleri elde etmek için kullanabileceğiniz bir bitiş noktasıyla uygulanır.

## <a name="create-a-managed-identity"></a>Yönetilen bir kimlik oluşturma

Yönetilen kimlikler aşağıdaki yöntemlerden herhangi biriyle oluşturulabilir:

* [Azure portalında](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)
* [Azure PowerShell](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md)
* [Azure Resource Manager](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md)
* [Azure CLI](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)

Yönetilen kimliği yapılandırmak için kalan adımlar, kullanılacağı senaryoya bağlıdır.

## <a name="managed-identity-scenarios-in-azure-hdinsight"></a>Azure HDInsight'ta yönetilen kimlik senaryoları

Yönetilen kimlikler Azure HDInsight'ta birden çok senaryoda kullanılır. Ayrıntılı kurulum ve yapılandırma yönergeleri için ilgili belgelere bakın:

* [Azure Data Lake Storage 2. Nesil](hdinsight-hadoop-use-data-lake-storage-gen2.md#create-a-user-assigned-managed-identity)
* [Kurumsal Güvenlik Paketi](domain-joined/apache-domain-joined-configure-using-azure-adds.md#create-and-authorize-a-managed-identity)
* [Müşteri tarafından yönetilen anahtar disk şifrelemesi](disk-encryption.md)

## <a name="faq"></a>SSS
### <a name="what-happens-if-i-delete-the-managed-identity-after-the-cluster-creation"></a>Küme oluşturulduktan sonra yönetilen kimliği silersem ne olur?
Kümeniz, yönetilen kimlik gerektiğinde sorunlarla karşınıza çıkacaktır. Küme oluşturulduktan sonra yönet kimliğini güncelleştirmenin veya değiştirmenin şu anda yolu yoktur. Bu nedenle, küme çalışma zamanında yönetilen kimliğin silinmediğinden emin olmak için önerimiz. Alternatif olarak kümeyi yeniden oluşturabilir ve yeni yönetilen bir kimlik atayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure kaynakları için yönetilen kimlikler nedir?](../active-directory/managed-identities-azure-resources/overview.md)
