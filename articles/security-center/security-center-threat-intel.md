---
title: Tehdit istihbaratı & Güvenlik uyarı haritası - Azure Güvenlik Merkezi
description: Azure Güvenlik Merkezi'ndeki güvenlik uyarı haritasını ve tehdit zekası özelliğini kullanarak VM'lerinizdeki ve bilgisayarlarınızdaki olası tehditleri nasıl kullanacağınızı öğrenin.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: a771a3a1-2925-46ca-8e27-6f6a0746f58b
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/3/2018
ms.author: rkarlin
ms.openlocfilehash: 39835d9d73adcbe474d3b70dfced313e18d1a3b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77603423"
---
# <a name="security-alerts-map-and-threat-intelligence"></a>Güvenlik uyarıları haritası ve tehdit bilgileri
Bu makale, güvenlikle ilgili sorunları gidermek için Azure Güvenlik Merkezi güvenlik uyarıları haritasını ve güvenlik olay tabanlı tehdit istihbarat haritasını kullanmanıza yardımcı olur.

> [!NOTE]
> Güvenlik *etkinlikleri* haritası düğmesi 31 Temmuz 2019 tarihinde kullanımdan kaldırılmıştır. Daha fazla bilgi ve alternatif hizmetler [için, Güvenlik Merkezi'nin Emeklilik özellikleri (Temmuz 2019)](security-center-features-retirement-july2019.md#menu_securityeventsmap)bölümüne bakın.


## <a name="how-the-security-alerts-map-works"></a>Güvenlik uyarıları haritası nasıl çalışır?
Güvenlik Merkezi, çevreye yönelik güvenlik tehditlerini belirlemenize yardımcı olan bir harita sağlar. Örneğin, belirli bir bilgisayarın bir botnetin parçası olup olmadığını ve tehdidin nereden geldiğini belirleyebilirsiniz. Saldırganlar botnet'i yöneten komut ve denetimle gizlice etkileşimedebilen kötü amaçlı yazılımları yasadışı olarak yüklediğinde bilgisayarlar bir botnet'te düğüm haline gelebilir. 

Bu haritayı oluşturmak için Güvenlik Merkezi, Microsoft içindeki birden çok kaynaktan gelen verileri kullanır. Güvenlik Merkezi bu verileri, ortamınıza yönelik olası tehditleri eşlemek için kullanır. 

[Güvenlik olayı yanıt süreci](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide#incident-response) adımlarından biri, tehlike altındaki sistemlerin önem derecesini tanımlamaktır. Bu aşamada aşağıdaki görevleri gerçekleştirmeniz gerekir:

- Saldırının yapısını belirleme.
- Saldırının çıkış noktasını belirleyin.
- Saldırının amacını belirleme. Saldırı kuruluşunuza belirli bilgileri elde etme amacıyla mı yapıldı yoksa rastgele bir saldırı mıydı?
- Gizliliği tehlikeye girmiş sistemleri tanımlayın.
- Erişilen dosyaları ve bu dosyaların gizlilik düzeyini tanımlayın.

Bu görevlere yardımcı olmak için Güvenlik Merkezi'ndeki Güvenlik uyarıları haritasını kullanabilirsiniz.

## <a name="access-the-security-alerts-map"></a>Güvenlik uyarıları haritasına erişin
Ortamınızdaki geçerli tehditleri görselleştirmek için Güvenlik uyarıları haritasını açın:

1. **Güvenlik Merkezi** panosunu açın.
2. Sol bölmede, **Tehdit Koruması** altında Güvenlik **uyarıları haritasını**seçin. Harita açılıyor.
3. Uyarı hakkında daha fazla bilgi almak ve düzeltme adımları almak için haritadaki Uyarı noktasını tıklayın ve yönergeleri izleyin. 
 
Güvenlik uyarıları haritası uyarılara dayanır. Bu uyarılar, ip adresibilinen riskli bir IP adresi (örneğin, bilinen bir kriptomadenci) veya tanınmayan bir IP adresi olsun veya olmasın, ağ iletişiminin başarıyla çözülmüş bir IP adresiyle ilişkili olduğu etkinliklere dayanır. daha önce riskli olarak. Harita, Azure'da daha önce seçtiğiniz abonelikler arasında uyarılar sağlar. 

Haritadaki uyarılar, nereden geldiklerinin algılandığı coğrafi konuma göre görüntülenir ve önem derecesine göre renk kodlanır. 
    ![Tehdit bilgileri](./media/security-center-threat-intel/security-center-alert-map.png)



## <a name="see-also"></a>Ayrıca bkz.
Bu makalede Güvenlik Merkezi'ndeki tehdit bilgilerini kullanarak şüpheli etkinlikleri tanımlamayı öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını yönetin ve yanıtlayın.](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) Güvenlik Merkezi'nde uyarıları yönetme ve güvenlik olaylarına yanıt vermeyi öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Farklı güvenlik uyarısı türleri hakkında bilgi edinin.
* [Azure Güvenlik Merkezi sorun giderme kılavuzu](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Güvenlik Merkezi’nde sık karşılaşılan sorunları gidermeyi öğrenin.