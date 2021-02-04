---
title: Kavramlar-vSphere rol tabanlı erişim denetimi (vSphere RBAC)
description: Azure VMware çözümü için vSphere rol tabanlı erişim denetimi 'nin temel özellikleri hakkında bilgi edinin
ms.topic: conceptual
ms.date: 02/02/2021
ms.openlocfilehash: 9d7c724fba194d86c2d73bb8323439de7c83c170
ms.sourcegitcommit: 44188608edfdff861cc7e8f611694dec79b9ac7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99537027"
---
# <a name="vsphere-role-based-access-control-vsphere-rbac-for-azure-vmware-solution"></a>Azure VMware çözümü için vSphere rol tabanlı erişim denetimi (vSphere RBAC)

Azure VMware çözümünde, vCenter cloudadmin adlı yerleşik bir yerel kullanıcıya sahiptir ve yerleşik CloudAdmin rolüne atanır. Yerel cloudadmin kullanıcısı, AD 'de kullanıcıları ayarlamak için kullanılır. Genel olarak, CloudAdmin rolü özel bulutunuzda iş yüklerini oluşturur ve yönetir. Azure VMware çözümünde, CloudAdmin rolünde diğer VMware bulut çözümlerinden farklı vCenter ayrıcalıkları vardır.     

> [!NOTE]
> Azure VMware çözümü Şu anda vCenter veya Azure VMware Çözüm portalında özel roller sunmamaktadır. 

Bir vCenter ve ESXi şirket içi dağıtımda, yöneticinin vCenter hesabına erişimi vardır administrator@vsphere.local . Ayrıca, atanan ek Active Directory (AD) kullanıcıları/grupları da bulunabilir. 

Azure VMware Çözüm dağıtımında, yöneticinin yönetici kullanıcı hesabına erişimi yok. Ancak, vCenter üzerinde CloudAdmin rolüne AD kullanıcıları ve grupları atayabilir.  

Özel bulut kullanıcısının erişimi yok ve Microsoft tarafından desteklenen ve yönetilen belirli yönetim bileşenlerini yapılandıramıyor. Örneğin, kümeler, konaklar, veri depoları ve dağıtılmış sanal anahtarlar.

## <a name="azure-vmware-solution-cloudadmin-role-on-vcenter"></a>VCenter 'da Azure VMware Çözüm CloudAdmin rolü

Azure VMware Çözüm CloudAdmin rolüne verilen ayrıcalıkları Azure VMware çözümünüz özel bulut vCenter ' da görebilirsiniz.

1. SDDC vSphere istemcisinde oturum açın ve **menü**  >  **yönetimi**' ne gidin.
1. **Access Control** altında **Roller**' i seçin.
1. Roller listesinden **CloudAdmin** öğesini seçin ve ardından **ayrıcalıklar**' ı seçin. 

   :::image type="content" source="media/role-based-access-control-cloudadmin-privileges.png" alt-text="VSphere Istemcisinde CloudAdmin rol ayrıcalıklarını görüntüleme":::

Azure VMware çözümünde CloudAdmin rolü vCenter üzerinde aşağıdaki ayrıcalıklara sahiptir. Her ayrıcalık hakkında ayrıntılı bir açıklama için [VMware ürün belgelerine](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-ED56F3C4-77D0-49E3-88B6-B99B8B437B62.html) bakın.

