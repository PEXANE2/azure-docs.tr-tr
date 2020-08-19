---
title: Azure SYNAPSE Analytics 'te Apache Spark için kitaplıkları yönetme
description: Azure SYNAPSE Analytics 'te Apache Spark tarafından kullanılan kitaplıkları ekleme ve yönetme hakkında bilgi edinin.
services: synapse-analytics
author: euangMS
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 07/22/2020
ms.author: euang
ms.reviewer: jrasnick, carlrab
ms.subservice: spark
ms.openlocfilehash: b7aea6565e8301e2aeb96263b8e7b1d2ea64995d
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88589570"
---
# <a name="manage-libraries-for-apache-spark-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te Apache Spark için kitaplıkları yönetme

Kitaplıklar, programlarınıza veya projelerinize dahil etmek isteyebileceğiniz yeniden kullanılabilir kod sağlar. Üçüncü tarafa veya yerel olarak oluşturulmuş bir kodu uygulamalarınız için kullanılabilir hale getirmek için, Spark Havuzlarınızdan birine (Önizleme) bir kitaplık yükleyebilirsiniz. Spark havuzu için bir kitaplık yüklendikten sonra, aynı havuzu kullanan tüm oturumlarda kullanılabilir. 

## <a name="default-installation"></a>Varsayılan yükleme
Azure SYNAPSE Analytics 'teki Apache Spark, tam bir anacondas yüklemesi ve ek kitaplıklar içerir. Tam kitaplıklar listesi, [Apache Spark sürüm desteği](apache-spark-version-support.md)' nde bulunabilir. 

Bir Spark örneği başlatıldığında, bu kitaplıklar otomatik olarak dahil edilir. Spark Havuzu (Önizleme) düzeyinde ek Python ve özel oluşturulmuş paketler eklenebilir.


## <a name="manage-python-packages"></a>Python paketlerini Yönet
Spark uygulamanız için kullanmak istediğiniz kitaplıkları tanımladıktan sonra, bunları bir Spark havuzuna (Önizleme) yükleyebilirsiniz. 

 Sanal ortamı yükseltmek için bir *requirements.txt* dosyası ( `pip freeze` komuttan çıkış) kullanılabilir. Bu dosyada yüklenmek veya yükseltmek üzere listelenen paketler, havuz başlatma sırasında PyPi 'den indirilir. Bu gereksinimler dosyası, bu Spark havuzundan bir Spark örneği oluşturulduğu her seferinde kullanılır.

> [!IMPORTANT]
> - Yüklemekte olduğunuz Paket büyükse veya yüklemesi uzun sürüyorsa, bu, Spark örneği başlangıç süresini etkiler.
> - GCC gibi yüklemede, derleme zamanında derleyici desteği gerektiren paketler desteklenmez.
> - Paketler düşürülemez, yalnızca eklenebilir veya yükseltilebilir.

### <a name="requirements-format"></a>Gereksinimler biçimi

