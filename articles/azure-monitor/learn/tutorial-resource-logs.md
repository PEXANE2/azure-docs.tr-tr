---
title: Azure Kaynağından kaynak günlükleri toplayın ve Azure Monitor ile analiz edin
description: Bir Azure kaynağından kaynak günlüklerini bir günlük sorgusuyla analiz edilebilen bir Günlük Analizi çalışma alanına toplamak için tanılama ayarlarını yapılandırma öğreticisi.
ms.subservice: ''
ms.topic: tutorial
author: bwren
ms.author: bwren
ms.date: 12/15/2019
ms.openlocfilehash: d356042d65c419163de4951e64a635a22ea90e6d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78269203"
---
# <a name="tutorial-collect-and-analyze-resource-logs-from-an-azure-resource"></a>Öğretici: Azure kaynağından kaynak günlüklerini toplama ve analiz

Kaynak günlükleri, bir Azure kaynağının ayrıntılı çalışması hakkında bilgi sağlar ve sistem ve kullanılabilirliklerini izlemek için yararlıdır. Azure kaynakları kaynak günlüklerini otomatik olarak oluşturur, ancak bunların toplanması gereken yeri yapılandırmanız gerekir. Bu öğretici, Azure aboneliğinizdeki bir kaynak için kaynak günlükleri toplamak ve günlük sorgusuyla çözümlemek için tanılama ayarı oluşturma işlemine geçer.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure Monitör'de Günlük Analizi çalışma alanı oluşturma
> * Kaynak günlüklerini toplamak için tanılama ayarı oluşturma 
> * Günlükleri çözümlemek için basit bir günlük sorgusu oluşturma


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için izlemek için bir Azure kaynağına ihtiyacınız var. Azure aboneliğinizde tanılama ayarlarını destekleyen tüm kaynakları kullanabilirsiniz. Kaynağın tanılama ayarlarını destekleyip desteklemediğini belirlemek için Azure portalındaki menüsüne gidin ve menünün **İzleme** bölümünde **Tanılama ayarları** seçeneği olduğunu doğrulayın.


