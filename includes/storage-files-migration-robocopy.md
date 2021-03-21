---
title: Bir klasör yapısını Azure Dosya Eşitleme topolojisine eşleme
description: Azure Dosya Eşitleme ile kullanmak için mevcut bir dosya ve klasör yapısını Azure dosya paylaşımlarına eşleyin. Geçiş belgeleri arasında paylaşılan ortak bir metin bloğu.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: cde85e245c8cc6ae8c55b24270f125bacc111737
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102547568"
---
```console
Robocopy /MT:16 /NP /NFL /NDL /B /MIR /IT /COPY:DATSO /COPY:DATSO /DCOPY:DAT /UNILOG:<FilePathAndName> <SourcePath> <Dest.Path> 
```

| Anahtar              | Anlamı |
|---------------------|---------|
| /MT                 | RoboCopy 'nin çoklu iş parçacıklı çalıştırmasına izin verir. Varsayılan 8 ' dir ve maksimum değer 128 ' dir. Bu değeri, çekirdek başına işlemci çekirdek sayısı ve iş parçacığı sayısı ile eşleştirin. Bir üretim sunucusunun sahip olabileceği diğer görevler için çekirdekler ayrılmalıdır ' i göz önünde bulundurun. |
| /NP                 | Her dosya ve klasör için kopyanın ilerlemesi gösterilmez. İlerlemeyi görüntülemek, kopyalama performansını önemli ölçüde düşürür. |
| /NFL                | Dosya adlarının günlüğe kaydedilmeyeceğini belirtir. Kopyalama performansını geliştirir. |
| /NDL                | Dizin adlarının günlüğe kaydedilmeyeceğini belirtir. Kopyalama performansını geliştirir. |
| /B                  | RoboCopy, bir yedekleme uygulamasının kullanacağı modda çalıştırır. RoboCopy 'nin geçerli kullanıcının izinleri olmayan dosyaları taşımasına izin verir. |
| /MıR                | *Kaynak yansıtma hedefi* , Robocopy 'nin yalnızca kaynak ve hedef arasında değişimleri kopyalamasını sağlar. Boş alt dizinler kopyalanacak. Hedefte değiştirilen veya mevcut olmayan öğeler (dosyalar veya klasörler) kopyalanacak. Hedefte bulunan ancak kaynakta yer almayan öğeler hedeften temizlenir (silinir). Bu anahtarı kullanırken, kaynak ve hedef klasör yapısını tam olarak eşleştirmek zorunludur. "Eşleşme", doğru kaynak ve klasör düzeyinden hedefteki eşleşen klasör düzeyine kopyaladığınız anlamına gelir. Yalnızca "yakala" kopyalama işlemi başarılı olabilir. Kaynak ve hedef `/MIR` uyuştudığında, kullanımı büyük ölçekli silme ve yeniden kopyalamaların oluşmasına neden olur. |
| /IT                 | Belirli yansıtma senaryolarında uygunluk korunmasını sağlar. </br>*Örnek* : iki Robocopy arasında çalışan bir dosya ÇALıŞıYORSA bir ACL değişikliği ve bir öznitelik güncelleştirmesi: Örneğin, *gizli* olarak işaretlenir. /IT olmadan, ACL değişikliği RoboCopy tarafından kaçırılarak hedef konuma aktarılmaz. |
|Kopya`[copyflags]`  | Dosya kopyasının doğruluğu (belirtilmezse varsayılan `/COPY:DAT` ), Copy Flags: `D` = Data, `A` = Attributes, `T` = damgas, `S` = Security = NTFS ACL 'leri, `O` = Owner Information, `U` = bir<u>u</u>dime bilgisi. Denetim bilgileri bir Azure dosya paylaşımında depolanamaz. |
| /DCOPY:`[copyflags]`| Dizinlerin kopyası için uygunluk (belirtilmezse varsayılan `/DCOPY:DA` ), Copy Flags: `D` = Data, `A` = öznitelikler, `T` = zaman damgaları. |
| /UNILOG:<file name> | GÜNLÜK dosyasının durumunu UNICODE olarak çıkış (varolan günlüğün üzerine yaz). |
| /LFSD               | **yalnızca katmanlı depolamaya sahip hedefler için** </br>/LFSE isteklerini kullanarak ' düşük boş alan modunda ' bir işlem yapın. Bu anahtar, yalnızca katmanlı depolamaya sahip olan hedefler için yararlıdır ve bu, RoboCopy tamamlanmadan önce yerel kapasitenin tükenmesine sahip olabilir. Bu anahtar, bir Azure Dosya Eşitleme bulut katmanlama etkinleştirilmiş hedefle kullanılmak üzere özel olarak eklenmiştir. Azure Dosya Eşitleme bağımsız olarak kullanılabilir. Bu modda, RoboCopy bir dosya kopyası hedef birimin boş alanının ' Floor ' değerinin altına geçmesine neden olduğunda duraklatılır. Bu değer `/LFSM:n` , bayrak biçimi ile belirtilebilir. Parametre `n` taban 2: `nKB` ,, veya olarak belirtilir `nMB` `nGB` . `/LFSM`Açık taban değeri olmadan belirtilmişse, taban, hedef birimin boyutunun yüzde 10 ' una ayarlanır. Düşük boş alan modu/MT,/EFSRAW,/B ve/ZBILE uyumsuzdur. |
| /Z                  | **dikkatli kullanım** </br>Dosyaları yeniden başlatma modunda kopyalar, kullanımı yalnızca kararsız bir ağ ortamında önerilir. Bu seçenek, ek günlük kaydı nedeniyle kopyalama performansını önemli ölçüde azaltır. |
| /ZB                 | **dikkatli kullanım** </br>Yeniden başlatma modunu kullanır. Erişim reddedilirse, bu seçenek yedekleme modunu kullanır. Bu seçenek, denetim noktası gösterme nedeniyle kopyalama performansını önemli ölçüde azaltır. |
   