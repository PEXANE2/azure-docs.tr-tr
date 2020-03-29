---
title: Avere vFXT kümesini yönetme - Azure
description: Avere kümesi nasıl yönetilir - düğüm ekleme veya kaldırma, yeniden başlatma, durdurma veya vFXT kümesini yok etme
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: rohogue
ms.openlocfilehash: 94db4a93025b6e3d633368d924e3e0c518d108ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76153488"
---
# <a name="manage-the-avere-vfxt-cluster"></a>Avere vFXT kümesini yönetme

Azure kümesi için Avere vFXT'nizin yaşam döngüsünün bir noktasında küme düğümleri eklemeniz veya kümeyi başlatmak veya yeniden başlatmanız gerekebilir. Projeniz bittiğinde kümeyi nasıl durdurup kalıcı olarak kaldıracağınıbilmeniz gerekir.

Bu makalede, küme düğümleri ve diğer temel küme işlemleri nasıl ekleyeceğiniz veya kaldırılacak şekilde açıklanmaktadır. Küme ayarlarını değiştirmeniz veya çalışmasını izlemeniz gerekiyorsa, [Avere Denetim Masası'nı](avere-vfxt-cluster-gui.md)kullanın.

Yönetim görevine bağlı olarak, üç farklı araçtan birini kullanmanız gerekebilir: Avere Denetim Masası, vfxt.py komut satırı küme yönetimi komut dosyası ve Azure portalı.

Bu tablo, her görev için hangi araçların kullanılabileceğine genel bir bakış sağlar.

| Eylem | Avere Kontrol Paneli | vfxt.py  | Azure portalında |
| --- | --- | --- | --- |
| Küme düğümleri ekleme | hayır | evet | hayır |
| Küme düğümlerini kaldırma | evet | hayır | hayır |
| Küme düğümlerini durdurma | Evet (hizmetleri yeniden başlatabilir veya yeniden başlatabilir) | hayır | portaldan bir düğüm VM'yi kapatması düğüm hatası olarak yorumlanır |
| Durdurulan düğüm başlatma | hayır | hayır | evet |
| Tek bir küme düğümlerini yok et | hayır | hayır | evet |
| Kümeyi yeniden başlatın |  |  |  |
| Kümeyi güvenli bir şekilde kapatın veya durdurun | evet | evet | hayır |
| Kümeyi yok et  | hayır | evet | Evet, ama veri bütünlüğü garanti edilmez. |

Her araç için ayrıntılı talimatlar aşağıda yer almaktadır.

## <a name="about-stopped-instances-in-azure"></a>Azure'da durdurulan örnekler hakkında

Herhangi bir Azure VM'sini kapattığınızda veya durdurduğunuzda, işlem ücretlerine maruz bırakmayı durdurur, ancak depolama alanı için ödeme niz gerekir. Bir vFXT düğümünü veya vFXT kümesinin tamamını kapattıysanız ve yeniden başlatmayı düşünmüyorsanız, ilgili VM'leri silmek için Azure portalını kullanmanız gerekir.

Azure portalında, *durdurulan* bir düğüm (yeniden başlatılabilir) Azure portalında **durdurulan** durumu gösterir. *Silinen* bir düğüm, durumu **durdurulur (ayrılmış)** gösterir ve artık işlem veya depolama ücretlerine maruz kalmaz.

VM'yi silmeden önce, avere denetim masası veya kümeyi durdurmak veya kapatmak için seçenekleri vfxt.py kullanarak değiştirilen tüm verilerin önbellekten arka uç depolamaya yazıldığından emin olun.

## <a name="manage-the-cluster-with-avere-control-panel"></a>Avere Kontrol Paneli ile kümeyi yönetme

Avere Denetim Masası şu görevler için kullanılabilir:

* Tek tek düğümleri durdurma veya yeniden başlatma
* Kümeden düğüm kaldırma
* Tüm kümeyi durdurma veya yeniden başlatma

Avere Denetim Masası veri bütünlüğüne öncelik verir, bu nedenle değiştirilen verileri yıkıcı bir işlemden önce arka uç depolamaya yazmaya çalışır. Bu, azure portalından daha güvenli bir seçenek haline getirir.

Bir web tarayıcısından Avere Kontrol Paneli'ne erişin. Yardıma ihtiyacınız varsa [vFXT kümesine erişme yönergelerini](avere-vfxt-cluster-gui.md) izleyin.

### <a name="manage-nodes-with-avere-control-panel"></a>Avere Kontrol Paneli ile düğümleri yönetme

**FXT Düğümleri** ayarları sayfasında tek tek düğümleri yönetmek için denetimler vardır.

Kapatmak, yeniden başlatmak veya düğümü kaldırmak için **FXT Düğümleri** sayfasındaki listedeki düğümü bulun ve **Eylemler** sütunundaki uygun düğmeyi tıklatın.

> [!NOTE]
> Etkin düğüm sayısı değiştiğinde IP adresleri küme düğümleri arasında hareket edebilir.

Daha fazla bilgi için Avere küme ayarları kılavuzunda [Küme > FXT Düğümlerini](<https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_fxt_nodes.html#gui-fxt-nodes>) okuyun.

### <a name="stop-or-reboot-the-cluster-with-avere-control-panel"></a>Avere Denetim Masası ile kümeyi durdurma veya yeniden başlatma

**Sistem Bakım** ayarları sayfasında küme hizmetlerini yeniden başlatmak, kümeyi yeniden başlatmak veya kümeyi güvenli bir şekilde kapatma komutları vardır. Ayrıntılar için [Yönetim > Sistem Bakımı](<https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_system_maintenance.html#gui-system-maintenance>) 'nı (Avere küme ayarları kılavuzunda) okuyun.

Bir küme kapanmaya başladığında, durum iletilerini **Pano** sekmesine gönderir. Birkaç dakika sonra iletiler durur ve sonunda Avere Denetim Masası oturumu yanıt vermeyi durdurur, bu da kümenin kapandığı anlamına gelir.

## <a name="manage-the-cluster-with-vfxtpy"></a>Kümeyi vfxt.py ile yönetin

vfxt.py küme oluşturma ve yönetimi için bir komut satırı aracıdır.

vfxt.py küme denetleyicisi VM önceden yüklenir. Başka bir sisteme yüklemek istiyorsanız, 'deki <https://github.com/Azure/AvereSDK>belgelere bakın.

vfxt.py komut dosyası şu küme yönetimi görevleri için kullanılabilir:

* Kümeye yeni düğümler ekleme
* Kümeyi durdurma veya başlatma  
* Bir kümeyi yok et

Avere Denetim Masası gibi, vfxt.py işlemleri de küme veya düğümü kapatmadan veya yok etmeden önce değiştirilen verilerin arka uç depolamada kalıcı olarak depolanmasını sağlamaya çalışır. Bu, azure portalından daha güvenli bir seçenek haline getirir.

GitHub: [vfxt.py ile bulut küme yönetimi](https://github.com/azure/averesdk/blob/master/docs/README.md) nde tam bir vfxt.py kullanım kılavuzu mevcuttur

### <a name="add-cluster-nodes-with-vfxtpy"></a>küme düğümleri vfxt.py

Küme düğümleri eklemek için örnek komut komut komut u, küme denetleyicisi üzerinde yer alıyor. Denetleyiciyi bulun ``./add-nodes`` ve küme bilgilerinizle özelleştirmek için bir düzenleyicide açın.

Küme bu komutu kullanmak için çalışıyor olmalıdır.

Aşağıdaki değerleri sağlama:

* Küme ile aynı kaynak grubunda değillerse küme için kaynak grubu adı ve ağ ve depolama kaynakları için de
* Küme konumu
* Küme ağı ve alt ağ
* Küme düğümü erişim rolü (yerleşik rolü [Avere Operatör'ü](../role-based-access-control/built-in-roles.md#avere-operator)kullanın)
* Küme yönetimi IP adresi ve yönetim şifresi
* Eklenecek düğüm sayısı (1, 2 veya 3)
* Düğüm örneği türü ve önbellek boyutu değerleri

Prototipi kullanmıyorsanız, yukarıda açıklanan tüm bilgiler de dahil olmak üzere aşağıdaki gibi bir komut oluşturmanız gerekir.

```bash
   vfxt.py --cloud-type azure --from-environment \
   --resource-group GROUP_NAME \
   [--network-resource-group GROUP_NAME --storage-resource-group GROUP_NAME]  \
   --location LOCATION --azure-network NETWORK_NAME --azure-subnet SUBNET_NAME \
   --add-nodes --nodes NODE_COUNT \
   --management-address CLUSTER_IP --admin-password ADMIN_PASSWORD \
   --instance-type TYPE --node-cache-size SIZE \
   --azure-role "Avere Operator" \
   --log ~/vfxt.log
```

Daha fazla bilgi için, vfxt.py kullanım kılavuzunda [kümeye düğüm ekle'yi](https://github.com/Azure/AvereSDK/blob/master/docs/using_vfxt_py.md#add-nodes-to-a-cluster) okuyun.

### <a name="stop-a-cluster-with-vfxtpy"></a>Kümeyi vfxt.py ile durdurun

```bash
vfxt.py --cloud-type azure --from-environment --stop --resource-group GROUPNAME --admin-password PASSWORD --management-address ADMIN_IP --location LOCATION --azure-network NETWORK --azure-subnet SUBNET
```

### <a name="start-a-stopped-cluster-with-vfxtpy"></a>vfxt.py ile durmuş küme başlatma

```bash
vfxt.py --cloud-type azure --from-environment --start --resource-group GROUPNAME --admin-password PASSWORD --management-address ADMIN_IP --location LOCATION --azure-network NETWORK --azure-subnet SUBNET --instances INSTANCE1_ID INSTANCE2_ID INSTANCE3_ID ...
```

Küme durdurulduğundan, küme düğümlerini belirtmek için örnek tanımlayıcıları geçmeniz gerekir. Daha fazla bilgi edinmek için vfxt.py kullanım kılavuzunda [hangi kümenin değiştirilmeye](https://github.com/Azure/AvereSDK/blob/master/docs/using_vfxt_py.md#specifying-which-cluster-to-modify) karar vereceği belirtin.

### <a name="destroy-a-cluster-with-vfxtpy"></a>bir kümeyi vfxt.py

```bash
vfxt.py --cloud-type azure --from-environment --destroy --resource-group GROUPNAME --admin-password PASSWORD --management-address ADMIN_IP --location LOCATION --azure-network NETWORK --azure-subnet SUBNET --management-address ADMIN_IP
```

Değiştirilen ``--quick-destroy`` verileri küme önbelleğinden kaydetmek istemiyorsanız, bu seçenek kullanılabilir.

Ek bilgi için [vfxt.py kullanım kılavuzunu](<https://github.com/Azure/AvereSDK/blob/master/docs/README.md>) okuyun.

## <a name="manage-cluster-vms-from-the-azure-portal"></a>Azure portalından küme VM'lerini yönetme

Azure portalı küme VM'lerini tek tek yok etmek için kullanılabilir, ancak küme önce temiz olarak kapatılmazsa veri bütünlüğü garanti edilmez.

Azure portalı şu küme yönetimi görevleri için kullanılabilir:

* Durdurulan vFXT düğümü başlatma
* Tek bir vFXT düğümünü durdurun (küme bunu düğüm hatası olarak yorumlar)
* Küme önbelleğindeki değiştirilen verilerin çekirdek filer'e yazıldığından emin olmanız gerekmiyorsa vFXT *kümesini* yok edin
* VFXT düğümlerini ve diğer küme kaynaklarını güvenli bir şekilde kapatıldıktan sonra kalıcı olarak kaldırın

### <a name="restart-vfxt-instances-from-the-azure-portal"></a>Azure portalından vFXT örneklerini yeniden başlatın

Durmuş bir düğümü yeniden başlatmanız gerekiyorsa, Azure portalını kullanmanız gerekir. Sol menüde **Sanal makineleri** seçin ve genel bakış sayfasını açmak için listedeki VM adını tıklatın.

VM'yi yeniden etkinleştirmek için genel bakış sayfasının üst kısmındaki **Başlat** düğmesini tıklatın.

![Durmuş vm başlatma seçeneğini gösteren Azure portal ekranı](media/avere-vfxt-start-stopped-incurring-annot.png)

### <a name="delete-cluster-nodes"></a>Küme düğümlerini silme

VFXT kümesinden bir düğüm silmek ancak kümenin geri kalanını tutmak istiyorsanız, önce Avere Denetim Masası ile [düğümü kümeden kaldırmanız](#manage-nodes-with-avere-control-panel) gerekir.

> [!CAUTION]
> Bir düğümü vFXT kümesinden çıkarmadan silerseniz, veriler kaybolabilir.

VFXT düğümü olarak kullanılan bir veya daha fazla örneği kalıcı olarak yok etmek için Azure portalını kullanın.
Sol menüde **Sanal makineleri** seçin ve genel bakış sayfasını açmak için listedeki VM adını tıklatın.

VM'yi kalıcı olarak yok etmek için genel bakış sayfasının üst kısmındaki **Sil** düğmesini tıklatın.

Küme düğümlerini güvenli bir şekilde kapatıldıktan sonra kalıcı olarak kaldırmak için bu yöntemi kullanabilirsiniz.

### <a name="destroy-the-cluster-from-the-azure-portal"></a>Azure portalından kümeyi yok edin

> [!NOTE]
> Önbellekte kalan istemci değişikliklerinin arka uç depolamaalanına yazılmasını istiyorsanız, `--destroy` azure portalındaki düğüm örneklerini kaldırmadan önce kümeyi temiz bir şekilde kapatmak için vfxt.py seçeneğini kullanın veya Avere Denetim Masası'nı kullanın.

Düğüm örneklerini Azure portalında silerek kalıcı olarak yok edebilirsiniz. Yukarıda açıklandığı gibi teker teker silebilir veya **Sanal Makineler** sayfasını kullanarak tüm küme VM'lerini bulabilir, onay kutularıyla seçebilir ve hepsini tek bir eylemde kaldırmak için **Sil** düğmesini tıklatabilirsiniz.

!["Küme" terimine göre filtrelenen portaldaki VM'lerin listesi, dört işaretten üçü işaretlenmiş ve vurgulanmış](media/avere-vfxt-multi-vm-delete.png)

### <a name="delete-additional-cluster-resources-from-the-azure-portal"></a>Azure portalından ek küme kaynaklarını silme

VFXT kümesi için özel olarak ek kaynaklar oluşturduysanız, kümeyi yıkmanın bir parçası olarak bunları kaldırmak isteyebilirsiniz. İhtiyacınız olan verileri veya diğer projelerle paylaşılan öğeleri içeren öğeleri yok etmeyin.

Küme düğümlerini silmenin yanı sıra, bu bileşenleri kaldırmayı düşünün:

* Küme denetleyicisi VM
* Küme düğümleriyle ilişkili veri diskleri
* Küme bileşenleriyle ilişkili ağ arabirimleri ve ortak IP'ler
* Sanal ağlar
* Depolama kapları ve depolama hesapları **(yalnızca** önemli bir veri içermedikleri takdirde)
* Kullanılabilirlik kümesi

![Bir test kümesi için oluşturulan kaynakları gösteren azure portalı "tüm kaynaklar" listesi](media/avere-vfxt-all-resources-list.png)

### <a name="delete-a-clusters-resource-group-from-the-azure-portal"></a>Azure portalından kümenin kaynak grubunu silme

Kümeyi barındıracak özel bir kaynak grubu oluşturduysanız, kaynak grubunu yok ederek küme için ilgili tüm kaynakları yok edebilirsiniz.

> [!Caution]
> Yalnızca değerli hiçbir şeyin grupta olmadığından eminseniz kaynak grubunu yok edin. Örneğin, kaynak grubundaki herhangi bir depolama kapsayıcısından gerekli verileri taşıdığınızdan emin olun.  

Kaynak grubunu silmek için, portalın sol menüsündeki **Kaynak gruplarını** tıklatın ve vFXT kümesi için oluşturduğunuz grubu bulmak için kaynak grupları listesine filtre uygulayın. Kaynak grubunu seçin ve panelin sağındaki üç noktayı tıklatın. **Kaynak grubunu sil**'i seçin. Geçit sizden silme işlemini onaylamanızı isteyecektir, ki bu geri alınamaz.

!["Kaynak grubunu sil" eylemini gösteren kaynak grubu](media/avere-vfxt-delete-resource-group.png)
