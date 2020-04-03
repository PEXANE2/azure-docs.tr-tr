---
title: Azure Veri Gezgini'ni Sanal Ağınıza Dağıtma
description: Azure Veri Gezgini'ni Sanal Ağınıza nasıl dağıttığınızı öğrenin
author: basaba
ms.author: basaba
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 10/31/2019
ms.openlocfilehash: b0530ddada68cc9d07753a3b8ab30bff642e26dd
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80618660"
---
# <a name="deploy-azure-data-explorer-cluster-into-your-virtual-network"></a>Azure Veri Gezgini kümesini Sanal Ağınıza dağıtma

Bu makalede, bir Azure Veri Gezgini kümesini özel bir Azure Sanal Ağına dağıttığınızda mevcut olan kaynaklar açıklanmaktadır. Bu bilgiler, sanal ağınızdaki (VNet) bir kümeyi bir alt ağa dağıtmanıza yardımcı olur. Azure Sanal Ağlar hakkında daha fazla bilgi için [bkz.](/azure/virtual-network/virtual-networks-overview)

   ![vnet diyagramı](media/vnet-deployment/vnet-diagram.png)

Azure Veri Gezgini, Sanal Ağınızda (VNet) bir kümeyi bir alt ağa dağıtmayı destekler. Bu özellik şunları yapmanızı sağlar:

