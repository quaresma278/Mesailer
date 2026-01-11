<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Personel Mesai Takip Sistemi</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');

        body {
            font-family: 'Inter', sans-serif;
        }

        .glass-card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
        }
    </style>
</head>
<body class="bg-slate-50 min-h-screen">
    <div class="max-w-6xl mx-auto p-4 md:p-8">
        <!-- Header -->
        <header class="mb-10 flex flex-col md:flex-row md:items-center justify-between gap-4">
            <div>
                <h1 class="text-3xl font-extrabold text-slate-900 flex items-center gap-3">
                    <i class="fas fa-clock text-blue-600"></i> Mesai Takip Paneli
                </h1>
                <p class="text-slate-500 mt-1">Veriler tarayıcınıza otomatik olarak kaydedilir.</p>
            </div>
            <div class="bg-white px-4 py-2 rounded-xl shadow-sm border border-slate-200 text-sm">
                <span class="text-slate-400">Mod:</span> <span class="text-blue-600 font-bold tracking-tight">Yerel Depolama</span>
            </div>
        </header>

        <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
            <!-- Form Bölümü -->
            <div class="lg:col-span-4 space-y-6">
                <div class="glass-card p-6 rounded-2xl shadow-xl border border-white">
                    <h2 class="text-lg font-bold mb-5 flex items-center gap-2 text-slate-800">
                        <i class="fas fa-plus-circle text-blue-500"></i> Yeni Kayıt Ekle
                    </h2>
                    <form id="mesai-form" class="space-y-4">
                        <div>
                            <label class="block text-xs font-semibold text-slate-500 uppercase mb-1">Personel Adı</label>
                            <input type="text" id="personelAd" required placeholder="Ad Soyad"
                                   class="w-full p-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent outline-none transition-all">
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-slate-500 uppercase mb-1">Mesai Tarihi</label>
                            <input type="date" id="tarih" required
                                   class="w-full p-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none">
                        </div>
                        <div class="grid grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-semibold text-slate-500 uppercase mb-1">Başlangıç</label>
                                <input type="time" id="baslangic" required
                                       class="w-full p-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-slate-500 uppercase mb-1">Bitiş</label>
                                <input type="time" id="bitis" required
                                       class="w-full p-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none">
                            </div>
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-slate-500 uppercase mb-1 text-orange-600">Mola (Dakika Düşülür)</label>
                            <div class="relative">
                                <span class="absolute left-3 top-3.5 text-slate-400"><i class="fas fa-coffee"></i></span>
                                <input type="number" id="mola" value="0" min="0"
                                       class="w-full p-3 pl-10 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-orange-500 outline-none">
                            </div>
                        </div>
                        <button type="submit"
                                class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-4 rounded-xl shadow-lg shadow-blue-100 transition-all flex items-center justify-center gap-2">
                            <i class="fas fa-save"></i> Listeye Ekle ve Kaydet
                        </button>
                    </form>
                </div>

                <!-- Özet Kartı -->
                <div class="bg-slate-900 text-white p-6 rounded-2xl shadow-2xl relative overflow-hidden">
                    <div class="relative z-10">
                        <div class="flex items-center justify-between mb-2">
                            <span class="text-slate-400 text-sm font-medium">Genel Toplam</span>
                            <i class="fas fa-calculator text-blue-400"></i>
                        </div>
                        <div class="text-5xl font-black text-white">
                            <span id="total-hours">0.00</span> <span class="text-lg font-normal text-slate-500">Saat</span>
                        </div>
                    </div>
                    <div class="absolute -right-4 -bottom-4 text-slate-800 opacity-20 transform rotate-12">
                        <i class="fas fa-clock text-9xl"></i>
                    </div>
                </div>
            </div>

            <!-- Liste Bölümü -->
            <div class="lg:col-span-8">
                <div class="bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden">
                    <div class="p-6 border-b border-slate-100 flex items-center justify-between bg-white sticky top-0 z-20">
                        <h3 class="font-bold text-slate-800">Mesai Kayıtları</h3>
                        <span id="record-count" class="text-xs bg-slate-100 text-slate-600 px-2 py-1 rounded-full font-bold">0 Kayıt</span>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-left">
                            <thead class="bg-slate-50 border-b border-slate-100">
                                <tr>
                                    <th class="px-6 py-4 text-xs font-bold text-slate-400 uppercase">Tarih</th>
                                    <th class="px-6 py-4 text-xs font-bold text-slate-400 uppercase">Personel</th>
                                    <th class="px-6 py-4 text-xs font-bold text-slate-400 uppercase">Aralık/Mola</th>
                                    <th class="px-6 py-4 text-xs font-bold text-slate-400 uppercase">Net Süre</th>
                                    <th class="px-6 py-4 text-xs font-bold text-slate-400 uppercase text-right">İşlem</th>
                                </tr>
                            </thead>
                            <tbody id="mesai-listesi" class="divide-y divide-slate-50">
                                <!-- JavaScript ile doldurulacak -->
                            </tbody>
                        </table>
                    </div>
                    <div id="empty-state" class="py-20 flex flex-col items-center justify-center text-slate-400">
                        <i class="fas fa-calendar-alt text-5xl mb-4 opacity-20"></i>
                        <p>Henüz bir çalışma kaydı bulunmuyor.</p>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Verileri localStorage'dan al veya boş dizi başlat
        let mesaiKayitlari = JSON.parse(localStorage.getItem('mesai_takip_verileri')) || [];

        // Sayfa yüklendiğinde işlemleri başlat
        document.addEventListener('DOMContentLoaded', () => {
            document.getElementById('tarih').valueAsDate = new Date();
            tabloyuGuncelle();
        });

        // Form Gönderimi
        document.getElementById('mesai-form').addEventListener('submit', (e) => {
            e.preventDefault();

            const personelAd = document.getElementById('personelAd').value;
            const tarih = document.getElementById('tarih').value;
            const baslangic = document.getElementById('baslangic').value;
            const bitis = document.getElementById('bitis').value;
            const mola = parseInt(document.getElementById('mola').value) || 0;

            const netSure = calculateNetHours(baslangic, bitis, mola);

            const yeniKayit = {
                id: Date.now(),
                personelAd,
                tarih,
                baslangic,
                bitis,
                mola,
                netSure
            };

            // Diziye ekle ve localStorage'a kaydet
            mesaiKayitlari.push(yeniKayit);
            verileriKaydet();

            // Formu temizle (Personel adı hariç kolaylık olması için)
            document.getElementById('baslangic').value = '';
            document.getElementById('bitis').value = '';
            document.getElementById('mola').value = '0';

            tabloyuGuncelle();
        });

        // Net Saat Hesaplama Fonksiyonu
        function calculateNetHours(start, end, breakMin) {
            const s = new Date(`1970-01-01T${start}:00`);
            const e = new Date(`1970-01-01T${end}:00`);

            let diff = (e - s) / (1000 * 60 * 60);
            if (diff < 0) diff += 24; // Gece vardiyası senaryosu

            return Math.max(0, diff - (breakMin / 60));
        }

        // Verileri Tarayıcı Hafızasına Kaydet
        function verileriKaydet() {
            localStorage.setItem('mesai_takip_verileri', JSON.stringify(mesaiKayitlari));
        }

        // Tabloyu ve Arayüzü Güncelle
        function tabloyuGuncelle() {
            const liste = document.getElementById('mesai-listesi');
            const emptyState = document.getElementById('empty-state');
            const totalHoursDisplay = document.getElementById('total-hours');
            const recordCountDisplay = document.getElementById('record-count');

            liste.innerHTML = '';
            let total = 0;

            // En son kaydı en üstte göster
            const gosterilecekVeri = [...mesaiKayitlari].sort((a, b) => new Date(b.tarih) - new Date(a.tarih));

            gosterilecekVeri.forEach(item => {
                total += item.netSure;
                const row = document.createElement('tr');
                row.className = "hover:bg-slate-50 transition-colors";
                row.innerHTML = `
                        <td class="px-6 py-4 font-medium text-slate-900">${new Date(item.tarih).toLocaleDateString('tr-TR')}</td>
                        <td class="px-6 py-4 text-slate-600">${item.personelAd}</td>
                        <td class="px-6 py-4">
                            <div class="text-xs text-slate-500">${item.baslangic} - ${item.bitis}</div>
                            <div class="text-[10px] text-orange-600 font-bold uppercase">${item.mola} dk mola</div>
                        </td>
                        <td class="px-6 py-4">
                            <span class="px-2.5 py-1 rounded-full text-xs font-bold bg-green-50 text-green-700 border border-green-100">
                                ${item.netSure.toFixed(2)} Saat
                            </span>
                        </td>
                        <td class="px-6 py-4 text-right">
                            <button onclick="kayitSil(${item.id})" class="text-slate-300 hover:text-red-500 transition-colors p-2">
                                <i class="fas fa-trash-alt"></i>
                            </button>
                        </td>
                    `;
                liste.appendChild(row);
            });

            totalHoursDisplay.textContent = total.toFixed(2);
            recordCountDisplay.textContent = `${mesaiKayitlari.length} Kayıt`;
            emptyState.style.display = mesaiKayitlari.length > 0 ? 'none' : 'flex';
        }

        // Kayıt Silme
        function kayitSil(id) {
            if (confirm('Bu mesai kaydını silmek istediğinize emin misiniz?')) {
                mesaiKayitlari = mesaiKayitlari.filter(item => item.id !== id);
                verileriKaydet();
                tabloyuGuncelle();
            }
        }
    </script>
</body>
</html>
