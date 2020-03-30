---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: rogarana
ms.openlocfilehash: 55456a6be938411d3c08a0eaa8fdbfb0844e7129
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "72391819"
---
Azure portalı veya SMB kullanılarak Azure dosya paylaşımında yapılan değişiklikler hemen algılanmaz ve sunucu bitiş noktasındaki değişiklikler gibi çoğaltılmaz. Azure Dosyaları'nda henüz değişiklik bildirimleri veya günlük ler bulunmadığından, dosyalar değiştirildiğinde otomatik olarak eşitleme oturumu başlatmanın bir yolu yoktur. Windows Server'da Azure Dosya Eşitlemi, dosyalar değiştiğinde otomatik olarak eşitleme oturumu başlatmak için [Windows USN günlükasyonunu](https://msdn.microsoft.com/library/windows/desktop/aa363798.aspx) kullanır.

Azure dosya paylaşımındaki değişiklikleri algılamak için Azure Dosya Eşitleme'nin *değişiklik algılama işi*olarak adlandırılan zamanlanmış bir işi vardır. Değişiklik algılama işi, dosya paylaşımındaki her dosyayı oyalar ve sonra dosyayı bu dosyanın eşitleme sürümüyle karşılaştırır. Değişiklik algılama işi dosyaların değiştiğini belirlediğinde, Azure Dosya Eşitleme bir eşitleme oturumu başlatır. Değişiklik algılama işi her 24 saatte bir başlatılır. Değişiklik algılama işi Azure dosya paylaşımındaki her dosyayı sayarak çalıştığından, değişiklik algılaması daha küçük ad alanlarından daha uzun sürer. Büyük ad alanları için, hangi dosyaların değiştiğini belirlemek her 24 saatte bir birden fazla sürebilir.

Azure dosya paylaşımında değiştirilen dosyaları hemen eşitlemek için **Invoke-AzStorageSyncChangeDetection** PowerShell cmdlet, Azure dosya paylaşımındaki değişikliklerin algılanmasını el ile başlatmak için kullanılabilir. Bu cmdlet, azure dosya paylaşımında bir tür otomatik işlem yapıldığı veya değişikliklerin bir yönetici tarafından yapıldığı senaryolar için tasarlanmıştır (örneğin, dosya ve dizinleri paylaşıma taşımak gibi). Son kullanıcı değişiklikleri için öneri, Azure Dosya Eşitleme aracısını bir IaaS VM'ye yüklemek ve son kullanıcıların Dosya paylaşımına IaaS VM üzerinden erişmelerini sağlamaktır. Bu şekilde tüm değişiklikler Invoke-AzStorageSyncChangeDetection cmdlet kullanmaya gerek kalmadan diğer aracılar ile hızlı bir şekilde eşitlenecektir. Daha fazla bilgi için [Invoke-AzStorageSyncChangeDetection](https://docs.microsoft.com/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection) belgelerine bakın.

>[!NOTE]
>REST kullanarak Azure dosya paylaşımında yapılan değişiklikler, SMB'nin son değiştirilen saatini güncelleştirmez ve eşitleme yle yapılan bir değişiklik olarak görülmez.

Windows Server'daki birimler için USN'ye benzer bir Azure dosyası paylaşımı için değişiklik algılama eklemeyi araştırıyoruz. [Azure Files UserVoice'ta](https://feedback.azure.com/forums/217298-storage/category/180670-files)oy vererek bu özelliği gelecekteki geliştirme için önceliklendirmemize yardımcı olun.
