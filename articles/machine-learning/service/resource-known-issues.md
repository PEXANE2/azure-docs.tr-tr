---
title: Sorun giderme & bilinen sorunlar
titleSuffix: Azure Machine Learning
description: Azure Machine Learning için bilinen sorunların, geçici çözümlerin ve sorun gidermenin bir listesini alın.
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 08/09/2019
ms.custom: seodec18
ms.openlocfilehash: 275cf20329be04e86c2e7c2a613f657733e652df
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71213439"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning"></a>Bilinen sorunlar ve sorun giderme Azure Machine Learning

Bu makale, Azure Machine Learning kullanırken hataları veya hataları bulmanıza ve düzeltmenize yardımcı olur.

## <a name="visual-interface-issues"></a>Görsel arabirim sorunları

Machine Learning hizmeti sorunları için görsel arabirim.

### <a name="long-compute-preparation-time"></a>Uzun süreli işlem hazırlık süresi

Yeni işlem oluşturun veya çağırabilir işlem zaman alır, birkaç dakika veya daha uzun olabilir. Takım en iyi duruma getirme için çalışıyor.


### <a name="cannot-run-an-experiment-only-contains-dataset"></a>Deneme yalnızca veri kümesi içeriyor 

Bir denemeyi çalıştırmak, veri kümesini görselleştirmek için yalnızca veri kümesi içeriyor. Ancak, yalnızca bir denemeyi çalıştırmaya izin verilmez. Bu sorunu etkin bir şekilde düzelttik.
 
Bu yüklemeden önce veri kümesini herhangi bir veri dönüştürme modülüne bağlayabilirsiniz (veri kümesinde sütunları seçin, meta verileri düzenleyebilir, verileri bölebilir vb.) ve denemeyi çalıştırabilirsiniz. Daha sonra veri kümesini görselleştirebilirsiniz. 

Aşağıdaki görüntüde nasıl kullanıldığı gösterilmektedir ![: visulize-Data](./media/resource-known-issues/aml-visualize-data.png)

## <a name="sdk-installation-issues"></a>SDK yükleme sorunları

**Hata iletisi: ' PyYAML ' kaldırılamıyor**

Python için SDK Azure Machine Learning: PyYAML, yüklenmiş bir proje. Bu nedenle, kısmi bir kaldırma işlemi varsa, hangi dosyaların kendisine ait olduğunu doğru bir şekilde belirleyemedik. Bu hatayı yoksayma sırasında SDK'sı yüklemeye devam etmek için kullanın:

```Python
pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
```

**Hata iletisi: `ERROR: No matching distribution found for azureml-dataprep-native`**

Anaconda 'nın Python 3.7.4 dağıtımında, azureml-SDK yüklemesini kesen bir hata vardır. Bu sorun, bu [GitHub sorunu](https://github.com/ContinuumIO/anaconda-issues/issues/11195) ile tartışılır ve bu komut kullanılarak yeni bir Conda ortamı oluşturulabilir:
```bash
conda create -n <env-name> python=3.7.3
```
Bu, 3.7.4 içinde bir Install sorunu bulunmayan Python 3.7.3 kullanarak bir Conda ortamı oluşturur.

## <a name="trouble-creating-azure-machine-learning-compute"></a>Azure Machine Learning işlem oluştururken sorun

GA sürümü önce Azure portalından, Azure Machine Learning çalışma alanı oluşturan bazı kullanıcıların bu çalışma alanında Azure Machine Learning işlem oluşturmak mümkün olmayabilir nadir bir fırsat yoktur. Bir destek isteği hizmetinde yükseltmek veya Portal veya SDK'yı kendiniz hemen engelini kaldırmak için yeni bir çalışma alanı oluşturun.

## <a name="image-building-failure"></a>Görüntü oluşturma hatası

Web hizmeti dağıtılırken hata oluşturma görüntüsü. Geçici çözüm olan eklemek için "pynacl 1.2.1 ==" Conda dosyasına görüntü yapılandırması için pip bağımlılık olarak.

## <a name="deployment-failure"></a>Dağıtım hatası

Gözlemlerseniz `['DaskOnBatch:context_managers.DaskOnBatch', 'setup.py']' died with <Signals.SIGKILL: 9>`, dağıtımınızda kullanılan VM 'ler için SKU 'yu daha fazla belleğe sahip bir şekilde değiştirin.

## <a name="fpgas"></a>FPGA

İstenen ve FPGA kotası için onaylanmış kadar FPGA modellerde dağıtmayı mümkün olmayacaktır. Erişim istemek için kota istek formunu doldurun: https://aka.ms/aml-real-time-ai

## <a name="automated-machine-learning"></a>Otomatik makine öğrenimi

Tensor Flow otomatik makine öğrenimi Şu anda Tensor Flow sürüm 1,13 ' i desteklememektedir. Bu sürümün yüklenmesi paket bağımlılıklarının çalışmayı durdurmasına neden olur. Bu sorunu gelecekte yayımlanacak bir sürümde gidermeye çalışıyoruz. 

