---
title: Öğretici - Azure bütçesi oluşturma ve yönetme | Microsoft Docs
description: Bu öğretici, kullandığınız Azure hizmetlerinin maliyetleriyle ilgili plan yapmanıza yardımcı olmaktadır.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 01/22/2020
ms.topic: conceptual
ms.service: cost-management-billing
manager: adwise
ms.custom: seodec18
ms.openlocfilehash: bb02c4903348a3b8c1d129f02be64109ec0f48eb
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76769763"
---
# <a name="tutorial-create-and-manage-azure-budgets"></a>Öğretici: Azure bütçesi oluşturma ve yönetme

Maliyet Yönetimi hizmetindeki bütçe işlevi, kuruluşunuzda sorumluluk kültürünü planlamanıza ve güçlendirmenize yardımcı olur. Bütçeleri kullanarak belirli bir dönem içinde kullandığınız veya abone olduğunuz Azure hizmetlerini takip edebilirsiniz. Bütçeler, maliyetleri önceden yönetmek için diğer kullanıcıları harcamaları konusunda bilgilendirmenize ve zaman içindeki harcamayı izlemenize yardımcı olur. Oluşturduğunuz bütçe eşikleri aşıldığında yalnızca bildirimler tetiklenir. Kaynaklarınızın hiçbiri etkilenmez ve tüketiminiz durdurulmaz. Bütçeleri kullanarak maliyet analizi sırasında harcamaları karşılaştırabilir ve izleyebilirsiniz.

Maliyet ve kullanım verileri genellikle 12-16 saat içinde kullanıma sunulur ve bütçeler, dört saatte bir bu maliyetlere göre değerlendirilir. E-posta bildirimleri normal şartlarda 12-16 saat içinde gönderilir.

Gelecekte bir son kullanma tarihi seçtiğinizde bütçeler, dönem sonunda (ay, üç ay veya yıl) otomatik olarak aynı bütçe tutarına geri döner. Aynı bütçe tutarına geri döndükleri için bütçedeki para birimlerinin ilerleyen dönemlerde farklı olması halinde ayrı bütçeler oluşturmanız gerekir.

Bu öğreticideki örnekler, Azure Kurumsal Anlaşma (EA) aboneliği için bütçe oluşturma ve düzenleme konusunda size yol gösterecektir.

