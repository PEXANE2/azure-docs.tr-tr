---
title: Azure Sentinel önizlemesinde not defterlerini kullanma özellikleri | Microsoft Docs
description: Bu makalede, Azure Sentinel arama özellikleri ile Not defterlerinin nasıl kullanılacağı açıklanır.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 1721d0da-c91e-4c96-82de-5c7458df566b
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 6372a7958caf108903321e5ee87ea6bf1a42271c
ms.sourcegitcommit: d585cdda2afcf729ed943cfd170b0b361e615fae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68689568"
---
# <a name="use-jupyter-notebooks-to-hunt-for-security-threats"></a>Güvenlik tehditleri aramak için Jupyıter not defterlerini kullanma

> [!IMPORTANT]
> Azure Sentinel Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Sentinel 'in temeli veri deposudur; yüksek performanslı sorgulamayı, dinamik şemayı birleştirir ve çok büyük veri birimlerine ölçeklendirir. Azure Sentinel portalı ve tüm Azure Sentinel araçları, bu veri deposuna erişmek için ortak bir API kullanır. Aynı API, [jupi](https://jupyter.org/) Not defterleri ve Python gibi dış araçlar için de kullanılabilir. Portalda birçok ortak görev gerçekleştirilebilirken, Jupyıter bu verilerle yapabileceklerinizi kapsamını genişletir. Makine öğrenimi, görselleştirme ve veri analizi için çok büyük bir kitaplık koleksiyonuyla tam programlama ile birleşir. Bu öznitelikler, Jupa 'nın güvenlik araştırması ve araştırma için etkileyici bir araç haline getirir.

![Örnek Not defteri](./media/notebooks/sentinel-notebooks-map.png)

