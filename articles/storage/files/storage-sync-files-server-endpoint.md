---
title: Azure Dosya Eşitleme sunucu uç noktası Ekle/Kaldır | Microsoft Docs
description: Azure Dosya Eşitleme bir sunucu uç noktası eklemeyi veya kaldırmayı öğrenin. Sunucu uç noktası, kayıtlı bir sunucuda, sunucu birimindeki bir klasör gibi belirli bir konumdur.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 07/19/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 9273ca66c0304afc5df58ace5dd584c20c90abfd
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87905066"
---
# <a name="addremove-an-azure-file-sync-server-endpoint"></a>Azure Dosya Eşitleme sunucusu uç noktası Ekle/Kaldır
Azure Dosya Eşitleme aracısı şirket içi dosya sunucularının sağladığı esneklik, performans ve uyumluluk özelliklerinden vazgeçmeden kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda toplamanızı sağlar. Bunu, Windows sunucularınızı Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürerek yapar. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilir ve dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

*Sunucu uç noktası* , *kayıtlı bir sunucuda*, sunucu birimindeki bir klasör veya birimin kökü gibi belirli bir konumu temsil eder. Aynı birimde birden çok sunucu uç noktası bulunabilir (örneğin, F:\sync1 ve F:\sync2). Bulut katmanlama ilkelerini, her sunucu uç noktası için ayrı ayrı yapılandırabilirsiniz. Bir eşitleme grubuna sunucu uç noktası olarak var olan bir dosya kümesiyle sunucu konumu eklerseniz, bu dosyalar eşitleme grubundaki diğer uç noktalarda bulunan diğer dosyalarla birleştirilir.

Azure Dosya Eşitleme uçtan uca dağıtma hakkında bilgi için bkz. [Azure dosya eşitleme dağıtma](storage-sync-files-deployment-guide.md) .

