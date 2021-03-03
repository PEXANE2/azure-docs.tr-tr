---
title: Azure NetApp Files NFSv 4.1 birimlerinde Kerberos 'un performans etkisi | Microsoft Docs
description: Kullanılabilir güvenlik seçeneklerini, sınanan performans vektörlerini ve Azure NetApp Files NFSv 4.1 birimlerinde Kerberos 'un beklenen performans etkisini açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/18/2021
ms.author: b-juche
ms.openlocfilehash: e9054f11c8f55e9fe00266840a9f6e257f14e659
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101747213"
---
# <a name="performance-impact-of-kerberos-on-azure-netapp-files-nfsv41-volumes"></a>Azure NetApp Files NFSv 4.1 birimlerinde Kerberos 'un performans etkisi

Azure NetApp Files, Kerberos modlarında (krb5, krb5i ve krb5p), AES-256 şifrelemesi ile [NFS istemci şifrelemesini](configure-kerberos-encryption.md) destekler. Bu makalede, NFSv 4.1 birimlerinde Kerberos 'un performans etkisi açıklanmaktadır. 

## <a name="available-security-options"></a>Kullanılabilir güvenlik seçenekleri 

Şu anda NFSv 4.1 birimleri için kullanılabilir olan güvenlik seçenekleri şunlardır: 

* **sec = sys** , NFS işlemlerinin kimliğini doğrulamak için AUTH_SYS kullanarak yerel UNIX UID 'Leri ve GID 'leri kullanır.
* **sec = krb5** , kullanıcıların kimliğini doğrulamak IÇIN yerel UNIX uid 'Leri ve GID 'Ler yerine Kerberos V5 kullanır.
* **sec = krb5i** , Kullanıcı kimlik doğrulaması Için Kerberos V5 kullanır ve verilerin değiştirilmesini engellemek için güvenli sağlama TOPLAMı kullanarak NFS işlemlerinin bütünlük denetimini gerçekleştirir.
* **sec = krb5p** Kullanıcı kimlik doğrulaması ve bütünlük denetimi Için Kerberos V5 kullanır. Trafik algılaması 'nı engellemek için NFS trafiğini şifreler. Bu seçenek en güvenli ayardır, ancak aynı zamanda en fazla performans yükünden de oluşur.

## <a name="performance-vectors-tested"></a>Performans vektörlerini test edildi

Bu bölümde çeşitli seçeneklerin tek istemci tarafı performans etkisi açıklanmaktadır `sec=*` .

* Performans etkisi iki düzeyde test edildi: düşük eşzamanlılık (düşük yük) ve yüksek eşzamanlılık (g/ç ve aktarım hızı üst sınırı).  
* Üç tür iş yükü test edilmiştir:  
    * Küçük işlem rastgele okuma/yazma (FIO kullanarak)
    * Büyük işlem sıralı okuma/yazma (FIO kullanarak)
    * Git gibi uygulamalar tarafından oluşturulan meta veriler ağır iş yükü

## <a name="expected-performance-impact"></a>Beklenen performans etkisi 

İki odak alanı vardır: açık yük ve üst sınır. Aşağıdaki listede güvenlik ayarı ve senaryo tarafından senaryoya göre performans etkisi güvenlik ayarı açıklanır. Tüm karşılaştırmalar güvenlik parametresine göre yapılır `sec=sys` . Test, tek bir istemci kullanılarak tek bir birimde gerçekleştirildi. 

Krb5 'in performans etkisi:

* Düşük eşzamanlılık (r/w):
    * Sıralı gecikme artan 0,3 MS.
    * Rastgele g/ç gecikme süresi arttı 0,2 ms.
    * Meta veri g/ç gecikme süresi 0,2 ms.
* Yüksek eşzamanlılık (r/w): 
    * En fazla sıralı üretilen iş krb5 tarafından etkilenmedi.
    * İş yükü saf yazma 'ya kaydıkça, tamamen etkinin sıfıra düşürüldüğü, saf okuma iş yükleri için en yüksek rastgele g/ç değeri %30 oranında azaltılır. 
    * Maksimum meta veri iş yükü %30 oranında azaltıldı.

Krb5i 'in performans etkisi: 

* Düşük eşzamanlılık (r/w):
    * Sıralı gecikme artan 0,5 ms.
    * Rastgele g/ç gecikme süresi arttı 0,2 ms.
    * Meta veri g/ç gecikme süresi 0,2 ms.
* Yüksek eşzamanlılık (r/w): 
    * İş yükü karışımından bağımsız olarak en fazla sıralı üretilen iş yükü %70 oranında azaltılır.
    * İş yükü saf yazmaya kaydıkça, saf okuma iş yükleri için %50 oranında en yüksek rastgele g/ç azaltılır. 
    * Maksimum meta veri iş yükü %30 oranında azaltıldı.

Krb5p 'in performans etkisi:

* Düşük eşzamanlılık (r/w):
    * Sıralı gecikme artan 0,8 MS.
    * Rastgele g/ç gecikme süresi arttı 0,2 ms.
    * Meta veri g/ç gecikme süresi 0,2 ms.
* Yüksek eşzamanlılık (r/w): 
    * İş yükü karışımından bağımsız olarak en fazla sıralı üretilen iş yükü %85 oranında azaltılır. 
    * İş yükü saf yazmaya kaydıkça, saf okuma 43 iş yükleri için %65 oranında en yüksek rastgele g/ç azalır. 
    * Maksimum meta veri iş yükü %30 oranında azaltıldı.

## <a name="next-steps"></a>Sonraki adımlar  

* [Azure NetApp Files için NFSv4.1 Kerberos şifrelemesini yapılandırma](configure-kerberos-encryption.md) 
