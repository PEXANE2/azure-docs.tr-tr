---
title: Paylaşılan kaynakla Laboratuvar oluşturma | Azure Lab Services
description: Öğrenciler arasında paylaşılan bir kaynak gerektiren bir laboratuvar oluşturmayı öğrenin.
services: lab-services
author: emaher
ms.service: lab-services
ms.topic: article
ms.date: 06/05/2020
ms.author: enewman
ms.openlocfilehash: c8e8c99b40eb2d84d001076186a254988377361d
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84895944"
---
# <a name="how-to-create-a-lab-with-a-shared-resource-in-azure-lab-services"></a>Azure Lab Services paylaşılan kaynakla Laboratuvar oluşturma

Bazen bir sınıf Laboratuvarı oluştururken, bir laboratuvardaki tüm öğrenciler arasında paylaşılması gereken bazı kaynaklar olabilir.  Örneğin, bir lisans sunucunuz veya bir veritabanı sınıfı için SQL Server vardır.  Bu makalede, bir laboratuvarda paylaşılan kaynağı etkinleştirme adımları ele alınacaktır.  Ayrıca, bu paylaşılan kaynağa erişimin nasıl sınırlandırılacağunu inceleyeceğiz.

## <a name="architecture"></a>Mimari

Aşağıdaki diyagramda gösterildiği gibi laboratuvarı olan bir laboratuar hesabı olacaktır.  Laboratuvar hesabı VNET eşleme ayarlarına sahip olacaktır, bu nedenle laboratuvar için sanal ağ paylaşılan kaynağın ağına bağlanır.  Aşağıdaki diyagramda, çakışmayan IP aralıklarına sahip iki sanal ağ vardır.  Bu IP aralıkları yalnızca örnek aralıklardır.  Ayrıca, paylaşılan kaynak sanal ağının laboratuvar hesabıyla aynı abonelikte olduğunu unutmayın.

![Paylaşılan kaynak mimarisi olan Laboratuvar Hizmetleri](./media/how-to-create-a-lab-with-shared-resource/shared-resource-architecture.png)

## <a name="setup-shared-resource"></a>Paylaşılan kaynak ayarla

Laboratuvar oluşturulmadan önce paylaşılan kaynağın sanal ağı oluşturulmalıdır.  Sanal ağ oluşturma hakkında daha fazla bilgi için bkz. [sanal ağ oluşturma](../virtual-network/quick-create-portal.md).  Sanal ağ aralıklarını, laboratuvar makinelerinin IP adresiyle çakışmayacak şekilde planlama önemli değildir.  Ağınızı planlama hakkında daha fazla bilgi için bkz. [sanal ağları](../virtual-network/virtual-network-vnet-plan-design-arm.md) planlama makalesi. Örneğimizde, paylaşılan kaynak 10.2.0.0/16 aralığına sahip bir sanal ağda bulunur.  Henüz yapılmazsa, paylaşılan kaynağı tutmak için [bir alt ağ oluşturun](../virtual-network/virtual-network-manage-subnet.md#add-a-subnet) .  Örnekte, 10.2.0.0/24 aralığını kullanıyoruz, ancak aralığınız ağınızın ihtiyaçlarına bağlı olarak farklı olabilir.

Paylaşılan kaynak, bir sanal makinede veya Azure tarafından sağlanmış bir hizmette çalışan yazılım olabilir. Paylaşılan kaynak özel IP adresi ile kullanılabilir olmalıdır.  Paylaşılan kaynağı yalnızca özel IP aracılığıyla kullanılabilir hale getirerek, bu paylaşılan kaynağa erişimi sınırlayamadınız.

Diyagramda Ayrıca, öğrenci VM 'sinden gelen trafiği kısıtlamak için kullanılabilen bir ağ güvenlik grubu (NSG) gösterilmektedir.  Örneğin, öğrenci VM 'sinin IP adreslerinden gelen trafiğin yalnızca bir paylaşılan kaynağa erişebileceğini ve başka hiçbir şey olamayacağını belirten bir güvenlik kuralı yazabilirsiniz.  Güvenlik kurallarını ayarlama hakkında daha fazla bilgi için bkz. [ağ güvenlik grubunu yönetme](../virtual-network/manage-network-security-group.md#work-with-security-rules). Paylaşılan bir kaynağa erişimi belirli bir laboratuvara kısıtlamak istiyorsanız, laboratuvar [hesabından laboratuvar ayarlarından](manage-labs.md#view-labs-in-a-lab-account) LABORATUVARıN IP adresini alın ve yalnızca o IP adresinden erişime izin vermek için bir gelen kuralı ayarlayın.  Bu IP adresi için 49152 ile 65535 arasında bağlantı kurulmasına izin vermeyi unutmayın.  İsteğe bağlı olarak, [sanal makine havuzu sayfasını](how-to-set-virtual-machine-passwords.md)kullanarak öğrencinin VM 'LERININ özel IP adresini bulabilirsiniz.

Paylaşılan kaynağınız gerekli yazılımları çalıştıran bir Azure sanal makinedir, sanal makine için varsayılan güvenlik duvarı kurallarını değiştirmeniz gerekebilir.

## <a name="lab-account"></a>Laboratuvar hesabı

Paylaşılan bir kaynak kullanmak için, laboratuvar hesabının eşlenmiş bir [sanal ağı](how-to-connect-peer-virtual-network.md)kullanacak şekilde ayarlanması gerekir.  Bu durumda, paylaşılan kaynağı tutan sanal ağ ile eşliyoruz.

>[!WARNING]
>Laboratuvar hesabı paylaşılan kaynak sanal ağı 'na **eşlendikten sonra** sınıfınız için laboratuvar oluşturulması gerekir.  
Şablon makinesi

Laboratuvar hesabınız sanal ağa eşlendikten sonra, şablon makinenin artık paylaşılan kaynağa erişimi olmalıdır.  Erişilmekte olan paylaşılan kaynağa bağlı olarak güvenlik duvarı kurallarını güncelleştirmeniz gerekebilir.