* Azure Veri Gezgini küme trafiğinizde [Ağ Güvenlik Grubu](/azure/virtual-network/security-overview) (NSG) kurallarını uygulayın.
* Şirket içi ağınızı Azure Veri Gezgini kümesinin alt ağına bağlayın.
* Veri bağlantı kaynaklarınızı[(Olay Hub'ı](/azure/event-hubs/event-hubs-about) ve [Olay İzolesi)](/azure/event-grid/overview)hizmet bitiş noktalarıyla güvence altına ala.r'a. [service endpoints](/azure/virtual-network/virtual-network-service-endpoints-overview)

## <a name="access-your-azure-data-explorer-cluster-in-your-vnet"></a>VNet'inizde Azure Veri Gezgini kümenize erişin

Azure Veri Gezgini kümenize her hizmet için aşağıdaki IP adreslerini (motor ve veri yönetimi hizmetleri) kullanarak erişebilirsiniz:

* **Özel IP**: VNet içindeki kümeye erişmek için kullanılır.
* **Public IP**: Yönetim ve izleme için VNet dışından kümeye erişmek için kullanılır ve kümeden başlayan giden bağlantılar için kaynak adresi olarak kullanılır.

Hizmete erişmek için aşağıdaki DNS kayıtları oluşturulur: 

* `[clustername].[geo-region].kusto.windows.net`(motor) `ingest-[clustername].[geo-region].kusto.windows.net` (veri yönetimi) her hizmet için genel IP eşlenir. 

* `private-[clustername].[geo-region].kusto.windows.net`(motor) `private-ingest-[clustername].[geo-region].kusto.windows.net` (veri yönetimi) her hizmet için özel IP eşlenir.

## <a name="plan-subnet-size-in-your-vnet"></a>VNet'inizde alt net boyutunu planlayın

Azure Veri Gezgini kümesini barındırmak için kullanılan alt ağın boyutu, alt ağ dağıtıldıktan sonra değiştirilemez. Azure Veri Gezgini, VNet'inizde her VM için bir özel IP adresi ve dahili yük dengeleyicileri (motor ve veri yönetimi) için iki özel IP adresi kullanır. Azure ağı, her alt ağ için beş IP adresi de kullanır. Azure Veri Gezgini, veri yönetimi hizmeti için iki VM'yi karşılar. Motor hizmeti VM'leri kullanıcı yapılandırma ölçeği kapasitesine göre sağlanmıştır.

Toplam IP adresi sayısı:

| Kullanım | Adres sayısı |
| --- | --- |
| Motor servisi | Örnek başına 1 |
| Veri yönetimi hizmeti | 2 |
| İç yük dengeleyiciler | 2 |
| Azure ayrılmış adresler | 5 |
| **Toplam** | **#engine_instances + 9** |

> [!IMPORTANT]
> Azure Veri Gezgini dağıtıldıktan sonra değiştirilemediğinden alt ağ boyutu önceden planlanmalıdır. Bu nedenle, rezerv buna göre subnet boyutu gerekli.

## <a name="service-endpoints-for-connecting-to-azure-data-explorer"></a>Azure Veri Gezgini'ne bağlanmak için hizmet bitiş noktaları

[Azure Hizmet Bitiş Noktaları,](/azure/virtual-network/virtual-network-service-endpoints-overview) Azure çok kiracılı kaynaklarınızı sanal ağınıza korumanızı sağlar.
Azure Veri Gezgini kümesini alt ağınıza dağıtmak, Azure Veri Gezgini alt ağının temel kaynaklarını kısıtlarken [Olay Hub'ı](/azure/event-hubs/event-hubs-about) veya [Olay Ağıtı](/azure/event-grid/overview) ile veri bağlantıları kurmanızı sağlar.

> [!NOTE]
> [Depolama](/azure/storage/common/storage-introduction) ve [Olay Hub] ile EventGrid kurulumlarını kullanırken, abonelikte kullanılan depolama [hesabı, güvenlik duvarı yapılandırmasında](/azure/storage/common/storage-network-security)güvenilen Azure platform hizmetlerine izin verirken Hizmet bitiş noktalarıyla Azure Veri Gezgini'nin alt ağına kilitlenebilir, ancak Olay Hub'ı güvenilir [Azure platform hizmetlerini](/azure/event-hubs/event-hubs-service-endpoints)desteklemediği için Hizmet Bitiş Noktası'nı etkinleştiremez.

## <a name="dependencies-for-vnet-deployment"></a>VNet dağıtımı için bağımlılıklar

### <a name="network-security-groups-configuration"></a>Ağ Güvenlik Grupları yapılandırması

[Ağ Güvenlik Grupları (NSG),](/azure/virtual-network/security-overview) Bir VNet içinde ağ erişimini denetleme olanağı sağlar. Azure Veri Gezgini'ne iki uç nokta kullanılarak erişilebilir: HTTS (443) ve TDS (1433). Aşağıdaki NSG kuralları, kümenizin yönetimi, izlenmesi ve düzgün çalışması için bu uç noktalara erişime izin verecek şekilde yapılandırılmalıdır.

#### <a name="inbound-nsg-configuration"></a>Gelen NSG yapılandırması

| **Kullanma**   | **Kimden**   | **Hedef**   | **Protokolü**   |
| --- | --- | --- | --- |
| Yönetim  |[ADX yönetim adresleri](#azure-data-explorer-management-ip-addresses)/AzureDataExplorerManagement(ServiceTag) | ADX alt ağ:443  | TCP  |
| Sistem durumunu izleme  | [ADX sistem durumu izleme adresleri](#health-monitoring-addresses)  | ADX alt ağ:443  | TCP  |
| ADX dahili iletişim  | ADX subnet: Tüm bağlantı noktaları  | ADX subnet:Tüm bağlantı noktaları  | Tümü  |
| Azure yük bakiyesi gelene izin ver (sistem durumu sondası)  | AzureLoadBalancer  | ADX alt ağ:80.443  | TCP  |

#### <a name="outbound-nsg-configuration"></a>Giden NSG yapılandırması

| **Kullanma**   | **Kimden**   | **Hedef**   | **Protokolü**   |
| --- | --- | --- | --- |
| Azure Depolamaya Bağımlılık  | ADX alt ağı  | Depolama:443  | TCP  |
| Azure Veri Gölü'ne bağımlılık  | ADX alt ağı  | AzureDataGölü:443  | TCP  |
| EventHub alımı ve hizmet izleme  | ADX alt ağı  | OlayHub:443.5671  | TCP  |
| Ölçümleri Yayımla  | ADX alt ağı  | AzureMonitör:443 | TCP  |
| Azure Monitor yapılandırma indirme  | ADX alt ağı  | [Azure Monitör yapılandırma bitiş noktası adresleri](#azure-monitor-configuration-endpoint-addresses):443 | TCP  |
| Aktif Dizin (varsa) | ADX alt ağı | AzureActiveDirectory:443 | TCP |
| Sertifika yetkilisi | ADX alt ağı | İnternet:80 | TCP |
| İç iletişim  | ADX alt ağı  | ADX Subnet:Tüm Bağlantı Noktaları  | Tümü  |
| Kullanılan `sql\_request` bağlantı noktaları `http\_request` ve eklentiler  | ADX alt ağı  | Internet:Özel  | TCP  |

### <a name="relevant-ip-addresses"></a>İlgili IP adresleri

#### <a name="azure-data-explorer-management-ip-addresses"></a>Azure Veri Gezgini yönetimi IP adresleri

| Bölge | Adresler |
| --- | --- |
| Orta Avustralya | 20.37.26.134 |
| Avustralya Merkez2 | 20.39.99.177 |
| Doğu Avustralya | 40.82.217.84 |
| Güneydoğu Avustralya | 20.40.161.39 |
| BrazilSouth | 191.233.25.183 |
| Orta Kanada | 40.82.188.208 |
| Doğu Kanada | 40.80.255.12 |
| Orta Hindistan | 40.81.249.251, 104.211.98.159 |
| Orta ABD | 40.67.188.68 |
| ORTA ABD EUAP | 40.89.56.69 |
| Doğu Asya | 20.189.74.103 |
| Doğu ABD | 52.224.146.56 |
| Doğu ABD 2 | 52.232.230.201 |
| Doğu US2 EUAP | 52.253.226.110 |
| Orta Fransa | 40.66.57.91 |
| Güney Fransa | 40.82.236.24 |
| Doğu Japonya | 20.43.89.90 |
| Batı Japonya | 40.81.184.86 |
| Güney Kore - Orta | 40.82.156.149 |
| Güney Kore - Güney | 40.80.234.9 |
| Orta Kuzey ABD | 40.81.45.254 |
| Kuzey Avrupa | 52.142.91.221 |
| Güney Afrika Kuzey | 102.133.129.138 |
| Güney Afrika Batı | 102.133.0.97 |
| Orta Güney ABD | 20.45.3.60 |
| Güneydoğu Asya | 40.119.203.252 |
| Güney Hindistan | 40.81.72.110, 104.211.224.189 |
| Güney Birleşik Krallık | 40.81.154.254 |
| Batı Birleşik Krallık | 40.81.122.39 |
| Orta Batı ABD | 52.159.55.120 |
| Batı Avrupa | 51.145.176.215 |
| Batı Hindistan | 40.81.88.112, 104.211.160.120 |
| Batı ABD | 13.64.38.225 |
| Batı ABD 2 | 40.90.219.23 |

#### <a name="health-monitoring-addresses"></a>Sistem durumu izleme adresleri

| Bölge | Adresler |
| --- | --- |
| Orta Avustralya | 191.239.64.128 |
| Orta Avustralya 2 | 191.239.64.128 |
| Doğu Avustralya | 191.239.64.128 |
| Güneydoğu Avustralya | 191.239.160.47 |
| Güney Brezilya | 23.98.145.105 |
| Orta Kanada | 168.61.212.201 |
| Doğu Kanada | 168.61.212.201 |
| Orta Hindistan | 23.99.5.162 |
| Orta ABD | 168.61.212.201 |
| ORTA ABD EUAP | 168.61.212.201 |
| Doğu Asya | 168.63.212.33 |
| Doğu ABD | 137.116.81.189 |
| Doğu ABD 2 | 137.116.81.189 |
| Doğu ABD 2 EUAP | 137.116.81.189 |
| Orta Fransa | 23.97.212.5 |
| Güney Fransa | 23.97.212.5 |
| Doğu Japonya | 138.91.19.129 |
| Batı Japonya | 138.91.19.129 |
| Güney Kore - Orta | 138.91.19.129 |
| Güney Kore - Güney | 138.91.19.129 |
| Orta Kuzey ABD | 23.96.212.108 |
| Kuzey Avrupa | 191.235.212.69 
| Güney Afrika Kuzey | 104.211.224.189 |
| Güney Afrika Batı | 104.211.224.189 |
| Orta Güney ABD | 23.98.145.105 |
| Güney Hindistan | 23.99.5.162 |
| Güneydoğu Asya | 168.63.173.234 |
| Güney Birleşik Krallık | 23.97.212.5 |
| Batı Birleşik Krallık | 23.97.212.5 |
| Orta Batı ABD | 168.61.212.201 |
| Batı Avrupa | 23.97.212.5 |
| Batı Hindistan | 23.99.5.162 |
| Batı ABD | 23.99.5.162 |
| Batı ABD 2 | 23.99.5.162, 104.210.32.14 |    

#### <a name="azure-monitor-configuration-endpoint-addresses"></a>Azure Monitör yapılandırma bitiş noktası adresleri

| Bölge | Adresler |
| --- | --- |
| Orta Avustralya | 52.148.86.165 |
| Orta Avustralya 2 | 52.148.86.165 |
| Doğu Avustralya | 52.148.86.165 |
| Güneydoğu Avustralya | 52.148.86.165 |
| Güney Brezilya | 13.68.89.19 |
| Orta Kanada | 13.90.43.231 |
| Doğu Kanada | 13.90.43.231 |
| Orta Hindistan | 13.71.25.187 |
| Orta ABD | 52.173.95.68 |
| ORTA ABD EUAP | 13.90.43.231 |
| Doğu Asya | 13.75.117.221 |
| Doğu ABD | 13.90.43.231 |
| Doğu ABD 2 | 13.68.89.19 |    
| Doğu ABD 2 EUAP | 13.68.89.19 |
| Orta Fransa | 52.174.4.112 |
| Güney Fransa | 52.174.4.112 |
| Doğu Japonya | 13.75.117.221 |
| Batı Japonya | 13.75.117.221 |
| Güney Kore - Orta | 13.75.117.221 |
| Güney Kore - Güney | 13.75.117.221 |
| Orta Kuzey ABD | 52.162.240.236 |
| Kuzey Avrupa | 52.169.237.246 |
| Güney Afrika Kuzey | 13.71.25.187 |
| Güney Afrika Batı | 13.71.25.187 |
| Güney Orta ABD | 13.84.173.99 |
| Güney Hindistan | 13.71.25.187 |
| Güneydoğu Asya | 52.148.86.165 |
| Güney Birleşik Krallık | 52.174.4.112 |
| Batı Birleşik Krallık | 52.169.237.246 |
| Orta Batı ABD | 52.161.31.69 |
| Batı Avrupa | 52.174.4.112 |
| Batı Hindistan | 13.71.25.187 |
| Batı ABD | 40.78.70.148 |
| Batı ABD 2 | 52.151.20.103 |

## <a name="expressroute-setup"></a>ExpressRoute kurulumu

Şirket içi ağı Azure Sanal Ağı'na bağlamak için ExpressRoute'u kullanın. Ortak bir kurulum, Kenarlık Ağ Geçidi Protokolü (BGP) oturumu aracılığıyla varsayılan rotanın (0.0.0.0/0) reklamını yapmaktır. Bu, Sanal Ağ'dan çıkan trafiği müşterinin öncül ağına iletilmesi için zorlar ve bu da trafiği bırakabilir ve giden akışların kırılmasına neden olur. Bu varsayılanı aşmak [için, Kullanıcı Tanımlı Rota (UDR)](/azure/virtual-network/virtual-networks-udr-overview#user-defined) (0.0.0.0/0) yapılandırılabilir ve sonraki atlama *Internet*olacaktır. UDR BGP'den önce geldiğinden, trafik Internet'e gidecektir.

## <a name="securing-outbound-traffic-with-firewall"></a>Güvenlik duvarı yla giden trafiği güvence altına alma

Alan adlarını sınırlamak için [Azure Güvenlik Duvarı'nı](/azure/firewall/overview) veya herhangi bir sanal cihazı kullanarak giden trafiği güvence altına almak istiyorsanız, güvenlik duvarına aşağıdaki Tam Nitelikli Alan Adları'na (FQDN) izin verilmelidir.

```
prod.warmpath.msftcloudes.com:443
production.diagnostics.monitoring.core.windows.net:443
graph.windows.net:443
*.update.microsoft.com:443
shavamanifestcdnprod1.azureedge.net:443
login.live.com:443
wdcp.microsoft.com:443
login.microsoftonline.com:443
azureprofilerfrontdoor.cloudapp.net:443
*.core.windows.net:443
*.servicebus.windows.net:443
shoebox2.metrics.nsatc.net:443
prod-dsts.dsts.core.windows.net:443
ocsp.msocsp.com:80
*.windowsupdate.com:80
ocsp.digicert.com:80
go.microsoft.com:80
dmd.metaservices.microsoft.com:80
www.msftconnecttest.com:80
crl.microsoft.com:80
www.microsoft.com:80
adl.windows.com:80
crl3.digicert.com:80
```

Ayrıca, asimetrik rota sorunlarını önlemek için alt ağdaki [rota tablosunu](/azure/virtual-network/virtual-networks-udr-overview) [yönetim adresleri](#azure-data-explorer-management-ip-addresses) ve sonraki hop *Internet* ile sistem durumu [izleme adresleriyle](#health-monitoring-addresses) tanımlamanız gerekir.

Örneğin, **Batı ABD** bölgesi için aşağıdaki ÜT'ler tanımlanmalıdır:

| Adı | Adres Ön Eki | Sonraki Hop |
| --- | --- | --- |
| ADX_Management | 13.64.38.225/32 | Internet |
| ADX_Monitoring | 23.99.5.162/32 | Internet |

## <a name="deploy-azure-data-explorer-cluster-into-your-vnet-using-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonunu kullanarak Azure Veri Gezgini kümesini VNet'inize dağıtma

Azure Veri Gezgini kümesini sanal ağınıza dağıtmak için, VNet Azure Kaynak Yöneticisi [şablonunuzda Azure Veri Gezgini kümesini dağıt'ı](https://azure.microsoft.com/resources/templates/101-kusto-vnet/) kullanın.

Bu şablon küme, sanal ağ, alt ağ, ağ güvenlik grubu ve ortak IP adreslerini oluşturur.
