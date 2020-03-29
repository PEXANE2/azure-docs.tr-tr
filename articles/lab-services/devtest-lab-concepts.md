---
title: DevTest Labs kavramları | Microsoft Dokümanlar
description: DevTest Labs'ın temel kavramlarını ve Azure sanal makinelerinin oluşturulmasını, yönetilmesini ve izlenmesini nasıl kolaylaştırabileceğinizi öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 105919e8-3617-4ce3-a29f-a289fa608fb2
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: spelluru
ms.openlocfilehash: 22fd78ccd58be1790fcd167da396600e8b876564
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75428910"
---
# <a name="devtest-labs-concepts"></a>DevTest Labs kavramları
## <a name="overview"></a>Genel Bakış
Aşağıdaki liste önemli DevTest Labs kavramları ve tanımları içerir:

## <a name="labs"></a>Labs
Laboratuvar, Sanal Makineler (VM' ler) gibi bir kaynak grubunu kapsayan ve sınırları ve kotaları belirterek bu kaynakları daha iyi yönetmenize olanak tanıyan altyapıdır.

## <a name="virtual-machine"></a>Sanal makine
Azure VM, Azure'un sunduğu [isteğe bağlı, ölçeklenebilir bilgi işlem kaynaklarından](/azure/architecture/guide/technology-choices/compute-decision-tree) biridir. Azure VM'ler, çalıştıran fiziksel donanımı satın almak ve korumak zorunda kalmadan sanallaştırma esnekliği sağlar, ancak yine de çalışan yazılımı yapılandırma, düzeltme ve yükleme gibi belirli görevleri gerçekleştirerek VM'yi korumanız gerekir bu kadar.

[Azure'daki Windows sanal makinelerine genel bakış,](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-overview) bir VM oluşturmadan önce neleri göz önünde bulundurmanız gerektiği, nasıl oluşturduğunuz ve nasıl yönettiğiniz hakkında bilgi verir.

## <a name="claimable-vm"></a>Talep Edilebilir VM
Azure Claimable VM, izinleri olan herhangi bir laboratuvar kullanıcısı tarafından kullanılabilen sanal bir makinedir. Laboratuvar yöneticisi, belirli temel görüntüler ve yapılarla VM'leri hazırlayabilir ve bunları paylaşılan bir havuza kaydedebilir. Bir laboratuvar kullanıcısı daha sonra, bu özel yapılandırmaya sahip bir gereksinim duyduğunda havuzdan çalışan bir VM talep edebilir.

İddia edilebilir bir VM başlangıçta belirli bir kullanıcıya atanmaz, ancak her kullanıcının listesinde "Talep edilebilir sanal makineler" altında gösterecektir. Bir VM bir kullanıcı tarafından talep edildikten sonra, kendi "Benim sanal makineleri" alanına taşınır ve artık başka bir kullanıcı tarafından talep edilebilir.

## <a name="environment"></a>Ortam
DevTest Labs'da bir ortam, bir laboratuvardaki Azure kaynakları koleksiyonunu ifade eder. [Bu blog gönderisi,](https://blogs.msdn.microsoft.com/devtestlab/2016/11/16/connect-2016-news-for-azure-devtest-labs-azure-resource-manager-template-based-environments-vm-auto-shutdown-and-more/) Azure Kaynak Yöneticisi şablonlarınızdan çoklu VM ortamlarının nasıl oluşturulabildiğini tartışır.

## <a name="base-images"></a>Temel görüntüler
Temel görüntüler, hızlı bir şekilde bir VM oluşturmak için önceden yüklenmiş ve yapılandırılmış tüm araçlar ve ayarları ile VM görüntüleridir. Varolan bir tabanı seçerek ve test aracınızı yüklemek için bir yapı ekleyerek bir VM sağlayabilirsiniz. Daha sonra, temelin VM'nin her hükmü için test aracısını yeniden yüklemek zorunda kalmadan kullanılabileceğini sağlamak için, sağlanan VM'yi bir temel olarak kaydedebilirsiniz.

## <a name="artifacts"></a>Yapıtlar
Yapılar, bir VM sağlandıktan sonra uygulamanızı dağıtmak ve yapılandırmak için kullanılır. Eserler olabilir:

* Aracılar, Kemancı ve Visual Studio gibi VM'ye yüklemek istediğiniz araçlar.
* VM'de çalıştırmak istediğiniz eylemler - örneğin bir repo klonlama.
* Test etmek istediğiniz uygulamalar.

Yapılar, dağıtım gerçekleştirmek ve yapılandırma uygulamak için yönergeler içeren [Azure Kaynak Yöneticisi](../azure-resource-manager/management/overview.md) JSON dosyalarıdır.

## <a name="artifact-repositories"></a>Artefakt depoları
Artefakt depoları, yapıtların iade edildiği git depolarıdır. Yapı depoları, yeniden kullanımı ve paylaşımı sağlayan kuruluşunuzdaki birden çok laboratuvara eklenebilir.

## <a name="formulas"></a>Formüller
Formüller, temel görüntülere ek olarak, hızlı VM sağlama için bir mekanizma sağlar. DevTest Labs'daki formül, laboratuvar VM'si oluşturmak için kullanılan varsayılan özellik değerlerinin listesidir.
Formüllerle, temel görüntü, VM boyutu, sanal ağ ve yapı teşekkünleri gibi aynı özelliklere sahip VM'ler, bu özellikleri her seferinde belirtmeye gerek kalmadan oluşturulabilir. Bir formülden VM oluştururken, varsayılan değerler olduğu gibi kullanılabilir veya değiştirilebilir.

## <a name="policies"></a>İlkeler
İlkeler, laboratuarınızdaki maliyeti denetlemeye yardımcı olur. Örneğin, tanımlı bir zamanlamaya dayalı olarak VM'leri otomatik olarak kapatmak için bir ilke oluşturabilirsiniz.

## <a name="caps"></a>Şapkalar
Kapaklar, laboratuarınızdaki atıkları en aza indirmek için bir mekanizmadır. Örneğin, kullanıcı başına veya bir laboratuvarda oluşturulabilecek VM sayısını kısıtlamak için bir kapak ayarlayabilirsiniz.

## <a name="security-levels"></a>Güvenlik düzeyleri
Güvenlik erişimi Azure Rolü Tabanlı Erişim Denetimi (RBAC) tarafından belirlenir. Erişimin nasıl çalıştığını anlamak için, RBAC tarafından tanımlanan izin, rol ve kapsam arasındaki farkları anlamanıza yardımcı olur.

* İzin - İzin, belirli bir eyleme (örn. tüm sanal makinelere okuma-erişim) tanımlanmış bir erişimdir.
* Rol - Rol, bir kullanıcıya gruplandırılabilen ve atanan izinler kümesidir. Örneğin, *abonelik sahibi* rolü abonelik içindeki tüm kaynaklara erişebilir.
* Kapsam - Kapsam, kaynak grubu, tek bir laboratuvar veya tüm abonelik gibi bir Azure kaynağının hiyerarşisi içindeki bir düzeydir.

DevTest Labs kapsamında, kullanıcı izinlerini tanımlamak için iki tür rol vardır: laboratuvar sahibi ve laboratuvar kullanıcısı.

* Laboratuvar Sahibi - Bir laboratuvar sahibi, laboratuvar içindeki tüm kaynaklara erişebilir. Bu nedenle, bir laboratuvar sahibi ilkeleri değiştirebilir, herhangi bir Sanal M'leri okuyabilir ve yazabilir, sanal ağı değiştirebilir ve benzeri şeyler yapabilir.
* Laboratuvar Kullanıcısı - Laboratuvar kullanıcısı, VM'ler, ilkeler ve sanal ağlar gibi tüm laboratuvar kaynaklarını görüntüleyebilir, ancak ilkeleri veya diğer kullanıcılar tarafından oluşturulan vm'leri değiştiremez.

DevTest Labs'da özel rollerin nasıl oluşturularak oluşturulabildiğini görmek için makaleye bakın, [kullanıcı izinlerini belirli laboratuvar ilkelerine ver.](devtest-lab-grant-user-permissions-to-specific-lab-policies.md)

Kapsamlar hiyerarşik olduğundan, bir kullanıcının belirli bir kapsamda izinleri olduğunda, bu izinler kapsanan her alt düzey kapsamda otomatik olarak verilir. Örneğin, bir kullanıcı abonelik sahibi rolüne atanmışsa, tüm sanal makineleri, tüm sanal ağları ve tüm laboratuarları içeren bir abonelikteki tüm kaynaklara erişebilir. Bu nedenle, bir abonelik sahibi otomatik olarak laboratuvar sahibi rolünü devralır. Ancak, tam tersi doğru değildir. Laboratuvar sahibi, abonelik düzeyinden daha düşük bir kapsam olan bir laboratuvara erişebilir. Bu nedenle, bir laboratuvar sahibi sanal makineleri veya sanal ağları veya laboratuvar dışında herhangi bir kaynak görmek mümkün olmayacaktır.

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları
Bu makalede tartışılan tüm kavramlar, Azure çözümünüzün altyapısını/yapılandırmasını tanımlamanızı ve tutarlı bir durumda sürekli olarak dağıtmanızı sağlayan Azure Kaynak Yöneticisi şablonları kullanılarak yapılandırılabilir.

[Azure Kaynak Yöneticisi şablonlarının yapısını ve sözdizimini anlamak,](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates#template-format) Azure Kaynak Yöneticisi şablonunun yapısını ve şablonun farklı bölümlerinde bulunan özellikleri açıklar.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
[DevTest Labs'da laboratuvar oluşturma](devtest-lab-create-lab.md)