## <a name="prerequisites"></a>Önkoşullar
Sunucu uç noktası oluşturmak için öncelikle aşağıdaki ölçütlerin karşılandığından emin olmanız gerekir: 
- Sunucuda Azure Dosya Eşitleme Aracısı yüklü ve kayıtlı. Azure Dosya Eşitleme aracısını yüklemeye yönelik yönergeler [Azure dosya eşitleme ile sunucu kaydetme/kaydını kaldırma](storage-sync-files-server-registration.md) makalesinde bulunabilir. 
- Depolama eşitleme hizmeti 'nin dağıtıldığından emin olun. Depolama eşitleme hizmeti dağıtma hakkında ayrıntılı bilgi için bkz. [nasıl dağıtılır Azure dosya eşitleme](storage-sync-files-deployment-guide.md) . 
- Bir eşitleme grubunun dağıtıldığından emin olun. [Eşitleme grubu oluşturmayı](storage-sync-files-deployment-guide.md#create-a sync-group-and-a-cloud-endpoint)öğrenin.
- Sunucusunun İnternet 'e bağlı olduğundan ve Azure 'un erişilebilir olduğundan emin olun. Sunucu ve hizmetimiz arasındaki tüm iletişimler için 443 numaralı bağlantı noktasını kullanıyoruz.

## <a name="add-a-server-endpoint"></a>Sunucu uç noktası ekleme
Sunucu uç noktası eklemek için, istenen eşitleme grubuna gidin ve "sunucu uç noktası ekle" seçeneğini belirleyin.

![Eşitleme grubu bölmesine yeni bir sunucu uç noktası ekleme](media/storage-sync-files-server-endpoint/add-server-endpoint-1.png)

**Sunucu uç noktası Ekle**altında aşağıdaki bilgiler gereklidir:

- **Kayıtlı sunucu**: sunucu uç noktasının oluşturulacağı sunucu veya kümenin adı.
- **Yol**: eşitleme grubunun bir parçası olarak eşitlenecek Windows Server 'daki yol.
- **Bulut katmanlama**: bulut katmanlamayı etkinleştirmek veya devre dışı bırakmak için bir anahtar. Etkinleştirildiğinde, bulut katmanlama dosyaları Azure dosya paylaşımınıza göre *katmanlaşacaktır* . Bu, sunucunuzdaki alan verimliliğini yönetmenize yardımcı olmak için şirket içi dosya paylaşımlarını veri kümesinin tam bir kopyası yerine bir önbelleğe dönüştürür.
- **Birim boş alanı**: sunucu uç noktasının bulunduğu birimde ayrılacak boş alan miktarı. Örneğin, birim boş alanı tek bir sunucu uç noktası olan bir birimde %50 olarak ayarlandıysa, yaklaşık olarak veri miktarı Azure dosyalarına göre katmanlanacaktır. Bulut katmanlama özelliğinin etkinleştirilip etkinleştirilmediği bağımsız olarak, Azure dosya paylaşımınızda her zaman eşitleme grubundaki verilerin tamamen bir kopyası bulunur.

Sunucu uç noktasını eklemek için **Oluştur** ' u seçin. Bir eşitleme grubunun ad alanı içindeki dosyalar artık eşitlenmiş durumda tutulacak. 

## <a name="remove-a-server-endpoint"></a>Sunucu uç noktasını kaldırma
Belirli bir sunucu uç noktası için Azure Dosya Eşitleme kullanmayı kesmek isterseniz, sunucu uç noktasını kaldırabilirsiniz. 

> [!Warning]  
> Microsoft mühendis tarafından açıkça belirtilmedikçe sunucu uç noktasını kaldırıp yeniden oluşturarak eşitleme, bulut katmanlaması veya Azure Dosya Eşitleme başka bir yönüyle ilgili sorunları gidermeye çalışmayın. Sunucu uç noktasının kaldırılması, bozucu bir işlemdir ve sunucu uç noktası içindeki katmanlı dosyalar, sunucu uç noktası yeniden oluşturulduktan sonra, eşitleme hatalarına neden olacak şekilde Azure dosya paylaşımındaki konumlarına "yeniden bağlanmaz". Ayrıca, sunucu uç noktası ad alanı dışında bulunan katmanlı dosyalar kalıcı olarak kaybolabilir. Bulut katmanlaması hiç etkinleştirilmediği halde, katmanlı dosyalar sunucu uç noktanızla bulunabilir.

Sunucu uç noktasını kaldırmadan önce tüm katmanlı dosyaların geri çekilmiş olduğundan emin olmak için sunucu uç noktasında bulut katmanlamayı devre dışı bırakın ve ardından sunucu uç noktası ad alanı içindeki tüm katmanlı dosyaları yeniden çağırmak için aşağıdaki PowerShell cmdlet 'ini yürütün:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <path-to-to-your-server-endpoint> -Order CloudTieringPolicy
```
Belirtildiğinde `-Order CloudTieringPolicy` , en son değiştirilen dosyalar önce geri alınacaktır.
Dikkate alınması gereken diğer isteğe bağlı ancak yararlı parametreler şunlardır:
* `-ThreadCount`kaç dosyanın paralel olarak geri çağrılabileceğini belirler.
* `-PerFileRetryCount`Şu anda engellenen bir dosya için bir geri çekmenin ne sıklıkta denenmeyeceğini belirler.
* `-PerFileRetryDelaySeconds`yeniden çağırma denemeleri arasındaki saniye cinsinden süreyi belirler ve bir önceki parametreyle birlikte her zaman kullanılmalıdır.

> [!Note]  
> Sunucuyu barındıran yerel birimde, tüm katmanlı verileri yeniden çağırmak için yeterli boş alan yoksa, `Invoke-StorageSyncFileRecall` cmdlet başarısız olur.  

Sunucu uç noktasını kaldırmak için:

1. Sunucunuzun kaydedildiği depolama Eşitleme hizmetine gidin.
2. İstenen eşitleme grubuna gidin.
3. Depolama eşitleme hizmetindeki eşitleme grubunda istediğiniz sunucu uç noktasını kaldırın. Bu, eşitleme grubu bölmesindeki ilgili sunucu uç noktasına sağ tıklanarak gerçekleştirilebilir.

    ![Bir eşitleme grubundan sunucu uç noktası kaldırılıyor](media/storage-sync-files-server-endpoint/remove-server-endpoint-1.png)

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Dosya Eşitleme ile sunucu kaydetme/kaydını silme](storage-sync-files-server-registration.md)
- [Azure Dosya Eşitleme dağıtımını planlama](storage-sync-files-planning.md)
- [Azure Dosya Eşitleme’yi izleme](storage-sync-files-monitoring.md)
