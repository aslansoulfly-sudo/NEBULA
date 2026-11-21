# NEBULA — AURELIA / NEO CITY (ziyaretçi modu)

Üyeliksiz, sosyal medya üzerinden katılımlı yaşayan bir 3D galaksi. Three.js ile AURELIA dünyası + Supabase (yalnızca herkese açık okuma).

## Dosyalar
- `index.html`: Three.js dünyası, karşılama ekranı, "Find your home" araması, sakin kartı.
- `supabase-config.js`: Supabase Project URL ve Publishable key (asla secret/service_role koyma).
- `netlify.toml`: Yayın ayarı + `/house/*` → `/index.html` yönlendirmesi (status 200).
- `assets/neo-city.webp`, `eden.webp`, `crystal-bay.webp`, `sky-district.webp` (+ `solar-dunes.webp`, `deep-blue.webp`): bölge kartı görselleri (720x540).
- `assets/houses/*.webp`: 2D ev görselleri (şeffaf arka plan). Lisans/kaynak: `assets/houses/LICENSES.md`.

## Ziyaretçi modu
- E-posta, şifre veya giriş yok. Herkes siteyi açıp galaksiyi özgürce gezebilir.
- Evler sosyal medya yorumlarından (ör. "JOIN NEBULA") ileride bot tarafından sunucu tarafında oluşturulacak; sitede ev oluşturma düğmesi yoktur.
- İstatistikler gerçek veriden gelir: Residents = evi olan farklı kişi sayısı, Homes = ev sayısı.

## Find your home (arama)
- Kullanıcı sosyal medya kullanıcı adını yazar (başındaki `@` ve boşluklar silinir, büyük/küçük harf fark etmez).
- Tam eşleşme varsa kamera eve uçar ve sakin kartı açılır (avatar, kullanıcı adı, ev adı, gezegen, bölge, katılım tarihi, Visit Home, Copy link).
- Tam eşleşme yoksa en fazla 5 benzer kullanıcı adı önerilir; hiç yoksa "Resident not found" mesajı gösterilir.

## Kalıcı ev bağlantıları
- Biçim: `https://nebula-yasayan-galaksiler.netlify.app/house/<EV-UUID>`
- Bağlantı açılınca karşılama ekranı atlanır, kamera doğrudan evin önünde başlar ve kart açılır.
- Aramadan veya bir eve çift tıklayınca adres çubuğu `/house/<id>` olur; geri/ileri tuşları çalışır.
- Geçersiz veya bulunamayan id → "This home could not be found" bildirimi ve normal görünüm.
- Sayfa `/house/...` altında da sunulduğu için yerel dosyalar mutlak yolla yüklenir (`/supabase-config.js`, `/assets/...`).

## Yerelde çalıştırma
`/house/*` yönlendirmesini taklit eden basit bir sunucu gerekir, örn. `npx netlify dev` (netlify.toml kurallarını uygular).

## NEO CITY (başlangıç bölgesi)
- Alacakaranlık mor-pembe gökyüzü, nebulalı dev halkalı gezegen, beyaz-cam gökdelenler (camgöbeği/mor ışık şeritleri), sakin körfez, cam yürüyüş yolları ve körfez köprüleri, uçan araçlar, su kenarında ışıklı havuzlu teraslı villalar ve yeşil park.
- Ziyaretçiler NEO CITY'de başlar. NEO CITY evleri sabit arsalara yerleşir (`NEO_PLOTS`: önce doğu sahil terası, sonra batı park, 82 arsa). Koordinatı 0,0 olan evler created_at sırasına göre sıradaki boş arsayı alır.
- EDEN batıda ikincil bir yüzen bölge olarak duruyor (uzaktayken gizlenir). EDEN dışındaki/diğer bölgelerin evleri köprüyle bağlı Crystal Bay adasına yerleşir.

## Evler (2D)
- Her ev, kameraya dönük 2D bir görsel (THREE.Sprite) + zeminde parlayan bir halka. Her ev tipi için tek doku paylaşılır; uzak evler gizlenir (LOD), halka kalır.
- `houses.model` → görsel: `organic_white`, `glass_tiers`, `rock_villa`, `eden_villa` (eden_villa.webp yoksa organic_white). Eski varsayılan `luxury_white` ev id'sine göre üç tipten birini seçer. `houses.color` görsele hafif renk verir.
- Önizleme: `/?demo=40` NEO CITY arsalarını örnek evlerle doldurur (gerçek sakin değildir, istatistiğe sayılmaz).

## Video modu (hazırlık)
- `/?video=1` arayüzü gizler, 720x1280 çizer ve `window.NEBULA_VIDEO.frame(t, {pos,target}, day)` ile belirli bir zamanda/kamerada tek kare üretir. Hazır kamera noktaları: `NEBULA_VIDEO.pois` (neoOverview, neoOverviewTall, neoAerial, neoRiver, neoHouse, edenOverview, crystalBay); `NEBULA_VIDEO.plots` ve `poseForHome(p)` ev yakın çekimi verir.
