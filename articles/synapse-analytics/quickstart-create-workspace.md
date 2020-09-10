---
title: 'Hızlı başlangıç: SYNAPSE çalışma alanı oluşturma'
description: Bu kılavuzdaki adımları izleyerek bir Synapse çalışma alanı oluşturun.
services: synapse-analytics
author: pimorano
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 09/03/2020
ms.author: pimorano
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 99f4471a3b64990fb71341dc8210bf7f8e5b0d5a
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89661941"
---
# <a name="quickstart-create-a-synapse-workspace"></a>Hızlı başlangıç: SYNAPSE çalışma alanı oluşturma
Bu hızlı başlangıçta Azure portal kullanarak bir Azure SYNAPSE çalışma alanı oluşturma adımları açıklanmaktadır.

## <a name="create-a-synapse-workspace"></a>Synapse çalışma alanı oluşturma

1. [Azure Portal](https://portal.azure.com)açın ve en üstteki **SYNAPSE**için arama yapın.
1. Arama sonuçlarında **Hizmetler**altında **Azure SYNAPSE Analytics (çalışma alanları Önizleme)** öğesini seçin.
1. Bu ayarları kullanarak bir çalışma alanı oluşturmak için **Ekle** ' yi seçin:

    |Tab|Ayar | Önerilen değer | Açıklama |
    |---|---|---|---|
    |Temel Bilgiler|**Çalışma alanı adı**|Bunu herhangi bir şekilde adlandırın.| Bu belgede, **MyWorkspace**kullanacağız.|
    |Temel Bilgiler|**Bölge**|Depolama hesabının bölgesiyle eşleştirin.|

1. Bir çalışma alanı oluşturmak için bir ADLSGEN2 hesabınızın olması gerekir. Yeni bir tane oluşturmak için en basit seçenektir. Mevcut bir işlemi yeniden kullanmak istiyorsanız, bazı ek yapılandırmalar gerçekleştirmeniz gerekir. 
1. 1. seçenek yeni bir ADLSGEN2 hesabı oluşturma 
    1. **Data Lake Storage Gen 2**' yi seçin altında **Yeni oluştur** ' a tıklayın ve **contosolake**olarak adlandırın.
    1. **Data Lake Storage Gen 2**' yi seçin altında **dosya sistemi** ' ne tıklayın ve ardından **kullanıcıları**adlandırın.
1. 2. seçenek bu belgenin en altındaki **depolama hesabı hazırlama** yönergelerine bakın.
1. Azure SYNAPSE çalışma alanınız, bu depolama hesabını "birincil" depolama hesabı ve çalışma alanı verilerini depolamak için kapsayıcı olarak kullanacaktır. Çalışma alanı, verileri Apache Spark tablolarında depolar. Spark uygulama günlüklerini **/SYNAPSE/WorkspaceName**adlı bir klasörde depolar.
1. **Gözden geçir ve oluştur** > **Oluştur**'u seçin. Çalışma alanınız birkaç dakika içinde hazırlanıyor.

## <a name="open-synapse-studio"></a>SYNAPSE Studio 'Yu açın

Azure SYNAPSE çalışma alanınız oluşturulduktan sonra, SYNAPSE Studio 'Yu açmak için iki yol vardır:

* [Azure Portal](https://portal.azure.com)SYNAPSE çalışma alanınızı açın. **Genel bakış** bölümünün üst kısmında, **SYNAPSE Studio 'yu Başlat**' ı seçin.
* Adresine gidin `https://web.azuresynapse.net` ve çalışma alanınızda oturum açın.

## <a name="prepare-an-existing-storage-account-for-use-with-synapse-analytics"></a>Mevcut bir depolama hesabını SYNAPSE Analytics ile kullanmak üzere hazırlama

1. [Azure portalını](https://portal.azure.com) açın.
1. Mevcut bir ADLSGEN2 Storage hesabına git
1. Sol bölmede **erişim denetimi (IAM)** seçeneğini belirleyin. Ardından, aşağıdaki rolleri atayın veya zaten atanmış olduklarından emin olun:
    * Kendinizi **sahip** rolüne atayın.
    * Kendinizi **Depolama Blobu veri sahibi** rolüne atayın.
1. Sol bölmede **kapsayıcılar** ' ı seçin ve bir kapsayıcı oluşturun.
1. Kapsayıcıya bir ad verebilirsiniz. Bu belgede kapsayıcı **kullanıcılarını**adı vereceğiz.
1. Varsayılan ayar olan **genel erişim düzeyini**kabul edin ve **Oluştur**' u seçin.

### <a name="configure-access-to-the-storage-account-from-your-workspace"></a>Çalışma alanınızdan depolama hesabına erişimi yapılandırma

Azure SYNAPSE çalışma alanınız için Yönetilen kimlikler zaten depolama hesabına erişebilmiş olabilir. Aşağıdakileri sağlamak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) ve çalışma alanınız için seçilen birincil depolama hesabını açın.
1. Sol bölmeden **erişim denetimi (IAM)** seçeneğini belirleyin.
1. Aşağıdaki rolleri atayın veya zaten atandıklarından emin olun. Çalışma alanı kimliği ve çalışma alanı adı için aynı adı kullanıyoruz.
    * Depolama hesabındaki **Depolama Blobu veri katılımcısı** rolü için çalışma alanı kimliği olarak **MyWorkspace** ' i atayın.
    * Çalışma alanı adı olarak **MyWorkspace** atayın.

1. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [SQL havuzu oluşturma](quickstart-create-sql-pool-studio.md) 
* [Apache Spark havuzu oluşturma](quickstart-create-apache-spark-pool-portal.md)
* [İsteğe bağlı SQL kullanma](quickstart-sql-on-demand.md)
