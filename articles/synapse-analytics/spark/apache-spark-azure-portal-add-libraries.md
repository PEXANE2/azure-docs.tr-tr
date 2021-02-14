---
title: Apache Spark için kitaplıkları yönetme
description: Azure SYNAPSE Analytics 'te Apache Spark tarafından kullanılan kitaplıkları ekleme ve yönetme hakkında bilgi edinin.
services: synapse-analytics
author: midesa
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 10/16/2020
ms.author: midesa
ms.reviewer: jrasnick
ms.subservice: spark
ms.openlocfilehash: 0458fb8b140166b7bdf0fc0df41dbb207fdce3c9
ms.sourcegitcommit: e972837797dbad9dbaa01df93abd745cb357cde1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100518530"
---
# <a name="manage-libraries-for-apache-spark-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te Apache Spark için kitaplıkları yönetme

Kitaplıklar, programlarınıza veya projelerinize dahil etmek isteyebileceğiniz yeniden kullanılabilir kod sağlar. Üçüncü tarafa veya yerel olarak oluşturulmuş bir kodu uygulamalarınız için kullanılabilir hale getirmek için, bir kitaplığı sunucusuz Apache Spark havuzlarınızdan birine yükleyebilirsiniz. Spark havuzu için bir kitaplık yüklendikten sonra, aynı havuzu kullanan tüm oturumlarda kullanılabilir. 

## <a name="before-you-begin"></a>Başlamadan önce
- Kitaplıkları yüklemek ve güncelleştirmek için, Azure SYNAPSE Analytics çalışma alanına bağlı birincil Gen2 depolama hesabında **Depolama Blobu veri katılımcısı** veya **Depolama Blobu veri sahibi** izinlerine sahip olmanız gerekir.
  
## <a name="default-installation"></a>Varsayılan yükleme
Azure SYNAPSE Analytics 'teki Apache Spark, tam bir anacondas yüklemesi artı ek kitaplıklar içerir. Tam kitaplıklar listesi, [Apache Spark sürüm desteği](apache-spark-version-support.md)' nde bulunabilir. 

Bir Spark örneği başlatıldığında, bu kitaplıklar otomatik olarak dahil edilir. Ek Python ve özel olarak oluşturulmuş paketler Spark havuz düzeyinde eklenebilir.


## <a name="manage-python-packages"></a>Python paketlerini Yönet
Spark uygulamanız için kullanmak istediğiniz kitaplıkları tanımladıktan sonra bunları bir Spark havuzuna yükleyebilirsiniz. 

 Sanal ortamı yükseltmek için bir *requirements.txt* dosyası ( `pip freeze` komuttan çıkış) kullanılabilir. Bu dosyada yüklenmek veya yükseltmek üzere listelenen paketler, havuz başlatma sırasında PyPI 'den indirilir. Bu gereksinimler dosyası, bu Spark havuzundan bir Spark örneği oluşturulduğu her seferinde kullanılır.

> [!IMPORTANT]
> - Yüklemekte olduğunuz Paket büyükse veya yüklemesi uzun sürüyorsa, bu, Spark örneği başlangıç süresini etkiler.
> - Yükleme zamanında derleyici desteği gerektiren GCC gibi paketler desteklenmez.
> - Paketler düşürülemez, yalnızca eklenebilir veya yükseltilebilir.
> - PySpark, Python, Scala/Java, .NET veya Spark sürümünün değiştirilmesi desteklenmez.
> - Pypı 'den paket yükleme, DEP özellikli çalışma alanları içinde desteklenmez.


### <a name="requirements-format"></a>Gereksinimler biçimi