Azure harcamalarınızı izleme amacıyla bütçe oluşturma konusunda bilgi edinmek için [Azure portalını kullanarak aboneliklere bütçe uygulama](https://www.youtube.com/watch?v=UrkHiUx19Po) videosunu izleyin.


Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure portalında bütçe oluşturma
> * Bir bütçeyi düzenleme

## <a name="prerequisites"></a>Ön koşullar

Bütçeler, farklı Azure hesabı türleri için desteklenir. Desteklenen hesap türlerinin tam listesini görüntülemek için, bkz. [Maliyet Yönetimi verilerini anlama](understand-cost-mgt-data.md). Bütçeleri görüntülemek için Azure hesabınızda en azından okuma yetkisine sahip olmanız gerekir.

 Azure EA aboneliklerinde bütçeleri görüntülemek için okuma yetkisine sahip olmanız gerekir. Bütçe oluşturmak ve yönetmek için katkıda bulunan izninizin olması gerekir. EA abonelikleri ve kaynak grupları için ayrı bütçeler oluşturabilirsiniz. Ancak EA ödeme hesapları için bütçe oluşturamazsınız.

Aşağıdaki Azure izinleri veya kapsamları abonelik başına kullanıcı ve grup tarafından bütçe oluşturulması için desteklenir. Kapsamlar hakkında daha fazla bilgi için bkz. [Kapsamları anlama ve birlikte çalışma](understand-work-scopes.md).

- Sahip – Bir abonelik için bütçe oluşturabilir, değiştirebilir veya silebilir.
- Katkıda bulunan ve Maliyet Yönetimi katkıda bulunanı - Kendi bütçelerini oluşturabilir, değiştirebilir veya silebilir. Başkaları tarafından oluşturulan bütçelerin miktarlarını değiştirebilir.
- Okuyucu ve Maliyet Yönetimi okuyucusu - İzin verilen bütçeleri görüntüleyebilir.

Maliyet Yönetimi verilerine izin atama hakkında daha fazla bilgi için bkz. [Maliyet Yönetimi verilerine erişim atama](../../cost-management/assign-access-acm-data.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

- https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-budget-in-the-azure-portal"></a>Azure portalında bütçe oluşturma

Aylık, üç aylık veya yıllık dönem için bir Azure abonelik bütçesi oluşturabilirsiniz. Azure portalındaki gezinti içeriği, abonelik veya yönetim grubu için bütçe oluşturma durumunuzu belirler.

Bütçe oluşturmak veya görüntülemek için, Azure portalında istediğiniz kapsamı açın ve menüden **Bütçeler**'i seçin. Örneğin **Abonelikler**'e gidin, listeden bir abonelik belirleyin ve menüden **Bütçeler**'i seçin. Bütçeler içindeyken yönetim grubu gibi farklı bir kapsama geçiş yapmak için **Kapsam** düğmesini kullanın. Kapsamlar hakkında daha fazla bilgi için bkz. [Kapsamları anlama ve birlikte çalışma](understand-work-scopes.md).

Bütçeleri oluşturduktan sonra bunlar için geçerli harcamalarınızın basit bir görünümü gösterilir.

**Ekle**'ye tıklayın.

![Önceden oluşturulmuş olan bütçelerin listesini gösteren örnek](./media/tutorial-acm-create-budgets/budgets01.png)

**Bütçe oluştur** penceresinde gösterilen kapsamın doğru olduğundan emin olun. Eklemek istediğiniz filtreleri seçin. Filtreler, bir abonelikteki kaynak grupları veya sanal makine hizmetleri gibi belirli maliyetlerle ilgili bütçeler oluşturmanızı sağlar. Maliyet analizinde kullanılan filtreler, bütçelere de uygulanabilir.

Kapsamınızı ve filtrelerinizi tanımladıktan sonra bir bütçe adı yazın. Ardından aylık, üç aylık veya yıllık bütçe sıfırlama dönemi seçin. Bu sıfırlama dönemi, bütçe tarafından çözümlenen zaman penceresini belirler. Bütçe tarafından değerlendirilen maliyet her yeni dönemin başlangıcında sıfırdan başlar. Üç aylık bir bütçe oluşturduğunuzda aylık bütçeyle aynı şekilde çalışır. Aradaki fark, üç aylık bütçe tutarının üç ay arasına eşit olarak bölünmesidir. Yıllık bütçe miktarı, takvim yılının 12 ayına eşit şekilde bölünür.

Kullandıkça Öde, MSDN veya Visual Studio aboneliğiniz varsa faturanızdaki fatura dönemi takvim ayından farklı olabilir. Bu abonelik türleri ve kaynak grupları için fatura döneminize veya takvim aylarına uyan bir bütçe oluşturabilirsiniz. Fatura döneminize uygun bir bütçe oluşturmak için **Faturalama ayı**, **Faturalama üç aylık dönemi** veya **Faturalama yılı** gibi bir sıfırlama dönemi seçin. Takvim ayına uygun bir bütçe oluşturmak için **Aylık**, **Üç Aylık** veya **Yıllık** sıfırlama dönemi seçin.

Ardından bütçenin geçersiz hale geldiği ve maliyetlerinizi değerlendirmeyi durdurduğu sona erme tarihini belirleyebilirsiniz.

Bütçede şu ana kadar seçtiğiniz alanlara bağlı olarak, bütçeniz için kullanılacak eşik değerini seçmenize yardımcı olacak bir grafik gösterilir. Önerilen bütçe, gelecekteki dönemlerde tabi olabileceğiniz en yüksek tahmini maliyeti temel alır. Bütçe tutarını değiştirebilirsiniz.

![Aylık maliyet verileriyle bütçe oluşturma işlemini gösteren örnek ](./media/tutorial-acm-create-budgets/monthly-budget01.png)

Bütçe tutarını yapılandırdıktan sonra bütçe uyarılarını yapılandırmak için **İleri**'ye tıklayın. Bütçeler için en az bir maliyet eşiği (bütçe yüzdesi) ve buna karşılık gelen bir e-posta adresi gerekir. İsteğe bağlı olarak tek bir bütçeye en fazla beş eşik ve beş e-posta adresi ekleyebilirsiniz. Bir bütçe eşiği karşılandığında e-posta bildirimleri genellikle 20 saatten az bir sürede alınır. Bildirimleri hakkında daha fazla bilgi için, bkz. [Maliyeti uyarılarını kullanma](../../cost-management/cost-mgt-alerts-monitor-usage-spending.md). Aşağıdaki örnekte, bütçenin %90'ına ulaşıldığında bir e-posta uyarısı oluşturulmuştur. Bütçeler API'si ile bütçe oluşturduğunuzda kullanıcılara uyarı gönderilmesi için roller de atayabilirsiniz. Kişilere rol atama işlemi Azure portalında gerçekleştirilemez. Azure Bütçeler API'si hakkında daha fazla bilgi için bkz. [Bütçeler API'si](/rest/api/consumption/budgets).

![Uyarı koşullarını gösteren örnek](./media/tutorial-acm-create-budgets/monthly-budget-alert.png)

Bir bütçe oluşturulduktan sonra maliyet analizi bölümünde gösterilir. Bütçenizin harcama eğilimi ile bağlantılı olarak görüntülenmesi, [maliyetlerinizi ve harcamalarınızı analiz etmeye](../../cost-management/quick-acm-cost-analysis.md) başladığınızda kullanacağınız ilk adımlardan biridir.

![Maliyet analizinde gösterilen örnek bütçe ve harcama](./media/tutorial-acm-create-budgets/cost-analysis.png)

Yukarıdaki örnekte abonelik için bir bütçe oluşturdunuz. Ancak bir kaynak grubu için de bütçe oluşturabilirsiniz. Bir kaynak grubu için bütçe oluşturmak isterseniz **Maliyet Yönetimi + Faturalandırma** &gt; **Abonelikler** &gt; yolunu izleyin, bir abonelik seçin > **Kaynak grupları** > bir kaynak grubu seçin > **Bütçeler** > ve **Ekle**'yi seçerek bir bütçe ekleyin.

## <a name="costs-in-budget-evaluations"></a>Bütçe maliyeti değerlendirmeleri

Bütçe maliyeti değerlendirmeleri artık ayrılmış örnek ve satın alma verilerini de içeriyor. Ücretlerin sizin için geçerli olması halinde değerlendirmelerinize dahil ettiğiniz ücretler için uyarılar alabilirsiniz. Bütçe eşiklerinin yeni maliyetleri dikkate alacak şekilde yapılandırıldığını doğrulamak için [Azure portalında](https://portal.azure.com) oturum açmanız önerilir. Faturalandırılan Azure ücretleriniz değişmez. Bütçeler artık maliyetlerinizin daha kapsamlı bir kümesine göre değerlendiriliyor. Ücretler sizin için uygun değilse bütçe davranışlarınız aynı şekilde kalır.

Bütçelerin yalnızca birinci taraf Azure tüketim ücretlerine göre değerlendirilmesi için yeni maliyetleri filtrelemek istiyorsanız, bütçenize aşağıdaki filtreleri ekleyin:

- Yayımcı Türü: Azure
- Ücret Türü: Kullanım

Bütçe maliyet değerlendirmeleri, gerçek maliyeti temel alır. Amorti edilen maliyetler kullanılmaz. Bütçelerde kullanabileceğiniz filtreleme seçenekleri hakkında daha fazla bilgi için bkz. [Gruplandırma ve filtreleme seçeneklerini anlama](quick-acm-cost-analysis.md#understanding-grouping-and-filtering-options).


## <a name="trigger-an-action-group"></a>Bir eylem grubunu tetikleme

Bir abonelik veya kaynak grubu kapsamı için bütçe oluşturma veya düzenleme aşamasında eylem grubu çağıracak şekilde yapılandırabilirsiniz. Bütçe eşiğinize ulaşıldığında eylem grubu çeşitli eylemler gerçekleştirebilir. Eylem Grupları şu anda yalnızca abonelik ve kaynak grubu kapsamları için desteklenmektedir. Eylem Grupları hakkında daha fazla bilgi için bkz. [Azure portalında eylem grubu oluşturma ve yönetme](../../azure-monitor/platform/action-groups.md). Eylem gruplarıyla bütçe tabanlı otomasyon kullanma hakkında daha fazla bilgi için bkz. [Azure bütçeleri ile maliyetleri yönetme](../manage/cost-management-budget-scenario.md).



Eylem grubu oluşturmak veya güncelleştirmek için bütçe oluşturma veya düzenleme aşamasında **Eylem gruplarını yönet**'e tıklayın.

![Eylem gruplarını yönet seçeneğini gösteren bütçe oluşturma örneği](./media/tutorial-acm-create-budgets/manage-action-groups01.png)


Ardından eylem grubunu oluşturmak için **Eylem grubu ekle**'ye tıklayın.


![Eylem grubu ekle kutusunun görüntüsü](./media/tutorial-acm-create-budgets/manage-action-groups02.png)

Eylem grubu oluşturulduktan sonra bütçenize geri dönmek için kutuyu kapatın.

Bütçenizi, tek bir eşik karşılandığında eylem grubunuzu kullanacak şekilde yapılandırın. En fazla beş farklı eşik desteklenir.

![Uyarı koşulu için eylem grubu seçimini gösteren örnek](./media/tutorial-acm-create-budgets/manage-action-groups03.png)

Aşağıdaki örnek %50, %75 ve %100 olarak ayarlanmış bütçe eşiklerini göstermektedir. Her biri belirlenen eylem grubundaki belirli eylemleri tetikleyecek şekilde yapılandırılmıştır.

![Farklı eylem grupları ve eylem türleriyle yapılandırılmış uyarı koşullarını gösteren örnek](./media/tutorial-acm-create-budgets/manage-action-groups04.png)

Bütçelerin eylem gruplarıyla tümleştirilmesi yalnızca ortak uyarı şeması devre dışı bırakılmış olan eylem grupları için kullanılabilir. Şemayı devre dışı bırakma hakkında daha fazla bilgi için bkz. [Ortam uyarı şemasını nasıl devre dışı bırakabilirim?](../../azure-monitor/platform/alerts-common-schema.md#how-do-i-enable-the-common-alert-schema)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure portalında bütçe oluşturma
> * Bir bütçeyi düzenleme

Maliyet yönetimi verileriniz için yinelenen dışarı aktarma işlemi oluşturmak üzere sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Dışarı aktarılan verileri oluşturma ve yönetme](tutorial-export-acm-data.md)
