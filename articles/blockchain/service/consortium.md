---
title: Azure Blockchain Service konsorsiyumu
description: Azure Blockchain hizmetinin konsorsiyum blockchain ağlarını nasıl uyguladığına genel bakış.
ms.date: 11/21/2019
ms.topic: conceptual
ms.reviewer: zeyadr
ms.openlocfilehash: 7b8885ba08d35db20d1eb7e75141cb173913b386
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247624"
---
# <a name="azure-blockchain-service-consortium"></a>Azure Blockchain Service konsorsiyumu

Azure Blockchain Hizmetini kullanarak, her blockchain ağının ağdaki belirli katılımcılarla sınırlandırılabildiği özel konsorsiyum blockchain ağları oluşturabilirsiniz. Yalnızca özel konsorsiyum blockchain ağındaki katılımcılar blockchain'i görüntüleyebilir ve bu ağla etkileşimde bulunabilir. Azure Blockchain Hizmeti'ndeki konsorsiyum ağları iki tür üye katılımcı rolü içerebilir:

* **Yönetici** - Konsorsiyum yönetimi eylemlerinde başarılı olan ve blockchain işlemlerine katılabilen ayrıcalıklı katılımcılar.

* **Kullanıcı** - Herhangi bir konsorsiyum yönetimi eyleminde bulunmayan ancak blockchain işlemlerine katılabilen katılımcılar.

Konsorsiyum ağları katılımcı rollerinin bir karışımı olabilir ve her rol türünün rasgele bir numarası olabilir. En az bir yönetici olmalıdır.

Aşağıdaki diyagram, birden çok katılımcısı olan bir konsorsiyum ağını gösterir:

![Özel konsorsiyum ağ diyagramı](./media/consortium/network-diagram.png)

Azure Blockchain Hizmeti'ndeki konsorsiyum yönetimi yle, konsorsiyum ağındaki katılımcıları yönetebilirsiniz. Konsorsiyumun yönetimi, ağın konsensüs modeline dayanmaktadır. Geçerli önizleme sürümünde, Azure Blockchain Hizmeti konsorsiyum yönetimi için merkezi bir konsensüs modeli sağlar. Yönetici rolü olan herhangi bir ayrıcalıklı katılımcı, katılımcıekleme veya ağdan kaldırma gibi konsorsiyum yönetimi eylemleri yapabilir.

## <a name="roles"></a>Roller

Konsorsiyuma katılanlar kişi veya kuruluş olabilir ve bir kullanıcı rolü veya yönetici rolü atanabilir. Aşağıdaki tabloda iki rol arasındaki üst düzey farklar listelenebvardır:

| Eylem | Kullanıcı rolü | Yönetici rolü
|--------|:----:|:------------:|
| Yeni üye oluşturma | Evet | Evet |
| Yeni üyeler davet edin | Hayır | Evet |
| Üye katılımcı rolünü ayarlama veya değiştirme | Hayır | Evet |
| Üye ekran adını değiştirme | Sadece kendi üye için | Sadece kendi üye için |
| Üyeleri kaldırma | Sadece kendi üye için | Evet |
| Blockchain işlemlerine katılın | Evet | Evet |

### <a name="user-role"></a>Kullanıcı rolü

Kullanıcılar, yönetici yetenekleri olmayan konsorsiyum katılımcılarıdır. Konsorsiyumla ilgili üyelerin yönetimine katılamazlar. Kullanıcılar üye ekran adlarını değiştirebilir ve kendilerini bir konsorsiyumdan kaldırabilirler.

### <a name="administrator"></a>Yönetici

Bir yönetici konsorsiyum içindeki üyeleri yönetebilir. Yönetici üyeleri davet edebilir, üyeleri kaldırabilir veya konsorsiyum içindeki üye rollerini güncelleyebilir.
Konsorsiyumda her zaman en az bir yönetici olmalıdır. Son yönetici, konsorsiyumdan ayrılmadan önce yönetici rolü olarak başka bir katılımcı belirtmelidir.

## <a name="managing-members"></a>Üyeleri yönetme

Konsorsiyuma yalnızca yöneticiler davet edebilir. Yöneticiler katılımcıları Azure abonelik kimliklerini kullanarak davet ederler.

Katılımcılar davet edildikten sonra Azure Blockchain Hizmeti'ne yeni bir üye yerleştirerek blockchain konsorsiyumuna katılabilirler. Davet edilen konsorsiyumu görüntülemek ve katılmak için, ağ yöneticisi tarafından davette kullanılan aynı Azure abonelik kimliğini belirtmeniz gerekir.

Yöneticiler, diğer yöneticiler de dahil olmak üzere herhangi bir katılımcıyı konsorsiyumdan çıkarabilir. Üyeler sadece kendilerini bir konsorsiyumdan çıkarabilirler.

## <a name="consortium-management-smart-contract"></a>Konsorsiyum yönetimi akıllı sözleşme

Azure Blockchain Service'de konsorsiyum yönetimi, konsorsiyum yönetimi akıllı sözleşmeleri ile yapılır. Yeni bir blockchain üyesi dağıttığınızda akıllı sözleşmeler düğümlerinize otomatik olarak dağıtılır.

Kök konsorsiyum yönetimi akıllı sözleşmenin adresi Azure portalında görülebilir. **RootContract adresi** blockchain üyesinin genel bakış bölümünde yer almaktadır.

![RootContract adresi](./media/consortium/rootcontract-address.png)

Konsorsiyum yönetimi [powershell modüllerini,](manage-consortium-powershell.md)Azure portalını kullanarak veya doğrudan Azure Blockchain Hizmeti oluşturulan Ethereum hesabını kullanarak akıllı sözleşme aracılığıyla konsorsiyum yönetimi akıllı sözleşme ile etkileşim kurabilirsiniz.

## <a name="ethereum-account"></a>Ethereum hesabı

Bir üye oluşturulduğunda, bir Ethereum hesap anahtarı oluşturulur. Azure Blockchain Service, konsorsiyum yönetimiyle ilgili işlemler oluşturmak için bu anahtarı kullanır. Ethereum hesap anahtarı Azure Blockchain Service tarafından otomatik olarak yönetilir.

Üye hesabı Azure portalında görüntülenebilir. Üye hesabı blockchain üyesinin genel bakış bölümündeyer almaktadır.

![Üye hesabı](./media/consortium/member-account.png)

Üye hesabınıza tıklayıp yeni bir şifre girerek Ethereum hesabınızı sıfırlayabilirsiniz. Hem Ethereum hesap adresi hem de parola sıfırlanacaktır.  

## <a name="next-steps"></a>Sonraki adımlar

Konsorsiyum yönetimi faaliyetlerine PowerShell üzerinden erişilebilir. Daha fazla bilgi için [PowerShell'i kullanarak Azure Blockchain Hizmeti'ndeki konsorsiyum üyelerini yönet'e](manage-consortium-powershell.md)bakın.