## <a name="log-in-to-azure"></a>Azure'da oturum açma
Azure portalına giriş [https://portal.azure.com](https://portal.azure.com)yapın.


## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
Azure Monitor'daki Log Analytics çalışma alanı, çeşitli kaynaklardan günlük verileri toplar ve dizinler ve güçlü bir sorgu dili kullanarak gelişmiş analizsağlar. Giriş Analitiği çalışma alanının, veri göndermek için bir tanıa ayar oluşturmadan önce var olması gerekir. Azure aboneliğinizde varolan bir çalışma alanı kullanabilir veya aşağıdaki yordamı kullanarak bir çalışma alanı oluşturabilirsiniz. 

> [!NOTE]
> **Azure Monitor** menüsündeki Log Analytics çalışma alanlarındaki verilerle çalışabiliyorken, **Log Analytics çalışma alanları** menüsünde çalışma alanları oluşturur ve yönetirsiniz.

1. **Tüm hizmetlerden** **Log Analytics çalışma alanlarını**seçin.
2. Ekranın üst kısmında **Ekle'yi** tıklatın ve çalışma alanı için aşağıdaki ayrıntıları sağlayın:
   - **Günlük Analitiği çalışma alanı**: Yeni çalışma alanının adı. Bu ad, tüm Azure Monitor aboneliklerinde genel olarak benzersiz olmalıdır.
   - **Abonelik**: Çalışma alanını depolamak için aboneliği seçin. Bu, izlenen kaynakla aynı abonelik olması gerekmez.
   - **Kaynak Grubu**: Varolan bir kaynak grubu seçin veya yeni sini oluşturmak için **yeni oluştur'u** tıklatın. Bu, izlenen kaynakla aynı kaynak grubu olması gerekmez.
   - **Konum**: Bir Azure bölgesi seçin veya yeni bir bölge oluşturun. Bu, izlenen kaynakla aynı konumda olması gerekmez.
   - **Fiyatlandırma katmanı**: Fiyatlandırma katmanı olarak *istediğiniz kadar öde'yi* seçin. Bu fiyatlandırma katmanını daha sonra değiştirebilirsiniz. Farklı fiyatlandırma katmanları hakkında daha fazla bilgi edinmek için **Günlük Analizi fiyatlandırma** bağlantısını tıklayın.

    ![Yeni çalışma alanı](media/tutorial-resource-logs/new-workspace.png)

3. Çalışma alanını oluşturmak için **Tamam'ı** tıklatın.

## <a name="create-a-diagnostic-setting"></a>Tanılama ayarı oluşturma
[Tanılama ayarları,](../platform/diagnostic-settings.md) belirli bir kaynak için kaynak günlüklerinin nereye gönderilmesi gerektiğini tanımlar. Tek bir tanılama ayarı birden çok [hedefe](../platform/diagnostic-settings.md#destinations)sahip olabilir, ancak bu öğreticide yalnızca bir Log Analytics çalışma alanı kullanırız.

1. Kaynak menünüzün **İzleme** bölümünde **Tanılama ayarlarını**seçin.
2. "Tanılama ayarları tanımlanmadı" iletisi olmalıdır. **Tanı ayarını ekle'yi**tıklatın.

    ![Tanılama ayarları](media/tutorial-resource-logs/diagnostic-settings.png)

3. Her tanılama ayarı üç temel bölümden oluşur:
 
   - **Adı**: Bu önemli bir etkisi yoktur ve sadece sizin için açıklayıcı olmalıdır.
   - **Gidilecek Yerler**: Günlükleri göndermek için bir veya daha fazla hedef. Tüm Azure hizmetleri, üç olası hedef kümesini paylaşır. Her tanılama ayarı bir veya daha fazla hedef tanımlayabilir, ancak belirli bir türde birden fazla hedef belirleyemez. 
   - **Kategorisi**: Her bir hedefe gönderilecek günlük kategorileri. Kategoriler kümesi her Azure hizmeti için değişir.

4. **Günlük Analitiği Çalışma Alanını Gönder'i** seçin ve ardından oluşturduğunuz çalışma alanını seçin.
5. Toplamak istediğiniz kategorileri seçin. Kullanılabilir kategorilerin tanımı için her hizmetiçin belgelere bakın.

    ![Tanılama ayarı](media/tutorial-resource-logs/diagnostic-setting.png)

6. Tanılama ayarlarını kaydetmek için **Kaydet'i** tıklatın.

    
 
 ## <a name="use-a-log-query-to-retrieve-logs"></a>Günlükleri almak için günlük sorgusu kullanma
Veriler, Kusto Query Language (KQL) ile yazılmış bir günlük sorgusu kullanılarak Günlük Analizi çalışma alanından alınır. Azure Monitor'daki öngörüler ve çözümler, belirli bir hizmet için veri almak için günlük sorguları sağlar, ancak günlük sorguları ve bunların sonuçlarıyla Doğrudan Log Analytics ile Azure portalında çalışabilirsiniz. 

1. Kaynak menünün **İzleme** bölümünde **Günlükler'i**seçin.
2. Log Analytics, kapsamı kaynağınıza ayarlanmış boş bir sorgu penceresi ile açılır. Tüm sorgular yalnızca bu kaynaktan gelen kayıtları içerir.

    > [!NOTE]
    > Azure Monitor menüsünden Günlükler'i açtıysanız, kapsam Log Analytics çalışma alanına ayarlanır. Bu durumda, tüm sorgular çalışma alanındaki tüm kayıtları içerir.
   
    ![Günlükler](media/tutorial-resource-logs/logs.png)

4. Örnekte gösterilen hizmet **AzureDiagnostics** tablosuna kaynak günlükleri yazar, ancak diğer hizmetler diğer tablolara yazabilir. Farklı Azure hizmetleri tarafından kullanılan tablolar [için Desteklenen hizmetler, şemalar ve Kategorilere](../platform/diagnostic-logs-schema.md) bakın Azure Kaynak Günlükleri için.

    > [!NOTE]
    > Birden çok hizmet AzureDiagnostics tablosuna kaynak günlükleri yazar. Azure Monitor menüsünden Log Analytics'i başlatıysanız, `where` belirli bir `ResourceProvider` hizmetinizi belirtmek için sütuna bir ekstre eklemeniz gerekir. Günlük Analizi'ni bir kaynağın menüsünden başlattığınızda, kapsam yalnızca bu kaynaktan kaydolacak şekilde ayarlanır, böylece bu sütun gerekli değildir. Örnek sorgular için hizmetbelgelerine bakın.


5. Sorgu yazın ve sonuçları incelemek için **Çalıştır'ı** tıklatın. 
6. Bkz. Günlük sorguları yazma yla ilgili bir öğretici için [Azure Monitor'daki günlük sorgularına başlayın.](../log-query/get-started-queries.md)

    ![Günlük sorgusu](media/tutorial-resource-logs/log-query-1.png)




## <a name="next-steps"></a>Sonraki adımlar
Artık Bir Log Analytics çalışma alanında kaynak günlükleri toplamayı öğrendiğiniz için, bu verileri çözümlemek için günlük sorguları yazma yla ilgili bir eğitimi tamamlayın.

> [!div class="nextstepaction"]
> [Azure Monitor'da günlük sorgularına başlayın](../log-query/get-started-queries.md)
