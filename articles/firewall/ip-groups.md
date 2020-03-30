---
title: Azure Güvenlik Duvarındaki IP Grupları
description: IP grupları, Azure Güvenlik Duvarı kuralları için IP adreslerini gruplandırmanızı ve yönetmeniz için izin verir.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 02/18/2020
ms.author: victorh
ms.openlocfilehash: 74e5a427d62d5249ffe6b0426b62a3577e43462f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77444491"
---
# <a name="ip-groups-preview-in-azure-firewall"></a>Azure Güvenlik Duvarı'nda IP Grupları (önizleme)

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

IP Grupları, Azure Güvenlik Duvarı kuralları için IP adreslerini aşağıdaki yollarla gruplandırmanıza ve yönetmenize olanak tanır:

- DNAT kurallarında kaynak adres olarak
- Ağ kurallarında kaynak veya hedef adres olarak
- Uygulama kurallarında kaynak adres olarak


BIR IP Grubu'nun tek bir IP adresi, birden çok IP adresi veya bir veya daha fazla IP adresi aralığı olabilir.

IP Grupları, Azure Güvenlik Duvarı DNAT'sinde, ağda ve uygulama kurallarında, bölgeler ve Azure'daki abonelikler arasında birden çok güvenlik duvarı için yeniden kullanılabilir. Grup adları benzersiz olmalıdır. Azure portalında, Azure CLI'de veya REST API'de bir IP Grubu yapılandırabilirsiniz. Başlamanıza yardımcı olmak için örnek bir şablon sağlanır.

## <a name="sample-format"></a>Örnek format

Aşağıdaki IPv4 adres biçimi örnekleri IP Gruplarında kullanılmak üzere geçerlidir:

- Tek adres: 10.0.0.0
- CIDR gösterimi: 10.1.0.0/32
- Adres aralığı: 10.2.0.0-10.2.0.31

## <a name="create-an-ip-group"></a>IP Grubu Oluşturma

Bir IP Grubu Azure portalı, Azure CLI veya REST API kullanılarak oluşturulabilir. Daha fazla bilgi için ip [grubu oluşturma (önizleme)](create-ip-group.md)bilgisine bakın.

## <a name="browse-ip-groups"></a>IP Gruplarına Gözat
1. Azure portalı arama çubuğunda **IP Grupları** yazın ve seçin. IP Grupları listesini görebilir veya yeni bir IP Grubu oluşturmak için **Ekle'yi** seçebilirsiniz.
2. Genel bakış sayfasını açmak için bir IP Grubu seçin. IP adreslerini veya IP Gruplarını düzenleyebilir, ekleyebilir veya silebilirsiniz.

   ![IP Gruplarına genel bakış](media/ip-groups/overview.png)

## <a name="manage-an-ip-group"></a>IP Grubunu Yönetme

IP Grubu'ndaki tüm IP adreslerini ve ip grubuyla ilişkili kuralları veya kaynakları görebilirsiniz. Bir IP Grubunu silmek için, önce IP Grubunu kullanan kaynaktan ayırmanız gerekir.

1. IP adreslerini görüntülemek veya düzenlediklerini görmek için sol bölmedeki **Ayarlar'ın** altındaki **IP Adreslerini** seçin.
2. Tek veya birden çok IP adresi(es) eklemek için **IP Adresleri Ekle'yi**seçin. Bu, yükleme için **Sürükle veya Gözat** sayfasını açar veya adresi el ile girebilirsiniz.
3.  IP adreslerini düzenlemek veya silmek için sağa (**...**) elipsleri seçme. Birden çok IP adresini düzenlemek veya silmek için kutuları seçin ve en üstte **Düzenle** veya **Sil'i** seçin.
4. Son olarak, dosyayı CSV dosya biçiminde dışa aktarabilirsiniz.

> [!NOTE]
> Bir kuralda hala kullanılıyorken ip grubundaki tüm IP adreslerini silerseniz, bu kural atlanır.


## <a name="use-an-ip-group"></a>IP Grubu Kullanma

Azure Güvenlik Duvarı DNAT,uygulama veya ağ kuralları oluştururken IP adresi(es) için **Kaynak türü** veya **Hedef türü** olarak **IP Grubu'nu** seçebilirsiniz.

> [!NOTE]
> IP Grupları Güvenlik Duvarı Politikası'nda desteklenmez. Şu anda yalnızca geleneksel güvenlik duvarı kurallarıyla desteklenir.

![Güvenlik Duvarındaki IP Grupları](media/ip-groups/fw-ipgroup.png)

## <a name="region-availability"></a>Bölge kullanılabilirliği

IP Grupları şu anda aşağıdaki bölgelerde kullanılabilir:

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

## <a name="related-azure-powershell-cmdlets"></a>İlgili Azure PowerShell cmdlets

Aşağıdaki Azure PowerShell cmdlets IP Grupları oluşturmak ve yönetmek için kullanılabilir:

- [New-AzIpGroup](https://docs.microsoft.com/powershell/module/az.network/new-azipgroup?view=azps-3.4.0)
- [Kaldır-AzIPGroup](https://docs.microsoft.com/powershell/module/az.network/remove-azipgroup?view=azps-3.4.0)
- [Get-AzIpGroup](https://docs.microsoft.com/powershell/module/az.network/get-azipgroup?view=azps-3.4.0)
- [Set-AzIpGroup](https://docs.microsoft.com/powershell/module/az.network/set-azipgroup?view=azps-3.4.0)
- [Yeni-AzFirewallNetworkRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallnetworkrule?view=azps-3.4.0)
- [Yeni-AzFirewallApplicationRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallapplicationrule?view=azps-3.4.0)
- [Yeni-AzFirewallNatRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallnatrule?view=azps-3.4.0)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Duvarı'nı nasıl dağıtıp yapılandırılamayı](tutorial-firewall-deploy-portal.md)öğrenin.