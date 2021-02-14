---
title: Kitaplık yükleme hatalarını giderme
description: Bu öğretici, kitaplık yükleme hatalarını giderme hakkında genel bakış sağlar.
services: synapse-analytics
author: midesa
ms.author: midesa
ms.service: synapse-analytics
ms.subservice: spark
ms.topic: conceptual
ms.date: 01/04/2021
ms.openlocfilehash: 60ea97ea2df271f867febec3fa0f0826a18dbbbf
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100417949"
---
# <a name="troubleshoot-library-installation-errors"></a>Kitaplık yükleme hatalarını giderme 
Üçüncü tarafa veya yerel olarak oluşturulmuş bir kodu uygulamalarınız için kullanılabilir hale getirmek için, bir kitaplığı sunucusuz Apache Spark havuzlarınızdan birine yükleyebilirsiniz. requirements.txt dosyasında listelenen paketler, havuz başlatma sırasında PyPi 'den indirilir. Bu gereksinimler dosyası, bu Spark havuzundan bir Spark örneği oluşturulduğu her seferinde kullanılır. Spark havuzu için bir kitaplık yüklendikten sonra, aynı havuzu kullanan tüm oturumlarda kullanılabilir. 

Bazı durumlarda, yüklemeye çalıştığınız kitaplığın Apache Spark havuzunuzdaki görünmediğinden emin olabilirsiniz. Bu durum genellikle, sağlanmış requirements.txt veya belirtilen kitaplıklarda bir hata olduğunda gerçekleşir. Kitaplık yükleme işleminde bir hata olduğunda, Apache Spark havuzu SYNAPSE temel çalışma zamanında belirtilen kitaplıklara geri döndürülür.

Bu belgenin amacı, yaygın sorunları sağlamaktır ve kitaplık yükleme hatalarını ayıklamanıza yardımcı olur.

## <a name="force-update-your-apache-spark-pool"></a>Apache Spark havuzunuzu güncelleştirmeye zorla
Apache Spark havuzunuzdaki kitaplıkları güncelleştirdiğinizde, havuz yeniden başlatıldıktan sonra bu değişiklikler alınacaktır. Etkin işleriniz varsa, bu işler Spark havuzunun orijinal sürümünde çalışmaya devam edecektir.

**Yeni ayarları zorlama** seçeneğini belirleyerek değişiklikleri uygulamaya zorlayabilirsiniz. Bu ayar, seçili Spark havuzunun tüm geçerli oturumlarını sonlandıracaktır. Oturumlar sona erdikten sonra, havuzun yeniden başlatılmasını beklemeniz gerekir. 

![Python kitaplıklarını ekleme](./media/apache-spark-azure-portal-add-libraries/update-libraries.png "Python kitaplıklarını ekleme")

## <a name="validate-your-permissions"></a>İzinlerinizi doğrulayın
Kitaplıkları yüklemek ve güncelleştirmek için, Azure SYNAPSE Analytics çalışma alanına bağlı birincil Azure Data Lake Storage 2. depolama hesabında **Depolama Blobu veri katılımcısı** veya **Depolama Blobu veri sahibi** izinlerine sahip olmanız gerekir.

Bu izinlere sahip olduğunu doğrulamak için aşağıdaki kodu çalıştırabilirsiniz:

