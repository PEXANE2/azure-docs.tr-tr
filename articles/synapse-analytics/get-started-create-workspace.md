---
title: 'Hızlı başlangıç: Başlarken-SYNAPSE çalışma alanı oluşturma'
description: Bu öğreticide, bir Synapse çalışma alanı, adanmış bir SQL havuzu ve sunucusuz Apache Spark havuzu oluşturmayı öğreneceksiniz.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: workspace
ms.topic: tutorial
ms.date: 10/07/2020
ms.openlocfilehash: d22d70a360290e7fbfabf1d05fc49d4ef6eb8c83
ms.sourcegitcommit: 9889a3983b88222c30275fd0cfe60807976fd65b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94986343"
---
# <a name="creating-a-synapse-workspace"></a>SYNAPSE çalışma alanı oluşturma

Bu öğreticide, bir Synapse çalışma alanı, adanmış bir SQL havuzu ve sunucusuz Apache Spark havuzu oluşturmayı öğreneceksiniz. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticinin adımlarını tamamlayabilmeniz için, **sahip** rolü atadığınız bir kaynak grubuna erişiminizin olması gerekir. Bu kaynak grubunda SYNAPSE çalışma alanını oluşturun.

## <a name="create-a-synapse-workspace-in-the-azure-portal"></a>Azure portal bir Synapse çalışma alanı oluşturun

1. [Azure Portal](https://portal.azure.com)açın ve en üstteki **SYNAPSE** için arama yapın.
1. Arama sonuçlarında **Hizmetler** altında **Azure SYNAPSE Analytics (çalışma alanları Önizleme)** öğesini seçin.
1. Çalışma alanı oluşturmak için **Ekle** ' yi seçin.
1. **Temel bilgiler** bölümünde tercih ettiğiniz **abonelik**, **kaynak grubu**, **bölge** girin ve ardından bir çalışma alanı adı seçin. Bu öğreticide, **MyWorkspace** kullanacağız.
1. **Data Lake Storage Gen 2**' yi seçmek için gidin. 
1. **Yeni oluştur** ' a tıklayın ve **contosolake** olarak adlandırın.
1. **Dosya sistemi** ' ne tıklayın ve **kullanıcıları** adlandırın. Bu, **Kullanıcılar** adlı bir kapsayıcı oluşturur
1. Çalışma alanı, bu depolama hesabını Spark tabloları ve Spark uygulama günlükleri için "birincil" depolama hesabı olarak kullanacaktır.
1. **Gözden geçir ve oluştur** > **Oluştur**'u seçin. Çalışma alanınız birkaç dakika içinde hazırlanıyor.

## <a name="open-synapse-studio"></a>SYNAPSE Studio 'Yu açın

Azure SYNAPSE çalışma alanınız oluşturulduktan sonra, SYNAPSE Studio 'Yu açmak için iki yol vardır:

* [Azure Portal](https://portal.azure.com)SYNAPSE çalışma alanınızı açın. **Genel bakış** bölümünün üst kısmında, **SYNAPSE Studio 'yu Başlat**' ı seçin.
* Adresine gidin `https://web.azuresynapse.net` ve çalışma alanınızda oturum açın.

## <a name="create-a-dedicated-sql-pool"></a>Adanmış bir SQL havuzu oluşturma

1. SYNAPSE Studio 'da, sol taraftaki bölmede **Manage**  >  **SQL havuzlarını** Yönet ' i seçin.
1. **Yeni** ' yi seçin
1. **SQL havuzu adı** için **SQLPOOL1** seçin
1. **Performans düzeyi** için **DW100C** seçin
1. **Gözden geçir ve oluştur** > **Oluştur**'u seçin. Adanmış SQL havuzunuz birkaç dakika içinde hazırlanacaktır. Adanmış SQL havuzunuz, **SQLPOOL1** olarak da bilinen ayrılmış bir SQL havuzu veritabanıyla ilişkilidir.

Adanmış bir SQL havuzu, etkin olduğu sürece faturalanabilir kaynakları kullanır. Daha sonra maliyetleri azaltmak için havuzu duraklatabilirsiniz.

## <a name="create-a-serverless-apache-spark-pool"></a>Sunucusuz Apache Spark havuzu oluşturma

1. SYNAPSE Studio 'da, sol taraftaki bölmede **Manage**  >  **Apache Spark havuzlarını** Yönet ' i seçin.
1. **Yeni** ' yi seçin 
1. **Apache Spark havuz adı** için **Spark1** girin.
1. **Düğüm boyutu** için **küçük** girin.
1. **Düğüm sayısı** için en az 3 ve en fazla 3 olarak ayarlayın
1. **Gözden geçir ve oluştur** > **Oluştur**'u seçin. Apache Spark havuzunuz birkaç saniye içinde hazırlanacaktır.

Spark havuzu, Azure SYNAPSE 'in kaç Spark kaynağı kullandığını söyler. Yalnızca kullandığınız kaynaklar için ödeme yaparsınız. Havuzu kullanmayı etkin bir şekilde durdurduğunuzda, kaynakların otomatik olarak zaman aşımına uğrar ve geri dönüştürülür.

## <a name="the-built-in-serverless-sql-pool"></a>Yerleşik sunucusuz SQL havuzu

Her çalışma alanı, **yerleşik** olarak adlandırılan önceden oluşturulmuş bir SUNUCUSUZ SQL havuzu ile gelir. Bu havuz silinemiyor. Sunucusuz SQL havuzları, adanmış SQL havuzlarıyla kapasiteyi ayırmak için SQL withou HAVING kullanmanıza olanak sağlar. Adanmış SQL havuzlarının aksine, sunucusuz SQL havuzu için faturalandırma, havuza ayrılan kapasite sayısını değil, sorguyu çalıştırmak için taranan veri miktarına bağlıdır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Adanmış bir SQL havuzu kullanarak çözümle](get-started-analyze-sql-pool.md)
