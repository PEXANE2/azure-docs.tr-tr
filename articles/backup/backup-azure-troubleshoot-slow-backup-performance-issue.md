---
title: Dosya ve klasörlerin yavaş yedeklenmesinde sorun giderme
description: Azure Backup performans sorunlarının nedenini tanılamanıza yardımcı olması için sorun giderme kılavuzu sağlar
ms.reviewer: saurse
ms.topic: troubleshooting
ms.date: 07/05/2019
ms.openlocfilehash: d65eca685748499f52a990c7ac588bf44cbbba31
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85194034"
---
# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Azure Backup’ta dosya ve klasörlerin yavaş yedekleme sorunlarını giderme

Bu makalede, Azure Backup kullanırken dosyalar ve klasörler için yavaş yedekleme performansının Nedenini tanılamanıza yardımcı olacak sorun giderme kılavuzu sağlanmıştır. Dosyaları yedeklemek için Azure Backup Aracısı 'nı kullandığınızda, yedekleme işlemi beklenenden uzun sürebilir. Bu gecikmeye aşağıdakilerden biri veya birkaçı neden olmuş olabilir:

* [Yedeklenmekte olan bilgisayarda performans sorunları var.](#cause1)
* [Başka bir işlem veya virüsten koruma yazılımı Azure Backup işlemini engelliyor.](#cause2)
* [Yedekleme aracısı bir Azure sanal makinesinde (VM) çalışıyor.](#cause3)  
* [Çok sayıda (milyonlarca) dosyayı yedekliyorsunuz.](#cause4)

Sorun giderme sorunlarını gidermeye başlamadan önce [en son Azure Backup aracısını](https://aka.ms/azurebackup_agent)indirip yüklemenizi öneririz. Çeşitli sorunları gidermek, özellikler eklemek ve performansı artırmak için yedekleme aracısında sık sık güncelleştirmeler yaptık.

Ayrıca, yaygın yapılandırma sorunlarından biriyle karşılaşmadığınızdan emin olmak için [Azure Backup hizmeti hakkında SSS](backup-azure-backup-faq.md) 'yi incelemenizi öneririz.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause-backup-job-running-in-unoptimized-mode"></a>Neden: yedekleme işi iyileştirilmemiş modda çalışıyor

* MARS Aracısı, tüm birimi tarayarak dizin veya dosyalardaki değişiklikleri denetleyerek, USN (güncelleştirme sıra numarası) değişiklik günlüğü veya **en iyi duruma getirilmiş modu** kullanarak yedekleme işini **iyileştirilmiş modda** çalıştırabilir.
* En iyi duruma getirilmiş mod, aracının birim üzerindeki her dosyayı tarayabilmesi ve değiştirilen dosyaları belirlemede meta verilere göre karşılaştırılabilmesi için yavaş bir moddur.
* Bunu doğrulamak için, MARS Aracısı konsolundan **Iş ayrıntıları** ' nı açın ve **verilerin aktarımını (iyileştirilmez, daha fazla zaman alabilir)** aşağıda gösterildiği gibi denetleyin:

    ![İyileştirilmemiş modda çalışıyor](./media/backup-azure-troubleshoot-slow-backup-performance-issue/unoptimized-mode.png)

* Aşağıdaki koşullar yedekleme işinin iyileştirilmemiş modda çalışmasına neden olabilir:
  * İlk yedekleme (Ilk çoğaltma olarak da bilinir), her zaman iyileştirilmemiş modda çalıştırılır
  * Önceki yedekleme işi başarısız olursa, sonraki zamanlanmış yedekleme işi en iyileştirilmemiş olarak çalışacaktır.

<a id="cause1"></a>

## <a name="cause-performance-bottlenecks-on-the-computer"></a>Neden: bilgisayardaki performans sorunları

Yedeklenen bilgisayardaki performans sorunları gecikmelere neden olabilir. Örneğin, bilgisayarın disk okuma veya diske yazma veya ağ üzerinden veri göndermek için kullanılabilir bant genişliği gibi performans sorunlarına neden olabilir.

Windows, bu performans sorunlarını algılamak için [Performans İzleyicisi](https://techcommunity.microsoft.com/t5/ask-the-performance-team/windows-performance-monitor-overview/ba-p/375481) (PerfMon) adlı yerleşik bir araç sağlar.

En iyi yedeklemeler için performans sorunlarını tanılamaya yardımcı olabilecek bazı performans sayaçları ve aralıkları aşağıda verilmiştir.

| Sayaç | Durum |
| --- | --- |
| Mantıksal disk (fiziksel disk)--% boşta |* %100 boşta = %50 boşta = sağlıklı</br>* %49 boşta = %20 boşta = uyarı ya da Izleyici</br>* %19 boşta %0 boşta = kritik veya spec dışı |
| Mantıksal disk (fiziksel disk)--% ort. Disk sn okuma veya yazma |* 0,001 MS ile 0,015 MS = sağlıklı</br>* 0,015 MS ile 0,025 MS = uyarı veya Izleyici</br>* 0,026 MS veya daha uzun = kritik veya spec dışı |
| Mantıksal disk (fiziksel disk)--geçerli disk sırası uzunluğu (tüm örnekler için) |6 dakikadan uzun süre 80 istek |
| Bellek--Sayfalamayan baytlar |* Havuzun %60 ' inden azı tüketildi = sağlıklı<br>* %61, kullanılan havuzun %80 ' i = uyarı veya Izleyici</br>* %80 taneden fazla% havuz kullanıldı = kritik veya spec dışı |
| Bellek--havuzda disk belleğine alınan baytlar |* Havuzun %60 ' inden azı tüketildi = sağlıklı</br>* %61, kullanılan havuzun %80 ' i = uyarı veya Izleyici</br>* %80 taneden fazla% havuz kullanıldı = kritik veya spec dışı |
| Bellek--kullanılabilir megabayt |* %50 boş bellek kullanılabilir veya daha fazla = sağlıklı</br>* boş belleğin %25 ' i kullanılabilir = Izleyici</br>* boş belleğin %10 ' ü kullanılabilir = uyarı</br>* 100 MB 'tan az veya kullanılabilir boş belleğin %5 ' inden az olması = kritik veya spec dışı |
| İşlemci-- \% Işlemci zamanı (tüm örnekler) |* %60 daha az tüketilen = sağlıklı</br>* %61-90% tüketilen = Izleyici veya uyarı</br>* %91-100% tüketilen = kritik |

> [!NOTE]
> Altyapının külün olduğunu belirlerseniz, daha iyi performans için diskleri düzenli olarak birleştirmeniz önerilir.
>
>

<a id="cause2"></a>

## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Neden: başka bir işlem veya virüsten koruma yazılımı Azure Backup engelliyor

Windows sistemindeki diğer işlemlerin Azure Backup Aracısı işleminin performansını olumsuz yönde etkilediği birkaç örnek gördük. Örneğin, verileri yedeklemek için hem Azure Backup Aracısı hem de başka bir program kullanıyorsanız veya virüsten koruma yazılımı çalışıyorsa ve yedeklenmek üzere dosyalar üzerinde bir kilit varsa, dosyalardaki birden fazla kilit çekişmeye neden olabilir. Bu durumda, yedekleme başarısız olabilir veya iş beklenenden uzun sürebilir.

Bu senaryodaki en iyi öneri, Azure Backup Aracısı için yedekleme saatinin değişmediğini görmek üzere diğer Yedekleme programını kapatmmsudur. Genellikle, birden çok yedekleme işinin aynı anda çalışmadığı ve birbirini etkilemelerini engellemek için yeterli olduğundan emin olun.

Virüsten koruma programları için aşağıdaki dosyaları ve konumları dışlanmasını öneririz:

* C:\Program Files\Microsoft Azure Kurtarma Hizmetleri işlem olarak Agent\bin\cbengine.exe
* C:\Program Files\Microsoft Azure kurtarma hizmetleri Aracısı \ klasörler
* Karalama konumu (Standart konumu kullanmıyorsanız)

<a id="cause3"></a>

## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Neden: bir Azure sanal makinesinde çalışan yedekleme Aracısı

Yedekleme aracısını bir VM üzerinde çalıştırıyorsanız, performans fiziksel bir makinede çalıştırılırken daha yavaş olur. Bu, ıOPS sınırlamaları nedeniyle beklenmektedir.  Ancak, Azure Premium depolamaya yedeklenmekte olan veri sürücülerine geçerek performansı iyileştirebilirsiniz. Bu sorunu düzeltmek için çalışıyoruz ve düzeltme gelecekteki bir sürümde kullanıma sunulacaktır.

<a id="cause4"></a>

## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Neden: çok sayıda (milyonlarca) dosyayı yedekleme

Büyük hacimli verilerin taşınması, daha küçük bir veri hacmi taşımadan daha uzun sürer. Bazı durumlarda, yedekleme zamanı yalnızca verilerin boyutuyla ve ayrıca dosya veya klasör sayısıyla ilgilidir. Bu özellikle, milyonlarca küçük dosya (birkaç kilobayt kadar bayt) yedeklendiğinde geçerlidir.

Bu davranış, verileri yedeklemekte ve Azure 'a taşırken, Azure 'un aynı anda dosyalarınızı kataloglarken meydana gelir. Nadir bazı senaryolarda, Katalog işlemi beklenenden uzun sürebilir.

Aşağıdaki göstergeler, bir sonraki adımlarda performans sorunlarını ve buna uygun işi anlamanıza yardımcı olabilir:

* **Kullanıcı arabirimi, veri aktarımının ilerlemesini gösteriyor**. Veriler hala aktarılmakta. Ağ bant genişliği veya veri boyutu gecikmelere neden olabilir.
* **UI veri aktarımı için ilerlemeyi gösterilmiyor**. C:\Program Files\Microsoft Azure Recovery Services \Temp dizininde bulunan günlükleri açın ve sonra günlüklerde FileProvider:: EndData girdisini kontrol edin. Bu giriş, veri aktarımının bittiğini ve Katalog işleminin gerçekleştiğini belirtir. Yedekleme işlerini iptal etmeyin. Bunun yerine, Katalog işleminin bitmesini biraz daha uzun süre bekleyin. Sorun devam ederse [Azure desteği](https://portal.azure.com/#create/Microsoft.Support)'ne başvurun.

Büyük diskleri yedeklemeye çalışıyorsanız, ilk yedekleme için [Azure Data Box](https://docs.microsoft.com/azure/backup/offline-backup-azure-data-box) kullanmanız önerilir (ilk çoğaltma).  Data Box kullanamıyoruz, ağ üzerinden uzun veri aktarımları sırasında ortamınızda gerçekleşen tüm geçici ağ sorunları yedekleme hatalarıyla sonuçlanır.  Bu hatalara karşı koruma sağlamak için, ilk yedeklemelerinize birkaç klasör ekleyebilirsiniz ve tüm klasörler başarıyla Azure 'a yedeklenene kadar daha fazla klasör eklemeye devam edebilirsiniz.  Sonraki artımlı yedeklemeler nispeten daha hızlı olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

* [Dosya ve klasörleri yedekleme hakkında sık sorulan sorular](backup-azure-file-folder-backup-faq.md)
