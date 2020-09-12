---
title: Maliyetleri planlama ve yönetme
titleSuffix: Azure Machine Learning
description: Azure portal maliyet analiziyle Azure Machine Learning maliyetlerini planlayın ve yönetin. Makine öğrenimi modelleri oluştururken, maliyetinizi düşürmek için daha fazla maliyet tasarrufu ipuçları edinin.
author: sdgilley
ms.author: sgilley
ms.custom: subject-cost-optimization
ms.reviewer: nigup
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 05/08/2020
ms.openlocfilehash: f20abffa429add8edcbffba16529c467cb9506fe
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89651105"
---
# <a name="plan-and-manage-costs-for-azure-machine-learning"></a>Azure Machine Learning maliyetlerini planlayın ve yönetin

Bu makalede Azure Machine Learning maliyetlerinin nasıl planlanacağı ve yönetileceği açıklanmaktadır. İlk olarak, herhangi bir kaynak eklemeden önce maliyetleri planlamaya yardımcı olması için Azure Fiyatlandırma hesaplayıcısı ' nı kullanın. Daha sonra, Azure kaynaklarını eklerken Tahmini maliyetleri gözden geçirin. Son olarak, modelinize yönetilen Azure Machine Learning işlem kümeleriyle eğitede maliyet tasarrufu ipuçları kullanın.

Azure Machine Learning kaynaklarını kullanmaya başladıktan sonra, bütçe ayarlamak ve maliyetleri izlemek için maliyet yönetimi özelliklerini kullanın. Ayrıca tahmin edilen maliyetleri gözden geçirin ve hareket etmek isteyebileceğiniz bölgeleri belirlemek için harcama eğilimlerini belirler.

Azure Machine Learning maliyetlerinin yalnızca Azure faturanızda bulunan Aylık maliyetlerin yalnızca bir kısmı olduğunu anlayın. Diğer Azure hizmetlerini kullanıyorsanız, üçüncü taraf hizmetler de dahil olmak üzere Azure aboneliğinizde kullanılan tüm Azure hizmetleri ve kaynakları için faturalandırılırsınız. Bu makalede, Azure Machine Learning maliyetlerinin nasıl planlanacağı ve yönetileceği açıklanmaktadır. Azure Machine Learning maliyetlerini yönetme hakkında bilgi sahibi olduktan sonra, aboneliğinizde kullanılan tüm Azure hizmetlerinin maliyetlerini yönetmek için benzer yöntemler uygulayın.

Makine öğrenimi modellerinizi eğitedığınızda, daha fazla maliyet tasarrufu ipuçlarından yararlanmak için yönetilen Azure Machine Learning işlem kümelerini kullanın:

* Otomatik ölçeklendirme için eğitim kümelerinizi yapılandırma
* Aboneliğinizde ve çalışma alanlarınızda kotalar ayarlama
* Eğitim çalıştırmada sonlandırma ilkelerini ayarlama
* Düşük öncelikli sanal makineler (VM) kullanma
* Azure ayrılmış VM örneği kullanma

## <a name="prerequisites"></a>Ön koşullar

Maliyet analizi, farklı türdeki Azure hesaplarını destekler. Desteklenen hesap türlerinin tam listesini görüntülemek için, bkz. [Maliyet Yönetimi verilerini anlama](../cost-management-billing/costs/understand-cost-mgt-data.md). Maliyet verilerini görüntülemek için Azure hesabınızda en azından okuma yetkisine sahip olmanız gerekir. 

Azure Maliyet Yönetimi verilerine erişim atama hakkında daha fazla bilgi için bkz. [Verilere erişim atama](../cost-management-billing/costs/assign-access-acm-data.md).

## <a name="estimate-costs"></a>Maliyetleri tahmin etme

Azure Machine Learning hesapta kaynakları oluşturmadan önce maliyetleri tahmin etmek için [Azure Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) ' nı kullanın. Sol tarafta **AI + Machine Learning**ve ardından başlamak için **Azure Machine Learning** öğesini seçin.  