```python
from pyspark.sql.types import StructType,StructField, StringType, IntegerType
data2 = [("James","Smith","Joe","4355","M",3000),
    ("Michael","Rose","Edward","40288","F",4000)
  ]

schema = StructType([ \
    StructField("firstname",StringType(),True), \
    StructField("middlename",StringType(),True), \
    StructField("lastname",StringType(),True), \
    StructField("id", StringType(), True), \
    StructField("gender", StringType(), True), \
    StructField("salary", IntegerType(), True) \
  ])
 
df = spark.createDataFrame(data=data2,schema=schema)

df.write.csv("abfss://<<ENTER NAME OF FILE SYSTEM>>@<<ENTER NAME OF PRIMARY STORAGE ACCOUNT>>.dfs.core.windows.net/validate_permissions.csv")

```
Bir hata alırsanız gerekli izinleriniz muhtemelen eksik olabilir. Gerekli izinleri alma hakkında bilgi edinmek için şu belgeyi ziyaret edin: [Depolama Blobu veri katılımcısı veya Depolama Blobu veri sahibi Izinleri atama](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal#assign-an-azure-built-in-role).

Ayrıca, bir işlem hattı çalıştırıyorsanız, çalışma alanı MSI, Depolama Blobu veri sahibine veya Depolama Blobu veri katılımcısı izinlerine de sahip olmalıdır. Çalışma alanı kimliğinizi bu izin olarak verme hakkında bilgi edinmek için, bkz. [çalışma alanı yönetilen kimliği için Izin verme](../security/how-to-grant-workspace-managed-identity-permissions.md).

## <a name="check-the-requirements-file"></a>Gereksinimler dosyasını denetleyin
Sanal ortamı yükseltmek için bir ***requirements.txt*** dosyası (PIP dondurma komutundan çıkış) kullanılabilir. Bu dosya, [PIP dondurma](https://pip.pypa.io/en/stable/reference/pip_freeze/) başvuru belgelerinde açıklanan biçime uyar.

Aşağıdaki kısıtlamalara dikkat etmek önemlidir:
   -  Pypı paket adı, tam bir sürümle birlikte listelenmiş olmalıdır. 
   -  Gereksinimler dosyasının içeriği ek boş satırlar veya karakterler içermemelidir. 
   -  [SYNAPSE çalışma zamanı](apache-spark-version-support.md) , her sunucusuz Apache Spark havuzuna önceden yüklenmiş bir kitaplıklar kümesi içerir. Temel çalışma zamanına önceden yüklenmiş olarak gelen paketler düşürülemez. Paketler yalnızca eklenebilir veya yükseltilebilir.
   -  PySpark, Python, Scala/Java, .NET veya Spark sürümünün değiştirilmesi desteklenmez.

Aşağıdaki kod parçacığında gereksinimler dosyası için gerekli biçim gösterilmektedir.

```
absl-py==0.7.0
adal==1.2.1
alabaster==0.7.10
```

## <a name="validate-wheel-files"></a>Tekerlek dosyalarını doğrula
SYNAPSE sunucusuz Apache Spark havuzları, Linux dağıtımını temel alınır. Tekerlek dosyalarını doğrudan PyPI 'dan indirip yüklerken, Linux üzerinde oluşturulmuş ve Spark havuzuyla aynı Python sürümünde çalışan sürümü seçtiğinizden emin olun.

>[!IMPORTANT]
>Özel paketler, oturumlar arasında eklenebilir veya değiştirilebilir. Ancak, güncelleştirilmiş paketi görmek için havuzun ve oturumun yeniden başlatılmasını beklemeniz gerekir.

## <a name="check-for-dependency-conflicts"></a>Bağımlılık çakışmalarını denetle
 Genel olarak, Python bağımlılık çözümlemesi yönetilebilmesi için karmaşık olabilir. Bağımlılık çakışmalarından yerel olarak hata ayıklamanıza yardımcı olmak için SYNAPSE çalışma zamanını temel alarak kendi sanal ortamınızı oluşturabilir ve değişikliklerinizi doğrulayabilirsiniz.

Ortamı yeniden oluşturmak ve güncelleştirmelerinizi doğrulamak için:
 1. SYNAPSE çalışma zamanını yerel olarak yeniden oluşturmak için şablonu [indirin](https://github.com/Azure-Samples/Synapse/blob/main/Spark/Python/base_environment.yml) . Şablon ve gerçek SYNAPSE ortamı arasında hafif farklar olabilir.
   
 2. [Aşağıdaki yönergeleri](https://docs.conda.io/projects/conda/latest/user-guide/tasks/manage-environments.html)kullanarak bir sanal ortam oluşturun. Bu ortam, belirtilen kitaplıklar listesiyle yalıtılmış bir Python yüklemesi oluşturmanıza olanak sağlar. 
    
    ```
    conda myenv create -f environment.yml
    conda activate myenv
    ```
   
 3. ``pip install -r <provide your req.txt file>``Sanal ortamı belirtilen paketleriniz ile güncelleştirmek için kullanın. Yükleme bir hatayla sonuçlandıysa, SYNAPSE temel çalışma zamanına önceden yüklenmiş olan ve belirtilen gereksinimler dosyasında belirtilen özellikler arasında bir çakışma olabilir. Bu bağımlılık çakışmalarının, sunucusuz Apache Spark havuzunuzdaki güncelleştirilmiş kitaplıkları almak için çözümlenmelidir.

## <a name="next-steps"></a>Sonraki adımlar
- Varsayılan kitaplıkları görüntüleme: [Apache Spark sürüm desteği](apache-spark-version-support.md)

