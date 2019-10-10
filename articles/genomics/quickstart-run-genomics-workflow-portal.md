---
title: 'Hızlı başlangıç: iş akışını çalıştırma'
description: Hızlı başlangıç, giriş verilerinin Azure Blob depolama alanına nasıl yükleneceğini ve Microsoft Genomiks hizmeti aracılığıyla bir iş akışını nasıl çalıştıracağınızı gösterir.
services: genomics
author: grhuynh
manager: cgronlun
ms.author: grhuynh
ms.service: genomics
ms.topic: quickstart
ms.date: 01/11/2019
ms.openlocfilehash: e7c90cc0ce85f2a90cc2ddc2cd086fd2626f4d96
ms.sourcegitcommit: 961468fa0cfe650dc1bec87e032e648486f67651
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72248532"
---
# <a name="quickstart-run-a-workflow-through-the-microsoft-genomics-service"></a>Hızlı başlangıç: Microsoft Genomiks hizmeti üzerinden iş akışı çalıştırma

Bu hızlı başlangıçta, giriş verilerinin Azure Blob depolama alanına nasıl yükleneceği ve Microsoft Genomiks hizmeti aracılığıyla bir iş akışının nasıl çalıştırılacağı gösterilmektedir. Microsoft Genomiks, ham okumadan başlayıp hizalanmış okuma ve varyant çağrıları üreten bir genom 'yi hızlı bir şekilde işleyebilen, ikincil analize yönelik ölçeklenebilir ve güvenli bir hizmettir. 

Yalnızca birkaç adımda kullanmaya başlayın: 
1.  Ayarla: Azure portal aracılığıyla bir Microsoft Genomiks hesabı oluşturun ve Microsoft Genomiks Python istemcisini yerel ortamınıza kurun. 
2.  Giriş verilerini karşıya yükleme: Azure portal aracılığıyla Microsoft Azure depolama hesabı oluşturun ve giriş dosyalarını karşıya yükleyin. Giriş dosyaları, son okuma (fastq veya Bad dosyaları) ile eşleştirilmelidir.
3.  Çalıştır: Microsoft Genomiks hizmeti aracılığıyla iş akışlarını çalıştırmak için Microsoft Genomiks komut satırı arabirimini kullanın. 

Microsoft Genomiks hakkında daha fazla bilgi için bkz. [Microsoft Genomiks nedir?](overview-what-is-genomics.md)

## <a name="set-up-create-a-microsoft-genomics-account-in-the-azure-portal"></a>Kurulum: Azure portal bir Microsoft Genomiks hesabı oluşturun

