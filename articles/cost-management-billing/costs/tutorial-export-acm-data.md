---
title: Öğretici - Azure Maliyet Yönetimi'nden dışarı aktarılan verileri oluşturma ve yönetme
description: Bu makalede, dış sistemlerde kullanabilmeniz için dışarı aktarılan Azure Maliyet Yönetimi verilerini nasıl oluşturup yönetebileceğiniz gösterilir.
author: bandersmsft
ms.author: banders
ms.date: 03/24/2020
ms.topic: tutorial
ms.service: cost-management-billing
ms.reviewer: adwise
ms.custom: seodec18
ms.openlocfilehash: b950235eccbd41b1dfa46c23985e804212d9e8f2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80155911"
---
# <a name="tutorial-create-and-manage-exported-data"></a>Öğretici: Dışarı aktarılan verileri oluşturma ve yönetme

Maliyet Analizi öğreticisini okuduysanız, Maliyet Yönetimi verilerinizi el ile indirme konusunda bilgi sahibisiniz. Ancak günlük, haftalık veya aylık düzende otomatik olarak Maliyet Yönetimi verilerinizi Azure depolama alanına aktaran, yinelenen bir görev oluşturabilirsiniz. Dışarı aktarılan veriler CSV biçimindedir ve Maliyet Yönetimi tarafından toplanan tüm bilgileri içerir. Daha sonra Azure depolama alanındaki dışarı aktarılan verileri dış sistemlerle kullanabilir ve kendi özel verilerinizle birleştirebilirsiniz. Ayrıca dışarı aktarılan verilerinizi pano gibi bir dış sistemde veya diğer mali sistemde kullanabilirsiniz.

