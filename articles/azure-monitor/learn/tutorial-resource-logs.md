---
title: Azure kaynağından kaynak günlüklerini toplayın ve Azure Izleyici ile çözümleyin
description: Bir Azure kaynağından kaynak günlüklerini, bir günlük sorgusuyla çözümlenebilecekleri bir Log Analytics çalışma alanına toplamak üzere tanılama ayarlarını yapılandırma öğreticisi.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: tutorial
author: bwren
ms.author: bwren
ms.date: 12/15/2019
ms.openlocfilehash: 90b2a9bc9e3e8aa6297f02a46163717a2bf58a22
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/28/2019
ms.locfileid: "75533562"
---
# <a name="tutorial-collect-and-analyze-resource-logs-from-an-azure-resource"></a>Öğretici: Azure kaynağından kaynak günlüklerini toplayın ve çözümleyin

Kaynak günlükleri bir Azure kaynağının ayrıntılı işlemine ilişkin öngörüler sağlar ve sistem durumunu ve kullanılabilirliğini izlemek için yararlıdır. Azure kaynakları otomatik olarak kaynak günlükleri oluşturur, ancak toplanacak yerleri yapılandırmanız gerekir. Bu öğretici, Azure aboneliğinizdeki bir kaynağın kaynak günlüklerini toplamak ve bir günlük sorgusuyla çözümlemek için bir tanılama ayarı oluşturma sürecinde size kılavuzluk ediyor.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure Izleyici 'de Log Analytics çalışma alanı oluşturma
> * Kaynak günlüklerini toplamak için bir tanılama ayarı oluşturma 
> * Günlükleri çözümlemek için basit bir günlük sorgusu oluşturma


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlayabilmeniz için izlemeniz gereken bir Azure kaynağınız olması gerekir. Azure aboneliğinizdeki herhangi bir kaynağı tanılama ayarlarını destekleyen bir kaynak kullanabilirsiniz. Bir kaynağın tanılama ayarlarını destekleyip desteklemediğini anlamak için Azure portal menüsüne gidin ve menünün **izleme** bölümünde bir **Tanılama ayarları** seçeneğinin olduğunu doğrulayın.


## <a name="log-in-to-azure"></a>Azure'da oturum açma
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.


## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
Azure Izleyici 'deki bir Log Analytics çalışma alanı, çeşitli kaynaklardan günlük verilerini toplar ve dizinler ve güçlü bir sorgu dili kullanarak gelişmiş analizler sağlar. Veri göndermek için bir tanılama ayarı oluşturmadan önce Log Analytics çalışma alanının mevcut olması gerekir. Azure aboneliğinizde var olan bir çalışma alanını kullanabilir veya aşağıdaki yordamla bir tane oluşturabilirsiniz. 

> [!NOTE]
> **Azure izleyici** menüsündeki Log Analytics çalışma alanlarında verilerle çalışıyorsanız, **Log Analytics çalışma alanları** menüsündeki çalışma alanlarını oluşturup yönetirsiniz.

1. **Tüm hizmetlerden** **Log Analytics çalışma alanları**' nı seçin.
2. Ekranın üst kısmındaki **Ekle** ' ye tıklayın ve çalışma alanı için aşağıdaki ayrıntıları sağlayın:
   - **Log Analytics çalışma alanı**: yeni çalışma alanının adı. Bu ad tüm Azure Izleyici abonelikleri genelinde genel olarak benzersiz olmalıdır.
   - **Abonelik**: çalışma alanını depolamak için aboneliği seçin. Bu, izlenmekte olan kaynakla aynı abonelik olması gerekmez.
   - **Kaynak grubu**: var olan bir kaynak grubunu seçin veya yeni bir tane oluşturmak Için **Yeni oluştur** ' a tıklayın. Bu, izlenmekte olan kaynakla aynı kaynak grubu olması gerekmez.
   - **Konum**: bir Azure bölgesi seçin ya da yeni bir tane oluşturun. Bu, izlenmekte olan kaynakla aynı konum olması gerekmez.
   - **Fiyatlandırma katmanı**: 7 günlük verileri koruyacak *ücretsiz* ' ı seçin. Bu fiyatlandırma katmanını daha sonra değiştirebilirsiniz. Farklı fiyatlandırma katmanları hakkında daha fazla bilgi edinmek için **Log Analytics fiyatlandırma** bağlantısına tıklayın.

    ![Yeni çalışma alanı](media/tutorial-resource-logs/new-workspace.png)

3. Çalışma alanını oluşturmak için **Tamam** ' ı tıklatın.