Aşağıdaki kod parçacığında gereksinimler dosyasının biçimi gösterilmektedir. Pypı paket adı, tam bir sürüm ile birlikte listelenir. Bu dosya, [PIP dondurma](https://pip.pypa.io/en/stable/reference/pip_freeze/) başvuru belgelerinde açıklanan biçimi izler. Bu örnek, belirli bir sürümü sabitler. 

```
absl-py==0.7.0
adal==1.2.1
alabaster==0.7.10
```

### <a name="install-python-packages"></a>Python paketlerini yükler
Spark uygulamanızı geliştirirken, var olan veya yeni kitaplıkları yüklemeniz gerektiğini fark edebilirsiniz. Kitaplıklar, havuz oluşturma sırasında veya sonrasında güncelleştirilemeyebilir.

> [!IMPORTANT]
> Kitaplıkları yüklemek için, SYNAPSE çalışma alanına bağlı birincil Gen2 depolama hesabında Depolama Blobu veri katılımcısı veya Depolama Blobu veri sahibi izinlerinizin olması gerekir.

#### <a name="install-packages-during-pool-creation"></a>Havuz oluşturma sırasında paketleri yükler
Havuz oluşturma sırasında bir Spark havuzuna kitaplık yüklemek için:
   
1. Azure portal Azure SYNAPSE Analytics çalışma alanınıza gidin.
   
2. **Apache Spark havuzu oluştur** ' u seçin ve **ek ayarlar** sekmesini seçin. 
   
3. Sayfanın **paketler** bölümündeki dosya seçicisini kullanarak ortam yapılandırma dosyasını karşıya yükleyin. 
   
    ![Havuz oluşturma sırasında Python kitaplıklarını ekleme](./media/apache-spark-azure-portal-add-libraries/apache-spark-azure-portal-add-library-python.png "Python kitaplıklarını ekleme")
 

#### <a name="install-packages-from-the-synapse-workspace"></a>Paketleri SYNAPSE çalışma alanından yükler
Azure SYNAPSE Analytics portalından bir Spark havuzuna güncelleştirmek veya daha fazla kitaplık eklemek için:

1.  Azure portal Azure SYNAPSE Analytics çalışma alanınıza gidin.
   
2.  Azure portal Azure SYNAPSE Analytics çalışma alanınızı başlatın.

3.  Ana Gezinti panelinden **Yönet** ' i seçin ve ardından **Apache Spark havuzları**' nı seçin.
   
4. Tek bir Spark havuzu seçin ve sayfanın  **paketler** bölümündeki dosya seçicisini kullanarak ortam yapılandırma dosyasını karşıya yükleyin.

    ![SYNAPSE 'e Python kitaplıkları ekleme](./media/apache-spark-azure-portal-add-libraries/apache-spark-azure-portal-update.png)
   
#### <a name="install-packages-from-the-azure-portal"></a>Azure portal paketleri yükler
Bir kitaplığı bir Spark havuzunda doğrudan Azure portal yüklemek için:
   
 1. Azure portal Azure SYNAPSE Analytics çalışma alanınıza gidin.
   
 2. **SYNAPSE kaynakları** bölümünde, **Apache Spark havuzları** sekmesini seçin ve listeden bir Spark havuzu seçin.
   
 3. Spark havuzunun **Ayarlar** bölümünden **paketler** ' i seçin. 

 4. Dosya seçiciyi kullanarak ortam yapılandırma dosyasını karşıya yükleyin.

    ![Ortam yapılandırma dosyasını karşıya yükle düğmesinin vurgualdığı ekran görüntüsü.](./media/apache-spark-azure-portal-add-libraries/apache-spark-add-library-azure.png "Python kitaplıklarını ekleme")

### <a name="verify-installed-libraries"></a>Yüklü kitaplıkları doğrulama

Doğru kitaplıkların doğru sürümlerinin yüklenip yüklenmediğini doğrulamak için aşağıdaki kodu çalıştırın

```python
import pkg_resources
for d in pkg_resources.working_set:
     print(d)
```
### <a name="update-python-packages"></a>Python paketlerini güncelleştirme
Paketler, oturumlar arasında dilediğiniz zaman eklenebilir veya değiştirilebilir. Yeni bir paket yapılandırma dosyası var olan paketlerin ve sürümlerin üzerine yazacak.  

Bir kitaplığı güncelleştirmek veya kaldırmak için:
1. Azure SYNAPSE Analytics çalışma alanınıza gidin. 

2. Azure portal veya Azure SYNAPSE çalışma alanını kullanarak, güncelleştirmek istediğiniz **Apache Spark havuzunu** seçin.

3. **Paketler** bölümüne gidin ve yeni bir ortam yapılandırma dosyası yükleyin
   
4. Değişikliklerinizi kaydettikten sonra, etkin oturumları sonlandırdıktan sonra havuzun yeniden başlatılmasına izin verin. İsteğe bağlı olarak, **Yeni ayarları zorlamak** için onay kutusunu seçerek etkin oturumları sona erdirmek için zorlayabilirsiniz.

    ![Python kitaplıklarını ekleme](./media/apache-spark-azure-portal-add-libraries/update-libraries.png "Python kitaplıklarını ekleme")
   

> [!IMPORTANT]
> **Yeni ayarları zorlama** seçeneğini belirleyerek seçili Spark havuzunun tüm geçerli oturumlarını sonlandıracaksınız. Oturumlar sona erdikten sonra, havuzun yeniden başlatılmasını beklemeniz gerekir. 
>
> Bu ayar işaretlenmezse, geçerli Spark oturumunun bitmesini beklemeniz veya el ile durdurmanız gerekir. Oturum sona erdikten sonra havuzun yeniden başlatılmasına izin vermek gerekir. 


## <a name="manage-a-python-wheel"></a>Python tekerleğini yönetme

### <a name="install-a-custom-wheel-file"></a>Özel bir tekerlek dosyası yükler
Özel yerleşik tekerlek paketleri, tüm tekerlek dosyalarını SYNAPSE çalışma alanıyla bağlantılı Azure Data Lake Storage (Gen2) hesabına yükleyerek Apache Spark havuza yüklenebilir. 

Dosyalar, depolama hesabının varsayılan kapsayıcısında aşağıdaki yola yüklenmelidir: 

```
abfss://<file_system>@<account_name>.dfs.core.windows.net/synapse/workspaces/<workspace_name>/sparkpools/<pool_name>/libraries/python/
```

Klasörü ```python``` zaten yoksa klasörü içine eklemeniz gerekebilir ```libraries``` .

>[!IMPORTANT]
>Özel paketler, oturumlar arasında eklenebilir veya değiştirilebilir. Ancak, güncelleştirilmiş paketi görmek için havuzun ve oturumun yeniden başlatılmasını beklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar
- Varsayılan kitaplıkları görüntüleme: [Apache Spark sürüm desteği](apache-spark-version-support.md)
