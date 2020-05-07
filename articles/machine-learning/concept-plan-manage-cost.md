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
ms.date: 04/22/2020
ms.openlocfilehash: b777e6a45bdfe78889366982a6c28e10e21c4fbc
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82857006"
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

## <a name="estimate-costs"></a>Tahmin maliyetleri

Azure Machine Learning hesapta kaynakları oluşturmadan önce maliyetleri tahmin etmek için [Azure Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) ' nı kullanın. Sol tarafta **AI + Machine Learning**ve ardından başlamak için **Azure Machine Learning** öğesini seçin.  

Aşağıdaki ekran, hesaplayıcı kullanılarak maliyet tahmini gösterir:

:::image type="content" source="media/concept-plan-manage-cost/capacity-calculator-cost-estimate.png" alt-text="Azure Hesaplayıcı 'da maliyet tahmini":::

Çalışma alanınıza yeni kaynaklar eklerken bu Hesaplayıcı 'a dönün ve maliyet tahminlerinizi güncelleştirmek için aynı kaynağı buraya ekleyin.

Enterprise Edition önizlemedeyken, ML ek maliyeti yoktur. Enterprise sürümü genel kullanıma sunulduğunda, makine öğrenimi ek maliyeti olur (eğitim ve ındyatıon için).  Daha ayrıntılı bilgi için [Azure Machine Learning fiyatlandırması](https://azure.microsoft.com/pricing/details/machine-learning/).

## <a name="get-cost-alerts"></a>Maliyet uyarılarını al

Maliyetleri yönetmek ve harcama bozuklukları ve fazla harcama riskleri konusunda otomatik olarak haber veren [Uyarılar](../cost-management/cost-mgt-alerts-monitor-usage-spending.md) oluşturmak için [bütçeler](../cost-management/tutorial-acm-create-budgets.md) oluşturun. Uyarılar, bütçe ve maliyet eşiklerine kıyasla harcamaya göre belirlenir. Bütçeler ve uyarılar Azure abonelikleri ve kaynak grupları için oluşturulur, bu nedenle genel maliyet izleme stratejisinin bir parçası olarak faydalıdır. Ancak, bütçelerin ve uyarıların maliyetleri daha yüksek bir düzeyde izlemek üzere tasarlandıklarından, bireysel Azure hizmeti maliyetlerini yönetmek için sınırlı işlevlere sahip olabilir.

## <a name="monitor-costs"></a>Maliyetleri izleme

Azure Machine Learning ile kaynakları kullanırken maliyetleriniz vardır. Azure Kaynak kullanımı birim maliyetleri, zaman aralıklarına (saniye, dakika, saat ve gün) veya istek birimi kullanımına göre değişir. Azure Machine Learning kullanımı başladığı anda, maliyetler tahakkuk edilir. Bu maliyetleri Azure portal [Maliyet Analizi](../cost-management/quick-acm-cost-analysis.md) bölmesinde görüntüleyin.

Farklı zaman aralıkları için grafikler ve tablolardaki maliyetleri görüntüleyin. Bazı örnekler güne, geçerli, önceki aya ve yıla göre yapılır. Ayrıca, bütçelerle ve tahmin edilen maliyetlere göre maliyetleri görüntüleyin. Zamana göre daha uzun görünümlere geçiş yapmak, harcama eğilimlerini belirlemenize ve fazla harcama oluşmuş olabileceğini görmenizi sağlar. Bütçeleri oluşturduysanız nerede aşıldığını görün.  

Machine Learning için ayrı bir hizmet alanı görmezsiniz.  Bunun yerine Machine Learning çalışma alanlarınızda eklediğiniz çeşitli kaynakları görürsünüz.

## <a name="use-amlcompute"></a>AmlCompute kullanma

Sürekli değişen veriler sayesinde, doğru modelleri sürdürmek için hızlı ve kolaylaştırılmış model eğitimi ve yeniden eğitimlere ihtiyacınız vardır. Ancak, özellikle GPU 'Larda derin öğrenme modelleri için sürekli eğitim bir maliyetle gelir. 

Azure Machine Learning kullanıcılar, AmlCompute olarak da adlandırılan yönetilen Azure Machine Learning işlem kümesini kullanabilir. AmlCompute, çeşitli GPU ve CPU seçeneklerini destekler. AmlCompute, aboneliğiniz adına Azure Machine Learning göre dahili olarak barındırılır, ancak Azure IaaS bulut ölçeğinde aynı kurumsal sınıf güvenlik, uyumluluk ve idare sağlar.

Bu işlem havuzları Azure 'un IaaS altyapısının içinde olduğundan, altyapınızın geri kalanı ile aynı güvenlik ve uyumluluk gereksinimleriyle öğreticinizi dağıtabilir, ölçeklendirebilir ve yönetebilirsiniz.  Bu dağıtımlar aboneliğinizde oluşur ve idare kurallarınızı uyar. [Azure Machine Learning işlem](how-to-set-up-training-targets.md#amlcompute)hakkında daha fazla bilgi edinin.

## <a name="configure-training-clusters-for-autoscaling"></a>Otomatik ölçeklendirme için eğitim kümelerini yapılandırma

İş yükünüzün gereksinimlerine göre otomatik ölçeklendirme kümeleri, yalnızca ihtiyacınız olanları kullanabilmeniz için maliyetlerinizi azaltmaya yardımcı olur. AmlCompute kümeleri, iş yükünüzün gereksinimlerine göre dinamik olarak otomatik ölçeklendirme için tasarlanmıştır. Küme, sağlanan en fazla düğüm sayısına ve abonelik için belirlenen kotanın içine ölçeklendirilebilir. Her çalıştırma tamamlandığında, küme düğümleri ve otomatik ölçeklendirmeyi belirlenen en düşük düğüm sayımına serbest bırakabilir.

En düşük ve en fazla düğüm sayısını ayarlamaya ek olarak, ölçeği ölçeklendirmeye başlamadan önce düğümün boşta kaldığı süreyi ince ayar. Varsayılan olarak, ölçek azaltma öncesindeki boşta geçen süre 120 saniyeye ayarlanır.

+ Daha az yinelemeli deneme gerçekleştirirseniz, maliyetleri kaydetmek için bu süreyi azaltın. 
+ Son derece yinelemeli geliştirme/test deneme gerçekleştirirseniz, eğitim betiğinizdeki veya ortamınızdaki her değişiklikten sonra sabit ölçekleme için ödeme yapmak zorunda olmadığınız için bunu artırmanız gerekebilir.

AmlCompute kümeleri, amlcompute [SDK sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py)olan AMLCOMPUTE [CLI](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/create?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-create-amlcompute)ve [REST API 'leri](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/machinelearningservices/resource-manager/Microsoft.MachineLearningServices/stable)ile birlikte Azure Portal değişen iş yükü gereksinimleriniz için yapılandırılabilir.

```azure cli
az ml computetarget create amlcompute --name testcluster --vm-size Standard_NC6 --min-nodes 0 --max-nodes 5 --idle-seconds-before-scaledown 300
```

## <a name="set-quotas-on-resources"></a>Kaynaklarda kota ayarlama

Diğer Azure işlem kaynaklarıyla aynı şekilde AmlCompute, bir veya daha fazla [Kota (veya limit) yapılandırmasıyla](how-to-manage-quotas.md#azure-machine-learning-compute)gelir. Bu kota, VM ailesine göre (örneğin, Dv2 serisi, NCv3 serisi) ve her abonelik için bölgeye göre değişir. Abonelikler, sizin için küçük varsayılanlar ile başlar, ancak aboneliğinizde kullanılacak Amlcompute kaynaklarının miktarını denetlemek için bu ayarı kullanın. 

Ayrıca, bir abonelik içindeki her çalışma alanı için, [VM ailesine göre çalışma alanı düzeyi kotasını](how-to-manage-quotas.md#workspace-level-quota)yapılandırın. Bu, her bir çalışma alanının potansiyel olarak olası ve belirli VM ailelerini kısıtlayabileceği maliyetler üzerinde daha ayrıntılı denetime sahip olmasını sağlar. 

Çalışma alanı düzeyinde kotalar ayarlamak için [Azure Portal](https://portal.azure.com)başlatın.  Aboneliğinizdeki herhangi bir çalışma alanını seçin ve sol bölmedeki **kullanımlar + kotalar** ' ı seçin. Sonra kotaları görüntülemek için **kotaları Yapılandır** sekmesini seçin. Birden çok çalışma alanını etkileyen bir ayar olduğundan, bu kotayı ayarlamak için abonelik kapsamında ayrıcalıklarınız olması gerekir.

## <a name="set-run-auto-termination-policies"></a>Çalıştırma otomatik sonlandırma ilkelerini ayarla 

Kendi sürelerini sınırlamak veya Azure Machine Learning yerleşik hiper parametre ayarlama veya otomatik Machine Learning özellikleri kullanırken belirli koşullarda bunları erken sonlandırmak için eğitim çalıştırmalarını yapılandırın. 

İşte kullanabileceğiniz birkaç seçenek:
* Bir çalıştırmanın seçtiğiniz işlem `max_run_duration_seconds` için (yerel veya uzak bulut işlem) üzerinde genişletileceği en uzun süreyi denetlemek Için RunConfiguration uygulamanızda çağrılan bir parametre tanımlayın.
* [Hiper parametre ayarlama](how-to-tune-hyperparameters.md#early-termination)için, bir bandıt ilkesinden erken sonlandırma Ilkesi, ortanca durdurma Ilkesi veya kesme seçim ilkesi tanımlayın. Ayrıca, farklı hiper parametre sweeps 'yi `max_total_runs` daha `max_duration_minutes` fazla denetlemek için veya gibi parametreleri de kullanın.
* [Otomatik makine öğrenimi](how-to-configure-auto-train.md#exit)için `enable_early_stopping` bayrağını kullanarak benzer sonlandırma ilkeleri ayarlayın. Ayrıca, `iteration_timeout_minutes` ve bir çalıştırmanın en `experiment_timeout_minutes` uzun süresini denetlemek için veya tüm denemeler için ve gibi özellikleri kullanın.

## <a name="use-low-priority-vms"></a>Düşük öncelikli VM’ler kullanma

Azure, sanal makine ölçek kümeleri, toplu Iş ve Machine Learning hizmeti arasında düşük öncelikli VM 'Ler olarak fazla unutilized kapasitesini kullanmanıza olanak tanır. Bu ayırmalar ön azaltıldı, ancak adanmış VM 'Lerle karşılaştırıldığında daha düşük bir fiyata gelir. Genel olarak, toplu iş yükleri için düşük öncelikli sanal makinelerin kullanılması veya resubmits (toplu Iş için geçiş için) veya yeniden başlatmalar aracılığıyla (denetim noktası ile derin öğrenme eğitimi için) kesilmesinin mümkün olması önerilir.

Düşük öncelikli VM 'Ler, VM ailesi ile olan ayrılmış kota değerinden ayrı tek bir kotaya sahiptir. [AmlCompute kotaları hakkında daha fazla](how-to-manage-quotas.md)bilgi edinin.

SANAL makinenizin önceliğini şu yollarla ayarlayın:

* Studio 'da, bir VM oluştururken **düşük öncelik** ' i seçin.

* Python SDK ile, sağlama yapılandırmanızda `vm_priority` özniteliğini ayarlayın.  

    ```python
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                               vm_priority='lowpriority',
                                                               max_nodes=4)
    ```

* CLı 'yi kullanarak şunları ayarlayın `vm-priority`:

    ```azurecli-interactive
    az ml computetarget create amlcompute --name lowpriocluster --vm-size Standard_NC6 --max-nodes 5 --vm-priority lowpriority
    ```

 Düşük öncelikli VM 'Ler, etkileşimli not defteri deneyimlerini desteklemesi gerektiğinden, işlem örnekleri için çalışmaz. 

## <a name="use-reserved-instances"></a>Ayrılmış örnekleri kullanma

Azure ayrılmış VM örneği, bir yıllık veya üç yıllık koşullara göre işlem kaynaklarına çok büyük tasarruf sağlamak için başka bir yol sağlar. Bu indirimler, Kullandıkça Öde fiyatlarının %72 ' ine kadar değişir ve doğrudan aylık Azure faturanızda uygulanır.

Azure Machine Learning Işlem, ayrılmış örnekleri kendiliğinden destekler. Bu nedenle, bir yıllık veya üç yıllık ayrılmış örnek satın aldıysanız, uçtan uca ek bir kurulum gerektirmeden Azure Machine Learning içinde kullanılan yönetilen işlem için bu ayrılmış örnek indirimi otomatik olarak uygulayacağız.


## <a name="next-steps"></a>Sonraki adımlar

* [Maliyet Analizi](../cost-management-billing/costs/quick-acm-cost-analysis.md)ile maliyetleri yönetme hakkında daha fazla bilgi edinin.
* [Azure Machine Learning işlem](how-to-set-up-training-targets.md#amlcompute)hakkında daha fazla bilgi edinin.