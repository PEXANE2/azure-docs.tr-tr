---
title: Öğretici-Azure bütçelerini oluşturma ve yönetme | Microsoft Docs
description: Bu öğretici, kullandığınız Azure hizmetleri maliyetlerine yönelik planlamaya ve hesaba yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 11/06/2019
ms.topic: conceptual
ms.service: cost-management
manager: adwise
ms.custom: seodec18
ms.openlocfilehash: 8c3c0574389fc9808af3cd70c928ede82d375076
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2019
ms.locfileid: "73720720"
---
# <a name="tutorial-create-and-manage-azure-budgets"></a>Öğretici: Azure bütçeleri oluşturma ve yönetme

Maliyet Yönetimi hizmetindeki bütçe işlevi, kuruluşunuzda sorumluluk kültürünü planlamanıza ve güçlendirmenize yardımcı olur. Bütçeleri kullanarak belirli bir dönem içinde kullandığınız veya abone olduğunuz Azure hizmetlerini takip edebilirsiniz. Bu kişiler, maliyetleri, maliyetleri önceden yönetmek ve zaman içinde harcamanın nasıl ilerlediğini izlemek için başkalarına bilgilendirmeye yardımcı olur. Oluşturduğunuz bütçe eşikleri aşıldığında yalnızca bildirimler tetiklenir. Kaynaklarınızın hiçbiri etkilenmemiştir ve tüketiminiz durdurulmaz. Bütçeleri, maliyetleri analiz ettiğiniz şekilde karşılaştırmak ve izlemek için kullanabilirsiniz.

Maliyet ve kullanım verileri genellikle 12-16 saat içinde kullanılabilir ve bütçeler her dört saatte bir bu maliyetlere göre değerlendirilir. E-posta bildirimleri normalde 12-16 saat içinde alınır.

Gelecekte bir sona erme tarihi seçtiğinizde, bütçeler dönemin sonunda otomatik olarak (aylık, üç aylık veya yıllık) sıfırlanır. Aynı bütçe miktarıyla sıfırlandıklarından, gelecekteki dönemler için bütçelenmiş para birimi miktarları farklılık gösterdiği zaman ayrı bütçeler oluşturmanız gerekir.

Bu öğreticideki örneklerde Azure Kurumsal Anlaşma (EA) aboneliğine yönelik bir bütçe oluşturma ve bunları düzenlemeyle ilgili yönergeler verilmektedir.

Azure 'da harcamalarınızı izlemek için Azure [maliyet yönetimi videosunun nasıl bütçe oluşturulacağını](https://www.youtube.com/watch?v=ExIVG_Gr45A) izleyin.


Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure portal bir bütçe oluşturun
> * Bir bütçeyi düzenleme

## <a name="prerequisites"></a>Ön koşullar

Bütçeler, çeşitli Azure hesap türleri için desteklenir. Desteklenen hesap türlerinin tam listesini görüntülemek için bkz. [maliyet yönetimi verilerini anlama](understand-cost-mgt-data.md). Bütçeleri görüntülemek için, Azure hesabınız için en azından okuma erişiminizin olması gerekir.

 Azure EA abonelikleri için, bütçeleri görüntülemek için okuma erişiminizin olması gerekir. Bütçeleri oluşturup yönetmek için katkıda bulunan izninizin olması gerekir. EA abonelikleri ve kaynak grupları için bireysel bütçeler oluşturabilirsiniz. Ancak, EA faturalandırma hesapları için bütçeler oluşturamazsınız.

Aşağıdaki Azure izinleri veya kapsamları, Kullanıcı ve grup bütçeleri için abonelik başına desteklenir. Kapsamlar hakkında daha fazla bilgi için bkz. [kapsamları anlama ve bunlarla çalışma](understand-work-scopes.md).

- Sahip – Bir abonelik için bütçe oluşturabilir, değiştirebilir veya silebilir.
- Katkıda bulunan ve maliyet yönetimi katılımcısı – kendi bütçelerini oluşturabilir, değiştirebilir veya silebilir. Başkaları tarafından oluşturulan bütçelerin miktarlarını değiştirebilir.
- Okuyucu ve maliyet yönetimi okuyucusu – iznine sahip oldukları bütçeleri görüntüleyebilir.

