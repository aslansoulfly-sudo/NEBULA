# NEBULA — AURELIA + Supabase bağlantılı başlangıç sürümü

Bu sürüm mevcut Three.js AURELIA prototipini Supabase Auth ve PostgreSQL tablolarına bağlar.

## Dosyalar
- `index.html`: Three.js dünyası, kayıt/giriş ve ev oluşturma arayüzü.
- `supabase-config.js`: Supabase Project URL ve Publishable key.
- `netlify.toml`: Netlify yayın ayarı.

## 1. Supabase bağlantısını ayarla
1. `supabase-config.js` dosyasını bir metin editörüyle aç.
2. `YOUR_SUPABASE_PROJECT_URL` yerine Supabase Project URL değerini yaz.
3. `YOUR_SUPABASE_PUBLISHABLE_KEY` yerine Publishable key değerini yaz.
4. Tırnakları silme. Secret veya service_role anahtarını kesinlikle kullanma.

Örnek biçim (temsili değerler):
```js
export const SUPABASE_URL = 'https://PROJECT-REF.supabase.co';
export const SUPABASE_PUBLISHABLE_KEY = 'sb_publishable_...';
```

## 2. Supabase SQL kurulumu
Bu uygulama önceki NEBULA SQL kurulumundaki şu tabloları bekler:
- `public.profiles`
- `public.planets`
- `public.districts`
- `public.houses`

Ayrıca `auth.users` tablosuna yeni kullanıcı eklenince `profiles` satırı oluşturan `handle_new_nebula_user` trigger'ı kurulmuş olmalıdır. Uygulama bu tabloları yeniden oluşturmaz.

## 3. Çalıştırma
`index.html` dosyasını doğrudan dosya yöneticisinden açmak yerine yerel bir web sunucusu kullan:
- VS Code + Live Server, veya
- `python -m http.server 8000`

Sonra `http://localhost:8000` adresini aç.

## 4. Test akışı
1. E-posta, şifre ve NEBULA kullanıcı adı girerek hesap oluştur.
2. Supabase Auth e-posta doğrulaması açıksa gelen e-postadaki bağlantıyı onayla.
3. Uygulamaya dön ve giriş yap.
4. `Evimi oluştur` düğmesine bas.
5. Supabase Table Editor'da `profiles` ve `houses` tablolarında yeni kayıtları kontrol et.
6. Uygulamayı başka bir tarayıcıda açtığında aynı evlerin görünmesi gerekir.

## Güvenlik ve prototip sınırları
- Publishable key istemci tarafında kullanılmak içindir; veritabanı güvenliği RLS politikalarına dayanır.
- Secret/service_role anahtarını hiçbir zaman tarayıcıya, GitHub'a veya bu dosyaya koyma.
- Bu başlangıç sürümü kullanıcı başına tek ev oluşturmayı sınırlar.
- Ev koordinatları istemcide mevcut ev sayısına göre atanır. Eşzamanlı çoklu kayıtlar için çakışmasız sunucu tarafı koordinat ataması sonraki aşamada yapılmalıdır.
- Instagram doğrulaması, Grok otomasyonu, arama/ziyaret bağlantıları ve 10.000 ev için tam LOD/alan optimizasyonu henüz dahil değildir.
- Gerçek kullanıcıları davet etmeden önce RLS politikaları, e-posta doğrulaması, kullanıcı adı benzersizliği ve sunucu tarafı ev oluşturma akışı ayrıca test edilmelidir.
