---
title: 'Hızlı başlangıç: iş akışı çalıştırma-Microsoft Genomiks'
description: Bu hızlı başlangıçta giriş verilerini Azure Blob Depolama'ya yükleme ve Microsoft Genomiks hizmeti üzerinden iş akışı çalıştırma adımları gösterilir.
services: genomics
author: grhuynh
manager: cgronlun
ms.author: grhuynh
ms.service: genomics
ms.topic: quickstart
ms.date: 01/11/2019
ms.openlocfilehash: 05b94ca9bd14392bad5288882a80f5c75590ef7b
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76931774"
---
# <a name="quickstart-run-a-workflow-through-the-microsoft-genomics-service"></a>Hızlı başlangıç: Microsoft Genomiks hizmeti ile bir iş akışı çalıştırma

Bu hızlı başlangıçta, giriş verilerini bir Azure Blob depolama hesabına yükler ve Python Genomiks istemcisini kullanarak Microsoft Genomiks hizmeti aracılığıyla bir iş akışı çalıştırırsınız. Microsoft Genomiks, ham okumalardan başlayarak hizalanmış okumalar ve varyant ilanları üreten, bir genomu hızlı bir şekilde işleyebilen ikincil analize yönelik ölçeklenebilir ve güvenli bir hizmettir. 

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio). 
- [Python 2.7.12 +](https://www.python.org/downloads/release/python-2714/), `pip` yüklü ve sistem yolunuzda `python`. Microsoft Genomiks istemcisi Python 3 ile uyumlu değildir. 

## <a name="set-up-create-a-microsoft-genomics-account-in-the-azure-portal"></a>Kurulum: Azure portalında bir Microsoft Genomiks hesabı oluşturma

Bir Microsoft Genomiks hesabı oluşturmak için Azure portal [Genomiks hesabı oluşturma](https://portal.azure.com/#create/Microsoft.Genomics) bölümüne gidin. Azure aboneliğiniz yoksa Microsoft Genomiks hesabı oluşturmadan bir hesap açın. 

![Azure portal Microsoft Genomiks](./media/quickstart-run-genomics-workflow-portal/genomics-create-blade.png "Azure portal Microsoft Genomiks")

Genomiks hesabınızı bir önceki resimde gösterildiği gibi aşağıdaki bilgilerle yapılandırın. 

 |**Ayar**          |  **Önerilen değer**  | **Alan açıklaması** |
 |:-------------       |:-------------         |:----------            |
 |Abonelik         | Aboneliğinizin adı|Bu, Azure hizmetleriniz için faturalandırma birimidir. Aboneliğiniz hakkında ayrıntılı bilgi için bkz. [Abonelikler](https://account.azure.com/Subscriptions) |      
 |Kaynak grubu       | MyResourceGroup       |  Kaynak grupları kolay yönetim için birden fazla Azure kaynağını (depolama hesabı, genomiks hesabı vs.) tek bir grupta toplamanızı sağlar. Daha fazla bilgi için bkz. [Kaynak Grupları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups). Geçerli kaynak grubu adları için bkz. [Adlandırma Kuralları](/azure/architecture/best-practices/resource-naming) |
 |Hesap adı         | MyGenomicsAccount     |Benzersiz bir hesap tanımlayıcı seçin. Geçerli adlar için bkz. [Adlandırma Kuralları](/azure/architecture/best-practices/resource-naming) |
 |Konum                   | Batı ABD 2                    |    Bu hizmet Batı ABD 2, Batı Avrupa ve Güneydoğu Asya konumlarında kullanılabilir |

Dağıtım sürecini izlemek için üstteki menü çubuğunda **Bildirimler** ' i seçebilirsiniz.

![Bildirimler](./media/quickstart-run-genomics-workflow-portal/genomics-notifications-box1.png "Bildirimler")

Microsoft Genomiks hakkında daha fazla bilgi için bkz. [Microsoft Genomiks nedir?](overview-what-is-genomics.md)

## <a name="set-up-install-the-microsoft-genomics-python-client"></a>Kurulum: Microsoft Genomiks Python istemcisini yükleme

Hem Python hem de Microsoft Genomiks Python istemcisini yerel ortamınıza yüklemeniz gerekir. 

### <a name="install-python"></a>Python'ı yükleme

Microsoft Genomiks Python istemcisi Python 2.7.12 veya sonraki bir 2.7. xx sürümü ile uyumludur. 2.7.14 önerilen sürümdür. Dosyayı [buradan](https://www.python.org/downloads/release/python-2714/) indirebilirsiniz. 

> [!IMPORTANT]
> Python 3. x, Python 2.7. xx ile uyumlu değildir.  MSGen bir Python 2.7 uygulamasıdır. MSGen'i çalıştırırken etkin Python ortamınızda Python'un 2.7.xx sürümünün kullanıldığından emin olun. MSGen'i Python 3.x sürümüyle kullanmaya çalışırsanız hatalarla karşılaşabilirsiniz.

### <a name="install-the-microsoft-genomics-client"></a>Microsoft Genomiks istemcisini yükleme

Microsoft Genomiks istemci `msgen`yüklemek için Python `pip` kullanın. Aşağıdaki talimatlarda Python uygulamasının sistem yolunuzda olduğu kabul edilmektedir. `pip` yüklemesi tanınmadığından sorun yaşıyorsanız, sistem yolunuza Python ve Scripts alt klasörünü eklemeniz gerekir.

```
pip install --upgrade --no-deps msgen
pip install msgen
```

`msgen` sistem genelinde bir ikili olarak yüklemek ve sistem genelinde Python paketlerini değiştirmek istemiyorsanız, `pip`ile `–-user` bayrağını kullanın.
Paket tabanlı yükleme veya setup.py dosyasını kullandığınızda gerekli tüm paketler yüklenir. Aksi takdirde, `msgen` için temel gerekli paketler şunlardır 

 * [Azure-storage](https://pypi.python.org/pypi/azure-storage). 
 * [İstekler](https://pypi.python.org/pypi/requests). 

Bu paketleri `pip`, `easy_install` veya standart `setup.py` yordamlarını kullanarak yükleyebilirsiniz. 

### <a name="test-the-microsoft-genomics-client"></a>Microsoft Genomiks istemcisini test etme
Microsoft Genomiks istemcisini test etmek için, Genomiks hesabınızdan yapılandırma dosyasını indirin. Azure portal, sol üstteki **tüm hizmetler** ' i seçip Genomiks hesaplarını arayıp seçip seçerek Genomiks hesabınıza gidin.

![Azure portal Microsoft Genomiks bulun](./media/quickstart-run-genomics-workflow-portal/genomics-filter-box.png "Azure portal Microsoft Genomiks bulun")

Az önce oluşturduğunuz Genomiks hesabını seçin, **erişim anahtarlarına**gidin ve yapılandırma dosyasını indirin.

![Microsoft Genomiks 'den yapılandırma dosyasını indirin](./media/quickstart-run-genomics-workflow-portal/genomics-mygenomicsaccount-box.png "Microsoft Genomiks 'den yapılandırma dosyasını indirin")

Aşağıdaki komutu kullanarak Microsoft Genomiks Python istemcisinin çalışıp çalışmadığını test edin

```Python
msgen list -f "<full path where you saved the config file>"
```

## <a name="create-a-microsoft-azure-storage-account"></a>Microsoft Azure Depolama hesabı oluşturma 
Microsoft Genomiks hizmeti girişlerinin Azure depolama hesabında blok blobları olarak depolanmasını bekler. Ayrıca çıkış dosyalarını Azure depolama hesabında kullanıcı tarafından belirtilen bir kapsayıcıya blok blobları olarak yazar. Girişler ve çıkışlar farklı depolama hesaplarında tutulabilir.
Azure depolama hesabınızda veri varsa Genomiks hesabınızla aynı konumda olduğundan emin olmanız gerekir. Aksi takdirde, Microsoft Genomiks hizmeti çalıştırılırken çıkış ücretleri tahakkuk edilir. Henüz bir Azure depolama hesabınız yoksa, bir tane oluşturmanız ve verilerinizi yüklemeniz gerekir. Depolama hesabının ne olduğu ve hangi hizmetleri sağladığını de kapsayan Azure [depolama hesapları hakkında](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account)daha fazla bilgi edinebilirsiniz. Bir Azure depolama hesabı oluşturmak için Azure portal [depolama hesabı oluştur](https://portal.azure.com/#create/Microsoft.StorageAccount-ARM) ' a gidin.  

![Depolama hesabı oluşturma sayfası](./media/quickstart-run-genomics-workflow-portal/genomics-storage-create-blade1.png "Depolama hesabı oluşturma sayfası")

Depolama hesabınızı önceki görüntüde gösterildiği gibi aşağıdaki bilgilerle yapılandırın. Bir depolama hesabı için standart seçeneklerin çoğunu kullanın, yalnızca hesabın BlobStorage olduğunu ve genel amaçlı değil olduğunu belirtin. Blob depolama indirme ve yükleme işlemlerinde 2-5 kat daha yüksek hız sunabilir.  Varsayılan dağıtım modeli Azure Resource Manager önerilir.  

 |**Ayar**          |  **Önerilen değer**  | **Alan açıklaması** |
 |:-------------------------       |:-------------         |:----------            |
 |Abonelik         | Azure aboneliğiniz |Aboneliğiniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.azure.com/Subscriptions) |      
 |Kaynak grubu       | MyResourceGroup       |  Genomiks hesabınızla aynı kaynak grubunu seçebilirsiniz. Geçerli kaynak grubu adları için bkz. [adlandırma kuralları](/azure/architecture/best-practices/resource-naming) |
 |Depolama hesabı adı         | MyStorageAccount     |Benzersiz bir hesap tanımlayıcı seçin. Geçerli adlar için bkz. [adlandırma kuralları](/azure/architecture/best-practices/resource-naming) |
 |Konum                  | Batı ABD 2                  | Çıkış ücretlerini azaltmak ve gecikmeyi azaltmak için Genomiks hesabınızın konumuyla aynı konumu kullanın.  | 
 |Performans                  | Standart                   | Varsayılan olarak standart seçeneği kullanılır. Standart ve Premium Depolama hesapları hakkında daha fazla bilgi için bkz. [Microsoft Azure depolama 'Ya giriş](https://docs.microsoft.com/azure/storage/common/storage-introduction)    |
 |Hesap türü       | BlobStorage       |  Blob depolama indirme ve yükleme işlemlerinde genel amaçlı depolama alanından 2-5 kat daha yüksek hız sunabilir. |
 |Çoğaltma                  | Yerel olarak yedekli depolama                  | Yerel olarak yedekli depolama, verilerinizi depolama hesabınızı oluşturduğunuz bölgedeki veri merkezi içinde çoğaltır. Daha fazla bilgi için bkz. [Azure Depolama çoğaltma](https://docs.microsoft.com/azure/storage/common/storage-redundancy)    |
 |Erişim katmanı                  | Sık Erişimli                   | Sık erişimli seçeneği, depolama hesabındaki nesnelere erişimin daha sık olduğunu belirtir.    |

Ardından depolama hesabınızı oluşturmak için **gözden geçir + oluştur** ' u seçin. Genomiks hesabınızı oluştururken yaptığınız gibi, dağıtım işlemini izlemek için üstteki menü çubuğunda **Bildirimler** ' i seçebilirsiniz. 

## <a name="upload-input-data-to-your-storage-account"></a>Giriş verilerini depolama hesabınıza yükleyin

Microsoft Genomiks hizmeti, giriş dosyaları olarak eşleştirilmiş uç okuma (fastq veya Baa dosyaları) bekliyor. Kendi verilerinizi yükleyebilir veya sunulan genel kullanıma açık örnek verileri kullanarak hizmeti keşfedebilirsiniz. Genel kullanıma açık örnek verileri kullanmak isterseniz buradan ulaşabilirsiniz:

[https://msgensampledata.blob.core.windows.net/small/chr21_1.fq.gz](https://msgensampledata.blob.core.windows.net/small/chr21_1.fq.gz)
[https://msgensampledata.blob.core.windows.net/small/chr21_2.fq.gz](https://msgensampledata.blob.core.windows.net/small/chr21_2.fq.gz)

Depolama hesabınızda biri giriş verileriniz, biri de çıkış verileriniz için olmak üzere iki blob kapsayıcısı oluşturmanız gerekir.  Giriş verilerini giriş blob kapsayıcısına yükleyin. [Microsoft Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/), [blobporter](https://github.com/Azure/blobporter)veya [AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)dahil olmak üzere çeşitli araçlar kullanılabilir. 

## <a name="run-a-workflow-through-the-microsoft-genomics-service-using-the-python-client"></a>Python istemcisini kullanarak Microsoft Genomiks hizmeti üzerinden iş akışı çalıştırma 

Bir iş akışını Microsoft Genomiks hizmeti üzerinden çalıştırmak için, verileriniz için giriş ve çıkış depolama kapsayıcısını belirtmek üzere *config. txt* dosyasını düzenleyin.
Genomiks hesabınızdan indirdiğiniz *config. txt* dosyasını açın. Belirtmeniz gereken bölümler, hem giriş hem de çıkış için abonelik anahtarınız ve alt kısımdaki altı öğe, depolama hesabı adı, anahtar ve kapsayıcı adıdır. Bu bilgileri, depolama hesabınızın **anahtarlarına erişmek** için Azure Portal giderek veya doğrudan Azure Depolama Gezgini aracılığıyla bulabilirsiniz.  

![Genomiks yapılandırması](./media/quickstart-run-genomics-workflow-portal/genomics-config.png "Genomiks yapılandırması")

GATK4 çalıştırmak istiyorsanız `process_name` parametresini `gatk4`olarak ayarlayın.

Varsayılan olarak, Genomiks hizmeti VCF dosyalarını çıktı. Bir VCF çıkışı yerine bir gVCF çıkışı isterseniz (GATK 3. x içindeki `-emitRefConfidence` eşdeğerdir ve GATK 4. x içindeki `emit-ref-confidence`), *config. txt* dosyasına `emit_ref_confidence` parametresini ekleyin ve yukarıdaki şekilde gösterildiği gibi `gvcf`olarak ayarlayın.  VCF çıktısına geri dönmek için, *config. txt* dosyasından kaldırın ya da `emit_ref_confidence` parametresini `none`olarak ayarlayın. 

### <a name="submit-your-workflow-to-the-microsoft-genomics-service-the-microsoft-genomics-client"></a>İş akışınızı Microsoft Genomiks hizmetine ve Microsoft Genomiks istemcisine gönderme

Aşağıdaki komutu kullanarak Microsoft Genomiks Python istemcisiyle iş akışınızı gönderin:

```python
msgen submit -f [full path to your config file] -b1 [name of your first paired end read] -b2 [name of your second paired end read]
```

İş akışlarınızın durumunu görüntülemek için aşağıdaki komutu kullanabilirsiniz: 
```python
msgen list -f c:\temp\config.txt 
```

İş akışınız tamamlandığında, Azure Depolama hesabınızdaki çıktı dosyalarını yapılandırdığınız çıkış kapsayıcısında görüntüleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure depolama 'ya örnek giriş verileri yüklediniz ve `msgen` Python istemcisi üzerinden Microsoft Genomiks hizmetine bir iş akışı gönderdiniz. Microsoft Genomiks hizmeti ile kullanılabilecek diğer giriş dosya türleri hakkında daha fazla bilgi için şu sayfalara bakın: [eşleştirilmiş FASTQ](quickstart-input-pair-FASTQ.md) | [BAM](quickstart-input-BAM.md) | [Çoklu FASTQ veya BAM](quickstart-input-multiple.md). Bu öğreticiyi [Azure not defteri öğreticimizi](https://aka.ms/genomicsnotebook) kullanarak da keşfedebilirsiniz.
