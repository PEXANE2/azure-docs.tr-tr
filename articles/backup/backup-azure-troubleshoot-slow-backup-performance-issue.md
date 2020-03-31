---
title: Dosya ve klasörlerin yavaş yedekleme sorunu
description: Azure Yedekleme performans sorunlarının nedenini tanılamanıza yardımcı olacak sorun giderme kılavuzu sağlar
ms.reviewer: saurse
ms.topic: troubleshooting
ms.date: 07/05/2019
ms.openlocfilehash: 6c650ee735ffcdd50f4361a867fa592f4965ab68
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79408701"
---
# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Azure Backup’ta dosya ve klasörlerin yavaş yedekleme sorunlarını giderme

Bu makalede, Azure Yedekleme kullanırken dosya ve klasörler için yavaş yedekleme performansı nın nedenini tanılamanıza yardımcı olacak sorun giderme kılavuzu bulunur. Dosyaları yedeklemek için Azure Yedekleme aracısını kullandığınızda, yedekleme işlemi beklenenden uzun sürebilir. Bu gecikme, aşağıdakilerden biri veya birkaçı tarafından neden olabilir:

* [Bilgisayarda yedeklenen performans darboğazları var.](#cause1)
* [Başka bir işlem veya virüsten koruma yazılımı Azure Yedekleme işlemini engelliyor.](#cause2)
* [Yedekleme aracısı bir Azure sanal makinesinde (VM) çalışıyor.](#cause3)  
* [Çok sayıda (milyonlarca) dosyayı yedekliyoruz.](#cause4)

Sorun giderme sorunlarını başlatmadan önce, en [son Azure Yedekleme aracısını](https://aka.ms/azurebackup_agent)indirmenizi ve yüklemenizi öneririz. Çeşitli sorunları gidermek, özellikler eklemek ve performansı artırmak için Yedekleme aracısına sık sık güncellemeler yaparız.

Ayrıca, ortak yapılandırma sorunlarından herhangi birini yaşamadığınızdan emin olmak için [Azure Yedekleme hizmeti SSS'sini](backup-azure-backup-faq.md) incelemenizi öneririz.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause-backup-job-running-in-unoptimized-mode"></a>Neden: En iyi duruma getirilmemiş modda çalışan yedekleme işi

* MARS aracısı, tüm birimi tarayarak dizin veya dosya değişiklikleri olup yok olup yok edilerek USN (güncelleştirme sıra numarası) değişiklik günlüğü veya **en iyi duruma getirilmemiş modu** kullanarak yedekleme işini en iyi duruma **getirilmiş modda** çalıştırabilir.
* Aracının birimdeki her dosyayı tarayıp değiştirilen dosyaları belirlemek için meta verilerle karşılaştırması olduğundan, en iyi duruma getirilmemiş mod yavaştır.
* Bunu doğrulamak için, MARS aracısı konsolundan **İş Ayrıntıları'nı** açın ve aşağıda gösterildiği gibi **veri aktarımı (en iyi duruma getirilmemiş, daha fazla zaman alabilir)** olup olmadığını görmek için durumu kontrol edin:

    ![En iyi duruma getirilmemiş modda çalıştırma](./media/backup-azure-troubleshoot-slow-backup-performance-issue/unoptimized-mode.png)

* Aşağıdaki koşullar yedekleme işinin en iyi duruma getirilmemiş modda çalışmasına neden olabilir:
  * İlk yedekleme (İlk Çoğaltma olarak da bilinir) her zaman en iyi duruma getirilmemiş modda çalışır
  * Önceki yedekleme işi başarısız olursa, bir sonraki zamanlanmış yedekleme işi en iyi duruma getirilmemiş olarak çalışır.

<a id="cause1"></a>

## <a name="cause-performance-bottlenecks-on-the-computer"></a>Neden: Bilgisayardaki performans darboğazları

Yedeklenen bilgisayardaki darboğazlar gecikmelere neden olabilir. Örneğin, bilgisayarın diske okuma veya yazma yeteneği veya ağ üzerinden veri göndermek için kullanılabilir bant genişliği, darboğazlara neden olabilir.

Windows, bu darboğazları algılamak için [Performans İzleyicisi](https://techcommunity.microsoft.com/t5/ask-the-performance-team/windows-performance-monitor-overview/ba-p/375481) (Perfmon) adında yerleşik bir araç sağlar.

Aşağıda, en iyi yedeklemeler için darboğazları tanılamada yardımcı olabilecek bazı performans sayaçları ve aralıkları ve dahilleri verebilirsiniz.

| Sayaç | Durum |
| --- | --- |
| Mantıksal Disk(Fiziksel Disk)--%boşta |* % 100 boşta % 50 boşta = Sağlıklı</br>* %49 boşta % 20 boşta = Uyarı veya Monitör</br>* %19 boşta % 0 boşta = Kritik veya Spec dışında |
| Mantıksal Disk(Fiziksel Disk)--%Avg. Disk Sn Oku veya Yaz |* 0.001 ms ile 0,015 ms = Sağlıklı</br>* 0,015 ms ile 0,025 ms = Uyarı veya Monitör</br>* 0.026 ms veya daha uzun = Kritik veya Spec Dışında |
| Mantıksal Disk(Fiziksel Disk)--Geçerli Disk Sıra Uzunluğu (tüm örnekler için) |6 dakikadan fazla 80 istek |
| Bellek-Havuz Non Paged Bayt |* Tüketilen havuzun %60'ından azı = Sağlıklı<br>* Tüketilen havuzun %61 ila %80'i = Uyarı veya Monitör</br>* Tüketilen % 80'den büyük havuz = Kritik veya Spec Dışında |
| Bellek--Havuz Sayfalı Baytlar |* Tüketilen havuzun %60'ından azı = Sağlıklı</br>* Tüketilen havuzun %61 ila %80'i = Uyarı veya Monitör</br>* Tüketilen % 80'den büyük havuz = Kritik veya Spec Dışında |
| Bellek-- Kullanılabilir Megabaytlar |* 50% ücretsiz bellek mevcut veya daha fazla = Sağlıklı</br>* Boş belleğin %25'i kullanılabilir = Monitör</br>* Ücretsiz bellek mevcut % 10 = Uyarı</br>* 100 MB'dan az veya boş belleğin %5'i kullanılabilir = Kritik veya Spec dışında |
| İşlemci--\%İşlemci Zamanı (tüm örnekler) |* Az% 60 tüketilen = Sağlıklı</br>* 61% ila% 90 tüketilen = Monitör veya Dikkat</br>* 91% için% 100 tüketilen = Kritik |

> [!NOTE]
> Altyapının suçlu olduğunu belirlerseniz, daha iyi performans için diskleri düzenli olarak parçalandırmanızı öneririz.
>
>

<a id="cause2"></a>

## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Neden: Azure Yedekleme'yi destekleyen başka bir işlem veya virüsten koruma yazılımı

Windows sistemindeki diğer işlemlerin Azure Yedekleme aracısı işleminin performansını olumsuz etkilediği birkaç örnek gördük. Örneğin, verileri yedeklemek için hem Azure Yedekleme aracısını hem de başka bir programı kullanıyorsanız veya virüsten koruma yazılımı çalışıyorsa ve yedeklenecek dosyalarda kilit varsa, dosyalardaki birden çok kilit çekişmeye neden olabilir. Bu durumda, yedekleme başarısız olabilir veya iş beklenenden daha uzun sürebilir.

Bu senaryodaki en iyi öneri, Azure Yedekleme aracısının yedekleme zamanının değişip değişmediğini görmek için diğer yedekleme programını kapatmaktır. Genellikle, birden çok yedekleme işinin aynı anda çalışmadığından emin olmak, birbirlerini etkilemelerini önlemek için yeterlidir.

Virüsten koruma programları için aşağıdaki dosyaları ve konumları hariç tutmanızı öneririz:

* C:\Program Files\Microsoft Azure Kurtarma Hizmetleri Aracısı\bin\cbengine.exe bir işlem olarak
* C:\Program Files\Microsoft Azure Kurtarma Hizmetleri Aracısı\ klasörleri
* Scratch konumu (standart konumu kullanmıyorsanız)

<a id="cause3"></a>

## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Neden: Azure sanal makinesinde çalışan yedekleme aracısı

Yedek aracıyı bir VM'de çalıştırıyorsanız, performans fiziksel bir makinede çalıştırdığınızdan daha yavaş olacaktır. Bu IOPS sınırlamaları nedeniyle bekleniyor.  Ancak, Azure Premium Depolama'ya yedeklenen veri sürücülerini değiştirerek performansı en iyi duruma getirebilirsiniz. Bu sorunu gidermek için çalışıyoruz ve düzeltme gelecekteki bir sürümde kullanılabilir olacak.

<a id="cause4"></a>

## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Neden: Çok sayıda (milyonlarca) dosyayı yedekleme

Büyük miktarda veriyi taşımak, daha küçük bir veri hacmini taşımaktan daha uzun sürer. Bazı durumlarda, yedekleme süresi yalnızca verilerin boyutuyla değil, aynı zamanda dosya veya klasör sayısıyla da ilişkilidir. Bu özellikle küçük milyonlarca dosya (birkaç kilobayt için birkaç bayt) yedekleniyor zaman doğrudur.

Bu davranış, siz verileri yedekleyip Azure'a taşımakla birlikte Azure'un aynı anda dosyalarınızı katalogladığı için oluşur. Bazı nadir senaryolarda, katalog işlemi beklenenden daha uzun sürebilir.

Aşağıdaki göstergeler darboğaz anlamanıza yardımcı olabilir ve buna göre sonraki adımlar üzerinde çalışmak:

* **UI veri aktarımı için ilerleme gösteriyor.** Veriler hala aktarılıyor. Ağ bant genişliği veya veri boyutu gecikmelere neden olabilir.
* **UI veri aktarımı için ilerleme göstermiyor.** C:\Program Files\Microsoft Azure Recovery Services Agent\Temp adresinde bulunan günlükleri açın ve ardından FileProvider::EndData girişinde günlükleri olup yok edin. Bu giriş, veri aktarımını tamamladığını ve katalog işleminin gerçekleştiğini belirtir. Yedekleme işlerini iptal etmeyin. Bunun yerine, katalog işleminin tamamlanması için biraz daha bekleyin. Sorun devam ederse, [Azure desteğine](https://portal.azure.com/#create/Microsoft.Support)başvurun.

## <a name="next-steps"></a>Sonraki adımlar

* [Dosya ve klasörleri yedekleme hakkında sık sorulan sorular](backup-azure-file-folder-backup-faq.md)