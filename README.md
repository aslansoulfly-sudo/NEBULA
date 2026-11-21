# NEBULA — AURELIA (ziyaretçi modu)

Üyeliksiz, sosyal medya üzerinden katılımlı yaşayan bir 3D galaksi. Three.js ile AURELIA dünyası + Supabase (yalnızca herkese açık okuma).

## Dosyalar
- `index.html`: Three.js dünyası, karşılama ekranı, "Find your home" araması, sakin kartı.
- `supabase-config.js`: Supabase Project URL ve Publishable key (asla secret/service_role koyma).
- `netlify.toml`: Yayın ayarı + `/house/*` → `/index.html` yönlendirmesi (status 200).
- `assets/aurelia-1..3.webp`: Karşılama ekranındaki bölge görselleri (Crystal Bay, Neo City, Sky District).

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