Aşağıdaki kod parçacığında gereksinimler dosyasının biçimi gösterilmektedir. Pypı paket adı, tam bir sürüm ile birlikte listelenir. Bu dosya, [PIP dondurma](https://pip.pypa.io/en/stable/reference/pip_freeze/) başvuru belgelerinde açıklanan biçimi izler. Bu örnek, belirli bir sürümü sabitler. 

```
absl-py==0.7.0
adal==1.2.1
alabaster==0.7.10
```

### <a name="install-python-packages"></a>Python paketlerini yükler
Spark uygulamanızı geliştirirken, var olan veya yeni kitaplıkları yüklemeniz gerektiğini fark edebilirsiniz. Kitaplıklar, havuz oluşturma sırasında veya sonrasında güncelleştirilemeyebilir.

#### <a name="install-packages-during-pool-creation"></a>Havuz oluşturma sırasında paketleri yükler
Havuz oluşturma sırasında kitaplıkları Spark havuzuna (Önizleme) yüklemek için:
   
1. Azure portal Azure SYNAPSE Analytics çalışma alanınıza gidin.
   
2. **Apache Spark havuzu oluştur** ' u seçin ve **ek ayarlar** sekmesini seçin. 
   
3. Sayfanın **paketler** bölümündeki dosya seçicisini kullanarak ortam yapılandırma dosyasını karşıya yükleyin. 
   
![Havuz oluşturma sırasında Python kitaplıklarını ekleme](./media/apache-spark-azure-portal-add-libraries/apache-spark-azure-portal-add-library-python.png "Python kitaplıklarını ekleme")
 

#### <a name="install-packages-from-the-synapse-workspace"></a>Paketleri SYNAPSE çalışma alanından yükler
Azure SYNAPSE Analytics portalından Spark havuzuna (Önizleme) güncelleştirmek veya ek kitaplıklar eklemek için:

1.  Azure portal Azure SYNAPSE Analytics çalışma alanınıza gidin.
   
2.  Azure portal Azure SYNAPSE Analytics çalışma alanınızı başlatın.

3.  Ana Gezinti panelinden **Yönet** ' i seçin ve ardından **Apache Spark havuzları**' nı seçin.
   
4. Tek bir Spark havuzu seçin ve sayfanın  **paketler** bölümündeki dosya seçicisini kullanarak ortam yapılandırma dosyasını karşıya yükleyin.

![SYNAPSE 'e Python kitaplıkları ekleme](./media/apache-spark-azure-portal-add-libraries/apache-spark-azure-portal-update.png "Python kitaplıklarını ekleme")
   
#### <a name="install-packages-from-the-azure-portal"></a>Azure portal paketleri yükler
Bir kitaplığı bir Spark havuzuna (Önizleme) doğrudan Azure portal yüklemek için:
   
 1. Azure portal Azure SYNAPSE Analytics çalışma alanınıza gidin.
   
 2. **SYNAPSE kaynakları** bölümünde, **Apache Spark havuzları** sekmesini seçin ve listeden bir Spark havuzu seçin.
   
 3. Spark havuzunun **Ayarlar** bölümünden **paketler** ' i seçin. 

 4. Dosya seçiciyi kullanarak ortam yapılandırma dosyasını karşıya yükleyin.

![Python kitaplıklarını ekleme](./media/apache-spark-azure-portal-add-libraries/apache-spark-add-library-azure.png "Python kitaplıklarını ekleme")

### <a name="verify-installed-libraries"></a>Yüklü kitaplıkları doğrulama

Doğru kitaplıkların doğru sürümlerinin yüklenip yüklenmediğini doğrulamak için aşağıdaki kodu çalıştırın

```python
import pip #needed to use the pip functions
for i in pip.get_installed_distributions(local_only=True):
    print(i)
```
### <a name="update-python-packages"></a>Python paketlerini güncelleştirme
Paketler, oturumlar arasında dilediğiniz zaman eklenebilir veya değiştirilebilir. Yeni bir paket yapılandırma dosyası karşıya yüklendiğinde, bu, var olan paketlerin ve sürümlerin üzerine yazar.  

Bir kitaplığı güncelleştirmek veya kaldırmak için:
1. Azure SYNAPSE Analytics çalışma alanınıza gidin. 

2. Azure portal veya Azure SYNAPSE çalışma alanını kullanarak, güncelleştirmek istediğiniz **Apache Spark havuzunu** seçin.

3. **Paketler** bölümüne gidin ve yeni bir ortam yapılandırma dosyası yükleyin
   
4. Değişikliklerinizi kaydettikten sonra, etkin oturumları sonlandırdıktan sonra havuzun yeniden başlatılmasına izin verin. İsteğe bağlı olarak, **Yeni ayarları zorlamak**için onay kutusunu seçerek etkin oturumları sona erdirmek için zorlayabilirsiniz.

![Python kitaplıklarını ekleme](./media/apache-spark-azure-portal-add-libraries/update-libraries.png "Python kitaplıklarını ekleme")
   

> [!IMPORTANT]
> **Yeni ayarları zorlama**seçeneğini belirleyerek seçili Spark havuzunun tüm geçerli oturumlarını sonlandıracaksınız. Oturumlar sona erdikten sonra, havuzun yeniden başlatılmasını beklemeniz gerekir. 
>
> Bu ayar işaretlenmezse, geçerli Spark oturumunun bitmesini beklemeniz veya el ile durdurmanız gerekir. Oturum sona erdikten sonra havuzun yeniden başlatılmasına izin vermek gerekir. 


## <a name="manage-a-python-wheel"></a>Python tekerleğini yönetme

### <a name="install-a-custom-wheel-file"></a>Özel bir tekerlek dosyası yükler
Özel yerleşik tekerlek paketleri, tüm tekerlek dosyalarını SYNAPSE çalışma alanıyla bağlantılı Azure Data Lake Storage (Gen2) hesabına yükleyerek Apache Spark havuza yüklenebilir. 

Dosyalar, depolama hesabının varsayılan kapsayıcısında aşağıdaki yola yüklenmelidir: 

```
abfss://<file_system>@<account_name>.dfs.core.windows.net/synapse/workspaces/<workspace_name>sparkpools/<pool_name>libraries/python/
```

>[!IMPORTANT]
>Özel paketler, oturumlar arasında eklenebilir veya değiştirilebilir. Ancak, güncelleştirilmiş paketi görmek için havuzun ve oturumun yeniden başlatılmasını beklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar
- Varsayılan kitaplıkları görüntüleme: [Apache Spark sürüm desteği](apache-spark-version-support.md)
