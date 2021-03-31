---
title: Azure Güvenlik duvarında IP grupları
description: IP grupları, Azure Güvenlik duvarı kuralları için IP adreslerini Grupve yönetmenize olanak sağlar.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 01/22/2021
ms.author: victorh
ms.openlocfilehash: 907ed0774db0e07a61a3ad568e6c451ea5d37b44
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "102501745"
---
# <a name="ip-groups-in-azure-firewall"></a>Azure Güvenlik duvarında IP grupları

IP grupları, Azure Güvenlik duvarı kuralları için IP adreslerini aşağıdaki yollarla Grupve yönetmenize olanak sağlar:

- DNAT kurallarında kaynak adresi olarak
- Ağ kurallarında kaynak veya hedef adres olarak
- Uygulama kurallarında kaynak adresi olarak


Bir IP grubunun tek bir IP adresi, birden çok IP adresi veya bir veya daha fazla IP adresi aralığı olabilir.

Azure 'daki bölgeler ve abonelikler arasında birden fazla güvenlik duvarı için Azure Güvenlik Duvarı DNAT, ağ ve uygulama kurallarında IP grupları yeniden kullanılabilir. Grup adları benzersiz olmalıdır. Azure portal, Azure CLı veya REST API bir IP grubu yapılandırabilirsiniz. Başlamanıza yardımcı olmak için örnek bir şablon sağlanır.

> [!NOTE]
> IP grupları Şu anda Azure Ulusal bulut ortamlarında kullanılamıyor.

## <a name="sample-format"></a>Örnek biçimi

Aşağıdaki IPv4 adresi biçimi örnekleri IP gruplarında kullanılmak üzere geçerlidir:

- Tek adres: 10.0.0.0
- CıDR gösterimi: 10.1.0.0/32
- Adres Aralığı: 10.2.0.0-10.2.0.31

## <a name="create-an-ip-group"></a>IP grubu oluştur

Azure portal, Azure CLı veya REST API kullanılarak bir IP grubu oluşturulabilir. Daha fazla bilgi için bkz. [IP grubu oluşturma](create-ip-group.md).

## <a name="browse-ip-groups"></a>IP gruplarına gözatamıyorum
1. Azure portal arama çubuğunda **IP grupları** yazın ve seçin. IP gruplarının listesini görebilir veya yeni bir IP grubu oluşturmak için **Ekle** ' yi seçebilirsiniz.
2. Genel Bakış sayfasını açmak için bir IP grubu seçin. IP adreslerini veya IP gruplarını düzenleyebilir, ekleyebilir veya silebilirsiniz.

   ![IP gruplarına genel bakış](media/ip-groups/overview.png)

## <a name="manage-an-ip-group"></a>IP grubunu yönetme

IP grubundaki tüm IP adreslerini ve onunla ilişkili kuralları ya da kaynakları görebilirsiniz. Bir IP grubunu silmek için, önce IP grubunun onu kullanan kaynakla ilişkilendirmesini kaldırmanız gerekir.

1. IP adreslerini görüntülemek veya düzenlemek için sol bölmedeki **Ayarlar** altında **IP adresleri** ' ni seçin.
2. Tek veya birden çok IP adresi eklemek için **IP adresi ekle**' yi seçin. Bu, bir karşıya yükleme için **Sürükle veya araştır** sayfasını açar veya adresi el ile girebilirsiniz.
3.    IP adreslerini düzenlemek veya silmek için sağ taraftaki üç nokta (**...**) seçiliyor. Birden çok IP adresini düzenlemek veya silmek için, kutuları seçin ve üst kısımdaki **Düzenle** veya **Sil** ' i seçin.
4. Son olarak, dosyayı CSV dosya biçiminde dışa aktarabilirsiniz.

> [!NOTE]
> Bir kuralda hala kullanımda olduğu sürece bir IP grubundaki tüm IP adreslerini silerseniz, bu kural atlanır.


## <a name="use-an-ip-group"></a>Bir IP grubu kullan

Artık Azure Güvenlik Duvarı DNAT, uygulama veya ağ kuralları oluştururken IP adresleri için **kaynak türü** veya **hedef türü** olarak **IP grubu** ' nu seçebilirsiniz.

![Güvenlik duvarında IP grupları](media/ip-groups/fw-ipgroup.png)

## <a name="region-availability"></a>Bölge kullanılabilirliği

IP grupları tüm genel bulut bölgelerinde kullanılabilir.

## <a name="ip-address-limits"></a>IP adresi sınırları

Her bir IP grubu için en fazla 5000 ayrı IP adresi veya IP ön ekleri olan güvenlik duvarı başına en fazla 100 IP grubuna sahip olabilirsiniz.

## <a name="related-azure-powershell-cmdlets"></a>İlgili Azure PowerShell cmdlet 'leri

Aşağıdaki Azure PowerShell cmdlet 'leri IP grupları oluşturmak ve yönetmek için kullanılabilir:

- [New-AzIpGroup](/powershell/module/az.network/new-azipgroup)
- [Remove-AzIPGroup](/powershell/module/az.network/remove-azipgroup)
- [Get-AzIpGroup](/powershell/module/az.network/get-azipgroup)
- [Set-AzIpGroup](/powershell/module/az.network/set-azipgroup)
- [New-AzFirewallNetworkRule](/powershell/module/az.network/new-azfirewallnetworkrule)
- [New-AzFirewallApplicationRule](/powershell/module/az.network/new-azfirewallapplicationrule)
- [New-AzFirewallNatRule](/powershell/module/az.network/new-azfirewallnatrule)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik duvarını dağıtmayı ve yapılandırmayı](tutorial-firewall-deploy-portal.md)öğrenin.