Maliyet yönetimi verilerine izin atama hakkında daha fazla bilgi için bkz. [maliyet yönetimi verilerine erişim atama](assign-access-acm-data.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

- https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-budget-in-the-azure-portal"></a>Azure portal bir bütçe oluşturun

Aylık, üç aylık veya yıllık bir dönem için Azure abonelik bütçesini oluşturabilirsiniz. Azure portal gezinti içeriğiniz, bir abonelik veya bir yönetim grubu için bir bütçe oluşturup oluşturamayacağını belirler.

Bir bütçe oluşturmak veya görüntülemek için, Azure portal istenen kapsamı açın ve menüdeki **bütçeler** ' ı seçin. Örneğin, **abonelikler**' e gidin, listeden bir abonelik seçin ve ardından menüdeki **bütçeler** ' ı seçin. Bütçeleri bir yönetim grubu gibi farklı bir kapsama geçmek için **kapsam** hap 'i kullanın. Kapsamlar hakkında daha fazla bilgi için bkz. [kapsamları anlama ve bunlarla çalışma](understand-work-scopes.md).

Bütçeleri oluşturduktan sonra, bunlara karşı geçerli harcamalarınızın basit bir görünümünü gösterir.

**Ekle**'ye tıklayın.

![Zaten oluşturulan bütçelerin listesini gösteren örnek](./media/tutorial-acm-create-budgets/budgets01.png)

**Bütçe oluştur** penceresinde, gösterilen kapsamın doğru olduğundan emin olun. Eklemek istediğiniz filtreleri seçin. Filtreler, bir abonelikteki kaynak grupları veya sanal makineler gibi bir hizmet gibi belirli maliyetlerde bütçeler oluşturmanızı sağlar. Maliyet analizinde kullanabileceğiniz herhangi bir filtre de bir bütçeye uygulanabilir.

Kapsamınızı ve filtrelerinizi tanımladıktan sonra bir bütçe adı yazın. Ardından, aylık, üç aylık veya yıllık bir bütçe sıfırlama dönemi seçin. Bu sıfırlama süresi, bütçe tarafından çözümlenen zaman penceresini belirler. Bütçe tarafından değerlendirilen maliyet her yeni dönemin başlangıcında sıfırdan başlar. Üç aylık bir bütçe oluşturduğunuzda, bu, aylık bir bütçeyle aynı şekilde çalışmaktadır. Fark, üç aylık dönemin bütçe tutarının çeyreğin üç ay arasında eşit olarak bölündüğü farktır. Yıllık bir bütçe miktarı, takvim yılının 12 ay boyunca eşit olarak bölünür.

Kullandıkça öde, MSDN veya Visual Studio aboneliğiniz varsa faturanızda fatura dönemi takvim ayına hizalanmayabilir. Bu abonelik türleri ve kaynak grupları için, fatura döneminize veya takvim aylarınıza göre hizalanan bir bütçe oluşturabilirsiniz. Fatura döneminize hizalanmış bir bütçe oluşturmak için fatura **ayı**, **Fatura çeyreği**veya **Fatura yılından**bir sıfırlama dönemi seçin. Takvim aya hizalanmış bir bütçe oluşturmak için **aylık**, **üç aylık**veya **yıllık**bir sıfırlama dönemi seçin.

Ardından, bütçenin geçersiz hale geldiği ve maliyetlerinizi değerlendirmeyi durdurduğu sona erme tarihini belirleyebilirsiniz.

Bütçeye şu ana kadar seçtiğiniz alanlara bağlı olarak, bütçeniz için kullanılacak bir eşik seçmenize yardımcı olacak bir grafik gösterilir. Önerilen bütçe, gelecekteki dönemlerde tabi olabileceğiniz en yüksek tahmini maliyeti temel alır. Bütçe tutarını değiştirebilirsiniz.

![Aylık maliyet verileriyle bütçe oluşturmayı gösteren örnek ](./media/tutorial-acm-create-budgets/monthly-budget01.png)

Bütçe tutarını yapılandırdıktan sonra, bütçe uyarılarını yapılandırmak için **İleri** ' ye tıklayın. Bütçeler için en az bir maliyet eşiği (bütçe yüzdesi) ve buna karşılık gelen bir e-posta adresi gereklidir. İsteğe bağlı olarak, tek bir bütçede en fazla beş eşik ve beş e-posta adresi ekleyebilirsiniz. Bir bütçe eşiği karşılandığında, e-posta bildirimleri genellikle 20 saatten az bir sürede alınır. Bildirimler hakkında daha fazla bilgi için bkz. [kullanım maliyeti uyarıları](cost-mgt-alerts-monitor-usage-spending.md). Aşağıdaki örnekte, bütçenin %90 ' ına ulaşıldığında bir e-posta uyarısı oluşturulur. Bütçeler API 'SI ile bir bütçe oluşturursanız, kullanıcılara uyarı almak için roller de atayabilirsiniz. Kişilere rol atama Azure portal desteklenmez. Azure bütçeleri API 'SI hakkında daha fazla bilgi için bkz. [Bütçe API 'si](/rest/api/consumption/budgets).

![Uyarı koşullarını gösteren örnek](./media/tutorial-acm-create-budgets/monthly-budget-alert.png)

Bir bütçe oluşturduktan sonra, maliyet analizi bölümünde gösterilir. Maliyetinizi harcama eğilimi ile bağlantılı olarak görüntülemek, [maliyetleri ve harcamalarınızı çözümlemeye](quick-acm-cost-analysis.md)başladığınızda ilk adımlardan biridir.

![Maliyet analizinde gösterilen örnek bütçe ve harcama](./media/tutorial-acm-create-budgets/cost-analysis.png)

Yukarıdaki örnekte, abonelik için bir bütçe oluşturdunuz. Ancak, bir kaynak grubu için bir bütçe de oluşturabilirsiniz. Bir kaynak grubu için bir bütçe oluşturmak istiyorsanız, **maliyet yönetimi + faturalandırma** &gt; **abonelikler** ' e gidin &gt; bir abonelik > **kaynak grupları** seçin > Kaynak Grubu > **bütçeleri** seçin > ve ardından Bütçe **ekleyin** .

## <a name="trigger-an-action-group"></a>Bir eylem grubu tetikleyin

Bir abonelik veya kaynak grubu kapsamı için bir bütçe oluşturduğunuzda ya da düzenlediğinizde, bir eylem grubu çağırmak için yapılandırabilirsiniz. Bütçe eşiğiniz karşılandığında eylem grubu çeşitli farklı eylemler gerçekleştirebilir. Eylem grupları hakkında daha fazla bilgi için [Azure Portal eylem grupları oluşturma ve yönetme](../azure-monitor/platform/action-groups.md)konusuna bakın. İşlem gruplarıyla bütçe tabanlı Otomasyon kullanma hakkında daha fazla bilgi için bkz. [Azure bütçeleriyle maliyetleri yönetme](../billing/billing-cost-management-budget-scenario.md).

Eylem grupları oluşturmak veya güncelleştirmek için, bir bütçe oluştururken veya düzenlenirken **eylem gruplarını yönet** ' e tıklayın.

![Eylem gruplarını yönetme göstermek için bir bütçe oluşturma örneği](./media/tutorial-acm-create-budgets/manage-action-groups01.png)


Sonra, **eylem grubu Ekle** ' ye tıklayın ve eylem grubunu oluşturun.


![Eylem Ekle Grup kutusunun görüntüsü](./media/tutorial-acm-create-budgets/manage-action-groups02.png)

Eylem grubu oluşturulduktan sonra, bütçenize geri dönmek için kutuyu kapatın.

Tek bir eşik karşılandığında, bütçenizi eylem grubunuzu kullanacak şekilde yapılandırın. En fazla beş farklı eşik desteklenir.

![Uyarı koşulunun eylem grubu seçimini gösteren örnek](./media/tutorial-acm-create-budgets/manage-action-groups03.png)

Aşağıdaki örnek, %50, 75% ve %100 olarak ayarlanan bütçe eşiklerini gösterir. Her biri belirlenen eylem grubu içinde belirtilen eylemleri tetiklemek üzere yapılandırılır.

![Çeşitli eylem gruplarıyla ve eylem türüyle yapılandırılan uyarı koşullarını gösteren örnek](./media/tutorial-acm-create-budgets/manage-action-groups04.png)

Eylem gruplarıyla bütçe tümleştirmesi yalnızca ortak uyarı şeması devre dışı olan eylem grupları için geçerlidir. Şemayı devre dışı bırakma hakkında daha fazla bilgi için, bkz [. nasıl yaparım? ortak uyarı şemasını etkinleştirmek istiyor musunuz?](../azure-monitor/platform/alerts-common-schema.md#how-do-i-enable-the-common-alert-schema)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure portal bir bütçe oluşturun
> * Bir bütçeyi düzenleme

Maliyet yönetimi verileriniz için yinelenen bir dışarı aktarma oluşturmak üzere sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Aktarılmış verileri oluşturma ve yönetme](tutorial-export-acm-data.md)