| Privilege | Description |
| --------- | ----------- |
| **Alarmlar** | Alarmı kabul et<br />Alarm oluştur<br />Alarm eylemini devre dışı bırak<br />Uyarıyı Değiştir<br />Alarmı kaldır<br />Alarm durumunu ayarla |
| **İzinler** | İzinleri değiştir |
| **İçerik Kitaplığı** | Kitaplık öğesi Ekle<br />Yayımlanmış bir kitaplık için abonelik oluşturma<br />Yerel Kitaplık oluştur<br />Abone olunan Kitaplık oluştur<br />Kitaplık öğesini Sil<br />Yerel kitaplığı Sil<br />Abone olunan kitaplığı Sil<br />Yayımlanmış bir kitaplığın aboneliğini silme<br />Dosyaları indirme<br />Kitaplık öğelerini çıkar<br />Abone olunan kitaplığı çıkar<br />Depolama alanını içeri aktar<br />Araştırma aboneliği bilgileri<br />Bir kitaplık öğesini abonelere yayımlayın<br />Bir kitaplığı abonelerine yayımlama<br />Depolama alanını oku<br />Kitaplık öğesini Eşitle<br />Abone olunan kitaplığı Eşitle<br />İç denetim yazın<br />Yapılandırma ayarlarını Güncelleştir<br />Güncelleştirme dosyaları<br />Kitaplığı Güncelleştir<br />Kitaplık öğesini Güncelleştir<br />Yerel kitaplığı Güncelleştir<br />Abone olunan kitaplığı Güncelleştir<br />Yayımlanmış bir kitaplığın aboneliğini güncelleştirme<br />Yapılandırma ayarlarını görüntüle |
| **Şifreleme işlemleri** | Doğrudan erişim |
| **Hatayla** | Alan ayır<br />Veri deposuna gözatamıyorum<br />Veri deposunu yapılandırma<br />Alt düzey dosya işlemleri<br />Dosyaları kaldır<br />Sanal makine meta verilerini Güncelleştir |
| **Klasör** | Klasör oluştur<br />Klasörü Sil<br />Klasörü taşı<br />Klasörü yeniden adlandır |
| **Genel** | Görevi iptal et<br />Genel etiket<br />Sağlık<br />Olayı günlüğe kaydet<br />Özel öznitelikleri Yönet<br />Hizmet yöneticileri<br />Özel öznitelik ayarla<br />Sistem etiketi |
| **Konak** | vSphere çoğaltması<br />Çoğaltmayı yönetmek &#160;&#160;&#160;&#160; |
| **vSphere etiketleme** | VSphere etiketi atama ve atamasını kaldırma<br />VSphere etiketi oluştur<br />VSphere etiket kategorisi oluştur<br />VSphere etiketini Sil<br />VSphere etiket kategorisini Sil<br />VSphere etiketini Düzenle<br />VSphere etiket kategorisini Düzenle<br />Kategori için UsedBy alanını değiştirme<br />Etiket için UsedBy alanını değiştirme |
| **Ağ** | Ağ ata |
| **Kaynak** | Öneriyi Uygula<br />Sanal uygulamayı kaynak havuzuna ata<br />Sanal makineyi kaynak havuzuna ata<br />Kaynak havuzu oluştur<br />Sanal makineyi kapalı geçir<br />Sanal makinede güç geçişi gerçekleştir<br />Kaynak havuzunu Değiştir<br />Kaynak havuzunu taşı<br />VMotion sorgula<br />Kaynak havuzunu kaldır<br />Kaynak havuzunu yeniden adlandır |
| **Zamanlanmış görev** | Görev Oluştur<br />Görevi Değiştir<br />Görevi kaldır<br />Görevi çalıştır |
| **Oturumlar** | İleti<br />Oturumu doğrula |
| **Profil** | Profil temelli depolama görünümü |
| **Depolama görünümü** | Görünüm |
| **vApp** | Sanal makine Ekle<br />Kaynak havuzu ata<br />VApp atama<br />Kopyalama<br />Oluştur<br />Sil<br />Dışarı Aktarma<br />İçeri Aktar<br />Taşı<br />Kapatma<br />Açma<br />Rename<br />Askıya Alma<br />Kaydı Kaldır<br />OVF ortamını görüntüle<br />vApp uygulama yapılandırması<br />vApp örneği yapılandırması<br />yapılandırmaya göre vApp Managed<br />vApp kaynak yapılandırması |
| **Sanal makine** | Yapılandırmayı Değiştir<br />&#160;&#160;&#160;&#160;disk kirası alma<br />Mevcut diski eklemek &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;yeni disk Ekle<br />Cihaz ekleme veya kaldırma &#160;&#160;&#160;&#160;<br />Gelişmiş yapılandırma &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;değişiklik CPU sayısı<br />&#160;&#160;&#160;&#160;değişiklik belleği<br />&#160;&#160;&#160;&#160;değişiklik ayarları<br />&#160;&#160;&#160;&#160;takas dosyası yerleşimini değiştirme<br />&#160;&#160;&#160;&#160;değişiklik kaynağı<br />&#160;&#160;&#160;&#160;Konağı USB cihazını yapılandırma<br />Ham cihaz yapılandırma &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;managedBy 'yi Yapılandır<br />&#160;&#160;&#160;&#160;bağlantı ayarlarını görüntüleme<br />Sanal diski &#160;&#160;&#160;&#160;Genişlet<br />&#160;&#160;&#160;&#160;cihaz ayarlarını değiştir<br />&#160;&#160;&#160;&#160;sorgu hatası toleransı uyumluluğu<br />Sorgu sahip olmayan dosyaları &#160;&#160;&#160;&#160;<br />Yollardan yeniden yükleme &#160;&#160;&#160;&#160;<br />Diski &#160;&#160;&#160;&#160;kaldır<br />&#160;&#160;&#160;&#160;yeniden adlandır<br />&#160;&#160;&#160;&#160;Konuk bilgilerini sıfırlama<br />&#160;&#160;&#160;&#160;ek açıklaması ayarla<br />&#160;&#160;&#160;&#160;disk değişiklik izlemeyi Değiştir<br />&#160;&#160;&#160;&#160;çatalını üst üste değiştirme<br />Sanal makine uyumluluğunu yükseltme &#160;&#160;&#160;&#160;<br />Stoku düzenleme<br />Mevcut &#160;&#160;&#160;&#160;oluştur<br />&#160;&#160;&#160;&#160;yeni oluştur<br />&#160;&#160;&#160;&#160;taşıma<br />Kayıt &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;kaldır<br />&#160;&#160;&#160;&#160;kaydını sil<br />Konuk işlemleri<br />&#160;&#160;&#160;&#160;Konuk işlemi diğer adı değişikliği<br />&#160;&#160;&#160;&#160;Konuk işlemi diğer adı sorgusu<br />&#160;&#160;&#160;&#160;Konuk işlem değişiklikleri<br />&#160;&#160;&#160;&#160;Konuk işlem programı yürütme<br />&#160;&#160;&#160;&#160;Konuk işlem sorguları<br />Etkileşim<br />&#160;&#160;&#160;&#160;cevap sorusu<br />Sanal makinede yedekleme işlemini &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;CD medyası yapılandırma<br />&#160;&#160;&#160;&#160;Floppy medyayı yapılandırma<br />&#160;&#160;&#160;&#160;cihazları bağlama<br />&#160;&#160;&#160;&#160;konsolu etkileşimi<br />&#160;&#160;&#160;&#160;ekran görüntüsü oluşturma<br />Tüm diskleri birleştirme &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;sürükleyip bırakma<br />VIX API tarafından Konuk işletim sistemi yönetimini &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;USB HID tarama kodları ekleme<br />&#160;&#160;&#160;&#160;VMware araçları 'nı yükler<br />&#160;&#160;&#160;&#160;duraklatma veya duraklamayı kaldırma<br />&#160;&#160;&#160;&#160;Temizleme veya küçültme işlemleri gerçekleştirme<br />&#160;&#160;&#160;&#160;kapatmayı kapat<br />&#160;&#160;&#160;&#160;açma<br />Sanal makinede kayıt oturumu &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;sanal makinede yeniden oynatma oturumu<br />&#160;&#160;&#160;&#160;askıya al<br />&#160;&#160;&#160;&#160;askıya alma hata toleransı<br />Yük devretme testini &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;test yeniden başlatma ikincil VM<br />Hata toleransını kapatma &#160;&#160;&#160;&#160;<br />Hata toleransını &#160;&#160;&#160;&#160;açın<br />Sağlama<br />&#160;&#160;&#160;&#160;disk erişimine Izin ver<br />&#160;&#160;&#160;&#160;dosya erişimine Izin ver<br />Salt okuma &#160;&#160;&#160;&#160;disk erişimine Izin ver<br />Sanal makine indirmeye Izin ver &#160;&#160;&#160;&#160;<br />Şablonu &#160;&#160;&#160;&#160;Kopyala<br />Sanal makineyi &#160;&#160;&#160;&#160;Kopyala<br />&#160;&#160;&#160;&#160;sanal makineden Şablon Oluştur<br />&#160;&#160;&#160;&#160;Konuk özelleştirme<br />&#160;&#160;&#160;&#160;şablonu dağıtma<br />Şablon olarak &#160;&#160;&#160;&#160;Işaretle<br />&#160;&#160;&#160;&#160;özelleştirme belirtimini değiştirme<br />Diskleri &#160;&#160;&#160;&#160;yükselt<br />&#160;&#160;&#160;&#160;özelleştirme belirtimlerini oku<br />Hizmet yapılandırması<br />Bildirimlere Izin ver &#160;&#160;&#160;&#160;<br />Genel olay bildirimlerinin yoklanmaya Izin &#160;&#160;&#160;&#160;<br />Hizmet yapılandırmasını yönetmek &#160;&#160;&#160;&#160;<br />Hizmet yapılandırmasını değiştirme &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;sorgu hizmeti yapılandırması<br />&#160;&#160;&#160;&#160;hizmeti yapılandırmasını oku<br />Anlık görüntü yönetimi<br />&#160;&#160;&#160;&#160;anlık görüntü oluştur<br />Anlık görüntüyü kaldırmak &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;anlık görüntüyü yeniden adlandır<br />Anlık görüntüyü &#160;&#160;&#160;&#160;al<br />vSphere çoğaltması<br />Çoğaltmayı yapılandırma &#160;&#160;&#160;&#160;<br />Çoğaltmayı yönetmek &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;Izleyicisi çoğaltması |
| **vService** | Bağımlılık oluştur<br />Bağımlılığı yok et<br />Bağımlılık yapılandırmasını yeniden yapılandırın<br />Güncelleştirme bağımlılığı |



## <a name="next-steps"></a>Sonraki adımlar

Azure VMware çözümü için vSphere rol tabanlı erişim denetimi hakkında temel bilgileri kapsadığınıza göre, şunları öğrenmek isteyebilirsiniz:

- [VMware ürün belgelerindeki](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-ED56F3C4-77D0-49E3-88B6-B99B8B437B62.html)her bir ayrıcalığın ayrıntıları.
- [Azure VMware çözümü özel bulutları nasıl izler ve onarır](concepts-monitor-repair-private-cloud.md).
- [Azure VMware Çözüm kaynağını etkinleştirme](enable-azure-vmware-solution.md).

<!-- LINKS - internal -->

<!-- LINKS - external-->
[VMware product documentation]: https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-ED56F3C4-77D0-49E3-88B6-B99B8B437B62.html

