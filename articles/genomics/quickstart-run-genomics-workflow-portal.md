---
title: 'Quickstart: İş akışı çalıştırma - Microsoft Genomik'
description: Bu hızlı başlangıçta giriş verilerini Azure Blob Depolama'ya yükleme ve Microsoft Genomiks hizmeti üzerinden iş akışı çalıştırma adımları gösterilir.
services: genomics
author: grhuynh
manager: cgronlun
ms.author: grhuynh
ms.service: genomics
ms.topic: quickstart
ms.date: 01/11/2019
ms.openlocfilehash: 05b94ca9bd14392bad5288882a80f5c75590ef7b
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76931774"
---
# <a name="quickstart-run-a-workflow-through-the-microsoft-genomics-service"></a>Hızlı Başlangıç: Microsoft Genomiks hizmeti üzerinden iş akışı çalıştırma

Bu hızlı başlangıçta, giriş verilerini bir Azure Blob depolama hesabına yükler ve Python Genomics istemcisini kullanarak Microsoft Genomics hizmeti nde bir iş akışı çalıştırırsınız. Microsoft Genomiks, ham okumalardan başlayarak hizalanmış okumalar ve varyant ilanları üreten, bir genomu hızlı bir şekilde işleyebilen ikincil analize yönelik ölçeklenebilir ve güvenli bir hizmettir. 

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) 
- [Python 2.7.12+](https://www.python.org/downloads/release/python-2714/) `pip` , yüklü `python` ve sistem yolunuzda. Microsoft Genomics istemcisi Python 3 ile uyumlu değildir. 

## <a name="set-up-create-a-microsoft-genomics-account-in-the-azure-portal"></a>Kurulum: Azure portalında bir Microsoft Genomiks hesabı oluşturma

Bir Microsoft Genomics hesabı oluşturmak için Azure portalında [bir Genomik hesabı oluşturun'](https://portal.azure.com/#create/Microsoft.Genomics) a gidin. Azure aboneliğiniz yoksa Microsoft Genomiks hesabı oluşturmadan bir hesap açın. 

![Azure portalında Microsoft Genomik](./media/quickstart-run-genomics-workflow-portal/genomics-create-blade.png "Azure portalında Microsoft Genomik")

Genomiks hesabınızı bir önceki resimde gösterildiği gibi aşağıdaki bilgilerle yapılandırın. 

 |**Ayar**          |  **Önerilen değer**  | **Alan açıklaması** |
 |:-------------       |:-------------         |:----------            |
 |Abonelik         | Aboneliğinizin adı|Bu, Azure hizmetleriniz için faturalandırma birimidir. Aboneliğiniz hakkında ayrıntılı bilgi için bkz. [Abonelikler](https://account.azure.com/Subscriptions) |      
 |Kaynak grubu       | MyResourceGroup       |  Kaynak grupları kolay yönetim için birden fazla Azure kaynağını (depolama hesabı, genomiks hesabı vs.) tek bir grupta toplamanızı sağlar. Daha fazla bilgi için bkz. [Kaynak Grupları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups). Geçerli kaynak grubu adları için bkz. [Adlandırma Kuralları](/azure/architecture/best-practices/resource-naming) |
 |Hesap adı         | MyGenomicsAccount     |Benzersiz bir hesap tanımlayıcı seçin. Geçerli adlar için bkz. [Adlandırma Kuralları](/azure/architecture/best-practices/resource-naming) |
 |Konum                   | Batı ABD 2                    |    Bu hizmet Batı ABD 2, Batı Avrupa ve Güneydoğu Asya konumlarında kullanılabilir |

Dağıtım işlemini izlemek için üst menü çubuğunda **Bildirimler'i** seçebilirsiniz.

![Bildirimler](./media/quickstart-run-genomics-workflow-portal/genomics-notifications-box1.png "Bildirimler")

Microsoft Genomik hakkında daha fazla bilgi için [bkz.](overview-what-is-genomics.md)

## <a name="set-up-install-the-microsoft-genomics-python-client"></a>Kurulum: Microsoft Genomiks Python istemcisini yükleme

Hem Python'u hem de Microsoft Genomics Python istemcisini yerel ortamınıza yüklemeniz gerekir. 

### <a name="install-python"></a>Python'ı yükleme

Microsoft Genomics Python istemcisi Python 2.7.12 veya daha sonraki 2.7.xx sürümüyle uyumludur. 2.7.14 önerilen sürümüdür. Dosyayı [buradan](https://www.python.org/downloads/release/python-2714/) indirebilirsiniz. 

> [!IMPORTANT]
> Python 3.x, Python 2.7.xx ile uyumlu değildir.  MSGen bir Python 2.7 uygulamasıdır. MSGen'i çalıştırırken etkin Python ortamınızda Python'un 2.7.xx sürümünün kullanıldığından emin olun. MSGen'i Python 3.x sürümüyle kullanmaya çalışırsanız hatalarla karşılaşabilirsiniz.

### <a name="install-the-microsoft-genomics-client"></a>Microsoft Genomiks istemcisini yükleme

Microsoft `pip` Genomics istemcisini `msgen`yüklemek için Python'u kullanın. Aşağıdaki talimatlarda Python uygulamasının sistem yolunuzda olduğu kabul edilmektedir. Tanımlanmamış yükleme `pip` ile ilgili sorunlarınız varsa, sistem yolunuza Python ve komut dosyaları alt klasörünü eklemeniz gerekir.

```
pip install --upgrade --no-deps msgen
pip install msgen
```

Sistem genelinde ikili olarak `msgen` yüklemek ve sistem genelinde Python paketlerini değiştirmek `–-user` istemiyorsanız, `pip`bayrağı .
Paket tabanlı yükleme veya setup.py dosyasını kullandığınızda gerekli tüm paketler yüklenir. Aksi takdirde, temel `msgen` gerekli paketler 

 * [Azure depolama](https://pypi.python.org/pypi/azure-storage). 
 * [İstekler](https://pypi.python.org/pypi/requests). 

Bu paketleri `pip`, `easy_install` veya standart `setup.py` yordamlarını kullanarak yükleyebilirsiniz. 

### <a name="test-the-microsoft-genomics-client"></a>Microsoft Genomiks istemcisini test etme
Microsoft Genomics istemcisini test etmek için Config dosyasını Genomik hesabınızdan indirin. Azure portalında, sol üstteki **Tüm hizmetleri** seçerek ve ardından Genomik hesaplarını arayarak ve seçerek Genomik hesabınıza gidin.

![Azure portalında Microsoft Genomics'i bulun](./media/quickstart-run-genomics-workflow-portal/genomics-filter-box.png "Azure portalında Microsoft Genomics'i bulun")

Az önce yaptığınız Genomik hesabını seçin, **Access Keys'e**gidin ve yapılandırma dosyasını indirin.

![Microsoft Genomics'ten config dosyasını indirin](./media/quickstart-run-genomics-workflow-portal/genomics-mygenomicsaccount-box.png "Microsoft Genomics'ten config dosyasını indirin")

Aşağıdaki komutu kullanarak Microsoft Genomiks Python istemcisinin çalışıp çalışmadığını test edin

```Python
msgen list -f "<full path where you saved the config file>"
```

## <a name="create-a-microsoft-azure-storage-account"></a>Microsoft Azure Depolama hesabı oluşturma 
Microsoft Genomiks hizmeti girişlerinin Azure depolama hesabında blok blobları olarak depolanmasını bekler. Ayrıca çıkış dosyalarını Azure depolama hesabında kullanıcı tarafından belirtilen bir kapsayıcıya blok blobları olarak yazar. Girişler ve çıkışlar farklı depolama hesaplarında tutulabilir.
Azure depolama hesabınızda veri varsa Genomiks hesabınızla aynı konumda olduğundan emin olmanız gerekir. Aksi takdirde, Microsoft Genomics hizmetini çalıştırırken çıkış ücretleri tahakkuk eder. Henüz bir Azure depolama hesabınız yoksa, bir tane oluşturmanız ve verilerinizi yüklemeniz gerekir. Azure depolama hesapları hakkında daha fazla bilgiyi [(depolama](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account)hesabının ne olduğu ve hangi hizmetleri sağladığı dahil) burada bulabilirsiniz. Bir Azure depolama hesabı oluşturmak için Azure portalında [depolama hesabı oluştur'a](https://portal.azure.com/#create/Microsoft.StorageAccount-ARM) gidin.  

![Depolama hesabı oluşturma sayfası](./media/quickstart-run-genomics-workflow-portal/genomics-storage-create-blade1.png "Depolama hesabı oluşturma sayfası")

Depolama hesabınızı, önceki resimde gösterildiği gibi aşağıdaki bilgilerle yapılandırın. Bir depolama hesabı için standart seçeneklerin çoğunu kullanın, yalnızca hesabın genel amaç değil BlobStorage olduğunu belirtin. Blob depolama indirme ve yükleme işlemlerinde 2-5 kat daha yüksek hız sunabilir.  Varsayılan dağıtım modeli, Azure Kaynak Yöneticisi önerilir.  

 |**Ayar**          |  **Önerilen değer**  | **Alan açıklaması** |
 |:-------------------------       |:-------------         |:----------            |
 |Abonelik         | Azure aboneliğiniz |Aboneliğiniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.azure.com/Subscriptions) |      
 |Kaynak grubu       | MyResourceGroup       |  Genomik hesabınızla aynı kaynak grubunu seçebilirsiniz. Geçerli kaynak grubu adları için Bkz. [Adlandırma kuralları](/azure/architecture/best-practices/resource-naming) |
 |Depolama hesabı adı         | MyStorageAccount     |Benzersiz bir hesap tanımlayıcı seçin. Geçerli adlar için [Bkz. Adlandırma kuralları](/azure/architecture/best-practices/resource-naming) |
 |Konum                  | Batı ABD 2                  | Çıkış ücretlerini azaltmak ve gecikme süresini azaltmak için Genomik hesabınızın konumuyla aynı konumu kullanın.  | 
 |Performans                  | Standart                   | Varsayılan olarak standart seçeneği kullanılır. Standart ve birinci sınıf depolama hesapları hakkında daha fazla bilgi için Microsoft [Azure depolama alanına Giriş'e](https://docs.microsoft.com/azure/storage/common/storage-introduction) bakın    |
 |Hesap türü       | BlobDepolama       |  Blob depolama indirme ve yükleme işlemlerinde genel amaçlı depolama alanından 2-5 kat daha yüksek hız sunabilir. |
 |Çoğaltma                  | Yerel olarak yedekli depolama                  | Yerel olarak yedekli depolama, verilerinizi depolama hesabınızı oluşturduğunuz bölgedeki veri merkezi içinde çoğaltır. Daha fazla bilgi için bkz: [Azure Depolama çoğaltma](https://docs.microsoft.com/azure/storage/common/storage-redundancy)    |
 |Erişim katmanı                  | Sık Erişimli                   | Sık erişimli seçeneği, depolama hesabındaki nesnelere erişimin daha sık olduğunu belirtir.    |

Ardından, depolama hesabınızı oluşturmak için **Gözden Geçir + oluştur'u** seçin. Genomik hesabınızın oluşturulmasında yaptığınız gibi, dağıtım işlemini izlemek için en üst menü çubuğunda **Bildirimler'i** seçebilirsiniz. 

## <a name="upload-input-data-to-your-storage-account"></a>Giriş verilerini depolama hesabınıza yükleyin

Microsoft Genomics hizmeti, giriş dosyaları olarak eşleştirilmiş uç okumalarını (fastq veya bam dosyaları) bekler. Kendi verilerinizi yükleyebilir veya sunulan genel kullanıma açık örnek verileri kullanarak hizmeti keşfedebilirsiniz. Genel kullanıma açık örnek verileri kullanmak isterseniz buradan ulaşabilirsiniz:

[https://msgensampledata.blob.core.windows.net/small/chr21_1.fq.gz](https://msgensampledata.blob.core.windows.net/small/chr21_1.fq.gz)
[https://msgensampledata.blob.core.windows.net/small/chr21_2.fq.gz](https://msgensampledata.blob.core.windows.net/small/chr21_2.fq.gz)

Depolama hesabınızda biri giriş verileriniz, biri de çıkış verileriniz için olmak üzere iki blob kapsayıcısı oluşturmanız gerekir.  Giriş verilerini giriş blob kapsayıcısına yükleyin. [Microsoft Azure Depolama Gezgini,](https://azure.microsoft.com/features/storage-explorer/) [BlobPorter](https://github.com/Azure/blobporter)veya [AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)dahil olmak üzere bunu yapmak için çeşitli araçlar kullanılabilir. 

## <a name="run-a-workflow-through-the-microsoft-genomics-service-using-the-python-client"></a>Python istemcisini kullanarak Microsoft Genomiks hizmeti üzerinden iş akışı çalıştırma 

Microsoft Genomics hizmetinde bir iş akışı çalıştırmak için, verileriniz için giriş ve çıktı depolama kapsayıcısını belirtmek için *config.txt* dosyasını tıklatın.
Genomik hesabınızdan indirdiğiniz *config.txt* dosyasını açın. Belirtmeniz gereken bölümler abonelik anahtarınız ve alttaki altı öğe, hem giriş hem de çıktı için depolama hesabı adı, anahtar ve kapsayıcı adıdır. Bu bilgileri, Azure portalında gezinmek ve depolama hesabınız için **Erişim tuşlarına** veya doğrudan Azure Depolama Gezgini'nden bulabilirsiniz.  

![Genomik config](./media/quickstart-run-genomics-workflow-portal/genomics-config.png "Genomik config")

GATK4'i çalıştırmak istiyorsanız, parametreyi `process_name` `gatk4`' ye ayarlayın

Varsayılan olarak, Genomik hizmeti VCF dosyalarını çıkartır. VCF çıkışı yerine bir gVCF çıkışı istiyorsanız `-emitRefConfidence` (GATK 3.x `emit-ref-confidence` ve GATK 4.x'e eşdeğer), parametreyi `emit_ref_confidence` *config.txt'nize* ekleyin ve önceki şekilde gösterildiği gibi ayarlayın. `gvcf`  VCF çıktısına geri dönmek için *config.txt* dosyasından kaldırın `emit_ref_confidence` veya `none`parametreyi 'ye ayarlayın 

### <a name="submit-your-workflow-to-the-microsoft-genomics-service-the-microsoft-genomics-client"></a>İş akışınızı Microsoft Genomiks hizmetine ve Microsoft Genomiks istemcisine gönderme

Aşağıdaki komutu kullanarak Microsoft Genomiks Python istemcisiyle iş akışınızı gönderin:

```python
msgen submit -f [full path to your config file] -b1 [name of your first paired end read] -b2 [name of your second paired end read]
```

İş akışlarınızın durumunu görüntülemek için aşağıdaki komutu kullanabilirsiniz: 
```python
msgen list -f c:\temp\config.txt 
```

İş akışınız tamamlandıktan sonra, Azure depolama hesabınızdaki çıktı dosyalarını yapılandırdığınız çıktı kapsayıcısında görüntüleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, örnek giriş verilerini Azure depolama alanına yükledin ve Python istemcisi `msgen` aracılığıyla Microsoft Genomics hizmetine bir iş akışı gönderdiniz. Microsoft Genomics hizmetiyle kullanılabilecek diğer giriş dosya türleri hakkında daha fazla bilgi edinmek için aşağıdaki sayfalara bakın: [eşleştirilmiş FASTQ](quickstart-input-pair-FASTQ.md) | [BAM](quickstart-input-BAM.md) | [Multiple FASTQ veya BAM](quickstart-input-multiple.md). Bu öğreticiyi [Azure not defteri öğreticimizi](https://aka.ms/genomicsnotebook) kullanarak da keşfedebilirsiniz.
