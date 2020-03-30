---
title: Azure Dosya Eşitleme sunucusu bitiş noktası ekleme/kaldırma | Microsoft Dokümanlar
description: Azure Dosyaları dağıtımı planlarken nelere dikkat etmen gerektiğini öğrenin.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 07/19/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 684b30a24e049722cb531cbc84e3a2cd90912ec8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255112"
---
# <a name="addremove-an-azure-file-sync-server-endpoint"></a>Azure Dosya Eşitleme sunucusu bitiş noktası ekleme/kaldırma
Azure Dosya Eşitleme aracısı şirket içi dosya sunucularının sağladığı esneklik, performans ve uyumluluk özelliklerinden vazgeçmeden kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda toplamanızı sağlar. Bunu, Windows Sunucularınızı Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürerek yapar. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilir ve dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

*Sunucu bitiş noktası,* kayıtlı bir *sunucudaki*sunucu hacmindeki klasör veya birimin kökü gibi belirli bir konumu temsil eder. Ad alanları çakışmıyorsa aynı birimde birden çok sunucu uç noktası bulunabilir (örneğin, F:\sync1 ve F:\sync2). Her sunucu bitiş noktası için bulut katmanlama ilkelerini ayrı ayrı yapılandırabilirsiniz. Eşitleme grubuna sunucu bitiş noktası olarak varolan bir dosya kümesini içeren bir sunucu konumu eklerseniz, bu dosyalar eşitleme grubundaki diğer uç noktalardaki diğer dosyalarla birleştirilir.

