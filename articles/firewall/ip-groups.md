---
title: Azure Güvenlik duvarında IP grupları
description: IP grupları, Azure Güvenlik duvarı kuralları için IP adreslerini Grupve yönetmenize olanak sağlar.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 02/18/2020
ms.author: victorh
ms.openlocfilehash: 74e5a427d62d5249ffe6b0426b62a3577e43462f
ms.sourcegitcommit: 6e87ddc3cc961945c2269b4c0c6edd39ea6a5414
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77444491"
---
# <a name="ip-groups-preview-in-azure-firewall"></a>Azure Güvenlik duvarında IP grupları (Önizleme)

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

IP grupları, Azure Güvenlik duvarı kuralları için IP adreslerini aşağıdaki yollarla Grupve yönetmenize olanak sağlar:

- DNAT kurallarında kaynak adresi olarak
- Ağ kurallarında kaynak veya hedef adres olarak
- Uygulama kurallarında kaynak adresi olarak


Bir IP grubunun tek bir IP adresi, birden çok IP adresi veya bir veya daha fazla IP adresi aralığı olabilir.

Azure 'daki bölgeler ve abonelikler arasında birden fazla güvenlik duvarı için Azure Güvenlik Duvarı DNAT, ağ ve uygulama kurallarında IP grupları yeniden kullanılabilir. Grup adları benzersiz olmalıdır. Azure portal, Azure CLı veya REST API bir IP grubu yapılandırabilirsiniz. Başlamanıza yardımcı olmak için örnek bir şablon sağlanır.

## <a name="sample-format"></a>Örnek biçim

Aşağıdaki IPv4 adresi biçimi örnekleri IP gruplarında kullanılmak üzere geçerlidir:

- Tek adres: 10.0.0.0
- CıDR gösterimi: 10.1.0.0/32
- Adres Aralığı: 10.2.0.0-10.2.0.31

## <a name="create-an-ip-group"></a>IP grubu oluştur

Azure portal, Azure CLı veya REST API kullanılarak bir IP grubu oluşturulabilir. Daha fazla bilgi için bkz. [IP grubu oluşturma (Önizleme)](create-ip-group.md).

## <a name="browse-ip-groups"></a>IP gruplarına gözatamıyorum
1. Azure portal arama çubuğunda **IP grupları** yazın ve seçin. IP gruplarının listesini görebilir veya yeni bir IP grubu oluşturmak için **Ekle** ' yi seçebilirsiniz.
2. Genel Bakış sayfasını açmak için bir IP grubu seçin. IP adreslerini veya IP gruplarını düzenleyebilir, ekleyebilir veya silebilirsiniz.

   ![IP gruplarına genel bakış](media/ip-groups/overview.png)

## <a name="manage-an-ip-group"></a>IP grubunu yönetme

IP grubundaki tüm IP adreslerini ve onunla ilişkili kuralları ya da kaynakları görebilirsiniz. Bir IP grubunu silmek için, önce IP grubunun onu kullanan kaynakla ilişkilendirmesini kaldırmanız gerekir.

1. IP adreslerini görüntülemek veya düzenlemek için sol bölmedeki **Ayarlar** altında **IP adresleri** ' ni seçin.
2. Tek veya birden çok IP adresi eklemek için **IP adresi ekle**' yi seçin. Bu, bir karşıya yükleme için **Sürükle veya araştır** sayfasını açar veya adresi el ile girebilirsiniz.
3.  IP adreslerini düzenlemek veya silmek için sağ taraftaki üç nokta ( **...** ) seçiliyor. Birden çok IP adresini düzenlemek veya silmek için, kutuları seçin ve üst kısımdaki **Düzenle** veya **Sil** ' i seçin.
4. Son olarak, dosyayı CSV dosya biçiminde dışa aktarabilirsiniz.

> [!NOTE]
> Bir kuralda hala kullanımda olduğu sürece bir IP grubundaki tüm IP adreslerini silerseniz, bu kural atlanır.


## <a name="use-an-ip-group"></a>Bir IP grubu kullan

Artık Azure Güvenlik Duvarı DNAT, uygulama veya ağ kuralları oluştururken IP adresleri için **kaynak türü** veya **hedef türü** olarak **IP grubu** ' nu seçebilirsiniz.

> [!NOTE]
> Güvenlik Duvarı Ilkesinde IP grupları desteklenmez. Şu anda yalnızca geleneksel güvenlik duvarı kuralları ile desteklenir.

![Güvenlik duvarında IP grupları](media/ip-groups/fw-ipgroup.png)

## <a name="region-availability"></a>Bölge kullanılabilirliği

IP grupları Şu anda aşağıdaki bölgelerde kullanılabilir:

- Batı ABD
- Batı ABD 2
- Doğu ABD
- Doğu ABD 2
- Orta ABD
- Orta Kuzey ABD
- Orta Batı ABD
- Orta Güney ABD
- Orta Kanada
- Kuzey Avrupa
- Batı Avrupa
- Orta Fransa
- Güney Birleşik Krallık
- Doğu Avustralya
- Orta Avustralya
- Güneydoğu Avustralya

## <a name="related-azure-powershell-cmdlets"></a>İlgili Azure PowerShell cmdlet 'leri

Aşağıdaki Azure PowerShell cmdlet 'leri IP grupları oluşturmak ve yönetmek için kullanılabilir:

- [New-AzIpGroup](https://docs.microsoft.com/powershell/module/az.network/new-azipgroup?view=azps-3.4.0)
- [Remove-AzIPGroup](https://docs.microsoft.com/powershell/module/az.network/remove-azipgroup?view=azps-3.4.0)
- [Get-Azıpgroup](https://docs.microsoft.com/powershell/module/az.network/get-azipgroup?view=azps-3.4.0)
- [Set-AzIpGroup](https://docs.microsoft.com/powershell/module/az.network/set-azipgroup?view=azps-3.4.0)
- [New-AzFirewallNetworkRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallnetworkrule?view=azps-3.4.0)
- [New-AzFirewallApplicationRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallapplicationrule?view=azps-3.4.0)
- [New-AzFirewallNatRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallnatrule?view=azps-3.4.0)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik duvarını dağıtmayı ve yapılandırmayı](tutorial-firewall-deploy-portal.md)öğrenin.