Aşağıdaki ekran, hesaplayıcı kullanılarak maliyet tahmini gösterir:

:::image type="content" source="media/concept-plan-manage-cost/capacity-calculator-cost-estimate.png" alt-text="Azure Hesaplayıcı 'da maliyet tahmini":::

Çalışma alanınıza yeni kaynaklar eklerken bu Hesaplayıcı 'a dönün ve maliyet tahminlerinizi güncelleştirmek için aynı kaynağı buraya ekleyin.

Enterprise Edition önizlemedeyken, ML ek maliyeti yoktur. Enterprise sürümü genel kullanıma sunulduğunda, bir ek ücret (eğitim ve ınsede yakılmaya yönelik) olacaktır.  Daha fazla bilgi için bkz. [Azure Machine Learning fiyatlandırması](https://azure.microsoft.com/pricing/details/machine-learning/).

## <a name="get-cost-alerts"></a>Maliyet uyarılarını al

Maliyetleri yönetmek ve harcama bozuklukları ve fazla harcama riskleri konusunda otomatik olarak haber veren [Uyarılar](../cost-management/cost-mgt-alerts-monitor-usage-spending.md) oluşturmak için [bütçeler](../cost-management/tutorial-acm-create-budgets.md) oluşturun. Uyarılar, bütçe ve maliyet eşiklerine kıyasla harcamaya göre belirlenir. Bütçeler ve uyarılar Azure abonelikleri ve kaynak grupları için oluşturulur, bu nedenle genel maliyet izleme stratejisinin bir parçası olarak faydalıdır. Ancak, bütçelerin ve uyarıların maliyetleri daha yüksek bir düzeyde izlemek üzere tasarlandıklarından, bireysel Azure hizmeti maliyetlerini yönetmek için sınırlı işlevlere sahip olabilir.

## <a name="monitor-costs"></a>Maliyetleri izleme

Azure Machine Learning ile kaynakları kullanırken maliyetleriniz vardır. Azure Kaynak kullanımı birim maliyetleri, zaman aralıklarına (saniye, dakika, saat ve gün) veya istek birimi kullanımına göre değişir. Azure Machine Learning kullanımı başladığı anda, maliyetler tahakkuk edilir. Bu maliyetleri Azure portal [Maliyet Analizi](../cost-management/quick-acm-cost-analysis.md) bölmesinde görüntüleyin.

Farklı zaman aralıkları için grafikler ve tablolardaki maliyetleri görüntüleyebilirsiniz. Ayrıca bütçeleri ve tahmin edilen maliyetlerden de maliyet görüntüleyebilirsiniz. Zamana göre daha uzun görünümlere geçiş yapmak, harcama eğilimlerini belirlemenize ve fazla harcama oluşmuş olabileceğini görmenizi sağlar. Bütçeleri oluşturduysanız nerede aşıldığını görün.  

Machine Learning için ayrı bir hizmet alanı görmezsiniz.  Bunun yerine Machine Learning çalışma alanlarınızda eklediğiniz çeşitli kaynakları görürsünüz.

## <a name="use-azure-machine-learning-compute-cluster-amlcompute"></a>Azure Machine Learning işlem kümesi (AmlCompute) kullanın

Sürekli değişen veriler sayesinde, doğru modelleri sürdürmek için hızlı ve kolaylaştırılmış model eğitimi ve yeniden eğitimlere ihtiyacınız vardır. Ancak, özellikle GPU 'Larda derin öğrenme modelleri için sürekli eğitim bir maliyetle gelir. 

Azure Machine Learning kullanıcılar, AmlCompute olarak da adlandırılan yönetilen Azure Machine Learning işlem kümesini kullanabilir. AmlCompute, çeşitli GPU ve CPU seçeneklerini destekler. AmlCompute, Azure Machine Learning tarafından aboneliğiniz adına dahili olarak barındırılır. Azure IaaS bulut ölçeğinde aynı kurumsal sınıf güvenlik, uyumluluk ve idare sağlar.

Bu işlem havuzları Azure 'un IaaS altyapısının içinde olduğundan, altyapınızın geri kalanı ile aynı güvenlik ve uyumluluk gereksinimleriyle öğreticinizi dağıtabilir, ölçeklendirebilir ve yönetebilirsiniz.  Bu dağıtımlar aboneliğinizde oluşur ve idare kurallarınızı uyar. [Azure Machine Learning işlem](how-to-create-attach-compute-sdk.md#amlcompute)hakkında daha fazla bilgi edinin.

## <a name="configure-training-clusters-for-autoscaling"></a>Otomatik ölçeklendirme için eğitim kümelerini yapılandırma

İş yükünüzün gereksinimlerine göre otomatik ölçeklendirme kümeleri, yalnızca ihtiyacınız olanları kullanabilmeniz için maliyetlerinizi azaltmaya yardımcı olur.

AmlCompute kümeleri, iş yükünüze göre dinamik olarak ölçeklendirilecek şekilde tasarlanmıştır. Küme, yapılandırdığınız en fazla düğüm sayısına göre ölçeklendirilebilir. Her çalıştırma tamamlandığında, küme düğümleri serbest bırakabilir ve yapılandırılmış en düşük düğüm sayımına ölçeklendirecektir.

[!INCLUDE [min-nodes-note](../../includes/machine-learning-min-nodes.md)]

Ayrıca, düğümlerin ölçeğini ölçeklendirmadan önce boşta kaldığı süreyi de yapılandırabilirsiniz. Varsayılan olarak, ölçek azaltma öncesindeki boşta geçen süre 120 saniyeye ayarlanır.

+ Daha az yinelemeli deneme gerçekleştirirseniz, maliyetleri kaydetmek için bu süreyi azaltın.
+ Son derece yinelemeli geliştirme/test deneme gerçekleştirirseniz, eğitim betiğinizdeki veya ortamınızdaki her değişiklikten sonra sabit ölçekleme için ödeme yapmak zorunda kalmaması için süreyi artırmanız gerekebilir.

AmlCompute kümeleri, amlcompute [SDK sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py&preserve-view=true)olan AMLCOMPUTE [CLI](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/create?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-create-amlcompute)ve [REST API 'leri](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/machinelearningservices/resource-manager/Microsoft.MachineLearningServices/stable)ile birlikte Azure Portal değişen iş yükü gereksinimleriniz için yapılandırılabilir.

```azurecli
az ml computetarget create amlcompute --name testcluster --vm-size Standard_NC6 --min-nodes 0 --max-nodes 5 --idle-seconds-before-scaledown 300
```

## <a name="set-quotas-on-resources"></a>Kaynaklarda kota ayarlama

AmlCompute bir [Kota (veya limit) yapılandırmasıyla](how-to-manage-quotas.md#azure-machine-learning-compute)gelir. Bu kota, VM ailesine göre (örneğin, Dv2 serisi, NCv3 serisi) ve her abonelik için bölgeye göre değişir. Abonelikler, sizin için küçük varsayılanlar ile başlar, ancak aboneliğinizde kullanılacak Amlcompute kaynaklarının miktarını denetlemek için bu ayarı kullanın. 

Ayrıca, bir abonelik içindeki her çalışma alanı için, [VM ailesine göre çalışma alanı düzeyi kotasını](how-to-manage-quotas.md#workspace-level-quota)yapılandırın. Bunun yapılması, her bir çalışma alanının potansiyel olarak olası ve belirli VM ailelerini kısıtlayabileceği maliyetler üzerinde daha ayrıntılı denetime sahip olmasını sağlar. 

Çalışma alanı düzeyinde kotalar ayarlamak için [Azure Portal](https://portal.azure.com)başlatın.  Aboneliğinizdeki herhangi bir çalışma alanını seçin ve sol bölmedeki **kullanımlar + kotalar** ' ı seçin. Sonra kotaları görüntülemek için **kotaları Yapılandır** sekmesini seçin. Birden çok çalışma alanını etkileyen bir ayar olduğundan kotayı ayarlamak için abonelik kapsamında ayrıcalıklarınız olması gerekir.

## <a name="set-run-autotermination-policies"></a>Çalıştırma oto sonlandırma ilkelerini ayarla 

Bazı durumlarda, eğitim çalıştırmalarını süreleri sınırlamak veya erkenden sona erdirmek için yapılandırmanız gerekir. Örneğin, Azure Machine Learning yerleşik hiper parametre ayarlama veya otomatik makine öğrenimi kullanırken.

İşte kullanabileceğiniz birkaç seçenek:
* `max_run_duration_seconds`Bir çalıştırmanın seçtiğiniz işlem için (yerel veya uzak bulut işlem) üzerinde genişletileceği en uzun süreyi denetlemek Için RunConfiguration uygulamanızda çağrılan bir parametre tanımlayın.
* [Hiper parametre ayarlama](how-to-tune-hyperparameters.md#early-termination)için, bir bandıt ilkesinden erken sonlandırma Ilkesi, ortanca durdurma Ilkesi veya kesme seçim ilkesi tanımlayın. Hiper parametre sweeps 'yi daha fazla denetlemek için veya gibi parametreleri `max_total_runs` kullanın `max_duration_minutes` .
* [Otomatik makine öğrenimi](how-to-configure-auto-train.md#exit)için bayrağını kullanarak benzer sonlandırma ilkeleri ayarlayın `enable_early_stopping` . Ayrıca, `iteration_timeout_minutes` ve `experiment_timeout_minutes` bir çalıştırmanın en uzun süresini denetlemek için veya tüm denemeler için ve gibi özellikleri kullanın.

## <a name="use-low-priority-vms"></a><a id="low-pri-vm"></a> Düşük öncelikli VM 'Ler kullanma

Azure, sanal makine ölçek kümeleri, toplu Iş ve Machine Learning hizmeti arasında düşük öncelikli VM 'Ler olarak fazla unutilized kapasitesini kullanmanıza olanak tanır. Bu ayırmalar ön azaltıldı, ancak adanmış VM 'Lerle karşılaştırıldığında daha düşük bir fiyata gelir. Genel olarak, toplu iş yükleri için düşük öncelikli VM 'Ler kullanmanızı öneririz. Ayrıca, kesintileri resubmits aracılığıyla kurtarılabilir (toplu Iş için geçiş için) veya yeniden başlatmalar aracılığıyla (checksize ile derin öğrenme eğitimi için) de kullanmanız gerekir.

Düşük öncelikli VM 'Ler, VM ailesi ile olan ayrılmış kota değerinden ayrı tek bir kotaya sahiptir. [AmlCompute kotaları hakkında daha fazla](how-to-manage-quotas.md)bilgi edinin.

 Düşük öncelikli VM 'Ler, etkileşimli not defteri deneyimlerini desteklemesi gerektiğinden, işlem örnekleri için çalışmaz.

## <a name="use-reserved-instances"></a>Ayrılmış örnekleri kullanma

İşlem kaynaklarına para kazanmanız için başka bir yöntem de Azure ayrılmış VM örneğidir. Bu teklifle, bir yıllık veya üç yıllık koşullara göre işlemeniz gerekir. Bu indirimler, Kullandıkça Öde fiyatlarının %72 ' ine kadar değişir ve doğrudan aylık Azure faturanızda uygulanır.

Azure Machine Learning Işlem, ayrılmış örnekleri kendiliğinden destekler. Bir yıllık veya üç yıllık ayrılmış örnek satın alırsanız, Azure Machine Learning yönetilen işlem için otomatik olarak indirim uygulayacağız.


## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakiler hakkında daha fazla bilgi edinin:
* [Kaynak kotalarını yönetme ve artırma](how-to-manage-quotas.md)
* [Maliyet Analizi ile maliyetleri yönetme](../cost-management-billing/costs/quick-acm-cost-analysis.md).
* [SDK](how-to-create-attach-compute-sdk.md#amlcompute) ile veya [Studio](how-to-create-attach-compute-studio.md#amlcompute)'da Azure Machine Learning işlem oluşturun.
