---
title: Öğretici-Azure maliyet yönetimi 'nden aktarılmış verileri oluşturma ve yönetme
description: Bu makalede, dış sistemlerde kullanabilmeniz için, dışarıya aktarılmış Azure maliyet yönetimi verilerini nasıl oluşturabileceğiniz ve yönetebileceğiniz gösterilmektedir.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 01/14/2020
ms.topic: tutorial
ms.service: cost-management-billing
manager: jasonh
ms.custom: seodec18
ms.openlocfilehash: 76ee5aba0f1d0769e15a5969409dfef2a018e477
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75987288"
---
# <a name="tutorial-create-and-manage-exported-data"></a>Öğretici: Dışarı aktarılan verileri oluşturma ve yönetme

Maliyet Analizi öğreticisini okuduysanız, Maliyet Yönetimi verilerinizi el ile indirme konusunda bilgi sahibisiniz. Ancak, günlük, haftalık veya aylık olarak Azure depolama için maliyet Yönetimi verilerinizi otomatik olarak dışarı aktarır, yinelenen bir görev oluşturun. Dışarı aktarılan veriler CSV biçimindedir ve Maliyet Yönetimi tarafından toplanan tüm bilgileri içerir. Daha sonra Azure depolama alanındaki dışarı aktarılan verileri dış sistemlerle kullanabilir ve kendi özel verilerinizle birleştirebilirsiniz. Ayrıca dışarı aktarılan verilerinizi pano gibi bir dış sistemde veya diğer mali sistemde kullanabilirsiniz.