Jupi deneyimini Azure Sentinel portalına tümleştirdik. Bu, verilerinizi analiz etmek için not defterlerini oluşturup yürütmelerini kolaylaştırır. *Ktomagic* kitaplığı, Azure Sentinel 'ten sorgular almanıza ve bunları doğrudan bir not defteri içinde çalıştırmanıza olanak tanıyan tutkalla 'yi sağlar. Sorgular [kusto sorgu dilini](https://kusto.azurewebsites.net/docs/query/index.html)kullanır. Microsoft 'un Güvenlik analistleri tarafından geliştirilen birkaç not defteri, Azure Sentinel ile paketlenmiştir. Bu not defterlerinden bazıları belirli bir senaryo için oluşturulmuştur ve olduğu gibi kullanılabilir. Diğerleri, kendi Not defterlerinizde kullanmak üzere kopyalayabileceğiniz veya bunları uyarlayabileceğiniz teknikleri ve özellikleri göstermek için örnek olarak tasarlanmıştır. Diğer Not defterleri de Azure Sentinel Community GitHub 'dan içeri aktarılabilir.

Tümleşik Jupyıter deneyimi, not defterlerini depolamak, paylaşmak ve yürütmek için [Azure Notebooks](https://notebooks.azure.com/) kullanır. Bu not defterlerini yerel olarak da (bilgisayarınızda bir Python ortamınız ve jupi 'niz varsa) veya Azure Databricks gibi diğer JupterHub ortamlarında de çalıştırabilirsiniz.

Not defterlerinin iki bileşeni vardır:

- sorgu ve kod girip çalıştırdığınız ve yürütme sonuçlarının görüntülendiği tarayıcı tabanlı arabirim.
- kodu ayrıştırmaktan ve yürütmeden sorumlu bir *çekirdek* . 

Azure Notebooks, bu çekirdek, varsayılan olarak Azure *ücretsiz bulut işlem ve depolama* üzerinde çalışır. Not defterleriniz karmaşık makine öğrenimi modelleri veya görselleştirmeler içeriyorsa, [veri bilimi sanal makineleri](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/) (dsvm) gibi daha güçlü, ayrılmış işlem kaynakları kullanmayı göz önünde bulundurmanız gerekir. Hesabınızı paylaşmayı seçmediğiniz takdirde, hesabınızdaki Not defterleri özel tutulur.

Azure Sentinel Not defterleri, Pandas, Matplotlib, bokeh ve diğerleri gibi birçok popüler Python kitaplıklarını kullanır. Arasından seçim yapabileceğiniz çok sayıda başka Python paketi vardır:

- görselleştirmeler ve grafikler
- veri işleme ve analiz
- istatistikler ve sayısal bilgi işlem
- Machine Learning ve derin öğrenme

Ayrıca, [msticter](https://github.com/Microsoft/msticpy/)adlı bir pakette bazı açık kaynaklı jupi güvenlik araçları da yayımladık. Bu paket, dahil edilen birçok Not defteri içinde kullanılır. Msticticizme araçları, araştırma ve araştırma için Not defteri oluşturma konusunda yardımcı olmak üzere tasarlanmıştır ve yeni özellikler ve geliştirmeler üzerinde etkin bir şekilde çalışıyoruz.

İlk not defterleri şunları içerir:

- **Kılavuzlu araştırma-Işlem uyarıları**: Etkilenen ana bilgisayarlarda etkinlikleri çözümleyerek uyarıları hızlı bir şekilde önceliklendirmenize olanak tanır.
- **Kılavuzlu-Windows ana bilgisayar Gezgini**: Bir konaktaki hesap etkinliğini, işlem yürütmelerini, ağ etkinliğini ve diğer olayları araştırmanıza olanak sağlar.  
- **Kılavuzlu arama-Office365-araştırma**: Birden çok O365 veri kümesindeki şüpheli Office 365 etkinliği için Hunt.

[Azure Sentinel Community GitHub deposu](https://github.com/Azure/Azure-Sentinel) , Microsoft tarafından oluşturulan veya topluluk tarafından katkıda bulunulan, gelecek Azure Sentinel Not defterlerinin konumudur.

## <a name="run-a-notebook"></a>Not defteri çalıştırma

Aşağıdaki örnekte, Azure Sentinel portalından projeyi Not defterleri ile doldurarak bir Azure Notebooks projesi oluşturacağız. Bu not defterlerini kullanmadan önce, Not defterinin bir kopyasını oluşturmak ve kopyada çalışmak iyi bir fikirdir. Kopyaların üzerinde çalışma, verilerinizin üzerine yazmadan, daha sonraki Not defteri sürümlerine güvenle güncelleştirme yapmanızı sağlar.

1. Azure Sentinel portalında gezinti menüsündeki **Not defterleri** ' ne tıklayın. Yeni bir Azure Notebooks projesi oluşturmak için **Azure Sentinel not defterlerini Kopyala** ' ya tıklayın veya mevcut not defteri projelerinizi açmak Için **not defterlerinize git**' e tıklayın.
  
   ![not defterlerini seçin](./media/notebooks/sentinel-azure-notebooks-home.png)

2. Önceki adımda **Azure Sentinel dizüstü bilgisayarları Kopyala** ' yı seçtiyseniz aşağıdaki iletişim kutusu görüntülenir. GitHub deposunu Azure Notebooks projenize kopyalamak için **Içeri aktar** ' a tıklayın. Mevcut bir Azure Notebooks hesabınız yoksa bir tane oluşturmanız ve oturum açmanız istenir.

   ![Not Defteri al](./media/notebooks/sentinel-notebooks-clone.png)

3. Yeni bir proje oluştururken projeyi yazmanız gerekir-varsayılan adı kullanın veya yeni bir tane yazın. **Kopya yinelemeli olarak Kopyala** seçeneğini denetmeyin-Bu seçenek, bağlantılı GitHub depoları anlamına gelir. **Içeri aktarmaya** tıklamak, GitHub içeriğini kopyalamaya başlar, bu da tamamlanması birkaç dakika sürebilir.

   ![Not Defteri al](./media/notebooks/sentinel-create-project.png)

4. Not defterlerini görmek için **Not defterleri** klasörünü açın. Her not defteri, bir Hunt veya araştırma gerçekleştirme adımlarında size yol gösterir. Kitaplıkları ve Not defteri için gereken diğer bağımlılıklar, Not defterinin kendisinden veya basit bir yapılandırma yordamıyla yüklenebilir. Not defteri projenizi Azure Sentinel aboneliğinize geri bağlayan yapılandırma, önceki adımlarda otomatik olarak sağlanır. Not defterleriniz Azure Sentinel Log Analytics çalışma alanınızda çalışmaya hazırız.

   ![Depoyu içeri aktar](./media/notebooks/sentinel-open-notebook1.png)

5. Bir not defteri açın. Not defterlerini çalıştırmak için ücretsiz Işlem varsayılan olarak seçilidir (vurgulanmış). Kullanmak üzere bir DSVM yapılandırdıysanız (yukarıya bakın), ilk not defterini açmadan önce DSVM 'yi seçin ve kimlik doğrulaması yapın. Bir not defterine tıklayarak açın.

   ![Not defteri seçin](./media/notebooks/sentinel-open-notebook2.png)

6. Python sürümü seçiliyor. Bir not defterini ilk açışınızda, bir çekirdek sürümü seçmenizi isteyebilir. Aksi takdirde, aşağıdaki gibi kullanılacak çekirdeği seçin. Python 3,6 veya üzeri, seçili çekirdek olmalıdır (Not defteri penceresinin sağ üst kısmında).

   ![Not defteri seçin](./media/notebooks/sentinel-select-kernel.png)

Azure Sentinel 'de verileri sorgulamaya yönelik hızlı bir giriş için ana not defterleri klasöründeki [getstarted](https://github.com/Azure/Azure-Sentinel/blob/master/Notebooks/Get%20Started.ipynb) not defterine bakın. **Örnek Not defterleri** alt klasöründe ek örnek Not defterleri bulunabilir. Örnek Not defterleri verilerle birlikte kaydedilir, böylece amaçlanan çıktıyı görmeniz daha kolay olur (bunları [nbviewer](https://nbviewer.jupyter.org/)'da görüntülemeniz önerilir). **HOWTOs** klasörü, örnek olarak açıklandığı gibi not defterlerini içerir, örneğin: varsayılan Python sürümünüzü ayarlama, dsvm yapılandırma, bir not defterinden Azure Sentinel yer işaretlerini oluşturma ve diğer konular.

Bu not defterleri hem faydalı araçlar, hem de kendi Not defterlerinizin geliştirilmesi için kullanabileceğiniz çizimler ve kod örnekleri gibi tasarlanmıştır.

Öneriler, özellikler için istekler, katkıda bulunulan Not defterleri, hata raporları ya da geliştirmeler ve mevcut not defterlerine ekler hakkında geri bildirimde bulunun. Bir sorun veya çatal oluşturmak için [Azure Sentinel Community GitHub](https://github.com/Azure/Azure-Sentinel) ' a gidin ve bir katkıyı karşıya yükleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Sentinel 'de Jupyıter not defterlerini kullanmaya nasıl başlacağınız hakkında daha fazla öğrenirsiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Tehditler için proaktif araya](hunting.md)
- [Avlarken ilginç bilgileri kaydetmek için yer işaretlerini kullanın](bookmarks.md)