## <a name="create-a-diagnostic-setting"></a>Tanılama ayarı oluştur
[Tanılama ayarları](../platform/diagnostic-settings.md) , belirli bir kaynak için kaynak günlüklerinin nereye gönderileceğini tanımlar. Tek bir tanılama ayarında birden çok [hedef](../platform/diagnostic-settings.md#destinations)bulunabilir, ancak bu öğreticide yalnızca bir Log Analytics çalışma alanı kullanacağız.

1. Kaynağınızın menüsünün **izleme** bölümü altında **Tanılama ayarları**' nı seçin.
2. "Tanılama ayarları tanımlanmamış" iletisine sahip olmalısınız. **Tanılama ayarı Ekle**' ye tıklayın.

    ![Tanılama ayarları](media/tutorial-resource-logs/diagnostic-settings.png)

3. Her tanılama ayarının üç temel bölümü vardır:
 
   - **Ad**: Bu, önemli bir etkiye sahip değildir ve yalnızca sizin için açıklayıcı olmalıdır.
   - **Hedefler**: günlükleri göndermek için bir veya daha fazla hedef. Tüm Azure hizmetleri aynı üç olası hedef kümesini paylaşır. Her bir tanılama ayarı bir veya daha fazla hedef tanımlayabilir, ancak belirli bir türün birden fazla hedefi olamaz. 
   - **Kategoriler**: hedeflerin her birine gönderilecek günlüklerin kategorileri. Kategori kümesi, her bir Azure hizmeti için farklılık gösterecektir.

4. **Log Analytics çalışma alanına gönder** ' i seçin ve ardından oluşturduğunuz çalışma alanını seçin.
5. Toplamak istediğiniz kategorileri seçin. Kullanılabilir kategorilerinin bir tanımı için her hizmet için belgelere bakın.

    ![Tanılama ayarı](media/tutorial-resource-logs/diagnostic-setting.png)

6. Tanılama ayarlarını kaydetmek için **Kaydet** ' e tıklayın.

    
 
 ## <a name="use-a-log-query-to-retrieve-logs"></a>Günlükleri almak için bir günlük sorgusu kullanma
Veriler, kusto sorgu dilinde (KQL) yazılmış bir günlük sorgusu kullanılarak Log Analytics çalışma alanından alınır. Azure Izleyici 'deki Öngörüler ve çözümler, belirli bir hizmet için veri almaya yönelik günlük sorguları sağlar, ancak günlük sorgularıyla doğrudan ve Log Analytics ile Azure portal sonuçlarla çalışabilirsiniz. 

1. Kaynağınızın menüsünün **izleme** bölümü altında **Günlükler**' i seçin.
2. Log Analytics, kaynağı olarak ayarlanmış bir boş sorgu penceresiyle açılır. Tüm sorgular yalnızca bu kaynaktaki kayıtları içerir.

    > [!NOTE]
    > Günlükleri Azure Izleyici menüsünden açtıysanız, kapsam Log Analytics çalışma alanına ayarlanır. Bu durumda, tüm sorgular çalışma alanındaki tüm kayıtları içerir.
   
    ![Günlükler](media/tutorial-resource-logs/logs.png)

4. Örnekte gösterilen hizmet, kaynak günlüklerini **AzureDiagnostics** tablosuna yazar, ancak diğer hizmetler diğer tablolara yazabilir. Farklı Azure hizmetleri tarafından kullanılan tablolar için [Azure Kaynak günlükleri Için desteklenen hizmetler, şemalar ve Kategoriler](../platform/diagnostic-logs-schema.md) bölümüne bakın.

    > [!NOTE]
    > Birden çok hizmet, kaynak günlüklerini AzureDiagnostics tablosuna yazar. Azure Izleyici menüsünden Log Analytics başlatırsanız, belirli hizmetinizi belirtmek için `ResourceProvider` sütununa sahip bir `where` açıklaması eklemeniz gerekir. Bir kaynağın menüsünden Log Analytics başlattığınızda, bu sütun gerekli olmaması için kapsam yalnızca bu kaynaktaki kayıtlar olarak ayarlanır. Örnek sorgular için hizmetin belgelerine bakın.


5. Sonuçları incelemek için bir sorguyu yazın ve **Çalıştır** ' a tıklayın. 
6. Günlük sorguları yazma hakkında bir öğretici için bkz. [Azure izleyici 'de günlük sorgularıyla çalışmaya başlama](../log-query/get-started-queries.md) .

    ![Günlük sorgusu](media/tutorial-resource-logs/log-query-1.png)




## <a name="next-steps"></a>Sonraki adımlar
Kaynak günlüklerinin bir Log Analytics çalışma alanına nasıl toplanacağını öğrendiğinize göre, bu verileri çözümlemek için günlük sorguları yazma hakkında bir öğretici doldurun.

> [!div class="nextstepaction"]
> [Azure Izleyici 'de günlük sorgularını kullanmaya başlama](../log-query/get-started-queries.md)
