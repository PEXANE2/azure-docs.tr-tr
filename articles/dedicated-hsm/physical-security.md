---
title: HSM fiziksel güvenlik - Azure Özel HSM | Microsoft Dokümanlar
description: Azure Özel HSM cihazlarının veri merkezlerinde fiziksel güvenliği hakkında bilgi
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: mbaldwin
ms.openlocfilehash: ede1af4625d06af4e280eda86d09ae1db3dfdfd1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70881035"
---
# <a name="azure-dedicated-hsm-physical-security"></a>Azure Özel HSM fiziksel güvenlik

Azure Özel HSM, önemli depolama için gelişmiş güvenlik gereksinimlerini karşılamanıza yardımcı olur. Müşterilerin ihtiyaçlarını karşılamak için tüm yaşam döngüsü boyunca sıkı güvenlik uygulamalarını takip ederek yönetilir.

## <a name="security-through-procurement"></a>Satın alma yoluyla güvenlik

Microsoft güvenli bir satın alma işlemini izler. Gözetim zincirini yönetiriz ve sipariş edilen ve gönderilen belirli cihazın veri merkezlerimize gelen cihaz olmasını sağlarız. Cihazlar kurcalama olayı plastik sırtlar bulunmaktadır. Veri merkezinin veri galerisinde görevlendirilene kadar güvenli bir depolama alanında saklanır.  HSM cihazlarını içeren raflar yüksek iş etkisi (HBI) olarak kabul edilir. Cihazlar her zaman önde ve arkada kilitli ve video gözetimi altında.

## <a name="security-through-deployment"></a>Dağıtım yoluyla güvenlik

HSM'ler, ilişkili ağ bileşenleriyle birlikte raflara yüklenir. Yüklendikten sonra, Azure Özel HSM Hizmeti'nin bir parçası olarak kullanıma sunulmadan önce yapılandırılmalı. Bu yapılandırma etkinliği, arka plan denetimine uğramış Microsoft çalışanları tarafından gerçekleştirilir. "Just In Time" (JIT) yönetimi, yalnızca doğru çalışanlara erişimi sınırlamak için ve yalnızca erişimin gerekli olduğu süre için kullanılır. Kullanılan yordamlar ve sistemler, HSM aygıtları ile ilgili tüm etkinliklerin günlüğe kaydedilmesini de sağlar.

## <a name="security-in-operations"></a>Operasyonlarda güvenlik

HSM'ler donanım cihazlarıdır (cihazın içindeki PCI kartı olan gerçek HSM) bu nedenle bileşen düzeyinde sorunlar ortaya çıkabilir. Potansiyel sorunlar fan ve güç kaynağı arızaları içerir ancak bunlarla sınırlı değildir. Bu tür bir olay, değiştirilebilir bileşenlerin yerine bakım veya kesme/düzeltme etkinlikleri gerektirir.

### <a name="component-replacement"></a>Bileşen değişimi

Bir aygıt sağlandıktan ve müşteri yönetimi altında sağlandıktan sonra, sıcak değiştirilebilir güç kaynağı değiştirilecek tek bileşendir. Bu bileşen güvenlik sınırının dışındadır ve bir kurcalama olayına neden olmaz. Bir bilet sistemi, bir Microsoft mühendisine HBI rafının arkasına erişme yetkisi vermek için kullanılır. Bilet işlendiğinde geçici bir fiziksel anahtar verilir. Bu anahtar, mühendise aygıta erişim sağlar ve etkilenen bileşeni değiştirmelerine olanak tanır. Başka bir erişim (diğer bir deyişle, kurcalama olayına neden olan) bir aygıt müşteriye tahsis edilmediğinde yapılır ve böylece güvenlik ve kullanılabilirlik riski en aza indirilir.  

### <a name="device-replacement"></a>Cihaz değiştirme

Toplam aygıt arızası durumunda, bileşen arızası sırasında kullanılana benzer bir işlem izlenir. Bir müşteri aygıtı sıfırlayamazsa veya aygıt bilinmeyen bir durumdaysa, veri taşıyan aygıtlar kaldırılır ve raf içi imha kutusuna yerleştirilir. Çöp kutusuna yerleştirilen cihazlar kontrollü ve güvenli bir şekilde imha edilecektir. HBI rafından veri taşıyan aygıtlar Microsoft veri merkezi bırakmaz.

### <a name="other-rack-access-activities"></a>Diğer Raf Erişim Etkinlikleri

Bir Microsoft mühendisinin HSM aygıtları tarafından kullanılan rafa (örneğin ağ aygıtı bakımı) erişmesi gerekiyorsa, HBI güvenli rafına erişmek için standart güvenlik yordamları kullanılır. Tüm girişler video gözetimi altında olacak. HSM aygıtları [FIPS 140-2 Düzey 3](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.140-2.pdf) olarak doğrulanır, böylece HSM Cihazlarına yetkisiz erişim müşteriye sinyal verilir ve veriler sıfırlanır.

## <a name="logical-level-security-considerations"></a>Mantıksal düzey güvenlik hususları

HSM'ler müşteri tarafından oluşturulan sanal bir ağa verilir. Burası müşterinin özel IUP Adresi alanıdır.  Bu yapılandırma değerli bir mantıksal ağ düzeyi yalıtımı sağlar ve yalnızca müşteri tarafından erişim sağlar. Bu, tüm mantıksal düzey güvenlik denetimlerinin müşterinin sorumluluğunda olduğu anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar

Örneğin yüksek kullanılabilirlik, güvenlik ve desteklenebilirlik gibi hizmetin tüm temel kavramlarının cihaz sağlama, uygulama tasarımı veya dağıtımından önce iyi anlaşılması önerilir.

* [Yüksek Kullanılabilirlik](high-availability.md)
* [Ağ Oluşturma](networking.md)
* [Desteklenebilirlik](supportability.md)
* [İzleme](monitoring.md)
* [Dağıtım mimarisi](deployment-architecture.md)