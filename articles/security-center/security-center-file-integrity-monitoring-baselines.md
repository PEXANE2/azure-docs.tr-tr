---
title: Azure Güvenlik Merkezi'nde Dosya Bütünlüğü İzleme
description: Azure Güvenlik Merkezi'nde taban çizgilerini Dosya Bütünlüğü İzleme ile nasıl karşılaştırışta bulunun.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: c8a2a589-b737-46c1-b508-7ea52e301e8f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/29/2019
ms.author: memildin
ms.openlocfilehash: bb45e1d1ee17a6daf16bd688982f79fda986bde5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73664402"
---
# <a name="compare-baselines-using-file-integrity-monitoring-fim"></a>Dosya Bütünlüğünü İzleme (FIM) kullanarak ana hatları karşılaştırma

Dosya Bütünlüğü İzleme (FIM), kaynaklarınızdaki hassas alanlarda değişiklikler meydana geldiğinde sizi bilgilendirir, böylece yetkisiz etkinliği araştırabilir ve ele alabilirsiniz. FIM, Windows dosyalarını, Windows kayıt defterlerini ve Linux dosyalarını izler.

Bu konu, dosyalarda ve kayıt defterlerinde FIM'in nasıl etkinleştirilen açıklanmaktadır. FIM hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'nde Dosya Bütünlüğü İzleme'ye](security-center-file-integrity-monitoring.md)bakın.

## <a name="why-use-fim"></a>Neden FIM kullanıyorsun?

İşletim sistemi, uygulamalar ve ilişkili yapılandırmalar kaynaklarınızın davranış ve güvenlik durumunu denetler. Bu nedenle, saldırganlar, bir kaynağın işletim sistemini ele geçirmek ve/veya algılanmadan etkinlikleri yürütmek için kaynaklarınızı denetleyen dosyaları hedefler.

Aslında, PCI-DSS & ISO 17799 gibi birçok mevzuata uygunluk standardı FIM denetimlerinin uygulanmasını gerektirir.  

## <a name="enable-built-in-recursive-registry-checks"></a>Yerleşik özyinelemeli kayıt defteri denetimlerini etkinleştirme

FIM kayıt defteri kovan varsayılanları, ortak güvenlik alanlarındaki özyinelemeli değişiklikleri izlemek için kullanışlı bir yol sağlar.  Örneğin, bir rakip, bir yürütmeyi başlangıç veya kapatmada yapılandırarak LOCAL_SYSTEM bağlamda yürütecek bir komut dosyası nı yapılandırabilir.  Bu tür değişiklikleri izlemek için yerleşik denetimi etkinleştirin.  

![Kayıt Defteri](./media/security-center-file-integrity-monitoring-baselines/baselines-registry.png)

>[!NOTE]
> Özyinelemeli denetimler yalnızca önerilen güvenlik kovanları için geçerlidir, özel kayıt defteri yolları için değil.  

## <a name="adding-a-custom-registry-check"></a>Özel kayıt defteri denetimi ekleme

FIM taban çizgileri, işletim sistemi için bilinen iyi bir durum özelliklerini tanımlayarak ve uygulamayı destekleyerek başlar.  Bu örnekte, Windows Server 2008 ve üzeri parola ilkesi yapılandırmalarına odaklanacağız.