Bir Microsoft Genomiks hesabı oluşturmak için [Azure Portal](https://portal.azure.com/#create/Microsoft.Genomics)gidin. Henüz bir Azure aboneliğiniz yoksa, bir Microsoft Genomiks hesabı oluşturmadan önce bir tane oluşturun. 

(./media/quickstart-run-genomics-workflow-portal/genomics-create-blade.png "Azure Portal Microsoft genomikon") ![Azure Portal Microsoft Genomiks]



Genomiks hesabınızı, önceki görüntüde gösterildiği gibi aşağıdaki bilgilerle yapılandırın. 

 |**Ayarlanmasını**          |  **Önerilen değer**  | **Alan açıklaması** |
 |:-------------       |:-------------         |:----------            |
 |Aboneliğiniz         | Abonelik adınız|Bu, Azure hizmetlerinizin fatura birimidir. aboneliğiniz hakkındaki ayrıntılar Için bkz. [abonelikler](https://account.azure.com/Subscriptions) |      
 |Kaynak grubu       | MyResourceGroup       |  Kaynak grupları, birden çok Azure kaynağını (depolama hesabı, Genomiks hesabı vb.) basit yönetim için tek bir gruba gruplandırmalarına olanak sağlar. Daha fazla bilgi için bkz. [kaynak grupları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups). Geçerli kaynak grubu adları için bkz. [adlandırma kuralları](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) |
 |Hesap adı         | MyGenomicsAccount     |Benzersiz bir hesap tanımlayıcısı seçin. Geçerli adlar için bkz. [adlandırma kuralları](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) |
 |Konum                   | Batı ABD 2                    |    Hizmet Batı ABD 2, Batı Avrupa ve Güneydoğu Asya 'da kullanılabilir |




Dağıtım sürecini izlemek için üstteki menü çubuğunda bildirimler ' e tıklayabilirsiniz.
Microsoft ![Genomiks bildirimleri](./media/quickstart-run-genomics-workflow-portal/genomics-notifications-box.png "Microsoft Genomiks bildirimleri")



## <a name="set-up-install-the-microsoft-genomics-python-client"></a>Ayarla: Microsoft Genomiks Python istemcisini yükler

Kullanıcıların hem Python hem de Microsoft Genomiks Python istemcisini yerel ortamlarına yüklemeleri gerekir. 

### <a name="install-python"></a>Python 'ı yükler

Microsoft Genomiks Python istemcisi Python 2,7 ile uyumludur. 12 veya üzeri 2.7. xx sürümü; 2.7.15, bu yazma sırasında en son sürümdür; 2.7.14 önerilen sürümdür. İndirmeyi [buradan](https://www.python.org/downloads/)bulabilirsiniz. 

NOTE: Python 3. x, Python 2.7. xx ile uyumlu değildir.  MSGen, bir Python 2,7 uygulamasıdır. MSGen çalıştırırken, etkin Python ortamınızın Python 'un 2.7. xx sürümünü kullandığınızdan emin olun. Python 'un 3. x sürümüyle MSGen kullanmaya çalışırken hata alabilirsiniz.


### <a name="install-the-microsoft-genomics-client"></a>Microsoft Genomiks istemcisini yükler

Microsoft Genomiks istemcisini yüklemek için Python PIP `msgen` kullanın. Aşağıdaki yönergeler Python 'un zaten sistem yolunuzda olduğunu varsayar. PIP yüklemesi tanınmadığından sorunlarla karşılaşırsanız, sistem yolunuza Python ve Scripts alt klasörünü eklemeniz gerekir.


```
pip install --upgrade --no-deps msgen
pip install msgen
```


@No__t-0 ' ı bir sistem genelinde ikili olarak yüklemek ve sistem genelinde Python paketlerini değiştirmek istemiyorsanız, `pip` ile `–-user` bayrağını kullanın.
Paket tabanlı yükleme veya setup.py kullanıyorsanız, gerekli tüm gerekli paketler yüklenir. Aksi halde, msgen 'i için gereken temel paketler şunlardır 

 * [Azure-Storage](https://pypi.python.org/pypi/azure-storage). 
 * [İstekleri](https://pypi.python.org/pypi/requests). 


Bu paketleri `pip`, `easy_install` ' i veya standart `setup.py` yordamlarını kullanarak yükleyebilirsiniz. 





### <a name="test-the-microsoft-genomics-client"></a>Microsoft Genomiks istemcisini test etme
Microsoft Genomiks istemcisini test etmek için, Genomiks hesabınızdan yapılandırma dosyasını indirin. Sol üstteki, filtreleyerek ve Genomiks hesapları için seçim yaparak Genomiks hesabınıza gidin.


Microsoft(./media/quickstart-run-genomics-workflow-portal/genomics-filter-box.png "Genomiks için Azure Portal Filtreinden") ![Microsoft genomiks için filtrele]Azure Portal



Az önce yaptığınız Genomiks hesabını seçin, **erişim anahtarlarına** gidin ve yapılandırma dosyasını indirin.

Microsoft Genomiks 'den Microsoft Genomiks(./media/quickstart-run-genomics-workflow-portal/genomics-mygenomicsaccount-box.png "indirme yapılandırma dosyasından") ![yapılandırma dosyasını indirin]


Microsoft Genomiks Python istemcisinin aşağıdaki komutla çalıştığını test edin


```
msgen list -f “<full path where you saved the config file>”
```

## <a name="create-a-microsoft-azure-storage-account"></a>Microsoft Azure Depolama hesabı oluşturma 
Microsoft Genomiks hizmeti girişlerin bir Azure depolama hesabında blok Blobları olarak depolanmasını bekler. Ayrıca, çıkış dosyalarını bir Azure depolama hesabındaki Kullanıcı tarafından belirtilen kapsayıcıya blok Blobları olarak yazar. Girişler ve çıktılar farklı depolama hesaplarında bulunabilir.
Verileriniz zaten bir Azure depolama hesabında bulunuyorsa, yalnızca Genomiks hesabınızla aynı konumda olduğundan emin olmanız gerekir. Aksi takdirde, Genomiks hizmeti çalıştırılırken çıkış ücretleri tahakkuk edilir. Henüz bir Microsoft Azure Depolama hesabınız yoksa, bir tane oluşturmanız ve verilerinizi yüklemeniz gerekir. Depolama hesabının ne olduğu ve hangi hizmetleri sağladığını de kapsayan Azure [depolama hesapları hakkında](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account)daha fazla bilgi edinebilirsiniz. Microsoft Azure Depolama bir hesap oluşturmak için [Azure Portal](https://portal.azure.com/#create/Microsoft.StorageAccount-ARM )gidin.  

![Depolama alanı oluşturma dikey]penceresi(./media/quickstart-run-genomics-workflow-portal/genomics-storage-create-blade.png "oluşturma dikey") penceresi

Depolama hesabınızı önceki görüntüde gösterildiği gibi aşağıdaki bilgilerle yapılandırın. Bir depolama hesabı için standart seçeneklerin çoğunu kullanın, yalnızca hesabın BLOB depolama olduğunu, genel amaçlı değil ' i belirtin. BLOB depolama, indirme ve karşıya yükleme işlemleri için 2-5x daha hızlı olabilir.  Varsayılan dağıtım modeli Azure Resource Manager önerilir.  


 |**Ayarlanmasını**          |  **Önerilen değer**  | **Alan açıklaması** |
 |:-------------------------       |:-------------         |:----------            |
 |Aboneliğiniz         | Azure aboneliğiniz |Aboneliğiniz hakkındaki ayrıntılar için bkz. [abonelikler](https://account.azure.com/Subscriptions) |      
 |Kaynak grubu       | MyResourceGroup       |  Genomiks hesabınızla aynı kaynak grubunu seçebilirsiniz. Geçerli kaynak grubu adları için bkz. [adlandırma kuralları](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) |
 |Depolama hesabı adı         | MyStorageAccount     |Benzersiz bir hesap tanımlayıcısı seçin. Geçerli adlar için bkz. [adlandırma kuralları](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) |
 |Konum                  | Batı ABD 2                  | Çıkış ücretlerini azaltmak ve gecikmeyi azaltmak için Genomiks hesabınızın konumuyla aynı konumu kullanın.  | 
 |Performans                  | Stand                   | Varsayılan değer standarttır. Standart ve Premium Depolama hesapları hakkında daha fazla bilgi için bkz. [tanıtım Microsoft Azure depolama](https://docs.microsoft.com/azure/storage/common/storage-introduction)    |
 |Hesap türü       | Blob depolama       |  BLOB depolama, indirme ve karşıya yükleme işlemleri için genel amaçlı olarak 2-5x daha hızlı olabilir. |
 |Yinelemesi                  | Yerel olarak yedekli depolama                  | Yerel olarak yedekli depolama, verilerinizi depolama hesabınızı oluşturduğunuz bölgedeki veri merkezi içinde çoğaltır. Daha fazla bilgi için bkz. [Azure Storage çoğaltma](https://docs.microsoft.com/azure/storage/common/storage-redundancy)    |
 |Erişim katmanı                  | Kolay                   | Sık erişimli erişim, depolama hesabındaki nesnelerin daha sık erişildiği anlamına gelir.    |


Depolama hesabınızı oluşturmak için `Review + create` ' a tıklayın. Genomiks hesabınızı oluştururken yaptığınız gibi, dağıtım işlemini izlemek için üst menü çubuğundaki bildirimler ' e tıklayabilirsiniz. 


## <a name="upload-input-data-to-your-storage-account"></a>Giriş verilerini depolama hesabınıza yükleme

Microsoft Genomiks hizmeti, eşleştirilmiş bitiş okumalarını giriş dosyaları olarak bekliyor. Kendi verilerinizi karşıya yüklemeyi seçebilir veya sizin için sağlanan genel kullanıma açık örnek verileri kullanmayı keşfedebilirsiniz. Genel kullanıma açık örnek verileri kullanmak isterseniz, burada barındırılır:


[https://msgensampledata.blob.core.windows.net/small/chr21_1.fq.gz](https://msgensampledata.blob.core.windows.net/small/chr21_1.fq.gz)
[https://msgensampledata.blob.core.windows.net/small/chr21_2.fq.gz](https://msgensampledata.blob.core.windows.net/small/chr21_2.fq.gz)


Depolama hesabınızda, giriş verileriniz için bir blob kapsayıcısı ve çıkış verileriniz için ikinci bir blob kapsayıcısı oluşturmanız gerekir.  Giriş verilerini giriş blobu kapsayıcınıza yükleyin. [Microsoft Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/), [blobporter](https://github.com/Azure/blobporter)veya [AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)dahil olmak üzere çeşitli araçlar kullanılabilir. 



## <a name="run-a-workflow-through-the-microsoft-genomics-service-using-the-python-client"></a>Python istemcisini kullanarak Microsoft Genomiks hizmeti üzerinden iş akışı çalıştırma 

Bir iş akışını Microsoft Genomiks hizmeti üzerinden çalıştırmak için, verileriniz için giriş ve çıkış depolama kapsayıcısını belirtmek üzere config. txt dosyasını düzenleyin.
Genomiks hesabınızdan indirdiğiniz config. txt dosyasını açın. Belirtmeniz gereken bölümler, abonelik anahtarınız ve en alttaki altı öğe, hem giriş hem de çıkış için depolama hesabı adı, anahtar ve kapsayıcı adıdır. Bu bilgileri, depolama hesabınıza yönelik anahtarlara veya doğrudan Azure Depolama Gezgini **erişmek** için portalda gezinerek bulabilirsiniz.  


![Genomiks yapılandırması](./media/quickstart-run-genomics-workflow-portal/genomics-config.png "Genomiks yapılandırması")


GATK4 çalıştırmak istiyorsanız, `process_name` parametresini `gatk4` olarak ayarlayın.

Varsayılan olarak, Genomiks hizmeti VCF dosyalarını çıktı. Bir VCF çıkışı yerine bir gVCF çıkışı istiyorsanız (GATK 3. x içinde `-emitRefConfidence` ve GATK 4. x içinde `emit-ref-confidence` ile eşdeğer), `emit_ref_confidence` parametresini `config.txt` ' e ekleyin ve yukarıdaki şekilde gösterildiği gibi `gvcf` olarak ayarlayın.  VCF çıktısına geri dönmek için `config.txt` dosyasından kaldırın ya da `emit_ref_confidence` parametresini `none` olarak ayarlayın. 

### <a name="submit-your-workflow-to-the-microsoft-genomics-service-the-microsoft-genomics-client"></a>Microsoft Genomiks istemcisine iş akışınızı Microsoft Genomiks hizmetine gönderme

Aşağıdaki komutla iş akışınızı göndermek için Microsoft Genomiks Python istemcisini kullanın:


```python
msgen submit -f [full path to your config file] -b1 [name of your first paired end read] -b2 [name of your second paired end read]
```


Aşağıdaki komutu kullanarak İş akışlarınızın durumunu görüntüleyebilirsiniz: 
```python
msgen list -f c:\temp\config.txt 
```


İş akışınız tamamlandığında, Azure Depolama hesabınızdaki çıktı dosyalarını yapılandırdığınız çıkış kapsayıcısında görüntüleyebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure depolama 'ya örnek giriş verileri yüklediniz ve `msgen` Python istemcisi üzerinden Microsoft Genomiks hizmetine bir iş akışı gönderdiniz. Microsoft Genomiks hizmeti ile kullanılabilecek diğer giriş dosya türleri hakkında daha fazla bilgi edinmek için şu sayfalara bakın: [EŞLEŞTIRILMIŞ fastq](quickstart-input-pair-FASTQ.md) | [BAE](quickstart-input-BAM.md) | [Çoklu fastq veya Bad](quickstart-input-multiple.md). [Azure Not defteri Öğreticimizi](https://aka.ms/genomicsnotebook) kullanarak da bu öğreticiyi inceleyebilirsiniz.