### <a name="experiment-charts"></a>Deneme grafikleri

Otomatik ML denemesi yinelemeleriyle gösterilen ikili sınıflandırma grafikleri (duyarlık-hatırla, ROC, kazanç eğrisi vb.), 4/12 ' den beri Kullanıcı arabiriminde doğru işlenmemektedir. Grafik çizimleri Şu anda daha iyi şekilde uygulanan modellerin daha düşük sonuçlarla gösterildiği ters sonuçları gösteriyor. Bir çözüm, araştırma aşamasındadır.

## <a name="databricks"></a>Databricks

Databricks ve Azure Machine Learning sorunları.

### <a name="failure-when-installing-packages"></a>Paketler yüklenirken hata oluştu

Azure Machine Learning SDK yüklemesi, daha fazla paket yüklendiğinde Azure Databricks başarısız olur. Gibi bazı paketler `psutil`, çakışmaları neden olabilir. Yükleme hatalarını önlemek için, kitaplık sürümünü dondurarak paketleri yükleme. Bu sorun, Azure Machine Learning SDK 'Sı değil Databricks ile ilgilidir. Bu sorunla diğer kitaplıklarla de karşılaşabilirsiniz. Örnek:

```python
psutil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
```

Alternatif olarak, Python kitaplıklarıyla ilgili sorunları gidermek için Init betiklerini kullanabilirsiniz. Bu yaklaşım resmi olarak desteklenmez. Daha fazla bilgi için bkz. [küme kapsamlı init betikleri](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

### <a name="cancel-an-automated-machine-learning-run"></a>Otomatik makine öğrenimi çalıştırmasını iptal et

Azure Databricks üzerinde otomatik makine öğrenimi özellikleri kullandığınızda, bir çalıştırmayı iptal etmek ve yeni bir deneme çalıştırması başlatmak için Azure Databricks kümenizi yeniden başlatın.

### <a name="10-iterations-for-automated-machine-learning"></a>otomatik makine öğrenimi için 10 > yineleme

Otomatik makine öğrenimi ayarları ' nda, 10 ' dan fazla yinelediğiniz takdirde `show_output` , `False` çalıştırmayı gönderdiğinizde olarak ayarlayın.

### <a name="widget-for-the-azure-machine-learning-sdkautomated-machine-learning"></a>Azure Machine Learning SDK/otomatik makine öğrenimi için pencere öğesi

Azure Machine Learning SDK pencere öğesi, bir Databricks not defterinde desteklenmez çünkü Not defterleri HTML pencere öğelerini ayrıştıramaz. Azure Databricks Not defteri hücresinizdeki bu python kodunu kullanarak portalda pencere öğesini görüntüleyebilirsiniz:

```
displayHTML("<a href={} target='_blank'>Azure Portal: {}</a>".format(local_run.get_portal_url(), local_run.id))
```

### <a name="import-error-no-module-named-pandascoreindexes"></a>İçeri aktarma hatası: ' Pandas. Core. Indexes ' adlı modül yok

Otomatik makine öğrenimi kullandığınızda bu hatayı görürseniz:

1. Azure Databricks kümenize iki paket yüklemek için şu komutu çalıştırın: 

   ```
   scikit-learn==0.19.1
   pandas==0.22.0
   ```

1. Kümeyi ayırın ve Not defterinize yeniden bağlayın. 

Bu adımlar sorunu çözmezse, kümeyi yeniden başlatmayı deneyin.

### <a name="failtosendfeather"></a>Failtosendgeçiş yumuşatma

Azure Databricks kümesindeki verileri okurken `FailToSendFeather` bir hata görürseniz, aşağıdaki çözümlere bakın:

* Paketi `azureml-sdk[automl_databricks]` en son sürüme yükseltin.
* Sürüm `azure-dataprep` 1.1.8 veya üstünü ekleyin.
* Sürüm `pyarrow` 0,11 veya üstünü ekleyin.

## <a name="azure-portal"></a>Azure portal

Doğrudan paylaşım bağlantısı SDK veya portalından çalışma alanınızda görüntülemeye giderseniz, uzantı normal genel bakış sayfası ile abonelik bilgilerini görüntülemek mümkün olmayacaktır. Siz de başka bir çalışma alanına geçmeniz mümkün olmayacaktır. Başka bir çalışma alanını görüntülemeniz gerekirse, geçici çözüm doğrudan [Azure Portal](https://portal.azure.com) gitmek ve çalışma alanı adını aramak olacaktır.

## <a name="diagnostic-logs"></a>Tanılama günlükleri

Bazen Yardım isteme, tanılama bilgilerini sağlarsanız, yararlı olabilir. Bazı günlükleri görmek için [Azure Portal](https://portal.azure.com) ziyaret edin ve çalışma alanınıza gidin ve **> günlüklerini çalıştırmak > deneyin > çalışma alanı**' nı seçin.  Ayrıca, bu bilgileri [çalışma alanı giriş sayfanızın (Önizleme)](https://ml.azure.com) **denemeleri** bölümünde bulabilirsiniz.

> [!NOTE]
> Azure Machine Learning, eğitim sırasında (örneğin, oto ml) veya eğitim işini çalıştıran Docker kapsayıcısı gibi çeşitli kaynaklardan günlük bilgileri günlüğe kaydeder. Bu günlüklerin birçoğu açıklanmamıştır. Sorunlarla karşılaşırsanız ve Microsoft Destek ile iletişime geçerek, sorun giderme sırasında bu günlükleri kullanabiliyor olabilirler.

## <a name="activity-logs"></a>Etkinlik günlükleri

Azure Machine Learning çalışma alanındaki bazı eylemler __etkinlik günlüğüne__bilgi vermez. Örneğin, bir eğitimi başlatma veya bir modeli kaydetme.

Bu eylemlerden bazıları, çalışma alanınızın __Etkinlikler__ alanında görünür, ancak aktiviteyi kimin başlattığını göstermez.

## <a name="resource-quotas"></a>Kaynak kotaları

Hakkında bilgi edinin [kaynak kotaları](how-to-manage-quotas.md) Azure Machine Learning'i kullanmaya çalışırken hatalarla karşılaşabilirsiniz.

## <a name="authentication-errors"></a>Kimlik Doğrulama hataları

Uzak bir işten bir işlem hedefinde yönetim işlemi gerçekleştirirseniz, aşağıdaki hatalardan birini alırsınız:

```json
{"code":"Unauthorized","statusCode":401,"message":"Unauthorized","details":[{"code":"InvalidOrExpiredToken","message":"The request token was either invalid or expired. Please try again with a valid token."}]}
```

```json
{"error":{"code":"AuthenticationFailed","message":"Authentication failed."}}
```

Örneğin, uzaktan yürütme için gönderilen bir ML ardışık düzeninde bir işlem hedefi oluşturmaya veya eklemeye çalıştığınızda bir hata alırsınız.

## <a name="overloaded-azurefile-storage"></a>Aşırı yüklenmiş AzureFile depolama

Bir hata `Unable to upload project files to working directory in AzureFile because the storage is overloaded`alırsanız, aşağıdaki geçici çözümleri uygulayın.

Veri aktarımı gibi diğer iş yükleri için dosya paylaşma 'yı kullanıyorsanız, dosya paylaşımının çalıştırmaları için kullanılabilmesi için blob 'ları kullanmak, bu nedenle söz konusu çalışma. İş yükünü iki farklı çalışma alanı arasında da bölebilirsiniz.

## <a name="webservices-in-azure-kubernetes-service-failures"></a>Azure Kubernetes hizmeti hatalarında WebServices 

Azure Kubernetes hizmetindeki birçok Web hizmeti hatası, kullanılarak `kubectl`kümeye bağlanarak hata ayıklanabilir. Bir Azure Kubernetes hizmet kümesi `kubeconfig.json` için şunu çalıştırarak edinebilirsiniz

```bash
az aks get-credentials -g <rg> -n <aks cluster name>
```

## <a name="updating-azure-machine-learning-components-in-aks-cluster"></a>AKS kümesindeki Azure Machine Learning bileşenleri güncelleştiriliyor

Azure Kubernetes hizmet kümesinde yüklü Azure Machine Learning bileşenlere yapılan güncelleştirmeler el ile uygulanmalıdır. Kümeyi Azure Machine Learning çalışma alanından ayırarak ve sonra kümeyi çalışma alanına yeniden iliştirerek bu güncelleştirmeleri uygulayabilirsiniz. Kümede SSL etkinse, kümeyi yeniden iliştirirken SSL sertifikasını ve özel anahtarı sağlamanız gerekir. 

```python
compute_target = ComputeTarget(workspace=ws, name=clusterWorkspaceName)
compute_target.detach()
compute_target.wait_for_completion(show_output=True)

attach_config = AksCompute.attach_configuration(resource_group=resourceGroup, cluster_name=kubernetesClusterName)

## If SSL is enabled.
attach_config.enable_ssl(
    ssl_cert_pem_file="cert.pem",
    ssl_key_pem_file="key.pem",
    ssl_cname=sslCname)

attach_config.validate_configuration()

compute_target = ComputeTarget.attach(workspace=ws, name=args.clusterWorkspaceName, attach_configuration=attach_config)
compute_target.wait_for_completion(show_output=True)
```

Artık SSL sertifikasına ve özel anahtara sahip değilseniz veya Azure Machine Learning tarafından oluşturulan bir sertifika kullanıyorsanız, kümeye bağlanıp gizli `kubectl` anahtarıalarakkümeyibağlamadanöncedosyalarıalabilirsiniz`azuremlfessl`.

```bash
kubectl get secret/azuremlfessl -o yaml
```

>[!Note]
>Kubernetes gizli dizileri temel-64 kodlu biçimde depolar. Ana 64 'nin `cert.pem` ve `key.pem` içindeki parolaların bileşenlerinin kodunu vermeden `attach_config.enable_ssl`önce kodu çözmelisiniz. 