Azure [Dosya Eşitlemi'nin](storage-sync-files-deployment-guide.md) uçuca nasıl dağıtılanabildiğini öğrenmek için Azure Dosya Eşitlemeyi'ni nasıl dağıtılayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bir sunucu bitiş noktası oluşturmak için öncelikle aşağıdaki ölçütlerin karşılandığından emin olmalısınız: 
- Sunucu, Azure Dosya Eşitleme aracısını yüklü yormuş ve kaydedilmiştir. Azure Dosya Eşitleme Aracısı'nı yükleme yönergeleri, [Azure Dosya Eşitleme makalesine sahip bir sunucuyu kaydet/kayıt dışı olarak](storage-sync-files-server-registration.md) kaydettirme de bulunabilir. 
- Bir Depolama Eşitleme Hizmetinin dağıtıldığından emin olun. Depolama Eşitleme Hizmeti'nin nasıl dağıtılanabildiğini öğrenmek için [Azure Dosya Eşitleme'yi nasıl dağıtabilirsiniz.](storage-sync-files-deployment-guide.md) 
- Eşitleme grubunun dağıtıldığından emin olun. [Eşitleme grubu oluşturmayı](storage-sync-files-deployment-guide.md#create-a sync-group-and-a-cloud-endpoint)öğrenin.
- Sunucunun Internet'e bağlı olduğundan ve Azure'a erişilebildiğinden emin olun. Sunucu ve hizmetimiz arasındaki tüm iletişim için port 443'u kullanıyoruz.

## <a name="add-a-server-endpoint"></a>Sunucu uç noktası ekleme
Sunucu bitiş noktası eklemek için istediğiniz eşitleme grubuna gidin ve "Sunucu bitiş noktası ekle"yi seçin.

![Eşitleme grubu bölmesine yeni bir sunucu uç noktası ekleme](media/storage-sync-files-server-endpoint/add-server-endpoint-1.png)

Sunucu bitiş noktası **ekle**altında aşağıdaki bilgiler gereklidir:

- **Kayıtlı sunucu**: Sunucu bitiş noktasını oluşturmak için sunucu nun veya kümenin adı.
- **Yol**: Windows Server'da eşitleme grubunun bir parçası olarak eşitlenecek yol.
- **Bulut Katmanlama**: Bulut katmanlamayı etkinleştirmek veya devre dışı etmek için bir anahtar. Etkinleştirildiğinde, bulut katmanlama dosyaları Azure dosya paylaşımlarınıza *katmanlar.* Bu, sunucunuzdaki alan verimliliğini yönetmenize yardımcı olmak için şirket içi dosya paylaşımlarını veri kümesinin tam bir kopyası yerine önbelleğe dönüştürür.
- **Volume Free Space**: sunucu bitiş noktasının bulunduğu birimde rezerve etmek için boş alan miktarıdır. Örneğin, birim boş alanı tek bir sunucu bitiş noktası olan bir birimde %50'ye ayarlanırsa, veri miktarının kabaca yarısı Azure Dosyalarına katmanlanır. Bulut katmanlamanın etkin olup olmadığına bakılmaksızın, Azure dosya paylaşımınızda her zaman eşitleme grubundaki verilerin tam bir kopyası vardır.

Sunucu bitiş noktasını eklemek için **Oluştur'u** seçin. Eşitleme grubunun ad alanındaki dosyalar artık eşit tutulur. 

## <a name="remove-a-server-endpoint"></a>Sunucu bitiş noktasını kaldırma
Belirli bir sunucu bitiş noktası için Azure Dosya Eşitlemeyi'ni kullanmayı bırakmak istiyorsanız, sunucu bitiş noktasını kaldırabilirsiniz. 

> [!Warning]  
> Bir Microsoft mühendisitarafından açıkça talimat verilmedikçe sunucu bitiş noktasını kaldırıp yeniden oluşturarak eşitleme, bulut katmanlama veya Azure Dosya Eşitlemesi'nin başka bir yönüyle ilgili sorunları gidermeye çalışmayın. Sunucu bitiş noktasının kaldırılması yıkıcı bir işlemdir ve sunucu bitiş noktası içindeki katmanlı dosyalar, sunucu bitiş noktası yeniden oluşturulduktan sonra Azure dosya paylaşımındaki konumlarına "yeniden bağlanmaz", bu da eşitleme hatalarına neden olur. Ayrıca, sunucu bitiş noktası ad alanı dışında bulunan katmanlı dosyalar kalıcı olarak kaybolabilir. Bulut katmanlama hiç etkinleştirilemese bile, katmanlı dosyalar sunucu bitiş noktanızda bulunabilir.

Sunucu bitiş noktasını kaldırmadan önce tüm katmanlı dosyaların geri çağrıldığından emin olmak için, sunucu bitiş noktasında bulut katmanlamayı devre dışı bırakın ve sunucu bitiş noktası ad alanınızdaki tüm katmanlı dosyaları geri çağırmak için aşağıdaki PowerShell cmdlet'i çalıştırın:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <path-to-to-your-server-endpoint> -Order CloudTieringPolicy
```
Belirtme, `-Order CloudTieringPolicy` önce en son değiştirilen dosyaları geri çağıracaktır.
Göz önünde bulundurulması gereken diğer isteğe bağlı ancak yararlı parametreler şunlardır:
* `-ThreadCount`paralel olarak kaç dosyanın geri çağrılabileceğini belirler.
* `-PerFileRetryCount`şu anda engellenen bir dosyanın ne sıklıkta geri çağırma girişiminde bulunulacağını belirler.
* `-PerFileRetryDelaySeconds`denemelerini geri çağırmak için yeniden deneme arasındaki saniye cinsinden süreyi belirler ve her zaman önceki parametre ile birlikte kullanılmalıdır.

> [!Note]  
> Sunucuyu barındıran yerel birim, katmanlı tüm verileri geri çağırmak için `Invoke-StorageSyncFileRecall` yeterli boş alana sahip değilse, cmdlet başarısız olur.  

Sunucu bitiş noktasını kaldırmak için:

1. Sunucunuzun kayıtlı olduğu Depolama Eşitleme Hizmeti'ne gidin.
2. İstediğiniz eşitleme grubuna gidin.
3. Depolama Eşitleme Hizmeti'ndeki eşitleme grubunda istediğiniz sunucu bitiş noktasını kaldırın. Bu, eşitleme grubu bölmesinde ilgili sunucu bitiş noktasını sağ tıklayarak gerçekleştirilebilir.

    ![Eşitleme grubundan sunucu bitiş noktasını kaldırma](media/storage-sync-files-server-endpoint/remove-server-endpoint-1.png)

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Dosya Eşitlemi ile bir sunucuya kaydol/kayıt dışı](storage-sync-files-server-registration.md)
- [Azure Dosya Eşitleme dağıtımı planlama](storage-sync-files-planning.md)
- [Azure Dosya Eşitleme’yi izleme](storage-sync-files-monitoring.md)