Azure maliyet verilerinizi Azure Depolama alanına aktarmak için zamanlama oluşturma hakkındaki [Azure Maliyet Yönetimi ile depolama alanı dışarı aktarma işlemlerini zamanlama](https://www.youtube.com/watch?v=rWa_xI1aRzo) videosunu izleyin.

>[!VIDEO https://www.youtube.com/embed/rWa_xI1aRzo]

Bu öğreticideki örnek, maliyet yönetimi verilerinizi dışarı aktarmada ve ardından verilerin başarılı bir şekilde dışarı aktarıldığını doğrulamada size yol gösterir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Günlük bir dışarı aktarma oluşturma
> * Verilerin toplandığını doğrulama

## <a name="prerequisites"></a>Ön koşullar
[Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) ve [Microsoft Müşteri Sözleşmesi](get-started-partners.md) müşterileri dahil olmak üzere birçok farklı Azure hesap türünde veriler dışarı aktarılabilir. Desteklenen hesap türlerinin tam listesini görüntülemek için, bkz. [Maliyet Yönetimi verilerini anlama](understand-cost-mgt-data.md). Aşağıdaki Azure izinleri veya kapsamları abonelik başına kullanıcı ve grup tarafından yapılan veri dışarı aktarımı için desteklenir. Kapsamlar hakkında daha fazla bilgi için bkz. [Kapsamları anlama ve birlikte çalışma](understand-work-scopes.md).

- Sahip – Bir abonelik için zamanlanan dışarı aktarmaları oluşturabilir, değiştirebilir veya silebilir.
- Katkıda bulunan – Kendi zamanlanan dışarı aktarmalarını oluşturabilir, değiştirebilir veya silebilir. Başkaları tarafından oluşturulan zamanlanmış dışarı aktarmaların adını değiştirebilir.
- Okuyucu – İzni oldukları dışarı aktarmaları zamanlayabilir.

Azure Depolama hesapları için:
- Yazma izinlerinin, dışarı aktarma üzerindeki izinlerden bağımsız olarak yapılandırılmış depolama hesabını değiştirmesi gerekir.
- Azure depolama hesabınızın blob veya dosya depolama için yapılandırılmış olması gerekir.

Yeni bir aboneliğiniz varsa Maliyet Yönetimi özelliklerini hemen kullanamazsınız. Maliyet Yönetimi özelliklerini kullanabilmek için 48 saat kadar beklemeniz gerekebilir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
[https://portal.azure.com](https://portal.azure.com/) adresinden Azure portalında oturum açın.

## <a name="create-a-daily-export"></a>Günlük bir dışarı aktarma oluşturma

Dışarı aktarma oluşturmak veya görüntülemek ya da zamanlamak için Azure portalında istediğiniz kapsamı açıp menüden **Maliyet analizi**'ni seçin. Örneğin **Abonelikler**'e gidin, listeden bir abonelik belirleyin ve menüden **Maliyet analizi**'ni seçin. Maliyet Analizi sayfasının en üstünde **Dışarı aktar**'ı seçin ve bir dışarı aktarma seçeneği belirleyin. Örneğin **Dışarı aktarma zamanla**'yı seçin.  

> [!NOTE]
> - Dışarı aktarmayı aboneliklerin yanı sıra, kaynak grupları, hesaplar, departmanlar ve kayıtlar özelinde de oluşturabilirsiniz. Kapsamlar hakkında daha fazla bilgi için bkz. [Kapsamları anlama ve birlikte çalışma](understand-work-scopes.md).
>- Ödeme hesabı kapsamında veya müşteri kiracısında iş ortağı olarak oturum açtığınızda verileri iş ortağı depolama hesabıyla ilişkilendirilmiş olan Azure Depolama hesabına aktarabilirsiniz. Ancak CSP kiracınızda etkin bir aboneliğe sahip olmanız gerekir.
>


**Ekle**'yi seçin, dışarı aktarma için bir ad yazın ve ardından **Ay başından bugüne kadarki maliyetlerin günlük dışarı aktarması**'nı seçin. **İleri**’yi seçin.

![Dışarı aktarma türünü gösteren yeni dışarı aktarma örneği](./media/tutorial-export-acm-data/basics_exports.png)

Azure depolama hesabınıza ait aboneliği belirttikten sonra depolama hesabınızı seçin.  Dışarı aktarılan dosyanın gitmesini istediğiniz depolama kapsayıcısını ve dizin yolunu belirtin. **İleri**’yi seçin.

![Depolama hesabı ayrıntılarını gösteren yeni dışarı aktarma örneği](./media/tutorial-export-acm-data/storage_exports.png)

Dışarı aktarma ayrıntılarını gözden geçirin ve **Oluştur**'u seçin.

Yeni dışarı aktarmanız, dışarı aktarma listesinde görünür. Yeni dışarı aktarmalar varsayılan olarak etkinleştirilir. Zamanlanmış bir dışarı aktarmayı devre dışı bırakmak veya silmek istiyorsanız, listedeki herhangi bir öğeye ve ardından **Devre dışı bırak** veya **Sil**'i seçin.

Başlangıçta, dışarı aktarmanın çalışmaya başlaması bir ila iki saat arası sürebilir. Ancak, verilerin dışarı aktarılan dosyalarda gösterilmesi en fazla dört saat sürebilir.

### <a name="export-schedule"></a>Dışarı aktarma zamanlama

Zamanlanan dışarı aktarmalar, ilk oluşturduğunuz andaki saatten ve haftanın gününden etkilenir. Zamanlanan dışarı aktarma oluşturduğunuzda dışarı aktarma, sonraki her bir çalışma için aynı aralığı korur. Örneğin günlük çalışma sıklığına sahip olan ve ay başından bugüne kadarki aralığı kapsayan dışarı aktarma işlemi günlük olarak çalışır. Benzer şekilde haftalık dışarı aktarma da zamanlandığı gibi her hafta aynı günde çalışır. Dışarı aktarmanın tam teslim süresi garanti edilmez ve dışarı aktarılan veriler dört saatlik çalışma süresi içinde kullanılabilir.
Har dışarı aktarma işlemi yeni bir dosya oluşturur ve eski dışarı aktarmaların üzerinde yazılmaz.

İki tür dışarı aktarma seçeneği vardır:

**Ay başından bugüne kadarki maliyetlerin günlük dışarı aktarması**: İlk dışarı aktarma anında çalışır. Sonraki dışarı aktarmalar ertesi gün ilk dışarı aktarma ile aynı saatte çalışır. Önceki günlük dışarı aktarmalardan alınan en güncel veriler toplanır.

**Özel**: Hafta başından bugüne ve ay başından bugüne seçenekleriyle haftalık ve aylık dışarı aktarma zamanlaması gerçekleştirmenizi sağlar. *İlk dışarı aktarma anında çalışır.*

Kullandıkça Öde, MSDN veya Visual Studio aboneliğiniz varsa faturanızdaki fatura dönemi takvim ayından farklı olabilir. Bu abonelik türleri ve kaynak grupları için fatura döneminize veya takvim aylarına uyan bir dışarı aktarma oluşturabilirsiniz. Fatura ayınıza uygun bir dışarı aktarma oluşturmak için **Özel**'e gidin ve **Faturalama döneminden bugüne**'yi seçin.  Takvim ayına hizalanmış bir dışarı aktarma oluşturmak için **Bugüne kadar aylık** seçeneğini tercih edin.
>
>

![Yeni dışarı aktarma - Özel bugüne kadar haftalık seçeneğini gösteren Temel sekmesi](./media/tutorial-export-acm-data/tutorial-export-schedule-weekly-week-to-date.png)

## <a name="verify-that-data-is-collected"></a>Verilerin toplandığını doğrulama

Maliyet Yönetimi verilerinizin toplandığını kolaylıkla doğrulayabilir ve Azure Depolama Gezgini’ni kullanarak dışarı aktarılan CSV dosyasını görüntüleyebilirsiniz.

Dışarı aktarma listesinde depolama hesabı adını seçin. Depolama hesabı sayfasında Gezgin'de Aç'ı seçin. Bir onay kutusu görürseniz, dosyayı Azure Depolama Gezgini’nde açmak için **Evet**’i seçin.

![Örnek bilgilerini ve Gezgin'de Aç bağlantısını gösteren depolama hesabı sayfası](./media/tutorial-export-acm-data/storage-account-page.png)

Depolama Gezgini'nde, açmak istediğiniz kapsayıcıya gidin ve bulunduğunuz aya karşılık gelen klasörü seçin. CSV dosyaları listesi gösterilir. Birini belirleyin ve **Aç**’ı seçin.

![Depolama Gezgini'nde gösterilen örnek bilgiler](./media/tutorial-export-acm-data/storage-explorer.png)

Dosya, CSV dosyası uzantılarını açmak üzere ayarlanmış program veya uygulama ile açılır. Bir örneği Excel’de verilmiştir.

![Excel'de gösterilen dışarı aktarılmış örnek CSV verileri](./media/tutorial-export-acm-data/example-export-data.png)


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
