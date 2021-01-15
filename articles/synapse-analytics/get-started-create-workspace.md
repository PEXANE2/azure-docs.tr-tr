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
ms.date: 12/31/2020
ms.openlocfilehash: 3a2636ec73d20f3011d8413c794e68ef41b1829c
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98209194"
---
# <a name="creating-a-synapse-workspace"></a>SYNAPSE çalışma alanı oluşturma

Bu öğreticide, bir Synapse çalışma alanı, adanmış bir SQL havuzu ve sunucusuz Apache Spark havuzu oluşturmayı öğreneceksiniz. 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticinin adımlarını tamamlayabilmeniz için, **sahip** rolü atadığınız bir kaynak grubuna erişiminizin olması gerekir. Bu kaynak grubunda SYNAPSE çalışma alanını oluşturun.

## <a name="create-a-synapse-workspace-in-the-azure-portal"></a>Azure portal bir Synapse çalışma alanı oluşturun

1. [Azure Portal](https://portal.azure.com)açın ve en üstteki **SYNAPSE** için arama yapın.
1. Arama sonuçlarında **Hizmetler**' ın altında **Azure SYNAPSE Analytics**' i seçin.
1. Çalışma alanı oluşturmak için **Ekle** ' yi seçin.
1. **Temel bilgiler** sekmesinde, proje ayrıntıları ' nın altında, tercih ettiğiniz **abonelik**, **kaynak grubu**, **bölge** girin ve ardından bir çalışma alanı adı seçin. Bu öğreticide, **MyWorkspace** kullanacağız.
1. **Data Lake Storage Gen 2**' yi seçerek **aboneliğinden** için düğmesine tıklayın.
1. **Hesap adına** göre **Yeni oluştur** ' a tıklayın ve yeni depolama hesabını **contosogölü** veya buna benzer şekilde adlandırın.
1. **Dosya sistemi adına** göre, **Yeni oluştur** ' a tıklayın ve BT **kullanıcılarını** adlandırın. Bu, **Kullanıcılar** adlı bir depolama kapsayıcısı oluşturur
1. Çalışma alanı, bu depolama hesabını Spark tabloları ve Spark uygulama günlükleri için "birincil" depolama hesabı olarak kullanacaktır.
1. "Data Lake Storage 2. hesabında Depolama Blobu veri katılımcısı rolünü ata" kutusunu işaretleyin. 
1. **Gözden geçir ve oluştur** > **Oluştur**'u seçin. Çalışma alanınız birkaç dakika içinde hazırlanıyor.

> [!NOTE]
> Mevcut ayrılmış bir SQL havuzundan (eski adıyla SQL DW) çalışma alanı özelliklerini etkinleştirmek için, [ADANMıŞ SQL havuzunuz (eski ADıYLA SQL DW) için bir çalışma alanı etkinleştirme](./sql-data-warehouse/workspace-connected-create.md)bölümüne bakın.


## <a name="open-synapse-studio"></a>SYNAPSE Studio 'Yu açın

Azure SYNAPSE çalışma alanınız oluşturulduktan sonra, SYNAPSE Studio 'Yu açmak için iki yol vardır:

* SYNAPSE çalışma alanınızı [Azure Portal](https://portal.azure.com)açın, SYNAPSE çalışma alanının **genel bakış** bölümünde SYNAPSE Studio 'yu Aç kutusunda **Aç** ' ı seçin.
* Adresine gidin `https://web.azuresynapse.net` ve çalışma alanınızda oturum açın.


## <a name="the-built-in-serverless-sql-pool"></a>Yerleşik sunucusuz SQL havuzu

Her çalışma alanı, **yerleşik** olarak adlandırılan önceden oluşturulmuş bir SUNUCUSUZ SQL havuzu ile gelir. Bu havuz silinemiyor. Sunucusuz SQL havuzları, adanmış SQL havuzlarıyla kapasiteyi ayırmak zorunda kalmadan SQL kullanmanıza imkan sağlar. Adanmış SQL havuzlarının aksine, sunucusuz SQL havuzu için faturalandırma, havuza ayrılan kapasite sayısını değil, sorguyu çalıştırmak için taranan veri miktarına bağlıdır.


## <a name="create-a-dedicated-sql-pool"></a>Adanmış bir SQL havuzu oluşturma

1. SYNAPSE Studio 'da, sol taraftaki bölmede   >  **SQL havuzlarını** Yönet ' i seçin.
1. **Yeni** ' yi seçin
1. **SQL havuzu adı** için **SQLPOOL1** seçin
1. **Performans düzeyi** için **DW100C** seçin
1. **Gözden geçir ve oluştur** > **Oluştur**'u seçin. Adanmış SQL havuzunuz birkaç dakika içinde hazırlanacaktır. Adanmış SQL havuzunuz, **SQLPOOL1** olarak da bilinen ayrılmış bir SQL havuzu veritabanıyla ilişkilidir.

Adanmış bir SQL havuzu, etkin olduğu sürece faturalanabilir kaynakları kullanır. Daha sonra maliyetleri azaltmak için havuzu duraklatabilirsiniz.

> [!NOTE] 
> Çalışma alanınızda yeni bir adanmış SQL Havuzu (eski adıyla SQL DW) oluştururken, adanmış SQL havuzu sağlama sayfası açılır. Sağlama, mantıksal SQL Server üzerinde gerçekleşir.


## <a name="create-a-serverless-apache-spark-pool"></a>Sunucusuz Apache Spark havuzu oluşturma

1. SYNAPSE Studio 'da, sol taraftaki bölmede   >  **Apache Spark havuzlarını** Yönet ' i seçin.
1. **Yeni** ' yi seçin 
1. **Apache Spark havuz adı** için **Spark1** girin.
1. **Düğüm boyutu** için **küçük** girin.
1. **Düğüm sayısı** için en az 3 ve en fazla 3 olarak ayarlayın
1. **Gözden geçir ve oluştur** > **Oluştur**'u seçin. Apache Spark havuzunuz birkaç saniye içinde hazırlanacaktır.

Spark havuzu, Azure SYNAPSE 'in kaç Spark kaynağı kullandığını söyler. Yalnızca kullandığınız kaynaklar için ödeme yaparsınız. Havuzu kullanmayı etkin bir şekilde durdurduğunuzda, kaynakların otomatik olarak zaman aşımına uğrar ve geri dönüştürülür.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Adanmış bir SQL havuzu kullanarak çözümle](get-started-analyze-sql-pool.md)