|İlke Adı                 | Kayıt Defteri Ayarı|
|---------------------------------------|-------------|
|Etki alanı denetleyicisi: Makine hesabı parola değişikliklerini reddet| MACHINE\System\CurrentControlSet\Services \Netlogon\Parameters\RefusePasswordChange|
|Etki alanı üyesi: Güvenli kanal verisini dijital olarak şifrele veya imzala (her zaman)|MACHINE\System\CurrentControlSet\Services \Netlogon\Parameters\RequireSignOrSeal|
|Etki alanı üyesi: Güvenli kanal verisini dijital olarak şifrele (uygun olduğunda)|MACHINE\System\CurrentControlSet\Services \Netlogon\Parameters\SealSecureChannel|
|Etki alanı üyesi: Güvenli kanal verisini dijital olarak imzala (uygun olduğunda)|MACHINE\System\CurrentControlSet\Services \Netlogon\Parameters\SignSecureChannel|
|Etki alanı üyesi: Makine hesabı parola değişikliklerini devreden çıkar|MACHINE\System\CurrentControlSet\Services \Netlogon\Parameters\DisablePasswordChange|
|Etki alanı üyesi: En uzun hesap parolası yaşı|MACHINE\System\CurrentControlSet\Services \Netlogon\Parameters\MaximumPasswordAge|
|Etki alanı üyesi: Güçlü (Windows 2000 veya daha sonraki) oturum anahtarı gerektir|MACHINE\System\CurrentControlSet\Services \Netlogon\Parameters\RequireStrongKey|
|Ağ güvenliği: NTLM'yi kısıtlama: Bu etki alanında NTLM kimlik doğrulaması|MACHINE\System\CurrentControlSet\Services \Netlogon\Parameters\RestrictNTLMInDomain|
|Ağ güvenliği: NTLM'yi kısıtla: Bu etki alanında sunucu özel durumları ekle|MACHINE\System\CurrentControlSet\Services \Netlogon\Parameters\DCAllowedNTLMServers|
|Ağ güvenliği: NTLM'yi kısıtlama: Bu etki alanında NTLM kimlik doğrulamasını denetleme|MACHINE\System\CurrentControlSet\Services \Netlogon\Parameters\AuditNTLMInDomain|

> [!NOTE]
> Çeşitli işletim sistemi sürümleri tarafından desteklenen kayıt defteri ayarları hakkında daha fazla bilgi edinmek için [Grup İlkesi Ayarları başvuru tablosuna](https://www.microsoft.com/download/confirmation.aspx?id=25250)bakın.

*FiM'i kayıt defteri taban çizgilerini izlemek için yapılandırmak için:*

1. Windows **Registry For Change Registry for Change Tracking** penceresinde, Windows **Kayıt Defteri Anahtarı** metin kutusuna kayıt defteri anahtarını girin.

    <code>

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters
    </code>

      ![Kayıt defterinde FIM'i etkinleştirme](./media/security-center-file-integrity-monitoring-baselines/baselines-add-registry.png)

## <a name="tracking-changes-to-windows-files"></a>Windows dosyalarındaki değişiklikleri izleme

1. Windows **Dosyasını Değiştir İzleme** penceresine, **yol** girin metin kutusuna, izlemek istediğiniz dosyaları içeren klasörü girin. Aşağıdaki şekildeki örnekte, **Contoso Web App** D:\ **ContosWebApp** klasör yapısı içinde sürücü.  
1. Ayar sınıfının adını sağlayarak, yinelemeyi etkinleştirerek ve joker karakter (*) sonekli üst klasörü belirterek özel bir Windows dosya girişi oluşturun.

    ![Dosyada FIM'i etkinleştirme](./media/security-center-file-integrity-monitoring-baselines/baselines-add-file.png)

## <a name="retrieving-change-data"></a>Değişiklik verilerini alma

Dosya Bütünlüğü İzleme verileri Azure Log Analytics / ConfigurationChange tablo kümesi içinde bulunur.  

 1. Kaynağa göre değişikliklerin özetini almak için bir zaman aralığı ayarlayın.
Aşağıdaki örnekte, son on dört gün içinde kayıt defteri ve dosya kategorilerinde tüm değişiklikleri alıyor:

    <code>

    > ConfigurationChange

    > | where TimeGenerated > ago(14d)

    > | where ConfigChangeType in ('Registry', 'Files')

    > | summarize count() by Computer, ConfigChangeType

    </code>

1. Kayıt defteri değişikliklerinin ayrıntılarını görüntülemek için:

    1. **Dosyaları** **where** yan tümcesinden kaldırma, 
    1. Özetleme satırını kaldırın ve bir sıralama yan tümcesi ile değiştirin:

    <code>

    > ConfigurationChange

    > | where TimeGenerated > ago(14d)

    > | where ConfigChangeType in ('Registry')

    > | order by Computer, RegistryKey

    </code>

Raporlar arşivleme için CSV'ye ihraç edilebilir ve/veya Power BI raporuna kanalize edilebilir.  

![FIM verileri](./media/security-center-file-integrity-monitoring-baselines/baselines-data.png)