Azure ücretlerinizi Azure depolama 'ya zamanlanan bir dışarı aktarma hakkında bilgi almak için [Azure maliyet yönetimi ile depolama 'ya dışarı aktarma zamanlamasını](https://www.youtube.com/watch?v=rWa_xI1aRzo) izleyin.

Bu öğreticideki örnek, maliyet yönetimi verilerinizi dışarı aktarmada ve ardından verilerin başarılı bir şekilde dışarı aktarıldığını doğrulamada size yol gösterir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Günlük bir dışarı aktarma oluşturma
> * Verilerin toplandığını doğrulama

## <a name="prerequisites"></a>Ön koşullar
Veri dışa aktarma [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) ve [Microsoft Müşteri Sözleşmesi](get-started-partners.md) müşterileri de dahil olmak üzere çeşitli Azure hesap türleri için kullanılabilir. Desteklenen hesap türlerinin tam listesini görüntülemek için bkz. [maliyet yönetimi verilerini anlama](understand-cost-mgt-data.md). Aşağıdaki Azure izinleri veya kapsamları, Kullanıcı ve grup tarafından verilerin dışarı aktarılması için abonelik başına desteklenir. Kapsamlar hakkında daha fazla bilgi için bkz. [kapsamları anlama ve bunlarla çalışma](understand-work-scopes.md).

- Sahip – Bir abonelik için zamanlanan dışarı aktarmaları oluşturabilir, değiştirebilir veya silebilir.
- Katkıda bulunan – Kendi zamanlanan dışarı aktarmalarını oluşturabilir, değiştirebilir veya silebilir. Başkaları tarafından oluşturulan zamanlanmış dışarı aktarmaların adını değiştirebilir.
- Okuyucu – İzni oldukları dışarı aktarmaları zamanlayabilir.

Azure Depolama hesapları için:
- Yazma izinlerinin, dışarı aktarma üzerindeki izinlerden bağımsız olarak yapılandırılmış depolama hesabını değiştirmesi gerekir.
- Azure depolama hesabınızın blob veya dosya depolama için yapılandırılmış olması gerekir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açın
[https://portal.azure.com](https://portal.azure.com/) adresinden Azure portalında oturum açın.

## <a name="create-a-daily-export"></a>Günlük bir dışarı aktarma oluşturma

Veri dışa aktarma oluşturmak veya görüntülemek veya bir dışarı aktarma zamanlamak için, Azure portal istenen kapsamı açın ve menüden **Maliyet Analizi** ' ni seçin. Örneğin, **abonelikler**' e gidin, listeden bir abonelik seçin ve ardından menüde **Maliyet Analizi** ' ni seçin. Maliyet analizi sayfasının en üstünde **Dışarı Aktar**'ı seçin ve sonra da dışarı aktarma seçeneğini belirtin. Örneğin, **zamanlamayı zamanla**' yı seçin.  

> [!NOTE]
> - Aboneliklerin yanı sıra kaynak grupları, hesaplar, departmanlar ve kayıtlar üzerinde de dışarı aktarma işlemleri oluşturabilirsiniz. Kapsamlar hakkında daha fazla bilgi için bkz. [kapsamları anlama ve bunlarla çalışma](understand-work-scopes.md).
>- Faturalandırma hesabı kapsamında veya bir müşterinin kiracısında bir iş ortağı olarak oturum açtığınızda, iş ortağı depolama hesabınıza bağlı bir Azure depolama hesabına veri aktarabilirsiniz. Ancak, CSP kiracınızda etkin bir aboneliğiniz olması gerekir.
>


**Ekle**' yi seçin, dışarı aktarma için bir ad yazın ve ardından **aylık maliyetler ' in günlük maliyetlerini** seçin seçeneğini belirleyin. **İleri**’yi seçin.

![Yeni dışarı aktarma örnek dışarı aktarma türü gösteriliyor](./media/tutorial-export-acm-data/basics_exports.png)

Azure depolama hesabınız için bir abonelik belirtin, ardından depolama hesabınızı seçin.  Depolama kapsayıcısı ve dışarı aktarma dosyası gitmek istediğiniz dizin yolunu belirtin. **İleri**’yi seçin.

![Depolama hesabı ayrıntılarını gösteren yeni dışarı aktarma örneği](./media/tutorial-export-acm-data/storage_exports.png)

Dışarı aktarma ayrıntılarınızı gözden geçirin ve **Oluştur**'u seçin.

Yeni dışarı aktarmanız, dışarı aktarma listesinde görünür. Varsayılan olarak, yeni dışarı aktarmaları etkinleştirilir. Zamanlanmış bir dışarı aktarmayı devre dışı bırakmak veya silmek istiyorsanız, listedeki herhangi bir öğeyi seçin ve sonra **devre dışı bırak** veya **Sil**' i seçin.

Başlangıçta, dışarı aktarmanın çalışmaya başlaması bir ila iki saat arası sürebilir. Ancak, verilerin dışarı aktarılan dosyalarda gösterilmesi en fazla dört saat sürebilir.

### <a name="export-schedule"></a>Dışarı aktarma zamanlaması

Zamanlanmış dışarı aktarmalar, başlangıçta dışarı aktarmayı başlattığınız saatten ve haftanın gününden etkilenir. Zamanlanan bir dışarı aktarma oluşturduğunuzda dışarı aktarma işlemi, sonraki her dışarı aktarma oluşumu için aynı sıklıkta çalışır. Örneğin, günlük bir sıklıkta bir aylık bir dışarı aktarma kümesi için dışarı aktarma günlük olarak çalıştırılır. Bir haftalık dışarı aktarma için benzer şekilde, dışarı aktarma işlemi her hafta zamanlandığı şekilde aynı günde çalışır. Dışarı aktarmanın tam teslim süresi garanti edilmez ve dışarı aktarılmış veriler, dört saatlik çalışma süresi içinde kullanılabilir. "
Her dışarı aktarma işlemi yeni bir dosya oluşturur, dolayısıyla daha eski dışarı aktarmaların üzerine yazılmaz.

İki tür dışa aktarma seçeneği vardır:

**Aylık maliyetler Için günlük dışa aktarma** : ilk dışa aktarma hemen çalışır. İzleyen dışarı aktarmalar ertesi gün ilk dışarı aktarmayla aynı saatte çalıştırılır. En son veriler önceki günlük dışarı aktarmalardan toplanır.

**Özel** – haftalık ve aylık dışarı aktarmaları, hafta-tarih ve ay-tarih seçenekleriyle zamanlamanıza izin verir. *İlk dışarı aktarma hemen çalıştırılır.*

Kullandıkça Öde, MSDN veya Visual Studio aboneliğiniz varsa faturalama döneminiz takvim ayıyla uyumlu olmayabilir. Bu tür aboneliklerde ve kaynak gruplarında, fatura döneminizle veya takvim aylarıyla uyumlu bir dışarı aktarma oluşturabilirsiniz. Faturanızda bir dışa aktarma oluşturmak için, **özel**' e gidin ve **faturalandırma dönemi**' ni seçin.  Takvim aya hizalanmış bir dışarı aktarma oluşturmak için, **ay-tarih**' i seçin.
>
>

![Yeni dışarı aktarma-özel haftalık haftalık seçimi gösteren temel bilgiler sekmesi](./media/tutorial-export-acm-data/tutorial-export-schedule-weekly-week-to-date.png)

## <a name="verify-that-data-is-collected"></a>Verilerin toplandığını doğrulama

Maliyet Yönetimi verilerinizin toplandığını kolaylıkla doğrulayabilir ve Azure Depolama Gezgini’ni kullanarak dışarı aktarılan CSV dosyasını görüntüleyebilirsiniz.

Dışarı aktarma listesinde depolama hesabı adını seçin. Depolama hesabı sayfasında, Explorer 'da aç ' ı seçin. Bir onay kutusu görürseniz, dosyayı Azure Depolama Gezgini açmak için **Evet** ' i seçin.

![Depolama hesabı sayfasında örnek bilgileri ve bağlantı Gezgini'nde Aç](./media/tutorial-export-acm-data/storage-account-page.png)

Depolama Gezgini'nde, açmak istediğiniz kapsayıcıya gidin ve bulunduğunuz aya karşılık gelen klasörü seçin. CSV dosyaları listesi gösterilir. Birini seçip **Aç**' ı seçin.

![Depolama Gezgini'nde gösterilen örnek bilgileri](./media/tutorial-export-acm-data/storage-explorer.png)

Dosya, CSV dosyası uzantılarını açmak üzere ayarlanmış program veya uygulama ile açılır. Bir örneği Excel’de verilmiştir.

![CSV Verileri Excel'de gösterilen örnek dışarı](./media/tutorial-export-acm-data/example-export-data.png)


## <a name="access-exported-data-from-other-systems"></a>Dışarı aktarılan verilere diğer sistemlerden erişme

Maliyet Yönetimi verilerinizi dışarı aktarma amaçlarından biri, verilere dış sistemlerden erişmektir. Bir pano sistemi veya diğer finansal sistemi kullanabilirsiniz. Bu sistemler büyük ölçüde farklılık gösterdiğinden bir örnek vermek uygun olmaz.  Bununla birlikte, [Azure Depolama'ya giriş](../../storage/common/storage-introduction.md) bölümünde uygulamalarınızdan verilerinize erişmeye başlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Günlük bir dışarı aktarma oluşturma
> * Verilerin toplandığını doğrulama

Boşta olan ve az kullanılan kaynakları belirleyerek verimliliği iyileştirmek ve geliştirmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Önerileri gözden geçirme ve gerekli eylemleri gerçekleştirme](tutorial-acm-opt-recommendations.md)
